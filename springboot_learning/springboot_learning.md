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

