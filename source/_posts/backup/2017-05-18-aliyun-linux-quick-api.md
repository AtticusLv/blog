---
title: 阿里云快速搭建公网ip访问api
date: 2017-05-18 11:38:04
tags:
- 阿里云
category:
- 业余学习
---
最近抢到了阿里云的6个月试用，于是想自己搭个服务器和数据库，弄个小网站出来，自己也是各种坑踩过来，公网私网映射，配置云安全组规则，也是花了小半天才弄明白这些过程，记录下：
# 搭建公网ip访问api
## 准备阶段
### 要运行的jar包服务
首先要先准备好一个可执行的jar包，我准备好了自己一个 **jwt token** 相关的服务，跑在4006端口上，具体请求的方法：
```
URL: 公网ip:4006/decodetoken
Method: POST
Header: Content-Type:application/json
Body: {"token":"testing"}
```
### EC服务器
我用的是阿里云免费试用的实例规格`ecs.xn4.small`，这里将隐私部分拿`xx`和`yy`代替了
```
配置
CPU： 1核
内存： 1GB
操作系统： Ubuntu 14.04 64位
公网ip： xx.xx.xxx.xxx
私有ip： yy.yy.yyy.yyy
带宽： 1Mbps
```
### 安装java
具体可以参考[Ubuntu安装Oracle Java8以及环境变量的正确设置方法](https://www.linuxdashen.com/ubuntu%E5%AE%89%E8%A3%85oracle-java8%E4%BB%A5%E5%8F%8A%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F%E7%9A%84%E6%AD%A3%E7%A1%AE%E8%AE%BE%E7%BD%AE%E6%96%B9%E6%B3%95)
```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```
安装过程中会弹出一个Oracle的使用条款，选择yes就可以，安装完后可以运行`java -version`来检查版本
## 运行jar服务
可以使用`nohup`+`&`在后台运行服务，同时会生成一个名字为`nohup.out`的文件会记录下服务运行日志，具体命令为
```
nohup java -jar jwt.jar &
```
我们可以用`jobs`命令来查看有哪些进程在运行，这个时候我们可以通过`curl`命令来测试api是否工作正常
```
curl yy.yy.yyy.yyy:4006/decodetoken -X POST -i -H "Content-Type:application/json" -d '{"token":"testing"}'
```
## 公网ip和内网ip映射
这一步我研究了一个晚上才明白，主要使用命令`iptables`参考教程[linux外网服务器跳转内网服务器实现内网访问（iptables）](http://blog.csdn.net/gyy823/article/details/45153711)
### 配置filter选项，使得ip和port都可以通过防火墙
```
iptables -t filter -A INPUT -p tcp --dport 4006 -j ACCEPT
iptables -t filter -A OUTPUT -p tcp --sport 4006 -j ACCEPT
```
### 配置nat转发规则选项
```
iptables -t nat -A PREROUTING -d 公网ip -p tcp --dport 4006 -j DNAT --to-destination 私网ip:4006
```
### 保存iptables配置
```
iptables-save
```
### 保存和调用iptables配置
iptables在系统退出的时候，设置都将不存在，需要手动去保存一下
```
iptables-save >/etc/iptables.rules
```
调用iptables设置
```
iptables-restore >/etc/iptables.rules
```
我们可以把它设置成关机开机自动配置
```
vi /etc/network/interfaces
```
在最后添加
```
pre-up iptables-restore >/etc/iptables.rules //开机时自动调用已经存在的iptables设置
post-down iptables-save >/etc/iptables.rules //关机时自动保存当前的iptables设置
```
另外一种保存方法是利用 **iptables-persistent**
**Ubuntu 14.04**
```
sudo invoke-rc.d iptables-persistent save
sudo invoke-rc.d iptables-persistent reload
```
**Ubuntu 16.04**
```
sudo netfilter-persistent save
sudo netfilter-persistent reload
```
## 阿里云安全组规则
这步当时弄了一上午才明白咋回事，在配置上述映射后，关键一步要在阿里云安全组规则里把相应的端口加上
{% asset_img anquan.png ruleOfSecurity %}
## 测试
这时我们就可以在公网测试api是否能通啦，这里我用的postman，也有很多其他的在线测试工具
{% asset_img token.png test %}
# Linux那些命令行
## 重启
```
reboot
```
## 查看系统资源占用
```
top
free
```
## 查看内网ip
```
ifconfig
```
## 查看进程
```
ps
```
## 查看磁盘使用
```
# 以Mb为单位
df -m
```
## 创建目录
```
mkdir 目录名
```
## 安装软件
目前我只了解apt方式，欢迎各路豪杰来补充
```
# 只要可以上网，可以用apt-cache search来查找
apt-cache search software-name
# apt-get
## 安装一个新的软件包
apt-get install packagename
## 卸载一个已安装的软件包(保留配置文件)
apt-get remove packagename
## 卸载一个已安装的软件包(删除配置文件)
apt-get --purge remove packagename
## 删除已经删掉的软件
apt-get autoremove——因为apt会把已装或已卸的软件都备份在硬盘上，所以如果需要空间的话，可以让这个命令来删除你已经删掉的软件。
## 清楚已卸载软件包的deb文件
apt-get autoclean——定期运行这个命令来清除那些已经卸载的软件包的.deb文件。通过这种方式，可以释放大量的磁盘空间。如果需求十分迫切，可以使用apt-get clean以释放更多空间。这个命令会将已安装软件包裹的.deb文件一并删除。
## 清楚安装软件备份
apt-get clean——这个命令会把安装的软件的备份也删除，不过这样不会影响软件的使用的。
## 更新所有已安装的软件包
apt-get upgrade
## 将系统升级到新版本
apt-get dist-upgrade
## 在软件包列表中搜索字符串
apt-cache search string
## 显示软件包信息
apt-cache showpkg pkgs
## 查看库里有多少软件
apt-cache stats
## 打印可用软件包列表
apt-cache dumpavail
## 显示软件包记录，类似于dpkg –print-avail
apt-cache show pkgs
## 打印软件包列表中所有软件包的名称
apt-cache pkgnames
```
## iptables
查看iptables配置
```
iptables -L -n
```
iptables是直接配置就会生效，重启后会丢失，保存命令为`iptables-save`，恢复默认为`iptables-restore`
```
# 清除原有规则
iptables -F
iptables -X

# 设定预设规则
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP

# 添加规则
# 留给ssh用的22端口
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT

# 设置允许ping
iptables -A INPUT -p icmp -j ACCEPT
iptables -A OUTPUT -p icmp -j ACCEPT

# 教程上说是允许loopback，不然会导致DNS无法正常关闭等问题
iptables -A INPUT -i lo -p all -j ACCEPT
iptables -A OUTPUT -o lo -p all -j ACCEPT

# 教程上说是丢弃坏的TCP包
iptables -A FORWARD -p TCP ! --syn -m state --state NEW -j DROP

# 教程上说处理IP碎片数量,防止攻击,允许每秒100个
iptables -A FORWARD -f -m limit --limit 100/s --limit-burst 100 -j ACCEPT

# 教程上说设置ICMP包过滤,允许每秒1个包,限制触发条件是10个包
iptables -A FORWARD -p icmp -m limit --limit 1/s --limit-burst 10 -j ACCEPT

# 如果访问转接慢，可以配置
iptables -t filter -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -t filter -A OUTPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -t nat -A PREROUTING -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -t nat -A POSTROUTING -m state --state ESTABLISHED,RELATED -j ACCEPT
```
# 参考
1. [Ubuntu 使用top/free查看内存占用大的原因](https://fukun.org/archives/02201800.html)
2. [吐槽-UbuntuServer用iptables麻烦+如何启动与停止](https://www.deamwork.com/archives/ubuntu-iptables-conn.orz6)
3. [通过iptables实现端口转发和内网共享上网](http://xstarcd.github.io/wiki/Linux/iptables_forward_internetshare.html)
4. [更改了iptables后，无法访问外网](http://bbs.chinaunix.net/thread-4069594-1-1.html)
5. [linux外网服务器跳转内网服务器实现内网访问（iptables）](http://blog.csdn.net/gyy823/article/details/45153711)
