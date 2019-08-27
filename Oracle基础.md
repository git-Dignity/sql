# Oracle #

* ## Oracle官方下载地址：
> <https://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html>

* ### 下载好后，我们就可以打开sql plus命令行玩转oracle，这个工具是oracle最底层，软件下载好会自动帮我们下载这个工具

=========================================

<!-- * 编码ASCII、ANSI、GBK、unicode、UTF-8渊源、占用字节等：
	> <https://blog.csdn.net/wskzgz/article/details/88710263> -->
	
	+ 默认sys、system这两个账号不会被锁定
	+ 可以在浏览器上登录oracle
<div align="center">
	<img src="https://raw.githubusercontent.com/git-Dignity/sql/master/img/1.%20%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%8A%E7%99%BB%E5%BD%95.png"  height="330" width="495">
</div>
	
	+ Gbk就是放中文：中文两个字节，其他一个字节
	+ Unicode，就什么都是两个字符，你好IT’ 四个字符占八个字节
	+ utf8一个汉字占3个字节，占一个字符；Gbk，就两个
	
=========================================

* ## 了解varchar、nvarchar、char
+ 比如 ‘ 我和coffee ’这句话
+ 那么varchar字段占2×2+6=10个字节的存储空间，而nvarchar字段占8×2=16个字节的存储空间。
+ 如字段值只是英文可选择varchar，而字段值存在较多的双字节（中文、韩文等）字符时用nvarchar，是啊，因为nvarchar什么都当做两个字符，但是我们英文就是一个，所以nvarchar最好放中文
+ char(n)：定长，索引效率高 程序里面使用trim去除多余的空白
	假如Char(10)：给他存123 他就是123后面加7个空格
+ oracle 中varchar2(10)  既10个字节3个汉字，mysql中varchar(10) 既10个字符10个汉字，所以现在可以将mysql的varchar字段减小1/3了，性能也能提高哦。














            


