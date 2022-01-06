# Windows 下安装 oracle
## 1. 下载 oracle 安装包
从 [oracle_home](https://www.oracle.com/cn/index.html "oracle 官网") 上下载 oracle 安装包  
![image](resources/imgs/1-1.png "oracle数据库产品")  
## 2. 安装
### 2.1 解压
解压下载下来的安装包，最好自己创建一个目录，解压到指定目录下，我这里以版本号创建了一个目录  
**注意：存放安装文件的目录最好不要包含中文和空格，否则后面安装的时候可能会出问题。**  
![image](resources/imgs/2-1.png "oracle安装包解压")  
双击`setup.exe`开始安装  
### 2.2 图示安装步骤
![image](resources/imgs/2-2.png "step 1")  
安装类型选择：  
![image](resources/imgs/2-3.png "step 2: 类型选择")  
安装方式选择：  
![image](resources/imgs/2-4.png "step 3: 安装方式")  
安装版本选择：  
![image](resources/imgs/2-5.png "step 4: 安装版本")  
运行oracle服务的用户，在本地创建一个oracle专用用户或者使用虚拟账户：    
![image](resources/imgs/2-6.png "step 5: 主目录用户")  
指定oracle基目录，注意最好使用**不含中文和空格**的目录路径：  
![image](resources/imgs/2-7.png "step 6: 指定基目录")  
选择要创建的数据库类型：  
![image](resources/imgs/2-8.png "step 7: 数据库类型")  
数据库标识符设置，若不想要容器数据库可将“创建为容器数据库”选项的`√`取消掉  
![image](resources/imgs/2-9.png "step 8: SID")  
内存分配，使用默认值即可，若机器内存较小，可手动配置一个合理的值  
![image](resources/imgs/2-10.png "step 9: 内存分配")  
字符集选择，在 Windows 下最好选择 ZHS16GBK 字符集  
![image](resources/imgs/2-11.png "step 10: 字符集选择")  
指定oracle数据文件存放目录，即DBF文件存放路径  
![image](resources/imgs/2-12.png "step 11: oracle数据文件路径")  
管理选项，直接下一步  
![image](resources/imgs/2-13.png "step 12")  
是否启用数据库恢复功能，若启用，需指定恢复存储区  
![image](resources/imgs/2-14.png "step 13: 是否开启数据恢复")  
数据库账户口令设置，理论上各账户口令应分别设置，由于是个人使用，懒得记忆口令，这里就直接将所有口令设置为相同了  
![image](resources/imgs/2-15.png "step 14: 设置账号口令")  
安装概要文件，没有问题直接点击"安装"即可  
![image](resources/imgs/2-16.png "step 15: 安装概要")  




