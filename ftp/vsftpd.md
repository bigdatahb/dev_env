# 1. 安装vsftpd服务
```shell
# 查找安装包
yum search vsftpd
# 安装
yum install vsftpd.x86_64
```
![image](https://user-images.githubusercontent.com/51871609/147453281-97420881-f492-412d-a414-d3a5d80ba27c.png)

# 2. 配置vsftpd.conf
```properties
# 开启 standalone 运行模式
listen=YES
# 关闭IPV6监听
listen_ipv6=NO
# 修改ftp连接端口, 默认是21
listen_port=6666
chroot_local_user=YES
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chroot_list
# 指定pam服务名,其对应的文件为 /etc/pam.d/vsftpd
pam_service_name=vsftpd
# 虚拟用户使用本地用户的权限
virtual_use_local_privs=YES
# 开启虚拟用户
guest_enable=YES
# 指定虚拟用户的宿主用户
guest_username=vsftpd
# 指定虚拟用户配置文件目录
user_config_dir=/etc/vsftpd/vuser_conf
# 开启被动模式
pasv_enable=YES
# 被动模式最小端口，本示例设置为30100
pasv_min_port=30100
# 被动模式最大端口，本示例设置为30200
pasv_max_port=30200
# 被动模式的IP地址，VPC环境下需要设置为服务器公网地址
pasv_address=10.0.110.22
```
# 3. 创建虚拟用户
```shell
useradd vsftpd -d /data/vsftpd -s /sbin/nologin
```
# 4. 配置虚拟用户
## 4.1 创建虚拟用户数据库
### 4.1.1 创建文件 /etc/vsftpd/vuser.txt
```shell
vi /etc/vsftpd/vuser.txt
# 往文件 /etc/vsftpd/vuser.txt 中写入虚拟用户账号信息，内容可以如下：
up
up123
down
down123
admin
admin123
```
### 4.1.2 利用文件 /etc/vsftpd/vuser.txt 创建虚拟数据库
```shell
# 创建虚拟数据库 vuser.db
db_load -T -t hash -f /etc/vsftpd/vuser.txt /etc/vsftpd/vuser.db
# 修改权限
chmod 600 /etc/vsftpd/vuser.db
```
## 4.2 修改PAM认证
```shell
# 修改文件 /etc/pam.d/vsftpd ， 注意这个文件名与 Vsftpd.conf 文件中的配置项 pam_service_name 指定的名字一样, 注释掉其他参数，加入如下两行内容:
auth    required        /lib64/security/pam_userdb.so db=/etc/vsftpd/vuser
account required        /lib64/security/pam_userdb.so db=/etc/vsftpd/vuser
```
## 4.3 创建单个虚拟用户配置
```shell
# 创建虚拟用户配置目录
mkdir /etc/vsftpd/vuser_conf
# 如果 chroot_list 文件不存在，创建该文件
touch /etc/vsftpd/chroot_list
# 创建虚拟用户对应的配置文件, 文件名要与虚拟用户名一致
vi /etc/vsftpd/vuser_conf/up
# 在up文件中加入如下内容：
local_root=/data/vsftpd/upload
write_enable=YES

download_enable=NO
# 创建目录 /data/vsftpd/upload
mkdir /data/vsftpd/upload
# 修改目录归属
chown -R vsftpd. /data/vsftpd/upload
```
# 5 防火墙配置
添加端口
```shell
firewall-cmd --zone=public --add-port=6666/tcp --permanent
firewall-cmd --zone=public --add-port=30100-30200/tcp --permanent
firewall-cmd --reload
```
查看端口
```shell
firewall-cmd --list-ports
```
# 6. SELINUX配置
## 6.1 临时关闭SELINUX
```shell
setenforce 0
```
## 6.2 永久关闭SELINIX
修改配置文件 `/etc/sysconfig/selinux`，将`SELINUX`配置项修改为 `SELINUX=disabled`， 重启操作系统

# 7. 启动服务
```shell
systemctl start vsftpd.service
```
# 8. 测试账号
## 8.1 测试只能上传不能下载的账号
![image](https://user-images.githubusercontent.com/51871609/147489219-7fe78def-66a2-4f86-b3e1-5a4201db4400.png)
## 8.2 测试只能下载不能上传的账号
```shell
# 配置账号 down, 创建文件 /etc/vsftpd/vuser_conf/down , 并写入如下内容
local_root=/data/vsftpd/download
write_enable=NO
download_enable=YES
```
![image](https://user-images.githubusercontent.com/51871609/147489624-2bfcb907-be84-4376-ba77-1589f8cabc6b.png)
