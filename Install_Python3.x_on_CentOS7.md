## Install Python 3.x on CentOS 7

### 利用Software Collection `scl`工具

Software Collections, also known as SCL is a community project that allows you to build, install, and use multiple versions of software on the same system, without affecting system default packages. 

由于Python 2.7.5是CentOS 7.0基础系统的关键部分，所以需要在不影响系统的情况下，并行安装Python 3.x版本。

#### 安装SCL Repo
`yum install centos-release-scl`

#### 安装Python 3.x
`sudo yum install rh-python36`

#### 启用Python 3.x
CentOS的默认环境使用Python 2.7.5，须系统新版本。

在当前shell中执行:

`scl enable rh-python36 bash`

#### 使用虚拟环境
1. 进入项目目录

`cd my_project_venv`

2. 启动新版本Python

`scl enable rh-python36 bash`

3. 建立虚拟环境

`python -m venv my_project_venv`

4. 启动虚拟环境

`source my_project_venv/bin/activate`

系统将显示：

`(my_project_venv) user@host:~/my_new_project$`

5. 关闭虚拟环境
`(my_project_venv) user@host:~/my_new_project$ deactivate`
