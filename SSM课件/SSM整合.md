# 1. 编写Spring框架

## 1.0 引入pom

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <spring.version>5.0.2.RELEASE</spring.version>
    <slf4j.version>1.6.6</slf4j.version>
    <log4j.version>1.2.12</log4j.version>
    <mysql.version>8.0.16</mysql.version>
    <mybatis.version>3.4.5</mybatis.version>
  </properties>

  <dependencies>
    <!-- spring -->
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.6.8</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>


    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>compile</scope>
    </dependency>

    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>${mysql.version}</version>
    </dependency>

    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
      <scope>provided</scope>
    </dependency>

    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.0</version>
      <scope>provided</scope>
    </dependency>

    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>

    <!-- log start -->
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>${log4j.version}</version>
    </dependency>

    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>${slf4j.version}</version>
    </dependency>

    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>${slf4j.version}</version>
    </dependency>
    <!-- log end -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>${mybatis.version}</version>
    </dependency>

    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.0</version>
    </dependency>

    <dependency>
      <groupId>c3p0</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.1.2</version>
      <type>jar</type>
      <scope>compile</scope>
    </dependency>
  </dependencies>
```



## 1.1.创建工程结构

![](.//image//design.png)

## 1.2.编写实体类

```java
public class Account implements Serializable {
    private Integer id;
    private String name;
    private Double money;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Double getMoney() {
        return money;
    }

    public void setMoney(Double money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", money=" + money +
                '}';
    }
}
```

## 1.3.编写Dao层和Service层接口

```java
public interface IAccountDao {

    List<Account> findAll();

}
```

```java
public interface IAccountService {

    List<Account> findAll();
}
```

## 1.4.编写Service层实现类

```java
@Service("accountService")
public class AccountServiceImpl implements IAccountService {


    public List<Account> findAll() {
        System.out.println("调用了业务层接口--------");
        return null;
    }
}
```

## 1.5.编写Controller层接口

```java
@Controller
public class AccountController {

    @RequestMapping("/findAll")
    public String forward(){
        System.out.println("执行了--------spring_mvc");
        return "success";
    }
}
```

## 1.6.编写spring配置文件

#### 1.6.1.注册扫描容器

```xml
<context:component-scan base-package="com.bk">
		<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
	</context:component-scan>
```

#### 1.6.2.整体约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xmlns:aop="http://www.springframework.org/schema/aop"
	   xmlns:tx="http://www.springframework.org/schema/tx"
	   xmlns:context="http://www.springframework.org/schema/context"
	   xsi:schemaLocation="http://www.springframework.org/schema/beans
	   http://www.springframework.org/schema/beans/spring-beans.xsd
	   http://www.springframework.org/schema/tx
	   http://www.springframework.org/schema/tx/spring-tx.xsd
	   http://www.springframework.org/schema/aop
	   http://www.springframework.org/schema/aop/spring-aop.xsd
	   http://www.springframework.org/schema/context
	   http://www.springframework.org/schema/context/spring-context.xsd">
	<context:component-scan base-package="com.bk">
		<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
	</context:component-scan>
	
</beans>
```

## 1.7 编写测试类

```java
public class test {

    @Test
    public void run1(){
        /*1.加载配置文件
        * 2.创建对象
        * 3.调用方法*/
        ApplicationContext ac=new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
        IAccountService accountService=ac.getBean("accountService",IAccountService.class);
        accountService.findAll();
    }
}
```

## 1.8 小结

第一阶段编写Spring框架完成

# 2. 编写SpringMVC框架

## 2.1 编写springmvc.xml

#### 2.1.1 注册扫描容器

```xml
<context:component-scan base-package="com.bk">
		<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
	</context:component-scan>
```

#### 2.1.2 开启视图解析器

```xml
<!--开启视图解析器-->
	<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/pages/"/>
		<property name="suffix" value=".jsp"/>
	</bean>
```

#### 2.1.3 开启注解支持

```xml
<!--开启注解支持-->
<mvc:annotation-driven/>
```

#### 2.1.4 整体约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	   xmlns:mvc="http://www.springframework.org/schema/mvc"
	   xmlns:context="http://www.springframework.org/schema/context"
	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xsi:schemaLocation="
http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">
</beans>
```

## 2.2 编写web.xml

#### 2.2.1 配置前端控制器

```xml
<!--配置前端控制器-->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--设置初始参数-->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
```

#### 2.2.2 配置中文乱码解析器

```xml
<!--配置中文乱码解析器-->
  <filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```

#### 2.2.3 编写controller函数

```java
@Controller
public class AccountController {

    @RequestMapping("/findAll")
    public String forward(){
        System.out.println("执行了--------spring_mvc");
        return "success";
    }
}
```

#### 2.2.4 测试结果

![](.//image//success.png)

# 3. Spring整合SpringMVC框架

## 3.1 注意点

spring整合springmvc，启动tomcat服务器的时候，需要加载spring的配置文件，此时就需要配置监听类去加载spring的配置文件，创建web工厂，存储ServletContext对象。

## 3.2 配置监听类

```xml
<!--配置Spring的监听器-->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <!--配置resource下的application.xml,因为上述监听类默认监听WEB-INF下-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>
```

## 3.3 测试

```java
@Controller
public class AccountController {

