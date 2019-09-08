# MySQL数据库笔记
---
## 〇、目录
    一、数据库简介 
	二、MySQL基础
	三、SQL语言
		1 DQL语言
		2 DML语言

## 一、数据库简介
1. 基本概念：
    - **数据库(Database, DB)**：存放数据的仓库，以特定方式有组织地存储数据的集合。
    - **数据库管理系统(Database Management System, DBMS)**：可操纵和管理数据库的软件，常见的有MySQL、Oracle、DB2、SqlServer等。
	- **结构化查询语言(Structured Query Language, SQL)**：一种可对数据库进行设计和增删查改的语言。
2. 数据库的优点：持久化存储、结构化查询。
3. 数据库的特点：
	- 通过**表(table)**来存储数据；
	- 一个数据库可以有多个表，每个表都有唯一名字来标识自己；
	- 表具有特性，定义了如何存储数据；
	- 表中的每一列称为**字段**，表由一个或多个字段组成。
4. 数据库分类：关系型数据库和非关系性数据库
	- 关系型数据库：采用了关系模型来组织数据的数据库，安全、容易理解，但浪费空间、扩展性差。
	- 非关系型数据库：基于键值对的存储方式，性能高、易扩展。

## 二、MySQL基础
1. DBMS分类：
	- 基于共享文件系统的DBMS——Access
	- 基于客户端-服务器的DBMS——MySQL、Oracle、SqlServer
2. MySQL特点:
	- 开源免费
	- 性能高、移植性好
	- 体积小且简单易用
3. MySQL使用命令：
	- 开启与关闭服务：  
		- 方式一：可以在计算机管理-服务设置；
		- 方式二：通过命令行窗口输入`net stop <服务器名>`关闭服务、通过`net start <服务器名>`开启服务。
	- 连接服务器：
		- 方式一：通过MySQL自带的客户端输入root密码进行连接；
		- 方式二：在系统命令行窗口输入`mysql -h <主机名> -P <端口号> -u <用户名> -p<密码>`进行连接，注意上述命令中，“主机名”如果是本机可以填写“localhost”，也可以跳过主机名与端口号的填写；“密码”如果明文显示，则直接在-p后填写，否则-p后回车填写。
	- 退出连接：
		- 方式一：输入命令`exit`
		- 方式二：快捷键Ctrl+C
	- 查看版本：
		- 在命令行中输入`mysql --version`或`mysql -V`
		- 连接状态下，输入`select version();`
4. 常见命令：
	- 展示数据库： `show databases;`
	- 打开数据库： `use <数据库名>;`
	- 在数据库中展示表： `show tables;`(如果展示其他库的表则可以直接`show tables from <库名>;`，但此时仍位于当前库)
	- 查看当前所在库：`select database();`
	- 创建表： `create table <表名>(<字段1> <类型>, <字段2> <类型>);`
	- 查看表结构：`desc <表名>;`
5. MySQL语法规范
	- 不区分大小写，但一般关键字大写，表名、列名小写；
	- 每条命令用`;`结尾；
	- 每条命令可以进行缩进和换行（根据需要，一般关键字单独占一行）；
	- 注释
		- 单行注释：`# 注释文字`或`--注释文字`
		- 多行注释：`/* 注释文字 */`
