## 交换机

工作原理：基于源MAC地址，学习构建MAC地址表，基于目的MAC地址进行转发。

解决MAC-flood方法：静态绑定，端口安全

## 路由器

根据数据包IP头部中的目的IP地址，在路由表中进行查找。最长子网掩码匹配。

### 工作原理

建立并维护路由表RIB，根据路由表进行数据转发。

### 优先级

路由优先级用于区分不同路由协议的优先级，取值0-255，越小越优先。

直连设备 0

OSPF 10

IS-IS 15

静态路由 STATIC 60

RIP 100 （接近淘汰）

### 迭代路由

路由条路中Flag中带R的表示迭代路由，迭代路由不论有多少接口和下一跳，仅统计为1条路由。

### 路由协议分类

#### 距离矢量 & 链路状态

* 距离矢量协议：RIP, BGP

* 链路状态协议（学习计算拓扑）：OSPF, IS-IS

距离矢量路由协议一般基于Bellman-Ford算法；链路状态协议基于Dijkstra算法，也叫SPF最短路径优先算法。E.g. OSPF路由协议基于Dijkstra算法，开销算入接口数量。

#### 内部网关协议 & 外部网关协议

IGP: RIP, OSPF, IS-IS

EGP: BGP

## RIP 

RIP (Routing Information Protocol) 路由信息协议

优先级为100，最大15跳 (16跳意味着不可达，无效路由)，UDP 520端口。RIP以跳数作为度量值，虽然简单但事实上选路可能有问题。

### 防环机制

* 最大跳数15
* 水平分割：一条路由信息不会发送给信息的来源
* 路由毒化的水平分割：把从邻居学习到的路由信息设为16跳，再发送给邻居。
* 抑制定时器和触发更新

## OSPF

OSPF (Open Shortest Path First), 开放式最短路径优先协议

是一种**内部网关协议IGP**，也是**链路状态**路由协议，采用Dijkstra算法。

华为设备OSPF协议优先级**Internal 10， External 150 （import-route）**。External涉及到引入。

支持VLSM，支持在ABR/ASBR手工路由汇总。ASBR上只能聚合由自己引入的外部路由。

OSPF不支持自动汇总，需要手工配置。

允许网络被划分为区域管理：减少对路由器内存和CPU的消耗，降低网络占用带宽，减少路由表震荡增加稳定性。

骨干区域：Area 0或者Area 0.0.0.0, 区域0

hello报文发现邻居，维护邻居关系，在点对点和广播网路中每10秒发送一次hello。

OSPF路由器之间通过LSA（Link State Advertisement，链路状态公告）交换网络拓扑信息。路由器之间交互的是链路状态信息，而不是之间直接交互路由。

### OSPF报文分类

* Hello - 维持OSPF邻居关系
* DD (Database Description) - 描述本地LSDB的摘要信息
* LSR (Link State Request) - 用于向对方**请求**所需的LSA
* LSU (Link State Update) - 用于向对方**发送**所需的LSA
* LSAck (Link State Acknowledgement)- 用于对收到的LSA进行确认

### Hello定时器

广播网络和点对点网络hello是10s。默认都是广播型网络，如以太网。

P2P和Broadcast为10s，NBMA和P2MP为30s。

### router-id

每个MA网段选取一个DR和BDR，作为代表和其他路由器Dother建立邻居关系。

router-id在OSPF区域内唯一标识一台路由器的IP地址，不能设置为相同。

#### OSPF的router-id选举规则

1. 优选手工配置的router-id。

2. 全剧模式下配置的router-id。

3. loopback接口地址中最大的地址作为router-id。

4. 普通物理接口中最大的地址作为router-id。

### Cost

一条OSPF路由的cost由该路由从起源一路到达本地的所有入接口cost值的总和。

### OSPF区域

骨干区域为0，所有非骨干区域必须与骨干区域直连。

如果有区域没有和Area 0相联，可以通过**虚链路**临时解决，只能横穿一个非骨干区域。

### OSPF路由器角色

* 区域内路由器 - Internal Router
* 区域边界路由器ABR - Area Border Router
* 骨干路由器 - Backbone Router (主干区域0)
* AS边界路由器ASBR - AS Boundary Router

## 来源

[1] [bilibili - 路由基础专题 RIP OSPF ISIS BGP](https://www.bilibili.com/video/BV1gM4m117Yh)