    @Autowired
    private IAccountService accountService;

    @RequestMapping("/findAll")
    public String forward(){
        System.out.println("执行了--------spring_mvc");
        accountService.findAll();
        return "success";
    }
}
```

![](.//image//result1.png)

成功整合了spring!

# 4. 编写Mybatis框架

## 4.1. 注解开发Dao层

```java
@Repository("accountDao")
public interface IAccountDao {

    @Select("SELECT * FROM account")
    List<Account> findAll();

}
```

## 4.2. 配置数据源DataSource

#### 4.2.1 创建SqlMapConfig.xml

引入Mybatis约束：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
		PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
		"http://mybatis.org/dtd/mybatis-3-config.dtd">
```

配置本地环境:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
		PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
		"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 配置连接数据库的信息 -->
	<properties resource="jdbcConfig.properties"/>
	<!-- 配置mybatis的环境 -->
	<environments default="mysql">
		<environment id="mysql">
			<!-- 配置事务管理 -->
			<transactionManager type="JDBC"/>
			<!--连接池-->
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driver}"/>
				<property name="url" value="${jdbc.url}"/>
				<property name="username" value="${jdbc.username}"/>
				<property name="password" value="${jdbc.password}"/>
			</dataSource>
		</environment>
	</environments>
    <!--引入映射配置文件-->
	<mappers>
		<package name="com.bk.dao"/>
	</mappers>
</configuration>
```

jdbcConfig.properties

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql:///eesy?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC
jdbc.username=root
jdbc.password=password
```

#### 4.2.2 引入映射配置文件开启注解扫描

```xml
 <!--引入映射配置文件-->
	<mappers>
		<package name="com.bk.dao"/>
	</mappers>
```

## 4.3 编写测试类

```java
@Test
    public void run2() throws Exception{
        /*1.读取输入流，获取数据源
        * 2.创建SqlSessionFactory工厂
        * 3.获得SqlSession对象
        * 4.拿到spring容器的dao接口
        * 5.调用实现方法*/
        InputStream in= Resources.getResourceAsStream("SqlMapConfig.xml");
        SqlSessionFactory factory=new SqlSessionFactoryBuilder().build(in);
        SqlSession sqlSession=factory.openSession();
        IAccountDao accountDao=sqlSession.getMapper(IAccountDao.class);
        List<Account> accounts=accountDao.findAll();
        for (Account account : accounts){
            System.out.println(account);
        }
    }
```

如果出错表示jdbcConfig.properties文件找不到，可以将该文件放入target下的classes文件夹下。

# 5. Spring整合Mybatis框架

## 5.1 配置applicationContext.xml

```xml
<!--1.配置数据源-->

	<context:property-placeholder location="jdbcConfig.properties"/>

	<bean id="dataSources" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="driverClass" value="${jdbc.driver}"/>
		<property name="jdbcUrl" value="${jdbc.url}"/>
		<property name="user" value="${jdbc.username}"/>
		<property name="password" value="${jdbc.password}"/>
	</bean>
	<!--2.配置sqlSessionFactory-->

	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSources"/>
	</bean>
	<!--3.映射扫描-->

	<bean id="mapperScannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="com.bk.dao"/>
	</bean>
```

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql:///eesy?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC
jdbc.username=root
jdbc.password=password
```

## 5.2 将Mapper接口交给spring容器

```java
@Repository("accountDao")
public interface IAccountDao {

    @Select("SELECT * FROM account")
    List<Account> findAll();

}
```

## 5.3 将容器中的Mapper接口注入，并实现方法

```java
@Service("accountService")
public class AccountServiceImpl implements IAccountService {

    @Autowired
    private IAccountDao accountDao;

    public List<Account> findAll() {
        System.out.println("调用了业务层接口--------");
        return accountDao.findAll();
    }
}
```

## 5.4 最终测试

```java
@Controller
public class AccountController {

    @Autowired
    private IAccountService accountService;

    @RequestMapping("/findAll")
    public String forward(){
        System.out.println("执行了--------spring_mvc");
        List<Account> accounts=accountService.findAll();
        for (Account account : accounts){
            System.out.println(account);
        }
        return "success";
    }
}
```

![](.//image//success1.png)

## 5.5 注意点

如果出现jdbcConfig.properties找不到的异常，自行将该文件放入如下图路径下：

<img src=".//image//jdbcConfig.png" style="zoom:80%;" />



# 6.总结

至此SSM框架整合完成！我对此花了一整天去重新温习了SSM的相关整合，也碰到了不少Bug，比如测试类的时候发现找不到applicationContext.xml，结果是因为我把pom.xml中的打包方法改为pom，恢复回war包方式打包就可以啦！明天继续努力，要加油呀！

<img src="https://pic1.zhimg.com/v2-0fa7c80355f498a636545b4137ccb0a4_r.jpg" style="zoom: 33%;" />

