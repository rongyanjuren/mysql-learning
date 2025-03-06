### 笔记参考来源

https://gitee.com/zwhddup/mysql-learning/tree/master

**这位同学的仓库下也有基础，高级的笔记（且有pdf版本）。**

**当然我这边已经copy过来了，因为之前找的图片存在问题。**

**建议看pdf版本，因为找的高级篇mk文件好像缺了。**



**个人已经改学其他视频了，原视频太长了╮(╯▽╰)╭，如果正在mysql开发优化的朋友可以坚持下去，而我目前只需要了解，加深就行。**

https://gitee.com/MingZii/learn_db

### Mysql学习视频

> 原学习视频：https://www.bilibili.com/video/BV1iq4y1u7vj?share_source=copy_web&vd_source=6164cc1e15b15d47186e6ecfe12edef8
>
> 改学视频：https://www.bilibili.com/video/BV1Kr4y1i7ru?share_source=copy_web&vd_source=6164cc1e15b15d47186e6ecfe12edef8

### 学习建议

#### 【MySQL上篇：基础篇】
##### 【第1子篇：数据库概述与MySQL安装篇】
p01-p11

学习建议：零基础同学必看，涉及理解和Windows系统下MySQL安装

##### 【第2子篇：SQL之SELECT使用篇】
p12-p48

学习建议：学习SQL的重点，必须重点掌握，建议课后练习多写

##### 【第3子篇：SQL之DDL、DML、DCL使用篇】
p49-p73

学习建议：学习SQL的重点，难度较SELECT低，练习写写就能掌握

##### 【第4子篇：其它数据库对象篇】
p74-p93

学习建议：对于希望早点学完MySQL基础，开始后续内容的同学，这个子篇可以略过。
在工作中，根据公司需要进行学习即可。

##### 【第5子篇：MySQL8新特性篇】
p94-p95

学习建议：对于希望早点学完MySQL基础，开始后续内容的同学，这个子篇可以略过。
在工作中，根据公司需要进行学习即可。

#### 【MySQL下篇：高级篇】
##### 【第1子篇：MySQL架构篇】
 p96-p114

学习建议：涉及Linux平台安装及一些基本问题，基础不牢固同学需要学习

##### 【第2子篇：索引及调优篇】
p115-p160

学习建议：面试和开发的重点，也是重灾区，需要全面细致的学习和掌握

##### 【第3子篇：事务篇】
p161-p186

学习建议：面试和开发的重点，需要全面细致的学习和掌握

##### 【第4子篇：日志与备份篇】
p187-p199

学习建议：根据实际开发需要，进行相应内容的学习



### 说明

>Mysql不严谨；
>
>养成写sql语句好习惯，下文皆为sql列子，需执行寻求其含义；
>
>字符和日期用单引号；
>
>本部分为视频内练手SQL笔记。

### 第三章 基本SELECT

#### SELECT使用

```sql
SELECT * FROM employees;
SELECT employee_id, last_name, salary FROM employees;
```

#### 别名使用

```sql
#as->alias
#列的别名可以使用一对""引起来
#AS 可以省略
SELECT employee_id AS emp_id, last_name lname, department_id "dept_id", salary * 12 annual_sal
FROM employees;
```

#### 去除重复行

```sql
#查询员工表一共有哪些部门id
SELECT DISTINCT department_id FROM employees
```

#### 空值参与运算

```sql
#NULL不等同于0, '', 'null'
SELECT * FROM employees;
#空值参与运算结果也为NULL
SELECT employee_id, salary "月工资", salary * (1 + commission_pct) * 12 "年工资" FROM employees;
```

#### 着重号``

```sql
#与关键字重名时使用
SELECT * FROM order;#错误
SELECT * FROM `order`;
```

#### 查询常数

```sql
SELECT '尚硅谷', employee_id, last_name FROM employees;
```

#### 显示表结构

```sql
DESCRIBE employees;#显示了表中字段的详细信息
DESC employees;#显示了表中字段的详细信息
DESC departments;
```

#### 过滤数据WHERE

```sql
#WHERE一定要放FROM后面且仅挨着FROM
#查询90号部门的员工信息
SELECT * FROM employees WHERE department_id = 90;
#查询last_name为'King'的员工信息
SELECT * FROM employees WHERE last_name = 'King';
```

#### 课后练习

* 查询员工12个月的工资总和，并起别名为ANNUAL SALARY

  ```sql
  SELECT employee_id, last_name, salary * 12 "ANNUAL SALARY"
  FROM employees;
  #加了奖金
  SELECT employee_id, last_name, salary * 12 * (1 + IFNULL(commission_pct, 0)) "ANNUAL SALARY"
  FROM employees;
  ```

* 查询employees表中去除重复的job_id以后的数据

  ```sql
  SELECT DISTINCT job_id FROM employees;
  ```

* 查询工资大于12000的员工姓名和工资

  ```sql
  SELECT last_name, salary FROM employees WHERE salary > 12000;
  ```

* 查询员工号为176的员工的姓名和部门号

  ```sql
  SELECT last_name, department_id FROM employees WHERE employee_id = 176;
  ```

* 显示表departments的结构，并查询其中的全部数据

  ```sql
  DESCRIBE departments;
  DESC departments;
  SELECT * FROM departments;
  ```


### 第四章 运算符

#### 算术运算符

> +、-、* 、/（div）、%（mod）

* 加法与减法运算符

  ```sql
  #DUAL为虚拟表
  SELECT 100, 100 + 0, 100 - 0, 100 + 50, 100 + 50 - 30, 100 + 35.5, 100 - 35.5
  FROM DUAL;
  SELECT 100 + '1' FROM DUAL; # 在java语言中结果是：149，这里是101，因为'1'会隐式转换为数值1
  SELECT 100 + 'a' FROM DUAL;#此时将'a'看做0处理，因为隐式转换无法执行
  SELECT 100 + NULL FROM DUAL;#NULL参数运算，结果为NULL
  ```

* 乘除

  ```sql
  SELECT 100, 100 * 1, 100 * 1.0, 100 / 1.0, 100 / 2, 100 + 2 * 5 / 2, 100 / 3, 100 DIV 0 FROM DUAL;
  ```

* 取模运算

  ```sql
  SELECT 12 % 3, 12 % 5, 12 MOD -5, -12 % 5, -12 % -5, 12 % 13, 12 % 20 FROM DUAL;
  #查询员工id为偶数的员工信息
  SELECT employee_id, last_name, salary FROM employees WHERE employee_id % 2 = 0;
  ```

#### 比较运算符

> =, <=>, <>, !=, <, <=, >, >=

