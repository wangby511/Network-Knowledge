# 基础知识

## 三张表

路由表，MAC地址表，ARP表。

### 路由表的安全

* 认证
* 静默接口
* GTSM(通用TTL安全保护机制)

```
ospf valid-ttl-hops 2 #TTL有效范围254-255
```

### ARP协议工作流程

1. 主机A查询自己的ARP缓存，发现没有主机B对应的MAC地址。
2. 主机A发送ARP Request广播报文
3. 主机B把主机A

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

#### 按工作机制和算法分类

* 距离矢量路由协议：RIP （类似于抄作业）

* 链路状态路由协议（学习计算拓扑）: OSPF, IS-IS（自主计算，保证路由计算准确性）

距离矢量路由协议一般基于Bellman-Ford算法；链路状态协议基于Dijkstra算法，也叫SPF最短路径优先算法。E.g. OSPF路由协议基于Dijkstra算法，开销算入接口数量。

链路状态路由协议中，每台路由器会给邻居发送LSA（Link State Advertisement, 链路状态通告），每个路由器有哪些接口，每个接口地址及哪些邻居。

#### 按工作区域分类

IGP (内部网关协议 Interior Gateway Protocols): RIP, OSPF, IS-IS

EGP (外部网关协议 Exterior Gateway Protocols): BGP

## RIP 

RIP (Routing Information Protocol) 路由信息协议

优先级为100，最大15跳 (16跳意味着不可达，无效路由)，UDP 520端口。RIP以跳数作为度量值，虽然简单但事实上选路可能有问题。

### 防环机制

* 最大跳数15
* 水平分割：一条路由信息不会发送给信息的来源
* 路由毒化的水平分割：把从邻居学习到的路由信息设为16跳，再发送给邻居。
* 抑制定时器和触发更新

## 来源

[1] [bilibili - 路由基础专题 RIP OSPF ISIS BGP](https://www.bilibili.com/video/BV1gM4m117Yh)
