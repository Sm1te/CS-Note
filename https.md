# HTTP

### 1. Basic Concept:

**Hypertext Transfer Protocol** (**HTTP**) is an [application layer](https://en.wikipedia.org/wiki/Application_layer) protocol in the [Internet protocol suite](https://en.wikipedia.org/wiki/Internet_protocol_suite) model for distributed, collaborative, **[hypermedia](https://en.wikipedia.org/wiki/Hypermedia)** information systems. HTTP is the foundation of data communication for the [World Wide Web](https://en.wikipedia.org/wiki/World_Wide_Web), where [hypertext](https://en.wikipedia.org/wiki/Hypertext) documents include [hyperlinks](https://en.wikipedia.org/wiki/Hyperlink) to other resources that the user can easily access, for example by a [mouse](https://en.wikipedia.org/wiki/Computer_mouse) click or by tapping the screen in a web browser.

​	80

**Hypertext Transfer Protocol Secure** (**HTTPS**) is an extension of the [Hypertext Transfer Protocol](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) (HTTP). It is used for [**secure communication**](https://en.wikipedia.org/wiki/Secure_communications) over a [computer network](https://en.wikipedia.org/wiki/Computer_network), and is widely used on the Internet. In HTTPS, the [communication protocol](https://en.wikipedia.org/wiki/Communication_protocol) is encrypted using [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) or, formerly, Secure Sockets Layer (SSL). The protocol is therefore also referred to as **HTTP over TLS ** or **HTTP over SSL**.

​	443



##### http/1.0

- only one web resource, and it will disconnect after request

##### http/1.1

- Access many web resources after connections.



### 2. Http request

**Client - request - server**

**General:**

```java
Request URL: https://www.google.com/    	The request for the address
Request Method: GET    						GET/POST
Status Code: 200  (from prefetch cache)		Status: 200
Remote Address: 142.250.190.4:443			Address
Referrer Policy: origin						Protocol
```

Response Header:

```java
accept-ch: Sec-CH-UA-Platform-Version
accept-ch: Sec-CH-UA-Full-Version
accept-ch: Sec-CH-UA-Arch
accept-ch: Sec-CH-UA-Model
accept-ch: Sec-CH-UA-Bitness
alt-svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000,h3-Q050=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"
bfcache-opt-in: unload
cache-control: private, max-age=0     				Cache Control
content-encoding: br
content-length: 42061
content-type: text/html; charset=UTF-8
date: Thu, 24 Feb 2022 17:22:42 GMT
expires: -1
server: gws
x-frame-options: SAMEORIGIN
x-xss-protection: 0
```



#### GET/Post:

- **Get**: have less parameter to use, and the size is restricted. It will show the data content in the URL address in browser. Not safe bur efficient.
- **Post**: No limit to the parameters to carry, so do size. Nothing shown in the browser. Safe but inefficient.  



#### Http Response:

**Server - Response - Client**



##### Response Header:

```
Accept:   				 Tell the browswer which kind of data it could work with
Accept-Encoding: 		 Which Encoding type it support: GBK, UTF - 8  GB2312
Accept-Language：		Language
Cache-Control：			Cache Control
Connection：				Connection status
HOST：					Host..../.
Refresh:				 Time for refresh
Location:				 Relocate
```

Example (Baidu.com):

```java
Cache-Control:private 缓存控制
Connection:Keep-Alive 连接
Content-Encoding:gzip 编码
Content-Type:text/html 类型
```



#### Reference Code:

1. [Informational responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#information_responses) (`100`–`199`)
2. [Successful responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#successful_responses) (`200`–`299`)
3. [Redirection messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#redirection_messages) (`300`–`399`)
4. [Client error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses) (`400`–`499`)
5. [Server error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#server_error_responses) (`500`–`599`)

![image-20220224114303016](C:\Java_Study\General Node\http_status_code.png)



