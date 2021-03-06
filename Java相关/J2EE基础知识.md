## Servlet总结
在Java Web程序中，Servlet主要负责接受用户请求HttpServletRequest,在doGet(),doPost()中做相应的处理，并将回应**HttpServletResponse**反馈给用户。Servlet可以设置初始化参数，供Servlet内部使用。一个Servlet类只会有一个实例，在它初始化时调用 **init()** 方法，在销毁时调用 **destory()** 方法。Servlet需要在web.xml中配置，（MyEclipse中创建Servlet会自动配置），一个Servlet可以设置多个URL访问。Servlet不是线程安全，因此要谨慎使用类变量。

## Servlet的优点
1. 只需要启动一个操作系统进程以及加载一个JVM，大大降低了系统的开销
2. 如果多个请求需要做同样处理的时候，这时候只需要加载一个类，这也大大降低了开销
3. 所有动态加载的类可以实现对网络协议以及请求解码的共享，大大降低了工作量。
4. Servlet能直接和Web服务器交互。Servlet还能在各个程序之间共享数据，使数据库连接池之类的功能很容易实现。

## Servlet接口中有哪些方法及Servlet生命周期探秘
Servlet接口定义了5个方法，其中前三个方法与Servlet生命周期相关：

- void init(ServletConfig config) throws ServletException 

- void service(ServletRequest req, ServletResponse resp) throws ServletException, java.io.IOException

- void destory()

- java.lang.String getServletInfo()

- ServletConfig getServletConfig()

**声命周期： Web容器加载Servlet并将其实例化后，Servlet生命周期开始**，容器运行其init()方法进行Servlet的初始化；请求到达时调用Servlet的service()方法，service()方法会根据需要调用与请求对应的doGet或doPost等方法；当服务器关闭或项目被卸载时服务器会将Servlet实例销毁，此时会调用Servlet的destroy()方法。 init方法和destory方法只会执行一次，service方法客户端每次请求Servlet都会执行。Servlet中有时会用到一些需要初始化与销毁的资源，因此可以把初始化资源的代码放入init方法中，销毁资源的代码放入destroy方法中，这样就不需要每次处理客户端的请求都要初始化与销毁资源。

## get和post请求的区别

1. get请求用于从服务器获取资源，post请求用于向服务器提交数据
2. get请求将表单中的数据按照name＝value的形式，通过?添加到action所指向的url后面，并且变量之间用＆连接；post是将表单中的数据放到HTTP协议的请求头或消息体中，传递到action所指向url
3. get传输数据受到URL长度限制(1024字节即256字符)；post可以传输大量数据，上传文件通常使用post方式
4. 使用get时参数会显示在地址栏上，而post不会，传输敏感数据推荐使用post方式

get方式提交表单的典型应用是搜索引擎。

## 什么情况下调用doGet()和doPost()

Form标签的method属性是get时调用doGet(),是post时调用doPost()方法

## Servlet与线程安全

Servlet**不是**线程安全的，多线程并发的读写会导致数据不同步的问题。

 解决的办法是尽量不要定义name属性，而是要把name变量分别定义在doGet()和doPost()方法内。虽然使用synchronized(name){}语句块可以解决问题，但是会造成线程的等待，不是很科学的办法。 注意：多线程的并发的读写Servlet类属性会导致数据不同步。但是如果只是并发地读取属性而不写入，则不存在数据不同步的问题。因此Servlet里的只读属性最好定义为final类型的。
 
 ## 转发和重定向的区别
转发是服务器行为，重定向是客户端行为

转发（Forward）通过RequestDispatcher对象的forward（HttpServletRequest request,HttpServletResponse response）方法实现的。RequestDispatcher可以通过HttpServletRequest 的getRequestDispatcher()方法获得。例如下面的代码就是跳转到login_success.jsp页面。

     request.getRequestDispatcher("login_success.jsp").forward(request, response);

重定向（Redirect）是利用服务器返回的状态码来实现的。客户端浏览器请求服务器的时候，服务器会返回一个状态码。服务器通过HttpServletResponse的setStatus(int status)方法设置状态码。如果服务器返回301或者302，则浏览器会到新的网址重新请求该资源。

