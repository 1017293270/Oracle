
期末项目设计报告
题    目	基于Oracle的职工信息的数据库设计

1.引言
1.1编写目的

《职工考勤管理信息系统》是为实现职工考勤信息管理的现代化，运用Oracle数据库管理系统开发的应用软件。它旨在通过职工基本信息、职工加班信息、职工出勤信息、职工出差信息和职工请假信息这五方面完成对职工的考勤。利用计算机技术做出这样一个系统就节省了很多人力资源，实现了科学、高效的职工考勤信息管理目标。

1.2术语表

序号	术语或缩略语	说明性定义
1	ATTENDANCE_NUM	该考勤项的天数
2	SALARY	职工的月薪
3	ATTENDANCE_PRICE	考勤所得金额
4	F_SALARY	最后实际的薪资

1.3参考资料

Oracle 12c数据库基础教程  科学出版社 主编赵卫东、刘永红、于曦

2.数据库环境

数据库系统	部署环境	设计工具

oracle	linux	sqldeveloper

3.逻辑设计

3.1实体模型

根据应用场景分析，共有3个原始的实体，分别是部门、职工和考勤项。

部门（DEPARTMENTS）:部门包括部门ID（DEPARTMENT_ID）和部门名称（DEPARTMENT_NAME），如图。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test61.png)

职工（STAFF）:职工包括职工ID（STAFF_ID），姓名（STAFF_NAME），性别（SEX），电话（PHONE_NUMBER），月薪（SALARY）等。职工的属性中还应该包括职工所属的部门ID（DEPARTMENT_ID），部门ID不能为空，表示职工必须属于某一个部门，此外应该还有职工的上司（MANAGER_ID），实际工资（F_SALARY），实际工资需要经过月薪加减考勤项的金额得出。职工的实体如图。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test62.png)

考勤项（ATTENDANCES）:考勤项包括考勤ID（出勤ID（ATTENDANCE_ID）或加班ID（OVER_ID）或出差ID（LEAVE_ID）或请假ID（TAKEOFF_ID）），考勤名称ATTENDANCE_NAME
（出勤（ATTENDANCE）或加班（OVER）或出差（LEAVE）或请假（TAKEOFF）），见图。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test63.png)

薪资（WAGES）：职工ID（STAFF_ID），姓名（STAFF_NAME），月薪（SALARY），考勤金额（ATTENDANCE_PRICE），实际工资（F_SALARY）等，如图。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test64.png)

薪资详单（WAGE_DETAILS）薪资详单的ID，职工ID（STAFF_ID），考勤ID（出勤ID（ATTENDANCE_ID）或加班ID（OVER_ID）或出差ID（LEAVE_ID）或请假ID（TAKEOFF_ID）），考勤天数DAY_NUM（加班天数（OVER_NUM）或请假天数（TAKEOFF_NUM）），以及加班金额（OVER_PRICE），缺勤金额（TAKEOFF_PRICE），奖金（BONUS）等，如图。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test65.png)

3.2实体联系模型

职工的实际薪资是需要对该月的考勤情况进行统计的，因此职工与考勤项存在一个“考勤”的联系，职工与考勤项之间的关系是多对多的关系，如图。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test66.png)

由于职工表中的部门不为空，即他们也存才“从属”关系，一个职工只可以属于一个部门，而一个部门可以包含多个职工，职工与部门之间的关系是多对一的关系，如图。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test67.png)

考虑到对职工薪资的计算属性太多，存在一个表中过于繁杂，于是将薪资关系细分为薪资与薪资详单两个实体，薪资中存储：职工ID（STAFF_ID），姓名（STAFF_NAME），月薪（SALARY），考勤金额（ATTENDANCE_PRICE），实际工资（F_SALARY），薪资详单中存储薪资详单的ID，职工ID（STAFF_ID），考勤ID及天数包括职工的所有考勤情况：加班天数（OVER_NUM），请假天数（TAKEOFF_NUM），以及加班金额（OVER_PRICE），超出规定请假后的缺勤金额（TAKEOFF_PRICE），全勤后的奖金（BONUS），考勤金额=奖金+加班金额*加班天数-缺勤金额*请假天数。实体间关系如图，职工每月都会有薪资，所以是多对多的关系，而一个职工每月薪资都有多个薪资详单，每个详单存储一类考勤项。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test68.png)

