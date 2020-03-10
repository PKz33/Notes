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
