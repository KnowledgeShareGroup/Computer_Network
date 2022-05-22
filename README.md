# 计算机网络 Computer_Network
## [网络协议书籍参考](https://gaia.cs.umass.edu/kurose_ross/wireshark.php)

## 互联网协议介绍Intro
互联网的核心是一系列协议，Internet Protocol Suite。他们对电脑如何连接和组网，做出了详尽的规定。
大家都遵守的规则，就叫做“协议”。
-[1][互联网协议入门](https://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)

### 五层模型

![five-layer-model](/images/五层.png)

### Ethernet 协议
早期的时候，每家公司都有自己的电信号分组方式。逐渐地，一种叫做"以太网"（Ethernet）的协议，占据了主导地位。
以太网规定，一组电信号构成一个数据包，叫做"帧"（Frame）。每一帧分成两个部分：标头（Head）和数据（Data）。
最底层的以太网协议规定了电子信号如何组成数据包packet，解决了子网内部的点对点通信
### IP 协议
规定网络地址的协议，叫做IP协议，它所定义的地址被称为IP地址。IP协议定义了一套自己的地址规则，称为IP地址。
它实现了路由功能，允许由某个局域网的A主机，向另一个局域网的B主机发送消息。
根据IP 协议发送的数据，就叫做IP数据包。不难想象，其中必定包括IP地址信息
### UDP 协议
需要在数据包中加入端口号，几乎就是在数据前面，加上端口号。
### TCP 协议
可以近似的认为它是有确认机制的UDP协议，每发出一个数据包都要确认。如果有一个数据包出现丢包，就收不到确认，发出方
就有必要重发这个数据包了.

**TCP/IP 是用于因特网(Internet)的通信协议**
***TCP (Transmission Control Protocol)和UDP(User Datagram Protocol)协议属于传输层协议。其中TCP提供IP环境下的数据可靠传输，它提供的服务包括数据流传送、可靠性、有效流控、全双工操作和多路复 用。通过面向连接、端到端和可靠的数据包发送。通俗说，它是事先为所发送的数据开辟出连接好的通道，然后再进行数据发送；而UDP则不为IP提供可靠性、 流控或差错恢复功能。一般来说，TCP对应的是可靠性要求高的应用，而UDP对应的则是可靠性要求低、传输经济的应用。 TCP支持的应用协议主要有：Telnet、FTP、SMTP等； UDP支持的应用层协议主要有：NFS（网络文件系统***
- [1][阮一峰关于TCP协议](https://www.ruanyifeng.com/blog/2017/06/tcp-protocol.html)
### DNS
[DNS原理](https://www.ruanyifeng.com/blog/2016/06/dns.html)

![DNS](/images/DNS_sample.png)

### 报文/帧/数据包的区别
|层     |名称                                           |
|-------|-----------------------------------------------|
|应用层 |报文 message: 一般指完整的信息,传输层实现报文交付  |
|传输层 |报文段 segment: 组成报文的每个分组                |
|网络层  |数据包 datapacket: 是TCP/IP通信协议传输中的数据单元; 数据报 datagram: 通过网络传输的基本数据单元称为数据报 Datagram                                        |
|链路层  |帧 frame: 数据链路层的协议数据单元                |
|物理层|PDU: 协议数据单元                                  |
## 阶段性总结

![data package](/images/data_frame.png)

网络通信就是交换数据包，电脑A向电脑B发送一个数据包，后者收到了，回复一个数据包从而实现两台电脑之间的通信。
发送一个数据包，需要知道两个地址
- [1] MAC 地址
- [2] IP 地址
有了这两个地址，数据包才能送到接收者手中。但是如果两台电脑不在同一个子网络的话，就无法知道对方的MAC地址，必须通过网关 gateway来转发.
