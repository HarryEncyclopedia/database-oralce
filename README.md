# database-oracle
新生入学登录 

sys 是oraccle数据库中的超级用户 拥有最高权限 拥有sysdba角色 有创建数据库的权限.如果在安装过程中没有设置口令 ，那么默认密码是change_no_install;
system 是用户操作员 权限巨大次于sys ,拥有sysoper角色 但是没有创建数据库的权限 默认密码是：manager;

两者最大区别 可以说是 有无创建数据库的权限的区别；
scott普通用戶 默認密碼：tiger
激活scott 
切換system賬號
alter user scott identified by new password;
在 sqlplus中
连接：
 conn 用户名/密码

disc【onnect】 此命令用来断开当前数据库连接;

show user 显示你当前用户名：
exit 退出;

创建用户：
create user 用户名[userName] identified by[密码][password] //刚创建用户时,用户没有任何权限；

修改密码：
alter user 用户名[userName] identified by 新密码【new password】
删除用户 
drop user 用户名 cascade:1,注意需要拥有dba权限 才可以删除。2,如果该用户已经创建了表，需先删除表再来删除用户，否则ERROR；

授予权限;
grant crate session to [userName] identified by password; //授予新用户登录的 权限；

grant 给予什么权限【dba,create session,connect ....】 to userName identified by 密码；这是授予账号 什么权限；
权限控制：
grannt select[update,insert,delete] on tableName to  userName ；授予用户可以查询普通用户的tableName的权限；

start 路径；导入
账户锁定：
alter user [userName] account lock;
