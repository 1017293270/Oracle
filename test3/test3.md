实验三

1.使用system用户给自己的账号分配分区使用权限。

ALTER USER ylxc QUOTA UNLIMITED ON USERS;

ALTER USER ylxc QUOTA UNLIMITED ON USERS02;

ALTER USER ylxc QUOTA UNLIMITED ON USERS03;

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test31.png)

2.在三个表空间中创建订单表与订单项表并插入数据。

创建表时判断是否已存在该表：

declare

 num   number;

begin

select count(1) into num from user_tables where TABLE_NAME = 'ORDER_DETAILS';

  if   num=1   then

  execute immediate 'drop table ORDER_DETAILS cascade constraints PURGE';

  end   if;

  select count(1) into num from user_tables where TABLE_NAME = 'ORDERS';

  if   num=1   then

  execute immediate 'drop table ORDERS cascade constraints PURGE';

  end   if;

end;
/

分区策略：通过时间范围作为分区标准，一年为一个分区，例如分区2015：

PARTITION PARTITION_2015 VALUES LESS THAN (TO_DATE(' 2016-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))

插入订单：

  v_order_id:=i;

  v_name := 'aa'|| 'aa';

  v_name := 'zhang' || i;

  v_tel := '139888883' || i;

  insert /*+append*/ into ORDERS (ORDER_ID,CUSTOMER_NAME,CUSTOMER_TEL,ORDER_DATE,EMPLOYEE_ID,DISCOUNT)values (v_order_id,v_name,v_tel,dt,V_EMPLOYEE_ID,dbms_random.value(100,0));

插入订单y一个订单包括3个产品：

  v:=dbms_random.value(10000,4000);

  v_name:='computer'|| (i mod 3 + 1);

  insert /*+append*/ into ORDER_DETAILS(ID,ORDER_ID,PRODUCT_NAME,PRODUCT_NUM,PRODUCT_PRICE)values (v_order_detail_id,v_order_id,v_name,2,v);

  v:=dbms_random.value(1000,50);

  v_name:='paper'|| (i mod 3 + 1);

  v_order_detail_id:=v_order_detail_id+1;

  insert /*+append*/ into ORDER_DETAILS(ID,ORDER_ID,PRODUCT_NAME,PRODUCT_NUM,PRODUCT_PRICE)values (v_order_detail_id,v_order_id,v_name,3,v);

  v:=dbms_random.value(9000,2000);

  v_name:='phone'|| (i mod 3 + 1);

  v_order_detail_id:=v_order_detail_id+1;

  insert /*+append*/ into ORDER_DETAILS(ID,ORDER_ID,PRODUCT_NAME,PRODUCT_NUM,PRODUCT_PRICE)values (v_order_detail_id,v_order_id,v_name,1,v)；
  
![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test32.png)

3.写出查询数据的语句，并分析语句的执行计划。

单分区查询

select * from ylxc.orders where order_date
between to_date('2017-1-1','yyyy-mm-dd') and to_date('2017-6-1','yyyy-mm-dd')

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test33.png)

多分区查询

select * from ylxc.orders where order_date
between to_date('2017-1-1','yyyy-mm-dd') and to_date('2018-1-1','yyyy-mm-dd')

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test34.png)

select * from ylxc.orders

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test35.png)