## ToS

Type of Service 8 bit: 

服务类型TOS，8bit，前3bit弃用，中间4bit表示如下，最后1bit必须置0。

TOS所说的服务类别，是指4bit字段：最小延时，最大吞吐量，最高可靠性，最小费用：

* 1000 – minimize delay #最小延迟 对应于对延迟敏感的应用，如telnet和人login等。
* 0100 – maximize throughput #最大吞吐量 对应于对吞吐量要求比较高的应用，如FTP文件应用，对文件传输吞吐量有比较高的要求。
* 0010 – maximize reliability #最高可靠性 对网络传输可靠性要求高的应用，如使用SNMP的应用、路由协议等等。
* 0001 – minimize monetary cost #最小费用
* 0000 – normal service #一般服务

总结：TOS总共占8个比特位，3bit弃用 + 4bit类型 + 1bit的0。

IPv6用Traffic Class流量类别替代了IPv4中的服务质量Type of Service，TOS仅对流量进行分类。

————————————————

### 资料来源

[1] https://blog.csdn.net/inthat/article/details/127348341
