# MySQL多表查询与事务
## 学习目标
1. 能够使用内连接进行多表查询
2. 能够使用左外连接和右外连接进行多表查询
3. 能够使用子查询进行多表查询
4. 能够理解多表查询的规律
5. 能够理解事务的概念
6. 能够说出事务的原理
7. 能够在MySQL中使用事务
8. 能够理解脏读,不可重复读,幻读的概念及解决办法
9. 能够使用DCL处理MySQL中的用户



# 第1章 多表查询

## 1.1 什么是多表查询

同时查询多张表获取到需要的数据
比如：我们想查询到开发部有多少人，需要将部门表和员工表同时进行查询
![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A201.png)

**多表查询的分类**：
![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A202.png)

**准备数据**:

```sql
-- 创建部门表
CREATE TABLE dept (
  id INT PRIMARY KEY AUTO_INCREMENT,
  NAME VARCHAR(20)
);

INSERT INTO dept (NAME) VALUES ('开发部'),('市场部'),('财务部');

-- 创建员工表
CREATE TABLE emp (
  id INT PRIMARY KEY AUTO_INCREMENT,
  NAME VARCHAR(10),
  gender CHAR(1),   -- 性别
  salary DOUBLE,   -- 工资
  join_date DATE,  -- 入职日期
  dept_id INT
);

INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('孙悟空','男',7200,'2013-02-24',1);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('猪八戒','男',3600,'2010-12-02',2);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('唐僧','男',9000,'2008-08-08',2);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('白骨精','女',5000,'2015-10-07',3);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('蜘蛛精','女',4500,'2011-03-14',1);
```

## 1.2 笛卡尔积现象

### 1.2.1 什么是笛卡尔积现象

多表查询时左表的每条数据和右表的每条数据组合，这种效果成为笛卡尔积

需求：查询每个部门有哪些人

具体操作：

```sql
SELECT * FROM dept, emp;
```

![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A203.png)

以上数据其实是左表的每条数据和右表的每条数据组合。左表有3条，右表有5条，最终组合后3*5=15条数据。

**左表的每条数据和右表的每条数据组合，这种效果称为笛卡尔乘积**
![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A204.png)

### 1.2.2 如何清除笛卡尔积现象的影响

我们发现不是所有的数据组合都是有用的，只有员工表.dept_id = 部门表.id 的数据才是有用的。所以需要通过条件过滤掉没用的数据。
![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A205.png)

```sql
SELECT * FROM dept, emp WHERE emp.`dept_id`=dept.`id`; 
```

![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A206.png)

## 1.3 内连接

**用左边表的记录去匹配右边表的记录，如果符合条件的则显示**

### 1.3.1 隐式内连接

隐式内连接：看不到`JOIN`关键字，条件使用`WHERE`指定
`SELECT 字段名 FROM 左表, 右表 WHERE 条件；`

### 1.3.2 显示内连接

显示内连接：使用`INNER JOIN ... ON`语句, 可以省略`INNER`
`SELECT 字段名 FROM 左表 INNER JOIN 右表 ON 条件；`

具体操作：

- 查询唐僧的信息，显示员工id，姓名，性别，工资和所在的部门名称，我们发现需要联合2张表同时才能查询出需要的数据，我们使用内连接

![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A215.png)

1. 确定查询哪些表

```sql
SELECT * FROM dept INNER JOIN emp;
```

![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A207.png)

1. 确定表连接条件，员工表.dept_id = 部门表.id 的数据才是有效的

```sql
SELECT * FROM dept INNER JOIN emp ON emp.`dept_id`=dept.`id`;
```

![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A208.png)

1. 确定表连接条件，我们查询的是唐僧的信息，部门表.name='唐僧'

```sql
SELECT * FROM dept INNER JOIN emp ON emp.`dept_id`=dept.`id` AND emp.`NAME`='唐僧';
```

![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A209.png)

1. 确定查询字段，查询唐僧的信息，显示员工id，姓名，性别，工资和所在的部门名称

```sql
SELECT emp.`id`, emp.`NAME`, emp.`gender`, emp.`salary`, dept.`NAME` FROM dept INNER JOIN emp ON emp.`dept_id`=dept.`id` AND emp.`NAME`='唐僧';
```

![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A210.png)

1. 我们发现写表名有点长，可以给表取别名，显示的字段名也使用别名

```sql
SELECT e.`id` 员工编号, e.`NAME` 员工姓名, e.`gender` 性别, e.`salary` 工资, d.`NAME` 部门名称 FROM dept d INNER JOIN emp e ON e.`dept_id`=d.`id` AND e.`NAME`='唐僧';
```

![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A211.png)

**总结内连接查询步骤：**

1. 确定查询哪些表
2. 确定表连接条件
3. 确定查询字段

## 1.4 左外连接

左外连接：使用`LEFT OUTER JOIN ... ON`，`OUTER`可以省略
`SELECT 字段名 FROM 左表 LEFT OUTER JOIN 右表 ON 条件；`
用左边表的记录去匹配右边表的记录，如果符合条件的则显示；否则，显示NULL
可以理解为：在内连接的基础上保证左表的数据全部显示

具体操作：

- 在部门表中增加一个销售部

```sql
INSERT INTO dept (NAME) VALUES ('销售部');
```

![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A212.png)

- 使用内连接查询

```sql
SELECT * FROM dept INNER JOIN emp ON emp.`dept_id`=dept.`id`;
```

![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A213.png)

- 使用左外连接查询

```sql
SELECT * FROM dept LEFT OUTER JOIN emp ON emp.`dept_id`=dept.`id`;
```

![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A214.png)

## 1.5 右外连接

