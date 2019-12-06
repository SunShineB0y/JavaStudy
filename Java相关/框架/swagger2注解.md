## swagger2 注解

### 1.@Api：用于注解类，是对类的说明
- tags="说明类的作用，可以在UI界面看到的注解"
- values="没什么特殊意思，在UI界面也可以看到，所以不需要配置"

		@Api(tags="APP用户注册Controller")
### 2.@ApiOperation:用在请求的方法上，说明方法方法的用途，作用
- value="说明方法的用途，作用"
- notes="方法的备注说明"
	
		@ApiOperation(value="用户注册",notes="手机号、密码都是必输项，年龄随便填，但必须是数字")

### 3.@ApiImplicitParams：用在请求的方法上，表示一组参数说明

-  @ApiImplicitParam，用在@ApiImplicitParams注解中，对某一个参数进行具体说明

	- name:参数名
	- value：参数的汉字解释
	- required:参数是否必须传，默认是true，即必须传
	- paramType:参数类型，用来说明参数是从哪儿获取到的

		- paramType是header时: 用@RequestHeader
		- paramType是query时: 用@RequestParam，说明参数是URL传递过来的
		- paramType是path时：用@PathVariable，说明参数是从URL链接中取到的
		- body:不常用
		- form：不常用
	- dataType:参数类型，默认String
	- defaultType:参数的默认值

		
			@ApiImplicitParams({
    		@ApiImplicitParam(name="mobile",value="手机号",required=true,paramType="form"),
    		@ApiImplicitParam(name="password",value="密码",required=true,paramType="form"),
    		@ApiImplicitParam(name="age",value="年龄",required=true,paramType="form",dataType="Integer")

### 4.@ApiResponses:用在请求的方法上，表示一组响应

- @ApiResponse:用在@ApiResponses中，一般用于表达一个错误的响应信息
		
	- code:数字，例如400
	- message:信息，例如“参数有误”
	- response:抛出异常的类

		
			@ApiOperation(value = "select1请求",notes = "多个参数，多种的查询参数类型")
			@ApiResponses({
    		@ApiResponse(code=400,message="请求参数没填好"),
    		@ApiResponse(code=404,message="请求路径没有或页面跳转路径不对")
			})
 

### 5.@ApiModel：用于响应类上，表示一个返回响应数据的信息
这种一般用在post创建的时候，使用@RequestBody这样的场景，请求参数无法使用@ApiImplicitParam注解进行描述的时候

- @ApiModelProperty：用在属性上，描述响应类的属性

			@ApiModel(description= "返回响应数据")
			public class RestMessage implements Serializable{
 
    		@ApiModelProperty(value = "是否成功")
    		private boolean success=true;
    		@ApiModelProperty(value = "返回对象")
    		private Object data;
			}