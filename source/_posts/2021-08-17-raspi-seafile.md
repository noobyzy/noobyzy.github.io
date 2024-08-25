---
title: Raspberry Pi 4B + Seafile 搭建私有云
date: 2021-08-17 11:22:41
tags: 
- RaspberryPi
- Seafile
- 私有云
categories: 
- [技术]
- [记录]
---
# 写在前面

这应该算是我第一篇正式blog，心情有点小激动。

确定了研究生去向以后心情比较轻松，最近实验室压力也不大（我比较摸鱼），所以想捣鼓这些小东西。
之前花了几天时间建站，过两天应该会再更一个blog复盘建站的过程。

这篇blog是随搭随更新的。网上的资料并不完全一致。至于能不能搭成功，我自己心里也没底。

---
---

# 搭建经过

## 硬件准备

- `Raspberry Pi 4B`, 已安装 `Raspbian GNU/Linux 10 (buster) armv7l` 系统（及其电源线）
- `TP-Link AC1200` 路由器，型号 `TL-WDR5610`（及其电源线）
- `Seagate SRD0NF1 1T` 移动硬盘 （及其数据线）
- 两根网线
- 一台电脑（Win10）
---

## 路由器安装

由于我自购了一个路由器（人傻钱多），第一步当然是安装路由器了。接下来步骤和说明书上一致，不想看可以跳过。

### 连接路由器

- 一根网线连接`WAN口`和`网线接口`
- 连接路由器电源
- 电脑通过无线连接来连接路由器（默认名称一般在路由器底部或背后）
- 检查指示灯

### 设置路由器

注意要先关闭代理
- 打开设置网址，TP-Link是[tplogin.cn](tplogin.cn)
- 创建管理员密码
- 上网设置，给我检测的是`自动获得IP地址`，注意是WAN口IP地址
- 无线设置，设置2.4/5G频段，设置WIFI名称和密码
  
以后管理连接该路由器的设备就可以在设置网址中进行了。

---

## 树莓派远程连接

