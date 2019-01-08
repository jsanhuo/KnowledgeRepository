[TOC]

# JavaWeb

### 一，TomCat容器推送的技术？

1. pull拉，通过定时的ajax，定时就进行请求。

2. push推送，异步Servlet，Tomcat可以把当前用户的连接一直保持着。Tomcat容器必须支持主动保持连接，异步处理完业务，主动关闭连接,开始startAsync()和结束complete()方法

3. 淘宝京东在线聊天：WebSocket协议，浏览器端js编写webSocket（也要指定服务器的地址和端口号）代码，相当于就是一个tcp客户端，服务器用webSocket编写的代码就相当于是一个tcp服务器，webSocket更大的好处是，tomcat服务器和通信服务器是独立部署的

### 二，http-web请求发生的事情

1.浏览器解析出请求的主机名
2.查找hosts文件中，是否有主机名对应的ip地址(假若本机未找到，会通过DNS外网查找)
3.发出http请求到apache
4.apache解析出主机名
5.apache解析出请求资源
6.apache取回资源并组成一个http响应(string)
7.apache将http响应返回给浏览器
8.解析http响应，并显示到http浏览器