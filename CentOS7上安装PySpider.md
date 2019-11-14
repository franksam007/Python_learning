## 方法1
### 创建虚拟环境

```
# 全面更新系统
sudo yum update -y

# CentOS默认没有安装pip
sudo python get-pip.py

# 安装虚拟环境支持软件
pip install virtualenv

# 创建虚拟环
mkdir pyspider 
cd pyspider/
virtualenv env
```

### 安装centos的开发环境、依赖库
```
sudo yum install libcurl-devel
sudo yum install libxml2-devel libxslt-devel python-devel
```

### 为了顺利pip顺利安装，打个补丁

```
export PYCURL_SSL_LIBRARY=nss 
pip uninstall pycurl
pip install pycurl --no-cache-dir
#或者
pip install pycurl --global-option="--with-nss" --no-cache-dir
```

### 安装pyspider

```
pip install pyspider
```

### 启动pyspider
```
pyspider
```

## 方法2：利用Python3
### 1.搭建环境：
    python版本：3.6.3

    系统环境：centos7.3



#### 1.1.搭建python3环境：
下载依赖 
```
yum install -y ncurses-devel openssl openssl-devel zlib-devel gcc make glibc-devel libffi-devel glibc-static glibc-utils sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libcurl-devel
```


```下载python
wget https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tgz
```

解压
```
tar -xf Python-3.6.3.tgz
```


编译安装
```
 ./configure --prefix=/usr/local/python3.6 --enable-shared

make && make install
```


建立软链接
```
ln -s /usr/local/python3.6/bin/python3 /usr/bin/python3

echo "/usr/local/python3.6/lib" > /etc/ld.so.conf.d/python3.5.conf

ldconfig
```


验证python3
```
[root@ceph-host-01 local]# python3

Python 3.6.3 (default, Oct  9 2017, 04:01:24) 

[GCC 4.8.5 20150623 (Red Hat 4.8.5-16)] on linux

Type "help", "copyright", "credits" or "license" for more information.

>>> 
```


pip
```
/usr/local/python3.6/bin/pip3 install --upgrade pip

ln -s /usr/local/python3.6/bin/pip /usr/bin/pip
```


#### 1.2.安装pyspider
```
pip install pyspider
```


启动python中的pycurl模块出现如下问题：
```
ImportError: pycurl: libcurl link-time ssl backend (nss) is different from compile-time ssl backend (none/other)
```

解决方法：
```
pip uninstall pycurl
export PYCURL_SSL_LIBRARY=nss
pip install pycurl
```

#### 1.3.安装phantomjs
官网下载：http://phantomjs.org/download.html
```
wget https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2
```

解压：
```
yum -y install unbzip2

bzip2 -d phantomjs-2.1.1-linux-x86_64.tar.bz2 

tar -xf phantomjs-2.1.1-linux-x86_64.tar

mv phantomjs-2.1.1-linux-x86_64 phantomjs

ln -sv /usr/local/phantomjs/bin/phantomjs /usr/bin/phantomjs
```


#### 1.4.启动pyspider
由于放在公网，编辑了一个配置文件config.json ，用于登录认证

```
[root@ceph-host-01 local]# vim config.json 

{

    "webui": {

        "port": "5000",

        "username": "abc",

        "password": "123456",

        "need-auth": true

    }

}
```

开启进程
```
nohup pyspider --config config.json &
```

进入web界面


## 方法3：centos7分布式部署pyspider

### 1.搭建环境：

系统版本：Linux centos-linux.shared 3.10.0-123.el7.x86_64 #1 SMP Mon Jun 30 12:09:22 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux

python版本：Python 3.5.1

#### 1.1.搭建python3环境：
选择集成环境Anaconda

