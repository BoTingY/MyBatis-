# MyBatis

## 一、MyBatis概述

> Java常用框架：
>
> - SSM三大框架：Spring + SpringMVC + MyBatis
> - SpringBoot
> - SpringCloud
>
> 框架其实就是对通用代码的封装，提前写好了一堆接口和类，我们可以在做项目的时候直接引入这些接口和类（引入框架），基于这些现有的接口和类进行开发，可以大大提高开发效率。
>
> 框架一般都以jar包的形式存在。(jar包中有class文件以及各种配置文件等)
>
> SSM三大框架的学习顺序：MyBatis、Spring、SpringMVC（仅仅是建议）

**三层架构**

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251215164942954.png" alt="image-20251215164942954" style="zoom:67%;" />

表现层（UI）：直接跟前端打交互（一是接收前端ajax请求，二是返回json数据给前端）

业务逻辑层（BLL）：一是处理表现层转发过来的前端请求（也就是具体业务），二是将从持久层获取的数据返回到表现层。

数据访问层（DAL）：直接操作数据库完成CRUD，并将获得的数据返回到上一层（也就是业务逻辑层）。

> Java持久层框架：MyBatis、Hibernate(实现了JPA规范)、jOOQ、Guzz、Spring Data(实现了JPA规范)、ActiveJDBC...



### 1.1 JDBC不足

> SQL语句都是写死在java程序中，改SQL语句需要修改java代码，违背开闭原则OCP。
>
> 给？传值也是繁琐的。
>
> 将结果集封装成Java对象也是繁琐的。



### 1.2 MyBatis了解

> MyBatis本质上就是对JDBC的封装，通过MyBatis完成CRUD。
>
> MyBatis在三层架构中负责持久层的，属于持久层框架。

![image-20251218085716604](C:\Tian_File\Typora_images\typora-user-images\image-20251218085716604.png)

ORM：对象关系映射

- O（Object）：Java虚拟机中的Java对象。
- R（Relational）：关系型数据库
- M（Mapping）：将Java虚拟机中的Java对象映射到数据库表中一行记录，或是将数据库表中一行记录映射成Java虚拟机中的一个Java对象。

ORM图示：

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251215190958616.png" alt="image-20251215190958616" style="zoom:67%;" />

**MyBatis属于半自动化ORM框架，Hibernate属于全自动化ORM框架。**

MyBatis框架特点：

- 支持定制化 SQL、存储过程、基本映射以及高级映射
- 避免了几乎所有的 JDBC 代码中手动设置参数以及获取结果集
- 支持XML开发，也支持注解式开发。【为了保证sql语句的灵活，所以**mybatis大部分是采用XML方式开发**。】
- 将接口和 Java 的 POJOs(Plain Ordinary Java Object，简单普通的Java对象)映射成数据库中的记录
- 体积小好学：两个jar包，两个XML配置文件。
- 完全做到sql解耦合。
- 提供了基本映射标签。
- 提供了高级映射标签。
- 提供了XML标签，支持动态SQL的编写。
- ......



## 二、MyBatis入门程序

### 2.1 版本控制

软件版本：

- IntelliJ IDEA：2023.3.1
- Navicat Premium 17
- MySQL数据库：9.2.0

组件版本：

- MySQL驱动：9.2.0
- MyBatis：3.5.10
- JDK：JDK21
- Junit：5.9.3
- Logback：1.5.18

### 2.2 MyBatis下载

