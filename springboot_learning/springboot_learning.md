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

