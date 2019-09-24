# Oracle #

* ## Oracle官方下载地址：
> <https://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html>

* ### 下载好后，我们就可以打开sql plus命令行玩转oracle，这个工具是oracle最底层，软件下载好会自动帮我们下载这个工具

=========================================
* sql plus命令:
	+ 默认sys、system这两个账号不会被锁定，其他都会被锁定
	+ 可以在浏览器上登录oracle，localhost:1158/em/console/logon/logon，sys/root123/sysdba
    + <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/1.%20%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%8A%E7%99%BB%E5%BD%95.png"  height="330" width="695">
	</div>
	
	+ Oracle的***卸载***可以在db_home文件夹下找到deinstall文件，里面有个bat的批处理卸载文件（使用管理员权限），一路点击回车即可完成删除。
	+ ---oracle删除
	```app\Administrator\product\11.2.0\dbhome_1\deinstall.bat

	指定要取消配置的所有单实例监听程序 【LISTENER】: Enter
	指定在此Oracle主目录中配置的数据库名的列表【ORCL】：Enter
	是否仍要修改ORCL数据库的详细资料？【n】：y
	```
	+ <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/2.%E5%88%A0%E9%99%A4oracle.png"  height="330" width="695">
	</div>

* 系统用户:
	+ 有四个系统用户，分别是***sys、system、sysman、scott***；
	+ sys和system这两个都是权限比较高的用户，并且sys权限高于system，其中sys用户必须以管理员sysdba权限才能登录
	+ sysman：用于操作企业管理器使用的，管理员级别权限；到这里这三个都是安装的时候就已经设置好的密码
	+ Scott：他是创始人的名字，用户权限最小的系统用户，默认的密码是tiger

* 使用系统用户登录:
	+ 使用system用户登录 ***[username/password] [@server] [as sysdba|sysoper]***  --  system可以不需要sysdba登录；
	如果你用普通用户以sysoper登录就得到dba的角色了跟sys一样的权限；
	如：`system/root123 @orcl as sysdba；` orcl就是自己设置的服务名，如果不是连别人的oracle，这里的@server就是写自己的@ip，
	如果oracle安装在本机，还可以省略@orcl，最终 `connect sys/root123 as sysdba;`
	+ <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/3.%E7%B3%BB%E7%BB%9F%E7%94%A8%E6%88%B7%E7%99%BB%E5%BD%95.png"  height="330" width="695"> 
	</div>

	+ 使用sys用户登录，和system一样，sys一定要用sysdba登录，connect换用户登录

	+ `show user`（***显示当前登录的用户***）
	+ oracle里面有很多数字字典表（其实就是一张表），比如今天的第一张字典表 - ***dba_users***
	+ ***desc***命令查看表的结构字段
	+ `desc dba_users; `查看当前用户的数据字典
	+ `select username from dba_users; `查看数据字典中的用户

	+ 要先使用一个账号登录，才可以输入语句 system/root123;
	+ ***启动用户（解锁）***：`alter user username(更改的地方) account unlock;`
	+ 对于scott这个用户默认是锁定的，现在我想把他解锁 alter user scott account unlock;
	+ connect scott/tiger 
	+ show user，发现他已经是user为‘scott’解锁状态了

	+ ***修改密码***：打开cmd，输入sqlplus/as sysdba，接着alter user system identified by 密码（随意设置），重新设置system的密码;

