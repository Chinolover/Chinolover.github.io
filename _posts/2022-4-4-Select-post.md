---
layout: post
title: Select
category: Mysql
thumbnail: /style/image/thumbnail.png
icon: book
---


* content
  {:toc}



[TOC]

### 基本Select

![image-20220804163412528](images/image-20220804163412528.png)



```mysql
-- 查询表中所有学生的信息。
SELECT * FROM student;
-- 查询表中所有学生的姓名和对应的英语成绩。
SELECT `name`,english FROM student;
-- 过滤表中重复数据distinct
SELECT DISTINCT english FROM student;
-- 要查询的记录，每个字段都相同，才会去重
SELECT DISTINCT `name`,english FROM student; 

-- 统计每个学生的总分
SELECT `name`,(english+math+chinese) FROM student;
-- 在所有学生总分加10分的情况
SELECT `name`,(english+math+chinese+10) FROM student;
-- 使用别名表示学生分数。
SELECT `name` AS '名字',(english+math+chinese) AS total_score FROM student;
```

### Select  where常用语法

![image-20220804170732592](images/image-20220804170732592.png)

```mysql
-- 查询姓名为赵云的学生成绩
SELECT * FROM student WHERE `name`='赵云';
-- 查询英语成绩大于90分的同学
SELECT `name`,english FROM student WHERE english>90;
-- 查询总分大于200分的所有同学
SELECT `name`,(english+math+chinese) AS total FROM student WHERE (english+math+chinese)>200;

-- 查询math大于60并且id大于4的学生成绩
SELECT * FROM student 
					WHERE id>4 AND math>60; 
-- 查询英语成绩大于语文成绩的同学
SELECT * FROM student 
					WHERE english>chinese; 
-- 查询总分大于200分并且数学成绩小于语文成绩,的姓赵的学生.
-- 赵%表示 名字以赵开头的就可以
SELECT * FROM student 
					WHERE (english+math+chinese)>200 AND math<chinese
					AND `name` LIKE '赵%';
					
-- 1.查询英语分数在80 - 90之间的同学。
-- between是闭区间
SELECT * FROM student
					WHERE english BETWEEN 80 AND 90;
-- 2.查询数学分数为89,90,91的同学。
SELECT * FROM student
					WHERE math IN(89,90,91);
-- 3.查询所有姓张的学生成绩。
SELECT * FROM student
					WHERE `name` LIKE '张%';
-- 4.查询数学分> 80,语文分> 80的同学。
SELECT * FROM student
					WHERE math>80 AND chinese>80;
```

###        order by

![image-20220810102407994](images/image-20220810102407994.png)

```mysql
-- 演示order by使用
-- 对数学成绩排序后输出[升序]。
SELECT * FROM student
				ORDER BY math;	-- 默认升序
-- 对总分按从高到低的顺序输出
SELECT `name`,(chinese+math+english) AS total FROM student
				ORDER BY total DESC;
-- 对姓李的学生成绩排序输出(升序)
SELECT `name`,(chinese+math+english) AS total FROM student
				WHERE `name` LIKE '李%' ORDER BY total;
```

### MAX，MIN，AVG，SUM，COUNT统计函数

```mysql
-- 演示统计函数

-- 统计一个班级共有多少学生?
SELECT COUNT(*) FROM student;
-- 统计数学成绩大于90的学生有多少个?
SELECT COUNT(*) FROM student
					WHERE math > 90;
-- 统计总分大于250的人数有多少?
SELECT COUNT(*) FROM student
					WHERE (chinese + math + english) > 250;
-- count(*)和count(列)的区别

-- count(*)会统计所有的列信息包括null
-- count(列)则不包括null


-- 演示sum函数的使用(仅对数值型有效)
-- 统计一个班级数学总成绩?
SELECT SUM(math) FROM student;
-- 统计一个班级语文、英语、数学各科的总成绩
SELECT SUM(chinese),SUM(math),SUM(english) FROM student;
-- 统计一个班级语文、英语、数学的成绩总和
SELECT SUM(chinese + math + english) FROM student;
-- 统计一个班级语文成绩平均分
SELECT SUM(chinese) / COUNT(*) FROM student;



-- 演示avg的使用
-- 求一个班级数学平均分?
SELECT AVG(math) FROM student;
-- 求一个班级总分平均分
SELECT AVG(chinese + math + english) FROM student;

-- 求出班级最高分以及最低分
SELECT MAX(chinese + math + english) AS high ,MIN(chinese + math + english) AS low FROM student;

```

