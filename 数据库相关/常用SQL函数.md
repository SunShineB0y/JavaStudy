## SQL函数 ##

### ROUND()函数
ROUND() 函数用于把数值字段舍入为指定的小数位数。

	SELECT ROUND(column_name,decimals) FROM table_name;

column_name：必需。要舍入的字段。

decimals：规定要返回的小数位数。

	ROUND(1.298, 1) ：1.3
	ROUND(1.298, 0) ：1

ROUND(X)： 返回参数X的四舍五入的一个整数。 

	ROUND(-1.23) ：-1
	ROUND(-1.58) ：-2

### CONCAT()函数
CONCAT()函数可以实现拼接

	CONCAT(DATE(CHECKTIME), ' 00:00:00')

	dbid的模糊查询：DBID LIKE CONCAT('%',CONCAT(#{dbid},'%'))
### DATE_FORMAT()函数和TO_CHAR()函数 ###
这两个函数都可以实现时间格式的转换，返回字符串类型的数据

DB2和ORACLE:
	TO_CHAR(CHECKTIME, 'yyyy-mm-dd hh24:mi:ss')

MYSQL：
	DATE_FORMAT(CHECKTIME,'%Y-%m-%d %H:%i:%s')

### DATE()函数 ###
在MySql中，NOW() 函数返回当前的日期和时间

	NOW()
CURDATE() 函数返回当前的日期

	CURDATE()
CURTIME() 函数返回当前的时间

	CURTIME()
DATE() 函数返回日期或日期/时间表达式的日期部分

	DATE(date)

EXTRACT() 函数用于返回日期/时间的单独部分，比如年、月、日、小时、分钟等等。
	
	EXTRACT(unit FROM date)，unit可以是YEAR,MONTH,DAY,HOUR,MINNTE,SECOND等

DATE_ADD() 函数向日期添加指定的时间间隔。

	DATE_ADD(date,INTERVAL 2 day) 表示加2天
	DATE_ADD(date,INTERVAL 2 hour) 表示加2小时

DATE_SUB() 函数从日期减去指定的时间间隔

	DATE_SUB(date,INTERVAL 2 hour) 表示减2小时
 
### NULL函数 ###

### CAST()函数 ###
类型转换

### COALESCE()函数 ###



### UCASE()函数
UCASE() 函数把字段的值转换为大写
	
	SELECT UCASE(column_name) FROM table_name;
用于SQL SERVER的用法：

	SELECT UPPER(column_name) FROM table_name;

### LCASE() 函数
LCASE() 函数把字段的指转换为小写，SQL SERVER则用LOWER()

### MID()函数 ###
MID()函数用于从文本字段中提取字符

	SELECT MID(column_name,start[,length]) FROM table_name;
column_name： 必需。要提取字符的字段。
start：必需。规定开始位置（起始值是 1）。
length：可选。要返回的字符数。如果省略，则 MID() 函数返回剩余文本。

ORACLE和DB2中用substr()函数实现此功能
SELECT substr(checktime,1,10) AS checkdate FROM table;

### LEN()函数 ###
LEN()函数返回文本字段中值的长度

	SELECT LEN(column_name) FROM table_name;

MySql中用LENGTH()函数：

	SELECT LENGTH(column_name) FROM table_name;
