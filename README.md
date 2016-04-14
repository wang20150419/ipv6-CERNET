# ipv6-CERNET

教育网双栈IPv6 在路由系统 openwrt 15.05 下稳定地 实现 ipv6 NAT 的方法：

各种依赖库 自行查找。本人查看了N多教程，这篇文章：http://my.oschina.net/u/444663/blog/509427?fromerr=JEGTryMR 最靠谱

创建 /etc/hotplug.d/iface/90-ipv6 ，然后重启即可。
