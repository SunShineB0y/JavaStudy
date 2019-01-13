# Struts2
Struts2是一个基于MVC设计模式的Web应用框架，它本质上相当于一个servlet，在MVC设计模式中，Struts2作为控制器(Controller)来建立模型与视图的数据交互。Struts2与Struts差异巨大。

## Struts2的核心过滤器：StrutsPrepareAndExecuteFilter
需要在web.xml中配置Struts2的核心过滤器

    <filter>
		<filter-name>strutsParepreAndExecuteFilter</filter-name>
		<filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-name>
	</filter>
	<filter-mapping>
	<filetr-name>strutsPrepareAndExecuteFilter</filetr-name>
	<url-pattern>/*</url-pattern>
	</filter-mapping>

在此说一下url-pattern的三种写法

1. 完全匹配：<url-pattern>/test/list.action</url-pattern>  
2. 路径匹配：<url-pattern>/*</url-pattern> Struts2会匹配根路径下的全部请求
3. 扩展名匹配：
	
	- Struts1中的写法：<url-pattern>*.do</url-pattern>

	- 匹配全部以html结尾的请求：<url-pattern>*.html</url-pattern>
	
	- 错误写法：<url-pattern>*</url-pattern>