## 三、SQL语言
SQL是一种结构化查询语言，可对数据库进行设计和增、删、查、改等操作。  
SQL的优点：
- 几乎所有DBMS都支持；
- 语法简单；
- 功能强大，可进行复杂的数据库操作。
### 1 DQL查询语言
1. 基础查询：
	- 语法：  
		`select 查询列表 from 表名`  
		类似于：Python中的`print(内容)`
	- 特点：
		- 查询列表可以是：表中的字段、常量值、表达式、函数；
		- 查询结果是一个虚拟的表格。
	- 用法举例：
		- 查询表中的单个字段:`SELECT last_name FROM employees;`
		- 查询表中的多个字段:`SELECT last_name, salary, email FROM employees;`
		- 查询表中的所有字段:`SELECT * FROM employees;`
		- 查询常量值：`SELECT 100;`或`SELECT 'john';`
		- 查询表达式：`SELECT 100%98;`得到100%98的值2
		- 查询函数，得到该函数的返回值：`SELECT VERSION();`
		- 起别名：  
			方式一：`SELECT 100%98 AS 结果;`或`SELECT last_name AS 姓, first_name AS 名 	FROM employees;`  
			方式二：`SELECT last_name 姓, first_name 名 FROM employees;`
		- 拼接查询：
			```sql
			SELECT 
				CONCAT(last_name, ' ', first_name) AS 姓名 
			FROM 
				employees;
			```
			**注意**
			>如果拼接的字段中存在`NULL`值，则拼接结果会直接显示为`NULL`，可以通过`IFNULL()`函数解决：
			> ```sql
			> SELECT 
			> 	CONCAT(last_name, ' ', first_name, ' ', IFNULL(commission_pct, 0)) AS 姓名 
			> FROM 
			> 	employees;
			> ```
			> `IFNULL(arg1, arg2)`函数有两个参数，第一个是待判断的字段`arg1`，第二个是如果该字段值为`NULL`，则替换为`arg2`。
	- 其他：  
		- 一般要先通过`USE myemployees;`先打开指定的库；
		- 通常为了区分关键字与字段名，在查询字段时可以将字段名用着重号` `` `括起来；
		- 为了区分关键字与别名，在起别名时要将名称用`""`括起来；
		- 为避免查询结果重复，可以用`SELECT DISTINCT department_id FROM employees;`；

