## Install Python 3.x on CentOS 7

### 利用Software Collection `scl`工具

Software Collections, also known as SCL is a community project that allows you to build, install, and use multiple versions of software on the same system, without affecting system default packages. 

由于Python 2.7.5是CentOS 7.0基础系统的关键部分，所以需要在不影响系统的情况下，并行安装Python 3.x版本。

#### 安装SCL Repo
`yum install centos-release-scl`

#### 安装Python 3.x
`sudo yum install rh-python36`

#### 启用Python 3.x
CentOS的默认环境使用Python 2.7.5，须系统新版本。

在当前shell中执行:

`scl enable rh-python36 bash`

#### 使用虚拟环境
1. 进入项目目录

`cd my_project_venv`

2. 启动新版本Python

`scl enable rh-python36 bash`

3. 建立虚拟环境

`python -m venv my_project_venv`

CentOS7上如果报错:
```
Error: Command '['/home/superset/incubator-superset/venv/bin/python', '-Im', 'ensurepip', '--upgrade', '--default-pip']' returned non-zero exit status 1.
```

可以采用以下方法绕过去。
```
python -m venv foo --without-pip
source foo/bin/activate
curl https://bootstrap.pypa.io/get-pip.py | python
```

4. 启动虚拟环境

`source my_project_venv/bin/activate`

系统将显示：

`(my_project_venv) user@host:~/my_new_project$`

5. 关闭虚拟环境
`(my_project_venv) user@host:~/my_new_project$ deactivate`

## (另一种方法） CentOS 7中实现Python 3.6与2.7共存及版本切换
目前大多数Linux发行版本中自带的Python还都是2.7.x系列,但是越来越多的包包都开始抛弃2.7.x版本了,记录一下在CentOS 7中安装Python 3.6实现与自带2.7两个版本共存并可以自由切换

### 1.安装Python 3.6和pip
先添加阿里云镜像站的EPEL仓库，然后一键傻瓜yum走起
```
# cd /etc/yum.repo.d/
# curl -o epel-7.repo http://mirrors.aliyun.com/repo/epel-7.repo
#### yum-config-manager --enable repoID（使用yum repolist all来查看）
# yum clean all
# yum install -y python36 python36-setuptools
```

### 2.update-alternatives分别安装两个版本的python和pip
```
# alternatives --install /usr/bin/python python /usr/bin/python2.7 1
# alternatives --install /usr/bin/python python /usr/bin/python3.6 2
# mv /usr/bin/pip /usr/bin/pip.bak    //原有的pip不是软连接，先把它干掉
# alternatives --install /usr/bin/pip pip /usr/bin/pip2.7 1
# alternatives --install /usr/bin/pip pip /usr/local/bin/pip3.6 2
```
检查一下设置是否生效
```
# alternatives --display python
python - status is auto.
 link currently points to /usr/bin/python3.6
/usr/bin/python2.7 - priority 1
/usr/bin/python3.6 - priority 2
Current `best' version is /usr/bin/python3.6.
```
### 3.切换Python和pip版本
```
# alternatives --config python
 
There are 2 programs which provide 'python'.
 
  Selection    Command
-----------------------------------------------
   1           /usr/bin/python2.7
*+ 2           /usr/bin/python3.6
 
Enter to keep the current selection[+], or type selection number:
 
# alternatives --config pip
```
在这里输入对应版本的序号就可以完成一键切换了~

### 4.后续yum问题
当切换成Python3以后发现yum命令不能用了,一执行就会报错:
```
# yum
  File "/usr/bin/yum", line 30
    except KeyboardInterrupt, e:
                            ^
SyntaxError: invalid syntax
```
这是因为系统yum命令使用的还需要使用Python2,分别修改/usr/libexec/urlgrabber-ext-down和/usr/bin/yum文件
```
# vi /usr/libexec/urlgrabber-ext-down
# vi /usr/bin/yum
```
```
首行的#! /usr/bin/python
都改为#! /usr/bin/python2.7
```
这样在切换到Python3.6以后也可以正常使用yum了

### 5.firewall-cmd也需要手工设置一下
```
# vim /usr/bin/firewall-cmd
``` 
首行改为
```
#!/usr/bin/python2.7 -Es
```
### 6.补充说明
貌似CentOS7里面使用yum install python36后内置的就有pip,但是调用的时候不能直接pip install xxx这种写法,得这样写:
```
# python -m pip install [Package_to_install]
```
这里需要将python切换到3.6以后再执行
