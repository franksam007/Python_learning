### 一、升级 Python 2.7.10 版本
#### 1. 准备安装包，系统是最小化安装
   下载安装依赖的相关包
<pre><code>[root@vip ~]# yum install vim gcc make wget -y
[root@vip ~]# yum install openssl-devel zlib-devel readline-devel sqlite-devel -y</code></pre>

   下载
<pre><code>[root@vip ~]# cd /usr/local/src
[root@vip ~]# wget https://www.python.org/ftp/python/2.7.10/Python-2.7.10.tgz</code></pre>
   解压
<pre><code>[root@vip ~]# tar -zxvf Python-2.7.10.tgz
[root@vip ~]# ls
Python-2.7.10  Python-2.7.10.tgz</code></pre>

#### 2. 编译配置安装

<pre><code>[root@vip ~]# cd Python-2.7.10
[root@vip Python-2.7.10]# ./configure --enable-shared --enable-loadable-sqlite-extensions \
    --prefix=/usr/local/python27 --with-zlib --with-ssl
[root@vip Python-2.7.10]# vim ./Modules/Setup    # 找到下边这一行内容，去掉注释
#zlib zlibmodule.c -I$(prefix)/include -L$(exec_prefix)/lib -lz
[root@vip Python-2.7.10]# make && make install</code></pre>
#### 3. 查看 python 版本信息

<pre><code>[root@vip Python-2.7.10]# python -V
Python 2.6.6
# 版本依旧是 2.6.6</code></pre>
#### 4. 用 python2.7 替换旧版本

<pre><code>[root@vip Python-2.7.10]# cd /usr/bin/
[root@vip bin]# ls python* -l   # 旧 python 版本信息
-rwxr-xr-x. 2 root root 4864 2月  22 2013  python
lrwxrwxrwx. 1 root root    6 10月 22 18:38 python2 -> python
-rwxr-xr-x. 2 root root 4864 2月  22 2013  python2.6
[root@vip bin]# mv /usr/bin/python /usr/bin/python2.6.6
[root@vip bin]# ln -s /usr/local/python27/bin/python2.7 /usr/bin/python
[root@vip bin]# ls python* -l
lrwxrwxrwx. 1 root root   33 10月 23 00:01 python -> /usr/local/python27/bin/python2.7
lrwxrwxrwx. 1 root root    6 10月 22 18:38 python2 -> python
-rwxr-xr-x. 2 root root 4864 2月  22 2013 python2.6
-rwxr-xr-x. 2 root root 4864 2月  22 2013 python2.6.6</code></pre>
#### 5. 重新验证 Python 版本信息

<pre><code>[root@vip bin]# python -V
Python 2.7.10
可以看到，系统识别的 python 版本已经是 python 2.7.10</code></pre>

执行 python -V 遇到的问题：

<pre><code>python: error while loading shared libraries: libpython2.7.so.1.0: cannot open shared object file: No such file or directory</code></pre>

原因：linux系统默认没有把/usr/local/python27/lib路径加入动态库搜索路径
解决：

<pre><code>[root@vip ~]# vim /etc/ld.so.conf
# 添加如下一行内容
/usr/local/python27/lib
[root@vip ~]# ldconfig  # 使新添加的路径生效</code></pre>
 

### 二、解决 yum 兼容性问题
因为 yum 是不兼容 Python 2.7 的，所以 yum 不能正常工作，我们需要指定 yum 的 Python 为 2.6。

#### 1. 升级 python 后 yum 出现的问题

<pre><code>[root@vip bin]# yum 
There was a problem importing one of the Python modules
required to run yum. The error leading to this problem was:
 No module named yum
... ... ... ...</code></pre>
#### 2. 编辑 yum 配置文件

<pre><code>[root@vip bin]# vim /usr/bin/yum
#!/usr/bin/python
# 第一行修改为 python2.6.6
#!/usr/bin/python2.6.6</code></pre>
#### 3. 验证 yum 问题解决

<pre><code>[root@vip bin]# yum repolist
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
... ... ... ...</code></pre>
 

### 三、升级 python 后，安装 pip 工具
#### 1. 下载安装

<pre><code>[root@vip ~]# wget https://bootstrap.pypa.io/get-pip.py
[root@vip ~]# python get-pip.py</code></pre>
#### 2. 设置软连接

<pre><code>[root@vip ~]# ln -s /usr/local/python27/bin/pip2.7 /usr/bin/pip</code></pre>
 

### 四、安装 ipython
<pre><code>[root@vip ~]# pip install ipython==1.2.1
[root@vip ~]# ln -s /usr/local/python27/bin/ipython /usr/bin/ipython</code></pre>
