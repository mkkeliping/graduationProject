## 统计点击次数总代码
注意：当进行分组统计后想要展示出来，记得把分组统计赋值给一个变量，然后在输出来，这样即可展示
```.py
#coding=utf-8
import pandas as pd#pandas是强大的数据科学库
from sqlalchemy import create_engine#编程都需要ORM ，这个是python用的最广泛的数据连接方式
engine = create_engine('mysql+pymysql://root:mysql@127.0.0.1:3307/shuju?charset=utf8')
sql = pd.read_sql('all_gzdata', engine, chunksize = 10000)
c=[i['realIP'].value_counts()for i in sql]#分块统计各个IP出现的次数
count3=pd.concat(c).groupby(level=0).sum()#合并统计结果，level=0表示按index进行分组
count3=pd.DataFrame(count3)#把series转化为DataFrame
count3=count3.reset_index()
count3.columns=['realIP','num']
count3[1]=1
count4=count3.groupby('num').sum()#统计不同点击次数分别出现的次数
```
## 连接数据库
连接数据库略参考上一篇
```.py
import pandas as pd#pandas是强大的数据科学库
from sqlalchemy import create_engine#编程都需要ORM ，这个是python用的最广泛的数据连接方式
engine = create_engine('mysql+pymysql://root:mysql@127.0.0.1:3307/shuju?charset=utf8')
sql = pd.read_sql('all_gzdata', engine, chunksize = 10000)
```
## 分块统计出现的次数
这块适合从数据库中读取数据时分块进行统计**某一属性的个数**，并作出最后合并用的。这里注意一下groupby函数，按照其后面的进行分组与数据库的groupby相
似。如果和下面代码相似的话，就是按照index进行分组，也就是最左边的一列。因为上一步骤是专门统计的某一列，所以此处某一列即为index，也就是按照某一列进行
分组
```.py
c=[i['realIP'].value_counts()for i in sql]#分块统计各个IP出现的次数
count3=pd.concat(c).groupby(level=0).sum()#合并统计结果，level=0表示按index进行分组
```
## series转化为DataFrame
此处需要学习，后续补充
```.py
count3=pd.DataFrame(count3)#把series转化为DataFrame
```
## 重置index
如果已经选出来一个原有属性作为列，这时希望把这个属性作为属性，在增加一个像123456789这样的index，我们可以进行重置，变成多一列的矩阵
```.py
count3=count3.reset_index()
```
## 为列命名
这里利用数组名然后利用columns方法就可以进行重新为列命名
```.py
count3.columns=['realIP','num']
```
## 新增一列
新增一列可以直接写名[1]=数值。这里加上一列是为了统计相同点击数的IP数。把其数都置为一，这样统计出回来就是数量。我们以后如果需要统计个数也可以借用这种
方法进行进行增加一列统计
```.py
count3[1]=1
            realIP  num  1
0            82033    2  1
1            95502    1  1
2           103182    1  1
3           116010    2  1
4           136206    1  1
5           140151    1  1
6           155761    1  1
7           159601    2  1
8           159758    1  1
9           213105    1  1
10          226318    2  1
11          235278    2  1
12          235790    2  1
13          270457    1  1
14          287601    2  1
15          358926    1  1
16          395385    2  1
17          418673    2  1
18          424206    3  1
19          430862    2  1
20          484209    1  1
21          496398    1  1
22          503676    1  1
23          540785    1  1
24          546996    1  1
25          606321    2  1
26          611448    8  1
27          614513    1  1
28          620302    1  1
29          629518    1  1
           ...  ... ..
230119  4294016014    2  1
230120  4294029180    9  1
230121  4294054926    1  1
230122  4294060347    2  1
230123  4294086926    1  1
230124  4294116791    1  1
230125  4294186868    1  1
230126  4294205040    4  1
230127  4294247863    3  1
230128  4294284558    7  1
230129  4294341134    1  1
230130  4294378935    3  1
230131  4294444471    2  1
230132  4294458682    3  1
230133  4294481678    1  1
230134  4294510007    3  1
230135  4294510713    8  1
230136  4294525553    1  1
230137  4294549006    1  1
230138  4294721082    3  1
230139  4294721905    1  1
230140  4294737166    1  1
230141  4294786618    4  1
230142  4294804343    1  1
230143  4294807822    1  1
230144  4294809358    2  1
230145  4294811150    1  1
230146  4294852154    3  1
230147  4294865422    2  1
230148  4294917690    1  1
```

## groupby的用法
groupyby(level=0)按照index进行分组，一般情况下可以理解为左边的一栏；groupyby（‘name’）就是按照name进行分类然后.sum就是把name值一样的进行
相加，这里记住不是统计个数，而是想加，统计个数是value_counts（）这个函数
```.py
pd.concat(c).groupby(level=0).sum()
count4=count3.groupby('num').sum()#统计不同点击次数分别出现的次数
               realIP       1
num                           
1      282822119614442  132119
2       94263287367104   44175
3       37197692424272   17573
4       21544774056238   10156
5       12573191322968    5952
6        8690131037955    4132
7        5516274211497    2632
8        4252335127905    2008
9        3110581459519    1482
10       2585641111647    1253
11       2018922763410     951
12       1571108398814     724
13       1266774320789     625
14       1094926995526     518
15       1029689081328     484
16        782148981881     387
17        739065993546     362
18        589543049156     293
19        586297008960     270
20        502306553174     247
21        419441307933     190
22        445502733704     213
23        322537474433     154
24        294649153178     135
25        305101095881     144
26        333462445180     147
27        254373860473     123
28        215604582141     111
29        232003161714     100
30        243608117906     109
               ...     ...
621         3684307470       1
643         4017821044       1
670          851799153       1
708         2955053173       1
752         3961223025       1
838         3063882359       1
845          794953849       1
897         1207702030       1
936         1383154801       1
955         1973622542       1
978         3549850737       1
1030         518198490       1
1050        3841730875       1
1053        4178061627       1
1089        1174147598       1
1147        1157370382       1
1305        1190924814       1
1332        2716436691       1
1353        3275680439       1
1413        2800322771       1
1455        2666105043       1
1682         242673847       1
1770         818595448       1
1858         276228279       1
1874         225896631       1
2000         259451063       1
2413        3791399227       1
3306        3875285307       1
41440       2609113527       1
42790       3812410744       1

```


