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
+ ===
  <br />

	* 创建表
	+ ```create table userinfo
	(id number(6,0),
	 username varchar2(20),
	 userpwd varchar2(20),
	 regdate date);
	 ```
+ ===
<br />

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

	


	























            


