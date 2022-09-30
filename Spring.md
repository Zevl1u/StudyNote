# IOC

## 核心概念

利用IoC和DI来实现代码充分解耦。

**IoC(Inversion of Control)**

代码书写现状：耦合度偏高

- 解决方案：使用对象时候，不要在程序中new，转换为**外部**提供
- 即对象的创建控制权由程序转移到外部

spring提供了一个容器，称为IoC容器，来充当IoC思想中的**“外部”**。IoC容器负责对象的创建、初始化等一系列工作，被创建或者被管理的对象在IoC容器中被称作**Bean**。

入门案例

1. 管理什么？Service和Dao
2. 如何将被管理对象告诉IoC容器？配置
3. 被管理对象交给IoC容器，如何获取到IoC容器？接口
4. IoC容器得到后，如何从容器中获取bean？（接口方法）

**DI(Dependency Injection)**

在容器中建立bean与bean之间依赖关系的整个过程，称为依赖注入。

入门案例

1. 基于IoC管理bean
2. Service层使用new形式创建Dao对象是否保留？否
3. Service层需要的Dao对象如何进入到Service中？提供set方法
4. Service和Dao之间关系如何描述？配置

## xml配置开发

```xml
‹bean   
id="bookDao"                                      bean的Id
name="dao bookDaoImp1 daoImpl"                    bean别名
class="com.itheima.dao.impl.BookDaoImpl"          bean类型，静态工厂类，FactoryBean类
scope="singleton"                                 控制bean的实例数量
init-method="init"
destroy-method="destory"                          生命周期销毀方法
autowire="byType"                                 自动装配类型
factory-method="getInstance"                      bean工厂方法，应用于静态工厂或实例工厂
factory-bean="com.itheima.factory.BookDaoFactory" 实例工厂bean
lazy-init="true"                                  控制bean延迟加载
/>
```

```xml
<bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
    <!--构造器注入引用类型-->
    <constructor-arg name="bookDao" ref="bookDao"/>
    <constructor-arg name="userDao" ref="userDao"/>
    <!--构造器注入简单类型-->
    <constructor-arg name="msg" value="WARN"/>
    <!--类型匹配与索引匹配-->
    <constructor-arg type="java.lang.String" index="3" value="WARN"/>
    <!--setter注入引用类型-->
    <property name="bookDao" ref="bookDao"/>
    <property name="userDao" ref="userDao"/>
    <!--setter注入简单类型-->
    <property name="msg" value="WARN"/>
    <!--setter注入集合类型-->
    <property name="names">
        <!--1ist集合-->
        <list>
            <!--集合注入简单类型-->
            <value>itcast</value>
            <!--集合注入引用类型-->
            <ref bean="dataSource"/>
        </list>
    </property>
</bean>
```

**实例化bean三种方法**

1. 无参构造方法

2. 使用静态工厂类获得（了解）

   1. 配置工厂类
   2. 配置factory-method

3. 实例工厂获得：配置factory-bean和factory-method属性（了解）

   实际中则是使用FactoryBean：创建一个类实现Factory<T\>接口，然后直接配置工厂bean对象直接获取bean

依赖注入方式选择

1. 强制依赖使用构造器进行 ，使用setter注入有概率不进行注入导致nu11对象出现
2. 可选依赖使用setter注入进行，灵活性强
3. Spring框架倡导使用构造器，第三方框架内部大多数采用构造器注入的形式进行数据初始化，相对严谨
4. 如果有必要可以两者同时使用，使用构造器注入完成强制依赖的注入，使用setter注入完成可选依赖的注入
5. 实际开发过程中还要根据实际情况分析 ，如果受控对象没有提供setter方法就必须使用构造器注入
6.  自己开发的模块推荐使用setter注入

依赖自动装配特征

- 自动装配用于引用类型依赖注入，不能对简单类型进行操作
- 使用按类型装配时（byType） 必须保障容器中相同类型的bean唯一，推荐使用
- 使用按名称装配时（byName）必须保障容器中具有指定名称的bean，因变量名与配置轉合 ，不推荐使用
- 自动装配优先级低于setter注入与构造器注入，同时出现时自动装配配置失效

加载properties文件

- 不加载系统属性：<context: property-placeholder location="jdbc.properties"system-properties-mode="NEVER"/>
- 加载多个properties文件‹context: property-placeholder location="jdbc.properties, msg.properties"/>
- 加载所有properties文件‹context: property-placeholder location="*.properties"/>
- 加载properties文件标准格式‹context: property-placeholder location="classpath:*. properties" />
- 从类路径或jar包中搜索并加载properties文件：<context: property-placeholder location="classpath*:*. properties" />

