​										 **Chapter 1: Computer Networks and the Internet**

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

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230103234856577.png" alt="image-20230103234852552" style="zoom:33%;" />

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

3.<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230104112948239.png" alt="image-20230104112948239" style="zoom:33%;" />

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



# The Network Core 网络核心