```sql
SELECT 1 = 2, 1 != 2 FROM DUAL;
SELECT 1 = 2, 1 != 2, 1 = '1', 1 = 'a', 0 = 'a' FROM DUAL;#字符串存在隐式转换，如果转换数值不成功，则看做为0

SELECT 'a' = 'a', 'ab' = 'ab', 'a' = 'b' FROM DUAL;#两边都是字符串时，按ANSI编码比较规则进行比较
SELECT 1 = NULL, NULL = NULL FROM DUAL;#只要有NULL参与判断，结果就为NULL

SELECT last_name, salary FROM employees WHERE salary = 6000;
SELECT last_name, salary FROM employees WHERE commission_pct = NULL;#此时执行不会有结果满足

#<=>与=的区别，是其可判断NULL，为NULL而生
SELECT 1 <=> 2, 1 <=> '1', 1 <=> 'a', 0 <=> 'a' FROM DUAL;
SELECT 1 <=> NULL, NULL <=> NULL FROM DUAL;

#查询commission_pct为NULL的数据有哪些
SELECT last_name, salary, commission_pct FROM employees WHERE commission_pct <=> NULL;

SELECT 3 <> 2, '4' <> NULL, '' != NULL, NULL != NULL FROM DUAL;#只要有NULL参与判断，结果就为NULL
```

> IS NULL, IS NOT NULL, ISNULL

```sql
#查询commission_pct为NULL的数据有哪些
SELECT last_name, salary, commission_pct FROM employees WHERE commission_pct IS NULL;
SELECT last_name, salary, commission_pct FROM employees WHERE ISNULL(commission_pct);

#查询commission_pct不为NULL的数据有哪些
SELECT last_name, salary, commission_pct FROM employees WHERE commission_pct IS NOT NULL;
SELECT last_name, salary, commission_pct FROM employees WHERE NOT commission_pct <=> NULL;
```

> LEAST()取最小，GREATEST()取最大，按字典序比较

```sql
SELECT LEAST('g', 'b', 't', 'm'), GREATEST('g', 'b', 't', 'm') FROM DUAL;

SELECT LEAST(first_name, last_name) FROM employees;
```

> BETWEEN 条件下界 AND 条件上界

```sql
#包含边界值
#查询工资在6000到8000的员工信息
SELECT employee_id, last_name, salary FROM employees WHERE salary BETWEEN 6000 AND 8000;
SELECT employee_id, last_name, salary FROM employees WHERE salary >= 6000 AND salary <= 8000;

#查询工资不在6000到8000的员工信息
SELECT employee_id, last_name, salary FROM employees WHERE salary < 6000 OR salary > 8000;
SELECT employee_id, last_name, salary FROM employees WHERE NOT salary BETWEEN 6000 AND 8000;
```

> IN (set), NOT IN (set)

```sql
#查询部门为10,20,30部门的员工信息
SELECT last_name, salary, department_id FROM employees WHERE department_id = 10 OR department_id = 20 OR department_id = 30;
SELECT last_name, salary, department_id FROM employees WHERE department_id IN (10, 20, 30);

#查询部门不是6000,7000,8000的员工信息
SELECT last_name, salary, department_id FROM employees WHERE salary NOT IN (6000, 7000, 8000);
```

> LIKE #模糊查询

```sql
#查询last_name中包含字符'a'的员工信息
# % 代表不确定个数的字符（0个，1个，多个）
SELECT last_name FROM employees WHERE last_name LIKE '%a%';

#查询last_name中以字符'a'开头的员工信息
SELECT last_name FROM employees WHERE last_name LIKE 'a%';


#查询last_name中包含字符'a'且包含字符'e'的员工信息
SELECT last_name FROM employees WHERE last_name LIKE '%a%' AND last_name LIKE '%e%';
SELECT last_name FROM employees WHERE last_name LIKE '%a%e%' OR last_name LIKE '%e%a%';

#_ 代表一个不确定的字符
#查询last_name中第二个字符是'a'的员工信息
SELECT last_name FROM employees WHERE last_name LIKE '_a%';

#查询last_name中第三个字符是'a'的员工信息
SELECT last_name FROM employees WHERE last_name LIKE '__a%';

#查询第二个字符是_且第三个字符是'a'的员工信息
#使用转义字符\
SELECT last_name FROM employees WHERE last_name LIKE '_\_a%';
#了解ESCAPE，指定相应符号为转义字符
SELECT last_name FROM employees WHERE last_name LIKE '_$_a%' ESCAPE '$';
```

> REGEXP,RLIKE#正则表达式

```sql
#正则表达式比较规则 百度
#^表示以一些字符开头
#$表示以一些字符结尾
SELECT 'shkstart' REGEXP '^shk', 'shkstart' REGEXP 't$', 'shkstart' REGEXP 'hk' FROM DUAL;
```

#### 逻辑运算符

> OR ||, AND &&, NOT !, XOR

```sql
SELECT last_name, salary, department_id FROM employees WHERE department_id = 10 OR department_id = 20;
SELECT last_name, salary, department_id FROM employees WHERE department_id = 10 AND department_id = 20;
SELECT last_name, salary, department_id FROM employees WHERE department_id = 50 AND salary > 6000;

SELECT employee_id, last_name, salary FROM employees WHERE NOT salary BETWEEN 6000 AND 8000;
SELECT employee_id, last_name, salary FROM employees WHERE !(salary BETWEEN 6000 AND 8000);

#2个条件只满足其1而另一个不满足
SELECT last_name, salary, department_id FROM employees WHERE department_id = 50 XOR salary > 6000;
#AND优先级高于OR
```

#### 位运算

> &, |, ^, ~, >>, <<

```sql
SELECT 1 & 1, 12 | 5, 12 ^ 5 FROM DUAL;
SELECT 4 >> 1, 4 << 1, 5 >> 1 FROM DUAL
SELECT 10 & ~1 FROM DUAL;
```

#### 课后练习

* 选择工资不在5000到12000的员工的姓名和工资

```sql
SELECT last_name, salary FROM employees WHERE NOT salary BETWEEN 5000 AND 12000;
SELECT last_name, salary FROM employees WHERE salary < 5000 OR salary > 12000;
```

* 选择在20或50号部门工作的员工姓名和部门号

```sql
SELECT last_name, department_id FROM employees WHERE department_id IN (20, 50);
SELECT last_name, department_id FROM employees WHERE department_id = 20 OR department_id = 50;
```

* 选择公司中没有管理者的员工姓名及job_id

```sql
SELECT last_name, job_id, manager_id FROM employees WHERE manager_id IS NULL;
SELECT last_name, job_id, manager_id FROM employees WHERE manager_id <=> NULL;
```

* 选择公司中有奖金的员工姓名，工资和奖金级别

```sql
SELECT last_name, salary, commission_pct FROM employees WHERE commission_pct IS NOT NULL;
SELECT last_name, salary, commission_pct FROM employees WHERE NOT commission_pct <=> NULL;
```

