# Spring boot ---helloworld项目

1.创建一个maven工程;(jar)

2.导入依赖spring boot相关依赖

```
<?xml version="1.0" encoding="UTF-8"?><project xmlns="http://maven.apache.org/POM/4.0.0"         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">    <modelVersion>4.0.0</modelVersion>    <groupId>com.huangzichao</groupId>    <artifactId>hello_01</artifactId>    <version>1.0-SNAPSHOT</version>    <dependencies>        <dependency>            <groupId>org.springframework.boot</groupId>            <artifactId>spring-boot-starter-web</artifactId>        </dependency>    </dependencies></project>
```

3.编写一个主程序---启动应用

6.简化部署

```java
<!--这个插件，可以将应用打包成一个可执行的jar包：-->    
<build>        
    <plugins>            
        <plugin>                
            <groupId>org.springframework.boot</groupId>               
            <artifactId>spring-boot-maven-plugin</artifactId>           
        </plugin>        
    </plugins>   
</build>
```

将这个应用打包成jar包，直接使用java -jar的命令进行执行

## 一 HelloWorld 探究

### 1 POM文件

1父项目

```java
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.7.RELEASE</version>
    </parent>

他的父项目是：
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.1.7.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>
  
 管理Spring Boot里面的所有依赖版本；
 
```

Spring Boot的版本仲裁中心；

导入依赖的时候默认是不需要写版本（若是没有在dependencies里面管理的依赖自然需要声明版本号）

### 2 依赖的导入

spring-boot-starter-we

spring-boot-starter：spring-boot场景启动器；帮我们导入了web模块正常运行所依赖的组件；

### 3 pringBootApplication

作用：说明这个类是SpringBoot的主配置类，SpringBoot运行这个类的main方法来启动SpringBoot应用；

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
```

@SpringBootConfiguration：Spring Boot的配置类；

​			标注在某个类上，表示这是一个Spring Boot的配置类；

​			@Configuration：配置类上标注这个注解，则说明该配置类是一个配置文件

@EnableAutoConfiguration：开启自动配置功能

​	以前需要我们配置的东西，Spring Boot帮我们自动配置；@EnableAutoConfiguration告诉Spring Boot开启自动配置功能，止痒自动配置才能生效；



## 二 使用Spring Initalizr创建Spring Boot项目

## 1.默认生成的Spring Boot项目：

​	主程序已经生成好了，我们只需要我们自己的逻辑

​	resource文件夹中目录结构

​	 static：保存所有的静态资源；js css images；

​      templates:保存所有的模板页面；（spring boot 默认jar包使用嵌入式的tomcat，默认不支持jsp页面）；可以使用模板引擎（freemarker，thymeleaf）

​	 application.properties:Spring Boot应用的配置文件；可以修改一些默认配置，

##    2.Spring Boot的配置文件

​	Spring Boot使用一个全局的配置文件，配置文件名是固定的；

​			application.properties

​			application.yml

​			配置文件的作用：修改SpringBoot自动配置的默认值，SpringBoot在底层都给我们自动配置好；

### 	1.yaml基本语法

####  语法格式：

​				属性:(空格)v:表示一对键值对（空格必须有）

以空格的缩进来控制层级关系，只要左对齐的一列数据，都是同一层级的

例如：

```java
server:
  port: 8081
```

#### 值的格式：

字面量：普通的值（数字，字符串，布尔）

​			字符串默认不用加上单引号或者双引号

​			“”：双引号，不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

​				例如----name: "zhangsan\nlist":输出：zhangsan 换行 list

​			'':单引号;会转义特殊字符，特殊字符最终只是一个普通的字符串数据

​				例如----name: 'zhangsan\nlis':输出：zhangsan \n list

对象，Map（属性和值）（键值对）

​			

数组(List,Set)

## **8 自动配置原理**

配置文件能配置的属性参照

1.Spring Boot启动

2.@Conditional派生注解(Spring 注解版原生的判断的属性提供的

## 9 日志框架

spring Boot选用的日志框架是：SLF4J和logback

以后开发的时候，日志记录方法的调用

1.如何在系统中使用SLF4J

```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

图示：

![1565335073294](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1565335073294.png)



每一个日志框架都有自己的配置文件，使用SLF4J以后，**配置文件还是做成日志实现框架自己本身的配置文件；**

### 1. 遗留问题

有可能每个框架底层使用的日志系统都是不一样的，因此需要将他们统一为SLF4J的框架

如何让系统中所有的日志都统一到slf4j；图示如下：

![1565337733625](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1565337733625.png)

