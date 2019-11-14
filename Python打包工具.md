## Python 包管理工具解惑

### 一、困惑
作为一个 Python 初学者，我在包管理上感到相当疑惑（嗯，是困惑）。主要表现在下面几个方面：

这几个包管理工具有什么不同？
* distutils
* setuptools
* distribute
* disutils2
* distlib
* pip
什么时候该用pip，什么时候该用 setup.py ，它们有关系么？
easy_install、ez_setup.py、setup.py、setup.cfg 分别都是干啥的？
wheel 和 pip 的关系？
Egg 和 whl 的关系？
如何发布自己的模块（发布到PyPI）？
如何进行模块的私有发布（不发布到PyPI）？

### 二、python 包管理工具大乱斗

1. distutils
distutils 是 python 标准库的一部分，2000年发布。使用它能够进行 python 模块的 安装 和 发布。

setup.py 就是利用 distutils 的功能写成，我们可以看一个简单的 setup.py 的例子。

在这里可以看到关于 setupt.py 格式的所有详细描述：Writing the Setup Script。

要安装一个模块到当前的 python 环境中，可以使用这个模块提供的 setup.py 文件：

`python setup.py install`

下面的代码会发布一个 python 模块，将其打包成 tar.gz 或者 zip 压缩包：

`python setup.py sdist`

甚至能打包成 rpm 或者 exe 安装包：
```
python setup.py bdist_rpm
python setup.py bdist_wininst
```
2. setuptools 和 distribute
setuptools 是一个为了增强 distutils 而开发的集合，2004年发布。它包含了 easy_install 这个工具。

ez_setup.py 是 setuptools 的安装工具。ez 就是 easy 的缩写。

简单的说，setuptools 是一个项目的名称，是基础组件。而 easy_install 是这个项目中提供的工具，它依赖基础组件工作。

为了方便描述，下面文章中提到的 setuptools 被认为与 easy_install 同义。

使用 setuptools 可以自动 下载、构建、安装和管理 python 模块。

例如，从 PyPI 上安装一个包：
```
easy_install SQLObject
```
下载一个包文件，然后安装它：
```
easy_install http://example.com/path/to/MyPackage-1.2.3.tgz
```
从一个 .egg 格式安装：
```
easy_install /my_downloads/OtherPackage-3.2.1-py2.3.egg
```
distribute 是 setuptools 的一个分支版本。分支的原因可能是有一部分开发者认为 setuptools 开发太慢了。但现在，distribute 又合并回了 setuptools 中。因此，我们可以认为它们是同一个东西。事实上，如果你查看一下 easy_install 的版本，会发现它本质上就是 distribute 。
```
# easy_install --version
distribute 0.6.28
```
3. Eggs
Eggs 格式是 setuptools 引入的一种文件格式，它使用 .egg 扩展名，用于 Python 模块的安装。

setuptools 可以识别这种格式。并解析它，安装它。

想要详细了解，可以看看这篇：The Quick Guide to Python Eggs。
```
Eggs are to Pythons as Jars are to Java..
```

4. pip
注意，从此处开始，easy_install 和 setuptools 不再同义。

pip 是目前 python 包管理的事实标准，2008年发布。它被用作 easy_install 的替代品，但是它仍有大量的功能建立在 setuptools 组件之上。

pip 希望不再使用 Eggs 格式（虽然它支持 Eggs），而更希望采用“源码发行版”（使用 python setup.py sdist 创建）。这可以充分利用 Requirements File Format 提供的方便功能。

pip 可以利用 requirments.txt 来实现在依赖的安装。在 setup.py 中，也存在一个 install_requires 表来指定依赖的安装。它们的区别在哪里？可以看这篇文章：setup.py vs requirements.txt （中文版）。

pip 支持 git/svn/hg 等流行的 VCS 系统，可以直接从 gz 或者 zip 压缩包安装，支持搜索包，以及指定服务器安装等等功能。

pip vs easy_install 详细介绍了两者的不同。它们可以说是各占胜场，但 pip 明显优势更大。

5. wheel

wheel 本质上是一个 zip 包格式，它使用 .whl 扩展名，用于 python 模块的安装，它的出现是为了替代 Eggs。

wheel 还提供了一个 bdist_wheel 作为 setuptools 的扩展命令，这个命令可以用来生成 wheel 包。

pip 提供了一个 wheel 子命令来安装 wheel 包。当然，需要先安装 wheel 模块。

setup.cfg 可以用来定义 wheel 打包时候的相关信息。

Wheel vs Egg 详细介绍了 wheel 和 Eggs 格式的区别，很显然，wheel 优势明显。

Python Wheels 网站展示了使用 Wheels 发行的 python 模块在 PyPI 上的占有率。

pypip.in 也支持 wheel。

6. distutils2 和 distlib

distutils2 被设计为 distutils 的替代品。从2009年开发到2012年。它包含更多的功能，并希望以 packaging 作为名称进入 python 3.3 成为标准库的一部分。但这个计划 后来停滞了 。

distlib 是 distutils2 的部分，它为 distutils2/packaging 提供的低级功能增加高级 API，使其便于使用。

这里 介绍了 distlib 没有进入 python 3.3 标准库的一些原因。

因此，可以暂时不必了解这两个工具，静观其变即可。

### 三、工具选择

对于刚刚开始的新手来说，自然是使用 pip 而不使用 easy_install 了。

如果发布模块，当然是使用 wheel 格式。

### 四、发布自己的模块

对于 python3 程序员来说，当然应该先看这一篇：Distributing Python Modules。

另外，Tutorial on Packaging and Distributing Projects 也足够详细和官方。

而 Python2 程序员则应该看这篇 Distributing Python Modules 。

当然，setuptools 的官方文档也是不错的教程：Building and Distributing Packages with Setuptools 。

这篇教程可以用来入门：Sharing Your Labor of Love: PyPI Quick and Dirty

至于如何发布自己的模块到 PyPI 或者搭建自己的私有包管理服务器，上面的文章已经讲得非常清楚了。
