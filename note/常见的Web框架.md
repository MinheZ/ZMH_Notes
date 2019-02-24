* [SpringMVC](#SpringMVC)
    * [SpringMVC处理请求的流程](#SpringMVC处理请求的流程)
* [Spring](#Spring)
    * [Spring AOP](#SpringAOP)

----------------------

# SpringMVC
MVC是Model-View-Controller的简称，即模型-视图-控制器。MVC是一种设计模式，它强行把应用程序的输入、处理和输出分开。

MVC中模型、视图和控制器都承担着不同的任务：
- **视图：** 用户看到并与之交互的界面，向用户展示数据，并接收用户的输入。它不参与任何业务逻辑处理。
- **模型：** 表示业务处理，相当于JavaBean。一个模型能为多个视图提供数据，提高了代码的复用性。
- **控制器：** 当用户单击Web上的提交按钮，控制器接收请求并调用相应的模型去处理请求。

## SpringMVC处理请求的流程
SpringMVC提供了前端控制器(Dispatcher Servlet)；处理器映射器(Handler Mapping)和处理适配器（Handler Adapter），视图解析器(View Resolver)进行视图管理；动作处理器Controller接口（包含ModelAndView，以及处理请求响应对象request和response），配置灵活，支持文件上传，数据简单转化等强大功能。
<div align="center"><img src="../pics//1550804996(1).png" width="650px"></div>

工作流程如下：
- 客户通过url发送请求
- 前端控制器(Dispatcher Servlet)接收到请求，通过系统或自定义的处理器映射器找到对应的Handler，生成处理器执行链HandlerExecutionChain(包括处理器对象和处理器拦截器)一并返回给DispatcherServlet。
- DispatcherServlet根据处理器Handler获取处理器适配器HandlerAdapter执行HandlerAdapter处理一系列的操作，如：参数封装，数据格式转换，数据验证等操作
- 执行处理器Handler(Controller，也叫页面控制器)。
- Handler执行完成返回ModelAndView
- HandlerAdapter将Handler执行结果ModelAndView返回到DispatcherServlet
- DispatcherServlet将ModelAndView传给ViewReslover视图解析器
- ViewReslover解析后返回具体View
- DispatcherServlet对View进行渲染视图（即将模型数据model填充至视图中）。
- DispatcherServlet响应用户。

---------------------

# Spring
Spring的两大核心技术：
- IoC(控制反转)
- AOP(面向切面编程)

## Spring AOP
与OOP相比，面向切面，传统的OOP开发中的代码逻辑是自上而下的，在这些过程中会产生一切横切性的问题，这些问题与主业务逻辑关系不大，会散落在代码的各个地方，难以维护。AOP的思想就是把业务逻辑和横切问题进行分离，从而达到解耦的目的，使代码的重用性和开发效率变高。
### AOP的应用场景
- 日志记录
- 权限验证
- 效率检查
- 事务管理等

### AOP的底层技术
Spring AOP底层用到2种代理机制：
- JDK的动态代理：针对实现了接口的类产生代理
- cglib代理：针对没有实现接口的类产生代理，应用的是底层字节码增强技术，生成当前类的子类对象。

### AOP开发中的相关术语
- **Joinpoint(连接点):** 所谓连接点是指那些被拦截到的点。在Spring中,这些点指的是方法,因为Spring只支持方法类型的连接点.
- **Pointcut(切入点):** 所谓切入点是指我们要对哪些Joinpoint进行拦截的定义.
- **Advice(通知/增强):** 所谓通知是指拦截到Joinpoint之后所要做的事情就是通知.通知分为前置通知,后置通知,异常通知,最终通知,环绕通知(切面要完成的功能)
- **Introduction(引介):** 引介是一种特殊的通知在不修改类代码的前提下, Introduction可以在运行期为类动态地添加一些方法或Field.
- **Target(目标对象):** 代理的目标对象
- **Weaving(织入):** 是指把增强应用到目标对象来创建新的代理对象的过程。Spring采用动态代理织入，而AspectJ采用编译期织入和类装在期织入。
- **Proxy（代理）:** 一个类被AOP织入增强后，就产生一个结果代理类
- **Aspect(切面):** 是切入点和通知（引介）的结合
