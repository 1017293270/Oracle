实验二

1.创建自己的用户并授权。

$ sqlplus system/123@//202.115.82.8:1521/pdborcl

SQL> CREATE ROLE role_clown;

SQL> GRANT connect,resource,CREATE VIEW TO role_clown;

SQL> CREATE USER ylxc IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;

SQL> ALTER USER ylxc QUOTA 50M ON users;

SQL> GRANT role_clown TO ylxc;

SQL> exit

2.新用户ylxc连接到pdborcl，创建表mytable和视图myview，插入数据，最后将myview的SELECT对象权限授予同学创建的xtx用户。

$ sqlplus ylxc/123@//202.115.82.8:1521/pdborcl

SQL> show user;

SQL> CREATE TABLE mytable (id number,name varchar(50));

SQL> INSERT INTO mytable(id,name)VALUES(1,'ylxc');

SQL> CREATE VIEW myview AS SELECT name FROM mytable;

SQL> SELECT * FROM myview;

SQL> GRANT SELECT ON myview TO xtx;

SQL>exit

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test21.png)

3.用户txt连接到pdborcl，查询ylxc授予它的视图myview。

$ sqlplus xtx/123@//202.115.82.8:1521/pdborcl

SQL> SELECT * FROM ylxc.myview;

SQL> exit

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test22.png)