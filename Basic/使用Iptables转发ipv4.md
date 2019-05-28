# 使用Iptables实现ipv4的转发
```
iptables -t nat -A PREROUTING -p tcp --dport 8888 -j DNAT --to-destination 167.179.88.203:8888
iptables -t nat -A PREROUTING -p udp --dport 8888 -j DNAT --to-destination 167.179.88.203:8888
iptables -t nat -A POSTROUTING -d 167.179.88.203 -p tcp --dport 8888 -j MASQUERADE
iptables -t nat -A POSTROUTING -d 167.179.88.203 -p udp --dport 8888 -j MASQUERADE
iptables -I FORWARD -d 167.179.88.203 -p tcp --dport 8888 -j ACCEPT
iptables -I FORWARD -s 167.179.88.203 -p tcp --sport 8888 -j ACCEPT
iptables -I FORWARD -d 167.179.88.203 -p udp --dport 8888 -j ACCEPT	
iptables -I FORWARD -s 167.179.88.203 -p udp --sport 8888 -j ACCEPT
```

- [CentOS配置iptables规则并使其永久生效 - 禁忌夜色153 - 博客园](https://www.cnblogs.com/jinjiyese153/p/8600855.html)