* 选择员工姓名的第三个字母是a的员工姓名

```sql
SELECT last_name FROM employees WHERE last_name LIKE '__a%';
```

* 显示出表employees表中first_name以'e'结尾的员工信息

```sql
SELECT first_name, last_name FROM employees WHERE first_name LIKE '%e';
SELECT first_name, last_name FROM employees WHERE first_name REGEXP 'e$';
#first_name以e开头的
SELECT first_name, last_name FROM employees WHERE first_name REGEXP '^e';
```

* 选择姓名中有字母a和k的员工姓名

```sql
SELECT last_name FROM employees WHERE last_name LIKE '%a%k%' OR last_name LIKE '%k%a%';
SELECT last_name FROM employees WHERE last_name LIKE '%a%' AND last_name LIKE '%k%';
```

* 显示出表employees部门编号在80-100之间的姓名、工种

```sql
SELECT last_name, job_id, department_id FROM employees WHERE department_id >= 80 AND department_id <= 100;

SELECT last_name, job_id, department_id FROM employees WHERE department_id BETWEEN 80 AND 100;
```

* 显示出表employees的manager_id是100,101,110的员工姓名、工资、管理者id

```sql
SELECT last_name, salary, manager_id FROM employees WHERE manager_id IN (100, 101, 110);
```

### 第五章 排序与分页

#### 排序

> 如果没有使用排序操作，默认情况下查询返回的数据是按照添加数据顺序显示的

```sql
#按照salary从高到低的顺序显示员工信息
#使用ORDER BY 对查询到的数据进行排序操作
#默认排序从低到高，升序ASC，降序DESC
#ORDER BY 后没有显示指明排序方式，默认按照升序排列
SELECT employee_id, last_name, salary FROM employees ORDER BY salary DESC;
SELECT employee_id, salary, salary * 12 annual_sal FROM employees ORDER BY annual_sal;

#列的别名只能在ORDER BY中使用，不能在WHERE中使用。SQL执行原理有关
SELECT employee_id, salary, salary * 12 annual_sal FROM employees WHERE annual_sal > 81600;#错误语法

#WHERE 需要声明在FROM后，ORDER BY前
SELECT employee_id, salary #再执行这句
FROM employees WHERE department_id IN (50, 60, 70) #先执行这句
ORDER BY department_id DESC; #最后执行这句

#多列排序
#二级排序，显示员工信息，按照department_id降序排列，department_id一样是按照salary的升序排列
SELECT employee_id, salary, department_id FROM employees ORDER BY department_id DESC, salary ASC;
```

#### 分页

```sql
#mysql使用limit实现数据的分页显示
#每页显示20条记录，此时显示第一页
SELECT employee_id, last_name FROM employees LIMIT 0,20;

#每页显示20条记录，此时显示第二页
SELECT employee_id, last_name FROM employees LIMIT 20,20;

#每页显示20条记录，此时显示第三页
SELECT employee_id, last_name FROM employees LIMIT 40,20;

#实际使用
#每页显示pageSize条记录，此时显示第pageNo页
#公式：LIMIT (pageNo-1) * pageSize, pageSize;

# WHERE ... ORDER BY ... LIMIT 声明顺序
SELECT employee_id, last_name, salary FROM employees WHERE salary > 6000 ORDER BY salary DESC LIMIT 0,10;

#LIMIT的格式(严格来说)：LIMIT 位置偏移量,条目数
#但LIMIT 0,条目数 等同于 LIMIT 条目数
SELECT employee_id, last_name, salary FROM employees WHERE salary > 6000 ORDER BY salary DESC LIMIT 10;

#表里有107条数据，我们只想要显示第32、33条数据怎么办呢
SELECT employee_id, last_name FROM employees LIMIT 31,2;

#Mysql 8.0新特性 LIMIT ... OFFSET ...
#表里有107条数据，我们只想要显示第32、33条数据怎么办呢
SELECT employee_id, last_name FROM employees LIMIT 2 OFFSET 31;

#查询员工表中工资最高的员工信息
SELECT employee_id, last_name, salary FROM employees ORDER BY salary DESC LIMIT 0, 1;

#LIMIT 可以在Mysql，PGSQL，MariaDB，SQLite等数据库中使用，表示分页
#不能使用在SQL Server，DB2，Oracle
```

#### 课后练习

* 查询员工的姓名、部门号、年薪，按年薪降序，按姓名升序显示

```sql
SELECT last_name, department_id, salary * 12 annual_salary FROM employees ORDER BY annual_salary DESC, last_name ASC;
```

* 选择工资不在8000 到17000的员工姓名、工资，按工资降序，显示第21到40位置的数据

```sql
SELECT last_name, salary 
FROM employees 
WHERE salary NOT BETWEEN 8000 AND 17000 
ORDER BY salary DESC 
LIMIT 20, 20;

SELECT last_name, salary 
FROM employees 
WHERE salary < 8000 OR salary > 17000
ORDER BY salary DESC 
LIMIT 20, 20;
```

* 查询邮箱中包含e的员工信息，并先按邮箱的字节数降序，再按部门号升序

```sql
SELECT employee_id, last_name, email, department_id 
FROM employees
WHERE email LIKE '%e%'
ORDER BY LENGTH(email) DESC, department_id ASC;
```

### 第六章 **多表**查询

#### 基本：表起别名、连接条件

```sql
#查询员工姓名和其所在部门
SELECT employee_id, department_name
FROM employees, departments; #错误，缺少连接条件

SELECT employee_id, department_name
FROM employees CROSS JOIN departments; #错误，缺少连接条件

SELECT employee_id, last_name, department_name
FROM employees, departments 
WHERE employees.department_id = departments.department_id;

#如果查询语句中出现了多个表中都存在的字段，则必须指明此字段所在表
SELECT employee_id, last_name, department_name, departments.department_id
FROM employees, departments 
WHERE employees.department_id = departments.department_id;

#建议：从sql优化的角度，建议多表查询时，每个字段前都指明其所在的表。
SELECT employee_id, last_name, departments.department_name, departments.department_id
FROM employees, departments 
WHERE employees.department_id = departments.department_id;

#可以给表起别名，在SELECT和WHERE中使用的别名，表起别名就必须用别名
SELECT employee_id, last_name, dept.department_name, dept.department_id
FROM employees emp, departments dept 
WHERE emp.department_id = dept.department_id;

#查询员工的employee_id, last_name, department_name, city
SELECT emp.employee_id, emp.last_name, dept.department_name, loc.city
FROM employees emp, departments dept, locations loc
WHERE emp.department_id = dept.department_id AND dept.location_id = loc.location_id;

#查询员工名为'Abel'的人在哪个城市工作
SELECT emp.employee_id, emp.last_name, dept.department_name, loc.city
FROM employees emp, departments dept, locations loc
WHERE emp.department_id = dept.department_id AND dept.location_id = loc.location_id AND last_name = 'Abel';

#如果有n个表实现多表查询，则需要n-1个连接条件
```

