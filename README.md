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
- 参考资料
    - https://www.snort.org/snort3
- 维护状态
    - 黑名单
    - 流表
    - 连接信息(如检测到可疑信息次数等)
- workflow
    - 根据流表判断新旧连接，新连接创建分别的监听对象
    - 持续监听所有连接，检测签名异常、非正常请求、违反协议等。

Web proxy
- 参考资料
    - https://github.com/squid-cache/squid
- 维护状态
    - 流表
    - 以某种形式组织的网页相关cache
- workflow
    - 以上面的squid为例，包括一个主要的服务程序squid，dns查询程序dnsserver，几个重写请求和执行认证的程序
    - 根据流表判断新旧连接，新连接创建新的相关instance
    - 连接请求过的资源会被以Interent Cache Protocol缓存
    - 每次连接请求新的资源，会通过hash中寻找资源，如果没有，可以向sibling node请求资料，如果sibling没有，直接向parent node要数据，parent也没有就会访问internet。然后将请求到的数据缓存下来。

EPC
- 参考资料
    - https://gitlab.eurecom.fr/oai/openair-cn/tree/master/
- 维护状态
    - 用户状态
    - SLA/Usage per device/plan（这是根据S6论文上总结的加上的，但是我并没有看到相应的东西）
- workflow
    - EPC由以下四个部分组成：移动管理实体，用来根据数据管理会话的状态、认证并追踪用户；服务网关；分组数据节点网关，作为LTE网络和其他分组数据网络间的端口，管理服务质量并提供深度数据包检测；政策及收费功能
    - 整个流的流向是比较复杂的一张图，大概经过上述的四个部件，并对相应的数据包进行封装，统计相应的数据

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