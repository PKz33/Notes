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
      Producer Producer = new Producer();
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
5. Advice（通知/增强）：指拦截到Joinpoint之后所要做的事情就是通知，通知分为前置通知，后置通知，异常通知，最终通知，环绕通知；Aspect（切面）：是切入点和通知（引介）的结合    
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
6.  
```
  <dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.8.7</version>
  </dependency>
  
  // 账户的业务层接口
  public interface IAccountService {
    void saveAccount();
  }
  
  // 账户的业务层实现类
  public class AccountServiceImpl implements IAccountService {
    @Override
    public void saveAccount() {
      System.out.println("执行保存");
    }
  }
  
  // 用于记录日志的工具类，提供了公共的代码
  public class Logger {
    // 用于打印日志：计划让其在切入点方法执行之前执行（切入点方法就是业务层方法）
    public void printLog() {
      System.out.println("Logger类中的printLog方法开始记录日志");
    }
    
    // 问题：配置环绕通知之后，切入点方法没有执行，而通知方法执行了
    // 分析：通过对比动态代理中的环绕通知代码，发现动态代理的环绕通知有明确的切入点方法调用，而当前代码中没有
    // 解决：Spring框架提供了一个接口：ProceedingJoinPoint，该接口有一个方法proceed()，此方法就相当于明确调用切入点方法
    //       该接口可以作为环绕通知的方法参数，在程序执行时，spring框架会提供该接口的实现类
    // spring中的环绕通知是框架提供的一种可以在代码中手动控制增强方法何时执行的方式
    public void aroundPrintLog(ProceedingJoinPoint pjp) {
      Object rtValue = null;
      try {
        Object[] args = pjp.getArgs(); // 得到方法执行所需的参数
        System.out.println("Logger类中的aroundPrintLog方法开始记录日志---前置");
        rtValue = pjp.proceed(args); // 明确调用业务层方法（切入点方法）
        System.out.println("Logger类中的aroundPrintLog方法开始记录日志---后置");
        return rtValue;
      }catch (Throwable t) {
        System.out.println("Logger类中的aroundPrintLog方法开始记录日志---异常");
        throw new RuntimeException(t);
      }finally {
        System.out.println("Logger类中的aroundPrintLog方法开始记录日志---最终");
      }
      System.out.println("Logger类中的aroundPrintLog方法开始记录日志");
    }
  }
  
  <beans>
    <!-- 配置spring的IOC，把service对象配置进来 -->
    <bean id="accountService" class="com.pkz.service.impl.AccountServiceImpl"></bean>
    
    <!--
      spring中基于XML的AOP配置步骤：
      1. 把通知Bean也交给spring管理  
      2. 使用aop:config标签表明开始AOP的配置 
      3. 使用aop:aspect标签表明配置切面
          id属性：是给切面提供一个唯一标识
          ref属性：是指定通知类bean的id
      4. 在aop:aspect标签的内部使用对应标签配置通知的类型  
          示例让printLog方法在切入点方法执行之前执行，所以是前置通知  
          aop:before：表示配置前置通知
          method属性：用于指定Logger类中哪个方法是前置通知
          pointcut属性：用于指定切入点表达式，该表达式的含义指的是对业务层中哪些方法增强
      5. 切入点表达式的写法：
          关键字：execution(表达式)
          表达式：访问修饰符 返回值 包名.包名.包名...类名.方法名(参数列表)
          标准的表达式写法：public void com.pkz.service.impl.AccountServiceImpl.saveAccount()
          访问修饰符可以省略：void com.pkz.service.impl.AccountServiceImpl.saveAccount()
          返回值可以使用通配符，表示任意返回值：* com.pkz.service.impl.AccountServiceImpl.saveAccount()
          包名可以使用通配符，表示任意包，但是有几级包，就需要写几个*
          包名可以使用..表示当前包及其子包* *..AccountServiceImpl.saveAccount()
          类名和方法名可以使用*实现通配* *..*.*()
          参数列表：
              可以直接写数据类型：基本数据类型直接写名称，如int；引用类型写包名.类名的方式，如java.lang.String
              可以使用通配符表示任意类型，但是必须有参数
              可以使用..表示有无参数均可，有参数可以是任意类型
          全通配写法：* *..*.*(..)
          实际开发中切入点表达式的通常写法：切到业务层实现类下的所有方法 * com.pkz.service.impl.*.*(..)
    -->
    
    <!-- 配置Logger类 -->
    <bean id="logger" class="com.pkz.utils.Logger"></bean>
    
    <!-- 配置AOP -->
    <aop:config>
      <aop:aspect id="logAdvice" ref="logger">
        <!-- 配置通知的类型，并建立通知方法和切入点方法的关联 -->
        <aop:before method="printLog" pointcut="execution(public void com.pkz.service.impl.AccountServiceImpl.saveAccount())">
        </aop:before>
      </aop:aspect>
    </aop:config>
    
  </beans>
  
  // 测试AOP的配置
  public class AOPTest {
    public static void main(String[] args) {
      // 1. 获取容器
      ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
      // 2. 获取对象
      IAccountService as = (IAccountService)ac.getBean("accountService");
      // 3. 执行方法
      as.saveAccount();
    }
  }
  
  
  <aop:config>
    <!-- 配置切面 -->
    <aop:aspect id="logAdvice" ref="logger">
      <!-- 配置前置通知：在切入点方法执行之前执行 -->
      <aop:before method="beforePrintLog" pointcut="execution(* com.pkz.service.impl.*.*(..))"></aop:before>
      
      <!-- 配置后置通知：在切入点方法正常执行之后执行，它和异常通知永远只能执行一个 -->
      <aop:after-returning method="afterReturningPrintLog" pointcut="execution(* com.pkz.service.impl.*.*(..))"></aop:after-returning>
      
      <!-- 配置异常通知：在切入点方法执行产生异常之后执行，它和后置通知永远只能执行一个 -->
      <aop:after-throwing method="afterThrowingPrintLog" pointcut-ref="pt1"></aop:after-throwing>
      
      <!-- 配置最终通知：无论切入点方法是否正常执行它都会在其后面执行 -->
      <aop:after method="afterPrintLog" pointcut-ref="pt1"></aop:after>
      
      <!-- 配置环绕通知 -->
      <aop:around method="aroundPrintLog" pointcut-ref="pt1"></aop:around>
      
      <!--
        配置切入点表达式，id属性用于指定表达式的唯一标识，expression属性用于指定表达式内容
        此标签写在aop:aspect标签内部只能当前切面使用
        写在aop:aspect外面，就代表所有切面可用，若放在外面，有约束条件：写在切面之前（位置）
      -->
      <aop:pointcut id="pt1" expression="execution(* com.pkz.service.impl.*.*(..))"></aop:pointcut>
    </aop:aspect>
  </aop:config>
```
- **基于注解的AOP配置**
1.
```
  <!-- 配置spring创建容器时要扫描的包 -->
  <context:component-scan base-package="com.pkz"></context:component-scan>
  
  <!-- 配置spring开启注解AOP的支持 -->
  <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
  
  @Component("logger")
  @Aspect // 表示当前类是一个切面类
  public class Logger {
  
    @Pointcut("execution(* com.pkz.service.impl.*.*(..))")
    private void pt1(){}
  
    // 前置通知
    @Before("pt1()")
    public void beforePrintLog(){}
    
    // 后置通知
    @AfterReturning("pt1()")
    public void afterReturningPrintLog(){}
    
    // 异常通知
    @AfterThrowing("pt1()")
    public void afterThrowingPrintLog(){}
    
    // 最终通知
    @After("pt1()")
    public void afterPrintLog(){}
    
    // 环绕通知
    @Around("pt1()")
    public Object aroundPrintLog(ProceedingJoinPoint pjp){ ... }
  }
```
2. 不使用XML的配置方式
```
  @Configuration
  @ComponentScan(basePackages="com.pkz")
  @EnableAspectJAutoProxy
  public class SpringConfiguration {
  }
```
