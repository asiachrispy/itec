---
layout: post
title: "postgresql mysql 命令 差异"
tag: "tec"
comment: true
published: true
date: 2012-09-04
---


这个文档给大家提供mysql & psql 的参考，这里列出了部分差异，

 	
### 一.关于登入和退出数据库


#### 1.服务的关闭，启动
##### 1.1 mysql 
	#sudo /etc/init.d/mysql start/restart/stop

#### 1.2.psql
	#sudo /etc/init.d/postgresql-9.0 start/restart/stop

#### 2.登入
##### 2.1.mysql
	#mysql -uroot -proot

##### 2.2.psql
以用户 postgres的身份 登入到 postgres 数据库（这个账户是安装psql自动创建的）

	#sudo -u postgres psql postgres

#### 3.退出
##### 3.1 mysql 
	#exit;
##### 3.2 psql 
	postgres=#\q

#### 4.psql客户端应用

这部分包含 PostgreSQL 客户端应用和工具的参考信息。这里的命令并非全部是通用工具， 有些可能需要特殊的权限。
这些应用的一般性特点是它们可以在任何主机上运行，与数据库服务器所处的位置无关。

1. clusterdb -- 对一个PostgreSQL数据库进行建簇
2. createdb -- 创建一个新的 PostgreSQL 数据库
3. createlang -- 定义一种新的 PostgreSQL 过程语言
4. createuser -- 定义一个新的 PostgreSQL 用户帐户
5. dropdb -- 删除一个现有 PostgreSQL 数据库
6. droplang -- 删除一种 PostgreSQL 过程语言
7. dropuser -- 删除一个 PostgreSQL 用户帐户
8. ecpg -- 嵌入的 SQL C 预处理器
9. pg_config -- 检索已安装版本的 PostgreSQL 的信息
10. pg_dump --  将一个PostgreSQL数据库抽出到一个脚本文件或者其它归档文件中
11. pg_dumpall -- 抽出一个 PostgreSQL 数据库集群到脚本文件中
12. pg_restore --  从一个由 pg_dump 创建的备份文件中恢复 PostgreSQL 数据库。
13. psql --  PostgreSQL 交互终端
14. vacuumdb -- 收集垃圾并且分析一个PostgreSQL 数据库		

### 二.关于创建数据库
**注意:**

* psql是大小写不敏感的，如果没有使用""，psql都会将它统一转为小写来保存！
psql使用 "" 来保证数据库名称的大写！表名，字段名称及其它约束名称也一样！

* MYSQL 是大小写敏感的！

#### 1.mysql 
	mysql>create database `SSDB`; 
	等同于 
	mysql>create database SSDB; 

#### 2.psql	
	postgres=#create database "SSDB";

#### 以下3条语句是相等的，都创建了一个数据库ssdb
	postgres=#create database SSDB;
	postgres=#create database ssdb;
	postgres=#create database `ssdb`;

### 三.关于数据库切换
#### 1.mysql 
	mysq>use SSDB；

#### 2.psql 
	postgres=#\c "SSDB";

### 四.关于序列发生器
#### 1.mysql ：默认从1开始，这里的语句指定自增初始值为1

	create table if not exists `SS_T_APPLICATION` (
		`ID`	int auto_increment not null,
		`SN`   	varchar(20)  not null,
		primary key (`ID`)
	)engine = InnoDB AUTO_INCREMENT=1 character set utf8;

#### 2.psql 
##### 2.1我们创建一个特定的序列名称
	CREATE SEQUENCE "APPLICATION_ID_SEQ" start 1;
	--默认从1开始,这里可以指定起始值
	
	create table  "SS_T_APPLICATION" (
		"ID"  int DEFAULT nextval('"APPLICATION_ID_SEQ"') NOT NULL,
		"SN"  varchar(20)  not null,
		primary key ("ID")
	);
--------------------
#### 2.2 使用默认的从1开始自增的序列，它会自动创建一个序列名称为“APPLICATION_id_seq”

	create table  "APPLICATION" (
		"ID"	serial not null,
		"SN" 	varchar(20)  not null,
		primary key ("ID")
	);

### 五.查看当前的最大序列号：
#### 1.mysql 
	mysql> select last_insert_id();

#### 2.psql
	postgres=# select currval('APPLICATION_ID_SEQ');

### 六. 设置自增ID的开始值。
#### 1.mysql
	mysql> alter table SSDB auto_increment = 3;

#### 2.psql 
	SSDB=# select setval('APPLICATION_ID_SEQ',1,false);

### 七.关于事务
####  1.mysql：以下是指定用法
	mysql>start transaction;
	mysql>commit;	
	mysql>rollback;

#### 2.psql :如果没有开启一个事务的话，使用commit和rollback会给出警告
	SSDB=#start transaction;
	SSDB=#commit transaction;
	SSDB=#commit;
	SSDB=#rollback transaction;
	SSDB=#rollback;

