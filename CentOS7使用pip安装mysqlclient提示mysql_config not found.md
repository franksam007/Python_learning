mysqlclient 是Python语言连接MariaDB / MySQL的链接库。Python程序可以使用mysqlclient连接到MariaDB / MySQL数据库，实现对MariaDB / MySQL数据库的操作。mysqlclient是MySQLdb1的一个分支，mysqlclient添加了对Python3的支持，并且修复了一些bug。

## 1、问题描述
当我们使用pip安装mysqlclient时，提示mysql_config not found错误，具体信息如下：

```
(python-django) [anxin@bogon local]# pip install mysqlclient
Collecting mysqlclient
  Downloading mysqlclient-1.3.12.tar.gz (89kB)
    100% |████████████████████████████████| 92kB 16kB/s 
    Complete output from command python setup.py egg_info:
    /bin/sh: mysql_config: 未找到命令
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/tmp/pip-build-n0gegrlb/mysqlclient/setup.py", line 17, in <module>
        metadata, options = get_config()
      File "/tmp/pip-build-n0gegrlb/mysqlclient/setup_posix.py", line 44, in get_config
        libs = mysql_config("libs_r")
      File "/tmp/pip-build-n0gegrlb/mysqlclient/setup_posix.py", line 26, in mysql_config
        raise EnvironmentError("%s not found" % (mysql_config.path,))
    OSError: mysql_config not found
    
    ----------------------------------------
```
## 2、问题原因
在系统上不存在mysql_config可执行文件，即：没有安装 mariadb 的链接库和头文件软件包。

## 3、解决方法：
mysql_config是mariadb-devel或者mysql-devel包的一部分，mariadb-devel包是mariadb的链接库和头文件，不是mariadb的开发版本。链接库和头文件的版本需要和你安装的数据库版本相符。在使用链接库的情况下，不推荐使用SCL源安装MariaDB / MySQL。

### 3.1、安装链接库和头文件
#### 1）使用CentOS基本源安装的MariaDB / MySQL，使用如下命令安装MariaDB / MySQL链接库

```
sudo yum install mariadb-devel
```
#或者 mysql-devel在CentOS7中不存在
```
sudo yum install mysql-devel
```
#### 2）使用CentOS IUS源安装的MariaDB / MySQL，安装MariaDB / MySQL对应版本的链接库包
```
sudo yum install mariadb101u-devel
```
#或者
```
sudo yum install mysql57u-devel
```
#### 3）使用CentOS SCL源安装的MariaDB / MySQL，需要安装其他源（如：Base源，EPEL源，官方源，IUS源）中相应的链接库包（mariadb-devel）。
