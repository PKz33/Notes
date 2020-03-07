## IO
- **IO概念**
![](./Pics/IO1.png)
- **FileOutputStream**
![](./Pics/IO2.png)
- **文件复制**
![](./Pics/IO3.png)
- **字符输出流**  
字符输出流的使用步骤：
1. 创建`FileWriter`对象，构造方法中绑定要写入数据的目的地  
2. 使用`FileWriter`中的方法`write`，把数据写入到内存缓冲区中（字符转换为字节的过程）
3. 使用`FileWriter`中的方法`flush`，把内存缓冲区中的数据，刷新到文件中
4. 释放资源（会先把内存缓冲区中的数据刷新到文件中）
- **flush方法和close方法**  
区别：  
`flush`：刷新缓冲区，流对象可以继续使用  
`close`：先刷新缓冲区，然后通知系统释放资源，流对象不可以再被使用
- **注意事项**
1. JDK7的新特性：在`try`的后边可以增加一个`()`，在括号中可以定义流对象，且这个流对象的作用域就在`try`中有效。`try`中的代码执行完毕，会自动把流对象释放，不用写`finally`，格式：
```
  try (定义流对象; 定义流对象...) {
    可能会产生异常的代码
  } catch (异常类变量 变量名) {
    异常的处理逻辑
  }
```
2. JDK9的新特性：`try`的前边可以定义流对象，在`try`后边的`()`中可以直接引入流对象的名称`(变量名)`，在`try`代码执行完毕之后，流对象也可以释放掉，不用写`finally`，格式：
```
  A a = new A();
  B b = new B();
  try (a, b) {
    可能会产生异常的代码
  } catch (异常类变量 变量名) {
    异常的处理逻辑
  }
```
- **Properties**
1. `java.util.Properties集合 extends Hashtable<k, v> implements Map<k, v>`
2. `Properties`集合是一个唯一和`IO`流相结合的集合：可以使用`Properties`集合中的方法`store`，把集合中的临时数据持久化写入到硬盘中存储。可以使用`Properties`集合中的方法`load`，把硬盘中保存的文件（键值对），读取到集合中使用
3. 属性列表中每个键及其对应值都是一个字符串。`Properties`集合是一个双列集合，`key`和`value`默认都是字符串
- **BufferedInputStream**
![](./Pics/IO4.png)
1. 构造方法：    
a. `BufferedOutputStream(OutputStream out)`：创建一个新的缓冲输出流，将数据写入指定的底层输出流    
b. `BufferedOutputStream(OutputStream out, int size)`：创建一个新的缓冲输出流，将具有指定缓冲区大小的数据写入指定的底层输出流。参数`OutputStream out`：字节输出流，可以传递`FileOutputStream`，缓冲流会给`FileOutputStream`增加一个缓冲区，提高写入效率。参数`int size`：指定缓冲流内部缓冲区的大小，不指定默认
2. 使用步骤：  
a. 创建`FileOutputStream`对象，构造方法中绑定要输出的目的地    
b. 创建`BufferedOutputStream`对象，构造方法中传递`FileOutputStream`对象，提高`FileOutputStream`对象效率  
c. 使用`BufferedOutputStream`对象中的方法`write`，把数据写入到内部缓冲区中  
d. 使用`BufferedOutputStream`对象中的方法`flush`，把内部缓冲区中的数据，刷新到文件中  
e. 释放资源（会先调用`flush`方法刷新数据，上一步可以省略）
- **对象的序列化流和反序列化流**
![](./Pics/IO5.png)。
1. 序列化和反序列化的时候，会抛出`NotSerializableException`异常。类通过实现`java.io.Serializable`接口启用序列化功能。未实现此接口的类将无法使其任何状态序列化或反序列化。
2. `Serializable`接口也叫标记型接口，要进行序列化和反序列化的类必须实现`Serializable`接口，会给类添加一个标记。进行序列化和反序列化的时候，就会检测是否有这个标记。有：就可以序列化和反序列化。没有：就会抛出`NotSerializableException`异常
3. `java.io.ObjectOutputStream extends OutputStream`，`ObjectOutputStream`：对象的序列化流，把对象以流的方式写入到文件中保存
4. 构造方法：`ObjectOutputStream(OutputStream out)`，创建写入指定`OutputStream`的`ObjectOutputStream`，参数`OutputStream out`，字节输出流
5. 特有的成员方法：`void writeObject(Object obj)`，将指定的对象写入`ObjectOutputStream`
6. 使用步骤：  
a. 创建`ObjectOutputStream`对象，构造方法中传递字节输出流  
b. 使用`ObjectOutputStream`对象中的方法`writeObject`，把对象写入到文件中  
c. 释放资源
7. `java.io.ObjectInputStream extends InputStream`，`ObjectInputStream`：对象的反序列化流，把文件中保存的对象，以流的方式读取出来使用
8. 构造方法：`ObjectInputStream(InputStream in)`，创建从指定`InputStream`读取的`ObjectInputStream`，参数`InputStream in`，字节输入流
9. 特有的成员方法：`void readObject(Object obj)`，从`ObjectOutputStream`读取对象
10. 使用步骤：    
a. 创建`ObjectInputStream`对象，构造方法中传递字节输入流    
b. 使用`ObjectInputStream`对象中的方法`readObject`读取保存对象的文件    
c. 释放资源  
d. 使用读取出来的对象（打印）
- **static关键字**    
静态关键字。静态优先于非静态加载到内存中（静态优先于对象进入到内存中），被`static`修饰的成员变量不能被序列化，序列化的都是对象
```
  private static int age;
  oos.writeObject(new Person("小美女", 18));
  Object o = ois.readObject();
  // Person{name = "小美女", age = 0}
```
- **transient关键字**  
瞬态关键字。被`transient`修饰的成员变量，不能被序列化
```
  private transient int age;
  oos.writeObject(new Person("小美女", 19));
  Object o = ois.readObject();
  // Person{name = "小美女", age = 0}
```
- **InvalidClassException原理**
![](./Pics/IO6.png)
