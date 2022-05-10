[TOC]

## `Recommended`

#### Dcumentation

> If you're looking for Python related learning materials, I highly, highly recommend you read **the official python documentation**
>
> You can click on this link: [Python 3.10.4 documentation](https://docs.python.org/zh-cn/3.10/index.html)

#### Software

> - Notepad++
> - Vscode
> - Pycharm

## Configuration environment

#### pip

##### pip command

```shell
pip list
python -m pip install --upgrade pip
# gen all module txt
pip freeze > module.txt
# del all modules
pip uninstall -r module.txt -y
# install all modules
pip install -r module.txt
```

##### pip change source

- source


```
清华：https://pypi.tuna.tsinghua.edu.cn/simple

阿里云：https://mirrors.aliyun.com/pypi/simple/

中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/

华中理工大学：http://pypi.hustunique.com/

山东理工大学：http://pypi.sdutlinux.org/ 

豆瓣：http://pypi.douban.com/simple/
```

- temporary use


> Add parameter when pip  `-i https://pypi.tuna.tsinghua.edu.cn/simple`

```shell
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple opencv-contrib-python
```

- permanent use

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

windows：Create pip directory, such as `C:\Users\xxx\pip`，new- built `pip.ini` 

Linux：modify  `~/.pip/pip.conf` 

Create if file does not exist, add the following content:

```ini
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host=mirrors.aliyun.com
```

#### Miniconda/conda

##### conda command

```shell
conda --version
conda list
conda update conda

# base不同版本历史
conda list --revisions
# 重置/回滚base环境
conda install --revision [revision number]
# eg:
conda install --revision 0
```

##### conda change source

```shell
# Existing mirror source
conda config --show channels

# tuna cource
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge 
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/

# ustc cource
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/

# show channel
conda config --set show_channel_urls yes

# default source
conda config --remove-key channels
```

##### Create | Activate | Deactivate | Delete Venv

```shell
# create
# 1-The version number is not required
conda create -n open_cv python=3.6
# 2-Create venv with specified package
conda create -n open_cv python=3.6 opencv-contrib-python

# activate
conda activate open_cv

# deactivate
conda deactivate

# delete
conda remove -n open_cv --all
```

##### no interpreter in the venv or conda envs directory

> `conda create -n new_env_test`只是创建一个没有任何内容的空conda环境。
>
> 如果想要在new_env_test中拥有bin文件夹，必须用`conda create -n my_env python`在创建环境时将python安装到env，或者`conda install -n my_env python`将Python添加到现有环境中。

#### Editor & IDE

##### notepad++

Running Python files, you need click `run` -> `run` -> input the following code：

```bash
# Run default interpreter
cmd /k python "$(FULL_CURRENT_PATH)" & ECHO. & PAUSE & EXIT

# Run conda virtual environment
cmd /k chdir /d $(CURRENT_DIRECTORY) & call conda activate spider & python $(FILE_NAME) & PAUSE & EXIT
```

##### vscode

##### pycharm

##### jupyter notebook

###### Jupyter notebook switch env

- 打开jupyter，点击new，会有现存的运行环境，若有创建的虚拟环境则直接使用；如无以前创建的虚拟环境，则点击terminal

- Activate the local environment to be added to jupyter（eg:tensorflow）

  ```bash
  conda activate tensorflow
  ```

- install ipykernel

  ```bash
  conda install ipykernel
  ```

- Inject the local environment into jupyter

  ```bash
  python -m ipykernel install --user --name tensorflow --display-name "tensorflow"	
  
  # display
  Installed kernelspec plot in C:\Users\***\AppData\Roaming\jupyter\kernels\plot
  ```

###### unable to automatically jump to `Chrome browser`

1. First close the Jupyter Notebook

2. open Anaconda Prompt or cmd，input ：`jupyter notebook --generate-config`
   在最后[y/n]字段输入y，回车。会发现生成了一个路径。按照路径找到这个文件，右键用记事本[任意一个编辑器]打开。

3. Add the following code at the end:

```bash
#c.NotebookApp.browser = ''
import webbrowser
webbrowser.register('chrome',None,webbrowser.GenericBrowser(u'C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe'))
c.NotebookApp.browser = 'chrome'
```

###### Jupyter notebook command

```bash
# view Jupyter notebook kernel
jupyter kernelspec list

# delete jupyter kernel
jupyter kernelspec remove kernelname
```

#### [Git-Cheat-Sheet](https://github.com/arslanbilal/git-cheat-sheet/blob/master/other-sheets/git-cheat-sheet-zh.md)

#### Yolov5

##### cuda

```shell
# Current installed version
nvcc -V
nvcc --version
# Maximum supported version
nvidia-smi
```

##### pytroch1.10.2

```bash
# CUDA 11.3
# pip 
pip3 install torch==1.10.2+cu113 torchvision==0.11.3+cu113 torchaudio===0.10.2+cu113 -f https://download.pytorch.org/whl/cu113/torch_stable.html
# conda 
conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
```

## Use skills

### Basic knowledge

#### string

##### format

- ##### %

1. ```python
   %[(name)][flags][width].[precision]typecode
   
       (name): 可选，用于选择指定的key
       flags: 可选，可供选择的值有:
           +: 右对齐；正数前加正好，负数前加负号；
           -: 左对齐；正数前无符号，负数前加负号；
            : 右对齐；正数前加空格，负数前加负号；
           0: 右对齐；正数前无符号，负数前加负号；用 0 填充空白处
       width: 可选，占有宽度
       .precision: 可选，小数点后保留的位数
       typecode: 必选
           s，获取传入对象的 __str__ 方法的返回值，并将其格式化到指定位置
           r，获取传入对象的 __repr__ 方法的返回值，并将其格式化到指定位置
           c，整数：将数字转换成其 unicode 对应的值，10进制范围为 0 <= i <= 1114111（py27则只支持 0-255）；字符：将字符添加到指定位置
           o，将整数转换成八进制表示，并将其格式化到指定位置
           x，将整数转换成十六进制表示，并将其格式化到指定位置
           d，将整数、浮点数转换成十进制表示，并将其格式化到指定位置
           e，将整数、浮点数转换成科学计数法，并将其格式化到指定位置（小写 e ）
           E，将整数、浮点数转换成科学计数法，并将其格式化到指定位置（大写 E ）
           f，将整数、浮点数转换成浮点数表示，并将其格式化到指定位置（默认保留小数点后6位）
           F，同上
           g，自动调整将整数、浮点数转换成 浮点型或科学计数法表示（超过6位数用科学计数法），并将其格式化到指定位置（如果是科学计数则是 e；）
           G，自动调整将整数、浮点数转换成 浮点型或科学计数法表示（超过6位数用科学计数法），并将其格式化到指定位置（如果是科学计数则是 E；）
           %，当字符串中存在格式化标志时，需要用 %% 表示一个百分号
   
   
   ```

2. eg:

   ```python
   [[fill]align][sign][#][0][width][,][.precision][type]
   
           fill: 【可选】空白处填充的字符
           align:【可选】对齐方式（需配合width使用）
               <: 内容左对齐
               >: 内容右对齐(默认)
               ＝: 内容右对齐，将符号放置在填充字符的左侧，且只对数字类型有效。 即使：符号 + 填充物 + 数字
               ^: 内容居中
           sign: 【可选】有无符号数字
               +: 正号加正，负号加负；
               -: 正号不变，负号加负；
               空格: 正号空格，负号加负；
           #：【可选】对于二进制、八进制、十六进制，如果加上 #，会显示 0b/0o/0x，否则不显示
           ,: 【可选】为数字添加分隔符，如：1,000,000
           width: 【可选】格式化位所占宽度
           .precision: 【可选】小数位保留精度
           type: 【可选】格式化类型
               传入” 字符串类型 “的参数
                   s: 格式化字符串类型数据
                   空白: 未指定类型，则默认是 None，同 s
               传入“ 整数类型 ”的参数
                   b: 将十进制整数自动转换成二进制表示然后格式化
                   c: 将十进制整数自动转换为其对应的 unicode 字符
                   d: 十进制整数
                   o: 将十进制整数自动转换成8进制表示然后格式化；
                   x: 将十进制整数自动转换成16进制表示然后格式化（小写 x ）
                   X: 将十进制整数自动转换成16进制表示然后格式化（大写 X ）
               传入“ 浮点型或小数类型 ”的参数
                   e: 转换为科学计数法（小写 e ）表示，然后格式化；
                   E: 转换为科学计数法（大写 E ）表示，然后格式化;
                   f: 转换为浮点型（默认小数点后保留 6 位）表示，然后格式化；
                   F: 转换为浮点型（默认小数点后保留 6 位）表示，然后格式化；
                   g: 自动在e和f中切换
                   G: 自动在E和F中切换
                  %: 显示百分比（默认显示小数点后 6 位）
   ```


- ##### format

1. ```python
   # 字符串格式化
   
   print("I am %s, %d years old." % ("Y", 18))
   print("I am %s, %s years old." % ("Y", 18))
   print("I am %s, %s years old." % ("Y", ["18"]))
   print("I am %s, %s years old." % ("Y", (18,)))
   
   # 保留
   print("percent %0.2f%%." % 23.36666)
   # 截取
   print("percent %.5s." % 23.36666)
   
   # 字典形式
   print("I am %(name)s, %(age)d years old." % {"name": "Y", "age": 18})
   
   # 其他
   print("I am \033[42;1m%(name)10s\033[0m, "
         "\033[42;1m%(age)-10d\033[0m years old."
         % {"name": "Y", "age": 18})
   
   print("user", "root", "password", "root", sep=":")
   ```

2. eg:

   ```python
   # format 格式
   print("I am {}, {} years old, who is {}.".format("Y", "22", "wise"))
   print("I am {0}, {1} years old, who is {2}.".format("Y", "22", "wise"))
   print("I am {1}, {1} years old, who is {1}.".format("Y", "22", "wise"))
   
   print("I am {name}, {age} years old, who is {adj}.".format(name="Y", age="22", adj="wise"))
   print("I am {name}, {age} years old, who is {adj}.".format(**{"name": "Y", "age": "22", "adj": "wise"}))
   
   print("I am {0[0]}, {0[1]} years old, who is {0[2]}.".format(["Y", "22", "wise"], [1, 2, 3]))
   print("I am {:s}, {:d} years old, who is {:f}.".format("good", 122, 12.22))
   print("I am {:s}, {:d} years old, who is {:f}.".format(*["good", 122, 12.22]))
   
   # 进制，百分比
   print("number: {:b}, {:o}, {:d}, {:x}, {:X}, {:0.2%}".format(12, 15, 17, 999, 999, 0.55))
   
   ```


#### [Built-in Functions](https://docs.python.org/zh-cn/3.10/library/functions.html)

### Python Module

##### 常用库

```
Requests
Kenneth Reitz写的最富盛名的http库。每个Python程序员都应该有它。

Scrapy
如果你从事爬虫相关的工作，那么这个库也是必不可少的。用过它之后你就不会再想用别的同类库了。

wxPython
Python的一个GUI（图形用户界面）工具。我主要用它替代tkinter。你一定会爱上它的。

Pillow 
    它是PIL（Python图形库）的一个友好分支。对于用户比PIL更加友好，对于任何在图形领域工作的人是必备的库。

SQLAlchemy
  个数据库的库。对它的评价褒贬参半。是否使用的决定权在你手里。

BeautifulSoup4 
我知道它很慢，但这个xml和html的解析库对于新手非常有用。

Twisted
  对于网络应用开发者最重要的工具。它有非常优美的api，被很多Python开发大牛使用。

NumPy
  我们怎么能缺少这么重要的库？它为Python提供了很多高级的数学方法。

SciPy 
既然我们提了NumPy，那就不得不提一下SciPy。这是一个Python的算法和数学工具库，它的功能把很多科学家从Ruby吸引到了Python。

matplotlib
一个绘制数据图的库。对于数据科学家或分析师非常有用。

Pygame
哪个程序员不喜欢玩游戏和写游戏？这个库会让你在开发2D游戏的时候如虎添翼。

Pyglet
3D动画和游戏开发引擎。非常有名的Python版本Minecraft就是用这个引擎做的。

pyQT
  Python的GUI工具。这是我在给Python脚本开发用户界面时次于wxPython的选择。

pyGtk   
  也是Python GUI库。很有名的Bittorrent客户端就是用它做的。

Scapy
   用Python写的数据包探测和分析库。

pywin32
一个提供和windows交互的方法和类的Python库。

nltk
自然语言工具包。我知道大多数人不会用它，但它通用性非常高。如果你需要处理字符串的话，它是非常好的库。但它的功能远远不止如此，自己摸索一下吧。

nose 
Python的测试框架。被成千上万的Python程序员使用。如果你做测试导向的开发，那么它是必不可少的。

SymPy
SymPy可以做代数评测、差异化、扩展、复数等等。它封装在一个纯Python发行版本里。

IPython 
怎么称赞这个工具的功能都不为过。它把Python的提示信息做到了极致。包括完成信息、历史信息、shell功能，以及其他很多很多方面。一定要研究一下它。
```

##### os

```python
os.getenv()
os.mkdir()
os.makedirs()
os.path.join()
```

###### create folder

```python
import os

path = r"E:\Code_files\Python\Py_learn\test_for_learning"

file_name = ['./file1','./file2','./file3']

# 指定路径创建文件夹

# 创建一级目录
try:
    os.mkdir(path = './file0')
except FileExistsError:
    print('文件夹已存在')

# 创建平行的多级目录
try:
    for name in file_name:
        os.mkdir(path+name)
except FileExistsError:
    print('文件夹已存在')

# 创建多级目录
try:
    os.makedirs(path + './file0' + './file1_1' + './file1_1_1')
except FileExistsError:
    print('文件夹已存在')
```

###### path.join

```python
# 路径的连接 - os.path.join() - 不太明白用途
path_1 = os.path.join(path, './file2', './file2_1', './file2_1_1')
os.mkdir(path_1)
```

###### create the specified file

```python
# version_1 函数中的 name 是新建文件的名字，msg 是写入的内容，类型为 str 类型，可任意传参

def text_create(path, name, msg):    
    # desktop_path = r"E:\Code_files\Python\Py_learn\test_for_learning"  # 新创建的txt文件的存放路径    
    full_path = path + '/'+ name + '.txt'   #也可以创建一个.doc的word文档    
    file = open(full_path, 'w')    # w 的含义为可进行读写
    file.write(msg)        #file.write()为写入指令
    file.close() 
    
path = r"E:\Code_files\Python\Py_learn\test_for_learning"
text = "new"
msg = "hello python"

text_create(path, text, msg)


# version_2

with open('E:/Code_files/Python/Py_learn/test_for_learning/text.txt',"w") as f :
    f.write("hello world")
    
#打开两个文件：地址可以改变
f  =  open('E:/Code_files/Python/Py_learn/test_for_learning/text.txt','r')
f1 =  open('E:/Code_files/Python/Py_learn/test_for_learning/text1.txt','w') 

#循环复制拷贝内容
for i in f:
    f1.write(i)

#关闭文件    
f.close()
f1.close()
```

##### shutil

###### 复制文件`shutil.copy(source, destination)`

shutil.copy(source, destination)将source处的文件复制到路径destination处的文件夹。

```python
os.chdir('C:\\')
#  part1 文件夹存在时，hello.txt 被复制到 C:\part1 中
#  part1 文件夹不存在时，hello.txt 被复制为 part1 文件，没有 .txt 后缀
shutil.copy('hello.txt', 'part1')
#  helloworld.txt 不存在时，hello.txt 被复制，名为 helloworld.txt
#  helloworld.txt 存在时，helloworld.txt 被 hello.txt 覆写 
shutil.copy('hello.txt','helloworld.txt')
```

###### 复制文件夹`shutil.copytree(source, destination)`

shutil.copytree(source, destination)复制整个文件夹，以及它包含的文件夹和文件。

```python
## 如果 destination 目录不存在， 系统会新建该文件夹

shutil.copytree('.\\EnglishSongs','.\\HeadFirstPython')

## 如果 source 目录，不存在，会报错

#  FileNotFoundError: [WinError 3] 系统找不到指定的路径。: '.\\EnglishSongs'

shutil.copytree('.\\EnglishSongs','.\\HeadFirstPython')
```

###### 移动文件和文件夹`shutil.move(source, destination)`

shutil.move(source, destination) 将路径source 处的文件和文件夹移动到路径 destination 处。

```python
#  destination 文件夹存在，且文件夹中不存在 A Whole New World.txt 时， A Whole New World.txt 被移动到 EnglishSongs 中

#  destination 文件夹存在，且文件夹中已存在 A Whole New World.txt 时， EnglishSongs 中 A Whole New World.txt 被新文件覆写

shutil.move('A Whole New World.txt', '.\\HeadFirstPython\\EnglishSongs')

# destination 文件夹不存在时， EnglishSongs 被作为一个文件名，A Whole New World.txt文件被移动到 .\\HeadFirstPython 并改名为 EnglishSongs(没有 .txt

# 的文本文件) 

shutil.move('A Whole New World.txt', '.\\HeadFirstPython\\EnglishSongs')

# destination 为一个文件名时，source 文件被移动并改名

shutil.move('phonemail.txt', '.\\HeadFirstPython\\phone_email.txt')

#  source 为一个文件夹时，EnglishSongs 移动到父文件夹中

#  destination 中存在同名文件夹时，程序报错

shutil.move('..\EnglishSongs','.')
```

###### 永久删除文件和文件夹`shutil.rmtree(path)`

shutil.rmtree(path) 可以删除一个文件夹及其中所有的内容。

```python
# 删除 path 处所有的文件和文件夹
shutil.rmtree(path)
```

###### os方法永久删除文件或空文件夹：

```python
# 删除 path 处的文件
os.unlink(path)

# 删除 path 处的文件夹，该文件夹必须为空
os.rmdir(path)
```

一个删除.txt文件的例子：

```python
#! python3
# 删除 quizfile0.txt -- quizfile34.txt，answerfile0.txt -- answerfile34.txt

import shutil
import os
import re

txtRegex = re.compile(r'(((quizfile)|(answerfile))(\d+)(\.txt))')
allTxtFile = txtRegex.findall(' '.join(os.listdir('C:\\HeadFirstPython')))

for afile in allTxtFile:
    afilename = list(afile)[0]
    os.unlink(afilename)

    # print(afilename)
```

```python
for filename in os.listdir('D:\\pyill\\pydsc'):
    if filename.endswith('.txt'):
        #删除以.txt结尾的文件
        os.unlink(filename)
    print filename

	if os.path.isdir('D:\\pyill\\' + filename) and  (not os.listdir('D:\\pyill\\+ filename')):
     #删除空文件夹
     os.rmdir('D:\\pyill\\' + filename)

#直接删除pyill文件夹及其子文件文件夹
shutil.rmtree('D:\\pyill')
```

###### 可恢复的删除`send2trash(path)`

send2trash()删除文件并将文件发送至回收站

```python
#  del file
send2trash.send2trash('phone_email.txt')

#  del folder
send2trash.send2trash('.\EnglishSongs')
```

```python
import send2trash

baconfile = open('bacon.txt','a')

baconfile.write('bacon is important')
baconfile.close()
send2trash.send2trash('bacon.txt')
```

##### math

```python
import math
```

##### sympy

##### numpy

##### pandas

###### *data color*

```python
import pandas as pd

def myColor(val):
    color = 'red' if val < 60 else color = 'green'
    return 'color:{}'.format(color)

df = pd.DataFrame(dict(math=[55, 87, 69, 43], english=[66, 89, 93, 51]))
df.style.applymap(mycolor)
```

##### lambda

###### *lambda syntax*

```python
lambda arg1 [, arg2, ..., argn]: expression 
```

###### *lambda&filter()*

```python
filter(function, iterable)
```

```python
# Define functions to filter odd numbers
def oddfn(self, x: int): 
	return x if (x % 2 == 1) else None
	
mylist = [5, 15, 10, 24]
filter_obj = filter(oddfn, mylist)

print("Odd list"，[item for item in filter_obj])

# lambda&filter()
mylist = [5, 15, 10, 24]
oddlist = list(filter(lambda x: (x%2 == 1), mylist))

print("Odd list"，oddlist)
```

###### *lambda&map()*

```python
map(function, iterable)
```

```python
mylist = [1, 2, 3, 4]
squ_list = list(map(lambda x: x**2, mylist))

print("second power"，squ_list)
```

###### *lambda&reduce()*

```python
reduce(function, iterable)
```

```python
'''
Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.
You must implement a solution with a linear runtime complexity and use only constant extra space.
'''
from functools import reduce

class Solution:
    def singleNumber(self, arr: list[int]) -> int:
        return reduce(lambda x, y: x ^ y, arr)
    
```

##### functools.reduce()`归约数据集`

##### openpyxl

##### requests

- [CSS&XPATH速查表1](./imgs/web_scraping/1css1.png)
- [CSS&XPATH速查表2](./imgs/web_scraping/1.反.tif)

##### [requests-cache](https://github.com/reclosedev/requests-cache)

##### beautiful soup

##### selenium

##### matplotlib

##### [pyecharts](https://pyecharts.org/#/zh-cn/intro)

> 教程：https://www.heywhale.com/mw/project/5eb7958f366f4d002d783d4a

##### seaborns

##### SciencePlots



##### opencv

```shell
# 仅安装主模块包
pip install opencv-python

# 安装完整包(包括主模块和附加模块)
pip install opencv-contrib-python
```

##### pipreqs

```bash
pip install pipreqs
# or
python -m pip install pipreqs
# use command
> pipreqs ./ --encoding=utf8
INFO: Successfully saved requirements file in ./req.txt
# install
pip install -r req.txt
```

##### pyinstaller

###  LeetCode

link: https://github.com/choya-lee/LeetCode
