# 一、Spring Boot入门

## 1、Spring Boot 简介

SpringBoot来简化Spring应用开发，约定大于配置，去繁从简

优点：

- 快速创建独立运行的Spring项目以及与主流框架集成
- 快速使用嵌入式的Servlet容器，应用无需打成WAR包
- starters自动依赖与版本控制
- 大量的自动配置，简化开发，也可修改默认值
- 无需配置XML,无代码生成，开箱即用
- 准生产环境的运行时应用监控
- 与云计算的天然集成
- J2EE开发的一站式解决方案

## 2、微服务简介

微服务：架构风格（服务微化）

一个应用都是一组小型服务；可以通过HTTP的方式进行互通；

单体应用：ALL IN ONE

微服务：每一个功能元素最终都是一个可独立替换和独立升级的软件单元；

## 3、Spring Boot HelloWorld

### 1、创建Maven工程

### 2、导入相关依赖

```xml
<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.9.RELEASE</version>
</parent>
```

```xml
<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
</dependencies>
```

### 3、编写HelloWorld应用程序

```java
@SpringBootApplication
public class HelloWorldApplication {

    public static void main(String[] args) {
        SpringApplication.run(HelloWorldApplication.class,args);
    }
}
```

Controller层：

```java
@Controller
public class HelloWorldController {

    @ResponseBody
    @RequestMapping("/hello")
    public String helloWorld(){
        return "Hello World";
    }
}
```

#### 注意点

