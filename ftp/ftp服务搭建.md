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
## 1. 修改配置文件 vsftpd.conf
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

![image](https://img9.doubanio.com/view/photo/l/public/p2554525534.webp)  