## 注解开发

```xml
  <!--打开spring注解开发模式-->
  <context:component-scan base-package="daoModule,serviceModule,webModule"/>
```

然后在对应bean上打上`@Component("beanid")`注解即可。对于不同的层，可以使用不同的注解，效果和`@Component("beanid")`完全一致。

1. Dao层：`@Repository`
2. Service层：`@Service`
3. Controller层：`@Controller`
4. bean作用范围：在bean上添加注解`@Scope`
5. bean生命周期：在对应方法上添加注解
   1. 初始化方法：`@PostConstruct`
   2. 销毁方法：`@PreDestroy`

## 全注解开发

![spring全注解开发](C:\Users\Zev\OneDrive\文档\Spring全注解开发.jpg)

![](C:\Users\Zev\OneDrive\文档\Spring全注解开发2.jpg)

### 注解依赖注入

![](C:\Users\Zev\OneDrive\文档\Spring依赖注入.jpg)

![](C:\Users\Zev\OneDrive\文档\Spring依赖注入2.jpg)

### 加载properties文件

![](C:\Users\Zev\OneDrive\文档\注解加载prop.jpg)

<img src="C:\Users\Zev\OneDrive\文档\注解加载prop2.jpg" style="zoom:50%;" />

### 注解加载第三方bean

<img src="C:\Users\Zev\OneDrive\文档\配置第三方bean.jpg"/>



### 第三方bean依赖注入

![](C:\Users\Zev\OneDrive\文档\配置第三方bean依赖注入.jpg)

![配置第三方bean依赖注入](C:\Users\Zev\OneDrive\文档\配置第三方bean依赖注入2.jpg)

### 两种开发模式对比

![](C:\Users\Zev\OneDrive\文档\xml和注解开发对比.jpg)

## 整合Mybatis

1. 导入对应包（spring-jdbc，spring-mybatis）
2. 创建配置主类，加上@Configuration表明是配置类；加上@ComponentScan("packagename")表明要扫描的bean所在包；@PropertySource("classpath:conf.properties")表明导入外部配置文件
3. 创建数据源配置类
   1. 在成员变量上配置数据源连接要用的驱动/url/用户名/密码等
   2. 然后用一个public方法返回一个数据源，在这个方法上添加@Bean表明将该方法返回值作为一个bean交给IoC容器处理（其实可以直接将该方法写道配置主类里，只是这样会许多bean写道一起，不方便管理）
   3. 然后还要将这个类导入到配置主类里（即在上一步那个类上面加上@Import(该数据源配置类.class)
4. 创建mybatis-spring配置类
   1. 用一个public方法返回一个SqlSessionFactoryBean对象，调用其setTypeAliasesPackage()方法设置要对应的POJO类所在包名；调用其setDataSource方法设置数据源，需要的数据源直接写在这个方法的参数上，IOC容器会自动装配上一步设置的数据源
   2. 用一个public方法返回一个MapperScannerConfigurer对象，在内部设置调用其setBasePackage方法设置其要扫描的包，用来自动创建代理对象完成数据库操作

整合junit

<img src="C:\Users\Zev\OneDrive\文档\spring整合junit.jpg" style="zoom:50%;" />

# AOP

Aspect Oriented Programming：面向切面编程，一种编程范式，知道开发者如何组织程序结构。

作用：在不改动原始设计的基础上为其进行功能增强

![](C:\Users\Zev\OneDrive\文档\aop基本概念.jpg)

## 快速入门

大部分东西都不需要改变，只需要

1. 导入对应pom坐标

2. 编写通知类实现绑定

   <img src="C:\Users\Zev\OneDrive\文档\aop快速入门.jpg" style="zoom:50%;" />

3. 在配置类上加上@EnableAspectJAutoProxy告诉spring使用注解方式aop

## AOP工作流程

1. Spring容器启动
2. 读取所有切面配置中的切入点
3. 了．初始化bean，判定bean对应的类中的方法是否匹配到任意切入点
   1. 匹配失败，创建对象
   2. 匹配成功 ，创建原始对象（目标对象）的代理对象
4. 获取bean执行方法
   1. 获取bean ，调用方法并执行 ，完成操作
   2. 获取的bean是代理对象时 ，根据代理对象的运行模式运行原始方法与增强的内容，完成操作

# 事务

