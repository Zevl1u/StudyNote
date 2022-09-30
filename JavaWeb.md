# MySQL安装与配置

参考：[安装与配置](https://blog.csdn.net/weixin_43605266/article/details/110477391)

## 安装与配置

1. 下载对应压缩包解压

2. 添加安装目录的bin路径到环境变量

3. 在安装目录根目录下新建文件`my.ini`，写入以下配置

   ```ini
   [mysql]
   # 设置mysql客户端默认字符集
   default-character-set=utf8
   [mysqld]
   # 设置3306端口
   port = 3306
   # 设置mysql的安装目录
   basedir = D:\\Dev\\mysql-8.0.29-winx64
   # 设置mysql数据库的数据的存放目录
   datadir = D:\\Dev\\mysql-8.0.29-winx64\\data
   # 允许最大连接数
   max_connections=20
   # 服务端使用的字符集默认为8比特编码的latin1字符集
   character-set-server=utf8
   # 创建新表时将使用的默认存储引擎
   default-storage-engine=INNODB
   # 创建模式
   sql_mode = NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLE
   ```

4. 命令行输入`mysqld --initialize`进行初始化，完成后会在根目录下出现一个名为data的文件夹，里面有一个`.err`后缀名的文件，里面存有管理员用户名和初始的密码

5. 命令行输入`mysqld --install`安装服务

6. 命令行输入`net start mysql`开启服务

7. 使用第4步中的用户名密码登录`mysql -uroot -pqCuXdTJsa9:m`

8. 登录成功后修改密码，`alter user 'root'@'localhost' identified with mysql_native_password by 'new password';`

**mysql服务的启动与关闭**

1. 运行`services.msc`
2. 找到mysql即可设置是否开机自动启动
3. mysql服务启动：`net start mysql`；关闭：`net stop mysql`

## 卸载

1. 命令行输入`net stop mysql`
2. 命令行输入`mysqld -remove mysql`
3. 删除相关环境变量

# SQL基本概念

MySQL是关系型数据库，建立在关系模型上的数据库。可以简单理解为多张相互连接的二维表组成的数据库。

- 由数据库，表，表数据一级一级构成
- sql语句可以单行或者多行书写，以分号结尾i
- 不区分大小写，建议大写
- 注释格式
  - `-- `注意要加个空格
  - `#`
  - `/**/`多行注释

## SQL分类

### DDL(Data Definition Language) 

数据定义语言，用于定义数据库对象：数据库，表，列等

- 操作数据库

  - `SHOW DATABASES`

  - `CREATE DATABASE IF NOT EXISTS dbname`

  - `DROP DATABASE IF EXISTS dbname`

  - `USE dbname`

  - `SELECT DATABASE()`：当前使用的数据库

- 操作表

  - Create, Retrieve, Update, Delete

  - `SHOW TABLES`

  - `DESC 表名`：查看表结构

  - ```sql
    CREATE TABLE tablename(
    	字段1 数据类型1,
        ...
    );
    ```

    最后一行末尾不加逗号

  - 修改表名：`ALTER TABLE 旧表名 RENAME TO 新表名`

  - 添加一列：`ALTER TABLE 表名ADD 列名 数据类型`

  - 修改数据类型：`ALTER TABLE tablename MODIFY 列名 新数据类型`

  - 修改列名和数据类型：`ALTER TABLE 表名 CHANGE 旧列名 新列名 新数据类型`

  - 删除列：`ALTER TABLE 表名 DROP 列名`

### DML(Data Manipulation Language)

对数据**表**中的数据进行**增删改**。

- 添加数据

  - 给指定列添加数据：`INSERT INTO 表名(列名1, 列名2 ,...) VALUES(值1, 值2, ...)`

  - 给全部列添加数据：`INSERT INTO 表名 VALUES(值1, 值2, ...)`

  - 批量添加数据
    - `INSERT INTO 表名(列名1, 列名2,...) VALUES(值1, 值2, ...), (值1, 值2, ...), ...`
    - `INSERT INTO 表名 VALUES(值1, 值2, ...), (值1, 值2, ...), ...`

- 修改表数据：`UPDATE 表名 SET 列名1=值1, 列名2=值2, ... [WHERE 条件]`

- 删除表数据：`DELETE FROM 表名 [WHERE 条件]`

### DQL(Data Query Language)

查询数据表数据：`SELECT 字段名 FROM 表名`

### DCL(Data Control language)

定义数据库访问权限，安全级别和创建用户等

- 语法

  ```sql
  SELECT
  	字段列表
  FROM
  	表名列表
  WHERE
  	条件列表
  GROUP BY
  	分组字段
  HAVING
  	分组后条件
  ORDER BY
  	排序字段
  LIMIT
  	分页限定
  ```

- DISTINCT关键字可以去除重复记录：`SELECT DISTINCT 字段1 FROM 表名`
- 使用AS起别名：`SELECT 字段1 AS 别名1 FROM 表名`，可省略

- 条件查询

  - <, >,=, !=, 

  - <>也是不等于

  - AND, OR 

  - BETWEEN a AND b

  - IN (值1, 值2, ...)：在这些值之中

  - 与NULL比较用IS NULL和IS NOT NULL

  - LIKE：模糊查询。_代表任意单个字符，%代表任意多个字符：`SELECT * FROM 表名 WHERE 字段名 LIKE "_a%"`

- 排序查询

  `SELECT 字段列表 FROM 表名 ORDER BY 排序字段名1 [排序方式1], 排序字段名2 [排序方式2], ...`

  - ASC：升序排序，默认

  - DESC：降序

  - 如果有多个排序条件，当前面条件值一样时，才会根据第二条件进行排序

- 聚合函数

  - 将一列数据作为一个整体，进行纵向计算
  - 聚合函数分类
    - COUNT(列名)：统计数量，一般选用不为null的列，一般写主键或者*号
    - MAX()
    - MIN()
    - SUM()
    - AVG()

  - `SELECT 聚合函数名(列名) FROM 表名`

  - 注意，NULL值不参与聚合函数计算

- 分组查询

  `SELECT 字段列表 FROM 表名 [WHERE 分组前限定条件] GROUP BY 分组字段名 [分组后条件过滤]`

  注意，分组之后查询的**字段列表**一般是是聚合函数和分组字段，其他字段无意义。

  WHERE和HAVING区别：

  - 执行时机不一样：WHERE是分组之前进行限定，HAVING是对分组之后数据进行过滤

  - 可判断的条件不一样：WHERE不能对聚合函数进行判断，HAVING可以。这是因为执行顺序是：WHERE -> 聚合函数 -> HAVING

- 分页查询

  `SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询条目数`

  - 起始索引：0

  - 计算公式：起始索引 = （当前页码-1） * 每页显示条数

## 数据类型

- 数值
  - 从TINYINT到BIGINT分别是不同范围（字节长度）的整数
  - FLOAT, DOUBLE，用的时候可以指定格式：DOUBLE(总位数，小数点后位数)
- 日期
  - DATE
  - DATETIME
- 字符串
  - CHAR()：如果达不到声明长度，会用空格补齐，浪费空间但性能高
  - VARCHAR()：声明最大长度，用多少有多少

## 约束

### 概念

- 作用域表中列上的规则，用于限制加入表的数据
- 约束的存在保证数据库中数据的正确性，有效性和完整性

### 分类

- NOT NULL：非空约束
- UNIQUE：唯一约束
- PRIMARY KEY：主键约束
- DEFAULT：默认约束，保存数据时，未指定时采用默认值
- FOREIGN KEY：外键约束，用来让两个表数据之间建立连接，保证数据一致性和完整性

### 示例

```SQL
-- 建表时添加约束
CREATE TABLE employee(
	id INT PRIMARY KEY AUTO_INCREMENT,-- 不连续情况从最后一套数据的值自增
	ename VARCHAR(50) NOT NULL UNIQUE,
	joindate DATE NOT NULL,
	salary DOUBLE(7,2) NOT NULL DEFAULT 5000.00
	...
)
```

```SQL
--建表后添加
ALTER TABLE 表名 MODIFY 字段名 数据类型 NOT NULL;
```

```sql
--删除约束
ALTER TABLE 表名 MODIFY 字段名 数据类型；
```

外键示例

```sql
-- 添加外键约束
CREATE TABLE employee(
	id INT PRIMARY KEY AUTO_INCREMENT,
	ename VARCHAR(50) NOT NULL UNIQUE,
	department_id TINYINT，
	...
    -- 添加外键约束，将部门id和部门表关联
    CONSTRAINT fk_emp_dept FOREIGN KEY(department_id) REFERENCES department(id)
)
```

此时department是主表，employee是从表。先要创建主表之后再创建从表。

```
-- 建表之后添加外键
ALTER TABLE employee ADD CONSTRAINT fk_emp_dept FOREIGN KEY(department_id) REFERENCES department(id);
-- 删除外键
ALTER TABLE employee DROP FOREIGN KEY fk_emp_dept;
```

## 数据库设计

表之间的关系

- 一对多
  - 如员工表和部门表
  - 在多的一方建立外键，指向一的那方的主键
- 多对多
  - 如商品和订单
  - 建立第三方中间表，中间表至少包含两个外键，分别关联两方主键
- 一对一
  - 如用户和用户详情
  - 用于表拆分，将常用字段放一张表，提升检索速度
  - 在任意一方加入外键，关联另一边主键，并设置外键唯一约束

## 多表查询

### 连接查询

内连接：查询A,B两表交集

- 隐式内连接：`SELECT 字段列表 FROM 表1, 表2 ... WHERE 条件;`
  - SELECT A.id, B.name FROM ...
- 显式内连接：`SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 条件`

外连接

- 左外连接：相当于查询A表所有数据和交集部分数据

  `SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件;`

- 右外连接：相当于查询B表所有数据和交集部分数据

  `SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件;`

### 子查询

查询里面嵌套查询，就是在WHERE后面条件换成一条查询。

根据查询结果

- 单行单列：`SELECT 字段列表 FROM 表名 WHERE 字段名 = (子查询)`
- 多行单列：`SELECT 字段列表 FROM 表名 WHERE 字段名in (子查询)`
- 多行多列：`SELECT 字段列表 FROM (子查询) WHERE 条件`

## 事务

- 数据库的事务(Transaction)是一种机制，一个操作序列，包含了一组数据库操作命令
- 事务把所有命令作为一个整体一起向系统提交或者撤销操作请求，即一组命令要么同时成功，要么同时失败
- 事务是一个不可分割的工作逻辑单元

```
-- 开启事务
START TRANSACTION; 或者 BEGIN;

-- 提交事务
COMMIT;
-- 回滚事务
ROLLBACK
```

事务的四大特征

- 原子性(Atomicity)：事务是不可分割的最小单位，要么同时成功，要么同时失败
- 一致性(Consistency)：事务完成时，必须使所有数据都保持一致状态
- 隔离性(Isolation)：多个事务之间，操作的可见性
- 持久性(Durability)：事务一旦提交或者回滚，它对数据库中数据的改变就是永久的

# JDBC

全称Java DataBase Connectivity，一组使用Java代码操作关系型数据库的接口规范。不同的数据库有对应不同的实现类。

## 快速入门

1. 创建工程，导入驱动jar包
2. 注册驱动：`Class.forName("com.mysql.jdbc.Driver")`
3. 获取连接：`Connection conn = DriverManager.getConnection(url, username, password)`
4. 定义SQL语句：`String sql = "update ..."`
5. 获取执行SQL对象：`Statement stmt = conn.createStatement()`
6. 执行SQL：`int n = stmt.executeUpdate(sql)`，n是受影响行数
7. 处理返回结果：
8. 释放资源：`stmt.close(); conn.close();`

## API详解

**DriverManager**

1. 注册驱动

   在执行`Class.forName("com.mysql.jdbc.Driver")`时候，将这个类加载到内存，类中有个静态代码块会自动执行`DriverManager.registerDriver()`注册驱动。

   在版本5之后的驱动包可以省略`Class.forName("com.mysql.jdbc.Driver")`，原因是可以自动加载jar包中一个文件信息自动配置。

2. 获取数据库连接

   url指连接路径，语法：`jdbc:mysql://ip:port/dbname?key1=value1&key2=value2&...`

**Connection**

1. 获取执行SQL的对象

   1. 普通执行SQL对象：`Statement createStatement()`

   2. 预编译SQL的执行SQL对象：`PreparedStatement prepareStatement(sql)`

      防止SQL注入

   3. 执行存储过程的对象：`CallableStatement prepareCall(sql)`

2. 管理事务：定义了三个发方法

   1. 开启事务：`setAutoCommit(boolean autoCommit)`

      ture为自动提交事务；false为手动提交事务，相当于开启事务

   2. 提交事务：`commit()`

   3. 回滚事务：`rollback()`，在catch中执行回滚

**Statement**

执行SQL语句

1. 执行DML/DDL语句：`int executeUpdate(sql)`

   - DML语句返回影响的行数
   - DDL执行成功也可能返回0

2. 执行DQL语句：`ResultSet executeQuery(sql)`

   ResultSet返回结果集对象。可通过`boolean next()`获取数据。它将光标往前移动一行，若有数据返回ture，否则false。然后通过`getXxx()`获取数据。

**PreparedStatement**

继承自Statement，预编译SQL语句并执行，可以预防SQL注入等问题。同时还加入了预编译功能，需要在连接数据库时候在末尾键值对加上`useServerPrepStmts=true`才能开启。

```java
String sql = "SELECT * FROM user WHERE username = ? AND password = ?";
PreparedStatement ps = conn.prepareStatement(sql);
ps.setXxx(index, value); // index是?位置，从1开始；value是对应值
ps.executeUpdate();/ps.executeQuery();
```

底层其实就是将注入相关的一些字符（如'等）加了\进行转义。

可以通过配置MySQL执行日志（需要重启生效）来查看是否有预编译效果及转义。

### 数据库连接池

类似线程池，提升资源利用率和效率。常见有C3P0、Druid（样例采用）。

基本流程

1. 导入jar包

2. 新建配置文件，写键值对

3. 加载配置文件：`Properties prop = new Properties(); prop.load(new FileInputStream(src/a.properties));`

   不确定怎么写文件路径时候可以用`System.getProperty("user.dir")`获取当前路径。

4. 获取连接池对象：`DataSource ds = DruidDataSourceFactory.createDataSource(prop);`

5. 获取数据库连接：`ds.getConnection();`

# Maven基础

专门用于管理和关键Java项目的工具，主要功能有

1. 提供了一套标准化的项目结构：所有IDE使用Maven构建的项目结构完全一样
2. 提供了一套标准化的构建流程（编译，测试，打包，发布...）
3. 提供了一套以来管理机制

## 安装与配置

1. 解压压缩包

2. 配置环境变量`%MAVEN_HOME%`，并且将`%MAVEN_HOME%\bin`添加到path

3. 配置本地仓库：修改`conf/settings.xml`中的`<localRepository>`为一个指定目录

4. 配置阿里云私服：修改`conf/settings.xml`中的`mirrors`标签，为其添加如下子标签

   ```xml
   <mirror>
   	<id>nexus-aliyun</id>
   	<mirrorOf>central</mirrorOf>
   	<name>Nexus aliyun</name>
   	<url>http://maven.aliyun.com/nexus/content/groups/public</url>
   </mirror>
   ```

## 常用命令

以maven构建项目的目录结构，在pom.xml所在目录，命令行执行

- compile：编译

  在pom.xml所在目录，命令行执行`mvn compile`即可编译

- clean：清理

- test：测试，执行test包下的测试类

- package：打包

- install：安装

## 生命周期

- Maven构建项目生命周期描述的是一次构建过程经历了多少事件

- Maven对项目构建声明周期划分为3套

  - clean：清理工作
  - default：核心工作，例如编译，测试，打包，安装等
  - site：产生报告，发布站点等

  在同一生命周期内，执行后面的命令，前面的所有命令会自动执行。如`mvn install`会自动执行`mvn compile` -> `mvn test` -> `mvn package`。

## Maven坐标

- 什么是坐标？
  - 资源的唯一标识
  - 使用坐标来定义项目或引入项目中需要的依赖
- 坐标组成
  - groupId：定义当前Maven项目隶属的组织名称，通常是域名反写
  - artifactId：定义当前Maven项目名称，通常是模块名称
  - version：定义当前项目版本号

## IDEA配置Maven

1. 在settings里面搜索maven，将自带的Maven home path修改成自己安装Maven的目录，修改User settings file为自己的settings.xml
2. 新建module，构建方式选择maven即可

导入maven项目

1. 选择右侧Maven面板，点击+号
2. 选中对应项目的pom.xml文件，双击加载
3. 如果没有Maven面板，选择View -> Apperance -> Tool Window Bars

## 其他细节

- 在pom.xml中按alt+insert可以面板导入依赖

- 可以在坐标中使用`<scope>test</scope>`设置依赖范围，默认compile

  | 依赖范围 | 编译classpath | 测试classpath | 运行classpath |       例子        |
  | :------: | :-----------: | :-----------: | :-----------: | :---------------: |
  | compile  |       Y       |       Y       |       Y       |      logback      |
  |   test   |       -       |       Y       |       -       |       JUnit       |
  | provided |       Y       |       Y       |       -       |    servlet-api    |
  | runtime  |       -       |       Y       |       Y       |     jdbc驱动      |
  |  system  |       Y       |       Y       |       -       | 存储在本地的jar包 |


# Mybatis基础

## 简介

一个持久化框架，用于简化JDBC编码。JDBC有以下缺点：

1. 硬编码：每次有改动都要改源码，然后要重新编译部署
2. 操作繁琐

## 快速入门

1. 创建user表，添加数据
2. 创建模块，导入坐标
3. 编写 MyBatis 核心配置文件：替换连接信息 解决硬编码问题
4. 编写 SQL映射文件：统一管理sql语句，解決硬编码问题
5. 编码
   1. 定义POJO类
   2. 加载核心配置文件，获取 SqlSessionFactory 对象
   3. 获取 SqlSession 对象，执行 SQL 语句
   4. 释放资源

## Mapper代理开发

1. 定义与SQL映射文件同名的Mapper接口，并且将Mapper接口和SQL映射文件放置在同一目录下

2. 设置SQL映射文件的namespace属性为Mapper接口全限定名

3. 在Mapper 接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值
    类型一致

4. 编码

   1. 通过 SqlSession 的getMapper方法获取 Mapper接口的代理对象
   2. 调用对应方法完成sq的执行

   细节：如果Mapper接口名称和SQL映射文件名称相同，并在同一目录下，则可以使用包扫描的方式简
   化SQL映射文件的加载

需要出入参数的地方，在mapper.xml中

1. `#{}`：在语句中用"?"代替，然后用的是预编译的sql，推荐
2. `${}`：直接替换，无妨防止sql注入，不推荐（只有在表名列名不固定的情况下可能用到，动态查询列名）

## 核心配置文件

## 配置文件完成增删改查

### 查

1. 参数占位符：
  1. `#{}`：在语句中用"?"代替，然后用的是预编译的sql，推荐
  2. `${}`：直接替换，无妨防止sql注入，不推荐（只有在表名列名不固定的情况下可能用到，动态查询列名）

2. parameterType:用于设置参数类型，该参数可以省略
3. SQL 语句中特殊字符处理：
   1. 转义字符
   2. `<![CDATA[内容]]>` :CD提示

#### 多条件查询

1. 散装参数：使用@Param注解：`@Param("sql语句参数占位符名")`，在方法上对应参数前加上此注解，表明对应的数据库字段
2. 对象参数：需要sql语句中参数名和实体类成员属性名对应上
3. map集合参数：key值和sql语句中参数名一样

#### 动态SQL

1. if标签：`<if test="id != null">id = #{id}</if>`用于判断参数是否有值，使用test属性进行条件判断

   - 存在的问题：第一个条件不需要逻辑运算符

   - 解决方案：`<where>`标签替换 where 关键字，会自动处理语句开头的and

2. choose(when, otherwise)：类似Java的switch语句

   ```xml
   <select id="selectByConditionSingle" resultMap="brandResultMap">
       select *
       from tb brand
       <where>
           <choose>
               <when test="status != null">
                   status = #{status}
               </when>
               <when test="companyName != null and companyName !=''">
                   company_name like #(companyName}
               </when>
               <!--使用了where标签可以智能添加where，otherwise可以不用-->
               <!--<otherwise>-->
               <!--    1=1-->
               <!--</otherwise>-->
           </choose>
       </where>
   </select>
   ```

#### Mybatis事务管理

默认可能是不自动提交事务，可以手动设置`sqlSession.commit();`或者`sqlSessionFactory.openSession(true);`

### 增

```xml
<insert id="addCategory" useGeneratedKeys="true" keyProperty="categoryId">
        insert into dishcategory_tbl(category_id, category_name)
        values (#{categoryId},#{categoryName});
</insert>
```

主键返回：就是添加了`useGeneratedKeys="true"`和`keyProperty="categoryId"`两个字段，其中categoryId就是对应的成员变量的主键。此时，传入对象的主键会被赋值，可以返回

### 改

方法定义成int就能获得影响的行数。

使用set标签可以自动根据字段调整sql语句

```xml
<update id="updateDish" parameterType="dish">
        UPDATE menu
        <set>
            <if test="dishName != null">dish_name=#{dishName},</if>
            <if test="dishCategory != null">dish_categoryid=#{dishCategory.categoryId},</if>
            <if test="dishIntroduction != null">dish_introduction=#{dishIntroduction},</if>
            <if test="dishIntroduction == null">dish_introduction='超级美味',</if>
            <if test="dishPrice != null">dish_price=#{dishPrice},</if>
            <if test="dishNote != null">dish_note=#{dishNote},</if>
            <if test="dishNote == null">dish_note='DEFAULT',</if>
            <if test="dishImagePath != null">dish_imagepath=#{dishImagePath},</if>
            <if test="dishStatus != null">dish_status=#{dishStatus},</if>
        </set>
        WHERE dish_id=#{dishId}
</update>
```

### 删

```xml
<delete id="deleteCategory">
        delete from dishcategory_tbl
        where category_id in (
            <foreach collection="array" separator="," item="id">
                #{id}
            </foreach>
            )
</delete>
```

`item="id"`类似与foreach里的局部变量。

## 参数封装

### 单参数

1. POJO类型：直接使用，实体类属性名 和 参数占位符名称 一致

2. Map集合：直接使用，键名 和参数占位符名称 一致

3. Collection：封装为Map集合

   ```java
   map.put(“collection",collection集合);
   map.put("argo”,collection集合）;
   ```

4. List：封装为Map集合

   ```java
   map.put(“collection'， list集合);
   map.put(mist”,list集合）;
   map.put(”argo”, list集合);
   ```

5. Array：封装为Map集合

   ```
   map.put("array'”，数组);
   map.put("argo”，数组);
   map.put(“argo”，参数值1)：
   ```

6. 其他参数：直接使用

### 多参数

封装为Map集合

```java
map.put“'param1”参数值1);
map.put(“arg1”，参数值2)；
map.put(param2”，参数值2)：
```

Mybatis提供了ParamNameResolver类来进行参数封装。

多参数时候，会封装成map，通过键`arg0, arg1, ...`或者`param1, param2, ...`可以获取对应位置参数。当使用了`@Param`这个注解，其实就是替换了默认的arg键名（param不变）。

## 注解完成增删改查

简单的可以用，复杂的不要用。

```java
@Select("select * from dishcategory_tbl")
@ResultMap("dishCategoryMap")
List<DishCategory> findAllDishCategory();
```

# http基础

## 基本介绍

1. 基于TCP协议，面向连接，安全
2. 基于请求-响应模型：一次请求对应一次响应
3. 无状态协议，对于事务处理没有记忆能力，每次请求响应都是独立的
   - 缺点：多次请求之间无法共享数据，Java使用会话技术（cookie，session）来解决这个问题
   - 优点：速度快

## HTTP-请求数据格式

```http
GET / HTTP/1.1
Host: www.itcast.cn
Connection: keep-alive
Cache-Control: max-age=0 Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 Chrome/91.0.4472.106
...
```

```http
POST / HTTP/1.1
Host: www.itcast. cn
Connection: keep-alive
Cache-Control: max-age=0 Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 Chrome/91.0.4472.106

username=superbaby&password=123456
```

请求数据分为三部分：

1. 请求行：请求数据的第一行。其中`GET`表示请求方式，`/`表示请求资源路径(这里是根目录），`HTTP/1.1`表示协议版本
2. 请求头：第二行开始，格式为`key:value`形式。
3. 请求体：`POST`请求的最后一部分，存放请求参数（要和前面请求头隔一个空行）

常见的HTTP 请求头：

- Host：表示请求的主机名
- User-Agent： 浏览器版本，例如Chrome浏览器的标识类似Mozilla/5.0，Chrome/79， IE浏览器的标识类似Mozilla/5.0 (Windows NT ..) like Gecko;
- Accept：表示浏览器能接收的资源类型，如text/\*， image/\*或者\*/\*表示所有；
- Accept-Language：表示浏览器偏好的语言，服务器可以据此返回不同语言的网页；
- Accept-Encoding：表示浏览器可以支持的压缩类型，例如gzip, deflate等。

GET请求和 POST请求区别：

1. GET请求请求参数在请求行中，没有请求体。POST请求请求参数在请求体中
2. GET请求请求参数大小有限制（浏览器url长度有限制），POST没有

## HTTP-响应数据格式

```http
HTTP/1.1 200 OK
Server: Tengine
Content-Type: text/html
Transfer-Encoding: chunked...

<html>
	<head>
		<title></title>
	</head>
	<body></body>
</html>
```

响应数据分为了三个部分：

1. 响应行：响应数据的第一行。其中`HTTP/1.1`表示协议版本，`200`表示响应状态码，`OK`表示状态码描述
2. 响应头：第二行开始，格式为`key:value`形式
3. 响应体：最后一部分。存放响应数据

常见的HTTP 响应头：

- Content-Type：表示该响应内容的类型，例如text/html，image/jpeg；
- Content-Length：表示该响应内容的长度（字节数）；
- Content-Encoding：表示该响应压缩算法，例如gzip;
- Cache-Control：指示客户端应如何缓存，例如max-age=300表示可以最多缓存300秒

HTTP状态码

| 状态码分类 | 说明                                                         |
| :--------: | :----------------------------------------------------------- |
|    1XX     | 响应中，临时状态码，表示请求已接受，告诉客户端应该继续请求或者如果已完成则忽略 |
|    2XX     | 成功                                                         |
|    3XX     | 重定向                                                       |
|    4XX     | 客户端错误，处理发生错误，问题在客户端，如：请求一个不存在的资源，客户端未被授权，禁止访问等 |
|    5XX     | 服务端错误，处理发生错误，问题在服务器端，如服务器端抛出异常，路由出错，http版本不支持等 |

# Tomcat

## 简介

- web服务器是一个应用程序（软件），**对 HTTP协议的操作进行封装**，使得程序员不必直接对协议进行操作，让web开发更加便捷。主要功能是“提供网上信息浏览服务”。

- Tomcat是Apache 软件基金会一个核心项目，是一个开源免费的轻量级Web服务器，支持Servlet/JSP和少量JavaEE规范
- JavaEE：Java Enterprise Edition, Java企业版。指Java企业级开发的技术规范总和。包含13项技术规范：JDBC, JNDI, EJB. RMI, JSP. Servlet, XML, IMS, Java IDL, JTS, JTA, JavaMail, JAF
- Tomcat 也被称为 Web容器、Servlet容器。Servlet 需要依赖于 Tomcat才能运行
- web服务器作用
  - 封装HTTP协议，简化开发
  - 将web项目部署到服务器上，对外提供服务

## 安装及目录结构

官网下载解压即可使用。卸载直接删除整个目录即可

### 目录结构

- bin：可执行文件
- conf：配置文件目录
- lib：tomcat依赖的jar包
- logs：日志文件目录
- temp：临时文件目录，tomcat运行过程中产生的临时文件
- webapps：应用发布目录
- work：工作目录，tomcat运行过程中项目产生的临时文件

### 运行

- 启动：bin\startup.bat
  - 如果有控制台中文乱码，修改`conf/logging.properties`下的`java.util.logging.ConsoleHandler.encoding = GBK`
- 关闭
  - 直接x掉运行窗口：强制关闭
  - `bin/shundown.bat`正常关闭
  - ctrl + c：正常关闭

### 配置

- 修改启动端口号：`conf\server.xml`   HTTP协议默认的端口号为80，如果将tomcat默认端口号从8080改成80，则将来访问tomcat时候不用输入端口号，默认就是80
- 启动时可能出现的问题
  - 端口号冲突：Address already in use：找到对应程序，将其关闭（或者修改tomcat启动端口号应该也行）
  - 启动窗口一闪而过：检查JAVA_HOME环境变量配置是否正确

## 部署项目

- 将项目放到webapps目录下即部署完成
- 一般JavaWeb项目会被打包成war包，然后将war包放到webapps目录下，tomcat会自动解压war包

## 在IDEA中创建Maven Web项目

- 和maven java项目区别就是main目录下多一个webapp目录

  ```
  webapp
  	html // 可自命名
  	WEB-INF // web核心目录，必须叫这个名字
  		web-xml // web配置文件，必须有
  ```

- 打包成war包后，解压后目录就是webapp下的目录，但是会将相关jar包（项目依赖）和字节码文件（项目源代码）放到WEB-INF目录下

# servlet

是JavaEE规范之一，其实就是一个接口，由我们定义实现类，然后使用web服务器运行。

## 快速入门

1. 导入servlet坐标
2. 定义一个类，实现servlet接口，在service方法中输入一句控制台打印
3. 在类上使用`@WebServlet`注解配置该servlet的访问路径
4. 启动tomcat，浏览器输入路径访问

## 流程及生命周期

由tomcat创建，然后调用里面的service方法。

生命周期

1. 加载和实例化：默认情况下，servlet会在第一次被访问的时候创建（可以在注解里使用loadOnStartUp改变，负整数是默认，0或正整数是在容器创建时候就创建对象）
2. 初始化：在servlet实例化之后，容器将调用servlet的init()方法初始化对象，完成如加载配置文件，创建连接等工作。init()方法只会被调用一次
3. 请求处理：每次请求servlet时候，容器都会调用其service方法进行处理
4. 服务终止：当需要释放内存或者容器关闭时，容器就会调用servlet对象的destory()方法完成资源释放。然后就被GC回收

### 原理

为了方便不同请求方式对应不同的处理方法，在service方法中根据不同请求方式调用不同的方法，所以定义了新的抽象方法（doGet/doPost）在service中调用，然后直接继承实现抽象方法即可（即HttpServlet）

### urlPattern

- servlet要被访问。一定要配置其urlPattern
- 一个servlet可以配置多个urlPattern
- urlPattern匹配规则
  - 精确匹配："/user/select"
  - 目录匹配："/user/*"
  - 扩展名匹配："*.do"
  - 任意匹配："/"or "/*"     不要配置"/"这种路径，会覆盖掉tomcat的DefaultServlet
  - 优先级从上到下递减

### 重定向和请求转发的区别

- 重定向(redirect)：一种资源跳转方式
  - url发生变化
  - 可以重定向到任意资源，内部外部都可
  - 两次请求，无法在多个资源间使用request共享数据
- 请求转发(forward)：一种在服务器内部的资源跳转方式
  - 浏览器路径不发生变化
  - 只能跳转到当前服务器内部资源
  - 一次请求，可以在转发资源间使用request共享数据

# 会话session

- 浏览器与服务器建立一次会话可以实现多次请求和响应
- 会话跟踪：一种维护浏览器状态的方法，服务器需要识别多次请求是否来自同一个浏览器，以便在同义词会话中的多次请求间共享数据
- 实现方式
  - 客户端会话跟踪技术：cookie
  - 服务器端会话跟踪技术：session

Cookie 和 Session 都是来完成一次会话内多次请求间数据共享的。
区别：

- 存储位置：Cookie 是将数据存储在客户端，Session 将数据存储在服务端
- 安全性：Cookie 不安全，Session 安全
- 数据大小：Cookie 最大3KB， Session 无大小限制
- 存储时间：Cookie 可以长期存储，Session 默认 30分钟
- 服务器性能：Cookie 不占服务器资源，Session 占用服务器资源

# Filter

- Fiter 表示过滤器，是JavaWeb 三大组件(Servlet、Filter、Listener)之一。
- 过滤器可以把对资源的请求拦截下来，从而实现一些特殊的功能。
- 过滤器一般完成一些通用的操作，比如：权限控制(判断用户是否已经登录)、统一编码处理、敏感字符处理等等

## 快速入门

1. 定义类，实现 Filter接口，并重写其所有方法

  ```java
  public class FilterDemo implements Filter {
  	public void init(FilterConfig filterConfig)
  	public void doFilter (ServletRequest request)
  	public void destroy() {}
  }
  ```

2. 配置Filter拦截资源的路径：在类上定义 @WebFilter 注解

  ```java
  @WebFilter("/*")
  public class FilterDemo implements Filter {}
  ```

3. 在doFilter方法中输出一句话，并放行

  ```java
  public void doFilter (ServletRequest request, ServletResponse, FilterChain chain){
  	System .out.printin("filter 被执行了...")；
  	...
  	chain.doFilter(request, response) ;
  }
  ```

放行前后都可以执行语句。一般在放行前对request进行过滤，放行后对response进行过滤。

可以配置多个过滤器实现过滤器链，在xml里可以配置顺序，若未配置则按类名字典顺序。

判断用户是否已经登录：

1. 判断是否访问登录相关资源
   1. 是：放行
   2. 否：进行登录验证
2. 判断用户是否已登录：session是否有user对象
   1. 已登录：直接放行
   2. 未登录：跳转到登录页面

# AJAX

- Asynchronous JavaScript And XML：异步的JavaScript和XML
- 作用：与服务器进行数据交换，通过AJAX可以给服务器发送请求，并获取服务器响应的数据
  - 即使用HTML+AJAX替换JSP页面
