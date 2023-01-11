# SQL_task5
本笔记为阿里云天池龙珠计划SQL训练营的学习内容，链接为：https://tianchi.aliyun.com/specials/promotion/aicampsql；
一、窗口函数
1.概念
窗口函数也叫OLAP函数，OLAP是Online Analytical Processing的简称，意思是对数据库数据进行实时分析处理，为便于理解，称之为窗口函数。常规的SELECT语句都是对整张表进行查询，而窗口函数可以让我们有选择的取某一部分数据进行汇总、计算和排序。
2.通用形式
<窗口函数> OVER ([PARTITION BY <列名>]（可省略）
ORDER BY <排序用列名>)
PARTITION BY设定窗口对象范围，类似于GROUP BY子句的分组功能，但是没有汇总功能，不会改变原始表中记录的行数。通过PARTITION BY分组后的记录的集合可以称为窗口。
ORDER BY用于排序。
3.注意
窗口函数只能在SELECT子句中使用；

二、窗口函数种类
1.专用窗口函数
（1）RANK函数
计算排序时，相同位次的记录会被跳过：
有3条记录排在第1位时：1,1,1,4....
（2）DENSE_RANK函数
计算排序时，相同位次的记录不会被跳过：
           有3条记录排在第1位时：1,1,1,2....
（3）ROW_NUMBER函数
赋予唯一的连续位次
           有3条记录排在第1位时：1,2,3,4....

2.聚合函数
使用方法无差，但计算结果是一个累计的聚合函数值，即当前所在行及之前所有行的聚合

三、窗口函数应用——计算移动平均
聚合函数在窗口函数中应用时，计算的是累计到当前行的所有数据的聚合；实际上可以指定更加详细的汇总范围，这个汇总范围称为框架（frame）。
<窗口函数> OVER ( ORDER BY <排序用列名>
ROWS n PRECEDING )

<窗口函数> OVER ( ORDER BY <排序用列名>
ROWS BETWEEN n PRECEDING AND n FOLLOWING)

PRECEDING：将框架指定为：截止到之前n行+自身行
FOLLOWING：将框架指定为：截止到之后n行+自身行
BETWEEN 1 PRECEDING AND 1 FOLLOWING：将框架指定为：之前1行+之后1行+自身行

四、GROUPING运算符
GROUP BY ... WITH ROLLUP 用于计算分类的合计

练习题
5.1
得到的结果：product_id,product_name,sale_price以及所售产品中按照id升序排序后的最高价格
5.2
SELECT
regist_date,
product_name,
sale_price,
SUM(sale_price) OVER (ORDER BY regist_date NULLS FIRST) AS price_sum
FROM
Product;

5.3
1)窗口函数不指定PARTITION BY就是对整个数据进行排序，不指定窗口
2)本质是因为SQL的执行顺序
FROM-->WHERE-->GROUP BY-->HAVING-->SELECT-->ORDER BY
如果在WHERE,GROUP BY,HAVING中使用窗口函数，就是提前进行了一次排序，排序之后再进行过滤就没有实际意义了，而ORDER BY语句执行顺序在SELECT语句之后，所以可以使用。
