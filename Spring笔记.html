Spring概念
	* spring是开源的轻量级框架

	* spring核心主要两部分：

		* aop：面向切面编程，扩展功能不是修改源代码来实现
		* ioc：控制反转：对象的创建不是通过new方式实现，而是交给spring配置创建类的对象
	* spring是一站式框架：

		* spring在javaee三层结构中，每层结构都提供不同的解决方法：

			* web层：springmvc
			* service层：spring的ioc
			* dao层：spring的jdbcTemplate（与Mybatis整合之后这部分由Mybatis实现）

Spring的ioc操作
	* 把对象的创建交给spring进行管理
	* ioc操作的两种方式：

		* 配置文件方式
		* 注解方式（重点）
	* ioc底层原理：

		* 主要使用的技术

			* xml配置文件
			* dom4j解决xml
			* 工厂设计模式
			* 反射（关于Java的反射机制参考：https://blog.csdn.net/xu__cg/article/details/52877573）

				* 反射步骤：

					* 获取想要操作的类的Class对象,：通常使用Class类中的forName()静态方法; (最安全/性能最好)

                                                    Class clazz=Class.forName("User类的全路径"); (最常用)
	* 当我们获得了想要操作的类的Class对象后，可以通过Class类中的方法获取并查看该类中的方法和属性。

                                                       //获取User类的所有的方法                                                         Method[] method=clazz.getDeclaredMethods();                                                        //获取User类的所有成员属性信息                                                        Field[] field=clazz.getDeclaredFields();                                                            //获取User类的所有构造方法信息                                                        Constructor[] constructor=clazz.getDeclaredConstructors();
	* 创建对象，方法有两种：

		* 使用Class对象的newInstance()方法来创建该Class对象对应类的实例，但是这种方法要求该Class对象对应的类有默认的空构造器。

                                                                User user =(User) clazz.newInstance();
	* 先使用Class对象获取指定的Constructor对象，再调用Constructor对象的newInstance()方法来创建 Class对象对应类的实例,通过这种方法可以选定构造方法创建实例。

                                                            //获取构造方法                                                            Constructor c=clazz.getDeclaredConstructor(String.class,String.class,int.class);                                                            //创建对象并设置属性                                                            User user=(User) c.newInstance("李四","男",20);
	* 实现步骤：

		* 第一步：创建xml配置文件，配置要创建的对象类

                    <bean id="user" class="User类全路径"/>

		* 第二步：创建工厂类，使用dom4j解析配置文件+反射

			* 先使用dom4j解析配置文件，根据id值，得到id值对应的class属性值
			* 使用反射创建对象
			* 这样在servlet中就可以通过调用工厂类方法来得到对象
	* ioc入门程序：

		* 第一步：导入jar包 
		* 第二步：创建类，在类中创建方法
		* 第三步：创建spring配置文件，配置创建类

			* spring核心配置文件名称和位置不是固定的，但是建议放到src下面，官方建议叫applicationContext.xml。
			* 引入schema约束
		* 第四步：测试
	* Spring的bean管理（xml方式）

		* Bean实例化的方式：

			* 使用类的无参数构造创建（要掌握）：类中只有有参数构造方法时要手动创建无参数构造方法
			* 使用静态工厂创建
			* 使用实例工厂创建
	* Bean标签常用属性：

		* id属性：唯一标识，可以任意起，不能包含特殊符号，根据id值得到配置对象
		* class属性：创建对象所在类的全路径
		* name属性（不常用）：与id类似，但是可以使用特殊符号
		* scope属性：

			* singleton属性值：单例的，为默认值
			* prototype属性值：多例的
			* request、session、globalSession等属性不常用，他们分别表示吧对象放到相应的域中

属性注入
	* 概念：创建对象的时候，向类里面的属性设置值
	* 属性注入的方式（spring中只支持前面两种方式）：

		* 使用类中的set方法注入（用的最多）
		* 使用有参数构造函数注入
		* 使用接口注入
	* spring中的有参数构造注入属性方式：

<bean id="dog" class="demo.Dog">         <!-- 使用有参数构造 -->         <constructor-arg name="name" value="hashiqi" ></constructor-arg>         <constructor-arg name="age" value="1" ></constructor-arg>         <constructor-arg name="color" value="black"></constructor-arg>    </bean>
	* spring中的set注入属性方式（重要）：

<!--使用Set注入属性  -->     <bean id="dog" class="demo.Dog">        <property name="name" value="金毛狮犬"></property>        <property name="color" value="yellow"></property>        <property name="age" value="3"></property>     </bean>
	* spring注入对象类型属性（重点）：

		* 创建两个类，将其中一个类作为另一个类的私有变量
		* 在配置文件中创建两个类的对象
		* 在配置文件中中利用ref属性来实现，ref中的值就是配置文件中创建对象时的id标识

