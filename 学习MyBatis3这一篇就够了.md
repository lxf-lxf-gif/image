# 学习MyBatis3这一篇就够了

[TOC]





------

**配套资料，免费下载**
链接：https://pan.baidu.com/s/1NMy3OAKYwqti1O2n5V_siw
提取码：9n8f
复制这段内容后打开百度网盘手机App，操作更方便哦

## 第一章 MyBatis3概述

### 1.1、概述

MyBatis 本是apache的一个开源项目iBatis，2010年这个项目由apache software foundation迁移到了google code，并且改名为MyBatis，2013年11月迁移到Github。

MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

### 1.2、特点

- 简单易学：本身就很小且简单，没有任何第三方依赖，最简单安装只要两个jar文件+配置几个sql映射文件易于学习，易于使用，通过文档和源代码，可以比较完全的掌握它的设计思路和实现。
- 灵活：MyBatis 不会对应用程序或者数据库的现有设计强加任何影响。 sql写在xml里，便于统一管理和优化，通过sql语句可以满足操作数据库的所有需求。
- 解除sql与程序代码的耦合：通过提供DAO层，将业务逻辑和数据访问逻辑分离，使系统的设计更清晰，更易维护，更易单元测试。sql和代码的分离，提高了可维护性。
- 提供映射标签，支持对象与数据库的orm字段关系映射。
- 提供对象关系映射标签，支持对象关系组建维护。
- 提供xml标签，支持编写动态sql。

### 1.3、对比

- JDBC
  - SQL夹在Java代码块里，耦合度高导致硬编码内伤。
  - 维护不易且实际开发需求中SQL是有变化，频繁修改的情况多见。
- Hibernate和JPA
  - 长难复杂SQL，对于Hibernate而言处理也不容易。
  - 内部自动生产的SQL，不容易做特殊优化。
  - 基于全映射的全自动框架，大量字段的POJO进行部分映射时比较困难，导致数据库性能下降。
- MyBatis
  - MyBatis是一个半自动化的持久化层框架，简单易学、使用灵活。
  - 解除SQL与程序代码的耦合，SQL和Java编码分开，功能边界清晰，一个专注业务、一个专注数据。

### 1.4、官网

