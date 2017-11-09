1、安装所需开发包和python包
<pre>yum install sqlite-devel -y
pip install pysqlite</pre>

2、每次使用yum安装额外的包之后都需要重新安装python,否则可能会有各种奇奇怪怪的问题出现
<pre>cd Python-2.7.12
make altinstall</pre>