##### 1.1.1.编译
```
# 下载依赖
yum install -y ncurses-devel openssl openssl-devel zlib-devel gcc make glibc-devel libffi-devel glibc-static glibc-utils sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-deve
# 下载python版本
wget https://www.python.org/ftp/python/3.5.1/Python-3.5.1.tgz
# 或者使用国内源
wget http://mirrors.sohu.com/python/3.5.1/Python-3.5.1.tgz
mv Python-3.5.1.tgz /usr/local/src;cd /usr/local/src
# 解压
tar -zxf Python-3.5.1.tgz;cd Python-3.5.1
# 编译安装
./configure --prefix=/usr/local/python3.5 --enable-shared
make && make install
# 建立软链接
ln -s /usr/local/python3.5/bin/python3 /usr/bin/python3
echo "/usr/local/python3.5/lib" > /etc/ld.so.conf.d/python3.5.conf
ldconfig
# 验证python3
python3
# Python 3.5.1 (default, Oct  9 2016, 11:44:24)
# [GCC 4.8.5 20150623 (Red Hat 4.8.5-4)] on linux
# Type "help", "copyright", "credits" or "license" for more information.
# >>>
# pip
/usr/local/python3.5/bin/pip3 install --upgrade pip
ln -s /usr/local/python3.5/bin/pip /usr/bin/pip
# 本人在安装时出现问题 将pip重装
wget https://bootstrap.pypa.io/get-pip.py --no-check-certificate
python get-pip.py
```
##### 1.1.2.集成环境anaconda
```
# 集成环境anaconda(推荐)
wget https://repo.continuum.io/archive/Anaconda3-4.2.0-Linux-x86_64.sh
# 直接安装即可
./Anaconda3-4.2.0-Linux-x86_64.sh
# 若出错，可能是解压失败
yum install bzip2
```
#### 1.2.安装mariaDB
```
# 安装
yum -y install mariadb mariadb-server
# 启动
systemctl start mariadb
# 设置为开机启动
systemctl enable mariadb
# 配置密码 默认为空
mysql_secure_installation
# 登录
mysql -u root -p
# 创建一个用户 自己设定账户密码
CREATE USER 'user_name'@'localhost' IDENTIFIED BY 'user_pass';
GRANT ALL PRIVILEGES ON *.* TO 'user_name'@'localhost' WITH GRANT OPTION;
CREATE USER 'user_name'@'%' IDENTIFIED BY 'user_pass';
GRANT ALL PRIVILEGES ON *.* TO 'user_name'@'%' WITH GRANT OPTION;
```
#### 1.3.安装pyspider
使用Anaconda
```
# 搭建虚拟环境sbird python版本3.*
conda create -n sbird python=3*
# 进入环境
source activate sbird
# 安装pyspider
pip install pyspider
# 报错 
# it does not exist.  The exported locale is "en_US.UTF-8" but it is not supported
# 执行 可写入.bashrc
export LC_ALL=en_US.utf-8
export LANG=en_US.utf-8
#ImportError: pycurl: libcurl link-time version (7.29.0) is older than compile-time version (7.49.0)
conda install pycurl
# 退出
source deactivate sbird
# 若在虚拟机内 出现无法访问localhost:5000 可关闭防火墙
systemctl stop firewalld.service
#########直接运行源码==============
mkdir git;cd git
# 下载
git clone https://github.com/binux/pyspider.git
# 安装
/root/anaconda3/envs/sbird/bin/python  /root/git/pyspider/run.py
```

其他方法
```
# 搭建虚拟环境
pip install virtualenv
mkdir python;cd python
# 创建虚拟环境pyenv3
virtualenv -p /usr/bin/python3 pyenv3
# 进入虚拟环境 激活环境
cd pyenv3/
source ./bin/activate
pip install pyspider
# 若pycurl报错 
yum install libcurl-devel
# 继续
pip install pyspider
# 关闭
deactivate
```

推荐用anaconda方式安装

若pyspider运行过程中出现错误，参考anaconda安装部分，至此，访问localhost:5000可看到页面。

