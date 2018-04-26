## 字符串的split用法
#### 简介
字符串的split用法,python中没有字符类型的说法，只有字符串。split相当于分割器，可以按照其后面的字符进行分割，也可以进行分割次数的设置。
分割N次后可以赋值为N+1个变量。
#### 实例
按照字符进行分类
```.py
str = ('www.google.com')
print(str)
str_split = str.split('.')
print(str_split)
结果展示：
['www', 'google', 'com']
```
按照字符进行分类，并设置分割次数（从头开始分割，分割N次有N+1个）代码展示
```.py
str = ('www.google.com')
str_split = str.split('.',1)
print(str_split)
结果展示：
['www', 'google.com']
```
按某一字符（或者字符串）分割，分割n次，并将分割完的的字符串赋值给新的N+1个变量,之一N表示分割次数
```.py
url=('www.google.com')
str1, str2 ,str3= url.split('.', 2)
print(str1)
print(str2)
print(str3)
结果展示：
www
google
com
注意点：这里如果重新赋值，记得要有N+1个变量进行承接数，个数不匹配会出现错误
```
