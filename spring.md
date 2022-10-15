## Spring 重学

### 1. Spring概念

> 1. spring 轻量级的开源的JavaEE框架。
> 2. 可以解决企业应用开发的复杂性
> 3. 两个核心部分 ：IOC和Aop
> > 1. IOC：控制反转，把创建对象的过程交给Spring容器进行管理
> > 2. Aop：面向切面，不修改源代码，进行功能增强
> 4. 特点：
>> 1. 方便解耦，简化开发
>> 2. Aop编程支持
>> 3. 方便程序测试
>> 4. 方便和其他框架进行整合
>> 5. 方便进行事物操作
>> 6. 降低APi开发难度

5. <font color=red>Spring5 学习</font>

# Spring Core Container

> ## Beans === Core === Context === Expression

<img src="./imgs/spring结构图.png" alt="Spring结构图">

# 如何使用Spring管理对象的步骤

> 1. 加载4个（beans、core、context、expression）核心jar包和一个commons-logging公共jar包
> 2. 创建java类----》》 创建Spring的配置文件（xml格式），在配置文件配置需要创建的对象（`<bean id="" class=""/>`）。
> 3. 加载配置文件----》》获取配置创建的对象----》》得到需要的对象

### 2. IOC

#### 1. IOC底层原理

> 什么是IOC？
> 控制反转，把对象的创建和对象之间的调用过程，交给Spring管理
> 使用IOC目的，降低耦合度
> 底层原理：xml解析，技术点：工厂模式和反射
>
> > IOC过程
> > <img src="./imgs/IOC过程.png">

#### 2. IOC接口（BeanFactory）

IoC 容器实际上就是个Map（key，value）,Map 中存放的是各种对象。
> 1. IOC思想基于IOC容器完成，IOC容器底层就是对象工厂
> 2. Spring提供IOC容器实现两种方式：（两个接口）。作用即：加载配置文件，通过工厂去创建对象
>
> > 2.1. BeanFactory：IOC容器基本实现，是Spring内部实现，一般不面向开发人员使用
> > 在加载配置文件的时候不会创建对象，在使用的时候才去创建对象
> > 2.2. ApplicationContext：BeanFactory接口的子接口，提供更多更强大的功能，工开发人员使用
> > 在加载配置文件的时候就进行创建对象
>
> 3. ApplicationContext主要实现类（2个）
>
> > FileSystemXmlApplicationContext 路径需要完整的带盘符的路径（c:...）
> > ClassPathXmlApplicationContext 项目路径（com....）

### 什么是Bean管理：指两个操作

> 1. Spring创建对象
> 2. Spring注入属性：给属性赋值

#### 3. IOC操作Bean管理（基于xml）

> 1. xml方式创建对象
>
> > `<bean id="User" class="com.xxx.xxx.User"></bean>`
> > 创建对象的时候，默认执行无参构造，完成对象创建
>
> 2. xml方式注入属性
>
> > DI：依赖注入，就是注入属性
> >
> > 1. 使用set方法注入
> >
> > > bean标签中使用property标签
> >
> > 2. 使用有参构造进行注入
> >
> > > bean标签中使用constructor-arg标签
> > 3. 注入属性，外部Bean：一个类中调用另一个类中方法
> > > 当属性为对象时，用ref属性，值为对象的id值
> > 4. 注入属性-内部bean和级联赋值
> > 5. 注入集合属性
       > > 注入数组类型
       > > List
       > > Map
       > > Spring中有两种类型Bean FactoryBean
> > > 普通Bean，xml定义的类型就是返回类型
> > > 工厂Bean， xml中定义bean类型可以和返回类型不一样，，定义时需要实现FactoryBean<T> 接口
> > > DI是IOC中一种具体实现，它就表示注入属性，要在创建对象的基础之上完成
> Bean的作用域：Spring里面设置bean实例是单实例还是多实例，默认是单实例对象
> 用scope属性设置作用域 :默认：singleton：单 ====prototype：多
> 多，在调用，getBean 的时候创建多实例对象
> 单，在加载时创建
> Bean的声明周期:对象创建到销毁
> > 1. 通过构造器创建bean实例（无参）
> > 2. 为bean的属性设置值和对其他bean的引用
> > 3. 调用bean的初始化方法
> > 4. bean可以使用
> > 5. 当容器关闭的时候，调用bean销毁的方法
       > 后置处理器BeanPostProcessor接口，有初始化（3）前后的两个方法，需要配置后置处理器。
       > xml方式的自动装配:根据指定装配规则（属性名称或者属性类型，Spring自动将匹配的属性值进行注入）
