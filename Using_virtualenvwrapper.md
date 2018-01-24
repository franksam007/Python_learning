### 1. 安装
#### 全局安装
   <code>$ pip install virtualenvwrapper</code>   
   或   
   <code>$ sudo pip install virtualenvwrapper</code>   
#### 用户安装
   <code>$ pip install --user virtualenvwrapper</code>   
   一般安装在<code>~/.local/</code>下
   
### 2. 配置、启动基础环境
#### 普通加载
   在启动脚本中(.bashrc, .profile, etc.)设置虚拟环境所在目录、开发项目目录和启动脚本和启动脚本位置:   
   <pre><code>export WORKON_HOME=$HOME/.virtualenvs
export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh</code></pre>
   在CentOS中，脚本安装在<code>/usr/bin/</code>下。
#### 懒加载
   在启动脚本中(.bashrc, .profile, etc.)加入下述行：
    <pre><code>export WORKON_HOME=$HOME/.virtualenvs
export PROJECT_HOME=$HOME/Devel
export VIRTUALENVWRAPPER_SCRIPT=/usr/local/bin/virtualenvwrapper.sh
source /usr/local/bin/virtualenvwrapper_lazy.sh</code></pre>   
   在CentOS中，脚本安装在<code>/usr/bin/</code>下。
#### 启动基础环境
   利用source命令执行启动脚本，从而启动基础环境

### 3. 创建、启动虚拟环境
#### 创建虚拟环境
   <code>mkvirtualenv -a /path/to/project_to_use_env -p $(which python2) env_name</code>   
   其中：   
   * <code>/path/to/project_to_use_env</code>是使用该虚拟环境的项目目录，启动虚拟环境时可自动切换到该路径
   * <code>env_name</code>是虚拟环境的命名
#### 查看可用虚拟环境
   使用<code>workon</code>命令可以查看可用虚拟环境
#### 激活虚拟环境
   使用<code>workon <i>env_name</i></code>命令激活虚拟环境<i>env_name</i>。
#### 退出当前虚拟环境
   当处于某个虚拟环境下时，使用<code>deactivate</code>退到系统的Python环境。
   
