# Maven多模块管理

## 1.场景描述

- 多模块在大型项目中是经常出现的。Maven多模块管理的学习是很有必要的！
- Maven管理多模块应用的实现是互联网项目中多使用分布式开发，那么每个独立的服务都会使用独立的项目进行维护，那么这样就需要很使用多模块应用管理，来完成项目的高度统一。
- Maven管理多模块需要父工程去进行统一的管理。

## 2.创建父工程

maven父工程必须遵循以下两点要求：

1. packaging标签的文本内容必须设置为pom.注：packaging标签是指定打包的方式，默认为jar，如果pom文件中没有packaging标签那么默认就是jar。
2. 把src删除掉

pom是项目对象模型(Project Object Module)，该文件是可以被子工程继承，maven多模块管理，其实就是让它的子模块的pom文件来继承父工程的pom。

## 3. 创建子模块

![](.//image//maven_parent.png)

```xml
<parent>
		<artifactId>001_maven_parent</artifactId>
		<groupId>com.maven_learning</groupId>
		<version>v1.0</version>
		<relativePath>../001_maven_parent/pom.xml</relativePath>
	</parent>
```

## 4.子模块会自动继承父工程所生成的依赖

```xml
<!--父工程添加依赖-->
	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.13</version>
		</dependency>
	</dependencies>
```

<img src=".//image//maven_dependency.png" style="zoom:67%;" />

## 5. maven-webapp项目创建过慢

- 添加archetypeCatalog-internal

## 6. 父工程管理依赖

- 父工程添加的依赖，所有的子模块会无条件的继承所造成的依赖冗余。

- 父工程需要加强管理依赖，标签：<dependencyManagement>

- 子工程需要的jar包，只需要声明依赖：

- ```xml
  <dependency>
  			<groupId>junit</groupId>
  			<artifactId>junit</artifactId>
  </dependency>
  ```

- 不需要声明版本号

## 7. 父工程管理依赖的版本号

- 在pom中<properties>元素下自定义的Maven属性
- 