右外连接：使用`RIGHT OUTER JOIN ... ON`，`OUTER`可以省略
`SELECT 字段名 FROM 左表 RIGHT OUTER JOIN 右表 ON 条件；`
用右边表的记录去匹配左边表的记录，如果符合条件的则显示；否则，显示NULL
可以理解为：在内连接的基础上保证右表的数据全部显示

具体操作：

- 在员工表中增加一个员工
  INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('沙僧','男',6666,'2013-02-24',NULL);
  ![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A217.png)
- 使用内连接查询
  SELECT * FROM dept INNER JOIN emp ON emp.`dept_id`=dept.`id`;
  ![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A213.png)
- 使用右外连接查询
  SELECT * FROM dept RIGHT OUTER JOIN emp ON emp.`dept_id`=dept.`id`;
  ![](imgs/%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A216.png)

## 1.6 子查询

一条SELECT语句结果作为另一条SELECT语法一部分（查询条件，查询结果，表）
`SELECT 查询字段 FROM 表 WHERE 查询条件;`
`SELECT * FROM employee WHERE salary=(SELECT MAX(salary) FROM employee);`
![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A201.png)
子查询需要放在（）中

子查询结果的三种情况：

1. 子查询的结果是一个值的时候
   ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A202.png)
2. 子查询结果是单例多行的时候
   ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A203.png)
3. 子查询的结果是多行多列
   ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A204.png)

> 说明：
> 子查询结果只要是`单列`，肯定在`WHERE`后面作为`条件`
> 子查询结果只要是`多列`，肯定在`FROM`后面作为`表`

### 1.1.1 子查询的结果是一个值的时候

子查询结果只要是`单列`，肯定在`WHERE`后面作为`条件`
`SELECT 查询字段 FROM 表 WHERE 字段=（子查询）;`

1. **查询工资最高的员工是谁？** 

   1. 查询最高工资是多少

   ```sql
     SELECT MAX(salary) FROM emp;
   ```

     ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A205.png)

   1. 根据最高工资到员工表查询到对应的员工信息

   ```sql
     SELECT * FROM emp WHERE salary=(SELECT MAX(salary) FROM emp);
   ```

     ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A206.png)

2. **查询工资小于平均工资的员工有哪些？**

   1. 查询平均工资是多少

   ```sql
     SELECT AVG(salary) FROM emp;
   ```

     ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A207.png)

   1. 到员工表查询小于平均的员工信息

   ```sql
     SELECT * FROM emp WHERE salary < (SELECT AVG(salary) FROM emp);
   ```

     ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A208.png)

### 1.1.2 子查询结果是单例多行的时候

子查询结果只要是`单列`，肯定在`WHERE`后面作为`条件`
子查询结果是单例多行，结果集类似于一个数组，父查询使用`IN`运算符
`SELECT 查询字段 FROM 表 WHERE 字段 IN （子查询）;`

1. **查询工资大于5000的员工，来自于哪些部门的名字**  

   1. 先查询大于5000的员工所在的部门id

   ```sql
     SELECT dept_id FROM emp WHERE salary > 5000;
   ```

     ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A209.png)

   1. 再查询在这些部门id中部门的名字

   ```sql
     SELECT dept.name FROM dept WHERE dept.id IN (SELECT dept_id FROM emp WHERE salary > 5000);
   ```

     ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A210.png)

2. **查询开发部与财务部所有的员工信息**

   1. 先查询开发部与财务部的id

   ```sql
   SELECT id FROM dept WHERE NAME IN('开发部','财务部');
   ```

   ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A211.png)

   1. 再查询在这些部门id中有哪些员工

   ```sql
   SELECT * FROM emp WHERE dept_id IN (SELECT id FROM dept WHERE NAME IN('开发部','财务部'));
   ```

   ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A212.png)

### 1.1.3 子查询的结果是多行多列

子查询结果只要是`多列`，肯定在`FROM`后面作为`表`
`SELECT 查询字段 FROM （子查询） 表别名 WHERE 条件;`
子查询作为表需要取别名，否则这张表没用名称无法访问表中的字段

- **查询出2011年以后入职的员工信息，包括部门名称**

  1. 在员工表中查询2011-1-1以后入职的员工

  ```sql
  SELECT * FROM emp WHERE join_date > '2011-1-1';
  ```

  ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A214.png)

  1. 查询所有的部门信息，与上面的虚拟表中的信息组合，找出所有部门id等于的dept_id

  ```sql
  SELECT * FROM dept d, (SELECT * FROM emp WHERE join_date > '2011-1-1') e WHERE e.dept_id = d.id;
  ```

  ![](imgs/%E5%AD%90%E6%9F%A5%E8%AF%A213.png)

使用表连接：

```sql
SELECT d.*, e.* FROM dept d INNER JOIN emp e ON d.id = e.dept_id WHERE e.join_date > '2011-1-1';
```

### 1.1.4 多表查询总结

> - 子查询结果只要是`单列`，肯定在`WHERE`后面作为`条件`
>   `SELECT 查询字段 FROM 表 WHERE 字段=（子查询）;`
> - 子查询结果只要是`多列`，肯定在`FROM`后面作为`表`
>   `SELECT 查询字段 FROM （子查询） 表别名 WHERE 条件;`

# 第2章 多表查询案例

