单行函数


```sql
SELECT lower('MAGE') as mage,upper('mage 私房菜') AS mage, initcap('mamh')
FROM EMPLOYEES;
```

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



















