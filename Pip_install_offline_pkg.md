### 1、查看本地安装包 ###
  在已经通过在线方式或其他方式安装完成的环境中（即已经具备所需的Python包），执行下述命令查询已安装的包：   
  <code>#> pip list</code>
  执行下述命令，将已安装包列表转换为pip可识别的文件形式：   
  <code>#> pip freeze > requirement.txt</code>   
### 2、利用包列表将python包下载得到本地 ###
  在准备放置包文件（如<code>~/python-pkg/</code>的目录下，执行：
  <code>#> pip download -r _requirement.txt_</code>
  注意此处的<code>_requirement.txt_</code>是前面用<code>pip freeze</code>命令生成的包列表
### 3、安装本地包 ###
  执行下述命令，安装本地存储的包：   
  <code>#> pip install --no-index --find-links=\</path/to/local/package/\> \<package-name\> </code> 
  