​	我们在公司开发中，根据不同的业务需求往往需要通过2张及以上的表中去查询需要的数据。所以我们有必要学习2张及以上的表的查询。其实不管是几张表的查询，都是有规律可循的。
## 2.1 准备数据
```sql
-- 部门表
CREATE TABLE dept (
  id INT PRIMARY KEY PRIMARY KEY, -- 部门id
  dname VARCHAR(50), -- 部门名称
  loc VARCHAR(50) -- 部门位置
);

-- 添加4个部门
INSERT INTO dept(id,dname,loc) VALUES 
(10,'教研部','北京'),
(20,'学工部','上海'),
(30,'销售部','广州'),
(40,'财务部','深圳');

-- 职务表，职务名称，职务描述
CREATE TABLE job (
  id INT PRIMARY KEY,
  jname VARCHAR(20),
  description VARCHAR(50)
);

-- 添加4个职务
INSERT INTO job (id, jname, description) VALUES
(1, '董事长', '管理整个公司，接单'),
(2, '经理', '管理部门员工'),
(3, '销售员', '向客人推销产品'),
(4, '文员', '使用办公软件');

-- 员工表
CREATE TABLE emp (
  id INT PRIMARY KEY, -- 员工id
  ename VARCHAR(50), -- 员工姓名
  job_id INT, -- 职务id
  mgr INT , -- 上级领导
  joindate DATE, -- 入职日期
  salary DECIMAL(7,2), -- 工资
  bonus DECIMAL(7,2), -- 奖金
  dept_id INT, -- 所在部门编号
  CONSTRAINT emp_jobid_ref_job_id_fk FOREIGN KEY (job_id) REFERENCES job (id),
  CONSTRAINT emp_deptid_ref_dept_id_fk FOREIGN KEY (dept_id) REFERENCES dept (id)
);

-- 添加员工
INSERT INTO emp(id,ename,job_id,mgr,joindate,salary,bonus,dept_id) VALUES 
(1001,'孙悟空',4,1004,'2000-12-17','8000.00',NULL,20),
(1002,'卢俊义',3,1006,'2001-02-20','16000.00','3000.00',30),
(1003,'林冲',3,1006,'2001-02-22','12500.00','5000.00',30),
(1004,'唐僧',2,1009,'2001-04-02','29750.00',NULL,20),
(1005,'李逵',4,1006,'2001-09-28','12500.00','14000.00',30),
(1006,'宋江',2,1009,'2001-05-01','28500.00',NULL,30),
(1007,'刘备',2,1009,'2001-09-01','24500.00',NULL,10),
(1008,'猪八戒',4,1004,'2007-04-19','30000.00',NULL,20),
(1009,'罗贯中',1,NULL,'2001-11-17','50000.00',NULL,10),
(1010,'吴用',3,1006,'2001-09-08','15000.00','0.00',30),
(1011,'沙僧',4,1004,'2007-05-23','11000.00',NULL,20),
(1012,'李逵',4,1006,'2001-12-03','9500.00',NULL,30),
(1013,'小白龙',4,1004,'2001-12-03','30000.00',NULL,20),
(1014,'关羽',4,1007,'2002-01-23','13000.00',NULL,10);

-- 工资等级表
CREATE TABLE salarygrade (
  grade INT PRIMARY KEY,
  losalary INT,
  hisalary INT
);

-- 添加5个工资等级
INSERT INTO salarygrade(grade,losalary,hisalary) VALUES 
(1,7000,12000),
(2,12010,14000),
(3,14010,20000),
(4,20010,30000),
(5,30010,99990);
```
分析4张表的关系：通过4张表可以查出一个员工的所有信息
![](imgs\多表查询01.png)

## 2.2 练习
### 2.2.1 练习1
查询所有员工信息。显示员工编号，员工姓名，工资，职务名称，职务描述

具体操作：
   1.确定要查询哪些表：emp e, job j

   ```sql
   SELECT * FROM emp e INNER JOIN job j;
   ```
   ![](imgs\多表查询02.png)

   2.确定表连接条件： e.job_id=j.id
   ```sql
   SELECT * FROM emp e INNER JOIN job j ON e.job_id=j.id;
   ```
   ![](imgs\多表查询03.png)
   ![](imgs\多表查询04.png)

   3.确定查询字段：员工编号，员工姓名，工资，职务名称，职务描述
   ```sql
   SELECT e.`id`, e.`ename`, e.`salary`, j.`jname`, j.`description` FROM emp e INNER JOIN job j ON e.job_id=j.id;
   ```
   ![](imgs\多表查询05.png)


### 2.2.2 练习2
查询所有员工信息。显示员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置

具体操作：
      1. 确定要查询哪些表，emp e, job j, dept d
   ```sql
   SELECT * FROM emp e INNER JOIN job j INNER JOIN dept d;
   ```
   ![](imgs\多表查询06.png)
   ![](imgs\多表查询07.png)

      2. 确定表连接条件 e.job_id=j.id and e.dept_id=d.id
   ```sql
   SELECT * FROM emp e INNER JOIN job j INNER JOIN dept d ON e.job_id=j.id AND e.dept_id=d.id;
   ```
   ![](imgs\多表查询08.png)
   ![](imgs\多表查询09.png)

      3. 确定查询字段：员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置
   ```sql
   SELECT e.`id`, e.`ename`, e.`salary`, j.`jname`, j.`description`, d.`dname`, d.`loc` FROM emp e INNER JOIN job j INNER JOIN dept d ON e.job_id=j.id AND e.dept_id=d.id;
   ```
   ![](imgs\多表查询10.png)

### 2.2.3 练习3
查询所有员工信息。显示员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级