#### 等值连接、非等值连接

* 非等值连接

```sql
SELECT emp.last_name, emp.salary, jg.grade_level
FROM employees emp, job_grades jg
WHERE emp.salary BETWEEN jg.lowest_sal AND jg.highest_sal;

SELECT emp.last_name, emp.salary, jg.grade_level
FROM employees emp, job_grades jg
WHERE emp.salary <= jg.highest_sal AND emp.salary >= jg.lowest_sal;
```

#### 自连接、非自连接

* 自连接

```sql
#查询员工id，员工姓名及其管理者的id和姓名
SELECT emp.employee_id, emp.last_name, mag.employee_id manager_id, mag.last_name manager_name
FROM employees emp, employees mag
WHERE emp.manager_id = mag.employee_id;
```

#### 内连接、外连接

* 内连接

> 内连接：合并同一列的两个以上的表的行，结果集中不包含一个表与另一个表不匹配的行

```sql
#查询员工的employee_id, department_name
SELECT emp.employee_id, dept.department_name
FROM employees emp, departments dept
WHERE emp.department_id = dept.department_id;
#SQL92见上

#SQL99语法实现上述内连接
SELECT emp.last_name, dep.department_name
FROM employees emp JOIN departments dep
ON emp.department_id = dep.department_id;

#SQL99语法实现内连接
#查询员工的employee_id, last_name, department_name, city
SELECT emp.employee_id, emp.last_name, dept.department_name, loc.city
FROM employees emp INNER JOIN departments dept
ON emp.department_id = dept.department_id 
JOIN locations loc
ON dept.location_id = loc.location_id;#INNER可以省略，完整为INNER JOIN
```

* 外连接

> 外连接：合并具有同一列的两个以上的表的行，结果集中除了包含一个表与另一个表匹配的行之外，
> 还查询到了左表或右表中不匹配的行
>
> 外连接的分类：左外连接、右外连接、满外连接
>
> 1. 左外连接：两个表在连接过程中除了返回满足连接条件的行以外还返回左表中不满足条件的行
> 2. 右外连接：两个表在连接过程中除了返回满足连接条件的行以外还返回右表中不满足条件的行
> 3. 满外连接：返回左外连接、右外连接、内连接的结果

```sql
#查询所有员工的last_name, department_name
#有所有两字=>注意外连接
#SQL92标准实现左外连接，使用+，下列子Mysql不支持，会执行错误
SELECT emp.last_name, dep.department_name
FROM employees emp, departments dep
WHERE emp.department_id = dep.department_id(+);#左外连接

#SQL92标准实现右外连接，使用+，下列子Mysql不支持，会执行错误
SELECT emp.last_name, dep.department_name
FROM employees emp, departments dep
WHERE emp.department_id(+) = dep.department_id;#右外连接

#查询所有员工的last_name, department_name
#SQL99语法实现左外连接
SELECT emp.last_name, dep.department_name
FROM employees emp LEFT OUTER JOIN departments dep
ON emp.department_id = dep.department_id;#OUTER可以省略

#SQL99语法实现右外连接
SELECT emp.last_name, dep.department_name
FROM employees emp RIGHT JOIN departments dep
ON emp.department_id = dep.department_id;#OUTER可以省略

#满外连接
#SQL99 Mysql不支持下述满外连接
SELECT emp.last_name, dep.department_name
FROM employees emp FULL JOIN departments dep
ON emp.department_id = dep.department_id;#OUTER可以省略，会报错

#实现满外连接效果
```

#### SQL99实现7种JOIN操作

<img src="images/SQL99 7operations.png" alt="image-20220531154418141" style="float:left;" />

> UNION：会执行去重操作
>
> UNION ALL：不去重

* 中间图：内连接

```sql
SELECT emp.employee_id, dept.department_name
FROM employees emp INNER JOIN departments dept
ON emp.department_id = dept.department_id
```

* 左上图：左外连接

```sql
SELECT emp.employee_id, dept.department_name
FROM employees emp LEFT OUTER JOIN departments dept
ON emp.department_id = dept.department_id
```

* 右上图：右外连接

```sql
SELECT emp.employee_id, dept.department_name
FROM employees emp RIGHT OUTER JOIN departments dept
ON emp.department_id = dept.department_id
```

* 左中图

```sql
SELECT emp.employee_id, dept.department_name
FROM employees emp LEFT OUTER JOIN departments dept
ON emp.department_id = dept.department_id
WHERE emp.department_id IS NULL#员工没部门
```

* 右中图

```sql
SELECT emp.employee_id, dept.department_name
FROM employees emp RIGHT OUTER JOIN departments dept
ON emp.department_id = dept.department_id
WHERE emp.employee_id IS NULL#部门没员工
```

* 左下图：满外连接

```sql
# 方式1：左上图 UNION ALL 右中图
SELECT emp.employee_id, dept.department_name
FROM employees emp LEFT OUTER JOIN departments dept
ON emp.department_id = dept.department_id
UNION ALL
SELECT emp.employee_id, dept.department_name
FROM employees emp RIGHT OUTER JOIN departments dept
ON emp.department_id = dept.department_id
WHERE emp.employee_id IS NULL
```

```sqlite
# 方式2：左中图 UNION ALL 右上图
SELECT emp.employee_id, dept.department_name
FROM employees emp LEFT OUTER JOIN departments dept
ON emp.department_id = dept.department_id
WHERE emp.department_id IS NULL
UNION ALL
SELECT emp.employee_id, dept.department_name
FROM employees emp RIGHT OUTER JOIN departments dept
ON emp.department_id = dept.department_id
```

* 右下图：左中图 UNION ALL 右中图

```sql
SELECT emp.employee_id, dept.department_name
FROM employees emp LEFT OUTER JOIN departments dept
ON emp.department_id = dept.department_id
WHERE emp.department_id IS NULL
UNION ALL
SELECT emp.employee_id, dept.department_name
FROM employees emp RIGHT OUTER JOIN departments dept
ON emp.department_id = dept.department_id
WHERE emp.employee_id IS NULL
```

#### SQL99新特性

* 自然连接

```sql
#原始连接
SELECT emp.employee_id, emp.last_name, dept.department_name
FROM employees emp JOIN departments dept
ON emp.department_id = dept.department_id
AND emp.manager_id = dept.manager_id;

#自然连接，会自动查询两张表中，所有相同的字段，然后进行等值连接
SELECT emp.employee_id, emp.last_name, dept.department_name
FROM employees emp NATURAL JOIN departments dept
```

