# Oracle #

* ## Oracle官方下载地址：
> <https://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html>

* ### 下载好后，我们就可以打开sql plus命令行玩转oracle，这个工具是oracle最底层，软件下载好会自动帮我们下载这个工具

=========================================
* sql plus命令:
	+ 默认sys、system这两个账号不会被锁定，其他都会被锁定
	+ 可以在浏览器上登录oracle，localhost:1158/em/console/logon/logon，sys/root123/sysdba
    + <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/1.%20%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%8A%E7%99%BB%E5%BD%95.png"  height="330" width="495">
	</div>
	
	+ Oracle的卸载可以在db_home文件夹下找到deinstall文件，里面有个bat的批处理卸载文件，一路点击回车即可完成删除。
	+ <div align="center">
		<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/2.%E5%88%A0%E9%99%A4oracle.png"  height="330" width="495">
	</div>

* 系统用户:
	+ 有四个系统用户，分别是sys、system、sysman、scott；
	+ sys和system这两个都是权限比较高的用户，并且sys权限高于system，其中sys用户必须以管理员sysdba权限才能登录
	+ sysman：用于操作企业管理器使用的，到这里这三个都是安装的时候就已经设置好的密码
	+ Scott：他是创始人的名字，用户权限最小的系统用户，默认的密码是tiger

* 使用系统用户登录:
	+ 使用system用户登录 [username/password] [@server] [as sysdba|sysoper]  --  system可以不需要sysdba登录
	如：system/root123 @orcl as sysdba； orcl就是自己设置的服务名，如果不是连别人的oracle，这里的@server就是写自己的@ip，
	如果oracle安装在本机，还可以省略@orcl，最终 connect sys/root123 as sysdba;

	+ 使用sys用户登录，和system一样，sys一定要用sysdba登录，connect换用户登录
	
=========================================

* ## 了解varchar、nvarchar、char
+ 比如 ‘ 我和coffee ’这句话
+ 那么varchar字段占2×2+6=10个字节的存储空间，而nvarchar字段占8×2=16个字节的存储空间。
+ 如字段值只是英文可选择varchar，而字段值存在较多的双字节（中文、韩文等）字符时用nvarchar，是啊，因为nvarchar什么都当做两个字符，但是我们英文就是一个，所以nvarchar最好放中文
+ char(n)：定长，索引效率高 程序里面使用trim去除多余的空白
	假如Char(10)：给他存123 他就是123后面加7个空格
+ oracle 中varchar2(10)  既10个字节3个汉字，mysql中varchar(10) 既10个字符10个汉字，所以现在可以将mysql的varchar字段减小1/3了，性能也能提高哦。














            