具体操作：
      1. 确定要查询哪些表，emp e, job j, dept d, salarygrade s
   ```sql
   SELECT * FROM emp e INNER JOIN job j INNER JOIN dept d INNER JOIN salarygrade s;
   ```
   ![](imgs\多表查询11.png)
   ![](imgs\多表查询12.png)

      2. 确定表连接条件 e.job_id=j.id and e.dept_id=d.id and e.salary between s.losalary and hisalary
   ```sql
   SELECT * FROM emp e INNER JOIN job j INNER JOIN dept d INNER JOIN salarygrade s ON e.job_id=j.id AND e.dept_id=d.id AND e.salary BETWEEN s.losalary AND hisalary;
   ```
   ![](imgs\多表查询13.png)
   ![](imgs\多表查询14.png)

      3. 确定查询字段：员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级
   ```sql
   SELECT e.`ename`, e.`salary`, j.`jname`, j.`description`, d.`dname`, d.`loc`, s.`grade` FROM emp e INNER JOIN job j INNER JOIN dept d INNER JOIN salarygrade s ON e.job_id=j.id AND e.dept_id=d.id AND e.salary BETWEEN s.losalary AND hisalary;
   ```
   ![](imgs\多表查询15.png)
#### 1.2.3.1 多表查询规律总结
1. 不管我们查询几张表，表连接查询会产出笛卡尔积，我们需要消除笛卡尔积，拿到正确的数据。我们需要找到表与表之间通过哪个字段关联起来的（通常是`外键=主键`）
2. 消除笛卡尔积规律：2张表需要1个条件，3张表需要2个条件，4张表需要3个条件。（条件数量=表的数量-1），每张表都要参与进来
3. 多表连接查询步骤：
   3.1. 确定要查询哪些表
   3.2. 确定表连接条件
   3.3. 确定查询字段

### 2.2.4 练习4
查询经理的信息。显示员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级

具体操作：
      1. 确定要查询哪些表，emp e, job j, dept d, salarygrade s
   ```sql
     SELECT * FROM emp e INNER JOIN job j INNER JOIN dept d INNER JOIN salarygrade s;
   ```
   ![](imgs\多表查询11.png)
   ![](imgs\多表查询12.png)

      2. 确定表连接条件 e.job_id=j.id and e.dept_id=d.id and e.salary between s.losalary and hisalary
   ```sql
   SELECT * FROM emp e INNER JOIN job j INNER JOIN dept d INNER JOIN salarygrade s ON e.job_id=j.id AND e.dept_id=d.id AND e.salary BETWEEN s.losalary AND hisalary;
   ```
   ![](imgs\多表查询13.png)
   ![](imgs\多表查询14.png)

   额外条件：只需要查询经理的信息（j.jname='经理'）
   ![](imgs\多表查询16.png)

      3. 确定查询字段：员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级
   ```sql
   SELECT e.`ename`, e.`salary`, j.`jname`, j.`description`, d.`dname`, d.`loc`, s.`grade` FROM emp e INNER JOIN job j INNER JOIN dept d INNER JOIN salarygrade s ON e.job_id=j.id AND e.dept_id=d.id AND e.salary BETWEEN s.losalary AND hisalary AND j.jname='经理';
   ```
   ![](imgs\多表查询17.png)

### 2.2.5 练习5
查询出部门编号、部门名称、部门位置、部门人数

具体操作：
      1. 去员工表中找到每个部门的人数和部门id
   ```sql
   SELECT dept_id, COUNT(*) FROM emp GROUP BY dept_id;
   ```
   ![](imgs\多表查询18.png)

      2. 再和部门表连接查询
   ```sql
   SELECT * FROM dept d INNER JOIN (SELECT dept_id, COUNT(*) FROM emp GROUP BY dept_id) e ON e.dept_id=d.`id`;
   ```
   ![](imgs\多表查询19.png)
   ![](imgs\多表查询20.png)

      3. 显示对应的字段
   ```sql
   SELECT d.`id`, d.dname, d.`loc`, e.total 部门人数 FROM dept d INNER JOIN (SELECT dept_id, COUNT(*) total FROM emp GROUP BY dept_id) e ON e.dept_id=d.`id`;
   ```
   ![](imgs\多表查询21.png)
   最终效果：
   ![](imgs\多表查询22.png)

### 2.2.6 练习6
查询所有员工信息。显示员工信息和部门名称，没有员工的部门也要显示

具体操作：
      1. 确定要查询哪些表，emp e, dept d
   ```sql
   SELECT * FROM emp e INNER JOIN dept d;
   ```
   ![](imgs\多表查询23.png)
   ![](imgs\多表查询24.png)

      2. 确定表连接条件 e.dept_id=d.id，没有员工的部门也要显示。右边数据要全部显示所以使用右外连接
   ```sql
   SELECT * FROM emp e RIGHT JOIN dept d ON e.dept_id=d.id;
   ```
   ![](imgs\多表查询25.png)
   ![](imgs\多表查询26.png)

   注意：没有员工的部门也要显示。右边数据要全部显示所以使用右外连接
   ![](imgs\多表查询27.png)

      3. 确定查询字段：员工信息，部门名称
   ```sql
   SELECT e.*, d.`dname` FROM emp e RIGHT JOIN dept d ON e.dept_id=d.id;
   ```
   ![](imgs\多表查询28.png)

### 2.2.7 练习7
查询所有员工信息。显示员工姓名，员工工资，职务名称，工资等级，并按工资升序排序

具体操作：
      1. 确定要查询哪些表，emp e, job j, salarygrade s
   ```sql
   SELECT * FROM emp e INNER JOIN job j INNER JOIN salarygrade s;
   ```
   ![](imgs\多表查询29.png)
   ![](imgs\多表查询30.png)

      2. 确定表连接条件 e.job_id=j.id and e.salary between s.losalary and s.hisalary
   ```sql
   SELECT * FROM emp e INNER JOIN job j INNER JOIN salarygrade s ON e.job_id=j.id AND e.salary BETWEEN s.losalary AND s.hisalary;
   ```
   ![](imgs\多表查询31.png)

      3. 确定查询字段：员工姓名，员工工资，工资等级，并按工资升序排序
   ```sql
   SELECT e.`ename`, j.`jname`, e.`salary`, s.`grade` FROM emp e INNER JOIN job j INNER JOIN salarygrade s ON e.job_id=j.id AND e.salary BETWEEN s.losalary AND s.hisalary ORDER BY e.`salary`;
   ```
   ![](imgs\多表查询32.png)

