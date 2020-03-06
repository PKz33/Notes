## Date类
- **SimpleDateFormat**
用法：
```
  // 1.创建SimpleDateFormat类对象，构造方法中传递指定的模式
  SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH时mm分ss秒");
  // 2.调用SimpleDateFormat类对象中的方法format，按照构造方法中指定的模式，把Date日期格式转化为符合模式的字符串
  Date date = new Date();
  String d = sdf.format(date);
  System.out.println(date); // Fri Mar 06 21:39:22 CST 2020
  System.out.println(d); // 2020年03月06日 21时39分22秒
  
  private static void demo() throws ParseException {
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH时mm分ss秒");
    // 调用SimpleDateFormat类对象中的方法parse，把符合构造方法中模式的字符串解析为Date日期
    Date date = sdf.parse("2020年03月06日 21时39分22秒");
    System.out.println(date);
  }
```
