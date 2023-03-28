# Geospatial_Data_Analysis


# 2017年高教社杯全国大学生数学建模竞赛题目

## Question B: "Take pictures for money" task pricing
"Take photos for money" is a self-service model under the mobile Internet. Users download the APP, register as a member of the APP, and then receive tasks that need to be photographed from the APP (such as going to the supermarket to check the shelf status of certain commodities) to earn rewards designated by the APP for the tasks. This self-service crowd-sourcing platform based on mobile Internet provides enterprises with a variety of business inspection and information collection. Compared with traditional market research methods, it can greatly save investigation costs, effectively ensure the authenticity of survey data, and shorten the survey cycle. Therefore, APP becomes the core of the platform operation, and task pricing in APP is its core element. If the price is not reasonable, some tasks will be unsolicited, leading to the failure of product inspection.
Attachment 1 is task data for a closed project, including the location, pricing, and completion of each task (" 1 "for completed," 0 "for incomplete); Annex II is the member information data, including the member's location, credit value, the task booking time and booking quota given in reference to its reputation. In principle, the higher the member's reputation, the more priority to start task selection, and the larger the quota (in fact, the task allocation is based on the proportion of booking quota); Attachment 3 is a new check item task data, only the task location information. Please complete the following questions:

1. Study the task pricing rule of the project in Annex I and analyze the reasons for the unfinished tasks.
2. Design a new task pricing scheme for the item in Annex I and compare it with the original scheme.
3. In reality, multiple tasks may be located in a concentrated location, so users will scramble to choose them. One consideration is to combine these tasks together and publish them as a package. Under this consideration, how to modify the previous pricing model, what impact on the final task completion?
4. Present your task pricing plan for the new item in Annex III and evaluate the implementation effect of the plan.


B题 “拍照赚钱”的任务定价

“拍照赚钱”是移动互联网下的一种自助式服务模式。用户下载APP，注册成为APP的会员，然后从APP上领取需要拍照的任务（比如上超市去检查某种商品的上架情况），赚取APP对任务所标定的酬金。这种基于移动互联网的自助式劳务众包平台，为企业提供各种商业检查和信息搜集，相比传统的市场调查方式可以大大节省调查成本，而且有效地保证了调查数据真实性，缩短了调查的周期。因此APP成为该平台运行的核心，而APP中的任务定价又是其核心要素。如果定价不合理，有的任务就会无人问津，而导致商品检查的失败。  
附件一是一个已结束项目的任务数据，包含了每个任务的位置、定价和完成情况（“1”表示完成，“0”表示未完成）；附件二是会员信息数据，包含了会员的位置、信誉值、参考其信誉给出的任务开始预订时间和预订限额，原则上会员信誉越高，越优先开始挑选任务，其配额也就越大（任务分配时实际上是根据预订限额所占比例进行配发）；附件三是一个新的检查项目任务数据，只有任务的位置信息。请完成下面的问题：

1. 研究附件一中项目的任务定价规律，分析任务未完成的原因。
2. 为附件一中的项目设计新的任务定价方案，并和原方案进行比较。
3. 实际情况下，多个任务可能因为位置比较集中，导致用户会争相选择，一种考虑是将这些任务联合在一起打包发布。在这种考虑下，如何修改前面的定价模型，对最终的任务完成情况又有什么影响？
4. 对附件三中的新项目给出你的任务定价方案，并评价该方案的实施效果。

附件一：已结束项目任务数据  
附件二：会员信息数据  
附件三：新项目任务数据

## 二、案例目标及实现思路

## 三、数据获取与探索

###### 1、Folium地理信息可视化包安装

```python
pip install folium
```

###### 2、数据读取与地图可视化

```python
import pandas as pd
#1.读取任务数据和会员数据
A = pd.read_excel('附件一：已结束项目任务数据.xls') 
B = pd.read_excel('附件二：会员信息数据.xlsx')
```

```python
A
```

.dataframe tbody tr th:only-of-type { vertical-align: middle } \\3cpre>\\3ccode>.dataframe tbody tr th { vertical-align: top } .dataframe thead th { text-align: right }

|      | 任务号码 | 任务gps纬度 | 任务gps经度 | 任务标价 | 任务执行情况 |
| ---- | -------- | ----------- | ----------- | -------- | ------------ |
| 0    | A0001    | 22.566142   | 113.980837  | 66.0     | 0            |
| 1    | A0002    | 22.686205   | 113.940525  | 65.5     | 0            |
| 2    | A0003    | 22.576512   | 113.957198  | 65.5     | 1            |
| 3    | A0004    | 22.564841   | 114.244571  | 75.0     | 0            |
| 4    | A0005    | 22.558888   | 113.950723  | 65.5     | 0            |
| ...  | ...      | ...         | ...         | ...      | ...          |
| 830  | A0831    | 23.044062   | 113.125784  | 65.5     | 0            |
| 831  | A0832    | 22.833262   | 113.280152  | 72.0     | 1            |
| 832  | A0833    | 22.814676   | 113.827731  | 85.0     | 1            |
| 833  | A0834    | 23.063674   | 113.771188  | 65.5     | 1            |
| 834  | A0835    | 23.123294   | 113.110382  | 85.0     | 1            |