* ### 2-5 表空间
	* 表空间分类:
	+ ***永久表空间***：持久化保存，例如表、视图、存储过程
	+ ***临时表空间***：临时查询的表
	+ ***UNDO表空间***：存储数据修改之前的数据，用于事务回滚操作
	<br />

	+ ***查看用户的表空间：dba_tablespaces（系统管理员查看的数据字典）、user_tablespaces（普通用户）数据字典***
	+ ***查看系统的表空间***：`select tablespace_name from dba_tablespaces;`
	+ <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/4.%20%E6%9F%A5%E7%9C%8B%E7%94%A8%E6%88%B7%E8%A1%A8%E7%A9%BA%E9%97%B4.png"  height="380" width="795"> 
	</div>

	+ ***查看system的默认表空间和临时表空间***：`select default_tablespace,temporary_tablespace from dba_users where username = 'SYSTEM'; `
	+ default_tablespace默认表空间，temporary_tablespace临时表空间
	+ <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/5.%20%E6%9F%A5%E7%9C%8B%E9%BB%98%E8%AE%A4%E8%A1%A8%E7%A9%BA%E9%97%B4%E5%92%8C%E4%B8%B4%E6%97%B6%E8%A1%A8%E7%A9%BA%E9%97%B4.png"  height="380" width="795"> 
	</div>

	+ ***dba_users、user_users数据字典***
	+ 查看用户表空间其实就是查看两张表，在oracle中指的是数据字典，分别为dba_tablespaces和user_tablespaces。然后这两个表空间有dba_users（管理员）和user_users（普通用户）,其实这两张数据字典表就是查看永久表空间，存储用户创建的数据库对象。上面这四张都是数据字典
	+ ***查SYSTEM的默认表空间或者临时表空间***：`select default_tablespace(默认表空间) | temporary_tablespace（临时表空间） from dba_users  where username='SYSTEM';`
	+ 因为这两张数据字典表都是dba_users 
	+ ***设置用户的默认表空间或临时表空间名字***：`ALTER USER system(要修改的用户名) DEFAULT TABLESPACE system(默认表空间新名字);`（默认system的默认表空间是users，现在我要改system的默认表空间的名字）

	* 创建表空间:
	+ ***创建表***：`CREATE TABLESPACE my_table DATAFILE '0721.dpf' SIZE 50m;`
	+ ***临时表创建***：`create temporary tablespace my_table tempfile '0822.dpf' size 50m;`

	+ 如果不知道我表空间存在哪里，可以看一下***dba_data_files***数据字典的file_name字段：desc dba_data_files(查看结构)
	+ <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/6.%20dba_data_files%E6%95%B0%E6%8D%AE%E5%AD%97%E5%85%B8%E6%9C%89%E8%A1%A8%E7%A9%BA%E9%97%B4%E5%AD%98%E5%9C%A8%E5%93%AA%E9%87%8C.png"  height="380" width="795"> 
	</div>

	+ ***查看TEST1_TABLESPACE表空间存在哪里（永久表）***：`select file_name from dba_data_files where tablespace_name = 'TEST1_TABLESPACE';`（记得表空间名字要大写）
	+ ***查看TEMPTEST1_TABLESPACE临时表空间存在哪里（临时表）***：`select file_name from dba_temp_files where tablespace_name = 'TEMPTEST1_TABLESPACE';`

	+ 修改表空间的状态：
	+ ***设置联机或脱机状态***：`alter tablespace tablespace_name online|offline;`
	+ tablespace_name表名字，online联机（默认）、offline脱机
	+ 只有在联机状态下才可以更改只读或可读写状态read only/read write
	+ 脱机状态下，表空间无法用

	+ test1_tablespace切换为***脱机状态下***：`alter tablespace test1_tablespace offline;`
	+ desc dba_tablespaces这个数据字典有个status字段可以查看***当前表空间的状态***

	+ ***查看表空间TEST1_TABLESPACE当前状态（是否为联机）***：`select status from dba_tablespaces where tablespace_name = 'TEST1_TABLESPACE';`
	+ ***表空间test1_tablespace修改为联机状态***：`alter tablespace test1_tablespace online;`

	+ ***设置表空间的只读或可读写状态***：`alter tablespace tablespace_name read only|read write`
	+ read only只读，read write读写（默认下）
	+ PS：只有联机状态下才能设置这个状态

	+ 练习：***设置只读状态***：`alter tablespace test1_tablespace read only;`
	+ 联机状态下才可以设置只读写，要是只读就显示read only，读写就显示联机online，因为可读写就是联机
	+ <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/7.%20%E8%AE%BE%E7%BD%AE%E8%A1%A8%E7%A9%BA%E9%97%B4%E5%8F%AF%E8%AF%BB%E6%88%96%E8%80%85%E5%8F%AF%E8%AF%BB%E5%86%99.png"  height="380" width="795"> 
	</div>

	+ ***设置读写状态***：`alter tablespace test1_tablespace read write;`

	+ 为表空间添加数据文件：`alter tablespace test1_tablespace add datafile 'test2_file.dpf' size 50m;`
	+ test1_tablespace表空间，'test2_file.dpf'数据文件 50m大小
  + ***查看表空间TEST1_TABLESPACE的数据文件存的位置***，默认是有一个的：`select file_name from dba_data_files where tablespace_name = 'TEST1_TABLESPACE'; `
	
	+ ***删除表空间tablespace_name数据文件***：`alter tablespace tablespace_name drop datafile 'filename.dpf'`
	+ 不能删除表空间默认的那个数据文件，如果要删除，就把表空间给删了

	+ ***删除表空间***：`drop tablespace tablespace_name（表空间名字）`
	+ ***删除表空间及数据文件***：`drop tablespace tablespace_name（表空间名字） including contents;`

	<br />

	* ### 3 管理表
	* 表中的约定：
	+ 1. 每一列数据必须具有相同数据类型
	+ 2. 列名唯一
	+ 3. 每一行数据的唯一性

	* ***字符型***：
	+ ***char(2000)、nchar(1000) 放中文  ***
	+ ***varchar2(4000)、nvarchar2(2000)***
  + 效率还是char定长好，因为他不用像varchar2去计算长度；如：如果char(10),你只给放3个，他会自己加7个空格达到10个长度，定长；而varchar2(10)就只给他3个，他就变成varchar2(3)，注意内存就varchar2()好，效率就char()好
	
	* 数值型
	+ ***number(p,s)***	十进制（常用）
	+ number(5,2)，有效数字5位，保留2位小数，如123.45
  + float(n)   二进制数据，1-126位，转换为十进制数时要乘以0.30103
	
	* 日期型
	+ ***date*** 精确到秒（常用）
	+ timestamp 精确到小数秒

	* 其他类型
	+ blob 可存放4G[二进制]
	+ clob 可存放4G[字符串形式]
  + ```列中数值指定大小接近一致使用char
		列中数值大小显著不同使用varchar
		列中所有数据大小接近一致使用uchar
		列中数据项的大小差异很大，则使用 nvarchar
		```


	* 创建表
	+ ```
	  create table userinfo
	  (id number(6,0),
	  username varchar2(20),
	  userpwd varchar2(20),
	  regdate date);
	  ```


  * 对表的操作
	+ desc查看表的结构 ：desc userinfo
	+ 表就像是你买房子的房产证，表空间就像是你真正居住的空间；
	+ ***添加字段*** ：`alter table table_name add column_name datatype;`
	+ 修改表的时候一般用alter table
	+ PS：添加字段：`alter table userinfo add remarks varchar2(500);`

	+ ***更改字段数据类型***：`alter table table_name modify column_name datatype;`
	+ 可以更改的是modify 数据类型的长度，或者整个类型更改；更改的时候要让这个字段的数据为空才可以修改字段类型

	+ ***删除字段***：`alter table table_name drop column column_name;`
	+ ***修改字段名字***：`alter table table_name rename column column_name to new_column_name;`
	+ table_name表名，column_name旧字段名，new_column_name新字段名字
	+ 只是改名字而已，类型是不变的

	+ ***修改表名***：`rename table_name to new_table_name;`
	+ ***删除表***：`truncate table table_name`		
	+ 删除表中所有数据，而不是删除表，即是截断表，比delete快
	+ ***删除表的结构和所有数据***：`drop table table_name(表名);`

	
