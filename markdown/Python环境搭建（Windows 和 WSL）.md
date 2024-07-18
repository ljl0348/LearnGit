
## 1、Windows下Python环境搭建

#### 1.1 Python3.11或最新版本下载
   官方网址为：[Python Releases for Windows | Python.org](https://www.python.org/downloads/windows/)
   选择对应版本后点击- Download Windows installer (64-bit)即可下载；
#### 1.2 安装Python 3.11或其他版本
直接打开安装包，界面如下：![[1720769109070.png]]
**将Add python.exe to PATH选中，将Python的安装路径添加到系统环境变量的Path变量中。**

Install Now为默认路径安装，Customize installation为选定路径安装。 
若选择Install Now则自动安装完成。[]()

若选择Customize installation(推荐)--------------点击Next----------更改安装路径-------点击Install----------安装完成-----------close
![[1720769859471.png]]
![[Pasted image 20240712153922.png]]

#### 1.3 Python环境变量配置

首先检测安装是否成功：
	按住键盘win+R，输入cmd然后点击确定，输入python，点击回车，出现图示界面则表示安装成功，环境变量配置成功。
![[Pasted image 20240712155153.png]]


若无上述提示则需要手动编辑系统环境变量。下方搜索栏输入“编辑系统环境变量”，并打开。
![[Pasted image 20240712154356.png]]
选择“环境变量”----------"系统变量"中选中"Path"，点击编辑
![[Pasted image 20240712160011.png]]
![[Pasted image 20240712160159.png]]
点击“新建”，输入Python的安装路径------点击确定，并进入cmd输入python去检测安装状态。


## 2、WSL下Python环境搭建
#### 2.1 python安装
	---------打开WSL终端
	---------输入python3 --version来确认Python是否已安装，并查看其版本号。
	---------如果Python未安装或需要安装pip和venv，可以使用以下命令：
	
	sudo apt update
	sudo apt upgrade
	sudo apt install python3-pip
	sudo apt install python3-venv

--------------
# 二、Python常用库安装
### 2.1 Windows下安装pandas、matplotlib、ipython、scikit-learn

进入命令终端后输入
	pip install pandas matplotlib ipython scikit-learn
--------等待自动安装完成---------安装完成后显示如下界面：
![[Pasted image 20240712171602.png]]

### 2.2 Windows下安装Numpy

----首先查看自己的Python版本 命令窗口下输入python查看，此处版本为3.11.9
![[Pasted image 20240712171817.png]]
----在cmd下输入python -v查看自己安装的位置
![[Pasted image 20240712172655.png]]

----下载对应版本的numpy
下载链接为：[numpy · PyPI](https://pypi.org/project/numpy/#files)

![[Pasted image 20240712172939.png]]
		(cp311 表示python 3.11          win_and64表示windows 64位)
	
----将下载的文件放到第一步python目录下的Scripts文件夹当中
![[Pasted image 20240712173405.png]]

-----重新打开下cmd,     cd到numpy目录下
![[Pasted image 20240712173844.png]]

----输入pip install numpy文件名字＋文件后缀
![[Pasted image 20240712174115.png]]

----开始菜单打开Python
![[Pasted image 20240712174315.png]]

----输入代码验证是否成功：
from numpy import *
		
random.rand(3,11)   

		代码中(3,11)改成对应的版本
----出现下面情况表示安装成功：
![[Pasted image 20240712174635.png]]

### 2.3 Windows下安装scipy库

----下载对应版本的scipy
下载链接为：[scipy · PyPI](https://pypi.org/project/scipy/#files)
![[Pasted image 20240712175048.png]]

----将下载的文件放到第一步python目录下的Scripts文件夹当中
![[Pasted image 20240712175227.png]]

-----重新打开下cmd,     cd到numpy目录下
![[Pasted image 20240712173844.png]]

----输入pip install numpy文件名字＋文件后缀
![[Pasted image 20240712175420.png]]

### 2.4 WSL下安装常用库

打开WSL终端，输入以下命令：
----（1）numpy库安装
		pip install numpy
		![[Pasted image 20240712181118.png]]
---- (2) pandas库安装
		pip install pandas
		![[Pasted image 20240712181252.png]]
---- （3）scipy安装
		pip install scipy
		![[Pasted image 20240712181408.png]]
		
----（4）matplotlib库安装
		pip install matplotlib
		![[Pasted image 20240712181557.png]]