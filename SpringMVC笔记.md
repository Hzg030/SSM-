SpringMVC架构原理
	* 一次请求的步骤：
 
		* 第一步：发起请求到前端控制器（DispatcheServlet）
		* 第二步：前端控制器请求HandlerMapping查找Handler
		* 第三步：处理器映射器（HandlerMapping）向前端控制器返回Handler
		* 第四步：前端控制器通过处理器适配器执行Handler
		* 第五步：处理器适配器（HandlerAdapter）执行Handler
		* 第六步：Handler执行完返回ModelAndView给处理器适配器
		* 第七步：处理器适配器再向前端控制器返回ModelAndView
		* 第八步：前端控制器调用视图解析器解析ModelAndView
		* 第九步：视图解析器解析ModelAndView，返回真正的视图（Jsp）给前端控制器
		* 第十步：前端控制器进行视图渲染，视图渲染将模型数据填充到request域中
		* 第十一步：前端控制器想用户响应结果


	* 各组件：

		* 前端控制器（DispatcheServlet）：作用：接收请求，响应结果，相当于转发器、中央处理器
		* 处理器映射器（HandlerMapping）：作用：根据请求的URL查找Handler
		* 处理器（Handler）：编写Handler时按照HandlerAdapter要求去做，这样适配器才可以去正确的执行Handler
		* 处理器适配器（HandlerAdapter）：作用：按照特定规则（HandlerAdapter要求的规则）去执行Handler
		* 视图解析器（View resolver）：作用：对视图进行解析，根据逻辑视图名解析成真正的视图（View）
		* 视图（View）：View是一个接口，实现类支持不同的View类型（jsp,freemarker,pdf......）

SpringMVC入门程序（配置文件方式）
	* 在web.xml中配政前端控制器

<!--SpringMVC前端控制器，本质就是个servlet  -->    <servlet>         <servlet-name>springmvc</servlet-name>         <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>         <!-- contextConfigLocation配置springMVC加载的配置文件（配置处理器映射器，适配器等等）             如果不配置contextConfigLocation，默认加载的是?/WEB-INF/servlet名称-servlet.xml（springmvc-servlet.xml）             但是一般都需要配置         -->         <init-param>             <param-name>contextConfigLocation</param-name>             <param-value>classpath:springmvc.xml</param-value>         </init-param>    </servlet>
	* 在web.xml中配置Servlet映射（servlet-mapping）：

