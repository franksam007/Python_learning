操作系统：CentOS 7 64位
Python版本：3.6.3
安装pyspider的时候报错：
```
 
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/tmp/pip-install-ffraxsu8/pycurl/setup.py", line 944, in <module>
        ext = get_extension(sys.argv, split_extension_source=split_extension_source)
      File "/tmp/pip-install-ffraxsu8/pycurl/setup.py", line 606, in get_extension
        ext_config = ExtensionConfiguration(argv)
      File "/tmp/pip-install-ffraxsu8/pycurl/setup.py", line 101, in __init__
        self.configure()
      File "/tmp/pip-install-ffraxsu8/pycurl/setup.py", line 233, in configure_unix
        raise ConfigurationError(msg)
    __main__.ConfigurationError: Could not run curl-config: [Errno 2] No such file or directory: 'curl-config': 'curl-config'
```

后面试着单独安装pycurl的时候发现报我错误和这个是一样的。

看日志可以发现是安装pyspider的时候依赖于pycurl,于是程序就先安装pycurl了，版本是7.43.0.1，在安装pycurl的时候发现找不到"curl-config"这个文件，网上查下发现是因为CentOS自带的curl版本过低，
ok升级一下curl版本，这里选取与pycurl相同的版本，后来发现版本就算不同也是可以的

## 升级安装curl
### 第一步：下载curl
```
wget https://curl.haxx.se/download/curl-7.43.0.tar.gz
```

### 第二步： 解压
```
tar -zxf curl-7.43.0.tar.gz
```

### 第三步：编译
```
cd curl-7.43.0
./configure
```

### 第四步：安装
```
make && make install
```

###第五步：添加环境变量
```
vim /etc/profile 
```
```
# 添加下面的环境变量
PATH=$PATH:/usr/local/curl/bin/
```

### 第六步：使环境变量生效
```
source /etc/profile
```

### 第七步：测试curl是否配置成功
```
curl -V
```

curl升级安装成功截图

此时再安装pyspider就成功了
当然你也可以先安装pycurl库再安装pyspider
```
pip3 install pyspider
```

这个时候还有一个小问题，你在使用Python进行import的时候可能是报下面的错误
```
[GCC 4.4.7 20120313 (Red Hat 4.4.7-18)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import pycurl
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: pycurl: libcurl link-time version (7.19.7) is older than compile-time version (7.43.0)
>>>
```

虽然curl已经升级了，但是libcurl库里还没有升级，把原来的删除，再做一下软链接就行
libcurl库的前缀是libcurl.so
删除原来的libcurl库软链接
```
rm -f /usr/lib64/libcurl.so.4*
```

新安装的libcurl在/usr/local/lib/目录下
查看新安装的lib
```
 ll /usr/local/lib/ | grep curl
```

libcurl.so库
在lib64目录下创建软链接指定libcurl.so库
```
ln -s /usr/local/lib/libcurl.so.4.3.0 /usr/lib64/libcurl.so.4.3.0
ln -s /usr/local/lib/libcurl.so.4.3.0 /usr/lib64/libcurl.so.4
```
 
再次导入pycurl模块就正常了
python导入pycurl模块正常截图

至此问题解决。
