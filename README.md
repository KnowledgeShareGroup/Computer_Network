# 计算机网络 Computer_Network

## 互联网协议介绍Intro
互联网的核心是一系列协议，Internet Protocol Suite。他们对电脑如何连接和组网，做出了详尽的规定。
大家都遵守的规则，就叫做“协议”。
-[1][互联网协议入门](https://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)

### 五层模型
![five-layer-model](/images/五层.png)
### IP 协议
规定网络地址的协议，叫做IP协议，它所定义的地址被称为IP地址。
根据IP 协议发送的数据，就叫做IP数据包。不难想象，其中必定包括IP地址信息
### UDP 协议
需要在数据包中加入端口号，几乎就是在数据前面，加上端口号。
### TCP 协议
可以近似的认为它是有确认机制的UDP协议，每发出一个数据包都要确认。如果有一个数据包出现丢包，就收不到确认，发出方
就有必要重发这个数据包了
-[1][阮一峰关于TCP协议](https://www.ruanyifeng.com/blog/2017/06/tcp-protocol.html)
### DNS
[DNS原理](https://www.ruanyifeng.com/blog/2016/06/dns.html)

## 阶段性总结
网络通信就是交换数据包，电脑A向电脑B发送一个数据包，后者收到了，回复一个数据包从而实现两台电脑之间的通信。
发送一个数据包，需要知道两个地址
- [1] MAC 地址
- [2] IP 地址
有了这两个地址，数据包才能送到接收者手中。但是如果两台电脑不在同一个子网络的话，就无法知道对方的MAC地址，必须通过网关 gateway来转发.
