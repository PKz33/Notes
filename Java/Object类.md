## Object类
- **equals方法**  
1. `Object`类`equals`方法，默认比较的是两个对象的地址值，没有意义。源码：  
```
	public boolean equals(Object obj) {
		return (this == obj);
	}
```
2. 自定义类中，覆盖重写`equals`方法：
```
	@Override
	public boolean equals(Object obj) {
		// 增加一个判断，传递的参数obj如果是this本身，
		// 直接返回true，提高程序的效率
		if (obj == this) {
			return true;
		}
		
		// 增加一个判断，参数obj如果是null，
		// 直接返回false，提高程序效率
		if (obj == null) {
			return false;
		}
		
		// 增加一个判断，防止类型转换一次ClassCastException
		if (obj instanceof Person) {
			// 使用向下转型，把obj转换为Person类型
			Person p = (Person) obj;
			// 比较两个对象的属性
			boolean b = this.name.equals(p.name) && this.age == p.age;
			return b;
		}
		// 不是Person类型直接返回false
		return false;
	}
```
3. `Objects`类的`equals`方法：对两个对象进行比较，防止空指针异常
```
	public static boolean equals(Object a, Object b) {
		return (a == b) || (a != null && a.equals(b));
	}
```