### 2.2.8 练习8
列出所有员工的姓名及其直接上级的姓名,没有领导的员工也需要显示

具体操作：
   ![](imgs\多表查询33.png)
      1. 确定要查询哪些表，emp e, emp m
   ```sql
   SELECT * FROM emp e INNER JOIN emp m;
   ```
   ![](imgs\多表查询34.png)

      2. 确定表连接条件 e.mgr=e2.id
   ```sql
   SELECT * FROM emp e INNER JOIN emp m ON e.`mgr`=m.`id`;
   SELECT * FROM emp e LEFT JOIN emp m ON e.`mgr`=m.`id`;
   ```
   ![](imgs\多表查询35.png)

      **我们发现少了一条数据，因为罗贯中是董事长没有上司**
      没有领导的员工也需要显示，所以左表的数据需要全部显示。使用左外连接
      ![](imgs\多表查询36.png)
    
      3. 确定查询字段：员工的姓名及其直接上级的姓名
   ```sql
   SELECT e.`ename`, IFNULL(m.`ename`, '没有') 上司 FROM emp e LEFT JOIN emp m ON e.`mgr`=m.`id`;
   ```
   ![](imgs\多表查询37.png)

### 2.2.9 练习9
查询入职期早于直接上级的所有员工编号、姓名、部门名称

具体操作：
      1. 确定要查询哪些表，emp e, emp m, dept d
   ```sql
   SELECT * FROM emp e INNER JOIN emp m INNER JOIN dept d;
   ```
   ![](imgs\多表查询38.png)

      2. 确定表连接条件 e.mgr=m.id and e.dept_id=d.id and e.dept_id=d.id and e.joindate<m.joindate
   ```sql
   SELECT * FROM emp e INNER JOIN emp m INNER JOIN dept d ON e.mgr=m.id AND e.dept_id=d.id AND e.joindate<m.joindate;
   ```
   ![](imgs\多表查询39.png)

      3. 确定查询字段：员工编号、姓名、部门名称
   ```sql
   SELECT e.`id`, e.`ename`, d.`dname` FROM emp e INNER JOIN emp m INNER JOIN dept d ON e.mgr=m.id AND e.joindate<m.joindate AND e.`dept_id`=d.`id`;
   ```
   ![](imgs\多表查询40.png)

### 2.2.10 练习10
查询工资高于公司平均工资的所有员工信息。显示员工信息，部门名称，上级领导，工资等级

具体操作：
  **先统计公司平均工资**
  ```sql
  SELECT AVG(salary) FROM emp;
  ```
  ![](imgs\多表查询41.png)
      1. 确定要查询哪些表，emp e, emp m, dept d, salarygrade s
  ```sql
  SELECT * FROM emp e INNER JOIN emp m INNER JOIN dept d INNER JOIN salarygrade s;
  ```
  ![](imgs\多表查询42.png)

      2. 确定表连接条件 e.dept_id=d.id and e.mgr=m.id and e.salary between s.losalary and hisalary and e.salary>公司平均薪金
  ```sql
  SELECT * FROM emp e INNER JOIN emp m INNER JOIN dept d INNER JOIN salarygrade s WHERE e.dept_id=d.id AND e.mgr=m.id AND e.salary BETWEEN s.losalary AND hisalary AND e.salary>(SELECT AVG(salary) FROM emp);
  ```
  ![](imgs\多表查询43.png)

      3. 确定查询字段：员工信息，部门名称，上级领导，工资等级。
  ```sql
  SELECT e.*, d.`dname`, m.`ename`, s.`grade` FROM emp e INNER JOIN emp m INNER JOIN dept d INNER JOIN salarygrade s WHERE e.dept_id=d.id AND e.mgr=m.id AND e.salary BETWEEN s.losalary AND hisalary AND e.salary>(SELECT AVG(salary) FROM emp);
  ```
  ![](imgs\多表查询44.png)


# 第3章 事务安全
## 3.1 事务的应用场景说明
​	在实际的业务开发中，有些业务操作要多次访问数据库。一个业务要发送多条SQL语句给数据库执行。需要将多次访问数据库的操作视为一个整体来执行，要么所有的SQL语句全部执行成功。如果其中有一条SQL语句失败，就进行事务的回滚，所有的SQL语句全部执行失败。
例如: 张三给李四转账，张三账号减钱，李四账号加钱
```sql
-- 创建数据表
CREATE TABLE account (
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(10),
	balance DOUBLE
);

-- 添加数据
INSERT INTO account (NAME, balance) VALUES ('张三', 1000), ('李四', 1000);
```

模拟张三给李四转500元钱，一个转账的业务操作最少要执行下面的2条语句：
1. 张三账号-500
2. 李四账号+500
```sql
-- 1. 张三账号-500
UPDATE account SET balance = balance - 500 WHERE id=1;
-- 2. 李四账号+500
UPDATE account SET balance = balance + 500 WHERE id=2;
```
​	假设当张三账号上-500元,服务器崩溃了。李四的账号并没有+500元，数据就出现问题了。我们需要保证其中一条SQL语句出现问题，整个转账就算失败。只有两条SQL都成功了转账才算成功。这个时候就需要用到事务

## 3.2 操作事务
MYSQL中可以有两种方式进行事务的操作：`1.手动提交事务`，`2.自动提交事务`