835 rows × 5 columns

```python
A.iloc[0,1]
```

```undefined
22.566142254795
```

```python
A.iloc[0,2]
```

```undefined
113.980836777953
```

```python
B
```

.dataframe tbody tr th:only-of-type { vertical-align: middle } \\3cpre>\\3ccode>.dataframe tbody tr th { vertical-align: top } .dataframe thead th { text-align: right }

|      | 会员编号 | 维度      | 经度       | 预订任务限额 | 预订任务开始时间 | 信誉值     |
| ---- | -------- | --------- | ---------- | ------------ | ---------------- | ---------- |
| 0    | B0001    | 22.947097 | 113.679983 | 114          | 06:30:00         | 67997.3868 |
| 1    | B0002    | 22.577792 | 113.966524 | 163          | 06:30:00         | 37926.5416 |
| 2    | B0003    | 23.192458 | 113.347272 | 139          | 06:30:00         | 27953.0363 |
| 3    | B0004    | 23.255965 | 113.318750 | 98           | 06:30:00         | 25085.6986 |
| 4    | B0005    | 33.652050 | 116.970470 | 66           | 06:30:00         | 20919.0667 |
| ...  | ...      | ...       | ...        | ...          | ...              | ...        |
| 1872 | B1873    | 22.840505 | 113.277245 | 1            | 08:00:00         | 0.0124     |
| 1873 | B1874    | 23.069415 | 113.287606 | 1            | 08:00:00         | 0.0121     |
| 1874 | B1875    | 23.333446 | 113.301736 | 1            | 08:00:00         | 0.0062     |
| 1875 | B1876    | 22.693506 | 113.994101 | 1            | 08:00:00         | 0.0036     |
| 1876 | B1877    | 23.133238 | 113.239864 | 1            | 08:00:00         | 0.0001     |

1877 rows × 6 columns

```python
#2.导入地图可视化包
import folium as f
#利用map函数创建地图，参数依次为地图中心位置（纬度，经度）、地图缩放大小、地理坐标系编码
M=f.Map([A.iloc[0,1],A.iloc[0,2]],zoom_start=14,crs='EPSG3857')
#在地图上画圆圈，利用Circle函数实现，参数依次为半径大小（单位：米）、圆心位置（纬度、经度）、颜色……
for t in range(len(A)): 
   f.Circle(radius=50, location=[A.iloc[t,1],A.iloc[t,2]], color='black', 
            fill=True, fill_color='black').add_to(M)

for t in range(len(B)): 
   f.Circle(radius=50, location=[B.iloc[t,1],B.iloc[t,2]], color='red', 
            fill=True, fill_color='red').add_to(M)
#保存地图，html文件
M.save('f.html')
```