> 从github上下载，地址：[https://github.com/mybatis/mybatis-3](https://github.com/mybatis/mybatis-3)

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216091218379.png" alt="image-20251216091218379" style="zoom:67%;" />

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216091235839.png" alt="image-20251216091235839" style="zoom: 67%;" />

> 将框架以及框架的源码都下载下来，下载框架后解压，打开mybatis目录

![image-20251216091307154](C:\Tian_File\Typora_images\typora-user-images\image-20251216091307154.png)

> 通过以上解压可以看到，框架一般都是以jar包的形式存在。我们的mybatis课程使用maven，所以这个jar我们不需要，官方手册需要。



### 2.3 MyBatis入门程序开发步骤

> **准备数据库**

- 汽车表t_car，字段包括：
  - id：主键（自增）【bigint】
  - car_num：汽车编号【varchar】
  - brand：品牌【varchar】
  - guide_price：厂家指导价【decimal类型，专门为财务数据准备的类型】
  - produce_time：生产时间【char，年月日即可，10个长度，'2022-10-11'】
  - car_type：汽车类型（燃油车、电车、氢能源）【varchar】
- 使用navicat for mysql工具建表
  - ![image-20251216091642882](C:\Tian_File\Typora_images\typora-user-images\image-20251216091642882.png)

- 使用navicat for mysql工具向t_car表中插入两条数据，如下：
  - ![image-20251216091723661](C:\Tian_File\Typora_images\typora-user-images\image-20251216091723661.png)

> **创建Project**

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216091806439.png" alt="image-20251216091806439" style="zoom:67%;" />

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216092037083.png" alt="image-20251216092037083" style="zoom:67%;" />

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216092106282.png" alt="image-20251216092106282" style="zoom:67%;" />

![image-20251216093145780](C:\Tian_File\Typora_images\typora-user-images\image-20251216093145780.png)

#### 步骤1：打包方式`jar`（不需要war，因为mybatis封装的是jdbc）

```xml
<groupId>com.powernode</groupId>
<artifactId>mybatis-001-introduction</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>jar</packaging>
```

![image-20251216092233496](C:\Tian_File\Typora_images\typora-user-images\image-20251216092233496.png)



#### 步骤2：引入依赖（mybatis依赖 + mysql驱动依赖）

> **Maven仓库**：[[Maven Repository: Search/Browse/Explore](https://mvnrepository.com/)](https://mvnrepository.com/)
>
> 在MVN仓库中可以搜索需要的依赖版本。

```xml
<!--mybatis依赖-->
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.10</version>
</dependency>
<!--mysql驱动-->
<!-- https://mvnrepository.com/artifact/com.mysql/mysql-connector-j -->
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <version>9.2.0</version>
</dependency>
```



#### 步骤3：在resources根目录下新建mybatis-config.xml配置文件（可以参考mybatis手册拷贝）

> 打开MyBatis中文文档网页，点击入门，往下面翻找配置文件信息，复制即可。
>
> **MyBatis中文文档**：[MyBatis 中文网 官网](https://mybatis.p2hp.com/)
>
> **一个数据库对应一个mybatis-config.xml配置文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/powernode"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!--sql映射文件创建好之后，需要将该文件路径配置到这里-->
        <mapper resource=""/>
    </mappers>
</configuration>
```

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216094326853.png" alt="image-20251216094326853" style="zoom:67%;" />

- 注意1：mybatis核心配置文件的文件名 不一定 是`mybatis-config.xml`，可以是其它名字。

- 注意2：mybatis核心配置文件存放的位置也可以随意。**建议选择放在resources根下，相当于放到了类的根路径下**。
- **resources目录：放在这个目录当中的一般都是资源文件、配置文件。**直接放到resources目录下的资源等同于放到了类的根路径下。



#### 步骤4：在resources根目录下新建CarMapper.xml配置文件（可以参考mybatis手册拷贝）

> **xxxMapper.xml，这个文件是专门用来编写SQL语句的配置文件**。
>
> **一张数据库表对应一个XXXMapper.xml配置文件**
>
> 在这个配置文件当中编写SQL语句，文件名不是固定的，存放位置也不是固定的，暂时放到类的根目录下。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace先随意写一个-->
<mapper namespace="car">
    <!--insert sql：保存一个汽车信息-->
    <insert id="insertCar">
        insert into t_car
            (id,car_num,brand,guide_price,produce_time,car_type) 
        values
            (null,'102','丰田mirai',40.30,'2014-10-05','氢能源')
    </insert>
</mapper>
```

- 注意1：**sql语句可以不用分号结尾**

- 注意2：CarMapper.xml文件的名字不是固定的。可以使用其它名字。

- 注意3：CarMapper.xml文件的位置也是随意的。这里选择放在resources根下，相当于放到了类的根路径下。

- 注意4：将CarMapper.xml文件路径配置到mybatis-config.xml：

  - ```xml
    <!--<mapper resource="CarMapper.xml"/> resource属性：这种方式是从类路径当中加载资源。-->
    <!--不推荐：<mapper url="file:///d:/CarMapper.xml"/> url属性：这种方式是从绝对路径当中加载资源。-->
    
    <mapper resource="CarMapper.xml"/>
    ```

  - <img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216093712587.png" alt="image-20251216093712587" style="zoom:67%;" />



#### 步骤5：编写MyBatisTest01代码(使用mybatis的类库，编写mybatis程序，连接数据库，做增删改查)

> 在MyBatis中，负责执行SQL语句的对象：SqlSession。
>
> SqlSession是专门用来执行SQL语句的，是一个Java程序和数据库之间的一次会话。（之前学过的HttpSession是浏览器和服务器之间的一次会话。）
>
> 要想获取SqlSession对象，需要先获取SqlSessionFactory对象，通过SqlSessionFactory工厂来生产SqlSession对象。
>
> 获取SqlSessionFactory对象需要通过SqlSessionFactoryBuilder对象的build方法，来获取一个SqlSessionFactory对象。
>
> 所以需要：SqlSessionFactoryBuilder --> SqlSessionFactory --> SqlSession
>
> mybatis的核心对象包括：
>         **SqlSessionFactoryBuilder**
>         **SqlSessionFactory**
>         **SqlSession**
>
> 
>
> 官方文档说：**从XML 中构建 SqlSessionFactory**，可以知道：在MyBatis中SqlSessionFactory对象是一个很重要的对象，并且SqlSessionFactory对象的创建需要XML。
>
> mybatis中有两个主要的配置文件：
>     其中一个是：mybatis-config.xml，这是核心配置文件，主要配置连接数据库的信息等。（一个）
>     另一个是：XxxxMapper.xml，这个文件是专门用来编写SQL语句的配置文件。（一个表一个）
>         t_user表，一般会对应一个UserMapper.xml
>         t_student表，一般会对应一个StudentMapper.xml

```java
package com.powernode.mybatis;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;

public class MyBatisIntroductionTest {
    public static void main(String[] args) {
        // 1. 创建SqlSessionFactoryBuilder对象
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        
        // 2. 创建SqlSessionFactory对象（需要一个输入流）
        // 一般这么写：InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
        // 通过源码分析：底层其实就是类加载器调用了getResourceAsStream方法，即:ClassLoader.getSystemClassLoader().getResourceAsStream()
        /**
         * 通过源代码分析发现：
         * 	InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
         * 底层的源代码其实就是：
         *	InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("mybatis-config.xml");
         */
        InputStream is = Thread.currentThread().getContextClassLoader().getResourceAsStream("mybatis-config.xml");
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
        // 3. 创建SqlSession对象
        // 如果使用的事务管理器是JDBC的话，底层实际上会执行：conn.setAutoCommit(false);
        SqlSession sqlSession = sqlSessionFactory.openSession(); 
        // 4. 执行sql
        int count = sqlSession.insert("insertCar"); // 这个"insertCar"必须是sql的id
        System.out.println("插入几条数据：" + count);
        // 5. 手动提交（mybatis默认采用的事务管理器是JDBC，默认是不提交的，需要手动提交。）
        sqlSession.commit(); // 如果使用的事务管理器是JDBC的话，底层实际上执行的是conn.commit();
        // 6. 关闭资源（只关闭是不会提交的）
        sqlSession.close();
    }
}
```

- 注意1：默认采用的事务管理器是：JDBC；JDBC事务默认是不提交的，**需要手动提交**。
- `Resources.getResourceAsStream`
  - 以后凡是遇到resource这个单词，大部分情况下，这种加载资源的方式就是从类的根路径下开始查找加载。
  - 采用这种方式，从类路径当中加载资源，项目的移植性很强。项目从windows移植到linux，代码不需要修改，因为这个资源文件一直都在类路径当中。
  - InputStream is = new FileInputStream("d:\\mybatis-config.xml");采用这种方式也可以。缺点：可移植性太差，程序不够健壮。可能会移植到其他的操作系统当中。导致以上路径无效，还需要修改java代码中的路径。这样违背了OCP原则。



#### 步骤6：运行程序，查看运行结果，以及数据库表中的数据。

![image-20251216095121436](C:\Tian_File\Typora_images\typora-user-images\image-20251216095121436.png)



### 2.4 关于MyBatis核心配置文件的名字和路径解析

> 核心配置文件的名字是随意的，只需要将代码中的配置文件名修改即可。

```java
// 文件名是出现在程序中的，文件名如果修改了，对应这里的java程序也改一下就行了。
InputStream is = Thread.currentThread().getContextClassLoader().getResourceAsStream("mybatis-config.xml");
```

> mybatis核心配置文件的名字不是固定的，但通常该文件的名字叫做：mybatis-config.xml
>
> mybatis核心配置文件的路径不是固定的，但通常该文件会存放到**类路径**当中，这样让项目的移植更加健壮。
>
> 在mybatis中提供了一个类：Resources【org.apache.ibatis.io.Resources】，该类可以从类路径当中获取资源，通常使用它来获取输入流InputStream。

```java
// 这种方式只能从类路径当中获取资源，也就是说mybatis-config.xml文件需要在类路径下。
InputStream is = Resources.getResourceAsStream("mybatis-config.xml");

// 通过源码分析：底层其实就是类加载器调用了getResourceAsStream方法，即:ClassLoader.getSystemClassLoader().getResourceAsStream()
/**
 * 通过源代码分析发现：
 * 	InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
 * 底层的源代码其实就是：
 *	InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("mybatis-config.xml");
 */
```



### 2.5 MyBatis的一个完整代码写法

```java
package com.powernode.mybatis;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;

public class MyBatisCompleteCodeTest {
    public static void main(String[] args) {
        SqlSession sqlSession = null;
        try {
            // 1.创建SqlSessionFactoryBuilder对象
            SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
            // 2.创建SqlSessionFactory对象
            SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config.xml"));
            // 3.创建SqlSession对象
            sqlSession = sqlSessionFactory.openSession();
            // 4.执行SQL
            int count = sqlSession.insert("insertCar");
            System.out.println("更新了几条记录：" + count);
            // 5.提交
            sqlSession.commit();
        } catch (Exception e) {
            // 回滚
            if (sqlSession != null) {
                sqlSession.rollback();
            }
            e.printStackTrace();
        } finally {
            // 6.关闭
            if (sqlSession != null) {
                sqlSession.close();
            }
        }
    }
}
```



### 2.6 引入Junit

> **JUnit是专门做单元测试的组件**，在实际开发中，单元测试一般是由我们Java程序员来完成的。测试过程中有**期望值**和**实际值**两个概念，期望值和实际值相同表示测试通过，期望值和实际值不同则单元测试执行时会报错。
>
> 引入JUnit是为了代替main方法。

#### 第一步：引入依赖

- ```xml
  <!--junit依赖-->
          <!-- API（编写测试用例） -->
          <dependency>
              <groupId>org.junit.jupiter</groupId>
              <artifactId>junit-jupiter-api</artifactId>
              <version>5.9.3</version>
              <scope>test</scope>
          </dependency>
          <!-- 引擎（执行测试用例） -->
          <dependency>
              <groupId>org.junit.jupiter</groupId>
              <artifactId>junit-jupiter-engine</artifactId>
              <version>5.9.3</version>
              <scope>test</scope>
          </dependency>
          <dependency>
              <groupId>org.junit.vintage</groupId>
              <artifactId>junit-vintage-engine</artifactId>
              <version>5.9.3</version>
              <scope>test</scope>
          </dependency>
  ```



#### 第二步：编写单元测试类【测试用例】，测试用例中每一个测试方法上使用`@Test注解`进行标注。

- 测试用例的名字以及每个测试方法的定义都是有规范的：

  - 测试用例的名字：要测试的类名 + Test
  - 测试方法声明格式：`public void test方法名(){}`

- ```java
  // 测试用例
  public class CarMapperTest{
      
      // 测试方法 一般是一个业务方法对应一个测试方式
      @Test
      public void testInsert(){
          
      }
      
      @Test
      public void testUpdate(){
          
      }
      
  }
  ```



#### 第三步：可以在类上执行，也可以在方法上执行。

- 在类上执行时，该类中所有的测试方法都会执行。

- 在方法上执行时，只执行当前的测试方法。

- 编写一个测试用例，来测试insertCar业务

  - ```java
    package com.powernode.mybatis;
    
    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.ibatis.session.SqlSessionFactoryBuilder;
    import org.junit.Test;
    
    public class CarMapperTest {
        
        @Test
        public void testInsertCar(){
            SqlSession sqlSession = null;
            try {
                // 1.创建SqlSessionFactoryBuilder对象
                SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
                // 2.创建SqlSessionFactory对象
                SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config.xml"));
                // 3.创建SqlSession对象
                sqlSession = sqlSessionFactory.openSession();
                // 4.执行SQL
                int count = sqlSession.insert("insertCar");
                System.out.println("更新了几条记录：" + count);
                // 5.提交
                sqlSession.commit();
            } catch (Exception e) {
                // 回滚
                if (sqlSession != null) {
                    sqlSession.rollback();
                }
                e.printStackTrace();
            } finally {
                // 6.关闭
                if (sqlSession != null) {
                    sqlSession.close();
                }
            }
        }
    }
    ```



### 2.7 引入日志框架Logback

> mybatis常见的集成的日志组件有：
>
> ​	SLF4J（沙拉风）：沙拉风是一个日志标准，其中有一个框架叫做logback，它实现了沙拉风规范。
>
> ​	LOG4J
>
> ​	LOG4J2
>
> ​	STDOUT_LOGGING
>
> ​	...
>
> 注意：log4j log4j2 logback都是同一个作者开发的。
>
> 其中STDOUT_LOGGING是标准日志，mybatis已经实现了这种标准日志。mybatis框架本身已经实现了这种标准。
>
> 
>
> 引入日志框架的目的是为了看清楚mybatis执行的具体sql，启用标准日志组件，只需要在`mybatis-config.xml`文件中添加以下配置：【可参考mybatis手册】



<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216102117517.png" alt="image-20251216102117517" style="zoom:67%;" />

> 开启日志：在`mybatis-config.xml`文件中使用`settings标签`进行配置开启。

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

 这个标签在编写的时候要注意，它应该出现在environments标签之前。注意顺序。当然，不需要记忆这个顺序。因为有dtd文件进行约束呢。我们只要参考dtd约束即可。

这种实现也是可以的，可以看到一些信息，比如：连接对象什么时候创建，什么时候关闭，sql语句是怎样的。 但是没有详细的日期，线程名字，等。如果你想使用更加丰富的配置，可以集成第三方的log组件。



> 标准日志也可以用，但是配置不够灵活，可以集成其他的日志组件，例如：log4j，logback等。
>
> logback是目前日志框架中性能较好的，较流行的。

#### 第一步：引入logback相关依赖

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216102345470.png" alt="image-20251216102345470" style="zoom:67%;" />

```xml
<!--引入logback的依赖，这个日志框架实现了slf4j规范-->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.5.18</version>
            <scope>compile</scope>
        </dependency>
```

#### 第二步：引入logback相关配置文件（文件名叫做logback.xml或logback-test.xml，放到类路径当中）

> 配置文件的名字必须叫做：logback.xml或者logback-test.xml，不能是其它的名字。
>
> 配置文件必须放到类的根路径下。不能是其他位置。
>
> 主要配置日志输出相关的级别以及日志具体的格式。

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216102025829.png" alt="image-20251216102025829" style="zoom:67%;" />

```xml
<?xml version="1.0" encoding="UTF-8"?>

<configuration debug="false">
    <!-- 控制台输出 -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>[%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
    </appender>

    <!--mybatis log configure-->
    <logger name="com.apache.ibatis" level="TRACE"/>
    <logger name="java.sql.Connection" level="DEBUG"/>
    <logger name="java.sql.Statement" level="DEBUG"/>
    <logger name="java.sql.PreparedStatement" level="DEBUG"/>

    <!-- 日志输出级别,logback日志级别包括五个：TRACE < DEBUG < INFO < WARN < ERROR -->
    <root level="DEBUG">
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="FILE"/>
    </root>

</configuration>
```

> 再次执行单元测试方法testInsertCar，查看控制台是否有sql语句输出

![image-20251216102441920](C:\Tian_File\Typora_images\typora-user-images\image-20251216102441920.png)



### 2.8 MyBatis工具类SqlSessionUtil的封装

> 每一次获取SqlSession对象代码太繁琐，封装一个工具类

```java
package com.powernode.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;

public class SqlSessionUtil {
    //工具类的构造方法一般都是私有化的,防止创建对象
    private SqlSessionUtil(){}

    private static SqlSessionFactory sqlSessionFactory;

    //类加载时初始化SqlSessionFactory对象
    static{
        //SqlSessionUtil工具类在进行第一次加载的时候，解析mybatis-config.xml文件，创建SqlSessionFactory对象。
        try {
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
    
    //工具类中的所有方法都是静态的，直接采用类名方便调用，不需要new对象。
    /**
     * 每调用一次openSession()可获取一个新的会话，该会话支持自动提交
     */
    //获取会话对象
    public static SqlSession openSession(){
        // 相当于JDBC中conn.setAutoCommit(true);
        return sqlSessionFactory.openSession(true);
    }
}

```

> 测试工具类，将testInsertCar()改造

```java
@Test
public void testInsertCar(){
    SqlSession sqlSession = SqlSessionUtil.openSession();
    // 执行SQL
    int count = sqlSession.insert("insertCar");
    System.out.println("插入了几条记录:" + count);
    sqlSession.close();
}
```



## 三、MyBatis的事务管理机制

> 在`mybatis-config.xml`文件中，可以通过以下的配置进行mybatis的事务管理
>
> `<transactionManager type="JDBC"/>`

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216103250766.png" alt="image-20251216103250766" style="zoom:67%;" />

> type属性的值包括两个：JDBC(jdbc)、MANAGED(managed) 
>
> 只有以上两个值可选，不区分大小写。
>
> 在MyBatis中提供了两种事务管理机制：JDBC事务管理机制、MANAGED事务管理机制



#### 3.1 JDBC事务管理机制

> mybatis框架自己管理事务，自己采用原生的JDBC代码去管理事务：

```java
// 开启事务
conn.setAutoCommit(false);

//....业务处理...

// 手动提交事务
conn.commit(); 
```

> 使用JDBC事务管理器的话，底层创建的事务管理器对象：`JdbcTransaction`对象。

如果你编写的代码是下面的代码：

```java
// 自动提交事务
SqlSession sqlSession = sqlSessionFactory.openSession(true);
```

> 表示没有开启事务。
>
> 这种方式压根不会执行，因为JDBC事务管理执行的是：`conn.setAutoCommit(false);`
> 在JDBC事务中，没有执行`conn.setAutoCommit(false);`那么`autoCommit`就是`true`。
> 如果`autoCommit`是`true`，就表示没有开启事务。只要执行任意一条DML语句就提交一次。
>
> 如果没有在JDBC代码中执行：`conn.setAutoCommit(false);`的话，默认的`autoCommit`是`true`。



#### 3.2 MANAGED事务管理

> mybatis不再负责事务的管理了。事务管理交给其它容器来负责。例如：`spring`。
>
> 只有mybatis的情况下，如果配置为：MANAGED，那么事务是没人管的。没有人管理事务表示事务压根没有开启。



**重点注意**

> 以后注意了，只要`autoCommit`是`true`，就表示没有开启事务。只有`autoCommit`是`false`，就表示开启了事务。



## 四、使用MyBatis完成CRUD

### 4.1 Insert

> 存在的问题是：SQL语句中的值不应该写死，值应该是用户提供的。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace先随便写-->
<mapper namespace="car">
    <insert id="insertCar">
        <!--在实际开发中是不存在的，一定是前端form表单提交过来的数据，然后将值传给SQL语句-->
        insert into t_car(car_num,brand,guide_price,produce_time,car_type) values('103', '奔驰E300L', 50.3, '2022-01-01', '燃油车')
    </insert>
</mapper>
```

> 在JDBC中，SQL语句的传值是这样的：

```java
// JDBC中使用 ? 作为占位符。
String sql = "insert into t_car(car_num,brand,guide_price,produce_time,car_type) values(?,?,?,?,?)";

// 给 ? 传值，那么MyBatis中应该怎么传值呢？
ps.setString(1,"103");
ps.setString(2,"奔驰E300L");
ps.setDouble(3,50.3);
ps.setString(4,"2022-01-01");
ps.setString(5,"燃油车");
```

> 在MyBatis中可以这样做：在Java程序中，将数据放到Map集合中,**在sql语句中使用 #{map集合的key} 来完成传值，#{} 等同于JDBC中的 ? ，#{}就是占位符**

```java
package com.powernode.mybatis;

import com.powernode.mybatis.utils.SqlSessionUtil;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.HashMap;
import java.util.Map;

public class CarMapperTest {
    @Test
    public void testInsertCar(){
        // 准备数据
        Map<String, Object> map = new HashMap<>();
        map.put("k1", "103");
        map.put("k2", "奔驰E300L");
        map.put("k3", 50.3);
        map.put("k4", "2020-10-01");
        map.put("k5", "燃油车");
        // 获取SqlSession对象
        SqlSession sqlSession = SqlSessionUtil.openSession();
        // 执行SQL语句（使用map集合给sql语句传递数据）
        int count = sqlSession.insert("insertCar", map);
        System.out.println("插入了几条记录：" + count);
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace先随便写-->
<mapper namespace="car">
    <insert id="insertCar">
        insert into t_car(car_num,brand,guide_price,produce_time,car_type) values(#{k1},#{k2},#{k3},#{k4},#{k5})
    </insert>
</mapper>
```

> 注意：**#{} 的里面必须填写map集合的key，不能随便写，应该写数据库表中的字段/pojo类的属性名**
>
> 如果#{}里写的是map集合中不存在的key，导致无法获取到map集合中的数据，最终导致该数据为NULL。
>
> 采用map集合传参，#{} 里写的是map集合的key，如果key不存在不会报错，数据库表中会插入NULL。

```java
Map<String, Object> map = new HashMap<>();
// 一般map集合的key起名的时候要见名知意，让key的可读性增强。
map.put("carNum", "103");
map.put("brand", "奔驰E300L");
map.put("guidePrice", 50.3);
map.put("produceTime", "2020-10-01");
map.put("carType", "燃油车");
```

> 如果给#{}传map集合中的key值，些许繁琐，可以将数据封装到对象中，然后执行`insert`操作时，传参不传`map`而是传`对象`。

```java
package com.powernode.mybatis.pojo;

/**
 * POJO类：简单普通的Java对象，封装数据用的。
 */
public class Car {
    private Long id;
    private String carNum;
    private String brand;
    private Double guidePrice;
    private String produceTime;
    private String carType;

    @Override
    public String toString() {
        return "Car{" +
                "id=" + id +
                ", carNum='" + carNum + '\'' +
                ", brand='" + brand + '\'' +
                ", guidePrice=" + guidePrice +
                ", produceTime='" + produceTime + '\'' +
                ", carType='" + carType + '\'' +
                '}';
    }

    public Car() {
    }

    public Car(Long id, String carNum, String brand, Double guidePrice, String produceTime, String carType) {
        this.id = id;
        this.carNum = carNum;
        this.brand = brand;
        this.guidePrice = guidePrice;
        this.produceTime = produceTime;
        this.carType = carType;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getCarNum() {
        return carNum;
    }

    public void setCarNum(String carNum) {
        this.carNum = carNum;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public Double getGuidePrice() {
        return guidePrice;
    }

    public void setGuidePrice(Double guidePrice) {
        this.guidePrice = guidePrice;
    }

    public String getProduceTime() {
        return produceTime;
    }

    public void setProduceTime(String produceTime) {
        this.produceTime = produceTime;
    }

    public String getCarType() {
        return carType;
    }

    public void setCarType(String carType) {
        this.carType = carType;
    }
}
```

```java
@Test
public void testInsertCarByPOJO(){
    // 创建POJO，封装数据
    Car car = new Car();
    car.setCarNum("103");
    car.setBrand("奔驰C200");
    car.setGuidePrice(33.23);
    car.setProduceTime("2020-10-11");
    car.setCarType("燃油车");
    // 获取SqlSession对象
    SqlSession sqlSession = SqlSessionUtil.openSession();
    // 执行SQL，传数据
    int count = sqlSession.insert("insertCarByPOJO", car);
    System.out.println("插入了几条记录" + count);
}
```

```xml
<insert id="insertCarByPOJO">
  <!--#{} 里写的是POJO的属性名-->
  insert into t_car(car_num,brand,guide_price,produce_time,car_type) values(#{carNum},#{brand},#{guidePrice},#{produceTime},#{carType})
</insert>
```

> #{} 里写的是POJO的属性名，如果#{}里不写POJO类的属性名，则在做插入操作时，有些值会为NULL。
>
> ![image-20251216120149986](C:\Tian_File\Typora_images\typora-user-images\image-20251216120149986.png)
>
> 错误信息中描述：在Car类中没有找到属性的getter方法。
>
> 原因：POJO类的属性名可能与表中的字段名称对应不上。
>
> **采用POJO传参，传的是POJO类的属性名**。（例如：getAge对应的是#{age}，getUserName对应的是#{userName}），如果这样的get方法不存在会报错。

> 注意：其实传参数的时候有一个属性parameterType，这个属性用来指定传参的数据类型，不过这个属性是可以省略的。

```xml
<insert id="insertCar" parameterType="java.util.Map">
  insert into t_car(car_num,brand,guide_price,produce_time,car_type) values(#{carNum},#{brand},#{guidePrice},#{produceTime},#{carType})
</insert>

<insert id="insertCarByPOJO" parameterType="com.powernode.mybatis.pojo.Car">
  insert into t_car(car_num,brand,guide_price,produce_time,car_type) values(#{carNum},#{brand},#{guidePrice},#{produceTime},#{carType})
</insert>
```



### 4.2 Delete

> 根据id进行删除汽车信息。

```xml
<!--delete语句：根据id删除某条记录-->
<delete id="deleteById">
    delete from t_car where id = #{id}
</delete>
```

```java
@Test
public void testDeleteCarById(){
    SqlSession sqlSession = SqlSessionUtil.openSession();
    int count = sqlSession.delete("deleteById",13);
    System.out.println("DelCount = " + count);
    sqlSession.commit();
    sqlSession.close();
}
```

**注意：当占位符只有一个的时候，${} 里面的内容可以随便写。**



### 4.3 Update

> 需求：修改id=34的Car信息，car_num为102，brand为"比亚迪汉"，guide_price为30.23，produce_time为"2018-09-10"，car_type为"电车"
> 修改前：

![image-20251216202651415](C:\Tian_File\Typora_images\typora-user-images\image-20251216202651415.png)

```xml
<update id="updateCarByPOJO">
  update t_car set car_num = #{carNum}, brand = #{brand},guide_price = #{guidePrice}, produce_time = #{produceTime}, car_type = #{carType} where id = #{id}
</update>
```

```java
@Test
public void testUpdateCarByPOJO(){
   // 准备数据
   Car car = new Car();
   car.setId(34L);
   car.setCarNum("102");
   car.setBrand("比亚迪汉");
   car.setGuidePrice(30.23);
   car.setProduceTime("2018-09-10");
   car.setCarType("电车");
   // 获取SqlSession对象
   SqlSession sqlSession = SqlSessionUtil.openSession();
   // 执行SQL语句
   int count = sqlSession.update("updateCarByPOJO", car);
   System.out.println("更新了几条记录：" + count);
}
```

![image-20251216202849256](C:\Tian_File\Typora_images\typora-user-images\image-20251216202849256.png)

> 当然了，如果使用**map**传数据也是可以的。



### 4.4 Select

> select语句和其它语句不同的是：查询会有一个结果集。

#### 查询一条数据

> 需求：查询id为1的Car信息

```xml
<select id="selectCarById">
  select * from t_car where id = #{id}
</select>
```

```java
@Test
public void testSelectCarById(){
    // 获取SqlSession对象
    SqlSession sqlSession = SqlSessionUtil.openSession();
    // 执行SQL语句
    Object car = sqlSession.selectOne("selectCarById", 1);
    System.out.println(car);
}
```

> 会出现异常报错，对于一个查询语句来说，你需要指定它的“结果类型”或者“结果映射”。
>
> 所以说，让mybatis查询之后返回一个Java对象的话，至少要告诉mybatis返回一个什么类型的Java对象，可以在`<select>`标签中添加`resultType`属性，用来指定查询要转换的类型，即：pojo类。

```xml
<select id="selectCarById" resultType="com.powernode.mybatis.pojo.Car">
  select * from t_car where id = #{id}
</select>
```

![image-20251216203903243](C:\Tian_File\Typora_images\typora-user-images\image-20251216203903243.png)

> 运行后之前的异常不再出现了，这说明添加了resultType属性之后，解决了之前的异常，可以看出resultType是不能省略的。
>
> 但是可能出现其他问题，某些字段查找出来为NULL值，这是因为表中字段名称和POJO类中的属性名称没有对应上。
>
> **解决方案**：做Select操作时，通过`as`关键字将表中字段名 **起别名** 为POJO类的属性名。
>
> **还有一种解决方法：设置SQL字段的`resultMap`属性（在“第十一部分”）**

```xml
<!--select语句：根据id查询一条记录-->
<!--resultType:通常写全限定类名，指定结果集要封装的java对象类型告诉mybatis-->
<!--表的属性名和类的属性名对应不上，解决方法：使用as起别名-->
<select id="selectById" resultType="com.powernode.pojo.Car">
    select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car where id = #{id}
</select>
```

![image-20251216204310917](C:\Tian_File\Typora_images\typora-user-images\image-20251216204310917.png)

> 通过测试得知，如果当查询结果的字段名和java类的属性名对应不上的话，可以采用as关键字起别名，**当然还有其它解决方案，我们后面再看**。



#### 查询多条数据

> 需求：查询所有的Car信息。

```xml
<!--select语句：查询所有-->
<!--resultType还是指定要封装的结果集的类型。不是指定List类型，是指定List集合中元素的类型-->
<select id="selectAll" resultType="com.powernode.pojo.Car">
    select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car
</select>
```

```java
@Test
public void testSelectAll(){
    SqlSession sqlSession = SqlSessionUtil.openSession();
    // selectList方法：mybatis通过这个方法就可以得知你需要一个List集合。它会自动给你返回一个List集合。
    List<Object> cars = sqlSession.selectList("selectAll");
    // 遍历
    cars.forEach(car -> System.out.println(car));
    sqlSession.close();
}
```

![image-20251216204614839](C:\Tian_File\Typora_images\typora-user-images\image-20251216204614839.png)

> 需要注意：select标签中resultType属性，这个属性用来告诉mybatis，查询结果集封装成什么类型的java对象。
>
> **resultType通常写的是：全限定类名**。



### 4.5 关于SQL Mapper的namespace

> 在SQL Mapper配置文件中`<mapper>`标签的`namespace`属性是用来指定命名空间的，这个命名空间主要是为了防止`sqlId`冲突的。

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216205033803.png" alt="image-20251216205033803" style="zoom:67%;" />

> 如果`CarMapper.xml`和`CarMapper2.xml`文件中都有`id="selectCarAll"`，将`CarMapper2.xml`配置到`mybatis-config.xml`文件中。

```xml
<mappers>
  <mapper resource="CarMapper.xml"/>
  <mapper resource="CarMapper2.xml"/>
</mappers>
```

> 问题：如果要在测试类中`selectAll`，是会访问`CarMapper.xml`文件，还是访问`CarMapper2.xml`文件？

```java
@Test
public void testNamespace(){
    // 获取SqlSession对象
    SqlSession sqlSession = SqlSessionUtil.openSession();
    // 执行SQL语句
    List<Object> cars = sqlSession.selectList("selectCarAll");
    // 输出结果
    cars.forEach(car -> System.out.println(car));
}
```

> 答案是会报错。因为`selectCarAll`在`Mapped Statements`集合中不明确，大致意思就是`selectCarAll`重名了，你要么在`selectCarAll`前添加一个名称空间，要有你改个其它名字。
>
> 解决方法：`namespace.id`的方式就可以解决。

```java
@Test
public void testNamespace(){
    // 获取SqlSession对象
    SqlSession sqlSession = SqlSessionUtil.openSession();
    // 执行SQL语句
    //List<Object> cars = sqlSession.selectList("car.selectCarAll");
    List<Object> cars = sqlSession.selectList("car2.selectCarAll");
    // 输出结果
    cars.forEach(car -> System.out.println(car));
}
```



## 五、MyBatis核心配置文件详情

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <!--配置事务管理器-->
            <transactionManager type="JDBC"/>
            <!--配置数据源，type类型有：UNPOOLED(未使用数据库连接池)、POOLED(使用了数据库连接池)、JNDI(自定义)-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/powernode"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="CarMapper.xml"/>
        <mapper resource="CarMapper2.xml"/>
    </mappers>
</configuration>
```

- configuration：根标签，表示配置信息。
- environments：环境（多个），以“s”结尾表示复数，也就是说mybatis的环境可以配置多个数据源。
  - default属性：表示默认使用的是哪个环境，default后面填写的是environment的id。**default的值只需要和environment的id值一致即可**。
- environment：具体的环境配置（**主要包括：事务管理器的配置 + 数据源的配置**）
  - id：给当前环境一个唯一标识，该标识用在environments的default后面，用来指定默认环境的选择。
- transactionManager：配置事务管理器
  - type属性：指定事务管理器具体使用什么方式，可选值包括两个
    - **JDBC**：使用JDBC原生的事务管理机制。**底层原理：事务开启conn.setAutoCommit(false); ...处理业务...事务提交conn.commit();**
    - **MANAGED**：交给其它容器来管理事务，比如WebLogic、JBOSS等。如果没有管理事务的容器，则没有事务。**没有事务的含义：只要执行一条DML语句，则提交一次**。
- dataSource：指定数据源
  - type属性：用来指定具体使用的数据库连接池的策略，可选值包括三个
    - **UNPOOLED**：采用传统的获取连接的方式，虽然也实现Javax.sql.DataSource接口，但是并没有使用池的思想。
      - property可以是：
        - driver 这是 JDBC 驱动的 Java 类全限定名。
        - url 这是数据库的 JDBC URL 地址。
        - username 登录数据库的用户名。
        - password 登录数据库的密码。
        - defaultTransactionIsolationLevel 默认的连接事务隔离级别。
        - defaultNetworkTimeout 等待数据库操作完成的默认网络超时时间（单位：毫秒）
    - **POOLED**：采用传统的javax.sql.DataSource规范中的连接池，mybatis中有针对规范的实现。
      - property可以是（除了包含**UNPOOLED**中之外）：
        - poolMaximumActiveConnections 在任意时间可存在的活动（正在使用）连接数量，默认值：10
        - poolMaximumIdleConnections 任意时间可能存在的空闲连接数。
        - 其它....
    - **JNDI**：采用服务器提供的JNDI技术实现，来获取DataSource对象，不同的服务器所能拿到DataSource是不一样。如果不是web或者maven的war工程，JNDI是不能使用的。
      - property可以是（最多只包含以下两个属性）：
        - initial_context 这个属性用来在 InitialContext 中寻找上下文（即，initialContext.lookup(initial_context)）这是个可选属性，如果忽略，那么将会直接从 InitialContext 中寻找 data_source 属性。
        - data_source 这是引用数据源实例位置的上下文路径。提供了 initial_context 配置时会在其返回的上下文中进行查找，没有提供时则直接在 InitialContext 中查找。
- mappers：在mappers标签中可以配置多个sql映射文件的路径。
- mapper：配置某个sql映射文件的路径
  - resource属性：使用相对于类路径的资源引用方式
  - url属性：使用完全限定资源定位符（URL）方式



### 5.1 environment

> 具体的环境配置（主要包括：事务管理器 + 数据源配置）
>
> **一个数据库对应一个SqlSessionFactory**
>
> `environments`字段中的`default`属性填写默认优先访问的环境。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--默认使用开发环境-->
    <!--<environments default="dev">-->
    <!--默认使用生产环境-->
    <environments default="production">
        <!--开发环境-->
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/powernode"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
        
        <!--生产环境-->
        <environment id="production">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="CarMapper.xml"/>
    </mappers>
</configuration>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="car">
    <insert id="insertCar">
        insert into t_car(id,car_num,brand,guide_price,produce_time,car_type) values(null,#{carNum},#{brand},#{guidePrice},#{produceTime},#{carType})
    </insert>
</mapper>
```

```java
package com.powernode.mybatis;

import com.powernode.mybatis.pojo.Car;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

public class ConfigurationTest {

    @Test
    public void testEnvironment() throws Exception{
        // 准备数据
        Car car = new Car();
        car.setCarNum("133");
        car.setBrand("丰田霸道");
        car.setGuidePrice(50.3);
        car.setProduceTime("2020-01-10");
        car.setCarType("燃油车");

        // 一个数据库对应一个SqlSessionFactory对象
        // 两个数据库对应两个SqlSessionFactory对象，以此类推
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();

        // 使用默认数据库
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config.xml"));
        SqlSession sqlSession = sqlSessionFactory.openSession(true);
        int count = sqlSession.insert("insertCar", car);
        System.out.println("插入了几条记录：" + count);

        // 使用指定数据库
        SqlSessionFactory sqlSessionFactory1 = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config.xml"), "dev");
        SqlSession sqlSession1 = sqlSessionFactory1.openSession(true);
        int count1 = sqlSession1.insert("insertCar", car);
        System.out.println("插入了几条记录：" + count1);
    }
}
```

![image-20251216210650350](C:\Tian_File\Typora_images\typora-user-images\image-20251216210650350.png)

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216210701862.png" alt="image-20251216210701862" style="zoom:67%;" />



### 5.2 transactionManager

> **事务管理器**，只有两个值：JDBC、MANAGED
>
> 当事务管理器是：JDBC
>
> - 采用JDBC的原生事务机制：
>   - 开启事务：conn.setAutoCommit(false);
>   - 处理业务......
>   - 提交事务：conn.commit();
>
> 当事务管理器是：MANAGED
>
> - 交给容器，如：Spring去管理事务，但目前使用的是本地程序，没有容器的支持，**当mybatis找不到容器的支持时：没有事务**。也就是说只要执行一条DML语句，则提交一次。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="dev">
        <environment id="dev">
            <transactionManager type="MANAGED"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/powernode"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="CarMapper.xml"/>
    </mappers>
</configuration>
```

```java
@Test
public void testTransactionManager() throws Exception{
    // 准备数据
    Car car = new Car();
    car.setCarNum("133");
    car.setBrand("丰田霸道");
    car.setGuidePrice(50.3);
    car.setProduceTime("2020-01-10");
    car.setCarType("燃油车");
    // 获取SqlSessionFactory对象
    SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
    SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config2.xml"));
    // 获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession();
    // 执行SQL
    int count = sqlSession.insert("insertCar", car);
    System.out.println("插入了几条记录：" + count);
}
```



### 5.3 dataSource

> **`datasource`数据源：凡是给程序提供`connection`对象的，都叫做数据源。**
>
> `dataSource`指定数据源，`type`属性用来指定具体使用的数据库连接池策略，可选值有三个：UNPOOLED、POOLED、JNDI
>
> **UNPOOLED**：不使用连接池，每一次都会创建新的连接对象。
>
> **POOLED**：使用连接池，会释放连接对象，下次还可以继续使用该连接对象。
>
> **JNDI**：采用服务器提供的JNDI技术实现，来获取DataSource对象，不同的服务器所能拿到DataSource是不一样。如果不是web或者maven的war工程，JNDI是不能使用的。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="UNPOOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/powernode"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="CarMapper.xml"/>
    </mappers>
</configuration>
```

```java
@Test
public void testDataSource() throws Exception{
    // 准备数据
    Car car = new Car();
    car.setCarNum("133");
    car.setBrand("丰田霸道");
    car.setGuidePrice(50.3);
    car.setProduceTime("2020-01-10");
    car.setCarType("燃油车");
    // 获取SqlSessionFactory对象
    SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
    SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config3.xml"));
    // 获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession(true);
    // 执行SQL
    int count = sqlSession.insert("insertCar", car);
    System.out.println("插入了几条记录：" + count);
    // 关闭会话
    sqlSession.close();
}
```

> 当`type`是`UNPOOLED`，控制台输出：

![image-20251216211740345](C:\Tian_File\Typora_images\typora-user-images\image-20251216211740345.png)

> 修改配置文件mybatis-config.xml中的配置：
>
> ```xml
> <dataSource type="POOLED">
> ```
>
> Java测试程序不需要修改，直接执行，看控制台输出：

![image-20251216211837218](C:\Tian_File\Typora_images\typora-user-images\image-20251216211837218.png)

> 通过测试得出：UNPOOLED不会使用连接池，每一次都会新建JDBC连接对象。POOLED会使用数据库连接池。【这个连接池是mybatis自己实现的。】

**JNDI的方式：表示对接JNDI服务器中的连接池。这种方式给了我们可以使用第三方连接池的接口。如果想使用dbcp、c3p0、druid（德鲁伊）等，需要使用这种方式。**



#### type="POOLED"时的属性

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/powernode"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
                
                <!--最大连接数-->
                <property name="poolMaximumActiveConnections" value="3"/>
                <!--这是一个底层设置，如果获取连接花费了相当长的时间，连接池会打印状态日志并重新尝试获取一个连接（避免在误配置的情况下一直失败且不打印日志），默认值：20000 毫秒（即 20 秒）。-->
                <property name="poolTimeToWait" value="20000"/>
                <!--强行回归池的时间-->
                <property name="poolMaximumCheckoutTime" value="20000"/>
                <!--最多空闲数量-->
                <property name="poolMaximumIdleConnections" value="1"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="CarMapper.xml"/>
    </mappers>
</configuration>
```

`poolMaximumActiveConnections`：最大的活动的连接数量。默认值10
`poolMaximumIdleConnections`：最大的空闲连接数量。默认值5
`poolMaximumCheckoutTime`：强行回归池的时间。默认值20秒。
`poolTimeToWait`：当无法获取到空闲连接时，每隔20秒打印一次日志，避免因代码配置有误，导致傻等。（时长是可以配置的）
当然，还有其他属性。对于连接池来说，以上几个属性比较重要。
最大的活动的连接数量就是连接池连接数量的上限。默认值10，如果有10个请求正在使用这10个连接，第11个请求只能等待空闲连接。

![image-20251218091953395](C:\Tian_File\Typora_images\typora-user-images\image-20251218091953395.png)

最大的空闲连接数量。默认值5，如何已经有了5个空闲连接，当第6个连接要空闲下来的时候，连接池会选择关闭该连接对象。来减少数据库的开销。
需要根据系统的并发情况，来合理调整连接池最大连接数以及最多空闲数量。充分发挥数据库连接池的性能。【可以根据实际情况进行测试，然后调整一个合理的数量。】
下图是默认配置：

![image-20251216212305105](C:\Tian_File\Typora_images\typora-user-images\image-20251216212305105.png)



### 5.4 properties

> 可以看到在`mybatis-config.xml`文件中，连接数据库的信息在多重字段内部，每次修改连接数据库信息，有些麻烦并且容易出现错误。

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251216212426006.png" alt="image-20251216212426006" style="zoom:67%;" />

> mybatis提供了更加灵活的配置，连接数据库的信息可以单独写到一个属性资源文件中，假设在类的根路径下创建`jdbc.properties`文件，配置如下：

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/powernode
jdbc.user=root
jdbc.password=1265179125
```

> 在mybatis核心配置文件中引入并使用：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--引入外部属性资源文件-->
    <properties resource="jdbc.properties"></properties>

    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!--${key}使用-->
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="CarMapper.xml"/>
    </mappers>
</configuration>
```

>**properties两个属性：resource、url**
>**resource：这个属性从类的根路径下开始加载。【常用的】**
>**url：从指定的url加载，假设文件放在d:/jdbc.properties，这个url可以写成：file:///d:/jdbc.properties。（注意是三个斜杠）**
>
>注意：如果不知道mybatis-config.xml文件中标签的编写顺序的话，可以有两种方式知道它的顺序：
>
>- 第一种方式：查看dtd约束文件。
>- 第二种方式：通过idea的报错提示信息。【一般采用这种方式】



### 5.5 mapper

> mapper标签用来指定SQL映射文件的路径，包含多种指定方式，这里先主要看其中两种：

> 第一种：**resource**，**从类的根路径下开始加载**【比url常用】

```xml
<mappers>
  <mapper resource="CarMapper.xml"/>
</mappers>
```

> 如果是这样写的话，必须保证类的根下有CarMapper.xml文件。
> 如果类的根路径下有一个包叫做test，CarMapper.xml如果放在test包下的话，这个配置应该是这样写：

```xml
<mappers>
  <mapper resource="test/CarMapper.xml"/>
</mappers>
```

> 第二种：**url，从指定的url位置加载**
> 假设CarMapper.xml文件放在d盘的根下，这个配置就需要这样写：

```xml
<mappers>
  <mapper url="file:///d:/CarMapper.xml"/>
</mappers>
```



## 六、WEB中应用MyBatis（使用MVC框架）

> **实现功能：**
>
> - 银行账户转账
> - <img src="C:\Tian_File\Typora_images\typora-user-images\image-20251217091500899.png" alt="image-20251217091500899" style="zoom:67%;" />
>
> **使用技术：**
>
> - HTML + Servlet + MyBatis
>
> **数据库表：**
>
> - <img src="C:\Tian_File\Typora_images\typora-user-images\image-20251217091543155.png" alt="image-20251217091543155" style="zoom:67%;" />



### 6.1 环境搭建

> IDEA中创建Maven WEB应用

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251217091657038.png" alt="image-20251217091657038" style="zoom:67%;" />

> IDEA配置Tomcat，这里Tomcat使用11版本。并部署应用到tomcat。

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251217091742826.png" alt="image-20251217091742826" style="zoom:67%;" />

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251217091811027.png" alt="image-20251217091811027" style="zoom:67%;" />

> 默认创建的maven web应用没有java和resources目录，包括两种解决方案:
>
> 第一种：自己手动加上
>
> ![image-20251217091910052](C:\Tian_File\Typora_images\typora-user-images\image-20251217091910052.png)
>
> 第二种：修改maven-archetype-webapp-1.4.jar中的配置文件
>
> <img src="C:\Tian_File\Typora_images\typora-user-images\image-20251217091944255.png" alt="image-20251217091944255" style="zoom:67%;" />
>
> <img src="C:\Tian_File\Typora_images\typora-user-images\image-20251217092008516.png" alt="image-20251217092008516" style="zoom: 50%;" />



> 创建出来的web.xml文件的版本较低，可以从tomcat11的样例文件中复制，然后修改。
>
> ```xml
> <?xml version="1.0" encoding="UTF-8"?>
> <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
>          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>          xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
>                       https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
>          version="5.0"
>          metadata-complete="true">
> 
> </web-app>
> ```



> 其他设置：
>
> - 删除index.jsp文件，因为我们这个项目不使用JSP，只使用html。
>
> - 确定pom.xml文件中的打包方式是war包。
> - 引入相关依赖
>   - 编译器版本修改为21
>   - 引入的依赖包括：mybatis，mysql驱动，junit，logback，servlet
>
> （注意：**Maven 3.9.11 + JDK21 + MyBatis 3.5.10** 环境适配**Servlet 5.0（Jakarta Servlet 5.0.0）**）

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.powernode</groupId>
  <artifactId>mybatis-002</artifactId>
  <packaging>war</packaging>
  <version>1.0</version>

  <name>mybatis-002 Maven Webapp</name>
  <url>http://localhost:8080/bank</url>
    
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
    </properties>

  <dependencies>
    <!--mybatis依赖-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.10</version>
    </dependency>
    <!--mysql驱动-->
    <dependency>
      <groupId>com.mysql</groupId>
      <artifactId>mysql-connector-j</artifactId>
      <version>9.2.0</version>
    </dependency>
    <!--引入logback的依赖，这个日志框架实现了slf4j规范-->
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.5.18</version>
      <scope>compile</scope>
    </dependency>
    <!--servlet依赖-->
    <dependency>
      <groupId>jakarta.servlet</groupId>
      <artifactId>jakarta.servlet-api</artifactId>
      <version>5.0.0</version>
      <scope>provided</scope>
    </dependency>

  </dependencies>

  <build>
    <finalName>mybatis-002</finalName>
  </build>
</project>
```



>引入相关配置文件，放到resources目录下（全部放到类的根路径下）
>
>- mybatis-config.xml
>- AccountMapper.xml
>- logback.xml
>- jdbc.properties
>
>![image-20251217092752769](C:\Tian_File\Typora_images\typora-user-images\image-20251217092752769.png)

> **jdbc.properties**

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/powernode
jdbc.username=root
jdbc.password=root
```

> **mybatis-config.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <properties resource="jdbc.properties"/>

    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!--一定要注意这里的路径哦！！！-->
        <mapper resource="AccountMapper.xml"/>
    </mappers>
</configuration>
```

> **AccountMapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="account">

</mapper>
```



### 6.2 index和其他页面信息

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>银行账户转账</title>
</head>
<body>
<!--/bank是应用的根，部署web应用到tomcat的时候一定要注意这个名字-->
<form action="/bank/transfer" method="post">
    转出账户：<input type="text" name="fromActno"/><br>
    转入账户：<input type="text" name="toActno"/><br>
    转账金额：<input type="text" name="money"/><br>
    <input type="submit" value="转账"/>
</form>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>异常信息</title>
</head>
<body>
<h1>余额不足！！！</h1>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>异常信息</title>
</head>
<body>
<h1>转账失败！！！</h1>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>转账报告</title>
</head>
<body>
<h1>转账成功!!!</h1>
</body>
</html>
```



### 6.3 创建软件包

> 创建pojo包、service包、dao包、web包、utils包、exceptions包

![image-20251217093204240](C:\Tian_File\Typora_images\typora-user-images\image-20251217093204240.png)



### 6.4 定义POJO类：Account

```java
package com.powernode.bank.pojo;

public class Account {
    private Long id;
    private String actno;
    private Double balance;

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", actno='" + actno + '\'' +
                ", balance=" + balance +
                '}';
    }

    public Account() {
    }

    public Account(Long id, String actno, Double balance) {
        this.id = id;
        this.actno = actno;
        this.balance = balance;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getActno() {
        return actno;
    }

    public void setActno(String actno) {
        this.actno = actno;
    }

    public Double getBalance() {
        return balance;
    }

    public void setBalance(Double balance) {
        this.balance = balance;
    }
}
```



### 6.5 编写`AccountDao`接口，以及`AccountDaoImpl`实现类

> 分析dao中至少要提供几个方法，才能完成转账：
>
> - 转账前需要查询余额是否充足：selectByActno
> - 转账时要更新账户：update

```java
package dao;

import pojo.Account;

/**
 * 账户的DAO对象，负责t_act表中数据的CRUD增删改查
 */
public interface AccountDao {
    /**
     * 通过actno查询账户
     * @param actno
     * @return
     */
    Account selectByActno(String actno);

    /**
     * 传入账户对象更新数据
     * @param account
     * @return
     */
    int updateByActno(Account account);
}

```

```java
package dao.Impl;

import dao.AccountDao;
import org.apache.ibatis.session.SqlSession;
import pojo.Account;
import utils.SqlSessionUtil;

public class AccountDaoImpl implements AccountDao {
    /**
     * 最开始在AccountDaoImpl实现类中对事务进行提交和关闭
     * 在后续为了保证处理数据和转账过程使用的是同一个SqlSession，就不在实现类中对事务提交和关闭了
     */

    @Override
    public Account selectByActno(String actno) {
        SqlSession sqlSession = SqlSessionUtil.openSession();

        // 参数：参数1：传入SQLID，参数2：传入方法参数给SQL语句
        // Account act = (Account) sqlSession.selectOne("account.selectByActno",actno);
        //sqlSession.close();
        return (Account) sqlSession.selectOne("account.selectByActno",actno);
    }

    @Override
    public int updateByActno(Account account) {
        SqlSession sqlSession = SqlSessionUtil.openSession();

        //int count = sqlSession.update("account.updateByActno",account);
        /*sqlSession.commit();
        sqlSession.close();*/
        return sqlSession.update("account.updateByActno",account);
    }
}
```



### 6.6 编写SQL映射文件

> 在`AccountDaoImpl`中编写了`mybatis`代码，需要编写SQL映射文件了。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace使用接口的全限定类名-->
<mapper namespace="dao.AccountDao">
    <!--id使用接口中的方法名-->
    <select id="selectByActno" resultType="pojo.Account">
        select * from t_act where actno = #{actno}
    </select>

    <update id="updateByActno">
        update t_act set balance = #{balance} where actno = #{actno}
    </update>
</mapper>
```



### 6.7 编写`AccountService`接口和`AccountServiceImpl`实现类

```java
package service;

import exceptions.MoneyNotEnoughException;
import exceptions.TransferException;

public interface AccountService {
    void transfer(String fromActno,String toActno,double money) throws MoneyNotEnoughException, TransferException;
}

```

```java
package service.Impl;

import dao.AccountDao;
import dao.Impl.AccountDaoImpl;
import exceptions.MoneyNotEnoughException;
import exceptions.TransferException;
import org.apache.ibatis.session.SqlSession;
import pojo.Account;
import service.AccountService;
import utils.GenerateDaoProxy;
import utils.SqlSessionUtil;

public class AccountServiceImpl implements AccountService {
    /**
     *  不再使用DAO实现类去实现DAO接口,可以通过下面两种方法动态生成DAO接口实现类。
     *  private AccountDao accountDao = new AccountDaoImpl();
     */

    /**
     *  方法一：自定义动态创建接口实现类
     *  通过GenerateDaoProxy工具类去实现DAO接口,参数：(SqlSession,接口)
     *  private AccountDao accountDao = (AccountDao) GenerateDaoProxy.generate(SqlSessionUtil.openSession(),AccountDao.class);
     */

    /**
     *  方法二：使用MyBatis中提供的getMapper方法
     *  在MyBatis当中，MyBatis提供了相关的机制也可以动态生成dao接口的实现类。（代理类：dao接口的代理）
     *  MyBatis当中实际上采用了代理模式，在内存中生成dao接口的代理类，然后创建代理类的实例。
     *  使用MyBatis的这种代理机制的前提：SqlMapper.xml文件中namespace必须是dao接口的全限定名称，id必须是dao接口中的方法名。
     */
    //使用SqlSession.getMapper(接口.class);
    private AccountDao accountDao = SqlSessionUtil.openSession().getMapper(AccountDao.class);

    @Override
    public void transfer(String fromActno, String toActno, double money) throws MoneyNotEnoughException, TransferException {
        //开启事务，和DAO中的事务是同一个对象
        SqlSession sqlSession = SqlSessionUtil.openSession();

        Account fromAct = accountDao.selectByActno(fromActno);
        //判断转出账户余额是否充足
        if (fromAct.getBalance() < money) {
            //如果转出账户余额不足，则抛出异常
            throw new MoneyNotEnoughException("余额不足！！！");
        }
        Account toAct = accountDao.selectByActno(toActno);
        fromAct.setBalance(fromAct.getBalance() - money);
        toAct.setBalance(toAct.getBalance() + money);

        //更新数据库
        int count = accountDao.updateByActno(fromAct);

        /* //模拟异常
         * 如果在此加入事务 SqlSessionUtil.openSession()会发现事务并不能进行。
         * 因为在AccountDao中也有事务，但是两者的事务对象不是同一个，所以不是进行的同一个事务。
         * 所以使用MVC架构中的ThreadLocal，来保证同一个线程使用同一个事务。
         */
        /*String str = null;
        str.toString();*/

        count += accountDao.updateByActno(toAct);

        if(count != 2){
            throw new TransferException("转账失败！！！");
        }

        //提交事务
        sqlSession.commit();
        //关闭事务
        SqlSessionUtil.close(sqlSession);
    }
}

```

> 在业务层面上，会出现转账失败或余额不足的情况，所以编写了自定义Exception类。

```java
package exceptions;

public class MoneyNotEnoughException extends Exception{
    public MoneyNotEnoughException(){}
    public MoneyNotEnoughException(String msg){
        super(msg);
    }
}
```

```java
package exceptions;

public class TransferException extends Exception{
    public TransferException(){}
    public TransferException(String msg){
        super(msg);
    }
}
```



### 6.8 编写`AccountController`

> 编写`AccountController`控制类，其实也就是`Servlet`，作用：调用`Service`完成转账业务。

```java
package servlet;

import exceptions.MoneyNotEnoughException;
import exceptions.TransferException;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import service.AccountService;
import service.Impl.AccountServiceImpl;

import java.io.IOException;

@WebServlet("/transfer")
public class AccountServlet extends HttpServlet {
    //为了让这个对象在其他方法也可以使用，声明为实例变量
    private AccountService accountService = new AccountServiceImpl();

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取表单数据
        String fromActno = request.getParameter("fromActno");
        String toActno = request.getParameter("toActno");
        double money = Double.parseDouble(request.getParameter("money"));

        try {
            //调用service的转账方法完成转账（调业务层）
            accountService.transfer(fromActno,toActno,money);
            //转账成功，调用View完成展示结果
            response.sendRedirect(request.getContextPath() + "/success.html");
        } catch (MoneyNotEnoughException e) {
            response.sendRedirect(request.getContextPath() + "/error1.html");
        } catch (TransferException e) {
            response.sendRedirect(request.getContextPath() + "/error2.html");
        }
    }
}
```



> 测试：启动服务器，打开浏览器，输入地址：http://localhost:8080/bank

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251218093718951.png" alt="image-20251218093718951" style="zoom:67%;" />



### 6.9 MyBatis对象作用域以及事务问题

#### 一、MyBatis核心对象的作用域

##### SqlSessionFactoryBuilder

> 这个类可以被实例化、使用和丢弃，**一旦创建了 SqlSessionFactory，就不再需要它了**。 因此 SqlSessionFactoryBuilder 实例的最佳作用域是方法作用域（也就是局部方法变量）。 你可以重用 SqlSessionFactoryBuilder 来创建多个 SqlSessionFactory 实例，但最好还是不要一直保留着它，以保证所有的 XML 解析资源可以被释放给更重要的事情。

##### SqlSessionFactory

> **SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在**，没有任何理由丢弃它或重新创建另一个实例。 **使用 SqlSessionFactory 的最佳实践是在应用运行期间不要重复创建多次**，多次重建 SqlSessionFactory 被视为一种代码“坏习惯”。因此 SqlSessionFactory 的最佳作用域是应用作用域。 有很多方法可以做到，最简单的就是使用单例模式或者静态单例模式。

##### SqlSession

> **每个线程都应该有它自己的 SqlSession 实例**。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。 **绝对不能将 SqlSession 实例的引用放在一个类的静态域，甚至一个类的实例变量也不行**。 也绝不能将 SqlSession 实例的引用放在任何类型的托管作用域中，比如 Servlet 框架中的 HttpSession。 如果你现在正在使用一种 Web 框架，考虑将 SqlSession 放在一个和 HTTP 请求相似的作用域中。 换句话说，每次收到 HTTP 请求，就可以打开一个 SqlSession，返回一个响应后，就关闭它。 这个关闭操作很重要，为了确保每次都能执行关闭操作，你应该把这个关闭操作放到 finally 块中。 下面的示例就是一个确保 SqlSession 关闭的标准模式：
>
> ```java
> try (SqlSession session = sqlSessionFactory.openSession()) {
>   // 你的应用逻辑代码
> }
> ```



#### 二、事务问题

> 在之前的转账业务中，更新了两个账户，我们需要保证它们的同时成功或同时失败，这个时候就需要使用事务机制，在transfer方法开始执行时开启事务，直到两个更新都成功之后，再提交事务，我们尝试将transfer方法进行如下修改：

```java
package com.powernode.bank.service.impl;

import com.powernode.bank.dao.AccountDao;
import com.powernode.bank.dao.impl.AccountDaoImpl;
import com.powernode.bank.exception.AppException;
import com.powernode.bank.exception.MoneyNotEnoughException;
import com.powernode.bank.pojo.Account;
import com.powernode.bank.service.AccountService;
import com.powernode.bank.utils.SqlSessionUtil;
import org.apache.ibatis.session.SqlSession;

public class AccountServiceImpl implements AccountService {

    private AccountDao accountDao = new AccountDaoImpl();

    @Override
    public void transfer(String fromActno, String toActno, double money) throws MoneyNotEnoughException, AppException {
        // 查询转出账户的余额
        Account fromAct = accountDao.selectByActno(fromActno);
        if (fromAct.getBalance() < money) {
            throw new MoneyNotEnoughException("对不起，您的余额不足。");
        }
        try {
            // 程序如果执行到这里说明余额充足
            // 修改账户余额
            Account toAct = accountDao.selectByActno(toActno);
            fromAct.setBalance(fromAct.getBalance() - money);
            toAct.setBalance(toAct.getBalance() + money);
            // 更新数据库（添加事务）
            SqlSession sqlSession = SqlSessionUtil.openSession();
            accountDao.update(fromAct);
            // 模拟异常
            String s = null;
            s.toString();
            accountDao.update(toAct);
            sqlSession.commit();
            sqlSession.close();
        } catch (Exception e) {
            throw new AppException("转账失败，未知原因！");
        }
    }
}
```

> 这样的结果和之前学习MVC架构遇到的问题是一样的，没有开启事务，遇到异常造成数据丢失。
>
> 主要原因：因为`service`和`dao`中使用的`SqlSession`对象不是同一个。
>
> 解决方法：为了保证`service`和`dao`中使用的`SqlSession`对象是同一个，可以将`SqlSession`对象存放到`ThreadLocal`当中。
>
> 修改`SqlSessionUtil`工具类：

```java
package utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;

public class SqlSessionUtil {
    private SqlSessionUtil(){}

    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 定义一个全局的（服务器级别），一个服务器当中定义一个即可。
     * 为了保证能够使用同一个事务，即使用同一个SqlSession
     */
    private static ThreadLocal<SqlSession> local = new ThreadLocal<>();


    public static SqlSession openSession(){
        //首先从ThreadLocal中获取SqlSession
        SqlSession sqlSession = local.get();
        if(sqlSession == null){
            sqlSession = sqlSessionFactory.openSession();
            //绑定：将SqlSession对象绑定到当前线程
            local.set(sqlSession);
        }
        return sqlSession;
    }

    //关闭SqlSession对象并将其从线程池中移除
    public static void close(SqlSession sqlSession){
        if(sqlSession != null){
            sqlSession.close();
            local.remove();
        }
    }

}
```

> 修改`dao`中的方法：`AccountDaoImpl`中所有方法中的提交`commit`和关闭`close`代码全部删除。
>
> `service`修改后的代码：

```java
package service.Impl;

import dao.AccountDao;
import dao.Impl.AccountDaoImpl;
import exceptions.MoneyNotEnoughException;
import exceptions.TransferException;
import org.apache.ibatis.session.SqlSession;
import pojo.Account;
import service.AccountService;
import utils.GenerateDaoProxy;
import utils.SqlSessionUtil;

/**
 * ClassName:  AccountServiceImpl
 * Description:
 * <p>
 * <p>
 * DateTime:   2025/12/17 9:52
 * Author: Tian
 */
public class AccountServiceImpl implements AccountService {
    /**
     *  不再使用DAO实现类去实现DAO接口,可以通过下面两种方法动态生成DAO接口实现类。
     *  private AccountDao accountDao = new AccountDaoImpl();
     */

    /**
     *  方法一：自定义动态创建接口实现类
     *  通过GenerateDaoProxy工具类去实现DAO接口,参数：(SqlSession,接口)
     *  private AccountDao accountDao = (AccountDao) GenerateDaoProxy.generate(SqlSessionUtil.openSession(),AccountDao.class);
     */

    /**
     *  方法二：使用MyBatis中提供的getMapper方法
     *  在MyBatis当中，MyBatis提供了相关的机制也可以动态生成dao接口的实现类。（代理类：dao接口的代理）
     *  MyBatis当中实际上采用了代理模式，在内存中生成dao接口的代理类，然后创建代理类的实例。
     *  使用MyBatis的这种代理机制的前提：SqlMapper.xml文件中namespace必须是dao接口的全限定名称，id必须是dao接口中的方法名。
     */
    //使用SqlSession.getMapper(接口.class);
    private AccountDao accountDao = SqlSessionUtil.openSession().getMapper(AccountDao.class);

    @Override
    public void transfer(String fromActno, String toActno, double money) throws MoneyNotEnoughException, TransferException {
        //开启事务，和DAO中的事务是同一个对象
        SqlSession sqlSession = SqlSessionUtil.openSession();

        Account fromAct = accountDao.selectByActno(fromActno);
        //判断转出账户余额是否充足
        if (fromAct.getBalance() < money) {
            //如果转出账户余额不足，则抛出异常
            throw new MoneyNotEnoughException("余额不足！！！");
        }
        Account toAct = accountDao.selectByActno(toActno);
        fromAct.setBalance(fromAct.getBalance() - money);
        toAct.setBalance(toAct.getBalance() + money);

        //更新数据库
        int count = accountDao.updateByActno(fromAct);

        /* //模拟异常
         * 如果在此加入事务 SqlSessionUtil.openSession()会发现事务并不能进行。
         * 因为在AccountDao中也有事务，但是两者的事务对象不是同一个，所以不是进行的同一个事务。
         * 所以使用MVC架构中的ThreadLocal，来保证同一个线程使用同一个事务。
         */
        /*String str = null;
        str.toString();*/

        count += accountDao.updateByActno(toAct);

        if(count != 2){
            throw new TransferException("转账失败！！！");
        }

        //提交事务
        sqlSession.commit();
        //关闭事务
        SqlSessionUtil.close(sqlSession);
    }
}
```

> 此时运行测试，可以发现`Service`和`dao`使用的是同一个`SqlSession`对象了。

![image-20251217140600099](C:\Tian_File\Typora_images\typora-user-images\image-20251217140600099.png)



### 6.10 分析当前程序存在的问题

> 观察DAO接口实现类，发现实现类并没有业务流程，只有对数据的CRUD，代码很固定。通过SqlSession对象调用insert、delete、update、select等方法，这个类中的方法没有任何业务逻辑。
>
> 思考：实现类能不能**动态生成**？
>
> 答案：可以，使用`javassist`生成类自动生成接口实现类。

```java
package com.powernode.bank.dao.impl;

import com.powernode.bank.dao.AccountDao;
import com.powernode.bank.pojo.Account;
import com.powernode.bank.utils.SqlSessionUtil;
import org.apache.ibatis.session.SqlSession;

public class AccountDaoImpl implements AccountDao {
    @Override
    public Account selectByActno(String actno) {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        Account act = (Account)sqlSession.selectOne("account.selectByActno", actno);
        return act;
    }

    @Override
    public int update(Account act) {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        int count = sqlSession.update("account.update", act);
        return count;
    }
}
```



## 七、使用javassist生成类

### 7.1 javassist的使用

> 引入`Javassist`依赖

```xml
<!--javassist依赖-->
<dependency>
   <groupId>org.javassist</groupId>
   <artifactId>javassist</artifactId>
   <version>3.29.2-GA</version>
</dependency>
```

> 样例代码：

```java
package com.powernode.javassist;

import javassist.ClassPool;
import javassist.CtClass;
import javassist.CtMethod;
import javassist.Modifier;

import java.lang.reflect.Method;

public class JavassistTest {
    public static void main(String[] args) throws Exception {
        // 获取类池
        ClassPool pool = ClassPool.getDefault();
        // 创建类
        CtClass ctClass = pool.makeClass("com.powernode.javassist.Test");
        // 创建方法
        // 1.返回值类型 2.方法名 3.形式参数列表 4.所属类
        CtMethod ctMethod = new CtMethod(CtClass.voidType, "execute", new CtClass[]{}, ctClass);
        // 设置方法的修饰符列表
        ctMethod.setModifiers(Modifier.PUBLIC);
        // 设置方法体
        ctMethod.setBody("{System.out.println(\"hello world\");}");
        // 给类添加方法
        ctClass.addMethod(ctMethod);
        // 调用方法
        Class<?> aClass = ctClass.toClass();
        Object o = aClass.newInstance();
        Method method = aClass.getDeclaredMethod("execute");
        method.invoke(o);
    }
}
```



### 7.2 使用Javassist生成Dao接口实现类

> 在`utils`包下，编写自动生成接口实现类的工具类：`GenerateDaoByJavassist`

```java
package utils;

import org.apache.ibatis.javassist.CannotCompileException;
import org.apache.ibatis.javassist.ClassPool;
import org.apache.ibatis.javassist.CtClass;
import org.apache.ibatis.javassist.CtMethod;
import org.apache.ibatis.mapping.SqlCommandType;
import org.apache.ibatis.session.SqlSession;

import java.lang.reflect.Method;
import java.util.Arrays;

/**
 * ClassName:  GenerateDaoProxy
 * Description: 工具类：可以动态的生成DAO的实现类。（或者说可以动态生成DAO的代理类）
 *        注意：凡是使用GenerateDaoProxy类的，SQLMapper.xml映射文件中namespace必须是dao接口的全名，id必须是dao接口中的方法名。
 * DateTime:   2025/12/17 16:17
 * Author: Tian
 */
public class GenerateDaoProxy { //假定GenerateDaoProxy是MyBatis框架的开发者编写的
    /**
     * 生成dao接口实现类，并且将实现类的对象创建出来并返回。
     */
    
    /**
     * 为什么方法的参数中要传入SqlSession呢？
     * 原因：为了获取SqlMapper.xml文件中的SQL语句类型
     */
    public static Object generate(SqlSession sqlSession, Class daoInterface){
        //类池
        ClassPool pool = ClassPool.getDefault();
        //制造类(接口 ---> 代理类,例如：com.dao.AccountDao ---> com.dao.AccountDaoProxy)
        //实际本质上就是在内存中动态生成一个代理类,参数为：要生成的接口实现类类名
        CtClass ctClass = pool.makeClass(daoInterface.getName() + "Proxy");
        //制造接口 参数：接口类名
        CtClass ctInterface = pool.makeInterface(daoInterface.getName());
        //实现接口
        ctClass.addInterface(ctInterface);

        //实现接口中所有的方法 (最繁琐)
        //通过接口获取所有的抽象方法
        Method[] methods = daoInterface.getDeclaredMethods();
        //遍历每一个方法
        Arrays.stream(methods).forEach(method -> {
            //method是接口中的抽象方法,现在要将每一个抽象方法实现

            try {
                //将方法内容进行拼接,例如拼接该方法：public Account selectByActno(String actno){ 代码; }
                StringBuilder methodCode = new StringBuilder();
                // 此处可以设置方法的修饰符列表 ctMethod.setModifiers(Modifier.PUBLIC);
                methodCode.append("public ");
                //拼接 返回值类型
                methodCode.append(method.getReturnType().getName());
                methodCode.append(" ");
                //拼接 方法名
                methodCode.append(method.getName());



                //拼接 方法形参列表
                methodCode.append("(");
                //获取方法形参类型
                Class<?>[] parameterTypes = method.getParameterTypes();
                for(int i = 0;i < parameterTypes.length;i++){
                    //每一个参数的类型
                    Class<?> parameterType = parameterTypes[i];
                    methodCode.append(parameterType.getName());
                    methodCode.append(" ");
                    //形参名
                    methodCode.append("arg" + i);
                    //添加","
                    if(i != parameterTypes.length - 1){
                        methodCode.append(",");
                    }
                }
                methodCode.append(")");



                //拼接 方法体中的内容
                methodCode.append("{");
                //方法体中的内容填写可以参考dao实现类，该语句中的类必须写为全限定类名，为了让系统识别引用。
                methodCode.append("org.apache.ibatis.session.SqlSession sqlSession = utils.SqlSessionUtil.openSession();");
                //为了方便获取SqlId,mybatis框架有一个规定，sqlId不能随便写，namespace必须是dao接口的全限定名称，id必须是接口中的方法名
                String sqlId = daoInterface.getName() + "." + method.getName();
                
                // 获取SqlMapper.xml中的SQL语句类型
                SqlCommandType sqlCommandType = sqlSession.getConfiguration().getMappedStatement(sqlId).getSqlCommandType();
                if(sqlCommandType == SqlCommandType.INSERT){

                }
                if(sqlCommandType == SqlCommandType.UPDATE){
                    //按照这样的来写：return sqlSession.update("account.updateByActno",account);
                    methodCode.append("return sqlSession.update(\"" + sqlId + "\",arg0);");
                }
                if(sqlCommandType == SqlCommandType.SELECT){
                    //因为查询需要强转类型，所以需要获取返回值类型
                    String returnType = method.getReturnType().getName();
                    //按照这样的来写：return (Account) sqlSession.selectOne("account.selectByActno",actno);
                    methodCode.append("return (" + returnType + ") sqlSession.selectOne(\"" + sqlId + "\",arg0);");
                }
                if(sqlCommandType == SqlCommandType.DELETE){

                }
                methodCode.append("}");

                CtMethod ctMethod = CtMethod.make(methodCode.toString(), ctClass);
                ctClass.addMethod(ctMethod);
            } catch (CannotCompileException e) {
                e.printStackTrace();
            }
        });

        //创建对象
        Object obj = null;
        try {
            Class<?> clazz = ctClass.toClass();
            obj = clazz.newInstance();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return obj;
    }
}
```

> 通过`SqlId`获取`SqlMapper.xml`中SQL语句类型

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251218102459641.png" alt="image-20251218102459641" style="zoom: 67%;" />

> **修改AccountMapper.xml文件：namespace必须是dao接口的全限定名称，id必须是dao接口中的方法名。**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace使用接口的全限定类名-->
<mapper namespace="dao.AccountDao">
    <!--id使用接口中的方法名-->
    <select id="selectByActno" resultType="pojo.Account">
        select * from t_act where actno = #{actno}
    </select>

    <update id="updateByActno">
        update t_act set balance = #{balance} where actno = #{actno}
    </update>
</mapper>
```

> 修改`service`类中获取`dao`对象的代码：
>
> ![image-20251218102740142](C:\Tian_File\Typora_images\typora-user-images\image-20251218102740142.png)



## 八、MyBatis中接口代理机制及使用（MyBatis框架内部实现了“第七章”内容）

> 通过“第七部分”可以看出，如果通过`Javassist`生成类去实现`Dao`接口实现类的动态创建，还是比较麻烦的。
>
> 好消息！！！“第七部分”的内容`mybatis`内部已经实现了。直接调用以下代码即可获取`dao`接口的代理类：

```java
// sqlSession的getMapper方法获取接口代理对象，也就是接口的实现类。参数为：接口
AccountDao accountDao = (AccountDao)sqlSession.getMapper(AccountDao.class);
```

> 使用以上代码的前提是：**AccountMapper.xml文件中的namespace必须和dao接口的全限定名称一致，id必须和dao接口中方法名一致。**
>
> 将`service`中获取`dao`对象的代码再次修改，如下：
>
> ![image-20251218103645916](C:\Tian_File\Typora_images\typora-user-images\image-20251218103645916.png)

### 需要注意：使用MyBatis之后，`dao`包一般都改名为`mapper`包



### 练习：通过`getMapper`方法实现对`t_car`的CRUD

> 需要mapper包、pojo包、utils包，以及`CarMapper.xml`、`logback.xml`、`mybatis-config.xml`文件
>
> 依赖：mybatis依赖、mysql依赖、junit依赖、logback依赖
>
> mapper：CarMapper接口
>
> pojo：Car实体类
>
> utils：SqlSessionUtil工具类

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.powernode.mapper.CarMapper">
    <insert id="insert">
        insert into t_car(id,car_num,brand,guide_price,produce_time,car_type) values(#{id},#{carNum},#{brand},#{guidePrice},#{produceTime},#{carType});
    </insert>

    <update id="update">
        update t_car set car_num=#{carNum},brand=#{brand},guide_price=#{guidePrice},produce_time=#{produceTime},car_type=#{carType} where id=#{id};
    </update>

    <select id="selectById" resultType="com.powernode.pojo.Car">
        select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car where id=#{id};
    </select>

    <select id="selectAll" resultType="com.powernode.pojo.Car">
        select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car
    </select>

    <delete id="deleteById">
        delete from t_car where id=#{id};
    </delete>
</mapper>
```

```java
import com.powernode.mapper.CarMapper;
import com.powernode.pojo.Car;
import com.powernode.utils.SqlSessionUtil;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

/**
 * ClassName:  CarMapperTest
 * Description:
 * DateTime:   2025/12/18 11:18
 * Author: Tian
 */
public class CarMapperTest {
    @Test
    public void testInsert(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        Car car = new Car(null,"0001","红旗",50.00,"2025-09-03","燃油车");
        int count = mapper.insert(car);
        System.out.println("count = " + count);
        sqlSession.commit();
        sqlSession.close();
    }

    @Test
    public void testSelect(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        Car car = mapper.selectById(15L);
        System.out.println(car);
    }

    @Test
    public void testSelectAll(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        List<Car> cars = mapper.selectAll();
        cars.forEach(car -> {
            System.out.println(car);
        });
    }

    @Test
    public void testDeleteById(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        int count = mapper.deleteById(10L);
        System.out.println("count = " + count);
        sqlSession.commit();
        sqlSession.close();
    }

    @Test
    public void testUpdate(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        Car car = new Car(11L,"1111","比亚迪秦",20.0,"2020-12-15","新能源");
        int count = mapper.update(car);
        System.out.println("count = " + count);
        sqlSession.commit();
        sqlSession.close();
    }
}
```

> 可以发现，之前调用的都是`SqlSession`中的方法，现在调用的是`SqlMapper`中的方法。
>
> 其实`SqlMapper`最后调用的还是`SqlSession`中的方法。



## 九、MyBatis小技巧

### 9.1 `#{}`和`${}`

> `#{}`：先编译sql语句，再给占位符传值，底层是PreparedStatement实现。可以防止sql注入，比较常用。
> `${}`：先进行sql语句拼接，然后再编译sql语句，底层是Statement实现。存在sql注入现象。只有在需要进行sql语句关键字拼接的情况下才会用到。
>
> **优先使用`#{}`，这是原则，避免SQL注入的风险。**
>
> **使用`#{}`传的值是以单引号`'内容'`拼接到SQL语句中，使用`${}`传的值是直接将内容拼接到SQL语句中**。



**（推荐）使用`#{}`传值**

![image-20251218161800086](C:\Tian_File\Typora_images\typora-user-images\image-20251218161800086.png)

> 通过执行可以清楚的看到，sql语句中是带有`?`的，这个`?`就是大家在JDBC中的占位符，专门用来接收值的。把“燃油车”以`String`类型的值，传递给`?`。这就是`#{}`，它会先进行sql语句的预编译，然后再给占位符传值。



**使用`${}`传值**

![image-20251218162044163](C:\Tian_File\Typora_images\typora-user-images\image-20251218162044163.png)

![image-20251218162056955](C:\Tian_File\Typora_images\typora-user-images\image-20251218162056955.png)

> 出现了异常，可以看到"燃油车"这个字符串是直接拼接到SQL语句中的。很显然，${} 是先进行sql语句的拼接，然后再编译，出现语法错误是正常的。
>
> **解决方法：`'${}'` 在`${}`外面加单引号`''`，例如：`car_type = '${carType}'`**



#### 必须使用`${}`的情况

##### 1、SQL语句关键字拼接时

> 当需要进行sql语句关键字拼接的时候。必须使用`${}`，例如：向sql语句中注入`asc`或`desc`关键字，来完成数据的升序或降序排列。

**尝试使用`#{}`传值**

```xml
<select id="selectAll" resultType="com.powernode.mybatis.pojo.Car">
  select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car order by carNum #{key}
</select>
```

```java
@Test
public void testSelectAll(){
    CarMapper mapper = (CarMapper) SqlSessionUtil.openSession().getMapper(CarMapper.class);
    List<Car> cars = mapper.selectAll("desc");
    cars.forEach(car -> System.out.println(car));
}
```

![image-20251218162648447](C:\Tian_File\Typora_images\typora-user-images\image-20251218162648447.png)

> 报错的原因是sql语句不合法，最终sql语句会是这样：
> `select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car order by carNum 'desc'`
>
> `desc`是一个关键字，不能带单引号的，所以在进行sql语句关键字拼接的时候，必须使用`${}`

> 正确写法：

```xml
<select id="selectAll" resultType="com.powernode.mybatis.pojo.Car">
  select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car order by carNum ${key}
</select>
```



##### 2、拼接表名

>  现实业务当中，可能会存在分表存储数据的情况。因为一张表存的话，数据量太大。查询效率比较低。
>     可以将这些数据有规律的分表存储，这样在查询的时候效率就比较高。因为扫描的数据量变少了。
>     日志表：专门存储日志信息的。如果t_log只有一张表，这张表中每一天都会产生很多log，慢慢的，这个表中数据会很多。
>     怎么解决问题？
>         可以每天生成一个新表。每张表以当天日期作为名称，例如：
>             t_log_20220901
>             t_log_20220902
>             ....
>     你想知道某一天的日志信息怎么办？
>         假设今天是20220901，那么直接查：t_log_20220901的表即可。

**尝试使用`#{}`传值**

```xml
<select id="selectAllByTableName" resultType="User">
  select * from t_user#{time}
</select>
```

```java
@Test
public void testSelectAllByTableName(){
    UserMapper mapper = SqlSessionUtil.openSession().getMapper(UserMapper.class);
    List<User> cars = mapper.selectAllByTableName("时间");
    users.forEach(user -> System.out.println(user));
}
```

> 最终SQL语句是这样的：`select * from t_user'时间'`
>
> 而正确的SQL语句是：`select * from t_user时间`

```xml
<select id="selectAllByTableName" resultType="User">
  select * from t_user${time}
</select>
```



##### 3、批量删除

> 批量删除的SQL语句：or 和 in 的方式
>
> - `delete from t_car where id = 1 or id = 2 or id = 3;`
> - `delete from t_car where id in(1, 2, 3);`
>
> 假设现在使用in的方式处理，前端传过来的字符串：1, 2, 3

**尝试使用`#{}`传值**

```xml
<delete id="deleteBatch">
  delete from t_car where id in(#{ids})
</delete>
```

```java
@Test
public void testDeleteBatch(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    int count = mapper.deleteBatch("1,2,3");
    System.out.println("删除了几条记录：" + count);
    SqlSessionUtil.openSession().commit();
}
```

> 最终SQL语句是这样的：`delete from t_car where id in('1,2,3')`
>
> 而正确的SQL语句是：`delete from t_car where id in(1,2,3)`

> 正确写法：

```xml
<delete id="deleteBatch">
  delete from t_car where id in(${ids})
</delete>
```



##### 4、模糊查询

> 在进行模糊查询时，SQL语句是这样的：`select * from t_car where name like %奔驰%`;

**尝试使用`#{}`传值**

```xml
<select id="selectLikeByBrand" resultType="Car">
  select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car where brand like '%#{brand}%'
</select>
```

```java
@Test
public void testSelectLikeByBrand(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    List<Car> cars = mapper.selectLikeByBrand("奔驰");
    cars.forEach(car -> System.out.println(car));
}
```

> 最终SQL语句是：`select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car where brand like '%'奔驰''%'`
>
> SQL报错！！！



>解决方法：
>
>第一种：使用`#{}`，配合`concat`函数，`concat`函数，这个是`mysql`数据库当中的一个函数，专门进行字符串拼接。

```xml
<select id="selectLikeByBrand" resultType="Car">
  select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car where brand like concat('%',#{brand},'%')
</select>
```

![image-20251218201906427](C:\Tian_File\Typora_images\typora-user-images\image-20251218201906427.png)

> 第二种：使用双引号的方式

```xml
<select id="selectLikeByBrand" resultType="Car">
  select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car where brand like "%"#{brand}"%"
</select>
```

![image-20251218202618998](C:\Tian_File\Typora_images\typora-user-images\image-20251218202618998.png)

> （推荐使用）第三种：使用`${}`

```xml
<select id="selectLikeByBrand" resultType="Car">
  select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car where brand like '%${brand}%'
</select>
```

![image-20251218202601317](C:\Tian_File\Typora_images\typora-user-images\image-20251218202601317.png)



### 9.2 `typeAliases`起别名

> 可以观察到`SqlMapper.xml`文件中的`select`字段中的`resultType`属性（用来指定查询结果集的封装类型）每次都要写`pojo`类的全限定类名，比较繁琐。
>
> 可以尝试将其起别名，简化编写。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.powernode.mapper.CarMapper">  
    <select id="selectById" resultType="com.powernode.pojo.Car">
        select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car where id=#{id};
    </select>

    <select id="selectAll" resultType="com.powernode.pojo.Car">
        select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car
    </select>
</mapper>
```

> 在`mybatis-config.xml`文件中使用`typeAliases`标签来起别名，包括两种方式：



#### 第一种方式：`typeAlias`

> 起别名需要在`mybatis-config.xml`文件中编写起别名的**标签**。

```xml
<typeAliases>
    <!--type属性：pojo类的全限定类名，alias属性：简化类名-->
    <typeAlias type="com.powernode.pojo.Car" alias="Car"></typeAlias>
</typeAliases>
```

> 首先要注意`typeAliases`标签的放置位置，如果报错，大概率是标签顺序的原因。

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251218203535310.png" alt="image-20251218203535310" style="zoom:67%;" />

> `typeAliases`标签中的`typeAlias`可以写多个。
>
> `<typeAlias type="" alias="">`	`type`属性：指定给哪个类起别名，`alias`属性：别名。
>
> `alias`属性不是必须的，如果不写，`type`属性指定的类型名的简写作为别名。
>
> `alias`是不区分大小写的。
>
> **注意：SqlMapper.xml文件中的namespace是不可以起别名的**



#### 第二种方式：`package`（常用）

> 如果一个包下的类太多，每个类都要起别名，会导致`typeAlias`标签配置较多，所以`mybatis`用提供`package`的配置方式，只需要指定包名，该包下的所有类都自动起别名，别名就是简类名，并且别名不区分大小写。
>
> **`package`也可以配置多个**。

```xml
<typeAliases>
  <!--将这个包下的所有类全部自动起别名，别名就是简化类名，不区分大小写-->
  <package name="com.powernode.mybatis.pojo"/>
</typeAliases>
```



### 9.3 `mappers`

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251218204740547.png" alt="image-20251218204740547" style="zoom:67%;" />

> SQL映射文件的配置方式包括四种：
>
> - `resource`：从类路径中加载
> - `url`：从指定的全限定资源路径中加载
> - `class`：使用映射器接口实现类的完全限定类名
> - `package`：将包内的映射器接口实现全部注册为映射器



#### resource

> 这种方式是从**类路径中**加载配置文件，所以这种方式要求SQL映射文件必须放在resources目录下或其子目录下。

![image-20251218204919160](C:\Tian_File\Typora_images\typora-user-images\image-20251218204919160.png)

```xml
<mappers>
  <mapper resource="AuthorMapper.xml"/>
  <mapper resource="CarMapper.xml"/>
  <mapper resource="UserMapper.xml"/>
</mappers>
```



#### url（不建议）

> 这种方式显然使用了**绝对路径**的方式，这种配置对SQL映射文件存放的位置随意，但是移植性太差，不建议使用。

```xml
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
  <mapper url="file:///var/mappers/BlogMapper.xml"/>
  <mapper url="file:///var/mappers/PostMapper.xml"/>
</mappers>
```



#### class

> mapper标签的作用是指定`SqlMapper.xml`文件的路径，指定接口名有什么用呢？
>
> ```xml
> <mapper class="com.powernode.mybatis.mapper.CarMapper"/>
> ```
>
> 如果`class`指定是：`com.powernode.mybatis.mapper.CarMapper`
> 那么mybatis框架会自动去`com/powernode/mybatis/mapper`目录下查找`CarMapper.xml`文件。
> 注意：也就是说：如果你采用这种方式，那么你必须保证`CarMapper.xml`文件和`CarMapper接口`必须在同一个目录下。并且名字一致。
>             CarMapper接口-> CarMapper.xml
>             LogMapper接口-> LogMapper.xml

```xml
<!-- 使用class属性，要写成全限定类名 -->
<mappers>
  <mapper class="com.powernode.mybatis.mapper.CarMapper"/>
</mappers>
```

> 如果使用这种方式必须满足以下条件：
>
> - **SQL映射文件和mapper接口放在同一个目录下**。
> - **SQL映射文件的名字也必须和mapper接口名一致**。
>
> 如果使用`class`，则需要将`SqlMapper.xml`文件和`mapper`接口放在同一个包下，达到同级目录。

![image-20251218205328901](C:\Tian_File\Typora_images\typora-user-images\image-20251218205328901.png)

> 也可以在`resources`包中创建和`mapper`接口的同级目录，将`SqlMapper.xml`文件放入其中，从而达到和`mapper`接口同级目录。
>
> 原因：`java`包和`resources`包都是根目录。
>
> 注意：在`resources`包中创建目录不是`package`，而是`dir`，并且多级目录不是以“`.`”的形式分隔，而是以“`/`”的形式分隔。例如：`com/powernode/mapper`，而不是`com.powernode.mapper`。
>
> （图片中显示的多级目录之间是“`.`”分隔，但其实在创建时是以"`/`"分隔的）

![image-20251218205842427](C:\Tian_File\Typora_images\typora-user-images\image-20251218205842427.png)



#### package（常用）

> 如果`class`较多，可以使用这种`package`的方式，但前提条件和上一种方式一样。
>
> - **SQL映射文件和mapper接口放在同一个目录下**。
> - **SQL映射文件的名字也必须和mapper接口名一致**。

```xml
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="com.powernode.mybatis.mapper"/>
</mappers>
```



### 9.4 IDEA配置文件模板

> 由于经常使用`mybatis-config.xml`和`SqlMapper.xml`文件，可以在IDEA中提前创建好模板，以后通过模板创建配置文件会更加方便。

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251219144534723.png" alt="image-20251219144534723" style="zoom:67%;" />

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251218211216326.png" alt="image-20251218211216326" style="zoom:67%;" />



### 9.5 插入数据时获取自动生成的主键

> 前提：**主键是自动生成的**。
>
> 业务背景：一个用户有多个角色。

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251218212652006.png" alt="image-20251218212652006" style="zoom:67%;" />

> 插入一条新的记录之后，自动生成了主键，而这个主键需要在其他表中使用时。
> 插入一个用户数据的同时需要给该用户分配角色：需要将生成的用户的id插入到角色表的user_id字段上。
> 第一种方式：可以先插入用户数据，再写一条查询语句获取id，然后再插入user_id字段。【比较麻烦】
> 第二种方式：mybatis提供了一种方式更加便捷。如下：

```java
//接口方法：自定义方法名。
//作用：获取自动生成的主键
void insertUseGeneratedKeys(Car car);
```

```xml
<!--
	userGeneratedKeys="true" 作用：使用自动生成的主键值
	keyProperty="id" 作用：指定主键值赋值给对象的哪个属性。
-->
<!--指定主键值赋值给Car对象的id属性-->
<insert id="insertUseGeneratedKeys" useGeneratedKeys="true" keyProperty="id">
  insert into t_car(id,car_num,brand,guide_price,produce_time,car_type) values(null,#{carNum},#{brand},#{guidePrice},#{produceTime},#{carType})
</insert>
```

> **可以获取到自动生成的主键的值**。



## 十、MyBatis参数处理

**准备数据库表：`t_student`**

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251219143053875.png" alt="image-20251219143053875" style="zoom:67%;" />

> 导入依赖
>
> 创建`SqlMapper.xml`文件、`Logback.xml`文件、`mybatis-config.xml`文件
>
> 创建`pojo`包、`utils`包、`mapper`包



### 10.1 单个简单类型参数

> 简单类型包括：
>
> - `byte short int long float double char`
> - `Byte Short Integer Long Float Double Character`
> - `String`
> - `java.util.Date`
> - `java.sql.Date`

> 需求：根据`name`查、根据`id`查、根据`birth`查、根据`sex`查

```java
package com.powernode.mybatis.mapper;

import com.powernode.mybatis.pojo.Student;

import java.util.Date;
import java.util.List;

public interface StudentMapper {
    //根据name查询
    List<Student> selectByName(String name);

    //根据id查询
    Student selectById(Long id);

    //根据birth查询
    List<Student> selectByBirth(Date birth);

    //根据sex查询
    List<Student> selectBySex(Character sex);
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.powernode.mybatis.mapper.StudentMapper">
    <select id="selectByName" resultType="student">
        select * from t_student where name = #{name}
    </select>
    <select id="selectById" resultType="student">
        select * from t_student where id = #{id}
    </select>
    <select id="selectByBirth" resultType="student">
        select * from t_student where birth = #{birth}
    </select>
    <select id="selectBySex" resultType="student">
        select * from t_student where sex = #{sex}
    </select>
</mapper>
```

```java
package com.powernode.mybatis.test;

import com.powernode.mybatis.mapper.StudentMapper;
import com.powernode.mybatis.pojo.Student;
import com.powernode.mybatis.utils.SqlSessionUtil;
import org.junit.Test;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.List;

public class StudentMapperTest {

    StudentMapper mapper = SqlSessionUtil.openSession().getMapper(StudentMapper.class);

    @Test
    public void testSelectByName(){
        List<Student> students = mapper.selectByName("张三");
        students.forEach(student -> System.out.println(student));
    }
    @Test
    public void testSelectById(){
        Student student = mapper.selectById(2L);
        System.out.println(student);
    }
    @Test
    public void testSelectByBirth(){
        try {
            Date birth = new SimpleDateFormat("yyyy-MM-dd").parse("2022-08-16");
            List<Student> students = mapper.selectByBirth(birth);
            students.forEach(student -> System.out.println(student));
        } catch (ParseException e) {
            throw new RuntimeException(e);
        }

    }
    @Test
    public void testSelectBySex(){
        List<Student> students = mapper.selectBySex('男');
        students.forEach(student -> System.out.println(student));
    }
}
```

> 通过测试得知，**简单类型对于mybatis来说都是可以自动识别类型的**。
>
> mybatis是可以自动推断出`ps.setXxx()`方法的。`ps.setString()`还是`ps.setInt()`，它可以自动推断。

其实SQL映射文件中的配置比较完整的写法是：

```xml
<select id="selectByName" resultType="student" parameterType="java.lang.String">
  <!--javaType属性：java属性的类型	jdbcType属性：数据库表字段类型-->
  select * from t_student where name = #{name, javaType=String, jdbcType=VARCHAR}
</select>
```

> 其中sql语句中的`javaType`，`jdbcType`，以及`select`标签中的`parameterType`属性，都是用来帮助mybatis进行类型确定的。不过这些配置多数是可以省略的。因为mybatis它有强大的自动类型推断机制。
>
> - `javaType`：可以省略
> - `jdbcType`：可以省略
> - `parameterType`：可以省略，作用：告诉MyBatis框架，方法的参数类型是什么类型。
>
> **如果参数只有一个的话，#{} 里面的内容就随便写了。对于 ${} 来说，注意加单引号。**

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251220105109450.png" alt="image-20251220105109450" style="zoom: 80%;" />



### 10.2 Map参数

> 需求：根据`name`和`age`查询，将参数放到map集合中。

```java
//根据name和age查询
List<Student> selectByParamMap(Map<String,Object> paramMap);
```

```xml
<select id="selectByParamMap" resultType="student">
  select * from t_student where name = #{nameKey} and age = #{ageKey}
</select>
```

```java
@Test
public void testSelectByParamMap(){
    // 准备Map
    Map<String,Object> paramMap = new HashMap<>();
    paramMap.put("nameKey", "张三");
    paramMap.put("ageKey", 20);

    List<Student> students = mapper.selectByParamMap(paramMap);
    students.forEach(student -> System.out.println(student));
}
```

> **mybatis也是可以识别map集合的**。
>
> **这种方式是手动封装Map集合，将每个条件以key和value的形式存放到集合中。然后在使用的时候通过#{map集合的key}来取值。**



### 10.3 实体类参数（常用）

> 需求：插入一条Student数据

```java
int insert(Student student);
```

```xml
<insert id="insert">
  insert into t_student values(null,#{name},#{age},#{height},#{birth},#{sex})
</insert>
```

```java
@Test
public void testInsert(){
    Student student = new Student();
    student.setName("李四");
    student.setAge(30);
    student.setHeight(1.70);
    student.setSex('男');
    student.setBirth(new Date());
    int count = mapper.insert(student);
    SqlSessionUtil.openSession().commit();
}
```

> **需要注意的是：#{} 里面写的是属性名字，这个属性名其本质上是：set/get方法名去掉set/get之后的名字**。



### 10.4 多参数

> **多参数：1、通过Map集合实现多参数传值；2、SQL传值命名为arg或param（通过MyBatis底层实现Map集合）**
>
> 需求：通过name和sex查询

```java
//根据name和sex查询 
List<Student> selectByNameAndSex(String name, Character sex);
```

```xml
<select id="selectByNameAndSex" resultType="student">
  select * from t_student where name = #{name} and sex = #{sex}
</select>
```

```java
@Test
public void testSelectByNameAndSex(){
    List<Student> students = mapper.selectByNameAndSex("张三", '女');
    students.forEach(student -> System.out.println(student));
    sqlSession.close();
}
```

![image-20251219151801255](C:\Tian_File\Typora_images\typora-user-images\image-20251219151801255.png)

> 执行结果发生异常：name参数未找到，可用参数包括`[arg1, arg0, param1, param2]`
>
> 修改`StudentMapper.xml`配置文件：尝试使用`[arg1, arg0, param1, param2]`参数

```xml
<select id="selectByNameAndSex" resultType="student">
  <!--select * from t_student where name = #{name} and sex = #{sex}-->
  select * from t_student where name = #{arg0} and sex = #{arg1}
</select>
```

![image-20251219151938282](C:\Tian_File\Typora_images\typora-user-images\image-20251219151938282.png)

> 实现原理：**实际上在mybatis底层会创建一个map集合，以arg和param命名为key，方法传入的参数为value**，例如以下代码：
>
> ```java
> Map<String,Object> map = new HashMap<>();
> map.put("arg0", name);
> map.put("arg1", sex);
> map.put("param1", name);
> map.put("param2", sex);
> 
> // 所以可以这样取值：#{arg0} #{arg1} #{param1} #{param2}
> // 其本质就是#{map集合的key}
> ```
>
> **参数可以都使用arg或者都使用param，但是需要注意的是arg是从0开始，param是从1开始的。当然arg和param可以混着用。**
>
> - arg0 是第一个参数
> - arg1 是第二个参数
> - param1是第一个参数
> - param2是第二个参数
>
> **在低版本的MyBatis中，用#{0}和#{1}这种形式，高版本的MyBatis采用上面的方式。**



### 10.5 `@Param注解(命名参数)`

> 多个参数的时候，可以不采用arg和param，也可以不使用map集合，可以自定义参数。
>
> **使用@Param注解**，可以增强可读性。

> 需求：根据name和age查询

```java
//注解中的value可以省略不写
List<Student> selectByNameAndAge(@Param(value="name") String name, @Param("age") int age);
```

```xml
<select id="selectByNameAndAge" resultType="student">
  select * from t_student where name = #{name} and age = #{age}
</select>
```

```java
@Test
public void testSelectByNameAndAge(){
    List<Student> stus = mapper.selectByNameAndAge("张三", 20);
    stus.forEach(student -> System.out.println(student));
}
```

> **@Param注解底层其实是一个Map集合，注解中的value值相当于Map集合的key**



### 10.6 `@Param`注解源码解析

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251220103752278.png" alt="image-20251220103752278" style="zoom:67%;" />

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251219153029295.png" alt="image-20251219153029295" style="zoom: 67%;" />

> 通过源码分析可以看出，MyBatis内部的多参数`arg`和`param`，只剩下`param`，`arg`失效了。
>
> 这是因为，使用**@Param注解**其实是将**Map集合**中的**arg参数**替换为了**自定义的参数名称**，但是**Map集合中的param参数还存在着**。
>
> 图片中方法的参数**Object[] args**数组中存储的就是接口方法中的参数。
>
> 简单理解：mybatis底层实现是使用map集合，将注解中的value值作为map集合的key值，map集合中的value值对应传入的参数值。

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251220104706063.png" alt="image-20251220104706063" style="zoom:67%;" />



## 十一、MyBatis查询语句专题

> 前期准备：
>
> 打包方式：`jar`
> 引入依赖：mysql驱动依赖、mybatis依赖、logback依赖、junit依赖。
> 引入配置文件：`jdbc.properties`、`mybatis-config.xml`、`logback.xml`
> 创建`pojo`类：`Car`
> 创建`Mapper`接口：`CarMapper`
> 创建`Mappe`r接口对应的映射文件：`com/powernode/mybatis/mapper/CarMapper.xml`
> 创建单元测试：`CarMapperTest`
> 拷贝工具类：`SqlSessionUtil`



### 11.1 返回Car

> 当查询结果是实体类，并且查询结果只有一条时：

```java
package com.powernode.mybatis.mapper;

import com.powernode.mybatis.pojo.Car;

public interface CarMapper {

    /**
     * 根据id主键查询：结果最多只有一条
     */
    Car selectById(Long id);
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.powernode.mybatis.mapper.CarMapper">
    <select id="selectById" resultType="Car">
        select id,car_num carNum,brand,guide_price guidePrice,produce_time produceTime,car_type carType from t_car where id = #{id}
    </select>
</mapper>
```

```java
package com.powernode.mybatis.test;

import com.powernode.mybatis.mapper.CarMapper;
import com.powernode.mybatis.pojo.Car;
import com.powernode.mybatis.utils.SqlSessionUtil;
import org.junit.Test;

public class CarMapperTest {

    @Test
    public void testSelectById(){
        CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
        Car car = mapper.selectById(35L);
        System.out.println(car);
    }
}
```

> 返回结果只有一条数据，也可以使用List集合接收。



### 11.2 返回`List<Car>`

> 当查询的记录条数是多条的时候，必须使用集合接收。如果使用单个实体类接收会出现异常。

```java
/**
* 查询所有的Car
* @return
*/
List<Car> selectAll();
```

```xml
<select id="selectAll" resultType="Car">
  select id,car_num carNum,brand,guide_price guidePrice,produce_time produceTime,car_type carType from t_car
</select>
```

```java
@Test
public void testSelectAll(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    List<Car> cars = mapper.selectAll();
    cars.forEach(car -> System.out.println(car));
}
```

> 如果返回结果为多条数据，但是使用单个实体类接收，会报错。

![image-20251219161403482](C:\Tian_File\Typora_images\typora-user-images\image-20251219161403482.png)



### 11.3 返回Map

> 当返回的数据，没有合适的实体类对应的话，可以采用Map集合接收。字段名做key，字段值做value。
> 查询如果可以保证只有一条数据，则返回一个Map集合即可。

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251219161520992.png" alt="image-20251219161520992" style="zoom: 67%;" />

```java
/**
 * 通过id查询一条记录，返回Map集合
 * @param id
 * @return
 */
Map<String, Object> selectByIdRetMap(Long id);
```

```xml
<!--resultMap="map"，这是因为mybatis内置了很多别名-->
<select id="selectByIdRetMap" resultType="map">
  select id,car_num carNum,brand,guide_price guidePrice,produce_time produceTime,car_type carType from t_car where id = #{id}
</select>
```

```java
@Test
public void testSelectByIdRetMap(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    Map<String,Object> car = mapper.selectByIdRetMap(35L);
    System.out.println(car);
}
```

> **resultMap="map"，这是因为mybatis内置了很多别名。【参见mybatis开发手册】**
>
> 由此可见，一个数据可以使用Map集合接收，多个集合可以使用`List<Map<String,Object>>`的形式接收。
>
> 如果返回的不是一条记录，是多条记录的话，只采用单个Map集合接收，这样同样会出现之前的异常：**TooManyResultsException**



### 11.4 返回`List<Map<String,Object>>`

> 查询结果条数 ≥ 1条数据，则可以返回一个存储Map集合的List集合。`List<Map>`等同于`List<Car>`

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251219162616910.png" alt="image-20251219162616910" style="zoom:67%;" />

```java
/**
 * 查询所有的Car，返回一个List集合，List集合中存储的是Map集合。
 * @return
 */
List<Map<String,Object>> selectAllRetListMap();
```

```xml
<select id="selectAllRetListMap" resultType="map">
  select id,car_num carNum,brand,guide_price guidePrice,produce_time produceTime,car_type carType from t_car
</select>
```

```java
@Test
public void testSelectAllRetListMap(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    List<Map<String,Object>> cars = mapper.selectAllRetListMap();
    System.out.println(cars);
}
```

> 执行结果：
>
> ```json
> [
>   {carType=燃油车, carNum=103, guidePrice=50.30, produceTime=2020-10-01, id=33, brand=奔驰E300L}, 
>   {carType=电车, carNum=102, guidePrice=30.23, produceTime=2018-09-10, id=34, brand=比亚迪汉}, 
>   {carType=燃油车, carNum=103, guidePrice=50.30, produceTime=2020-10-01, id=35, brand=奔驰E300L}, 
>   {carType=燃油车, carNum=103, guidePrice=33.23, produceTime=2020-10-11, id=36, brand=奔驰C200},
>   ......
> ]
> ```



### 11.5 返回`Map<String,Map>`需要使用`@MapKey`注解标注大Map的key

> 使用Car的id做key，以后取出对应的Map集合时更方便。

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251219162930725.png" alt="image-20251219162930725" style="zoom:67%;" />

```java
/**
 * 获取所有的Car，返回一个大Map集合。
 * 大Map集合的key是Car的id。
 * 大Map集合的value是封装对应Car的Map集合。
 * @return
 */
@MapKey("id")
Map<Long,Map<String,Object>> selectAllRetMap();
```

```xml
<select id="selectAllRetMap" resultType="map">
  select id,car_num carNum,brand,guide_price guidePrice,produce_time produceTime,car_type carType from t_car
</select>
```

```java
@Test
public void testSelectAllRetMap(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    Map<Long,Map<String,Object>> cars = mapper.selectAllRetMap();
    System.out.println(cars);
}
```

> 执行结果：

```json
{
    64={carType=燃油车, carNum=133, guidePrice=50.30, produceTime=2020-01-10, id=64, brand=丰田霸道}, 
    66={carType=燃油车, carNum=133, guidePrice=50.30, produceTime=2020-01-10, id=66, brand=丰田霸道}, 
    67={carType=燃油车, carNum=133, guidePrice=50.30, produceTime=2020-01-10, id=67, brand=丰田霸道}, 
    69={carType=燃油车, carNum=133, guidePrice=50.30, produceTime=2020-01-10, id=69, brand=丰田霸道},
    ......
}
```



### 11.6 `resultMap`结果映射

> 在前面章节中，为了保证Select语句查询的结果能完全封装到`resultType`设置的`pojo`类中，对其Select语句中的字段名起别名。
>
> Select查询操作时，查询的表字段名和java对象的属性名对应不上，当时是使用`as`关键字对SQL语句起别名对应java对象的属性名。
>
> 现在有三种方式，可以解决这个问题：
>
> - 第一种方式：`as`给列起别名
> - 第二种方式：使用`resultMap`进行结果映射
> - 第三种方式：是否**开启驼峰命名自动映射**（**配置`settings`**）



#### 第一种方式：`as`给列起别名

```xml
<select id="selectAll" resultType="Car">
    select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car
</select>
```



#### 第二种方式：使用`resultMap`进行结果映射

> 专门定义一个结果映射（`resultMap`），在这个结果映射当中指定数据库表的字段名和Java类的属性名的对应关系。
>
> `<resultMap id="" type="">`
>
> - `id`属性：指定`resultMap`的唯一标识，这个`id`要在`select`标签中的`resultMap`属性中使用。
>
> - `type`属性：用来指定`POJO`类的类名。

```java
/**
 * 查询所有Car，使用resultMap进行结果映射
 * @return
 */
List<Car> selectAllByResultMap();
```

```xml
<!--
    resultMap:
      id：这个结果映射的标识，作为select标签的resultMap属性的值。
      type：结果集要映射的类。可以使用别名。
-->
<resultMap id="carResultMap" type="car">
  <!--表中的主键，建议配置一个id标签，官方解释是：为了提高mybatis的性能。建议主键这么写。-->
  <id property="id" column="id"/>
    
  <result property="carNum" column="car_num"/>
  <!--当属性名和数据库列名一致时，可以省略。但建议都写上。-->
    
  <!--javaType用来指定属性类型。jdbcType用来指定列类型。一般可以省略。-->
  <result property="brand" column="brand" javaType="string" jdbcType="VARCHAR"/>
  <result property="guidePrice" column="guide_price"/>
  <result property="produceTime" column="produce_time"/>
  <result property="carType" column="car_type"/>
</resultMap>

<!--resultMap属性的值必须和resultMap标签中id属性值一致。-->
<select id="selectAllByResultMap" resultMap="carResultMap">
  select * from t_car
</select>
```

```java
@Test
public void testSelectAllByResultMap(){
    CarMapper carMapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    List<Car> cars = carMapper.selectAllByResultMap();
    System.out.println(cars);
}
```



#### （常用）第三种方式：是否开启驼峰命名自动映射（配置`settings`）

> 使用这种方式其实就是要求命名规范，属性名遵循Java的命名规范，数据库表的列名遵循SQL的命名规范。
>
> Java命名规范：首字母小写，后面每个单词首字母大写，遵循驼峰命名方式。
>
> SQL命名规范：全部小写，单词之间采用下划线分割。

比如以下的对应关系：

| **实体类中的属性名** | **数据库表的列名** |
| -------------------- | ------------------ |
| carNum               | car_num            |
| carType              | car_type           |
| produceTime          | produce_time       |

如何启用该功能，在`mybatis-config.xml`文件中进行配置：

```xml
<!--放在properties标签后面-->
<settings>
  <setting name="mapUnderscoreToCamelCase" value="true"/>
</settings>
```

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251220103341364.png" alt="image-20251220103341364" style="zoom:67%;" />

```java
/**
* 查询所有Car，启用驼峰命名自动映射
* @return
*/
List<Car> selectAllBymapUnderscoreToCamelCase();
```

```xml
<select id="selectAllByMapUnderscoreToCamelCase" resultType="Car">
  select * from t_car
</select>
```

```java
@Test
public void testSelectAllByMapUnderscoreToCamelCase(){
    CarMapper carMapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    List<Car> cars = carMapper.selectAllByMapUnderscoreToCamelCase();
    System.out.println(cars);
}
```



### 11.7 返回总记录条数

> 需求：查询总记录条数

```java
/**
 * 获取总记录条数
 * @return
 */
Long selectTotal();
```

```xml
<!--long是别名，可参考mybatis开发手册-->
<select id="selectTotal" resultType="long">
  select count(*) from t_car
</select>
```

```java
@Test
public void testSelectTotal(){
    CarMapper carMapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    Long total = carMapper.selectTotal();
    System.out.println(total);
}
```

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251220163237135.png" alt="image-20251220163237135" style="zoom:50%;" />

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251220163249773.png" alt="image-20251220163249773" style="zoom: 50%;" />

> **完整代码**：

> **CarMapper接口**

```java
package com.powernode.mapper;

import com.powernode.pojo.Car;
import org.apache.ibatis.annotations.MapKey;

import java.util.List;
import java.util.Map;

/**
 * ClassName:  CarMapper
 * Description:
 * DateTime:   2025/12/20 14:20
 * Author: Tian
 */
public interface CarMapper {
    /**
     * 根据id查找，返回结果只有一个数据
     * @param id
     * @return Car
     */
    //Car selectById(Long id);
    List<Car> selectById(Long id);

    /**
     * 查询所有Car
     * @return list
     */
    List<Car> selectAll();

    /**
     * 根据id查询将数据存放到map集合
     * @param id
     * @return map
     */
    Map<String,Object> selectByRetMap(Long id);

    /**
     * 查询所有数据并放到List集合中，每条数据存放在Map集合中
     * @return map
     */
    List<Map<String,Object>> selectAllRetListMap();

    /**
     * 使用一个大Map接收所有的数据，大Map的key值存放的是Car的id，大Map的value值存放的是对应id的Car的信息
     * @return map
     */
    @MapKey("id")
    Map<Long,Map<String,Object>> selectAllRetMap();

    /**
     * 查询所有的Car，使用resultMap进行结果映射
     * @return car
     */
    List<Car> selectAllByResultMap();

    /**
     * 在mybatis-config.xml文件中配置settings标签设置Map驼峰命名映射
     * @return car
     */
    List<Car> selectAllByMapUnderscoreToCamelCase();

    /**
     * 获取总记录条数
     * @return long
     */
    Long selectTotal();
}
```

> **CarMapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.powernode.mapper.CarMapper">
    <select id="selectById" resultType="Car">
        select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car where id = #{id}
    </select>

    <select id="selectAll" resultType="Car">
        select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car
    </select>

    <select id="selectByRetMap" resultType="map">
        select id,car_num as CarNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car where id = #{id}
    </select>

    <select id="selectAllRetListMap" resultType="map">
        select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car
    </select>

    <select id="selectAllRetMap" resultType="map">
        select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType from t_car
    </select>

    <resultMap id="carResultMap" type="car">
        <id property="id" column="id"/>
        <result property="carNum" column="car_num"></result>
        <result property="brand" column="brand"></result>
        <result property="guidePrice" column="guide_price"></result>
        <result property="produceTime" column="produce_time"></result>
        <result property="carType" column="car_type"></result>
    </resultMap>
    <select id="selectAllByResultMap" resultMap="carResultMap">
        select * from t_car
    </select>

    <select id="selectAllByMapUnderscoreToCamelCase" resultType="car">
        select * from t_car
    </select>

    <select id="selectTotal" resultType="long">
        select count(*) from t_car
    </select>
</mapper>
```

> **CarMapperTest**

```java
import com.powernode.mapper.CarMapper;
import com.powernode.pojo.Car;
import com.powernode.utils.SqlSessionUtil;
import org.junit.Test;

import java.util.List;
import java.util.Map;

/**
 * ClassName:  CarMapperTest
 * Description:
 * DateTime:   2025/12/20 14:21
 * Author: Tian
 */
public class CarMapperTest {
    private CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);

    /**
     * 返回Car
     */
    @Test
    public void testSelectById(){
        //Car car = mapper.selectById(15L);
        List<Car> cars = mapper.selectById(15L);
        cars.forEach(car -> {
            System.out.println(car);
        });
    }

    /**
     * 返回List<Car>
     */
    @Test
    public void testSelectAll(){
        List<Car> cars = mapper.selectAll();
        cars.forEach(car -> {
            System.out.println(car);
        });
    }

    /**
     * 返回Map
     */
    @Test
    public void testSelectByRetMap(){
        Map<String,Object> carMap = mapper.selectByRetMap(15L);
        System.out.println(carMap);
    }

    /**
     * 返回listMap
     */
    @Test
    public void testSelectAllRetListMap(){
        List<Map<String,Object>> listMap = mapper.selectAllRetListMap();
        /*listMap.forEach(map -> {
            System.out.println(map);
        });*/
        System.out.println(listMap);
    }

    /**
     * 返回大Map
     */
    @Test
    public void testSelectAllRetMap(){
        Map<Long,Map<String,Object>> maps = mapper.selectAllRetMap();
        System.out.println(maps);
    }

    /**
     * 使用resultMap标签映射类属性名和表字段名
     */
    @Test
    public void testSelectAllByResultMap(){
        List<Car> cars = mapper.selectAllByResultMap();
        cars.forEach(car -> {
            System.out.println(car);
        });
    }

    /**
     * 在mybatis-config.xml文件中配置settings标签设置Map驼峰命名映射
     */
    @Test
    public void testSelectAllByMapUnderscoreToCamelCase(){
        List<Car> cars = mapper.selectAllByMapUnderscoreToCamelCase();
        cars.forEach(car -> {
            System.out.println(car);
        });
    }

    /**
     * 获取总记录条数
     */
    @Test
    public void testSelectTotal(){
        Long count = mapper.selectTotal();
        System.out.println("总记录条数：" + count);
    }
}
```

> **mybatis-config.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <!--配置文件-->
    <properties resource="jdbc.properties"/>

    <!--设置驼峰命名自动映射-->
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

    <!--给SQL映射的resultType起别名-->
    <typeAliases>
        <package name="com.powernode.pojo"/>
    </typeAliases>

    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <package name="com.powernode.mapper"/>
    </mappers>
</configuration>
```



## 十二、动态SQL

> 有些业务场景，也需要SQL语句进行动态拼接。比如：批量删除、批量插入、多条件查询

```sql
select * from t_car where brand like '丰田%' and guide_price > 30 and .....;
```

> **下面学习的标签，其实都是给SQL条件增加了一个过滤，满足条件SQL条件才能执行，否则不执行SQL条件。**
>
> **mybatis的动态SQL当中，不能使用&&，只能使用and。**



### 12.1 if标签

> **给if标签当做if语句进行使用，if标签中的test属性就相当于条件判断**
>
> **test属性中写表达式时，如果传入参数时使用了@Param注解，则表达式中使用参数时，便可使用注解的value值。如果没有使用@Param注解，需要写成规定的参数名称arg或param。如果参数传入的是POJO类，则表达式中填写POJO类的属性名。例如：car.brand**
>
> **if标签中test是必须的，test属性的值必须是true或false**
>
> 需求：多条件查询。
> 可能的条件包括：品牌（brand）、指导价格（guide_price）、汽车类型（car_type）
>
> 使用方法：
>
> ```xml
> <!--test中可以写条件判断语句-->
> <if test="">
>     <!--若条件判断为true，则执行if标签中的代码-->
> </if>
> ```

```java
/**
 * 根据多条件查询Car
 * @param brand
 * @param guidePrice
 * @param carType
 * @return
 */
List<Car> selectByMultiCondition(@Param("brand") String brand, @Param("guidePrice") Double guidePrice, @Param("carType") String carType);
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.powernode.mybatis.mapper.CarMapper">

    <select id="selectByMultiCondition" resultType="car">
        select * from t_car where
        <if test="brand != null and brand != ''">
            brand like #{brand}"%"
        </if>
        <if test="guidePrice != null and guidePrice != ''">
            and guide_price >= #{guidePrice}
        </if>
        <if test="carType != null and carType != ''">
            and car_type = #{carType}
        </if>
    </select>

</mapper>
```

```java
@Test
public void testSelectByMultiCondition(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    List<Car> cars = mapper.selectByMultiCondition("丰田", 20.0, "燃油车");
    System.out.println(cars);
}
```

![image-20251220201853760](C:\Tian_File\Typora_images\typora-user-images\image-20251220201853760.png)

> 如果第一个条件为空，剩下两个条件不为空。
>
> ```java
> List<Car> cars = mapper.selectByMultiCondition("", 20.0, "燃油车");
> ```

![image-20251220201930802](C:\Tian_File\Typora_images\typora-user-images\image-20251220201930802.png)

> 报错了，SQL语句有问题，where后面出现了and，这是由于SQL语句拼接的原因。
>
> **解决方案：where后面添加一个恒成立的条件**

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251220202059274.png" alt="image-20251220202059274" style="zoom: 67%;" />

![image-20251220202117037](C:\Tian_File\Typora_images\typora-user-images\image-20251220202117037.png)

> 如果三个条件都是空。
>
> ```java
> List<Car> cars = mapper.selectByMultiCondition("", null, "");
> ```

![image-20251220202152443](C:\Tian_File\Typora_images\typora-user-images\image-20251220202152443.png)

> 三个条件都不为空。
>
> ```java
> List<Car> cars = mapper.selectByMultiCondition("丰田", 20.0, "燃油车");
> ```

![image-20251220202232836](C:\Tian_File\Typora_images\typora-user-images\image-20251220202232836.png)



### 12.2 where标签

> where标签的作用：让where子句更加动态智能。
>
> - 所有条件都为空时，where标签保证**不会生成where子句**。
> - 自动去除某些条件**前面**多余的and或or。
>
> **where标签可以包裹if标签使用，类似与Java语法**
>
> **where标签只可以自动去除前面多余的and或or，不可以自动去除后面多余的and和or。**
>
> 需求：继续使用if标签中的需求。

```java
/**
* 根据多条件查询Car，使用where标签
* @param brand
* @param guidePrice
* @param carType
* @return
*/
List<Car> selectByMultiConditionWithWhere(@Param("brand") String brand, @Param("guidePrice") Double guidePrice, @Param("carType") String carType);
```

```xml
<select id="selectByMultiConditionWithWhere" resultType="car">
  select * from t_car
  <where>
    <if test="brand != null and brand != ''">
      and brand like #{brand}"%"
    </if>
    <if test="guidePrice != null and guidePrice != ''">
      and guide_price >= #{guidePrice}
    </if>
    <if test="carType != null and carType != ''">
      and car_type = #{carType}
    </if>
  </where>
</select>
```

```java
@Test
public void testSelectByMultiConditionWithWhere(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    List<Car> cars = mapper.selectByMultiConditionWithWhere("丰田", 20.0, "燃油车");
    System.out.println(cars);
}
```

![image-20251220202954923](C:\Tian_File\Typora_images\typora-user-images\image-20251220202954923.png)

> 如果所有条件都是空
>
> ```java
> List<Car> cars = mapper.selectByMultiConditionWithWhere("", null, "");
> ```

![image-20251220203030981](C:\Tian_File\Typora_images\typora-user-images\image-20251220203030981.png)

> 它可以自动去掉前面多余的and，也可以自动去掉前面多余的or。

```java
List<Car> cars = mapper.selectByMultiConditionWithWhere("丰田", 20.0, "燃油车");
```

```xml
<select id="selectByMultiConditionWithWhere" resultType="car">
  select * from t_car
  <where>
    <if test="brand != null and brand != ''">
      or brand like #{brand}"%"
    </if>
    <if test="guidePrice != null and guidePrice != ''">
      and guide_price >= #{guidePrice}
    </if>
    <if test="carType != null and carType != ''">
      and car_type = #{carType}
    </if>
  </where>
</select>
```

![image-20251220203201771](C:\Tian_File\Typora_images\typora-user-images\image-20251220203201771.png)

> 注意：where标签**不会**自动去除**后面**多余的and或or。



### 12.3 trim标签

>trim标签的属性：
>
>- `prefix`：在trim标签中的语句前**添加**内容
>- `suffix`：在trim标签中的语句后**添加**内容
>- `prefixOverrides`：前缀**覆盖掉（去掉）**
>- `suffixOverrides`：后缀**覆盖掉（去掉）**
>
>**trim标签是对内部语句进行操作的，trim标签会自动判断，如果加了前缀，但是trim标签内部没有语句，则不会加前缀。**

```java
/**
* 根据多条件查询Car，使用trim标签
* @param brand
* @param guidePrice
* @param carType
* @return
*/
List<Car> selectByMultiConditionWithTrim(@Param("brand") String brand, @Param("guidePrice") Double guidePrice, @Param("carType") String carType);
```

```xml
<select id="selectByMultiConditionWithTrim" resultType="car">
  select * from t_car
  <trim prefix="where" suffixOverrides="and|or">
    <if test="brand != null and brand != ''">
      brand like #{brand}"%" and
    </if>
    <if test="guidePrice != null and guidePrice != ''">
      guide_price >= #{guidePrice} and
    </if>
    <if test="carType != null and carType != ''">
      car_type = #{carType}
    </if>
  </trim>
</select>
```

```java
@Test
public void testSelectByMultiConditionWithTrim(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    List<Car> cars = mapper.selectByMultiConditionWithTrim("丰田", 20.0, "");
    System.out.println(cars);
}
```

![image-20251220203503025](C:\Tian_File\Typora_images\typora-user-images\image-20251220203503025.png)

> 如果所有条件为空，where会被加上吗？
>
> 答案：不会。trim标签会自动判断，如果加了前缀，但是trim标签内部没有语句，则不会加前缀。
>
> ```java
> List<Car> cars = mapper.selectByMultiConditionWithTrim("", null, "");
> ```

![image-20251220203830029](C:\Tian_File\Typora_images\typora-user-images\image-20251220203830029.png)



### 12.4 set标签

> 主要使用在update语句当中，用来生成set关键字，**同时去掉最后多余的逗号“,”**。
>
> **set标签只更新提交的不为空的字段，如果提交的数据是空或者""，那么这个字段将不更新**。

```java
/**
* 更新信息，使用set标签
* @param car
* @return
*/
int updateWithSet(Car car);
```

```xml
<update id="updateWithSet">
  update t_car
  <set>
    <if test="carNum != null and carNum != ''">car_num = #{carNum},</if>
    <if test="brand != null and brand != ''">brand = #{brand},</if>
    <if test="guidePrice != null and guidePrice != ''">guide_price = #{guidePrice},</if>
    <if test="produceTime != null and produceTime != ''">produce_time = #{produceTime},</if>
    <if test="carType != null and carType != ''">car_type = #{carType},</if>
  </set>
  where id = #{id}
</update>
```

```java
@Test
public void testUpdateWithSet(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    Car car = new Car(38L,"1001","丰田霸道2",10.0,"",null);
    int count = mapper.updateWithSet(car);
    System.out.println(count);
    SqlSessionUtil.openSession().commit();
}
```

![image-20251220204118088](C:\Tian_File\Typora_images\typora-user-images\image-20251220204118088.png)



### 12.5 choose when otherwise

> `choose when otherwise`这三个标签是在一起使用的。
>
> **按照顺序匹配，哪个条件成立执行哪个，没有条件成立，就执行otherwise中的SQL语句**

```xml
<choose>
  <when>   </when>
  <when>   </when>
  <when>   </when>
  <otherwise>   </otherwise>
</choose>
```

> 等同于：`if elseif else`

```java
if(){
    
}else if(){
    
}else if(){
    
}else if(){
    
}else{

}
```

> 注意：**只有一个分支会被选择**，如果没有匹配项，就会执行`otherwise`分支。



> 需求：先根据品牌查询，如果没有提供品牌，再根据指导价格查询，如果没有提供指导价格，就根据生产日期查询。

```java
/**
* 使用choose when otherwise标签查询
* @param brand
* @param guidePrice
* @param produceTime
* @return
*/
List<Car> selectWithChoose(@Param("brand") String brand, @Param("guidePrice") Double guidePrice, @Param("produceTime") String produceTime);
```

```xml
<select id="selectWithChoose" resultType="car">
  select * from t_car
  <where>
    <choose>
      <when test="brand != null and brand != ''">
        brand like #{brand}"%"
      </when>
      <when test="guidePrice != null and guidePrice != ''">
        guide_price >= #{guidePrice}
      </when>
      <otherwise>
        produce_time >= #{produceTime}
      </otherwise>
    </choose>
  </where>
</select>
```

```java
@Test
public void testSelectWithChoose(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    //List<Car> cars = mapper.selectWithChoose("丰田霸道", 20.0, "2000-10-10");
    //List<Car> cars = mapper.selectWithChoose("", 20.0, "2000-10-10");
    //List<Car> cars = mapper.selectWithChoose("", null, "2000-10-10");
    List<Car> cars = mapper.selectWithChoose("", null, "");
    System.out.println(cars);
}
```

![image-20251220204545991](C:\Tian_File\Typora_images\typora-user-images\image-20251220204545991.png)



### 12.6 foreach标签

> 使用方法：
>
> ```xml
> <!--
> 	collection：集合或数组
> 	item：集合或数组中的元素
> 	separator：分隔符
> 	open： foreach循环拼接的所有sql语句的最前面以什么开始
> 	close：foreach循环拼接的所有sql语句的最后面以什么结束
> -->
> <!--collection：传入参数集合   item：集合中的单个元素（自定义名称）	separator：为每个item后面添加的分隔符并自动填充空格	open和close是给foreach标签填写的内容外面以什么开始和结束-->
> <foreach collection="" item="" separator="" open="" close="">
> 
> </foreach>
> ```
>
> 
>
> 循环数组或集合，动态生成sql，比如这样的SQL：

```sql
## 批量删除
delete from t_car where id in(1,2,3);
delete from t_car where id = 1 or id = 2 or id = 3;
```

```sql
## 批量插入
insert into t_car values
  (null,'1001','凯美瑞',35.0,'2010-10-11','燃油车'),
  (null,'1002','比亚迪唐',31.0,'2020-11-11','新能源'),
  (null,'1003','比亚迪宋',32.0,'2020-10-11','新能源')
```



#### 批量删除（in关键字）

```java
/**
* 通过foreach完成批量删除
* @param ids
* @return
*/
int deleteBatchByForeach(@Param("ids") Long[] ids);
```

```xml
<!--
	collection：集合或数组
	item：集合或数组中的元素
	separator：分隔符
	open：foreach标签中所有内容的开始
	close：foreach标签中所有内容的结束
-->
<delete id="deleteBatchByForeach">
  delete from t_car where id in
  <foreach collection="ids" item="id" separator="," open="(" close=")">
    #{id}
  </foreach>
</delete>
```

```java
@Test
public void testDeleteBatchByForeach(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    int count = mapper.deleteBatchByForeach(new Long[]{40L, 41L, 42L});
    System.out.println("删除了几条记录：" + count);
    SqlSessionUtil.openSession().commit();
}
```

![image-20251220205021233](C:\Tian_File\Typora_images\typora-user-images\image-20251220205021233.png)



#### 批量删除（or关键字）

```java
/**
* 通过foreach完成批量删除
* @param ids
* @return
*/
int deleteBatchByForeach2(@Param("ids") Long[] ids);
```

```xml
<delete id="deleteBatchByForeach2">
  delete from t_car where
  <foreach collection="ids" item="id" separator="or">
    id = #{id}
  </foreach>
</delete>
```

```java
@Test
public void testDeleteBatchByForeach2(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    int count = mapper.deleteBatchByForeach2(new Long[]{40L, 41L, 42L});
    System.out.println("删除了几条记录：" + count);
    SqlSessionUtil.openSession().commit();
}
```

![image-20251220205201673](C:\Tian_File\Typora_images\typora-user-images\image-20251220205201673.png)



#### 批量添加

```java
/**
* 批量添加，使用foreach标签
* @param cars
* @return
*/
int insertBatchByForeach(@Param("cars") List<Car> cars);
```

```xml
<insert id="insertBatchByForeach">
  insert into t_car values 
  <foreach collection="cars" item="car" separator=",">
    (null,#{car.carNum},#{car.brand},#{car.guidePrice},#{car.produceTime},#{car.carType})
  </foreach>
</insert>
```

```java
@Test
public void testInsertBatchByForeach(){
    CarMapper mapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    Car car1 = new Car(null, "2001", "兰博基尼", 100.0, "1998-10-11", "燃油车");
    Car car2 = new Car(null, "2001", "兰博基尼", 100.0, "1998-10-11", "燃油车");
    Car car3 = new Car(null, "2001", "兰博基尼", 100.0, "1998-10-11", "燃油车");
    List<Car> cars = Arrays.asList(car1, car2, car3);
    int count = mapper.insertBatchByForeach(cars);
    System.out.println("插入了几条记录" + count);
    SqlSessionUtil.openSession().commit();
}
```

![image-20251220205303157](C:\Tian_File\Typora_images\typora-user-images\image-20251220205303157.png)



### 12.7 sql标签和include标签

> sql标签和include标签一起使用。
>
> sql标签用来声明sql片段。简单来说：**sql标签**相当于**工具类**，将SQL语句封装。
>
> include标签用来将声明的sql片段包含到某个sql语句当中。简单来说：**include标签**相当于**调用工具类**，调用封装的SQL语句。
>
> 作用：代码复用，易维护。

```xml
<sql id="carCols">
    id,car_num carNum,brand,guide_price guidePrice,produce_time produceTime,car_type carType
</sql>

<select id="selectAllRetMap" resultType="map">
  select 
    <include refid="carCols"/> 
    from t_car
</select>

<select id="selectByIdRetMap" resultType="map">
  select 
    <include refid="carCols"/> 
    from t_car where id = #{id}
</select>
```

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251221135808723.png" alt="image-20251221135808723" style="zoom:67%;" />



> **完整代码：**

**CarMapper接口**

```java
package com.powernode.mapper;

import com.powernode.pojo.Car;
import org.apache.ibatis.annotations.Param;

import java.util.List;

public interface CarMapper {
    /**
     * 根据brand、guidePrice、carType进行联合查找
     * @param brand
     * @param guidePrice
     * @param carType
     * @return list
     */
    List<Car> selectByMultiCondition(@Param("brand") String brand,@Param("guidePrice") Double guidePrice,@Param("carType") String carType);

    /**
     * 使用where根据brand、guidePrice、carType进行联合查询
     * @param brand
     * @param guidePrice
     * @param carType
     * @return list
     */
    List<Car> selectByMultiConditionWhere(@Param("brand") String brand,@Param("guidePrice") Double guidePrice,@Param("carType") String carType);

    /**
     * 使用trim根据brand、guidePrice、carType进行联合查询
     * @param brand
     * @param guidePrice
     * @param carType
     * @return list
     */
    List<Car> selectByMultiConditionWithTrim(@Param("brand") String brand,@Param("guidePrice") Double guidePrice,@Param("carType") String carType);

    /**
     * 使用set标签更新数据
     * @param car
     * @return int
     */
    int updateWithSet(Car car);

    /**
     * 使用choose when otherwise标签筛选条件查询
     * @param brand
     * @param guidePrice
     * @param produceTime
     * @return list
     */
    List<Car> selectWithChoose(@Param("brand") String brand,@Param("guidePrice") Double guidePrice,@Param("produceTime") String produceTime);

    /**
     * 通过foreach完成批量删除
     * @param ids
     * @return int
     */
    int deleteBatchByForeach(@Param("ids") Long[] ids);

    /**
     * 通过foreach完成批量删除
     * @param ids
     * @return int
     */
    int deleteBatchByForeach2(@Param("ids") Long[] ids);

    /**
     * 使用foreach批量插入数据
     * @param cars
     * @return int
     */
    int insertBatchByForeach(@Param("cars") List<Car> cars);
}
```

**CarMapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.powernode.mapper.CarMapper">
    <select id="selectByMultiCondition" resultType="car">
        select * from t_car where 1=1
        <if test="brand != null and brand != ''">
            brand like "%"#{brand}"%"
        </if>
        <if test="guidePrice != null and guidePrice != ''">
            and guide_price >= #{guidePrice}
        </if>
        <if test="carType != null and carType != ''">
            and car_type = #{carType}
        </if>
    </select>

    <select id="selectByMultiConditionWhere" resultType="car">
        select * from t_car
        <where>
            <if test="brand != null and brand != ''">
                and brand like "%"#{brand}"%"
            </if>
            <if test="guidePrice != null and guidePrice != ''">
                and guide_price >= #{guidePrice}
            </if>
            <if test="carType != null and carType != ''">
                and car_type = #{carType}
            </if>
        </where>
    </select>

    <select id="selectByMultiConditionWithTrim" resultType="car">
        select * from t_car
        <trim prefix="where" suffixOverrides="and|or">
            <if test="brand != null and brand != ''">
                brand like "%"#{brand}"%" and
            </if>
            <if test="guidePrice != null and guidePrice != ''">
                guide_price >= #{guidePrice} and
            </if>
            <if test="carType != null and carType != ''">
                car_type = #{carType} and
            </if>
        </trim>
    </select>

    <update id="updateWithSet">
        update t_car
        <set>
            <if test="carNum != null and carNum != ''">
                car_num = #{carNum} ,
            </if>
            <if test="brand != null and brand != ''">
                brand = #{brand} ,
            </if>
            <if test="guidePrice != null and guidePrice != ''">
                guide_price = #{guidePrice} ,
            </if>
            <if test="produceTime != null and produceTime != ''">
                produce_time = #{produceTime} ,
            </if>
            <if test="carType != null and carType != ''">
                car_type = #{carType} ,
            </if>
        </set>
        where id = #{id}
    </update>

    <select id="selectWithChoose" resultType="car">
        select * from t_car
        <where>
            <choose>
                <when test="brand != null and brand != ''">
                    brand like "%"#{brand}"%"
                </when>
                <when test="guidePrice != null and guidePrice != ''">
                    guide_price >= #{guidePrice}
                </when>
                <otherwise>
                    produce_time = #{produceTime}
                </otherwise>
            </choose>
        </where>
    </select>

    <delete id="deleteBatchByForeach">
        delete from t_car
        <where>
            id in
            <foreach collection="ids" item="id" separator="," open="(" close=")">
                #{id}
            </foreach>
        </where>
    </delete>

    <delete id="deleteBatchByForeach2">
        delete from t_car
        <where>
            <foreach collection="ids" item="id" separator="or">
                id = #{id}
            </foreach>
        </where>
    </delete>

    <insert id="insertBatchByForeach">
        insert into t_car values
        <foreach collection="cars" item="car" separator=",">
            (#{car.id},#{car.carNum},#{car.brand},#{car.guidePrice},#{car.produceTime},#{car.carType})
        </foreach>
    </insert>
</mapper>
```

**CarMapperTest**

```java
import com.powernode.mapper.CarMapper;
import com.powernode.pojo.Car;
import com.powernode.utils.SqlSessionUtil;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.ArrayList;
import java.util.List;

/**
 * ClassName:  CarMapperTest
 * Description:
 * DateTime:   2025/12/20 20:11
 * Author: Tian
 */
public class CarMapperTest {
    private SqlSession sqlSession = SqlSessionUtil.openSession();

    /**
     * 使用if标签
     */
    @Test
    public void testSelectByMultiCondition(){
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        //List<Car> cars = mapper.selectByMultiCondition("丰田",5.0,"新能源");
        //List<Car> cars = mapper.selectByMultiCondition("丰田",5.0,"燃油车");
        //List<Car> cars = mapper.selectByMultiCondition("",null,"燃油车"); //报错 where后面直接跟and
        //List<Car> cars = mapper.selectByMultiCondition("",null,"燃油车");
        List<Car> cars = mapper.selectByMultiCondition("",null,"");
        cars.forEach(car -> {
            System.out.println(car);
        });
    }

    /**
     * 使用where标签
     */
    @Test
    public void testSelectByMultiConditionWhere(){
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        List<Car> cars = mapper.selectByMultiConditionWhere("丰田",null,null);
        cars.forEach(car -> {
            System.out.println(car);
        });
    }

    /**
     * 使用trim标签
     */
    @Test
    public void testSelectByMultiConditionWithTrim(){
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        List<Car> cars = mapper.selectByMultiConditionWithTrim("",null,"");
        cars.forEach(car -> {
            System.out.println(car);
        });
    }

    /**
     * 使用set标签
     */
    @Test
    public void testUpdateWithSet(){
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        Car car = new Car(12L,"","比亚迪唐",null,"","新能源");
        int count = mapper.updateWithSet(car);
        sqlSession.commit();
        System.out.println(count);
    }

    /**
     * 使用choose when otherwise标签
     */
    @Test
    public void testSelectWithChoose(){
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        List<Car> cars = mapper.selectWithChoose("丰田",null,"");
        cars.forEach(car -> {
            System.out.println(car);
        });
    }

    /**
     * 使用foreach标签批量删除（使用in关键字）
     */
    @Test
    public void testDeleteBatchByForeach(){
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        Long[] longs = new Long[]{4L,11L,12L};
        int count = mapper.deleteBatchByForeach(longs);
        sqlSession.commit();
        sqlSession.close();
        System.out.println(count);
    }

    /**
     * 使用foreach标签批量删除（使用or关键字）
     */
    @Test
    public void testDeleteBatchByForeach2(){
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        Long[] longs = new Long[]{1L,2L,3L};
        int count = mapper.deleteBatchByForeach2(longs);
        sqlSession.commit();
        sqlSession.close();
        System.out.println(count);
    }


    /**
     * 使用foreach标签批量插入
     */
    @Test
    public void testInsertBatchByForeach(){
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        List<Car> cars = new ArrayList<>();
        Car car1 = new Car(1L,"1111","丰田霸道",20.0,"2000-10-01","燃油车");
        Car car2 = new Car(2L,"1112","丰田地道",21.0,"2001-10-02","燃油车");
        Car car3 = new Car(3L,"1113","丰田天道",22.0,"2002-10-03","燃油车");
        cars.add(car1);
        cars.add(car2);
        cars.add(car3);
        int count = mapper.insertBatchByForeach(cars);
        System.out.println(count);
        sqlSession.commit();
        sqlSession.close();
    }
}
```



## 十三、MyBatis高级映射和延迟加载

> 低级映射：单表查找，数据库表中的一条数据对应一个对象。
>
> **高级映射：多表联查，数据库表中的一条数据对应多个对象，比如：一个班级多个学生。**



> **项目准备**：
>
> 打包方式：`jar`
>
> 依赖：`mybatis`依赖、`mysql`依赖、`junit`依赖、`logback`依赖
>
> 配置文件：`mybatis-config.xml`、`logback.xml`、`jdbc.propertis`
>
> 工具类：`SqlSessionUtil`
>
> 准备数据库表：`t_stu`、`t_class`
>
> 创建POJO类：`Student`、`Clazz`
>
> 创建mapper接口：`StudentMapper`、`ClazzMapper`
>
> 创建mapper映射文件：`StudentMapper.xml`、`ClazzMapper.xml`

![image-20251222152138909](C:\Tian_File\Typora_images\typora-user-images\image-20251222152138909.png)

![image-20251222152158126](C:\Tian_File\Typora_images\typora-user-images\image-20251222152158126.png)



### 13.1 多对一

> **多对一**：“一”的主键在“多”的表中做**外键**。
>
> 多对一，多在前，多就为主表，一就为副表。
>
> 一对多，一在前，一就为主表，多就为副表。

![image-20251222153137290](C:\Tian_File\Typora_images\typora-user-images\image-20251222153137290.png)

![image-20251222152955201](C:\Tian_File\Typora_images\typora-user-images\image-20251222152955201.png)



> **多对一**在**查询所有关联信息**中有多种方式，常见的包括三种：
>
> - 第一种方式：**级联属性映射**（一条SQL语句）
> - 第二种方式：**association**（一条SQL语句）
> - 第三种方式：**分步查询**（两条SQL语句）
>   - 这种方式常用：
>     - 优点1：可复用（SQL语句分为两条，可以单独使用）
>     - 优点2：**支持懒加载（延迟加载）**

#### 第一种方式：级联属性映射

> t_stu作为主表，t_stu表中含有cid（t_class表的主键），那么Student类中需要通过cid包含t_class表的所有信息，所以Student类中应该以Clazz类作为属性包含t_class表中的所有信息。
>
> POJO类Student中添加一个属性：Clazz clazz，表示学生关联的班级对象。

```java
package com.powernode.mybatis.pojo;

public class Student {
    private Integer sid;
    private String sname;
    private Clazz clazz;

    public Clazz getClazz() {
        return clazz;
    }

    public void setClazz(Clazz clazz) {
        this.clazz = clazz;
    }

    @Override
    public String toString() {
        return "Student{" +
                "sid=" + sid +
                ", sname='" + sname + '\'' +
                ", clazz=" + clazz +
                '}';
    }

    public Student() {
    }

    public Student(Integer sid, String sname) {
        this.sid = sid;
        this.sname = sname;
    }

    public Integer getSid() {
        return sid;
    }

    public void setSid(Integer sid) {
        this.sid = sid;
    }

    public String getSname() {
        return sname;
    }

    public void setSname(String sname) {
        this.sname = sname;
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.powernode.mybatis.mapper.StudentMapper">

    <resultMap id="studentResultMap" type="Student">
        <id property="sid" column="sid"/>
        <result property="sname" column="sname"/>
        
        <!--在Student类中属性存放的是Clazz类，所以访问属性时使用“类名.属性”的方式-->
        <result property="clazz.cid" column="cid"/>
        <result property="clazz.cname" column="cname"/>
    </resultMap>

    <!--
		多表联查（一次性全部查到）
		select标签中使用了resultMap属性，因为select语句通过查找t_stu表的cid进一步查找t_class表中的字段信息
    -->
    <select id="selectBySid" resultMap="studentResultMap">
        select s.*, c.* from t_student s join t_clazz c on s.cid = c.cid where sid = #{sid}
    </select>

</mapper>
```

```java
@Test
public void testSelectBySid(){
    StudentMapper mapper = SqlSessionUtil.openSession().getMapper(StudentMapper.class);
    Student student = mapper.selectBySid(1);
    System.out.println(student);
}
```

![image-20251222154546561](C:\Tian_File\Typora_images\typora-user-images\image-20251222154546561.png)



#### 第二种方式：`association`

> 其他位置都不需要修改，只需要修改`resultMap`属性中的配置：`association`即可。
>
> association翻译为：关联
>
> 学生对象关联一个班级对象。

```xml
<resultMap id="studentResultMap" type="Student">
  <id property="sid" column="sid"/>
  <result property="sname" column="sname"/>
    
  <!--
	使用association标签	
	property属性：pojo类的属性名称
	javaType属性：pojo类的属性类型
  -->
  <association property="clazz" javaType="Clazz">
    <id property="cid" column="cid"/>
    <result property="cname" column="cname"/>
  </association>
</resultMap>
```



#### （常用）第三种方式：分步查询

> 其他位置不需要修改，只需要修改以及添加以下三处：
>
> 第一处：在`association`标签中的`select`属性填写`sqlId`。（`sqlId=namespace+id`）其中`column`属性作为这条子sql语句的条件。

```xml
<resultMap id="studentResultMap" type="Student">
  <id property="sid" column="sid"/>
  <result property="sname" column="sname"/>
    
  <!--
	property属性：pojo类的属性名称
	select属性：执行第二句SQL语句的SqlId
	column属性：执行第二句SQL语句要传入的参数
  -->
  <association property="clazz"
               select="com.powernode.mybatis.mapper.ClazzMapper.selectByCid"
               column="cid"/>
</resultMap>

<!--第一步执行的SQL语句-->
<select id="selectBySid" resultMap="studentResultMap">
  select s.* from t_student s where sid = #{sid}
</select>
```

> 第二处：在`ClazzMapper`接口中添加方法，**第二步SQL语句执行调用的方法**。

```java
package com.powernode.mybatis.mapper;

import com.powernode.mybatis.pojo.Clazz;

public interface ClazzMapper {

    /**
     * 根据cid获取Clazz信息，多对一第二步要执行的SQL语句
     * @param cid
     * @return
     */
    Clazz selectByCid(Integer cid);
}
```

> 第三处：在`ClazzMapper.xml`文件中进行配置，**配置的是第二步SQL执行的语句**。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.powernode.mybatis.mapper.ClazzMapper">
    <!--第二步执行的SQL语句-->
    <select id="selectByCid" resultType="Clazz">
        select * from t_clazz where cid = #{cid}
    </select>
</mapper>
```

> 执行结果，可以很明显看到先后有两条sql语句执行：

![image-20251222155802462](C:\Tian_File\Typora_images\typora-user-images\image-20251222155802462.png)

> 分步的优点：
>
> - 第一个优点：代码复用性增强。
> - 第二个优点：支持延迟加载。（暂时访问不到的数据可以先不查询，提高程序的执行效率。）
>   - **也就是说如果查询的只是第一步查询得到的结果，第二步查询就不执行**。



#### 多对一延迟加载

> 要想支持延迟加载，只需要在`association`标签中添加`fetchType="lazy"`即可。**注意：这里只是局部开启延迟加载**
>
> 修改`StudentMapper.xml`文件：

```xml
<resultMap id="studentResultMap" type="Student">
  <id property="sid" column="sid"/>
  <result property="sname" column="sname"/>
  <association property="clazz"
               select="com.powernode.mybatis.mapper.ClazzMapper.selectByCid"
               column="cid"
               fetchType="lazy"/>
</resultMap>
```

> 现在只查询学生名字，修改测试程序：

```java
public class StudentMapperTest {
    @Test
    public void testSelectBySid(){
        StudentMapper mapper = SqlSessionUtil.openSession().getMapper(StudentMapper.class);
        Student student = mapper.selectBySid(1);
        //System.out.println(student);
        // 只获取学生姓名
        String sname = student.getSname();
        System.out.println("学生姓名：" + sname);
    }
}
```

![image-20251222160140166](C:\Tian_File\Typora_images\typora-user-images\image-20251222160140166.png)

> 如果后续需要使用到学生所在班级的名称，这个时候才会执行关联的sql语句，修改测试程序：

```java
public class StudentMapperTest {
    @Test
    public void testSelectBySid(){
        StudentMapper mapper = SqlSessionUtil.openSession().getMapper(StudentMapper.class);
        Student student = mapper.selectBySid(1);
        //System.out.println(student);
        // 只获取学生姓名
        String sname = student.getSname();
        System.out.println("学生姓名：" + sname);
        // 到这里之后，想获取班级名字了
        String cname = student.getClazz().getCname();
        System.out.println("学生的班级名称：" + cname);
    }
```

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251222160230996.png" alt="image-20251222160230996" style="zoom:80%;" />

> 通过以上的执行结果可以看到，只有当使用到班级名称之后，才会执行关联的sql语句，这就是延迟加载。



### 全局开启延迟加载

![image-20251222160550862](C:\Tian_File\Typora_images\typora-user-images\image-20251222160550862.png)

> 需要在`setting`设置，默认情况下，没有开启。

```xml
<settings>
  <setting name="lazyLoadingEnabled" value="true"/>
</settings>
```

> 全局延迟加载开启后，`association`标签中的`fetchType="lazy"`就不需要了。
>
> **开启全局延迟加载之后，所有的SQL都会支持延迟加载**，如果个别SQL不需要延迟加载，可在`association`标签中设置`fetchType="eager"`取消延迟加载。



### 13.2 一对多

> 一对多：“一”做主表，“多”做副表。
>
> t_class作为主表，t_stu表中含有cid（t_class表的主键），那么Student类中需要通过cid获取t_stu表的所有信息，所以Class类中应该包含所有cid关联的学生信息。就需要使用集合存放，在Clazz类中添加`List<Student> `属性。

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251222160836384.png" alt="image-20251222160836384" style="zoom: 80%;" />

```java
public class Clazz {
    private Integer cid;
    private String cname;
    private List<Student> stus;
    // set get方法
    // 构造方法
    // toString方法
}
```



> **一对多**在**查询所有关联信息**中，通常包括两种实现方式：
>
> - 第一种方式：`collection`标签
> - 第二种方式：分步查询

#### 第一种方式：collection标签

```java
package com.powernode.mybatis.mapper;

import com.powernode.mybatis.pojo.Clazz;

public interface ClazzMapper {

    /**
     * 根据班级编号查询班级信息，同时班级中所有的学生信息也要查询。
     * @param cid
     * @return
     */
    Clazz selectClazzAndStusByCid(Integer cid);
    
    
    /**
     * 根据cid获取Clazz信息，之间作为多对一的第二步SQL执行语句调用的方法。
     * @param cid
     * @return
     */
    Clazz selectByCid(Integer cid);
}
```

```xml
<resultMap id="clazzResultMap" type="Clazz">
  <id property="cid" column="cid"/>
  <result property="cname" column="cname"/>
    
  <!--
	collection标签：集合标签
		property属性：pojo类的属性名称
		ofType属性：指定集合中的元素类型
  -->
  <collection property="stus" ofType="Student">
    <id property="sid" column="sid"/>
    <result property="sname" column="sname"/>
    <!--Student的cid属性就不用再查了，不然会一直递归查找-->
  </collection>
</resultMap>

<!--多表联查（一次性全部查到）-->
<select id="selectClazzAndStusByCid" resultMap="clazzResultMap">
  select * from t_clazz c left join t_student s on c.cid = s.cid where c.cid = #{cid}
</select>
```

```java
@Test
public void testSelectClazzAndStusByCid() {
    ClazzMapper mapper = SqlSessionUtil.openSession().getMapper(ClazzMapper.class);
    Clazz clazz = mapper.selectClazzAndStusByCid(1001);
    System.out.println(clazz);
}
```

![image-20251222162545199](C:\Tian_File\Typora_images\typora-user-images\image-20251222162545199.png)



#### 第二种方式：分步查询

> 修改以下三个位置即可：
>
> 第一处：在`collection`标签中`select`属性填写`sqlId`。（`sqlId=namespace+id`）其中`column`属性作为这条子sql语句的条件。

```xml
<resultMap id="clazzResultMap" type="Clazz">
  <id property="cid" column="cid"/>
  <result property="cname" column="cname"/>
  <!--主要看这里-->
  <collection property="stus"
              select="com.powernode.mybatis.mapper.StudentMapper.selectByCid"
              column="cid"/>
</resultMap>

<!--第一步执行的SQL语句-->
<select id="selectClazzAndStusByCid" resultMap="clazzResultMap">
  select * from t_clazz c where c.cid = #{cid}
</select>
```

> 第二处：在`StudentMapper`接口中添加方法，**第二步SQL语句执行调用的方法**。

```java
/**
* 根据班级编号获取所有的学生。
* @param cid
* @return
*/
List<Student> selectByCid(Integer cid);
```

> 第三处：在`StudentMapper.xml`文件中进行配置，**配置的是第二步SQL执行的语句**。

```xml
<!--第二步执行的SQL语句-->
<select id="selectByCid" resultType="Student">
  select * from t_student where cid = #{cid}
</select>
```

![image-20251222164312067](C:\Tian_File\Typora_images\typora-user-images\image-20251222164312067.png)



> 注意：在“多对一”中可以将第二步查询语句在Clazz测试类中测试，因为一个Student对应一个Clazz。但是，在“一对多”中不可以将第二步查询语句在Student测试类中测试，因为一个Clazz对应多个Student，但是第二步查询的返回值类型是Student。



#### 一对多延迟加载

> 一对多延迟加载机制和多对一是一样的。同样是通过两种方式：
>
> - 开启局部延迟加载：`fetchType="lazy"`
> - 开启全局延迟加载：修改全局的配置`setting`，配置文件中`lazyLoadingEnabled=true`，如果开启全局延迟加载，想让某个sql不使用延迟加载：`fetchType="eager"`。



## 十四、MyBatis缓存

![image-20251222211019098](C:\Tian_File\Typora_images\typora-user-images\image-20251222211019098.png)

> **缓存：`cache`**
>
> **缓存的作用：通过减少IO次数的方式，来提高程序的执行效率**。
>
> mybatis缓存：将select语句的查询结果放到缓存（内存）当中，下一次还是同一条select语句的话，直接从缓存中取，不再查数据库。
>
> - 优点一：减少了IO的次数
> - 优点二：不再执行繁琐的查找算法，效率大大提升。
>
> mybatis缓存包括：
>
> - **一级缓存**：将查询到的数据存储到`SqlSession`中。（SqlSession生命周期：一次会话）
> - **二级缓存**：将查询到的数据存储到`SqlSessionFactory`中。（SqlSessionFactory生命周期：数据库的开启与关闭）
> - 集成其它**第三方缓存**：比如**EhCache**（Java语言开发）、Memcache（C语言开发）等。
>
> **缓存只针对于DQL语句，也就是说缓存机制只针对select语句。**



### 14.1 一级缓存

> **一级缓存的范围是SqlSession**。一级缓存默认是开启的，不需要做任何配置。
>
> 原理：只要使用**同一个SqlSession对象执行同一条SQL语句**，就会执行一级缓存。

```java
package com.powernode.mybatis.mapper;

import com.powernode.mybatis.pojo.Car;

public interface CarMapper {
    /**
     * 根据id获取Car信息。
     * @param id
     * @return
     */
    Car selectById(Long id);
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.powernode.mybatis.mapper.CarMapper">

  <select id="selectById" resultType="Car">
    select * from t_car where id = #{id}
  </select>

</mapper>
```

```java
package com.powernode.mybatis.test;

import com.powernode.mybatis.mapper.CarMapper;
import com.powernode.mybatis.pojo.Car;
import com.powernode.mybatis.utils.SqlSessionUtil;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

public class CarMapperTest {

    @Test
    public void testSelectById() throws Exception{
        // 注意：不能使用我们封装的SqlSessionUtil工具类，因为封装的SqlSessionUtil工具类中使用同一个SqlSession对象。
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = builder.build(Resources.getResourceAsStream("mybatis-config.xml"));

        SqlSession sqlSession1 = sqlSessionFactory.openSession();

        CarMapper mapper1 = sqlSession1.getMapper(CarMapper.class);
        Car car1 = mapper1.selectById(83L);
        System.out.println(car1);

        CarMapper mapper2 = sqlSession1.getMapper(CarMapper.class);
        Car car2 = mapper2.selectById(83L);
        System.out.println(car2);

        SqlSession sqlSession2 = sqlSessionFactory.openSession();

        CarMapper mapper3 = sqlSession2.getMapper(CarMapper.class);
        Car car3 = mapper3.selectById(83L);
        System.out.println(car3);

        CarMapper mapper4 = sqlSession2.getMapper(CarMapper.class);
        Car car4 = mapper4.selectById(83L);
        System.out.println(car4);

    }
}
```

![image-20251222174952877](C:\Tian_File\Typora_images\typora-user-images\image-20251222174952877.png)

>什么情况下不执行缓存？
>
>- 第一种：**使用不同的SqlSession对象**
>- 第二种：**查询条件变化了**



#### 一级缓存失效情况

##### 第一种：手动清空一级缓存

> 在第一次查询和第二次查询之间，手动清空了一级缓存。

```java
sqlSession.clearCache();
```

##### 第二种：数据库中执行了增删改的操作

> 在第一次查询和第二次查询之间，执行了增删改操作（这个增删改和哪张表没有关系，只要有insert delete update操作，一级缓存就失效）。

```java
/**
* 保存账户信息
*/
void insertAccount();
```

```xml
<insert id="insertAccount">
  insert into t_act values(3, 'act003', 10000)
</insert>
```

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251222175500650.png" alt="image-20251222175500650" style="zoom:80%;" />

![image-20251222175527550](C:\Tian_File\Typora_images\typora-user-images\image-20251222175527550.png)

> 可以发现，在执行完第一次查询之后，执行了数据库表的插入操作，在第二次执行查询时，一级缓存就失效了。



### 14.2 二级缓存

> **二级缓存的范围是SqlSessionFactory**。
>
> 使用**二级缓存**需要具备以下几个条件：
>
> 1. 设置`settings`，`<setting name="cacheEnabled" value="true">` 全局开启或关闭所有映射器配置文件中已配置的任何缓存。默认就是true，无需设置。
> 2. 在需要使用二级缓存的`SqlMapper.xml`文件中添加配置：`<cache />`
> 3. 使用二级缓存的**实体类对象**必须是**可序列化**的，也就是必须**实现java.io.Serializable接口**。
> 4. `SqlSession`对象关闭或提交之后，一级缓存中的数据才会被写入到二级缓存当中，此时二级缓存才可用。

**CarMapper.xml**

```xml
<cache/>
```

**Car实体类**

```java
public class Car implements Serializable {
//......
}
```

**测试类**

```java
@Test
public void testSelectById2() throws Exception{
    //二级缓存存放在sqlSessionFactory当中
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));

    SqlSession sqlSession1 = sqlSessionFactory.openSession();
    CarMapper mapper1 = sqlSession1.getMapper(CarMapper.class);
    //第一次缓存未命中，执行SQL查询
    Car car1 = mapper1.selectById(83L);
    System.out.println(car1);

    // 关键一步，sqlSession对象关闭，一级缓存中的数据写入到二级缓存当中。
    sqlSession1.close();

    SqlSession sqlSession2 = sqlSessionFactory.openSession();
    CarMapper mapper2 = sqlSession2.getMapper(CarMapper.class);
    //第二次缓存命中，不用执行SQL查询，直接从缓存中提取数据
    Car car2 = mapper2.selectById(83L);
    System.out.println(car2);
}
```

![image-20251222180348691](C:\Tian_File\Typora_images\typora-user-images\image-20251222180348691.png)



#### 二级缓存失效情况

> **只要两次查询之间出现了增删改操作，二级缓存就会失效（一级缓存也会失效）**



#### 二级缓存的相关配置

![image-20251222180736031](C:\Tian_File\Typora_images\typora-user-images\image-20251222180736031.png)

>- eviction：指定从缓存中移除某个对象的淘汰算法。默认采用LRU策略。
>  - LRU：Least Recently Used。最近最少使用。优先淘汰在间隔时间内使用频率最低的对象。(其实还有一种淘汰算法LFU，最不常用。)
>  - FIFO：First In First Out。一种先进先出的数据缓存器。先进入二级缓存的对象最先被淘汰。
>  - SOFT：软引用。淘汰软引用指向的对象。具体算法和JVM的垃圾回收算法有关。
>  - WEAK：弱引用。淘汰弱引用指向的对象。具体算法和JVM的垃圾回收算法有关。
>
>- flushInterval：
>  - 二级缓存的刷新时间间隔。单位毫秒。如果没有设置。就代表不刷新缓存，只要内存足够大，一直会向二级缓存中缓存数据。除非执行了增删改。
>
>- readOnly：
>  - true：多条相同的sql语句执行之后返回的对象是共享的同一个。性能好。但是多线程并发可能会存在安全问题。
>  - false：多条相同的sql语句执行之后返回的对象是副本，调用了clone方法。性能一般。但安全。
>
>- size：
>  - 设置二级缓存中最多可存储的java对象数量。默认值1024。



### 14.3 MyBatis集成EhCache

> 集成`EhCache`是为了代替mybatis自带的二级缓存，**一级缓存是无法替代的**。
>
> mybatis对外提供了接口可以集成第三方的缓存组件。比如`EhCache`、`Memcache`等。
>
> `EhCache`是Java写的，`Memcache`是C语言写的，所以mybatis集成`EhCache`较为常见。
>
> 按照以下步骤操作，就可以完成集成：



> 第一步：引入mybatis整合ehcache的依赖

```xml
<!--mybatis集成ehcache的组件-->
<dependency>
  <groupId>org.mybatis.caches</groupId>
  <artifactId>mybatis-ehcache</artifactId>
  <version>1.2.2</version>
</dependency>
```

> 第二步：在类的根路径下新建`echcache.xml`文件，并提供以下配置信息。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <!--磁盘存储:将缓存中暂时不使用的对象,转移到硬盘,类似于Windows系统的虚拟内存-->
    <diskStore path="e:/ehcache"/>
  
    <!--defaultCache：默认的管理策略-->
    <!--eternal：设定缓存的elements是否永远不过期。如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断-->
    <!--maxElementsInMemory：在内存中缓存的element的最大数目-->
    <!--overflowToDisk：如果内存中数据超过内存限制，是否要缓存到磁盘上-->
    <!--diskPersistent：是否在磁盘上持久化。指重启jvm后，数据是否有效。默认为false-->
    <!--timeToIdleSeconds：对象空闲时间(单位：秒)，指对象在多长时间没有被访问就会失效。只对eternal为false的有效。默认值0，表示一直可以访问-->
    <!--timeToLiveSeconds：对象存活时间(单位：秒)，指对象从创建到失效所需要的时间。只对eternal为false的有效。默认值0，表示一直可以访问-->
    <!--memoryStoreEvictionPolicy：缓存的3 种清空策略-->
    <!--FIFO：first in first out (先进先出)-->
    <!--LFU：Less Frequently Used (最少使用).意思是一直以来最少被使用的。缓存的元素有一个hit 属性，hit 值最小的将会被清出缓存-->
    <!--LRU：Least Recently Used(最近最少使用). (ehcache 默认值).缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存-->
    <defaultCache eternal="false" maxElementsInMemory="1000" overflowToDisk="false" diskPersistent="false"
                  timeToIdleSeconds="0" timeToLiveSeconds="600" memoryStoreEvictionPolicy="LRU"/>

</ehcache>
```

> 第三步：修改`SqlMapper.xml`文件中的`<cache/>`标签，添加`type`属性。

```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```

> 第四步：编写测试程序

```java
@Test
public void testSelectById2() throws Exception{
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
    
    SqlSession sqlSession1 = sqlSessionFactory.openSession();
    CarMapper mapper1 = sqlSession1.getMapper(CarMapper.class);
    Car car1 = mapper1.selectById(83L);
    System.out.println(car1);
    
    sqlSession1.close();
    
    SqlSession sqlSession2 = sqlSessionFactory.openSession();
    CarMapper mapper2 = sqlSession2.getMapper(CarMapper.class);
    Car car2 = mapper2.selectById(83L);
    System.out.println(car2);
}
```

![image-20251222181405193](C:\Tian_File\Typora_images\typora-user-images\image-20251222181405193.png)



## 十五、MyBatis逆向工程

> 所谓的逆向工程：根据数据库表逆向生成Java的pojo类，SqlMapper.xml文件，以及Mapper接口类等。
>
> 要完成这个工作，需要借助别人写好的逆向工程插件。
>
> 需要给插件配置信息：
>
> - pojo类名、包名以及生成位置。
> - SqlMapper.xml文件名以及生成位置。
> - Mapper接口名以及生成位置。
> - 连接数据库的信息。
> - 指定哪些表参与逆向工程。
> - ......



### 15.1 逆向工程配置与生成

#### 第一步：基础环境准备

> 打包方式：`jar`
>
> 依赖：`mybatis`依赖、`mysql`依赖、`junit`依赖、`logback`依赖
>
> 配置文件：`mybatis-config.xml`、`logback.xml`、`jdbc.propertis`
>
> 工具类：`SqlSessionUtil`
>
> 准备数据库表：`t_car`

#### 第二步：在`pom.xml`中添加逆向工程插件

```xml
<!--定制构建过程-->
<build>
  <!--可配置多个插件-->
  <plugins>
    <!--其中的一个插件：mybatis逆向工程插件-->
    <plugin>
      <!--插件的GAV坐标-->
      <groupId>org.mybatis.generator</groupId>
      <artifactId>mybatis-generator-maven-plugin</artifactId>
      <version>1.4.1</version>
      <!--允许覆盖-->
      <configuration>
        <overwrite>true</overwrite>
      </configuration>
      <!--插件的依赖-->
      <dependencies>
        <!--mysql驱动依赖-->
        <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>8.0.30</version>
        </dependency>
      </dependencies>
    </plugin>
  </plugins>
</build>
```

#### 第三步：配置`generatorConfig.xml`

> 该文件必须叫做：`generatorConfig.xml`
>
> 该文件必须放在类的根路径下。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!--
        targetRuntime有两个值：
            MyBatis3Simple：生成的是基础版，只有基本的增删改查。
            MyBatis3：生成的是增强版，除了基本的增删改查之外还有复杂的增删改查。
    -->
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!--防止生成重复代码-->
        <plugin type="org.mybatis.generator.plugins.UnmergeableXmlMappersPlugin"/>
      
        <commentGenerator>
            <!--是否去掉生成日期-->
            <property name="suppressDate" value="true"/>
            <!--是否去除注释-->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>

        <!--连接数据库信息-->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/powernode"
                        userId="root"
                        password="1265179125">
        </jdbcConnection>

        <!-- 生成pojo包名和位置 -->
        <javaModelGenerator targetPackage="com.powernode.pojo" targetProject="src/main/java">
            <!--是否开启子包-->
            <property name="enableSubPackages" value="true"/>
            <!--是否去除字段名的前后空白-->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- 生成SQL映射文件的包名和位置 -->
        <sqlMapGenerator targetPackage="com.powernode.mapper" targetProject="src/main/resources">
            <!--是否开启子包-->
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>

        <!-- 生成Mapper接口的包名和位置 -->
        <javaClientGenerator
                type="xmlMapper"
                targetPackage="com.powernode.mapper"
                targetProject="src/main/java">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>

        <!-- 表名和对应的实体类名-->
        <table tableName="t_car" domainObjectName="Car"/>

    </context>
</generatorConfiguration>
```



#### 第四步：运行插件

![image-20251222204912094](C:\Tian_File\Typora_images\typora-user-images\image-20251222204912094.png)

![image-20251222182644874](C:\Tian_File\Typora_images\typora-user-images\image-20251222182644874.png)



### 15.2 测试逆向工程

> MyBatis逆向工程自动生成的POJO类**没有构造方法**，**没有重写**`toString`方法，但是可以根据自动生成的POJO类，**自行添加**即可。
>
> 逆向工程会在`SqlMapper`接口中自动生成增删改查的方法。

```java
package com.powernode.mybatis.test;

import com.powernode.mybatis.mapper.CarMapper;
import com.powernode.mybatis.pojo.Car;
import com.powernode.mybatis.pojo.CarExample;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.math.BigDecimal;
import java.util.List;

public class GeneratorTest {
    @Test
    public void testGenerator() throws Exception{
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
        SqlSession sqlSession = sqlSessionFactory.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        // 增
        /*Car car = new Car();
        car.setCarNum("1111");
        car.setBrand("比亚迪唐");
        car.setGuidePrice(new BigDecimal(30.0));
        car.setProduceTime("2010-10-12");
        car.setCarType("燃油车");
        int count = mapper.insert(car);
        System.out.println("插入了几条记录：" + count);*/
        // 删
        /*int count = mapper.deleteByPrimaryKey(83L);
        System.out.println("删除了几条记录：" + count);*/
        // 改
        // 根据主键修改
        /*Car car = new Car();
        car.setId(89L);
        car.setGuidePrice(new BigDecimal(20.0));
        car.setCarType("新能源");
        int count = mapper.updateByPrimaryKey(car);
        System.out.println("更新了几条记录：" + count);*/
        // 根据主键选择性修改
        /*car = new Car();
        car.setId(89L);
        car.setCarNum("3333");
        car.setBrand("宝马520Li");
        car.setProduceTime("1999-01-10");
        count = mapper.updateByPrimaryKeySelective(car);
        System.out.println("更新了几条记录：" + count);*/

        // 查一个
        Car car = mapper.selectByPrimaryKey(89L);
        System.out.println(car);
        // 查所有
        List<Car> cars = mapper.selectByExample(null);
        cars.forEach(c -> System.out.println(c));
        // 多条件查询
        // QBC 风格：Query By Criteria 一种查询方式，比较面向对象，看不到sql语句。
        CarExample carExample = new CarExample();
        carExample.createCriteria()
                .andBrandEqualTo("丰田霸道")
                .andGuidePriceGreaterThan(new BigDecimal(60.0));
        carExample.or().andProduceTimeBetween("2000-10-11", "2022-10-11");

        mapper.selectByExample(carExample);
        sqlSession.commit();
    }
}
```



#### CarExample类

> **CarExample类负责封装查询条件**，可以按照条件进行查询。
>
> `QBC`风格：`Query By Criteria`一种查询方式，比较面向对象，看不到sql语句。
>
> 封装条件，通过CarExample对象来封装查询条件。



## 十六、MyBatis使用PageHelper插件

> `PageHelper`是做导航分页的。

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251223104140631.png" alt="image-20251223104140631" style="zoom:67%;" />



### 16.1 limit分页

![image-20251223111216982](C:\Tian_File\Typora_images\typora-user-images\image-20251223111216982.png)

> mysql的limit字段后面填写`limit num1,num2`
>
> - 第一个数字：startIndex（起始下标，下标从0开始）
> - 第二个数字：pageSize（每页显示的记录条数）
>
> 假设已知页码pageNum，还有每页显示的记录条数pageSize，可以动态获取startIndex。
>
> `startIndex = (pageNum - 1) * pageSize`

> 标准通用的mysql分页SQL：

```sql
select * from table limit (pageNum - 1)*pageSize,pageSize
```



```java
/**
 * 通过分页的方式获取Car列表
 * @param startIndex 页码
 * @param pageSize 每页显示记录条数
 * @return
 */
List<Car> selectAllByPage(@Param("startIndex") Integer startIndex, @Param("pageSize") Integer pageSize);
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.powernode.mybatis.mapper.CarMapper">

    <select id="selectAllByPage" resultType="Car">
        select * from t_car limit #{startIndex},#{pageSize}
    </select>
</mapper>
```

```java
@Test
public void testPage()throws Exception{
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
    SqlSession sqlSession = sqlSessionFactory.openSession();
    CarMapper mapper = sqlSession.getMapper(CarMapper.class);

    // 页码
    Integer pageNum = 2;
    // 每页显示记录条数
    Integer pageSize = 3;
    // 起始下标
    Integer startIndex = (pageNum - 1) * pageSize;

    List<Car> cars = mapper.selectAllByPage(startIndex, pageSize);
    cars.forEach(car -> System.out.println(car));

    sqlSession.commit();
    sqlSession.close();
}
```

![image-20251223105137362](C:\Tian_File\Typora_images\typora-user-images\image-20251223105137362.png)

> 问题：如果获取导航页数（分页的页数），以及获取当前导航页的起始页、最后页比较难，也就是获取分页相关的数据比较难。可以借助mybatis的PageHelper插件。



### 16.2 PageHelper插件

> 使用PageHelper插件进行分页，更加的便捷。

#### 第一步：引入依赖

```xml
<dependency>
  <groupId>com.github.pagehelper</groupId>
  <artifactId>pagehelper</artifactId>
  <version>5.3.1</version>
</dependency>
```

#### 第二步：在mybatis-config.xml文件中配置插件

> `typeAliases`标签下面进行配置：

```xml
<plugins>
  <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```

#### 第三步：编写代码

> **重点注意：**
>
> - 在查询语句之前开启分页功能。
> - 在查询语句之后封装PageInfo对象（PageInfo对象将来会存储到request域当中，在页面上展示）
>
> 封装分页信息对象 new PageInfo()，pageInfo对象是PageHelper插件提供的，用来封装分页相关的信息对象。

```java
List<Car> selectAll();
```

```xml
<select id="selectAll" resultType="Car">
  select * from t_car
</select>
```

```java
@Test
public void testPageHelper() throws Exception{
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
    SqlSession sqlSession = sqlSessionFactory.openSession();
    CarMapper mapper = sqlSession.getMapper(CarMapper.class);

    // 开启分页 在查询前开启分页
    PageHelper.startPage(2, 2);

    // 执行查询语句
    List<Car> cars = mapper.selectAll();

    // 获取分页信息对象 查询后再封装PageInfo对象
    PageInfo<Car> pageInfo = new PageInfo<>(cars, 5);
    System.out.println(pageInfo);
}
```

> 执行结果：

```
PageInfo{
  pageNum=2, pageSize=2, size=2, startRow=3, endRow=4, total=6, pages=3, 
  list=Page{count=true, pageNum=2, pageSize=2, startRow=2, endRow=4, total=6, pages=3, reasonable=false, pageSizeZero=false}
  [Car{id=86, carNum='1234', brand='丰田霸道', guidePrice=50.5, produceTime='2020-10-11', carType='燃油车'}, 
  Car{id=87, carNum='1234', brand='丰田霸道', guidePrice=50.5, produceTime='2020-10-11', carType='燃油车'}], 
  prePage=1, nextPage=3, isFirstPage=false, isLastPage=false, hasPreviousPage=true, hasNextPage=true, 
  navigatePages=5, navigateFirstPage=1, navigateLastPage=3, navigatepageNums=[1, 2, 3]
}
```



## 十七、MyBatis注解式开发

> mybatis中也提供了注解式开发方式，采用注解可以减少Sql映射文件的配置。
>
> **注意：MyBatis中使用注解时，就不用再创建SQL映射文件了。**
>
> 当然，使用注解式开发的话，sql语句是写在java程序中的，这种方式也会给sql语句的维护带来成本。
>
> **对于简单的单表的增删改查可以使用注解，但若是复杂的SQL语句还是建议使用SQL映射文件。**

> 使用注解编写复杂的SQL是这样的：

![image-20251223110347613](C:\Tian_File\Typora_images\typora-user-images\image-20251223110347613.png)

### 17.1 @Insert

> 不用写SqlMapper.xml映射文件

```java
@Insert(value="insert into t_car values(null,#{carNum},#{brand},#{guidePrice},#{produceTime},#{carType})")
int insert(Car car);
```

```java
@Test
public void testInsert() throws Exception{
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
    SqlSession sqlSession = sqlSessionFactory.openSession();
    CarMapper mapper = sqlSession.getMapper(CarMapper.class);
    Car car = new Car(null, "1112", "卡罗拉", 30.0, "2000-10-10", "燃油车");
    int count = mapper.insert(car);
    System.out.println("插入了几条记录：" + count);
    sqlSession.commit();
    sqlSession.close();
}
```



### 17.2 @Delete

```java
@Delete("delete from t_car where id = #{id}")
int deleteById(Long id);
```

```java
@Test
public void testDelete() throws Exception{
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
    SqlSession sqlSession = sqlSessionFactory.openSession();
    CarMapper mapper = sqlSession.getMapper(CarMapper.class);
    mapper.deleteById(89L);
    sqlSession.commit();
    sqlSession.close();
}
```



### 17.3 @Update

```java
@Update("update t_car set car_num=#{carNum},brand=#{brand},guide_price=#{guidePrice},produce_time=#{produceTime},car_type=#{carType} where id=#{id}")
int update(Car car);
```

```java
@Test
public void testUpdate() throws Exception{
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
    SqlSession sqlSession = sqlSessionFactory.openSession();
    CarMapper mapper = sqlSession.getMapper(CarMapper.class);
    Car car = new Car(88L,"1001", "凯美瑞", 30.0,"2000-11-11", "新能源");
    mapper.update(car);
    sqlSession.commit();
    sqlSession.close();
}
```



### 17.4 @Select

> 配置了驼峰命名映射，就可以直接写`@Select`注解。

```java
@Select("select * from t_car where id=#{id}")
Car selectById(Long id);
```

```java
@Test
public void testSelectById() throws Exception{
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
    SqlSession sqlSession = sqlSessionFactory.openSession();
    CarMapper carMapper = sqlSession.getMapper(CarMapper.class);
    Car car = carMapper.selectById(88L);
    System.out.println(car);
}
```



### 17.5 @Results

> 若是未配置驼峰命名映射，`@Select`注解就需要配合`@Results`标签

```java
@Select("select * from t_car where id = #{id}")
@Results({
    @Result(column = "id", property = "id"),
    @Result(column = "car_num", property = "carNum"),
    @Result(column = "brand", property = "brand"),
    @Result(column = "guide_price", property = "guidePrice"),
    @Result(column = "produce_time", property = "produceTime"),
    @Result(column = "car_type", property = "carType")
})
Car selectById(Long id);
```



## 十八、手写MyBatis框架（掌握原理）

### 18.1 dom4j解析XML文件

> 第一步：引入dom4j的依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.group</groupId>
    <artifactId>parse-xml-by-dom4j</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <!--dom4j依赖-->
        <dependency>
            <groupId>org.dom4j</groupId>
            <artifactId>dom4j</artifactId>
            <version>2.1.3</version>
        </dependency>
        <!--jaxen依赖-->
        <dependency>
            <groupId>jaxen</groupId>
            <artifactId>jaxen</artifactId>
            <version>1.2.0</version>
        </dependency>
        <!--junit依赖-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```



> 第二步：编写配置文件godbatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<configuration>
    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/powernode"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
        <mappers>
            <mapper resource="sqlmapper.xml"/>
        </mappers>
    </environments>
</configuration>
```



> 第三步：解析godbatis-config.xml

```java
package com.powernode.dom4j;

import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.Node;
import org.dom4j.io.SAXReader;
import org.junit.Test;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * 使用dom4j解析XML文件
 */
public class ParseXMLByDom4j {
    @Test
    public void testGodBatisConfig() throws Exception{

        // 读取xml，获取document对象
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(Thread.currentThread().getContextClassLoader().getResourceAsStream("godbatis-config.xml"));

        // 获取<environments>标签的default属性的值
        Element environmentsElt = (Element)document.selectSingleNode("/configuration/environments");
        String defaultId = environmentsElt.attributeValue("default");
        System.out.println(defaultId);

        // 获取environment标签
        Element environmentElt = (Element)document.selectSingleNode("/configuration/environments/environment[@id='" + defaultId + "']");

        // 获取事务管理器类型
        Element transactionManager = environmentElt.element("transactionManager");
        String transactionManagerType = transactionManager.attributeValue("type");
        System.out.println(transactionManagerType);

        // 获取数据源类型
        Element dataSource = environmentElt.element("dataSource");
        String dataSourceType = dataSource.attributeValue("type");
        System.out.println(dataSourceType);

        // 将数据源信息封装到Map集合
        Map<String,String> dataSourceMap = new HashMap<>();
        dataSource.elements().forEach(propertyElt -> {
            dataSourceMap.put(propertyElt.attributeValue("name"), propertyElt.attributeValue("value"));
        });

        dataSourceMap.forEach((k, v) -> System.out.println(k + ":" + v));

        // 获取sqlmapper.xml文件的路径
        Element mappersElt = (Element) document.selectSingleNode("/configuration/environments/mappers");
        mappersElt.elements().forEach(mapper -> {
            System.out.println(mapper.attributeValue("resource"));
        });
    }
}
```

![image-20251224145128743](C:\Tian_File\Typora_images\typora-user-images\image-20251224145128743.png)

> 第四步：编写配置文件sqlmapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<mapper namespace="car">
    <insert id="insertCar">
        insert into t_car(id,car_num,brand,guide_price,produce_time,car_type) values(null,#{carNum},#{brand},#{guidePrice},#{produceTime},#{carType})
    </insert>
    <select id="selectCarByCarNum" resultType="com.powernode.mybatis.pojo.Car">
        select id,car_num carNum,brand,guide_price guidePrice,produce_time produceTime,car_type carType from t_car where car_num = #{carNum}
    </select>
</mapper>
```



> 第五步：解析sqlmapper.xml

```java
@Test
public void testSqlMapper() throws Exception{
    // 读取xml，获取document对象
    SAXReader saxReader = new SAXReader();
    Document document = saxReader.read(Thread.currentThread().getContextClassLoader().getResourceAsStream("sqlmapper.xml"));

    // 获取namespace
    Element mapperElt = (Element) document.selectSingleNode("/mapper");
    String namespace = mapperElt.attributeValue("namespace");
    System.out.println(namespace);

    // 获取sqlId
    mapperElt.elements().forEach(statementElt -> {
        // 标签名
        String name = statementElt.getName();
        System.out.println("name:" + name);
        // 如果是select标签，还要获取它的resultType
        if ("select".equals(name)) {
            String resultType = statementElt.attributeValue("resultType");
            System.out.println("resultType:" + resultType);
        }
        // sql id
        String id = statementElt.attributeValue("id");
        System.out.println("sqlId:" + id);
        // sql语句
        String sql = statementElt.getTextTrim();
        System.out.println("sql:" + sql);
    });
}
```

![image-20251224145230436](C:\Tian_File\Typora_images\typora-user-images\image-20251224145230436.png)



### 18.2 GodBatis

> 手写框架没有思路，可以先参考一下MyBatis的框架流程，来逆推都需要哪些类。

```java
@Test
public void testInsert(){
    SqlSession sqlSession = null;
    try {
        // 1.创建SqlSessionFactoryBuilder对象
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        // 2.创建SqlSessionFactory对象
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config.xml"));
        // 3.创建SqlSession对象
        sqlSession = sqlSessionFactory.openSession();
        // 4.执行SQL
        Car car = new Car(null, "111", "宝马X7", "70.3", "2010-10-11", "燃油车");
        int count = sqlSession.insert("insertCar",car);
        System.out.println("更新了几条记录：" + count);
        // 5.提交
        sqlSession.commit();
    } catch (Exception e) {
        // 回滚
        if (sqlSession != null) {
            sqlSession.rollback();
        }
        e.printStackTrace();
    } finally {
        // 6.关闭
        if (sqlSession != null) {
            sqlSession.close();
        }
    }
}

@Test
public void testSelectOne(){
    SqlSession sqlSession = null;
    try {
        // 1.创建SqlSessionFactoryBuilder对象
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        // 2.创建SqlSessionFactory对象
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config.xml"));
        // 3.创建SqlSession对象
        sqlSession = sqlSessionFactory.openSession();
        // 4.执行SQL
        Car car = (Car)sqlSession.selectOne("selectCarByCarNum", "111");
        System.out.println(car);
        // 5.提交
        sqlSession.commit();
    } catch (Exception e) {
        // 回滚
        if (sqlSession != null) {
            sqlSession.rollback();
        }
        e.printStackTrace();
    } finally {
        // 6.关闭
        if (sqlSession != null) {
            sqlSession.close();
        }
    }
}
```



#### 第一步：IDEA创建模块

> 创建Maven模块，打包方式jar，引入相关依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.god</groupId>
    <artifactId>godbatis</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <dependencies>
        <!--dom4j依赖-->
        <dependency>
            <groupId>org.dom4j</groupId>
            <artifactId>dom4j</artifactId>
            <version>2.1.3</version>
        </dependency>
        <!--jaxen依赖-->
        <dependency>
            <groupId>jaxen</groupId>
            <artifactId>jaxen</artifactId>
            <version>1.2.0</version>
        </dependency>
        <!--junit依赖-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```



#### 第二步：创建`Resources`资源工具类（方便获取指向配置文件的输入流）

```java
package org.god.core;

import java.io.InputStream;

public class Resources {

    /**
     * 从类路径中获取配置文件的输入流
     * @param config
     * @return 输入流，该输入流指向类路径中的配置文件
     */
    public static InputStream getResourcesAsStream(String config){
        return Thread.currentThread().getContextClassLoader().getResourceAsStream(config);
    }
}
```



#### 第三步：创建`SqlSessionFactoryBuilder`类

> 提供一个无参数构造方法，再提供一个build方法，该build方法要返回SqlSessionFactory对象。

```java
package org.god.core;

import java.io.InputStream;

public class SqlSessionFactoryBuilder {

    /**
     * 创建构建器对象
     */
    public SqlSessionFactoryBuilder() {
    }

    /**
     * 获取SqlSessionFactory对象
     * 该方法主要功能是：读取godbatis核心配置文件，并构建SqlSessionFactory对象
     * @param inputStream 指向核心配置文件的输入流
     * @return SqlSessionFactory对象
     */
    public SqlSessionFactory build(InputStream inputStream){
        // 解析配置文件，创建数据源对象
        // 解析配置文件，创建事务管理器对象
        // 解析配置文件，获取所有的SQL映射对象
        // 将以上信息封装到SqlSessionFactory对象中
        // 返回
        return null;
    }
}
```



#### 第四步：分析`SqlSessionFactory`类中属性

> - 【**接口**】事务管理器（为了切换方便，将事务管理器定义为接口）
>   - JDBC
>   - MANAGED
> - 数据源（经分析数据源在SqlSessionFactory用不到）【无】
>   - POOLED
>   - UNPOOLED
>   - JNDI
> - 【**Map集合**】SQL映射对象集合Mappers
>   - Map<String,MappedStatement>
>     - key：SqlId（namespace + id）
>     - value：SQL标签的所有信息（resultType属性、SQL语句等）

![image-20251224150407831](C:\Tian_File\Typora_images\typora-user-images\image-20251224150407831.png)



#### 第五步：创建`JDBCTransaction`接口实现类

> 事务管理器最好是定义一个接口，可以方便切换不同的事务管理器，然后每一个具体的事务管理器都实现这个接口。

```java
package org.god.core;

import java.sql.Connection;

public interface TransactionManager {
    /**
     * 提交事务
     */
    void commit();

    /**
     * 回滚事务
     */
    void rollback();

    /**
     * 关闭事务
     */
    void close();

    /**
     * 开启连接
     */
    void openConnection();
    
    /**
     * 获取连接对象
     * @return 连接对象
     */
    Connection getConnection();
}
```

```java
package org.god.core;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

/**
 * 事务管理器（接口实现类）
 */
public class JDBCTransaction implements TransactionManager {
    /**
     * 连接对象，控制事务时需要
     */
    private Connection conn;

    /**
     * 数据源对象（获取Connection连接对象）
     */
    private DataSource dataSource;

    /**
     * 自动提交标志：
     * true表示自动提交
     * false表示不自动提交
     */
    private boolean autoCommit;

    /**
     * 构造事务管理器对象
     * @param autoCommit
     */
    public GodJDBCTransaction(DataSource dataSource, boolean autoCommit) {
        this.dataSource = dataSource;
        this.autoCommit = autoCommit;
    }

    /**
     * 提交事务
     */
    public void commit(){
        try {
            conn.commit();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 回滚事务
     */
    public void rollback(){
        try {
            conn.rollback();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public void close() {
        try {
            conn.close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public void openConnection() {
        try {
            this.conn = dataSource.getConnection();
            this.conn.setAutoCommit(this.autoCommit);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public Connection getConnection() {
        return conn;
    }
}
```



#### 第六步：事务管理器中需要数据源，定义`UNPOOLEDDataSource`

```java
package org.god.core;

import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.SQLFeatureNotSupportedException;
import java.util.logging.Logger;

/**
 * 数据源实现类，不使用连接池
 */
public class GodUNPOOLEDDataSource implements javax.sql.DataSource{
    private String url;
    private String username;
    private String password;

    public GodUNPOOLEDDataSource(String driver, String url, String username, String password) {
        try {
            // 注册驱动
            Class.forName(driver);
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
        this.url = url;
        this.username = username;
        this.password = password;
    }

    @Override
    public Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, username, password);
    }

    @Override
    public Connection getConnection(String username, String password) throws SQLException {
        return null;
    }

    @Override
    public PrintWriter getLogWriter() throws SQLException {
        return null;
    }

    @Override
    public void setLogWriter(PrintWriter out) throws SQLException {

    }

    @Override
    public void setLoginTimeout(int seconds) throws SQLException {

    }

    @Override
    public int getLoginTimeout() throws SQLException {
        return 0;
    }

    @Override
    public Logger getParentLogger() throws SQLFeatureNotSupportedException {
        return null;
    }

    @Override
    public <T> T unwrap(Class<T> iface) throws SQLException {
        return null;
    }

    @Override
    public boolean isWrapperFor(Class<?> iface) throws SQLException {
        return false;
    }
}
```



#### 第七步：创建`MappedStatement`类

```java
package org.god.core;

/**
 * SQL映射实体类（）
 */
public class GodMappedStatement {
    private String sqlId;
    private String resultType;
    private String sql;
    private String parameterType;

    private String sqlType;

    @Override
    public String toString() {
        return "GodMappedStatement{" +
                "sqlId='" + sqlId + '\'' +
                ", resultType='" + resultType + '\'' +
                ", sql='" + sql + '\'' +
                ", parameterType='" + parameterType + '\'' +
                ", sqlType='" + sqlType + '\'' +
                '}';
    }

    public String getSqlId() {
        return sqlId;
    }

    public void setSqlId(String sqlId) {
        this.sqlId = sqlId;
    }

    public String getResultType() {
        return resultType;
    }

    public void setResultType(String resultType) {
        this.resultType = resultType;
    }

    public String getSql() {
        return sql;
    }

    public void setSql(String sql) {
        this.sql = sql;
    }

    public String getParameterType() {
        return parameterType;
    }

    public void setParameterType(String parameterType) {
        this.parameterType = parameterType;
    }

    public String getSqlType() {
        return sqlType;
    }

    public void setSqlType(String sqlType) {
        this.sqlType = sqlType;
    }

    public GodMappedStatement(String sqlId, String resultType, String sql, String parameterType, String sqlType) {
        this.sqlId = sqlId;
        this.resultType = resultType;
        this.sql = sql;
        this.parameterType = parameterType;
        this.sqlType = sqlType;
    }
}
```



#### 第八步：完善`SqlSessionFactory`类

```java
package org.god.core;

import javax.sql.DataSource;
import java.util.List;
import java.util.Map;

/**
 * SqlSession工厂对象，使用SqlSessionFactory可以获取会话对象
 */
public class SqlSessionFactory {
    private TransactionManager transactionManager;
    private Map<String, GodMappedStatement> mappedStatements;

    public SqlSessionFactory(TransactionManager transactionManager, Map<String, GodMappedStatement> mappedStatements) {
        this.transactionManager = transactionManager;
        this.mappedStatements = mappedStatements;
    }

    public TransactionManager getTransactionManager() {
        return transactionManager;
    }

    public void setTransactionManager(TransactionManager transactionManager) {
        this.transactionManager = transactionManager;
    }

    public Map<String, GodMappedStatement> getMappedStatements() {
        return mappedStatements;
    }

    public void setMappedStatements(Map<String, GodMappedStatement> mappedStatements) {
        this.mappedStatements = mappedStatements;
    }
}
```



#### 第九步：完善`SqlSessionFactoryBuilder`中的`build`方法

```java
package org.god.core;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import javax.sql.DataSource;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

/**
 * SqlSessionFactory对象构建器
 */
public class SqlSessionFactoryBuilder {

    /**
     * 创建构建器对象
     */
    public SqlSessionFactoryBuilder() {
    }

    /**
     * 获取SqlSessionFactory对象
     * 该方法主要功能是：读取godbatis核心配置文件，并构建SqlSessionFactory对象
     *
     * @param inputStream 指向核心配置文件的输入流
     * @return SqlSessionFactory对象
     */
    public SqlSessionFactory build(InputStream inputStream) throws DocumentException {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(inputStream);
        Element environmentsElt = (Element) document.selectSingleNode("/configuration/environments");
        String defaultEnv = environmentsElt.attributeValue("default");
        Element environmentElt = (Element) document.selectSingleNode("/configuration/environments/environment[@id='" + defaultEnv + "']");
        // 解析配置文件，创建数据源对象
        Element dataSourceElt = environmentElt.element("dataSource");
        DataSource dataSource = getDataSource(dataSourceElt);
        // 解析配置文件，创建事务管理器对象
        Element transactionManagerElt = environmentElt.element("transactionManager");
        TransactionManager transactionManager = getTransactionManager(transactionManagerElt, dataSource);
        // 解析配置文件，获取所有的SQL映射对象
        Element mappers = environmentsElt.element("mappers");
        Map<String, GodMappedStatement> mappedStatements = getMappedStatements(mappers);
        // 将以上信息封装到SqlSessionFactory对象中
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactory(transactionManager, mappedStatements);
        // 返回
        return sqlSessionFactory;
    }

    private Map<String, GodMappedStatement> getMappedStatements(Element mappers) {
        Map<String, GodMappedStatement> mappedStatements = new HashMap<>();
        mappers.elements().forEach(mapperElt -> {
            try {
                String resource = mapperElt.attributeValue("resource");
                SAXReader saxReader = new SAXReader();
                Document document = saxReader.read(Resources.getResourcesAsStream(resource));
                Element mapper = (Element) document.selectSingleNode("/mapper");
                String namespace = mapper.attributeValue("namespace");

                mapper.elements().forEach(sqlMapper -> {
                    String sqlId = sqlMapper.attributeValue("id");
                    String sql = sqlMapper.getTextTrim();
                    String parameterType = sqlMapper.attributeValue("parameterType");
                    String resultType = sqlMapper.attributeValue("resultType");
                    String sqlType = sqlMapper.getName().toLowerCase();
                    // 封装GodMappedStatement对象
                    GodMappedStatement godMappedStatement = new GodMappedStatement(sqlId, resultType, sql, parameterType, sqlType);
                    mappedStatements.put(namespace + "." + sqlId, godMappedStatement);
                });

            } catch (DocumentException e) {
                throw new RuntimeException(e);
            }
        });
        return mappedStatements;
    }


    private TransactionManager getTransactionManager(Element transactionManagerElt, DataSource dataSource) {
        String type = transactionManagerElt.attributeValue("type").toUpperCase();
        TransactionManager transactionManager = null;
        if ("JDBC".equals(type)) {
            // 使用JDBC事务
            transactionManager = new GodJDBCTransaction(dataSource, false);
        } else if ("MANAGED".equals(type)) {
            // 事务管理器是交给JEE容器的
        }
        return transactionManager;
    }

    private DataSource getDataSource(Element dataSourceElt) {
        // 获取所有数据源的属性配置
        Map<String, String> dataSourceMap = new HashMap<>();
        dataSourceElt.elements().forEach(propertyElt -> {
            dataSourceMap.put(propertyElt.attributeValue("name"), propertyElt.attributeValue("value"));
        });

        String dataSourceType = dataSourceElt.attributeValue("type").toUpperCase();
        DataSource dataSource = null;
        if ("POOLED".equals(dataSourceType)) {

        } else if ("UNPOOLED".equals(dataSourceType)) {
            dataSource = new GodUNPOOLEDDataSource(dataSourceMap.get("driver"), dataSourceMap.get("url"), dataSourceMap.get("username"), dataSourceMap.get("password"));
        } else if ("JNDI".equals(dataSourceType)) {

        }
        return dataSource;
    }
}
```



#### 第十步：在`SqlSessionFactory`中添加`openSession`方法

```java
public SqlSession openSession(){
    transactionManager.openConnection();
    SqlSession sqlSession = new SqlSession(transactionManager, mappedStatements);
    return sqlSession;
}
```



#### 第十一步：编写`SqlSession`类中`commit rollback close`方法

```java
package org.god.core;

import java.sql.SQLException;
import java.util.Map;

/**
 * 数据库会话对象
 */
public class SqlSession {
    private TransactionManager transactionManager;
    private Map<String, GodMappedStatement> mappedStatements;

    public SqlSession(TransactionManager transactionManager, Map<String, GodMappedStatement> mappedStatements) {
        this.transactionManager = transactionManager;
        this.mappedStatements = mappedStatements;
    }

    public void commit(){
        try {
            transactionManager.getConnection().commit();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public void rollback(){
        try {
            transactionManager.getConnection().rollback();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public void close(){
        try {
            transactionManager.getConnection().close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}
```



#### 第十二步：编写`SqlSession`类中的`insert`方法

```java
/**
 * 插入数据
 *
 * @param sqlId 要执行的sqlId
 * @param obj   插入的数据
 * @return
 */
public int insert(String sqlId, Object obj) {
    GodMappedStatement godMappedStatement = mappedStatements.get(sqlId);
    Connection connection = transactionManager.getConnection();
    // 获取sql语句
    // insert into t_car(id,car_num,brand,guide_price,produce_time,car_type) values(null,#{carNum},#{brand},#{guidePrice},#{produceTime},#{carType})
    String godbatisSql = godMappedStatement.getSql();
    // insert into t_car(id,car_num,brand,guide_price,produce_time,car_type) values(null,?,?,?,?,?)
    String sql = godbatisSql.replaceAll("#\\{[a-zA-Z0-9_\\$]*}", "?");

    // 重点一步
    Map<Integer, String> map = new HashMap<>();
    int index = 1;
    while (godbatisSql.indexOf("#") >= 0) {
        int beginIndex = godbatisSql.indexOf("#") + 2;
        int endIndex = godbatisSql.indexOf("}");
        map.put(index++, godbatisSql.substring(beginIndex, endIndex).trim());
        godbatisSql = godbatisSql.substring(endIndex + 1);
    }

    final PreparedStatement ps;
    try {
        ps = connection.prepareStatement(sql);

        // 给?赋值
        map.forEach((k, v) -> {
            try {
                // 获取java实体类的get方法名
                String getMethodName = "get" + v.toUpperCase().charAt(0) + v.substring(1);
                Method getMethod = obj.getClass().getDeclaredMethod(getMethodName);
                ps.setString(k, getMethod.invoke(obj).toString());
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        });
        int count = ps.executeUpdate();
        ps.close();
        return count;
    } catch (Exception e) {
        throw new RuntimeException(e);
    }
}
```



#### 第十三步：编写`SqlSession`类中的`selectOne`方法

```java
/**
 * 查询一个对象
 * @param sqlId
 * @param parameterObj
 * @return
 */
public Object selectOne(String sqlId, Object parameterObj){
    GodMappedStatement godMappedStatement = mappedStatements.get(sqlId);
    Connection connection = transactionManager.getConnection();
    // 获取sql语句
    String godbatisSql = godMappedStatement.getSql();
    String sql = godbatisSql.replaceAll("#\\{[a-zA-Z0-9_\\$]*}", "?");
    // 执行sql
    PreparedStatement ps = null;
    ResultSet rs = null;
    Object obj = null;
    try {
        ps = connection.prepareStatement(sql);
        ps.setString(1, parameterObj.toString());
        rs = ps.executeQuery();
        if (rs.next()) {
            // 将结果集封装对象，通过反射
            String resultType = godMappedStatement.getResultType();
            Class<?> aClass = Class.forName(resultType);
            Constructor<?> con = aClass.getDeclaredConstructor();
            obj = con.newInstance();
            // 给对象obj属性赋值
            ResultSetMetaData rsmd = rs.getMetaData();
            int columnCount = rsmd.getColumnCount();
            for (int i = 1; i <= columnCount; i++) {
                String columnName = rsmd.getColumnName(i);
                String setMethodName = "set" + columnName.toUpperCase().charAt(0) + columnName.substring(1);
                Method setMethod = aClass.getDeclaredMethod(setMethodName, aClass.getDeclaredField(columnName).getType());
                setMethod.invoke(obj, rs.getString(columnName));
            }
        }
    } catch (Exception e) {
        throw new RuntimeException(e);
    } finally {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
        try {
            ps.close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
    return obj;
}
```



### 18.3 GodBatis使用Maven打包

> 双击“install”打包文件。

![image-20251224152206875](C:\Tian_File\Typora_images\typora-user-images\image-20251224152206875.png)

> 查看本地仓库中是否已经有jar包：

![image-20251224152256219](C:\Tian_File\Typora_images\typora-user-images\image-20251224152256219.png)



### 18.4 使用GodBatis

> 使用GodBatis就和使用MyBatis是一样的。
>
> 第一步：准备数据库表t_user

![image-20251224152412795](C:\Tian_File\Typora_images\typora-user-images\image-20251224152412795.png)

> 第二步：创建模块，普通的Java Maven模块：godbatis-test

> 第三步：引入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  
  <groupId>com.powernode</groupId>
  <artifactId>godbatis-test</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
  
  <dependencies>
    <!--godbatis依赖-->
    <dependency>
      <groupId>org.god</groupId>
      <artifactId>godbatis</artifactId>
      <version>1.0.0</version>
    </dependency>
    <!--mysql-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.30</version>
    </dependency>
    <!--junit-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

> 第四步：编写pojo类

```java
package com.powernode.godbatis.pojo;

public class User {
    private String id;
    private String name;
    private String email;
    private String address;

    @Override
    public String toString() {
        return "User{" +
                "id='" + id + '\'' +
                ", name='" + name + '\'' +
                ", email='" + email + '\'' +
                ", address='" + address + '\'' +
                '}';
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public User() {
    }

    public User(String id, String name, String email, String address) {
        this.id = id;
        this.name = name;
        this.email = email;
        this.address = address;
    }
}
```

> 第五步：编写核心配置文件`godbatis-config.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<configuration>
    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="UNPOOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/powernode"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
        <mappers>
            <mapper resource="UserMapper.xml"/>
        </mappers>
    </environments>
</configuration>
```

> 第六步：编写sql映射文件`UserMapper.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<mapper namespace="user">
    <insert id="insertUser">
        insert into t_user(id,name,email,address) values(#{id},#{name},#{email},#{address})
    </insert>
    <select id="selectUserById" resultType="com.powernode.godbatis.pojo.User">
        select * from t_user where id = #{id}
    </select>
</mapper>
```

> 第七步：编写测试类

```java
package com.powernode.godbatis.test;

import com.powernode.godbatis.pojo.User;
import org.god.core.Resources;
import org.god.core.SqlSession;
import org.god.core.SqlSessionFactory;
import org.god.core.SqlSessionFactoryBuilder;
import org.junit.Test;

public class GodBatisTest {
    
    @Test
    public void testInsertUser() throws Exception{
        User user = new User("1", "zhangsan", "zhangsan@1234.com", "北京大兴区");
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourcesAsStream("godbatis-config.xml"));
        SqlSession sqlSession = sqlSessionFactory.openSession();
        int count = sqlSession.insert("user.insertUser", user);
        System.out.println("插入了几条记录：" + count);
        sqlSession.commit();
        sqlSession.close();
    }
    
    @Test
    public void testSelectUserById() throws Exception{
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourcesAsStream("godbatis-config.xml"));
        SqlSession sqlSession = sqlSessionFactory.openSession();
        Object user = sqlSession.selectOne("user.selectUserById", "1");
        System.out.println(user);
        sqlSession.close();
    }
}
```

> 第八步：运行结果

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251224152643270.png" alt="image-20251224152643270" style="zoom:80%;" />

<img src="C:\Tian_File\Typora_images\typora-user-images\image-20251224152657336.png" alt="image-20251224152657336" style="zoom:80%;" />



### 18.5 总结

![MyBatis框架核心原理](C:\Tian_File\Typora_images\typora-user-images\MyBatis框架核心原理.jpg)

