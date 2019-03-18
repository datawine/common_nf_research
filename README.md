# common_nf_research

对一些常见或者popular的nf进行调研

NAT
- 资料来源
    - S6框架
    - https://tools.ietf.org/html/rfc2663
- 维护状态
    - 哈希流表
    - 公网地址池（内网-外网地址及端口映射）
- workflow
    - 通过流表判断新旧连接
    - 维护一个内网-外网地址映射，对于新的连接，创建映射并放到地址池中。一般来说需要重新计算涉及到ip地址的计算（如校验和），都需要重新计算。至于映射的释放，端口的变更，有各种不同的实现考虑。

Firewall(Basic)
- 资料来源
    - S6框架
    - https://github.com/antonioribeiro/firewall
    - https://github.com/puppetlabs/puppetlabs-firewall
- 维护状态
    - 哈希流表
    - 黑白名单
    - 连接的状态（源、目的地址和端口，请求个数，连接时间）
- workflow
    - 通过流表判断新旧连接
    - 如果有新的连接，对黑白名单进行查询，决定是否通过。有些防火墙会统计连接有关的信息，进行动态拦截。

Load balancer
- 参考资料
    - https://github.com/github/glb-director/tree/master/docs
- 维护状态
    - 哈希流表
    - 重定向映射
    - 连接状态
    - 重定向目的服务器状态池
- workflow
    - 根据流表判断新旧连接
    - 一条新的连接需要首先通过哈希ipv4五元组获得一个默认的重定向，然后根据动态负载算法进行微调。
    - 旧连接会根据负载均衡控制器，每隔一段时间进行一次health check，如果有违背规则的连接则会被重定向。

Traffic Monitoring(prads)
- 参考资料
    - http://manpages.ubuntu.com/manpages/xenial/en/man1/prads.1.html
- 维护状态
    - asset(流状态)
    - 全局统计数据
    - 哈希流表
- workflow
    - 根据流表判断新旧连接
    - 新连接创建新的asset对象
    - 根据实际信息更新统计数据（主要统计流量）

IDS/IPS
维护状态
workflow

Web proxy
维护状态
workflow

EPC
维护状态
workflow

IMS
维护状态
workflow

iptables

NIDS cluster

Cisco MGX

F5

A10

Nginx

OpenVPN

Squid

Cisco IOS

Linux tc

NetFlow