## 常用命令

#### 概念

网关： 通常指路由器的 IP 地址。当目标地址不在本地网络时，数据包会被发送到网关，由网关进行下一步转发。

默认路由： 路由表中的一条特殊规则，当数据包的目标地址不匹配任何其他更具体的路由规则时，就会使用这条默认路由。可以理解为“最后的出路”。

#### 查看路由表
```
# 查看默认网关
ip route show default
# 或者
ip route | grep default

# 查看完整路由表（包含网关信息）
ip route show
```

#### 添加默认路由
```
ip route add default via <网关IP> dev <接口名>
示例：设置默认网关为 192.168.1.1，使用 eth0 接口
ip route add default via 192.168.1.1 dev eth0

删除
ip route del default
```

#### 添加静态路由
```
ip route add <目标网络/掩码> via <网关IP> dev <接口名>
# 示例：要去往 172.16.0.0/16 网络，需要通过 192.168.1.254 这个网关
ip route add 172.16.0.0/16 via 192.168.1.254 dev eth0

删除
ip route del <目标网络/掩码>
# 示例：删除之前添加的到 172.16.0.0/16 的路由
ip route del 172.16.0.0/16
```

#### 多个网卡

```
default via 192.168.1.1 dev eth0        # 默认路由，走 eth0
192.168.1.0/24 dev eth0 scope link      # 本地网络路由，走 eth0
10.10.10.0/24 dev eth1 scope link       # 本地网络路由，走 eth1
```

需要添加一条静态路由，明确告诉系统：“要去 172.16.0.0/16 网络，请走 eth1 和网关 10.10.10.1”。

```
sudo ip route add 172.16.0.0/16 via 10.10.10.1 dev eth1
```

### 默认转发

#### 数据包转发功能（IP Forwarding）

数据包转发功能，而这个功能在Linux中是默认禁用的，需要手动启用才能让系统充当路由器（Linux是否充当路由器角色，在不同网络接口间转发数据包）。

Linux 的默认设置是：`禁用`数据包转发。

```
# 检查当前状态
cat /proc/sys/net/ipv4/ip_forward
# 输出: 0 （表示禁用）
```

Linux 默认只能处理以自身为目标的数据包，收到目标地址不是本机的数据包时，默认会丢弃，系统不会在不同网络接口之间路由数据包。

临时启动(重启后失效)
```
# 启用 IPv4 转发
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

# 启用 IPv6 转发
echo 1 | sudo tee /proc/sys/net/ipv6/conf/all/forwarding
```

默认禁止的原因：
* 最小权限原则：大多数 Linux 主机是终端设备，不是路由器
* 安全防护：防止系统意外成为网络攻击的中转站
* 性能优化：减少不必要的包处理开销
* 简化配置：避免普通用户无意中创建复杂网络环境

## 华为设备

```
display mac-address // 命令用来查看所有类型的MAC地址表项信息。
```

## 来源

https://support.huawei.com/enterprise/zh/doc/EDOC1000178145/7404084d#display_mac-address_1
