## Uninstall pkg not installed by pip
当python包不是由pip安装的情况下，卸载时回报错，可用两种方法解决：

### 直接删除

直接找到的安装目录，直接通过sudo rm 去对文件夹进行删除。（如果找不到文件夹，可以通过下面python命令方式找到，一般都在dist-packages目录下）
```
import site
site.getsitepackages()
```

### 强行安装更新更高的版本

`sudo pip install numpy --ignore-installed numpy`

其中numpy替换为要更新的包名