### Group by

![image-20220821154157797](images/image-20220821154157797.png)

```mysql
CREATE TABLE dept( /*部门表*/
deptno MEDIUMINT   UNSIGNED  NOT NULL  DEFAULT 0, 
dname VARCHAR(20)  NOT NULL  DEFAULT "",
loc VARCHAR(13) NOT NULL DEFAULT ""
);

INSERT INTO dept VALUES(10, 'ACCOUNTING', 'NEW YORK'), (20, 'RESEARCH', 'DALLAS'), (30, 'SALES', 'CHICAGO'), (40, 'OPERATIONS', 'BOSTON');


SELECT * FROM salgrade;

#创建表EMP雇员
CREATE TABLE emp
(empno  MEDIUMINT UNSIGNED  NOT NULL  DEFAULT 0, /*编号*/
ename VARCHAR(20) NOT NULL DEFAULT "", /*名字*/
job VARCHAR(9) NOT NULL DEFAULT "",/*工作*/
mgr MEDIUMINT UNSIGNED ,/*上级编号*/
hiredate DATE NOT NULL,/*入职时间*/
sal DECIMAL(7,2)  NOT NULL,/*薪水*/
comm DECIMAL(7,2) ,/*红利*/
deptno MEDIUMINT UNSIGNED NOT NULL DEFAULT 0 /*部门编号*/
);

 
 INSERT INTO emp VALUES(7369, 'SMITH', 'CLERK', 7902, '1990-12-17', 800.00,NULL , 20), 
(7499, 'ALLEN', 'SALESMAN', 7698, '1991-2-20', 1600.00, 300.00, 30),  
(7521, 'WARD', 'SALESMAN', 7698, '1991-2-22', 1250.00, 500.00, 30),  
(7566, 'JONES', 'MANAGER', 7839, '1991-4-2', 2975.00,NULL,20),  
(7654, 'MARTIN', 'SALESMAN', 7698, '1991-9-28',1250.00,1400.00,30),  
(7698, 'BLAKE','MANAGER', 7839,'1991-5-1', 2850.00,NULL,30),  
(7782, 'CLARK','MANAGER', 7839, '1991-6-9',2450.00,NULL,10),  
(7788, 'SCOTT','ANALYST',7566, '1997-4-19',3000.00,NULL,20),  
(7839, 'KING','PRESIDENT',NULL,'1991-11-17',5000.00,NULL,10),  
(7844, 'TURNER', 'SALESMAN',7698, '1991-9-8', 1500.00, NULL,30),  
(7900, 'JAMES','CLERK',7698, '1991-12-3',950.00,NULL,30),  
(7902, 'FORD', 'ANALYST',7566,'1991-12-3',3000.00, NULL,20),  
(7934,'MILLER','CLERK',7782,'1992-1-23', 1300.00, NULL,10);



#工资级别表
CREATE TABLE salgrade
(
grade MEDIUMINT UNSIGNED NOT NULL DEFAULT 0,
losal DECIMAL(17,2)  NOT NULL,
hisal DECIMAL(17,2)  NOT NULL
);

INSERT INTO salgrade VALUES (1,700,1200);
INSERT INTO salgrade VALUES (2,1201,1400);
INSERT INTO salgrade VALUES (3,1401,2000);
INSERT INTO salgrade VALUES (4,2001,3000);
INSERT INTO salgrade VALUES (5,3001,9999);


```

