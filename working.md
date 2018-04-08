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

3. ​