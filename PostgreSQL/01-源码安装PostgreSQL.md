# 下载源码安装包
进入[PostgreSQL官网](https://www.postgresql.org/), 选择`download`  
![image](resources/imgs/pg-01.png)
![image](resources/imgs/pg-02.png)

# 环境准备
## C编译器
PostgreSql是用C语言写的，编译源码需要服务器具有C编译器，详细信息可以查看[官方手册](https://www.postgresql.org/docs/)  
安装GCC  
`sudo apt-get install gcc`  
若想安装指定版本，需要查询软件,可以使用如下命令  
`sudo apt-cache search <pkgName>`  
安装指定版本    
`sudo apt-get install <pkgName>=<version>`  

## readline 库
执行 `configure` 需要 readline 库  
readline 使 pg 数据库能记住用户的命令历史  
```shell
sudo apt-get install lib64readline-dev
sudo apt install libreadline-dev
```

## zlib
若不适用zlib进行编译， 则 pg_dump 和 pg_restore 的文档压缩功能将失效  
`apt-get install zlib1g zlib1g-dev`

## make 工具
编译工作使用的 Makefile , 因此需要 **make** 工具  
官方手册要求 make版本 &gt; 3.8.1  
`sudo apt-get install make`

# 安装
将源码包上传至服务器并解压   

## configure 
如果出现 `creating makefile` 字样，则说明 configure 成功啦  
![image](resources/imgs/pg-03.png "./configure")    

## make & make install 
make install 会在 `/usr/local` 下面创建 `pgsql` 文件夹, 因此需要 root 权限  
`sudo make & make install`


# 初始化、启动数据库
## 创建用户 postgres 
`sudo adduser postgres`
![image](resources/imgs/pg-04.png "创建用户 postgres")  

## 创建 data 目录, 并更改目录归属
`sudo mkdir /usr/local/pgsql/data`
`sudo chown -R postgres. /usr/local/pgsql/data`  

## 切换至 postgres 用户
`su - postgres`

## 初始化数据库
`/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data/`  
![image](resources/imgs/pg-05.png "初始化数据库")

## 启动数据库
`/usr/local/pgsql/bin/pg_ctl -D /usr/local/pgsql/data/ -l logfile start`  
![image](resources/imgs/pg-06.png "启动数据库")

## 创建数据库 test
`/usr/local/pgsql/bin/createdb test`  

## 修改默认用户 postgres 的密码
连接数据库 test  
`/usr/local/pgsql/bin/psql test`  
修改密码  
![image](resources/imgs/pg-07.png "修改postgres的密码")


## 客户端连接数据库
开启防火墙端口号  
```
sudo ufw allow 5432
```

数据库默认配置是只能服务器本机能进行连接，可以通过配置文件 `data` 目录下的 `pg_hba.conf` 和 `postgresql.conf` 进行配置  

修改 `pg_hba.conf` 配置  
```
# 添加 host 配置行， 下面这行表示 允许 10.0.110.1 的ip地址可以访问 PostgreSql 的所有数据库
host    all             all             10.0.110.1/32            md5
```
修改 `postgresql.conf` 配置  
```
# 配置监听所有ip地址
listen_addresses = '*'
```

修改配置文件后，重启数据库服务  
` /usr/local/pgsql/bin/pg_ctl -D /usr/local/pgsql/data/ -l logfile restart`


若是只修改了 `postgresql.conf` 文件中的某些项，可以通过如下指令让配置生效  
`./pg_ctl reload -D /usr/local/pgsql/data/`


使用 dbeaver 进行远程连接  
![image](resources/imgs/pg-08.png "远程连接")




