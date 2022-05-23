# 1、HTTP历史

1. 70 年底 TCP/IP 协议诞生，80 年代进入 unix 内核
2. 89 年 WEB 诞生
   * URI：统一资源标识符
   * HTML：超文本标记语言
   * HTTP：超文本传输协议

3. 90 年代初，HTTP0.9 诞生，只能传入简单文本，只支持 GET请求
4. 93 年，NCSA（美国国家超级计算应用中心），开发出了一个图文并排的浏览器，开发出了服务器软件 Apache。

​	   92 年，发明了JPEG图像格式，95 年发明了 MP3 格式。

	5. 96年，Http1.0 发布：
	
	*  POST 请求等
	* 响应状态码，可能的错误原因
	* 协议版本号
	* HTTP header概念
	* 传输的数据不仅限于文本
	
	6. HTTP/1.1 作为严格标准
	
	* PUT、DELETE方法
	* 缓存管理和控制
	* 持久连接，连接管理
	* 响应数据块，用于大数据文件
	* 强制要求Host头（互联网主机托管成为可能）
	
	7. HTTP/2（http1.0 连接慢，Google SPDY 协议推动了 http2 的诞生）
	
	* 二进制协议，不在是纯文本
	* 可以发起多个请求，废弃 http1.1 的通道
	* 算法压缩头部，减少数据传输量
	* 服务器可向客户端推送数据
	* 加密通信
	
	衍生出gRPC新协议

8. HTTP/3 进入标准化定制（ Google QUIC 推动）



# 2、HTTP 是什么？不是什么？

### **HTTP 是什么？超文本传输协议**

> **HTTP 是一个在计算机世界里专门在两点之间传输文字、图片、音频、视频等超文本数据的约定和规范**

* 双向协议

* 计算机中，两点之间传输数据的协议和规范

* 超文本：文字、图片、视频，超链接：从一个文本跳到另一个文本，形成网状关系。

  > 例如 HTML ：标签解析

### HTTP不是什么？

* 不是存在的实体，只是一个协议；

* 不是互联网，互联网中有很多协议，普通文件 FTP ，电子邮件 SMTP POP3；

* 不是编程语言，http 是计算机和计算机的交流；

* 不是 html ，html 是超文本的载体，HTTP 传输最多的是html；

* 不是孤立的协议：HTTP 经常跑在 TCP/IP 协议栈上，IP 协议实现寻址和路由，TCP 实现可靠数据传输，

  DNS 实现域名查找，SSL/TLS 实现安全通信。WebSocket、HTTPDNS 依赖 HTTP 协议。

![img](http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/2781919e73f5d258ff1dc371af632acc.png)



# 3、HTTP相关各种概念

<img src="https://static001.geekbang.org/resource/image/51/64/5102fc33d04b59b36971a5e487779864.png" alt="img" style="zoom:50%;" />

### 网络世界

互联网 ≠ 万维网

还有 SSH 安全登录、即时通讯服务、电子邮件、BT、FTP文件下载。



### 浏览器

>  **检索查看互联网上网页资源的应用程序 —— 客服端**

 还具备 HTML 排版引擎、JAVAScript 引擎实现动态效果、开发者工具、插件扩展等。



### WEB 服务器

>  分硬件和软件

**硬件：**物理形式或者云形式主机，目前表现为反向代理、负载均衡的集群。

**软件**：提供 web 服务的应用程序。处理磁盘网页、图片静态文件，转发给tomcat、node.js等业务应用，返回动态信息。

* Apache
* Nginx
* Jetty、tomcat
* windows IIS

### CDN

> Content Delivery Network，内容分发网络。利用 HTTP 协议缓存和代理技术，代理源站响应客户端的请求

* 网络加速：CDN 的调度算法很优秀，可以找到里用户最近的节点，大幅缩短响应时间。
* 还提供负载均衡、安全防护、边缘计算、跨运营商网络功能。



### 爬虫

基本技术：HTTP、HTML



### HTML/WebService/WAF

##### HTML: 

​	HTML4 和 HTML5

##### Web Service:

* 不同于 Web Server。
* W3C 定义的应用服务开发规范，client-server 主从架构，WSDL定义服务接口，HTTP 协议传输 XML 或 SOAP 消息。基于 Web(HTTP) 的服务架构技术。