2. 条件查询：
	- 语法：  
		`select 查询列表 from 表名 where 筛选条件;`
	- 分类：
		- 按条件表达式筛选，包括的条件运算符有：`>`、`<`、`=`、`!=`、`<>`、`>=`、`<=`；
		- 按逻辑表达式筛选，包括的逻辑运算符有：`&&`、`||`、`！`、`and`、`or`、`not`；
		- 模糊查询，包括的条件运算符有：`like`、`between ... and ...`、`in`、`is null`、`is not null`；
	- 用法举例：
		- 按条件表达式筛选：
			```sql
			SELECT
				*
			FROM
				employees
			WHERE
				salary>12000;
			```
		- 按逻辑表达式筛选：
			```sql
			SELECT
				last_name, salary, commission_pct
			FROM
				employees
			WHERE
				salary>=10000 AND salary<20000;
			```
		- 模糊查询：
			```sql
			# 查询名中第二个字符为a的员工名
			SELECT
				last_name
			FROM
				employees
			WHERE
				last_name LIKE "_a%";
			
			# 查询员工编号在100到120之间的员工信息
			SELECT
				*
			FROM
				employees
			WHERE
				employee_id BETWEEN 100 AND 120;
			
			# 查询工种名称是IT_PROT, AD_VP, AD_PRES之一的员工名和工种
			SELECT
				last_name, job_id
			FROM
				employees
			WHERE
				job_id IN ('IT_PROT', 'AD_VP', 'AD_PRES');

			# 查询没有奖金的员工名和奖金率
			SELECT
				last_name, commission_pct
			FROM
				employees
			WHERE
				commission_pct IS NULL;
			```
			**注意**  
			> 1. `like`特点：一般与通配符搭配使用，包括：%（任意数量的字符，包含0个）、_（任意单个字符）如果所查询目标需包含通配符所用字符，使用转义符号`\`实现；**且`like`判断不只局限于字符型，也可用于数值型**；  
			> 2. `between and`特点：**结果包含临界值**，且临界值调换位置意义不同；  
			> 3. `in`特点：`in`列表的值类型必须一致或兼容，且不支持通配符；  
			> 4. `is null`：`<>`不能用于判断`null`，`is null`和`is not null`可以用来判断；安全等于`<=>`：可以代替`is`，不仅可以用于判断`null`，还可以判断普通值。
3. 排序查询
	- 语法：  
		`select 查询列表 from 表名 【where 筛选列表】 order by 排序列表 【asc|desc】`
	- 特点：
		- `asc`表示升序，`desc`表示降序，如果不写默认为升序；
		- `order by`支持单个字段、多个字段、表达式、函数、别名；
		- `order by`子句一般放在查询语句的最后，但`limit`子句除外。
	- 用法举例：
		- 从高到低排序
			```sql
			# 查询员工信息：要求工资从高到低
			SELECT
				*
			FROM
				employees
			ORDER BY
				salary DESC;
			```
		- 从低到高排序
			```sql
			# 查询员工信息：要求工资从低到高
			SELECT
				*
			FROM
				employees
			ORDER BY
				salary ASC;
			```
		- 带条件的排序查询
			```sql
			# 查询部门编号>=90的员工信息，按入职时间的先后顺序进行排序
			SELECT
				*
			FROM
				employees
			WHERE
				department_id >= 90
			ORDER BY
				hiredate ASC;
			```
		- 按表达式排序
			```sql
			# 按年薪的高低显示员工的信息和年薪
			SELECT
				*, salary*12*(1+IFNULL(commission_pct,0)) AS 年薪
			FROM
				employees
			ORDER BY
				salary*12*(1+IFNULL(commission_pct,0)) DESC;
			```
			**注意**
			> 也可以按别名排序，如上述实例可以改为以下形式
			> ```sql
			> # 按年薪的高低显示员工的信息和年薪
			> SELECT
			> 	*, salary*12*(1+IFNULL(commission_pct,0)) AS 年薪
			> FROM
			> 	employees
			> ORDER BY
			> 	年薪 DESC;
			> ```
		- 按函数排序
			```sql
			# 按名的长度显示员工的姓名和工资
			SELECT 
				LENGTH(last_name) 名字长度, last_name, salary
			FROM 
				employees
			ORDER BY 
				LENGTH(last_name) DESC;
			```
		- 先后排序
			```sql
			# 查询员工信息，要求先按工资降序，再按员工编号升序排序
			SELECT
				*
			FROM
				employees
			ORDER BY salary DESC, employee_id ASC;
			```
4. 常见函数
	- 概念：将实现某种功能的逻辑语句封装在方法体中，对外暴露方法名。
	- 好处：隐藏了实现细节，提高代码的重用性。
	- 调用：`select 函数名(实参列表) 【from 表】`
	- 分类：
		- 单行函数：`concat`、`length`、`ifnull`等；
		- 分组函数：做统计使用，又称为统计函数、聚合函数或组函数。
	- 字符函数举例：
		- 字符串长度
			```sql
			# length
			SELECT LENGTH('john');
			```
		- 拼接字符串函数
			```sql
			# concat
			SELECT 
				CONCAT(last_name,'_',first_name) 姓名 
			FROM
				employees;
			```
		- 大小写转换
			```sql
			# upper lower
			SELECT UPPER('jonh');
			SELECT LOWER('JOHN');
			
			SELECT 
				CONCAT(UPPER(last_name), ' ', LOWER(first_name)) 姓名 
			FROM 
				employees;
			```
		- 截取字符串
			```sql
			# substr
			# 截取从第6个字符开始，其后所有字符
			SELECT SUBSTR('this is a string', 6) output;
			| output
			| is a string
			```  
			```sql
			# 截取从从第1个字符开始，共4个字符
			SELECT SUBSTR('this is a string', 1, 4) output;
			| output
			| this
			```  
			**注意**
			> SQL中字符串索引从1开始。
		- `INSTR`
			```sql
			# instr
			# 返回子串第一次出现的位置索引
			SELECT INSTR('this is a string', 'str') output;
			```
		- `TRIM`
			```sql
			# trim
			# 返回前后去除某字符后的字符串长度
			SELECT LENGTH(TRIM('   abc  ')) as output1;
			SELECT LENGTH(TRIM('a' FROM 'aaaaabcaaa')) as output2;
			```
			**注意**
			> 如果参数为字符串，则默认判断除去前后空格后的字符串长度，否则判断去除前后指定字符（如'a'）后的长度。
		- 填充字符串
			```sql
			# lpad左填充，rpad右填充
			# 用'*'向左侧填充字符串，使字符串最终长度为10
			SELECT LPAD('a',10,'*') as output;
			# 用'*'向右侧填充字符串，使字符串最终长度为10
			SELECT RPAD('a',10,'*') as output;
			```
			**注意**
			> 如果长度参数值小于原始字符串，则会将原始字符串从右侧截断。
		- 替换字符串
			```sql
			# replace
			# 将字符串中所有的"char"都替换为"string"
			SELECT REPLACE('they are char1 and char2', 'char', 'string') as output;
			```
	- 数学函数举例:
		- 四舍五入
			```sql
			# round
			# 四舍五入至整数位
			SELECT ROUND(1.55);
			| 2
			SELECT ROUND(-1.55);
			| -2
			# 四舍五入至小数点后2位
			SELECT ROUND(1.537, 2);
			| 1.54
			```
		- 取整
			```sql
			# ceil 向上取整，返回>=参数值的最小整数
			SELECT CEIL(1.2);
			| 2
			SELECT CEIL(-1.2);
			| -1
			# floor 向下取整，返会<=参数值的最大整数
			SELECT FLOOR(1.78);
			| 1
			SELECT FLOOR(-1.24);
			| -2
			# truncate 截断
			SELECT TRUNCATE(1.261, 1);
			| 1.2
			# mod 取余
			SELECT MOD(10,3);
			| 1	
			```
	- 日期函数举例：
		- 当前日期和时间
			```sql
			# now 返回当前系统日期和时间
			SELECT NOW();
			```
		- 当前日期
			```sql
			# curdate 返回当前系统日期
			SELECT curdate();	
			```
		- 当前时间
			```sql
			# curtime 返回当前系统时间
			SELECT curtime();
			```
		- 获取指定部分
			```sql
			SELECT YEAR(NOW()) 年;
			SELECT YEAR('1998-1-1') 年;
			
			SELECT MONTH(NOW()) 月;
			| 9
			SELECT MONTHNAME(NOW()) 月;
			| September
			```
		- 将指定格式字符串转换为日期
			```sql
			# str_to_date
			SELECT STR_TO_DATE('3-2-1998', '%c-%d-%Y');
			| 1998-03-02
			```
		- 将日期转换为字符
			```sql
			SELECT DATE_FORMAT(NOW(), '%m/%d/%Y');
			| 09/05/2019
			```
	- 其他函数
		- `version()` 版本号
		- `datebase()` 当前数据库
		- `user()` 当前用户
	- 流程控制函数
		- `IF`函数
			```sql
			SELECT
				last_name, commission_pct, 
				IF(commission_pct IS NULL, '无奖金', '有奖金') 备注
			FROM
				employees
			```
		- `CASE`函数使用1
			语法：   
				`case 要判断的字段或表达式`   
				`when 常量1 then 要显示的值或语句`  
				`when 常量1 then 要显示的值或语句`  
				`...`  
				`else 要显示的值或语句`  
				`end`
			```sql
			SELECT
				salary 原始工资, department_id,
				case department_id
				when 30 then salary*1.1
			    when 40 then salary*1.2
			    when 50 then salary*1.3
			    else salary
			    end 新工资
			FROM
				employees;
			```
		- `CASE`函数使用2  
			`case`  
			`when 条件1 then 要显示的值或语句`  
			`when 条件2 then 要显示的值或语句`  
			`...`  
			`else 要显示的值或语句`  
			`end`
			```sql
			SELECT
				salary,
			    case
			    when salary>20000 then 'A'
			    when salary>15000 then 'B'
			    when salary>10000 then 'C'
			    else 'D'
			    end 级别
			FROM
				employees;
			```
	- 分组函数：
		- `sum` 求和，`avg` 求平均值，`max` 最大值，`min` 最小值，`count` 计数：
			```sql
			SELECT SUM(salary) FROM employees;
			SELECT AVG(salary) FROM employees;
			SELECT SUM(salary), MAX(salary) FROM employees;
			```
		- 特点：
			1. 一般`sum`和`avg`用于处理数值型（其他类型不报错但无意义），`max`、`min`和`count`可以处理任何类型（数值型、日期型、字符型等）；
			2. 以上分组函数均忽略`NULL`值；
			3. 以上分组函数均能与`distinct`搭配使用实现去重运算
				```sql
				SELECT SUM(DISTINCT salary), SUM(salary) FROM employees;
				| 397900.00, 691400.00
				```
			4. `count`函数：
				```sql
				两种用法对比
				SELECT COUNT(salary) FROM employees;
				SELECT COUNT(*) FROM employees;  # 用来计数所有记录（效率最高，一般使用该方式）
				SELECT COUNT(常量值) FROM employees;  # 用来计数所有记录（相当于加了一列全为该常量值并统计个数，相当与统计所有记录个数）
				```
			5. 与分组函数一同查询的字段要求是`group by`后出现的字段。
5. 分组查询
	- 语法：  
		`select 分组函数, 列(必须出现在group by的后面)`  
		`from 表`  
		`【where 筛选条件】`  
		`group by 分组列表`   
		`【having 筛选条件】`  
		`【order by 排序依据】`
	- 注意：  
		要求查询列表只能从分组函数和group by后出现的字段中选择。
	- 用法举例：
		- 简单分组查询
			```sql
			# 查询每个工种的最高工资
			SELECT MAX(salary)
			FROM employees
			GROUP BY job_id;
			```
		- 添加`where`筛选条件
			```sql
			# 查询邮箱中包含a字符的每个部门的平均工资
			SELECT department_id, AVG(salary)
			FROM employees
			WHERE email LIKE '%a%'
			GROUP BY department_id;
			```
		- 利用`having`筛选
			```sql
			# 查询员工个数大于2的部门
			# 1 查询每个部门的员工个数
			# 2 根据1的结果进行筛选
			SELECT COUNT(*), department_id
			FROM employees
			GROUP BY department_id
			HAVING COUNT(*) > 2;
			```
			**注意**
			> `having`用于对**分组后**的结果进行筛选，而`where`是作用于**分组之前**。
			> ```sql
			> # 查询每个工种有奖金的员工最高工资,且最高工资大于12000
			> SELECT  job_id, MAX(salary)
			> FROM employees
			> WHERE commission_pct IS NOT NULL
			> GROUP BY job_id
			> HAVING MAX(salary) > 12000;
			> ```
			> 由上可见，当分组函数作为条件是用到的筛选子句为`having`。考虑到性能问题，一般来说，能用`where`进行分组前筛选时尽量选择分组前筛选，比如当以分组条件作为筛选条件时。
		- 按表达式或函数进行分组
			```sql
			# 按员工姓名长度分组，查询每一组的员工个数，筛选出员工个数大于5的组
			SELECT COUNT(*) 个数, LENGTH(last_name) 长度
			FROM employees
			GROUP BY LENGTH(last_name)
			HAVING COUNT(*) > 5;
			```
		- 按多个字段分组
			```sql
			# 查询每个部门每个工种的员工的平均工资
			SELECT AVG(salary), department_id, job_id
			FROM employees
			GROUP BY department_id, job_id;
			```
		- 添加排序
			```sql
			# 查询编号不为空的部门中平均工资大于10000的员工平均工资, 并按平均工资升序排序
			SELECT AVG(salary)
			FROM employees
			WHERE department_id IS NOT NULL
			GROUP BY department_id
			HAVING AVG(salary)
			ORDER BY AVG(salary) ASC;
			```
6. 连接查询（多表查询）
	- 简介：当查询字段来自多个表时会使用连接查询。
	- 笛卡尔乘积现象：表1有m行，表2有n行，查询结果出现m*n行，原因是没有有效的连接条件。
	- 连接查询分类：
		- 按功能分类：内连接（包括等值连接、非等值连接和自连接）、外连接（左外连接、右外连接和全外连接）、交叉连接；
		- 按年代分类：sql92标准（在mysql中仅支持内连接）和sql99标准（支持内连接、外连接中的左右外连接及交叉连接）。
	- sql92标准举例：
		- 等值连接  
			特点：  
				1. 多表等值连接的结果为多表的交集部分；  
				2. n表连接要求至少需要n-1个连接条件；  
				3. 多表的顺序没有要求。
			```sql
			# 查询员工名及对应的部门名
			SELECT last_name, department_name
			FROM employees, departments
			WHERE employees.department_id = departments.department_id;
			```
		    **为表起别名**  
			当表名比较长时，可以采用类似为字段起别名的方式给表起一个别名
			```sql
			# 查询工种号、员工名、工种名
			SELECT e.last_name, e.job_id, j.job_title
			FROM employees AS e, jobs j
			WHERE e.job_id = j.job_id;
			```
			**注意**
			> 为表起别名后，将无法使用原始表名进行查询。
			
			**带筛选条件的连接查询**
			```sql
			# 查询有奖金的员工名和对应的部门名
			SELECT last_name, department_name, commission_pct
			FROM employees e, departments d
			WHERE e.department_id=d.department_id AND e.commission_pct IS NOT NULL;
			```
			**带分组的连接查询**
			```sql
			# 查询每个城市的部门个数
			SELECT city, COUNT(*)
			FROM departments d, locations l
			WHERE d.location_id = l.location_id
			GROUP BY city;
			```
			**带排序的连接查询**
			```sql
			# 查询每个工种的工种名和员工的个数，并按员工个数降序排序
			SELECT job_title, COUNT(*)
			FROM jobs j, employees e
			WHERE j.job_id = e.job_id
			GROUP BY job_title
			ORDER BY COUNT(*) DESC;
			```
			**多表连接**
			```sql
			# 查询员工名和对应的部门名及所在城市
			SELECT last_name, department_name, city
			FROM employees e, departments d, locations l
			WHERE e.department_id = d.department_id AND d.location_id = l.location_id;
			```
		- 非等值连接  
			与等值连接的区别仅在于连接条件的变化。
			```sql
			# 查询员工的工资和工资级别
			SELECT salary, grade_level
			FROM employees e, job_grades jg
			WHERE e.salary >= jg.lowest_sal AND e.salary < jg.highest_sal;
			```
		- 自连接
			```sql
			# 查询员工名和对应的上级的名
			SELECT e.employee_id, e.last_name, m.employee_id, m.last_name
			FROM employees e, employees m
			WHERE e.manager_id = m.employee_id;
			```	
	- sql99标准举例:
		- 语法  
			`select 查询列表`  
			`from 表1 别名`  
			`【连接类型】 join 表2 别名 on 连接条件`   
			`【where 筛选条件】`  
			`【group by 分组条件】`  
			`【having 筛选条件】`  
			`【order by 排序条件】`
		- 连接类型关键字：`【inner】` 内连接、`left 【outer】` 左外、`right 【outer】` 右外、`full 【outer】` 全外、`cross` 交叉连接
		- 内连接（`inner`可省略）  
			得到多个表的交集部分。
			```sql
			# 查询员工个数大于3的部门名和员工个数，并按个数降序排序（综合）
			SELECT department_name, COUNT(*)
			FROM employees e
			INNER JOIN departments d
			ON e.department_id = d.department_id
			GROUP BY department_name
			HAVING COUNT(*) > 3
			ORDER BY COUNT(*) DESC;
			
			# 查询员工名、部门名、工种名，并按部门名排序（多表连接）
			SELECT last_name, department_name, job_title
			FROM employees e
			INNER JOIN departments d ON e.department_id = d.department_id
			INNER JOIN jobs j ON e.job_id = j.job_id
			ORDER BY department_name ASC;

			# 查询员工的工资级别（非等值连接）
			SELECT salary, grade_level
			FROM employees e
			INNER JOIN job_grades jg
			ON e.salary BETWEEN jg.lowest_sal AND jg.highest_sal;
			
			# 查询员工名及其上级领导名（自连接）
			SELECT e.last_name, m.last_name
			FROM employees e
			INNER JOIN employees m
			ON e.manager_id = m.employee_id;
			```
		- 外连接（outer可以省略）  
			用于查询一个表中有而另一个表中没有的情况。  
			特点：外连接的查询结果是主表中的所有记录，如果从表中有与之匹配的记录则显示值，否则，显示`null`；在左外连接中，`left join`左边的是主表，右外连接中，`right join`右边的是主表；左外和右外交换两个表的顺序可以实现同样的效果；全外连接可以查询表1中有而表2中没有的记录及表2中有而表1中没有的记录（sql99不支持）。  
			**注意**
			> 一般来说，查询结果是哪个表中的信息，则设该表为主表。
			```sql
			# 查询哪个部门没有员工
			# 左外连接
			SELECT d.*
			FROM departments d
			LEFT OUTER JOIN employees e
			ON d.department_id = e.department_id
			WHERE e.employee_id IS NULL;

			# 右外连接
			SELECT d.*
			FROM employees e
			RIGHT OUTER JOIN departments d
			ON d.department_id = e.department_id
			WHERE e.employee_id IS NULL;
			```
		- 交叉连接  
			结果为两个表的笛卡尔乘积。
			```sql
			# 查询没有男友的女明星
			SELECT b.*, bo.*
			FROM beauty b
			CROSS JOIN boys bo;
			```