#### 1.4.安装Supervisor
```
# 安装
yum install supervisor -y
# 若无法检索 则添加阿里的epel源
vim /etc/yum.repos.d/epel.repo
# 添加以下内容
[epel]
name=Extra Packages for Enterprise Linux 7 - $basearch
baseurl=http://mirrors.aliyun.com/epel/7/$basearch
http://mirrors.aliyuncs.com/epel/7/$basearch
failovermethod=priority
enabled=1
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7

[epel-debuginfo]
name=Extra Packages for Enterprise Linux 7 - $basearch - Debug
baseurl=http://mirrors.aliyun.com/epel/7/$basearch/debug
http://mirrors.aliyuncs.com/epel/7/$basearch/debug
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=0

[epel-source]
name=Extra Packages for Enterprise Linux 7 - $basearch - Source
baseurl=http://mirrors.aliyun.com/epel/7/SRPMS
http://mirrors.aliyuncs.com/epel/7/SRPMS
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=0
# 安装
yum install supervisor -y
# 测试是否安装成功
echo_supervisord_conf
```
##### 1.4.1.Supervisor用法
```
supervisord     #supervisor的服务器端部分 启动
supervisorctl   #启动supervisor的命令行窗口
# 假设创建进程pyspider01
vim /etc/supervisord.d/pyspider01.ini
# 写入以下内容
[program:pyspider01]

command      = /root/anaconda3/envs/sbird/bin/python  /root/git/pyspider/run.py
directory    = /root/git/pyspider
user         = root
process_name = %(program_name)s
autostart    = true
autorestart  = true
startsecs    = 3

redirect_stderr         = true
stdout_logfile_maxbytes = 500MB
stdout_logfile_backups  = 10
stdout_logfile          = /pyspider/supervisor/pyspider01.log
# 重载
supervisorctl reload
# 启动
supervisorctl start pyspider01
# 也可这样启动
supervisord -c /etc/supervisord.conf
# 查看状态
supervisorctl status
# output 
pyspider01                       RUNNING   pid 4026, uptime 0:02:40
# 关闭
supervisorctl shutdown
```
#### 1.5.安装redis
```
# 消息队列采用redis
mkdir download;cd download
wget http://download.redis.io/releases/redis-3.2.4.tar.gz
tar xzf redis-3.2.4.tar.gz
cd redis-3.2.4
make
# 或者直接yum安装
yum -y install redis
# 启动
systemctl start redis.service
# 重启
systemctl restart redis.service
# 停止
systemctl stop redis.service
# 查看状态
systemctl status redis.service
# 更改文件/etc/redis.conf
vim /etc/redis.conf
# 更改内容
daemonize no 改为 daemonize yes
bind 127.0.0.1 改为 bind 10.211.55.22(当前服务器ip)
# 重启redis
systemctl restart redis.service
```
#### 1.6.关于自启动
```
# Supervisor添加到自启动服务
systemctl enable supervisord.service
# redis添加到自启动服务
systemctl enable redis.service
# 关闭防火墙自启动
systemctl disable firewalld.service
```
至此，pyspider单个服务器运行环境搭建且部署完毕，启动localhost:5000进入web界面。

也可编写脚本运行，在/pyspider/supervisor/pyspider01.log查看运行状态。

### 2.分布式部署
刚才配置的服务器，将其命名为centos01，按照这样的配置，再分别部署两台centos02、centos03。

如下：
```
服务器名称	ip	说明
centos01	10.211.55.22	redis,mariaDB, scheduler
centos02	10.211.55.23	fetcher, processor, result_worker,phantomjs
centos03	10.211.55.24	fetcher, processor,,result_worker,webui
```
#### 2.1.centos01
进入服务器centos01，经过第一步，基本环境已经搭好，首先编辑配置文件/pyspider/config.json
```
{
  "taskdb": "mysql+taskdb://user_name:user_pass@10.211.55.22:3306/taskdb",
  "projectdb": "mysql+projectdb://user_name:user_pass@10.211.55.22:3306/projectdb",
  "resultdb": "mysql+resultdb://user_name:user_pass@10.211.55.22:3306/resultdb",
  "message_queue": "redis://10.211.55.22:6379/db",
  "logging-config": "/pyspider/logging.conf",
  "phantomjs-proxy":"10.211.55.23:25555",
  "webui": {
    "username": "",
    "password": "",
    "need-auth": false,
    "host":"10.211.55.24",
    "port":"5000",
    "scheduler-rpc":"http:// 10.211.55.22:5002",
    "fetcher-rpc":"http://10.211.55.23:5001"
  },
  "fetcher": {
    "xmlrpc":true,
    "xmlrpc-host": "0.0.0.0",
    "xmlrpc-port": "5001"
  },
  "scheduler": {
    "xmlrpc":true,
    "xmlrpc-host": "0.0.0.0",
    "xmlrpc-port": "5002"
  }
}
```
尝试运行下：
```
/root/anaconda3/envs/sbird/bin/python /root/git/pyspider/run.py -c /pyspider/config.json scheduler
# 报错
ImportError: No module named 'mysql'
# 下载 mysql-connector-python
cd ~/git/
git clone https://github.com/mysql/mysql-connector-python.git
# 安装
source activate sbird
cd mysql-connector-python
python setup.py install
# 安装redis
pip install redis
source deactivate
# 运行
/root/anaconda3/envs/sbird/bin/python /root/git/pyspider/run.py -c /pyspider/config.json scheduler
# 输出 ok
[I 161010 15:57:25 scheduler:644] scheduler starting...
[I 161010 15:57:25 scheduler:779] scheduler.xmlrpc listening on 0.0.0.0:5002
[I 161010 15:57:25 scheduler:583] in 5m: new:0,success:0,retry:0,failed:0
```

运行成功后，可直接更改/etc/supervisord.d/pyspider01.ini如下：
```
[program:pyspider01]

command      = /root/anaconda3/envs/sbird/bin/python /root/git/pyspider/run.py -c /pyspider/config.json scheduler
directory    = /root/git/pyspider
user         = root
process_name = %(program_name)s
autostart    = true
autorestart  = true
startsecs    = 3

redirect_stderr         = true
stdout_logfile_maxbytes = 500MB
stdout_logfile_backups  = 10
stdout_logfile          = /pyspider/supervisor/pyspider01.log
# 重载
supervisorctl reload
# 查看状态
supervisorctl status
```
centos01部署完毕。

