## 1. 使用解释器
  将python放入执行路径中，直接在命令行中运行：
  - 直接执行：<code>python</code>
  - 执行命令：<code>python -c command [arg] ...</code>，建议用引号把命令用引号括起来，因为命令中经常会有在shell中有特殊意义的字符
  - 执行模块：<code>python -m module [arg] ...</code>
### 1.1 传递命令行参数
  解释器的命令行参数可以通过sys模块的argv变量（数组）来访问，argv最小长度为1。
  - 当使用‘-c’，argv[0] = '-c'
  - 当使用‘-m’，argv[0] = 模块的全名
### 1.2 源代码编码
  python默认将源代码当做UTF-8编码。  
  使用特殊注释来指定特定的编码：   
  <code># -*- coding: encoding -*-</code>  
  例如：
  <code># -*- coding: utf-8 -*-</code>

## 2. 非正式简单介绍
### 2.1 Number
  - <code>**</code>计算乘方
  - 交互模式下，最后一个表达式的值被赋给<code>_</code>，后面的脚本可以直接引用（这个变量为只读变量，不可直接赋值）
  - 表达复数时，使用<code>j</code>或<code>J</code>表达虚数部分，例如<code>3+4j</code>
### 2.2 String
  - 可使用单引号或双引号
  - 字符串是不可变的
  - 特殊字符需要用<code>\\</code>转义
  - 在字符串前面加<code>r</code>（代表raw string）禁止转义，例如<code>print r'C:\some<b>\n</b>ame'</code>
  - 使用三个单引号或双引号表示多行字符串（<code>"""..."""</code>或<code>'''...'''</code>），行尾的换行符自动被包括在字符串中，也可以在行尾增加<code>\\</code>字符来禁止这种行为
  - 字符串可以用<code>+</code>连接
  - 字符串可以用<code>*</code>代表重复，例如<code>'Py'*3</code>会得到<code>'PyPyPy'</code>
  - 相邻的两个字符串（字面量）会自动连接，例如<code>'Py' 'thon'</code>会自动连接成<code>'Python'</code>。这个动作不适用于变量和表达式。
  - 使用括号括起来多行字符串，建立长字符串，例如：   
    <pre><code>text = ('Put several strings within parentheses '
            'to have them joined together.')</code></pre>
  - 字符串本身是一个字符的数组，可以用索引来访问其中的字符，索引从0开始。复数的索引是指从字符串尾向前计数
  - 利用索引可以对字符串分片，例如:
    * <code>word[0:2]</code>代表从索引0（包含）到2（不包含）的子串，即[0, 2)
    * <code>word[2:]</code>代表从索引2（包含）到尾的子串
    * <code>word[:4]</code>代表从头到索引4（不包含）的子串   
    索引越界处理：
    * 如果第一个索引超过字符串长度，分片则返回空字符串
    * 如果第二个索引超过字符串长度，分片自动在字符串尾部停止
  - 内置函数<code>len()</code>返回字符串长度
### 2.3 Unicode String
  - 在字符串前面加<code>u</code>建立Unicode字符串，例如<code>u'Hello World !'</code>
  - 在字符串中使用Unicode-Escape编码添加特殊字符，例如<code>u'Hello\u0020World !'</code>相当于<code>u'Hello World !'</code>
  - 在字符串前面加<code>ur</code>将使用Raw-Unicode-Escape编码，不会禁止形如<code>\uXXXX</code>的转义   
    例如：<code>ur'Hello\u0020World !'</code>仍然会变为<code>u'Hello World !'</code>，  
    而<code>ur'Hello<b>\\\\</b>u0020World !'</code>则会变为<code>u'Hello\\\\\\\\u0020World !'</code>
  - 特殊字符可以通过内置的encode函数将Unicode字符串转为其他编码，例如<code>u"äöü".encode('utf-8')</code>会编码为<code>'\xc3\xa4\xc3\xb6\xc3\xbc'</code>
  - 使用unicode函数可将字节序列转为unicode字符串，例如<code>unicode('\xc3\xa4\xc3\xb6\xc3\xbc', 'utf-8')</code>会变为<code>u'\xe4\xf6\xfc'</code>
### 2.4 List
  - 利用<code>[]</code>定义，例如<code>a = [1, 2, 3, 4]</code>
  - 可用<code>[:]</code>分片，类似字符串操作
  - 可以直接利用索引赋值，也可用<code>[:]</code>进行分片赋值
  - 可用<code>[:]</code>进行整个或分片清除，例如<code>a[0:2] = []</code>或<code>a[:] = []</code>
  - 可用<code>+</code>连接，例如<code>b = a + [5, 6, 7, 8]</code>
  - 可用append方法在附加元素，例如<code>b.append(9)</code>
  - 内置函数<code>len()</code>返回列表长度
  - 可建立嵌套列表，例如<code>d = [[11, 12, 13], [21, 22, 23]]</code>，使用<code>d[i][j]</code>访问元素
### 2.5 其他杂项
  - 支持多重赋值，例如<code>a, b = 1, 2</code>
  - 条件判断逻辑与C语言类似  
    * 0为false，非0为true
    * 对于字符串和列表，长度为0代表false，非零代表true
  - 使用_缩进_组织程序块，错误的缩进会引发程序逻辑错误
  - print函数会在不同项之间自动加上空格，例如<code>print 'Hello', a</code>会输出<code>Hello world</code>
  （假定<code>a = 'world'</code>），即'Hello'和a均不带空格，但最终输出会在两者之间加上空格  