1. **将系统中其他日志框架先排除出去**
2. **用中间包来替换原有的日志框架**
3. **我们导入slf4j其他的实现**

总结：

1）SpringBoot底层也是使用sfl4j+logback的方式进行记录

2）SpringBoot也把其他的日志都替换成了slf4j

3）中间替换包

4）如果我们要引入其他的框架，一定要把这个框架的默认依赖移除掉

​		Spring框架用的是Commons-logging;

SpringBoot能自动适配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其他框架的时候，需要把该框架(指的是其他框架)的默认依赖排除掉

### 2.日志的使用

1 ,默认配置

SpringBoot默认帮我们配置好了日志

5.切换日志框架

可以按照slf4j的日志适配图，进行相关的切换

slf4j+log4j的相关方式；

# 四 web开发

## 使用SpringBoot

1）创建SpringBoot应用，选中我们需要的模块

2）添加少量配置

3）自己编写业务代码

自动配置原理？

这个场景springBoot帮我们配置了什么，能不能修改？修改哪些配置？能不能扩展？





# 五 SpringBoot操作数据库

### 2 整合SpringData JPA	

1) 第一步：添加依赖：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.7.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.huangzichao</groupId>
    <artifactId>spring-boot-data-jpa</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-boot-data-jpa</name>
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
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
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

```

2）添加数据源，在application.yml文件

```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
    username: root
    password: huangziyi520
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
#更新或者创建数据表结构
      ddl-auto: update
#控制台显示SQL
    show-sql: true
```

3) 编写一个实体类(bean),和数据表进行映射,并且配置好映射关系

```java
package com.huangzichao.springboot.entity;

import javax.persistence.*;

@Entity//使用JPA注解配置映射关系，告诉JPA这是一个实体类(同时和数据表进行映射)
@Table(name = "tb1_user")//和名字为tb1_user进行映射；如果省略，默认表名就是类名，不过得小写user
public class User {
    
    @Id//这是一个主键
    @GeneratedValue(strategy = GenerationType.IDENTITY)//自增主键
    private Integer id;
    
    @Column(name = "last_name",length = 50)//这是和数据表对应的一个列,并且列名是last_name
    private String lastName;
    
    @Column//如果省略列名，则默认列名就是属性名
    private String email;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}

```

4）编写一个repository接口来操作对应的数据表(Repository)

```java
package com.huangzichao.springboot.repository;

import com.huangzichao.springboot.entity.User;
import org.springframework.data.jpa.repository.JpaRepository;

//继承JpaRepository来完成对数据库的操作,在<>中，第一个是实体类，第二个是主键
//在这JpaRepository接口中，已经有了相应的方法了，只要照着用就行
public interface UserRepository extends JpaRepository<User,Integer> {
}

```

5) controller层

```java
package com.huangzichao.springboot.controller;


import com.huangzichao.springboot.entity.User;
import com.huangzichao.springboot.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class userController {

    @Autowired
    UserRepository userRepository;
//查询数据
    @GetMapping("/user/{id}")
    public User getUser(@PathVariable("id") Integer id){
        User user=userRepository.getOne(id);
        System.out.println(user.toString());
        return user;
    }
//插入数据
    @GetMapping("/uer")
    public User insertUser(User user){
        userRepository.save(user);
        return user;
    }
}

```

# 六 SpringBoot启动配置原理

68集~71集

# SpringBoot高级部分内容

# 七 SpringBoot与缓存

JSR-107(一般不怎么用)

需要导入依赖：

```xml
<dependency>
      <groupId>javax.cache</groupId>
       <artifactId>cache-api</artifactId>
 </dependency>
```

 Spring缓存抽象 ()

在整合jpa的条件下，只需要在程序入口类和返回结果的方法上面添加两个注解即可

pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.7.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.huangzichao</groupId>
    <artifactId>spring-boot-data-jpa</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-boot-data-jpa</name>
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
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
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

```

入口类

```java
package com.huangzichao.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cache.annotation.EnableCaching;

@SpringBootApplication
@EnableCaching//开启基于注解的缓存
public class SpringBootDataJpaApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootDataJpaApplication.class, args);
    }

}

```

创建一个userService类，在这个类中，将方法返回值缓存起来

