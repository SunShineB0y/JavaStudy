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
	
	
## Struts的核心配置文件struts.xml

    <package name="aaa" namespace="/front" extends="struts-default">
		<action name="denglu" class="cn.java.action.front.frontAction" method="login"></action>
		<result name="abc">/page/front/success.jsp</result>
		<result name="def" type="redirect">/page/front/success.jsp</result>

		<action name="zhuce" class="cn.java.action.front.frontAction" method="register"></action>
		<action>...</action>
	</package>

上面这个`<package>...</package>`可以配置很多个，用来对应不同action中的不同方法，下面介绍每一部分代表什么：

- package：包

- name：包名，没什么具体的意义，就是随便起的一个名字

- namespace：为当前包下的所有action设置一个全局路径

- action标签：用于配置某一个action类

- name：虚拟路径，用于访问具体action类中真正的方法

- class：action类的具体路径

- method：action类中具体的action方法名

- result：执行结果

- name：name中的字符串对应action中return的执行结果，通过这个字符串以及后面配置的路径找到并返回具体的jsp页面，默认就是转发，上面的`abc`就代表转发的路径，加上`type="redirect"`则代表重定向，此处的**转发和重定向**就是struts2的两种跳转方式。

访问cn.java.action.front包下frontAction类的login方法：

    http://localhost:8080/项目名/front/denglu.action

访问cn.java.action.front包下frontAction类的register方法：

    http://localhost:8080/项目名/front/zhuce.action

## 通过Struts2获取表单提交过来的参数信息
1. 第一种方法是通过get/set方法进行获取

	
	- 把属性封装到一个实体类中,通过get/set方法进行获取，使用这样的方式时表单中的对象要加上对象名，如`user.username、user.password`

	    	public class adminAction extends ActionSupport{
				private User user = new User();
				public User get	User(){
					return user;
				}
				
				public void setUser(User user){
					this.user = user;
				}
			 
				//...使用user.get..()方法,实现具体业务逻辑

			} 
		
	- 不进行封装，直接使用属性的get/set方法实现业务逻辑，用这种方式时表单中的对象不需要加对象名，如`username、password` 

			public class adminAction extends ActionSupport{
				private String username;

				private String password;

				public String getUsername(){
					return username;	
				}

				public void setUsername(String username){
					this.username = username;
				}

				public String getPassword(){
					return password;
				}
				
				public void setPassword(String password){
					this.password = password;
				}
				
				...
				
				//...直接使用属性名,实现具体业务逻辑
			}
	
	- 第一种方式更好的体现了封装的思想，当表单传过来的参数较多时，推荐使用第一种方式，这样action类中不会有太多的get/set方法，action类不会显得那么臃肿。