### 字符串相关函数

![image-20220821214540889](images/image-20220821214540889.png)

```mysql
-- 字符串函数

-- CHARSET(str)  返回字串字符集
SELECT CHARSET(ename) FROM emp;

-- CONCAT (string2 [... ])  连接字串
SELECT CONCAT(ename,'的工作是',job) FROM emp;

-- INSTR (string ,substring )  返回substring在string中出现的位置没有返回0
SELECT INSTR('where is my wife','wife') FROM DUAL;
-- DUAL 亚原表，测试用

-- UCASE (string2 )  转换成大写
SELECT UCASE('where is my wife') FROM DUAL;

-- LCASE (string2 )  转换成小写
SELECT LCASE(ename),LCASE(job) FROM emp;

-- LEFT (string2， length )  从string2中的左边起取length个字符
-- RIGHT (string2， length )  从string2中的右边起取length个字符
SELECT LEFT(ename,2) FROM emp;
SELECT RIGHT(ename,2) FROM emp;

-- LENGTH (string )  string长度[按照字节]
SELECT LENGTH('王兴华') FROM emp;

-- REPLACE (str ,search_str,replace_str)  在str中用replace_ str 替换search_ str
SELECT ename,REPLACE(job,'MANAGER','经理') FROM emp;

-- STRCMP (string1 ,string2 )  逐字符比较两字串大小，
SELECT STRCMP(ename,ename) from emp;

-- SUBSTRING (str，position [,length])  从str的position开始[从1开始计算] ,取length个字符
SELECT SUBSTRING(ename,1,2) FROM emp;

-- LTRIM (string2 ) RTRIM (string2 )   trim(string)  去除前端空格或后端空格
SELECT LTRIM('        窝嫩叠         123   ') FROM DUAL;
SELECT RTRIM('        窝嫩叠         123   ') FROM DUAL;
SELECT TRIM('        窝嫩叠         123             ') FROM DUAL;  -- 去除前后端空格

-- 以首字母小写的方式显示所有员Temp表的姓名
SELECT REPLACE(ename,LEFT(ename,1),LCASE(LEFT(ename,1))) FROM emp;	

```

### 数学函数

![image-20220903171437586](images/image-20220903171437586.png)

```mysql
-- 数学函数

-- ABS(num)  绝对值
SELECT ABS(-1.3) FROM DUAL;
-- BIN (decimal_ number )  十进制转二进制
SELECT BIN(10) FROM DUAL;
-- CEILING (number2 )  向上取整，得到比num2大的最小整数
SELECT CEILING(2.3) from DUAL;
-- CONV( number2,from_ base,to_ base)  进制转换
SELECT CONV(10,2,10) FROM DUAL;
-- FLOOR (number2 )  向下取整，得到比num2小的最大整数
SELECT FLOOR(2.3) FROM DUAL;
-- FORMAT (number,decimal_ _places )  保留小数位数
SELECT FORMAT(123.45678,2) FROM DUAL;
-- HEX (DecimalNumber )  转十六进制
SELECT HEX(32) FROM dual;
-- LEAST (number，number2 [,.])  求最小值
SELECT LEAST(0,3,-2,13.3,4) FROM DUAL;
-- MOD (numerator ,denominator )  求余
SELECT MOD(10,3) FROM DUAL;
-- RAND([seed])
SELECT RAND(2) FROM DUAL;
-- RAND([seed])其范围为0≤v≤1.0
```



### 时间日期相关函数 

![image-20220822171433473](images/image-20220822171433473.png)

