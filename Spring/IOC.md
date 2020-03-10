## IOC
控制反转  
- **程序的耦合**
1. 耦合：程序间的依赖关系。包括：类之间的依赖；方法之间的依赖
2. 解耦：降低程序间的依赖关系。实际开发中应该做到：编译期不依赖，运行时才依赖
```
  // 1. 注册驱动
  // new对象增加耦合性
  // DriverManager.registerDriver(new com.mysql.jdbc.Driver());
  // 新问题，参数被写死
  Class.forName("com.mysql.jdbc.Driver");
  // 2. 获取连接
  Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db", "root", "123");
  // 3. 获取操作数据库的预处理对象
  PreparedStatement pstm = conn.prepareStatement("select * from account");
  // 4. 执行SQL，得到结果集
  ResultSet rs = pstm.executeQuery();
  // 5. 遍历结果集
  while(rs.next()){
    System.out.println(rs.getString("name"));
  }
  // 6. 释放资源
  rs.close();
  pstm.close();
  conn.close();
```
3. 解耦的思路：第一步，使用反射创建对象，而避免使用`new`关键字；第二步，通过读取配置文件获取要创建的对象全限定类名  
4. `Bean`：在计算机英语中，有可重用组件的含义；`JavaBean`：用`java`语言编写的可重用组件；`javabean > 实体类`
```
  // 创建Bean对象的工厂
  // 需要一个配置文件来配置需要的JavaBean，配置的内容：唯一标识=全限定类名（key=value）
  // 通过读取配置文件中配置的内容，反射创建对象
  public calss BeanFactory {
    // 定义一个Properties对象
    private static Properties props;
    
    // 定义一个Map，用于存放要创建的对象，称之为容器
    private static Map<String, Object> beans;
    
    // 使用静态代码块为Properties对象赋值
    static {
      try {
        // 实例化对象
        props = new Properties();
        // 获取properties文件的流对象
        InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
        props.load(in);
        
        // 实例化容器
        beans = new HashMap<String, Object>();
        // 取出配置文件中所有的Key
        Enumeration keys = props.keys();
        // 遍历枚举
        while(keys.hasMoreElements()) {
          // 取出每个Key
          String key = keys.nextElement().toString();
          // 根据key获取value
          String beanPath = props.getProperty(key);
          // 反射创建对象
          Object value = Class.forName(beanPath).newInstance();
          // 把key和value存入容器中
          beans.put(key, value);
        }
      } catch(Exception e) {
        throw new ExceptionInInitializerError("初始化properties失败");
      }
    }
    

    // 根据Bean的名称获取bean对象
    public static Object getBean(String beanName) {
      return beans.get(beanName);
    } 
    /*
    public static Object getBean(String beanName) {
      Object bean = null;
      try {
        String beanPath = props.getProperty(beanName);
        // 注：每次都会创建一个对象，并非单例
        bean = Class.forName(beanPath).newInstance();
      } catch (Exception e) {
        e.printStackTrace();
      }
      return bean;
    }
    */
    
  }
```
5.  
![](./Pics/IOC1.png)
