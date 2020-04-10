# mysql-final-test

2019-2020 mysql final test

姓名：刘小娟

学号：17202207

说明1：考试为开卷，可以上网，自觉不要相互电话和QQ；

说明2：考试时间为：2020 年 4 月 10 日 8:10分-9:40分。

说明3：试题比较多，抓紧时间，超时提交的github commit不被接受。未避免完全超时提交，可以在做题过程中多 commit 几次。

说明4：结束后发邮件给 42225@hdu.edu.cn 标题为“MySQL期末考试,姓名+学号”，内容为github链接。


1 打印当前时间（例如 2020-04-07 13:41:42），写出SQL语句和结果
```sql
     select current_timestamp,
     current_timestamp();
     +---------------------+--------------------+
     |  current_timestamp  | current_timestamp  |
     +---------------------+--------------------+
     |2020-04-10 08：10：30|2020-04-10 08：10：30|
     +---------------------+--------------------+
```        


2 组合打印自己的姓名和学号
```sql
    select concat("刘小娟+",17202207);
    +--------------------------+
    |concat("刘小娟+",17202207) |
    +--------------------------+
    |刘小娟+17202207            |
    +--------------------------+
```

(例如 张三+123456 或者 zhangsan+123456 显示需包含加号)，写出SQL语句和结果

3 建立如下表1和表2，写出建表语句和插入语句。

表1：其中deptno为主键
```sql
use student_2;
create table ts4(
          deptno int(11),
          dname varchar(45),  
           loc  varchar(45),
          primary key(deptno)
     );
    
    insert into ts4(deptno,dname,loc)
     values(10, "ACCOUNTING", "NEW YORK"),
           (20, "RESEARCH", "DALLAS"),
           (30, "SALES", "CHICAGO"),
           (40, "OPERATIONS", "BOSTON");
   select * from ts4;
```



表2：其中empno字段为主键
```sql
 create table ts5(
      empno int(11),
      ename varchar(45),
      job   varchar(45),
      mgr   varchar(45),
      hiredate Date,
      sal float,
      comm  int(11),
      dempno int(11),
      primary key(empno)
      );
      
   insert into ts5 (empno, ename,job,mgr,hiredate,sal,comm, dempno)
        values(7369, "SMITH", "CLERK", 7902, "1981-03-12", 800.00, NULL, 20),
	(7499, "ALLEN", "SALESMAN", 7698, "1982-03-12", 1600, 300, 30),
	(7521, "WARD", "SALESMAN", 7698, "1838-03-12", 1250, 500, 30),
	(7566, "JONES", "MANAGER", 7839, "1981-03-12", 2975, NULL, 20),
	(7654, "MARTIN", "SALESMAN", 7698, "1981-01-12", 1250, 1400, 30),
	(7698, "BLAKE", "MANAGER", 7839, "1985-03-12", 2450, NULL, 10),
	(7788, "SCOTT", "ANALYST", 7566, "1981-03-12", 3000, NULL, 20),
	(7839, "KING", "PRESIDENT", NULL, "1981-03-12", 5000, NULL, 10),
	(7844, "TURNER", "SALESMAN", 7689, "1981-03-12", 1500, 0, 30),
	(7878, "ADAMS", "CLERK", 7788, "1981-03-12", 1100, NULL,20),
	(7900, "JAMES", "CLERK", 7698,"1981-03-12",  950, NULL, 30),
	(7902, "FORD", "ANALYST", 7566, "1981-03-12", 3000, NULL, 20),
	(7934, "MILLER", "CLERK", 7782, "1981-03-12", 1300, NULL, 10);
select * from ts5;
```
![]
3.1 表2 中再插入一条记录：
```sql
 insert into ts5(empno, ename,job,mgr,hiredate,sal,comm,dempno)
          values(17202207,'liuxiaojuan,'student', 7782, "1999-07-16", NULL, NULL, 10);
select * from ts5;
```


3.2 表中入职时间（Hiredate字段）最短的人。
```sql
   select ename,min(hiredate) from ts5;
```



