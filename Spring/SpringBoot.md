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
- **自定义starter**  
1. 新建一个Empty Project，在里面添加两个Module：autoconfigurer和starter  
2. autoconfigurer核心代码
```
// HelloProperties.java
package com.pkz33.starter.autoconfigurer;

import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties(prefix = "pkz33.hello")
public class HelloProperties {
    private String prefix;
    private String suffix;

    public String getPrefix() {
        return prefix;
    }

    public void setPrefix(String prefix) {
        this.prefix = prefix;
    }

    public String getSuffix() {
        return suffix;
    }

    public void setSuffix(String suffix) {
        this.suffix = suffix;
    }
}


// HelloService.java
package com.pkz33.starter.autoconfigurer;

public class HelloService {

    HelloProperties helloProperties;

    public HelloProperties getHelloProperties() {
        return helloProperties;
    }

    public void setHelloProperties(HelloProperties helloProperties) {
        this.helloProperties = helloProperties;
    }

    public String sayHello(String name){
        return helloProperties.getPrefix() + "-" + name + "-" + helloProperties.getSuffix();
    }
}


// HelloServiceAutoConfiguration.java
import org.springframework.context.annotation.Configuration;

@Configuration
@ConditionalOnWebApplication
@EnableConfigurationProperties(HelloProperties.class)
public class HelloServiceAutoConfiguration {

    @Autowired
    HelloProperties helloProperties;

    @Bean
    public HelloService helloService(){
        HelloService service = new HelloService();
        service.setHelloProperties(helloProperties);
        return service;
    }
}


// pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.1</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.pkz33.starter</groupId>
    <artifactId>autoconfigurer</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>autoconfigurer</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
    </dependencies>

</project>


// resources目录下新建META-INF/spring.factories
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.pkz33.starter.autoconfigurer.HelloServiceAutoConfiguration
``` 
3. starter核心代码  
```
// pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.pkz33.starter</groupId>
    <artifactId>starter</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>com.pkz33.starter</groupId>
            <artifactId>autoconfigurer</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
    </dependencies>
</project>
```  
4. 分别执行Maven Projects-autoconfigurer-Lifecycle-install和Maven Projects-starter-Lifecycle-install，安装自定义依赖到本地仓库  
5. 新建工程stater-test进行测试，核心代码  
```
// HelloController.java
package com.pkz33.startertest.controller;

import com.pkz33.starter.autoconfigurer.HelloService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    @Autowired
    HelloService helloService;

    @GetMapping("/hello")
    public String hello(){
        return helloService.sayHello("pkz33");
    }
}


// pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.1</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.pkz33</groupId>
    <artifactId>starter-test</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>starter-test</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>com.pkz33.starter</groupId>
            <artifactId>starter</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>


// application.properties
pkz33.hello.prefix=PKZ
pkz33.hello.suffix=33
```
- **缓存**  
1. `@Cacheable`，被标注方法执行之前先检查缓存有没有数据，若没有则方法执行之后将结果缓存  
2. `@CachePut`，先调用被标注方法，然后将方法执行之后的结果缓存（更新缓存）  
3. `@CacheEvict`，缓存清除，默认在被标注方法执行之后执行  
- **MQ**  
数据流：Publisher-Exchange-Queue-Consumer  
- **Elasticsearch**  
层级：ES-索引-类型-文档  
- **异步**  
1. `@EnableAsync`，开启异步功能    
2. `@Async`，标注方法为异步  
- **定时**  
1. `@EnableScheduling`，开启定时功能  
2. `@Scheduled(cron = "* * * * * MON-SUN")`，标注方法定时执行，参数位分别对应：second，minute，hour，day of month，month，day of week  
- **Spring Cloud**  
1. 常用组件：Netflix Eureka，服务发现；Netflix Ribbon，客户端负载均衡；Netflix Hystix，断路器；Netflix Zuul，服务网关；Spring Cloud Config，分布式配置  
