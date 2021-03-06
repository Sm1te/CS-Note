## Basic info about git



![Git-cheatsheet](C:\Java_Study\General Node\img\Git-cheatsheet.jpg)







#### Basic Linux command：

```linux
1）、cd : 改变目录。
2）、cd . . 回退到上一个目录，直接cd进入默认目录
3）、pwd : 显示当前所在的目录路径。
4）、ls(ll): 都是列出当前目录中的所有文件，只不过ll(两个ll)列出的内容更为详细。
5）、touch : 新建一个文件 如 touch index.js 就会在当前目录下新建一个index.js文件。
6）、rm: 删除一个文件, rm index.js 就会把index.js文件删除。
7）、mkdir: 新建一个目录,就是新建一个文件夹。
8）、rm -r : 删除一个文件夹, rm -r src 删除src目录
9）、mv 移动文件, mv index.html src index.html 是我们要移动的文件, src 是目标文件夹,当然, 这样写,
必须保证文件和目标文件夹在同一目录下。
10）、reset 重新初始化终端/清屏。
11）、clear 清屏。
12）、history 查看命令历史。
13）、help 帮助。
14）、exit 退出。
15）、#表示注释

```



#### set personal info:

```
git config --global user.name "kuangshen" #名称
git config --global user.email 24736743@qq.com #邮箱
```

#### How it works:

![image-20220226014332464](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220226014332464.png)

- Workspace：工作区，就是你平时存放项目代码的地方 
- Index / Stage：暂存区，用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列 表信息 
- Repository：仓库区（或本地仓库），就是安全存放数据的位置，这里面有你提交到所有版本的数 据。其中HEAD指向最新放入仓库的版本 
- Remote：远程仓库，托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据 交换



### Process

１、在工作目录中添加、修改文件； ２、将需要进行版本管理的文件放入暂存区域； ３、将暂存区域的文件提交到git仓库。

![image-20220226014601804](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220226014601804.png)





### Daily use

![image-20220226014634965](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220226014634965.png)

1. **创建全新的仓库，需要用GIT管理的项目的根目录执行：**

```
# 在当前目录新建一个Git代码库
$ git init
```

执行后可以看到，仅仅在项目目录多出了一个.git目录，关于版本等的所有信息都在这个目录里面。

2. **另一种方式是克隆远程目录，由于是将远程服务器上的仓库完全镜像一份至本地**

```
# 克隆一个项目和它的整个代码历史(版本信息)
$ git clone [url]
```



```
#查看指定文件状态
git status [filename]
#查看所有文件状态
git status
```



有些时候我们不想把某些文件纳入版本控制中，比如数据库文件，临时文件，设计文件等 

在主目录下建立**".gitignore"**文件，此文件有如下规则： 

1. 忽略文件中的空行或以井号（#）开始的行将会被忽略。 
2. 可以使用Linux通配符。例如：星号（*）代表任意多个字符，问号（？）代表一个字符，方括号 （[abc]）代表可选字符范围，大括号（{string1,string2,...}）代表可选的字符串等。
3. 如果名称的最前面有一个感叹号（!），表示例外规则，将不被忽略。
4. 如果名称的最前面是一个路径分隔符（/），表示要忽略的文件在此目录下，而子目录中的文件不 忽略。 
5. 如果名称的最后面是一个路径分隔符（/），表示要忽略的是此目录下该名称的子目录，而非文件 （默认文件或目录都忽略）。



#### Remote repository

Add existing repository

```
git remote add origin git@github.com:Sm1te/Web-Development.git
git branch -M main
git push -u origin main
```



### Clone

Go to that directory

**git clone + url from website.**

also git log will show the log history



#### Branches and merging

git branch
