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
