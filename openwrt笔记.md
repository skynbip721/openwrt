openwrt系统下载（不同版本、选择适合自己的版本就可以了）
原本
https://downloads.openwrt.org/releases/24.10.1/targets/x86/generic/

大神修改版
https://openwrt.ai/?target=x86%2F64&id=generic
https://downloads.openwrt.org/releases/24.10.1/targets/x86/generic/
https://downloads.immortalwrt.org/
https://immortalwrt.kyarucloud.moe/
---------------------------------------------------------------------------------------------------------------
使用VirtualBox把IMG文件转换为VDI文件

必须安装了VirtualBox虚拟机，在虚拟机安装目录下操作
语法：
$ VBoxManage convertdd input.img output.vdi
$ VBoxManage convertdd input.img output.vdi

如下：
C:\Program Files\Oracle\VirtualBox>VBoxManage convertdd E:\openwrt\istoreos-24.10.1-2025052311-x86-64-squashfs-combined.img E:\openwrt\istoreos-24.10.1.vdi

Converting from raw image file="E:\刷openwrt软路由\openwrt-24.10.1-x86-generic-generic-ext4-combined.img" to file="E:\刷openwrt软路由\openwrt-24.10.1-x86.vdi"...
Creating dynamic image with size 126353408 bytes (121MB)...
---------------------------------------------------------------------------------------------------------------
配置虚拟机
名称：openwrt
文件夹：D:\VirtualBox
类型：Linux
版本：other Linux （64-bit）
内存：1024或更高
处理器：1CPU
使用已有的虚拟硬盘文件--注册--选择已经转换的文件“openwrt-24.10.1-x86.vdi”
网络选择桥接
---------------------------------------------------------------------------------------------------------------
网络配置
用户名：root
初始密码：password

网络--接口--修改
DHCP服务器
1、基本设置
勾选”忽略此接口“
2、IPV6设置
路由通告服务--停用
DHCPV6服务--停用
保存
---------------------------------------------------------------------------------------------------------------
基本设置
IPV4地址：192.168.50.200
IPV4网关：192.168.50.1
高级设置
使用自定义的DNS服务器：192.168.50.1

网卡的IPV4地址修改
IP地址：192.168.50.123
子网掩码：255.255.255.0
默认网关：192.168.50.200
首先DNS服务器：192.168.50.200
---------------------------------------------------------------------------------------------------------------
OpenWrt安装OpenClash（三种方式）

先安装所需其它插件
opkg update

opkg install coreutils-nohup bash iptables dnsmasq-full curl ca-certificates ipset ip-full iptables-mod-tproxy iptables-mod-extra libcap libcap-bin ruby ruby-yaml kmod-tun kmod-inet-diag unzip luci-compat luci luci-base

opkg install coreutils-nohup bash curl ca-certificates ipset ip-full libcap libcap-bin ruby ruby-yaml kmod-tun kmod-inet-diag unzip kmod-nft-tproxy luci-compat luci luci-base

1、下载OpenClash
OpenWrt固件插件（网站）: /packages/
https://op.dllkids.xyz/packages/
搜索下面插件，下载安装
luci-app-openclash_0.46.079-r109_all.ipk

2、下载OpenClash
#iptables
opkg update
opkg install bash iptables dnsmasq-full curl ca-bundle ipset ip-full iptables-mod-tproxy iptables-mod-extra ruby ruby-yaml kmod-tun kmod-inet-diag unzip luci-compat luci luci-base
opkg install /tmp/openclash.ipk

apk add bash iptables dnsmasq-full curl ca-bundle ipset ip-full iptables-mod-tproxy iptables-mod-extra ruby ruby-yaml kmod-tun kmod-inet-diag unzip luci-compat luci luci-base
apk add -q --force-overwrite --clean-protected --allow-untrusted /tmp/openclash.apk

#nftables
opkg update
opkg install bash dnsmasq-full curl ca-bundle ip-full ruby ruby-yaml kmod-tun kmod-inet-diag unzip kmod-nft-tproxy luci-compat luci luci-base
opkg install /tmp/openclash.ipk

apk update
apk add bash dnsmasq-full curl ca-bundle ip-full ruby ruby-yaml kmod-tun kmod-inet-diag unzip kmod-nft-tproxy luci-compat luci luci-base
apk add -q --force-overwrite --clean-protected --allow-untrusted /tmp/openclash.apk

3、下载OpenClash
wget https://github.com/vernesong/OpenClash/releases/download/v0.45.35-beta/luci-app-openclash_0.45.35-beta_all.ipk -O openclash.ipk

opkg update

opkg install openclash.ipk

opkg install luci luci-base luci-compat

