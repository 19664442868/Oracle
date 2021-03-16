## 试验一

### 1. 试验目的

分析SQL执行计划，执行SQL语句的优化指导。理解分析SQL语句的执行计划的重要作用。

### 2. 试验内容

* 对Oracle12c中的HR人力资源管理系统中的表进行查询与分析。
* 首先运行和分析教材中的样例：本训练任务目的是查询两个部门('IT'和'Sales')的部门总人数和平均工资，以下两个查询的结果是一样的。但效率不相同。
* 设计自己的查询语句，并作相应的分析，查询语句不能太简单。

#### 2.1 查询语句1

```sql
set autotrace on

SELECT d.department_name,count(e.job_id)as "部门总人数",
avg(e.salary)as "平均工资"
from hr.departments d,hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT','Sales')
GROUP BY d.department_name;
```

##### 执行时间

![执行时间图](.\pic\1.png)

##### 执行结果

![执行结果图](.\pic\2.png)

##### SQL优化建议

![优化建议](.\pic\3.png)

#### 2.2 查询语句2

```sql
set autotrace on

SELECT d.department_name,count(e.job_id)as "部门总人数",
avg(e.salary)as "平均工资"
FROM hr.departments d,hr.employees e
WHERE d.department_id = e.department_id
GROUP BY d.department_name
HAVING d.department_name in ('IT','Sales');
```

##### 执行时间

![执行时间](.\pic\4.png)

##### 执行结果

![执行结果](.\pic\5.png)

##### SQL优化建议

![优化建议](.\pic\6.png)

#### 2.3 SQL分析

 从查询执行完成用时可以看出，查询2的效率更高；从输出的执行计划可以看出，查询2比查询1耗费的cpu资源更多，统计信息中查询2的逻辑读次数比查询1多出一倍，而且查询2也比查询1多用到了一个索引，因此语句2的查询效率更高。

### 3. 自定义查询语句

#### 3.1查询SQL

查询在IT和Sales部门中工资大于4500的员工信息

```sql
set autotrace on

select 
e.first_name ,e.salary,d.department_name
from hr.employees e,hr.departments d
where e.department_id=d.department_id 
and e.salary>4500 
and d.department_name in('IT','Sales');
```

#### 3.2 分析

##### 执行时间

![](.\pic\7.png)

##### 执行结果

![](.\pic\8.png)

##### SQL优化建议

![](.\pic\9.png)

### 4.试验总结

在这次试验中，通过对相同功能的两条查询语句自动跟踪，分析统计结果和耗时来判断查询效率的高低。通过这次试验我了解到如何自动跟踪查询SQL，分析在磁盘和内存查询的次数，判断查询语句的好坏，通常情况下，在磁盘查询次数越多，查询语句的效率就越低，所以在查询时，应尽量避免大量的全表扫描，多使用索引提高查询的效率。试验中遇到的问题就是上传GitHub时，一开始图片没有显示，是因为一开始图片的路径时绝对路径，上传到服务器后，就找不到图片了，更改图片的路径为相对路径后得到解决。