* ### 4 操作表中数据
	
	+ ***insert插入指定字段***：`insert into table_name (column1,column2,...) values(value1,value2,...)`
	+ 注：插入指定字段要确保其他字段是允许为空的，不允许为空就报错
	+ 表中插入***整条数据***：`insert：insert into table_name values(value1,value2,...)`
	+ 插入表全部数据时不需要列出数据表的字段，***values后面插入数据要跟表的字段一一对应！***
	+ 如：`insert into userinfo values(1,'xxx',sysdate);  `
	+ ***sysdate***：获取当前时间，是系统提供的函数

	+ ***创建表时给定字段添加默认值default***
	+ ```
		create table userinfo1
		(id,number(6,0),
		regdate date default sysdate);
	```
* ***使用默认值***
	+ ***插入***：insert into userinfo1 values(1);
	+ 这样是不行的，还没有一一对应
	+ 正确的做法是：`insert into userinfo1(id) values(1);`
	+ 表名前面要用小括号，需要给哪个字段添加就写上

	+ ***为已存在的表的字段添加默认值***：`alter table userinfo modify email default '无';`
	+ modify，default为关键字
	+ 当然我给email这个字段添加默认值，我也可以在***添加的时候给他设置值***
	+ `insert into userinfo1(id,email) values(4,'aaa');`

	+ ***在建表时复制表***： `create table table_new as select column1,...|*from table_old;`
	+ 如：`create table userinfo_new as select * from userinfo;`
	+ 创建表的时候，可以复制别的表的表结构（字段）好表数据，即把别的表复制一份到userinfo_new这个新表上
	
	+ 在创建表的时候，***只复制个别字段结构和数据***：`create table userinfo_new1 as select id,username from userinfo;`
	+ 只复制userinfo的id和name到这个userinfo_new1这个新表上

	+ ***在添加时复制表***
	+ `insert into table_new [(column1,...)] select column1,... |* from table_old;`
	+ table_new这个表是已经存在的，从select到最后这个位置本来是values，用select代替，还有这个table_new这个表的字段要和后面的select的字段的顺序类型一致，相匹配
	+ ***在添加时复制表（全部）***：`insert into userinfo_new select * from userinfo;`		已更新4条
	+ 添加数据时，把数据从别的表中拿过来，这个语句查出userinfo的全部数据添加到userinfo_new表中
	+ userinfo_new只有id和username，而userinfo多了个为空的email字段，但是userinfo的前两个字段跟他是一样的，只要顺序对的上就可以

	+ ***在添加时复制表（部分）***：`insert into userinfo_new(id,username) select id,username from userinfo;`
	+ ***指定复制某些字段***，userinfo的字段名字可以跟userinfo_new的字段名字不一样，但类型要是一样的