```mysql
-- 时间日期相关函数

CREATE TABLE mes(
		id INT,
		content VARCHAR(30),
		send_time DATETIME);
		
INSERT INTO mes
				VALUES (1,'北京时间',CURRENT_TIMESTAMP());
INSERT into mes VALUES(2,'北京时间',NOW());
INSERT into mes VALUES(3,'广州世界',NOW());
UPDATE mes
		SET content = '广州时间'
		WHERE id = 3;

SELECT * FROM mes;
		
		

-- CURRENT_DATE ( )   当前日期
SELECT CURRENT_DATE FROM DUAL;
-- CURRENT_TIME ( )   当前时间
SELECT CURRENT_TIME FROM DUAL;
-- CURRENT_TIMESTAMP( )  当前时间戳
SELECT CURRENT_TIMESTAMP from DUAL;

SELECT NOW() FROM DUAL;

-- 显示所有留言信息， 发布日期只显示日期，不用显示时间.
SELECT id,content,DATE(send_time) FROM mes; 
-- 请查询在10分钟内发布的帖子
SELECT * FROM mes WHERE DATE_ADD(send_time,INTERVAL 10 MINUTE) >= NOW(); -- YEAR，MINUTE，SECOND，DAY，HOUR......
-- 请在mysql的sq|语句中求出2011-11-11和1990-1-1相差多少天
SELECT DATEDIFF('2011-11-11','1990-1-1') FROM DUAL;
-- 请用mysq|的sql语句求出你活了多少天? [练习]
SELECT DATEDIFF(CURRENT_DATE,'2001-3-17') from DUAL;
-- 如果你能活80岁，求出你还能活多少天.[练习]
SELECT DATEDIFF(DATE_ADD('2001-3-17',INTERVAL 80 YEAR),CURRENT_DATE) FROM DUAL;


-- DATE (datetime )   返回datetime的日期部分
-- DATE_ ADD (date2 , INTERVAL d_value d_type )  在date2中加上日期或时间
-- DATE_ SUB (date2 , INTERVAL d_ value d_ type )  在date2_上减去1个时间
-- DATEDIFF (date1 ,date2 )  两个日期差(结果是天)
-- TIMEDIFF(date1,date2)   两个时间差(多少小时多少分钟多少秒)
-- NOW()   当前时间
-- YEAR| Month | DATE (datetime )FROM_ UNIXTIME()   年月日
SELECT YEAR(NOW()) FROM DUAL;
SELECT MONTH(NOW()) FROM DUAL;
SELECT DAY(NOW()) FROM DUAL;  -- 类似强制转换

-- 返回的是1970-1-1到现在的秒数
SELECT UNIX_TIMESTAMP() FROM DUAL;

-- %Y-%m-%d %H:%i:%s 为固定格式，自己看需求使用	，前面参数为1970-1-1到现在的秒数
SELECT FROM_UNIXTIME(1618483484,'%Y-%m-%d %H:%i:%s') FROM DUAL;

 

```

### PWD加密函数

![image-20220823152009728](images/image-20220823152009728.png)

```mysql
	-- 演示加密函数和系统函数
	
	-- USER()查询用户  可以查询登录到mysql的有哪些用户以及登录的IP
	SELECT USER() FROM DUAL;
	-- DATABASE()数据库名称
	SELECT DATABASE() FROM dual;
	
	-- MD5(str) 为字符串算出一个MD5 32的字符串，常用(用户密码)加密
	-- root密码是hsp->加密md5->在数据库中存放的是加密后的密码
	SELECT MD5('wxh') FROM DUAL;	
	SELECT LENGTH(MD5('wxh')) FROM DUAL;
	
	CREATE TABLE wxh_user(
				id INT,
				`name` VARCHAR(32) NOT NULL DEFAULT '',
				pwd CHAR(32) NOT NULL DEFAULT '');		-- 回忆之前VARCHAR是变长，CHAR是定长的
				
	INSERT INTO wxh_user VALUES (100,'王兴华',MD5('wxh0317**..'));
	SELECT * FROM wxh_user;
	
	-- 需要查询用户的时候如下
	SELECT * FROM wxh_user WHERE `name`='王兴华' AND pwd=MD5('wxh0317**..');
	
	-- PASSWORD(str) -- 加密函数，MySQI数 据库的用户密码就是PASSWORD函数加密
SELECT PASSWORD('hsp') FROM DUAL; --数据库的* 81220D972A52D4C51BB1C37518A2613706220DAC
-- select * from mysql.user \G 从原文密码str计算并破回密码字符串
通常用于对mysql数据库的用户密码加密
-- mysql . user
表示数据库.表
SELECT * FROM mysql.user
```

