# (*)常见考点-HTTP 应用层

HTTP报文抓取方法：

- Firefox浏览器自带开发者工具F12 
- Burp，注意使用Burp破解版并导入证书才可抓取HTTPS流量
- 其他，暂不讲解

### 1. F12抓取Http报文

浏览器开发者工具F12

![image-20220330003646520](https://tva1.sinaimg.cn/large/e6c9d24ely1h0r85e4rkpj21h40iwq92.jpg)

### 2. Burp Suite使用讲解    TCP三次握手  R

BurpSuite原理导图：![image-20241203125435823](C:\Users\王瑾\AppData\Roaming\Typora\typora-user-images\image-20241203125435823.png)

Burp Suite 是用于攻击web 应用程序的集成平台。它包含了许多工具，并为这些工具设计了许多接口，以促进加快攻击应用程序的过程。所有的工具都共享一个能处理并显示HTTP 消息，持久性，认证，代理，日志，警报的一个强大的可扩展的框架。

1. Target(目标)-显示 目标目录结构的的一个功能


2. Proxy(代理)一拦截HTTP/S的代理服务器， 作为-个在浏览器和目标应用程序之间的中间人，允许你拦截，查看，修改在两个方

  向上的原始数据流.

3. Spider(蜘蛛)一应 用智能感应的网络爬虫，它能完整的枚举应用程序的内容和功能。

4. Scanner(扫描器)-高级工具， 执行后，它能自动地发现web应用程序的安全漏洞。

5. Intruder(入侵)---个 定制的高度可配置的工具，对web应用程序进行自动化攻击,如:枚举标识符，收集有用的数据，以及使用模糊不清的技术探测常规漏洞。信息收集工具NMAP

6. Repeater(中继器)---个 靠手动操作来触发单独的HTTP请求，并分析应用程序响应的工具。

7. Sequencer(会话)一用来分 析那些不可预知的应用程序会话令牌和重要数据项的随机性的工具。

8. Decoder(解码器)--进行 手动执行或对应用程序数据者智能解码编码的工具。密码

9. Comparer(对比)一-通 常是通过一些相关的请求和响应得到两项数据的一个可视化的“差异"。

10. Extender(扩展)-可以让你加载Burp Suite的扩展，使用你自己的或第三方代码来扩展Burp Suit的功能。

11. Options(设置)-对Burp Suite的- -些设置

12. Alerts(警告)- -Burp Suite在运行过程中发生的- -写错误

……………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………………

#### URL相关知识

![image-20240103171710182](C:\Users\王瑾\AppData\Roaming\Typora\typora-user-images\image-20240103171710182.png)

常见端口：

- HTTPS:443 

- HTTP:80

URL称为**统一资源定位器**(Uniform Resource Locator)，Web 浏览器使用 URL 从 Web 服务器请求页面。

特点：

- URL 编码格式为`%[十六进制数]` 例如`%61`代表ASCII码十进制为`97`的字符`a`
- 只能使用 ASCII 字符集，通过因特网进行发送

示例：

- URL：`http://user:passwd@www.example.cn:666/info/index.html`

- 协议：http协议


- 登录信息：user:passwd


- 服务器地址：www.example.cn


- 端口：666


- 层次路径：info

#### HTTP八大方法

1. **GET：获取资源**
2. **POST：传输实体内容**
3. DELETE：删除资源
4. OPTIONS：询问支持的请求方法
5. PUT：传输文件
6. HEAD：获取报文首部
7. TRACE：路径追踪
8. CONNECT：要求用隧道协议连接代理



#### 请求报文（Request）

请求和响应结构图

![](https://content.markdowner.net/pub/7jBRbx-RMyrvdm)

什么是请求报文（Request）？

>  请求资源时，客户端向Web服务器发送一个文本形式的请求报文，请求报文按照**请求行、请求头部、空行和请求数据**4部分有序组成。



- 一个完整的GET请求报文如下：


```http
GET /1.php HTTP/1.1    -------------请求行
Host: 192.168.1.100    -------------请求头部
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:98.0) Gecko/20100101 Firefox/98.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
                       ----------空行
											------------请求数据
```

请求行

```http
GET /get/ HTTP/1.1
```

请求头部分

```http
Host: 123.206.87.240:8002
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
Cookie: PHPSESSID=1
```

空行（此空行不可省）

```
							#空行
```

请求数据（由于是调用get方法，一般请求请求数据的body部分为空）

```
              #空数据               
```

- 一个带参数的GET请求报文


```http
GET /get/?name=王权 HTTP/1.1
Host: 123.206.87.240:8002
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
Origin: http://123.206.87.240:8002
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://123.206.87.240:8002/post/
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
Cookie: PHPSESSID=1
Connection: close


```

- 一个带数据的POST请求报文


``` http
POST /post/?age=123&name=234 HTTP/1.1  ------------------------------请求行
Host: 123.206.87.240:8002
Content-Length: 10
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
Origin: http://123.206.87.240:8002
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://123.206.87.240:8002/post/
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
Cookie: PHPSESSID=1
Connection: close
                     ------------------------------空
name=h3zh1           ------------------------------请求数据
```



#### 响应报文（Response）

什么是响应报文（Response）？

> ​	在接收到请求后，服务器会解析请求，定位请求资源，最后将资源复本写到TCP套接字发送到客户端。一个响应由状态行、响应头部、空行和响应数据4部分组成。



一个状态码为200的响应报文示例

```http
HTTP/1.1 200 OK      -----------------------------状态行 HTTP/1.1  200表示访问成功 OK表示访问成功
Server: nginx                -----------------------------------响应报头
Date: Tue, 09 Jun 2020 14:03:01 GMT -----------------------------------消息报头
Content-Type: text/html      -----------------------------------消息报头
Connection: close            -----------------------------------------消息报头
Content-Length: 95
                              -----------------------------------空行
hello world                   -----------响应数据 
```

 404报文

```http
HTTP/1.1 404 Not Found        ----------------------------状态行 404 not found表示没有找到资源
Server: nginx                 -----------------------------------消息报头
Date: Tue, 09 Jun 2020 14:05:55 GMT   -----------------------------------消息报头
Content-Type: text/html       -----------------------------------消息报头
Content-Length: 564           -----------------------------------消息报头
Connection: close             -----------------------------------消息报头
                              -----------------------------------空行
<html>
<head><title>404 Not Found</title></head>
<body bgcolor="white">
<center><h1>404 Not Found</h1></center>
<hr><center>nginx</center>
</body>
</html>
<!-- a padding to disable MSIE and Chrome friendly error page -->
<!-- a padding to disable MSIE and Chrome friendly error page -->
```



#### HTTP字段表

| Header                  | 解释                                                         | 示例                                                         |
| :---------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Accept                  | 指定客户端能够接收的内容类型                                 | Accept: text/plain, text/html,application/json               |
| Accept-Charset          | 浏览器可以接受的字符编码集。                                 | Accept-Charset: iso-8859-5                                   |
| Accept-Encoding         | 指定浏览器可以支持的web服务器返回内容压缩编码类型。          | Accept-Encoding: compress, gzip                              |
| Accept-Language         | 浏览器可接受的语言                                           | Accept-Language: en,zh                                       |
| Accept-Ranges           | 可以请求网页实体的一个或者多个子范围字段                     | Accept-Ranges: bytes                                         |
| **Authorization**       | HTTP授权的授权证书                                           | Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==            |
| Cache-Control           | 指定请求和响应遵循的缓存机制                                 | Cache-Control: no-cache                                      |
| Connection              | 表示是否需要持久连接。（HTTP 1.1默认进行持久连接）           | Connection: close                                            |
| **Cookie**              | HTTP请求发送时，会把保存在该请求域名下的所有cookie值一起发送给web服务器。 | Cookie: $Version=1; Skin=new;                                |
| Content-Length          | 请求的内容长度                                               | Content-Length: 348                                          |
| **Content-Type**        | 请求的与实体对应的MIME信息                                   | Content-Type: application/x-www-form-urlencoded              |
| Date                    | 请求发送的日期和时间                                         | Date: Tue, 15 Nov 2010 08:12:31 GMT                          |
| Expect                  | 请求的特定的服务器行为                                       | Expect: 100-continue                                         |
| If-Match                | 只有请求内容与实体相匹配才有效                               | If-Match: “737060cd8c284d8af7ad3082f209582d”                 |
| If-None-Match           | 如果内容未改变返回304代码，参数为服务器先前发送的Etag，与服务器回应的Etag比较判断是否改变 | If-None-Match: “737060cd8c284d8af7ad3082f209582d”            |
| If-Range                | 如果实体未改变，服务器发送客户端丢失的部分，否则发送整个实体。参数也为Etag | If-Range: “737060cd8c284d8af7ad3082f209582d”                 |
| Max-Forwards            | 限制信息通过代理和网关传送的时间                             | Max-Forwards: 10                                             |
| Pragma                  | 用来包含实现特定的指令                                       | Pragma: no-cache                                             |
| Proxy-Authorization     | 连接到代理的授权证书                                         | Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==      |
| Range                   | 只请求实体的一部分，指定范围                                 | Range: bytes=500-999                                         |
| TE                      | 客户端愿意接受的传输编码，并通知服务器接受接受尾加头信息     | TE: trailers,deflate;q=0.5                                   |
| Upgrade                 | 向服务器指定某种传输协议以便服务器进行转换（如果支持）       | Upgrade: HTTP/2.0, SHTTP/1.3, IRC/6.9, RTA/x11               |
| **User-Agent**          | User-Agent的内容包含发出请求的用户信息                       | User-Agent: Mozilla/5.0 (Linux; X11)                         |
| **Via**                 | 通知中间网关或代理服务器地址，通信协议                       | Via: 1.0 fred, 1.1 nowhere.com (Apache/1.1)                  |
| **X-Forwarded-For**     | 它代表客户端，也就是HTTP的请求端真实的IP，只有在通过了HTTP 代理或者负载均衡服务器时才会添加该项。（HTTP_X_FORWARDED_FOR） | **X-Forwarded-For**:127.0.0.1 localhost                      |
| **Referer**             | 先前网页的地址，当前请求网页紧随其后,即来路。HTTP_REFERER    | Referer: [http://www.zcmhi.com/archives...](http://www.zcmhi.com/archives/71.html) |
| **Clinet-IP**           | 用户真正IP地址      接收变量HTTP_CLIENT_IP                   | Clinet-ip:123.123.123.123                                    |
| **If-Modified-Since**   | 如果请求的部分在指定时间之后被修改则请求成功，未被修改则返回304代码 | If-Modified-Since: Sat, 29 Oct 2010 19:43:31 GMT             |
| **If-Unmodified-Since** | 只在实体在指定时间之后未被修改才请求成功                     | If-Unmodified-Since: Sat, 29 Oct 2010 19:43:31 GMT           |
| **From**                | 发出请求的用户的Email                                        | From: user@email.com                                         |
| **Host**                | 指定请求的服务器的域名和端口号                               | Host: www.zcmhi.com                                          |
| **Request Method**      | HTTP通常使用请求方法                                         | **GET、POST、**HEAD、OPTIONS、PUT、 DELETE、TRACE 、 CONNECT |
| **Status Code**         | 浏览器接收并显示网页前，此网页所在的服务器会返回一个包含HTTP状态码的信息头（server header）用以响应浏览器的请求。 | 200、404、301、302、500                                      |

注：一些值Content-Type值

1. text/plain
2. text/html
3. text/css
4. image/jpeg
5. image/png
6. image/svg+xml
7. audio/mp4
8. video/mp4
9. application/javascript
10. application/pdf
11. application/zip
12. application/atom+xml

#### 附部分状态码解释

**200** 服务器已成功处理了请求并提供了请求的网页。

**301** 状态码是**永久重定向（Moved Permanently）**，表示所请求的资源已经永久地转移到新的位置

​		Location字段表示重定向后的资源新位置

**302** 状态码是**临时重定向（Move Temporarily）**，表示所请求的资源临时地转移到新的位置，一般24~48小时内会用到302。

**304** Not Modified（重定向） 当测试实体自上次检索以来是否被修改时，出现此结果。

**403** Forbidden  客户端错误

**401** Unauthorized 客户端错误

**413** Payload Too Large（客户端错误） 当请求长度过长时，出现此结果。

**400** BadRequest（客户端错误） 当参数无效时，出现此结果。

**404** Not Found（客户端错误） 当资源不存在时，出现此结果。

**405** Method Not Allowed（客户端错误）由于方法和资源组合不正确而出现此错误。 例如，您不能对一个实体集合使用 DELETE 或 PATCH。

**412** Precondition Failed 客户端错误

**501** Not Implemented（服务器错误） 当未实施某个请求的操作时，出现此结果。

**503** Service Unavailable（服务器错误） 当 Web API 服务不可用时，出现此结果。

### 3.题目

#### View-source 类:

浏览器打开html源代码的三种方式:

1. F12
2. CTRL + U
3. 右键查看网页源代码
4. 浏览器地址栏输入`view-source:网址`

例题1：view_source https://adworld.xctf.org.cn/task/answer?type=web&number=3&grade=0&id=5061&page=1 

例题2：[BSidesCF 2019]Futurella  https://buuoj.cn/challenges#[BSidesCF 2019]Futurella  

例题3：key究竟在哪里呢？ http://lab1.xseclab.com/base7_eb68bd2f0d762faf70c89799b3c1cc52/index.php  hackinglab  

例题4：头等舱  https://ctf.bugku.com/challenges/detail/id/77.html

#### 前端绕过题:

例题1：disabled_button  https://adworld.xctf.org.cn/task/answer?type=web&number=3&grade=0&id=5066&page=1    

例题2：计算器  https://ctf.bugku.com/challenges/detail/id/69.html  Bugku  html之maxlength

例题3：web3  https://ctf.bugku.com/challenges/detail/id/73.html   Bugku html实体

​			html实体在线解密：https://www.qqxiuzi.cn/bianma/zifushiti.php  

​			可以直接使用burpsuite

```
&#102;&#108;&#97;&#103;&#123;&#56;&#101;&#56;&#55;&#102;&#55;&#50;&#98;&#99;&#49;&#50;&#97;&#97;&#49;&#54;&#49;&#50;&#53;&#98;&#101;&#101;&#57;&#49;&#54;&#57;&#98;&#51;&#100;&#98;&#55;&#51;&#57;&#125;
```



#### HTTP字段分析

该类题目考核形式主要是是**HTTP字段伪造**，根据需要对特定字段进行补充/更改即可拿到flag

例题1：web基础$_GET ， https://ctf.bugku.com/challenges/detail/id/70.html

例题2：web基础$_POST ，https://ctf.bugku.com/challenges/detail/id/71.html

例题3：请求方式  https://www.ctfhub.com/#/skilltree

例题4：key又找不到了 http://lab1.xseclab.com/base8_0abd63aa54bef0464289d6a42465f354/index.php    涉及重定向

例题5：302跳转   https://www.ctfhub.com/#/skilltree

例题6：Cookie  https://www.ctfhub.com/#/skilltree    

例题7：HAHA浏览器 http://hackinglab.cn/ShowQues.php?type=bases  hackinglab  UA头  

例题8：种族歧视 http://lab1.xseclab.com/base1_0ef337f3afbe42d5619d7a36c19c20ab/index.php  hackinglab  

Accept-Language  

```
Accept-Language: zh-CN,zh;q=0.9,en;q=0.7
优先支持中文，如果没有中文则支持英文。
q:表示语言的权重, 0 <= q <= 1
```

例题9：本地管理员  https://ctf.bugku.com/challenges/detail/id/79.html   Bugku   常见的管理员用户名

​			base64编码`dGVzdDEyMw==`

127.0.0.1 / localhost

​			管理员常用用户名：root、admin

例题10：xff_referer  https://adworld.xctf.org.cn/task/answer?type=web&number=3&grade=0&id=5068&page=1 

​			 xff | referer|clinet-ip 

#### HTTP字段综合题

例题1：[极客大挑战 2019]Http  https://buuoj.cn/challenges#[极客大挑战 2019]Http

​				action="abc.php" method="post"

​				action="abc.php" method="get"

#### 常见考点：robots协议

​	robots协议也叫robots.txt（统一小写），是一个存放于网站根目录下的文本，通常告诉网络搜索引擎，网站中的那些内容可以爬取，允许什么类型爬虫去爬取。比如：https://www.jd.com/robots.txt 、https://www.taobao.com/robots.txt。

​	实际例题中一般作为web签到题，或者用于引出实际题目。

例题1: robots https://adworld.xctf.org.cn/task/answer?type=web&number=3&grade=0&id=5063&page=1