* ***update更新***
	+ ***更新字段数据***：`update table_name set column1 = value1,...[where conditions]`
	+ 更新数据的时候，新的更新值数据类型要和之前的类型一致；不加上where就是更新全部数据，加上指具体哪条
	+ 如：`update userinfo set userpwd = '111',email = '111@qq.com';`		已更新4条
	+ 这样就是全部数据都更新了，多个值的更新
	
* ***delete删除***
	+ `delete from table_name [where conditions]`
	+ 无条件删除全部数据（***trancate效率更高***）
	+ 但是delete可以删除加条件，trancate删除的是全部数据，不可以加条件

* 怎么创建数据库用户？
	+ 使用最高权限的sys用户登录数据库：conn sys/密码 as sysdba;
	+ 创建数据库用户，例如此处创建一个test用户，登录密码自定义：create user  test identified by  密码；
	+ 授权，授予test用户连接数据库的权限：grant create session to test;
	+ 授权，给test用户授操作表空间的权限：grant  unlimited tablespace to  test;

* 用sqlplus增删改不用commit吗？
	+ 自动提交：若把AUTOCOMMIT设置为ON，则在插入、修改、删除语句执行后，系统将自动进行提交，这就是自动提交。其格式为：SQL>SET AUTOCOMMIT ON；







