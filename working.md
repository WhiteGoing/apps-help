# 1. SQL

##1.1 关系代数

#### 1. 关系代数的基本操作

1. 并(Union)：　R∪S

   关系R和关系S具有相同的n个属性，且相应的属性取自同一个域，可以将其进行并运算，结果仍为n元关系。并运算要求两个属性的性质必须一致，并且运算的结果要**消除重复的元组**。

2. 差(Difference): R－S

   关系R和关系S具有相同的n个属性，且相应的属性取自同一个域，可以将其进行差运算，结果仍为n元关系。结果为属于R而不属于S的所有元组组成。

3. 笛卡儿积(Cartesian Product): R×S

   设关系R和S的元数分别为r和s。定义R和S的笛卡儿积R×S是一个(r+s)元的元组集合，每个元组的前r个分量(属性值)来自R的一个元组，后s个分量是S的一个元组，记为R×S。

4. 投影(Projection): πi,j(R)

   这个操作是对一个关系进行**垂直分割**，消除某些列，并重新安排的顺序，再扇区重复元组。

   如，πi,j(R)操作是只留下那个R表的i,j两行，再去重

5. 选择(Selection): σb>’4’ (R) 

   这个操作是对一个关系进行**水平分割**，b>'4'是选择的行应该要满足的条件。

#### 2. 关系代数的组合操作

1. 交(Intersection): R∩S

   设关系R和关系S具有相同的元数n，且相应的属性取自同一个域，则关系R和关系S的交由既属于关系R又属于关系S的元组组成，其结果仍为n元的关系。关系的交可以由关系的差来表示:R∩S = R - (R - S)或R∩S = S - (S - R)

