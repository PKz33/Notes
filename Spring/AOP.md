## AOP
- **代理的概念**  
![](./Pics/代理.png) 
```
  // 代理对生产厂家要求的接口标准  
  public interface IProducer {
    // 销售
    public void saleProduct(float money);
    
    // 售后
    public void afterService(float money);
  }
  
  
  // 生产者
  public class Producer implements IProducer {
    // 销售
    public void saleProduct(float money) {
      System.out.println("销售产品，代理费：" + money);
    }
    
    // 售后
    public void afterService(float money) {
      System.out.println("提供售后服务，代理费：" + money);
    }
  }
  
  
  // 模拟消费者
  public class Client {
    public static void main(String[] args) {
      // 厂家直销
      Producer Producer = new ProxyProducer();
      Producer.saleProduct(1000f);
    }
  }
```  
- **动态代理**  
1. 特点：字节码随用随创建，随用随加载，不修改源码的基础上对方法增强  
2. 基于接口的动态代理  
a. 涉及类`Proxy`，由JDK提供  
b. 创建代理对象使用`Proxy`类中的`newProxyInstance`方法，要求被代理类最少实现一个接口，如果没有则不能用  
c. `newProxyInstance`方法的参数：  
`ClassLoader`：类加载器，用于加载代理对象字节码，和被代理对象使用相同的类加载器，固定写法  
`Class[]`：字节码数组，用于让代理对象和被代理对象有相同方法，固定写法  
`InvocationHandler`：用于提供增强的代码，编写如何代理。一般都是写一个该接口的实现类，通常情况下都是匿名内部类，但不是必须  
```
  final Producer producer = new Producer();

  IProducer proxyProducer = (IProducer) Proxy.newProxyInstance(producer.getClass().getClassLoader(), 
    producer.getClass().getInterfaces(), 
    new InvocationHandler() {
      /**
       * 作用：执行被代理对象的任何接口方法都会经过该方法
       * 方法参数的含义
       * @param proxy 代理对象的引用
       * @param method 当前执行的方法
       * @param args 当前执行方法所需的参数
       * @return 和被代理对象方法相同的返回值
       * @throws Throwable
       */
      @Override
      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // 提供增强的代码
        Object returnValue = null;
        // 1. 获取方法执行的参数
        Float money = (Float)args[0];
        // 2. 判断当前方法是不是销售
        if("saleProduct".equals(method.getName())) {
          returnValue = method.invoke(producer, money * 0.8f);
        }
        return returnValue;
      }
    });
    
    proxyProducer.saleProduct(10000f);
```
3. 基于子类的动态代理  
a. 涉及类`Enhancer`，由第三方`cglib`库提供  
b. 使用`Enhancer`类中的`create`方法创建代理对象，被带离类不能是最终类  
c. `create`方法的参数：  
`Class`：字节码，用于指定被代理对象的字节码  
`Callback`：用于提供增强的代码，编写如何代理。一般都是写一个该接口的实现类，通常情况下都是匿名内部类，但不是必须的。一般写该接口的子接口实现类：`MethodInterceptor`
```
  Producer cglibProducer = (Producer)Enhancer.create(producer.getClass(), new MethodInterceptor() {
    /**
     * 执行被代理对象的任何方法都会经过该方法
     * @param proxy
     * @param method
     * @param args
     * 以上三个参数和基于接口的动态代理中`invoke`方法的参数是一样的
     * @param mehodProxy：当前执行方法的代理对象 
     * @return 
     * @throws Throwable
     */
     @Override
     public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
      // 提供增强的代码
      Object returnValue = null;
      
      // 1. 获取方法执行的参数
      Float money = (Float)args[0];
      // 2. 判断当前方法是不是销售
      if("saleProduct".equals(method.getName())) {
        returnValue = method.invoke(producer, money * 0.8f);
      }
      return returnValue;
     }
  });
  
  cglibProducer.saleProduct(10000f);
```
4.  
```
  <!-- 配置文件 -->
  <!-- 配置代理的service -->
  <bean id="proxyAccountService" factory-bean="beanFactory" factory-method="getAccountService"></bean>
  
  <!-- 配置beanfactory -->
  <bean id="beanFactory" class="com.pkz.factory.BeanFactory">
    <!-- 注入service -->
    <property name="accountService" ref="accountService"></property>
    <!-- 注入事务管理器 -->
    <property name="txManager" ref="txManager"></property>
  </bean>
  
  <!-- 配置service -->
  <bean id="accountService" class="com.pkz.service.impl.AccountServiceImpl">
    <!-- 注入dao -->
    <property name="accountDao" ref="accountDao"></property>
  </bean>
  
  <!-- 配置Dao对象 -->
  <bean id="accountDao" class="com.pkz.dao.impl.AccountDaoImpl">
    <!-- 注入QueryRunner -->
    <property name="runner" ref="runner"></property>
  </bean>
  
  <!-- 配置QueryRunner -->
  <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="protorype"></bean>
  

  // 用于创建Service的代理对象的工厂
  public class BeanFactory {
    private IAccountService accountService;
    
    private TransactionManager txManager;
    
    public final void setAccountService(IAccountService accountService) {
      this.accountService = accountService;
    }
    
    public void setTxManager(TransactionManager txManager) {
      this.txManager = txManager;
    }
    
    // 获取Service代理对象
    public IAccountService getAccountService() {
      Proxy.newProxyInstance(accountService.getClass().getClassLoader(),
        accountService.getClass().getInterfaces(),
        new InvocationHandler() {
          @Override
          public Object invoke(Object proxy, Method method, Object[] args) throws Throwable{
            Object rtValue = null;
            try {
              // 1. 开启事务
              txManager.beginTransaction();
              // 2. 执行操作
              rtValue = method.invoke(accountService, args);
              // 3. 提交事务
              txManager.commit();
              // 4. 返回结果
              return rtValue;
            } catch(Exception e) {
              // 5. 回滚操作
              txManager.rollback();
              throw new RuntimeException(e);
            } finally {
              // 6. 释放连接
              txManager.release();
            }
          }
      });
    }
  }
  
  // 使用Junit单元测试，测试配置
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(locations = "classpath:bean.xml")
  public class AccountServiceTest {
    @Autowired
    @Qualifier("proxyAccountService")
    private IAccountService as;
    
    @Test
    public void testTransfer() {
      as.transfer("aaa", "bbb", 1000f);
    }
  }
```
- **面向切面编程**  
1. AOP，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术  
2. AOP作用：在程序运行期间，不修改源码对已有方法进行增强  
3. AOP优势：减少重复代码；提高开发效率；维护方便  
4. AOP实现方式：使用动态代理技术  
5.   
Advice（通知/增强）：指拦截到Joinpoint之后所要做的事情就是通知，通知分为前置通知，后置通知，异常通知，最终通知，环绕通知  
Aspect（切面）：是切入点和通知（引介）的结合  
```
  // 只是连接点，但不是切入点，因为没有被增强
  void test();
  
  // 整个invoke方法在执行就是环绕通知
  @Override
  public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    if("test".equals(method.getName())) {
      return method.invoke(accountService, args);
    }
    
    Object rtValue = null;
    try {
      // 1. 开启事务
      txManager.beginTransaction(); // 前置通知
      // 2. 执行操作
      rtValue = method.invoke(accountService, args); // 在环绕通知中有明确的切入点方法调用
      // 3. 提交事务
      txManager.commit(); // 后置通知
      // 4. 返回结果
      return rtValue;
    } catch (Exception e) {
      // 5. 回滚操作
      txManager.rollback(); // 异常通知
      throw new RuntimeException(e);
    } finally {
      // 6. 释放连接
      txManager.release(); // 最终通知
    }
  }
```