**区别：**

1. 地址栏显示不同：forward是服务器请求资源,服务器直接访问目标地址的URL,把那个URL的响应内容读取过来,然后把这些内容再发给浏览器.浏览器根本不知道服务器发送的内容从哪里来的,所以它的地址栏还是原来的地址. redirect是服务端根据逻辑,发送一个状态码,告诉浏览器重新去请求那个地址.所以地址栏显示的是新的URL。一句话表述：**重定向会使URL发生变化，而转发不会。** 
2. 数据共享不同：转发页面和转发到的页面可以共享request里的数据，而重定向不能。
3. 运用地方不同：转发一般用于用户登录的时候,根据角色转发到相应的模块。重定向一般用于用户注销登陆时返回主页面和跳转到其它的网站等
4. 效率上转发更高，而重定向较低。

## 自动刷新（Refresh）

自动刷新可以实现一段时间后自动跳转到其他页面，也可以实现一段时间后自动刷新本页面。
Servlet中通过HttpServletResponse对象设置Header属性实现自动刷新。例如：

    Response.setHeader("Refresh","1000;URL=http://localhost:8080/servlet/example.htm");


其中1000为时间，单位为毫秒。URL指定就是要跳转的页面（如果设置自己的路径，就会实现每过一秒自动刷新本页面一次），电商平台的支付成功页面会自动跳转到商品详情页面，这就用到了自动刷新。


## JSP的内置对象及作用
JSP共有9个内置对象：

- request:封装客户端的请求，其中包含来自GET或POST请求的参数

- response：封装服务器对客户端的响应

- session:封装用户会话的对象

- pageContext:通过该对象可以获得其他对象

- application:封装服务器运行环境的对象

- out:输出服务器响应的输出流对象

- config：Web应用的配置对象

- page:JSP页面本身（相当于java中的this）	

- exception:封装页面抛出异常的对象 

## JSP的四大作用域

1. page，只在当前页面有效，也就是用户请求的页面有效，当当前页面关闭或转到其他页面时，page对象将在响应回馈给客户端后释放。

2. request，在当前请求中有效，request可以通过setAttribute()方法实现页面中的信息传递，也可以通过forward()方法进行页面间的跳转，需要注意的是request是转发不是重定向，转发相对于浏览器来说是透明的，也就是无论页面如何跳转，地址栏上显示的依旧是最初的地址。

3. session，在当前会话中有效。当一个台电脑上的同一浏览器对服务器进行多次访问时，在这多次访问之间传递的信息就是session作用域的范围。它从浏览器发出第一个HTTP请求即可认为会话开始，但是会话结束的时间是不确定的，因为在浏览器关闭时并不会通知服务器，一般Tomcat设置的默认时间为120分钟，也可以通过setMaxInactiveInterval(int)方法进行设置，或是通过invalidate()方法强制结束当前会话。

4. application，在所有的应用程序中都有效，也就是当服务器开始到服务器结束这段时间，application作用域中存储的数据都是有效的，同样可以通过setAttribute赋值和getAttribute取值。 


## include指令
在JSP中使用include指令可以包含其他文件，被包含的文件就好像是JSP的一部分，会被同时编译执行，可以是JSP文件、HTML文件或文本文件。 语法格式如下： <%@ include file="文件相对路径" /%>

## include动作
jsp:include动作元素用来包含静态和动态的文件，该动作把指定的文件插入正在生成的页面。语法格式如下：<jsp:include  page="文件相对路径" flush="true" />

## Cookie
Cookie 是一种会话跟踪机制，由Web服务器生成，发送到浏览器，用于在客户端保持状态。Cookie分为会话Cookie和持久Cookie，会话Cookie保存在内存中，会随着会话的关闭而销毁；持久Cookie保存在本地，可以设定生命周期，到期销毁。Cookie实际上是一小段的文本信息。客户端请求服务器，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。客户端浏览器会把Cookie保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。服务器还可以根据需要修改Cookie的内容。
