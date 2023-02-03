​                                                   [小林coding](https://xiaolincoding.com/network/#%E9%80%82%E5%90%88%E4%BB%80%E4%B9%88%E7%BE%A4%E4%BD%93)

​		 **Chapter 1: Computer Networks and the Internet**

# 1.1 What is the Internet?

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230103232125283.png" alt="image-20230103232125283" style="zoom:50%;" />

## 专业名词汇总

**host:主机	end system:端系统	packet:分组(数据包)**

**packet switch:分组交换,包交换**	

fiber, copper, radio, satellite:光纤、铜、无线电、卫星

**bandwidth:带宽**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230103232650348.png" alt="image-20230103232650348" style="zoom:50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230103233046999.png" alt="image-20230103233046999" style="zoom:50%;" />

*Protocols* *define the* ***format,* *order*** *of* *messages sent and received* *among network entities, and* ***actions** taken* *on message transmission, receipt* 

协议定义了网络实体之间发送和接收的消息的**格式、顺序**，以及对消息传输、接收**采取的操作**



## 课后习题

1.以service视图看待Internet

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230103233437964.png" alt="image-20230103233437964" style="zoom:50%;" />

**==对于'应用创造者'和'应用使用者'两方面的'service'==**

# 1.2 The Network Edge 网络边缘

1.关于**网络边缘,接入网络,物理介质和网络核心**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230103234902955.png" alt="image-20230103234902955" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230103234856577.png" alt="image-20230103234852552"  />

2.![image-20230103235840930](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230103235840930.png)

(1) 如何将端系统连接到边缘路由器

+ 用户接入网
+ 机构接入网(学校,公司)
+ 移动设备接入网(WIFI, 5G)

(2) **频分复用 FDM**

(3) HFC 混合光纤同轴电缆

**非对称** : 高达40 Mbps–1.2 Gbs下行传输速率，30-100 Mbps上行传输速率

cable headend : 有线电视数据转发器

(4) 通过DSL,  互联网和电话线共用一段线路

**也是非对称的**, 下行 > 上行

3.<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230104112948239.png" alt="image-20230104112948239"  />

(1) 路由器的那一端可实现firewall,NAT

(2) 无线接入网络:

+ 无线局域网:WLAN wireless local area networks 

+ 广域蜂窝接入网:wide-area cellular access networks (5G)

(3)企业网络接入

(4)数据中心网络接入 

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230104113842453.png" alt="image-20230104113842453"  />

(1)数据包传输延迟: L / R

(2) 双绞线TP: twisted pair

+ 五类线:100Mbps 1Gbps 以太网
+ 六类线:10Gbps

(3)coaxial cable:同轴电缆	两个同心铜导体,双向传播

Fiber optic cable:光线网线	高速(10-100Gbps),低错率

(4)无线电:

+ ==**广播 broadcast**==
+ ==**半双工 half-duplex**==

无线电链路类型:

+ 无线局域网 WIFI 
+ 广域 5G
+ 蓝牙bluetooth: 短距离,有限的速率, 电缆的替代
+ 地面微波
+ 卫星

## 专业名词

network edge 网络边缘		network core 网络核心

cable 电缆		modem:调制解调器 

ISP: Internet service provider 互联网服务供应商

DSL : digital subscriber line  数字用户专线 



access point : 无线接入点/访问点

## 课后习题

1.

+ Ethernet 以太网:

  > Wired. Up to 100's Gbps per link.

+ 802.11 WiFi  

  >  Wireless. 10’s to 100’s of Mbps per device.

+ Cable access network 电缆接入网络

  > Wired. Up to 10’s to 100’s of Mbps downstream per user.

+ Digital Subscriber Line (DSL)

  > Wired. Up to 10’s of Mbps downstream per user.

+ 4G cellular LTE (长期演进技术(Long Term Evolution))

  > Wireless. Up to 10’s Mbps per device.



<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230104160226025.png" alt="image-20230104160226025" style="zoom:33%;" />

2.拥有最高传输速率和最低误码率的是

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230104160107575.png" alt="image-20230104160107575" style="zoom:33%;" />



# 1.3 The Network Core 网络核心

![image-20230127223318925](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127223318925.png)

> **==forwarding(转发)是local(本地)方法, Routing(路由)是全局方法==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127223452384.png" alt="image-20230127223452384" style="zoom:25%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127223504008.png" alt="image-20230127223504008" style="zoom:25%;" />

> 类比routing和forwarding

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127224001801.png" alt="image-20230127224001801" style="zoom: 25%;" />

> 排队时延的产生, 且内存耗尽时有可能会丢失packet

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127224054509.png" alt="image-20230127224054509" style="zoom:25%;" />

> 回顾一下电话使用的电路交换 "call"

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127224233226.png" alt="image-20230127224233226" style="zoom:25%;" />

> ==**FDM和TDM是电路交换中的!!!!!!!!!!!!!!**==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127224855586.png" alt="image-20230127224855586" style="zoom:25%;" />

> 35个用户中有10个以上同时传输数据的可能是0.0004,很小,所以我们可以放入35个用户, 而电路交换只能放入10个

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127225159370.png" alt="image-20230127225159370" style="zoom:25%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127225112047.png" alt="image-20230127225112047" style="zoom: 25%;" />

> ISP, IXP

## 课后习题

1.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127225422920.png" alt="image-20230127225422920" style="zoom: 50%;" />

> ==**再次注意, FDM TDM是电路交换的技术**==

2.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127225537587.png" alt="image-20230127225537587" style="zoom: 50%;" />

3.

![image-20230127225736340](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127225736340.png)

> **最大连接数是所有连接的总和**

4.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127230450931.png" alt="image-20230127230450931" style="zoom: 50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127230623385.png" alt="image-20230127230623385" style="zoom:50%;" />

5.

![image-20230127230708796](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127230708796.png)

6.

![image-20230127230815242](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127230815242.png)

> "平均"与"瞬时"

# 1.4 Performance: Delay, Loss and Throughput in Computer Networks 性能：计算机网络中的延迟、损失和吞吐量

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127234016370.png" alt="image-20230127234016370" style="zoom: 33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127234202548.png" alt="image-20230127234202548" style="zoom: 33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127234708297.png" alt="image-20230127234708297" style="zoom: 33%;" />

> traceroute程序

## 课后习题

### 1

Processing delay : Time needed to perform an integrity check, lookup packet information in a local table and move the packet from an input link to an output link in a router.执行完整性检查、在本地表中查找数据包信息以及在路由器中将数据包从输入链路移动到输出链路所需的时间。

Propagation delay : Time need for bits to physically propagate through the transmission medium from end one of a link to the other.比特通过传输介质从链路的一端物理传播到另一端所需的时间。

Queueing delay : Time spent waiting in packet buffers for link transmission.在数据包缓冲区等待链路传输所花费的时间。

Transmission delay : Time spent transmitting packets bits into the link.将数据包位传输到链路所花费的时间。

### 2

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127235502148.png" alt="image-20230127235502148" style="zoom:33%;" />

L / R 是传输时延(发送时延)

3.略(和2大同小异)

### 4

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127235651769.png" alt="image-20230127235651769" style="zoom:33%;" />

L / R -> 8000 / 100 Mbps 答案是B

### 5

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127235844647.png" alt="image-20230127235844647" style="zoom:33%;" />

答案是D

1000Km / 3 x 10 ^ 8

### 6

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128000056081.png" alt="image-20230128000056081" style="zoom:33%;" />

答案是B

剩下的题略 ,与章节习题chong'fu

# 1.5 Protocol layers and Their Service Models

![image-20230128105017098](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128105017098.png)

> message, segment, datagram, frame

## 课后习题

### 1

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128105354764.png" alt="image-20230128105354764" style="zoom: 33%;" />

> 进程与进程之间的数据传输 : 传输层(Transport layer)

### 2

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128105654133.png" alt="image-20230128105654133" style="zoom: 33%;" />

### 3

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128105825508.png" alt="image-20230128105825508" style="zoom:33%;" />

# 1.6 Networks under attack

> 有专门的一章讲网络安全, 此小节就是概述一下
>
> 大致了解 "坏人能做什么"和"我们能做些什么防御"

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128110359483.png" alt="image-20230128110359483" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128110403589.png" alt="image-20230128110403589" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128110407882.png" alt="image-20230128110407882" style="zoom:33%;" />

+ **authentication(身份验证)**: proving you are who you say you are
  + **cellular networks provides hardware identity via SIM card**; **no such hardware assist in traditional Internet**

+ **confidentiality(机密性)**: via encryption(加密)

+ **integrity checks(完整性检查)**: digital signatures prevent/detect tampering 数字签名**防止/检测篡改**

+ **access restrictions(访问限制)**: password-protected VPNs 受密码保护的VPN

+ **firewalls(防火墙):** specialized “middleboxes” in access and core networks:
  + off-by-default: filter incoming packets to restrict senders, receivers, applications 默认关闭：过滤传入数据包以限制发件人、收件人和应用程序
  + detecting/reacting to DOS attacks 检测/响应DOS攻击

# 1.7 History of Computer Networking; Chapter 1 Summary

> 略