4.物理设计

4.1表汇总

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test69.png)

4.2数据库的设计
部门表（DEPARTMENTS）包括DEPARTMENT_ID和DEPARTMENT_NAME两个属性，见表。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test611.png)



职工表（STAFFS）包括职工ID（STAFF_ID），姓名（STAFF_NAME），性别（SEX），电话（PHONE_NUMBER），月薪（SALARY），所属的部门ID（DEPARTMENT_ID），职工的上司（MANAGER_ID），实际工资（F_SALARY），见表。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test610.png)


考勤项表（ATTENDANCES）考勤项包括考勤ID（ATTENDANCE_ID），考勤名称ATTENDANCE_NAME（出勤（ATTENDANCE）或加班（OVER）或出差（LEAVE）或请假（TAKEOFF）），见表。
字段名	数据类型	可以为空	注释

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test612.png)

薪资表（WAGES）包括职工ID（STAFF_ID），姓名（STAFF_NAME），月薪（SALARY），考勤金额（ATTENDANCE_PRICE），实际工资（F_SALARY），日期（DATE），见表。
薪资表中的实际工资即为职工表的实际工资，经过薪资表中的信息汇总计算得来，计算公式是：实际工资（F_SALARY）=月薪（SALARY）-考勤金额（ATTENDANCE_PRICE），而考勤金额是经过薪资详单中的信息汇总计算得来，计算公式是：考勤金额（ATTENDANCE_PRICE）=奖金（BONUS）+加班金额（OVER_PRICE）*加班天数（OVER_NUM）-缺勤金额（TAKEOFF_PRICE）*请假天数（TAKEOFF_NUM）。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test613.png)

薪资详单表（WAGE_DETAILS）包含薪资详单的ID，职工ID（STAFF_ID），出勤ID（ATTENDANCE_ID），考勤名称（ATTENDANCE_NAME），加班ID（OVER_ID），加班天数（OVER_NUM），出差ID（LEAVE_ID），请假ID（TAKEOFF_ID），请假天数（TAKEOFF_NUM），以及加班金额（OVER_PRICE），缺勤金额（TAKEOFF_PRICE），奖金（BONUS），日期（DATE），见表。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test614.png)

4.3用户创建与空间分配

设计好表的结构后，需要考虑用户与空间的分配问题。该系统的角色有两类，分别为管理员与职工，只有管理员同时拥有读写权限，职工只能进行读的权限。此外还新建了两个表空间USERS02，USERS03用于存储薪资记录。

使用SYSTEM（管理员）创建表空间USERS02，USERS03，表空间由两个数据文件组成，每个文件的初始大小都是100M，自动增长，每次扩展50M，文件大小无限制。

$ sqlplus system/123@pdborcl

SQL>create tablespace users02 datafile

‘/home/oracle/app/oracle/oradata/orcl/pdborcl/pdbtest_users02_1.dbf’

SIZE 100M AUTOEXTEND ON NEXT 50M MAXSIZE UNLIMITED,

‘/home/oracle/app/oracle/oradata/orcl/pdborcl/pdbtest_users02_2.dbf’

SIZE 100M AUTOEXTEND ON NEXT 50M MAXSIZE UNLIMITED

EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;

SQL>create tablespace users03 datafile

‘/home/oracle/app/oracle/oradata/orcl/pdborcl/pdbtest_users03_1.dbf’

SIZE 100M AUTOEXTEND ON NEXT 50M MAXSIZE UNLIMITED,

‘/home/oracle/app/oracle/oradata/orcl/pdborcl/pdbtest_users03_2.dbf’

SIZE 100M AUTOEXTEND ON NEXT 50M MAXSIZE UNLIMITED

EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;


表空间USERS02，USERS03创建完成后仍然使用SYSTEM创建角色staff_view和用户staff1，由于用户staff1仅为普通用户，所以授予connect，resource权限就足够了。

