### 性能问题

   python的flask框架，简单、轻量，做web后端很实用。但是原生的python，执行效率并不高。这里不深究，感兴趣可以做性能对比测试。如果有业务需要，每秒超过 10k的并发，使用原生的python很占资源。(这也要看具体业务)



### 解决方案

   python生态中，有很多解释器/编译器，能够提升python代码的执行效率。比如JPython,CPython,Pypy等。整体而言，Pypy可以直接兼容python。Pypy使用JIT技术。感兴趣可看看pypy官网的数据，可参考，实际需要自己测 http://speed.pypy.org/



### 安装pypy,setuptools,flask,redis(python驱动),mysqldb(python驱动),gunicorn

#### pypy安装

<pre><code>mkdir /software

cd /software
wget http://dl.fedoraproject.org/pub/epel/6/x86_64/pypy-libs-2.0.2-1.el6.x86_64.rpm
wget http://dl.fedoraproject.org/pub/epel/6/x86_64/pypy-2.0.2-1.el6.x86_64.rpm
wget http://dl.fedoraproject.org/pub/epel/6/x86_64/pypy-devel-2.0.2-1.el6.x86_64.rpm

rpm -i pypy-libs-2.0.2-1.el6.x86_64.rpm
rpm -i pypy-2.0.2-1.el6.x86_64.rpm
rpm -i pypy-devel-2.0.2-1.el6.x86_64.rpm</code></pre>



#### setuptools安装

<pre><code>cd /software
wget https://pypi.python.org/packages/d3/16/21cf5dc6974280197e42d57bf7d372380562ec69aef9bb796b5e2dbbed6e/setuptools-20.10.1.tar.gz#md5=cc3f063d05e3bff4d3fa07a5a1017c3b

tar zxvf setuptools-20.10.1.tar.gz
cd setuptools-20.10.1
pypy setup.py install</code></pre>

#### 安装flask

<pre><code>cd /software
wget https://pypi.python.org/packages/db/9c/149ba60c47d107f85fe52564133348458f093dd5e6b57a5b60ab9ac517bb/Flask-0.10.1.tar.gz

tar zxvf Flask-0.10.1.tar.gz 
cd Flask-0.10.1
pypy setup.py  install</code></pre>

#### 安装redis

<pre><code>cd /software
wget https://pypi.python.org/packages/68/44/5efe9e98ad83ef5b742ce62a15bea609ed5a0d1caf35b79257ddb324031a/redis-2.10.5.tar.gz#md5=3b26c2b9703b4b56b30a1ad508e31083

tar zxvf redis-2.10.5.tar.gz 
cd redis-2.10.5
pypy setup.py  install</code></pre>

#### 安装MySQLdb

<pre><code>yum -y install python-devel.x86_64
cd /software
wget https://pypi.python.org/packages/a5/e9/51b544da85a36a68debe7a7091f068d802fc515a3a202652828c73453cad/MySQL-python-1.2.5.zip#md5=654f75b302db6ed8dc5a898c625e030c

unzip MySQL-python-1.2.5.zip
cd MySQL-python-1.2.5
pypy setup.py install</code></pre>

#### 安装gunicorn

<pre><code>cd /software
wget https://pypi.python.org/packages/1e/67/95248e17050822ab436c8a43dbfc0625a8545775737e33b66508cffad278/gunicorn-19.4.5.tar.gz#md5=ce45c2dccba58784694dd77f23d9a677

tar zxvf gunicorn-19.4.5.tar.gz
cd gunicorn-19.4.5
pypy setup.py install</code></pre>

### 测试pypy及flask

可以做下对比测试，压测的时候，注意下 /etc/security/limit.conf，/etc/sysctl.conf  配置调优等。
