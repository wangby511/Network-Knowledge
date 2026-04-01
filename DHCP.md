## DHCP

### 过程

* 客户端 广播发送DHCP Discover报文
* 服务器 回应DHCP Offer报文
* 客户端 广播发送DHCP Request报文
* 服务器 回应DHCP ACK报文

DHCP中继：负责转发DHCP client和server之间的报文，协助server向client动态分配网络参数。

把广播变成单播
