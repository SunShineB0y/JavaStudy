## Servlet总结
在Java Web程序中，Servlet主要负责接受用户请求HttpServletRequest,在doGet(),doPost()中做相应的处理，并将回应**HttpServletResponse**反馈给用户。Servlet可以设置初始化参数，供Servlet内部使用。一个Servlet类只会有一个实例，在它初始化时调用**init(）**方法，在销毁时调用**destory()**方法。Servlet需要在web.xml中配置，（MyEclipse中创建Servlet会自动配置），一个Servlet可以设置多个URL访问。Servlet不是线程安全，因此要谨慎使用类变量。

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
3. get传输数据收到URL长度限制(1024字节即256字符)；post可以传输大量数据，上传文件通常使用post方式
4. 使用get时参数会显示在地址栏上，而post不会，传输敏感数据推荐使用post方式

get方式提交表单的典型应用是搜索引擎。