* USING连接使用

```sql
#原始连接
SELECT emp.employee_id, emp.last_name, dept.department_name
FROM employees emp JOIN departments dept
ON emp.department_id = dept.department_id

#USING连接使用
SELECT emp.employee_id, emp.last_name, dept.department_name
FROM employees emp JOIN departments dept
USING(department_id);
```

#### 课后练习

* 显示所有员工的姓名、部门号、部门名称

```sql
SELECT emp.last_name, emp.department_id, dept.department_name 
FROM employees emp LEFT OUTER JOIN departments dept
ON emp.department_id = dept.department_id;
```

* 查询90号部门员工的job_id,和90号部门的location_id

```sql
SELECT emp.job_id, dept.location_id
FROM employees emp JOIN departments dept
ON emp.department_id = dept.department_id
WHERE dept.department_id = 90
```

* 选择所有有奖金的员工的last_name,department_name,location_id,city

```sql
SELECT emp.last_name, emp.commission_pct, dept.department_name, dept.location_id, loca.location_id, loca.city
FROM employees emp LEFT OUTER JOIN departments dept
ON emp.department_id = dept.department_id 
LEFT OUTER JOIN locations loca
ON dept.location_id = loca.location_id
WHERE emp.commission_pct IS NOT NULL;
```

* 选择city在Toronto工作的员工的last_name,job_id,department_id,department_name

```sql
#SQL99语法
SELECT emp.last_name, emp.job_id, emp.department_id, dept.department_name
FROM employees emp JOIN departments dept
ON emp.department_id = dept.department_id
JOIN locations loca
ON dept.location_id = loca.location_id
WHERE loca.city = 'Toronto';

#SQL92语法
SELECT emp.last_name, emp.job_id, emp.department_id, dept.department_name
FROM employees emp, departments dept, locations loca
WHERE emp.department_id = dept.department_id 
AND dept.location_id = loca.location_id
AND loca.city = 'Toronto';
```

* 查询员工所在的部门名称,部门地址,姓名,工作,工资,其中员工所在部门的部门名称为'Executive'

```sql
SELECT dept.department_name, loca.street_address, emp.last_name, emp.job_id, emp.salary
FROM departments dept LEFT OUTER JOIN employees emp
ON emp.department_id = dept.department_id
LEFT OUTER JOIN locations loca
ON dept.location_id = loca.location_id
WHERE dept.department_name = 'Executive';
```

* 选择指定员工的姓名，员工号，以及他的管理者的姓名和员工号，结果类似于下面的格式

```sql
SELECT emp1.last_name, emp1.employee_id, emp2.last_name, emp2.employee_id
FROM employees emp1 LEFT OUTER JOIN employees emp2
ON emp1.manager_id = emp2.employee_id;
```

* 查询哪些部门没有员工

```sql
SELECT dept.department_id, dept.department_name
FROm departments dept LEFT OUTER JOIN employees emp
ON dept.department_id = emp.department_id
WHERE emp.employee_id IS NULL;

#本题也可以使用子查询
SELECT dept.department_id, dept.department_name
FROM departments dept
WHERE NOt EXISTS(
SELECT *
FROM employees emp
WHERE emp.department_id = dept.department_id
);
```

* 查询哪个城市没有部门

```sql
SELECT loca.city
FROM locations loca LEFT OUTER JOIN departments dept
ON loca.location_id = dept.location_id
WHERE dept.department_id IS NULL;
```

* 查询部门为Sales或IT的员工信息

```sql
SELECT emp.employee_id, emp.last_name, emp.salary, dept.department_name
FROM employees emp INNER JOIN departments dept
ON emp.department_id = dept.department_id
WHERE dept.department_name IN ('Sales', 'IT');
```

### 第七章 单行函数

#### 数值函数

```sql
SELECT ABS(-123), ABS(32),#返回回绝对值
SIGN(-23), SIGN(43),#判断正负，1为正，-1为负
PI(),
CEIL(32.32), CEILING(-43.23), #往上取
FLOOR(32.32), FLOOR(-43.23),#往下取
MOD(12, 5)
FROM DUAL;

SELECT RAND(), RAND(), RAND(10), RAND(10), RAND(-1), RAND(-1)#随机数
FROM DUAL;

SELECT ROUND(12.33),#四舍五入
ROUND(12.345, 2), #精确小数点后2位并四舍五入
ROUND(12.324, -1), #看小数点前1位并四舍五入
TRUNCATE(12.66, 1),#看小数点后1位，并舍掉；截断的意思
TRUNCATE(12.66, -1)#看小数点前1位，并舍掉；截断的意思
FROM DUAL;

SELECT TRUNCATE(ROUND(12.55), 1)#单行函数可以嵌套
FROM DUAL;

#角度与弧度互换
SELECT RADIANS(30), RADIANS(60), RADIANS(90),#角度换为弧度
DEGREES(2 * PI()), DEGREES(RADIANS(90))#弧度换角度
FROM DUAL;

#三角函数
SELECT SIN(RADIANS(30)), #sin
DEGREES(ASIN(1)), #arcsin
TAN(RADIANS(45)), #tan
DEGREES(ATAN(1)), #arctan
DEGREES(ATAN2(1, 1)) #两个点的 ATAN2(y2 - y1, x2 - x1)
FROM DUAL;

#指数和对数
SELECT POW(2,5), POWER(2,4), #指数
EXP(2), #e的2次方
LN(10), #ln2
LOG10(10), LOG2(4), #log(x)
LN(EXP(2))#默认e为底
FROM DUAL;

#进制转换
SELECT BIN(10),
HEX(10),
OCT(10),
#等同于上面
CONV(10, 10, 2),
CONV(10, 10, 16),
CONV(10, 10, 8)
FROM DUAL;
```

#### 字符串函数

```sql
#字符串连接
SELECT CONCAT(emp.last_name, ' worked for ', mgr.last_name) detail
FROM employees emp JOIN employees mgr
WHERE emp.manager_id = mgr.employee_id;
#用特定符号连接
SELECT CONCAT_WS('-', 'h', 'e', 'l')
FROM DUAL;

#在这里字符串索引从1开始，字符替换
SELECT INSERT('hello world', 2, 3, 'aaaaa'),
REPLACE('hello', 'llo', 'mmm')
FROM DUAL;

SELECT UPPER('HelLo'), LOWER('HelLo')
FROM DUAL;

SELECT LEFT('HelLo', 2),#取左边两个
RIGHT('HelLo', 3)
FROM DUAL;

SELECT employee_id, last_name, LPAD(salary, 10, '*')#实现右对齐，RPAD左对齐
FROM employees;
```

#### 日期时间函数