### 3.2.1 手动提交事务
  事务有关的SQL语句：
| SQL语句              | 描述   |
| ------------------ | ---- |
| start transaction; | 开启事务 |
| commit;            | 提交事务 |
| rollback;          | 回滚事务 |

**手动提交事务使用步骤**：
    第1种情况：开启事务 -> 执行SQL语句 -> 成功 -> 提交事务
    第2种情况：开启事务 -> 执行SQL语句 -> 失败 -> 回滚事务
   ![](imgs\事务01.png)


**案例演示1**：模拟张三给李四转500元钱（成功）
目前数据库数据如下：
![](imgs\事务03.png)

1. 使用DOS控制台进入MySQL
2. 执行以下SQL语句： `1.开启事务`， `2.张三账号-500`， `3.李四账号+500`
   ```sql
   START TRANSACTION;
   UPDATE account SET balance = balance - 500 WHERE id=1;
   UPDATE account SET balance = balance + 500 WHERE id=2;
   ```
   ![](imgs\事务02.png)
3. 使用SQLYog查看数据库：发现数据并没有改变
   ![](imgs\事务03.png)
4. 在控制台执行`commit`提交任务：
   ![](imgs\事务04.png)
5. 使用SQLYog查看数据库：发现数据改变
   ![](imgs\事务05.png)

---
**案例演示2**：模拟张三给李四转500元钱（失败）
目前数据库数据如下：
![](imgs\事务05.png)
1. 在控制台执行以下SQL语句：`1.开启事务`， `2.张三账号-500`
   ```sql
   START TRANSACTION;
   UPDATE account SET balance = balance - 500 WHERE id=1;
   ```
   ![](imgs\事务06.png)
2. 使用SQLYog查看数据库：发现数据并没有改变
   ![](imgs\事务07.png)
3. 在控制台执行`rollback`回滚事务：
   ![](imgs\事务08.png)
4. 使用SQLYog查看数据库：发现数据没有改变
   ![](imgs\事务09.png)

>总结:
>如果事务中SQL语句没有问题，commit提交事务，会对数据库数据的数据进行改变。
>如果事务中SQL语句有问题，rollback回滚事务，会回退到开启事务时的状态。

### 3.2.2 自动提交事务
​	MySQL的每一条DML(增删改)语句都是一个单独的事务，每条语句都会自动开启一个事务，执行完毕自动提交事务，MySQL默认开始自动提交事务
![](imgs\事务10.png)

1. 将金额重置为1000
   ![](imgs\事务11.png)

2. 执行以下SQL语句
   ```sql
   UPDATE account SET balance = balance - 500 WHERE id=1;
   ```

3. 使用SQLYog查看数据库：发现数据已经改变
   ![](imgs\事务12.png)

   通过修改MySQL全局变量"autocommit"，取消自动提交事务
   使用SQL语句：`show variables like '%commit%';`查看MySQL是否开启自动提交事务
   ![](imgs\事务13.png)
   0:OFF（关闭自动提交）
   1:ON（开启自动提交）

4. 取消自动提交事务，设置自动提交的参数为OFF，执行SQL语句：`set autocommit = 0;`
   ![](imgs\事务14.png)

5. 在控制台执行以下SQL语句：张三-500
   ```sql
   UPDATE account SET balance = balance - 500 WHERE id=1;
   ```
   ![](imgs\事务15.png)

6. 使用SQLYog查看数据库，发现数据并没有改变
   ![](imgs\事务16.png)

7. 在控制台执行`commit`提交任务
   ![](imgs\事务17.png)

8. 使用SQLYog查看数据库，发现数据改变
   ![](imgs\事务18.png)

## 3.3 事务原理
​	事务开启之后, 所有的操作都会临时保存到事务日志, 事务日志只有在得到`commit`命令才会同步到数据表中，其他任何情况都会清空事务日志(rollback，断开连接)
   ![](imgs\事务19.png)

## 3.4 回滚点
​	在某些成功的操作完成之后，后续的操作有可能成功有可能失败，但是不管成功还是失败，前面操作都已经成功，可以在当前成功的位置设置一个回滚点。可以供后续失败操作返回到该位置，而不是返回所有操作，这个点称之为回滚点。

设置回滚点语法：`savepoint 回滚点名字;`
回到回滚点语法: `rollback to 回滚点名字;`

具体操作：
1. 将数据还原到1000
   ![](imgs\事务20.png)
2. 开启事务
   ![](imgs\事务21.png)
3. 让张三账号减3次钱
   ```sql
   UPDATE account SET balance = balance - 10 WHERE id=1;
   UPDATE account SET balance = balance - 10 WHERE id=1;
   UPDATE account SET balance = balance - 10 WHERE id=1;
   ```
   ![](imgs\事务22.png)
4. 设置回滚点：`savepoint abc;`
   ![](imgs\事务23.png)
5. 让张三账号减4次钱
   ```sql
   UPDATE account SET balance = balance - 10 WHERE id=1;
   UPDATE account SET balance = balance - 10 WHERE id=1;
   UPDATE account SET balance = balance - 10 WHERE id=1;
   UPDATE account SET balance = balance - 10 WHERE id=1;
   ```
   ![](imgs\事务24.png)
6. 回到回滚点：`rollback to abc;`
   ![](imgs\事务25.png)
7. 分析过程
   ![](imgs\事务26.png)

>总结：设置回滚点可以让我们在失败的时候回到回滚点，而不是回到事务开启的时候。

