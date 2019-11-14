## 1. PySpider安装报错

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

### 升级安装curl（注意：此方法导致系统的yum出错，因为yum依赖python2.7的pycurl调用libcurl，会造成版本不一致）
#### 第一步：下载curl
```
wget https://curl.haxx.se/download/curl-7.43.0.tar.gz
```

#### 第二步： 解压
```
tar -zxf curl-7.43.0.tar.gz
```

#### 第三步：编译
```
cd curl-7.43.0
./configure
```

#### 第四步：安装
```
make && make install
```

#### 第五步：添加环境变量
```
vim /etc/profile 
```
```
# 添加下面的环境变量
PATH=$PATH:/usr/local/curl/bin/
```

#### 第六步：使环境变量生效
```
source /etc/profile
```

#### 第七步：测试curl是否配置成功
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

### 另外方法
在安装 pyspider 的时候遇到了这个问题， pyspider 依赖 pycurl 这个库，而 pycurl 要求系统中存在相对应的库。(如果系统自动安装我们要先卸载)
```
pip3 uninstall pycurl

# 以下两句可以解决问题
sudo yum install libcurl-devel
sudo yum install libxml2-devel libxslt-devel python-devel


export PYCURL_SSL_LIBRARY=nss >> ~/.bashrc

source ~/.bashrc

pip3 install pycurl --global-option="--with-nss" --no-cache-dir

# 注意如果报错时，显示link time为openssl，则需要将环境变量和安装变量的nss换为openssl(ssl)
```

## 2. PySpider启动时报错
```
...
    raise ValueError("Invalid configuration:\n  - " + "\n  - ".join(errors))
ValueError: Invalid configuration:
  - Deprecated option 'dir_browser.enable': use 'middleware_stack' instead.
  - Deprecated option 'domaincontroller': use 'domain_controller' instead.
```
  这是WsgiDAV发布了版本 pre-release 3.x导致的。
  
* 解决办法1：
  
  修改 pyspider/webui/webdav.py 第203行：
```
  config = DEFAULT_CONFIG.copy()
 config.update({
    'mount_path': '/dav',
    'provider_mapping': {
        '/': ScriptProvider(app)
    },
    #'domaincontroller': NeedAuthController(app),
    'http_authenticator': {
        'HTTPAuthenticator':NeedAuthController(app),
    },
    
    'verbose': 1 if app.debug else 0,
    
    'dir_browser': {'davmount': False,
                    #'enable': True,
                    'msmount': False,
                    'response_trailer': ''},
})
dav_app = WsgiDAVApp(config)
```

然后执行：
#python setup.py install

* 解决方法2：

wsgidav发布的3.x版本目前仍然是测试版，相对于2.x(例如2.4.1)更改了一些用法，上面报错的两个部分就是的。pyspider的3.0及以上版本在安装时，会默认安装wsgidav的3.x版（具体的版本可能会有偏差）。其实上面错误信息已经提示该如何改了，不过那样改比较麻烦。可以换个方法，换回wsgidav的2.x版本就不会报错了。先把3.x版卸载，再装2.x版（pip安装wsgidav会默认安装2.x版  我的是2.4.1版）。下面是具体的卸载安装的命令
windows下进入cmd,(linux下打开终端）,输入： 
```
pip uninstall wsgidav  
pip install wsgidav
```
如果报错失败，按照下面的再试一次，（一般linux不会出错，windows下可能输入下面的命令）
 ```
 python -m pip uninstall wsgidav
python -m pip install wsgidav
 ```
如果安装的wsgidav版本还是3.x版本,可以在卸载这个版本之后， 在安装命令后面加上具体版本

例如  `python -m pip install wsgidav==2.4.1`
