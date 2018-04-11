## 数据预处理
数据处理方式有：数据清洗、数据集成、数据变换。                      

![数据预处理总展示图](https://github.com/mkkeliping/graduationProject/blob/master/picture/dataproprocess02.png)                      

### 数据清洗。
通过上次对数据的分析可以知道，里面存在大量的不需要的数据需要进行清洗。其中包括用户浏览信息路径的，这些数据不但没有作用，反而会影响推荐结果，
因此需要进一步进行筛选。                               
代码实现：
```.py
#coding=utf-8
import pandas as pd#pandas是强大的数据科学库
from sqlalchemy import create_engine#编程都需要ORM ，这个是python用的最广泛的数据连接方式

engine = create_engine('mysql+pymysql://root:mysql@127.0.0.1:3307/shuju?charset=utf8')
sql = pd.read_sql('all_gzdata', engine, chunksize = 10000)
for i in sql:
  d = i[['realIP', 'fullURL']] #只要网址列
  d = d[d['fullURL'].str.contains('\.html')].copy() #只要含有.html的网址
  #保存到数据库的cleaned_gzdata表中（如果表不存在则自动创建）
  d.to_sql('cleaned_gzdata', engine, index = False, if_exists = 'append')
  #这里是导出数据，第一个参数是数据库表的名字，第二个是地址、不存在就自动创建，如果有名字必须加上最后一句
  ```
  
 ### 选取具体列
 选取具体列，只需要把其列名写出来进行选取，并且进行复制
 ```.py
   d = i[['realIP', 'fullURL']] #只要网址列
   d = d[d['fullURL'].str.contains('\.html')].copy() #只要含有.html的网址
```
### 保存到数据库
保存到数据库要用方法to_sql（表明，地址，index，判断存在的一个if_exists = 'append'）如果不存在就会新建一个表。
### 数据变换
有些页面存在翻页情况，虽然网址不同，但属于同一类网页（比如翻页网页，后面包含-1，-2，-3等网页）实质是同一类网页。这些网页会影响用户推荐进而导致推荐不准确，对于这些数据，我们要进行数据变换。首先把这类数据进行还原，然后再去重，代码展示如下
```.py
import pandas as pd#pandas是强大的数据科学库
import warnings
warnings.filterwarnings("ignore", 'This pattern has match groups')
from sqlalchemy import create_engine#编程都需要ORM ，这个是python用的最广泛的数据连接方式
engine = create_engine('mysql+pymysql://root:mysql@127.0.0.1:3307/shuju?charset=utf8')
for i in sql: #逐块变换并去重
  d = i.copy()
  d['fullURL'] = d['fullURL'].str.replace('_\d{0,2}.html', '.html') #将下划线后面部分去掉，规范为标准网址
  d = d.drop_duplicates() #删除重复记录
  d.to_sql('changed_gzdata', engine, index = False, if_exists = 'append') #保存
```
#### 归类操作
![数据预处理总展示图](https://github.com/mkkeliping/graduationProject/blob/master/picture/dataproprocess01.png)    
这里对于倒数第二行有个错误，没有改正出来
```.py
import pandas as pd#pandas是强大的数据科学库
import warnings
warnings.filterwarnings("ignore", 'This pattern has match groups')
from sqlalchemy import create_engine#编程都需要ORM ，这个是python用的最广泛的数据连接方式
engine = create_engine('mysql+pymysql://root:mysql@127.0.0.1:3307/shuju?charset=utf8')
sql = pd.read_sql('all_gzdata', engine, chunksize = 10000)
for i in sql: #逐块变换并去重
  d = i.copy()
  d['type_1'] = d['fullURL'] #复制一列
  d.loc['type_1',d['fullURL'].str.contains('(ask)|(askzt)')] = 'zixun'
  #将含有ask、askzt关键字的网址的类别一归为咨询（后面的规则就不详细列出来了，实际问题自己添加即可）
  d.to_sql('splited_gzdata', engine, index = False, if_exists = 'append') #保存
  ```
知识点：这块中需要学习改变网页类型操作，删除重复操作
### BUG调试
bug界面
![groupy bug展示图](https://github.com/mkkeliping/graduationProject/blob/master/picture/dataproprocess04.png)
解决方法代码
简单解释就是不匹配，用输入的代码进行忽视它
```.py
import warnings
warnings.filterwarnings("ignore", 'This pattern has match groups')
```
原因解释图             


![原因解释图](https://github.com/mkkeliping/graduationProject/blob/master/picture/dataproprocess03.png)