## 3.5 事务的四大特性
### 3.5.1 事务的四大特性
| 事务特性             | 含义                                       |
| ---------------- | ---------------------------------------- |
| 原子性（Atomicity）   | 事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。       |
| 一致性（Consistency） | 事务前后数据的完整性必须保持一致                         |
| 隔离性（Isolation）   | 是指多个用户并发访问数据库时，一个用户的事务不能被其它用户的事务所干扰，多个并发事务之间数据要相互隔离，不能相互影响。 |
| 持久性（Durability）  | 指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来即使数据库发生故障也不应该对其有任何影响 |

### 3.5.2 事务的隔离级别
事务在操作时的理想状态：多个事务之间互不影响，如果隔离级别设置不当就可能引发并发访问问题。
| 并发访问的问题 | 含义                                       |
| ------- | ---------------------------------------- |
| 脏读      | 一个事务读取到了另一个事务中尚未提交的数据                    |
| 不可重复读   | 一个事务中两次读取的数据内容不一致，要求的是一个事务中多次读取时数据是一致的，这是事务update时引发的问题 |
| 幻读      | 一个事务中两次读取的数据的数量不一致，要求在一个事务多次读取的数据的数量是一致的，这是insert或delete时引发的问题 |
   ![](imgs\事务58.png)
   ![](imgs\事务59.png)
   ![](imgs\事务60.png)



MySQL数据库有四种隔离级别：上面的级别最低，下面的级别最高。“是”表示会出现这种问题，“否”表示不会出现这种问题。

| 级别   | 名字   | 隔离级别             | 脏读   | 不可重复读 | 幻读   | 数据库默认隔离级别         |
| ---- | ---- | ---------------- | ---- | ----- | ---- | ----------------- |
| 1    | 读未提交 | read uncommitted | 是    | 是     | 是    |                   |
| 2    | 读已提交 | read committed   | 否    | 是     | 是    | Oracle和SQL Server |
| 3    | 可重复读 | repeatable read  | 否    | 否     | 是    | MySQL             |
| 4    | 串行化  | serializable     | 否    | 否     | 否    |                   |

**MySQL事务隔离级别相关的命令**
1. 查询全局事务隔离级别
   ```sql
   show variables like '%isolation%';
   -- 或
   select @@tx_isolation;
   ```
   ![](imgs\事务27.png)

2. 设置事务隔离级别，需要退出MSQL再进入MYSQL才能看到隔离级别的变化
   ```sql
   set global transaction isolation level 级别字符串;
   -- 如:
   set global transaction isolation level read uncommitted;
   ```
   ![](imgs\事务28.png)


#### 2.5.2.1 脏读的演示
将数据进行恢复：`UPDATE account SET balance = 1000;`
1. 打开A窗口登录MySQL，设置全局的隔离级别为最低
   ```sql
   mysql -uroot -proot
   set global transaction isolation level read uncommitted;
   ```
   ![](imgs\事务29.png)
2. 打开B窗口,AB窗口都开启事务
   ```sql
   use day23;
   start transaction;
   ```
   ![](imgs\事务30.png)
3. A窗口更新2个人的账户数据，未提交
   ```sql
   update account set balance=balance-500 where id=1;
   update account set balance=balance+500 where id=2;
   ```
   ![](imgs\事务31.png)
4. B窗口查询账户
   ```sql
   select * from account;
   ```
   ![](imgs\事务32.png)

5. A窗口回滚
   ```sql
   rollback;
   ```
   ![](imgs\事务33.png)
6. B窗口查询账户，钱没了
   ![](imgs\事务34.png)

脏读非常危险的，比如张三向李四购买商品，张三开启事务，向李四账号转入500块，然后打电话给李四说钱已经转了。李四一查询钱到账了，发货给张三。张三收到货后回滚事务，李四的再查看钱没了。

**解决脏读的问题**：将全局的隔离级别进行提升
将数据进行恢复：`UPDATE account SET balance = 1000;`
1. 在A窗口设置全局的隔离级别为`read committed`
   ```sql
   set global transaction isolation level read committed;
   ```
   ![](imgs\事务35.png)
2. B窗口退出MySQL，B窗口再进入MySQL
   ![](imgs\事务36.png)
3. AB窗口同时开启事务
   ![](imgs\事务37.png)
4. A更新2个人的账户，未提交
   ```sql
   update account set balance=balance-500 where id=1;
   update account set balance=balance+500 where id=2;
   ```
   ![](imgs\事务38.png)
5. B窗口查询账户
   ![](imgs\事务39.png)
6. A窗口commit提交事务
   ![](imgs\事务40.png)
7. B窗口查看账户
   ![](imgs\事务41.png)

>结论：read committed的方式可以避免脏读的发生



#### 2.5.2.2 不可重复读的演示
将数据进行恢复：`UPDATE account SET balance = 1000;`
1. 开启A窗口
   ```sql
   set global transaction isolation level read committed;
   ```
   ![](imgs\事务42.png)

2. 开启B窗口，在B窗口开启事务
   ```sql
   start transaction;
   select * from account;
   ```
   ![](imgs\事务43.png)
3. 在A窗口开启事务，并更新数据
   ```sql
   start transaction;
   update account set balance=balance+500 where id=1;
   commit;
   ```
   ![](imgs\事务44.png)

4. B窗口查询
   ```sql
   select * from account;
   ```
   ![](imgs\事务45.png)

两次查询输出的结果不同，到底哪次是对的？不知道以哪次为准。
很多人认为这种情况就对了，无须困惑，当然是后面的为准。我们可以考虑这样一种情况，比如银行程序需要将查询结果分别输出到电脑屏幕和发短信给客户，结果在一个事务中针对不同的输出目的地进行的两次查询不一致，导致文件和屏幕中的结果不一致，银行工作人员就不知道以哪个为准了。

