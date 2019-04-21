## JSON

### 1.JSON语法规则
JSON语法是JavaScript对象表示法语法的子集。
- 数据在名称/值对中
- 数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

#### JSON名称/值对
名称/值对的形式是"名称":"值"

	"firstName" : "John"
等价于JavaScript语句：
	
	firstName = "John"

#### JSON值
JSON值可以是以下类型：

- 数字（整数或浮点数）
- 字符串（在双引号中）
- 逻辑值（true或false）
- 数组（在方括号中）
- 对象（在花括号中）
- null

#### JSON对象
JSON对象可以包含多个名称/值对

	{"firstName":"John","lastName":"Doe"}

等价于JavaScript语句：

	firstName = "John"
	lastName = "Doe"

#### JSON 数组
JSON数组可以包含多个对象

	{
		"employees": [
			{ "firstName":"John" , "lastName":"Doe" },
			{ "firstName":"Anna" , "lastName":"Smith" },
			{ "firstName":"Peter" , "lastName":"Jones" }
		]
	}

对象 "employees" 是包含三个对象的数组。每个对象代表一条关于某人（有姓和名）的记录。

#### JSON使用JavaScript语法
 JSON 使用 JavaScript 语法，所以无需额外的软件就能处理 JavaScript 中的 JSON。

JavaScript创建一个对象数组，并像这样进行赋值：

	var employees = [
	{ "firstName":"Bill" , "lastName":"Gates" },
	{ "firstName":"George" , "lastName":"Bush" },
	{ "firstName":"Thomas" , "lastName": "Carter" }
	];

访问employees数组中的第一项：

	emploees[0].lastName;

返回的内容是：
	
	Gates

可以像这样修改数据：

	employees[0].lastName = "Jobs";

#### JSON文件
- JSON文件的文件类型是".json"
- JSON文件的MIME类型是"application/json"

### 2.JSON使用

JSON 最常见的用法之一，是从 web 服务器上读取 JSON 数据（作为文件或作为 HttpRequest），将 JSON 数据转换为 JavaScript 对象，然后在网页中使用该数据。

创建包含 JSON 语法的 JavaScript 字符串：

	var txt = '{ "employees" : [' +
	'{ "firstName":"Bill" , "lastName":"Gates" },' +
	'{ "firstName":"George" , "lastName":"Bush" },' +
	'{ "firstName":"Thomas" , "lastName":"Carter" } ]}';