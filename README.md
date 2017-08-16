# database-oracle
新生入学登录 

SQLPLUS
cmd
sqlplus [用户名]/[密码][@数据库] [参数]
sqlplus sys/orcl as sysdba  -- 登录 sys 用户，必须指定 sysdba 或 sysoper 身份
sqlplus system/orcl         -- 数据库管理员
    
创建一个自己的用户(比如 vip/vip)
create user vip identified by vip;     -- 注意，新创建的用户，什么权限都没有，需要授权后才能使用
grant create session to vip;           -- 授予登录的权限
grant connect to vip;                  -- 角色是很多权限的打包，connect 是一种角色，它包含了连接查看数据的一些基本权限
grant dba to vip;                      -- dba 是绝大多数权限的集合，它基本能做所有事情，所以很少单独授予用户。但在测试环境中，这样，很爽。

-- 上面的创建用户、授予权限两步操作，可以简化为下面一步：
grant dba to vip identified by vip;    -- 注意，使用分号结尾
    
切换到用户
sqlplus vip/vip  -- 在 cmd 下
conn vip/vip     -- 在 sqlplus 中
    
使用
create table aaa (id int);
    
激活内置的测试账号，这里面有几张示例表，可以用它们练习下查询
alter user scott account unlock;
conn scott/tiger
    
修改密码
alter user scott identified by [newpassword];
    
甜点

示例

[题目] 从 scott 用户的 emp/dept 表中，找到“来自芝加哥最有钱的那个人”。

首先，我们需要理清思路。

这里总共有两个条件：

这个人是来自芝加哥的
这个人是最有钱的，是芝加哥最有钱的。
我们可以看出，第二个条件是基于第一个条件的。

所以，分两步查询：

找出所有来自芝加哥的人
从这些人中，找到最有钱的那个。这一步，可以通过 max 函数和 order by 两种方式实现。
下面是语句示例：

---- 第一步：找到来自芝加哥的所有人。这么两种写法等价：

select e.* from emp e
  join dept d on (e.deptno=d.deptno)
  where d.loc='CHICAGO';

select e.* from emp e, dept d
  where d.deptno = e.deptno
        and d.loc='CHICAGO';


---- 第二步，基于上面结果，筛选出最有钱的那个

-- 可以通过 max 函数
select e.* from emp e, dept d
  where e.deptno = d.deptno
        and d.loc='CHICAGO'
        and sal = 
            (select max(sal) from emp e, dept d
              where e.deptno = d.deptno
                    and d.loc='CHICAGO');

-- 可以通过排序的方式
select e.ename from
  (select e.*, d.* from emp e, dept d
    where e.deptno = d.deptno
          and d.loc='CHICAGO')
where rownum = 1;
注意，实现的方式，不止上面的那些。但总体 思路 是一样的。

所以，思路永远是最重要的。

题库

在芝加哥工作的人中，谁的工资最高
查询每个部门下有多少员工
查询除去 salesman 所有平均工资超过 1500 的部门
查询在 new york 工作的所有员工的姓名，部门名称和工资信息
查询姓名为 King 的员工的编号，名称跟部门
查询各种工作的最低工资
查询工龄大于10年的所有员工信息
查询每个部门员工数量，平均工资和平均工作年限
统计各部门每个工种的人数，平均工资。
查询从事同一种工作但不属于同一部门的员工信息。
查询所有员工工资都大于1000的部门的信息及员工信息
查询入职日期早于其直接上级的所有员工信息。
列出雇员中（除去mgr为空的人)工资第二高的人。
列出1981年来公司所有员工的总收入（包括sal和comm）
