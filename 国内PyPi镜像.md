## 国内镜像
阿里云 [http://mirrors.aliyun.com/pypi/simple/][1]

中国科技大学 [https://pypi.mirrors.ustc.edu.cn/simple/][2] 

豆瓣(douban) [http://pypi.douban.com/simple/][3] 

清华大学 [https://pypi.tuna.tsinghua.edu.cn/simple/][4]

中国科学技术大学 [http://pypi.mirrors.ustc.edu.cn/simple/][5]

华中理工大学：[http://pypi.hustunique.com/][6]

山东理工大学：[http://pypi.sdutlinux.org/][7] 

## 临时使用

可以在使用pip的时候加参数-i https://pypi.tuna.tsinghua.edu.cn/simple

例如：`pip install -i https://pypi.tuna.tsinghua.edu.cn/simplegevent`，这样就会从清华这边的镜像去安装gevent库。


## 永久修改，一劳永逸：

linux下，修改 ~/.pip/pip.conf (没有就创建一个)， 修改 index-url至tuna，内容如下：
```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple/
[install]
trusted-host=pypi.tuna.tsinghua.edu.cn
 ```

windows下，直接在user目录中创建一个pip目录，如：C:\Users\xx\pip，新建文件pip.ini，内容如下
```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple/
[install]
trusted-host=pypi.tuna.tsinghua.edu.cn
 ```