```sql
SELECT CURDATE(), CURTIME(), NOW(),#当前时间
UTC_DATE(), UTC_TIME()#格林威治当前时间
FROM DUAL;

SELECT UNIX_TIMESTAMP(), UNIX_TIMESTAMP('2022-08-05 15:32:21'),#返回时间戳
FROM_UNIXTIME(1659684777)#时间戳转日期
FROM DUAL;

SELECT YEAR(CURDATE()), MONTH(CURDATE()), DAY(CURDATE()),#年月日
HOUR(CURTIME()), MINUTE(NOW()), SECOND(SYSDATE())#时分秒
FROM DUAL;

SELECT MONTHNAME('2021-10-26'), DAYNAME('2021-10-26'), WEEKDAY('2021-10-26'),#月，周2，周2为1
QUARTER(CURDATE()), WEEK(CURDATE()), DAYOFYEAR(NOW()),#季度，今年第几周，今年第几天
DAYOFMONTH(NOW()), DAYOFWEEK(NOW())#这月第几天，这周第几天
FROM DUAL;

#EXTRACT(type FROM date)
SELECT EXTRACT(MINUTE FROM NOW()),EXTRACT(WEEK FROM NOW()),#分，今年第几周
EXTRACT(QUARTER FROM NOW()), EXTRACT(MINUTE_SECOND FROM NOW()),#第几季度，分秒合一起
EXTRACT(YEAR FROM '2021-10-26')
FROM DUAL;

SELECT DATE_ADD(NOW(), INTERVAL 1 DAY) AS col1, #当前时间加1天
DATE_ADD('2021-10-21 23:32:12', INTERVAL 1 SECOND) AS col2,#加1秒
ADDDATE('2021-10-21 23:32:12', INTERVAL 1 SECOND) AS col3,#加1秒
DATE_ADD('2021-10-21 23:32:12', INTERVAL '1_1' MINUTE_SECOND) AS col4,#分秒都加1
DATE_ADD(NOW(), INTERVAL -1 YEAR) AS col5, #可以是负数，减1年
DATE_ADD(NOW(), INTERVAL '1_1' YEAR_MONTH) AS col6 #需要单引号，年月都加1
FROM DUAL;

SELECT ADDTIME(NOW(), 20), SUBTIME(NOW(), 30),#秒加，秒减
SUBTIME(NOW(), '1:1:3'), DATEDIFF(NOW(),'2021-10-01'),#时分秒相减，年月日相减
TIMEDIFF(NOW(), '2021-10-25 22:10:10'),#时间间隔多少天
FROM_DAYS(366), TO_DAYS('0000-12-25'),#从0000年1月1日起，N天以后的日期；日期date距离0000年1月1日的天数
LAST_DAY(NOW()), MAKEDATE(YEAR(NOW()), 12),#返回date所在月份的最后一天日期；返回给定年数的第几天的日期
MAKETIME(10,21,23)#返回时分秒；返回相加后的时间
FROM DUAL;

SELECT PERIOD_ADD(202002, 10)#在给定日期增加月份
FROM DUAL;

#日期格式化与解析
#格式化
SELECT DATE_FORMAT(CURDATE(), '%Y-%M-%D'),
DATE_FORMAT(NOW(), '%Y-%m-%d'), DATE_FORMAT(NOW(),'%H:%i:%S'), CURDATE(),
DATE_FORMAT(NOW(),'%Y-%M-%D %h:%i:%S %W %w %T %r')
FROM DUAL;
#格式化逆过程
SELECT STR_TO_DATE('2022-August-5th 05:24:11', '%Y-%M-%D %h:%i:%S') AS time
FROM DUAL;

#设置好的格式，现用
SELECT GET_FORMAT(DATE, 'USA')
FROM DUAL;
SELECT DATE_FORMAT(NOW(), GET_FORMAT(DATETIME, 'USA'))
FROM DUAL;
```

#### 流程控制函数

```sql
#流程控制
SELECT last_name, salary, IF(salary >= 6000, '高工资', '低工资')
FROM employees;

SELECT last_name, commission_pct, IF(commission_pct IS NOT NULL, commission_pct, 0) details,
salary * 12 * (1 + IF(commission_pct IS NOT NULL, commission_pct, 0)) annual_sal
FROM employees;

SELECT last_name, commission_pct, IFNULL(commission_pct, 0) details
FROM employees;

SELECT last_name, salary, CASE WHEN salary >= 15000 THEN '白骨精'
	                           WHEN salary >= 10000 THEN '潜力股'
                               WHEN salary >= 8000 THEN '小屌丝'
                               ELSE '草根'
							   END details
FROM employees;

#查询部门号为10,20,30的员工信息，为10打印其工资1.1倍，
#为20打印1.2，为30打印1.3，其他打印1.4
SELECT employee_id, last_name, department_id, salary, CASE department_id 
WHEN 10 THEN salary * 1.1
WHEN 20 THEN salary * 1.2
WHEN 30 THEN salary * 1.3
ELSE salary * 1.4
END salary
FROM employees;

#查询部门号为10,20,30的员工信息，为10打印其工资1.1倍，
#为20打印1.2，为30打印1.3
SELECT employee_id, last_name, department_id, salary, CASE department_id 
WHEN 10 THEN salary * 1.1
WHEN 20 THEN salary * 1.2
WHEN 30 THEN salary * 1.3
END details
FROM employees
WHERE department_id IN (10, 20, 30);
```

#### 加密解密函数

```sql
#加密
SELECT MD5('mysql'), SHA('mysql')
FROM DUAL;

#加密解密
SELECT ENCODE('dylan','mysql'), DECODE(ENCODE('dylan','mysql'),'mysql')#mysql8.0 弃用
FROM DUAL;

#Mysql信息函数
SELECT VERSION(),#版本
CONNECTION_ID(),#mysql服务器连接数
DATABASE(),#当前数据库
USER(),#当前用户
CHARSET('dylan'),
COLLATION('dylan')
FROM DUAL;
```

#### 其他函数

```sql
#其它函数
SELECT FORMAT(12.325, 2),#保留2位小数
FORMAT(12.325, -2),#小于等于0保留整数
CONV(16, 10, 2), CONV(8888, 10, 16), CONV(NULL, 10, 2)#进制转换
FROM DUAL;

SELECT INET_ATON('192.168.1.100'),#加密
#转换规则
#以“192.168.1.100”为例，计算方式为192乘以256的3次方，加上168乘以256的2次方，加上1乘以256，再加上100。
INET_NTOA('3232235876')#解密
FROM DUAL;

#测试表达式执行效率
SELECT BENCHMARK(1000, MD5('mysql123123123')) time
FROM DUAL;

#实现字符集转换
SELECT CHARSET('dylan'), CHARSET(CONVERT('dylan' USING 'gbk'))
FROM DUAL;
```