3.3 有几种职位（job字段）？在关系代数中，本操作是什么运算？

3.4 将 MILLER 的 comm 增加 100； 然后，找到 comm 比 MILLER 低的人；
```sql
   update ts5
   set comm=100 where ename='miller';
   
   select * from ts5 where comm< (select comm from ts5 where ename='miller');
```





3.5 计算每个人的收入(ename, sal + comm)；计算总共有多少人；计算所有人的平均收入。 提示：计算时 NULL 要当做 0 处理； 

3.6 显示每个人的下属, 没有下属的显示 NULL。本操作使用关系代数中哪几种运算？
```sql
   select *
   from ((ts5 t1 inner join ts5 t2 on t1.mgr=t2.empno) inner join ts5 t3 on t2.mgr=t3.empno) inner join ts5 t4 
   on t3.mgr=t4.empno;
```
使用了内连接



3.7 建立一个视图：每个人的empno, ename, job。简述为什么要建立本视图。
```sql
    create view v5
    as
    select empno,ename,job
    from ts5;
    
    select * from v5;
 ```
 
 
 
 **原因**：为了透过本视图看到自己感兴趣的数据及其变化
 
 


3.8 为表2增加一个约束：deptno字段需要在表1中存在；这称做什么完整性？
```sql
   alter ts5
   constraint ts5_fk2 foreign key(deptno) references ts4(depno);
```
这是参照完整性


3.9 为表2增加一个索引：ename 字段。简述为什么要在 ename 字段建立索引
```sql
    create index indexname on ts5(ename)
```
因为名字具有唯一性


3.10 将表2的 sal 字段改名为 salary
```sql
  alter table ts5
  change sal salary varchar(45);
```

3.11 撰写一个函数 get_deptno_from_empno，输入 empno，输出对应的 deptno。 简述函数和存储过程有什么不同。
```sql
    delimiter $$
    create function get_deptno_from_empno(empno int)
    returns int
    begin
    return(select deptno from ts5 where ts5.empno=empno);
    end$$
    
    select get_deptno_from_empno(7499);
```


4 建立一个新用户，账号为自己的姓名拼音，密码为自己的学号；
```sql
    create user 'liuxiaojuan'@'%' identified by'17202207';
4.1 将表1的SELECT, INSERT, UPDATE(ename)权限赋给该账号。
```sql 
    grant select,insert,update(ename) to test'liuxiaojuan'@'%' identified by'17202207';
```
4.2 显示该账号权限
```sql
show grants for 'liuxiaojuan'@'%';
```
4.3 `with grant option` 是什么意思。


**答**：将权限再授予他人

5 表 1 和表 2 这样设计是否符合第一范式，是否符合第二范式，为什么？

6 画出表 1 和表 2 所对应的 E-R 图

7 什么是外模式，什么是内模式。为什么要分成这几层？


**答**：外模式是用户与数据库系统的接口，是用户用到的那部分数据的描述
        内模式是数据库在物理储存方面的描述，定义所有内部记录类型，索引和文件的组织方式以及数据控制方面的细节。
	
8 什么是ACID？


**答**：ACID原则是数据库事务正常执行的四个，分别指原子性、一致性、独立性及持久性

8.1 编写一个事务，“将 MILLER 的 comm 增加 100，如果增加后的 comm 大于 1000 则回滚”；

8.2 如何查看 MySQL 当前的隔离级别？

8.3 如果隔离级别为 READ-UNCOMMITED, 完成 “MILLER 的 comm 增加 100” 事务操作完成后，可能读到的结果有哪些，原因是什么？

9 有哪些场景不适合用关系型数据库？为什么？


**答**：
1）海量数据存储；

2）多格式的数据存储；

3）对查询速度要求快的数据存储；


因为关系型数据库适用于
1）需要做复杂处理的数据；

2）数据量不是特别大的数据；

3）对安全性要求高的数据；

4）数据格式单一的数据；

