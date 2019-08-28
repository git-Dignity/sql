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
	+ 永久表空间：持久化保存，例如表、视图、存储过程
	+ 临时表空间：临时查询的表
	+ UNDO表空间：存储数据修改之前的数据，用于事务回滚操作
	<br />

	+ 查看用户的表空间：dba_tablespaces（系统管理员查看的数据字典）、user_tablespaces（普通用户）数据字典
	+ <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/4.%20%E6%9F%A5%E7%9C%8B%E7%94%A8%E6%88%B7%E8%A1%A8%E7%A9%BA%E9%97%B4.png"  height="330" width="695"> 
	</div>













            


