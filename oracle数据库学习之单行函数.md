# 单行函数

* lower() 大小转换为小写
* upper()  全部转换为大小
* initcap() 首字母转换为大写



```sql
SELECT lower('MAGE') as mage,upper('mage 私房菜') AS mage, initcap('mamh')
FROM EMPLOYEES;
```

* caoncat()                  连接2个字符串
* subtree('helloworld', 1, 5) 截取字符串
* length()
* instr() 判断某个字符在一个字符串中首次出现的位置
* lpad()
* rpad()
* trim()
* replace()


```sql
SELECT concat('hello ', 'world'), substr('helloworld',2,4),LENGTH('helloworld')
FROM EMPLOYEES;
```

```sql
SELECT EMPLOYEE_ID, SALARY, lpad(SALARY, 10, ' '),rpad(SALARY, 10, '-')
FROM EMPLOYEES;
```

```sql
SELECT trim('H' FROM 'HellowWorldHelo'), replace('h', 'helloworld')
FROM dual;
```

```sql

SELECT replace('abcdab', 'b', 'm') 
FROM dual
```

```sql
SELECT round(534.125, 2),round(123.45),round(123.45,-2)
FROM dual
```

```sql
SELECT trunc(534.125, 2),trunc(123.45),trunc(123.45,-2)
FROM dual
```

```sql

SELECT sysdate, sysdate + 1, sysdate - 3
FROM dual;

```


```sql
SELECT EMPLOYEE_ID, LAST_NAME, trunc((sysdate - hire_date)/30), months_between(sysdate, HIRE_DATE)
FROM EMPLOYEES;
```

```sql
select add_months(sysdate,2), add_months(sysdate, -3)
from dual;
```


```sql

select LAST_NAME,HIRE_DATE
from EMPLOYEES
WHERE HIRE_DATE=last_day(HIRE_DATE) -1;
;

```


转换函数
* to_char()
* to_number()
* to_date()

date  <==> varchar2 <==> number

```sql
SELECT to_char(1234567.89, '999,999,999.99') FROM dual;
--其中9是不补齐的


SELECT to_char(1234567.89, '00,999,999.99') FROM dual;
--前面是0会补齐的


SELECT to_char(1234567.89, 'L00,000,999.99') FROM dual;
--L是本地货币符号

```


通用函数

* nvl()将空值转换为已知的值
* nvl2(expr1, expr2, expr3)
* nullif()
* coalesce()

```sql
SELECT EMPLOYEE_ID, LAST_NAME,SALARY*12*(1 + nvl(EMPLOYEES.COMMISSION_PCT, 0 )) AS salary FROM EMPLOYEES

SELECT LAST_NAME,nvl(to_char(DEPARTMENT_ID), 'no department' ) as department FROM EMPLOYEES;

```

```sql
SELECT
  LAST_NAME,
  nvl2(EMPLOYEES.COMMISSION_PCT, COMMISSION_PCT + 0.015, 0.1)
FROM EMPLOYEES;

```

条件表达式

* case表达式
* decode 函数

```sql
SELECT
  EMPLOYEE_ID,
  LAST_NAME,
  DEPARTMENT_ID,
  CASE DEPARTMENT_ID
  WHEN 10
    THEN SALARY * 1.1
  WHEN 20
    THEN SALARY * 1.2
  WHEN 30
    THEN SALARY * 1.3
  ELSE SALARY * 1.0
  END
    AS salary
FROM EMPLOYEES
WHERE DEPARTMENT_ID IN (10, 20, 30);


```

```sql
SELECT
  LAST_NAME,
  DEPARTMENT_ID,
  decode(DEPARTMENT_ID,
         10, SALARY * 10,
         20, SALARY * 20,
         30, SALARY * 30,
         SALARY
  ) AS salary
FROM EMPLOYEES
WHERE DEPARTMENT_ID IN (10, 20, 30);

```

















