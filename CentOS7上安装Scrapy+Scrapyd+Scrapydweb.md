## 1. 安装scrapyd
### 1.1 依赖库
* Python 2.6 or above
* Twisted 8.0 or above
* Scrapy 0.17 or above
安装过程将负责安装缺少的库

### 1.2 安装scrapyd
利用PyPi库

`pip install scrapyd`

利用源代码(git)

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
