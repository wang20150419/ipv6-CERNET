# ipv6-CERNET

openwrt 15.05 比较稳定地 实现 ipv6 NAT的方法：

各种依赖库 自行查找。本人查看了N多教程，这篇文章：http://my.oschina.net/u/444663/blog/509427?fromerr=JEGTryMR 最靠谱

创建 /etc/hotplug.d/iface/90-ipv6，然后重启即可。
    
#!/bin/sh
# opkg install kmod-ipt-nat6 iputils-tracepath6
# put it to /etc/hotplug.d/iface
# gw: $(tracepath6 -n tv.byr.cn | grep ' 1: ' | awk 'NR==1 {print $2}') 
# dev: $(uci -q get network.wan6.ifname)
# firewall.user: ip6tables -t nat -A POSTROUTING -o $(uci -q get network.wan6.ifname) -j MASQUERADE

[ "$ACTION" = ifup ] || exit 0

[ "$INTERFACE" = wan6 ] && {
   # xxxx:xxxx 任取
   ip -6 addr add 4006:E024:xxxx:xxxx::1/64 dev br-lan  
   # 建议 运行 tracepath6, 换成最终结果
   route -A inet6 add default gw  $(tracepath6 -n tv.byr.cn | grep ' 1: ' | awk 'NR==1 {print $2}')  
   # 建议 运行 uci -q get ....，换成最终结果
   ip6tables -t nat -A POSTROUTING -o $(uci -q get network.wan6.ifname) -j MASQUERADE
}

