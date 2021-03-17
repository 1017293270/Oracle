实验一

1.
SELECT *
FROM hr.departments
ORDER BY department_id;

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/biao1.png)

SELECT *
FROM hr.employees
ORDER BY employee_id;

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/biao2.png)

2.

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/sql1.png)

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/sql2.png)

查询1进行了一次索引搜索，一次全表搜索，是先过滤再汇总；查询2进行了一次索引搜索，两次全表搜索，是先汇总再过滤，参与汇总与计算的数据比查询1更多，即查询1更优。

3.
SELECT first_name as "员工姓名",department_name as "部门",
salary as "平均工资"
FROM hr.departments NATURAL JOIN hr.employees
ORDER BY salary DESC;

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/mysql.png)