> > 1. 自动装配过程
> > 2. bean标签中的autowire属性，byName和byType（相同类型的bean不能定义多个）两种方式注入
       IOC操作Bean管理（外部属性文件）
       > > 使用yml或properties配置文件中的内容动态配置
> >

#### 4. IOC操作Bean管理（基于注解（@...））

1. 注解：

> 是代码特殊标记，格式：@注解名称（属性名称=属性值，属性名称=属性值...）
> 使用注解，注解作用在类上面，方法、属性上面
> 使用注解的目的，简化xml配置

2. Spring针对Bean管理中创建对象提供注解（）

> @Component
> @Service
> @Controller
> @Repository

* 上面四个注解功能是一样的，都可以用来创建bean实例

3. 基于注解方式实现对象创建

> 引入AOP依赖
> 开启组件扫描
> > xml中引入命名空间，schema/context 开启组件扫描 `<context:component-san base-package=""></context>`
> > 多个类用逗号隔开，或者类上级目录，扫描该包中所有类

4. 开启组件扫描中的一些细节配置

> use-default-filters=false 自定义过滤器
> > `<context:include-filter 和 exclude-filter type="annotation" expression="包名"`

5. 基于注解方式实现属性注入
   @AutoWired ：根据属性类型进行自动装配
   @Qualifier ：根据属性名称进行自动装配（注入）

> 一个接口有多个实现类，想要根据接口类注入的话，主要指定每个实现类的具体名称，即在@xxx（value给定不同的值）
> @Resource ： 可以根据类型注入、可以根据名称注入
> @Value： 注入普通土类型属性

6. 纯注解开发

> 创建配置类（例如：SpringConfig），@Configuration：声明为创建的类为配置类 @ComponentScan ：扫描指定包路径中的注解
> > 加载配置类
> > `ApplicationContext context=new
> AnnotationConfigApplicationContext(SpringConfig.class);
> context.getBean("注解的value值",具体类对象.class)`

### 3. AOP：

> 面向切面编程，利用AOP可以对业务的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高开发效率
> 通俗描述：不通过修改源代码方式，在主干功能里面添加新功能。
> 1. 连接点
     > 类中哪些方法可以被增强，这些方法叫连接点
> 2. 切入点
     > 实际被真正增强的方法，成为切入点
> 3. 通知（增强）
     > 实际增强的逻辑部分称为通知（增强）
     > 通知有多重类型
> > 1. 前置通知
> > 2. 后置通知
> > 3. 环绕通知
> > 4. 异常通知
> > 5. 最终通知
       > 4.切面
       > 是动作：把通知应用到切入点的过程

> AOP底层原理：动态代理
> 两种情况动态代理
> 有接口
> > JDK 动态代理
> > 创建接口实现类代理对象，增强类的方法。
>
原生方式：`Proxy.newProxyIntance(JDKProxy.class.getClassLoader(),“接口数组”,new一个实现InvocationHandler接口的代理对象)`

> 没有接口
> > CGLIB动态代理
> > 创建当前类的子类的代理对象
> Spring中：框架一般都是基于AspectJ实现，AspectJ：是独立的AOP框架

切入点表达式
> 语法结构：
> > execution([权限修饰符],[返回类型],[类全路径],[方法名称],[参数列表])
> > execution(* com....方法(..)):====*：所有权限修饰符，后面有一个空格，返回值可省略不写，(..)表示参数列表

> 注解方式
> >
> 配置文件方式
> >

### 4. JdbcTemplate

Spring框架对JDBC进行分装，使用JdbcTemplate方便实现对数据库操作
使用步骤：
> 1. 引入jar包，mysql，连接池...
> 2. 配置数据库连接池，在配置文件中
> 3. 配置JdbcTemplate对象，注入dataSource，用set方法注入=========如何用注解方式注入？
> 4. 创建service类，创建，创建dao类，在dao中注入JdbcTemplate对象
> 5.

### 5. 事物管理

什么是事物？
> 数据库操作的最基本单元，逻辑上一组操作，要么都成功，如果有一个操作失败，事物中所有操作都将失败。
> 事物四大特性（ACID）：
> 1. 原子性：
> 2. 一致性：
> 3. 隔离性：
> 4. 持久性：

### 6. Spring5新特性

* jgklajl

