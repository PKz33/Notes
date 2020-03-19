## Numpy
- **高效的运算工具**  
```
import numpy as np

score = np.array([[1, 2, 3, 4],
                 [5, 6, 7, 8]])

type(score) # numpy.ndarray
```  
- **ndarray与Python原生list运算效率对比**  
![](./Pics/Numpy内存风格.png)
1. `ndarray`存储相同类型，通用性不强；`Python`的`list`可存储不同类型，通用性强  
2. `ndarray`支持向量化（并行化）运算  
```
import random
import time

# 生成一个大数组
python_list = []

for i in range(100000000):
    python_list.append(random.random())

ndarray_list = np.array(python_list)
len(ndarray_list)  # 100000000

# 原生pythonlist求和
t1 = time.time()
a = sum(python_list)
t2 = time.time()
d1 = t2 - t1

# ndarray求和
t3 = time.time()
b = np.sum(ndarray_list)
t4 = time.time()
d2 = t4 - t3

d1  # 0.5563862323760986
d2  # 0.17122745513916016
```
- **ndarray**  
1. 属性  
```
score.shape  # (2, 4)
score.ndim  # 2
score.size  # 8
score.dtype  # dtype('int32')
score.itemsize  # 4
```  
2. 形状
```
a = np.array([[1,2,3],[4,5,6]])
b = np.array([1,2,3,4])
c = np.array([[[1,2,3],[4,5,6]],[[1,2,3],[4,5,6]]])

a.shape  # (2, 3)
b.shape  # (4,)
c.shape  # (2, 2, 3)
```  
3. 类型  
```
data = np.array([1.1, 2.2, 3.3])

data.dtype  # dtype('float64')

# 创建数组的时候指定类型
np.array([1.1, 2.2, 3.3], dtype="float32")  # array([1.1, 2.2, 3.3], dtype=float32)

np.array([1.1, 2.2, 3.3], dtype=np.float32)  # array([1.1, 2.2, 3.3], dtype=float32)
```
4. 生成数组的方法  
```
# 生成全0的数组
np.zeros(shape=(3, 4), dtype="float32")

# 生成全1的数组
np.ones(shape=[2, 3], dtype=np.int32)

# 从现有数组生成
# np.array() 深拷贝
data1 = np.array(score)

# np.asarray() 浅拷贝
data2 = np.asarray(score)

# np.copy() 深拷贝
data3 = np.copy(score)

score[1, 1] = 1000

# 生成固定范围的数组
np.linspace(0, 10, 5)  # array([ 0. ,  2.5,  5. ,  7.5, 10. ])
np.arange(0, 10, 5)  # array([ 0,  5, 10])

# 生成随机数组
# 均匀分布
data1 = np.random.uniform(low=-1, high=1, size=1000000)

import matplotlib.pyplot as plt

# 1. 创建画布
plt.figure(figsize=(20, 8), dpi=80)

# 2. 绘制直方图
plt.hist(data1, 1000)

# 3. 显示图像
plt.show()

# 正态分布
data2 = np.random.normal(loc=1.75, scale=0.1, size=1000000)

# 1. 创建画布
plt.figure(figsize=(20, 8), dpi=80)

# 2. 绘制直方图
plt.hist(data2, 1000)

# 3. 显示图像
plt.show()
```  
![](./Pics/均匀分布.png)  
![](./Pics/正态分布.png)  
5. 案例：随机生成8只股票2周的交易日涨幅数据  
```
stock_change = np.random.normal(loc=0, scale=1, size=(8, 10))

# 获取第一个股票的前3个交易日的涨跌幅数据
stock_change[0, :3]
```
6. 形状修改  
```
# 返回新的ndarray，原始数据没有改变
stock_change.reshape((10, 8))

# 没有返回值，对原始的ndarray进行了修改
stock_change.resize((10, 8))

# 转置
stock_change.T
```  
7. 类型修改  
```
stock_change.astype("int32")

c = np.array([1,2,3,4])
c.tostring()  # b'\x01\x00\x00\x00\x02\x00\x00\x00\x03\x00\x00\x00\x04\x00\x00\x00'
```  
8. 数组的去重
```
temp = np.array([[1,2,3,4],[3,4,5,6]])

np.unique(temp)  # array([1, 2, 3, 4, 5, 6])

temp.flatten()  # array([1, 2, 3, 4, 3, 4, 5, 6])

set(temp.flatten())  # {1, 2, 3, 4, 5, 6}
```  
9. 逻辑运算
```
stock_change = np.random.normal(loc=0, scale=1, size=(8, 10))
stock_change
'''
array([[ 1.30733538, -0.06969345,  1.00189604,  1.50736683,  0.56222421,
         0.30112432, -0.49827228, -0.61493628,  0.03010815,  0.14902767],
       [ 1.20010821,  0.83089414, -2.15981579,  0.68029085,  1.17694178,
        -0.43263882, -0.77328468, -0.96121294, -1.71803718, -1.33901869],
       [ 0.06829301,  0.61959446,  0.16560389, -1.84085443,  0.21302321,
         1.55478498,  1.12282039,  1.68685958, -0.43746782,  0.01970137],
       [ 2.21179052, -0.25768081, -0.5091288 , -1.33644633,  2.52756701,
        -1.17081225, -0.5014668 , -1.59071788,  0.94168531, -0.31124633],
       [ 1.39965904,  0.25204531, -0.2860908 , -1.26650929,  1.41920609,
        -1.47622364,  0.11571416, -0.43411683, -0.62465217, -1.47153829],
       [ 1.37692746,  0.0658293 ,  0.14858455, -0.48247065,  0.15131784,
         1.02875507, -0.74138676, -0.4928217 , -0.52533497,  1.38065942],
       [ 0.6014077 ,  0.92072824, -0.79696289, -0.44284016,  0.90564427,
        -0.14751333,  0.4064149 , -0.14025677,  1.72904881, -0.06063341],
       [-0.39016997, -1.07520642, -0.28718814, -1.43646239, -0.36953765,
         0.56104894,  0.65051168, -0.59575838, -0.51245004,  1.593463  ]])
'''

# 逻辑判断，如果大于0.5就标记为True，否则为Flase
stock_change > 0.5
'''
array([[ True, False,  True, False,  True,  True,  True, False, False,
        False],
       [False,  True,  True, False, False, False, False, False, False,
         True],
       [ True,  True,  True, False, False,  True,  True, False, False,
         True],
       [False, False,  True, False, False, False, False, False, False,
        False],
       [False, False, False, False, False, False, False, False, False,
         True],
       [ True, False,  True, False, False,  True, False, False, False,
        False],
       [False, False, False, False, False,  True, False, False, False,
        False],
       [False, False, False, False,  True, False, False,  True,  True,
        False]])
'''

stock_change[stock_change > 0.5]
'''
array([1.30733538, 1.00189604, 1.50736683, 0.56222421, 1.20010821,
       0.83089414, 0.68029085, 1.17694178, 0.61959446, 1.55478498,
       1.12282039, 1.68685958, 2.21179052, 2.52756701, 0.94168531,
       1.39965904, 1.41920609, 1.37692746, 1.02875507, 1.38065942,
       0.6014077 , 0.92072824, 0.90564427, 1.72904881, 0.56104894,
       0.65051168, 1.593463  ])
'''
```