reboot
---------------------------------------------------------------------------------------------------------------
命名	                                            说明
opkg update	                                  从 OpenWrt 软件包存储库获取可用软件包列表。
opkg list	                                       显示可用软件包及其描述信息。
opkg list | grep -e <search>	       显示以<search>为关键字在软件包名称或描述信息中过滤的结果
opkg install <packages>	               安装一个软件包
opkg remove <packages>	           卸载一个已安装的软件包
---------------------------------------------------------------------------------------------------------------
硬件相关
cat /proc/cpuinfo
# 查看CPU信息

uname -m
# 查看CPU架构

cat /proc/meminfo
# 查看内存使用情况

df -h
# 查看磁盘的使用率
---------------------------------------------------------------------------------------------------------------
系统相关
uname -a
# 查看内核信息

opkg print-architecture
# 可接受的架构

dmesg
# 读取内核的日志

logread
# 读取系统日志

ps -w
# 列出进程

uptime
# 显示运行时间、CPU负载

vi etc/config/network
# 修改后台地址或绑定网口

/etc/init.d/uhttpd restart 
# Luci 重启命令

/etc/init.d/uhttpd enable 
# 开机自启动  

/etc/init.d/uhttpd start 
# 启动uhttpd  

/etc/init.d/firewall restart
# 重启防火墙

/etc/init.d/network restart
# 重启网络服务  

reboot
# 重启设备
---------------------------------------------------------------------------------------------------------------
安装管理
opkg update
# 更新软件包列表

opkg install ***
# 安装软件包

opkg remove ***
# 卸载软件包

opkg install *.ipk
# 批量安装软件包

opkg [install/remove] [包名] --force-depends
# 强制安装和卸载

opkg list |grep ***
# 查找软件包

opkg list-installed
# 列出所有安装的包

opkg info ***
# 查看包的信息

opkg files ***
# 查看包的文件

rm -f /var/lock/opkg.lock
# 清理opkg update缓存文件
---------------------------------------------------------------------------------------------------------------
磁盘管理
fsisk -l 
# 列出素所有分区表“-u”与“ -l”搭配使用，显示分割数

fsisk -m 
# 显示菜单和帮助信息

fsisk -a 
# 活动分区标记/引导分区

fsisk -d 
# 删除分区

fsisk -l 
# 显示分区类型

fsisk -n 
# 新建分区

fsisk -p 
# 显示分区信息

fsisk -q 
# 退出不保存

fsisk -t 
# 设置分区号

fsisk -v 
# 进行分区检查

fsisk -w 
# 保存修改

fsisk -x 
# 扩展应用，高级功能

fsisk -s 
# 指定分区

fsisk -v 
# 版本信息

mount –t ntfs-3g /dev/sdb1 /mnt/usb
# 挂载ntfs硬盘

sleep 30 && mount -t ntfs-3g /dev/sdb1 /mnt/sdb1
# 挂载磁盘到文件夹
---------------------------------------------------------------------------------------------------------------
无线相关
iwinfo wlan0 info 
# 查看无线网卡的信息

wifi down 
wifi up
# 重启无线服务

iw dev wlan0 scan
# 扫描热点

iwinfo wlan0 assoclist  
iw dev wlan0 station dump
# 查看设备连接的客户端
---------------------------------------------------------------------------------------------------------------
其他命令
passwd
# 修改登录密码

firstboot
# 重置命令

chmod +x ***
# 脚本赋权

nslookup www.baidu.com 202.96.69.38
# DNS查询测试

du -s /root/* | sort -nr
# 查看文件目录大小

ifconfig eth0 down
ifconfig eth0 hw ether XX:XX:XX:XX:XX:XX //更改的MAC地址
ifconfig eth0 up
# 更改MAC地址
ifconfig br-lan
uci show network
uci show network.lan
cat /etc/config/network
ubus call network.interface.lan status

opkg install vsftpd openssh-sftp-server
/etc/init.d/vsftpd enable
/etc/init.d/vsftpd start
# 安装 SFTP 文件传输
---------------------------------------------------------------------------------------------------------------
原本的源：/etc/opkg/distfeeds.conf
src/gz openwrt_core https://downloads.openwrt.org/releases/24.10.1/targets/x86/generic/packages
src/gz openwrt_base https://downloads.openwrt.org/releases/24.10.1/packages/i386_pentium4/base
src/gz openwrt_kmods https://downloads.openwrt.org/releases/24.10.1/targets/x86/generic/kmods/6.6.86-1-1574ac9bc0ae1e7c064ca9bfff396adb
src/gz openwrt_luci https://downloads.openwrt.org/releases/24.10.1/packages/i386_pentium4/luci
src/gz openwrt_packages https://downloads.openwrt.org/releases/24.10.1/packages/i386_pentium4/packages
src/gz openwrt_routing https://downloads.openwrt.org/releases/24.10.1/packages/i386_pentium4/routing
src/gz openwrt_telephony https://downloads.openwrt.org/releases/24.10.1/packages/i386_pentium4/telephony
---------------------------------------------------------------------------------------------------------------
