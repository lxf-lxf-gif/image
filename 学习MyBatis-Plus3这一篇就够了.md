# 学习MyBatis-Plus3这一篇就够了

[TOC]





------

**配套资料，免费下载**
链接：https://pan.baidu.com/s/1yQS9hGP3r_zZbkuo-jlijA
提取码：4v88
复制这段内容后打开百度网盘手机App，操作更方便哦

## 第一章 MyBatis-Plus3概述

### 1.1、简介

MyBatis-Plus（简称 MP）是一个 MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

我们的愿景是成为 MyBatis 最好的搭档，就像魂斗罗中的1P、2P，基友搭配，效率翻倍。

### 1.2、特性

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

### 1.3、框架结构

![framework](https://img-blog.csdnimg.cn/img_convert/65f466de040a62eab8cda68ecab3578a.png)

### 1.4、项目地址

**官网地址**：[点击打开](https://baomidou.com/)

**源码地址**：[点击打开](https://github.com/baomidou/mybatis-plus)

**文档地址**：[点击打开](https://baomidou.com/guide/)

**配置地址**：[点击打开](https://baomidou.com/config/)

### 1.5、版本介绍

全新的 `MyBatis-Plus` 3.0 版本基于 JDK8，提供了 `lambda` 形式的调用，所以安装集成 MP3.0 要求如下：

- JDK 8+
- Maven or Gradle

### 1.6、快速安装

- **Spring Boot**

  - Maven：

    ```xml
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.4.0</version>
    </dependency>
    ```
    
- Gradle：
  
  ```xml
    compile group: 'com.baomidou', name: 'mybatis-plus-boot-starter', version: '3.4.0'
    ```
  
- **Spring MVC**

  - Maven：

    ```xml
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus</artifactId>
        <version>3.4.0</version>
    </dependency>
    ```
    
- Gradle：
  
  ```xml
    compile group: 'com.baomidou', name: 'mybatis-plus', version: '3.4.0'
    ```

> 警告：引入 `MyBatis-Plus` 之后请不要再次引入 `MyBatis` 以及 `MyBatis-Spring`，以避免因版本差异导致的问题。

### 1.7、开发环境

- Jdk：jdk1.8.0_261
- Idea：IntelliJ IDEA 2020.1.2 x64
- Maven：apache-maven-3.3.9
- MySQL：mysql-5.5.61-win64

## 第二章 MyBatis-Plus3增删改查

### 2.1、项目搭建

![image-20201002120850548](https://img-blog.csdnimg.cn/img_convert/8be2ec054e4a7b740f7adb4df6906189.png)

![image-20201002121010118](https://img-blog.csdnimg.cn/img_convert/6c3b9654874782e1102fff95eef1bcd7.png)

![image-20201002121134665](https://img-blog.csdnimg.cn/img_convert/7e7a8d317a71928953d45116e5dd061e.png)

![image-20201002121200279](https://img-blog.csdnimg.cn/img_convert/af2a6e8b43a2be67485d3f4887c66917.png)

新建完成以后，打开pom.xml后添加以下依赖：

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.0</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.49</version>
</dependency>
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
    <scope>provided</scope>
</dependency>
```

在src/mian/java目录中，创建以下包文件

- com.caochenlei.mpdemo.pojo
- com.caochenlei.mpdemo.mapper

### 2.2、项目配置（1）

MyBatis-Plus 的配置异常的简单，我们仅需要一些简单的配置即可使用 MyBatis-Plus 的强大功能！

- Spring Boot 工程：

  - 配置 MapperScan 注解

    ```java
    @SpringBootApplication
    @MapperScan("com.caochenlei.mpdemo.mapper")
    public class MpDemoApplication {
    
        public static void main(String[] args) {
            SpringApplication.run(MpDemoApplication.class, args);
        }
    
    }
    ```
  
- Spring MVC 工程：

  - 配置 MapperScan 对象

    ```xml
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.caochenlei.mpdemo.mapper"/>
    </bean>
    ```
    
- 调整 SqlSessionFactory 为 MyBatis-Plus 的 SqlSessionFactory
  
  ```xml
    <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    ```

### 2.3、项目配置（2）

application.properties

```yaml
#mysql
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mp?useUnicode=true&characterEncoding=utf8
spring.datasource.username=root
spring.datasource.password=123456

#mybatis-plus
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

### 2.4、数据导入

```sql
## 创建库
CREATE DATABASE mp;
## 使用库
USE mp;
## 创建表
CREATE TABLE tbl_employee(
   id INT(11) PRIMARY KEY AUTO_INCREMENT,
   last_name VARCHAR(50),
   email VARCHAR(50),
   gender CHAR(1),
   age INT
);
## 导入数据
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Tom','tom@qq.com',1,22);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Jerry','jerry@qq.com',0,25);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Black','black@qq.com',1,30);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('White','white@qq.com',0,35);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Tiger','tiger@qq.com',1,28);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Bobby','bobby@qq.com',0,16);
## 查询数据
SELECT * FROM tbl_employee;
```

### 2.5、创建实体

com.caochenlei.mpdemo.pojo.Employee

```java
@TableName("tbl_employee")
public class Employee {
    @TableId(value = "id", type = IdType.AUTO)
    private Integer id;
    @TableField(value = "last_name")
    private String lastName;
    @TableField(value = "email")
    private String email;
    @TableField(value = "gender")
    private Integer gender;
    @TableField(value = "age")
    private Integer age;

    public Employee() {}

    public Employee(Integer id, String lastName, String email, Integer gender, Integer age) {
        this.id = id;
        this.lastName = lastName;
        this.email = email;
        this.gender = gender;
        this.age = age;
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

    public Integer getGender() {
        return gender;
    }

    public void setGender(Integer gender) {
        this.gender = gender;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "id=" + id +
                ", lastName='" + lastName + '\'' +
                ", email='" + email + '\'' +
                ", gender=" + gender +
                ", age=" + age +
                '}';
    }
}
```

### 2.6、创建接口

com.caochenlei.mpdemo.mapper.EmployeeMapper

```java
@Repository
public interface EmployeeMapper extends BaseMapper<Employee> {

}
```

### 2.7、测试准备

com.caochenlei.mpdemo.MpDemoApplicationTests

```java
@SpringBootTest
class MpDemoApplicationTests {
    @Autowired
    private EmployeeMapper employeeMapper;

    @Test
    void contextLoads() {
        List<Employee> employees = employeeMapper.selectList(null);
        employees.forEach(System.out::println);
    }
}
```

### 2.8、增删改查

#### 2.8.1、insert

**需求描述**：插入一个员工，员工姓名为“张三”、邮箱为"zhangsan@qq.com"、男性、25岁

```java
@Test
void testInsert() {
    int result = employeeMapper.insert(new Employee(null, "zhangsan", "zhangsan@qq.com", 0, 25));
    System.out.println("result:" + result);
}
```

#### 2.8.2、updateById

**需求信息**：将id为1的员工的姓名更改为"Jennie"

```java
@Test
void testUpdateById() {
    // 先查询
    Employee employee = employeeMapper.selectById(1);
    employee.setLastName("Jennie");
    // 再修改
    int result = employeeMapper.updateById(employee);
    System.out.println(result);
}
```

#### 2.8.3、selectById

**需求描述**：查询id为1的员工信息

```java
@Test
void testSelectById() {
    Employee employee = employeeMapper.selectById(1);
    System.out.println(employee);
}
```

#### 2.8.4、selectByMap

**需求描述**：查询性别为男性（0）且年龄在25岁的员工信息

```java
@Test
void testSelectByMap() {
    Map<String, Object> map = new HashMap<>();
    map.put("gender",0);
    map.put("age",25);
    List<Employee> employees = employeeMapper.selectByMap(map);
    employees.forEach(System.out::println);
}
```

#### 2.8.5、selectBatchIds

**需求描述**：查询id分别为1、2、3的员工的信息

```java
@Test
void testSelectBatchIds() {
    List<Employee> employees = employeeMapper.selectBatchIds(Arrays.asList(1, 2, 3));
    employees.forEach(System.out::println);
}
```

#### 2.8.6、deleteById

**需求信息**：删除id为1的员工信息

```java
@Test
void testDeleteById() {
    int result = employeeMapper.deleteById(1);
    System.out.println(result);
}
```

#### 2.8.7、deleteByMap

**需求描述**：删除性别为男性（0）且年龄在25岁的员工信息

```java
@Test
void testDeleteByMap() {
    Map<String, Object> map = new HashMap<>();
    map.put("gender", 0);
    map.put("age", 25);
    int result = employeeMapper.deleteByMap(map);
    System.out.println(result);
}
```

#### 2.8.8、deleteBatchIds

**需求描述**：删除id分别为4、5、6的员工的信息

```java
@Test
void testDeleteBatchIds() {
    int result = employeeMapper.deleteBatchIds(Arrays.asList(4, 5, 6));
    System.out.println(result);
}
```

## 第三章 MyBatis-Plus3注解介绍

### 3.1、@TableName

描述：表名注解

| 属性             | 类型    | 必须指定 | 默认值 | 描述                                                         |
| ---------------- | ------- | -------- | ------ | ------------------------------------------------------------ |
| value            | String  | 否       | “”     | 表名                                                         |
| schema           | String  | 否       | “”     | schema                                                       |
| keepGlobalPrefix | boolean | 否       | false  | 是否保持使用全局的 tablePrefix 的值(如果设置了全局 tablePrefix 且自行设置了 value 的值) |
| resultMap        | String  | 否       | “”     | xml 中 resultMap 的 id                                       |
| autoResultMap    | boolean | 否       | false  | 是否自动构建 resultMap 并使用(如果设置 resultMap 则不会进行 resultMap 的自动构建并注入) |

### 3.2、@TableId

描述：主键注解

| 属性  | 类型   | 必须指定 | 默认值      | 描述       |
| ----- | ------ | -------- | ----------- | ---------- |
| value | String | 否       | “”          | 主键字段名 |
| type  | Enum   | 否       | IdType.NONE | 主键类型   |

**IdType**

| 值          | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| AUTO        | 数据库ID自增                                                 |
| NONE        | 无状态,该类型为未设置主键类型(注解里等于跟随全局,全局里约等于 INPUT) |
| INPUT       | insert前自行set主键值                                        |
| ASSIGN_ID   | 分配ID(主键类型为Number(Long和Integer)或String)(since 3.3.0),使用接口`IdentifierGenerator`的方法`nextId`(默认实现类为`DefaultIdentifierGenerator`雪花算法) |
| ASSIGN_UUID | 分配UUID,主键类型为String(since 3.3.0),使用接口`IdentifierGenerator`的方法`nextUUID`(默认default方法) |

### 3.3、@TableField

描述：字段注解(非主键)

| 属性             | 类型                         | 必须指定 | 默认值                   | 描述                                                         |
| ---------------- | ---------------------------- | -------- | ------------------------ | ------------------------------------------------------------ |
| value            | String                       | 否       | “”                       | 数据库字段名                                                 |
| el               | String                       | 否       | “”                       | 映射为原生 `#{ ... }` 逻辑,相当于写在 xml 里的 `#{ ... }` 部分 |
| exist            | boolean                      | 否       | true                     | 是否为数据库表字段                                           |
| condition        | String                       | 否       | “”                       | 字段 `where` 实体查询比较条件,有值设置则按设置的值为准,没有则为默认全局的 `%s=#{%s}` |
| update           | String                       | 否       | “”                       | 字段 `update set` 部分注入, 例如：update="%s+1"：表示更新时会set version=version+1(该属性优先级高于 `el` 属性) |
| insertStrategy   | Enum                         | N        | DEFAULT                  | 举例：NOT_NULL: `insert into table_a(<if test="columnProperty != null">column</if>) values (<if test="columnProperty != null">#{columnProperty}</if>)` |
| updateStrategy   | Enum                         | N        | DEFAULT                  | 举例：IGNORED: `update table_a set column=#{columnProperty}` |
| whereStrategy    | Enum                         | N        | DEFAULT                  | 举例：NOT_EMPTY: `where <if test="columnProperty != null and columnProperty!=''">column=#{columnProperty}</if>` |
| fill             | Enum                         | 否       | FieldFill.DEFAULT        | 字段自动填充策略                                             |
| select           | boolean                      | 否       | true                     | 是否进行 select 查询                                         |
| keepGlobalFormat | boolean                      | 否       | false                    | 是否保持使用全局的 format 进行处理                           |
| jdbcType         | JdbcType                     | 否       | JdbcType.UNDEFINED       | JDBC类型 (该默认值不代表会按照该值生效)                      |
| typeHandler      | Class<? extends TypeHandler> | 否       | UnknownTypeHandler.class | 类型处理器 (该默认值不代表会按照该值生效)                    |
| numericScale     | String                       | 否       | “”                       | 指定小数点后保留的                                           |

**FieldStrategy**

| 值        | 描述                                                      |
| --------- | --------------------------------------------------------- |
| IGNORED   | 忽略判断                                                  |
| NOT_NULL  | 非NULL判断                                                |
| NOT_EMPTY | 非空判断(只对字符串类型字段,其他类型字段依然为非NULL判断) |
| DEFAULT   | 追随全局配置                                              |

**FieldFill**

| 值            | 描述                 |
| ------------- | -------------------- |
| DEFAULT       | 默认不处理           |
| INSERT        | 插入时填充字段       |
| UPDATE        | 更新时填充字段       |
| INSERT_UPDATE | 插入和更新时填充字段 |

### 3.4、@Version

描述：乐观锁注解、标记 `@Verison` 在字段上

### 3.5、@EnumValue

描述：通枚举类注解（注解在枚举字段上）

### 3.6、@TableLogic

描述：表字段逻辑处理注解（逻辑删除）

| 属性   | 类型   | 必须指定 | 默认值 | 描述         |
| ------ | ------ | -------- | ------ | ------------ |
| value  | String | 否       | “”     | 逻辑未删除值 |
| delval | String | 否       | “”     | 逻辑删除值   |

### 3.7、@SqlParser

描述：租户注解，支持method上以及mapper接口上

| 属性   | 类型    | 必须指定 | 默认值 | 描述                                                         |
| ------ | ------- | -------- | ------ | ------------------------------------------------------------ |
| filter | boolean | 否       | false  | true: 表示过滤SQL解析，即不会进入ISqlParser解析链，否则会进解析链并追加例如tenant_id等条件 |

### 3.8、@KeySequence

描述：序列主键策略 `oracle`

属性：value、resultMap

| 属性  | 类型   | 必须指定 | 默认值     | 描述                                                         |
| ----- | ------ | -------- | ---------- | ------------------------------------------------------------ |
| value | String | 否       | “”         | 序列名                                                       |
| clazz | Class  | 否       | Long.class | id的类型, 可以指定String.class，这样返回的Sequence值是字符串"1" |

## 第四章 MyBatis-Plus3条件构造器

### 4.1、数据导入

```sql
## 使用库
USE mp;
## 清空表
TRUNCATE TABLE tbl_employee;	
## 导入数据
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Allan0','123@qq.com',0,21);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Allan1','123@qq.com',0,22);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Allan2','123@qq.com',0,23);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Allan3','123@qq.com',0,24);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Allan4','123@qq.com',0,25);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Allan5','123@qq.com',0,26);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Allan6','123@qq.com',0,27);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Allan7','123@qq.com',0,28);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Allan8','123@qq.com',0,29);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Allan9','123@qq.com',0,30);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Baby0','123@qq.com',1,21);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Baby1','123@qq.com',0,22);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Baby2','123@qq.com',1,23);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Baby3','123@qq.com',0,24);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Baby4','123@qq.com',1,25);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Baby5','123@qq.com',0,26);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Baby6','123@qq.com',1,27);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Baby7','123@qq.com',0,28);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Baby8','123@qq.com',1,29);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Baby9','123@qq.com',0,30);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Tom0','123@qq.com',1,21);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Tom1','123@qq.com',0,22);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Tom2','123@qq.com',1,23);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Tom3','123@qq.com',0,24);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Tom4','123@qq.com',1,25);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Tom5','123@qq.com',0,26);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Tom6','123@qq.com',1,27);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Tom7','123@qq.com',0,28);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Tom8','123@qq.com',1,29);
INSERT INTO tbl_employee(last_name,email,gender,age) VALUES('Tom9','123@qq.com',0,30);
## 查询数据
SELECT * FROM tbl_employee;
```

### 4.2、构造器简介

MyBatis-Plus 通过 EntityWrapper（简称 EW，MP 封装的一个查询条件构造器）或者 Condition（与 EW 类似） 来让用户自由的构建查询条件，简单便捷，没有额外的负担， 能够有效提高开发效率，它主要用于处理 sql 拼接，排序，实体参数查询等。

> 注意：使用的是数据库字段，不是 Java 属性！

> 警告：MyBatis-Plus不支持以及不赞成在 RPC 调用中把 Wrapper 进行传输，Wrapper 很重，传输 Wrapper 可以类比为你的 controller 用 map 接收值(开发一时爽，维护火葬场)，正确的 RPC 调用姿势是写一个 DTO 进行传输，被调用方再根据 DTO 执行相应的操作。

### 4.3、构造器使用（1）

#### 4.3.1、带条件的查询

**需求描述**：查询所有姓名的包含B、且姓名为女（1）、且年龄大于24岁的员工信息

```java
@Test
void testSelectList1() {
    QueryWrapper<Employee> queryWrapper = new QueryWrapper<>();
    queryWrapper
            .like("last_name","B")
            .eq("gender",1)
            .gt("age",24);
    List<Employee> employees = employeeMapper.selectList(queryWrapper);
    employees.forEach(System.out::println);
}
```

**需求描述**：查询所有员工信息

```java
@Test
void testSelectList2() {
    List<Employee> employees = employeeMapper.selectList(null);
    employees.forEach(System.out::println);
}
```

**需求描述**：查询所有女生的数量（1）

```java
@Test
void testSelectList3() {
    QueryWrapper<Employee> queryWrapper = new QueryWrapper<>();
    queryWrapper.eq("gender", 1);
    Integer count = employeeMapper.selectCount(queryWrapper);
    System.out.println(count);
}
```

#### 4.3.2、带条件的修改

**需求信息**：将年龄大于25岁的女生（1）的性别修改为男生（0）

```java
@Test
void testUpdate() {
    UpdateWrapper<Employee> updateWrapper = new UpdateWrapper<>();
    updateWrapper
        .eq("gender", 1)
        .gt("age", 25)
        ;
    Employee employee = new Employee();
    employee.setGender(0);
    employeeMapper.update(employee, updateWrapper);
}
```

#### 4.3.3、带条件的删除

**需求信息**：将姓名带有“Tom”的员工信息删除

```java
@Test
void testDelete() {
    QueryWrapper<Employee> queryWrapper = new QueryWrapper<>();
    queryWrapper.like("last_name", "Tom");
    int result = employeeMapper.delete(queryWrapper);
    System.out.println(result);
}
```

### 4.4、构造器使用（2）

**参数说明**：

- 以下出现的第一个入参`boolean condition`表示该条件**是否**加入最后生成的sql中
- 以下代码块内的多个方法均为从上往下补全个别`boolean`类型的入参，默认为`true`
- 以下出现的泛型`Param`均为`Wrapper`的子类实例(均具有`AbstractWrapper`的所有方法)
- 以下方法在入参中出现的`R`为泛型，在普通wrapper中是`String`，在LambdaWrapper中是**函数**(例：`Entity::getId`,`Entity`为实体类，`getId`为字段`id`的**getMethod**)
- 以下方法入参中的`R column`均表示数据库字段，当`R`具体类型为`String`时则为数据库字段名(**字段名是数据库关键字的自己用转义符包裹**)！而不是实体类数据字段名，另当`R`具体类型为`SFunction`时项目runtime不支持eclipse自家的编译器
- 以下举例均为使用普通wrapper，入参为`Map`和`List`的均以`json`形式表现
- 使用中如果入参的`Map`或者`List`为**空**，则不会加入最后生成的sql中

**AbstractWrapper**：

说明：AbstractWrapper 是 QueryWrapper(LambdaQueryWrapper) 和 UpdateWrapper(LambdaUpdateWrapper) 的父类用于生成 sql 的 where 条件，entity 属性也用于生成 sql 的 where 条件，注意 entity 生成的 where 条件与使用各个 api 生成的 where 条件**没有任何关联行为**

#### 4.4.1、allEq

```java
allEq(Map<R, V> params)
allEq(Map<R, V> params, boolean null2IsNull)
allEq(boolean condition, Map<R, V> params, boolean null2IsNull)
```

- 全部 `eq` (或个别 `isNull`）

个别参数说明：

`params`：`key`为数据库字段名，`value`为字段值
`null2IsNull`：为`true`则在`map`的`value`为`null`时调用 `isNull`方法，为`false`时则忽略`value`为`null`的

- 例1: `allEq({id:1,name:"老王",age:null})`—>`id = 1 and name = '老王' and age is null`
- 例2: `allEq({id:1,name:"老王",age:null}, false)`—>`id = 1 and name = '老王'`

```java
allEq(BiPredicate<R, V> filter, Map<R, V> params)
allEq(BiPredicate<R, V> filter, Map<R, V> params, boolean null2IsNull)
allEq(boolean condition, BiPredicate<R, V> filter, Map<R, V> params, boolean null2IsNull) 
```

个别参数说明：

`filter` : 过滤函数，是否允许字段传入比对条件中
`params` 与 `null2IsNull` : 同上

- 例1: `allEq((k,v) -> k.indexOf("a") >= 0, {id:1,name:"老王",age:null})`—>`name = '老王' and age is null`
- 例2: `allEq((k,v) -> k.indexOf("a") >= 0, {id:1,name:"老王",age:null}, false)`—>`name = '老王'`

#### 4.4.2、eq

```java
eq(R column, Object val)
eq(boolean condition, R column, Object val)
```

- 等于 =
- 例: `eq("name", "老王")`—>`name = '老王'`

#### 4.4.3、ne

```java
ne(R column, Object val)
ne(boolean condition, R column, Object val)
```

- 不等于 <>
- 例: `ne("name", "老王")`—>`name <> '老王'`

#### 4.4.4、gt

```java
gt(R column, Object val)
gt(boolean condition, R column, Object val)
```

- 大于 >
- 例: `gt("age", 18)`—>`age > 18`

#### 4.4.5、ge

```java
ge(R column, Object val)
ge(boolean condition, R column, Object val)
```

- 大于等于 >=
- 例: `ge("age", 18)`—>`age >= 18`

#### 4.4.6、lt

```java
lt(R column, Object val)
lt(boolean condition, R column, Object val)
```

- 小于 <
- 例: `lt("age", 18)`—>`age < 18`

#### 4.4.7、le

```java
le(R column, Object val)
le(boolean condition, R column, Object val)
```

- 小于等于 <=
- 例: `le("age", 18)`—>`age <= 18`

#### 4.4.8、between

```java
between(R column, Object val1, Object val2)
between(boolean condition, R column, Object val1, Object val2)
```

- BETWEEN 值1 AND 值2
- 例: `between("age", 18, 30)`—>`age between 18 and 30`

#### 4.4.9、notBetween

```java
notBetween(R column, Object val1, Object val2)
notBetween(boolean condition, R column, Object val1, Object val2)
```

- NOT BETWEEN 值1 AND 值2
- 例: `notBetween("age", 18, 30)`—>`age not between 18 and 30`

#### 4.4.10、like

```java
like(R column, Object val)
like(boolean condition, R column, Object val)
```

- LIKE ‘%值%’
- 例: `like("name", "王")`—>`name like '%王%'`

#### 4.4.11、notLike

```java
notLike(R column, Object val)
notLike(boolean condition, R column, Object val)
```

- NOT LIKE ‘%值%’
- 例: `notLike("name", "王")`—>`name not like '%王%'`

#### 4.4.12、likeLeft

```java
likeLeft(R column, Object val)
likeLeft(boolean condition, R column, Object val)
```

- LIKE ‘%值’
- 例: `likeLeft("name", "王")`—>`name like '%王'`

#### 4.4.13、likeRight

```java
likeRight(R column, Object val)
likeRight(boolean condition, R column, Object val)
```

- LIKE ‘值%’
- 例: `likeRight("name", "王")`—>`name like '王%'`

#### 4.4.14、isNull

```java
isNull(R column)
isNull(boolean condition, R column)
```

- 字段 IS NULL
- 例: `isNull("name")`—>`name is null`

#### 4.4.15、isNotNull

```java
isNotNull(R column)
isNotNull(boolean condition, R column)
```

- 字段 IS NOT NULL
- 例: `isNotNull("name")`—>`name is not null`

#### 4.4.16、in

```java
in(R column, Collection<?> value)
in(boolean condition, R column, Collection<?> value)
```

- 字段 IN (value.get(0), value.get(1), …)
- 例: `in("age",{1,2,3})`—>`age in (1,2,3)`

```java
in(R column, Object... values)
in(boolean condition, R column, Object... values)
```

- 字段 IN (v0, v1, …)
- 例: `in("age", 1, 2, 3)`—>`age in (1,2,3)`

#### 4.4.17、notIn

```java
notIn(R column, Collection<?> value)
notIn(boolean condition, R column, Collection<?> value)
```

- 字段 NOT IN (value.get(0), value.get(1), …)
- 例: `notIn("age",{1,2,3})`—>`age not in (1,2,3)`

```java
notIn(R column, Object... values)
notIn(boolean condition, R column, Object... values)
```

- 字段 NOT IN (v0, v1, …)
- 例: `notIn("age", 1, 2, 3)`—>`age not in (1,2,3)`

#### 4.4.18、inSql

```java
inSql(R column, String inValue)
inSql(boolean condition, R column, String inValue)
```

- 字段 IN ( sql语句 )
- 例: `inSql("age", "1,2,3,4,5,6")`—>`age in (1,2,3,4,5,6)`
- 例: `inSql("id", "select id from table where id < 3")`—>`id in (select id from table where id < 3)`

#### 4.4.19、notInSql

```java
notInSql(R column, String inValue)
notInSql(boolean condition, R column, String inValue)
```

- 字段 NOT IN ( sql语句 )
- 例: `notInSql("age", "1,2,3,4,5,6")`—>`age not in (1,2,3,4,5,6)`
- 例: `notInSql("id", "select id from table where id < 3")`—>`id not in (select id from table where id < 3)`

#### 4.4.20、groupBy

```java
groupBy(R... columns)
groupBy(boolean condition, R... columns)
```

- 分组：GROUP BY 字段, …
- 例: `groupBy("id", "name")`—>`group by id,name`

#### 4.4.21、orderByAsc

```java
orderByAsc(R... columns)
orderByAsc(boolean condition, R... columns)
```

- 排序：ORDER BY 字段, … ASC
- 例: `orderByAsc("id", "name")`—>`order by id ASC,name ASC`

#### 4.4.22、orderByDesc

```java
orderByDesc(R... columns)
orderByDesc(boolean condition, R... columns)
```

- 排序：ORDER BY 字段, … DESC
- 例: `orderByDesc("id", "name")`—>`order by id DESC,name DESC`

#### 4.4.23、orderBy

```java
orderBy(boolean condition, boolean isAsc, R... columns)
```

- 排序：ORDER BY 字段, …
- 例: `orderBy(true, true, "id", "name")`—>`order by id ASC,name ASC`

#### 4.4.24、having

```java
having(String sqlHaving, Object... params)
having(boolean condition, String sqlHaving, Object... params)
```

- HAVING ( sql语句 )
- 例: `having("sum(age) > 10")`—>`having sum(age) > 10`
- 例: `having("sum(age) > {0}", 11)`—>`having sum(age) > 11`

#### 4.4.25、func

```java
func(Consumer<Children> consumer)
func(boolean condition, Consumer<Children> consumer)
```

- func 方法(主要方便在出现if…else下调用不同方法能不断链)
- 例: `func(i -> if(true) {i.eq("id", 1)} else {i.ne("id", 1)})`

#### 4.4.26、or

```java
or()
or(boolean condition)
```

- 拼接 OR

注意事项:

主动调用`or`表示紧接着下一个**方法**不是用`and`连接!(不调用`or`则默认为使用`and`连接)

- 例: `eq("id",1).or().eq("name","老王")`—>`id = 1 or name = '老王'`

```java
or(Consumer<Param> consumer)
or(boolean condition, Consumer<Param> consumer)
```

- OR 嵌套
- 例: `or(i -> i.eq("name", "李白").ne("status", "活着"))`—>`or (name = '李白' and status <> '活着')`

#### 4.4.27、and

```java
and(Consumer<Param> consumer)
and(boolean condition, Consumer<Param> consumer)
```

- AND 嵌套
- 例: `and(i -> i.eq("name", "李白").ne("status", "活着"))`—>`and (name = '李白' and status <> '活着')`

#### 4.4.28、nested

```java
nested(Consumer<Param> consumer)
nested(boolean condition, Consumer<Param> consumer)
```

- 正常嵌套 不带 AND 或者 OR
- 例: `nested(i -> i.eq("name", "李白").ne("status", "活着"))`—>`(name = '李白' and status <> '活着')`

#### 4.4.29、apply

```java
apply(String applySql, Object... params)
apply(boolean condition, String applySql, Object... params)
```

- 拼接 sql

注意事项:

该方法可用于数据库**函数**动态入参的`params`对应前面`applySql`内部的`{index}`部分，这样是不会有sql注入风险的，反之会有!

- 例: `apply("id = 1")`—>`id = 1`
- 例: `apply("date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")`—>`date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")`
- 例: `apply("date_format(dateColumn,'%Y-%m-%d') = {0}", "2008-08-08")`—>`date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")`

#### 4.4.30、last

```java
last(String lastSql)
last(boolean condition, String lastSql)
```

- 无视优化规则直接拼接到 sql 的最后

注意事项:

只能调用一次，多次调用以最后一次为准，有sql注入的风险，请谨慎使用

- 例: `last("limit 1")`

#### 4.4.31、exists

```java
exists(String existsSql)
exists(boolean condition, String existsSql)
```

- 拼接 EXISTS ( sql语句 )
- 例: `exists("select id from table where age = 1")`—>`exists (select id from table where age = 1)`

#### 4.4.32、notExists

```java
notExists(String notExistsSql)
notExists(boolean condition, String notExistsSql)
```

- 拼接 NOT EXISTS ( sql语句 )
- 例: `notExists("select id from table where age = 1")`—>`not exists (select id from table where age = 1)`

## 第五章 MyBatis-Plus3代码生成器

### 5.1、数据导入

```sql
## 删除表
DROP TABLE IF EXISTS `tbl_user`;
## 创建表
CREATE TABLE `tbl_user` (
  `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `name` VARCHAR(30) DEFAULT NULL COMMENT '姓名',
  `age` INT(11) DEFAULT NULL COMMENT '年龄',
  `email` VARCHAR(30) DEFAULT NULL COMMENT '邮箱',
  PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8 COMMENT='用户';
## 导入数据
INSERT  INTO `tbl_user`(`id`,`name`,`age`,`email`) VALUES (1,'Jone',18,'test1@baomidou.com');
INSERT  INTO `tbl_user`(`id`,`name`,`age`,`email`) VALUES (2,'Jack',20,'test2@baomidou.com');
INSERT  INTO `tbl_user`(`id`,`name`,`age`,`email`) VALUES (3,'Tom',28,'test3@baomidou.com');
INSERT  INTO `tbl_user`(`id`,`name`,`age`,`email`) VALUES (4,'Sandy',21,'test4@baomidou.com');
INSERT  INTO `tbl_user`(`id`,`name`,`age`,`email`) VALUES (5,'Billie',24,'test5@baomidou.com');
```

### 5.2、代码生成器简介

AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

### 5.3、代码生成器使用

#### 5.3.1、添加依赖

pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.4.0</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.49</version>
    </dependency>
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-generator</artifactId>
        <version>3.4.0</version>
    </dependency>
    <dependency>
        <groupId>org.apache.velocity</groupId>
        <artifactId>velocity-engine-core</artifactId>
        <version>2.2</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.12</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
<build>
    <!-- 插件管理 -->
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-resources-plugin</artifactId>
            <version>2.5</version>
        </plugin>
    </plugins>
    <!-- 资源管理 -->
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
                <include>**/*.conf</include>
            </includes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
                <include>**/*.conf</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```

#### 5.3.2、添加配置

application.properties

```properties
#server
server.port=8080

#mysql
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mp?useUnicode=true&characterEncoding=utf8
spring.datasource.username=root
spring.datasource.password=123456

#mybatis-plus
mybatis-plus.mapper-locations=classpath*:**/mapper/xml/*.xml
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

#### 5.3.3、启动配置

MpDemoApplication.java

```java
@SpringBootApplication
@MapperScan("com.caochenlei.mpdemo.mapper")
public class MpDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(MpDemoApplication.class, args);
    }

}
```

#### 5.3.4、代码生成

CodeGenerator.java

```java
public class CodeGenerator {

    public static String scanner(String tip) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder help = new StringBuilder();
        help.append("请输入" + tip + "：");
        System.out.println(help.toString());
        if (scanner.hasNext()) {
            String ipt = scanner.next();
            if (StringUtils.isNotBlank(ipt)) {
                return ipt;
            }
        }
        throw new MybatisPlusException("请输入正确的" + tip + "！");
    }

    public static void main(String[] args) {
        // 代码生成器
        AutoGenerator mpg = new AutoGenerator();

        // 全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir(projectPath + "/src/main/java");//设置代码生成路径
        gc.setFileOverride(true);//是否覆盖以前文件
        gc.setOpen(false);//是否打开生成目录
        gc.setAuthor("caochenlei");//设置项目作者名称
        gc.setIdType(IdType.AUTO);//设置主键策略
        gc.setBaseResultMap(true);//生成基本ResultMap
        gc.setBaseColumnList(true);//生成基本ColumnList
        gc.setServiceName("%sService");//去掉服务默认前缀
        mpg.setGlobalConfig(gc);

        // 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/mp?useSSL=false&useUnicode=true&characterEncoding=utf8");
        dsc.setDriverName("com.mysql.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("123456");
        mpg.setDataSource(dsc);

        // 包配置
        PackageConfig pc = new PackageConfig();
        pc.setParent("com.caochenlei.mpdemo");
        pc.setMapper("mapper");
        pc.setXml("mapper.xml");
        pc.setEntity("pojo");
        pc.setService("service");
        pc.setServiceImpl("service.impl");
        pc.setController("controller");
        mpg.setPackageInfo(pc);

        // 策略配置
        StrategyConfig sc = new StrategyConfig();
        sc.setNaming(NamingStrategy.underline_to_camel);
        sc.setColumnNaming(NamingStrategy.underline_to_camel);
        sc.setEntityLombokModel(true);
        sc.setRestControllerStyle(true);
        sc.setControllerMappingHyphenStyle(true);
        sc.setTablePrefix("tbl_");
        sc.setInclude(scanner("表名，多个英文逗号分割").split(","));
        mpg.setStrategy(sc);

        // 生成代码
        mpg.execute();
    }

}
```

#### 5.3.5、工程结构

![image-20201003150519754](https://img-blog.csdnimg.cn/img_convert/507411d125be89ac7b1503b464eb90a1.png)

#### 5.3.6、添加代码

UserController.java

```java
@RestController
@RequestMapping("/user")
public class UserController {
    @Autowired
    private UserService userService;

    @RequestMapping("/all")
    public List<User> getAll() {
        return userService.list();
    }
}
```

#### 5.3.7、启动运行

MpDemoApplication.java 中运行主方法以此来启动整个工程

```java
@SpringBootApplication
@MapperScan("com.caochenlei.mpdemo.mapper")
public class MpDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(MpDemoApplication.class, args);
    }

}
```

#### 5.3.8、测试方法

使用浏览器打开：http://localhost:8080/user/all

![image-20201003150941091](https://img-blog.csdnimg.cn/img_convert/977da9d6221c512517877b24352f06cc.png)

#### 5.3.9、温馨提示

需要Lombok插件支持，只需要安装一下就可以了，打开 IDEA，进入 File -> Settings -> Plugins，安装完成后重启

![image-20201004212357130](https://img-blog.csdnimg.cn/img_convert/f856df73917ee06e6db169372466527a.png)

### 5.4、代码生成器方法

通用 Service CRUD 封装 IService 接口，进一步封装 CRUD 采用 `get 查询单行` `remove 删除` `list 查询集合` `page 分页` 前缀命名方式区分 `Mapper` 层避免混淆，泛型 T 为任意实体对象，建议如果存在自定义通用 Service 方法的可能，请创建自己的 IBaseService 继承 Mybatis-Plus 提供的基类，对象 Wrapper 为 条件构造器。

#### 5.4.1、save

```java
// 插入一条记录（选择字段，策略插入）
boolean save(T entity);
// 插入（批量）
boolean saveBatch(Collection<T> entityList);
// 插入（批量）
boolean saveBatch(Collection<T> entityList, int batchSize);
```

参数说明：

| 类型          | 参数名     | 描述         |
| ------------- | ---------- | ------------ |
| T             | entity     | 实体对象     |
| Collection<T> | entityList | 实体对象集合 |
| int           | batchSize  | 插入批次数量 |

#### 5.4.2、saveOrUpdate

```java
// TableId 注解存在更新记录，否插入一条记录
boolean saveOrUpdate(T entity);
// 根据updateWrapper尝试更新，否继续执行saveOrUpdate(T)方法
boolean saveOrUpdate(T entity, Wrapper<T> updateWrapper);
// 批量修改插入
boolean saveOrUpdateBatch(Collection<T> entityList);
// 批量修改插入
boolean saveOrUpdateBatch(Collection<T> entityList, int batchSize);
```

参数说明：

| 类型          | 参数名        | 描述                             |
| ------------- | ------------- | -------------------------------- |
| T             | entity        | 实体对象                         |
| Wrapper<T>    | updateWrapper | 实体对象封装操作类 UpdateWrapper |
| Collection<T> | entityList    | 实体对象集合                     |
| int           | batchSize     | 插入批次数量                     |

#### 5.4.3、remove

```java
// 根据 entity 条件，删除记录
boolean remove(Wrapper<T> queryWrapper);
// 根据 ID 删除
boolean removeById(Serializable id);
// 根据 columnMap 条件，删除记录
boolean removeByMap(Map<String, Object> columnMap);
// 删除（根据ID 批量删除）
boolean removeByIds(Collection<? extends Serializable> idList);
```

参数说明：

| 类型                               | 参数名       | 描述                    |
| ---------------------------------- | ------------ | ----------------------- |
| Wrapper<T>                         | queryWrapper | 实体包装类 QueryWrapper |
| Serializable                       | id           | 主键ID                  |
| Map<String, Object>                | columnMap    | 表字段 map 对象         |
| Collection<? extends Serializable> | idList       | 主键ID列表              |

#### 5.4.4、update

```java
// 根据 UpdateWrapper 条件，更新记录 需要设置sqlset
boolean update(Wrapper<T> updateWrapper);
// 根据 whereEntity 条件，更新记录
boolean update(T entity, Wrapper<T> updateWrapper);
// 根据 ID 选择修改
boolean updateById(T entity);
// 根据ID 批量更新
boolean updateBatchById(Collection<T> entityList);
// 根据ID 批量更新
boolean updateBatchById(Collection<T> entityList, int batchSize);
```

参数说明：

| 类型          | 参数名        | 描述                             |
| ------------- | ------------- | -------------------------------- |
| Wrapper<T>    | updateWrapper | 实体对象封装操作类 UpdateWrapper |
| T             | entity        | 实体对象                         |
| Collection<T> | entityList    | 实体对象集合                     |
| int           | batchSize     | 更新批次数量                     |

#### 5.4.5、get

```java
// 根据 ID 查询
T getById(Serializable id);
// 根据 Wrapper，查询一条记录。结果集，如果是多个会抛出异常，随机取一条加上限制条件 wrapper.last("LIMIT 1")
T getOne(Wrapper<T> queryWrapper);
// 根据 Wrapper，查询一条记录
T getOne(Wrapper<T> queryWrapper, boolean throwEx);
// 根据 Wrapper，查询一条记录
Map<String, Object> getMap(Wrapper<T> queryWrapper);
// 根据 Wrapper，查询一条记录
<V> V getObj(Wrapper<T> queryWrapper, Function<? super Object, V> mapper);
```

参数说明：

| 类型                        | 参数名       | 描述                            |
| --------------------------- | ------------ | ------------------------------- |
| Serializable                | id           | 主键ID                          |
| Wrapper<T>                  | queryWrapper | 实体对象封装操作类 QueryWrapper |
| boolean                     | throwEx      | 有多个 result 是否抛出异常      |
| T                           | entity       | 实体对象                        |
| Function<? super Object, V> | mapper       | 转换函数                        |

#### 5.4.6、list

```java
// 查询所有
List<T> list();
// 查询列表
List<T> list(Wrapper<T> queryWrapper);
// 查询（根据ID 批量查询）
Collection<T> listByIds(Collection<? extends Serializable> idList);
// 查询（根据 columnMap 条件）
Collection<T> listByMap(Map<String, Object> columnMap);
// 查询所有列表
List<Map<String, Object>> listMaps();
// 查询列表
List<Map<String, Object>> listMaps(Wrapper<T> queryWrapper);
// 查询全部记录
List<Object> listObjs();
// 查询全部记录
<V> List<V> listObjs(Function<? super Object, V> mapper);
// 根据 Wrapper 条件，查询全部记录
List<Object> listObjs(Wrapper<T> queryWrapper);
// 根据 Wrapper 条件，查询全部记录
<V> List<V> listObjs(Wrapper<T> queryWrapper, Function<? super Object, V> mapper);
```

参数说明：

| 类型                               | 参数名       | 描述                            |
| ---------------------------------- | ------------ | ------------------------------- |
| Wrapper<T>                         | queryWrapper | 实体对象封装操作类 QueryWrapper |
| Collection<? extends Serializable> | idList       | 主键ID列表                      |
| Map<?String, Object>               | columnMap    | 表字段 map 对象                 |
| Function<? super Object, V>        | mapper       | 转换函数                        |

#### 5.4.7、page

```java
// 无条件分页查询
IPage<T> page(IPage<T> page);
// 条件分页查询
IPage<T> page(IPage<T> page, Wrapper<T> queryWrapper);
// 无条件分页查询
IPage<Map<String, Object>> pageMaps(IPage<T> page);
// 条件分页查询
IPage<Map<String, Object>> pageMaps(IPage<T> page, Wrapper<T> queryWrapper);
```

参数说明：

| 类型       | 参数名       | 描述                            |
| ---------- | ------------ | ------------------------------- |
| IPage<T>   | page         | 翻页对象                        |
| Wrapper<T> | queryWrapper | 实体对象封装操作类 QueryWrapper |

#### 5.4.8、count

```java
// 查询总记录数
int count();
// 根据 Wrapper 条件，查询总记录数
int count(Wrapper<T> queryWrapper);
```

参数说明：

| 类型       | 参数名       | 描述                            |
| ---------- | ------------ | ------------------------------- |
| Wrapper<T> | queryWrapper | 实体对象封装操作类 QueryWrapper |

#### 5.4.9、chain

query

```java
// 链式查询 普通
QueryChainWrapper<T> query();
// 链式查询 lambda 式。注意：不支持 Kotlin
LambdaQueryChainWrapper<T> lambdaQuery(); 

// 示例：
query().eq("column", value).one();
lambdaQuery().eq(Entity::getId, value).list();
```

update

```java
// 链式更改 普通
UpdateChainWrapper<T> update();
// 链式更改 lambda 式。注意：不支持 Kotlin 
LambdaUpdateChainWrapper<T> lambdaUpdate();

// 示例：
update().eq("column", value).remove();
lambdaUpdate().eq(Entity::getId, value).update(entity);
```

## 第六章 MyBatis-Plus3配置详解

### 6.1、配置概述

本文讲解了`MyBatis-Plus`在使用过程中的配置选项，其中部分配置继承自`MyBatis`原生所支持的配置。

### 6.2、配置方式

- Spring Boot：

  - application.properties（本文采用）

    ```properties
    mybatis-plus.mapper-locations=classpath*:**/mapper/xml/*.xml
    mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
    ......
    ```
    
- application.yaml（推荐使用）
  
  ```yaml
    mybatis-plus:
      ......
      configuration:
        ......
      global-config:
        ......
        db-config:
          ......  
    ```
  
- Spring MVC：

  - ```xml
    <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="configuration" ref="configuration"/> <!--  非必须  -->
        <property name="globalConfig" ref="globalConfig"/> <!--  非必须  -->
        ......
    </bean>
    
    <bean id="configuration" class="com.baomidou.mybatisplus.core.MybatisConfiguration">
        ......
    </bean>
    
    <bean id="globalConfig" class="com.baomidou.mybatisplus.core.config.GlobalConfig">
        <property name="dbConfig" ref="dbConfig"/> <!--  非必须  -->
        ......
    </bean>
    
    <bean id="dbConfig" class="com.baomidou.mybatisplus.core.config.GlobalConfig.DbConfig">
        ......
    </bean>
    ```

### 6.3、配置选项

#### 6.3.1、mapperLocations

- 类型：`String[]`
- 默认值：`["classpath*:/mapper/**/*.xml"]`

MyBatis Mapper 所对应的 XML 文件位置，如果您在 Mapper 中有自定义方法（XML 中有自定义实现），需要进行该配置，告诉 Mapper 所对应的 XML 文件位置，Maven 多模块项目的扫描路径需以 `classpath*:` 开头 （即加载多个 jar 包下的 XML 文件）。

```properties
mybatis-plus.mapper-locations=classpath*:**/mapper/xml/*.xml
```

#### 6.3.2、typeAliasesPackage

- 类型：`String`
- 默认值：`null`

MyBaits 别名包扫描路径，通过该属性可以给包中的类注册别名，注册后在 Mapper 对应的 XML 文件中可以直接使用类名，而不用使用全限定的类名（即 XML 中调用的时候不用包含包名）。

```properties
mybatis-plus.type-aliases-package=com.caochenlei.mpdemo.pojo
```

#### 6.3.3、typeHandlersPackage

- 类型：`String`
- 默认值：`null`

TypeHandler 扫描路径，如果配置了该属性，SqlSessionFactoryBean 会把该包下面的类注册为对应的 TypeHandler，TypeHandler 通常用于自定义类型转换。

```properties
mybatis-plus.type-handlers-package=com.caochenlei.mpdemo.type
```

#### 6.3.4、typeEnumsPackage

- 类型：`String`
- 默认值：`null`

枚举类 扫描路径，如果配置了该属性，会将路径下的枚举类进行注入，让实体类字段能够简单快捷的使用枚举属性。

```properties
mybatis-plus.type-enums-package=com.caochenlei.mpdemo.myenum
```

#### 6.3.5、checkConfigLocation

- 类型：`boolean`
- 默认值：`false`

启动时是否检查 MyBatis XML 文件的存在，默认不检查。

```properties
mybatis-plus.check-config-location=false
```

#### 6.3.6、executorType

- 类型：`ExecutorType`
- 默认值：`simple`

通过该属性可指定 MyBatis 的执行器，MyBatis 的执行器总共有三种：

- ExecutorType.SIMPLE：该执行器类型不做特殊的事情，为每个语句的执行创建一个新的预处理语句（PreparedStatement）
- ExecutorType.REUSE：该执行器类型会复用预处理语句（PreparedStatement）
- ExecutorType.BATCH：该执行器类型会批量执行所有的更新语句

```properties
mybatis-plus.executor-type=simple
```

#### 6.3.7、configurationProperties

- 类型：`Properties`
- 默认值：`null`

指定外部化 MyBatis Properties 配置，通过该配置可以抽离配置，实现不同环境的配置部署。

#### 6.3.8、configuration

- 类型：`Configuration`
- 默认值：`null`

原生 MyBatis 所支持的配置，本部分（Configuration）的配置大都为 MyBatis 原生支持的配置，这意味着您可以通过 MyBatis XML 配置文件的形式进行配置。

##### 6.3.8.1、mapUnderscoreToCamelCase

- 类型：`boolean`
- 默认值：`true`

是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN（下划线命名） 到经典 Java 属性名 aColumn（驼峰命名） 的类似映射。此属性在 MyBatis 中原默认值为 false，在 MyBatis-Plus 中，此属性也将用于生成最终的 SQL 的 select body，如果您的数据库命名符合规则无需使用 `@TableField` 注解指定数据库字段名。

```properties
mybatis-plus.configuration.map-underscore-to-camel-case=true
```

##### 6.3.8.2、defaultEnumTypeHandler

- 类型：`Class<? extends TypeHandler`
- 默认值：`org.apache.ibatis.type.EnumTypeHandler`

默认枚举处理类，如果配置了该属性，枚举将统一使用指定处理器进行处理。

需要注意，它的取值可以有以下几种，可以使用内置，也可以自定义：

- org.apache.ibatis.type.EnumTypeHandler : 存储枚举的名称
- org.apache.ibatis.type.EnumOrdinalTypeHandler : 存储枚举的索引
- com.baomidou.mybatisplus.extension.handlers.MybatisEnumTypeHandler : 枚举类需要实现IEnum接口或字段标记@EnumValue注解.(3.1.2以下版本为EnumTypeHandler)

```properties
mybatis-plus.configuration.default-enum-type-handler=org.apache.ibatis.type.EnumTypeHandler
```

##### 6.3.8.3、aggressiveLazyLoading

- 类型：`boolean`
- 默认值：`true`

当设置为 true 的时候，懒加载的对象可能被任何懒属性全部加载，否则，每个属性都按需加载。需要和 lazyLoadingEnabled 一起使用。

```properties
mybatis-plus.configuration.aggressive-lazy-loading=true
mybatis-plus.configuration.lazy-loading-enabled=true
```

##### 6.3.8.4、autoMappingBehavior

- 类型：`AutoMappingBehavior`
- 默认值：`partial`

MyBatis 自动映射策略，通过该配置可指定 MyBatis 是否并且如何来自动映射数据表字段与对象的属性，总共有 3 种可选值：

- AutoMappingBehavior.NONE：不启用自动映射
- AutoMappingBehavior.PARTIAL：只对非嵌套的 resultMap 进行自动映射
- AutoMappingBehavior.FULL：对所有的 resultMap 都进行自动映射

```properties
mybatis-plus.configuration.auto-mapping-behavior=partial
```

##### 6.3.8.5、autoMappingUnknownColumnBehavior

- 类型：`AutoMappingUnknownColumnBehavior`
- 默认值：`NONE`

MyBatis 自动映射时未知列或未知属性处理策略，通过该配置可指定 MyBatis 在自动映射过程中遇到未知列或者未知属性时如何处理，总共有 3 种可选值：

- AutoMappingUnknownColumnBehavior.NONE：不做任何处理 (默认值)
- AutoMappingUnknownColumnBehavior.WARNING：以日志的形式打印相关警告信息
- AutoMappingUnknownColumnBehavior.FAILING：当作映射失败处理，并抛出异常和详细信息

```properties
mybatis-plus.configuration.auto-mapping-unknown-column-behavior=none
```

##### 6.3.8.6、localCacheScope

- 类型：`String`
- 默认值：`SESSION`

Mybatis一级缓存，默认为 SESSION。

- SESSION：session级别缓存，同一个session相同查询语句不会再次查询数据库
- STATEMENT：关闭一级缓存

单服务架构中（有且仅有只有一个程序提供相同服务），一级缓存开启不会影响业务，只会提高性能。 微服务架构中需要关闭一级缓存，原因：Service1先查询数据，若之后Service2修改了数据，之后Service1又再次以同样的查询条件查询数据，因走缓存会出现查处的数据不是最新数据。

```properties
mybatis-plus.configuration.local-cache-scope=session
```

##### 6.3.8.7、cacheEnabled

- 类型：`boolean`
- 默认值：`true`

开启Mybatis二级缓存，默认为 true。

```properties
mybatis-plus.configuration.cache-enabled=true
```

##### 6.3.8.8、callSettersOnNulls

- 类型：`boolean`
- 默认值：`false`

指定当结果集中值为 null 的时候是否调用映射对象的 Setter（Map 对象时为 put）方法，通常运用于有 Map.keySet() 依赖或 null 值初始化的情况。

通俗的讲，即 MyBatis 在使用 resultMap 来映射查询结果中的列，如果查询结果中包含空值的列，则 MyBatis 在映射的时候，不会映射这个字段，这就导致在调用到该字段的时候由于没有映射，取不到而报空指针异常。

当您遇到类似的情况，请针对该属性进行相关配置以解决以上问题。

> 注意：基本类型（int、boolean 等）是不能设置成 null 的。

```properties
mybatis-plus.configuration.call-setters-on-nulls=false
```

##### 6.3.8.9、configurationFactory

- 类型：`Class<?>`
- 默认值：`null`

指定一个提供 Configuration 实例的工厂类。该工厂生产的实例将用来加载已经被反序列化对象的懒加载属性值，其必须包含一个签名方法`static Configuration getConfiguration()`。（从 3.2.3 版本开始）

```properties
mybatis-plus.configuration.configuration-factory=
```

##### 6.3.8.10、MyBatis3的配置属性

这里只列出 MyBatis3 的 settings 标签的属性，更多配置，请自行探索！

我们这里以日志打印为例，一般使用驼峰命名对应 “ - ”

```properties
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

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

#### 6.3.9、globalConfig

- 类型：`com.baomidou.mybatisplus.core.config.GlobalConfig`
- 默认值：`GlobalConfig::new`

MyBatis-Plus 全局策略配置。

##### 6.3.9.1、banner

- 类型：`boolean`
- 默认值：`true`

是否控制台 print mybatis-plus 的 LOGO。

```properties
mybatis-plus.global-config.banner=true
```

##### 6.3.9.2、enableSqlRunner

- 类型：`boolean`
- 默认值：`false`

是否初始化 SqlRunner(com.baomidou.mybatisplus.extension.toolkit.SqlRunner)

```properties
mybatis-plus.global-config.enable-sql-runner=false
```

##### 6.3.9.3、superMapperClass

- 类型：`Class`
- 默认值：`com.baomidou.mybatisplus.core.mapper.Mapper.class`

通用Mapper父类(影响sqlInjector，只有这个的子类的 mapper 才会注入 sqlInjector 内的 method)

```properties
mybatis-plus.global-config.super-mapper-class=com.baomidou.mybatisplus.core.mapper.Mapper
```

##### 6.3.9.4、dbConfig

- 类型：`com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig`
- 默认值：`null`

MyBatis-Plus 全局策略中的 DB 策略配置。

###### 6.3.9.4.1、idType

- 类型：`com.baomidou.mybatisplus.annotation.IdType`
- 默认值：`ASSIGN_ID`

全局默认主键类型。

```properties
mybatis-plus.global-config.db-config.id-type=assign_id
```

###### 6.3.9.4.2、tablePrefix

- 类型：`String`
- 默认值：`null`

表名前缀。

```properties
mybatis-plus.global-config.db-config.table-prefix=tbl_
```

###### 6.3.9.4.3、schema

- 类型：`String`
- 默认值：`null`

schema。

```properties
mybatis-plus.global-config.db-config.schema=
```

###### 6.3.9.4.4、columnFormat

- 类型：`String`
- 默认值：`null`

字段 format，例: `%s`，(对主键无效)

```properties
mybatis-plus.global-config.db-config.column-format=
```

###### 6.3.9.4.5、propertyFormat

- 类型：`String`
- 默认值：`null`

entity 的字段(property)的 format，只有在 column as property 这种情况下生效例: `%s`，(对主键无效)(since 3.3.0)。

```properties
mybatis-plus.global-config.db-config.property-format=
```

###### 6.3.9.4.6、tableUnderline

- 类型：`boolean`
- 默认值：`true`

表名是否使用驼峰转下划线命名，只对表名生效。

```properties
mybatis-plus.global-config.db-config.table-underline=true
```

###### 6.3.9.4.7、capitalMode

- 类型：`boolean`
- 默认值：`false`

大写命名，对表名和字段名均生效。

```properties
mybatis-plus.global-config.db-config.capital-mode=false
```

###### 6.3.9.4.8、logicDeleteField

- 类型：`String`
- 默认值：`null`

全局的entity的逻辑删除字段属性名，(逻辑删除下有效)

```properties
mybatis-plus.global-config.db-config.logic-delete-field=
```

###### 6.3.9.4.9、logicDeleteValue

- 类型：`String`
- 默认值：`1`

逻辑已删除值，(逻辑删除下有效)

```properties
mybatis-plus.global-config.db-config.logic-delete-value=1
```

###### 6.3.9.4.10、logicNotDeleteValue

- 类型：`String`
- 默认值：`0`

逻辑未删除值，(逻辑删除下有效)

```properties
mybatis-plus.global-config.db-config.logic-not-delete-value=0
```

###### 6.3.9.4.11、insertStrategy

- 类型：`com.baomidou.mybatisplus.annotation.FieldStrategy`
- 默认值：`NOT_NULL`

字段验证策略之 insert，在 insert 的时候的字段验证策略。

```properties
mybatis-plus.global-config.db-config.insert-strategy=not_null
```

###### 6.3.9.4.12、updateStrategy

- 类型：`com.baomidou.mybatisplus.annotation.FieldStrategy`
- 默认值：`NOT_NULL`

字段验证策略之 update，在 update 的时候的字段验证策略。

```properties
mybatis-plus.global-config.db-config.update-strategy=not_null
```

###### 6.3.9.4.13、selectStrategy

- 类型：`com.baomidou.mybatisplus.annotation.FieldStrategy`
- 默认值：`NOT_NULL`

字段验证策略之 select，在 select 的时候的字段验证策略既 wrapper 根据内部 entity 生成的 where 条件。

```properties
mybatis-plus.global-config.db-config.select-strategy=not_null
```

### 6.4、配置小结

```properties
#mybatis-plus
mybatis-plus.mapper-locations=classpath*:**/mapper/xml/*.xml
mybatis-plus.type-aliases-package=com.caochenlei.mpdemo.pojo
mybatis-plus.type-handlers-package=com.caochenlei.mpdemo.type
mybatis-plus.type-enums-package=com.caochenlei.mpdemo.enum
mybatis-plus.check-config-location=false
mybatis-plus.executor-type=simple
#mybatis-plus.configuration
mybatis-plus.configuration.map-underscore-to-camel-case=true
mybatis-plus.configuration.default-enum-type-handler=org.apache.ibatis.type.EnumTypeHandler
mybatis-plus.configuration.aggressive-lazy-loading=true
mybatis-plus.configuration.lazy-loading-enabled=true
mybatis-plus.configuration.auto-mapping-behavior=partial
mybatis-plus.configuration.auto-mapping-unknown-column-behavior=none
mybatis-plus.configuration.local-cache-scope=session
mybatis-plus.configuration.cache-enabled=true
mybatis-plus.configuration.call-setters-on-nulls=false
mybatis-plus.configuration.configuration-factory=
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
#mybatis-plus.global-config
mybatis-plus.global-config.banner=true
mybatis-plus.global-config.enable-sql-runner=false
mybatis-plus.global-config.super-mapper-class=com.baomidou.mybatisplus.core.mapper.Mapper
#mybatis-plus.global-config.db-config
mybatis-plus.global-config.db-config.id-type=assign_id
mybatis-plus.global-config.db-config.table-prefix=tbl_
mybatis-plus.global-config.db-config.schema=
mybatis-plus.global-config.db-config.column-format=
mybatis-plus.global-config.db-config.property-format=
mybatis-plus.global-config.db-config.table-underline=true
mybatis-plus.global-config.db-config.capital-mode=false
mybatis-plus.global-config.db-config.logic-delete-field=
mybatis-plus.global-config.db-config.logic-delete-value=1
mybatis-plus.global-config.db-config.logic-not-delete-value=0
mybatis-plus.global-config.db-config.insert-strategy=not_null
mybatis-plus.global-config.db-config.update-strategy=not_null
mybatis-plus.global-config.db-config.select-strategy=not_null
```

## 第七章 MyBatis-Plus3插件扩展

创建包：com.caochenlei.mpdemo.config

新建类：com.caochenlei.mpdemo.config.MybatisPlusConfig

```java
@Configuration
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        
        return interceptor;
    }

}
```

### 7.1、分页插件

**插件功能**：提供数据分页功能。

**添加插件**：

```java
@Configuration
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 添加分页插件
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }

}
```

**属性设置**：

| 属性名   | 类型     | 默认值 | 描述                                                         |
| -------- | -------- | ------ | ------------------------------------------------------------ |
| overflow | boolean  | false  | 溢出总页数后是否进行处理(默认不处理,参见 `插件#continuePage` 方法) |
| maxLimit | Long     |        | 单页分页条数限制(默认无限制,参见 `插件#handlerLimit` 方法)   |
| dbType   | DbType   |        | 数据库类型(根据类型获取应使用的分页方言,参见 `插件#findIDialect` 方法) |
| dialect  | IDialect |        | 方言实现类(参见 `插件#findIDialect` 方法)                    |

**测试方法**：

```java
@Test
void testPagination() {
    Page<Employee> page = new Page<>(1,5);
    Page<Employee> employeePage = employeeMapper.selectPage(page, null);
    employeePage.getRecords().forEach(System.out::println);
    System.out.println("当前页：" + employeePage.getCurrent());
    System.out.println("总页数：" + employeePage.getPages());
    System.out.println("记录数：" + employeePage.getTotal());
    System.out.println("是否有上一页：" + employeePage.hasPrevious());
    System.out.println("是否有下一页：" + employeePage.hasNext());
}
```

**控制台截图**：

![image-20201004143442128](https://img-blog.csdnimg.cn/img_convert/ef7a42c27c059478bfbe84310c4637d7.png)

**数据库截图**：

![image-20201004143504869](https://img-blog.csdnimg.cn/img_convert/0e4dbaf11ff219aa5137b4a8f961b6a3.png)

### 7.2、执行分析插件

**插件功能**：防止全表更新与全表删除，依次保护数据库的安全，建议开发环境使用，不建议生产环境使用。

**添加插件**：

```java
@Configuration
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 添加分页插件
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        // 添加执行分析插件
        interceptor.addInnerInterceptor(new BlockAttackInnerInterceptor());
        return interceptor;
    }

}
```

**测试方法**：

```java
@Test
void testBlockAttack() {
    int result = employeeMapper.delete(null);
    System.out.println(result);
}
```

**控制台截图**：

![image-20201004203841923](https://img-blog.csdnimg.cn/img_convert/641483b91f9ed98157682515ef5e60f1.png)

**数据库截图**：

![image-20201004203901759](https://img-blog.csdnimg.cn/img_convert/664ba37d9b3b759ff38eb5f4e4695b65.png)

### 7.3、性能分析插件

**插件功能**：该功能依赖 `p6spy` 组件，完美的输出打印 SQL 及执行时长， `3.1.0` 以上版本支持。

**添加依赖**：

```xml
<dependency>
    <groupId>p6spy</groupId>
    <artifactId>p6spy</artifactId>
    <version>3.9.1</version>
</dependency>
```

**修改配置**：application.properties

```xml
#mysql
#spring.datasource.driver-class-name=com.mysql.jdbc.Driver
#spring.datasource.url=jdbc:mysql://localhost:3306/mp?useUnicode=true&characterEncoding=utf8
#spring.datasource.username=root
#spring.datasource.password=123456
#p6spy
spring.datasource.driver-class-name=com.p6spy.engine.spy.P6SpyDriver
spring.datasource.url=jdbc:p6spy:mysql://localhost:3306/mp
spring.datasource.username=root
spring.datasource.password=123456
```

**添加配置**：spy.properties

```properties
# 3.2.1以上使用
modulelist=com.baomidou.mybatisplus.extension.p6spy.MybatisPlusLogFactory,com.p6spy.engine.outage.P6OutageFactory
# 自定义日志打印
logMessageFormat=com.baomidou.mybatisplus.extension.p6spy.P6SpyLogger
# 日志输出到控制台
appender=com.baomidou.mybatisplus.extension.p6spy.StdoutLogger
# 使用日志系统记录 sql
#appender=com.p6spy.engine.spy.appender.Slf4JLogger
# 设置 p6spy driver 代理
deregisterdrivers=true
# 取消JDBC URL前缀
useprefix=true
# 配置记录 Log 例外,可去掉的结果集有error,info,batch,debug,statement,commit,rollback,result,resultset.
excludecategories=info,debug,result,commit,resultset
# 日期格式
dateformat=yyyy-MM-dd HH:mm:ss
# 实际驱动可多个
driverlist=com.mysql.jdbc.Driver
# 是否开启慢SQL记录
outagedetection=true
# 慢SQL记录标准 2 秒
outagedetectioninterval=2
```

**注意问题**：

- driver-class-name 为 p6spy 提供的驱动类
- url 前缀为 jdbc:p6spy 跟着冒号为对应数据库连接地址
- 打印出sql为null，在excludecategories增加commit
- 批量操作不打印sql，去除excludecategories中的batch
- 批量操作打印重复的问题请使用MybatisPlusLogFactory (3.2.1新增）
- 该插件有性能损耗，不建议生产环境使用

**添加插件**：该插件不用添加自动集成

**测试方法**：

```java
@Test
void testPagination() {
    Page<Employee> page = new Page<>(1,5);
    Page<Employee> employeePage = employeeMapper.selectPage(page, null);
    employeePage.getRecords().forEach(System.out::println);
    System.out.println("当前页：" + employeePage.getCurrent());
    System.out.println("总页数：" + employeePage.getPages());
    System.out.println("记录数：" + employeePage.getTotal());
    System.out.println("是否有上一页：" + employeePage.hasPrevious());
    System.out.println("是否有下一页：" + employeePage.hasNext());
}
```

**控制台截图**：

![image-20201004205521523](https://img-blog.csdnimg.cn/img_convert/26a31d71304c1436eb2704d48992c1b9.png)

**数据库截图**：

![image-20201004205704834](https://img-blog.csdnimg.cn/img_convert/d88232ccd0d7529af7092cbd64c4bc22.png)

### 7.4、乐观锁插件

**插件功能**：当要更新一条记录的时候，希望这条记录没有被别人更新，可以使用乐观锁，MyBatis-Plus就提供了乐观锁插件。

**实现方式**：乐观锁实现方式如下

1. 取出记录时，获取当前version
2. 更新时，带上这个version
3. 执行更新时， set version = newVersion where version = oldVersion
4. 如果version不对，就更新失败

**添加字段**：

- 数据库：

  ```sql
  ALTER TABLE `tbl_employee` ADD COLUMN `version` INT(11) DEFAULT '1' NULL AFTER `age`; 
  ```
  
- 实体类：

  ```java
  @Version
  private Integer version;
  
  public Integer getVersion() {
      return version;
  }
  
  public void setVersion(Integer version) {
      this.version = version;
  }
  ```

**注意事项**：

- 支持的数据类型只有:int,Integer,long,Long,Date,Timestamp,LocalDateTime
- 整数类型下 `newVersion = oldVersion + 1`
- `newVersion` 会回写到 `entity` 中
- 仅支持 `updateById(id)` 与 `update(entity, wrapper)` 方法
- 在 `update(entity, wrapper)` 方法下， `wrapper` 不能复用

**添加插件**：

```java
@Configuration
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 添加分页插件
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        // 添加执行分析插件
        interceptor.addInnerInterceptor(new BlockAttackInnerInterceptor());
        // 添加乐观锁插件
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    }

}
```

**测试方法**：

```java
@Test
void testOptimisticLocker(){
    Employee employee = employeeMapper.selectById(1);
    employee.setLastName("Bob");
    int result = employeeMapper.updateById(employee);
    System.out.println(result);
}
```

**控制台截图**：

![image-20201004210810414](https://img-blog.csdnimg.cn/img_convert/821729e40b177862b0abfcf4e4231186.png)

**数据库截图**：

![image-20201004210838714](https://img-blog.csdnimg.cn/img_convert/a00a4b3630f16ef199493ed1973abd99.png)

### 7.5、快速开发插件

**插件功能**：可以通过接口方法名直接创建对应mapper中的sql标签，还可以Mapper和xml可以来回跳，更多功能自行探索！

**计划支持：**

- 连接数据源之后 xml 里自动提示字段
- sql 增删改查
- 集成 MP 代码生成
- 其它

**安装方法**：

需要MybatisX插件支持，只需要安装一下就可以了，打开 IDEA，进入 File -> Settings -> Plugins，安装完成后重启

![image-20201004212623541](https://img-blog.csdnimg.cn/img_convert/9b1803a41d1f4694f99347527f8de08f.png)

## 第八章 MyBatis-Plus3其它功能

### 8.1、Sql 注入器

根据 MybatisPlus 的 AutoSqlInjector 可以自定义各种你想要的 SQL 注入到全局中，相当于自定义 MybatisPlus 自动注入的方法。之前需要在 xml 中进行配置的 SQL 语句，现在通过扩展 AutoSqlInjector 在加载 mybatis 环境时就注入。

**接口方法**：然后在添加一个@Repository注解

UserMapper.java

```java
@Repository
public interface UserMapper extends BaseMapper<User> {

    public void deleteAll();

}
```

> 注意：我们不需要编写xml映射，因为我们会采用sql注入的形式，在 MybatisPlus 启动的时候就注入。

**创建对象**：在该com.caochenlei.mpdemo.injector(没有创建)包下创建MyMappedStatement.java、MySqlInjector.java

MyMappedStatement.java

```java
public class MyMappedStatement extends AbstractMethod {
    @Override
    public MappedStatement injectMappedStatement(Class<?> mapperClass, Class<?> modelClass, TableInfo tableInfo) {
        // 接口中的方法名
        String method = "deleteAll";
        // 该方法执行语句
        String sql = "delete from " + tableInfo.getTableName();
        // 创建SqlSource
        SqlSource sqlSource = languageDriver.createSqlSource(configuration, sql, modelClass);
        // 构造一个删除的MappedStatement并返回
        return this.addDeleteMappedStatement(mapperClass, method, sqlSource);
    }
}
```

MySqlInjector.java

```java
public class MySqlInjector extends DefaultSqlInjector {
    @Override
    public List<AbstractMethod> getMethodList(Class<?> mapperClass) {
        List<AbstractMethod> methodList = super.getMethodList(mapperClass);
        methodList.add(new MyMappedStatement());
        return methodList;
    }
}
```

**注释“执行分析插件”**：会影响代码执行，它会阻止全表删除操作，所以先注释掉，反正我们已经学会了

MybatisPlusConfig.java

```java
@Configuration
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 添加分页插件
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        // 添加执行分析插件
        //interceptor.addInnerInterceptor(new BlockAttackInnerInterceptor());
        // 添加乐观锁插件
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    }

}
```

**添加“自定义SQL注入对象”**：

MybatisPlusConfig.java

```java
@Configuration
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 添加分页插件
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        // 添加执行分析插件
        //interceptor.addInnerInterceptor(new BlockAttackInnerInterceptor());
        // 添加乐观锁插件
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    }

    @Bean
    public MySqlInjector mySqlInjector() {
        return new MySqlInjector();
    }

}
```

**测试方法**：

```java
@Autowired
private UserMapper userMapper;

@Test
void testDeleteAll() {
    userMapper.deleteAll();
}
```

**控制台截图**：没有写xml映射代码，它也会执行删除，原因是我们在启动的时候自动注入了

![image-20201005223516996](https://img-blog.csdnimg.cn/img_convert/8901fa75eec495fab89d3ffd559d2abf.png)

**数据库截图**：

![image-20201005223600547](https://img-blog.csdnimg.cn/img_convert/883cc595767e5fd3017979d14c2cabc9.png)

### 8.2、逻辑删除

**逻辑删除**：数据删除并不会真正的从数据库中将数据删除掉，而是将当前被删除的这条数据中的一个逻辑删除字段置为删除状态。

**导入数据**：

```sql
## 删除表
DROP TABLE IF EXISTS `tbl_student`;
## 创建表
CREATE TABLE `tbl_student` (
  `id` INT(11) NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `name` VARCHAR(255) DEFAULT NULL COMMENT '学生姓名',
  `age` INT(11) DEFAULT NULL COMMENT '学生年龄',
  `grade` VARCHAR(255) DEFAULT NULL COMMENT '学生年纪',
  `status` VARCHAR(255) DEFAULT NULL COMMENT '学生状态',
  `deleted` INT(11) DEFAULT '0' COMMENT '是否删除（0:未删除、1:已删除）',
  PRIMARY KEY (`id`)
) DEFAULT CHARSET=utf8 COMMENT='学生';
## 导入数据
INSERT  INTO `tbl_student`(`id`,`name`,`age`,`grade`,`status`,`deleted`) VALUES (1,'张三',NULL,NULL,NULL,0);
```

**代码生成**：运行代码生成器，生成代码

![image-20201006112349574](https://img-blog.csdnimg.cn/img_convert/92791979b98be99a4ee2ef244f7cf906.png)

**添加配置**：application.properties，如果以下配置已经存在，请忽略此步骤

```properties
#那个字段是逻辑删除字段，也可以在字段上添加注解@TableLogic，在这里边配置是全局生效
mybatis-plus.global-config.db-config.logic-delete-field=deleted
#删除后字段的值为1
mybatis-plus.global-config.db-config.logic-delete-value=1
#未删除字段的值为0
mybatis-plus.global-config.db-config.logic-not-delete-value=0
```

**测试方法**：

```java
@Autowired
private StudentMapper studentMapper;

@Test
void testLogicDelete(){
    int result = studentMapper.deleteById(1);
    System.out.println(result);
}
```

**控制台截图**：

![image-20201006112734978](https://img-blog.csdnimg.cn/img_convert/7daeb4912188a1b2af8ad40085745949.png)

**数据库截图**：你会发现数据还存在，只是将其中的逻辑删除标志由0改为了1，代表逻辑删除

![image-20201006112806827](https://img-blog.csdnimg.cn/img_convert/0b556934a55f689bfb3db00b5a6873ca.png)

### 8.3、通用枚举

实现自定义枚举有两种方式，一种是使用注解而另一种是使用实现接口IEnum的方式，在接下来的案例中，我们会分别使用这两种进行讲解，这样可以让大家学的更全面一些。

#### 8.3.1、保存枚举值

**添加配置**：application.properties，如果有以下配置，请忽略此步骤

```java
mybatis-plus.type-enums-package=com.caochenlei.mpdemo.myenum
mybatis-plus.configuration.default-enum-type-handler=org.apache.ibatis.type.EnumOrdinalTypeHandler    
```

**创建枚举类**：com.caochenlei.mpdemo.myenum.AgeEnum

```java
public enum AgeEnum implements IEnum<Integer> {
    ONE(1, "一岁"),
    TWO(2, "二岁"),
    THREE(3, "三岁");

    private int value;
    private String desc;

    AgeEnum(int value, String desc) {
        this.value = value;
        this.desc = desc;
    }

    @Override
    public Integer getValue() {
        //数值作为枚举值保存到数据库
        return this.value;
    }
}
```

**修改实体类**：com.caochenlei.mpdemo.pojo.Student

```java
/**
 * 学生年龄
 */
private AgeEnum age;
```

**测试方法**：

```java
@Test
void testAgeEnum() {
    Student student = new Student();
    student.setName("李四");
    student.setAge(AgeEnum.THREE);
    int result = studentMapper.insert(student);
    System.out.println(result);
}
```

**控制台截图**：

![image-20201006174914564](https://img-blog.csdnimg.cn/img_convert/bfcf1f48b9b9e90adcdcaf4f954b2eb2.png)

**数据库截图**：

![image-20201006174932618](https://img-blog.csdnimg.cn/img_convert/8272d33940f9d93110fe427cbe878d4f.png)

#### 8.3.2、保存枚举名称

**添加配置**：application.properties，如果有以下配置，请忽略此步骤

```java
mybatis-plus.type-enums-package=com.caochenlei.mpdemo.myenum
mybatis-plus.configuration.default-enum-type-handler=org.apache.ibatis.type.EnumOrdinalTypeHandler    
12
```

**创建枚举类**：com.caochenlei.mpdemo.myenum.GradeEnum

```java
public enum GradeEnum {
    PRIMARY(1, "小学"),
    SECONDORY(2, "中学"),
    HIGH(3, "高中");

    private int code;
    @EnumValue//描述作为枚举值保存到数据库
    private String desc;

    GradeEnum(int code, String desc) {
        this.code = code;
        this.desc = desc;
    }

    public int getCode() {
        return code;
    }

    public void setCode(int code) {
        this.code = code;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }
}
```

**修改实体类**：com.caochenlei.mpdemo.pojo.Student

```java
/**
 * 学生年纪
 */
private GradeEnum grade;
```

**测试方法**：

```java
@Test
void testGradeEnum() {
    Student student = new Student();
    student.setName("王五");
    student.setGrade(GradeEnum.HIGH);
    int result = studentMapper.insert(student);
    System.out.println(result);
}
```

**控制台截图**：

![image-20201006175356426](https://img-blog.csdnimg.cn/img_convert/8ba82a7d54d3985f7d819a229b97d90c.png)

**数据库截图**：

![image-20201006175418116](https://img-blog.csdnimg.cn/img_convert/6f6e92716d8b48a7aa7c6438850a3312.png)

### 8.4、自动填充功能

我们现在有一个需求，每当插入一个学生的时候，自动的给学生状态设置为”插入“，每当修改的时候，如果状态标志位为null，就把标志字段设置为”修改“，这时候就需要自动填充功能了。

metaobject：元对象，是 Mybatis 提供的一个用于更加方便，更加优雅的访问对象的属性，给对象的属性设置值的一个对象，还会用于包装对象，支持对 Object 、Map、Collection等对象进行包装，本质上 metaObject 获取对象的属性值或者是给对象的属性设置值，最终是要通过 Reflector 获取到属性的对应方法的 Invoker，最终 invoke。

**添加注解**：

```java
/**
 * 学生状态
 */
@TableField(fill = FieldFill.INSERT_UPDATE)
private String status;
```

**创建包**：com.caochenlei.mpdemo.metaObjectHandler

**创建自动填充功能处理器**：com.caochenlei.mpdemo.metaObjectHandler.MyMetaObjectHandler

```java
public class MyMetaObjectHandler implements MetaObjectHandler {
    @Override
    public void insertFill(MetaObject metaObject) {
        System.out.println("start insert fill ....");
        this.strictInsertFill(metaObject, "status",String.class,"插入");
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        System.out.println("start update fill ....");
        this.strictUpdateFill(metaObject, "status",String.class,"更新");
    }
}
```

**注册自动填充功能处理器**：MybatisPlusConfig

```java
@Bean
public MyMetaObjectHandler myMetaObjectHandler(){
    return new MyMetaObjectHandler();
}
```

**测试方法**：依次执行两个方法

```java
@Test
void testInsertFill(){
    Student student = new Student();
    student.setName("小六");
    int result = studentMapper.insert(student);
    System.out.println(result);
}

@Test
void testUpdateFill(){
    Student student = studentMapper.selectById(4);
    student.setName("李四-修改");
    student.setStatus(null);
    int result = studentMapper.updateById(student);
    System.out.println(result);
}
```

**控制台截图**：

testInsertFill之后

![image-20201006195955674](https://img-blog.csdnimg.cn/img_convert/586a3c21258c9fa468a13ac37c3a2ed3.png)

testUpdateFill之后

![image-20201006200553890](https://img-blog.csdnimg.cn/img_convert/5815d12e6c80e4fe647da717da1e31a9.png)

**数据库截图**：

testInsertFill之后

![image-20201006195929637](https://img-blog.csdnimg.cn/img_convert/508bdb0ae5d4d9aac62446e132cee21e.png)

testUpdateFill之后

![image-20201006200651061](https://img-blog.csdnimg.cn/img_convert/f90de454b8e3e005c3e654bce82d69e0.png)

### 8.5、字段类型处理器

字段类型处理器，用于 JavaType 与 JdbcType 之间的转换，用于 PreparedStatement 设置参数值和从 ResultSet 或 CallableStatement 中取出一个值，本文讲解 mybaits-plus 内置常用类型处理器如何通过TableField注解快速注入到 mybatis 容器中。

```java
@Data
@Accessors(chain = true)
@TableName(autoResultMap = true)
public class User {
    private Long id;

    ...


    /**
     * 注意！！ 必须开启映射注解
     *
     * @TableName(autoResultMap = true)
     *
     * 以下两种类型处理器，二选一，也可以同时存在
     *
     * 注意！！ 选择对应的 JSON 处理器也必须存在对应 JSON 解析依赖包
     */
    @TableField(typeHandler = JacksonTypeHandler.class)
    // @TableField(typeHandler = FastjsonTypeHandler.class)
    private OtherInfo otherInfo;

}
```

### 8.6、自定义ID生成器

**导入数据**：

```sql
## 删除表
DROP TABLE IF EXISTS `tbl_product`;
## 新建表
CREATE TABLE `tbl_product` (
  `pid` int(11) NOT NULL COMMENT '商品主键',
  `pname` varchar(255) DEFAULT NULL COMMENT '商品名称',
  PRIMARY KEY (`pid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='商品';
```

**生成代码**：运行CodeGenerator

![image-20201006210828320](https://img-blog.csdnimg.cn/img_convert/ab9befd263d092403766e1a247cc7281.png)

**温馨提示**：自3.3.0开始，默认使用雪花算法+UUID(不含中划线)

**重写方法**：

| 方法     | 主键生成策略      | 主键类型            | 说明                                                         |
| -------- | ----------------- | ------------------- | ------------------------------------------------------------ |
| nextId   | ASSIGN_ID         | Long,Integer,String | 支持自动转换为String类型，但数值类型不支持自动转换，需精准匹配，例如返回Long，实体主键就不支持定义为Integer |
| nextUUID | ASSIGN_UUID，UUID | String              | 默认不含中划线的UUID生成                                     |

**创建包**：com.caochenlei.mpdemo.incrementer

**创建类**：com.caochenlei.mpdemo.incrementer.CustomIdGenerator

```java
public class CustomIdGenerator implements IdentifierGenerator {
    private final AtomicLong al = new AtomicLong(1);

    @Override
    public Long nextId(Object entity) {
        //可以将当前传入的class全类名来作为bizKey或者提取参数来生成bizKey进行分布式Id调用生成
        String bizKey = entity.getClass().getName();
        System.out.println("bizKey:" + bizKey);
        MetaObject metaObject = SystemMetaObject.forObject(entity);
        String name = (String) metaObject.getValue("pname");
        final long id = al.getAndAdd(1);
        System.out.println("为" + name + "生成主键值->:" + id);
        return id;
    }
}
```

**注册类**：MybatisPlusConfig

```java
@Bean
public IdentifierGenerator customIdGenerator(){
    return new CustomIdGenerator();
}
```

**修改主键策略**

```java
/**
 * 商品主键
 */
@TableId(value = "pid", type = IdType.ASSIGN_ID)
private Long pid;
```

**测试方法**：

```java
@Autowired
private ProductMapper productMapper;

@Test
void testCustomIdGenerator() {
    Product product = new Product();
    product.setPname("手机");
    int result = productMapper.insert(product);
    System.out.println(result);
}
```

**控制台截图**：

![image-20201006213221636](https://img-blog.csdnimg.cn/img_convert/853e9cb8c2bc17468478d61774dc52df.png)

**数据库截图**：

![image-20201006213248514](https://img-blog.csdnimg.cn/img_convert/608baf9ae39a92debdbbd29f121b791b.png)

### 8.7、Sequence主键

在实际开发中，我们经常会使用到MySQL和Oracle数据库，但是这两种数据库对于主键有不同的策略，如下：

- MySQL：支持主键自增，type = IdType.Auto
- Oracle：支持序列自增，type = IdType.INPUT

那我们Oracle又要如何使用主键策略，在这里，我们就不进行一步一步介绍了，我们只提出解决方法

1. 在实体类对象上添加注解@KeySequence(value=”序列名”, clazz=主键属性类型.class)

2. 在实体类对象的主键字段上添加注解@TableId(value = “主键名称”, type = IdType.INPUT)

   ```java
   @KeySequence(value = "SEQ_ORACLE_INTEGER_KEY", clazz = Integer.class)
   public class YourEntity {
       
       @TableId(value = "ID", type = IdType.INPUT)
       private Integer id;
   
   }
   ```
   
3. 在全局配置中注册com.baomidou.mybatisplus.incrementer.OracleKeyGenerator

   ```java
   @Bean
   public IKeyGenerator keyGenerator() {
       return new oracleKeyGenerator();
   }
   ```

除了Oracle，MyBatis-Plus还内置支持以下数据库序列：

- DB2KeyGenerator
- H2KeyGenerator
- KingbaseKeyGenerator
- OracleKeyGenerator
- PostgreKeyGenerator

如果内置支持不满足你的需求，可实现IKeyGenerator接口来进行扩展。