![image](https://user-images.githubusercontent.com/55887580/228315320-f75211e2-a5cb-4e19-ab3f-76d776ea4b7c.png)


![image](https://user-images.githubusercontent.com/55887580/228315401-cc1f93fd-c932-45fe-9662-5a36d18a07a3.png)




## 四、指标计算

###### 1、指标设计

```undefined
探究影响任务定价的主要因素，是本案例的主要任务。实际上，一个任务的定价不仅与其周围的任务数量、会员数量有关，还与任务发布时间有一定关系。
通过分析数据，我们发现任务的发布时间有一定的规律，即任务从6：30开始发布一批，之后三分钟发布一批贸易知道8：00任务发放结束。
```

根据以上分析，对附件一的每个任务，设计了12个指标

字段名称 字段解释

```less
Z1  对每一个任务，计算其 Q 公里范围内的所有任务数量
Z2  对每一个任务，计算其 Q 公里范围内的所有任务平均价格
Z2	对每一个任务，计算其 Q 公里范围内的所有会员数量
Z3	对每一个任务，计算其 Q 公里范围内的所有会员信誉平均值
Z4	对每一个任务，计算其 Q 公里范围内的所有会员所有时段可预订任务限额
Z5	对每一个任务，计算其 Q 公里范围内的所有会员在 6:30 时段可预订任务限额
Z6	对每一个任务，计算其 Q 公里范围内的所有会员在 6:33-6:45 时段可预订任务限额
Z7	对每一个任务，计算其 Q 公里范围内的所有会员在 6:48-7:03 时段可预订任务限额
Z8	对每一个任务，计算其 Q 公里范围内的所有会员在 7:06-7:21 时段可预订任务限额
Z9	对每一个任务，计算其 Q 公里范围内的所有会员在 7:24-7:39 时段可预订任务限额
Z10	对每一个任务，计算其 Q 公里范围内的所有会员在 7:42-7:57 时段可预订任务限额
Z11	对每一个任务，计算其 Q 公里范围内的所有会员在 8:00 时段可预订任务限额
```

其中Q为公里

## 五、程序实现

#### 1、Z1~Z5的计算

```python
import pandas as pd      #导入pandas库
import math               #导入数学函数包
A=pd.read_excel('附件一：已结束项目任务数据.xls') 
B=pd.read_excel('附件二：会员信息数据.xlsx')
A_W0=A.iloc[0,1]  #第0个任务的维度
A_J0=A.iloc[0,2]  #第0个任务的经度
A_W1=A.iloc[1,1]  #第1个任务的维度
A_J1=A.iloc[1,2]  #第1个任务的经度
B_W0=B.iloc[0,1]  #第0个会员的维度
B_J0=B.iloc[0,2]  #第0个会员的经度
#第0个任务到第1个任务之间的距离
d1=111.19*math.sqrt((A_W0-A_W1)**2+(A_J0-A_J1)**2*
math.cos((A_W0+A_W1)*math.pi/180)**2);  
#第0个任务到第0个会员之间的距离
d2=111.19*math.sqrt((A_W0-B_W0)**2+(A_J0-B_J0)**2*
   math.cos((A_W0+B_W0)*math.pi/180)**2);
print('第0个任务到第1个任务之间的距离 ',d1)
print('第0个任务到第0个会员之间的距离 ',d2)
```

```undefined
第0个任务到第1个任务之间的距离  13.71765563354376
第0个任务到第0个会员之间的距离  48.41201229628393
```

```python
import pandas as pd     #导入pandas库
import numpy as np      #导入numpy库
import math               #导入数学函数库
A=pd.read_excel('附件一：已结束项目任务数据.xls') 
B=pd.read_excel('附件二：会员信息数据.xlsx')
A_W0=A.iloc[0,1]  #第0个任务的维度
A_J0=A.iloc[0,2]  #第0个任务的经度
# 预定义数组D1,用于存放第0个任务与所有任务之间的距离
# 预定义数组D2,用于存放第0个任务与所有会员之间的距离
D1=np.zeros((len(A)))
D2=np.zeros((len(B)))
for t in range(len(A)):
     A_Wt=A.iloc[t,1]  #第t个任务的维度
     A_Jt=A.iloc[t,2]  #第t个任务的经度
     #第0个任务到第t个任务之间的距离
     dt=111.19*math.sqrt((A_W0-A_Wt)**2+(A_J0-A_Jt)**2*
       math.cos((A_W0+A_Wt)*math.pi/180)**2);  
     D1[t]=dt
for k in range(len(B)):
     B_Wk=B.iloc[k,1] #第k个会员的维度
     B_Jk=B.iloc[k,2] #第k个会员的经度
     #第0个任务到第k个会员之间的距离
     dk=111.19*math.sqrt((A_W0-B_Wk)**2+(A_J0-B_Jk)**2*
         math.cos((A_W0+B_Wk)*math.pi/180)**2); 
     D2[k]=dk
```

```python
print('第0个任务到第t个任务之间的距离:',D1)
print('第0个任务到第k个会员之间的距离:',D2)
```

```makefile
第0个任务到第t个任务之间的距离: [  0.          13.71765563   2.1832139   20.68868509   2.49640437
  20.45047092   2.02098319   1.93986332   9.94037198   5.10756368
   6.03794875   6.31279022   2.03332205   7.71496502   8.17411737
   5.32790828   6.38589159   9.94579264   2.28216922   8.33877549
   9.04664348   6.61468372   6.37344585   4.04451543   2.77471416
   8.85894546   6.67029682   3.43526419   0.18415874  13.87878228
  12.09422895   5.53411942   2.53045351  13.17378353  11.98863144
   5.71679957  21.68853625  35.61596943  17.71053466  12.09682323
  12.89954308  20.72116616  29.25735922  15.16117831  10.28273508
   ..............................................................
   85.12968156  62.2476069   30.11050454  57.67224278  91.69956714]
第0个任务到第k个会员之间的距离: [ 48.4120123    1.71402098  85.23709477 ... 100.20090358  14.19957514
  85.36443222]
```

###### 对第0个任务计算指标z1~z5

```python
Z1 = len(D1[D1<5])
print('周围任务数量：',Z1)


Z2=A.iloc[D1<=5,3].mean()
Z3=len(D2[D2<=5])   
Z4=B.iloc[D2<=5,5].mean()
Z5=B.iloc[D2<=5,3].sum()
```

```undefined
周围任务数量： 18
```

```python
Z2=A.iloc[D1<=5,3].mean()
print('周围任务平均价格：',Z2)
```

```undefined
周围任务平均价格： 66.19444444444444
```

```python
Z3=len(D2[D2<=5])   
print('周围会员数量：',Z3)
```

```undefined
周围会员数量： 45
```

```python
Z4=B.iloc[D2<=5,5].mean()
print('周围会员平均信誉值：',Z4)
```

```yaml
周围会员平均信誉值： 1302.327115555555
```

```python
Z5=B.iloc[D2<=5,3].sum()
print('周围会员总共可预定任务限额：',Z5)
```

```undefined
周围会员总共可预定任务限额： 548
```

###### 所有任务计算指标z1~z5

```python
import pandas as pd     #导入pandas库
import numpy as np      #导入numpy库
import math               #导入数学函数包
A=pd.read_excel('附件一：已结束项目任务数据.xls') 
B=pd.read_excel('附件二：会员信息数据.xlsx')
# 预定义,存放所有任务的指标Z1、Z2、Z3、Z4、Z5
Z=np.zeros((len(A),6))
for t in range(len(A)):
    A_Wt=A.iloc[t,1]  #第q个任务的维度
    A_Jt=A.iloc[t,2]  #第q个任务的经度
    # 预定义数组D1,用于存放第q个任务与所有任务之间的距离
    # 预定义数组D2,用于存放第q个任务与所有会员之间的距离
    D1=np.zeros((len(A)))
    D2=np.zeros((len(B)))
    for i in range(len(A)):
       A_Wi=A.iloc[i,1]  #第t个任务的维度
       A_Ji=A.iloc[i,2]  #第t个任务的经度
       #第q个任务到第t个任务之间的距离
       d1=111.19*math.sqrt((A_Wt-A_Wi)**2+(A_Jt-A_Ji)**2*
          math.cos((A_Wt+A_Wi)*math.pi/180)**2);  
       D1[i]=d1
    for k in range(len(B)):
       B_Wk=B.iloc[k,1]          #第k个会员的维度
       B_Jk=B.iloc[k,2]  #第k个会员的经度
       #第q个任务到第k个会员之间的距离
       d2=111.19*math.sqrt((A_Wt-B_Wk)**2+(A_Jt-B_Jk)**2*
          math.cos((A_Wt+B_Wk)*math.pi/180)**2); 
       D2[k]=d2
    Z[t,0]=t
    Z[t,1]=len(D1[D1<=5])
    Z[t,2]=A.iloc[D1<=5,3].mean()
    Z[t,3]=len(D2[D2<=5])
    Z[t,4]=B.iloc[D2<=5,5].mean()
    Z[t,5]=B.iloc[D2<=5,3].sum()
```

```python
Z
```

```csharp
array([[0.00000000e+00, 1.80000000e+01, 6.61944444e+01, 4.50000000e+01,
        1.30232712e+03, 5.48000000e+02],
       [1.00000000e+00, 9.00000000e+00, 6.83888889e+01, 4.30000000e+01,
        1.24269663e+02, 1.52000000e+02],
       [2.00000000e+00, 2.40000000e+01, 6.58750000e+01, 6.00000000e+01,
        1.01404358e+03, 6.79000000e+02],
       ...,
       [8.32000000e+02, 1.10000000e+01, 6.80454545e+01, 3.00000000e+01,
        9.68627900e+01, 7.70000000e+01],
       [8.33000000e+02, 2.90000000e+01, 6.71206897e+01, 4.40000000e+01,
        5.06203720e+02, 8.61000000e+02],
       [8.34000000e+02, 1.20000000e+01, 7.74166667e+01, 1.00000000e+01,
        5.68841400e+01, 8.70000000e+01]])
```

#### 2、Z6~Z12的计算

```python
import datetime
def find_I(h1,m1,h2,m2,D2,B):
   I1=B.iloc[:,4].values>=datetime.time(h1,m1)
   I2=B.iloc[:,4].values<=datetime.time(h2,m2)
   I3=D2<=5
   I=I1&I2&I3
   return I
```

```python
#以第0个任务为例，计算z5~z12

import pandas as pd     #导入pandas库
import numpy as np      #导入nmypy库
import math             #导入数学函数模
A=pd.read_excel('附件一：已结束项目任务数据.xls') 
B=pd.read_excel('附件二：会员信息数据.xlsx')
Z=np.zeros((len(A),13))
A_W0=A.iloc[0,1]  #第0个任务的维度
A_J0=A.iloc[0,2]  #第0个任务的经度
D2=np.zeros((len(B))) #预定义，第0个任务与所有会员之间的距离
for k in range(len(B)):
    B_Wk=B.iloc[k,1]   #第k个会员的维度
    B_Jk=B.iloc[k,2]   #第k个会员的经度
    d2=111.19*math.sqrt((A_W0-B_Wk)**2+(A_J0-B_Jk)**2*
       math.cos((A_W0+B_Wk)*math.pi/180)**2);
    D2[k]=d2
    
Z5=B.iloc[D2<=5,3].sum()
Z6=B.iloc[find_I(6,30,6,30,D2,B),3].sum()
Z7=B.iloc[find_I(6,33,6,45,D2,B),3].sum()
Z8=B.iloc[find_I(6,48,7,3,D2,B),3].sum()
Z9=B.iloc[find_I(7,6,7,21,D2,B),3].sum()
Z10=B.iloc[find_I(7,24,7,39,D2,B),3].sum()
Z11=B.iloc[find_I(7,42,7,57,D2,B),3].sum()
Z12=B.iloc[find_I(8,0,8,0,D2,B),3].sum()

Z6_12=sum([Z6,Z7,Z8,Z9,Z10,Z11,Z12])
print('Z5= ',Z5)
print('sum(Z6~Z12)=',Z6_12)
```

```bash
Z5=  548
sum(Z6~Z12)= 548
```

#### 3、所有指标的计算

```python
import pandas as pd     #导入pandas库
import numpy as np      #导入nmypy库
import math             #导入数学函数模

A=pd.read_excel('附件一：已结束项目任务数据.xls') 
B=pd.read_excel('附件二：会员信息数据.xlsx')
Z=np.zeros((len(A),13))
for t in range(len(A)):
   A_Wt=A.iloc[t,1]  #第t个任务的维度
   A_Jt=A.iloc[t,2]  #第t个任务的经度
   D1=np.zeros(len(A))
   D2=np.zeros(len(B))
   for i in range(len(A)):
      A_Wi=A.iloc[i,1]  #第i个任务的维度
      A_Ji=A.iloc[i,2]  #第i个任务的经度
      d1=111.19*math.sqrt((A_Wt-A_Wi)**2+(A_Jt-A_Ji)**2*
         math.cos((A_Wt+A_Wi)*math.pi/180)**2);  
      D1[i]=d1
   for k in range(len(B)):
      B_Wk=B.iloc[k,1]   #第k个会员的维度
      B_Jk=B.iloc[k,2]   #第k个会员的经度
      d2=111.19*math.sqrt((A_Wt-B_Wk)**2+(A_Jt-B_Jk)**2*
         math.cos((A_Wt+B_Wk)*math.pi/180)**2);
      D2[k]=d2

   Z[t,0]=t
   Z[t,1]=len(D1[D1<=5])
   Z[t,2]=A.iloc[D1<=5,3].mean()
   Z[t,3]=len(D2[D2<=5])
   Z[t,4]=B.iloc[D2<=5,5].mean()
   Z[t,5]=B.iloc[D2<=5,3].sum()
   Z[t,6]=B.iloc[find_I(6,30,6,30,D2,B),3].sum()
   Z[t,7]=B.iloc[find_I(6,33,6,45,D2,B),3].sum()
   Z[t,8]=B.iloc[find_I(6,48,7,3,D2,B),3].sum()
   Z[t,9]=B.iloc[find_I(7,6,7,21,D2,B),3].sum()
   Z[t,10]=B.iloc[find_I(7,24,7,39,D2,B),3].sum()
   Z[t,11]=B.iloc[find_I(7,42,7,57,D2,B),3].sum()
   Z[t,12]=B.iloc[find_I(8,0,8,0,D2,B),3].sum()
np.save('Z',Z)
```

```python
Z
```

```lua
array([[  0.        ,  18.        ,  66.19444444, ...,   9.        ,
         13.        , 128.        ],
       [  1.        ,   9.        ,  68.38888889, ...,  12.        ,
         14.        ,  76.        ],
       [  2.        ,  24.        ,  65.875     , ...,  17.        ,
         13.        , 157.        ],
       ...,
       [832.        ,  11.        ,  68.04545455, ...,  16.        ,
          4.        ,  12.        ],
       [833.        ,  29.        ,  67.12068966, ...,  66.        ,
        109.        ,  38.        ],
       [834.        ,  12.        ,  77.41666667, ...,  11.        ,
          0.        ,  15.        ]])
```

## 五、任务定价模型构建

### 1、指标数据预处理

###### 1、空值处理

```python
#先将12个指标的存放数组Z转换成数据框，进而通过数据框的fillna()方法将空值填充
import numpy as np 
import pandas as pd
Z=np.load('Z.npy')
Data=pd.DataFrame(Z[:,1:])
Data=Data.fillna(0)
Z
```

```lua
array([[  0.        ,  18.        ,  66.19444444, ...,   9.        ,
         13.        , 128.        ],
       [  1.        ,   9.        ,  68.38888889, ...,  12.        ,
         14.        ,  76.        ],
       [  2.        ,  24.        ,  65.875     , ...,  17.        ,
         13.        , 157.        ],
       ...,
       [832.        ,  11.        ,  68.04545455, ...,  16.        ,
          4.        ,  12.        ],
       [833.        ,  29.        ,  67.12068966, ...,  66.        ,
        109.        ,  38.        ],
       [834.        ,  12.        ,  77.41666667, ...,  11.        ,
          0.        ,  15.        ]])
```

###### 2、相关性分析

```python
#我们通过数据处理获得的12个指标，其之间是否存在较强的相关性？
R = Data.corr()
R
```

.dataframe tbody tr th:only-of-type { vertical-align: middle } \\3cpre>\\3ccode>.dataframe tbody tr th { vertical-align: top } .dataframe thead th { text-align: right }

|      | 0          | 1          | 2          | 3          | 4          | 5          | 6          | 7          | 8          | 9          | 10         | 11         |
| ---- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
| 0    | 1.000000   | \-0.616120 | 0.703197   | 0.117067   | 0.740254   | 0.221523   | 0.288878   | 0.677010   | 0.546820   | 0.353876   | 0.558365   | 0.730319   |
| 1    | \-0.616120 | 1.000000   | \-0.637442 | \-0.236682 | \-0.591313 | \-0.223977 | \-0.331366 | \-0.385930 | \-0.361395 | \-0.466696 | \-0.461521 | \-0.549973 |
| 2    | 0.703197   | \-0.637442 | 1.000000   | 0.103925   | 0.745033   | 0.042267   | 0.200093   | 0.628015   | 0.523935   | 0.383107   | 0.461526   | 0.949560   |
| 3    | 0.117067   | \-0.236682 | 0.103925   | 1.000000   | 0.381843   | 0.588993   | 0.451751   | 0.137975   | 0.105006   | 0.171130   | 0.196037   | 0.072652   |
| 4    | 0.740254   | \-0.591313 | 0.745033   | 0.381843   | 1.000000   | 0.600282   | 0.682588   | 0.679009   | 0.730280   | 0.543601   | 0.732003   | 0.723339   |
| 5    | 0.221523   | \-0.223977 | 0.042267   | 0.588993   | 0.600282   | 1.000000   | 0.671223   | 0.146441   | 0.381157   | 0.347796   | 0.568981   | \-0.014438 |
| 6    | 0.288878   | \-0.331366 | 0.200093   | 0.451751   | 0.682588   | 0.671223   | 1.000000   | 0.209410   | 0.497034   | 0.362163   | 0.420605   | 0.133269   |
| 7    | 0.677010   | \-0.385930 | 0.628015   | 0.137975   | 0.679009   | 0.146441   | 0.209410   | 1.000000   | 0.297241   | 0.262464   | 0.470540   | 0.657746   |
| 8    | 0.546820   | \-0.361395 | 0.523935   | 0.105006   | 0.730280   | 0.381157   | 0.497034   | 0.297241   | 1.000000   | 0.468725   | 0.516445   | 0.499932   |
| 9    | 0.353876   | \-0.466696 | 0.383107   | 0.171130   | 0.543601   | 0.347796   | 0.362163   | 0.262464   | 0.468725   | 1.000000   | 0.470236   | 0.286150   |
| 10   | 0.558365   | \-0.461521 | 0.461526   | 0.196037   | 0.732003   | 0.568981   | 0.420605   | 0.470540   | 0.516445   | 0.470236   | 1.000000   | 0.398002   |
| 11   | 0.730319   | \-0.549973 | 0.949560   | 0.072652   | 0.723339   | \-0.014438 | 0.133269   | 0.657746   | 0.499932   | 0.286150   | 0.398002   | 1.000000   |

```python
#绘制各变量数据相关性的热力图
import seaborn as sns
import matplotlib
#指定默认字体
matplotlib.rcParams['font.sans-serif'] = ['SimHei']
matplotlib.rcParams['font.family']='sans-serif'
#解决负号'-'显示为方块的问题
matplotlib.rcParams['axes.unicode_minus'] = False
#绘制热力图关键代码
sns.heatmap(Data.corr(),cmap="YlGnBu_r",linewidths=0.1,vmax=1.0, square=True,linecolor='white', annot=True)
```

```x86asm
<matplotlib.axes._subplots.AxesSubplot at 0x19f7c163148>
```

![image-20230329012310702](https://gitee.com/flycloud2009_cloudlou/img/raw/master/img/202303290133521.png)


###### 3、标准化处理

```python
#这里采用均值-方差规范方法
#导入预处理库
from sklearn import preprocessing
#均值-方差规范化(Z-Score规范化)
data = preprocessing.scale(Data)
pd.DataFrame(data)
```

.dataframe tbody tr th:only-of-type { vertical-align: middle } \\3cpre>\\3ccode>.dataframe tbody tr th { vertical-align: top } .dataframe thead th { text-align: right }

|      | 0          | 1          | 2          | 3          | 4          | 5          | 6          | 7          | 8          | 9          | 10         | 11         |
| ---- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
| 0    | \-0.033442 | \-0.857642 | 0.193732   | 2.469184   | 1.147050   | 2.112414   | 1.915995   | \-0.476946 | 0.157467   | \-0.396735 | \-0.221478 | 0.422689   |
| 1    | \-0.753540 | \-0.182698 | 0.133707   | \-0.247119 | \-0.497675 | 0.125506   | \-0.816221 | \-0.653897 | \-0.901890 | \-0.209398 | \-0.181570 | \-0.068339 |
| 2    | 0.446624   | \-0.955893 | 0.643915   | 1.804475   | 1.691138   | 2.112414   | 2.414133   | 0.496284   | 0.687146   | 0.102830   | \-0.221478 | 0.696532   |
| 3    | \-1.313616 | 1.850677   | \-1.066782 | \-0.532798 | \-1.116524 | \-0.478514 | \-0.816221 | \-0.919324 | \-0.901890 | \-0.958745 | \-0.700374 | \-0.767110 |
| 4    | 1.406755   | \-0.925991 | 1.004062   | 1.433901   | 2.010946   | 2.112414   | 2.429229   | 1.513752   | 0.531358   | 0.914622   | 0.057879   | 0.838175   |
| ...  | ...        | ...        | ...        | ...        | ...        | ...        | ...        | ...        | ...        | ...        | ...        | ...        |
| 830  | 1.886820   | \-0.847878 | 0.463842   | \-0.269858 | 1.529158   | 0.411621   | 1.206524   | 0.982899   | 1.995764   | 1.539078   | 1.095488   | 0.960932   |
| 831  | \-0.353485 | 0.224953   | \-0.886709 | \-0.470258 | \-0.559976 | 0.522888   | \-0.152036 | \-0.919324 | \-0.652630 | \-0.209398 | \-0.381110 | \-0.776553 |
| 832  | \-0.593518 | \-0.288328 | \-0.256452 | \-0.310313 | \-0.809176 | \-0.478514 | \-0.529414 | \-0.676016 | \-0.434527 | 0.040384   | \-0.580650 | \-0.672682 |
| 833  | 0.846678   | \-0.572757 | 0.163719   | 0.633524   | 2.447048   | 3.225083   | 2.655655   | 0.562640   | 2.774703   | 3.162663   | 3.609695   | \-0.427167 |
| 834  | \-0.513507 | 2.593969   | \-0.856697 | \-0.402493 | \-0.767643 | \-0.478514 | \-0.816221 | 0.120263   | \-0.465684 | \-0.271844 | \-0.740282 | \-0.644353 |

835 rows × 12 columns

###### 4、主成分分析

```python
#1.导入主成分分析模块PCA
from sklearn.decomposition import PCA
#2.利用PCA创建主成分分析对象pca
pca = PCA(n_components = 0.9) # 保留95%的信息量，那么PCA就会自动选使得信息量>=95%的特征数量（贡献率)
#3.调用pca对象中的fit()方法，对待分析的数据进行拟合训练
pca.fit(data)
```

```scss
PCA(n_components=0.9)
```

```python
#4.调用pca对象中的transform()方法，返回提取的主成分
x= pca.transform(data)
x
```

```lua
array([[ 1.88813556,  2.75633337,  1.78617768,  0.48935664,  1.28646235,
        -0.48351853],
       [-1.1349675 , -0.2409384 ,  0.14757657,  0.64031776, -0.3301473 ,
        -0.263724  ],
       [ 3.10338507,  2.25231433,  1.32128386,  0.05102004,  1.2776191 ,
        -0.1233936 ],
       ...,
       [-1.48822184, -0.17173705, -0.31113801,  0.84155013, -0.01903963,
        -0.22246347],
       [ 5.59135074,  3.98004815, -2.65199462, -0.6723497 , -1.12130499,
         0.3550163 ],
       [-2.45968856, -0.13832587, -0.08423316, -1.3640026 , -0.2457046 ,
         1.51009867]])
```

```python
#5.通过pca对象中的components_属性、explained_variance_属性、explained_variance_ratio_属性，
#返回主成分分析中对应的特征向量、特征值和主成分方差百分比(贡献率)


#返回特征向量
tzxl = pca.components_
print('特征向量:')
print(tzxl)
print()

#返回特征值
tz = pca.explained_variance_
print('特征值:')
print(tz)
print()

#返回主成分方差百分比（贡献率）
gxl = pca.explained_variance_ratio_
print('方差百分比（贡献率）、每个特征在原始数据信息占比:')
print(gxl)
```

```lua
特征向量:
[[ 0.33374174 -0.28965409  0.32903688  0.14329823  0.39790519  0.20823662
   0.23896688  0.28231708  0.29428419  0.24414444  0.30591633  0.31244789]
 [-0.20402499  0.08763272 -0.32570851  0.40217845  0.07577229  0.53198212
   0.41760583 -0.22216794  0.0535007   0.11460792  0.12360449 -0.37294204]
 [ 0.06199872 -0.04408505  0.08889273  0.62706601  0.02467587  0.05848869
  -0.02328601  0.31765507 -0.44061552 -0.48336889 -0.19145921  0.15224327]
 [-0.10906755 -0.61400776  0.11189973  0.28168321 -0.15209996 -0.1469648
  -0.11233818 -0.30155108 -0.24740316  0.51486107 -0.2109514  -0.01245172]
 [-0.09748172  0.05587617  0.20586487  0.13773047  0.07432791 -0.16309512
   0.33468972 -0.37902011  0.48274625 -0.23269545 -0.54353412  0.23581049]
 [-0.24592211  0.56781209  0.07076979  0.2370881   0.07580318 -0.08781261
  -0.11464317  0.35107648  0.03399221  0.57193268 -0.25619566  0.11017719]]

特征值:
[6.02239628 2.1069067  0.91921536 0.74194921 0.63357744 0.48293426]

方差百分比（贡献率）、每个特征在原始数据信息占比:
[0.50126532 0.17536529 0.07650954 0.06175505 0.05273489 0.04019632]

这里我们可以看出，原来的12个指标数据，经过主分析分析后，在累计贡献率0.9以上的要求下，降为6个综合指标数据，即6个主成分。基于这六个主成分数据，就可以构建任务定价模型了
```

### 2、多元线性回归模型

```undefined
根据上面获得六个主成分数据，将附件1的任务定价数据拆分为未执行任务和已执行任务两种情况
```

```python
#线性回归
A=pd.read_excel('附件一：已结束项目任务数据.xls') 
A4=A.iloc[:,4].values
x_0=x[A4==0,:] #未执行任务主成分数据
x_1=x[A4==1,:] #执行任务主成分数据
y=A.iloc[:,3].values
y=y.reshape(len(y),1)
y_0=y[A4==0]#未执行任务定价数据
y_1=y[A4==1]#执行任务定价数据



from sklearn.linear_model import LinearRegression as LR
lr = LR()    #创建线性回归模型类
lr.fit(x_1, y_1) #拟合
Slr=lr.score(x_1,y_1)   # 判定系数 R^2
c_x=lr.coef_        # x对应的回归系数
c_b=lr.intercept_   # 回归系数常数项
print('判定系数： ',Slr)

```

```undefined
判定系数：  0.526173439561583

可以看出，多元线性回归模型的判定系数为 0.526173439561583，其线性关系较弱，所以这里考虑使用非线性神经网络模型
```

### 3、神经网络模型

```python
from sklearn.neural_network import MLPRegressor 
#两个隐含层300*5
clf = MLPRegressor(solver='lbfgs', alpha=1e-5,hidden_layer_sizes=(300,5), random_state=1,max_iter=5000) 
clf.fit(x_1, y_1);   
rv1=clf.score(x_1,y_1)
y_0r=clf.predict(x_0)
print('拟合优度： ',rv1)
```

```undefined
拟合优度：  0.9565623194412773
```

```python
print('未执行的任务，利用模型重新预测定价：',y_0r)
```

```undefined
未执行的任务，利用模型重新预测定价： [ 80.44388588  66.81514851  74.94944184  69.32090977  75.61555564
  66.50766349  73.39593324  64.55910094  64.21833084  70.95075732
  64.53694016  70.762575    51.73427798  80.17798174  70.86391429
  78.44687835  65.5200754   78.15422279  69.08827421  74.56798559
  70.95826185  73.31931118  49.51370471  79.07693939  82.8844776
  50.02346443  67.9868474   74.37683493  71.10611037  47.39867621
  54.85182513  65.89467657  50.02364029  72.44343038  70.79706155
  64.91992986  59.66025425  70.99421182  59.83164316  78.87093592
  76.62932912  46.13669937  81.40580123  74.56798559  70.49298111
  72.65927885  87.4222359   63.50814134  58.92072527  83.93482824
  ...............................................................
  71.34241185  76.13707588  72.39188053  74.79377183  77.57277078
  71.35024166  77.9940469   53.88832827]
```

## 六、方案评价

```python
xx=pd.concat((Data,A.iloc[:,[3]]),axis=1) #12个指标+任务定价，自变量
xx=xx.values#转化为数组
yy=A4.reshape(len(A4),1)                  #任务执行情况，因变量
#对自变量与因变量按训练80%、测试20%随机拆分
from sklearn.model_selection import train_test_split
xx_train, xx_test, yy_train, yy_test = train_test_split(xx, yy, test_size=0.2, random_state=4)

from sklearn import svm
#用高斯核，训练数据类别标签作平衡策略
clf = svm.SVC(kernel='rbf',class_weight='balanced')  
clf.fit(xx_train, yy_train) 
rv2=clf.score(xx_train, yy_train);#模型准确率
yy1=clf.predict(xx_test)
yy1=yy1.reshape(len(yy1),1)
r=yy_test-yy1
rv3=len(r[r==0])/len(r) #预测准确率
print('模型准确率： ',rv2)
print('预测准确率： ',rv3)
xx_0=np.hstack((Z[A4==0,1:],y_0r.reshape(len(y_0r),1)))#预测自变量
P=clf.predict(xx_0)   #预测结果，1-执行，0-未被执行
R1=len(P[P==1])      #预测被执行的个数
R1=int(R1*rv3)       #任务完成增加量
print('任务完成增加量： ',R1)
R2=sum(y_0r)-sum(y_0)   #成本增加额
print('成本增加额： ',R2)
```

```css
模型准确率：  0.9790419161676647
预测准确率：  0.6747904191616766
任务完成增加量：  105
成本增加额：  [374.76380159]
```