### 流程控制函数

![image-20220824150504385](images/image-20220824150504385.png)

```mysql
-- 流程控制函数

-- IF(expr1,expr2,expr3)				如果expr1为True ,则返回expr2否则返回expr3
SELECT IF(TRUE,'上海','北京') from DUAL;
-- IFNULL(expr1,expr2)    如果expr1不为空NULL,则返回expr1,否则返回expr2
SELECT IFNULL(NULL,'天津') FROM DUAL;
SELECT IFNULL('河南','三门峡') FROM DUAL;
-- SELECT CASE WHEN expr1 THEN expr2
-- 		WHEN expr3 THEN expr4 ELSE expr5
-- 				END; [类似多重分支.]
-- 如果expr1为TRUE,则返回expr2,如果expr2
-- 为t,返回expr4,否则返回expr5
SELECT CASE 
	WHEN FALSE THEN
		'Jack'
		WHEN TRUE THEN 
		'Maria'
	ELSE
		'Michacl'
END;

-- 查询emp表，如果comm是null ,则显示0.0
SELECT IF(comm IS NULL,0,comm) FROM emp;		-- 判断为null时候，不能用= 要用is
-- 如果emp表的job是CLERK则显示职员，如果是MANAGER则显示经理
-- 如果是SALESMAN则显示销售人员，其它正常显示.
SELECT ename,(SELECT CASE
								WHEN job = 'CLERK' THEN '职员'
								WHEN job = 'MANAGER' THEN '经理'
								WHEN job = 'SALESMAN' THEN '销售人员'
								ELSE job END) AS 'job'
				FROM emp;






```

### 查询增强  

```mysql
-- 查询加强

-- 使用where子句
-- ?如何查找1992.1.1后入职的员工
SELECT * FROM emp WHERE hiredate>'1992-1-1';
-- 如何使用like操作符
-- %:表示0到多个字符  _:表示单个字符
-- ?如何显示首字符为S的员工姓名和工资 	
SELECT ename,sal FROM emp WHERE ename LIKE 'S%';
-- ?如何显示第三个字符为大写o的所有员工的姓名和工资
SELECT ename,sal FROM emp WHERE ename LIKE '__O%';
-- 如何显示没有上级的雇员的情况
SELECT * FROM emp WHERE mgr IS NULL;

-- 查询表结构
DESC emp;

-- 如何按照工资的从高到低的顺序，显示雇员的信息
SELECT * FROM emp ORDER BY sal DESC;
-- 按照部门号升序而雇员的工资降序排列,显示雇员信息
SELECT * FROM emp ORDER BY deptno,sal DESC; 
```

![image-20220824192454187](images/image-20220824192454187.png)

```mysql
-- 分页查询
-- 按雇员的id号升序取出，每页显示3条记录， 请分别显示第一页，第二页，第三页
-- 基本语法: select ... limit start, rows
-- 表示从start+1行开始取，取出rows行，start从0开始计算

-- 按雇员的id号降序取出，每页显示5条记录。 请分别显示第3页，第5页对应的sql语句
SELECT * FROM emp -- 第一页
			ORDER BY empno DESC
			LIMIT 0,3;			-- 从第0+1记录开始，一页读取3条数据
			
SELECT * FROM emp  -- 第二页
			ORDER BY empno DESC
			LIMIT 3,3;			-- 从第0+1记录开始，一页读取3条数据			
			
SELECT * FROM emp  -- 第三页
			ORDER BY empno DESC
			LIMIT 6,3;			-- 从第0+1记录开始，一页读取3条数据		
```

### Group by增强

