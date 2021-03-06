[TOC]

# 普遍的网络结构
1.客户端-服务器
2.端-端，无中心

# 如何选择可靠的传输协议
1.可靠性
2.传输量
3.时间
4.安全

# brower send messages to server
Brower->HTTP->socket(like door,open)->
TCP->TCP->socket(like door,open)->HTTP->Server

# tcp
1.数据不会丢失
2.数据片段有排序
3.速度相对于udp慢

# http
没有状态记录，无状态协议
默认情况是持续连接
拥有两种连接方式：持续连接 和 非持续连接

## http服务端发送响应给客户端

```
1. The HTTP client process initiates a TCP connection to the server
www.someSchool.edu on port number 80, which is the default port number for HTTP. Associated with the TCP connection, there will be a socket at the
client and a socket at the server.
2. The HTTP client sends an HTTP request message to the server via its socket. The
request message includes the path name /someDepartment/home.index.
(We will discuss HTTP messages in some detail below.)
3. The HTTP server process receives the request message via its socket, retrieves
the object /someDepartment/home.index from its storage (RAM or
disk), encapsulates the object in an HTTP response message, and sends the
response message to the client via its socket.
4. The HTTP server process tells TCP to close the TCP connection. (But TCP
doesn’t actually terminate the connection until it knows for sure that the client
has received the response message intact.)
5. The HTTP client receives the response message. The TCP connection terminates. The message indicates that the encapsulated object is an HTML file. The
client extracts the file from the response message, examines the HTML file,
and finds references to the 10 JPEG objects.
6. The first four steps are then repeated for each of the referenced JPEG objects.
```

# RTT
 round-trip time(RTT)

# http request message
```
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Connection: keep-alive
Host: www.baidu.com
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.129 Safari/537.36
```
`User-agent`代理发送是用mac chrome
`Accept-Language`语言是中英文

# PUT Method and DELETE Method
```
 The PUT method is often used in conjunction with Web publishing tools. It allows a user to upload an object to a specific path (directory) on a specific Web server. The PUT method is also used by applications that need to upload objects to Web servers. 
 
 The DELETE method allows a user, or an application, to delete an object on a Web server.
```

# Web cache
Web cache is a proxy server(like CDN).The struct like this
```
Brower<->Web cache<->Origin server
```
When Brower send request message to Orgin server.
In fact:
Brower send request message to Web cache.
Web cache check it if Web cache has it on locally.

If Web cache has it,and send it to Origin server.

If Web cache hasn't it,and request Origin server.When Web cache 
receive the response,Web cache save it on locally,and send it to Brower.

# DNS
## Three type of DNS
1.Root DNS servers
2.Top-level domain (TLD) servers:
response like:
```
com,cn,org,uk,jp
```
3.Authoritative DNS servers:
response IP
Authoritative DNS servers set by like 114,alibaba,google.
The process is like this:

suppose request www.baidu.com

1.brower->local DNS->Root DNS(responed local DNS about (com)TLD address)
2.local DNS->(com)TLD DNS(responed local DNS about baidu.com Authoritative DNS address)
3.local DNS->Authoritative DNS(responed local DNS about baidu.com IP)
4.local DNS->(send ip of www.baidu.com to brower)brower

# ip
ip不保证分段交付，不保证顺序交付数据

# tcp
tcp 有阻塞控制来防止网络传输遇到的阻塞
传输层依然保持在各自的host，跨越host传输数据是网络层做的事。传输层就像家里整理打包快递的，网络层就像物流

#  multiplexing
传输层将数据封装外包头部
打包方式:
{
    source port+destination port{
        Other header fields{
            Application data
        }
    }
}

#  demultiplexing
将传输层收到的封装包解开，找到对应的socket（每个socket都有独一无二的标识）

# 网络层
网络层封装传输层给的segment，把这个封装好的塞进ip数据包

# network layer 
every router has forwarding table. When the router received package .It will read forwarding table .Use package header value to query output where. 

# 物理层
发送0，1信号，媒介是电磁波、光缆、电缆等
# 链路层
将0，1信号分组，并定义意义
# 帧
以太网规定，一组电信号构成一个数据包，叫做"帧"（Frame）

# MAC地址
以太网规定，连入网络的所有设备，都必须具有"网卡"接口。数据包必须是从一块网卡，传送到另一块网卡。网卡的地址，就是数据包的发送地址和接收地址，这叫做MAC地址。

依靠mac地址只能在子网络之间通信，而子网络通信都是通过广播形式传播，效率低，每台机子都会收到包，很不好。

网络如果想连接成功必须知道mac地址和ip地址

# 子网掩码
两台电脑的子网掩码加上ip地址判断是否在同一个子网络

# ARP协议
前提：知道接收者的ip地址

如果两台机子都在同一个子网络下，通过广播的方式找到与自己需要找的ip地址相匹配的ip地址。找到后，那台机子发送mac地址

如果两台机子不在一个子网络，则通过两个子网络之间的网关来判断

# DHCP协议
自动分配ip的协议

# 交换机
存储mac地址表的地方，并转发到要去的mac地址