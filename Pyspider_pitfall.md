1. PySpider启动时报错
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