```mysql
-- 使用分组函数和分组子句group by

-- 显示每种岗位的雇员总数、 平均工资。
SELECT COUNT(*),job,AVG(sal) FROM emp GROUP BY job;
-- 显示雇员总数，以及获得补助的雇员数。
SELECT COUNT(*),COUNT(comm) FROM emp;	-- 因为count对列不统计NULL
-- 统计没有获得补助的雇员数
SELECT COUNT(IF(comm IS NULL,1,NULL)) FROM emp;
-- 显示管理者的总人数。
SELECT COUNT(DISTINCT mgr ) FROM emp;	-- 去重
-- 显示雇员工资的最大差额。
	SELECT MAX(sal) - MIN(sal) FROM emp;

```

### 多表查询

```mysql
-- 多表查询

-- 在默认情况下:当两个表查询时,规则
-- 1.从第一张表中，取出一行和第二张表的每一行
-- 进行组合，返回结果[含有两张表的所有列].
-- 2.一共返回的记录数第一张表行数第二张表的行数
-- 3.这样多表查询默认处理返回的结果，称为笛卡尔集
-- 4.解决这个多表的关键就是要写出正确的过滤条件where
Select * from emp,dept;		-- 结果被称为笛卡尔集

-- ?显示雇员名雇员工资及所在部门的名字[笛卡尔集]
Select * from emp,dept;	

-- 老韩小技巧:多表查询的条件不能少于表的个数-1,否则会出现笛卡尔集

-- ?如何显示部门号为10的部门名、员工名和工资
SELECT ename,sal,dname FROM emp,dept WHERE emp.deptno = dept.deptno;
-- ?显示各个员工的姓名，工资，及其工资的级别
```

### 自连接 

```mysql
-- 多表查询 自连接

-- 思考题:显示公司员工名字和他的上级的名字
SELECT worker.ename AS '职员名',boss.ename AS '上级名' 
								FROM emp worker,emp boss 
								WHERE worker.mgr = boss.empno;
								
-- 自连接特点：
		1.把一张表当两张表用
		2.需要给表起别名   注意格式：表名 表别名
		3.如果列名不明确，可以指定列的别名
```

### 子查询

```mysql
-- ●什么是子查询
-- 子查询是指嵌入在其它sq|语句中的select语句，也叫嵌套查询
-- ●单行子查询
-- 单行子查询是指只返回一行数据的子查询语句

-- 如何显示与SMITH同一部门的所有员工?
SELECT * FROM emp
					WHERE deptno=(
						SELECT deptno FROM emp
										WHERE ename = 'SMITH');
-- ●多行子查询
-- 多行子查询指返回多行数据的子查询使用关键字 in

-- 课堂练习:如何查询和部门10的工作相同的雇员的名字、岗位、工资、部门号，但是不含10自己的. 
SELECT ename,job,sal,deptno FROM emp
					WHERE job IN (
							SELECT DISTINCT job FROM emp
								WHERE deptno = 10) AND deptno != 10; 
```

### 子查询临时表	

```mysql
-- 查询不同工作的最高工资人员
-- 子查询临时表	
SELECT ename,ls.job,sal FROM(		-- 这里注意一定要ls.job 不然就会出现歧义	
		SELECT job,MAX(sal)AS maxsal FROM emp
		GROUP BY job) ls,emp
				WHERE ls.maxsal = emp.sal AND ls.job = emp.job;
```

### ALL、ANY

```mysql
-- 在多行子查询中使用all操作符
-- 请思考:如何显示工资比部门30的所有员工的工资高的员工的姓名、工资和部门号
SELECT ename,sal,deptno FROM emp
		WHERE sal>(SELECT MAX(sal) FROM 
															emp WHERE deptno = 30)
															
SELECT ename,sal,deptno FROM emp
		WHERE sal>ALL(SELECT sal FROM 
															emp WHERE deptno = 30)															
															
-- 在多行子查询中使用any操作符
-- 请思考:如何显示工资比部门30的其中一个员工的工资高的员工的姓名、工资和部门号												
SELECT ename,sal,deptno FROM emp
		WHERE sal>(SELECT MIN(sal) FROM 
															emp WHERE deptno = 30)
															
SELECT ename,sal,deptno FROM emp
		WHERE sal>ANY(SELECT sal FROM 
															emp WHERE deptno = 30)	

```