##### WAF：

* 应用网络防火墙，专门检测 HTTP 流量，防护 Web 应用的安全技术。

* 通常位于 Web 服务器之前，阻止 SQL 注入、跨站脚本攻击。能集成 nginx，应用较多的开源项目 ModSecurity.



<img src="https://static001.geekbang.org/resource/image/1e/81/1e7533f765d2ede0abfab73cf6b57781.png" alt="img" style="zoom: 33%;" />

### TCP/IP

##### TCP:

> Transmission Control  Protocol，传输控制协议**，位于 IP 协议之上，基于 IP 协议提供可靠、字节流形式的通信。
>
> 可靠：数据不丢失	字节流：数据完整

##### IP 协议：

> Internet Protocol，解决寻址和路由问题。IP 地址可以解决互联网上的每一台计算机。



### DNS

> Domain Name System，域名系统。

* “域名“（Domain Name）又叫“主机名”（Host）
* 世界有13组根 DNS 服务器



### URI/URL

> URI，统一资源标识符，标识互联网上唯一资源。
>
> URL，统一资源定位符，”网址“。

三部分组成：

* 协议名：访问该资源时使用的协议。“http”
* 主机名：域名或 IP 地址。"nginx.org"
* 路径：资源在主机上的位置。”/api/en/download.html"



### HTTPS

> 全称是 "HTTP over SSL/TLS"，运行在 SSL/TLS 协议上的 HTTP。
>
> SSL/TLS 是建立在 TCP/IP 上。

SSL 全称是 ”Secure Socket Layer“，发展到 3.0 被标准化，改名为 TLS，”Transport Layer Security“。

综合了加密对称、非加密对称、摘要算法、数字签名、数字证书等技术。



### 代理

> 中转站，可以转发客户端请求，可以转发服务端应答。

##### 分类：

1. 匿名代理：外界只看到代理服务器。
2. 透明代理：外界既知道代理，也知道客户端。
3. 正向代理：靠近客户端，代表客户端向服务器发送请求。比如 VPN
4. 反向代理：靠近服务器，代表服务器响应客户端请求。

##### 功能：

1. 负载均衡
2. 内容缓存
3. 安全防护
4. 数据处理



# 4、计算机网络分层

### TCP/IP 网络分层模型

<img src="https://static001.geekbang.org/resource/image/2b/03/2b8fee82b58cc8da88c74a33f2146703.png" alt="img" style="zoom: 25%;" />

1. **连接层**（link layer）：以太网、WIFI 底层网络上发送原始数据包，工作在网卡层次，使用 MAC（Media Access Control Address） 地址标记网络设备，也叫 MAC 层。
2. **网际层 或 网络互连层**（Internet layer）：IP 协议处于这一层。在“连接层”基础上，以 IP 地址取代 MAC 地址，在网络中找设备只要把 IP 地址再翻译成 MAC 地址即可。
3. **传输层**（transport layer）：TCP 和 UDP 处于这一层。保证数据在 IP 地址标记的两点间可靠的传输。

> ​	TCP 是有状态，先建立连接，在发送数据。UDP 是无状态，直接发送数据，不保证可靠性。

4. **应用层**（application layer）：各种面向具体的协议，HTTP、SSH、FTP、SMTP等。

> ​	MAC 层的传输单位是帧（frame），IP 层的传输单位是包（packet），TCP 层的传输单位是段（segment），HTTP 的传输单位	则是消息或报文（message）。但这些名词并没有什么本质的区分，可以统称为数据包。



### OSI 网络分层模型

> **开放式系统互联通信参考模型**（Open System Interconnection Reference Model）

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/3abcf1462621ff86758a8d9571c07cdc.png" alt="img" style="zoom:25%;" />

1. 第一层：物理层，网络物理形式，电缆、光纤、网卡、集线器等
2. 第二层：数据链路层，TCP/IP 连接层 MAC
3. 第三层：网络层，TCP/IP 网际层 IP
4. 第四层：传输层，TCP/IP 传输层 TCP
5. 第五层：会话层，维护网络中的链接状态，保持会话和同步
6. 第六层：表示层，数据转换为合适、可理解的语法和语义。
7. 第七层：应用层，面向具体的应用传输数据。



### 两个分层模型的映射关系

