## 花了两天时间又啃了一遍图解HTTP


### TCP/IP协议族被划分为4个层
#### 应用层、传输层、网络层、链路层
#### 分层的好处：修改协议的时候可以部分替换，把各层的接口无缝替换即可。没个层可以更自由
#### 总结，TCP/IP协议族分为4个层，各个层都有负责不同工作的协议

### 应用层
包含了各种应用协议服务：FTP、HTTP、DNS

### 传输层
包含了两个性质不同的协议：TCP（传输控制协议）、UDP（用户数据包协议）

### 网络层
用来处理网络上的数据包，规定了路径、如何传送到目的地。即在广泛的网络设备中，去选择一条传输线路。

### 链路层
链接整个网络的硬件设备：操作系统、硬件驱动、网络适配器、传输介质、路由交换设备等
![4层的连接图](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/sicengjiexi.png)
![4层的连接图](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/sicengchuanshu.png)

#### 通信传输流程
发送方：发送http报文->传输层（TCP协议）分割消息，加上记号和端口号->网络层（IP协议）增加通讯目的地的MAC地址等->链路层（硬件介质）依赖物理介质传输封装好的http消息。每通过一层都会增加该层的首部

接收方：反解刚才发送方的步骤。每通过一层都会消掉该层的首部

### 与HTTP紧密关联的协议：IP（网际协议）、TCP、DNS

#### IP协议
IP协议位于网络层，IP协议不是IP地址。IP协议的工作是把数据包保证确实的发送到对方那里
中间需要满足两个条件 IP地址和MAC地址，两个条件才能确定目的地
IP地址是可变的
MAC地址是网卡出厂就被固定了，在未修改前是唯一的

IP间的通讯依赖于MAC地址，ARP协议可以通过通讯方的MAC地址查到对应的IP地址（网吧常见的ARP攻击）

![数据发送经过的节点图](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/shujufasong.png)


#### TCP
TCP位于传输层，提供可靠的字节流服务。为了方便传输，将大块数据分割成以报文段为单位的数据包进行管理
TCP的可靠是能把数据准确可靠的传输给对方。
为了准确无误的将数据发送给对方，TCP协议采用了三次握手策略
用TCP协议把数据包发送出去后，TCP不会置之不理，它会向对方确认是否发送成功。
握手过程中使用了TCP的标志，SYN和ACK
发送端先发送一个带SYN标志的数据包给对方，接收方收到后回传一个带有SYN/ACK标志的数据包以表示确认送达
最后发送端再回传一个带ACK标志的数据包，代表握手成功，请求结束
若在握手中的某个阶段发生莫名中断，TCP协议会再次以相同的顺序重新发送相同的数据包
除了三次握手，TCP还有其他的手段来保证通讯的可靠性，四次挥手等
![三次握手图](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/sanciwoshou.png)

#### DNS
DNS和HTTP一样位于应用层，提供域名->IP地址之间的解析服务
计算机可以被赋予IP地址，也被赋予了主机名和域名。
用户通常通过主机名和域名来访问对方的网络资源，而不是直接通过IP地址
因为域名和主机名相比IP地址的一堆数字来说更容易人类的记忆，也相对更好理解
为了解决上述问题，DNS服务应运而生
DNS通过IP地址解析域名或域名解析IP地址
![DNS解析图](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/dnsjiexi.png)


### 各种协议与HTTP协议的关系
请求一个域名数据->DNS解析域名
拿着真实IP吧啦吧啦
![各种协议与HTTP协议的关系图](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/httpguanxi.png)