### 多列子查询

![image-20220826212852485](images/image-20220826212852485.png) 

```mysql
-- 请思考如何查询与smith的部门和岗位完全相同的所有雇员(并且不含smith本人)
-- 之前的方法
SELECT ename,emp.deptno,emp.job from 
							(SELECT deptno,job 
									FROM emp 
									WHERE ename = 'SMITH')ls, emp
		where emp.deptno=ls.deptno AND emp.job = ls.job;
		
-- 多列子查询
SELECT * FROM emp 
				WHERE (deptno,job) = (
					SELECT deptno,job FROM emp
							WHERE ename = 'smith') AND ename!= 'smith';

```

### 练习

```mysql
-- 查找每个部门工资高于本部门平均工资的人的资料

SELECT deptno,AVG(sal) FROM emp
			GROUP BY deptno;

SELECT ename,emp.deptno,sal FROM (SELECT deptno,AVG(sal) AS avgsal FROM emp
			GROUP BY deptno) ls,emp
			WHERE emp.deptno = ls.deptno AND emp.sal > avgsal;
			
-- 查找每个部门工资最高的人的详细资料
	SELECT * FROM emp
					WHERE sal IN (SELECT MAX(sal) FROM emp
	GROUP BY deptno)
	
-- 	查询每个部门的信息(包括:部门名,编号，地址)和人员数量,我们一起完成
SELECT dname,temp.deptno,loc,sum_ FROM (  
					SELECT deptno,COUNT(ename)AS sum_ FROM emp
	GROUP BY deptno)temp,dept 
					WHERE temp.deptno = dept.deptno;
					
-- 还有一种写法表 .*表示将该表所有列都显示出来
SELECT dept.*,sum_ FROM (  -- 这样也可以
					SELECT deptno,COUNT(ename)AS sum_ FROM emp
	GROUP BY deptno)temp,dept 
					WHERE temp.deptno = dept.deptno;
```

### 合并查询

```mysql
-- 合并查询

SELECT ename,sal,job FROM emp WHERE sal>2500;

SELECT ename,sal,job FROM emp WHERE job = 'MANAGER';

-- union all 就是将两个查询结果合并，不会去重
SELECT ename,sal,job FROM emp WHERE sal>2500
UNION ALL
SELECT ename,sal,job FROM emp WHERE job = 'MANAGER';

-- union 就是将两个查询结果合并，会去重
SELECT ename,sal,job FROM emp WHERE sal>2500
UNION 
SELECT ename,sal,job FROM emp WHERE job = 'MANAGER';
```

### 外连接

```mysql
-- 外连接

-- 列出部门名称和这些部门的员工名称和工作，同时要求显示出那些没有员工的部门。

-- 外连接
-- 左外连接 (如果左侧的表完全显示我们就说是左外连接)
SELECT ename,dname,job FROM dept LEFT JOIN emp ON emp.deptno = dept.deptno;
-- 右外连接 (如果右侧的表完全显示我们就说是右外连接)
SELECT ename,dname,job FROM emp RIGHT JOIN dept ON emp.deptno = dept.deptno;

格式：SELECT ... FROM A表 RIGHT/LEFT JOIN B表 ON （约束条件）

-- 在使用left join时，on和where条件的区别如下： 1、 on条件是在生成临时表时使用的条件，它不管on中的条件是否为真，都会返回左边表中的记录,右表不符合条件null填充 2、where条件是在临时表生成好后，再对临时表进行过滤的条件。这时已经没有left join的含义了，临时表已经生成好了，条件不为真的就全部过滤掉  
```