### 八. 关于修改表的操作
psql只能使用 ALTER 来修改一个字段的相关操作，而没有 modify；
#### 1.mysql :modify 之后可以加上 cloumn
	mysql>ALTER TABLE `SS_T_APPLICATION` MODIFY 		`CREATE_DATE` TIMESTAMP not null  DEFAULT 		CURRENT_TIMESTAMP;

#### 2.psql： ALTER  之后可以加上 cloumn 
	SSDB=#ALTER TABLE "SS_T_APPLICATION"  ALTER   		"CREATE_DATE" set  DEFAULT CURRENT_TIMESTAMP;

### 九. 简单使用命令区别
mysql 使用 show [] 来查询相关资源状态； psql使用 \d [] 来查询相关资源状态；
#### 1.mysql
	a. show tables或show tables from database_name; // 显示当前数据库中所有表的名称
	b. show databases; // 显示mysql中所有数据库的名称
	c. show columns from table_name from database_name; 或show columns from database_name.table_name;   // 显示表中列名称
	d. show grants for user_name@localhost;   //   显示一个用户的权限，显示结果类似于grant 命令
	e. show index from table_name;   // 显示表的索引
	f. show status;   // 显示一些系统特定资源的信息，例如，正在运行的线程数量
	g. show variables; // 显示系统变量的名称和值
	h. show   processlist; // 显示系统中正在运行的所有进程，也就是当前正在执行的查询。
	大多数用户可以查看他们自己的进程，但是如果他们拥有process权限，就可以查看所有人的进程，包括密码。
	i. show table status; // 显示当前使用或者指定的database中的每个表的信息。信息包括表类型和表的最新更新时间
	j. show privileges;   // 显示服务器所支持的不同权限
	k. show create database database_name; // 显示create database 语句是否能够创建指定的数据库
	l. show create table table_name; // 显示create database 语句是否能够创建指定的数据库
	m. show engies;   // 显示安装以后可用的存储引擎和默认引擎。
	n. show innodb status; // 显示innoDB存储引擎的状态
	o. show logs; // 显示BDB存储引擎的日志
	p. show warnings; // 显示最后一个执行的语句所产生的错误、警告和通知
	q. show errors; // 只显示最后一个执行语句所产生的错误 

#### 2.psql
	\d [名字]        描述表, 索引, 序列, 或者视图
	\d{t|i|s|v|S} [模式] (加 "+" 获取更多信息)
	列出表/索引/序列/视图/系统表
	\da [模式]       列出聚集函数
	\db [模式]       列出表空间 (加 "+" 获取更多的信息)
	\dc [模式]       列出编码转换
	\dC              列出类型转换
	\dd [模式]       显示目标的注释
	\dD [模式]       列出域
	\df [模式]       列出函数 (加 "+" 获取更多的信息)
	\dg [模式]       列出组
	\dn [模式]       列出模式 (加 "+" 获取更多的信息)
	\do [名字]       列出操作符
	\dl              列出大对象, 和 \lo_list 一样
	\dp [模式]       列出表, 视图, 序列的访问权限
	\dT [模式]       列出数据类型 (加 "+" 获取更多的信息)
	\du [模式]       列出用户
	\l               列出所有数据库 (加 "+" 获取更多的信息)
	\z [模式]        列出表, 视图, 序列的访问权限 (和 \dp 一样)

### 十. 数据库备份
		
#### 1.mysql

##### 备份: 备份数据库 mydb到文件mydb.sql

	mysql> mysqldump -u root -p  mydb > mydb.sql	mysqldump  备份命令
	root       用户名(root管理员)
	mydb   备份的数据库名;
	>           备份符号
	mydb.sql    备份的文件名
	
##### 还原: 	还原数据库文件 mydb.sql到数据库test中
	mysql> mysql -u root -p test < mydb.sql
	root     用户名(root管理员)
	test   	数据库名;
	<           还原符号
	mydb.sql    还原的文件名

#### 2.psql
##### 备份:
	#pg_dump > postgres.out 
	--备份与用户名同名的数据库到postgres.out文件
	#pg_dump mydb > mydb.out 
	---备份数据库mydb到mydb.out文件
	#pg_dump -Ft mydb > mydb.tar 
	--备份数据库mydb的数据库到一个mydb.tar文件

##### 还原:
	#psql -e test < mydb.out 
	--导入备份数据文件mydb.out到新的数据库test中
	#pg_restore -d newdb mydb.tar 
	--导入备份数据mydb.tar的数据到数据库newdb中


### 十一.配置文件

	1.mysql的配置文件是/etc/my.cnf

	2.psql的配置文件包含多个文件，这里列出如下2个
		--主要用于设置影响数据库群集的各种参数
		
		$PSQL_HOME/9.0/data/postgresql.conf  
		$PSQL_HOME/9.0/data/pg_hba.conf				
		--客户端认证控制文件




