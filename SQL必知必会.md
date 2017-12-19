办法就是使用 DISTINCT 关键字，顾名思义，它指示数据库只返回不同
的值。
SELECT DISTINCT vend_id
FROM Products;
SELECT DISTINCT vend_id 告诉 DBMS 只返回不同（具有唯一性）的
vend_id 行，所以正如下面的输出，只有 3 行。如果使用 DISTINCT 关
键字，它必须直接放在列名的前面。

DISTINCT 关键字作用于所有的列，不仅仅是跟在其后的那一列。例

如，你指定 SELECT DISTINCT vend_id, prod_price ，因为指定的
两列不完全相同，所以所有的行都会被检索出来。