$ sqlplus system/123@pdborcl

SQL> CREATE ROLE staff_view;

SQL> GRANT connect,resource,CREATE VIEW TO staff_view;

SQL> CREATE USER staff1 IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;

SQL> ALTER USER staff1 QUOTA UNLIMITED ON users;

SQL> ALTER USER staff1 QUOTA UNLIMITED ON users02;

SQL> ALTER USER staff1 QUOTA UNLIMITED ON users03;

SQL> GRANT con_res_view TO staff1;

SQL> exit


由于企业职工多且每个职工的薪资记录也需要多条数据记录计算，数量非常大，所以将薪资表与薪资详单表设计为基于日期的分区表，以加快局部时间范围的查询速度。存储空间的分配规划如下表。

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test615.png)

4.4创建表，约束和索引
在用户和空间分配完成后就可以创建表了，创建表的命令为CREATE TABLE，在进行薪资表与薪资详单表的创建时需要按分区存储，分区类型选择“RANG”范围分区，核心的语句为“PARTITION BY RANGE(DATE)”，表示按日期DATE的范围进行分区，下面是薪资表的创建。

CREATE TABLE WAGES

(

  STAFF_ID NUMBER(10, 0) NOT NULL

, STAFF_NAME VARCHAR2(40 BYTE) NOT NULL

, DATE DATE NOT NULL

, SALARY NUMBER(8, 2) NOT NULL

, ATTENDANCE_PRICE NUMBER(8, 2) NOT NULL

, F_SALARY NUMBER(8, 2) NOT NULL

, CONSTRAINT WAGES_PK PRIMARY KEY

  (

    STAFF_ID , DATE

  )

TABLESPACE USERS

PCTFREE 10  INITRANS 1

STORAGE

(  BUFFER_POOL DEFAULT  )

NOCOMPRESS  NOPARALLEL

PARTITION BY RANGE (DATE)

(

  PARTITION PARTITION_BEFORE_2018 VALUES LESS THAN (TO_DATE(' 2018-01 ', 'SYYYY-MM', 'NLS_CALENDAR=GREGORIAN'))

  NOLOGGING

  TABLESPACE USERS

  PCTFREE 10

  INITRANS 1

  STORAGE

  (

    INITIAL 8388608

    NEXT 1048576

    MINEXTENTS 1

    MAXEXTENTS UNLIMITED

    BUFFER_POOL DEFAULT

  )

  NOCOMPRESS NO INMEMORY

, PARTITION PARTITION_BEFORE_2019 VALUES LESS THAN (TO_DATE(' 2019-01', 'SYYYY-MM', 'NLS_CALENDAR=GREGORIAN'))

  NOLOGGING

  TABLESPACE USERS02

  PCTFREE 10

   INITRANS 1

  STORAGE

  (

    BUFFER_POOL DEFAULT

  )

  NOCOMPRESS NO INMEMORY

, PARTITION PARTITION_AFTER_2019 VALUES MORE THAN (TO_DATE(' 2019-01', 'SYYYY-MM', 'NLS_CALENDAR=GREGORIAN'))

  NOLOGGING

  TABLESPACE USERS03

  PCTFREE 10

  INITRANS 1

  STORAGE

  (

    BUFFER_POOL DEFAULT

  )

  NOCOMPRESS NO INMEMORY

);

分区PARTITION_BEFORE_2018存储日期在2018年之前的职工薪资，这个分区储存在表空间USERS中，而分区PARTITION_BEFORE_2019存储日期在2018年到2019年的职工薪资，这个分区储存在表空间USERS02中，分区PARTITION_AFTER_2019存储日期在2019年之后的职工薪资，这个分区储存在表空间USERS03中。

由于WAGE_DETAILS是WAGES的从表，WAGE_DETAILS的记录数量比WAGES要多，所以WAGE_DETAILS表也要进行分区储存，同样是基于日期进行分区储存。

CREATE TABLE WAGE_DETALS

(

  ID NUMBER(12,0) NOT NULL

, STAFF_ID NUMBER(10, 0) NOT NULL

, ATTENDANCE_ID NUMBER(2, 0) NOT NULL

, ATTENDANCE_NAME VARCHAR2(20 BYTE)

, DATE DATE NOT NULL

, OVER_PRICE NUMBER(8, 2) NOT NULL

, OVER_NUM NUMBER(2, 0) NOT NULL

, TAKEOFF_PRICE NUMBER(8, 2) NOT NULL

, TAKEOFF_NUM NUMBER(2, 0) NOT NULL

, CONSTRAINT WAGES_PK PRIMARY KEY

  (   ID   )

TABLESPACE USERS

PCTFREE 10  INITRANS 1

STORAGE

(  BUFFER_POOL DEFAULT  )

NOCOMPRESS  NOPARALLEL

PARTITION BY RANGE (DATE)

(

  PARTITION PARTITION_BEFORE_2018 VALUES LESS THAN (TO_DATE(' 2018-01 ', 'SYYYY-MM', 'NLS_CALENDAR=GREGORIAN'))

  NOLOGGING

  TABLESPACE USERS

  PCTFREE 10

  INITRANS 1

  STORAGE

  (

    INITIAL 8388608

    NEXT 1048576

    MINEXTENTS 1

    MAXEXTENTS UNLIMITED

    BUFFER_POOL DEFAULT

  )

  NOCOMPRESS NO INMEMORY

, PARTITION PARTITION_BEFORE_2019 VALUES LESS THAN (TO_DATE(' 2019-01', 'SYYYY-MM', 'NLS_CALENDAR=GREGORIAN'))

  NOLOGGING

  TABLESPACE USERS02

  PCTFREE 10

   INITRANS 1

  STORAGE

  (

    BUFFER_POOL DEFAULT

  )

  NOCOMPRESS NO INMEMORY

, PARTITION PARTITION_AFTER_2019 VALUES MORE THAN (TO_DATE(' 2019-01', 'SYYYY-MM', 'NLS_CALENDAR=GREGORIAN'))

  NOLOGGING

  TABLESPACE USERS03

  PCTFREE 10

  INITRANS 1

  STORAGE

  (

    BUFFER_POOL DEFAULT

  )

  NOCOMPRESS NO INMEMORY

);


4.5创建触发器、插入数据

在职工信息系统中的薪资表的F_SALARY和ATTENDANCE_PRICE是自动计算的字段，计算公式为：实际工资（F_SALARY）=月薪（SALARY）+考勤金额（ATTENDANCE_PRICE），考勤金额（ATTENDANCE_PRICE）=奖金（BONUS）+加班金额（OVER_PRICE）*加班天数（OVER_NUM）-缺勤金额（TAKEOFF_PRICE）*请假天数（TAKEOFF_NUM）。当修改薪资表的月薪或者薪资详单表的各类金额都会影响最后的实际薪资，所以设计触发器来实现自动计算。

CREATE OR REPLACE EDITIONABLE TRIGGER "WAGES_TRIG_ROW_LEVEL"

BEFORE INSERT OR UPDATE OF DISCOUNT ON "WAGES"

FOR EACH ROW 

declare

  m number(8,2);

BEGIN

  if inserting then

       :new.TRADE_RECEIVABLE := - :new.salary;

  else

      select sum(BONUS+OVER_NUM*OVER_PRICE-TAKEOFF_PRICE*TAKEOFF_NUM) into m 
      from WAGE_DETAILS where STAFF_ID=:old.STAFF_ID and DATE=:old.DATE;

      if m is null then

        m:=0;

      end if;

      :new.TRADE_RECEIVABLE := m + :new.salary;

  end if;

END;

/

现在需要给创建好的表插入数据，先是需要给部门表、职工表以及考勤项表进行数据插入，部门表与考勤项表的数据是较为固定的，插入的数据较少，职工表就要大一些。

INSERT INTO DEPARTMENTS(DEPARTMENT_ID,DEPARTMENT_NAME) values (1,'总经办');

INSERT INTO STAFFS(STAFF_ID,STAFF_NAME,SEX,PHONE_NUMBER,DATE,SALARY,MANAGER_ID,DEPARTMENT_ID)VALUES (1,'王老板',NULL,NULL,to_date('2016-01','yyyy-mm'),50000,NULL,1);



INSERT INTO DEPARTMENTS(DEPARTMENT_ID,DEPARTMENT_NAME) values (2,'销售部');

INSERT INTO STAFFS(STAFF_ID,STAFF_NAME,SEX,PHONE_NUMBER,DATE,SALARY,MANAGER_ID,DEPARTMENT_ID)VALUES (11,'张三',NULL,NULL,to_date('2016-01','yyyy-mm'),7000,NULL,1);

INSERT INTO DEPARTMENTS(DEPARTMENT_ID,DEPARTMENT_NAME) values (2,'销售部');

INSERT INTO STAFFS(STAFF_ID,STAFF_NAME,SEX,PHONE_NUMBER,DATE,SALARY,MANAGER_ID,DEPARTMENT_ID)VALUES (12,'李四',NULL,NULL,to_date('2016-01','yyyy-mm'),5000,NULL,1);

INSERT INTO DEPARTMENTS(DEPARTMENT_ID,DEPARTMENT_NAME) values (2,'销售部');

INSERT INTO STAFFS(STAFF_ID,STAFF_NAME,SEX,PHONE_NUMBER,DATE,SALARY,MANAGER_ID,DEPARTMENT_ID)VALUES (13,'王五',NULL,NULL,to_date('2016-01','yyyy-mm'),4500,NULL,1);
INSERT INTO DEPARTMENTS(DEPARTMENT_ID,DEPARTMENT_NAME) values (2,'销售部');


INSERT INTO DEPARTMENTS(DEPARTMENT_ID,DEPARTMENT_NAME) values (3,'开发部');

INSERT INTO STAFFS(STAFF_ID,STAFF_NAME,SEX,PHONE_NUMBER,DATE,SALARY,MANAGER_ID,DEPARTMENT_ID)VALUES (14,'赵六',NULL,NULL,to_date('2016-01','yyyy-mm'),5500,NULL,1);

INSERT INTO DEPARTMENTS(DEPARTMENT_ID,DEPARTMENT_NAME) values (3,'开发部');

INSERT INTO STAFFS(STAFF_ID,STAFF_NAME,SEX,PHONE_NUMBER,DATE,SALARY,MANAGER_ID,DEPARTMENT_ID)VALUES (15,'琴七',NULL,NULL,to_date('2016-01','yyyy-mm'),5000,NULL,1);

INSERT INTO DEPARTMENTS(DEPARTMENT_ID,DEPARTMENT_NAME) values (3,'开发部');

INSERT INTO STAFFS(STAFF_ID,STAFF_NAME,SEX,PHONE_NUMBER,DATE,SALARY,MANAGER_ID,DEPARTMENT_ID)VALUES (16,'何九',NULL,NULL,to_date('2016-01','yyyy-mm'),8000,NULL,1);

INSERT INTO DEPARTMENTS(DEPARTMENT_ID,DEPARTMENT_NAME) values (3,'开发部');


INSERT INTO ATTENDANCES (ATTENDANCES_ID,ATTENDANCES_NAME) VALUES ('1','出勤');

INSERT INTO ATTENDANCES (ATTENDANCES_ID,ATTENDANCES_NAME) VALUES ('2','出差');

INSERT INTO ATTENDANCES (ATTENDANCES_ID,ATTENDANCES_NAME) VALUES ('3','请假');

INSERT INTO ATTENDANCES (ATTENDANCES_ID,ATTENDANCES_NAME) VALUES ('4','加班');

如果在WAGES表中插入1万条数据，那么WAGE_DETAILS表将会存在4万条数据，循环进行数据的插入。

declare

  dt date;

  m number(8,2);

  V_EMPLOYEE_ID NUMBER(6);

  v_staff_id number(10);

  v_staff_name varchar2(100);

  v number(10,2);

begin

  for i in 1..10000

  loop

    if i mod 2 =0 then

      dt:=to_date('2015-3','yyyy-mm')+(i mod 60);

    else

      dt:=to_date('2016-3','yyyy-mm')+(i mod 60);

    end if;

    V_EMPLOYEE_ID:=CASE I MOD 6 WHEN 0 THEN 11 WHEN 1 THEN 111 WHEN 2 THEN 112

                                WHEN 3 THEN 12 WHEN 4 THEN 121 ELSE 122 END;

    v_staff_id:=SEQ_STAFF_ID.nextval; 

    v_staff_name := 'a'|| 'a';

    v_staff_name := 'he' || i;

    insert /*+append*/ into WAGES (STAFF_ID,STAFF_NAME,DATE,EMPLOYEE_ID,BONUS)

    values (v_staff_id,v_staff_name,V_EMPLOYEE_ID,dbms_random.value(100,0));

    v:=dbms_random.value(10000,4000);

    v_name:='computer'|| (i mod 3 + 1);

    insert /*+append*/ into WAGE_DETAILS(ID,STAFF_ID,STAFF_NAME,OVER_NUM,OVER_PRICE,TAKEOFF_NUM,TAKEOFF_PRICE)

    values (SEQ_WAGE_DETAILS_ID.NEXTVAL,v_staff_id,v_staff_name,2,v,3,v);

    v:=dbms_random.value(1000,50);

    select sum(BONUS+OVER_NUM*OVER_PRICE-TAKEOFF_PRICE*TAKEOFF_NUM) into m 
    
    from WAGE_DETAILS where STAFF_ID=v_staff_id and DATE=dt;

    if m is null then

     m:=0;

    end if;

    UPDATE WAGES SET TRADE_RECEIVABLE = m + salary  WHERE STAFF_ID=v_staff_id;

    IF I MOD 1000 =0 THEN

    commit; 

    END IF;

  end loop;

end;

/

4.6创建包、函数

职工信息系统还设计的有函数，放在程序包MyPack中。函数get_salary()函数的作用是计算某员工在公司工作所得所有金额。

create or replace PACKAGE MyPack IS

  FUNCTION get_salary(STAFF_ID NUMBER) RETURN NUMBER;

END MyPack;

/

create or replace PACKAGE BODY MyPack IS

  FUNCTION Bonus(STAFF_ID NUMBER) RETURN NUMBER

  AS

    N NUMBER(20,2); 

    BEGIN

      SELECT SUM(F_SALARY) into N  FROM WAGES O

      WHERE O.STAFF_ID=STAFF_ID AND O.DATE =DATE;

      RETURN N;

EXCEPTION

  WHEN NO_DATA_FOUND THEN

DBMS_OUTPUT.PUT_LINE(‘你需要的数据不存在’);

  WHEN OTHERS THEN

    DBMS_OUTPUT.PUT_LINE(SQLCODE||’---’SQLERRM);

    END;

END MyPack;

/

5.安全型设计

5.1数据库的备份与恢复

备份与恢复是数据库设计不可缺失的一环，如果数据库崩溃却无法进行恢复结果可能是毁灭性的。在任何情况下，无论数据库服务器有多稳定，可靠，我们都需要制定备份与恢复方案来备份重要数据。

1.进行用户管理备份(表空间USERS为例)

$ sqlplus system/123@pdborcl

SQL>SELECT file_name FROM dba_data_files where tablespace_name=’USERS’;

SQL>ALTER tablespace users begin backup;

SQL>!cp /home/oracle/app/oracle/oradata/orcl/pdborcl/SAMPLE_SCHEMA_users01.dbf 

/home/oracle/SAMPLE_SCHEMA_users01.dbf

SQL>!cp /home/oracle/app/oracle/oradata/orcl/control01.ctl

/home/oracle/control01.ctl


2.恢复数据库

SQL>!cp /home/oracle/SAMPLE_SCHEMA_users01.dbf

/home/oracle/app/oracle/oradata/orcl/pdborcl/SAMPLE_SCHEMA_users01.dbf 

SQL>shutdown immediate

SQL>startup mount

SQL>recover database;