#### 2.2.centos02
在centos02中，需要运行result_worker、processor、phantomjs、fetcher

分别建立文件：
```
/etc/supervisord.d/result_worker.ini

[program:result_worker]

command      = /root/anaconda3/envs/sbird/bin/python /root/git/pyspider/run.py -c /pyspider/config.json result_worker
directory    = /root/git/pyspider
user         = root
process_name = %(program_name)s
autostart    = true
autorestart  = true
startsecs    = 3

redirect_stderr         = true
stdout_logfile_maxbytes = 500MB
stdout_logfile_backups  = 10
stdout_logfile          = /pyspider/supervisor/result_worker.log
/etc/supervisord.d/processor.ini

[program:processor]

command      = /root/anaconda3/envs/sbird/bin/python /root/git/pyspider/run.py -c /pyspider/config.json processor
directory    = /root/git/pyspider
user         = root
process_name = %(program_name)s
autostart    = true
autorestart  = true
startsecs    = 3

redirect_stderr         = true
stdout_logfile_maxbytes = 500MB
stdout_logfile_backups  = 10
stdout_logfile          = /pyspider/supervisor/processor.log
/etc/supervisord.d/phantomjs.ini

[program:phantomjs]

command      = /pyspider/phantomjs --config=/pyspider/pjsconfig.json /pyspider/phantomjs_fetcher.js 25555
directory    = /root/git/pyspider
user         = root
process_name = %(program_name)s
autostart    = true
autorestart  = true
startsecs    = 3

redirect_stderr         = true
stdout_logfile_maxbytes = 500MB
stdout_logfile_backups  = 10
stdout_logfile          = /pyspider/supervisor/phantomjs.log
/etc/supervisord.d/fetcher.ini

[program:fetcher]

command      = /root/anaconda3/envs/sbird/bin/python /root/git/pyspider/run.py -c /pyspider/config.json fetcher
directory    = /root/git/pyspider
user         = root
process_name = %(program_name)s
autostart    = true
autorestart  = true
startsecs    = 3

redirect_stderr         = true
stdout_logfile_maxbytes = 500MB
stdout_logfile_backups  = 10
stdout_logfile          = /pyspider/supervisor/fetcher.log
```
在pyspider目录中建立pjsconfig.json
```
{
  /*--ignore-ssl-errors=true */
  "ignoreSslErrors": true,
  
  /*--ssl-protocol=true */
  "sslprotocol": "any",
  
  /* Same as: --output-encoding=utf8 */
  "outputEncoding": "utf8",
  
  /* persistent Cookies. */
  /*cookiesfile="e:/phontjscookies.txt",*/
  cookiesfile="pyspider/phontjscookies.txt",
  
  /* load image */
  autoLoadImages = false
}
```
下载phantomjs至/pyspider/文件夹，将git/pyspider/pyspider/fetcher/phantomjs_fetcher.js复制到phantomjs_fetcher.js
```
# 重载
supervisorctl reload
# 查看状态
supervisorctl status
# output
fetcher                          RUNNING   pid 3446, uptime 0:00:07
phantomjs                        RUNNING   pid 3448, uptime 0:00:07
processor                        RUNNING   pid 3447, uptime 0:00:07
result_worker                    RUNNING   pid 3445, uptime 0:00:07
centos02部署完毕。
```
2.3.centos03
部署这三个进程fetcher, processor, result_worker和centos02 一样，本服务器主要是在前面的基础上加上webui

建立文件：
```
/etc/supervisord.d/webui.ini

[program:webui]

command      = /root/anaconda3/envs/sbird/bin/python /root/git/pyspider/run.py -c /pyspider/config.json webui
directory    = /root/git/pyspider
user         = root
process_name = %(program_name)s
autostart    = true
autorestart  = true
startsecs    = 3

redirect_stderr         = true
stdout_logfile_maxbytes = 500MB
stdout_logfile_backups  = 10
stdout_logfile          = /pyspider/supervisor/webui.log
# 重载
supervisorctl reload
# 查看状态
supervisorctl status
# output
fetcher                          RUNNING   pid 2724, uptime 0:00:07
processor                        RUNNING   pid 2725, uptime 0:00:07
result_worker                    RUNNING   pid 2723, uptime 0:00:07
webui                            RUNNING   pid 2726, uptime 0:00:07
```
### 3.总结
访问 http://10.211.55.24:5000 即可，可以进行爬取