* ### 5 ***约束***
	* 非空约束
	+ 在创建表时设置非空约束
	+ ```create table table_name(
		column_name datatype not null,...
	);```
	+ PS:数据类型后面加上not null（非空约束）
	+ 如果设置了非空约束，插入时插入为空就报错；要是没给他值，可以结合默认值

	+ 在修改表时添加非空约束：alter table table_name modify column_name datatype not null;
	+ 给已存在的表的字段添加非空约束：alter table userinfo modify username varchar2(20) not null;
	+ PS:有时候报错是因为：表中的数据有些是空的数据，所以要把他们先删了。
	+ 在修改表时去除非空约束：alter table table_name modify column_name datatype null;
	+ not null 改为 null，允许为空

	* 主键约束
	+ 一张表只能设计一个主键约束；
	+ 主键约束可以由多个字段构成（联合主键或复合主键）
	+ 在创建表时设置主键约束：```create table table_name(
		column_name datatype primary key,...
	)```

	+ 如： ```create table userinfo_p
	(id number(6,0) primary key,
	 username varchar2(20),
	 userpwd varchar2(20)
	);```
	+ 为id添加主键约束，主键也是默认是非空not null
	+ 在创建表时设置主键约束：constraint constraint_name primary key(column_name1,...)
	+ 联合主键 constraint
	+ 栗子：```create table userinfo_p1
		(id number(6,0),
		 username varchar2(20),
		 userpwd varchar2(20),
		 constraint pk_id_username primary key(id,username)
		);```
		+ constraint这是关键字；	pk_id_username这个是联合主键的名字（自己起的）
		+ 联合主键：为id和username共同创建主键
		+ 如果你查一下desc表的结构，你会发现id和username都是not null不为空
		+ 如果我忘记联合主键的名字，去查一下***desc user_constraints***这个数据字典
		+ <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/8.%E6%9F%A5%E6%89%BE%E8%81%94%E5%90%88%E4%B8%BB%E9%94%AE%E7%9A%84%E6%95%B0%E6%8D%AE%E5%AD%97%E5%85%B8.png"  height="380" width="795"> 
		</div>

	  + 查出表中的联合主键名字：select constraint_name,constraint_type from user_constraints where table_name = 'USERNAME_P1' 
		+ constraint_name联合主键字段，constraint_type类型；table_name表名一定要大写
		+ 如果表没有给他设置联合主键的话，就查找出默认的id主键，默认主键名字是系统给他起的（名字一般叫SYS_COO什么什么）

		+ 在修改表时添加主键约束：add constraint constraint_name primary key(column_name1,...);
		+ constraint_name主键的名字一般都是pk_xxx这样子的，primary key(column_name1)要设为主键的字段
		+ 更改约束的名称：rename constraint old_name to new_name
		+ 如：alter table userinfo rename constraint_pk_id to new_pk_id;
		+ 禁用主键约束：alter table userinfo disable constraint new_pk_id;
		+ 查看主键状态：select constraint_name,status from user_constraints where table_name = 'USERINFO';
		+ alter table userinfo add constraint pk_id primary key(id);
		+ 添加主键之前，表中的字段是为空的，唯一的
		+ 删除主键约束： disable|enable constraint constraint_name
		+ 禁用|启用 constraint约束关键字
		+ 就比如我现在不想要主键约束，我就先禁用disable他，以后想要就enable启用他
		+ 删除主键约束：drop constaint constraint_name 
		+ 注意drop的是constaint，其他都有个r,constraint
		+ drop primary key[cascade]
		+ 因为主键只有一个，所以可以不用写名字；[cascade]删除主键约束，可选项（外键用的），就是有外键引用他，也会将其外键的主键给删除
		+ 删除表的主键：alter table userinfo_p drop primary key;

		+ 在创建表时设置外键约束
		+ 从表中外键字段的值必须来自主表中的相应字段的值，或者为null值
		+ ```create table table1
		(column_name datatype references
		table2(column_name),...);```
		+ create table 创建表名
		(字段名 字段类型 references
		外键表名(外键表的主键字段));
		+ 设置外键约束时，主表的字段必须是主键，table1是从表，table2是主表，主从表的字段类型要是一样

		+ 练习：
		+ ```create table typeinfo
		(typeid varchar2(10) primary key,
		 typename varchar2(20)
		);```

		+ ```create table userinfo_f
		(id varchar2(10) primary key,
		 username varchar2(20),
		 typeid_new varchar2(10) references typeinfo(typeid));```
		+ userinfo_f从表创建的时候，设置外键约束，这个typeid_new和主表typeinfo的typeid关键外键，类型要一样
		
		+ 在创建表时设置外键约束（第二种方法）
		+ constraint constraint_name foreign key(column_name) references
		table_name(column_name) [on delete cascade]
		+ constraint关键字，外键名字constraint_name一般叫fk_xxx
		+ [on delete cascade]是级联删除，如果主表中该条记录被删除，那么从表中使用了这条记录的值也会被删除

		+ 根据第一种方法的例子继续
		+ create table userinfo_f1
		(id varchar2(10) primary key,
		 username varchar2(20),
		 typeid_new varchar2(10),
		 constraint fk_typeid_new foreign key(typeid_new) references typeinfo(typeid);

		+ 接下来介绍[on delete cascade]是级别删除
		+ create table userinfo_f2
		(id varchar2(10) primary key,
		 username varchar2(20),
		 typeid_new varchar2(10),
		 constraint fk_typeid_new2 foreign key(typeid_new) references typeinfo(typeid) on delete cascade
		+ fk_typeid_new2这些外键约束的名字要唯一，主表删，从表对应的数据也会跟着删

		+ 如：主表插入一行：insert into typeinfo values(1,1)	（注意这里插入的值是1）
		+ 从表也插入一行：insert into userinfo_f1(id,typeid_new)values(1,2);
		+ 这样就报‘违反完整约束条件’的错，因为typeid_new是关联外键的，所以值要和主表的一样，所以不能是2，要1或者null就可以

		+ 再如：主表插入一行：insert into typeinfo values(2,2)	（注意这里插入的值是2）
		+ 从表也插入一行：insert into userinfo_f1(id,typeid_new)values(2,null);
		+ 这样也是可以的，2或者null都可以
		+ 总结：***从表的关联的外键必须和主表的值一样或者是null***

		+ 在修改表时添加外键约束
		+ add constraint constraint_name foreign key(column_name) references table_name(column_name)[on delete cascade]
		+ 在已存在的表添加外键约束：alter table userinfo_f4 add constraint fk_typeid_alter foreign key(typeid_new)references typeinfo(typeid);
		+ userinfo_f4的typeid_new和另外的typeinfo表的typeid关联外键约束
		+ 当然后面也可以设置级别删除
		+ 删除外键约束：disable | enable constraint constraint_name
		+ 禁用 | 启用 外键
		+ <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/9.%20%E5%A4%96%E9%94%AE%E7%BA%A6%E6%9D%9F%E7%A6%81%E7%94%A8%E5%90%AF%E7%94%A8.png"  height="380" width="795"> 
		</div>
		
		+ user_constraints可以查表的主键外键约束名字、是否禁用
		+ 要是不知道表的约束是否为启用，可查看user_constraints数据字典
		+ 删除外键约束：drop constraint constraint_name;
		+ 彻底删除外键约束，前面加alter table 表名字 + 

		* 唯一约束
		+ 唯一约束和主键约束的区别：
		+ 1）主键字段必须是非空的，唯一约束允许有一个空值
		+ 2）唯一约束和主键约束都是保证字段值的唯一性
		+ 3）唯一约束在一张表中可以有多个，主键每张表中只有一个
		
		+ 在创建表时设置唯一约束：create table table_name(column_name datatype unique,...)
		+ 如：```create table userinfo_u
		(id varchar2(10) primary key,
		 username varchar2(20) unique,
		 userpwd varchar2(20)
		);```
		+ 所谓唯一约束就是在表中的字段是唯一的，但是可以为空；比如用户名一般都是唯一的
		+ 是字段的值是唯一的，表中可以设置多个唯一约束
		+ 在创建表时设置唯一约束（第二种方法）：constraint constraint_name unique(column_name)
		+ 表级设置唯一约束，每一个唯一约束的名字都是唯一的
		+ 假如有多个这样的，就写多个这条语句，不能写在unique()括号里面的，因为约束名字是唯一的
		+ 如：```create table userinfo_u1
		(id varchar2(10) primary key,
		 username varchar2(20),
		 userpwd varchar2(20),
		 constraint un_username unique(username)
		);```
		+ 表级设置唯一约束
		+ 在修改表时添加唯一约束：add constraint constraint_name unique(column_name);
		+ 一般命名唯一约束都是un_xxx
		+ 删除唯一约束：disable|enable constraint constraint_name
		+ u:唯一约束、p：主键、r：外键、c：检查约束
		+ 删除唯一约束：drop constraint constraint_name

		+ 在创建表时设置检查约束：create table table_name (column_name datatype check(expressions),...)
		+ expressions[表达式，即约束条件]
		+ 如：```create table userinfo_c
		(id varchar2(10) primary key,
		username varchar2(20),
		salary number(6,0) check(salary>0));```
		+ 在创建表设置检查约束，让工资salary大于0，check关键字
		+ 插入数据：insert into userinfo_c values(1,'aaa',-50);
		+ 这样就报错，因为salary要大于0，很明显-50小于0

		+ 在创建表时设置检查约束：constraint constraint_name check(expressions)
		+ 可以在列级设置，也可以在这里的表级设置，一般命名都是ck_xxx
		+ 如：```create table userinfo_c1
		(id varchar2(10) primary key,
		username varchar2(20),
		salary number(6,0),
		constraint ck_salary check(salary>0) 
		);```
		+ ck_salary约束名字，在表级设置检查约束
		+ 在修改表时添加检查约束：add constraint constraint_name check(expressions);
		+ 如：alter table userinfo_c3 add constraint ck_salary_new check(salary>0);
		+ 在已存在的表中添加检查约束
		+ 删除检查约束：disable | enable constraint constraint_name
		+ 删除检查约束：drop constraint constraint_name 

		+ 总结：非空约束、主键约束、外键约束、唯一约束、检查约束
		+ 1.五个中就主键约束是要求唯一的，而且可以由多个字段组成，也就是表级约束可以多个字段写在一起
		+ 2.五个中只有外键是涉及两个表的
		+ <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/9.%20%E5%A4%96%E9%94%AE%E7%BA%A6%E6%9D%9F%E7%A6%81%E7%94%A8%E5%90%AF%E7%94%A8.png"  height="380" width="795"> 
		</div>

		+ 创建表（修改）时设置约束
		+ 只有唯一约束和最特殊：alter table table_name modify column_name datatype not null;
		+ 不为空就not null，为空就null
		+ 更改约束名字：rename constraint old_name to new_name
		+ 数据字典（user_constraints），不知道约束名字可以使用这个字典去查看
		+ 删除约束
		+ 只有非空约束最特殊设为null就删除，默认也是null：alter table table_name modify column_name datatype null;
		+ disable|enable constraint constraint_name
		+ drop constraint constraint_name
		+ drop primary key，就是主键比较特殊，只有一个，可以用这个方法删除
		+ 都是在alter table table_name之后加上
		+ ***唯一约束不能有重复值，可以有空值，但是空值只有一个***

		+ 联合主键和主键的区别是什么？
		+ 主键的一个目的就是确定数据的唯一性，它跟唯一约束的区别就是，唯一约束可以有一个NULL值，但是主键不能有NULL值，再说联合主键，联合主键就是说，当一个字段可能存在重复值，无法确定这条数据的唯一性时，再加上一个字，两个字段联合起来确定这条数据的唯一性。比如你提到的id和name为联合主键,在插入数据时，当id相同，name不同，或者id不同，name相同时数据是允许被插入的，但是当id和name都相同时，数据是不允许被插入的。
		+ 表级约束和列级约束有什么区别吗？
		+ 非空约束只能在列级级设置，不能在表级设置！
		+ 其他约束既可以在列级设置，也可以在表级设置！
		+ 
		+ 唯一约束：保证字段值的唯一性
		+	唯一约束和主键约束都是保证字段值的唯一性；
		+	唯一约束可以为空，主键约束不可以；
		+	唯一约束在一张表中可以有多个，主键每张表中只能有一个；
		+	设置列级唯一约束：
		+ 
		+ 怎么更改check约束：alter table emp1 add constraint ch_sal check(sal>500);怎么更改呀？想变成sal>400
		+ 回答：alter table emp1 modify constraint ch_sal check(sal>400);
		+ 修改表时添加检查约束：ADD CONSTRAIN constraint_name CHECK(expressions);

		+ 非空约束和默认值约束只能在列级设置，不能在表级设置；非空约束一般而言我们是针对某一列进行约束所以只能进行列级约束
		+ 在修改表的结构的时候修改非空约束和默认值约束，用的是修改字段的语句（MODIFY）。

		+ 总结：
		+ 非空约束 NOT NUL （禁止插入字段为空）
		+	主键约束 PRIMARY KEY （每张表只能有一个，可以由多个字段构成）
		+	外键约束 FOREIGN KEY （约束字段与外表字段匹配，类型相同，数据必须 IN {外表数据}）
		+	唯一约束 UNIQUE （保证数据的唯一性，可以由多字段构成） 
		+	检查约束 CHECK （保证数据值的安全可靠，并允许范围内）



		+ 查询



















		
	
	 

	























            