1. 启动配置中应将两个类路径对应
   ![](.//image//tip1.png)
2. 启动类应放在最外层，要将Application类放在最外侧，即包含所有子包 ，spring-boot会自动加载启动类所在包下及其子包下的所有组件
   ![](.//image//tip2.png)

### 4、启动成功！

![](.//image//success1.png)

## 4、简化部署

情景：假如服务器没有安装Tomcat，只要将SpringBoot项目打包成可执行的jar包，安装如下插件：

```java
<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
	</plugins>
</build>
```

## 5、打包运行

![](.//image//package.png)

执行jar包：

```shell
java -jar 01_SpringBoot_HelloWorld-1.0-SNAPSHOT.jar
```

![](.//image//success2.png)

## 6、HelloWorld场景解析

### 1、pom文件

![](.//image//parent1.png)

![](.//image//parent2.png)

### 2、导入的依赖

![](.//image//parent3.png)

### 3、启动场景

Spring Boot将所有的功能场景抽取出来，做成一个个的starters（启动器），只需要在项目中引入这些starter相关场景的所有依赖就会被导入进来。

## 7、配置文件

### 1、简介

SpringBoot使用全局的配置文件,配置文件名是固定的：

application.properties

application.yml

配置文件的作用: 修改springboot自动配置的默认值：SpringBoot在底层给我们自动配置好；

yaml：以数据为中心

```yaml
server:
  port: 8090
```

### 2、基本语法

k:(空格)v: 表示键值对

以空格的缩进来控制层级关系：只要左对齐的一列数据，都是同一层级:

```yaml
server:
	port: 8080
	path: /hello
```

属性和值都是大小写敏感

### 3、值的语法

#### 字面量：普通的值(数字、布尔、字符串)

​	k: v: 字面量直接写；

​			字符串默认不用加单引号或者双引号；

#### 对象、Map（属性和值）（键值对）：

​	k：v: 	

​			对象还是k:v的方式

```yaml
server:
	port: 8080
	path: /hello
```

行内写法：

```yaml
server: {port: 8080, path: /hello}
```

#### 数组（List、Set）

用-值来表示数组中的一个元素:

```yaml
pets:
 - cat 
 - dog
 - pig
```

### 4、配置文件进行Bean注入

#### 1、创建一个实体类,添加Getter和Setter方法以及toString方法

```java
public class Person {
    private String name;
    private String age;
    private String telephone;
    private String email;
    private Cat cat;
}
```

#### 2、添加注解以实现配置文件注入

```java
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private String age;
    private String telephone;
    private String email;
    private Cat cat;
}
```

#### 3、编写配置文件application.yml

```yaml
server:
  port: 8090
person:
  name: bossking
  age: 18
  telephone: 13078163530
  email: 724574109@qq.com
  cat:
    name: cc
    age: 10
```

#### 4、启动SpringBoot

#### 5、编写测试类，将Person类对象自动注入容器

```java
public class Springboot01QuickstartApplicationTests {

    @Autowired
    Person person;

    @Test
    public void contextLoads() {
        System.out.println(person);
    }

}
```

### 5、@Value和@ConfigurationProperties为属性注入值对比

如果application.properties中含有相同对象的不同值，那么yml文件的属性值会被覆盖

|                  | @ConfigurationProperties | @Value     |
| ---------------- | ------------------------ | ---------- |
| 功能             | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（驼峰） | 支持                     | 不支持     |
| SpEL             | 不支持                   | 支持       |
| JSR303校验       | 支持                     | 不支持     |
| 复杂类型封装     | 支持                     | 不支持     |

如果说，只是使用配置文件中的某个值，则使用@Value进行值的注入。

如果说，我们编写一个实体类来和配置文件进行映射，则使用@ConfigurationProperties

@ConfigurationProperties是默认从全局配置文件中读取值。

### 6、@PropertySource

```java
@PropertySource(value={"classpath:文件名"})
```

@PropertySource加载类路径下的配置文件，value可以传递数组，即加载多个配置文件。

### 7、@ImportResource

导入Spring的配置文件，让配置文件中的内容生效；

在主配置类中，将Spring的配置文件加载进来：

```java
 @SpringBootApplication
@ImportResource(locations = {"classpath:SpringConfiguration.xml"})
public class Springboot01QuickstartApplication {

    public static void main(String[] args) {
        Cat cat=new Cat();
        cat.setAge("10");
        cat.setName("cc");
        System.out.println(cat);
        SpringApplication.run(Springboot01QuickstartApplication.class, args);
    }

}
```

### 8、SpringBoot推荐给容器添加组件的方式:推荐使用全注解

1、编写配置类

@Configuration：指明当前类是一个配置类；来替代之前的Spring配置文件

2、使用@Bean给容器中添加组件

```java
@Configuration
public class Config {
    @Bean(name="mimi")
    public Cat Mimi(){
        System.out.println("Hello Mimi!");
        return new Cat();
    }
}
```

### 9、配置文件占位符

#### 1、随机数

#### 2、占位符获取之前配置的值，如果没有可以使用:指定默认值

```yaml
server:
  port: 8090
person:
  name: bossking
  age: ${random.int(20)}
  telephone: 13078163530
  email: 724574109@qq.com
  cat:
    name: ${person.name}_cc
    age: ${random.int(10)}

```

### 10、Profile

#### 1、多Profile文件

我们在主配置文件编写的时候，文件名可以是application-{profile}.properties/yml;

SpringBoot默认使用application.properties的配置；

![](.//image//springProfile.png)

![](.//image//springProfileActive.png)

![](.//image//activeResult.png)



#### 2、yml支持多文档块方式

```yaml
server:
  port: 8090
person:
  name: bossking
  age: ${random.int(20)}
  telephone: 13078163530
  email: 724574109@qq.com
  cat:
    name: ${person.name}_cc
    age: ${random.int(10)}
spring:
  profiles:
    active: dev
---  #区分文档块
server:
  port: 8091
spring:
  profiles: prod #指定环境名
---
server:
  port: 8092
spring:
  profiles: dev #指定环境名
```

激活dev的开发环境

#### 3、激活配置profile

1、在配置文件中指定激活的环境

```properties
spring.profiles.active=dev
```

```yaml
spring:
  profiles:
    active: dev
```

2、命令行

```program aruguments
--spring.profiles.active=prod
```

![](.//image//programArguments.png)

3、虚拟机参数

VM options：

```VM options
-Dspring.profiles.active=dev
```

### 11、配置文件的加载位置

SpringBoot启动会扫描以下位置的application.properties或者application.yml文件作为Spring Boot的默认配置文件

- file: /config/
- file:/
- classpath: /config/
- classpath:/

以上是按照优先级由高到低的顺序，所有位置的文件都会被加载，高优先的配置内容会覆盖低优先级的配置内容i

SpringBoot会从这四个位置全部加载主配置文件

我们也可以通过配置spring.config.location来改变默认配置

```shell
java -jar **.jar --spring.config.location=加载文件路径
```

### 12、外部配置加载顺序

**SpringBoot也可以从以下位置加载配置； 优先级从高到低；高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配置**

**1.命令行参数**

所有的配置都可以在命令行上进行指定

java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --server.port=8087  --server.context-path=/abc

多个配置用空格分开； --配置项=值



2.来自java:comp/env的JNDI属性

3.Java系统属性（System.getProperties()）

4.操作系统环境变量

5.RandomValuePropertySource配置的random.*属性值



==**由jar包外向jar包内进行寻找；**==

==**优先加载带profile**==

**6.jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件**

![](.//image//outside.png)

![](.//image//outside_properties.png)

**7.jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件**



==**再来加载不带profile**==

**8.jar包外部的application.properties或application.yml(不带spring.profile)配置文件**

**9.jar包内部的application.properties或application.yml(不带spring.profile)配置文件**



10.@Configuration注解类上的@PropertySource

11.通过SpringApplication.setDefaultProperties指定的默认属性

所有支持的配置加载来源；

### 13、自动配置原理

1）@EnableAutoConfiguration

![](.//image//enableconfiguration.png)

2) @Import({AutoConfigurationImportSelector.class})

![](.//image//autoImportSelector.png)

```java
protected AutoConfigurationImportSelector.AutoConfigurationEntry getAutoConfigurationEntry(AutoConfigurationMetadata autoConfigurationMetadata, AnnotationMetadata annotationMetadata) {
        if (!this.isEnabled(annotationMetadata)) {
            return EMPTY_ENTRY;
        } else {
            AnnotationAttributes attributes = this.getAttributes(annotationMetadata);
            List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
            configurations = this.removeDuplicates(configurations);
            Set<String> exclusions = this.getExclusions(annotationMetadata, attributes);
            this.checkExcludedClasses(configurations, exclusions);
            configurations.removeAll(exclusions);
            configurations = this.filter(configurations, autoConfigurationMetadata);
            this.fireAutoConfigurationImportEvents(configurations, exclusions);
            return new AutoConfigurationImportSelector.AutoConfigurationEntry(configurations, exclusions);
        }
    }
```

![](.//image//getConfigurations.png)

```java
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
        List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
        Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
        return configurations;
    }
```

![](.//image//loadFactoryName.png)

- SpringFactoriesLoader.loadFactoryNames( )
- 扫描所有jar包类路径下META-INF/spring.factories
- 把扫描到的这些文件的内容包装成properties对象
- 从properties中获取到EnableAutoConfiguration.class对应的值，然后把他们添加在容器中
- 将类数据下META-INF/spring.factories里面配置的所有EnableAutoConfiguration.class

![](.//image//spring_factories.png)

- 以**HttpEncodingAutoConfiguration(Http编码自动配置)**为例解释自动配置原理：

```java
@Configuration   //表示这是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件
@EnableConfigurationProperties(HttpEncodingProperties.class)  //启动指定类的ConfigurationProperties功能；将配置文件中对应的值和HttpEncodingProperties绑定起来；并把HttpEncodingProperties加入到ioc容器中

@ConditionalOnWebApplication //Spring底层@Conditional注解（Spring注解版），根据不同的条件，如果满足指定的条件，整个配置类里面的配置就会生效；    判断当前应用是否是web应用，如果是，当前配置类生效

@ConditionalOnClass(CharacterEncodingFilter.class)  //判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；

@ConditionalOnProperty(prefix = "spring.http.encoding", value = "enabled", matchIfMissing = true)  //判断配置文件中是否存在某个配置  spring.http.encoding.enabled；如果不存在，判断也是成立的
//即使我们配置文件中不配置spring.http.encoding.enabled=true，也是默认生效的；
public class HttpEncodingAutoConfiguration {
  
  	//他已经和SpringBoot的配置文件映射了
  	private final HttpEncodingProperties properties;
  
   //只有一个有参构造器的情况下，参数的值就会从容器中拿
  	public HttpEncodingAutoConfiguration(HttpEncodingProperties properties) {
		this.properties = properties;
	}
  
    @Bean   //给容器中添加一个组件，这个组件的某些值需要从properties中获取
	@ConditionalOnMissingBean(CharacterEncodingFilter.class) //判断容器没有这个组件？
	public CharacterEncodingFilter characterEncodingFilter() {
		CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
		filter.setEncoding(this.properties.getCharset().name());
		filter.setForceRequestEncoding(this.properties.shouldForce(Type.REQUEST));
		filter.setForceResponseEncoding(this.properties.shouldForce(Type.RESPONSE));
		return filter;
	}
```

根据当前不同条件判断，决定这个配置类是否生效？

一旦这个配置类生效；这个配置类就会给容器中添加各种组件；这些组件的属性是从对应的properties中获取的，这些列里面的每一个属性又是和配置文件绑定的。

精髓：

- springboot启动会加载大量的自动配置类
- 我们看我们需要的功能有没有springboot默认写好的自动配置类
- 只要我们要用的组件由，我们就不需要再来配置
- 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性，我们就可以在配置文件中指定这些属性的值。

# 二、日志

## 1、日志框架

**市面上的日志框架；**

JUL、JCL、Jboss-logging、logback、log4j、log4j2、slf4j....

| 日志门面  （日志的抽象层）                                   | 日志实现                                             |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| ~~JCL（Jakarta  Commons Logging）~~    SLF4j（Simple  Logging Facade for Java）    **~~jboss-logging~~** | Log4j  JUL（java.util.logging）  Log4j2  **Logback** |

左边选一个门面（抽象层）、右边来选一个实现；

日志门面：  SLF4J；

日志实现：Logback；

## 2、如何在系统中使用SLF4j

以后开发的时候，日志记录方法的调用，不应该来直接调用日志的实现类，而是调用日志抽象层的方法；

给系统导入slf4的jar和logback的实现jar

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

![](http://www.slf4j.org/images/concrete-bindings.png)

蓝色代表匹配的日志框架，深青色代表适配层，适配其他日志框架。

## 3、SpringBoot日志关系

总结：

1. SpringBoot底层也是使用slf4j+logback的方式进行日志记录
2. SpringBoot也把其他的日志都替换成了slf4j。

## 4、SpringBoot日志的默认配置

```java
@SpringBootTest
class SpringbootLoggingApplicationTests {

//  记录器
    Logger logger=LoggerFactory.getLogger(getClass());
    @Test
    void contextLoads() {
        /*日志级别由低到高
        * trace<debug<info<warn<error
        * 可以调整日志级别，日志就会在这个级别以后的高级别生效*/
        logger.trace("这是trace日志。。。。。");
        logger.debug("这是debug日志........");
        /*SpringBoot默认给我们使用的是info级别，没有指定级别的就用SpringBoot默认规定的级别：root级别*/
        logger.info("这是info日志..........");
        logger.warn("这是warn日志..........");
        logger.error("这是error日志.........");
    }

}
```

### 设置Log文件路径

```properties
logging.level.com=trace
# 生成Log日志文件
#logging.file.name=springboot.log
# 在当前目录下生成日志文件
logging.file.path=spring/log
# 在控制台输出的日志的格式
logging.pattern.console=
# 在日志文件中输出的格式
logging.pattern.file=
```