<!--配置处理器映射器  -->    <servlet-mapping>         <servlet-name>springmvc</servlet-name>         <!-- 第一种方式：*.action，接收所有以.action结尾的访问 并由DispatcheServlet进行解析             第二种：/，接收所有的访问并且都由DispatcheServlet进行解析，而对于静态文件需要配置为不让DispatcheServlet进行解析             使用此种方式可以实现RESTful风格的url             第三种：/*，这样配置不对，使用这种配置，最终要转发到一个jsp页面时，仍然会由DispatcheServlet解析Jsp,buneng genju jsp页面来找到Handler         -->         <url-pattern>/</url-pattern>    </servlet-mapping>
	* 在classpath下的springmvc.xml中配置Handle映射

<!--配置Handler  name表示UserController的映射url-->    <bean name="/index.action" class="demo.UserController"/>
	* 在classpath下的springmvc.xml中配置处理器映射器

<!--配置处理器映射器     将bean的name作为url进行查找，需要在配置Handler时指定beanname（就是url）-->    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
	* 在classpath下的springmvc.xml中配置处理器适配器

<!--配置处理器适配器  -->    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
	* 编写Handler:需要实现controller接口，才能由org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter 适配器执行

public class UserController implements Controller{    public ModelAndView handleRequest(HttpServletRequest arg0, HttpServletResponse arg1) throws Exception {         // 调用service查找数据库查询用户列表，这里使用静态数据模拟         User user = new User();         user.setNickname("大王");         user.setId(15);                  //返回 ModelAndView         ModelAndView mav = new ModelAndView();                  //addObject相当于request的setAttribut方法         mav.addObject("userselect", user);                  //指定视图         mav.setViewName("/WEB-INF/index.jsp");         return mav;    }}

		* 编写jsp
	* 在classpath下的springmvc.xml中配置视图解析器

<!--配置视图解析器     解析Jsp视图，默认使用jstl，所以要导入jstl 的jar包-->    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">    <!--配置jsp文件的前缀prefix后缀 suffix -->        <property name="prefix" value="/WEB-INF/"></property>        <property name="suffix" value=".jsp"></property>    </bean>
	* 注意：处理器映射器找不到Handler时报的是：HTTP Status 404 - 错误，而如果是该错误后面带有jsp页面路径则表示处理器映射器找到了Handler但是该jsp页面地址出错了。

非注解的映射器和适配器
	* 非注解映射器：

		* org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping
		* org.springframework.web.servlet.handler.SimpleUrlHandlerMapping
		* 多个映射器可以并存，前端控制器判断url能让哪些映射器映射，就让正确的映射器处理
	* 非注解的适配器：

		* org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter要求编写的Hanlder要实现Controller接口
		* org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter  要求编写的Handler实现HttpRequestHandler接口，使用此方法可以通过修改response，设置响应的数据格式，比如响应json数据
	* 前端控制器从DispatcherServlet.properties文件中加载处理映射器、适配器、视图解析器等组件，如果不在springmvc.xml中配置，就使用默认加载的

        注解的映射器和适配器（二者必须搭配使用）
	* 版本说明：

		* 注解映射器

			* 在spring3.1之前使用的是org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping注解映射器
			* 在spring3.1之后使用的是org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping注解映射器
		* 注解适配器
		* 在spring3.1之前使用的是org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdpter注解适配器
		* 在spring3.1之后使用的是org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter注解适配器
	* 配置注解映射器、适配器：

<!--配置注解的处理器映射器  -->    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/><!--配置注解的处理器适配器  -->    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>
	* 另一种方式：

<!--使用mvc:annotation-driven可以代替上面注解映射器适配器的配置    mvc:annotation-driven默认还加载了很多参数的绑定方法，比如json转换解析器就默认加载了，如果使用mvc:annotation-driven就不用再去配置别的    实际开发中使用mvc:annotation-driven -->    <mvc:annotation-driven></mvc:annotation-driven>
	* 开发注解Handler:

//注解Handler，使用@Controller标识这是一个控制器@Controllerpublic class UserController2{        //用户查询方法    //@RequestMapping将下面的方法和url进行映射，一个方法对应一个url    //一般建议将url和方法写的一样    @RequestMapping("/selectUser")    public ModelAndView selectUser(){         User user = new User();         user.setNickname("大王王");         user.setId(15);                  User user2 = new User();         user2.setNickname("小王王");         user2.setId(18);                  List<User> list = new ArrayList<User>();         list.add(user);         list.add(user2);                  //返回 ModelAndView         ModelAndView mav = new ModelAndView();                  //addObject相当于request的setAttribut方法         mav.addObject("userlist", list);                  //指定视图         mav.setViewName("/WEB-INF/index.jsp");         return mav;    }    //可定义多种方法}
	* 在spring.xml中开启组件扫描，加载Handler：

<!--对于注解的Handler可以单个配置，但是实际开发中使用组件扫描    组件扫描可以扫描controller、service。。。    base-package指定要扫描的包-->    <context:component-scan base-package="demo"></context:component-scan>@RequestMapping映射以及属性
	* 可同时放在类前跟类方法前，路径就是二者结合的路径，映射是去匹配@RequestMapping注解
	* 属性：

		* value：映射路径，*代表任意字符（一个或多个），？代表一个字符，**代表任意目录
		* method：指定请求方式（RequestMethod.GET/POST/PUT/DELETE）
		* params：指定请求必须包含的参数以及参数的格式（params={"name=hzg","age!=23"，"!height（不能有height参数）"}）
		* headers：指定请求头的内容

REST风格
	* GET：查
	* POST：增
	* DELETE：删
	* PUT：改
	* 使用HiddenHttpMethodFilter过滤器：

		* 在web.xml中配置过滤器：

    <filter>             <filter-name>HiddenHttpMethodFilter</filter-name>             <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>     </filter>     <filter-mapping>             <filter-name>HiddenHttpMethodFilter</filter-name>             <servlet-name>spring</servlet-name>     </filter-mapping>
	* 在jsp中使用以下方式来发起PUT或DELETE请求：

<form action="..." method="post"> <input type="hidden" name="_method" value="put" />......</form>使用原生态servletAPI
	* 在方法参数中加入@RequestParam（"name"） String name即可，相当于String name = request.gerParameter("name")

		* @RequestParam中的属性：

			* value：变量名
			* required：参数是不是必须的
			* defaultValue：参数默认值
	* 在方法参数前加入@RequestHeader可以获取请求头部信息
	* 在方法参数前加入@CookieValue可以获取cookie信息

		* 服务端在接收客户端第一次请求时，会给该客户端分配一个session（该session包含一个sessionid），并且服务端会在第一次响应客户端时，将该sessionid赋值给JSESSIONID，并传递给客户端的cookie
	* 在方法参数前加入POJO可以直接获取POJO对象，前提是表单中name属性必须和POJO中属性一致（支持级联属性）

处理模型数据
	* 跳转时需要带数据：V，M则可以使用以下方式：ModelAndView、ModelMap、Map、Model、（前面四个把数据放在request域中）@SessionAttributes、@ModelAttribute
	* ModelAndView方式：

		* 可以通过构造方法跟set方法给View赋值
        ModelAndView mav = new ModelAndView("/WEB-INF/index.jsp");                  //指定视图         mav.setViewName("/WEB-INF/index.jsp");
		* 通过addObject方法添加参数：

    mav.addObject("userlist", list);

		* 传递的参数在前端可通过EL方式取出：#{requestScope.userlist.user.name }
	* @SessionAtttrbutes（value = "param"）：

		* 如果将param参数放到了request域中，则同时将该参数放到session域中。
	* @SessionAtttrbutes（type= "Elementtype"）：

		* 如果将Elementtype类型放到了request域中，则同时将该类型放到session域中。
		* tips:该注解放在类上面
	* @ModelAttribute:

		* 一个controller类中如果有带该注解的方法，则最先执行该注解修饰的方法、
		* 应用场景：修改用户信息时应先将该用户的信息查询得到，再在查询到的结果的基础上进行修改
		* 约定：

			* 在ModelAttribute方法中查询到的POJO放置到对应的Model中时要求key是POJO类名首字母小写，这样@RequestMapping注解映射的方法才能直接取得该POJO；
			* 除此之外，也可以在@RequestMapping注解映射的方法参数中使用@ModelAttribute（"pojokey"）对应@ModelAttribute方法中的key

视图解析器
	* 流程：controller中无论返回值是什么都会被转化成ModelAndView，再通过视图解析器解析成视图
	* 视图的顶级接口：View
	* 视图解析器：ViewResolver
	* 常见的视图和解析器：InternalResoutceView、InternalResourceViewResolver
	* 如何在springmvc中实现从一个jsp直接跳到另一个jsp：

		* 在springmvc中添加一下标签：

			* <mvc:view-controller path="请求的controller路径"  view-name="跳转的路径">，view-name会被视图解析器自动加上前缀后缀
	* 在controller中指定跳转方式，该方式不会被视图解析器加上前缀后缀

		* forward：return "forward:/view/success.jsp";
		* rddirect：return "rddirect:/view/success.jsp";
	* 处理静态资源：html,css,js,图片，视频

		* 原因：在springmvc中直接访问静态资源会404，因为在配置dispatcherservlet时，会拦截所有请求，然后去找@RequestMapping，而静态资源无法配置该注解，所以404
		* 解决方法：如果有springmvc对一个的@RequestMapping则交给springmvc处理，如果没有对应的@RequestMapping，则交给tomcat默认的servlet处理。
		* 实现方法：在springmvc中增加一个<mvc:default-servlet-handler />标签

类型转换器
	* spring自带一些常见的类型转换器
	* 可以自定义类型转换器：

		* 步骤：

			* 编写自定义类型转换器的类（要求实现Converter接口）
			* 将自定义转换器纳入spring IOC容器
			* 在将自定义转换器纳入SpringMVC提供的转换器bean
			* 将SpringMVC提供的转换器bean注册到annotation-driven中

数据格式化
	* 实现步骤：

		* 在springmvc中配置数据格式化注解所依赖的bean
		* 通过注解来使用数据格式化：在要格式化的数据属性上加@DateTimeFormat（pattern="匹配的模式，如yy-MM-dd"）注解
		* 格式化不是将传递过来的参数转化成要求的格式，而是规范前端传递参数时的格式

国际化...错误处理
	* 在controller方法参数中加入BindingResult  result可以捕获前一个参数发生的错误

数据校验
	* JSR303

Ajax请求SpringMVC并返回请求数据
	* 通过@ResponseBody注解实现，并以json数组返回数据

上传文件
	* 与servlet上传文件方式本质一样，都是通过commons-fileUpload.jar和commons-io.jar来实现
	* SpringMVC可以简化文件上传的代码，但是必须满足条件：实现MultiparResolver接口，而该接口的实现类SpringMVC也已经提供了CommonsMultpartResolver
	* 具体步骤（直接使用CommonsMultpartResolver实现上传）：

		* 导入jar包
		* 配置CommonsMultpartResolver，将其加入IOC容器，包括基本属性如defaultEncoding、maxUploadSize等
		* 在controller方法中使用MultiparFile参数来接收文件

拦截器
	* 拦截器的原理和过滤器相同。
	* SpringMVC：想要实现拦截器，必须实现一个接口：HandlerInterceptor


