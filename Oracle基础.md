# Oracle #

* ## Oracle官方下载地址：
www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html
* ### 如果他电脑有开oracle数据库，则会显示OK（0毫秒）

=========================================

* ## 编码ASCII、ANSI、GBK、unicode、UTF-8
* 编码ASCII、ANSI、GBK、unicode、UTF-8渊源、占用字节等：
	> <https://blog.csdn.net/wskzgz/article/details/88710263>
	
	+ ASCII码：　一个英文字母（不分大小写）占一个字节的空间，一个中文汉字占两个字节的空间。
	+ UTF-8编码：一个英文字符等于一个字节，一个中文（含繁体）等于三个字节。变长
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














            