文档地址：[点击打开](https://mybatis.org/mybatis-3/zh/index.html)

源码地址：[点击打开](https://github.com/mybatis/mybatis-3)

### 1.5、下载

打开源码地址，在右侧导航栏找到“Release”，目前我们使用的是mybatis-3.5.5，因为会时常更新，建议和本教程采用版本一致。

![image-20200920150321906](https://img-blog.csdnimg.cn/img_convert/24fd7efa1405b3d4c6f22378b29b6642.png)

打开新页面后，往下拉，找到这里。

![image-20200920150724350](https://img-blog.csdnimg.cn/img_convert/a08adbdf41f94bfbe7197dda35363b52.png)

## 第二章 MyBatis3的增删改查

### 2.1、环境准备

- Eclipse：Oxygen.3a Release (4.7.3a)
- Java：1.8.0_261
- MySQL：5.5.61
- Oracle：11gR2
- 工程默认编码：UTF-8

![image-20200920154826882](https://img-blog.csdnimg.cn/img_convert/9646d86c1652fad33c3339aacd5fe4e0.png)

- 配置文件编码：UTF-8

![image-20200920154906460](https://img-blog.csdnimg.cn/img_convert/df04265c999cad37e3a1aa9bacbc4f5a.png)

- 单元测试版本：Junit4

### 2.2、创建工程

![image-20200920155144482](https://img-blog.csdnimg.cn/img_convert/5351c032e8b93c24108bbefc2b95a45e.png)

![image-20200920155350466](https://img-blog.csdnimg.cn/img_convert/469c78b297acf3f79ab557e754aae76a.png)

![image-20200920155519115](https://img-blog.csdnimg.cn/img_convert/46b0a6835284f43648148c864323fc2e.png)

![image-20200920155557728](https://img-blog.csdnimg.cn/img_convert/f4af236eec203a47a9460e8e6a2b8d23.png)

![image-20200920155638339](https://img-blog.csdnimg.cn/img_convert/3f5048d599037de9cc3abfd972ff089e.png)

![image-20200920155719715](https://img-blog.csdnimg.cn/img_convert/2e571fa5d562cb1114212313f91363fe.png)

![image-20200920160057999](https://img-blog.csdnimg.cn/img_convert/5b88217d6252ff89bfd073339c79d346.png)

![image-20200920160130472](https://img-blog.csdnimg.cn/img_convert/4d4356cdf9f77782369d52f050693e56.png)

![image-20200920160241466](https://img-blog.csdnimg.cn/img_convert/fa9b30080886aeac1ba0e80fb49b3259.png)

![image-20200920160323723](https://img-blog.csdnimg.cn/img_convert/f74474b85d4930d6461e53096981bc16.png)

![image-20200920160456122](https://img-blog.csdnimg.cn/img_convert/55d7a9ac5503d79ea506c9e3f7619f82.png)

![image-20200920160530107](https://img-blog.csdnimg.cn/img_convert/081de3a4277c0fb8d0e937dd3f09e528.png)

![image-20200920160645519](https://img-blog.csdnimg.cn/img_convert/192c19bfdd103bb42da5160ef842b47e.png)

### 2.3、导入依赖

- mysql的数据库驱动包
  - mysql-connector-java-5.1.47-bin.jar
- oracle的数据库驱动包
  - ojdbc6-11.2.0.4.0.jar
- mybatis的开发工具包
  - mybatis-3.5.5.jar
- mybatis的日志记录包
  - log4j-1.2.17.jar
  - log4j.xml

把以上所有依赖的JAR包，全部拷贝到lib文件夹中，然后右键Build Path一下！

![image-20200920162113991](https://img-blog.csdnimg.cn/img_convert/1db0c8e28460ee1022bec67dc2f7b945.png)

![image-20200920162246439](https://img-blog.csdnimg.cn/img_convert/246e5cca7a9b045077721d73c3cdbf80.png)

![image-20200920162335534](https://img-blog.csdnimg.cn/img_convert/e3e95f5ac21699f9b7b85727ba03c116.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
	<appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
		<param name="Encoding" value="UTF-8" />
		<layout class="org.apache.log4j.PatternLayout">
			<param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m  (%F:%L) \n" />
		</layout>
	</appender>
	<logger name="java.sql">
		<level value="debug" />
	</logger>
	<logger name="org.apache.ibatis">
		<level value="info" />
	</logger>
	<root>
		<level value="debug" />
		<appender-ref ref="STDOUT" />
	</root>
</log4j:configuration>
1234567891011121314151617181920
```

把以上xml配置代码，拷贝到 log4j.xml 文件中，然后保存，效果如下图：

![image-20200920162714486](https://img-blog.csdnimg.cn/img_convert/cef12904e563331f337f2c1d98110448.png)

到这里，依赖文件和配置文件都已经安排的明明白白了，接下来，我们准备数据库！

### 2.4、创建数据库

创建mysql数据库的测试数据库：mybatis_crud，然后依次执行以下语句

```sql
CREATE DATABASE `mybatis_crud`CHARACTER SET utf8;
1
```

> 注意：如果你没有安装mysql，请自行参考《学习MySQL这一篇就够了》进行安装！

```sql
CREATE TABLE `employee` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `last_name` varchar(255) DEFAULT NULL,
  `email` varchar(255) DEFAULT NULL,
  `gender` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
1234567
```

创建oracle数据库的测试数据库：

> 注意：如果你没有安装oracle，请自行参考《学习Oracle这一篇就够了》进行安装！

```sql
CREATE TABLE employee (
  id number not null primary key,
  last_name varchar2(255) default null,
  email varchar2(255) default null,
  gender varchar2(255) default null
);
123456
```

Oracle需要创建序列：

```sql
create sequence EMPLOYEES_SEQ
start with 1
increment by 1
nomaxvalue
minvalue 1
nocycle
cache 10000;
1234567
```

### 2.5、编写CRUD

**第一步：创建mybatis-config.xml**

![image-20200920172414876](https://img-blog.csdnimg.cn/img_convert/574d53460347edd9ef8d1063f0fe2cde.png)

文件的内容拷贝以下信息：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 加载数据库配置文件信息 -->
	<properties resource="dbconfig.properties"></properties>

	<!-- 配置框架的全局配置信息 -->
	<settings>
		<setting name="mapUnderscoreToCamelCase" value="true" />
		<setting name="jdbcTypeForNull" value="NULL" />
		<setting name="lazyLoadingEnabled" value="true" />
		<setting name="aggressiveLazyLoading" value="false" />
	</settings>

	<!-- 配置框架的多数据源信息 -->
	<environments default="dev_mysql">
		<!-- 配置mysql开发环境，如果没有mysql可以去掉这个配置段 -->
		<environment id="dev_mysql">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="${mysql.driver}" />
				<property name="url" value="${mysql.url}" />
				<property name="username" value="${mysql.username}" />
				<property name="password" value="${mysql.password}" />
			</dataSource>
		</environment>
		<!-- 配置oracle开发环境，如果没有oracle可以去掉这个配置段 -->
		<environment id="dev_oracle">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="${oracle.driver}" />
				<property name="url" value="${oracle.url}" />
				<property name="username" value="${oracle.username}" />
				<property name="password" value="${oracle.password}" />
			</dataSource>
		</environment>
	</environments>

	<!-- 数据库厂商起别名 -->
	<databaseIdProvider type="DB_VENDOR">
		<property name="MySQL" value="mysql" />
		<property name="Oracle" value="oracle" />
		<property name="SQL Server" value="sqlserver" />
	</databaseIdProvider>

	<!-- 批量注册映射文件 -->
	<mappers>
		<package name="com.caochenlei.mybatis.mapper" />
	</mappers>
</configuration>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

**第二步：创建dbconfig.properties**

![image-20200920173716475](https://img-blog.csdnimg.cn/img_convert/7979b69c12f4cbc91b29595adaee5709.png)

文件的内容拷贝以下信息：

```properties
#mysql数据库配置，如果没有mysql可以去掉这个配置段
mysql.driver=com.mysql.jdbc.Driver
mysql.url=jdbc:mysql://localhost:3306/mybatis_crud
mysql.username=root
mysql.password=123456

#oracle数据库配置，如果没有oracle可以去掉这个配置段
oracle.driver=oracle.jdbc.OracleDriver
oracle.url=jdbc:oracle:thin:@localhost:1521:orcl
oracle.username=system
oracle.password=123456
1234567891011
```

**第三步：编写实体类**

Employee.java（全路径：/mybatis-crud/src/com/caochenlei/mybatis/crud/Employee.java）

```java
package com.caochenlei.mybatis.crud;

public class Employee {
	private Integer id;
	private String lastName;
	private String email;
	private String gender;

	public Employee() {
		super();
	}

	public Employee(Integer id, String lastName, String email, String gender) {
		super();
		this.id = id;
		this.lastName = lastName;
		this.email = email;
		this.gender = gender;
	}

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

	public String getGender() {
		return gender;
	}

	public void setGender(String gender) {
		this.gender = gender;
	}

	@Override
	public String toString() {
		return "Employee [id=" + id + ","
				+ " lastName=" + lastName + ","
				+ " email=" + email + ","
				+ " gender=" + gender + "]";
	}
}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960
```

**第四步：编写接口类**

EmployeeMapper.java（全路径：/mybatis-crud/src/com/caochenlei/mybatis/mapper/EmployeeMapper.java）

```java
package com.caochenlei.mybatis.mapper;

import com.caochenlei.mybatis.crud.Employee;

public interface EmployeeMapper {
	public Long addEmp(Employee employee);

	public Employee getEmpById(Integer id);

	public Long updateEmp(Employee employee);

	public Long deleteEmpById(Integer id);
}
12345678910111213
```

**第五步：编写映射文件**

EmployeeMapper.xml（全路径：/mybatis-crud/src/com/caochenlei/mybatis/mapper/EmployeeMapper.xml）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.caochenlei.mybatis.mapper.EmployeeMapper">

	<!-- mysql数据库语句映射，如果没有mysql可以去掉这个配置段 -->

	<!-- public Long addEmp(Employee employee); -->
	<insert id="addEmp" parameterType="com.caochenlei.mybatis.crud.Employee" useGeneratedKeys="true" keyProperty="id" databaseId="mysql">
		INSERT INTO `employee`(`last_name`,`email`,`gender`)
		VALUES(#{lastName},#{email},#{gender})
	</insert>

	<!-- public Employee getEmpById(Integer id); -->
	<select id="getEmpById" resultType="com.caochenlei.mybatis.crud.Employee" databaseId="mysql">
		SELECT * FROM `employee` WHERE `id` = #{id}
	</select>

	<!-- public boolean updateEmp(Employee employee); -->
	<update id="updateEmp" databaseId="mysql">
		UPDATE `employee`
		SET `last_name`=#{lastName},`email`=#{email},`gender`=#{gender}
		WHERE `id`=#{id}
	</update>

	<!-- public Long deleteEmpById(Integer id); -->
	<delete id="deleteEmpById" databaseId="mysql">
		DELETE FROM `employee` WHERE `id`=#{id}
	</delete>

	<!-- oracle数据库语句映射，如果没有oracle可以去掉这个配置段 -->

	<!-- public Long addEmp(Employee employee); -->
	<insert id="addEmp" databaseId="oracle">
		<selectKey keyProperty="id" order="BEFORE" resultType="Integer">
			SELECT EMPLOYEES_SEQ.nextval FROM DUAL
		</selectKey>

		INSERT INTO employee(id,last_name,email,gender)
		VALUES(#{id},#{lastName},#{email},#{gender})
	</insert>

	<!-- public Employee getEmpById(Integer id); -->
	<select id="getEmpById" resultType="com.caochenlei.mybatis.crud.Employee" databaseId="oracle">
		SELECT * FROM employee WHERE id = #{id}
	</select>

	<!-- public boolean updateEmp(Employee employee); -->
	<update id="updateEmp" databaseId="oracle">
		UPDATE employee
		SET last_name=#{lastName},email=#{email},gender=#{gender}
		WHERE id=#{id}
	</update>

	<!-- public Long deleteEmpById(Integer id); -->
	<delete id="deleteEmpById" databaseId="oracle">
		DELETE FROM employee WHERE id=#{id}
	</delete>

</mapper>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061
```

**第六步：测试MySQL的增查改删**

> 注意：当前的数据源切换到了mysql，请检查mybatis-config.xml
>
> <environments default=“dev_mysql”>，请依次运行EmployeeTest.java中的测试方法

```java
package com.caochenlei.mybatis.crud;

import java.io.IOException;
import java.io.InputStream;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import com.caochenlei.mybatis.mapper.EmployeeMapper;

public class EmployeeTest {

	@Test
	public void addEmpTest() {
		try {
			String resource = "mybatis-config.xml";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
			SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
			SqlSession openSession = sqlSessionFactory.openSession(true);
			EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);

			Employee employee = new Employee();
			employee.setLastName("zhangsan");
			employee.setGender("男");
			employee.setEmail("774908833@qq.com");
			Long rowCounts = employeeMapper.addEmp(employee);
			System.out.println("影响行数：" + rowCounts);
			System.out.println("自动增长：" + employee.getId());

			openSession.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	@Test
	public void getEmpByIdTest() {
		try {
			String resource = "mybatis-config.xml";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
			SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
			SqlSession openSession = sqlSessionFactory.openSession(true);
			EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);

			Employee employee = employeeMapper.getEmpById(1);
			System.out.println(employee);

			openSession.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	@Test
	public void updateEmpTest() {
		try {
			String resource = "mybatis-config.xml";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
			SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
			SqlSession openSession = sqlSessionFactory.openSession(true);
			EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);

			Employee employee = employeeMapper.getEmpById(1);
			employee.setGender("女");
			Long rowCounts = employeeMapper.updateEmp(employee);
			System.out.println("影响行数：" + rowCounts);

			openSession.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	@Test
	public void deleteEmpByIdTest() {
		try {
			String resource = "mybatis-config.xml";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
			SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
			SqlSession openSession = sqlSessionFactory.openSession(true);
			EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);

			Long rowCounts = employeeMapper.deleteEmpById(1);
			System.out.println("影响行数：" + rowCounts);

			openSession.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899
```

**第七步：测试Oracle的增查改删**

> 注意：当前的数据源切换到了oracle，请检查mybatis-config.xml
>
> <environments default=“dev_oracle”>，请依次运行EmployeeTest.java中的测试方法

```java
package com.caochenlei.mybatis.crud;

import java.io.IOException;
import java.io.InputStream;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import com.caochenlei.mybatis.mapper.EmployeeMapper;

public class EmployeeTest {

	@Test
	public void addEmpTest() {
		try {
			String resource = "mybatis-config.xml";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
			SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
			SqlSession openSession = sqlSessionFactory.openSession(true);
            
			EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
			Employee employee = new Employee();
			employee.setLastName("zhangsan");
			employee.setGender("男");
			employee.setEmail("774908833@qq.com");
			Long rowCounts = employeeMapper.addEmp(employee);
			System.out.println("影响行数：" + rowCounts);
			System.out.println("自动增长：" + employee.getId());

			openSession.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	@Test
	public void getEmpByIdTest() {
		try {
			String resource = "mybatis-config.xml";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
			SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
			SqlSession openSession = sqlSessionFactory.openSession(true);
            
			EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
			Employee employee = employeeMapper.getEmpById(1);
			System.out.println(employee);

			openSession.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	@Test
	public void updateEmpTest() {
		try {
			String resource = "mybatis-config.xml";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
			SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
			SqlSession openSession = sqlSessionFactory.openSession(true);
            
			EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
			Employee employee = employeeMapper.getEmpById(1);
			employee.setGender("女");
			Long rowCounts = employeeMapper.updateEmp(employee);
			System.out.println("影响行数：" + rowCounts);

			openSession.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	@Test
	public void deleteEmpByIdTest() {
		try {
			String resource = "mybatis-config.xml";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
			SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
			SqlSession openSession = sqlSessionFactory.openSession(true);
            
			EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
			Long rowCounts = employeeMapper.deleteEmpById(1);
			System.out.println("影响行数：" + rowCounts);

			openSession.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899
```

其实我们会发现，我们只是切换了数据源，而代码逻辑没有一点改变，改变的只是sql语句，这就是MyBatis的魅力了！

那么接下来，我们所有的操作都是基于mysql数据库进行的操作，请大家务必注意一点，其它章节的代码有可能会在本章节的基础上进行，也有可能会另起工程，具体请往后看！

### 2.6、其它查询

> 注意：所有测试数据基于mysql数据库

添加以下测试数据

```sql
#清空表
TRUNCATE TABLE employee;

#添加数据向employee表
INSERT INTO employee(id,last_name,email,gender) 
VALUES(NULL,"张三","123@qq.com","男");
INSERT INTO employee(id,last_name,email,gender) 
VALUES(NULL,"李四","123@qq.com","男");
INSERT INTO employee(id,last_name,email,gender) 
VALUES(NULL,"王五","123@qq.com","女");
INSERT INTO employee(id,last_name,email,gender) 
VALUES(NULL,"小六","123@qq.com","男");
INSERT INTO employee(id,last_name,email,gender) 
VALUES(NULL,"小七","123@qq.com","男");
INSERT INTO employee(id,last_name,email,gender) 
VALUES(NULL,"老八","123@qq.com","女");
INSERT INTO employee(id,last_name,email,gender) 
VALUES(NULL,"老九","123@qq.com","男");
123456789101112131415161718
```

#### 2.6.1、模糊查询

**需求信息**：将名称所有含有小字的员工的信息查询出来

**接口方法：**

EmployeeMapper.java

```java
public List<Employee> getEmpsLikeLastName(String lastName);
1
```

**映射文件：**

EmployeeMapper.xml

```xml
<!-- public List<Employee> getEmpsLikeLastName(String lastName); -->
<select id="getEmpsLikeLastName" resultType="com.caochenlei.mybatis.crud.Employee" databaseId="mysql">
    SELECT * FROM `employee` WHERE `last_name` like '%${lastName}%'
</select>
1234
```

**测试代码：**

EmployeeTest.java

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
List<Employee> emps = employeeMapper.getEmpsLikeLastName("小");
for (Employee employee : emps) {
	System.out.println(employee);
}
12345
```

**控制台结果：**

![image-20201001083817448](https://img-blog.csdnimg.cn/img_convert/0036e4b99853fb7786f3b1634d2ab619.png)

**注意问题：**

- \#{ }：是预编译处理，MyBatis在处理#{ }时，它会将sql中的#{ }替换为？，然后调用PreparedStatement的set方法来赋值，传入字符串后，会在值两边加上单引号，使用占位符的方式提高效率，可以防止sql注入。
- ${ }：表示拼接sql串，将接收到参数的内容不加任何修饰拼接在sql中，可能引发sql注入。

#### 2.6.2、排序查询

**需求信息**：将员工id按照降序顺序查询

**接口方法：**

EmployeeMapper.java

```java
public List<Employee> getEmpsOrderByIdDesc();
1
```

**映射文件：**

EmployeeMapper.xml

```xml
<!-- public List<Employee> getEmpsOrderByIdDesc(); -->
<select id="getEmpsOrderByIdDesc" resultType="com.caochenlei.mybatis.crud.Employee" databaseId="mysql">
	SELECT * FROM `employee` ORDER BY id DESC
</select>	
1234
```

**测试代码：**

EmployeeTest.java

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
List<Employee> emps = employeeMapper.getEmpsOrderByIdDesc();
for (Employee employee : emps) {
	System.out.println(employee);
}
12345
```

**控制台结果：**

![image-20201001084602969](https://img-blog.csdnimg.cn/img_convert/21d8ba55f9815a04f4427bbfeea6ef00.png)

#### 2.6.3、分页查询

在 mybatis 中，使用 RowBounds 进行分页，非常方便，不需要在 sql 语句中写 limit，即可完成分页功能。但是由于它是在 sql 查询出所有结果的基础上截取数据的，所以在数据量大的sql中并不适用，它更适合在返回数据结果较少的查询中使用，最核心的是在 mapper 接口层，传参时传入 RowBounds(int offset, int limit) 对象，即可完成分页。

> 注意：由于 java 允许的最大整数为 2147483647，所以 limit 能使用的最大整数也是 2147483647，一次性取出大量数据可能引起内存溢出，所以在大数据查询场合慎重使用。
>
> 分页插件：详情见9.4.1可真正实现物理分页查询。

**需求信息**：从索引3处（第一条索引为0）开始获取2条记录

**接口方法：**

EmployeeMapper.java

```java
public List<Employee> getEmpsByPage(RowBounds rowBounds);
1
```

**映射文件：**

EmployeeMapper.xml

```xml
<!-- public List<Employee> getEmpsByPage(RowBounds rowBounds); -->
<select id="getEmpsByPage" resultType="com.caochenlei.mybatis.crud.Employee" databaseId="mysql">
    SELECT * FROM `employee` WHERE `last_name` like '%${lastName}%'
</select>
1234
```

**测试代码：**

EmployeeTest.java

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
//从索引3处（第一条索引为0）开始获取2条记录
RowBounds rowBounds = new RowBounds(3, 2);
List<Employee> emps = employeeMapper.getEmpsByPage(rowBounds);
for (Employee employee : emps) {
    System.out.println(employee);
}
1234567
```

**控制台结果：**

![image-20201001102515063](https://img-blog.csdnimg.cn/img_convert/2d35983872fb6d8a6ddd2ac51535fcc8.png)

## 第三章 MyBatis3的全局配置

MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息， 配置文档的顶层结构如下：

- configuration（配置）
  - properties（属性）
  - settings（设置）
  - typeAliases（类型别名）
  - typeHandlers（类型处理器）
  - objectFactory（对象工厂）
  - plugins（插件）
  - environments（环境配置）
    - environment（环境变量）
      - transactionManager（事务管理器）
      - dataSource（数据源）
  - databaseIdProvider（数据库厂商标识）
  - mappers（映射器）

### 3.1、properties

**第一种：内置属性**

```xml
<properties>
	<property name="mysql.driver" value="com.mysql.jdbc.Driver" />
	<property name="mysql.url" value="jdbc:mysql://localhost:3306/mybatis_crud" />
	<property name="mysql.username" value="root" />
	<property name="mysql.password" value="123456" />
</properties>
123456
```

**第二种：引入文件**

```xml
<properties resource="dbconfig.properties"></properties>
1
```

**第三种：混合使用**

```xml
<properties resource="dbconfig.properties">
	<property name="mysql.driver" value="com.mysql.jdbc.Driver" />
	<property name="mysql.url" value="jdbc:mysql://localhost:3306/mybatis_crud" />
	<property name="mysql.username" value="root" />
	<property name="mysql.password" value="123456" />
</properties>
123456
```

### 3.2、settings

这些都是非常重要的调整，可以修改MyBatis在运行时的行为方式。这个下表说明了这些设置、它们的含义和默认值。

| 设置                             | 描述                                                         | 取值                                                         | 默认值                                        |
| -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| cacheEnabled                     | 该配置是影响所有映射器中配置缓存的全局开关。                 | true \| false                                                | true                                          |
| lazyLoadingEnabled               | 该配置是延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。在特定关联关系中可通过设置 fetchType 属性来覆盖该项的开关状态。 | true \| false                                                | false                                         |
| aggressiveLazyLoading            | 当启用时，对任意延迟属性的调用会使带有延迟加载属性的对象完整加载，反之，每种属性将会按需加载。 | true \| false                                                | false (true in ≤3.4.1)                        |
| multipleResultSetsEnabled        | 是否允许单一语句返回多结果集，需要兼容驱动。                 | true \| false                                                | true                                          |
| useColumnLabel                   | 使用列标签代替列名。不同的驱动会有不同的表现，具体可参考相关驱动文档或通过测试这两种不同的模式来观察所用驱动的结果。 | true \| false                                                | true                                          |
| useGeneratedKeys                 | 允许JDBC 支持自动生成主键，需要驱动兼容。如果设置为 true，则这个设置强制使用自动生成主键，尽管一些驱动不能兼容但仍可正常工作（比如 Derby）。 | true \| false                                                | false                                         |
| autoMappingBehavior              | 指定 MyBatis 应如何自动映射列到字段或属性。 NONE：表示取消自动映射。 PARTIAL：表示只会自动映射，没有定义嵌套结果集和映射结果集。 FULL：表示会自动映射任意复杂的结果集，无论是否嵌套。 | NONE PARTIAL FULL                                            | PARTIAL                                       |
| autoMappingUnknownColumnBehavior | 指定自动映射当中未知列（或未知属性类型）时的行为。 默认是不处理，只有当日志级别达到 WARN 级别或者以下，才会显示相关日志，如果处理失败会抛出 SqlSessionException异常。 | NONE WARNING FAILING                                         | NONE                                          |
| defaultExecutorType              | 配置默认的执行器。 SIMPLE：是普通的执行器。 REUSE：会重用预处理语句。 BATCH：执行器将重用语句并执行批量更新。 | SIMPLE REUSE BATCH                                           | SIMPLE                                        |
| defaultStatementTimeout          | 设置超时时间，它决定驱动等待数据库响应的秒数。               | 任意正整数值                                                 | Not Set (null)                                |
| defaultFetchSize                 | 设置数据库驱动程序默认返回的条数限制，此参数可以重新设置。   | 任意正整数值                                                 | Not Set (null)                                |
| defaultResultSetType             | 指定按语句设置忽略它的滚动策略。(Since: 3.5.2)               | FORWARD_ONLY SCROLL_SENSITIVE SCROLL_INSENSITIVE DEFAULT (same behavior with ‘Not Set’) | Not Set (null)                                |
| safeRowBoundsEnabled             | 允许在嵌套语句中使用分页（RowBounds）。 如果允许，设置 false。 | true \| false                                                | false                                         |
| safeResultHandlerEnabled         | 允许在嵌套语句中使用分页（ResultHandler）。 如果允许，设置false | true \| false                                                | true                                          |
| mapUnderscoreToCamelCaseEnables  | 是否开启自动驼峰命名规则映射，即从经典数据库列名 A_COLUMN 到经典 Java 属性名 aColumn 的类似映射。 | true \| false                                                | false                                         |
| localCacheScope                  | MyBatis 利用本地缓存机制（Local Cache）防止循环引用（circular references）和加速关联复嵌套査询。 SESSION：这种情况下会缓存一个会话中执行的所有查询。 STATEMENT：代表本地会话仅用在语句执行上，对相同 SqlScssion 的不同调用将不会共享数据。 | SESSION \| STATEMENT                                         | SESSION                                       |
| jdbcTypeForNull                  | 当没有为参数提供特定的 JDBC 类型时，为空值时指定 JDBC 类型。某些驱动需要指定列的 JDBC 类型，多数情况直接用一般类型即可，比如 NULL、VARCHAR 或 OTHER。 | JdbcType 枚举值                                              | OTHER                                         |
| lazyLoadTriggerMethods           | 指定哪个对象的方法触发一次延迟加载。                         | 一个逗号分隔的方法名称列表                                   | equals,clone, hashCode,toString               |
| defaultScriptingLanguage         | 指定动态 SQL 生成的默认语言。                                | 类型别名或指定类的全名称                                     | org.apache. ibatis.scripting. xmltags.XMLLang |
| defaultEnumTypeHandler           | 指定默认情况下用于枚举的TypeHandler。(Since: 3.4.5)          | 类型别名或指定类的全名称                                     | org.apache. ibatis.type. EnumTypeHandler      |
| callSettersOnNulls               | 指定当结果集中值为 null 时，是否调用映射对象的 setter（map 对象时为 put）方法，这对于 Map.keySet() 依赖或 null 值初始化时是有用的。注意，基本类型（int、boolean 等）不能设置成 null。 | true \| false                                                | false                                         |
| returnInstanceForEmptyRowMyBatis | 默认情况下，MyBatis在返回行的所有列都为null时返回null。启用此设置后，MyBatis将返回空实例。注意，它也适用于嵌套结果（即collectioin和association）。(Since: 3.4.2) | true \| false                                                | false                                         |
| logPrefix                        | 指定 MyBatis 增加到日志名称的前缀。                          | 任何字符串                                                   | Not set                                       |
| logImpl                          | 指定 MyBatis 所用日志的具体实现，未指定时将自动査找。        | SLF4J LOG4J LOG4J2 JDK_LOGGING COMMONS_LOGGING STDOUT_LOGGING NO_LOGGING | Not set                                       |
| proxyFactory                     | 指定 MyBatis 创建具有延迟加载能力的对象所用到的代理工具。    | CGLIB \| JAVASSIST                                           | JAVASSIST (MyBatis 3.3 or above)              |
| vfsImpl                          | 指定 VFS 的实现类。                                          | 自定义VFS实现的类的全名称，用逗号分隔。                      | Not set                                       |
| useActualParamName               | 允许使用方法签名中的名称作为语句参数名称。 为了使用该特性，你的项目必须采用 Java 8 编译，并且加上 -parameters 选项。(Since: 3.4.1) | true \| false                                                | true                                          |
| configurationFactory             | 指定提供配置实例的类。返回的配置实例用于加载反序列化对象的惰性属性。此类必须具有签名静态配置getConfiguration() 的方法。(Since: 3.2.3) | 类型别名或指定类的全名称                                     | Not set                                       |
| shrinkWhitespacesInSql           | 从SQL中删除多余的空白字符。注意，这也会影响SQL中的文字字符串。(Since 3.5.5) | true \| false                                                | false                                         |

一个较为完整的settings配置的示例如下：

```xml
<settings>
	<setting name="cacheEnabled" value="true" />
	<setting name="lazyLoadingEnabled" value="true" />
	<setting name="multipleResultSetsEnabled" value="true" />
	<setting name="useColumnLabel" value="true" />
	<setting name="useGeneratedKeys" value="false" />
	<setting name="autoMappingBehavior" value="PARTIAL" />
	<setting name="autoMappingUnknownColumnBehavior" value="WARNING" />
	<setting name="defaultExecutorType" value="SIMPLE" />
	<setting name="defaultStatementTimeout" value="25" />
	<setting name="defaultFetchSize" value="100" />
	<setting name="safeRowBoundsEnabled" value="false" />
	<setting name="mapUnderscoreToCamelCase" value="false" />
	<setting name="localCacheScope" value="SESSION" />
	<setting name="jdbcTypeForNull" value="NULL" />
	<setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString" />
</settings>
1234567891011121314151617
```

### 3.3、typeAliases

类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写。例如：

```xml
<typeAliases>
	<typeAlias alias="Author" type="domain.blog.Author" />
	<typeAlias alias="Blog" type="domain.blog.Blog" />
	<typeAlias alias="Comment" type="domain.blog.Comment" />
	<typeAlias alias="Post" type="domain.blog.Post" />
	<typeAlias alias="Section" type="domain.blog.Section" />
	<typeAlias alias="Tag" type="domain.blog.Tag" />
</typeAliases>
12345678
```

当这样配置时，Blog 可以用在任何使用 domain.blog.Blog 的地方。

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如：

```xml
<typeAliases>
	<package name="domain.blog" />
</typeAliases>
123
```

每一个在包 domain.blog 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。 比如 domain.blog.Author 的别名为 author；若有注解，则别名为其注解值。见下面的例子：

```java
@Alias("author")
public class Author {
    ...
}
1234
```

下面是一些为常见的 Java 类型内建的类型别名。它们都是不区分大小写的，注意，为了应对原始类型的命名重复，采取了特殊的命名风格。

| 别名       | 映射的类型 |
| ---------- | ---------- |
| _byte      | byte       |
| _long      | long       |
| _short     | short      |
| _int       | int        |
| _integer   | int        |
| _double    | double     |
| _float     | float      |
| _boolean   | boolean    |
| string     | String     |
| byte       | Byte       |
| long       | Long       |
| short      | Short      |
| int        | Integer    |
| integer    | Integer    |
| double     | Double     |
| float      | Float      |
| boolean    | Boolean    |
| date       | Date       |
| decimal    | BigDecimal |
| bigdecimal | BigDecimal |
| object     | Object     |
| map        | Map        |
| hashmap    | HashMap    |
| list       | List       |
| arraylist  | ArrayList  |
| collection | Collection |
| iterator   | Iterator   |

### 3.4、typeHandlers

MyBatis 在设置预处理语句（PreparedStatement）中的参数或从结果集中取出一个值时， 都会用类型处理器将获取到的值以合适的方式转换成 Java 类型。下表描述了一些默认的类型处理器。

> 注意：从 3.4.5 开始，MyBatis 默认支持 JSR-310（日期和时间 API） 。

| 类型处理器                   | Java 类型                       | JDBC 类型                                                    |
| ---------------------------- | ------------------------------- | ------------------------------------------------------------ |
| `BooleanTypeHandler`         | `java.lang.Boolean`, `boolean`  | 数据库兼容的 `BOOLEAN`                                       |
| `ByteTypeHandler`            | `java.lang.Byte`, `byte`        | 数据库兼容的 `NUMERIC` 或 `BYTE`                             |
| `ShortTypeHandler`           | `java.lang.Short`, `short`      | 数据库兼容的 `NUMERIC` 或 `SMALLINT`                         |
| `IntegerTypeHandler`         | `java.lang.Integer`, `int`      | 数据库兼容的 `NUMERIC` 或 `INTEGER`                          |
| `LongTypeHandler`            | `java.lang.Long`, `long`        | 数据库兼容的 `NUMERIC` 或 `BIGINT`                           |
| `FloatTypeHandler`           | `java.lang.Float`, `float`      | 数据库兼容的 `NUMERIC` 或 `FLOAT`                            |
| `DoubleTypeHandler`          | `java.lang.Double`, `double`    | 数据库兼容的 `NUMERIC` 或 `DOUBLE`                           |
| `BigDecimalTypeHandler`      | `java.math.BigDecimal`          | 数据库兼容的 `NUMERIC` 或 `DECIMAL`                          |
| `StringTypeHandler`          | `java.lang.String`              | `CHAR`, `VARCHAR`                                            |
| `ClobReaderTypeHandler`      | `java.io.Reader`                | -                                                            |
| `ClobTypeHandler`            | `java.lang.String`              | `CLOB`, `LONGVARCHAR`                                        |
| `NStringTypeHandler`         | `java.lang.String`              | `NVARCHAR`, `NCHAR`                                          |
| `NClobTypeHandler`           | `java.lang.String`              | `NCLOB`                                                      |
| `BlobInputStreamTypeHandler` | `java.io.InputStream`           | -                                                            |
| `ByteArrayTypeHandler`       | `byte[]`                        | 数据库兼容的字节流类型                                       |
| `BlobTypeHandler`            | `byte[]`                        | `BLOB`, `LONGVARBINARY`                                      |
| `DateTypeHandler`            | `java.util.Date`                | `TIMESTAMP`                                                  |
| `DateOnlyTypeHandler`        | `java.util.Date`                | `DATE`                                                       |
| `TimeOnlyTypeHandler`        | `java.util.Date`                | `TIME`                                                       |
| `SqlTimestampTypeHandler`    | `java.sql.Timestamp`            | `TIMESTAMP`                                                  |
| `SqlDateTypeHandler`         | `java.sql.Date`                 | `DATE`                                                       |
| `SqlTimeTypeHandler`         | `java.sql.Time`                 | `TIME`                                                       |
| `ObjectTypeHandler`          | Any                             | `OTHER` 或未指定类型                                         |
| `EnumTypeHandler`            | Enumeration Type                | VARCHAR 或任何兼容的字符串类型，用来存储枚举的名称（而不是索引序数值）。 |
| `EnumOrdinalTypeHandler`     | Enumeration Type                | 任何兼容的 `NUMERIC` 或 `DOUBLE` 类型，用来存储枚举的序数值（而不是名称）。 |
| `SqlxmlTypeHandler`          | `java.lang.String`              | `SQLXML`                                                     |
| `InstantTypeHandler`         | `java.time.Instant`             | `TIMESTAMP`                                                  |
| `LocalDateTimeTypeHandler`   | `java.time.LocalDateTime`       | `TIMESTAMP`                                                  |
| `LocalDateTypeHandler`       | `java.time.LocalDate`           | `DATE`                                                       |
| `LocalTimeTypeHandler`       | `java.time.LocalTime`           | `TIME`                                                       |
| `OffsetDateTimeTypeHandler`  | `java.time.OffsetDateTime`      | `TIMESTAMP`                                                  |
| `OffsetTimeTypeHandler`      | `java.time.OffsetTime`          | `TIME`                                                       |
| `ZonedDateTimeTypeHandler`   | `java.time.ZonedDateTime`       | `TIMESTAMP`                                                  |
| `YearTypeHandler`            | `java.time.Year`                | `INTEGER`                                                    |
| `MonthTypeHandler`           | `java.time.Month`               | `INTEGER`                                                    |
| `YearMonthTypeHandler`       | `java.time.YearMonth`           | `VARCHAR` 或 `LONGVARCHAR`                                   |
| `JapaneseDateTypeHandler`    | `java.time.chrono.JapaneseDate` | `DATE`                                                       |

你可以重写已有的类型处理器或创建你自己的类型处理器来处理不支持的或非标准的类型。

具体做法为：实现 org.apache.ibatis.type.TypeHandler 接口或继承一个很便利的类 org.apache.ibatis.type.BaseTypeHandler， 并且可以（可选地）将它映射到一个 JDBC 类型。比如：

```java
// ExampleTypeHandler.java
@MappedJdbcTypes(JdbcType.VARCHAR)
public class ExampleTypeHandler extends BaseTypeHandler<String> {
  @Override
  public void setNonNullParameter(PreparedStatement ps, int i, String parameter, JdbcType jdbcType) 
      throws SQLException {
    ps.setString(i, parameter);
  }

  @Override
  public String getNullableResult(ResultSet rs, String columnName) throws SQLException {
    return rs.getString(columnName);
  }

  @Override
  public String getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
    return rs.getString(columnIndex);
  }

  @Override
  public String getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
    return cs.getString(columnIndex);
  }
}
123456789101112131415161718192021222324
<!-- mybatis-config.xml -->
<typeHandlers>
  <typeHandler handler="org.mybatis.example.ExampleTypeHandler"/>
</typeHandlers>
1234
```

使用上述的类型处理器将会覆盖已有的处理 Java String 类型的属性以及 VARCHAR 类型的参数和结果的类型处理器。 要注意 MyBatis 不会通过检测数据库元信息来决定使用哪种类型，所以你必须在参数和结果映射中指明字段是 VARCHAR 类型， 以使其能够绑定到正确的类型处理器上。这是因为 MyBatis 直到语句被执行时才清楚数据类型。

通过类型处理器的泛型，MyBatis 可以得知该类型处理器处理的 Java 类型，不过这种行为可以通过两种方法改变：

- 在类型处理器的配置元素（typeHandler 元素）上增加一个 `javaType` 属性（比如：`javaType="String"`）；
- 在类型处理器的类上增加一个 `@MappedTypes` 注解指定与其关联的 Java 类型列表。 如果在 `javaType` 属性中也同时指定，则注解上的配置将被忽略。

可以通过两种方式来指定关联的 JDBC 类型：

- 在类型处理器的配置元素上增加一个 `jdbcType` 属性（比如：`jdbcType="VARCHAR"`）；
- 在类型处理器的类上增加一个 `@MappedJdbcTypes` 注解指定与其关联的 JDBC 类型列表。 如果在 `jdbcType` 属性中也同时指定，则注解上的配置将被忽略。

当在 `ResultMap` 中决定使用哪种类型处理器时，此时 Java 类型是已知的（从结果类型中获得），但是 JDBC 类型是未知的。 因此 Mybatis 使用 `javaType=[Java 类型], jdbcType=null` 的组合来选择一个类型处理器。 这意味着使用 `@MappedJdbcTypes` 注解可以限制类型处理器的作用范围，并且可以确保，除非显式地设置，否则类型处理器在 `ResultMap` 中将不会生效。 如果希望能在 `ResultMap` 中隐式地使用类型处理器，那么设置 `@MappedJdbcTypes` 注解的 `includeNullJdbcType=true` 即可。 然而从 Mybatis 3.4.0 开始，如果某个 Java 类型**只有一个**注册的类型处理器，即使没有设置 `includeNullJdbcType=true`，那么这个类型处理器也会是 `ResultMap` 使用 Java 类型时的默认处理器。

最后，可以让 MyBatis 帮你批量注册类型处理器：

```xml
<!-- mybatis-config.xml -->
<typeHandlers>
  <package name="org.mybatis.example"/>
</typeHandlers>
1234
```

注意在使用批量注册功能的时候，只能通过注解方式来指定 JDBC 的类型。

你可以创建能够处理多个类的泛型类型处理器。为了使用泛型类型处理器， 需要增加一个接受该类的 class 作为参数的构造器，这样 MyBatis 会在构造一个类型处理器实例的时候传入一个具体的类。

```java
//GenericTypeHandler.java
public class GenericTypeHandler<E extends MyObject> extends BaseTypeHandler<E> {

  private Class<E> type;

  public GenericTypeHandler(Class<E> type) {
    if (type == null) throw new IllegalArgumentException("Type argument cannot be null");
    this.type = type;
  }
  ...
12345678910
```

### 3.5、objectFactory

每次 MyBatis 创建结果对象的新实例时，它都会使用一个对象工厂（ObjectFactory）实例来完成实例化工作。 默认的对象工厂需要做的仅仅是实例化目标类，要么通过默认无参构造方法，要么通过存在的参数映射来调用带有参数的构造方法。 如果想覆盖对象工厂的默认行为，可以通过创建自己的对象工厂来实现。比如：

```java
// ExampleObjectFactory.java
public class ExampleObjectFactory extends DefaultObjectFactory {
  public Object create(Class type) {
    return super.create(type);
  }
  public Object create(Class type, List<Class> constructorArgTypes, List<Object> constructorArgs) {
    return super.create(type, constructorArgTypes, constructorArgs);
  }
  public void setProperties(Properties properties) {
    super.setProperties(properties);
  }
  public <T> boolean isCollection(Class<T> type) {
    return Collection.class.isAssignableFrom(type);
  }}
1234567891011121314
<!-- mybatis-config.xml -->
<objectFactory type="org.mybatis.example.ExampleObjectFactory">
  <property name="someProperty" value="100"/>
</objectFactory>
1234
```

ObjectFactory 接口很简单，它包含两个创建实例用的方法，一个是处理默认无参构造方法的，另外一个是处理带参数的构造方法的。 另外，setProperties 方法可以被用来配置 ObjectFactory，在初始化你的 ObjectFactory 实例后， objectFactory 元素体中定义的属性会被传递给 setProperties 方法。

### 3.6、plugins

MyBatis 允许你在映射语句执行过程中的某一点进行拦截调用。

默认情况下，MyBatis 允许使用插件来拦截的方法调用包括：

- Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed)
- ParameterHandler (getParameterObject, setParameters)
- ResultSetHandler (handleResultSets, handleOutputParameters)
- StatementHandler (prepare, parameterize, batch, update, query)

这些类中方法的细节可以通过查看每个方法的签名来发现，或者直接查看 MyBatis 发行包中的源代码。 如果你想做的不仅仅是监控方法的调用，那么你最好相当了解要重写的方法的行为。 因为在试图修改或重写已有方法的行为时，很可能会破坏 MyBatis 的核心模块。 这些都是更底层的类和方法，所以使用插件的时候要特别当心。

通过 MyBatis 提供的强大机制，使用插件是非常简单的，只需实现 Interceptor 接口，并指定想要拦截的方法签名即可。

```java
// ExamplePlugin.java
@Intercepts({@Signature(
  type= Executor.class,
  method = "update",
  args = {MappedStatement.class,Object.class})})
public class ExamplePlugin implements Interceptor {
  private Properties properties = new Properties();
  public Object intercept(Invocation invocation) throws Throwable {
    // implement pre processing if need
    Object returnObject = invocation.proceed();
    // implement post processing if need
    return returnObject;
  }
  public void setProperties(Properties properties) {
    this.properties = properties;
  }
}
1234567891011121314151617
<!-- mybatis-config.xml -->
<plugins>
  <plugin interceptor="org.mybatis.example.ExamplePlugin">
    <property name="someProperty" value="100"/>
  </plugin>
</plugins>
123456
```

上面的插件将会拦截在 Executor 实例中所有的 “update” 方法调用， 这里的 Executor 是负责执行底层映射语句的内部对象。

> 注意：**覆盖配置类**
>
> 除了用插件来修改 MyBatis 核心行为以外，还可以通过完全覆盖配置类来达到目的。只需继承配置类后覆盖其中的某个方法，再把它传递到 SqlSessionFactoryBuilder.build(myConfig) 方法即可。再次重申，这可能会极大影响 MyBatis 的行为，务请慎之又慎。

### 3.7、environments

MyBatis 可以配置成适应多种环境，这种机制有助于将 SQL 映射应用于多种数据库之中， 现实情况下有多种理由需要这么做。例如，开发、测试和生产环境需要有不同的配置，或者想在具有相同 Schema 的多个生产数据库中使用相同的 SQL 映射。还有许多类似的使用场景。

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

所以，如果你想连接两个数据库，就需要创建两个 SqlSessionFactory 实例，每个数据库对应一个。而如果是三个数据库，就需要三个实例，依此类推，记起来很简单：

**每个数据库对应一个 SqlSessionFactory 实例**

为了指定创建哪种环境，只要将它作为可选的参数传递给 SqlSessionFactoryBuilder 即可。可以接受环境配置的两个方法签名是：

```java
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment, properties);
12
```

如果忽略了环境参数，那么将会加载默认环境，如下所示：

```java
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, properties);
12
```

environments 元素定义了如何配置环境：

```xml
<environments default="development">
  <environment id="development">
    <transactionManager type="JDBC">
      <property name="..." value="..."/>
    </transactionManager>
    <dataSource type="POOLED">
      <property name="driver" value="${driver}"/>
      <property name="url" value="${url}"/>
      <property name="username" value="${username}"/>
      <property name="password" value="${password}"/>
    </dataSource>
  </environment>
</environments>
12345678910111213
```

注意一些关键点：

- 默认使用的环境 ID（比如：default=“development”）。
- 每个 environment 元素定义的环境 ID（比如：id=“development”）。
- 事务管理器的配置（比如：type=“JDBC”）。
- 数据源的配置（比如：type=“POOLED”）。

默认环境和环境 ID 顾名思义。 环境可以随意命名，但务必保证默认的环境 ID 要匹配其中一个环境 ID。

那如何创建多数据源呢，可以参考以下配置：

```xml
<environments default="dev_mysql">
	<!-- 配置mysql开发环境，如果没有mysql可以去掉这个配置段 -->
	<environment id="dev_mysql">
		<transactionManager type="JDBC" />
		<dataSource type="POOLED">
			<property name="driver" value="${mysql.driver}" />
			<property name="url" value="${mysql.url}" />
			<property name="username" value="${mysql.username}" />
			<property name="password" value="${mysql.password}" />
		</dataSource>
	</environment>
	<!-- 配置oracle开发环境，如果没有oracle可以去掉这个配置段 -->
	<environment id="dev_oracle">
		<transactionManager type="JDBC" />
		<dataSource type="POOLED">
			<property name="driver" value="${oracle.driver}" />
			<property name="url" value="${oracle.url}" />
			<property name="username" value="${oracle.username}" />
			<property name="password" value="${oracle.password}" />
		</dataSource>
	</environment>
</environments>
12345678910111213141516171819202122
```

**事务管理器（transactionManager）**

在 MyBatis 中有两种类型的事务管理器（也就是 type="[JDBC|MANAGED]"）：

- JDBC – 这个配置直接使用了 JDBC 的提交和回滚设施，它依赖从数据源获得的连接来管理事务作用域。
- MANAGED – 这个配置几乎没做什么。它从不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 默认情况下它会关闭连接。然而一些容器并不希望连接被关闭，因此需要将 closeConnection 属性设置为 false 来阻止默认的关闭行为。

> 注意：如果你正在使用 Spring + MyBatis，则没有必要配置事务管理器，因为 Spring 模块会使用自带的管理器来覆盖前面的配置。

这两种事务管理器类型都不需要设置任何属性。它们其实是类型别名，换句话说，你可以用 TransactionFactory 接口实现类的全限定名或类型别名代替它们。

```java
public interface TransactionFactory {
  default void setProperties(Properties props) { // 从 3.5.2 开始，该方法为默认方法
    // 空实现
  }
  Transaction newTransaction(Connection conn);
  Transaction newTransaction(DataSource dataSource, TransactionIsolationLevel level, boolean autoCommit);
}
1234567
```

在事务管理器实例化后，所有在 XML 中配置的属性将会被传递给 setProperties() 方法。你的实现还需要创建一个 Transaction 接口的实现类，这个接口也很简单：

```java
public interface Transaction {
  Connection getConnection() throws SQLException;
  void commit() throws SQLException;
  void rollback() throws SQLException;
  void close() throws SQLException;
  Integer getTimeout() throws SQLException;
}
1234567
```

使用这两个接口，你可以完全自定义 MyBatis 对事务的处理。

**数据源（dataSource）**

dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。大多数 MyBatis 应用程序会按示例中的例子来配置数据源。虽然数据源配置是可选的，但如果要启用延迟加载特性，就必须配置数据源。

有三种内建的数据源类型（也就是 type="[UNPOOLED|POOLED|JNDI]"）：

- **UNPOOLED**– 这个数据源的实现会每次请求时打开和关闭连接。虽然有点慢，但对那些数据库连接可用性要求不高的简单应用程序来说，是一个很好的选择。 性能表现则依赖于使用的数据库，对某些数据库来说，使用连接池并不重要，这个配置就很适合这种情形。UNPOOLED 类型的数据源仅仅需要配置以下 5 种属性：

  - `driver` – 这是 JDBC 驱动的 Java 类全限定名（并不是 JDBC 驱动中可能包含的数据源类）。

  - `url` – 这是数据库的 JDBC URL 地址。

  - `username` – 登录数据库的用户名。

  - `password` – 登录数据库的密码。

  - `defaultTransactionIsolationLevel` – 默认的连接事务隔离级别。

  - `defaultNetworkTimeout` – 等待数据库操作完成的默认网络超时时间（单位：毫秒）。

    > 注意：作为可选项，你也可以传递属性给数据库驱动，只需在属性名加上“driver.”前缀即可，如：`driver.encoding=UTF8`

- **POOLED**– 这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证时间。 这种处理方式很流行，能使并发 Web 应用快速响应请求。除了上述提到 UNPOOLED 下的属性外，还有更多属性用来配置 POOLED 的数据源：

  - `poolMaximumActiveConnections` – 在任意时间可存在的活动（正在使用）连接数量，默认值：10
  - `poolMaximumIdleConnections` – 任意时间可能存在的空闲连接数。
  - `poolMaximumCheckoutTime` – 在被强制返回之前，池中连接被检出（checked out）时间，默认值：20000 毫秒（即 20 秒）
  - `poolTimeToWait` – 这是一个底层设置，如果获取连接花费了相当长的时间，连接池会打印状态日志并重新尝试获取一个连接（避免在误配置的情况下一直失败且不打印日志），默认值：20000 毫秒（即 20 秒）。
  - `poolMaximumLocalBadConnectionTolerance` – 这是一个关于坏连接容忍度的底层设置， 作用于每一个尝试从缓存池获取连接的线程。 如果这个线程获取到的是一个坏的连接，那么这个数据源允许这个线程尝试重新获取一个新的连接，但是这个重新尝试的次数不应该超过 `poolMaximumIdleConnections` 与 `poolMaximumLocalBadConnectionTolerance` 之和。 默认值：3（新增于 3.4.5）
  - `poolPingQuery` – 发送到数据库的侦测查询，用来检验连接是否正常工作并准备接受请求。默认是“NO PING QUERY SET”，这会导致多数数据库驱动出错时返回恰当的错误消息。
  - `poolPingEnabled` – 是否启用侦测查询。若开启，需要设置 `poolPingQuery` 属性为一个可执行的 SQL 语句（最好是一个速度非常快的 SQL 语句），默认值：false。
  - `poolPingConnectionsNotUsedFor` – 配置 poolPingQuery 的频率。可以被设置为和数据库连接超时时间一样，来避免不必要的侦测，默认值：0（即所有连接每一时刻都被侦测 — 当然仅当 poolPingEnabled 为 true 时适用）。

- **JNDI** – 这个数据源实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的数据源引用。这种数据源配置只需要2个属性：

  - `initial_context` – 这个属性用来在 InitialContext 中寻找上下文（即，initialContext.lookup(initial_context)）。这是个可选属性，如果忽略，那么将会直接从 InitialContext 中寻找 data_source 属性。

  - `data_source` – 这是引用数据源实例位置的上下文路径。提供了 initial_context 配置时会在其返回的上下文中进行查找，没有提供时则直接在 InitialContext 中查找。

    > 注意：和其它数据源配置类似，可以通过添加前缀“env.”直接把属性传递给 InitialContext。如：`env.encoding=UTF8`

### 3.8、databaseIdProvider

MyBatis 可以根据不同的数据库厂商执行不同的语句，这种多厂商的支持是基于映射语句中的 `databaseId` 属性。 MyBatis 会加载带有匹配当前数据库 `databaseId` 属性和所有不带 `databaseId` 属性的语句。 如果同时找到带有 `databaseId` 和不带 `databaseId` 的相同语句，则后者会被舍弃。 为支持多厂商特性，只要像下面这样在 mybatis-config.xml 文件中加入 `databaseIdProvider` 即可，databaseIdProvider 对应的 DB_VENDOR 实现会将 databaseId 设置为 `DatabaseMetaData#getDatabaseProductName()` 返回的字符串。 由于通常情况下这些字符串都非常长，而且相同产品的不同版本会返回不同的值，你可能想通过设置属性别名来使其变短：

```xml
<databaseIdProvider type="DB_VENDOR">
	<property name="MySQL" value="mysql" />
	<property name="Oracle" value="oracle" />
	<property name="SQL Server" value="sqlserver" />
</databaseIdProvider>
12345
```

在提供了属性别名时，databaseIdProvider 的 DB_VENDOR 实现会将 databaseId 设置为数据库产品名与属性中的名称第一个相匹配的值，如果没有匹配的属性，将会设置为 “null”。 在这个例子中，如果 `getDatabaseProductName()` 返回“Oracle (DataDirect)”，databaseId 将被设置为“oracle”。

你可以通过实现接口 `org.apache.ibatis.mapping.DatabaseIdProvider` 并在 mybatis-config.xml 中注册来构建自己的 DatabaseIdProvider：

```java
public interface DatabaseIdProvider {
  default void setProperties(Properties p) { // 从 3.5.2 开始，该方法为默认方法
    // 空实现
  }
  String getDatabaseId(DataSource dataSource) throws SQLException;
}
123456
```

### 3.9、mappers

既然 MyBatis 的行为已经由上述元素配置完了，我们现在就要来定义 SQL 映射语句了。 但首先，我们需要告诉 MyBatis 到哪里去找到这些语句。 在自动查找资源方面，Java 并没有提供一个很好的解决方案，所以最好的办法是直接告诉 MyBatis 到哪里去找映射文件。 你可以使用相对于类路径的资源引用，或完全限定资源定位符（包括 `file:///` 形式的 URL），或类名和包名等。例如：

第一种形式：

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
123456
```

第二种形式：

```xml
<!-- 使用完全限定资源定位符（URL） -->
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
  <mapper url="file:///var/mappers/BlogMapper.xml"/>
  <mapper url="file:///var/mappers/PostMapper.xml"/>
</mappers>
123456
```

第三种形式：

```xml
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
123456
```

> 注意：这种形式必须将映射器接口实现类和映射文件放在同一目录中，否则会找不到相对应的xxx.xml。

第四种形式：

```xml
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
1234
```

这些配置会告诉 MyBatis 去哪里找映射文件，剩下的细节就应该是每个 SQL 映射文件了，也就是接下来我们要讨论的。

## 第四章 MyBatis3的映射配置

MyBatis 的真正强大在于它的语句映射，这是它的魔力所在。由于它的异常强大，映射器的 XML 文件就显得相对简单。如果拿它跟具有相同功能的 JDBC 代码进行对比，你会立即发现省掉了将近 95% 的代码。MyBatis 致力于减少使用成本，让用户能更专注于 SQL 代码。

SQL 映射文件只有很少的几个顶级元素（按照应被定义的顺序列出）：

- `cache` – 该命名空间的缓存配置。
- `cache-ref` – 引用其它命名空间的缓存配置。
- `resultMap` – 描述如何从数据库结果集中加载对象，是最复杂也是最强大的元素。
- `sql` – 可被其它语句引用的可重用语句块。
- `insert` – 映射插入语句。
- `update` – 映射更新语句。
- `delete` – 映射删除语句。
- `select` – 映射查询语句。

### 4.1、select

select 查询语句是 MyBatis 中最常用的元素之一，光能把数据存到数据库中价值并不大，还要能重新取出来才有用，多数应用也都是查询比修改要频繁。 MyBatis 的基本原则之一是：在每个插入、更新或删除操作之间，通常会执行多个查询操作。因此，MyBatis 在查询和结果映射做了相当多的改进。一个简单查询的 select 元素是非常简单的。比如：

```xml
<select id="selectPerson" parameterType="int" resultType="hashmap">
  SELECT * FROM PERSON WHERE ID = #{id}
</select>
123
```

这个语句名为 selectPerson，接受一个 int（或 Integer）类型的参数，并返回一个 HashMap 类型的对象，其中的键是列名，值便是结果行中的对应值。

> 注意：参数符号（#{id}）

这就告诉 MyBatis 创建一个预处理语句（PreparedStatement）参数，在 JDBC 中，这样的一个参数在 SQL 中会由一个“?”来标识，并被传递到一个新的预处理语句中，就像这样：

```java
// 近似的 JDBC 代码，非 MyBatis 代码...
String selectPerson = "SELECT * FROM PERSON WHERE ID=?";
PreparedStatement ps = conn.prepareStatement(selectPerson);
ps.setInt(1,id);
1234
```

当然，使用 JDBC 就意味着使用更多的代码，以便提取结果并将它们映射到对象实例中，而这就是 MyBatis 的拿手好戏，参数和结果映射的详细细节会分别在后面单独的小节中说明。

select 元素允许你配置很多属性来配置每条语句的行为细节，例如：

```xml
<select
  id="selectPerson"
  parameterType="int"
  resultType="hashmap"
  resultMap="personResultMap"
  flushCache="false"
  useCache="true"
  timeout="10"
  fetchSize="256"
  statementType="PREPARED"
  resultSetType="FORWARD_ONLY">
    ...
</select>
12345678910111213
```

那Select 元素的属性都是什么含义呢，请看下表：

| 属性            | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| `id`            | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType` | 将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。 |
| `resultType`    | 期望从这条语句中返回结果的类全限定名或别名。 注意，如果返回的是集合，那应该设置为集合包含的类型，而不是集合本身的类型。 resultType 和 resultMap 之间只能同时使用一个。 |
| `resultMap`     | 对外部 resultMap 的命名引用。结果映射是 MyBatis 最强大的特性，如果你对其理解透彻，许多复杂的映射问题都能迎刃而解。 resultType 和 resultMap 之间只能同时使用一个。 |
| `flushCache`    | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：false。 |
| `useCache`      | 将其设置为 true 后，将会导致本条语句的结果被二级缓存缓存起来，默认值：对 select 元素为 true。 |
| `timeout`       | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。 |
| `fetchSize`     | 这是一个给驱动的建议值，尝试让驱动程序每次批量返回的结果行数等于这个设置值。 默认值为未设置（unset）（依赖驱动）。 |
| `statementType` | 可选 STATEMENT，PREPARED 或 CALLABLE。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `resultSetType` | FORWARD_ONLY，SCROLL_SENSITIVE, SCROLL_INSENSITIVE 或 DEFAULT（等价于 unset） 中的一个，默认值为 unset （依赖数据库驱动）。 |
| `databaseId`    | 如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有不带 databaseId 或匹配当前 databaseId 的语句；如果带和不带的语句都有，则不带的会被忽略。 |
| `resultOrdered` | 这个设置仅针对嵌套结果 select 语句：如果为 true，将会假设包含了嵌套结果集或是分组，当返回一个主结果行时，就不会产生对前面结果集的引用。 这就使得在获取嵌套结果集的时候不至于内存不够用。默认值：`false`。 |
| `resultSets`    | 这个设置仅适用于多结果集的情况。它将列出语句执行后返回的结果集并赋予每个结果集一个名称，多个名称之间以逗号分隔。 |

#### 4.1.1、select返回一个对象

**需求信息**：根据员工ID查询员工信息然后将查询结果封装到一个对象中

**接口方法：**

```java
public Employee getEmpById(Integer id);
1
```

**映射配置：**

```xml
<!-- public Employee getEmpById(Integer id); -->
<select id="getEmpById" resultType="com.caochenlei.mybatis.crud.Employee" databaseId="mysql">
	SELECT * FROM `employee` WHERE `id` = #{id}
</select>
1234
```

**测试方法：**

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
Employee employee = employeeMapper.getEmpById(1);
System.out.println(employee);
123
```

#### 4.1.2、select返回一个List

**需求信息**：查询所有员工信息然后将查询结果封装到一个集合中

**接口方法：**

```java
public List<Employee> getEmpToList();
1
```

**映射配置：**

```xml
<!-- public List<Employee> getEmpToList(); -->
<select id="getEmpToList" resultType="com.caochenlei.mybatis.crud.Employee" databaseId="mysql">
	SELECT * FROM `employee`
</select>
1234
```

**测试方法：**

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
List<Employee> list = employeeMapper.getEmpToList();
for (Employee employee : list) {
	System.out.println(employee);
}
12345
```

#### 4.1.3、select返回一个Map

**需求信息**：根据员工ID查询员工信息然后将查询结果封装到一个Map中，键是列名，值是所对应的值

**接口方法：**

```java
public Map<String, Object> getEmpToMapById(Integer id);
1
```

**映射配置：**

```xml
<!-- public Map<String, Object> getEmpToMapById(Integer id); -->
<select id="getEmpToMapById" resultType="map" databaseId="mysql">
	SELECT * FROM `employee` WHERE `id` = #{id}
</select>
1234
```

**测试方法：**

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
Map<String, Object> map = employeeMapper.getEmpToMapById(2);
Set<String> keySet = map.keySet();
for (String key : keySet) {
	Object value = map.get(key);
	System.out.println(key + ":" + value);
}
1234567
```

------

**需求信息**：根据员工名称模糊查询员工信息然后将查询结果封装到一个Map中，键是每个员工的名字，值是所查询出的对象

**接口方法：**

```java
@MapKey("lastName")
public Map<String, Employee> getEmpToMapLikeLastName(String lastName);
12
```

**映射配置：**

```xml
<!-- public Map<String, Employee> getEmpToMapLikeLastName(String lastName); -->
<select id="getEmpToMapLikeLastName" resultType="com.caochenlei.mybatis.crud.Employee" databaseId="mysql">
	SELECT * FROM `employee` WHERE `last_name` like #{lastName}
</select>
1234
```

**测试方法：**

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
Map<String, Employee> map = employeeMapper.getEmpToMapLikeLastName("%zhang%");
Set<String> keySet = map.keySet();
for (String key : keySet) {
	Employee employee = map.get(key);
	System.out.println(key + ":" + employee);
}
1234567
```

#### 4.1.4、自定义结果的映射

**需求信息**：根据员工ID查询员工信息然后将查询结果封装到一个对象中，要求使用自己定义的封装结构resultMap

**接口方法：**

```java
public Employee getEmpByMapping(Integer id);
1
```

**映射配置：**

```xml
<!-- public Employee getEmpByMapping(Integer id); -->
<resultMap type="com.caochenlei.mybatis.crud.Employee" id="empMap">
	<!-- 设置主键映射 -->
	<id column="id" property="id" />
	<!-- 普通字段映射 -->
	<result column="last_name" property="lastName" />
	<result column="email" property="email" />
	<result column="gender" property="gender" />
</resultMap>
<select id="getEmpByMapping" resultMap="empMap" databaseId="mysql">
	SELECT * FROM `employee` WHERE `id` = #{id}
</select>
123456789101112
```

**测试方法：**

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
Employee employee = employeeMapper.getEmpByMapping(2);
System.out.println(employee);
123
```

#### 4.1.5、级联封装关联对象

**需求信息**：根据员工ID查询员工信息级联查询员工所在部门的信息

**准备工作：**

第一步：修改employee表结构，添加一列 dep_id，然后打开表，为这几个员工随便给一个编号（1、2、3）

```sql
ALTER TABLE `employee` ADD COLUMN `dep_id` INT;
1
```

第二步：创建department表，属性有dep_id、dep_name

```java
CREATE TABLE `department` (
  `dep_id` INT NOT NULL AUTO_INCREMENT,
  `dep_name` VARCHAR (255),
  PRIMARY KEY (`dep_id`)
) ;

INSERT INTO `department` (`dep_id`, `dep_name`) VALUES ('1', '开发部'); 
INSERT INTO `department` (`dep_id`, `dep_name`) VALUES ('2', '公关部'); 
INSERT INTO `department` (`dep_id`, `dep_name`) VALUES ('3', '测试部'); 
123456789
```

第三步：新增Department.java，添加以下代码

```java
package com.caochenlei.mybatis.crud;

public class Department {
	private Integer depId;
	private String depName;

	public Department() {
		super();
	}

	public Department(Integer depId, String depName) {
		super();
		this.depId = depId;
		this.depName = depName;
	}

	public Integer getDepId() {
		return depId;
	}

	public void setDepId(Integer depId) {
		this.depId = depId;
	}

	public String getDepName() {
		return depName;
	}

	public void setDepName(String depName) {
		this.depName = depName;
	}

	@Override
	public String toString() {
		return "Department [depId=" + depId + ", depName=" + depName + "]";
	}
}
12345678910111213141516171819202122232425262728293031323334353637
```

第四步：修改Employee.java，添加以下代码

```java
private Department dep;

public Department getDep() {
	return dep;
}

public void setDep(Department dep) {
	this.dep = dep;
}
123456789
```

**接口方法：**

```java
public Employee getEmpByIdWithDep1(Integer id);
1
```

**映射配置：**

```xml
<!-- public Employee getEmpByIdWithDep1(Integer id); -->
<resultMap type="com.caochenlei.mybatis.crud.Employee" id="empCascadeDep1">
	<!-- 设置主键映射 -->
	<id column="id" property="id" />
	<!-- 普通字段映射 -->
	<result column="last_name" property="lastName" />
	<result column="email" property="email" />
	<result column="gender" property="gender" />
	<!-- 级联查询部门 -->
	<result column="dep_id" property="dep.depId" />
	<result column="dep_name" property="dep.depName" />
</resultMap>
<select id="getEmpByIdWithDep1" resultMap="empCascadeDep1" databaseId="mysql">
	SELECT *
	FROM employee e,department d
	WHERE e.dep_id = d.dep_id
	AND e.id = #{id};
</select>
123456789101112131415161718
```

**测试方法：**

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
Employee employee = employeeMapper.getEmpByIdWithDep1(2);
System.out.println(employee);
System.out.println(employee.getDep());
1234
```

#### 4.1.6、association级联封装

**需求信息**：根据员工ID查询员工信息级联查询员工所在部门的信息

**接口方法：**

```java
public Employee getEmpByIdWithDep2(Integer id);
1
```

**映射配置：**

```xml
<!-- public Employee getEmpByIdWithDep2(Integer id); -->
<resultMap type="com.caochenlei.mybatis.crud.Employee" id="empCascadeDep2">
	<!-- 设置主键映射 -->
	<id column="id" property="id" />
	<!-- 普通字段映射 -->
	<result column="last_name" property="lastName" />
	<result column="email" property="email" />
	<result column="gender" property="gender" />
	<!-- 级联查询部门 -->
	<association property="dep">
		<!-- 设置主键映射 -->
		<id column="dep_id" property="depId" />
		<!-- 普通字段映射 -->
		<result column="dep_name" property="depName" />
	</association>
</resultMap>
<select id="getEmpByIdWithDep2" resultMap="empCascadeDep2" databaseId="mysql">
	SELECT *
	FROM employee e,department d
	WHERE e.dep_id = d.dep_id
	AND e.id = #{id};
</select>
12345678910111213141516171819202122
```

**测试方法：**

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
Employee employee = employeeMapper.getEmpByIdWithDep2(2);
System.out.println(employee);
System.out.println(employee.getDep());
1234
```

#### 4.1.7、association分步查询

**需求信息**：根据员工ID查询员工信息级联查询员工所在部门的信息

**准备工作：**

第一步：创建DepartmentMapper.java，拷贝以下代码

```java
package com.caochenlei.mybatis.mapper;

import com.caochenlei.mybatis.crud.Department;

public interface DepartmentMapper {

	public Department getDepById(Integer depId);

}
123456789
```

第二步：创建DepartmentMapper.xml，拷贝以下代码

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.caochenlei.mybatis.mapper.DepartmentMapper">

	<!-- public Employee getDepById(Integer id); -->
	<select id="getDepById" resultType="com.caochenlei.mybatis.crud.Department" databaseId="mysql">
		SELECT * FROM `department` WHERE `dep_id` = #{depId}
	</select>

</mapper>
123456789101112
```

**接口方法：**

```java
public Employee getEmpByIdWithDep3(Integer id);
1
```

**映射配置：**

```xml
<!-- public Employee getEmpByIdWithDep3(Integer id); -->
<resultMap type="com.caochenlei.mybatis.crud.Employee" id="empCascadeDep3">
	<!-- 设置主键映射 -->
	<id column="id" property="id" />
	<!-- 普通字段映射 -->
	<result column="last_name" property="lastName" />
	<result column="email" property="email" />
	<result column="gender" property="gender" />
	<!-- 级联查询部门 -->
	<association property="dep"
				 select="com.caochenlei.mybatis.mapper.DepartmentMapper.getDepById"
				 column="dep_id">
	</association>
</resultMap>
<select id="getEmpByIdWithDep3" resultMap="empCascadeDep3" databaseId="mysql">
	SELECT * FROM `employee` WHERE `id` = #{id} 
</select>
1234567891011121314151617
```

**测试方法：**

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
Employee employee = employeeMapper.getEmpByIdWithDep3(2);
System.out.println(employee);
System.out.println(employee.getDep());
1234
```

#### 4.1.8、association延迟加载

**需求信息**：使用以上三种关联查询的任意一种，开启延迟加载，根据员工ID查询员工名称，观察是否不会关联查询部门信息，答案：不会发送语句，这就实现了延迟加载的效果，然后方便的话，再把部门信息输出出来，再次运行，观察是否会发送语句，答案：会发送。

**准备工作：**

第一步：修改mybatis-config.xml中的settings配置，在里边加入以下代码

```xml
<!-- 开启延迟加载 -->
<setting name="lazyLoadingEnabled" value="true" />
<setting name="aggressiveLazyLoading" value="false" />
123
```

**测试方法：**

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
Employee employee = employeeMapper.getEmpByIdWithDep3(2);
System.out.println(employee.getLastName());
123
```

#### 4.1.9、collection级联封装

**准备工作：**

第一步：创建DepartmentTest.java，在里边进行代码测试，例如

```java
package com.caochenlei.mybatis.crud;

import java.io.IOException;
import java.io.InputStream;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

public class DepartmentTest {

	@Test
	public void test1() {
		try {
			String resource = "mybatis-config.xml";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
			SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
			SqlSession openSession = sqlSessionFactory.openSession(true);

			openSession.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

}
1234567891011121314151617181920212223242526272829
```

第二步：修改Department.java，新增以下代码

```java
private List<Employee> emps;

public List<Employee> getEmps() {
	return emps;
}

public void setEmps(List<Employee> emps) {
	this.emps = emps;
}
123456789
```

**接口方法：**

```java
public Department getDepByIdWithEmps1(Integer depId);
1
```

**映射配置：**

```xml
<!-- public Department getDepByIdWithEmps1(Integer depId); -->
<resultMap type="com.caochenlei.mybatis.crud.Department" id="depCascadeEmps1">
	<!-- 设置主键映射 -->
	<id column="dep_id" property="depId" />
	<!-- 普通字段映射 -->
	<result column="dep_name" property="depName" />
	<!-- 级联查询员工 -->
	<collection property="emps" ofType="com.caochenlei.mybatis.crud.Employee">
		<!-- 设置主键映射 -->
		<id column="id" property="id" />
		<!-- 普通字段映射 -->
		<result column="last_name" property="lastName" />
		<result column="email" property="email" />
		<result column="gender" property="gender" />
	</collection>
</resultMap>
<select id="getDepByIdWithEmps1" resultMap="depCascadeEmps1">
	SELECT *
	FROM employee e,department d
	WHERE e.dep_id = d.dep_id
	AND d.dep_id = #{depId};
</select>
12345678910111213141516171819202122
```

**测试方法：**

```java
DepartmentMapper departmentMapper = openSession.getMapper(DepartmentMapper.class);
Department department = departmentMapper.getDepByIdWithEmps1(1);
System.out.println(department);
System.out.println(department.getEmps());
1234
```

#### 4.1.10、collection分步查询

**需求信息**：根据部门dep_id查询部门信息级联查询该部门下所有员工的信息

**接口方法：**

```java
// EmployeeMapper.java
public Employee getEmpByDepId(Integer depId);

// DepartmentMapper.java
public Department getDepByIdWithEmps2(Integer depId);
12345
```

**映射配置：**

```xml
// EmployeeMapper.xml
<!-- public Employee getEmpByDepId(Integer depId); -->
<select id="getEmpByDepId" resultType="com.caochenlei.mybatis.crud.Employee" databaseId="mysql">
	SELECT * FROM `employee` WHERE `dep_id` = #{depId}
</select>

// DepartmentMapper.xml
<!-- public Department getDepByIdWithEmps2(Integer depId); -->
<resultMap type="com.caochenlei.mybatis.crud.Department" id="depCascadeEmps2">
	<!-- 设置主键映射 -->
	<id column="dep_id" property="depId" />
	<!-- 普通字段映射 -->
	<result column="dep_name" property="depName" />
	<!-- 级联查询员工 -->
	<collection property="emps"
				select="com.caochenlei.mybatis.mapper.EmployeeMapper.getEmpByDepId"
				column="dep_id">
	</collection>
</resultMap>
<select id="getDepByIdWithEmps2" resultMap="depCascadeEmps2">
	SELECT * FROM `department` WHERE `dep_id` = #{depId}
</select>
12345678910111213141516171819202122
```

**测试方法：**

```java
DepartmentMapper departmentMapper = openSession.getMapper(DepartmentMapper.class);
Department department = departmentMapper.getDepByIdWithEmps2(1);
System.out.println(department);
System.out.println(department.getEmps());
1234
```

#### 4.1.11、collection延迟加载

**需求信息**：使用以上两种关联查询的任意一种，开启延迟加载，根据部门dep_id查询部门信息，观察是否不会关联查询员工信息，答案：不会发送语句，这就实现了延迟加载的效果，然后方便的话，再把该部门下所有员工信息输出出来，再次运行，观察是否会发送语句，答案：会发送。

**准备工作：**如果有这两个配置，请自行忽略此步骤

第一步：修改mybatis-config.xml中的settings配置，在里边加入以下代码

```xml
<!-- 开启延迟加载 -->
<setting name="lazyLoadingEnabled" value="true" />
<setting name="aggressiveLazyLoading" value="false" />
123
```

**测试方法：**

```java
DepartmentMapper departmentMapper = openSession.getMapper(DepartmentMapper.class);
Department department = departmentMapper.getDepByIdWithEmps2(1);
System.out.println(department.getDepName());
123
```

#### 4.1.12、discriminator鉴别器

**准备工作：**修改employee表中部分数据的性别为女

**需求信息**：如果员工是女生就级联查询它所在的部门，如果是男生就只查询男生自己的信息，不级联查询它所在的部门。

**接口方法：**

```java
public Employee getDepWithGirl(Integer id);
1
```

**映射配置：**

```xml
<!-- public Employee getDepWithGirl(Integer id); -->
<resultMap type="com.caochenlei.mybatis.crud.Employee" id="depWithGirl">
	<!-- 设置主键映射 -->
	<id column="id" property="id" />
	<!-- 普通字段映射 -->
	<result column="last_name" property="lastName" />
	<result column="email" property="email" />
	<result column="gender" property="gender" />
	<!-- 级联查询员工 -->
	<discriminator javaType="string" column="gender">
		<case value="女" resultType="com.caochenlei.mybatis.crud.Employee">
			<association property="dep" 
						 select="com.caochenlei.mybatis.mapper.DepartmentMapper.getDepById" 
						 column="dep_id">
			</association>
		</case>
	</discriminator>
</resultMap>
<select id="getDepWithGirl" resultMap="depWithGirl">
	SELECT * FROM `employee` WHERE `id` = #{id}
</select>
123456789101112131415161718192021
```

**测试方法：**

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
Employee employee1 = employeeMapper.getDepWithGirl(2);//2号员工为女生
System.out.println(employee1);
System.out.println(employee1.getDep());

Employee employee2 = employeeMapper.getDepWithGirl(4);//4号员工为男生
System.out.println(employee2);
System.out.println(employee2.getDep());
12345678
```

**控制台输出：**

![image-20200922222140602](https://img-blog.csdnimg.cn/img_convert/95756a8956f1f049e0dd287b4ee17cbd.png)

#### 4.1.13、本章做完最终结构

**工程结构：**

> 注意：如果想要查看源码，请下载配套资料，内含源码！

![image-20200922222435648](https://img-blog.csdnimg.cn/img_convert/5d49c375b6d1ac6a18ff45b0a9d97f99.png)

**数据库结构：**

employee表：

![image-20200922222751745](https://img-blog.csdnimg.cn/img_convert/058b11bdcfe91941cac4a0efb04402cf.png)

department表：

![image-20200922222812350](https://img-blog.csdnimg.cn/img_convert/870564a386385ac98d876c58557a3403.png)

### 4.2、insert, update 和 delete

数据变更语句 insert，update 和 delete 的实现非常接近。

下面是 insert，update 和 delete 语句的结构：

```xml
<insert
  id="insertAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  keyProperty=""
  keyColumn=""
  useGeneratedKeys=""
  timeout="20">
    ...
</insert>    

<update
  id="updateAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">
    ...
</update>    

<delete
  id="deleteAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">
    ...
</delete>    
1234567891011121314151617181920212223242526272829
```

下面是 insert，update 和 delete 语句的属性：

| 属性               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| `id`               | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType`    | 将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。 |
| `flushCache`       | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：（对 insert、update 和 delete 语句）true。 |
| `timeout`          | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。 |
| `statementType`    | 可选 STATEMENT，PREPARED 或 CALLABLE。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `useGeneratedKeys` | （仅适用于 insert 和 update）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系型数据库管理系统的自动递增字段），默认值：false。 |
| `keyProperty`      | （仅适用于 insert 和 update）指定能够唯一识别对象的属性，MyBatis 会使用 getGeneratedKeys 的返回值或 insert 语句的 selectKey 子元素设置它的值，默认值：未设置（`unset`）。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `keyColumn`        | （仅适用于 insert 和 update）设置生成键值在表中的列名，在某些数据库（像 PostgreSQL）中，当主键列不是表中的第一列的时候，是必须设置的。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `databaseId`       | 如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有不带 databaseId 或匹配当前 databaseId 的语句；如果带和不带的语句都有，则不带的会被忽略。 |

下面是 insert，update 和 delete 语句的示例：

```xml
<insert id="insertAuthor">
  insert into Author (id,username,password,email,bio)
  values (#{id},#{username},#{password},#{email},#{bio})
</insert>

<update id="updateAuthor">
  update Author set
    username = #{username},
    password = #{password},
    email = #{email},
    bio = #{bio}
  where id = #{id}
</update>

<delete id="deleteAuthor">
  delete from Author where id = #{id}
</delete>
1234567891011121314151617
```

如前所述，插入语句的配置规则更加丰富，在插入语句里面有一些额外的属性和子元素用来处理主键的生成，并且提供了多种生成方式。

首先，如果你的数据库支持自动生成主键的字段（比如 MySQL 和 SQL Server），那么你可以设置 useGeneratedKeys=”true”，然后再把 keyProperty 设置为目标属性就 OK 了。例如，如果上面的 Author 表已经在 id 列上使用了自动生成，那么语句可以修改为：

```xml
<insert id="insertAuthor" useGeneratedKeys="true" keyProperty="id">
  insert into Author (username,password,email,bio)
  values (#{username},#{password},#{email},#{bio})
</insert>
1234
```

如果你的数据库还支持多行插入, 你也可以传入一个 `Author` 数组或集合，并返回自动生成的主键。

```xml
<insert id="insertAuthor" useGeneratedKeys="true" keyProperty="id">
  insert into Author (username, password, email, bio) values
  <foreach item="item" collection="list" separator=",">
    (#{item.username}, #{item.password}, #{item.email}, #{item.bio})
  </foreach>
</insert>
123456
```

对于不支持自动生成主键列的数据库和可能不支持自动生成主键的 JDBC 驱动，MyBatis 有另外一种方法来生成主键。

这里有一个简单（也很傻）的示例，它可以生成一个随机 ID（不建议实际使用，这里只是为了展示 MyBatis 处理问题的灵活性和宽容度）：

```xml
<insert id="insertAuthor">
  <selectKey keyProperty="id" resultType="int" order="BEFORE">
    select CAST(RANDOM()*1000000 as INTEGER) a from SYSIBM.SYSDUMMY1
  </selectKey>
  insert into Author
    (id, username, password, email,bio, favourite_section)
  values
    (#{id}, #{username}, #{password}, #{email}, #{bio}, #{favouriteSection,jdbcType=VARCHAR})
</insert>
123456789
```

实际上，对于Oracle数据库，我们可以采用序列的方式进行主键自增长，例如：

```xml
<insert id="insertAuthor">
  <selectKey keyProperty="id" resultType="Integer" order="BEFORE">
    select AUTHOR_SEQ.nextval from dual
  </selectKey>
  insert into Author
    (id, username, password, email,bio, favourite_section)
  values
    (#{id}, #{username}, #{password}, #{email}, #{bio}, #{favouriteSection,jdbcType=VARCHAR})
</insert>
123456789
```

在上面的示例中，首先会运行 selectKey 元素中的语句，并设置 Author 的 id，然后才会调用插入语句。这样就实现了数据库自动生成主键类似的行为，同时保持了 Java 代码的简洁。

selectKey 元素描述如下：

```xml
<selectKey
  keyProperty="id"
  resultType="int"
  order="BEFORE"
  statementType="PREPARED">
    ...
</selectKey>    
1234567
```

selectKey 属性描述如下：

| 属性            | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| `keyProperty`   | `selectKey` 语句结果应该被设置到的目标属性。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `keyColumn`     | 返回结果集中生成列属性的列名。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `resultType`    | 结果的类型。通常 MyBatis 可以推断出来，但是为了更加准确，写上也不会有什么问题。MyBatis 允许将任何简单类型用作主键的类型，包括字符串。如果生成列不止一个，则可以使用包含期望属性的 Object 或 Map。 |
| `order`         | 可以设置为 `BEFORE` 或 `AFTER`。如果设置为 `BEFORE`，那么它首先会生成主键，设置 `keyProperty` 再执行插入语句。如果设置为 `AFTER`，那么先执行插入语句，然后是 `selectKey` 中的语句 - 这和 Oracle 数据库的行为相似，在插入语句内部可能有嵌入索引调用。 |
| `statementType` | 和前面一样，MyBatis 支持 `STATEMENT`，`PREPARED` 和 `CALLABLE` 类型的映射语句，分别代表 `Statement`, `PreparedStatement` 和 `CallableStatement` 类型。 |

### 4.3、参数获取

**单个参数：**

```
mybatis不会做特殊处理，#{参数名/任意名}，取出参数值。
1
```

**多个参数：**

```
mybatis会做特殊处理，多个参数会被封装成一个map，#{key}就是从map中获取指定key的值。
	key：当全局配置参数useActualParamName为true，使用#{arg0},...,#{argN}或者#{param1},...,#{paramN}来获取。
	     当全局配置参数useActualParamName为false，使用#{0},...,#{N}或者#{param1},...,#{paramN}来获取。
	     注意：自从3.4.1版本以后，useActualParamName默认就为true。
	value：传入的参数值。
12345
```

**命名参数：**

```
使用@Param参数注解明确指定封装参数时map的key；@Param("id")，多个参数会被封装成一个map。
	key：使用@Param注解指定的值。
	value：传入的参数值。
123
```

**POJO：**

```
如果多个参数正好是我们业务逻辑的数据模型，我们就可以直接传入pojo，#{属性名}就是取出传入的pojo的属性值。
1
```

**MAP：**

```
如果多个参数不是业务模型中的数据，没有对应的pojo，不经常使用，为了方便，我们也可以传入map，#{key}就是取出map中对应的值。
1
```

> 注意：如果多个参数不是业务模型中的数据，但是经常要使用，推荐来编写一个TO（Transfer Object）数据传输对象。

**Collection或数组：**

```
如果是Collection（List、Set）类型或者是数组，也会特殊处理，也是把传入的Collection（List、Set）或者数组封装在map中。
	key：对于单一参数Collection使用（collection、list）、数组使用（array）来获取，例如：Collection使用#{collection[0]},...#{collection[N]}，List集合也可以使用#{list[0]},...#{list[N]}，数组可以使用#{array[0]},...,#{array[N]}这种形式来获取键所对应的值。
	value：集合或数组索引所对应的值。
123
```

> 注意：启用后`useActualParamName`，可以使用其实际参数名称引用单个`List`或`Collection`类型参数。 为了使用该特性，你的项目必须采用 Java 8 编译，并且加上 -parameters 选项。(Since: 3.4.1)

**#{} 和 ${} 的区别：**

相同点：

```
#{}：可以获取map中的值或者pojo对象属性的值。
${}：可以获取map中的值或者pojo对象属性的值。
12
```

不同点：

```
#{}：是以预编译的形式，将参数设置到sql语句中，防止sql注入。
${}：取出的值直接拼装在sql语句中，会有安全问题。
因此大多情况下，我们取参数的值都应该去使用#{}，除了一些特定场景，需要在预编译前拼接sql语句的情况，比如按照年份分表查询。同时#{}它支持更丰富的用法：规定参数的一些规则：javaType、jdbcType、mode（存储过程）、numericScale、resultMap、typeHandler、jdbcTypeName，例如：#{email,jdbcType=OTHER}。
123
```

### 4.4、sql片段

这个元素可以用来定义可重用的 SQL 代码片段，以便在其它语句中使用。 参数可以静态地（在加载的时候）确定下来，并且可以在不同的 include 元素中定义不同的参数值。比如：

```xml
<sql id="userColumns"> ${alias}.id,${alias}.username,${alias}.password </sql>
1
```

这个 SQL 片段可以在其它语句中使用，例如：

```xml
<select id="selectUsers" resultType="map">
  select
    <include refid="userColumns"><property name="alias" value="t1"/></include>,
    <include refid="userColumns"><property name="alias" value="t2"/></include>
  from some_table t1 cross join some_table t2
</select>
123456
```

也可以在 include 元素的 refid 属性或内部语句中使用属性值，例如：

```xml
<sql id="sometable">
  ${prefix}Table
</sql>

<sql id="someinclude">
  from
    <include refid="${include_target}"/>
</sql>

<select id="select" resultType="map">
  select
    field1, field2, field3
  <include refid="someinclude">
    <property name="prefix" value="Some"/>
    <property name="include_target" value="sometable"/>
  </include>
</select>
1234567891011121314151617
```

## 第五章 MyBatis3的动态SQL

### 5.1、概述

动态 SQL 是 MyBatis 的强大特性之一。如果你使用过 JDBC 或其它类似的框架，你应该能理解根据不同条件拼接 SQL 语句有多痛苦，例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。利用动态 SQL，可以彻底摆脱这种痛苦。

使用动态 SQL 并非一件易事，但借助可用于任何 SQL 映射语句中的强大的动态 SQL 语言，MyBatis 显著地提升了这一特性的易用性。

如果你之前用过 JSTL 或任何基于类 XML 语言的文本处理器，你对动态 SQL 元素可能会感觉似曾相识。在 MyBatis 之前的版本中，需要花时间了解大量的元素。借助功能强大的基于 OGNL 的表达式，MyBatis 3 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少。

- if
- trim (where, set)
- choose (when, otherwise)
- foreach

### 5.2、select (if, where, set)

**需求信息**：查询员工，要求携带了哪个字段查询条件就带上这个字段的值。

**查询方式一：**

我们可以使用if标签来对每个属性进行判断，看它是不是为空，如果为空说明不需要查询这个字段，不为空则需要查询。

```xml
<!-- public List<Employee> getEmpsByConditionIf1(); -->
<select id="getEmpsByConditionIf1" resultType="com.caochenlei.mybatis.crud.Employee">
	select * from employee where
	<if test="id!=null">
		id = #{id}
	</if>
	<if test="lastName!=null">
		and last_name like #{lastName}
	</if>
	<if test="email!=null">
		and email like #{email}
	</if>
	<if test="gender!=null">
		and gender like #{gender}
	</if>
	<if test="dep!=null">
		and dep_id = #{dep.depId}
	</if>
</select>
12345678910111213141516171819
```

**查询方式二：**

但是，上述的方法它有一个很大的弊端，一旦id属性没有设置，而其它属性设置了，这个查询就会报错了，因为在where后边直接连接了一个and语句，这显然是不合法的，所以，大多数公司采用以下这种方式：

```xml
<!-- public List<Employee> getEmpsByConditionIf2(); -->
<select id="getEmpsByConditionIf2" resultType="com.caochenlei.mybatis.crud.Employee">
	select * from employee where 1 = 1
	<if test="id!=null">
		and id = #{id}
	</if>
	<if test="lastName!=null">
		and last_name like #{lastName}
	</if>
	<if test="email!=null">
		and email like #{email}
	</if>
	<if test="gender!=null">
		and gender like #{gender}
	</if>
	<if test="dep!=null">
		and dep_id = #{dep.depId}
	</if>
</select>
12345678910111213141516171819
```

**查询方式三：**

但是，以上的写法也是有问题的，在sql查询的时候，无缘无故的多加了一个 1 = 1，显然这跟我们的业务并没有什么关系，那有没有一种既能解决where后有and报错问题，又不用写多余的代码呢？答案是肯定的，我们只要使用where标签来代替where语句即可，例如：

```xml
<!-- public List<Employee> getEmpsByConditionIf3(); -->
<select id="getEmpsByConditionIf3" resultType="com.caochenlei.mybatis.crud.Employee">
	select * from employee
	<where>
		<if test="id!=null">
			and id = #{id}
		</if>
		<if test="lastName!=null">
			and last_name like #{lastName}
		</if>
		<if test="email!=null">
			and email like #{email}
		</if>
		<if test="gender!=null">
			and gender like #{gender}
		</if>
		<if test="dep!=null">
			and dep_id = #{dep.depId}
		</if>
	</where>
</select>
123456789101112131415161718192021
```

------

**需求信息**：更新员工，要求携带了哪个字段就更新哪个字段的值。

**更新方式：**

通过以上三个例子，我们是不是可以类比实现update更新，那我们来看看一个最佳的update更新代码片段是怎样写的？

```xml
<!-- public boolean updateEmpByConditionIf(Employee employee); -->
<update id="updateEmpByConditionIf">
	update employee
	<set>
		<if test="lastName!=null">
			last_name = #{lastName},
		</if>
		<if test="email!=null">
			email = #{email},
		</if>
		<if test="gender!=null">
			gender = #{gender},
		</if>
		<if test="dep!=null">
			dep_id = #{dep.depId},
		</if>
	</set>
	where id = #{id}
</update>
12345678910111213141516171819
```

### 5.3、trim (where, set)

通过以上的案例，我们学会了if标签的使用以及where标签的使用，但是，我们想一个问题，现在的and是在每一个条件的前边，那我要是想要把and放到每一个标签的后边，那是不是也可以呢（where标签会不会自动给我处理了多余的and）？显然这是不现实的，where标签只能处理and在条件前边的情况，那我非要实现这种情况又该怎么办呢？这时候，我们就要使用trim标签了，它的功能更加强大，例如：

```xml
<!-- public List<Employee> getEmpsByConditionTrim(Employee employee); -->
<select id="getEmpsByConditionTrim" resultType="com.caochenlei.mybatis.crud.Employee">
	select * from employee
	<!-- 
		trim：
			prefix：给拼串后的整个字符串加一个前缀
			prefixOverrides：去掉整个字符串前面多余的字符，支持或（|）
			suffix：给拼串后的整个字符串加一个后缀
			suffixOverrides：去掉整个字符串后面多余的字符，支持或（|）
	 -->
	<trim prefix="where" suffixOverrides="AND | OR">
		<if test="id!=null">
			id = #{id} and
		</if>
		<if test="lastName!=null">
			last_name like #{lastName} and
		</if>
		<if test="email!=null">
			email like #{email} and
		</if>
		<if test="gender!=null">
			gender like #{gender} and
		</if>
		<if test="dep!=null">
			dep_id = #{dep.depId} and
		</if>
	</trim>
</select>
12345678910111213141516171819202122232425262728
```

以上的代码正确的演示了一个查询语句where的使用，那么，如果我想要通过trim更新数据库中的记录，那又该怎么做呢？没错，我想你心里可能已经有想法了，来看看是不是：

```xml
<!-- public boolean updateEmpByConditionTrim(Employee employee); -->
<update id="updateEmpByConditionTrim">
	update employee
	<!-- 
		trim：
			prefix：给拼串后的整个字符串加一个前缀
			prefixOverrides：去掉整个字符串前面多余的字符，支持或（|）
			suffix：给拼串后的整个字符串加一个后缀
			suffixOverrides：去掉整个字符串后面多余的字符，支持或（|）
	 -->
	<trim prefix="set" suffixOverrides=",">
		<if test="lastName!=null">
			last_name = #{lastName},
		</if>
		<if test="email!=null">
			email = #{email},
		</if>
		<if test="gender!=null">
			gender = #{gender},
		</if>
		<if test="dep!=null">
			dep_id = #{dep.depId},
		</if>
	</trim>
	where id = #{id}
</update>
1234567891011121314151617181920212223242526
```

### 5.4、choose (when, otherwise)

**需求信息**：如果带了id就用id查，如果带了lastName就用lastName查，只会进入其中一个，如果一个都没有，默认查询所有女生信息，这样又该如何实现呢？

**实现方式：**

有时候，我们不想使用所有的条件，而只是想从多个条件中选择一个使用。针对这种情况，MyBatis 提供了 choose 元素，它有点像 Java 中的 switch 语句。

```xml
<!-- public List<Employee> getEmpsByConditionChoose(Employee employee); -->
<select id="getEmpsByConditionChoose" resultType="com.caochenlei.mybatis.crud.Employee">
	select * from employee
	<where>
		<choose>
			<when test="id!=null">
				id = #{id}
			</when>
			<when test="lastName!=null">
				last_name like #{lastName}
			</when>
			<when test="email!=null">
				email = #{email}
			</when>
			<otherwise>
				gender = '女'
			</otherwise>
		</choose>
	</where>
</select>
1234567891011121314151617181920
```

其实以上的情况为什么不使用if进行判断呢？因为单纯的使用if判断，不适宜默认情况的执行，这时候使用choose (when, otherwise)就非常合适了。

### 5.5、foreach

动态 SQL 的另一个常见使用场景是对集合进行遍历（尤其是在构建 IN 条件语句的时候）。比如：

```xml
<!-- public List<Employee> getEmpsByConditionForeach(@Param("ids") List<Integer> ids); -->
<select id="getEmpsByConditionForeach" resultType="com.caochenlei.mybatis.crud.Employee">
	select * from employee
	<!-- 
		foreach：
			collection：指定要遍历的集合（list类型的参数会特殊处理封装在map中，map的key就叫list）
			item：将当前遍历出的元素赋值给指定的变量
			separator：每个元素之间的分隔符
			open：遍历出所有结果拼接一个开始的字符串
			close：遍历出所有结果拼接一个结束的字符串
			index：遍历list的时候，index就是索引，item就是当前值
				      遍历map的时候，index就是map的key，item就是map[key]的值
				      
		#{变量名}就能取出变量的值也就是当前遍历出的元素					      
	 -->
	<foreach collection="ids" item="item_id" separator="," open="where id in(" close=")">
		#{item_id}
	</foreach>
</select>
12345678910111213141516171819
```

除了可以用于构建IN查询，还可以对数据进行批量插入。例如：

```xml
<!-- public void addEmps(@Param("emps")List<Employee> emps); -->
<insert id="addEmps">
	insert into employee (id,last_name,email,gender,dep_id) 
	values
	<foreach collection="emps" item="emp" separator=",">
		(#{emp.id},#{emp.lastName},#{emp.email},#{emp.gender},#{emp.dep.depId})
	</foreach>
</insert>
12345678
```

但是，此时又会出现问题，因为mysql支持values()，而Oracle不支持values()，那么如果要是向oracle数据库中进行批量插入，我们又该如何实现呢？

**方式一：**

```xml
<!-- public void addEmpsForOracle1(@Param("emps") List<Employee> emps); -->
<insert id="addEmpsForOracle1" databaseId="oracle">
	<foreach collection="emps" item="emp" open="begin" close="end;">
		insert into employee (id,last_name,email,gender)
		values(employees_seq.nextval,#{emp.lastName},#{emp.email},#{emp.gender});
	</foreach>
</insert>
1234567
```

**方式二：**

```xml
<!-- public void addEmpsForOracle2(@Param("emps") List<Employee> emps); -->
<insert id="addEmpsForOracle2" databaseId="oracle">
	insert into employee (id,last_name,email,gender)
	<foreach collection="emps" item="emp" separator="union" 
		open="select employees_seq.nextval,lastName,email,gender from(" 
		close=")">
		select #{emp.lastName} lastName,#{emp.email} email,#{emp.gender} gender from dual
	</foreach>
</insert>
123456789
```

### 5.6、两个内置参数

mybatis默认还有两个内置参数：

- _parameter：代表整个参数
  - 单个参数：_parameter就是这个参数
  - 多个参数：参数会被封装为一个map，_parameter就是代表这个map
- _databaseId：如果配置了databaseIdProvider标签，那么databaseId就是代表当前数据库的别名，比如：oracle

### 5.7、bind

bind：可以将OGNL表达式的值绑定到一个变量中，方便后来引用这个变量的值。

**需求信息**：我们现在要针对不同的数据库使用不同的模糊查询语句，而且一个使用bind绑定变量后引用，而另一个则是直接使用内置对象进行引用，如下：

```xml
<!-- public List<Employee> getEmpsTestInnerParameter(Employee employee); -->
<select id="getEmpsTestInnerParameter" resultType="com.caochenlei.mybatis.crud.Employee">
	<bind name="_lastName" value="'%'+lastName+'%'" />
	<if test="_databaseId=='mysql'">
		select * from employee
		<if test="_parameter!=null">
			<!-- 使用bind绑定的变量_lastName -->
			where last_name like #{_lastName}
		</if>
	</if>
	<if test="_databaseId=='oracle'">
		select * from employee
		<if test="_parameter!=null">
			<!-- 使用内置对象_parameter -->
			where last_name like #{_parameter.lastName}
		</if>
	</if>
</select>
123456789101112131415161718
```

## 第六章 MyBatis3的缓存机制

### 6.1、概述

MyBatis 包含一个非常强大的查询缓存特性，它可以非常方便地配置和定制，缓存可以极大的提升查询效率。

MyBatis系统中默认定义了两级缓存，它们分别是：一级缓存和二级缓存。

1. 默认情况下，只有一级缓存（sqlSession级别的缓存，也称为本地缓存）开启。
2. 二级缓存需要手动开启和配置，它是基于namespace级别的缓存。
3. 为了提高扩展性，MyBatis定义了缓存接口Cache，我们可以通过实现Cache接口来自定义二级缓存。

### 6.2、一级缓存

一级缓存(local cache)，即本地缓存，作用域默认为 sqlSession。当 Session flush 或 close 后，该 Session 中的所有 Cache 将被清空。

本地缓存不能被关闭，但可以调用 clearCache() 来清空本地缓存或者改变缓存的作用域。

在 mybatis3.1 之后，可以配置本地缓存的作用域.，在settings 中配置如下：

| 设置            | 描述                                                         | 取值                 | 默认值  |
| --------------- | ------------------------------------------------------------ | -------------------- | ------- |
| localCacheScope | MyBatis 利用本地缓存机制（Local Cache）防止循环引用（circular references）和加速关联复嵌套査询。 SESSION：这种情况下会缓存一个会话中执行的所有查询。 STATEMENT：代表本地会话仅用在语句执行上，对相同 SqlScssion 的不同调用将不会共享数据。 | SESSION \| STATEMENT | SESSION |

一级缓存失效的几种情况：

- sqlSession不同
- sqlSession相同，查询条件不同（当前一级缓存中还没有这个数据）
- sqlSession相同，两次查询之间执行了增删改操作（这次增删改可能对当前数据有影响）
- sqlSession相同，手动清除了一级缓存（缓存清空）

### 6.3、二级缓存

二级缓存（second level cache），它是基于namespace级别的全局作用域缓存，二级缓存默认不开启，需要手动配置，要启用全局的二级缓存，只需要在你的 SQL 映射文件中添加一行：

```xml
<cache />
1
```

基本上就是这样，这个简单语句的效果如下：

- 映射语句文件中的所有 select 语句的结果将会被缓存。
- 映射语句文件中的所有 insert、update 和 delete 语句会刷新缓存。
- 缓存会使用最近最少使用算法（LRU, Least Recently Used）算法来清除不需要的缓存。
- 缓存不会定时进行刷新（也就是说，没有刷新间隔）。
- 缓存会保存列表或对象（无论查询方法返回哪种）的 1024 个引用。
- 缓存会被视为读/写缓存，这意味着获取到的对象并不是共享的，可以安全地被调用者修改，而不干扰其它调用者或线程所做的潜在修改。

> 注意：缓存只作用于 cache 标签所在的映射文件中的语句。如果你混合使用 Java API 和 XML 映射文件，在共用接口中的语句将不会被默认缓存，你需要使用 @CacheNamespaceRef 注解指定缓存作用域。

这些属性可以通过 cache 元素的属性来修改。比如：

```xml
<!-- 
	cache：
		eviction：缓存的回收策略。
			• LRU – 最近最少使用：移除最长时间不被使用的对象。（默认值）
			• FIFO – 先进先出：按对象进入缓存的顺序来移除它们。
			• SOFT – 软引用：基于垃圾回收器状态和软引用规则移除对象。
			• WEAK – 弱引用：更积极地基于垃圾收集器状态和弱引用规则移除对象。
		flushInterval：刷新间隔，该属性可以被设置为任意的正整数，单位为毫秒， 默认情况是不设置。
		size：引用数目，该属性可以被设置为任意正整数，要注意欲缓存对象的大小和运行环境中可用的内存资源，默认值是 1024。
		readOnly：只读，该属性可以被设置为 true 或 false。只读的缓存会给所有调用者返回缓存对象的相同实例。 
		type：指定自定义缓存的全类名，实现Cache接口即可。
 -->
<cache eviction="LRU"
		flushInterval="60000"
		size="1024"
		readOnly="true"
		type="" />
1234567891011121314151617
```

> 注意：二级缓存是事务性的。这意味着，当 sqlSession 完成并提交时，或是完成并回滚，但没有执行 flushCache=true 的 insert/delete/update 语句时，缓存会获得更新。

你可能会想要在多个命名空间中共享相同的缓存配置和实例，要实现这种需求，你可以使用 cache-ref 元素来引用另一个缓存。

```xml
<cache-ref namespace="com.someone.application.data.SomeMapper"/>
1
```

那么我们简单梳理一下，二级缓存的使用步骤：

1. 开启全局二级缓存配置：<setting name=“cacheEnabled” value=“true”/>
2. 去mapper.xml中配置使用二级缓存：<cache />
3. 我们的POJO需要实现序列化接口

### 6.4、和缓存有关的设置

**mybatis-config.xml的settings标签中的配置：**

- <setting name=“localCacheScope” value=“SESSION”/>，本地缓存（一级缓存）作用域，当前会话的所有数据保存在会话缓存中，如果值为STATEMENT可以禁用一级缓存
- <setting name=“cacheEnabled” value=“true”/>，二级缓存开启和关闭的开关，如果为true则开启二级缓存

**xxxxxMapper.xml的每个select标签中的配置：**

- useCache=“true”：默认值为true，如果值为false代表select标签不使用缓存（一级缓存可使用，二级缓存不使用）
- flushCache=“false”：默认值为false，如果值为false代表select标签不清除缓存

**xxxxxMapper.xml的每个insert、update、delete标签中的配置：**

- flushCache=“true”：默认值为true，如果值为true代表清除缓存（一级二级都会清除）

**sqlSession的调用代码中：**

- sqlSession.clearCache()：只是清除当前session的一级缓存，而二级缓存不会清除

### 6.5、自定义缓存的方法

除了上述自定义缓存属性的方式，你也可以通过实现你自己的缓存，或为其它第三方缓存方案创建适配器，来完全覆盖缓存行为。

```xml
<cache type="com.domain.something.MyCustomCache"/>
1
```

这个示例展示了如何使用一个自定义的缓存实现。type 属性指定的类必须实现 org.apache.ibatis.cache.Cache 接口，且提供一个接受 String 参数作为 id 的构造器。 这个接口是 MyBatis 框架中许多复杂的接口之一，但是行为却非常简单。

```java
public interface Cache {
  String getId();
  int getSize();
  void putObject(Object key, Object value);
  Object getObject(Object key);
  boolean hasKey(Object key);
  Object removeObject(Object key);
  void clear();
}
123456789
```

为了对你的缓存进行配置，只需要简单地在你的缓存实现中添加公有的 JavaBean 属性，然后通过 cache 元素传递属性值，例如，下面的例子将在你的缓存实现上调用一个名为 `setCacheFile(String file)` 的方法：

```xml
<cache type="com.domain.something.MyCustomCache">
  <property name="cacheFile" value="/tmp/my-custom-cache.tmp"/>
</cache>
123
```

你可以使用所有简单类型作为 JavaBean 属性的类型，MyBatis 会进行转换。 你也可以使用占位符（如 ${cache.file}），以便替换成在配置文件属性中定义的值。

从版本 3.4.2 开始，MyBatis 已经支持在所有属性设置完毕之后，调用一个初始化方法。 如果想要使用这个特性，请在你的自定义缓存类里实现 `org.apache.ibatis.builder.InitializingObject` 接口。

```java
public interface InitializingObject {
  void initialize() throws Exception;
}
123
```

> 注意：上一节中对缓存的配置（如清除策略、可读或可读写等），不能应用于自定义缓存。

请注意，缓存的配置和缓存实例会被绑定到 SQL 映射文件的命名空间中。 因此，同一命名空间中的所有语句和缓存将通过命名空间绑定在一起。 每条语句可以自定义与缓存交互的方式，或将它们完全排除于缓存之外，这可以通过在每条语句上使用两个简单属性来达成。 默认情况下，语句会这样来配置：

```xml
<select ... flushCache="false" useCache="true"/>
<insert ... flushCache="true"/>
<update ... flushCache="true"/>
<delete ... flushCache="true"/>
1234
```

鉴于这是默认行为，显然你永远不应该以这样的方式显式配置一条语句。但如果你想改变默认的行为，只需要设置 flushCache 和 useCache 属性。比如，某些情况下你可能希望特定 select 语句的结果排除于缓存之外，或希望一条 select 语句清空缓存。类似地，你可能希望某些 update 语句执行时不要刷新缓存。

### 6.6、与第三方缓存整合

**前导部分：**

接下来可能需要下载一些软件包，如果你没兴趣，或者，你怕版本有冲突，你可以直接使用我提供好的软件包，而且我也建议你使用我提供的，下载流程了解即可。

**整合方法：**

1. 导入第三方缓存包

2. 导入与第三方缓存整合的适配包

3. mapper.xml中使用自定义缓存，例如：

   - ```xml
     <cache type="org.mybatis.caches.ehcache.EhcacheCache"></cache>
     1
     ```

4. 导入第三方缓存包所依赖的配置文件

**案例演示：**整合ehcache

第一步：去ehcache官网，下载第三方缓存包，[点击打开官网](http://www.ehcache.org/)，然后往下拉，找到整合包

![image-20200925184317968](https://img-blog.csdnimg.cn/img_convert/7553dfd9701927edbedc6dff353b2301.png)

点击下载：

![image-20200925185026536](https://img-blog.csdnimg.cn/img_convert/aade3bf7a0712384db6e5e6d8fad0544.png)

右键解压：

![image-20200925185523042](https://img-blog.csdnimg.cn/img_convert/a28296e37fab6acce1c4cdd4a1a19780.png)

拷贝jar包到工程的lib文件夹中，然后Build Path：

![image-20200925185627081](https://img-blog.csdnimg.cn/img_convert/443fff2df3c10209cb17868103a12210.png)

第二步：去mybatis官网，下载第三方整合包，[点击打开官网](https://github.com/mybatis/)，然后往下拉，找到整合包

![image-20200925172933951](https://img-blog.csdnimg.cn/img_convert/cce73b0e9d9271136ea215b309ff6e83.png)

点击进去后，找到最近发布的新版本，如下图所示：

![image-20200925173030385](https://img-blog.csdnimg.cn/img_convert/005d06920cf5371dd513d48c4252612c.png)

点击下载：

![image-20200925182719530](https://img-blog.csdnimg.cn/img_convert/c346b4c6ae02408082c68e1ce871f339.png)

右键解压：

![image-20200925182955545](https://img-blog.csdnimg.cn/img_convert/dffd4beda4f992fd6a71e88f0c5a96fc.png)

进入目录：进行安装，如果你不会maven，你就直接使用我的配套资料中提供的已经编译好的版本

![image-20200925183637311](https://img-blog.csdnimg.cn/img_convert/895eafaca98c3f155683e09f0e58d175.png)

编译完成，在target文件夹中会出现mybatis-ehcache-1.2.1.jar就是mybatis和ehcache的整合包

![image-20200925183925698](https://img-blog.csdnimg.cn/img_convert/df3e8d78a33706cec4be38ad60462148.png)

找到安装后的jar包，然后拷贝到工程的lib文件夹中，然后Build Path一下：

![image-20200925190001299](https://img-blog.csdnimg.cn/img_convert/a6dee7bc4fc5334c93a83cbe878c7783.png)

第三步：开启自定义缓存，我们现在EmployeeMapper.xml中添加

```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache"></cache>
1
```

第四步：编辑ehcache配置文件，在src目录下，新建一个ehcache.xml文件，把以下代码拷贝进去：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd">
 <!-- 磁盘保存路径 -->
 <diskStore path="d:\ehcache" />
 
 <!-- 缓存配置选项 -->
 <defaultCache 
   maxElementsInMemory="1" 
   maxElementsOnDisk="10000000"
   eternal="false" 
   overflowToDisk="true" 
   timeToIdleSeconds="120"
   timeToLiveSeconds="120" 
   diskPersistent="false"
   diskExpiryThreadIntervalSeconds="120"
   memoryStoreEvictionPolicy="LRU">
 </defaultCache>
</ehcache>
<!-- 
属性说明：
	diskStore：指定数据在磁盘中的存储位置。
	defaultCache：当借助CacheManager.add("demoCache")创建Cache时，EhCache便会采用<defalutCache/>指定的的管理策略
 
以下属性是必须的：
	maxElementsInMemory - 在内存中缓存的element的最大数目 
	maxElementsOnDisk - 在磁盘上缓存的element的最大数目，若是0表示无穷大
	eternal - 设定缓存的elements是否永远不过期。如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断
	overflowToDisk - 设定当内存缓存溢出的时候是否将过期的element缓存到磁盘上
 
以下属性是可选的：
	timeToIdleSeconds - 当缓存在EhCache中的数据前后两次访问的时间超过timeToIdleSeconds的属性取值时，这些数据便会删除，默认值是0，也就是可闲置时间无穷大
	timeToLiveSeconds - 缓存element的有效生命期，默认是0，也就是element存活时间无穷大
	diskSpoolBufferSizeMB - 这个参数设置DiskStore(磁盘缓存)的缓存区大小，默认是30MB，每个Cache都应该有自己的一个缓冲区
	diskPersistent - 在VM重启的时候是否启用磁盘保存EhCache中的数据，默认是false。
	diskExpiryThreadIntervalSeconds - 磁盘缓存的清理线程运行间隔，默认是120秒。每个120s，相应的线程会进行一次EhCache中数据的清理工作
	memoryStoreEvictionPolicy - 当内存缓存达到最大，有新的element加入的时候， 移除缓存中element的策略。默认是LRU（最近最少使用），可选的有LFU（最不常使用）和FIFO（先进先出）
 -->
1234567891011121314151617181920212223242526272829303132333435363738
```

第五步：找到EmployeeTest.java的getEmpByIdTest方法，测试一下查询语句

![image-20200925191732319](https://img-blog.csdnimg.cn/img_convert/6660d461229abf2461906847f004fb95.png)

我们到我们之前指定的文件夹中去看看有没有缓存文件

![image-20200925191822685](https://img-blog.csdnimg.cn/img_convert/44c7081a4c46ed2e11a43e98a6e639cf.png)

------

![image-20200925191849438](https://img-blog.csdnimg.cn/img_convert/fbd483e78d80e4a2ddead28d041b9998.png)

我们会发现它确实已经进行了缓存，而且文件名是namespace的命名空间，这也证实了一点，二级缓存（second level cache），它是基于namespace级别的全局作用域缓存，如果你测试了，发现文件夹没有任何东西，可能你的`<setting name="cacheEnabled" value="true"/>`的值是false或者没有配置，默认它是true，如果你的版本和我的版本不一致，它就可能默认是有变化，所以，我们建议，用到哪些属性，就显示的声明一下，这样无论版本有没有变化默认值，都不影响使用。

## 第七章 MyBatis3的逆向工程

### 7.1、概述

MyBatis生成器（MBG）是MyBatis的代码生成器。它将为MyBatis的所有版本生成代码。它将内省一个数据库表（或多个表），并将生成可用于访问表的工件。这减轻了设置对象和配置文件以与数据库表进行交互的麻烦。MBG试图对简单CRUD（创建，查询，更新，删除）的大部分数据库操作产生重大影响。您仍将需要手工编写SQL和对象代码以进行联接查询或存储过程。

MBG会根据其配置方式以不同的样式和不同的语言生成代码。例如，MBG可以生成Java或Kotlin代码。MBG可以生成MyBatis3兼容的XML，尽管现在认为MBG是旧版使用，生成的代码的较新样式不需要XML。

当作为Eclipse功能运行时，生成器还可以合并Java文件并将用户修改保存到生成的Java文件中。生成器使用Eclipse Java分析器和AST walker来完成此任务。Eclipse功能还具有一些用户界面增强功能，这些功能使生成器的运行更加容易。最后，Eclipse功能为Eclipse帮助系统提供了完整的生成器用户手册。

Eclipse功能可在Eclipse市场上找到：https://marketplace.eclipse.org/content/mybatis-generator

MBG除了JRE之外没有其它依赖项，需要Java 8或更高版本，另外，还需要一个实现DatabaseMetaData接口的JDBC驱动程序。

### 7.2、下载

逆向工程：[点击打开](https://github.com/mybatis/generator)

打开逆向工程的网页地址，在右侧导航栏找到“Release”，目前我们使用的是MyBatis Generator Release 1.4.0，因为会时常更新，建议和本教程采用版本一致。

![image-20200920152353414](https://img-blog.csdnimg.cn/img_convert/44d0c38dd73fe9a657da096a45735181.png)

打开新页面后，往下拉，找到这里。

![image-20200920153823093](https://img-blog.csdnimg.cn/img_convert/d5e277bed43656e11a55e7a15a814d3c.png)

### 7.3、配置详解

#### 7.3.1、generatorConfiguration

<generatorConfiguration>元素是MyBatis Generator配置文件的根元素。该文件应包含以下DOCTYPE：

```xml
<！DOCTYPE generatorConfiguration PUBLIC
  “-// mybatis.org//DTD MyBatis Generator配置1.0 // EN”
  “ http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd”>
123
```

**必填属性：**无

**可选属性：**无

**子元素：**

- <properties> (0 or 1)
- <classPathEntry> (0…N)
- <context> (1…N)

#### 7.3.2、properties

<properties>元素用于指定用于配置解析的外部属性文件。配置中的任何属性都将接受`$ {property}`形式的`属性`。将在指定的属性文件中搜索匹配值，并将替换该匹配值。该属性文件是Java属性文件的常规格式。

如果此处指定的属性与系统属性之间发生名称冲突，则系统属性将获胜。

<properties>元素是<generatorConfiguration> 元素的子元素。

**必填属性：**以下属性之一是唯一的。

| 属性     | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| resource | 属性文件的限定名称。指定资源时，将在类路径中搜索属性文件。因此，在`com.myproject`程序包中必须存在指定为`com/myproject/generatorConfig.properties`的文件 。 |
| url      | 用于属性文件的URL值。当以`file:///C:/myfolder/generatorConfig.properties`之类的形式使用时，可用于在文件系统上的特定位置指定属性文件 。 |

**可选属性：**无

**子元素：**无

#### 7.3.3、classPathEntry

<classPathEntry>元素用于将类路径位置添加到MyBatis Generator（MBG）运行的类路径中。在<classPathEntry>元素是一个选项的子元素<generatorConfiguration>元素。MBG在以下情况下从这些位置加载类：

- 加载用于数据库自省的JDBC驱动程序时
- 在JavaModelGenerator中加载根类以检查重写的方法时

如果您在MBG外部设置类路径（例如，使用`java`命令的`-cp`参数），则此元素是可选的，但不是必需的

**重要说明：**加载扩展MBG类之一或实现MBG接口之一的类时，不会使用这些位置。在这些情况下，您必须以将MBG添加到类路径的相同方式（例如，使用`java`命令的`-cp`参数 ）将外部类添加到运行时类路径。

**必填属性：**

| 属性     | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| location | 要添加到类路径的JAR / ZIP文件的完整路径名，或要添加到类路径的目录的全路径名。 |

**可选属性：**无

**子元素：**无

#### 7.3.4、context

<context>元素用于指定生成一组对象的环境。子元素用于指定要连接的数据库，要生成的对象的类型以及要进行内省的表。可以在<generatorConfiguration> 元素内列出多个<context>元素， 以允许在MyBatis Generator（MBG）的同一运行中从不同的数据库或具有不同的生成参数生成对象。

**必填属性：**

| 属性 | 描述                                               |
| ---- | -------------------------------------------------- |
| id   | 此上下文的唯一标识符。该值将在某些错误消息中使用。 |

**可选属性：**

| 属性                   | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| defaultModelType       | 如果目标运行时为“ MyBatis3Simple”，“ MyBatis3DynamicSql”或“ MyBatis3Kotlin”，则将忽略此属性。此属性用于为生成的模型类型设置默认值。模型类型定义MBG如何生成域类。对于某些模型类型，MBG将为每个表生成单个域类，而对于其它模型类型，MBG可能会根据表的结构生成不同的类。该属性支持以下值：有条件的这是默认值。此模型与分层模型相似，不同之处在于，如果该单独的类仅包含一个字段，则不会生成单独的类。因此，如果一个表只有一个主键字段，则该字段将合并到基本记录类中。平面该模型为任何表仅生成一个域类。该类将保留表中的所有字段。如果表具有主键，则该模型将生成一个主键类，另一个类将保存表中的任何BLOB列，另一个类将保留其余字段。类之间存在适当的继承关系。 |
| targetRuntime          | 此属性用于为生成的代码指定运行时目标。该属性支持以下特殊值： **MyBatis3DynamicSql**这是默认值。 使用该值，MBG将生成与MyBatis版本3.4.2和更高版本以及Java 8和更高版本兼容的对象（例如，Java模型和映射器接口将使用通用类型和其他Java 8功能，例如默认方法）。**重要提示：**在此目标运行时中生成的Java代码取决于“ MyBatis Dynamic SQL”支持库版本1.1.3或更高版本。其他重要信息：无论为“defaultModelType”指定什么内容，都将以“ FLAT”样式生成模型对象。这也意味着不存在“具有BLOB”和“不具有BLOB”的方法。无论为<javaClientGenerator>的“类型”指定了什么，映射器都将作为带注释的映射器生成。不会生成XML。<sqlMapGenerator>不是必需的，如果指定，将被忽略。MyBatis Dynamic SQL以“每个查询”的方式而不是其他运行时的“全有或全无”的方式支持表别名。因此，已配置的表别名将被忽略。 **MyBatis3Kotlin**使用该值，MBG将生成与MyBatis版本3.4.2及更高版本兼容的Kotlin对象。**重要说明：**此目标运行时生成的Kotlin代码取决于“ MyBatis Dynamic SQL”支持库版本1.1.4或更高版本。其他重要信息：无论为“ defaultModelType”指定什么内容，都将以“ FLAT”样式生成模型对象。这也意味着不存在“具有BLOB”和“不具有BLOB”的方法。无论为<javaClientGenerator>的“类型”指定了什么，映射器都将作为带注释的映射器生成。不会生成XML。<sqlMapGenerator>不是必需的，如果指定，将被忽略。MyBatis Dynamic SQL以“每个查询”的方式而不是其他运行时的“全有或全无”的方式支持表别名。因此，已配置的表别名将被忽略。 **MyBatis3**使用该值，MBG将生成与MyBatis 3.0版及更高版本以及JSE 5.0版及更高版本兼容的对象（例如，Java模型和映射器接口将使用通用类型）。这些生成的对象中的“示例”方法实际上支持无限的动态where子句。此外，使用这些生成器生成的Java对象支持许多JSE 5.0功能，包括参数化类型和注释。 **MyBatis3Simple**使用该值，MBG将生成与MyBatis 3.0版及更高版本以及JSE 5.0版及更高版本兼容的对象（例如，Java模型和映射器接口将使用通用类型）。用此目标运行时生成的映射器是非常基本的CRUD操作，仅没有“示例”方法且动态SQL很少。用这些生成器生成的Java对象支持许多JSE 5.0功能，包括参数化类型和注释。 |
| introspectedColumnImpl | 使用此值来指定扩展`org.mybatis.generator.api.IntrospectedColumn`的类的标准名称。如果要在计算列信息时更改代码生成器的行为，则使用此方法。 |

**子元素：**

- <property> (0…N)
- <plugin> (0…N)
- <commentGenerator> (0 or 1)
- <connectionFactory> (connectionFactory 或 jdbcConnection 是必需的)
- <jdbcConnection> (connectionFactory 或 jdbcConnection 是必需的)
- <javaTypeResolver> (0 or 1)
- <javaModelGenerator> (1 是必需的)
- <sqlMapGenerator> (0 or 1)
- <javaClientGenerator> (0 or 1)
- <table> (1…N)

#### 7.3.5、property

下表列出了可以通过<property>子元素指定的属性：

| 属性名              | 属性值                                                       |
| ------------------- | ------------------------------------------------------------ |
| autoDelimitKeywords | 如果为true，则MBG如果将SQL关键字用作表中的列名，则将分隔SQL关键字。MBG维护许多不同数据库的SQL关键字列表。但是，该列表可能并不完整。如果某个特定关键字不在MBG的列表中，则可以强制使用`<columnOverride>`分隔该列 。请参阅`org.mybatis.generator.internal.db.SqlReservedWords`类的源代码，以获取MBG可以识别的关键字列表。默认值为false。 |
| beginningDelimiter  | 用作需要定界符的SQL标识符的开始标识符定界符的值。如果标识符包含空格，MBG将自动分隔SQL标识符。如果在<table>或<columnOverride>配置中特别要求，MBG还将分隔SQL标识符。默认值为双引号（“）。 |
| endingDelimiter     | 用作需要定界符的SQL标识符的结束标识符定界符的值。如果标识符包含空格，MBG将自动分隔SQL标识符。如果在<table>或<columnOverride>配置中特别要求，MBG还将分隔SQL标识符。默认值为双引号（“）。 |
| javaFileEncoding    | 使用此属性可以指定在处理Java文件时使用的编码。新生成的Java文件将以这种编码方式写入文件系统，而现有Java文件将在执行合并时以这种编码方式读取。如果未指定，则将使用平台默认编码。有关有效编码的信息，请参见`java.nio.charset.Charset`。 |
| javaFormatter       | 使用此属性可以为生成的Java文件指定用户提供的格式化程序的完整类名。该类必须实现`org.mybatis.generator.api.JavaFormatter，` 并且必须具有默认的（无参数）构造函数。每个上下文都包含Java格式化程序的单个实例。默认的Java格式化程序是 `org.mybatis.generator.api.dom.DefaultJavaFormatter`。 |
| targetJava8         | 使用此属性可以指定生成的代码可以使用Java 8+功能。例如，用于集合实例化的菱形运算符。有效值为`true`或`false`。默认值为`true`。 |
| kotlinFileEncoding  | 使用此属性可以指定使用Kotlin文件时要使用的编码。新生成的Kotlin文件将以这种编码方式写入文件系统。如果未指定，则将使用平台默认编码。有关有效编码的信息，请参见`java.nio.charset.Charset`。 |
| kotlinFormatter     | 使用此属性可以为生成的Kotlin文件指定用户提供的格式化程序的完整类名。该类必须实现`org.mybatis.generator.api.KotlinFormatter，` 并且必须具有默认的（无参数）构造函数。每个上下文都包含Kotlin格式化程序的单个实例。默认的Kotlin格式化程序是 `org.mybatis.generator.api.dom.DefaultKotlinFormatter`。 |
| xmlFormatter        | 使用此属性可以为生成的XML文件指定用户提供的格式化程序的完整类名。该类必须实现`org.mybatis.generator.api.XmlFormatter，` 并且必须具有默认的（无参数）构造函数。每个上下文都包含XML格式化程序的单个实例。默认的XML格式化程序是 `org.mybatis.generator.api.dom.DefaultXmlFormatter`。默认格式器使用XML DOM类中内置的格式。 |

#### 7.3.6、plugin

<plugin>元素用于定义插件。插件可用于扩展或修改MyBatis Generator（MBG）生成的代码。此元素是<context>元素的子元素。可以在上下文中指定任意数量的插件。插件将按照配置中列出的顺序进行调用。

**必填属性：**

| 属性 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| type | 实现插件的类的完全限定名称。该类必须实现`org.mybatis.generator.api.Plugin`接口 ，并且必须具有公共的默认构造函数。请注意，扩展适配器类`org.mybatis.generator.api.PluginAdapter` 比实现整个接口要容易得多。 |

**可选属性：**无

**子元素：**

- <property> (0 or 1)

#### 7.3.7、commentGenerator

<commentGenerator>元素用于定义注释生成器的属性。Comment Generator用于为MyBatis Generator（MBG）生成的各种元素（Java字段，Java方法，XML元素等）生成注释。默认的Comment Generator将JavaDoc注释添加到所有生成的Java元素中，以在Eclipse插件中启用Java合并功能。而且，注释会添加到每个生成的XML元素中。注释的目的还在于通知用户元素已生成且需要重新生成。此元素是<context>元素的可选子元素。

默认实现是`org.mybatis.generator.internal.DefaultCommentGenerator`。如果您只想修改某些行为，则默认实现是为可扩展性而设计的。

**必填属性：**无

**可选属性：**

| 属性 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| type | 这可以用于指定用户提供的注释生成器的类型。该类必须实现`org.mybatis.generator.api.CommentGenerator`接口， 并且必须具有公共的默认构造函数。该属性还接受特殊值DEFAULT，在这种情况下将使用默认实现（与未指定类型具有相同的效果）。 |

**子元素：**

- <property> (0…N)

**支持属性：**

下表列出了可以通过<property>子元素指定的默认注释生成器的属性：

| 属性名              | 属性值                                                       |
| ------------------- | ------------------------------------------------------------ |
| suppressAllComments | 此属性用于指定MBG是否在生成的代码中包含任何注释。该属性支持以下值：false，这是默认值。如果属性为false或未指定，则所有生成的元素将包括注释，指示该元素是生成的元素。如果该属性为true，则不会将注释添加到任何生成的元素。 **警告：**如果将此值设置为true，则将禁用所有代码合并。如果禁用所有注释，则可能会发现`UnmergeableXmlMappersPlugin`有用。这将使生成器遵守XML文件的覆盖标志。 |
| suppressDate        | 此属性用于指定MBG是否在生成的注释中包括生成时间戳。该属性支持以下值：假*这是默认值。* 如果属性为false或未指定，则所有生成的注释将包括生成元素时的时间戳。真正如果该属性为true，则不会将时间戳添加到生成的注释中。 |
| addRemarkComments   | 此属性用于指定MBG是否在生成的注释中包括来自数据库的表和列注释。该属性支持以下值：false，这是默认值。如果属性为false或未指定，则在生成元素时，所有生成的注释将**不**包含数据库的表和列注释。真正当该属性为true时，数据库的表和列注释将添加到生成的注释中。 **警告：**如果 suppressAllComments 选项为true，则将忽略此选项。 |
| dateFormat          | 将日期写入生成的注释时要使用的日期格式字符串。此字符串将用于构造`java.text.SimpleDateFormat`对象。可以在此处指定该对象的任何有效格式字符串。默认情况下，日期字符串将来自`java.util.Date`的`toString（）`方法。从1.3.4开始。 **警告：**如果 suppressAllComments 选项为true，则将忽略此选项。 **警告：**如果 suppressDate 选项为true，则将忽略此选项。 |

**简单示例：**

```xml
<commentGenerator>
  <property name="suppressDate" value="true" />
</commentGenerator>
123
```

#### 7.3.8、connectionFactory

<connectionFactory>元素用于指定连接工厂，以获取内省表所需的数据库连接。MyBatis Generator使用JDBC的DatabaseMetaData类来发现您在配置中指定的表的属性。每个<context>元素都需要一个<connectionFactory>或<jdbcConnection >元素。

**必填属性：**无

**可选属性：**

| 属性 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| type | 这可以用于指定用户提供的连接工厂的类型。该类必须实现`org.mybatis.generator.api.ConnectionFactory`接口， 并且必须具有公共的默认构造函数。该属性还接受特殊值DEFAULT，在这种情况下将使用默认实现（与未指定类型具有相同的效果）。 |

**子元素：**

- <property>（0…N）

**支持属性：**

> 注意：对于默认连接工厂，超出下面详细说明的任何指定属性都将添加到JDBC驱动程序的属性中。

下表列出了可以使用<property>子元素指定的默认连接工厂的属性：

| 属性名        | 属性值                                                       |
| ------------- | ------------------------------------------------------------ |
| driverClass   | 此属性用于指定JDBC驱动程序的标准类名。默认连接工厂需要此属性。 |
| connectionURL | 此属性用于指定数据库的JDBC连接URL。默认连接工厂需要此属性。  |
| userId        | 此属性用于指定连接的用户ID（用户名）                         |
| password      | 此属性用于指定连接的密码。                                   |

**简单示例：**

```xml
<connectionFactory>
  <property name="driverClass" value="com.mysql.jdbc.Driver"/>
  <property name="connectionURL" value="jdbc:mysql://localhost:3306/mybatis_crud"/>
  <property name="userId" value="root"/>
  <property name="password" value="123456"/>
</connectionFactory>
123456
```

#### 7.3.9、jdbcConnection

<jdbcConnection>元素用于指定内省表所需的数据库连接的属性。MyBatis Generator使用JDBC的DatabaseMetaData类来发现您在配置中指定的表的属性。每个<context>元素都需要一个<connectionFactory>或<jdbcConnection >元素。

**必填属性：**

| 属性          | 描述                                     |
| ------------- | ---------------------------------------- |
| driverClass   | 用于访问数据库的JDBC驱动程序的标准类名。 |
| connectionURL | 用于访问数据库的JDBC连接URL。            |

**可选属性：**

| 属性     | 描述                       |
| -------- | -------------------------- |
| userId   | 用于连接数据库的用户标识。 |
| password | 用于连接数据库的密码。     |

**子元素：**

- <property> (0…N)

> 注意：此处指定的任何属性都将添加到JDBC驱动程序的属性中。

**简单示例：**

```xml
<jdbcConnection 
	driverClass="com.mysql.jdbc.Driver" 
	connectionURL="jdbc:mysql://localhost:3306/mybatis_crud" 
	userId="root" 
	password="123456" />
12345
```

#### 7.3.10、javaTypeResolver

<javaTypeResolver>元素用于定义Java Type Resolver的属性。Java类型解析器用于根据数据库列信息计算Java类型。缺省的Java Type Resolver尝试通过尽可能替换Integral类型（Long，Integer，Short等）来使JDBC DECIMAL和NUMERIC类型更易于使用。如果这种行为是不希望的，请将属性“ forceBigDecimals”设置为“ true”。如果您想要不同于默认行为的行为，也可以替换自己的实现。此元素是<context>元素的可选子元素。

> 注意：如果使用MyBatis3Kotlin运行时，则生成器将在适用时自动将Java类型转换为它们相应的Kotlin等效项。

**必填属性：**无

**可选属性：**

| 属性 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| type | 这可以用来指定用户提供的Java类型解析器。该类必须实现`org.mybatis.generator.api.JavaTypeResolver`接口 ，并且必须具有公共的默认构造函数。该属性还接受特殊值DEFAULT，在这种情况下将使用默认实现（与未指定类型具有相同的效果）。 |

**子元素：**

- <property> (0…N)

**支持属性：**

下表列出了可以通过<property>子元素指定的默认Java类型解析器的属性：

![image-20200926090052050](https://img-blog.csdnimg.cn/img_convert/108dfafa2b698be6d213155e80f07c01.png)

**简单示例：**

此元素指定我们始终要对DECIMAL和NUMERIC列使用java.math.BigDecimal类型：

```xml
<javaTypeResolver>
  <property name="forceBigDecimals" value="true" />
</javaTypeResolver>
123
```

#### 7.3.11、javaModelGenerator

<javaModelGenerator>元素用于定义Java模型生成器的属性。Java模型生成器将构建与自省表匹配的主键类，记录类和“按示例查询”类。此元素是<context>元素的必需子元素。

**必填元素：**

| 属性          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| targetPackage | 这是将放置生成的类的包。在默认生成器中，属性“ enableSubPackages”控制如何计算实际包。如果为true，则计算出的包将是targetPackage加上表的目录和架构的子包（如果存在）。如果为false（默认值），则计算出的包将与targetPackage属性中指定的包完全相同。MyBatis Generator将根据所生成软件包的需要创建文件夹。 |
| targetProject | 这用于为生成的对象指定目标项目。在Eclipse环境中运行时，它指定将在其中保存对象的项目和源文件夹。在其他环境中，该值应该是本地文件系统上的现有目录。如果该目录不存在，MyBatis Generator将不会创建该目录。 |

**可选元素：**无

**子元素：**

- <property> (0…N)

**支持属性：**

下表列出了可以通过<property>子元素指定的默认Java模型生成器的属性：

![image-20200926091513815](https://img-blog.csdnimg.cn/img_convert/62dc0596a6b307be1b21798bdc569a25.png)

**简单示例：**

该元素指定我们始终希望将生成的类放置在“'test.model”包中，并且希望使用基于表模式和目录的子包。我们还希望修剪字符串列。

```xml
<javaModelGenerator targetPackage="test.model"
     targetProject="\MyProject\src">
  <property name="enableSubPackages" value="true" />
  <property name="trimStrings" value="true" />
</javaModelGenerator>
12345
```

#### 7.3.12、sqlMapGenerator

<sqlMapGenerator>元素用于定义SQL映射生成器的属性。SQL Map Generator为每个自省表构建MyBatis格式的SQL map XML文件。

仅当您选择的javaClientGenerator需要XML时，此元素才是<context>元素的必需子元素。

基于MyBatis Dynamic SQL的运行时不会生成XML，并且如果指定了该元素，则会忽略该元素。

如果未指定javaClientGenerator，则适用以下规则：

- 如果指定sqlMapGenerator，则MBG将仅生成SQL映射XML文件和模型类。
- 如果未指定sqlMapGenerator，则MBG仅生成模型类。

**必填属性：**

| 属性          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| targetPackage | 这是将放置生成的SQL Map的程序包。在默认生成器中，属性“ enableSubPackages”控制如何计算实际包。如果为true，则计算出的包将是targetPackage加上表的目录和架构的子包（如果存在）。如果为false（默认值），则计算出的包将与targetPackage属性中指定的包完全相同。MyBatis Generator（MBG）将根据生成的软件包的需要创建文件夹。 |
| targetProject | 这用于为生成的SQL映射指定目标项目。在Eclipse环境中运行时，它指定将在其中保存对象的项目和源文件夹。在其他环境中，该值应该是本地文件系统上的现有目录。如果该目录不存在，MBG将不会创建该目录。 |

**可选属性：**无

**子元素：**

- <property> (0…N)

**支持属性：**

下表列出了可以通过<property>子元素指定的默认SQL Map生成器的属性：

| 物业名称          | 物业价值                                                     |
| ----------------- | ------------------------------------------------------------ |
| enableSubPackages | 此属性用于选择MBG是否将根据自省表的目录和架构为对象生成不同的Java包。例如，假设在模式MYSCHMA中有一个表MYTABLE。还要假设targetPackage属性设置为“ com.mycompany”。如果此属性为true，则将为表生成的SQL Map放置在包“ com.mycompany.myschema”中。如果该属性为false，则将生成的SQL Map放置在“ com.mycompany”架构中。*默认值为false。* |

**简单示例：**

该元素指定我们始终希望将生成的SQL Map放置在“'test.model”包中，并且希望基于表模式和目录使用子包。

```xml
<sqlMapGenerator targetPackage="test.model"
     targetProject="\MyProject\src">
  <property name="enableSubPackages" value="true" />
</sqlMapGenerator>
1234
```

#### 7.3.13、javaClientGenerator

<javaClientGenerator>元素用于定义Java客户端生成器的属性。Java客户端生成器构建Java接口和类，以方便使用所生成的Java模型和XML映射文件。对于MyBatis，生成的对象采用mapper接口的形式。此元素是<context>元素的可选子元素。如果未指定此元素，则MyBatis Generator（MBG）将不会生成Java客户端接口和类。

**必填属性：**

![image-20200926092611734](https://img-blog.csdnimg.cn/img_convert/4fb4ef02d8d18a462f293608d6acc7a1.png)

**可选属性：**无

**子元素：**

- <property> (0…N)

**支持属性：**

![image-20200926092656538](https://img-blog.csdnimg.cn/img_convert/5a509270d4a7c8a76f88707b99833fb7.png)

**简单示例：**

该元素指定我们始终希望将生成的接口和对象放置在“'test.model”包中，并且希望基于表模式和目录使用子包。它还指定我们要生成映射程序接口，该接口引用MyBatis3的XML配置文件。

```xml
<javaClientGenerator targetPackage="test.model"
     targetProject="\MyProject\src" type="XMLMAPPER">
  <property name="enableSubPackages" value="true" />
</javaClientGenerator>
1234
```

#### 7.3.14、table

<table>元素用于选择数据库中的表以进行自省。选定的表将导致为每个表生成以下对象：

- MyBatis格式化的SQL Map文件
- 构成表的“模型”的一组类，包括：
  - 一个与表的主键匹配的类（如果表具有主键）。
  - 一个类，用于匹配表中不在主键中的字段和非BLOB字段。如果有一个，此类将扩展主键。
  - 一个用于保存表中任何BLOB字段（如果有）的类。该类将扩展前两个类之一，具体取决于表的配置。
  - 用于在不同的“按示例”方法（selectByExample，deleteByExample）中生成动态where子句的类。
- （可选）MyBatis映射器界面

必须至少指定一个<table>元素作为<context>元素的必需子元素。您可以指定无限的表元素。

MyBatis Generator（MBG）尝试自动处理数据库标识符的区分大小写。在大多数情况下，无论您为catalog，schema和tableName 属性指定了什么，MBG都能找到表。MBG的过程遵循以下步骤：

1. 如果`catalog`，`schema`或 `tableName`属性中的任何一个都包含空格，则MBG将根据指定的确切大小写查找表。在这种情况下，MBG将自动在生成的SQL中分隔表标识符。
2. 否则，如果数据库报告标识符存储为大写，则MBG会自动将任何表标识符转换为大写。
3. 否则，如果数据库报告标识符存储为小写，则MBG会自动将任何表标识符转换为小写。
4. 其它MBG将根据指定的确切大小写查找表。

在大多数情况下，此过程可以完美运行。但是，在某些情况下它将失败。例如，假设您创建一个像这样的表：

```sql
create table "myTable" (
     ...some columns
)
123
```

由于表名是有界的，因此即使数据库通常以大写形式存储标识符，大多数数据库也会使用指定的精确大小写来创建表。在这种情况下，您应该在表配置中指定属性 `delimitIdentifiers =“ true”`。

**必填属性：**

| 属性      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| tableName | 数据库表的名称（不包括架构或目录）。如果需要，指定的值可以包含SQL通配符。 |

**可选属性：**

![image-20200926094014525](https://img-blog.csdnimg.cn/img_convert/8d9b8046046aae235f2f89fcfb0d4028.png)

**子元素：**

- <property> (0…N)
- <generatedKey> (0 or 1)
- <domainObjectRenamingRule> (0 or 1)
- <columnRenamingRule> (0 or 1)
- <columnOverride> (0…N)
- <ignoreColumn> (0…N)

**支持属性：**

下表列出了可以通过<property>子元素指定的默认SQL Map生成器的属性：

![image-20200926095151568](https://img-blog.csdnimg.cn/img_convert/1a38e4e2169921b15f67ede21860472a.png)

**简单示例：**

该元素指定我们始终希望为模式MYSCHEMA中的表MYTABLE生成代码。我们还希望忽略表中名为“ fred”的列，并且希望覆盖列“ BEG_DATE”，以便生成的属性名称为“ startDate”。

```xml
<table tableName="MYTABLE" schema="MYSCHEMA">
  <ignoreColumn column="fred"/>
  <columnOverride column="BEG_DATE" property="startDate"/>
</table>
1234
```

### 7.4、如何生成代码

第一步：创建一个全新的工程**mybatis-mbg**

第二步：创建一个lib文件夹，把以下依赖添加进去，然后全部选中，右键Build Path一下

- mybatis-3.5.5.jar
- mybatis-generator-core-1.4.0.jar
- log4j-1.2.17.jar
- mysql-connector-java-5.1.47-bin.jar

第三步：拷贝之前工程的log4j.xml日志配置到src目录下

第四步：在工程根目录下创建generatorConfig.xml，用来配置生成代码的具体配置信息

```xml
<!DOCTYPE generatorConfiguration PUBLIC
 "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
 "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
	<context id="mybatis_crud" targetRuntime="MyBatis3">
		<!-- 数据库连接信息配置 -->
		<jdbcConnection driverClass="com.mysql.jdbc.Driver" connectionURL="jdbc:mysql://localhost:3306/mybatis_crud" userId="root" password="123456" />
		<!-- JavaBean的生成策略 -->
		<javaModelGenerator targetPackage="com.caochenlei.example.dao" targetProject="./src">
			<property name="enableSubPackages" value="true" />
			<property name="trimStrings" value="true" />
		</javaModelGenerator>
		<!-- SqlMapper的生成策略 -->
		<sqlMapGenerator targetPackage="com.caochenlei.example.mapper" targetProject="./src">
			<property name="enableSubPackages" value="true" />
		</sqlMapGenerator>
		<!-- JavaDao的生成策略 -->
		<javaClientGenerator type="XMLMAPPER" targetPackage="com.caochenlei.example.mapper" targetProject="./src">
			<property name="enableSubPackages" value="true" />
		</javaClientGenerator>
		<!-- 数据表与JavaBean的映射 -->
		<table tableName="department" domainObjectName="Department" />
		<table tableName="employee" domainObjectName="Employee" />
	</context>
</generatorConfiguration>
12345678910111213141516171819202122232425
```

第五步：在src目录下创建MBGRunner.java，用于启动代码生成器，代码如下：

```java
import java.io.File;
import java.util.ArrayList;
import java.util.List;

import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

public class MBGRunner {
	public static void main(String[] args) throws Exception {
		List<String> warnings = new ArrayList<String>();
		boolean overwrite = true;
		File configFile = new File("generatorConfig.xml");
		ConfigurationParser cp = new ConfigurationParser(warnings);
		Configuration config = cp.parseConfiguration(configFile);
		DefaultShellCallback callback = new DefaultShellCallback(overwrite);
		MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
		myBatisGenerator.generate(null);
	}
}
123456789101112131415161718192021
```

第六步：运行main方法，然后刷新工程，你会发现多了一点东西，如下，这就是生成的代码：

![image-20200926095950477](https://img-blog.csdnimg.cn/img_convert/8927d0427cb23b1986d7f13ae2eae215.png)

### 7.5、生成文件介绍

![image-20201001093000137](https://img-blog.csdnimg.cn/img_convert/88dd6513ef0e78d91116b5507c7f70d4.png)

一般一个表会生成4个文件，以employee为例，一般会生成：

1. Employee.java：实体类POJO，该类中的属性对应表中的每个字段
2. EmployeeExample.java：mybatis的逆向工程中会生成实例及实例对应的example，example用于添加条件，相当where后面的部分 ，用于简化sql语句的书写
3. EmployeeMapper.java：生成默认的公用方法，比如增删改查，带条件查询等等方法
4. EmployeeMapper.xml：以上方法的sql实现

### 7.6、生成方法介绍

EmployeeMapper接口中的函数及方法：

| 接口方法                                                     | 接口描述                                |
| ------------------------------------------------------------ | --------------------------------------- |
| long countByExample(EmployeeExample example);                | 根据条件计数                            |
| int deleteByExample(EmployeeExample example);                | 根据条件删除                            |
| int deleteByPrimaryKey(Integer id);                          | 根据主键删除                            |
| int insert(Employee record);                                 | 插入一条记录                            |
| int insertSelective(Employee record);                        | 插入一条记录 （只插入值不为null的字段） |
| List<Employee> selectByExample(EmployeeExample example);     | 根据条件查询                            |
| Employee selectByPrimaryKey(Integer id);                     | 根据主键查询                            |
| int updateByExampleSelective (@Param(“record”) Employee record, @Param(“example”) EmployeeExample example); | 根据条件更新 （只更新值不为null的字段） |
| int updateByExample (@Param(“record”) Employee record, @Param(“example”) EmployeeExample example); | 根据条件更新                            |
| int updateByPrimaryKeySelective(Employee record);            | 根据主键更新 （只更新值不为null的字段） |
| int updateByPrimaryKey(Employee record);                     | 根据主键更新                            |

example实例解析：example用于添加条件，相当where后面的部分

```java
xxxExample example = new xxxExample(); 
Criteria criteria = new xxxExample().createCriteria();
12
```

其它条件方法：

example结构：

![image-20201001094944201](https://img-blog.csdnimg.cn/img_convert/f691edf70bfc1a09f93b361482c6911b.png)

| 方法                                    | 说明                                        |
| --------------------------------------- | ------------------------------------------- |
| example.setOrderByClause(“字段名 ASC”); | 添加升序排列条件，DESC为降序                |
| example.setDistinct(false)              | 去除重复，boolean型，true为选择不重复的记录 |
| example.clear()                         | 清除条件                                    |
| example.or()                            | 添加另一个criteria以此来构成or的关系        |

criteria结构：

![image-20201001094751117](https://img-blog.csdnimg.cn/img_convert/b426c361694ccb1be0e41a048c91591a.png)

| 方法                                       | 说明                                    |
| ------------------------------------------ | --------------------------------------- |
| criteria.andXxxIsNull                      | 添加字段xxx为null的条件                 |
| criteria.andXxxIsNotNull                   | 添加字段xxx不为null的条件               |
| criteria.andXxxEqualTo(value)              | 添加xxx字段等于value条件                |
| criteria.andXxxNotEqualTo(value)           | 添加xxx字段不等于value条件              |
| criteria.andXxxGreaterThan(value)          | 添加xxx字段大于value条件                |
| criteria.andXxxGreaterThanOrEqualTo(value) | 添加xxx字段大于等于value条件            |
| criteria.andXxxLessThan(value)             | 添加xxx字段小于value条件                |
| criteria.andXxxLessThanOrEqualTo(value)    | 添加xxx字段小于等于value条件            |
| criteria.andXxxIn(List<？>)                | 添加xxx字段值在List<？>条件             |
| criteria.andXxxNotIn(List<？>)             | 添加xxx字段值不在List<？>条件           |
| criteria.andXxxLike(“%”+value+”%”)         | 添加xxx字段值为value的模糊查询条件      |
| criteria.andXxxNotLike(“%”+value+”%”)      | 添加xxx字段值不为value的模糊查询条件    |
| criteria.andXxxBetween(value1,value2)      | 添加xxx字段值在value1和value2之间条件   |
| criteria.andXxxNotBetween(value1,value2)   | 添加xxx字段值不在value1和value2之间条件 |

### 7.7、生成方法使用

#### 7.7.1、查询案例

selectByPrimaryKey()

```java
//相当于select * from user where id = 100;
User user = XxxMapper.selectByPrimaryKey(100);
12
```

#### 7.7.2、新增案例

insert()

```java
//相当于insert into user(id,username) values (10086,'张三');
User user = new User();
user.setId(10086);
user.setUsername("张三");
XxxMapper.insert(user);
12345
```

#### 7.7.3、修改案例

updateByPrimaryKey()

```java
//相当于update user set username='李四' where id = 10086;
User user =new User();
user.setId(10086);
user.setUsername("李四");
XxxMapper.updateByPrimaryKey(user);
12345
```

#### 7.7.4、删除案例

deleteByPrimaryKey()

```java
//相当于delete from user where id = 100;
XxxMapper.deleteByPrimaryKey(100);
12
```

#### 7.7.5、查询数量

countByExample()

```java
//查询所有用户名为张三的用户数量
//相当于select count(*) from user where username = '张三';
UserExample example = new UserExample();
Criteria criteria = example.createCriteria();
criteria.andUsernameEqualTo("张三");
int count = XxxMapper.countByExample(example);
123456
```

## 第八章 MyBatis3的工作原理

### 8.1、环境准备

> 注意：如果你是从前几章一步一步跟过来的，可以接着往下走，如果你是直接从这一章开始的，你可以去配套资料中去找，我已经准备好的工程代码，这个工程主要是用于DEBUG调试过程中，比较方便的查看方法的执行。

拷贝mybatis-crud工程为mybatis-analysis，然后依次去掉以下几个东西：

1. 删掉ehcache.xml

2. 删掉Department.java、DepartmentTest.java、EmployeeMapperDynamicSQLTest.java

3. 删掉DepartmentMapper.java、EmployeeMapperDynamicSQL.java、DepartmentMapper.xml、EmployeeMapperDynamicSQL.xml

4. 去掉Employee.java中的`private Department dep;`以及它所对应的get和set方法

5. 对于EmployeeTest.java只保留getEmpByIdTest这一个测试方法，其余全部删掉

6. 对于EmployeeMapper.java只保留`public Employee getEmpById(Integer id);`这一个接口方法，其余全部删掉

7. 对于EmployeeMapper.xml只保留`select id="getEmpById"`这一个映射片段，其余全部删掉

8. 修改mybatis-config.xml配置信息，可直接拷贝以下代码

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   	<!-- 加载数据库配置文件信息 -->
   	<properties resource="dbconfig.properties"></properties>
   
   	<!-- 配置框架的全局配置信息 -->
   	<settings>
   		<setting name="mapUnderscoreToCamelCase" value="true" />
   		<setting name="jdbcTypeForNull" value="NULL" />
   	</settings>
   
   	<!-- 配置框架的多数据源信息 -->
   	<environments default="dev_mysql">
   		<!-- 配置mysql开发环境 -->
   		<environment id="dev_mysql">
   			<transactionManager type="JDBC" />
   			<dataSource type="POOLED">
   				<property name="driver" value="${mysql.driver}" />
   				<property name="url" value="${mysql.url}" />
   				<property name="username" value="${mysql.username}" />
   				<property name="password" value="${mysql.password}" />
   			</dataSource>
   		</environment>
   		<!-- 配置oracle开发环境 -->
   		<environment id="dev_oracle">
   			<transactionManager type="JDBC" />
   			<dataSource type="POOLED">
   				<property name="driver" value="${oracle.driver}" />
   				<property name="url" value="${oracle.url}" />
   				<property name="username" value="${oracle.username}" />
   				<property name="password" value="${oracle.password}" />
   			</dataSource>
   		</environment>
   	</environments>
   
   	<!-- 数据库厂商起别名 -->
   	<databaseIdProvider type="DB_VENDOR">
   		<property name="MySQL" value="mysql" />
   		<property name="Oracle" value="oracle" />
   	</databaseIdProvider>
   
   	<!-- 批量注册映射文件 -->
   	<mappers>
   		<mapper resource="com/caochenlei/mybatis/mapper/EmployeeMapper.xml" />
   	</mappers>
   </configuration>
   12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849
   ```

9. 去掉ehcache-2.10.5.jar、mybatis-ehcache-1.2.1.jar、slf4j-api-1.7.25.jar、slf4j-jdk14-1.7.25.jar这几个依赖JAR包，不会去这一步可跳过，主要是slf4j会影响日志打印

10. 运行EmployeeTest.java的getEmpByIdTest方法，观察是否可以正常运行，如果觉得麻烦，可以直接去配套资料找我准备好的工程

### 8.2、结构划分

我们可以把mybatis划分为以下几个层次，这里了解即可，主要是方便我们对执行过程的整个流程的把握，为后边的源码讲解做铺垫。

mybatis暴露给用户的其实是接口层，我们使用接口层就可以实现增删改查的方法，但实际上底层进行参数处理调用的时候，这一部分的工作其实是数据处理层进行的，而数据处理层底层时则是使用的原生的JDBC的方式进行和数据库打交道的，而我们传递给数据处理层的一些配置、注解、映射片段都需要我们框架支撑层来支撑。

![image-20200927101818589](https://img-blog.csdnimg.cn/img_convert/31a60be11b8c9a568e9ee174b219462c.png)

### 8.3、SqlSessionFactory初始化

首先，mybatis加载核心配置文件“mybatis-config.xml”，通过SqlSessionFactoryBuilder的build方法进行创建SqlSessionFactory，那这个build方法究竟做了什么工作，我们往下看

![image-20200927103827678](https://img-blog.csdnimg.cn/img_convert/22e76b484f8f56826554a971188f9077.png)

当我们DEBUG进入后，发现它直接调用了另一个build方法，一进去，首先mybatis先构建了一个XMLConfigBuilder也就是XML配置解析器，这个解析器的最主要的作用就是用来解析“mybatis-config.xml”和所有的"xxxxMapper.xml"文件里边的配置信息的，那它又是如何解析我们的配置文件的，接着往下走

**SqlSessionFactoryBuilder类**

![image-20200927104118320](https://img-blog.csdnimg.cn/img_convert/cbd4aa26a3f5cc19def9b50eb4de5004.png)

![image-20200927104253721](https://img-blog.csdnimg.cn/img_convert/3f9b0761dd41dd1e39bd072fba49bafb.png)

这里用了一个锁机制，如果以前已经解析过配置就不能在解析了，这里相对来说不重要，往下看关键的地方来了`parser.evalNode("/configuration")`，它首先解析configuration根节点，我们来看看它是如何解析configuration根节点的

**XMLConfigBuilder类**

![image-20200927105001321](https://img-blog.csdnimg.cn/img_convert/d3cdf22f097cba518bc97a0df2285e5a.png)

往下走就到了parseConfiguration方法，在这个方法中，我们不难看出，mybatis正在处理configuration的子节点，然后将它们解析出来，实际上这一步就开始往configuration对象中进行设置各种配置信息了，而且你注意代码最后一行`mapperElement(root.evalNode("mappers"));`，它是在获取所有的mapper映射xml，我们不妨走进去看看她是怎么获取的

**XMLConfigBuilder类**

![image-20200927110110101](https://img-blog.csdnimg.cn/img_convert/031783f7042592f3b17d34422fb8ae58.png)

我们进来不难看到，它首先检查我们在配置中是不是配置了包扫描，如果是包扫描，它就用包扫描的形式获取每一个mapper映射xml，如果不是，那它就按照是不是设置了resource、url、class这几种来获取，我们是用resource形式注册的，所以我们会接着往下走，直到走到`XMLMapperBuilder mapperParser`在这里它使用了另外一个XML解析器，这个解析器是专门解析Mapper映射xml的，我们来看看，它都解析哪些标签？

![image-20200927151848283](https://img-blog.csdnimg.cn/img_convert/18296a8a4f57beeb16953ccdffc97ca3.png)

**XMLConfigBuilder类**

![image-20200927152004511](https://img-blog.csdnimg.cn/img_convert/b6cc8725be50cd74cdf554b118028be4.png)

它首先解析了"xxxxMapper.xml"的跟标签mapper，然后在解析它的子标签

**XMLMapperBuilder类**

![image-20200927152353300](https://img-blog.csdnimg.cn/img_convert/36c7707d01748dfd2d567d954e4a5ac0.png)

依次开始解析mapper的子标签，比如我们最常见的增删改查标签

**XMLMapperBuilder类**

![image-20200927153249131](https://img-blog.csdnimg.cn/img_convert/0dcfbe147479c8d679eee3ae0616a0cf.png)

那我们就往下走，就以增删改查标签为例，看看它们是如何处理这些标签的，如果你在增删改查标签上配置了databaseId，它会首先拿到这个属性，根据这个属性是否为空来进行建立不同的Statement以对应不同的数据库

**EmployeeMapper.xml**

![image-20200927153354986](https://img-blog.csdnimg.cn/img_convert/29074917c0dae8cec06e331d269b8c78.png)

**XMLMapperBuilder类**

![image-20200927153334412](https://img-blog.csdnimg.cn/img_convert/399b07e184e2b5834e30bdccc0f15e8a.png)

我们接着往下走，发现它又创建了一个XML解析器，这个解析器就是专门用来解析增删改查标签的，我们来看看它又做了什么工作

![image-20200927153639006](https://img-blog.csdnimg.cn/img_convert/037b0ba836b2bb510db78e3840d2f747.png)

好家伙，这个方法要做的事情就太多了，凡是你能在标签上写的属性，通过这个方法，都能拿到，咱们重点看最后一行代码

**XMLStatementBuilder类**

![image-20200927155017069](https://img-blog.csdnimg.cn/img_convert/b81f1a4a2cde683a0fcbb5d099c8cfba.png)

也就是这个

![image-20200927155200783](https://img-blog.csdnimg.cn/img_convert/18b965c4d814915604c74c4abfbd0657.png)

它把增删改查标签全部封装到了一个MappedStatement中，所以，每一个增删改查标签都会对应一个MappedStatement，在这个对象中，你就可以都拿到当前标签的所有信息，然后将这个封装好的MappedStatement放到了`builderAssistant.addMappedStatement`其实就是放到了`configuration.addMappedStatement`

到此为止，核心配置文件 和所有的"xxxxMapper.xml"映射文件的配置信息，全部获取到，并封装到了`configuration`中，等解析完成以后，就会把`configuration`对象返回，然后进行一系列回调，最终回到parse这个方法

**Configuration类**

![image-20200927125609610](https://img-blog.csdnimg.cn/img_convert/f9405887602a892d235b2859e25c1c58.png)

`configuration`对象返回以后，就会调用`build(parser.parse())`，然后创建SqlSessionFactory

**SqlSessionFactoryBuilder类**

![image-20200927130046431](https://img-blog.csdnimg.cn/img_convert/405fc64317dc04e8fcf48f93e6e4f313.png)

但实际上，它返回的却是带configuration的DefaultSqlSessionFactory类实例对象，至此，一个SqlSessionFactory就创建完毕了

**SqlSessionFactoryBuiler类**

![image-20200927130126645](https://img-blog.csdnimg.cn/img_convert/40bc888bbc3ac00e048df82fc0dbb313.png)

我们可以把以上过程中重要的步骤梳理成一个时序图

![image-20200927130307874](https://img-blog.csdnimg.cn/img_convert/b16a5947a08f8730acbdc4a72b47f85a.png)

### 8.4、SqlSession初始化

当SqlSessionFactory完成创建并初始化以后，通过调用openSession来获取一个SqlSession对象

![image-20200927160538781](https://img-blog.csdnimg.cn/img_convert/8e7b2b8e2060dacd35f059962ee93bd5.png)

它会从数据源打开一个会话，然后通过配置信息获取执行器类型，创建事务等一系列操作，我们来看看它是如何创建执行器Executor的

**DefaultSqlSessionFactory类**

![image-20200927160742106](https://img-blog.csdnimg.cn/img_convert/52db8b332dc69a9a4938664470421515.png)

![image-20200927161030166](https://img-blog.csdnimg.cn/img_convert/51338e26b67169d2e6c1c0f1147ac47a.png)

它会通过全局配置获取执行器类型，根据执行器类型来创建，到底是一个什么执行器，执行器默认是SIMPLE，所以它就会创建一个SimpleExecutor，这个全局变量可以在“settings”标签中配置，我们这里采用默认，除了创建执行器，它还会判断当前二级缓存是否开启，如果开启，还要再创建一个二级缓存执行器（其实是将上一个执行器进行了一层包装），等这一些列步骤做完，还有最最关键的一步，是我们后来插件开发经常用到的`interceptorChain.pluginAll(executor)`，执行完所有的拦截器（其实是将上一个执行器又进行了一层包装），然后最终才能返回一个能用的Executor。

![image-20200927161334236](https://img-blog.csdnimg.cn/img_convert/02a54d8e50ac611bc3d19e628a4e64cf.png)

**Configuration类**

![image-20200927161120064](https://img-blog.csdnimg.cn/img_convert/4c19775c65140351ade634491d2eac90.png)

等执行器创建完成并返回，我们就开始着手准备创建SqlSession，这里返回的又是一个DefaultSqlSession，所以最终返回的是一个带 configuration和executor的DefaultSqlSession，到此，一个可用的SqlSession就获取到了，它实际上用的是DefaultSqlSession

**DefaultSqlSessionFactory类**

![image-20200927161915232](https://img-blog.csdnimg.cn/img_convert/6fc35f2a5fb7fd13806aaf02465311da.png)

我们可以把以上过程中重要的步骤梳理成一个时序图

![image-20200927162956748](https://img-blog.csdnimg.cn/img_convert/8062d0d44e8fea26db978a20ab4cad2a.png)

### 8.5、getMapper的流程

根据前一章节的讲解，我们获取到了SqlSession，接下来，我们来看看它调用getMapper方法都做了哪些处理

![image-20200927165014056](https://img-blog.csdnimg.cn/img_convert/bb48518ee03da7a4337b0602a468175e.png)

在这里他又调用了DefaultSqlSession的getMapper，接着往下走

**DefaultSqlSession类**

![image-20200927165109680](https://img-blog.csdnimg.cn/img_convert/30eda797f79748bfe1c7a01e89b71473.png)

在这里他又调用了Configuration的getMapper，接着往下走

**Configuration类**

![image-20200927165419154](https://img-blog.csdnimg.cn/img_convert/31e60d24802c36376d96a2d5bf3285d8.png)

在这里他又调用了MapperRegistry的getMapper，接着往下走，重头戏来了，前方高能，在这个方法中，它利用反射技术创建了一个MapperProxy实例，那他底层是怎么做的，我们接着往下看

**MapperRegistry类**

![image-20200927165521671](https://img-blog.csdnimg.cn/img_convert/10498382a2553f77e8f12059b4f2ecd5.png)

根据接口类型获取MapperProxy，然后通过反射创建MapperProxy的代理对象

**MapperProxyFactory类**

![image-20200927174428637](https://img-blog.csdnimg.cn/img_convert/e8c74988d0ac05ac4a1c2c65aa3fde47.png)

![image-20200927174510233](https://img-blog.csdnimg.cn/img_convert/9b3a6409743ca522a04025a9ed937d2a.png)

那这个MapperProxy是个什么东西呢？其实本质上他也是一个InvocationHandler

![image-20200927174640649](https://img-blog.csdnimg.cn/img_convert/52eb289f2449f21b2a24271900643a44.png)

然后返回的是MapperProxy的动态代理对象，所以，我们其实时是使用的MapperProxy动态代理来进行接下来的一系列的操作的

![image-20200927174906770](https://img-blog.csdnimg.cn/img_convert/db7b576126369d125fd0f8df668ce3b0.png)

我们可以把以上过程梳理成一个时序图

![image-20200927174956596](https://img-blog.csdnimg.cn/img_convert/9f6727d12f4d454874e90a6ae506d119.png)

### 8.6、查询的流程

这个查询流程相对来说就复杂多了，但是同时也是最重要的一个流程，大家务必保持十二分的精神，我们接着深入源码，请往下看

![image-20200927175758620](https://img-blog.csdnimg.cn/img_convert/06800efd24ca50355471097cfc846f9a.png)

当DEBUG进去，发现其实是MapperProxy的invoke方法

**MapperProxy类**

![image-20200927180020106](https://img-blog.csdnimg.cn/img_convert/cf254e4a7f3e546918081e410d552bae.png)

它首先会运行`new MapperMethod`，我们先进入瞅瞅，他干了啥

**MapperProxy类**

![image-20200927180650253](https://img-blog.csdnimg.cn/img_convert/cbd4fd0b801dc18c1acf353abb92529f.png)

首先会先进入构造方法，不急，一步一步来

**MapperMethod类**

![image-20200927180942084](https://img-blog.csdnimg.cn/img_convert/e07d6994d12c6ac47251f95e62c8690d.png)

**MapperMethod$SqlCommand类**

![image-20200927190138057](https://img-blog.csdnimg.cn/img_convert/914161375f738e50a788d721ac309cf8.png)

首先先对SQL命令进行处理，通过接口 + 方法名当作id，获取configuration中配置的MappedStatement

**MapperMethod$SqlCommand类**

![image-20200927185811057](https://img-blog.csdnimg.cn/img_convert/337199b800b8ede600f0e9ee683f84eb.png)

然后创建方法签名

**MapperMethod$MethodSignature类**

![image-20200927190918362](https://img-blog.csdnimg.cn/img_convert/d6b0b385700a65952a93497e024c470e.png)

在上边这个方法中，最重要的就是最后一行代码的参数设置，这个是我们重点看的代码，因为这里涉及到了我们在映射文件中取值的问题，如果我们在声明接口放的时候使用了@Param，那么将会在这里发挥作用，它首先会把那些标注了注解的参数以指定的名称放入参数集合中，剩下的没有被标注的部分还是使用索引的形式，放入集合中，完成了这一步，我们基本上就开始回退上一步了，一直到下边这

**ParamNameResolver类**

![image-20200927191330239](https://img-blog.csdnimg.cn/img_convert/92a9443a357a1e8249612764047d73be.png)

在这里还是使用sqlSession执行指定的方法，并通过args传递参数，最终交给MapperMethod类的execute方法进行执行

**MapperProxy$PlainMethodInvoker类**

![image-20200927192830417](https://img-blog.csdnimg.cn/img_convert/6fe1ae7ae8d04c54c12313acf44d31c5.png)

因为在执行前会判断我们的类型到底是想要做什么操作？增删改查？

**MapperMethod类**

![image-20200927193111448](https://img-blog.csdnimg.cn/img_convert/9fb3d7d3e28b69cc4a9e558be783ebe1.png)

由于我们现在是查询一个对象，所以它会走到这里，我们进去瞧瞧

![image-20200927193301309](https://img-blog.csdnimg.cn/img_convert/612852303e173b72a25d1abb7cb42eb9.png)

我们发现即使是查询一个，他底层使用的其实是列表，然后给你返回第一个，如果你要是使用查询一个对象的方法查询多个那这里肯定会报错，我们看看`this.selectList(statement, parameter);`是怎么实现的

**DefaultSqlSession类**

![image-20200927193346802](https://img-blog.csdnimg.cn/img_convert/700d971da8dacc39bffc588c26b95bf4.png)

他原来调用了本类中另外一个selectList重载方法

**DefaultSqlSession类**

![image-20200927193523542](https://img-blog.csdnimg.cn/img_convert/073a52fff4b47859740897eb013b564f.png)

首先先从全局配置中获取增删改查所对应的MappedStatement对象，而这个statement其实就是上边提到过的接口名 + 方法名所组合成的id字符串，当找到MappedStatement之后，通过执行器进行执行，但是在执行以前，他会对对参数进行包装处理，也就是调用方法`wrapCollection(parameter)`

**DefaultSqlSession类**

![image-20200927193539882](https://img-blog.csdnimg.cn/img_convert/7bb26c1f518e18fc26a44cdf1fd7efe8.png)

![image-20200927193737519](https://img-blog.csdnimg.cn/img_convert/fd3c6395cdc48e86f2b9536fd5ccd558.png)

实际上调用的是`ParamNameResolver.wrapToMapIfCollection`方法

**DefaultSqlSession类**

![image-20200927194104302](https://img-blog.csdnimg.cn/img_convert/97114a49cc64a68cd64b449693f88b1a.png)

这里就是之前前几章我们说过的，如果参数是Collection（List、Set）类型或者是数组，也会特殊处理，也是把传入的Collection（List、Set）或者数组封装在map中。key：对于单一参数Collection使用（collection、list）、数组使用（array）来获取，例如：Collection使用#{collection[0]},…#{collection[N]}，List集合也可以使用#{list[0]},…#{list[N]}，数组可以使用#{array[0]},…,#{array[N]}这种形式来获取键所对应的值。而 value：集合或数组索引所对应的值。就是在这里进行处理的

**ParamNameResolver类**

![image-20200927194229439](https://img-blog.csdnimg.cn/img_convert/2421485d465cca9e329c8505b14ae7e6.png)

当参数处理完毕之后，我们就会进入到query方法，先进行绑定sql语句处理，然后将绑定好的语句缓存到本地缓存中

**CachingExecutor类**

![image-20200927194630427](https://img-blog.csdnimg.cn/img_convert/6691296366e199eb37f563d27e07d7b5.png)

绑定后的语句中有各种参数和sql语句

![image-20200927203626595](https://img-blog.csdnimg.cn/img_convert/0e0c401d3a87b9fab718c5d683a82b23.png)

然后在调用query方法，先拿到二级缓存配置，看看是否有二级缓存，如果没有，就执行`delegate.query`查询，如果有，那就直接从二级缓存中缓存查询

**CachingExecutor类**

![image-20200927203752208](https://img-blog.csdnimg.cn/img_convert/f07cad3de64ae778ea6e3f392c3e5ede.png)

`delegate.query`其实是先查看本地缓存（一级缓存）是否有数据，如果本地缓存有数据，先从本地缓存拿出来数据，如果本地缓存没有数据，那就执行`queryFromDatabase`进行查询，从这两步，我们会知道，查询会优先查找二级缓存然后才是一级缓存

**SimpleExecutor类**

![image-20200927205420848](https://img-blog.csdnimg.cn/img_convert/5f027f9ca5eaa7d06e6cd251e749e255.png)

调用`queryFromDatabase`，内部调用的是`doQuery`

**SimpleExecutor类**

![image-20200927205940103](https://img-blog.csdnimg.cn/img_convert/5f27873b185cba9b4e15603da4fbf061.png)

在`doQuery`这里创建了一个StatementHandler对象

**SimpleExecutor类**

![image-20200927210218758](https://img-blog.csdnimg.cn/img_convert/6f5d68b299498fd13a9343c074df618d.png)

在newStatementHandler的时候，它实际上是根据我们标签配置中的参数来决定到底创建哪一种StatementHandler

![image-20200927210813950](https://img-blog.csdnimg.cn/img_convert/15396e5556e6469766fe4334e033e406.png)

**Configuration类**

![image-20200927210310640](https://img-blog.csdnimg.cn/img_convert/811cd0c2a057371eb73e2a5a6c184252.png)

因为默认配置是PREPARED，所以最终返回的是PreparedStatementHandler，然后再执行`(StatementHandler) interceptorChain.pluginAll(statementHandler)`所有的拦截器，这样PreparedStatementHandler就已经完成创建和插件拦截了

**RoutingStatementHandler类**

![image-20200927210548369](https://img-blog.csdnimg.cn/img_convert/7d4e678c993eda531c4557404f295ffb.png)

调用有参构造方法，内部调用的却是父类的构造方法，接着往下执行，我们看看，做了哪些工作

**PreparedStatementHandler类**

![image-20200927212544228](https://img-blog.csdnimg.cn/img_convert/313fecd495e65f000e7d788e4c73c0a7.png)

这个构造方法中，做了很多事情，他初始化了configuration、executor、mappedStatement、boundSql等等一系列的对象，但是最重要的是最后两行的代码parameterHandler、resultSetHandler，也就是在执行PreparedStatement之前他需要对传进来的参数进行处理，对传出去的结果进行处理，就是利用这两大对象，具体是怎么处理的我们进去看看

**BaseStatementHandler类**

![image-20200927212616331](https://img-blog.csdnimg.cn/img_convert/9bf4846f8eb62f424e9d7cce23a259e9.png)

它首先创建了ParameterHandler，但是当你进入这个方法，发现其实返回的是DefaultParameterHandler，继续往下走，然后他也执行了拦截器链也就是把所有的插件也执行了一遍，最终这个parameterHandler才能用，实际是DefaultParameterHandler

**Configuration类**

![image-20200927213030176](https://img-blog.csdnimg.cn/img_convert/3b886bce01494c75a7acfec720d40120.png)

**XMLLanguageDriver类**

![image-20200927213107521](https://img-blog.csdnimg.cn/img_convert/ca44aecac3722b93e8b692a6364a2eb4.png)

当往下执行，它首先创建了ResultSetHandler，但是当你进入这个方法，发现其实返回的是DefaultResultSetHandler，继续往下走，然后他也执行了拦截器链也就是把所有的插件也执行了一遍，最终这个ResultSetHandler才能用，实际是DefaultResultSetHandler

**Configuration类**

![image-20200927213353836](https://img-blog.csdnimg.cn/img_convert/5a96e0e2f0e4020bdf35417ff99a78bc.png)

**DefaultResultSetHandler类**

![image-20200927213432345](https://img-blog.csdnimg.cn/img_convert/8f3c9860b070b2f2ae95c743b0c92d5f.png)

然后往后返回，就到了doQuery，准备预编译，产生PrepareStatement对象

**SimpleExecutor类**

![image-20200927213808622](https://img-blog.csdnimg.cn/img_convert/aef7db4a48e7e39829bc2d5e42937649.png)

然后调用PreparedStatementHandler对语句进行预编译

**SimpleExecutor类**

![image-20200927213906298](https://img-blog.csdnimg.cn/img_convert/b4d97145d7df65fa5f9f0a22cdc3c128.png)

![image-20200927214016699](https://img-blog.csdnimg.cn/img_convert/bdeb7527ba88b44f93e79282ba7c1458.png)

进一步往下走

**RoutingStatementHandler类**

![image-20200927214334067](https://img-blog.csdnimg.cn/img_convert/2a825bdd2680ed4b6d02b0f9ec706033.png)

再往下走一步准备预编译

**PreparedStatementHandler(BaseStatementHandler)类**

![image-20200927214529584](https://img-blog.csdnimg.cn/img_convert/e7f7148c8b462dfb3471dab08d9059ad.png)

到这里已经与编译好了，这时候，控制台就会输出日志

**PreparedStatementHandler类**

![image-20200927214746498](https://img-blog.csdnimg.cn/img_convert/8d421bcf472b7ba3c000d0f066dc8694.png)

控制台输出

![image-20200927214852754](https://img-blog.csdnimg.cn/img_convert/d90766af9eb127e723dfdf8f79ee0971.png)

预编译完成，我们回到这里，接下来执行`handler.parameterize(stmt);`这一步就开始设置参数

**SimpleExecutor类**

![image-20200927215013760](https://img-blog.csdnimg.cn/img_convert/b7388f63dc6e1e99bdf39362e7e32ab2.png)

调用方法，设置参数

**RoutingStatementHandler类**

![image-20200927215217381](https://img-blog.csdnimg.cn/img_convert/f21632d569aaf82e8b78bf3a681c1f50.png)

**PreparedStatementHandler类**

![image-20200927215318334](https://img-blog.csdnimg.cn/img_convert/00bc1ad1aeb288d187d5d11c3d4550dd.png)

在这里也会做一系列处理，我们重点看绿色部分，他是调用TypeHandler给预编译的sqPreparedStatement设置参数

**DefaultParameterHandler类**

![image-20200927215523194](https://img-blog.csdnimg.cn/img_convert/335190d6fea3ac4b9a76d3435efbc399.png)

参数也设置完了，接下来我们该执行了，返回到SimpleExecutor的doQuery进行`handler.query(stmt, resultHandler);`查询，查询出来的结果交给ResultHandler来处理，往下走，看他怎么实现的

**SimpleExecutor类**

![image-20200927215821166](https://img-blog.csdnimg.cn/img_convert/ba6524de4596a4324b4505e294fc6d55.png)

**RoutingStatementHandler类**

![image-20200927215934661](https://img-blog.csdnimg.cn/img_convert/f1d7f0e7537e983dec8c8836245563ed.png)

我们会发现最终他调用的是JDBC原生的`ps.execute();`，查询到的结果交给了`ResultSetHandler`来处理，我们来看看他是怎么处理结果的

**PreparedStatementHandler类**

![image-20200927215954275](https://img-blog.csdnimg.cn/img_convert/6b7607a9af4ed9085bef8b8a52c04490.png)

查出来的数据使用`ResultSetHandler`处理结果，内部其实是用的TypeHandler来获取值的，最终给我们返回数据

**DefaultResultSetHandler类**

![image-20200927220343030](https://img-blog.csdnimg.cn/img_convert/3e7665befdee65b1d9fcef3e49c6940d.png)

到这里查询的流程就已经完成了，接下来就是返回数据了，这里就没什么好看的了

我们可以把以上过程中重要的步骤梳理成一个时序图，注意对应类不一定对应，重点看执行步骤

![image-20200928073305137](https://img-blog.csdnimg.cn/img_convert/9b9a977dc38781c7cd950e8fa96c5a69.png)

我们也可以用一张流程图简单总结下

- StatementHandler：处理sql语句预编译，设置参数等相关工作；
- ParameterHandler：设置预编译参数用的
- ResultHandler：处理结果集
- TypeHandler：在整个过程中，进行数据库类型和javaBean类型的映射

![image-20200928073556359](https://img-blog.csdnimg.cn/img_convert/2d4f22e43cd68452a90a0faf475beb1a.png)

### 8.7、工作原理总结

我们以一张图的形式简单梳理一下mybatis的工作原理

![image-20200928073843234](https://img-blog.csdnimg.cn/img_convert/cd9006b7be6eea4b6e414eab31f501e0.png)

## 第九章 MyBatis3的插件开发

### 9.1、概述

MyBatis在四大对象（Executor、StatementHandler、ParameterHandler、ResultSetHandler）的创建过程中，都会有插件进行介入。插件可以利用动态代理机制一层层的包装目标对象，而实现在目标对象执行目标方法之前进行拦截的效果。

![image-20200928075046519](https://img-blog.csdnimg.cn/img_convert/a1f7d34ff8393df4e112414e64091e62.png)

![image-20200928075122512](https://img-blog.csdnimg.cn/img_convert/654898510b0d9d9784c68253e2764f8b.png)

![image-20200928075202319](https://img-blog.csdnimg.cn/img_convert/0c9d008404f5605723e1ec97f0b9d33b.png)

![image-20200928075230293](https://img-blog.csdnimg.cn/img_convert/fba86e2c601465aca06a995c257fb8b7.png)

MyBatis允许在已映射语句执行过程中的某一点进行拦截调用。

默认情况下，MyBatis 允许使用插件来拦截的方法调用包括：

- Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed)
- StatementHandler (prepare, parameterize, batch, update, query)
- ParameterHandler (getParameterObject, setParameters)
- ResultSetHandler (handleResultSets, handleOutputParameters)

### 9.2、插件开发

#### 9.2.1、环境准备

直接拷贝mybatis-analysis，然后重名为mybatis-plugin，然后在src文件夹中创建存放插件的`com.caochenlei.mybatis.plugin`包

#### 9.2.2、开发插件

编写插件实现Interceptor接口，并使用 @Intercepts注解完成插件签名

MyFirstPlugin.java（全路径：/mybatis-plugin/src/com/caochenlei/mybatis/plugin/MyFirstPlugin.java）

```java
package com.caochenlei.mybatis.plugin;

import org.apache.ibatis.executor.statement.StatementHandler;
import org.apache.ibatis.plugin.Interceptor;
import org.apache.ibatis.plugin.Intercepts;
import org.apache.ibatis.plugin.Invocation;
import org.apache.ibatis.plugin.Signature;

//标注这是一个插件
@Intercepts({ @Signature(//设置插件签名
				type = StatementHandler.class, //要拦截的对象
				method = "parameterize", //要拦截的方法
				args = java.sql.Statement.class) })//要拦截参数的类型
public class MyFirstPlugin implements Interceptor {

	//处理方法
	@Override
	public Object intercept(Invocation invocation) throws Throwable {
		System.out.println(" MyFirstPlugin Running ... ");
		Object proceed = invocation.proceed();//代表放行
		System.out.println(" MyFirstPlugin Back ... ");
		return proceed;
	}

}
12345678910111213141516171819202122232425
```

MySecondPlugin.java（全路径：/mybatis-plugin/src/com/caochenlei/mybatis/plugin/MySecondPlugin.java）

```java
package com.caochenlei.mybatis.plugin;

import org.apache.ibatis.executor.statement.StatementHandler;
import org.apache.ibatis.plugin.Interceptor;
import org.apache.ibatis.plugin.Intercepts;
import org.apache.ibatis.plugin.Invocation;
import org.apache.ibatis.plugin.Signature;

//标注这是一个插件
@Intercepts({ @Signature(//设置插件签名
				type = StatementHandler.class, //要拦截的对象
				method = "parameterize", //要拦截的方法
				args = java.sql.Statement.class) })//要拦截参数的类型
public class MySecondPlugin implements Interceptor {

	//处理方法
	@Override
	public Object intercept(Invocation invocation) throws Throwable {
		System.out.println(" MySecondPlugin Running ... ");
		Object proceed = invocation.proceed();//代表放行
		System.out.println(" MySecondPlugin Back ... ");
		return proceed;
	}

}
12345678910111213141516171819202122232425
```

#### 9.2.3、注册插件

在mybatis-config.xml的settings和environments中间配置插件，只需要把插件的全类名配上就成了，是不是很简单

```xml
	<!-- 配置插件 -->
	<plugins>
		<plugin interceptor="com.caochenlei.mybatis.plugin.MyFirstPlugin" />
		<plugin interceptor="com.caochenlei.mybatis.plugin.MySecondPlugin" />
	</plugins>
12345
```

#### 9.2.4、测试插件

我们直接运行一个查询方法，运行效果如控制台

![image-20200928081722777](https://img-blog.csdnimg.cn/img_convert/ed3b536b469c4543c7d2ef6c14f301ec.png)

### 9.3、插件原理

1. 按照插件注解声明，按照插件配置顺序调用插件plugin方法，生成被拦截对象的动态代理。
2. 多个插件依次生成目标对象的代理对象，层层包裹，先声明的先包裹，形成拦截器链。
3. 目标方法执行时依次从外到内执行插件的intercept方法。

可以用下边这张图进行描述：

![image-20200928075613956](https://img-blog.csdnimg.cn/img_convert/7ea5bda3244457f5f90a63021d4830b7.png)

### 9.4、使用场景

- 进行分页查询
- 进行批量操作
- 调用存储过程
- 处理枚举类型

#### 9.4.1、进行分页查询

PageHelper是MyBatis中非常方便的第三方分页插件。

如果你也在用 MyBatis，建议尝试该分页插件，这一定是**最方便**使用的分页插件。

下载地址：[点击打开](https://github.com/pagehelper/Mybatis-PageHelper)

文档地址：[点击打开](https://github.com/pagehelper/Mybatis-PageHelper/blob/master/README_zh.md)

API地址：[点击打开](https://apidoc.gitee.com/free/Mybatis_PageHelper/)

插件下载：

打开下载地址，然后找到右侧的Releases，点击

![image-20200928083316332](https://img-blog.csdnimg.cn/img_convert/0d08490ec8a514b6cd4f3b943ba29760.png)

![image-20200928083427066](https://img-blog.csdnimg.cn/img_convert/2ebc8f12d753134dd08763e9e7c0d153.png)

![image-20200928084319133](https://img-blog.csdnimg.cn/img_convert/4526af188be72e08a246d095d8631809.png)

解压之后，使用maven进行安装，如果不会安装，直接使用我提供好的jar包

![image-20200928085400705](https://img-blog.csdnimg.cn/img_convert/ac3057b9d8a82d1cac05291cbf6afa9a.png)

既然我们要使用分页插件，那我们需要导入那几个包呢？

- pagehelper-5.2.0.jar
- C:\Users\CaoChenLei.m2\repository\com\github\jsqlparser\jsqlparser\3.2\jsqlparser-3.2.jar

我们把以上两个插件放到mybatis-plugin的lib中，然后Build Path

![image-20200928090105833](https://img-blog.csdnimg.cn/img_convert/5a0bc98bee8b8b52bfc7a85e4961421e.png)

既然分页插件有了，我们该怎么用呢？

> 官方文档：[点击查看](https://github.com/pagehelper/Mybatis-PageHelper/blob/master/wikis/zh/HowToUse.md)

**第一步：如何配置参数**

```xml
<!-- 
    plugins在配置文件中的位置必须符合要求，否则会报错，顺序如下:
    properties?, settings?, 
    typeAliases?, typeHandlers?, 
    objectFactory?,objectWrapperFactory?, 
    plugins?, 
    environments?, databaseIdProvider?, mappers?
-->
<plugins>
    <!-- com.github.pagehelper为PageHelper类所在包名 -->
    <plugin interceptor="com.github.pagehelper.PageInterceptor">
        <!-- 使用下面的方式配置参数，后面会有所有的参数介绍 -->
        <property name="param1" value="value1"/>
	</plugin>
</plugins>
123456789101112131415
```

分页插件可选参数如下：

- `dialect`：默认情况下会使用 PageHelper 方式进行分页，如果想要实现自己的分页逻辑，可以实现 `Dialect`(`com.github.pagehelper.Dialect`) 接口，然后配置该属性为实现类的全限定名称。

下面几个参数都是针对默认 dialect 情况下的参数。使用自定义 dialect 实现时，下面的参数没有任何作用。

1. `helperDialect`：分页插件会自动检测当前的数据库链接，自动选择合适的分页方式。 你可以配置`helperDialect`属性来指定分页插件使用哪种方言。配置时，可以使用下面的缩写值：
   `oracle`,`mysql`,`mariadb`,`sqlite`,`hsqldb`,`postgresql`,`db2`,`sqlserver`,`informix`,`h2`,`sqlserver2012`,`derby`
   **特别注意：**使用 SqlServer2012 数据库时，需要手动指定为 `sqlserver2012`，否则会使用 SqlServer2005 的方式进行分页。
   你也可以实现 `AbstractHelperDialect`，然后配置该属性为实现类的全限定名称即可使用自定义的实现方法。
2. `offsetAsPageNum`：默认值为 `false`，该参数对使用 `RowBounds` 作为分页参数时有效。 当该参数设置为 `true` 时，会将 `RowBounds` 中的 `offset` 参数当成 `pageNum` 使用，可以用页码和页面大小两个参数进行分页。
3. `rowBoundsWithCount`：默认值为`false`，该参数对使用 `RowBounds` 作为分页参数时有效。 当该参数设置为`true`时，使用 `RowBounds` 分页会进行 count 查询。
4. `pageSizeZero`：默认值为 `false`，当该参数设置为 `true` 时，如果 `pageSize=0` 或者 `RowBounds.limit = 0` 就会查询出全部的结果（相当于没有执行分页查询，但是返回结果仍然是 `Page` 类型）。
5. `reasonable`：分页合理化参数，默认值为`false`。当该参数设置为 `true` 时，`pageNum<=0` 时会查询第一页， `pageNum>pages`（超过总数时），会查询最后一页。默认`false` 时，直接根据参数进行查询。
6. `params`：为了支持`startPage(Object params)`方法，增加了该参数来配置参数映射，用于从对象中根据属性名取值， 可以配置 `pageNum,pageSize,count,pageSizeZero,reasonable`，不配置映射的用默认值， 默认值为`pageNum=pageNum;pageSize=pageSize;count=countSql;reasonable=reasonable;pageSizeZero=pageSizeZero`。
7. `supportMethodsArguments`：支持通过 Mapper 接口参数来传递分页参数，默认值`false`，分页插件会从查询方法的参数值中，自动根据上面 `params` 配置的字段中取值，查找到合适的值时就会自动分页。 使用方法可以参考测试代码中的 `com.github.pagehelper.test.basic` 包下的 `ArgumentsMapTest` 和 `ArgumentsObjTest`。
8. `autoRuntimeDialect`：默认值为 `false`。设置为 `true` 时，允许在运行时根据多数据源自动识别对应方言的分页 （不支持自动选择`sqlserver2012`，只能使用`sqlserver`），用法和注意事项参考下面的**场景五**。
9. `closeConn`：默认值为 `true`。当使用运行时动态数据源或没有设置 `helperDialect` 属性自动获取数据库类型时，会自动获取一个数据库连接， 通过该属性来设置是否关闭获取的这个连接，默认`true`关闭，设置为 `false` 后，不会关闭获取的连接，这个参数的设置要根据自己选择的数据源来决定。
10. `aggregateFunctions`(5.1.5+)：默认为所有常见数据库的聚合函数，允许手动添加聚合函数（影响行数），所有以聚合函数开头的函数，在进行 count 转换时，会套一层。其他函数和列会被替换为 count(0)，其中count列可以自己配置。

**重要提示：**

当 `offsetAsPageNum=false` 的时候，由于 `PageNum` 问题，`RowBounds`查询的时候 `reasonable` 会强制为 `false`。使用 `PageHelper.startPage` 方法不受影响。

**第二步：如何选择参数**

单独看每个参数的说明可能是一件让人不爽的事情，这里列举一些可能会用到某些参数的情况。

**场景一**

如果你仍然在用类似ibatis式的命名空间调用方式，你也许会用到`rowBoundsWithCount`， 分页插件对`RowBounds`支持和 MyBatis 默认的方式是一致，默认情况下不会进行 count 查询，如果你想在分页查询时进行 count 查询， 以及使用更强大的 `PageInfo` 类，你需要设置该参数为 `true`。

**注：** `PageRowBounds` 想要查询总数也需要配置该属性为 `true`。

**场景二**

如果你仍然在用类似ibatis式的命名空间调用方式，你觉得 `RowBounds` 中的两个参数 `offset,limit` 不如 `pageNum,pageSize` 容易理解， 你可以使用 `offsetAsPageNum` 参数，将该参数设置为 `true` 后，`offset`会当成 `pageNum` 使用，`limit` 和 `pageSize` 含义相同。

**场景三**

如果觉得某个地方使用分页后，你仍然想通过控制参数查询全部的结果，你可以配置 `pageSizeZero` 为 `true`， 配置后，当 `pageSize=0` 或者 `RowBounds.limit = 0` 就会查询出全部的结果。

**场景四**

如果你分页插件使用于类似分页查看列表式的数据，如新闻列表，软件列表， 你希望用户输入的页数不在合法范围（第一页到最后一页之外）时能够正确的响应到正确的结果页面， 那么你可以配置 `reasonable` 为 `true`，这时如果 `pageNum<=0` 会查询第一页，如果 `pageNum>总页数` 会查询最后一页。

**场景五**

如果你在 Spring 中配置了动态数据源，并且连接不同类型的数据库，这时你可以配置 `autoRuntimeDialect` 为 `true`，这样在使用不同数据源时，会使用匹配的分页进行查询。 这种情况下，你还需要特别注意 `closeConn` 参数，由于获取数据源类型会获取一个数据库连接，所以需要通过这个参数来控制获取连接后，是否关闭该连接。 默认为 `true`，有些数据库连接关闭后就没法进行后续的数据库操作。而有些数据库连接不关闭就会很快由于连接数用完而导致数据库无响应。所以在使用该功能时，特别需要注意你使用的数据源是否需要关闭数据库连接。

当不使用动态数据源而只是自动获取 `helperDialect` 时，数据库连接只会获取一次，所以不需要担心占用的这一个连接是否会导致数据库出错，但是最好也根据数据源的特性选择是否关闭连接。

**第三步：如何使用分页**

分页插件支持以下几种调用方式：

```xml
//第一种，RowBounds方式的调用
List<User> list = sqlSession.selectList("x.y.selectIf", null, new RowBounds(0, 10));

//第二种，Mapper接口方式的调用，推荐这种使用方式。
PageHelper.startPage(1, 10);
List<User> list = userMapper.selectIf(1);

//第三种，Mapper接口方式的调用，推荐这种使用方式。
PageHelper.offsetPage(1, 10);
List<User> list = userMapper.selectIf(1);

//第四种，参数方法调用
//存在以下 Mapper 接口方法，你不需要在 xml 处理后两个参数
public interface CountryMapper {
    List<User> selectByPageNumSize(
            @Param("user") User user,
            @Param("pageNum") int pageNum, 
            @Param("pageSize") int pageSize);
}
//配置supportMethodsArguments=true
//在代码中直接调用：
List<User> list = userMapper.selectByPageNumSize(user, 1, 10);

//第五种，参数对象
//如果 pageNum 和 pageSize 存在于 User 对象中，只要参数有值，也会被分页
//有如下 User 对象
public class User {
    //其他fields
    //下面两个参数名和 params 配置的名字一致
    private Integer pageNum;
    private Integer pageSize;
}
//存在以下 Mapper 接口方法，你不需要在 xml 处理后两个参数
public interface CountryMapper {
    List<User> selectByPageNumSize(User user);
}
//当 user 中的 pageNum!= null && pageSize!= null 时，会自动分页
List<User> list = userMapper.selectByPageNumSize(user);

//第六种，ISelect 接口方式
//jdk6,7用法，创建接口
Page<User> page = PageHelper.startPage(1, 10).doSelectPage(new ISelect() {
    @Override
    public void doSelect() {
        userMapper.selectGroupBy();
    }
});
//jdk8 lambda用法
Page<User> page = PageHelper.startPage(1, 10).doSelectPage(()-> userMapper.selectGroupBy());

//也可以直接返回PageInfo，注意doSelectPageInfo方法和doSelectPage
pageInfo = PageHelper.startPage(1, 10).doSelectPageInfo(new ISelect() {
    @Override
    public void doSelect() {
        userMapper.selectGroupBy();
    }
});
//对应的lambda用法
pageInfo = PageHelper.startPage(1, 10).doSelectPageInfo(() -> userMapper.selectGroupBy());

//count查询，返回一个查询语句的count数
long total = PageHelper.count(new ISelect() {
    @Override
    public void doSelect() {
        userMapper.selectLike(user);
    }
});
//lambda
total = PageHelper.count(()->userMapper.selectLike(user));
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869
```

**重要提示**

**`PageHelper.startPage`方法重要提示**

只有紧跟在`PageHelper.startPage`方法后的**第一个**Mybatis的**查询（Select）**方法会被分页。

**请不要配置多个分页插件**

请不要在系统中配置多个分页插件(使用Spring时,`mybatis-config.xml`和`Spring<bean>`配置方式，请选择其中一种，不要同时配置多个分页插件)！

**分页插件不支持带有`for update`语句的分页**

对于带有`for update`的sql，会抛出运行时异常，对于这样的sql建议手动分页，毕竟这样的sql需要重视。

**分页插件不支持嵌套结果映射**

由于嵌套结果方式会导致结果集被折叠，因此分页查询的结果在折叠后总数会减少，所以无法保证分页结果数量正确。

**第四步：具体的使用**

注册插件

```xml
	<!-- 配置插件 -->
	<plugins>
		<plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
	</plugins>
1234
```

常见的第一种方式：

```java
// 获取第1页，2条内容，默认查询总数count
PageHelper.startPage(1, 2);

EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
List<Employee> emps = employeeMapper.getEmps();
for (Employee employee : emps) {
	System.out.println(employee);
}
12345678
```

常见的第二种方式：

```java
// 获取第1页，2条内容，默认查询总数count
PageHelper.startPage(1, 2);

EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
List<Employee> emps = employeeMapper.getEmps();
PageInfo<Employee> pageInfo = new PageInfo<Employee>(emps);
for (Employee employee : pageInfo.getList()) {
	System.out.println(employee);
}

// PageInfo包含了非常全面的分页属性
System.out.println(pageInfo.getPageNum());
System.out.println(pageInfo.getPageSize());
System.out.println(pageInfo.getStartRow());
System.out.println(pageInfo.getEndRow());
System.out.println(pageInfo.getTotal());
System.out.println(pageInfo.getPages());
System.out.println(pageInfo.getNavigateFirstPage());
System.out.println(pageInfo.getNavigateLastPage());
System.out.println(pageInfo.isIsFirstPage());
System.out.println(pageInfo.isIsLastPage());
System.out.println(pageInfo.isHasPreviousPage());
System.out.println(pageInfo.isHasNextPage());
1234567891011121314151617181920212223
```

#### 9.4.2、进行批量操作

默认的 openSession() 方法没有参数，它的创建有如下特性

- 会开启一个事务(也就是不自动提交)。
- 连接对象会从由活动环境配置的数据源实例得到。
- 事务隔离级别将会使用驱动或数据源的默认设置。
- 预处理语句不会被复用，也不会批量处理更新。

还有的 openSession(true) 方法带有参数，它的创建有如下特性

- 会开启一个事务(会自动提交)。
- 连接对象会从由活动环境配置的数据源实例得到。
- 事务隔离级别将会使用驱动或数据源的默认设置。
- 预处理语句不会被复用，也不会批量处理更新。

等等，mybatis提供了很多重载方法，如下

![image-20200928133302966](https://img-blog.csdnimg.cn/img_convert/9a6a69dbefe0af4b3f2e91a965c5e94b.png)

openSession 方法提供了ExecutorType类型的参数，它是一个枚举类型，它可以有以下取值

- ExecutorType.SIMPLE：这个执行器类型不做特殊的事情（这是默认装配 的），它为每个语句的执行创建一个新的预处理语句。
- ExecutorType.REUSE：这个执行器类型会复用预处理语句。
- ExecutorType.BATCH：这个执行器会批量执行所有更新语句。

批量操作我们是使用mybatis提供的BatchExecutor进行的， 他的底层就是通过jdbc攒sql的方式进行的。我们可以让他攒够一定数量后发给数据库一次。而不是一条一条给数据库发送语句，而且有的数据库对同一次执行的语句大小也有长度限制，那么，我们具体怎么使用mybatis给我们提供的批量操作呢，我们这里以批量插入为例进行讲解。

**添加接口方法：**

```java
public Long addEmp(Employee employee);
1
```

**添加映射信息：**

```xml
<!-- public Long addEmp(Employee employee); -->
<insert id="addEmp" parameterType="com.caochenlei.mybatis.crud.Employee" useGeneratedKeys="true" keyProperty="id" databaseId="mysql">
	INSERT INTO `employee`(`last_name`,`email`,`gender`)
	VALUES(#{lastName},#{email},#{gender})
</insert>
12345
```

**添加测试信息：**

最重要的代码`sqlSessionFactory.openSession(ExecutorType.BATCH);`和`openSession.commit();`

```java
SqlSession openSession = sqlSessionFactory.openSession(ExecutorType.BATCH);
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
long start = System.currentTimeMillis();
for (int i = 0; i < 10000; i++) {
	String name = UUID.randomUUID().toString().substring(0, 5);
	employeeMapper.addEmp(new Employee(null, "lisi-" + name, "lisi@qq.com", "男"));
}
openSession.commit();
openSession.close();
long end = System.currentTimeMillis();
System.out.println("耗时时间：" + (end - start));
1234567891011
```

**控制台的输出：**

![image-20200928140128354](https://img-blog.csdnimg.cn/img_convert/5bc0f4230592ba0276f59e9f01fb392d.png)

**注意的事项：**

1. 与Spring整合中，我们推荐额外的配置一个可以专门用来执行批量操作的sqlSession，需要用到批量操作的时候，我们可以注入配置的这个批量sqlSession，通过他获取到mapper映射器进行操作。
2. 批量操作是在session.commit()以后才发送sql语句给数据库进行执行的。
3. 如果我们想让其提前执行，以方便后续可能的查询操作获取数据，我们可以使用sqlSession.flushStatements()方 法，让其直接冲刷到数据库进行执行。

#### 9.4.3、调用存储过程

实际开发中，我们通常也会写一些存储过程，mybatis也支持对存储过程的调用，一个简单的存储过程的格式如下：

```sql
#存储过程定义
delimiter $$
create procedure test()
begin
	select 'hello,world';
end $$
delimiter ;

#存储过程调用
call test();
12345678910
```

那我们mybatis是如何调用的呢？

1. select标签中设置属性：statementType=“CALLABLE”
2. 标签体中调用语法： {call procedure_name(#{param1_info},#{param2_info})}

接下来，我们就以mysql为例进行测试，首先先创建以上那个存储过程，然后调用，观察是否成功

**添加接口方法：**

```java
public String getProcedureTest();
1
```

**添加映射信息：**

```xml
<!-- public String getProcedureTest(); -->
<select id="getProcedureTest" resultType="string">
	call test();
</select>
1234
```

**添加测试信息：**

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
String procedureTest = employeeMapper.getProcedureTest();
System.out.println(procedureTest);
123
```

**控制台的输出：**

![image-20200928144910121](https://img-blog.csdnimg.cn/img_convert/4af68ee93fdc4978aea2614f91c55f93.png)

------

上边我们简单使用了一下存储过程，我们接下来切换到Oracle数据库，进行一个带有游标复杂的存储过程的调用，mybatis对存储过程的游标提供了一个JdbcType=CURSOR的支持， 可以智能的把游标读取到的数据，映射到我们声明的结果集中，接下来我们实现Oracle带游标分页的存储过程的调用

在Oracle数据库中定义存储过程

```sql
CREATE OR REPLACE 
PROCEDURE hello_test(p_start IN INT,p_end IN INT,p_count OUT INT,ref_cur OUT sys_refcursor) AS
BEGIN
    SELECT COUNT(*) INTO p_count FROM employee;
    OPEN ref_cur FOR
    SELECT * FROM (SELECT e.*, rownum AS hanghao FROM employee e) emp 
    WHERE emp.hanghao >= p_start AND emp.hanghao <= p_end;
END hello_test;
12345678
```

**创建分页对象：**

OraclePage.java（全路径：/mybatis-plugin/src/com/caochenlei/mybatis/crud/OraclePage.java）

```java
package com.caochenlei.mybatis.crud;

import java.util.List;

/**
 * 封装分页查询数据
 */
public class OraclePage {
	private int start;
	private int end;
	private int count;
	private List<Employee> emps;

	public int getStart() {
		return start;
	}

	public void setStart(int start) {
		this.start = start;
	}

	public int getEnd() {
		return end;
	}

	public void setEnd(int end) {
		this.end = end;
	}

	public int getCount() {
		return count;
	}

	public void setCount(int count) {
		this.count = count;
	}

	public List<Employee> getEmps() {
		return emps;
	}

	public void setEmps(List<Employee> emps) {
		this.emps = emps;
	}
}

12345678910111213141516171819202122232425262728293031323334353637383940414243444546
```

**添加接口方法：**

```java
public void getPageByProcedure(OraclePage page);
1
```

**添加映射信息：**

```xml
<!-- public void getPageByProcedure(OraclePage page); -->
<select id="getPageByProcedure" statementType="CALLABLE" databaseId="oracle">
	{call hello_test(
	#{start,mode=IN,jdbcType=INTEGER},
	#{end,mode=IN,jdbcType=INTEGER},
	#{count,mode=OUT,jdbcType=INTEGER},
	#{emps,mode=OUT,jdbcType=CURSOR,javaType=ResultSet,resultMap=empMap}
	)}
</select>
<resultMap type="com.caochenlei.mybatis.crud.Employee" id="empMap">
	<!-- 设置主键映射 -->
	<id column="ID" property="id" />
	<!-- 普通字段映射 -->
	<result column="LAST_NAME" property="lastName" />
	<result column="EMAIL" property="email" />
	<result column="GENDER" property="gender" />
</resultMap>
1234567891011121314151617
```

**添加测试信息：**

```java
EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
OraclePage oraclePage = new OraclePage();
oraclePage.setStart(1);
oraclePage.setEnd(3);
employeeMapper.getPageByProcedure(oraclePage);
System.out.println("总记录数：" + oraclePage.getCount());
List<Employee> emps = oraclePage.getEmps();
for (Employee employee : emps) {
	System.out.println(employee);
}
12345678910
```

**控制台的输出：**

![image-20200928165100784](https://img-blog.csdnimg.cn/img_convert/30d3057a0b9f5780dccad19d0d8e8083.png)

#### 9.4.4、处理枚举类型

**第一种方式**：`测试全局配置EnumTypeHandler`

环境搭建，拷贝mybatis-plugin重命名为mybatis-enum，去掉以下东西

1. 删掉Employee.java、EmployeeTest.java、OraclePage.java、com.caochenlei.mybatis.crud

2. 删掉EmployeeMapper.java、EmployeeMapper.xml

3. 删掉MyFirstPlugin.java、MySecondPlugin.java

4. 在src下创建包`com.caochenlei.mybatis.pojo`

5. 修改mybatis-config.xml为以下代码

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   	<!-- 加载数据库配置文件信息 -->
   	<properties resource="dbconfig.properties"></properties>
   
   	<!-- 配置框架的全局配置信息 -->
   	<settings>
   		<setting name="mapUnderscoreToCamelCase" value="true" />
   		<setting name="jdbcTypeForNull" value="NULL" />
   	</settings>
   
   	<!-- 使用框架提供的枚举处理 -->
   	<typeHandlers>
   		<typeHandler handler="org.apache.ibatis.type.EnumTypeHandler" javaType="com.caochenlei.mybatis.pojo.UserStatus" />
   	</typeHandlers>
   
   	<!-- 配置框架的多数据源信息 -->
   	<environments default="dev_mysql">
   		<!-- 配置mysql开发环境 -->
   		<environment id="dev_mysql">
   			<transactionManager type="JDBC" />
   			<dataSource type="POOLED">
   				<property name="driver" value="${mysql.driver}" />
   				<property name="url" value="${mysql.url}" />
   				<property name="username" value="${mysql.username}" />
   				<property name="password" value="${mysql.password}" />
   			</dataSource>
   		</environment>
   		<!-- 配置oracle开发环境 -->
   		<environment id="dev_oracle">
   			<transactionManager type="JDBC" />
   			<dataSource type="POOLED">
   				<property name="driver" value="${oracle.driver}" />
   				<property name="url" value="${oracle.url}" />
   				<property name="username" value="${oracle.username}" />
   				<property name="password" value="${oracle.password}" />
   			</dataSource>
   		</environment>
   	</environments>
   
   	<!-- 数据库厂商起别名 -->
   	<databaseIdProvider type="DB_VENDOR">
   		<property name="MySQL" value="mysql" />
   		<property name="Oracle" value="oracle" />
   	</databaseIdProvider>
   
   	<!-- 批量注册映射文件 -->
   	<mappers>
   		<!-- 使用包扫描 -->
   		<package name="com.caochenlei.mybatis.mapper" />
   	</mappers>
   </configuration>
   12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455
   ```

6. 最终项目的结构

   ![image-20200928172245040](https://img-blog.csdnimg.cn/img_convert/3dc543e184d1c9df892f433e4c94a6b9.png)

创建数据库，以方便我们来进行测试

```sql
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `status` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
123456
```

在com.caochenlei.mybatis.pojo中创建UserStatus枚举对象

UserStatus.java（全路径：/mybatis-enum/src/com/caochenlei/mybatis/pojo/UserStatus.java）

```java
package com.caochenlei.mybatis.pojo;

public enum UserStatus {
	LOGIN, LOGOFF, FROZEN
}
12345
```

在com.caochenlei.mybatis.pojo中创建User对象

User.java（全路径：/mybatis-enum/src/com/caochenlei/mybatis/pojo/User.java）

```java
package com.caochenlei.mybatis.pojo;

public class User {
	private Integer id;
	private String name;
	private UserStatus status;

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

	public UserStatus getStatus() {
		return status;
	}

	public void setStatus(UserStatus status) {
		this.status = status;
	}

	@Override
	public String toString() {
		return "User [id=" + id + ","
				+ " name=" + name + ","
				+ " status=" + status + "]";
	}
}
1234567891011121314151617181920212223242526272829303132333435363738
```

在com.caochenlei.mybatis.mapper中创建UserMapper接口

UserMapper.java（全路径：/mybatis-enum/src/com/caochenlei/mybatis/mapper/UserMapper.java）

```java
package com.caochenlei.mybatis.mapper;

import com.caochenlei.mybatis.pojo.User;

public interface UserMapper {
	public void insertOne(User user);

	public User selectOne(Integer id);
}
123456789
```

在com.caochenlei.mybatis.mapper中创建UserMapper映射xml

UserMapper.xml（全路径：/mybatis-enum/src/com/caochenlei/mybatis/mapper/UserMapper.xml）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.caochenlei.mybatis.mapper.UserMapper">

	<!-- public void insertOne(User user); -->
	<insert id="insertOne" parameterType="com.caochenlei.mybatis.pojo.User">
		INSERT INTO user(id,name,status) VALUES(null,#{name},#{status})
	</insert>
	
	<!-- public User selectOne(Integer id); -->
	<select id="selectOne" resultType="com.caochenlei.mybatis.pojo.User">
		SELECT * FROM user WHERE id = #{id}
	</select>
	
</mapper>
1234567891011121314151617
```

在com.caochenlei.mybatis.mapper中创建UserMapperTest测试类

UserMapperTest.java（全路径：/mybatis-enum/src/com/caochenlei/mybatis/mapper/UserMapperTest.java）

```java
package com.caochenlei.mybatis.mapper;

import java.io.IOException;
import java.io.InputStream;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import com.caochenlei.mybatis.pojo.User;
import com.caochenlei.mybatis.pojo.UserStatus;

public class UserMapperTest {

	@Test
	public void insertOneTest() {
		try {
			String resource = "mybatis-config.xml";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
			SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
			SqlSession openSession = sqlSessionFactory.openSession(true);

			UserMapper userMapper = openSession.getMapper(UserMapper.class);
			userMapper.insertOne(new User(null, "zhangsan", UserStatus.LOGIN));

			openSession.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	@Test
	public void selectOneTest() {
		try {
			String resource = "mybatis-config.xml";
			InputStream inputStream = Resources.getResourceAsStream(resource);
			SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
			SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
			SqlSession openSession = sqlSessionFactory.openSession(true);

			UserMapper userMapper = openSession.getMapper(UserMapper.class);
			User user = userMapper.selectOne(1);
			System.out.println(user.getStatus());

			openSession.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354
```

先运行`public void insertOneTest()`，控制台输出

![image-20200928181546853](https://img-blog.csdnimg.cn/img_convert/c41739f04588e02369a40a1477b29ebd.png)

再运行`public void selectOneTest()`，控制台输出

![image-20200928181634248](https://img-blog.csdnimg.cn/img_convert/acd53e1319dcdb0fbbb20d4ebe680d77.png)

最终结论，如果我们处理枚举的typeHandler使用的是`org.apache.ibatis.type.EnumTypeHandler`，那么在存储的时候是直接存储的**枚举的名称**。

**第二种方式：**`测试全局配置EnumOrdinalTypeHandler`

拷贝第一种方式的工程，命名为mybatis-enum2

修改配置文件中的typeHandler为`org.apache.ibatis.type.EnumOrdinalTypeHandler`

修改测试文件中的`User user = userMapper.selectOne(2);`，查询2不要查询1（插入的是几就查询几）

先运行`public void insertOneTest()`，控制台输出

![image-20200928190605196](https://img-blog.csdnimg.cn/img_convert/b03750486b482d4cf4cd092c6a18be32.png)

再运行`public void selectOneTest()`，控制台输出

![image-20200928191159278](https://img-blog.csdnimg.cn/img_convert/62fbff0b48211c5dc1827ff1ed1123f3.png)

虽然仍然是LOGIN字符串，但是你看数据库中，他已经存储为枚举的索引了

![image-20200928191244884](https://img-blog.csdnimg.cn/img_convert/f3c36f15b3f79ef094ac629cc7c305cc.png)

最终结论，如果我们处理枚举的typeHandler使用的是`org.apache.ibatis.type.EnumOrdinalTypeHandler`，那么在存储的时候是直接存储的**枚举的索引**。

**第三种方式：**`测试自定义的MyEnumTypeHandler`

拷贝第一种方式的工程，命名为mybatis-enum3

修改配置文件中的typeHandler为`com.caochenlei.mybatis.plugin.MyEnumTypeHandler`，咱们先写上，一会实现

修改测试文件中的`User user = userMapper.selectOne(3);`，查询3不要查询2也不要查询1（插入的是几就查询几）

删除之前的UserStatus.java，然后重新创建一个带参数的枚举类，默认的枚举处理器是处理不了的，我们需要自定义枚举处理器处理才行

UserStatus.java（全路径：/mybatis-enum3/src/com/caochenlei/mybatis/pojo/UserStatus.java）

```java
package com.caochenlei.mybatis.pojo;

public enum UserStatus {
	LOGIN(200, "登录成功"), LOGOFF(300, "登录失败"), FROZEN(400, "账号冻结");

	private Integer code;
	private String msg;

	private UserStatus(Integer code, String msg) {
		this.code = code;
		this.msg = msg;
	}

	public Integer getCode() {
		return code;
	}

	public void setCode(Integer code) {
		this.code = code;
	}

	public String getMsg() {
		return msg;
	}

	public void setMsg(String msg) {
		this.msg = msg;
	}

	// 按照状态码返回枚举对象
	public static UserStatus getEmpStatusByCode(Integer code) {
		switch (code) {
		case 200:
			return UserStatus.LOGIN;
		case 300:
			return UserStatus.LOGOFF;
		case 400:
			return UserStatus.FROZEN;
		default:
			return UserStatus.LOGOFF;
		}
	}
}
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

MyEnumTypeHandler.java（全路径：/mybatis-enum3/src/com/caochenlei/mybatis/plugin/MyEnumTypeHandler.java）

```java
package com.caochenlei.mybatis.plugin;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.TypeHandler;

import com.caochenlei.mybatis.pojo.UserStatus;

public class MyEnumTypeHandler implements TypeHandler<UserStatus> {
	@Override
	public void setParameter(PreparedStatement ps, int i, UserStatus parameter, JdbcType jdbcType) 
			throws SQLException {
		ps.setString(i, parameter.getCode().toString());
	}

	@Override
	public UserStatus getResult(ResultSet rs, String columnName) 
			throws SQLException {
		int code = rs.getInt(columnName);
		return UserStatus.getEmpStatusByCode(code);
	}

	@Override
	public UserStatus getResult(ResultSet rs, int columnIndex) 
			throws SQLException {
		int code = rs.getInt(columnIndex);
		return UserStatus.getEmpStatusByCode(code);
	}

	@Override
	public UserStatus getResult(CallableStatement cs, int columnIndex) 
			throws SQLException {
		int code = cs.getInt(columnIndex);
		return UserStatus.getEmpStatusByCode(code);
	}
}
12345678910111213141516171819202122232425262728293031323334353637383940
```

先运行`public void insertOneTest()`，控制台输出

![image-20200928192616950](https://img-blog.csdnimg.cn/img_convert/a5365d7c6ba742a9297139c429f375a3.png)

数据库记录

![image-20200928213538737](https://img-blog.csdnimg.cn/img_convert/26825a4811ff38ceb6777b00f43b16ce.png)

再运行`public void selectOneTest()`，控制台输出

![image-20200928191159278](https://img-blog.csdnimg.cn/img_convert/62fbff0b48211c5dc1827ff1ed1123f3.png)

最终结论，如果我们处理枚举的typeHandler使用自定义的`com.caochenlei.mybatis.plugin.MyEnumTypeHandler`，那么在存储的时候是直接存储的**自定义枚举的数值（因为是自定义的，当然也支持存储枚举名称，具体看自己怎么实现了）**。

虽然咱们演示的是枚举的处理，但是通过实现TypeHandler，我们还可以对其它类型进行自定义处理。

## 第十章 MyBatis3的注解开发

### 10.1、环境准备

我们直接将工程mybatis-crud拷贝一份，更名为mybatis-annotation，然后做以下变动

1. 删除EmployeeMapperDynamicSQLTest.java

2. 删除EmployeeMapperDynamicSQL.java

3. 删除EmployeeMapperDynamicSQL.xml

4. 删除DepartmentMapper.xml

5. 删除EmployeeMapper.xml

6. 将测试类DepartmentTest.java和EmployeeTest.java所有测试方法全部删除，只保留空文件

7. 将接口类DepartmentMapper.java和EmployeeMapper.java所有接口方法全部删除，只保留空文件

8. 删除ehcache.xml

9. 修改mybatis-config.xml为以下代码

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   	<!-- 加载数据库配置文件信息 -->
   	<properties resource="dbconfig.properties"></properties>
   
   	<!-- 配置框架的全局配置信息 -->
   	<settings>
   		<setting name="mapUnderscoreToCamelCase" value="true" />
   		<setting name="jdbcTypeForNull" value="NULL" />
   
   		<!-- 开启二级缓存 -->
   		<setting name="cacheEnabled" value="true" />
   
   		<!-- 开启延迟加载 -->
   		<setting name="lazyLoadingEnabled" value="true" />
   		<setting name="aggressiveLazyLoading" value="false" />
   	</settings>
   
   	<!-- 配置框架的多数据源信息 -->
   	<environments default="dev_mysql">
   		<!-- 配置mysql开发环境 -->
   		<environment id="dev_mysql">
   			<transactionManager type="JDBC" />
   			<dataSource type="POOLED">
   				<property name="driver" value="${mysql.driver}" />
   				<property name="url" value="${mysql.url}" />
   				<property name="username" value="${mysql.username}" />
   				<property name="password" value="${mysql.password}" />
   			</dataSource>
   		</environment>
   		<!-- 配置oracle开发环境 -->
   		<environment id="dev_oracle">
   			<transactionManager type="JDBC" />
   			<dataSource type="POOLED">
   				<property name="driver" value="${oracle.driver}" />
   				<property name="url" value="${oracle.url}" />
   				<property name="username" value="${oracle.username}" />
   				<property name="password" value="${oracle.password}" />
   			</dataSource>
   		</environment>
   	</environments>
   
   	<!-- 数据库厂商起别名 -->
   	<databaseIdProvider type="DB_VENDOR">
   		<property name="MySQL" value="mysql" />
   		<property name="Oracle" value="oracle" />
   	</databaseIdProvider>
   
   	<!-- 批量注册映射文件 -->
   	<mappers>
   		<package name="com.caochenlei.mybatis.mapper" />
   	</mappers>
   </configuration>
   1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556
   ```

10. 删除ehcache-2.10.5.jar、mybatis-ehcache-1.2.1.jar、slf4j-api-1.7.25.jar、slf4j-jdk14-1.7.25.jar四个JAR包

最终项目结构图如下，如果直接看我的这章，请在配套资料中找到对应工程

![image-20200929152448845](https://img-blog.csdnimg.cn/img_convert/2e142f57676efa3c98f58ef77979723b.png)

数据库数据添加

```sql
#清空表
TRUNCATE TABLE employee;
TRUNCATE TABLE department;

#添加数据向employee表
INSERT INTO employee(id,last_name,email,gender,dep_id) 
VALUES(NULL,"张三","123@qq.com","男",1);
INSERT INTO employee(id,last_name,email,gender,dep_id) 
VALUES(NULL,"李四","123@qq.com","男",2);
INSERT INTO employee(id,last_name,email,gender,dep_id) 
VALUES(NULL,"王五","123@qq.com","女",3);
INSERT INTO employee(id,last_name,email,gender,dep_id) 
VALUES(NULL,"小六","123@qq.com","男",1);
INSERT INTO employee(id,last_name,email,gender,dep_id) 
VALUES(NULL,"小七","123@qq.com","男",2);
INSERT INTO employee(id,last_name,email,gender,dep_id) 
VALUES(NULL,"老八","123@qq.com","女",3);
INSERT INTO employee(id,last_name,email,gender,dep_id) 
VALUES(NULL,"老九","123@qq.com","男",1);

#添加数据向department表
INSERT INTO department(dep_id,dep_name) VALUES(1,"运营部");
INSERT INTO department(dep_id,dep_name) VALUES(2,"开发部");
INSERT INTO department(dep_id,dep_name) VALUES(3,"产品部");
123456789101112131415161718192021222324
```

### 10.2、增删改查

#### 10.2.1、增加注解

**接口方法：**

EmployeeMapper.java

```java
/**
 * 插入员工
 * @param emp
 * @Insert：代表插入一条数据
 * 	value：插入语句，如果只有一个数据源，可以直接@Insert("insert ...")
 * 	databaseId：数据库别名，如果只有一个数据源，可以省略，@since 3.5.5
 */
@Insert(value = "insert into employee(id,last_name,email,gender,dep_id) values(null,#{lastName},#{email},#{gender},#{dep.depId})", databaseId = "mysql")
public void insertOne(Employee emp);
123456789
```

**测试代码：**

EmployeeTest.java

```java
@Test
public void insertOneTest() {
	try {
		String resource = "mybatis-config.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
		SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
		SqlSession openSession = sqlSessionFactory.openSession(true);

		EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
		Employee emp = new Employee(null, "白小白", "baixiaobai@qq.com", "女");
		Department dep = new Department(4, "采购部");
		emp.setDep(dep);
		employeeMapper.insertOne(emp);

		openSession.close();
	} catch (IOException e) {
		e.printStackTrace();
	}
}
1234567891011121314151617181920
```

**控制台截图：**

![image-20200929154632694](https://img-blog.csdnimg.cn/img_convert/236b56ffb3fe7f856ed3bd6e10dcc82e.png)

**数据库截图：**

![image-20200929154655846](https://img-blog.csdnimg.cn/img_convert/4617fdf78880729a752704890d0dd0a5.png)

#### 10.2.2、查找注解

**《查找一个》**

**接口方法：**

EmployeeMapper.java

```java
/**
 * 根据员工编号查找员工
 * @param id
 * @return
 * @Select：代表查找数据
 * 	value：查找语句，如果只有一个数据源，可以直接@Select("select ...")
 *	databaseId：数据库别名，如果只有一个数据源，可以省略，@since 3.5.5
 */
@Select(value = "select * from employee where id = #{id}", databaseId = "mysql")
public Employee selectEmpById(Integer id);
12345678910
```

**测试代码：**

EmployeeTest.java

```java
@Test
public void selectEmpByIdTest() {
	try {
		String resource = "mybatis-config.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
		SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
		SqlSession openSession = sqlSessionFactory.openSession(true);

		EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
		Employee employee = employeeMapper.selectEmpById(1);
		System.out.println(employee);

		openSession.close();
	} catch (IOException e) {
		e.printStackTrace();
	}
}
123456789101112131415161718
```

**控制台截图：**

![image-20200929160803297](https://img-blog.csdnimg.cn/img_convert/ab8207f6cb365c22dd480a81f863b55d.png)

**数据库截图：**

![image-20200929160827145](https://img-blog.csdnimg.cn/img_convert/19e7bc0d59fc4c6a91271206816aee7f.png)

------

**《查找全部》**

**接口方法：**

EmployeeMapper.java

```java
/**
 * 查询所有员工信息
 * @return
 * 注意：虽然当前有两个数据源，但是我没有设置databaseId，当接口方法唯一时，databaseId也可省略
 * 并且，有且只有一个属性的时候，那个属性是value的时候，所以 value= 也可以省略
 */
@Select("select * from employee")
public List<Employee> selectAll();
12345678
```

**测试代码：**

EmployeeTest.java

```java
@Test
public void selectAllTest() {
	try {
		String resource = "mybatis-config.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
		SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
		SqlSession openSession = sqlSessionFactory.openSession(true);

		EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
		List<Employee> emps = employeeMapper.selectAll();
		emps.forEach(emp -> System.out.println(emp));

		openSession.close();
	} catch (IOException e) {
		e.printStackTrace();
	}
}
123456789101112131415161718
```

**控制台截图：**

![image-20200929162125132](https://img-blog.csdnimg.cn/img_convert/7bd76bafacf23f85d846b2e813ef404c.png)

**数据库截图：**

![image-20200929162200771](https://img-blog.csdnimg.cn/img_convert/bf72db94dcfc7f4fefc1f4e290625d26.png)

#### 10.2.3、更新注解

**接口方法：**

EmployeeMapper.java

```java
/**
 * 更新员工信息
 * @param emp
 * @Update：更新员工信息
 * 	value：更新语句，如果只有一个数据源，可以直接@Update("update ...")
 * 	databaseId：数据库别名，如果只有一个数据源，可以省略，@since 3.5.5
 */
@Update(value = "update employee set last_name=#{lastName},email=#{email},gender=#{gender},dep_id=#{dep.depId} where id = #{id}", databaseId = "mysql")
public void updateEmp(Employee emp);
123456789
```

**测试代码：**

EmployeeTest.java

```java
@Test
public void updateEmpTest() {
	try {
		String resource = "mybatis-config.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
		SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
		SqlSession openSession = sqlSessionFactory.openSession(true);

		EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
		Employee employee = employeeMapper.selectEmpById(8);
		System.out.println(employee);
		employee.setLastName("罗小黑");
		employeeMapper.updateEmp(employee);

		openSession.close();
	} catch (IOException e) {
		e.printStackTrace();
	}
}
1234567891011121314151617181920
```

**控制台截图：**

![image-20200929164637377](https://img-blog.csdnimg.cn/img_convert/1908fae229d6d6adeadd104493e121de.png)

**数据库截图：**

![image-20200929164700863](https://img-blog.csdnimg.cn/img_convert/5d4488a57c75a266bb7754cafe747f7c.png)

#### 10.2.4、删除注解

**接口方法：**

EmployeeMapper.java

```java
/**
 * 删除员工信息
 * @param id
 * @Delete：删除员工信息
 * 	value：
 */
@Delete(value = "delete from employee where id=#{id}", databaseId = "mysql")
public void deleteEmpById(Integer id);
12345678
```

**测试代码：**

EmployeeTest.java

```java
@Test
public void deleteEmpByIdTest() {
	try {
		String resource = "mybatis-config.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
		SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
		SqlSession openSession = sqlSessionFactory.openSession(true);

		EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
		employeeMapper.deleteEmpById(8);

		openSession.close();
	} catch (IOException e) {
		e.printStackTrace();
	}
}
1234567891011121314151617
```

**控制台截图：**

![image-20200929165919749](https://img-blog.csdnimg.cn/img_convert/e2cb345aef93afb1350856e10981392d.png)

**数据库截图：**

![image-20200929165937857](https://img-blog.csdnimg.cn/img_convert/b66f74b96f20b9405733e141954a01d8.png)

### 10.3、一对一配置

**应用场景**：一个员工只能对应一个部门，但是一个部门可以有多个员工

**接口方法：**

DepartmentMapper.java

```java
/**
 * 根据部门编号查询部门
 * @param depId
 * @return
 */
@Select("select * from department where dep_id=#{depId}")
public Department findDepById(Integer depId);
1234567
```

EmployeeMapper.java

```java
/**
 * 查找员工的信息级联查找部门信息
 * @param id
 * @return
 * @Results：代替的是<resultMap>标签，注解中可以使用单个@Result注解，也可以使用@Result集合
 * 	@Result：代替的是<id>标签和<result>标签
 * 		id：是否为主键，是为true，否为false
 * 		column：数据库的列名
 * 		property：实体类的属性名
 * 		@One：一对一配置，代替的是<assocation>标签，是多表查询的关键，在注解中用来指定子查询返回单一对象
 * 			select：指定用来多表查询的sqlmapper，它是对应接口方法的全限定方法名
 * 			fetchType：懒加载配置，会覆盖全局的配置参数 lazyLoadingEnabled，一对一一般是立即加载
 * 				FetchType.LAZY：延迟加载
 * 				FetchType.EAGER：立即加载
 */
@Select("select * from employee where id=#{id}")
@Results({ 
		@Result(id = true, column = "id", property = "id"),
		@Result(column = "last_name", property = "lastName"), 
		@Result(column = "email", property = "email"),
		@Result(column = "gender", property = "gender"),
		@Result(column = "dep_id", property = "dep", one = @One(select = "com.caochenlei.mybatis.mapper.DepartmentMapper.findDepById", fetchType = FetchType.EAGER))
})
public Employee getEmpWithDep(Integer id);
123456789101112131415161718192021222324
```

**测试代码：**

```java
@Test
public void getEmpWithDepTest() {
	try {
		String resource = "mybatis-config.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
		SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
		SqlSession openSession = sqlSessionFactory.openSession(true);

		EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
		Employee employee = employeeMapper.getEmpWithDep(1);
		System.out.println(employee);
		System.out.println(employee.getDep());

		openSession.close();
	} catch (IOException e) {
		e.printStackTrace();
	}
}
12345678910111213141516171819
```

**控制台截图：**

![image-20200929190147682](https://img-blog.csdnimg.cn/img_convert/1ca7755c79c1fd892f21a016cf69f0b0.png)

**数据库截图：**

employee表

![image-20200929190946297](https://img-blog.csdnimg.cn/img_convert/abb762dc8d19874d731420ff0e93699e.png)

department表

![image-20200929190959157](https://img-blog.csdnimg.cn/img_convert/8419627fbe8798a32f9ab82aa32023c1.png)

### 10.4、一对多配置

**应用场景**：一个员工只能对应一个部门，但是一个部门可以有多个员工

**接口方法：**

EmployeeMapper.java

```java
@Select("select * from employee where dep_id=#{depId}")
public List<Employee> findEmpByDepId(Integer depId);
12
```

DepartmentMapper.java

```java
/**
 * 根据部门编号查询部门级联查询该部门下所有员工
 * @param depId
 * @return
 * @Results：代替的是<resultMap>标签，注解中可以使用单个@Result注解，也可以使用@Result集合
 * 	@Result：代替的是<id>标签和<result>标签
 * 		id：是否为主键，是为true，否为false
 * 		column：数据库的列名
 * 		property：实体类的属性名
 * 		@Many：一对多配置，代替的是<collection>标签，是多表查询的关键，在注解中用来指定子查询返回对象集合
 * 			select：指定用来多表查询的sqlmapper，它是对应接口方法的全限定方法名
 * 			fetchType：懒加载配置，会覆盖全局的配置参数 lazyLoadingEnabled，一对多一般是延迟加载
 * 				FetchType.LAZY：延迟加载
 * 				FetchType.EAGER：立即加载
 */
@Select("select * from department where dep_id=#{depId}")
@Results({ 
	@Result(id = true, column = "dep_id", property = "depId"),
	@Result(column = "dep_name", property = "depName"),
	@Result(column = "dep_id", property = "emps", many = @Many(select = "com.caochenlei.mybatis.mapper.EmployeeMapper.findEmpByDepId", fetchType = FetchType.LAZY)) 
})
public Department getDepWithEmps(Integer depId);
12345678910111213141516171819202122
```

**测试代码：**

DepartmentTest.java

```java
@Test
public void getDepWithEmpsTest() {
	try {
		String resource = "mybatis-config.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
		SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
		SqlSession openSession = sqlSessionFactory.openSession(true);

		DepartmentMapper departmentMapper = openSession.getMapper(DepartmentMapper.class);
		Department department = departmentMapper.getDepWithEmps(1);
		System.out.println(department);
		System.out.println(department.getEmps());

		openSession.close();
	} catch (IOException e) {
		e.printStackTrace();
	}
}
12345678910111213141516171819
```

**控制台截图：**

![image-20200929212822398](https://img-blog.csdnimg.cn/img_convert/e9b54c18826933cb30183654de4dc9fa.png)

**数据库截图：**

employee表

![image-20200929212837176](https://img-blog.csdnimg.cn/img_convert/d0dd14406f3aac38b9b80c786c47ddbb.png)

department表

![image-20200929212900190](https://img-blog.csdnimg.cn/img_convert/88a19fa0a9870338f07bf75d12f3545d.png)

### 10.5、本章小结

**常见注解：**

```java
@Insert:实现新增
@Update:实现更新
@Delete:实现删除
@Select:实现查询
@Result:实现结果集封装
@Results:可以与@Result一起使用，封装多个结果集
@ResultMap:实现引用@Results定义的封装
@One:实现一对一结果集封装
@Many:实现一对多结果集封装
123456789
```

**注意事项：**

```java
注意:
	1.mybatis-config.xml该怎么写还怎么写，注解开发，只是用来替代mapper.xml映射文件的，不要写mapper.xml映射文件
	3.不同的sql语句，要对应不同的@Select,@Insert,@Update,@Delete注解
	
步骤:
	1.不要写mapper.xml映射文件
	2.在接口方法上直接写@Select,@Insert,@Update,@Delete配置sql语句就可以了
	
案例:
	@Select("select * from user")
	List<User> findAll();
```