我们需要在电脑上远程连接树莓派，这里我选择的是`MobaXterm`。[官网链接](https://mobaxterm.mobatek.net/)

### 树莓派连接路由器

- 树莓派连接电源线
- 树莓派连接网线，网线另一端连接路由器的一个LAN口上

### 电脑SSH连接树莓派

- 在`MobaXterm`中通过SSH连接树莓派，树莓派IP地址在 [tplogin.cn](tplogin.cn) 可以看到
- 当然我这边采用了RDP协议，因为我本身在树莓派中安装的系统是带图形界面的

### 树莓派设置静态IP

这一部分是抄 [https://zhuanlan.zhihu.com/p/57202972](https://zhuanlan.zhihu.com/p/57202972) 的。我之前也看到过其他设置静态IP的方法，暂不清楚这个方法行不行得通。

- 打开DHCP配置文件
```bash
$ sudo nano /etc/dhcpcd.conf
```

- 在该文件中添加/修改以下几项
```bash
#指定接口eth0
interface eth0   

# 指定静态IP， /24表示子网掩码为 255.255.255.0
static ip_address=192.168.1.101/24

# 路由器/网关IP地址
static routers=192.168.1.1
```

- 保存并关闭该文件，随后重启树莓派
```bash
$ sudo reboot
```
---

## 挂载移动硬盘

- 连接移动硬盘至树莓派
- 查看是否连接成功
```bash
$ df -hT
```

可以看到列表中出现了一行
```bash
Filesystem     Type      Size  Used Avail Use% Mounted on
/dev/sda1      fuseblk   932G  148M  932G   1% /media/root/Seagate Portable Drive
```
我们看到这边硬盘格式是`fuseblk`，据[某论坛](https://ubuntuforums.org/showthread.php?t=1096841), 我们暂且认为
这边`fuseblk == NTFS`。

### 挂载硬盘
- 挂载硬盘
```bash
$ sudo mkdir /home/pi/seagate
$ sudo mount /dev/sda1 /home/pi/seagate
```
如果在挂载时出现如下错误
```bash
Mount is denied because the NTFS volume is already exclusively opened.
The volume may be already mounted, or another software may use it which
could be identified for example by the help of the 'fuser' command.
```
```bash
$ fuser -m /dev/sda1
```
发现
```bash
/dev/sda1:            1425
```
那就杀死TA！
```bash
$ kill 1425
```
再次进行`mount`操作

### 开机自动挂载
大佬说 NTFS 格式 Linux 不能直接读取（没有求证），下面解决这一问题。[参考网址](https://zhuanlan.zhihu.com/p/57202972)
- 安装NTFS格式可读写软件
```bash
$ sudo apt-get install ntfs-3g
$ modprobe fuse
```
- 硬盘开机自动挂载
```bash
$ sudo nano /etc/fstab
```
并在最后一行添加
```bash
/dev/sda1 /home/pi/seagate fuseblk defaults,noexec,umask=0000 0 0
```

**Update!!**: 昨晚临走的时候直接拔了树莓派电源。今天重新打开的时候，发现它进入了`emergency mode`。经过了一波搜索，我发现了[链接](https://unix.stackexchange.com/questions/457477/systemd-booting-into-emergency-mode-with-error-dependency-failed-for-home)问题比较相似。于是就把昨天添加的一行修改为
```bash
/dev/sda1 /home/pi/seagate ntfs defaults,noexec,_netdev,umask=0000 0 0
```
`reboot`后开机成功

---
## 部署Seafile

### 安装seafile
- 更新源、安装python环境
```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install python2.7 libpython2.7 python-setuptools python-pil python-ldap python-urllib3 sqlite3 python-requests
```
- 下载安装包，可以在树莓派通过`wget`下载，也可以电脑下载后传到树莓派里。这边我选择后者

- 在github中下载了`seafile-server_7.0.5_stable_pi.tar.gz`。[链接](https://github.com/haiwen/seafile-rpi/releases/tag/v7.0.5)
- 树莓派中在 `/home/pi/seafile`目录下解压
```bash
$ tar -xzf seafile-server_7.0.5_stable_pi.tar.gz
```
- 进行安装
```bash
$ bash setup-seafile.sh
```
- 设置server信息

```sql
# 服务器名称
[server name]: noobBran
# 局域网内树莓派IP
[This server's ip or domain]: 192.168.1.101
# 指定seafile数据存放位置，我们将其改到硬盘目录下
[default: /home/pi/seafile/seafile-data ] /home/pi/seagate/seafile-data
# seafile服务器端口号，使用默认8082
[default: 8082 ] 
```
**注意!!**: 如果出现如下报错
```bash
Failed to sync seafile database
```
在`setup-seafile.sh`中进行修改
```bash
- seafile_db=${TOPDIR}/seafile-data/seafile.db #删除这一行
+ seafile_db=${seafile_data_dir}/seafile.db #添加这一行（没有前面 + 号）
```
随后重新安装即可。出现以下字样即安装成功
```bash
-----------------------------------------------------------------
Your seafile server configuration has been completed successfully.
-----------------------------------------------------------------

run seafile server:     ./seafile.sh { start | stop | restart }
run seahub  server:     ./seahub.sh  { start <port> | stop | restart <port> }

-----------------------------------------------------------------
If the server is behind a firewall, remember to open these tcp ports:
-----------------------------------------------------------------

port of seafile fileserver:   8082
port of seahub:               8000
```

### 开启seafile
- 开启服务
```bash
bash seafile.sh start && bash seahub.sh start
```
并配置管理员邮箱和密码

打开浏览器并访问`http://localhost:8000`


- 设置开机自启动。在`seafile-server_7.0.5`目录下建一个`seafile_start.sh`。并填写
```bash
sudo bash seafile.sh start
sudo bash seahub.sh start
```
- 打开`/etc/rc.local`，在`exit 0`前添加
```bash
sudo bash /home/pi/seafile/seafile-server_7.0.5/seafile_start.sh
```

### seafile局域网访问失败
- 目前其他机器通过局域网访问失败。下面解决这一问题
- 打开`seafile/conf/gunicorn.conf`
```bash
- bind = "127.0.0.1:8000"  # 删除这一行
+ bind = "192.168.1.101:8000" # 添加这一行
```
- 重启seafile
```bash
sudo bash seafile.sh restart
sudo bash seahub.sh restart
```

---
## 其他设备连接私有云
前提条件：同一局域网下

1. 浏览器输入`http://192.168.1.101:8000`
2. 下载客户端。下载链接: [https://www.seafile.com/download/](https://www.seafile.com/download/)

---
---

# 成果展示
![Win10挂载盘-1-外观](seadrive1.png)

![Win10挂载盘-2-内饰](seadrive2.png)

![Win10浏览器访问](web_seadrive.png)

![Ipad客户端访问](ipad.PNG)

![小米手机访问](xiaomi.jpg)

---
---

# Reference
[https://blog.csdn.net/windsnow1/article/details/96281071/](https://blog.csdn.net/windsnow1/article/details/96281071/)

[https://cloud.tencent.com/developer/article/1681154](https://cloud.tencent.com/developer/article/1681154)

[https://zhuanlan.zhihu.com/p/57202972](https://zhuanlan.zhihu.com/p/57202972)

[https://blog.csdn.net/qq_34178764/article/details/104404738?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.control&spm=1001.2101.3001.4242](https://blog.csdn.net/qq_34178764/article/details/104404738?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.control&spm=1001.2101.3001.4242)

[https://blog.csdn.net/windsnow1/article/details/96281071/](https://blog.csdn.net/windsnow1/article/details/96281071/)

[https://www.kancloud.cn/kancloud/seafile-manual/51506](https://www.kancloud.cn/kancloud/seafile-manual/51506)
