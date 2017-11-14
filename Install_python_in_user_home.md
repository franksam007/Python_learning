#### 0. 安装依赖包

      [root@vip ~]# yum install vim gcc make wget -y
      [root@vip ~]# yum install openssl-devel zlib-devel readline-devel sqlite-devel -y

#### 1. 将Python安装到本地目录

   * 创建目录，下载并解压Python包：    

      mkdir ~/python      
      cd ~/python   
      wget https://www.python.org/ftp/python/2.7.14/Python-2.7.14.tgz   
      tar zxfv Python-2.7.14.tgz   
      find ~/python -type d | xargs chmod 0755   
      cd Python-2.7.14   
    
   * 编译源文件：

      ./configure --prefix=$HOME/python
      make && make install
    
    注意prefix选项是必须的，用来指定make命令的输入目录，默认是/usr/local（一般用户没有权限）
    
   * 指定自用Python位置   
    python命令默认指向系统的python。编辑~/.bash_profile文件，增加下述行：

      export PATH=$HOME/python/bin/:$PATH
      export PYTHONPATH=$HOME/python
    
   * 更新环境

      source ~/.bash_profile
      
      或者重新登录

   * 检查python指向
    
      which python
      
      它应该指向自己的目录：~/python/bin/python

#### 2. Install pip

   * 执行下述命令，利用本地用户安装pip：

      wget --no-check-certificate https://bootstrap.pypa.io/get-pip.py -O - | python - --user
      
      或
      
      直接下载get-pip.py，然后执行python get-pip.py --user
      
   * 更新pip指向
   编辑~/.bash_profile，增加下述行：

      export PATH=$HOME/.local/bin:$PATH
   * 更新、校验pip指向
   
      source ~/.bashrc_profile
      
      或者重新登录

      which pip

     它应该指向本地目录：~/.local/bin
