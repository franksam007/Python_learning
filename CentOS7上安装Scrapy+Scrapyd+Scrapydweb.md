## 1. 安装scrapyd
### 1.1 依赖库
* Python 2.6 or above
* Twisted 8.0 or above
* Scrapy 0.17 or above
安装过程将负责安装缺少的库

### 1.2 安装scrapyd
* 利用PyPi库

`pip install scrapyd`

* 利用源代码(git)

`pip install --upgrade git+https:https://github.com/scrapy/scrapyd.git`

或:
```
git clone https://github.com/scrapy/scrapyd.git
cd scrapyd
python setup.py install
```

### 1.3 启动scrapyd
`scrapyd`

启动后可访问Web界面

`http://localhost:6800/`

### 1.4 配置
Scrapyd在以下位置搜索配置文件，并按优先顺序依次解析它们：
```
/etc/scrapyd/scrapyd.conf （Unix）
c:\scrapyd\scrapyd.conf （Windows）
/etc/scrapyd/conf.d/* （按字母顺序，Unix）
scrapyd.conf
~/.scrapyd.conf （用户主目录）
```
样例：
```
[scrapyd]
eggs_dir    = eggs
logs_dir    = logs
items_dir   =
jobs_to_keep = 5
dbs_dir     = dbs
max_proc    = 0
max_proc_per_cpu = 4
finished_to_keep = 100
poll_interval = 5.0
bind_address = 127.0.0.1
http_port   = 6800
debug       = off
runner      = scrapyd.runner
application = scrapyd.app.application
launcher    = scrapyd.launcher.Launcher
webroot     = scrapyd.website.Root

[services]
schedule.json     = scrapyd.webservice.Schedule
cancel.json       = scrapyd.webservice.Cancel
addversion.json   = scrapyd.webservice.AddVersion
listprojects.json = scrapyd.webservice.ListProjects
listversions.json = scrapyd.webservice.ListVersions
listspiders.json  = scrapyd.webservice.ListSpiders
delproject.json   = scrapyd.webservice.DeleteProject
delversion.json   = scrapyd.webservice.DeleteVersion
listjobs.json     = scrapyd.webservice.ListJobs
daemonstatus.json = scrapyd.webservice.DaemonStatus
```

## 2. 安装Scrapydweb
### 2.1 前置条件
确保所有主机都已经安装和启动Scrapyd 。

‼️ 如果需要远程访问 Scrapyd，则需在Scrapyd 配置文件 中设置 'bind_address = 0.0.0.0'，然后重启 Scrapyd。

### 2.2 安装
* 通过 pip:

`pip install scrapydweb`

如果 pip 安装结果不是最新版本的 scrapydweb，请先执行python -m pip install --upgrade pip，或者前往 https://pypi.org/project/scrapydweb/#files 下载 tar.gz 文件并执行安装命令 pip install scrapydweb-x.x.x.tar.gz

* 通过 git:

`pip install --upgrade git+https://github.com/my8100/scrapydweb.git`

或:
```
git clone https://github.com/my8100/scrapydweb.git
cd scrapydweb
python setup.py install
```
### 2.3 启动
通过运行命令 scrapydweb 启动 ScrapydWeb（首次启动将自动生成配置文件）。
访问 http://127.0.0.1:5000 （建议使用 Google Chrome 以获取更好体验）。
