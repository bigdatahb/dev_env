# 安装 vsftpd 服务
```bash
# 使用如下命令查看是否安装了 vsftpd 服务
systemctl status vsftpd.service
```
![image](resources/imgs/1.png)  
```bash
# 切换至root用户，安装 vsftpd 服务
yum install vsftpd
```
![install vsftpd](resources/imgs/2.png "yum install vsftpd")

# 配置 vsftpd 服务
## 1. 简易版
### 1.1 简单配置 vsftpd.conf
安装`vsftpd`服务后, 在 `/etc/vsftpd/` 目录下有相关的服务配置文件  
我们若想简单使用 `ftp` 服务，那么稍微修改下 `vsftpd.conf` 文件就可以了
直接打开 `vsftpd.conf` 文件，添加如下配置项：  
```properties
pasv_min_port=6000
pasv_max_port=6100
```
### 1.2 修改防火墙规则
因为 `ftp` 服务在进行`TCP`连接时，需要用到端口，因此要想正常连接和传输数据，需要将对应的端口打开
**在防火墙里添加服务规则有2种方式: a) 直接将服务添加进防火墙规则分区 b)将服务的指定端口协议加入防火墙**  
**任选一种配置都可以是ftp能够正常连接** 
#### 1.2.1 防火墙命令简介
`RHEL7` 系统开始使用命令 `firewall-cmd`对防火墙进行配置，若是`RHEL6`或更早期版本，需要查阅 `iptables`命令
`firewall-cmd`常用命令：
```shell
# 查看 firewalld 服务状态
firewall-cmd --state
# 查看帮助
firewall-cmd -h
# 查看所有分区
firewall-cmd --get-zones
# 查看当前激活的分区
firewall-cmd --get-active-zone
# 查看 public 分区添加的服务
firewall-cmd --list-services --zone=public
# 查看 public 分区添加的端口
firewall-cmd --list-ports --zone=public
# 向分区 public 添加服务, 永久生效(--permanent)
firewall-cmd --zone=public --add-service=<service_name> --permanent
# 移除服务
firewall-cmd --zone=public --remove-service=<service_name> --permanent
# 向分区 public 添加tcp协议端口, 永久生效(--permanent)
firewall-cmd --zone=public --add-port=21/tcp --permanent
firewall-cmd --zone=public --add-port=8000-9000/tcp --permanent
# 移除端口
firewall-cmd --zone=public --remove-port=21/tcp --permanent
# 刷新防火墙规则， 所有配置修改后需要刷新才能生效
firewall-cmd --reload
```
#### 1.2.2 直接将 ftp 服务加入防火墙
```shell
# 永久添加 ftp 服务
firewall-cmd --zone=public --add-service=ftp --permanent
# 刷新防火墙规则
firewall-cmd --reload
# 查看分区中的已添加的服务
firewall-cmd --zone=public --list-services
```
#### 1.2.3 将指定端口加入防火墙规则
```shell
# 永久添加端口, 这里需要添加2种端口，一种是进行ftp连接的21端口, 一种是用于数据传输的高端口(如果是被动模式的话)
firewall-cmd --zone=public --add-port=21/tcp --permanent
# 假设 pasv_min_port 和 pasv_max_port 设置的端口范围为 5000-5100
firewall-cmd --zone=public --add-port=5000-5100/tcp --permanent
# 刷新规则
firewall-cmd --reload
# 查看已开放的端口
firewall-cmd --zone=public --list-ports
```
### 1.3 启动vsftpd服务，用客户端进行连接
```shell
# 启动服务
systemctl start vsftpd.service
# 查看服务状态
systemctl status vsftpd.service
# 重启服务
systemctl restart vsftpd.service
```
当服务正常启动后就可以通过客户端进行连接了：  
**Windows 进行ftp连接:**
![windows](resources/imgs/3.png "windows 连接ftp服务")
**其他Linux机器连接ftp服务：**  
![Linux](resources/imgs/4.png "linux 连接ftp服务")

<font color=red>注意: </font>  

## 2. 精修版
### 2.1 修改配置文件 vsftpd.conf
具体配置项信息可以参考当前目录下的 vsftpd.conf.properties 文件或者 在linux服务器上通过 man vsftpd.conf 查看 manual
```properties
# 禁止匿名用户登录
anonymous_enable=NO
# 本地用户可以登录
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
# 开启登录、上传、下载的日志记录文件
xferlog_enable=YES
# 日志格式
xferlog_std_format=YES
# chroot_list 文件中的登录用户可以切换至其他目录
chroot_local_user=YES
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chroot_list
# 以 standalone 模式运行 vsftpd 服务
listen=YES
# 修改ftp连接端口为 1369
listen_port=1369
# 关闭主动模式
port_enable=NO
# 开启被动模式
pasv_enable=YES
# 被动模式下数据传输端口的最小值
pasv_min_port=50000
# 被动模式下，数据传输端口的最大值
pasv_max_port=51000
# 若是对外开放的ftp服务，这里要配置成公网IP, 内部使用配置成当前机器的内部地址即可
pasv_address=10.0.110.21
# 禁止 user_list 文件中的用户登录
userlist_enable=YES
tcp_wrappers=YES
guest_enable=YES
pam_service_name=/etc/pam.d/vsftpd
guest_username=vsftpd
virtual_use_local_privs=YES
user_config_dir=/etc/vsftpd/ftplogin
allow_writeable_chroot=YES
```

![image](https://img9.doubanio.com/view/photo/l/public/p2554525534.webp "海蒂与爷爷")  
