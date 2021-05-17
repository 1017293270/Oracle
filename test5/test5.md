1.创建一个包，包名是MyPack。

create or replace PACKAGE MyPack IS

  FUNCTION Get_SaleAmount(V_DEPARTMENT_ID NUMBER) RETURN NUMBER;

  PROCEDURE Get_Employees(V_EMPLOYEE_ID NUMBER);

END MyPack;

/

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test51.png)

2.在MyPack中创建一个函数SaleAmount ，查询部门表，统计每个部门的销售总金额，每个部门的销售额是由该部门的员工(ORDERS.EMPLOYEE_ID)完成的销售额之和。函数SaleAmount要求输入的参数是部门号，输出部门的销售金额。

FUNCTION Get_SaleAmount(V_DEPARTMENT_ID NUMBER) RETURN NUMBER

AS

N NUMBER(20,2);

BEGIN

    SELECT SUM(O.TRADE_RECEIVABLE) into N  FROM ORDERS O,EMPLOYEES E

    WHERE O.EMPLOYEE_ID=E.EMPLOYEE_ID AND E.DEPARTMENT_ID =V_DEPARTMENT_ID;

    RETURN N;

END;


![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test52.png)

查询所有部门的总金额：

select MyPack.Get_SaleAmount(11) AS 部门11应收金额,MyPack.Get_SaleAmount(12) AS 部门12应收金额 from dual;

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test53.png)

3.在MyPack中创建一个过程，在过程中使用游标，递归查询某个员工及其所有下属，子下属员工。

PROCEDURE GET_EMPLOYEES(V_EMPLOYEE_ID NUMBER)

AS

LEFTSPACE VARCHAR(2000);

begin

    LEFTSPACE:=' ';

    for v in

    (SELECT LEVEL,EMPLOYEE_ID,NAME,MANAGER_ID FROM employees
    START WITH EMPLOYEE_ID = V_EMPLOYEE_ID
    CONNECT BY PRIOR EMPLOYEE_ID = MANAGER_ID)

    LOOP

    DBMS_OUTPUT.PUT_LINE(LPAD(LEFTSPACE,(V.LEVEL-1)*4,' ')||
                         V.EMPLOYEE_ID||' '||v.NAME);

    END LOOP;

END;

查询员工ID为1和11及其下属：

set serveroutput on

DECLARE

  V_EMPLOYEE_ID NUMBER;    

BEGIN

  V_EMPLOYEE_ID := 1;

  MYPACK.Get_Employees (  V_EMPLOYEE_ID => V_EMPLOYEE_ID) ;  

  V_EMPLOYEE_ID := 11;

  MYPACK.Get_Employees (  V_EMPLOYEE_ID => V_EMPLOYEE_ID) ;    

END;

/

![Image text](https://github.com/ylxc9527/oracle/raw/master/img/test54.png)

4.由于订单只是按日期分区的，上述统计是全表搜索，因此统计速度会比较慢，如何提高统计的速度呢？

日期分区不会影响总金额的计算，可以进行部门分区缩减搜索范围从而提高统计速度。


