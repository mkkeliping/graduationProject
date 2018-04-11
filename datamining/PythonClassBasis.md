## Python面向对象基础篇
简介：python设计之处就是面向对象化进行编程，python中创建类和对象是比较容易的。本篇介绍这一块
面向对象复习：类、类变量、数据成员、方法重写、封装、继承、实例化、对象
### python类创建
```.python 
class name:
class 是关键字，name是类名
```
### python类中方法创建
```.py
def name(self):
  print (self.name)
  print (self.age)
其中name是方法名字，self是参数必须写上，python中方法可以有多个
```
### 面向对象三大特征
#### 封装
将数据封装到某处，在从某处进行调用。通过self间接调用被封装的内容。
```.py
obj1=name('a',18)
obj1.name
 Python默认会将obj1传给self参数,即：obj1.name(obj1)，所以，此时方法内部的 self ＝ obj1，即：self.name 是a；self.age 是 18
```
#### 继承
python可以继承多个类，Java和c#只能继承一个类。如果继承多个类，寻找方法有两种，第一种是深度优先（相当于一棵树一直向下走），广度优先（一层层往下走）
#### 多态
python不支持Java和C#的强类型多态写法，python支持鸭子类型。
