## Matplotlib
- **画二维图标的python库**  
1. `Canvas`（画板）：位于最底层，用户一般接触不到  
2. `Figure`（画布）：建立在`Canvas`之上  
3. `Axes`（绘图区）：建立在`Figure`之上  
4. `Axis`（坐标轴）、`Legend`（图例）等辅助显示层以及图像层建立在`Axes`之上  
![](./Pics/Matplotlib.png)    
```
import matplotlib.pyplot as plt
%matplotlib inline
  
plt.figure()
plt.plot([1, 0, 9], [4, 5, 6])
plt.show()
```  
![](./Pics/示例1.png)  
- **折线图的绘制与显示**  
1.  
```
# 展现某城市一周的天气，比如从星期一到星期日的天气温度如下
# 1. 创建画布
plt.figure(figsize=(20, 8), dpi=80)

# 2. 绘制图像
plt.plot([1, 2, 3, 4, 5, 6, 7], [17, 17, 18, 15, 11, 11, 13])

# 保存图像
# plt.savefig("weather.png")

# 3. 显示图像
plt.show()
```  
![](./Pics/折线图1.png)  
2. 辅助显示层（注：这里没有处理中文乱码的问题）  
```
# 需求：画出某城市11点到12点1小时内每分钟的温度变化折线图，温度范围在15度到18度
#       再添加一个城市的温度变化，温度在1度3度
import random

# 1. 准备数据 x y
x = range(60)
y_city1 = [random.uniform(15, 18) for i in x]
y_city2 = [random.uniform(1,3) for i in x]

# 2. 创建画布
# figsize：画布大小
# dpi：dot per inch，图像的清晰度
plt.figure(figsize=(20, 8), dpi=80)

# 3. 绘制图像
plt.plot(x, y_city1, color="r", linestyle="-.", label="城市1")
plt.plot(x, y_city2, color="b", label="城市2")

# 显示图例
plt.legend()

# 修改x、y刻度
# 准备x的刻度说明
x_label = ["11点{}分".format(i) for i in x]
plt.xticks(x[::5], x_label[::5])
plt.yticks(range(0, 40, 5))

# 添加网格显示
plt.grid(True, linestyle="--", alpha=0.5)

# 添加描述信息
plt.xlabel("时间变化")
plt.ylabel("温度变化")
plt.title("城市1、城市2在11点到12点每分钟的温度变化状况")

# 4. 显示
plt.show()
```    
![](./Pics/折线图2.png)  
3. 多个坐标系显示（面向对象的画图方法）  
```
# 两个城市的温度分开显示

# 1. 准备数据 x y
x = range(60)
y_city1 = [random.uniform(15, 18) for i in x]
y_city2 = [random.uniform(1, 3) for i in x]

# 2. 创建画布
figure, axes = plt.subplots(nrows=1, ncols=2, figsize=(20, 8), dpi=80)

# 3. 绘制图像
axes[0].plot(x, y_city1, color="r", linestyle="-.", label="City1")
axes[1].plot(x, y_city2, color="b", label="City2")

# 显示图例
axes[0].legend()
axes[1].legend()

# 修改x、y刻度
# 准备x的刻度说明
x_label = ["{} past".format(i) for i in x]
axes[0].set_xticks(x[::5])
axes[0].set_xticklabels(x_label[::5])
axes[0].set_yticks(range(0, 40, 5))
axes[1].set_xticks(x[::5])
axes[1].set_xticklabels(x_label[::5])
axes[1].set_yticks(range(0, 40, 5))

# 添加网格显示
axes[0].grid(linestyle="--", alpha=0.5)
axes[1].grid(linestyle="--", alpha=0.5)

# 添加描述信息
axes[0].set_xlabel("Time")
axes[0].set_ylabel("Temperature")
axes[0].set_title("Temperature between 11 and 12")
axes[1].set_xlabel("Time")
axes[1].set_ylabel("Temperature")
axes[1].set_title("Temperature between 11 and 12")

# 显示
plt.show()
```  
![](./Pics/折线图3.png)
