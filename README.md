# mysql-skill
一些在mysql中的知识点
<h3>索引</h3>
1.索引是表的目录，在查找内容之前可以先在目录中查找索引位置，以此快速定位查询数据。对于索引，会保存在额外的文件中<br>
2.索引分类：<br>
普通索引、唯一索引(主键默认唯一索引)、组合索引、全文索引（仅可以适用于MyISAM引擎的数据表） <br>
<b>注：对于创建的多列索引,只要查询的条件中用到了最左边的列,索引一般就会被使用</b><br>
如:<br>
组合索引(a,b,c,d)<br>
查询条件为a,用到了索引a<br>
查询条件为b or c or d,没用到索引<br>
查询条件为a,b 用到索引a,b<br>
查询条件为a,c or a,d 用到索引a<br>
（索引不可跳）<br>
<hr>
从MySQL5.5版本之后，MySQL的默认内置存储引擎已经是InnoDB了，他的主要特点有：<br>

（1）灾难恢复性比较好；<br>
（2）支持事务。默认的事务隔离级别为可重复度，通过MVCC（并发版本控制）来实现的;<br>
（3）使用的锁粒度为行级锁，可以支持更高的并发；<br>
（4）支持外键；<br>
（5）配合一些热备工具可以支持在线热备份；<br>
（6）在InnoDB中存在着缓冲管理，通过缓冲池，将索引和数据全部缓存起来，加快查询的速度；<br>
（7）对于InnoDB类型的表，其数据的物理组织形式是聚簇表。所有的数据按照主键来组织。数据和索引放在一块，都位于B+数的叶子节点上；<br>
<hr>
<h3>mysql提高查询效率</h3>
1.参数为子查询用exists代替in
  -- 慢
SELECT *
FROM Class_A
WHERE id IN (SELECT id
FROM Class_B);

-- 快
SELECT *
FROM Class_A A
WHERE EXISTS
(SELECT *
FROM Class_B B
WHERE A.id = B.id);

2.避免排序
有时查询的时候不排序，可以提高查询速度
排序的子句：
group by 子句
order by 子句
聚合函数(sum、count、avg、max、min)
distinct
集合运算符(union、intersect、except)
窗口函数(rank、row_number等)

使用union all 代替union
使用exists 代替distinct
使用where代替having

3.在索引列上使用函数索引会失效
使用 is null会失效，使用or有时会失效，使用否定式会失效，索引列的顺序错误会失效

4.减少中间表