### URL（统一资源定位符）与URI（统一资源标识符）
URL是web浏览器等访问web页面时需要输入的网页地址（www.github.com)
URI是用来标识某一互联网资源，定义了uniform（http:,ftp:,https:)，resource(文字文档，图片文件，各种资源)，identifier（表示可标识的对象，也称标识符）。就是由某个协议方案表示的资源的定位标识符(http://www.github.com)
URL是URI的子集


## HTTP协议
HTTP协议规定请求从客户端发送，最后服务端响应请求并返回。

### HTTP请求报文的构成（客户端发送到服务端）
```
请求首部---
GET /hbh/show HTTP/1.1  请求方法 URI 协议版本号
Host:xxx.jpg            请求资源
空行分割
请求主体---
name=liuxiaoliang&age=18
```
### HTTP响应报文的构成（服务端返回给客户端）
```
响应首部--
HTTP/1.1 200 OK     服务器对应的HTTP版本 响应状态码 原因短语
DATE:xxx,           首部字段的属性，日期
Content-Length:99   
空行分割
响应主体---
<html>
...
```

### HTTP是不保存状态的协议（无状态协议）
HTTP协议自身不对请求和响应之间的通信状态进行保存
协议对于发送过的请求或响应都不做赤计划处理
这是为了更快的处理大量事物。
至少目前HTTP1.1是无状态的
为了实现保持用户状态的功能，引入了会话机制（cookie管理用户状态）
![无状态示例图](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/wuzhuangtai.png)

### HTTP可使用的方法

GET:获取资源，请求资源是文本就原样返回，动态程序就返回执行后的结果
POST:传输实体主题，POST的主要目的并不是获取响应的主题内容，而是传输内容给服务端
PUT:用来传输文件，PUT方法自身不带验证机制，具有幂等性
HEAD:获得报文首部，和GET方法一样，只返回HTTP响应首部
DELETE:删除文件，和PUT作用相反
OPTIONS:询问支持的方法，获取支持的访问方法
TRACE:追踪路径
CONNECT:要求用隧道协议链接代理


### 持久链接节省通信量
HTTP协议的初始版本中，每进行一次通信就要断开一次TCP链接
![建立-断开TCP连接图](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/jianliheduankai.png)

同一个页面包含多个静态资源的时候，就会浪费很多没必要的资源去链接和断开链接
HTTP/1.1 和一部分的HTTP/1.0有了keep-alive，只要任意一端没有明确提出断开链接，则保持TCP链接状态。类似打包
![keepalive](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/keepalive.png)

持久链接减少了TCP链接的重复建立，断开链接所造成的额外开销
HTTP/1.1中，默认所有链接都是持久链接
持久链接，多数请求以管线化方式发送
并行发送多个请求，不需要一个接一个的等待响应(浏览器不同)


## HTTP请求报文
请求端的HTTP报文叫做请求报文
响应端的HTTP报文叫做响应报文

报文一般分为两部分，报文首部，空行，报文主题（不一定有）
![报文结构组成配图](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/baowenjiegou.png)

HTTP请求头中的range

### HTTP请求报文首部字段
通用首部字段：请求报文和响应报文两方都会使用的字段
请求首部字段：从客户端发起请求的补充字段
响应首部字段：从服务端返回响应的补充字段
实体首部字段：针对请求和响应报文的实体部分使用的字段，补充了资源的更新时间和实体的信息
HTTP1.1规范了47种首部字段，非HTTP1.1正式规范的字段也有很多很多，例如Cookie、Set-Cookie


### HTTP的瓶颈
![http瓶颈](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/httppingjing.png)

### 服务器推和主动轮询
服务器推：长时间的占用链接不能释放
主动轮询：频繁创建链接、断开链接，消耗很多资源

### SPDY 2010年谷歌发起

### 使用浏览器进行全双工通信的webSocket
刚开始只是H5标准的一部分，现在逐渐变成独立的协议标准
基于http协议基础之上的协议或者规范
web浏览器与web服务器之间，全双工通信标准
一旦web服务器与客户端之间建立起了websocket链接，之后所有的通信都依靠这个协议进行
通信中可以互相发送json、xml、html或图片等任意格式的数据
通信中可以互相发送报文
支持服务器推送，不必等待客户端请求
建立通信以后就一直保持着连接状态，减少了开销
为了实现websocket通信，在http连接建立之后，需要完成一次握手


![websocket握手配图](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/websocketwoshou.png)
成功握手确立websocket连接，通信不再使用http的数据帧

![websocekt通信配图](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/websockettongxin.png)
![websocket响应](https://raw.githubusercontent.com/codesway/static/master/%E5%9B%BE%E8%A7%A3HTTP/websocketxiangying.png)
JS可以使用websocket的API



## HTTP2.0
在2014年11月实现标准化