#### 课后练习

* 显示系统时间

```sql
SELECT NOW()
FROM DUAL;
```

* 查询员工工号，姓名，工资，以及工资提高百分之20%后的结果

```sql
SELECT employee_id, last_name, salary, salary * (1 + 0.2) upSalary
FROM employees;
```

* 将员工的姓名按首字母排序，写出姓名的长度

```sql
SELECT last_name, LENGTH(last_name) len_lastName
FROM employees
#ORDER BY LEFT(last_name, 1)
ORDER BY last_name;
```

* 查询员工id,last_name,salary,并作为一个列输出，别名为OUT_PUT

```sql
SELECT CONCAT_WS('_', employee_id, last_name, salary) OUT_PUT
FROM employees;
```

* 查询公司个员工工作的年数、工作的天数，并按工作年数的降序排序

```sql
SELECT last_name, EXTRACT(YEAR FROM NOW()) - EXTRACT(YEAR FROM hire_date) work_years,
DATEDIFF(NOW(), hire_date) work_days
FROM employees
ORDER BY work_years DESC;
```

* 查询员工姓名，hire_date，department_id,满足以下条件：雇用时间在1997年之后，department_id为80或90或110，commission_pct不为空

```sql
SELECT last_name, department_id, hire_date
FROM employees
WHERE department_id IN (80, 90, 110)
AND EXTRACT(YEAR FROM hire_date) >= 1997
#AND hire_date >= '1997-01-01' #存在着隐式转换
AND commission_pct IS NOT NULL
```

* 查询公司中入职超过10000天的员工姓名、入职时间

```sql
SELECT last_name, hire_date, DATEDIFF(NOW(), hire_date) work_days
FROM employees
WHERE DATEDIFF(NOW(), hire_date) > 10000;
```

* 做一个查询，产生下面的结果：<last_name> earns <salary> monthly but wants <salary*3>

```sql
SELECT CONCAT(last_name, ' earns ', TRUNCATE(salary, 0), ' monthly but wants ', TRUNCATE(salary * 3, 0)) dream_salary
FROM employees;
```

* 使用case-when，按照下面的条件：
  job               grade
  AN_PRES        A
  ST_MAN         B
  IT_PROG        C
  SA_REP          D
  ST_CLERK      E

```sql
SELECT last_name, job_id, CASE job_id
WHEN 'AD_PRES' THEN 'A'
WHEN 'ST_MAN' THEN 'B'
WHEN 'IT_PROG' THEN 'C'
WHEN 'SA_REP' THEN 'D'
WHEN 'ST_CLERK' THEN 'E'
ELSE 'O'
END grade
FROM employees;
```

### 第八章 聚合函数

#### 5大常用聚合函数

>Mysql中聚合函数不能嵌套使用

```sql
#AVG, SUM
SELECT AVG(salary), SUM(salary)
FROM employees;

#MAX，MIN
SELECT MAX(salary), MIN(salary)
FROM employees;
SELECT MAX(last_name)
FROM employees;

#COUNT
#计算指定字段在查询结构中出现的个数
SELECT COUNT(employee_id), COUNT(salary), COUNT(1), COUNT(*)
FROM employees;
#如果计算表中有多少条记录，如何实现
#方式1：COUNT(1)
#方式2：COUNT(*)
#方式3：COUNT(具体字段)：不一定对
#如果使用MyISAM引擎上述三种方式效率一样
#使用InnoDB引擎，则三者效率COUNT(1) = COUNT(*) > COUNT(具体字段)
#COUNT()不计算NULL

#AVG = SUM / COUNT；AVG，SUM，COUNT自动过滤NULL
SELECT AVG(commission_pct), SUM(commission_pct) / COUNT(commission_pct)
FROM employees;
#查询公司中平均奖金率
SELECT AVG(commission_pct) FROM employees;#错误的
SELECT SUM(commission_pct) / COUNT(*) FROM employees;#正确方法
```

#### GROUP BY

```sql
#查询各个部门的平均工资，最高工资
SELECT department_id, AVG(salary), MAX(salary)
FROM employees
GROUP BY department_id;

#查询各个job_id的平均工资
SELECT job_id, AVG(salary)
FROM employees
GROUP BY job_id;

#使用多个列分组
#查询各个department_id,job_id的平均工资
SELECT department_id, job_id, AVG(salary)
FROM employees
GROUP BY department_id, job_id;
#上下操作一样
SELECT department_id, job_id, AVG(salary)
FROM employees
GROUP BY job_id, department_id;

#GROUP BY 声明在FROM后面、WHERE后面，ORDER BY前面、LIMIT前面

#使用WITH ROLLUP
#增加计算了salary整体平均
SELECT department_id, AVG(salary)
FROM employees
GROUP BY department_id WITH ROLLUP;
```

#### HAVING使用

> HAVING起过滤数据的作用
>
> WHERE里面不能放聚合函数
>
> WHERE与HAVING对比：
>
> 1.HAVING适用范围更广
>
> 2.过滤条件无聚合函数，使用WHERE效率更高

```sql
#查询各个部门中最高工资比10000高的部门信息
#如果过滤条件使用了聚合函数，则必须用HAVING替代WHERE
#HAVING 必须放在 GROUP BY后面
SELECT department_id, MAX(salary) max_salary
FROM employees
GROUP BY department_id
HAVING MAX(salary) > 10000;
#通常HAVING 依托 GROUP BY

#查询部门id为10,20,30,40,这几个部门中最高工资比10000高的部门信息
#方式1，过滤条件无聚合函数，要使用WHERE，效率高
SELECT department_id, MAX(salary) max_salary
FROM employees
WHERE department_id IN (10, 20, 30, 40)
GROUP BY department_id
HAVING MAX(salary) > 10000;
#方式2
SELECT department_id, MAX(salary) max_salary
FROM employees
GROUP BY department_id
HAVING MAX(salary) > 10000 AND department_id IN (10, 20, 30, 40);
```

#### SQL执行原理

```sql
#SQL92
SELECT ...........(存在聚合函数)
FROM..............
WHERE 多表连接条件 AND 不包含聚合函数的过滤条件
GROUP BY... , ...
HAVING 包含聚合函数的过滤条件
ORDER BY ... , ...（ASC/DESC)
LIMIT ..., ...

#SQL99
SELECT ...........(存在聚合函数)
FROM...... (LEFT / RIGHT) JOIN .... ON多表的连接条件
(LEFT / RIGHT) JOIN .... ON ...
WHERE 不包含聚合函数的过滤条件
GROUP BY... , ...
HAVING 包含聚合函数的过滤条件
ORDER BY ... , ...（ASC/DESC)
LIMIT ..., ...

#SQL语句执行过程
#详情：
#FROM ... , ... -> ON -> (LEFT / RIGHT JOIN) 
#-> WHERE -> GROUP BY -> HAVING -> SELECT 
#-> DISTINCT(去重) -> ORDER BY -> LIMIT
```

