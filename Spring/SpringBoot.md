## Spring Boot
- **启动器**  
xxx-starter-xxx  
- **基础注解**  
1. `@SpringBootApplication`，启动类  
2. `@SpringBootConfiguration`，Spring Boot的配置类
3. `@Configuration`，配置类，等效于配置文件
4. `@EnableAutoConfiguration`，开启自动配置  
5. `@ConfigurationProperties(prefix = "xxx")`，将被标注类的属性和配置文件中的属性绑定。注意与`@Value`的区别  
6. `@ImportResource`，导入自定义配置文件，使其生效。`@ImportResource(locations = {"classpath:xxx.xml"})`  
7. `@EnableConfigurationProperties(XxxProperties.class)`，开启参数类（XxxProperties.class）的`@ConfigurationProperties`功能，从而将XxxProperties类中的属性与配置文件中的属性绑定起来
- **自动配置**  
1. 启动类通过`@EnableAutoConfiguration`开启自动配置功能，将类路径下META-INF/spring.factories中所有xxx.xxx...xxx.XxxAutoConfiguration组件加入到容器中  
2. 对于每一个XxxAutoConfiguration组件，其中需要自动配置的属性的值封装在XxxProperties类中（通过`@EnableConfigurationProperties(XxxProperties.class)`关联）  
3. 对于每一个XxxProperties类，类中的属性对应的值，可以从配置文件中获取，通过`@ConfigurationProperties(prefix = "xxx.xxx...xxx")`将属性一一绑定  
4. 可以通过xxxConfigurerAdapter和xxxCustomizer进行配置的扩展和定制
- **日志**  
抽象：SLF4J，实现：Log4j、JUL、Log4j2、Logback
