## OSPF

OSPF (Open Shortest Path First), 开放式最短路径优先协议

是一种**内部网关协议IGP**，也是**链路状态**路由协议，采用Dijkstra算法。

华为设备OSPF协议优先级**Internal 10, External 150 (import-route)**。External涉及到引入。

支持VLSM，支持在ABR/ASBR手工路由汇总。ASBR上只能聚合由自己引入的外部路由。

### 优点

采用组播形式收发部分协议报文

支持区域划分

支持对*等价路由*进行负载分担

支持报文认证

### 其他特点

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

### Router ID

Router-ID用于在自治系统中唯一表示一台运行OSPF的路由器，32位无符号整数。

每个MA网段选取一个DR和BDR，作为代表和其他路由器Dother建立邻居关系。

router-id在OSPF区域内唯一标识一台路由器的IP地址，不能设置为相同。

#### Router ID的选举规则

1. 手动配置OSPF路由器的Router ID

```
(AR1)
ospf 1 router-id 6.6.6.6
```
```
dis current-configuration // 查看配置
dis ospf brief // 查看OSPF配置
```

2. 如果没有手动配置Router ID，则路由器使用**Loopback接口地址中最大的IP地址**作为Router ID

```
(AR1)
interface LoopBack 100
ip add 100.100.100.100 32
```

3. 如果没有Loopback接口，则路由器使用**物理接口中最大的IP地址**作为Router ID

### Cost

一条OSPF路由的cost由该路由从起源一路到达本地的所有入接口cost值的总和。

### OSPF Area 区域

区域ID也是一个32bit的非负整数

```
(AR1)
ospf 1
area 0.0.0.3
// area 3
```

骨干区域为0，所有非骨干区域必须与骨干区域直连。

如果有区域没有和Area 0相联，可以通过**虚链路**临时解决，只能横穿一个非骨干区域。

### OSPF度量值

### OSPF路由器角色

* 区域内路由器 - Internal Router
* 区域边界路由器ABR - Area Border Router
* 骨干路由器 - Backbone Router (主干区域0)
* AS边界路由器ASBR - AS Boundary Router

## 来源

[1] [bilibili - 路由基础专题 RIP OSPF ISIS BGP](https://www.bilibili.com/video/BV1gM4m117Yh)
