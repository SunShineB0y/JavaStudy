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

### CAST()函数 ###
CAST (expression AS data_type)

	expression：任何有效的sql表达式。
	AS：用于分隔两个参数，在AS之前的是要处理的数据，在AS之后是要转换的数据类型。
	data_type：目标系统所提供的数据类型。

使用CAST函数进行数据类型转换时，在下列情况下能够被接受：

（1）两个表达式的数据类型完全相同。
（2）两个表达式可隐性转换。
（3）必须显式转换数据类型。

如果试图进行不可能的转换（例如，将含有字母的 char 表达式转换为 int 类型），SQServer 将显示一条错误信息。
如果转换时没有指定数据类型的长度，则SQServer自动提供长度为30。

### DECODE()函数 ###
DECODE()函数是Oracle特有的函数。使用：DECODE(value,if 条件1，then 值1，if 条件2，then 值2，...，else 其他值)

	Select decode(sign(var1-var2),1,var1,var2) from dual
	Sign()函数根据某个值是0、正数、负数，分别返回0、1、-1；
	Select decode(sign(100-90),1,100,90) from dual，则输出结果为100；
	Select decode(sign(100-90),-1,100,90) from dual 则输出结果是90。

### COALESCE()函数 ###
返回参数列表中的第一个非空表达式（如果所有参数均为 NULL，则 返回 NULL），且函数里面的数据类型，必须全部都跟第一列的数据类型一致。
	
	coalesce(a,b,c);
	说明：如果a==null,则选择b；如果b==null,则选择c；如果a!=null,则选择a；如果a b c 都为null ，则返回为null（没意义）。

### NVL()函数 ###
NVL函数是一个空值转换函数

	NVL（表达式1，表达式2）

如果表达式1为空值，NVL返回值为表达式2的值，否则返回表达式1的值。

该函数的目的是把一个空值（null）转换成一个实际的值。其表达式的值可以是数字型、字符型和日期型。但是表达式1和表达式2的数据类型必须为同一个类型。

	NVL2(表达式1,表达式2, 表达式3)

如果该函数的第一个表达式为null那么显示第三个表达式的值，如果第一个表达式的值不为null，则显示第二个表达式的值。

### NULLIF函数 ###
NULLIF(exp1,expr2)函数的作用是如果exp1和exp2相等则返回空(NULL)，否则返回第一个值。

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

在MySQL中，DATE_ADD() 函数向日期添加指定的时间间隔，DATE_SUB() 函数从日期减去指定的时间间隔。而在DB2中，用“+”或“-”即可实现时间运算

	DATE_ADD(date,INTERVAL 2 day) 表示加2天
	DATE_ADD(date,INTERVAL 2 hour) 表示加2小时
	DATE_SUB(date,INTERVAL 2 hour) 表示减2小时
	(#{checktime})+1 HOUR  DB2的写法，checktime是YYYY-MM-DD HH:mm:ss格式，表示加一小时，(#{checktime})-1 DAY 则表示减去一天
 
### NULL函数 ###


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