<img src="https://static001.geekbang.org/resource/image/9d/94/9d9b3c9274465c94e223676b6d434194.png" alt="img" style="zoom:25%;" />

OSI 的分层模型在四层以上分的太细，TCP/IP 实际应用时的会话管理、编码转换、压缩等经常联系的很紧密难以分开。

HTTP 协议同时包含了连接管理和数据格式定义。

**”四层负载均衡“**：工作在传输层上，基于 TCP/IP 协议，例如 IP 地址、端口号实现对后端服务的负载均衡。

**“七层负载均衡”**：工作在应用层，如果是 HTTP 协议，解析 HTTP 协议报文里的 URI、主机名、资源类型等数据，采用适当策略转发给后端服务器。



### TCP/IP 协议栈工作方式

<img src="https://static001.geekbang.org/resource/image/70/6f/70bc19acacf2245fa841349f15cb7a6f.png" alt="img" style="zoom:25%;" />



# 5、域名

> 对 IP 地址的二次抽象。极客时间的域名“time.geekbang.org”，这里的“org”就是顶级域名，“geekbang”是二级域名，“time”则是主机名。

Nginx Web 服务器中，域名可以用来标识虚拟主机，决定哪个虚拟主机对外提供服务。

``` nginx
server {
    listen 80;                       #监听80端口
    server_name  time.geekbang.org;  #主机名是time.geekbang.org
    ...
}
```

### 域名解析

DNS 核心系统是一个三层树状、分布式服务，基本对应域名结构：

1. 根域名服务器（Root DNS Server）：管理**顶级域名服务器**，返回 “com“ ”net“ ”cn“等顶级域名的IP地址。

2. 顶级域名服务器（Top-leavel DNS Server）：管理各自域名下的**权威域名服务器**，com 顶级域名服务器可以返回 apple.com 域名服务器的 IP 地址。

3. 权威域名服务器（Authoritative DNS Server）：管理自己域名下**主机**的 IP 地址。

   <img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/6b020454987543efdd1cf6ddec784bf2.png" alt="img" style="zoom:25%;" />

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220408181942806.png" alt="image-20220408181942806" style="zoom: 50%;" />

**问题：**所有人都访问，会导致核心DNS系统压力巨大。因此需要两种手段减轻压力（基本思路是**缓存**）：

1. **非权威域名服务器**：大公司建立，作为用户 DNS 的代理，代替用户访问核心 DNS 系统，会缓存之前的查询结果，如果有记录则无需再请求根服务器，直接返回对应的 IP 地址。

> ​	知名 DNS ：Google 的”8.8.8.8“，Microsoft 的”4.2.2.1“，CloudFlare 的”1.1.1.1“等。

2. **操作系统会对DNS解析结果做缓存**。另外，操作系统还有一个特殊主机映射文件 hosts，如果在缓存中找不到，则会去这个文件查找。

<img src="https://static001.geekbang.org/resource/image/e5/ac/e51df3245609880641043af65bba94ac.png" alt="img" style="zoom:25%;" />

Nginx 中一条配置指令 resolver ，配置 DNS 服务器，如果没有它，Nginx 无法查询域名对应的 IP，无法反向代理到外部的网站。

```nginx	
resolver 8.8.8.8 valid=30s;  #指定Google的DNS，缓存30秒
```

### 域名玩法

1. 重定向。域名代替 IP 地址，可以让外服务器域名不变，主机 IP 地址随意变动。可以更改 DNS 记录，让域名指向其它机器。

2. 开源软件（例如 bind9）搭建一个内部 DNS，作为名字服务器。这样内部的各种内部服务都用域名标记，例如数据库服务 ”mysql.inner.app“，商品服务”goods.inner.app“。

3. 基于域名实现的负载均衡。

   * 一个域名可以解析多个 IP 地址，即多台主机。客户端收到多个 IP 地址，轮询算法依次向服务器发起请求，实现负载均衡。

   * 域名解析配置内部策略，返回离客户端最近的主机或质量最好的主机。在 DNS 端把请求发送到不同服务器，实现负载均衡就。

**恶意 DNS:**

1. "域名屏蔽"，域名直接不解析，返回错误。
2. ”域名劫持”，也叫”域名污染“，访问 A 网站，给了你 B 网站。



