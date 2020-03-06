## System类
- **arraycopy方法**  
`public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)`：将数组中指定的数据拷贝到另一个数组中  
参数：  
`src`：源数组  
`srcPos`：源数组中的起始位置    
`dest`：目标数组  
`destPos`：目标数组中的起始索引  
`length`：要复制的数组元素的数量
```
  // 定义源数组
  int[] src = {1,2,3,4,5};
  // 定义目标数组
  int[] dest = {6,7,8,9,10};
  System.out.println("复制前：" + Arrays.toString(dest));
  // 使用System类中的arraycopy方法把源数组的前3个元素复制到目标数组的前3个位置上
  System.arraycopy(src, 0, dest, 0, 3);
  System.out.println("复制后：" + Arrays.toString(dest));
```
