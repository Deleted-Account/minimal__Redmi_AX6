obsoleted.
new version with NSS: https://github.com/Deleted-Account/xiaomi-ax6-openwrt-build


---

2022年12月03日：

1. 相比11月底，上游源把 v2ray-geoip v2ray-geosite 这两个依赖去掉了，编译出来的固件又轻盈了1.几MB。  
2. ~~Lean 大佬的 高性能模式 FullCone-NAT，看好多网友说开启后会变成 NAT4，我没试过（配置里面我关闭了 luci-app-turboacc_INCLUDE_PDNSD 的安装，那个 高性能模式 FullCone-NAT 也就没有了）。~~  (后来又有了，奇怪)
3. ~~上游源不知道什么时候 (估计 Lean 去掉 AX6 支持之后这段时间不知道什么时候）可以说修复了 IPQ807x 无线"漏油"的问题。现在用 sdf8057 大佬的源编译出来的固件，同时开启 2.4 和 5G WiFi 做无线 AP，可用内存依然保持 110~150MB，日常大概剩余 130MB，已经不会因为内存慢慢泄漏殆尽导致路由重启了。~~ ~~(以为还是会漏，观察了几天，内存掉到100MB又慢慢反弹了，已经无需定时重启。)~~ ~~(好像还是会漏，漏的速度很慢，开双频WiFi，目前可用内存掉到了85，单开5G WiFi，可用内存200+）~~ 平稳运行20多天了，当前可用内存120MB。  

---

2022年11月26日：

1. 上游源换成 sdf8057 大佬的，参考 https://github.com/sdf8057/cloudbuild 已经可以正常编译  
2. 内核是5.10，有线 NSS 驱动正常，kv 漫游正常  
3. 上游默认没安装 luci-ssl-openssl (大部分人用不到这个。有外网 https 登录的需求，才考虑安装这组件)。顺便还添加了上游源默认没安装的 htop。

---

2022年07月31日：

看了些网友的说法，5.15 内核 NSS 跑不起来，  
个人猜测之前是 Lean 大佬移植的，后来跟网友吵翻，就没了 5.15 的 NSS 支持。

所以继续用 5.10.109 养老嘛。

自动编译时间我改了。之前是周二周六的清晨（格林威治时间的周一晚上）。  
那个帮助编译加速的 cache，据说 Github 是保留 7 天。

现在改成： - cron: 15 21 2,7,12,17,22,27 * *  
我算了下，月底如果是 31 天，27 号到 2 号是 6 天，cache 还在。

---

2022年06月11日：

年久失修，我都不晓得现在里面还有没有 NSS

上个月末刷了个 5.15 内核的固件，TurboACC 发现是 SFE,

局域网有线传输千兆，CPU 占用能跑到二三十%，  
然后刷回 5.10.109 一切又正常了。

最近固件忽然涨了6MB的大小（16>>22），我都懒得云编译时候 SSH 上去看什么情况


事多人懒，我就做个 AP，偶尔可能带出门可能要用下梯子，继续用 5.10.109 养老好了

---

基本上只包含 Passwall（SSR，无其他协议）、SmartDNS、SQM 的一个超精简 AX6 固件  

(其实 SQM 也可以删的，NSS 和这货不兼容。不像 x86 固件上打开 SFE，SQM 实测还是有效果)

有 FullConeNAT。考虑了下，UPnP 也删除了

CoreMark跑分没卵用，删

---

11月，NSS 没正常加载，FullconeNAT 打开后断网 的问题，上游已修复


kv漫游回来断网的问题，12 月底 Boos 已修复，感谢大佬，赞美大佬！~

---

1月20号之后，用 Boos 的源云编译，一直失败  
大佬叫用另一个CI，我是小白，不知道如何解决  

当时骚操作是直接 merge Boos 的部分文件到 Lean 的源

(2月17号，又可以正常编译成功了)

---

3月出头，Lean 的源好像大佬们吵架，删掉了所有 XiaoMi 设备的支持。Boos 大佬您可千万别放弃啊，感恩~

---

xiaorouji 把 Passwall 的 LuCI 分出来了

如果我没编译进去，一定是我的姿势不对

目前是用 svn co 命令，加入 https://github.com/xiaorouji/openwrt-passwall 的 shadowsocksr-libev simple-obfs 以及分支下的 luci-app-passwall
