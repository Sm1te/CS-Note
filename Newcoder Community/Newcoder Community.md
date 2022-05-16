# Newcoder Community

# 2. 开发社区登录模块

## 2.3会话管理

### HTTP的基本性质

- HTTP是简单的
- HTTP是可扩展的
- HTTP是无状态的，有会话的

### Cookie

- 是服务器发送到浏览器，并保存在浏览器端的一小块数据。
- 浏览器下次访问该服务器时，会自动携带块该数据，将其发送给服务器。

set/get cookie(CookieValue 可以获得指定的cookie)

```java
@RequestMapping(path = "/cookie/set", method = RequestMethod.GET)
@ResponseBody
public String setCookie(HttpServletResponse response) {
    // 创建cookie
    Cookie cookie = new Cookie("code", CommunityUtil.generateUUID());
    // 设置cookie生效的范围
    cookie.setPath("/community/alpha");
    // 设置cookie的生存时间
    cookie.setMaxAge(60 * 10);
    // 发送cookie
    response.addCookie(cookie);

    return "set cookie";
}

@RequestMapping(path = "/cookie/get", method = RequestMethod.GET)
@ResponseBody
public String getCookie(@CookieValue("code") String code) {
    System.out.println(code);
    return "get cookie";
}
```

### Session

- 是JavaEE的标准，用于在服务端记录客户端信息。
- 数据存放在服务端更加安全，但是也会增加服务端的内存压力。

```java
@RequestMapping(path = "/session/set", method = RequestMethod.GET)
@ResponseBody
public String setSession(HttpSession session) {
    session.setAttribute("id", 1);
    session.setAttribute("name", "Test");
    return "set session";
}

@RequestMapping(path = "/session/get", method = RequestMethod.GET)
@ResponseBody
public String getSession(HttpSession session) {
    System.out.println(session.getAttribute("id"));
    System.out.println(session.getAttribute("name"));
    return "get session";
}
```

session 的分布式配置解决方案

![Untitled](Newcoder%20Community/distributed_database.png)

## 2.4 生成验证码

### Kaptcha

- 导入 jar 包
- 编写 Kaptcha 配置类
- 生成随机字符、生成图片



## 2.5 开发登录，推出功能

### 1. 处理login_ticket database

1. 在entity中创建一个LoginTicket类来作为将来的可以用来跟login_ticket dataset交互的类

```java
public class LoginTicket {
    private int id;
    private int userId;
    private String ticket;
    private int status;
    private Date expired;
}
```

2. dao中创建LoginTicketMapper接口实现交互(这里直接使用注解实现了mysql的功能)

```java
@Mapper
public interface LoginTicketMapper {
    @Insert({"insert into login_ticket(user_id, ticket, status, expired) ",
            "values(#{userId}, #{ticket},#{status},#{expired})"
    })
    @Options(useGeneratedKeys = true, keyProperty = "id")
    int insertLoginTicket(LoginTicket loginTicket);

    @Select({"select  id, user_id, ticket, status, expired",
            "from login_ticket where ticket=#{ticket}"
    })
    LoginTicket selectByTicket(String ticket);

    @Update({
            "<script>",
            "update login_ticket set status = #{status} where ticket=#{ticket} ",
            "<if test=\"ticket!=null\">",
            "</if>",
            "</script>"
    })
    int updateStatus(String ticket, int status);
}
```

3. 经过测试没有问题，确实可以增改查之后，在UserSerivce层开始设置login function

   - Check null values

   - Validate account

     - 有无user
     - status是否为0（账户是否激活）

   - Validate password

     - ```java
       password = CommunityUtil.md5(password + user.getSalt());
       user.getPassword().equals(password)
       ```

   - Generate login ticket

     - 我们在返回的map中只需要提供ticket就行，用户可以根据这个来查找具体的ticket

```java
public Map<String, Object> login(String username, String password, long expiredSeconds){
        Map<String, Object> map = new HashMap<>();

        // Null values
        if(StringUtils.isBlank(username)){
            map.put("usernameMsg", "Please enter your username!");
            return map;
        }
        if(StringUtils.isBlank(password)){
            map.put("passwordMsg", "Please enter your password!");
            return map;
        }

        // Validate account
        User user = userMapper.selectByName(username);
        if(user == null){
            map.put("usernameMsg", "Please enter your valid username!");
            return map;
        }

        if(user.getStatus() == 0){
            map.put("usernameMsg", "Please activate your account first!");
            return map;
        }

        //Validate password
        password = CommunityUtil.md5(password + user.getSalt());
        if(user.getPassword().equals(password)){
            map.put("passwordMsg", "Wrong password!");
            return map;
        }

        // Generate login ticket
        LoginTicket loginTicket = new LoginTicket();
        loginTicket.setUeserId(user.getId());
        loginTicket.setTicket(CommunityUtil.generateUUID());
        loginTicket.setStatus(0);
        loginTicket.setExpired(new Date(System.currentTimeMillis() + expiredSeconds * 1000));
        loginTicketMapper.insertLoginTicket(loginTicket);

        map.put("ticket", loginTicket.getTicket());
        return map;
    }
```

### 2. 处理登录请求

获得path，储存cookie

```java
 @Value("${server.servlet.context-path}")
    private String contextPath;
```

用post来实现处理请求

```java
@RequestMapping(path = "/login", method = RequestMethod.POST)
    public String login(String username, String password, String code, boolean rememberme,
                        Model model, HttpSession session, HttpServletResponse response) {
        // 检查验证码
        String kaptcha = (String) session.getAttribute("kaptcha");
        if (StringUtils.isBlank(kaptcha) || StringUtils.isBlank(code) || !kaptcha.equalsIgnoreCase(code)) {
            model.addAttribute("codeMsg", "验证码不正确!");
            return "/site/login";
        }

        // 检查账号,密码
        int expiredSeconds = rememberme ? REMEMBER_EXPIRED_SECONDS : DEFAULT_EXPIRED_SECONDS;
        Map<String, Object> map = userService.login(username, password, expiredSeconds);
        if (map.containsKey("ticket")) {
            Cookie cookie = new Cookie("ticket", map.get("ticket").toString());
            cookie.setPath(contextPath);
            cookie.setMaxAge(expiredSeconds);
            response.addCookie(cookie);
            return "redirect:/index";
        } else {
            model.addAttribute("usernameMsg", map.get("usernameMsg"));
            model.addAttribute("passwordMsg", map.get("passwordMsg"));
            return "/site/login";
        }
    }
```

另外在login界面中，如果我们在上面的代码放了一个复杂对象，是可以直接拿到它的属性值的，但是我们现在也可以通过${param.properties}来在前端获取

```html
<form class="mt-5" method="post" th:action="@{/login}">
</from>

<input type="text" th:class="|form-control ${usernameMsg!=null?'is-invalid':''}|"
        th:value="${param.username}"
        id="username" name="username" placeholder="请输入您的账号!" required>
<div class="invalid-feedback" th:text="${usernameMsg}">
    该账号不存在!
</div>
```

### 3. Logout

```java
public void logout(String ticket) {
	loginTicketMapper.updateStatus(ticket, 1);
}

@RequestMapping(path = "/logout", method = RequestMethod.GET)
    public String logout(@CookieValue("ticket") String ticket) {
        userService.logout(ticket);
        return "redirect:/login";
}
```

