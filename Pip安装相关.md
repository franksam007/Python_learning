# 1. Ubentu上安装
`#apt install python3-pip`

# 2. 利用get-pip.py安装
* 下载get-pip.py
  * 对于Python 3：https://bootstrap.pypa.io/get-pip.py
  * 对于Python 2：https://bootstrap.pypa.io/pip/2.7/get-pip.py
* 利用python执行get-pip.py
`python3 get-pip.py`

# 3. Pip错误处理

## pip错误 ImportError: No module named _internal
```
Traceback (most recent call last):
File "/home/ubuntu/.local/bin/pip", line 7, in <module>
     
from pip._internal import main
     
ImportError: No module named _internal
```
 
解决方案：强制重新安装pip3
```
wget https://bootstrap.pypa.io/get-pip.py  --no-check-certificate
     
sudo python3 get-pip.py --force-reinstall
```
或
```
python3 -m pip install --upgrade pip

pip install --upgrade pip
```