<!--使用Set注入类型属性  -->     <bean id="dog" class="demo.Dog">        <property name="name" value="金毛狮犬"></property>        <property name="color" value="yellow"></property>        <property name="age" value="3"></property>     </bean>         <bean id="user" class="demo.User">        <property name="nickname" value="qiqi"></property>        <property name="dog" ref="dog"></property>     </bean>
	* springP名称空间注入（了解）
	* spring注入复杂类型：

		* 数组
		* List集合
		* map集合
		* properties类型

<!-- 数组 -->        <property name="array">             <list>                 <value>1</value>                 <value>2</value>             </list>        </property>        <!-- List -->        <property name="List">             <list>                 <value>1</value>                 <value>2</value>             </list>        </property>        <!-- map -->        <property name="array">             <map>                 <entry key="" value=""></entry>             </map>        </property>        <!-- properties -->        <property name="array">             <props>                 <prop key="aa">sdasdw</prop>             </props>        </property>IOC和DI的区别
	* IOC：控制反转，把对象创建交给spring进行配置
	* DI：依赖注入，向类里面的属性中设置值
	* 两者关系：依赖注入不能单独存在，需要在IOC基础之上完成

SpringIOC操作的注解实现（重点掌握）
	* Bean管理

		* 注解：代码里面特殊标记，使用注解可以完成功能

			* 注解写法：@注解名称（属性名称=属性值）
			* 注解可以使用在类上面、方法上面。属性上面
		* 步骤：

			* 创建类，配置文件
			* 在配置文件中开启注解扫描器：到指定包里扫描类，方法，属性上的和注解

        <context:component-scan base-package="Java类型所在包路径"></context:component-scan>
	* 在类上添加注解

         @Component(value="user")//value可省略不写

		* 创建对象的四个注解：目前来讲这四个注解功能一样，主要是为了后续版本的扩展而存在

			* @Component
			* @Controller（控制层）
			* @Service（业务层）
			* @Repository（持久层）
		* 创建对象的单实例、多实例：由@Scope（value = "prototype"/"singleton"）实现
	* 注入属性:

		* 使用注解方式时不需要set方法
		* 在需要注入属性的属性上面加上注解，注解有两种

			* @Autowired
			* @Resource(name="dog")（标识为dog的类型注入，这种方式比较常用）

SpringIOC操作的注解与配置文件混合实现
	* 创建对象操作使用配置文件方式实现
	* 注入属性的操作使用注解的方式实现

AOP原理
	* AOP概述

		* AOP一般说法：面向切面（方面）编程，扩展功能不是通过修改源代码来实现。
		* AOP概念说法：AOP采取横向抽取机制，取代了传统纵向继承体系重复性代码
	* AOP原理（了解）

		* 横向抽取机制，底层使用动态代理方式实现
	* AOP操作术语

		* Joinpoint（连接点）：类里面的哪些方法可以被增强，这些方法就叫做连接点
		* Pointcut（切入点）：在类里面有很多方法可以被增强，比如在实际操作中，只是对若干个类进行增强，则这几个实际被增强的方法就称为切入点
		* Advice（通知/增强）：增强的逻辑，称为增强，比如要扩展一个日志功能的业务逻辑。则这个业务逻辑就叫做通知/增强

			* 通知分为前置通知：把业务逻辑添加在切入点（被增强的方法）之前执行
			* 后置通知：把业务逻辑添加在切入点（被增强的方法）之后执行
			* 异常通知：切入点（被增强的方法）出现异常时执行
			* 最终通知：在后置之后执行
			* 环绕通知：分别在切入点（被增强的方法）之前之后都执行
	* Aspect（切面）：把增强应用到具体的切入点上，这个过程就叫做切面
	* Intreduction（引进）：可以用动态的方式向类中加属性、方法
	* Target（目标对象）：切入点所在类就叫做Target
	* Weaving（织入）：把增强应用到目标的过程
	* Proty：一个类被AOP织入增强之后，就产生一个结果代表类

Spring里的AOP操作
	* 在spring里面要进行AOP操作，需要aspectj来实现

		* Aspectj：本身不是spring的一部分，和spring一起使用来进行AOP操作
	* 操作前准备：

		* 创建Spring核心配置文件applicationContext.xml
		* 使用表达式来配置切入点：

			* 常用表达式:

				* execution（<访问修饰符>?<返回类型><方法名>（<参数>）<异常>）

					* 一般写法：execution(* demo.User.add（..）)，*表示所有的修饰符都满足条件，（..）表示如果有参数也包含
					* 其他写法：execution(* demo.User.*（..）)，*表示所有的修饰符都满足条件，（..）表示如果有参数也包含，第二个*表示所有方法都增强

						* execution(*  *.*（..）)，所有类所有方法都增强
	* 使用Aspectj实现AOP有两种方式

		* 基于Aspectj的xml配置：

			* 步骤：

				* 第一步：在配置文件中配置增强类和被增强类的对象
				* 第二步：配置AOP操作

					* 1.配置切入点
					* 2.配置切面

