## 第二章 Python数据分析简介
Python是一门简单易学且功能强大的编程语言，拥有高级数据结构，并且能够高效面向对象的进行编程。Python是一门编程语言；开发效率显著，Python被誉为胶水，因为其可以融合C/C++语言，可以解决Python的运行效率，其实Python的运行效率 与C/C++语言相媲美；随着程序库的开发，未来Python将成为主流语言。
#### 2.1 搭建Python开发平台
新生搭建Python环境理解基本概念请参考文档：
#### 2.2 Python使用入门
运行方式：有两种形式，第一种是在在命令窗口直接输命令语句，第二种是把完整代码写成.py文件，然后执行<br>
基本命令：基本运算、判断与循环、函数<br>
		数据结构：列表、元组、字典、集合统称为容器<br>
库的导入与添加：我们需要把库模块加载进来以提高Python的速度。例如import math
			导入库有三种方法，一种是直接利用import进行导入。第二种是利用from 库名 import 函数名，此方法应用于不导入所有方法，第三种可以给导入的库进行命名，比如import math as m.用的时候就用m调用相关方法。
导入future库：对于python 2来说print直接输出相关变量，对于python 3版本来说，print（a）把其当成一个模块进行输出，针对这一点，我们可以导入相关库使得python2与python3一致。
From _future_ import print_function
添加第三方库：参考2.3.1
#### 2.3 Python数据分析工具
Python本身数据分析功能不强，需要添加第三方数据库增加其功能，本书用到的库有Numpy（提供数组支持，以及相应的高效的处理函数）、Scipy（提供矩阵支持，以及矩阵相关的数据计算模块）、Matsplotlib（强大的可视化模块，做图库）、pandas（强大、灵活的数据分析和探索工具）、Scikit_learn（支持回归、聚集、分类等强大的机器学习库）。StatsModels(统计建模和计量经济学、包括描述统计、统计模型估计和推测)Keras（深度学习库、用于建立深度网络和深度学习模型）Gisim（用于文本主题模型的库、文本挖掘可以用的到）本书会对这些库做一些基本的讲解，如果想要详细了解，去官网找到相关的帮助文档进行深入学习，限于文本篇幅，只介绍了一些库，还有一些库没有介绍，我们可以搜索相关知识进行解决
##### 2.3.1 Numpy
Python内部虽然有数组，但当数据量较大时用python就比较慢，Numpy处理速度是C语言级别的。因此编程时，尽量使用内置函数进行处理数组以免发生瓶颈。由于我安装的是anoconda(一个开源的Python发行版本)里面安装了这个包，不用安装，如果小伙伴安装的是Python版本，那么需要安装此包。为了查证电脑是否安装此包，可以利用代码验证。[学习numpy的官方链接](http://www.numpy.org/index.html),[中文简单链接](http://reverland.org/python/2012/08/22/numpy/)
```py
import numpy as np    #导入numpy并且一般命名为np
a=np.array([2,0,1,5]) #定义数组
print(a)              #输出数组
print(a[:3])          #输出数组前三个值
print(min(a))         #输出数组最小值
```    
#####  2.3.2 Scipe
Numpy是在数组上有matlab的影子，scipe就可以说是半个matlab。因为numpy主要提供多维数组、一般数组，不是矩阵。scipe提供了真正的矩阵，并且提供了大量的基于矩阵的对象与函数。<br>
scipe包含的功能有最优化、线性代数、积分、插值、拟合、特殊函数、快速傅里叶变换、信号处理与图像处理、常积分方程求解以及其他科学与工程中常用的计算，显然，这些是为挖掘与建模必备的。 <br>
Scipe依赖于Numpy,因此需要安装完numpy后安装Scipe,利用代码试用是否安装成功，学习参考文档[学习scipy的官方链接](http://www.scipy.org/index.html),[中文简单链接](http://reverland.org/python/2012/08/24/scipy/)
```py
from scipy.optimize import fsolve #导入求解方程组的函数
def f(x):#定义求解方程组
    x1=x[0]
    x2=x[1]
    return [2*x1-x2**2-1,x1**2-x2-2]
result=fsolve(f,[1,1])#输入初值并求解
print(result)#结果为[ 1.91963957  1.68501606]正确
```
#####  2.3.3 Matplotlib
无论数据挖掘还是数学建模都离不开数据可视化的问题，Matplotlib是著名的二维绘图库，也可以进行简单的三维绘图，它不但提供了一整套和matlib相似并更为丰富的命令，可以让我们非常快捷的用Python可视化数据，并且允许输出达到出版质量的各种图像格式。代码验证环境，深入学习[官方链接](http://www.matplotlib.org/index.html)，[简单中文参考链接](http://reverland.org/python/2012/09/07/matplotlib-tutorial/),可以观看利用此工具制作出来的[图片](http://www.matplotlib.org/gallery.html)
```py
import numpy as np #导入Numpy
import matplotlib.pyplot as plt#导入 matplotlib.pyplot
x=np.linspace(0,10,1000)#作图的变量自变量
y=np.sin(x)+1#因变量y
z=np.cos(x**2)+1#因变量x
plt.figure(figsize=(8,4))#设置图像大小
plt.plot(x,y,label='$\sin x+1$',color='red',linewidth=2)#作图，设置标签，设置线条颜色、设置线条大小
plt.plot(x,z,'b--',label='$\cos x^2+1$')#作图，设置标签，设置线条类型
plt.xlabel('Time(s)')#x轴名称
plt.ylabel('Volt')#y轴名称
plt.title('A Simple Example' )#设置图片名称
plt.ylim(0,2.2)#显示Y轴范围
plt.legend()#显示图例
plt.show()#显示做图结果
```
**注意:**
如果在标签中使用中文例如横纵坐标命名则会不显示，这是因为默认英文字体，我们只需要作图前手动指定为中文即可，例如黑体（SimHei）<br>
```py
plt.rcParams['font.sans-serif']=['SimHei']
```
如果保存作图图像，其中负号可能不能正常显示，这时我们只需要
```py
plt.rcParams['axes.unicode_minus']=False
```
#####  2.3.4 Pandas
本书主力工具，是Python下最强大的数据分析和探索工具，它包含高级的数据结构和精巧的工具，使得处理数据快速和简单。Pandas构建在numpy，使得numpy为中心的应用更容易使用。其中Pandas名称来源于面板数据（panel data）和数据分析（data analysis）前期为金融数据分析开发。              
 Pandas功能强大支持类似于sql的增删改查，并且有丰富的数据处理函数；支持时间序列分析功能；支持灵活处理缺少数据。此版块内容丰富，可以写本书，参考webMaknney编写的《利用python进行数据分析》                
 具体内容后面补充
#####  2.3.5 StatModel
Pandas着眼于数据的读取，处理与探索，而StatModel更加注重数据的统计建模分析使得Python有了R语言的味道，StatModel支持与Pandas进行数据交互，因此组合成为Python下强大的数据挖掘工具
#####  2.3.6 Scikit-Learn 
#####  2.3.7 Keras 
#####  2.3.8 Gensim


#### 2.4 小结
本章主要对Python进行介绍，软件安装，软件入门、注意事项以及Python其他工具。后面会详细介绍。

