# Python CGI编程

[简单了解]

`CGI(Common Gateway Interface)`,通用网关接口。

它是一段程序,运行在服务器上如：HTTP服务器，提供同客户端HTML页面的接口。

## 网页浏览

为了更好的了解CGI是如何工作的，我们可以从在网页上点击一个链接或URL的流程：

1、使用你的浏览器访问URL并连接到HTTP web 服务器。

2、Web服务器接收到请求信息后会解析URL，并查找访问的文件在服务器上是否存在。若存在，返回文件内容；若不存在，返回对应的错误提示。

3、浏览器从服务器上接收信息，并显示接收的文件或者错误信息。

CGI程序可以是Python脚本，PERL脚本，SHELL脚本，C或者C++程序等。

## CGI架构图

![image-20250801215423183](https://cdn.jsdelivr.net/gh/starlight-gli/image-host/img/image-20250801215423183.png)