<aop:config>        <!--1配置切入点                  expression表达式，id为切入点的唯一标识        -->        <aop:pointcut expression="execution(* demo.User.add(..))" id="userpointcut"/>        <!--2配置切面              method,增强类里面使用哪个方法作为前置             pointcut-ref，切面到哪个切入点        -->        <aop:aspect ref="userplus">             <aop:before method="before" pointcut-ref="userpointcut"/>        </aop:aspect>     </aop:config>
	* 注意：编写环绕方法是有如下操作

public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable{         System.out.println("..........环绕增强在这里............");                  proceedingJoinPoint.proceed();                  System.out.println("..........环绕增强在这里............");    }
	* 基于Aspectj的注解方式：

		* 步骤：

			* 创建Spring核心配置文件：
			* 在配置文件中配置增强类和被增强类的对象
			* 在配置文件中AOP操作

<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
	* 在增强类上面使用注解

@Aspectpublic class UserPlus {    public UserPlus() {         // TODO Auto-generated constructor stub    }        @Before(value="execution(* demo.User.add(..))")    public void before(){         System.out.println("..........前置增强在这里............");    }}Spring使用jdbcTemplate实现数据库crud操作
	* ccrud操作（作了解，与MYbatis整合后由Mybatis来对数据库进行操作）
	* 配置cep0连接池

		* 导入jar包
		* 创建Spring配置文件，在配置文件中配置连接池

<!--      用途1：Spring的xml配置文件中，可以通过${属性名}使用properties文件配置的值      用途2：可以使用@Value("${属性名}")注解读取properties文件配置的值，再给字段赋值             方法1：注解在字段上，给字段赋值             方法2：注解在字段的setter方法中赋值              -->    <context:property-placeholder location="jdbc.properties"/>    <!--配置c3p0连接池  -->    <bean id="dataSource"  class="com.mchange.v2.c3p0.ComboPooledDataSource">         <property name="driverClass" value="${jdbc.driver}"></property>         <property name="jdbcUrl" value="${jdbc.url}"></property>         <property name="user" value="${jdbc.username}"></property>         <property name="password" value="${jdbc.password}"></property>             </bean>Spring事务管理
	* 事务相关概念：

		* 事务的概念，数据库操作中一组不可中断的操作，要么全部执行，要么都不执行
		* 事务的特性：原子性，一致性，隔离性，持久性
		* 不考虑隔离性产生的问题
		* 解决读问题：

			* 设置隔离级别
	* Spring事务管理API：

		* Spring事务管理有两种方式：

			* 编程式事务管理（一般不用，了解就好）
			* 声明式事务管理

				* 基于xml配置文件实现
				* 基于注解方式实现
			* Spring进行事务管理的API介绍：

				* 接口（PlatformTransactionManager（事务管理器））

					* Spring针对不同的dao层框架，提供接口不同的实现类
					* 进行事务操作前要先配置事务管理器
		* 事务管理步骤：

			* 根据数据库创建对应类，完成注入关系
			* 在配置文件中创建相关类对象并注入属性
			* 然后进行声明式事务管理：

				* 配置文件方式：

					* 第一步：配置事务管理器

<!--创建事务管理器  -->    <bean id="transactionManager" class="org.springframework.jdb.datasource.DataSourceTranctionManager">         <!--  注入datasource属性-->         <property name="dataSource" ref="dataSource"></property>    </bean>
	* 第二步：配置事务增强

<!-- 配置事务增强器 -->    <tx:advice id="txadvice" transaction-manager="transactionManager">         <!-- 做事务操作 -->         <tx:attributes>             <!-- 设置进行事务操作方法匹配规则 -->             <tx:method name="要增强的方法"/>         </tx:attributes>    </tx:advice>
	* 第三步：配置切面

<!-- 配置切面 -->    <aop:config>         <aop:pointcut expression="" id=""/>         <aop:advisor advice-ref="" pointcut-ref=""/>    </aop:config>
	* 注解形式:

		* 第一步：配置事务管理器（同上）
		* 第二步：开启事务注解

<!--开启事务注解  -->    <tx:annotation-driven transaction-manager="transactionManager"/>
	* 第三步：在要使用事务的方法所在类上面添加注解

        @Transactional