#### 课后练习

* WHER子句可否使用聚合函数进行过滤？NO
* 查询公司员工工作最大值，最小值，平均值，总和

```sql
SELECT MAX(salary), MIN(salary), AVG(salary), SUM(salary)
FROM employees;
```

* 查询个job_id的员工工资最大值，最小值，平均值，总和

```sql
SELECT job_id, MAX(salary), MIN(salary), AVG(salary), SUM(salary)
FROM employees
GROUP BY job_id;
```

* 选择各个job_id的员工人数

```sql
SELECT job_id, COUNT(*)
FROM employees
GROUP BY job_id;
```

* 查询员工最高工资与最低工资的差距

```sql
SELECT MAX(salary) - MIN(salary) Difference
FROM employees;
```

* 查询各个管理者首先员工的最低工资，其中最低工资不能低于6000，没有管理者的员工不计算在内

```sql
SELECT manager_id, MIN(salary)
FROM employees
WHERE manager_id IS NOT NULL
GROUP BY manager_id
HAVING MIN(salary) >= 6000;
```

* 查询所有部门名字，location_id，员工数量和平均工资，并按平均工资降序

```sql
SELECT dept.department_name, dept.location_id, COUNT(emp.employee_id), AVG(salary)
FROM departments dept LEFT OUTER JOIN employees emp
ON dept.department_id = emp.department_id
GROUP BY dept.department_name, dept.location_id
HAVING AVG(salary) IS NOT NULL
ORDER BY AVG(salary) DESC;
```

* 查询每个工种、每个部门的部门名、工种名和最低工资

```sql
SELECT dept.department_name, emp.job_id, MIN(salary)
FROM departments dept LEFT OUTER JOIN employees emp
ON dept.department_id = emp.department_id
GROUP BY dept.department_name, emp.job_id;
```

### 第九章 子查询

>子查询分类:
>单行子查询、多行子查询
>相关子查询、不相关子查询

```sql
#谁的工资比Abel的高？
#子查询先于主查询执行
#外查询（主查询），内查询（子查询）
SELECT last_name, salary
FROM employees
WHERE salary > 
(SELECT salary FROM employees WHERE last_name = 'Abel');
#自连接完成上述练习
SELECT e1.last_name, e1.salary
FROM employees e1 JOIN employees e2
ON e1.salary > e2.salary
WHERE e2.last_name = 'Abel';
```

#### 单行子查询

>内查询/子查询的结果为一个

```sql
#查询工资大于149号员工工资的员工的信息
SELECT last_name, salary
FROM employees
WHERE salary > 
(SELECT salary FROM employees WHERE employee_id = '149');

#返回job_id与141号员工相同，salary比143号员工多的员工姓名，job_id和工资
SELECT last_name, job_id, salary
FROM employees
WHERE job_id =
(SELECT job_id FROM employees WHERE employee_id = '141')
AND salary >
(SELECT salary FROM employees WHERE employee_id = '143')

#返回公司工资最少的员工的last_name,job_id和salary
SELECT last_name, job_id, salary
FROM employees
WHERE salary =
(SELECT MIN(salary) FROM employees)

#查询与141号或174号员工的department_id和manager_id相同的其他员工
#的employee_id,manager_id,department_id
#方式1
SELECT employee_id, manager_id, department_id
FROM employees
WHERE manager_id = 
(SELECT manager_id FROM employees WHERE employee_id = 141)
AND department_id =
(SELECT department_id FROM employees WHERE employee_id = 141)
AND employee_id != 141
#方式2 了解 不是多行查询
SELECT employee_id, manager_id, department_id
FROM employees
WHERE (manager_id, department_id) = 
(SELECT manager_id, department_id FROM employees WHERE employee_id = 141)
AND employee_id != 141

#查询最低工资大于110号部门最低工资的部门id和其最低工资
SELECT department_id, MIN(salary)
FROM employees
WHERE department_id IS NOT NULL
GROUP BY department_id
HAVING MIN(salary) > 
(SELECT MIN(salary) FROM employees WHERE department_id = 110)

#显示员工的employee_id,last_name和location。其中，若员工department_id与location_id
#为1800的department_id相同，则location为'Canada',其余则为'USA'
SELECT employee_id, last_name, CASE department_id 
WHEN (SELECT department_id FROM departments WHERE location_id = 1800) THEN 'Canada'
ELSE 'USA'
END location
FROM employees;
```

#### 多行子查询

> 也称为集合比较子查询
>
> 内查询返回多行
>
> 使用多行比较符:IN, ANY, ALL, SOME(同ANY)

```sql
#IN
SELECT employee_id, last_name
FROM employees
WHERE salary IN
(SELECT MIN(salary) FROM employees GROUP BY department_id)

#ANY
#返回其它job_id中比job_id为'IT_PROG'部门任一工资低的员工的员工号、
#姓名、job_id、salary
SELECT employee_id, last_name, job_id, salary
FROM employees
WHERE salary < ANY
(SELECT salary FROM employees WHERE job_id = 'IT_PROG')
AND job_id != 'IT_PROG'

#ALL
#返回其它job_id中比job_id为'IT_PROG'部门所有工资低的员工的员工号、
#姓名、job_id、salary
SELECT employee_id, last_name, job_id, salary
FROM employees
WHERE salary < ALL
(SELECT salary FROM employees WHERE job_id = 'IT_PROG')
AND job_id != 'IT_PROG'

#查询平均工资最低的部门id
#子查询当作表，要起别名
#方式1
SELECT department_id
FROM employees
GROUP BY department_id
HAVING AVG(salary) =
(SELECT MIN(avg_salary)
FROM (SELECT AVG(salary) avg_salary
FROM employees
WHERE department_id IS NOT NULL
GROUP BY department_id
ORDER BY AVG(salary)) dept_avg_salary)
#方式2
SELECT department_id
FROM employees
GROUP BY department_id
HAVING AVG(salary) <= ALL
(SELECT AVG(salary)
FROM employees
WHERE department_id IS NOT NULL
GROUP BY department_id
ORDER BY AVG(salary))
#方式3
SELECT department_id
FROM employees
WHERE department_id IS NOT NULL
GROUP BY department_id
ORDER BY AVG(salary)
LIMIT 0,1

#空值问题要排除
SELECT last_name
FROM employees
WHERE employee_id NOT IN
(SELECT manager_id FROM employees WHERE manager_id IS NOT NULL)
```

> 看黑马的视频了。原来那个好长