```java
package com.huangzichao.springboot.service;

import com.huangzichao.springboot.entity.User;
import com.huangzichao.springboot.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Autowired
    UserRepository userRepository;

    /**
     * CacheManager管理多个Catch组件，对缓存的真正CRUD操作在Catch组件中，没一个缓存组件有自己唯一的一个名字
     * 几个属性:
     *      catcheNames/values:指定组件的名字(注:可以任意的名字)
     *      key：缓存数据使用的key；可以用它来指定。默认是使用方法 参数的值 1-方法返回值 编写:SpEL;#id;参数id的值 #a0---意思是第一个参数的值 #p0 #root.args[0]
     *      keyGenerator:指定缓存管理器；或者cacheResolver指定获取解析器
     *      condition:指定符合条件的情况下才缓存；例如: condition = "#id>0"
     *      unless:否定条件-----满足这个条件，返回值不缓存 例如：unless = "#result=null"
     *      sync:是否使用异步模式
     *      
     *
     *      keyGenerator:key的生成器，可以指定自己的key的生成组件id
     *       key和/keyGenerator：二选一使用
     * @param id
     * @return
     */
    @Cacheable(cacheNames = {"myUser"},key = "#id")
    public User getUser(Integer id){
        System.out.println("查询"+id+"号员工");
        return userRepository.getOne(id);
    }
}

```

各种缓存注解以及对应的controller操作层

service层次

```java
package com.huangzichao.springboot.service;

import com.huangzichao.springboot.entity.User;
import com.huangzichao.springboot.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.annotation.CacheEvict;
import org.springframework.cache.annotation.CachePut;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Autowired
    UserRepository userRepository;

    /**
     * CacheManager管理多个Catch组件，对缓存的真正CRUD操作在Catch组件中，没一个缓存组件有自己唯一的一个名字
     * 几个属性:
     *      cacheNames/values:指定组件的名字(注:可以任意的名字)
     *      key：缓存数据使用的key；可以用它来指定。默认是使用方法 参数的值 1-方法返回值 编写:SpEL;#id;参数id的值 #a0 #p0 #root.args[0]
     *      keyGenerator:指定缓存管理器；或者cacheResolver指定获取解析器
     *      condition:指定符合条件的情况下才缓存；例如: condition = "#id>0"
     *      unless:否定条件-----满足这个条件，返回值不缓存 例如：unless = "#result=null"
     *      sync:是否使用异步模式
     *      在方法内部代码执行之前执行
     * @param id
     * @return
     */
    @Cacheable(cacheNames = {"myUser"},key = "#id")
    public User getUser(Integer id){
        System.out.println("查询"+id+"号员工");
        return userRepository.getOne(id);
    }

    /**
     * @CachePut的作用是更新和添加数据，同时把数据放到cacheNames为myUser中，与上面的@Cacheable的cacheNames相同，相当于把数据放到上面的缓存中
     * #result----表示的是该方法返回的值
     * 需要注意的一点是，下面的代码是先执行方法里面的代码然后再执行注解中的，与上面的代码相反
     * @param user
     * @return
     */
    @CachePut(cacheNames = {"myUser"},key = "#result.id")
    public User update(User user){
       return userRepository.save(user);
    }
    /**
     * @CacheEvict:缓存清除
     * 若是加上key，则是清除了key为id数值的缓存数据
     * 若是allEntries = false 则是清空所有缓存的数据
     * 默认是在方法执行之后执行，如果出现异常，缓存就不会清除
     * beforeInvocation = false,缓存的清除在方法执行之后才清除
     * beforeInvocation = true,缓存的清除是在方法执行之前清除的
     */
    @CacheEvict(cacheNames = {"myUser"},key = "#id")
    public void deleteUser(Integer id){
        userRepository.deleteById(id);
    }

    /**
     * @Caching(
     *      cacheable = {
     *         @Cacheable(value = "myUser",key = "lastName")
     *      },
     *      put = {
     *          @CachePut(value = "myUser",key = "#result.id"),
     *          @CachePut(value = "myUser",key = "#result.email")
     *      }
     * )
     * //这个注解的作用是同时使其内部的三个注解起作用，但是每次查询名字的时候还是会执行方法里面的代码
     */
     
}

```

controller层次:

```java
package com.huangzichao.springboot.controller;


import com.huangzichao.springboot.entity.User;
import com.huangzichao.springboot.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class userController {

    @Autowired
    UserService userService;

    @GetMapping("/user/{id}")
    public User getUser(@PathVariable("id") Integer id){
        User user=userService.getUser(id);
        System.out.println(user.toString());
        return user;
    }

    @GetMapping("/user")
    public User update(User user){
        return userService.update(user);
    }

    @GetMapping("/deleteUser/{id}")
    public void delete(@PathVariable("id") Integer id){
        userService.deleteUser(id);
    }
}

```

其余的和整合jpa，对数据库的操作是一样的

整合Redis









































