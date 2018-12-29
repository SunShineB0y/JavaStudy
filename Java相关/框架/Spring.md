## Spring IOC
Spring IOC指Spring的控制反转，将对象的控制权，包括对象的创建以及其他生命周期的控制交给Spring容器去管理。

DI指依赖注入，DI是实现IOC的一种方式，Spring在创建对象的过程中，动态的将对象依赖属性通过配置的方式进行注入。

## Spring AOP
Spring  AOP指Spring的面向切面编程。即将封装好的对象剖开，将那些影响了多个类的公共行为封装到一个可重用模块。简单来说就是把那些与业务无关，却又被业务模块所共同调用的逻辑抽取出去进行封装，减少重复代码，降低模块之间的耦合度。

Spring IOC和AOP都是基于Java的反射机制实现的。

AOP两种实现模式：JDK动态代理和Cglib动态代理。

1. 如果目标对象实现了接口，默认情况下会采用JDK的动态代理实现AOP；

1. 如果目标对象没有实现接口，默认情况下回采用Cglib动态代理实现AOP。这种方式只要业务类没有被标记为final就可以，因为它会生成业务类的子类作为代理类。


## Spring MVC
#### 简介
Spring MVC是一用常见的用于开发Java Web应用的开发模式，是一种典型的MVC模式的开发框架。它的核心是DispatcherServlet  `即前端控制器`  ，DispatcherServlet 充当C `即Controller` 的角色，用于接收与转发用户请求，并响应处理结果。DispatcherServlet是整个流程控制的中心，由它调用其它组件处理用户的请求，它的存在降低了组件之间的耦合性。

#### 核心思想
将业务数据抽取与业务数据呈现相分离。

#### 工作原理

1. 用户通过浏览器发送请求到DispatcherServlet
2. DispatcherServlet调用HandlerMapping处理器映射器。
3. HandlerMapping找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。
4. DispatcherServlet调用HandlerAdapter处理器适配器。
5. HandlerAdapter经过适配调用具体的处理器 `即Controller，也叫后端控制器` 。
6. Controller执行完成后返回ModelAndView到HandlerAdapter。
7. HandlerAdapter返回ModelAndView到DispatcherServlet。
8. DispatcherServlet调用viewResolver视图解析器解析ModelAndView，此时的model是模型数据，view是一个逻辑上的view。
9. viewResolver解析完成后返回view到DispatcherServlet。
10. DispatcherServlet将model模型数据填充到view中，并渲染view。
11. Dispatcher返回视图，响应用户。

#### 组件说明

1. 前端控制器DispatcherServlet（不需要工程师开发）,由框架提供。

  作用：接收请求，响应结果。用户请求到达前端控制器，它就相当于mvc模式中的c，dispatcherServlet是整个流程控制的中心，由它调用其它组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性。

2. 处理器映射器HandlerMapping(不需要工程师开发),由框架提供。

  作用：根据请求的url查找Handler
HandlerMapping负责根据用户请求找到Handler即处理器，springmvc提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

3. 处理器适配器HandlerAdapter。

  作用：按照特定规则（HandlerAdapter要求的规则）去执行Handler。通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行

4. 处理器Handler(需要工程师开发)

  注意：编写Handler时按照HandlerAdapter的要求去做，这样适配器才可以去正确执行Handler
Handler 是继DispatcherServlet前端控制器的后端控制器，在DispatcherServlet的控制下Handler对具体的用户请求进行处理。
由于Handler涉及到具体的用户业务请求，所以一般情况需要工程师根据业务需求开发Handler。

5. 视图解析器View resolver(不需要工程师开发),由框架提供。

  作用：进行视图解析，根据逻辑视图名解析成真正的视图（view）
View Resolver负责将处理结果生成View视图，View Resolver首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成View视图对象。 

6. 视图View(需要工程师开发jsp...)

 View是一个接口，实现类支持不同的View类型（jsp、freemarker、pdf...）