2. **联接（Join）**：

   联接操作可将两个关系连在一起，形成一个新的关系，是**笛卡儿积和选择操作**的组合，分为θ联接和F联接两种。

   > 1. **θ**联接：**θ**联接是从关系R和S的笛卡儿积中选取属性值满足某个**θ**操作的元组，记为：
   >
   >    ![image](https://images2015.cnblogs.com/blog/872539/201610/872539-20161020123151935-261097945.png)
   >
   >    当θ为等号时称为等值联接
   >
   >    其值相当于：σ(RXS)　及先进行笛卡儿积操作X，再进行选择操作σ
   >
   > 2. F联接：与θ操作类似，只是最终结果需要满足F公式

3. 自然联接(Natural Join)：　R⋈S

   自然联接是一种特殊的等值联接，它要求两个关系中进行比较的分量必须是相同的属性组，并要在结果中把重复的属性去掉。

   和等值联接唯一的区别就是：去除了多余的属性，及选择时的属性有两组，去除S中的那一组即可

   其相当于：先进行笛卡儿积X，再进行选择操作σ，最后进行投影操作π（删除多余属性）

4. 除(Division): R÷S 

## 1.2 SQL语言

###1. SELECT语句的句法

SELECT语句的完整句法如下：

```sql
SELECT [DISTINCT] 目标表的列名或列表达式序列（全部属性可以用*来表示）
FROM 基本表名(或)视图序列表|表引用
[ WHERE 行条件表达式 ]
[ GROUP BY 列名序列
 	[ HAVING 组条件表达式　]]
[ ORDER BY 列名　[ ASC|DESC], ... ]
```

整个语句的执行过程如下：

> 1. 读取FROM子句中基本表、视图的数据，执行笛卡儿积操作(或读取表引用所返回的查询结果或多表联接的结果)
> 2. 选取满足WHERE子句中给出的条件表达式的元组
> 3. 按GROUP子句中指定列的值分组，同时提取满足HAVING子句中组条件表达式的那些组
> 4. 按SELECT子句中给出的列名或列表达式求值输出
> 5. ORDER子句对输出的目标表进行排序，按附加说明ASC升序排列，或按DESC降序排列

SELECT语句中，WHERE子句称为"行条件子句"，GROUP子句称为"分组子句"，HAVING子句称为"组条件子句"，ORDER子句称为"排序子句"



在WHERE中F可以使用的运算符有：

> 1. 算术运算符：<, <=, >, >=, <>或!=
> 2. 逻辑运算符：AND, OR, NOT
> 3. 集合成员资格运算符：IN, NOT IN
> 4. 谓词：EXISTS, ALL, SOME, UNIQUE, LIKE
> 5. 聚合函数：AVG(平均值), MIN, MAX, SUM, COUNT
> 6. 还可以是其它SELECT语句的嵌套
> 7. IS NULL, BETWEEN .... AND...., 

###2. 联接操作

联接条件可在WHERE中指定，也可以在FROM子句中指定。

​									联接类型及其说明：

|       连接类型       |                    说明                    |
| :--------------: | :--------------------------------------: |
|    INNER JOIN    |           内联接：结果为两个联接表中的匹配行的联接           |
| LEFT OUTER JOIN  | 左外联接：结果包括左表（出现在JOIN子句的左边）中的所有行，不包括右表中的不匹配行 |
| RIGHT OUTER JOIN | 右外联接：结果包括右表（出现在JOIN子句的右边）中的所有行，不包括左表中的不匹配行 |
| FULL OUTER JOIN  |      完全外联接：结果包括所有联接表中的所有行，不论它们是否匹配       |
|    CROSS JOIN    | 交叉联接：结果包括两个联接表中所有可能的行组合，交叉联接返回的是两个表的笛卡儿积 |

​								联接条件和说明

|  连接条件   |                    说明                    |
| :-----: | :--------------------------------------: |
| ON　联接条件 | 具体列出两个关系出现在哪些相应属性上做联接条件比较，联接条件应该写在联接类型的右边 |

实例：

内联接：

```sql
SELECT DISTINCT X.JNO
FROM SPJ X INNER JOIN
	 SPJ Y ON X.JNO=Y.JNO
WHERE (X.PNO='P3') AND (Y.PNO='P5')
```

外联接：

```sql
SELECT J.JNO, J.JNAME, SPJ1.SNO, SPJ1.QTY AS P3_QTY     # AS可以省略
FROM J FULL OUTER JOIN
	(SELECT * FROM SPJ WHERE PNO='P3') SPJ1 ON J.JNO=SPJ1.JNO;
```



###3. 聚合函数

> COUNT (*), COUNT(列名), SUM(列名), AVG(列名), MAX(列名), MIN(列名)

实例：

```sql
SELECT COUNT(DISTINCT(SNO)) COUNT_P3
FROM SPJ
WHERE PNO='P3';
```



###4. 数据分组

将查询到的结果进行分组，然后再对每个分组进行统计，SQL提供了GROUP BY子句和HAVING子句

例子：

```sql
SELECT SPJ.JNO, COUNT(DISTINCT (SPJ.PNO)) COUNT_PNO, SUM(QTY) SUM_QTY
FROM J, SPJ
WHERE J.JCITY = ‘上海’ AND J.JNO=SPJ.JNO
GROUP BY SPJ.JNO
HAVING COUNT(DISTINCT (PNO))>3
ORDER BY 2,3 DESC;                   # 第二行升序，第三行降序
```



### 5. 集合操作

1.  集合的并、交、差操作

   当两个子查询结果的结构完全相同时，可以让这两个子查询进行下述操作：

   > 1. 并操作
   >
   >    (SELECT 查询语句１)
   >
   >    UNION  [ALL]
   >
   >    (SELECT 查询语句２)
   >
   >    上面的ALL是可选的，在有ALL时，表示返回的结果没有去重，反之去重了，下面的类似
   >
   > 2. 交操作
   >
   >    (SELECT 查询语句１)
   >
   >    INTERSECT  [ALL]
   >
   >    (SELECT 查询语句２)
   >
   > 3. 差操作
   >
   >    (SELECT 查询语句１)
   >
   >    EXCEPT  [ALL]
   >
   >    (SELECT 查询语句２)

2. 集合的比较操作

   在SELECT语句的条件下，允许进行集合的比较操作。这类操作有：集合成员的资格比较、集合成员的算术比较、空关系的测试以及重复元组的测试

   > 1. 资格比较
   >
   >    元组　IN (NOT IN)      (集合)
   >
   >    集合可以是一个SELECT语句，或者时元组的集合，但要求其与前面的元组结构相同
   >
   >    ```sql
   >    SELECT JNO, JNAME
   >    FROM J
   >    WHERE JNO NOT IN 
   >    	(SELECT JNO
   >         FROM S, SPJ
   >         WHERE S.SNO=SPJ.SNO
   >         	AND PNO='P3'
   >        );
   >    ```
   >
   >    ​
   >
   > 2. 算术比较
   >
   >    元组　$\theta$   SOME　(集合)
   >
   >    元组　$\theta$   ALL　(集合)
   >
   >    要求：“元组”与集合中“元素”的结构一致，$\theta$是算术比较运算。“$\theta$　SOME”表示操作数左边的那个元组与右边集合中至少一个元素满足$\theta$运算（ALL则表示所有都满足，ANY是SOME的同义词）。
   >
   >    IN等同于"=SOME"
   >
   >    NOT IN等同于"<>ALL"
   >
   > 3. 空关系检测
   >
   >    (NOT) EXITSTS    (集合)
   >
   > 4. 重复元组检测
   >
   >    (NOT) UNIQUE   (集合)

3. SQL的数据更新

   1. 数据插入　INSERT：

      > 1. 插入单个元组
      >
      >    INSERT INTO 基本表名(列名表)
      >
      >    ​	　VALUES  (元组表)
      >
      >    VALUES后的元组值中列的顺序必须与基本表一一对应。如果基本表名后有列表名，则应该包括关系的所有非空的属性，如果基本表名后没有列表名，这表示插入元组的每个分量的值。
      >
      >    Insert ignore into    ; 忽略已经存在的数据
      >
      >    insert replace into   ;替换以及存在的数据
      >
      > 2. 插入子查询的结果
      >
      >    INSERT  INTO  基本表名(列名表)
      >
      >    ​	　SELECT 查询语句
      >
      >    ***由于SELECT子句后可跟表达式，而常量是表达式，因此可以用带SELECT语句的插入语句为新插入的元组的某些分量赋值。***
      >
      >    如：
      >
      >    INSERT INTO SPJ
      >
      >    SELECT SNO, PNO, 'J7', PRICE, 60
      >
      >    FROM SPJ
      >
      >    WHERE JNO='J1';
      >
      > 3. 数据删除
      >
      >    DELETE FROM <表名>
      >
      >    WHERE <条件表达式>
      >
      > 4. 数据修改
      >
      >    UPDATE 基本表名
      >
      >    SET 列名＝值表达式[, 列名＝值表达式...]
      >
      >    [WHERE 条件表达式]
      >
      >    修改指定表中满足条件表达式的元组中的指定属性值，SET指用‘值表达式’代替之前的值，WHERE没有的话表示修改所有的值
      >
      >    如：
      >
      >    UPDATE SPJ
      >
      >    SET PRICE=PRICE * (1+0.6)
      >
      >    WHERE PNO='P4'
      >
      >    AND SNO='S5'
      >
      > 5. 用 **ALTER TABLE ... ADD ... **语句可以向已存在的表插入新字段（column），并且能够与创建表时一样，在字段名和数据类型后加入NOT NULL、DEFAULT等限定，[可参考](http://www.runoob.com/sqlite/sqlite-alter-command.html)
      >
      >    ​
      >
      > 6. 对视图的更新操作
      >
      >    对视图元组的更新操作(INSERT, DELETE, UPDATE)有如下三条规则：
      >
      >    > 1. 如果一个视图是从**多个基本表**使用**联接操作**导出的，那么**不允许**对这个视图执行更新操作
      >    > 2. 如果在到处视图的过程中使用了**分组**和**聚合操作**，也**不允许**对这个视图执行更新操作
      >    > 3. 如果视图是从单个基本表使用**选择**、**投影**操作导出的，并且包含了基本表的**主键或某个候选键**，这样的视图称为“行列子集视图”，**可以**被执行更新操作。
      >
      >    在SQL2中，允许更新的视图在定义时，必须加上"WITH CHECK OPTION"

   2. ​

   ​

### 5. 补充

1. sql语句根据条件查询指定数量的数据

   SELECT * form 表名 WHERE 条件 limit 5,10; //检索6-15条数据

   SELECT * form 表名 WHERE 条件 limit 5,-1; //检索6到最后一条数据

   SELECT * form 表名 WHERE 条件 limit 5; //检索前5条数据 

2. 使用CREATE 语句创建索引

   CREATE INDEX index_name ON table_name(column_name,column_name) include(score)

   普通索引

   CREATE  INDEX index_name ON table_name (column_name) ;

   唯一索引

   CREATE UNIQUE INDEX index_name ON table_name (column_name) ;

   非空索引

   CREATE PRIMARY KEY INDEX index_name ON table_name (column_name) ;

   主键索引 

   使用ALTER TABLE　语句创建索引

   alter table table_name add index index_name (column_list) ;

   alter table table_name add unique (column_list) ;

   alter table table_name add primary key (column_list) ;

   删除索引

   drop index index_name on table_name ;

   alter table table_name drop index index_name ;

   alter table table_name drop primary key ;

   强制索引

   题目：针对salaries表emp_no字段创建索引idx_emp_no，查询emp_no为10005, 使用强制索引。

   SQLite中，使用 INDEXED BY 语句进行强制索引查询，[可参考](http://www.runoob.com/sqlite/sqlite-indexed-by.html)

   `SELECT * FROM salaries INDEXED BY idx_emp_no WHERE emp_no = '10005'`

     MySQL中，使用 FORCE INDEX 语句进行强制索引查询，[可参考](http://www.jb51.net/article/49807.htm)  

   `SELECT * FROM salaries FORCE INDEX idx_emp_no WHERE emp_no = '10005'`

3. 视图

   ```sql
   # 创建
   CREATE VIEW view_name AS
   SELECT column_name(s)
   FROM table_name
   WHERE condition

   ＃更新　SQL CREATE OR REPLACE VIEW Syntax
   CREATE OR REPLACE VIEW view_name AS
   SELECT column_name(s)
   FROM table_name
   WHERE condition

   ＃撤销　SQL DROP VIEW Syntax
   DROP VIEW view_name
   ```

4. 触发器

   创建一个Update触发器： 

   ```sql
    Create Trigger truStudent 

          On Student                         --在Student表中创建触发器 

          for Update                          --为什么事件触发 delete...

        As                                        --事件触发后所要做的事情 

          if Update(StudentID)            

          begin 

            Update BorrowRecord 

              Set StudentID=i.StudentID 

              From BorrowRecord br , Deleted   d ,Inserted i      --Deleted和Inserted临时表 

              Where br.StudentID=d.StudentID 

          end 

   ```

   ​     理解触发器里面的两个临时的表：Deleted , Inserted 。注意Deleted 与Inserted分别表示触发事件的表“旧的一条记录”和“新的一条记录”。 

5. ​