**解决不可重复读的问题**：将全局的隔离级别进行提升为：`repeatable read`
将数据进行恢复：`UPDATE account SET balance = 1000;`

1. A窗口设置隔离级别为：`repeatable read`
   ```sql
   set global transaction isolation level repeatable read;
   ```
   ![](imgs\事务46.png)

2. B窗口退出MySQL，B窗口再进入MySQL
   ```sql
   start transaction;
   select * from account;
   ```
   ![](imgs\事务47.png)

3. A窗口更新数据
   ```sql
   start transaction;
   update account set balance=balance+500 where id=1;
   commit;
   ```
   ![](imgs\事务48.png)

4. B窗口查询
   ```sql
   select * from account;
   ```
   ![](imgs\事务49.png)

>结论：同一个事务中为了保证多次查询数据一致，必须使用`repeatable read`隔离级别
>![](imgs\事务50.png)



#### 2.5.2.3 幻读的演示
**在MySQL中无法看到幻读的效果**。但我们可以将事务隔离级别设置到最高，以挡住幻读的发生
将数据进行恢复：`UPDATE account SET balance = 1000;`

1. 开启A窗口
   ```sql
   set global transaction isolation level serializable; -- 设置隔离级别为最高
   ```
   ![](imgs\事务51.png)

2. A窗口退出MySQL，A窗口重新登录MySQL
   ```sql
   start transaction;
   select count(*) from account;
   ```
   ![](imgs\事务52.png)

3. 再开启B窗口，登录MySQL
4. 在B窗口中开启事务，添加一条记录
   ```sql
   start transaction; -- 开启事务
   insert into account (name,balance) values ('LaoWang', 500);
   ```
   ![](imgs\事务53.png)
5. 在A窗口中commit提交事务，B窗口中insert语句会在A窗口事务提交后立马运行
   ![](imgs\事务54.png)
6. 在A窗口中接着查询，发现数据不变
   ```sql
   select count(*) from account;
   ```
   ![](imgs\事务55.png)
7. B窗口中commit提交当前事务
   ![](imgs\事务56.png)
8. A窗口就能看到最新的数据
   ![](imgs\事务57.png)
>结论：使用serializable隔离级别，一个事务没有执行完，其他事务的SQL执行不了，可以挡住幻读


# 第4章 DCL
​	我们现在默认使用的都是root用户，超级管理员，拥有全部的权限。但是，一个公司里面的数据库服务器上面可能同时运行着很多个项目的数据库。所以，我们应该可以根据不同的项目建立不同的用户，分配不同的权限来管理和维护数据库。
   ![](imgs\DCL01.png)
## 4.1 创建用户
`CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';`
**关键字说明：**
      1. `用户名`：将创建的用户名
      2. `主机名`：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符%
      3. `密码`：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器

**具体操作：**
```sql
-- user1用户只能在localhost这个IP登录mysql服务器
CREATE USER 'user1'@'localhost' IDENTIFIED BY '123';
-- user2用户可以在任何电脑上登录mysql服务器
CREATE USER 'user2'@'%' IDENTIFIED BY '123';
```

## 4.2 授权用户
用户创建之后，基本没什么权限！需要给用户授权
![](imgs\DCL02.png)

**授权格式**：
`GRANT 权限1, 权限2... ON 数据库名.表名 TO '用户名'@'主机名';`
**关键字说明**：
      1. `GRANT` 授权关键字
      2. 授予用户的权限，如`SELECT`，`INSERT`，`UPDATE`等。如果要授予所的权限则使用`ALL`
      3. `数据库名.表名`：该用户可以操作哪个数据库的哪些表。如果要授予该用户对所有数据库和表的相应操作权限则可用*表示，如`*.*`
      4. `'用户名'@'主机名'`: 给哪个用户授权


**具体操作：**

1. 给user1用户分配对test这个数据库操作的权限
   ```sql
   GRANT CREATE,ALTER,DROP,INSERT,UPDATE,DELETE,SELECT ON test.* TO 'user1'@'localhost';
   ```
   ![](imgs\DCL03.png)
2. 给user2用户分配对所有数据库操作的权限
   ```sql
   GRANT ALL ON *.* TO 'user2'@'%';
   ```
   ![](imgs\DCL04.png)

## 4.3 撤销授权
`REVOKE  权限1, 权限2... ON 数据库.表名 FROM '用户名'@'主机名';`

**具体操作：**
* 撤销user1用户对test操作的权限
   ```sql
   REVOKE ALL ON test.* FROM 'user1'@'localhost';
   ```
   ![](imgs\DCL05.png)

## 4.4 查看权限
`SHOW GRANTS FOR '用户名'@'主机名';`
**具体操作：**
* 查看user1用户的权限
   ```sql
   SHOW GRANTS FOR 'user1'@'localhost';
   ```
   ![](imgs\DCL06.png)

## 4.5 删除用户
`DROP USER '用户名'@'主机名';`
**具体操作：**
* 删除user2
   ```sql
    DROP USER 'user2'@'%';
   ```
   ![](imgs\DCL07.png)

## 4.6 修改用户密码
### 4.6.1 修改管理员密码
`mysqladmin -uroot -p password 新密码  -- 新密码不需要加上引号`
>注意：需要在未登陆MySQL的情况下操作。

**具体操作：**
   ```sql
mysqladmin -uroot -p password 123456
输入老密码
   ```
   ![](imgs\DCL08.png)

### 4.6.2 修改普通用户密码
`set password for '用户名'@'主机名' = password('新密码');`
>注意：需要在登陆MySQL的情况下操作。

**具体操作：**
   ```sql
`set password for 'user1'@'localhost' = password('666666');`
   ```
   ![](imgs\DCL09.png)