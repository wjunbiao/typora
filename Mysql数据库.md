# MySql数据库

## 为什么需要数据库

保存数据，下次访问的时候，数据是最新的数据。

## 数据库原理图

![image-20240601113619038](./assets/image-20240601113619038.png)

## 安装数据库

![image-20240601143543549](./assets/image-20240601143543549-1717223744745-1.png)

```java
//my.ini配置文件
[client]//表示客户端的配置
port=3306//端口是3306
default-character-set=utf8//字符集是utf8
[mysqld]//表示对服务的配置
# 设置为自己MYSQL的安装目录
basedir=D:\mySql\mysql-5.7.19-winx64\
# 设置为MYSQL的数据目录
datadir=D:\mySql\mysql-5.7.19-winx64\data\
port=3306//系统自己去创建
character_set_server=utf8
#跳过安全检查
skip-grant-tables

```

![image-20240601125132257](./assets/image-20240601125132257.png)
小细节；每次操作完后退回到D盘，不然有时候一些操作会出现正在访问的错误

![image-20240601125409812](./assets/image-20240601125409812.png)
启动和停止mySql的命令

![image-20240601125528615](./assets/image-20240601125528615.png)
用户是root

```mysql
net start mysql #启动数据库
net stop mysql #停止数据库
use mysql; #表示使用数据库,注意有一个分号
update user set authentication_string=password('hsp') where user='root' and Host='localhost';#修改root用户的密码为hsp,然后从本地登录
flush privileges;//刷新权限才能生效
```

### 命令行连接Mysql

只要是一个服务往往都会监听一个端口，商品是在my.ini配置文件中指定的

![image-20240601143543549](./assets/image-20240601143543549-1717223744745-1.png)

![image-20240601143804425](./assets/image-20240601143804425.png)

```
grant all privileges on *.* to root@'%' identified by '123456' with grant option;
flush privileges;
设置mysq允许外部访问
```

### 安装使用图形化MySQL管理软件 

![image-20240601145847677](./assets/image-20240601145847677.png)

![image-20240601150436072](./assets/image-20240601150436072.png)

![image-20240601154140061](./assets/image-20240601154140061.png)

![image-20240601154204153](./assets/image-20240601154204153.png)

### 数据库的三层结构 

![image-20240601155254957](./assets/image-20240601155254957-1717228375676-3.png)

![image-20240601155412456](./assets/image-20240601155412456.png)

![image-20240601155549118](./assets/image-20240601155549118.png)

### 体会通过Java操作Mysql

![image-20240601155946949](./assets/image-20240601155946949.png)

 ![image-20240601160852091](./assets/image-20240601160852091.png)

## 数据库相关操作

### 创建数据库

![image-20240601182019473](./assets/image-20240601182019473.png)
![image-20240601182612371](./assets/image-20240601182612371.png)
默认字符集是utf8,排序规则是不区分大小写

![image-20240601183003800](./assets/image-20240601183003800.png)

![image-20240601183838841](./assets/image-20240601183838841.png)
表中如果没有指定字符集和校对规则，就会以数据库的校对规则为准

```mysql
#使用指令创建数据库db02
CREATE DATABASE db02
#删除数据库指令
DROP DATABASE db03
#创建一个使用utf8字符集的db03数据库
CREATE DATABASE db03 CHARACTER SET utf8
#创建一个使用utf8字符集，并带校对规则（区分大小写）的db04数据库
CREATE DATABASE db04 CHARACTER SET utf8 COLLATE utf8_bin

#测试表区分大小写是否是根据数据
SELECT * FROM t1 WHERE name = 'tom';
```

#### 查看、删除数据库

![image-20240601185408536](./assets/image-20240601185432686.png)

```mysql
#查看当前数据库服务器中的所有数据库
SHOW DATABASES
#查看前面创建的db01数据库的定义信息
SHOW CREATE DATABASE db01
#在创建数据库，表的时候，为了规避关键字，可以使用反引号解决
CREATE DATABASE db01
#删除数据库（一定要慎重，全球追杀）
DROP DATABASE db01


```

#### 备份和恢复数据库

![image-20240601191701098](./assets/image-20240601191701098.png)

![image-20240601191837216](./assets/image-20240601191837216.png)



![image-20240601201745081](./assets/image-20240601201745081.png)

![image-20240601202843393](./assets/image-20240601202843393.png)

![image-20240601203243368](./assets/image-20240601203243368.png)

### 表相关操作

#### 创建表

![image-20240601204347936](./assets/image-20240601204347936.png)

![image-20240601204217959](./assets/image-20240601204217959.png)

```mysql
CREATE TABLE `user`(
				id int,
				`name` VARCHAR(255),
				`password` VARCHAR(32),
				`birthday` DATE)
				CHARACTER SET utf8  COLLATE utf8_bin;

```

####  创建表练习

![image-20240604185807228](./assets/image-20240604185807228.png)

```mysql
#创建表的课堂练习
CREATE TABLE emp(
		id INT,
		`name` VARCHAR(32),
		sex CHAR(1),
		birthday DATE,
		entry_date DATETIME,
		job VARCHAR(32),
		salary DOUBLE,
		`resume` TEXT
		)CHARACTER SET utf8 COLLATE utf8_bin ENGINE INNODB

-- 添加一条
INSERT INTO emp VALUES(
		1,"小妖怪",'男','2022-1-1','2022-1-1 10:10:10', 
		'巡山的',3000,'大王叫我来巡山')
		SELECT * FROM emp
```

#### 修改表和删除

![image-20240604192340226](./assets/image-20240604192340226.png)

```mysql
-- 添加列
ALTER TABLE tablename 
ADD (column datatype [DEFAULT expr]
    [,column datatype]...);
-- 修改列
ALTER TABLE tablename
MODIFY(column datatype [DEFAULT expr]
      [,column datatype]...)
-- 删除列
ALTER TABLE tablename
DROP (column)
-- 查看表结构 
desc 表名；
-- 修改表名
Rename table 表名 to 新表名
-- 修改字符集
alter table 表名 character set 字符集
```

 ![image-20240604193025281](./assets/image-20240604193025281.png)

```mysql
DESC employee
		
ALTER TABLE emp ADD image VARCHAR(32) NOT NULL DEFAULT '' AFTER RESUME
ALTER TABLE emp MODIFY job VARCHAR(60) NOT NULL DEFAULT ''
ALTER TABLE emp DROP sex
RENAME TABLE emp TO employee
ALTER TABLE employee CHARACTER SET utf8
ALTER TABLE employee CHANGE `name` user_name VARCHAR(60) NOT NULL DEFAULT ''
```



### mysql的列数据类型

![image-20240601210836737](./assets/image-20240601210836737.png)
![image-20240601210914726](./assets/image-20240601210914726.png)
![image-20240601210250164](./assets/image-20240601210250164.png)

![image-20240601210929875](./assets/image-20240601210929875.png)
参考手册

![image-20240601211145996](./assets/image-20240601211145996.png)

#### 数值类型

![image-20240601211627370](./assets/image-20240601211627370.png)

![image-20240601211526484](./assets/image-20240601211526484.png)

![image-20240601213308964](./assets/image-20240601213308964.png)

![image-20240601213331880](./assets/image-20240601213331880.png)

#### 小数类型

![image-20240601213503319](./assets/image-20240601213503319.png)

![image-20240601213722979](./assets/image-20240601213722979.png)
![image-20240601213738955](./assets/image-20240601213738955.png)
这三个小数的精度不一样。 

 ![image-20240604143124117](./assets/image-20240604143124117.png)
decimal可以存非常非常大的数。

#### 字符串的基本使用

![image-20240604143554680](./assets/image-20240604143554680.png)
varchar 可存入65535个字节，实际要看它的编码，比如utf8编码，前3个用来记录字节数，然后除3就等于21844 ，如果是unicode编码就是除2

细到极致！如果不看过一周，会全部忘记

![image-20240604144455214](./assets/image-20240604144455214.png)
是21844，老韩真牛逼

![image-20240604165252223](./assets/image-20240604165252223.png)

 

```mysql
CREATE TABLE t06(
			num1 FLOAT,
			num2 DOUBLE,
			num3 DECIMAL)
#添加数据 
INSERT INTO t06 VALUES (88.123456789123456789,88.123456789123456789,88.123456789123456789)
SELECT * FROM t06


CREATE TABLE t07(
			`name` CHAR(255))
	
CREATE TABLE t08(
			`name` VARCHAR(21844))

CREATE TABLE t09(
			`name` VARCHAR(32766))CHARACTER SET gbk;
DROP TABLE t09
```

**细节：**

 ![image-20240604182523763](./assets/image-20240604182523763.png)

![image-20240604182825408](./assets/image-20240604182825408.png)

![image-20240604183015847](./assets/image-20240604183015847.png)
char有char的价值。

![image-20240604183817337](./assets/image-20240604183817337-1717497498217-1.png)

![image-20240604183826830](./assets/image-20240604183826830.png)
TEXT这种更得于空间利用

#### 日期类型的基本使用

![image-20240604184051941](./assets/image-20240604184051941.png)

![image-20240604185638599](./assets/image-20240604185638599.png)

```mysql
CREATE TABLE t14(
	birthday DATE, -- 生日
	jobtime DATETIME, -- 工作时间，记录年月日时分秒
	login_time TIMESTAMP 
	NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP)-- 			  登录时间，如果希望login_time列自动更新
-- 	不能为空，默认是当前时间戳，修改是当前时间戳
INSERT INTO t14 (birthday,jobtime) VALUES('2022-11-11','2022-11-11 10:10:10');
SELECT * FROM t14
```

### CRUD（增删改查）

![image-20240604194920215](./assets/image-20240604194920215.png)

 ![image-20240604194950624](./assets/image-20240604194950624.png)

   ![image-20240604200434729](./assets/image-20240604200434729.png)

**insert细节：**

![image-20240604202323628](./assets/image-20240604202323628.png)

![image-20240604200850911](./assets/image-20240604200850911.png)
可以转化的

 ![image-20240604201520299](./assets/image-20240604201520299.png)
一条insert可以添加多条数据

![image-20240604202343146](./assets/image-20240604202343146.png)

#### update

![image-20240604202819892](./assets/image-20240604202819892.png)

![image-20240604202838039](./assets/image-20240604202838039.png)

**细节：**

![image-20240604203139122](./assets/image-20240604203139122.png)

#### delete

![image-20240604203638799](./assets/image-20240604203638799.png)

**细节：**

![image-20240604203709890](./assets/image-20240604203709890.png)

#### Select

![image-20240604204230339](./assets/image-20240604204230339.png)

![image-20240604204959225](./assets/image-20240604205040266.png)

![image-20240604205122977](./assets/image-20240604205122977.png)

![image-20240604205218249](./assets/image-20240604205218249.png)

![image-20240604205624959](./assets/image-20240604205624959.png)

![image-20240604205638231](./assets/image-20240604205638231-1717505799049-3.png)

 ![image-20240604210608169](./assets/image-20240604210608169.png)

##### 排序

![image-20240604210825792](./assets/image-20240604210825792.png)

![image-20240604211053137](./assets/image-20240604211053137.png)

![image-20240604211644863](./assets/image-20240604211644863.png)
这种起别名更加清晰一点

##### 合计/统计函数-count

![image-20240604211816828](./assets/image-20240604211816828.png)

![image-20240604212205498](./assets/image-20240604212205498.png)

##### 合计函数-sum 

![image-20240604212540804](./assets/image-20240604212540804.png)

##### 合计函数-avg

![image-20240604212555897](./assets/image-20240604212555897.png)

#####  合计函数-Max/min

![image-20240604212656059](./assets/image-20240604212656059.png)

#####  分组

![image-20240604212905352](./assets/image-20240604212905352.png)

​				 <img src="./assets/image-20240604233222368.png" alt="image-20240604233222368"  />      

#### 查询加强

![image-20240607131925623](./assets/image-20240607131925623.png)

![image-20240607132052980](./assets/image-20240607132052980.png)

![image-20240607132404522](./assets/image-20240607132404522.png)

![image-20240607132548912](./assets/image-20240607132548912.png)
如果要判断某个字段是否为空，用is 不能用等号  

![image-20240607132743587](./assets/image-20240607132743587.png)

![image-20240607133014925](./assets/image-20240607133014925.png)

![image-20240607134004382](./assets/image-20240607134004382.png)

![image-20240607133551644](./assets/image-20240607133551644.png)

![image-20240607133830563](./assets/image-20240607133830563.png)

要提前算出来写上，最好不要直接写到参数位置，报错的。

![image-20240607134059420](./assets/image-20240607134059420.png)

![1717738990889](./assets/1717738990889.png)
![image-20240607134707202](./assets/image-20240607134707202.png)

巧妙利用了count（)不统计空的规则 

伴随程序员终生的2个东东：螺旋递升，套娃

尝试写->修改->尝试

![image-20240607135244068](./assets/image-20240607135244068.png)
注意它们之间的顺序，如果写错了是通不过的。

![image-20240607135756239](./assets/image-20240607135756239.png)

## 常用函数 

### 字符串常用函数

![image-20240605165710824](./assets/image-20240605165710824.png)

```mysql
#演示字符串的相关操作
SELECT * FROM emp
SELECT CHARSET(ename) FROM emp
SELECT CONCAT(ename ,"    工作是   ",job) FROM emp
-- dual 亚元表，系统表，可以作为测试表使用
SELECT INSTR('hanshunping','ping') FROM DUAL;
SELECT UCASE(ename) FROM emp
SELECT LCASE(ename) FROM emp
SELECT LEFT(ename,2) FROM emp
SELECT RIGHT(ename,2) FROM emp
SELECT LENGTH('韩顺平')FROM emp
SELECT job  FROM emp
SELECT ename,REPLACE(job,'MANAGER','经理')FROM emp
SELECT STRCMP('hsp','asp')FROM DUAL -- 返回-1，0，1
SELECT SUBSTRING(ename,1,2) FROM emp
SELECT LTRIM("  韩顺平") FROM DUAL
SELECT CONCAT(LCASE(SUBSTRING(ename,1,1)),SUBSTRING(ename,2)) FROM emp
SELECT CONCAT(LCASE(LEFT(ename,1)),RIGHT(ename,LENGTH(ename)-1)) FROM emp
```

### 数学函数

![image-20240605181509941](./assets/image-20240605181509941.png)

![image-20240605181645271](./assets/image-20240605181645271.png)

![image-20240605181649073](./assets/image-20240605181649073.png)

 ![image-20240605181837258](./assets/image-20240605181837258.png)

![image-20240605183813315](./assets/image-20240605183813315.png)

### 日期函数

![image-20240605183903935](./assets/image-20240605183903935-1717583944828-5.png)

![image-20240605183958040](./assets/image-20240605183958040.png)

![image-20240605192822043](./assets/image-20240605192822043.png)

![image-20240605192848851](./assets/image-20240605192848851.png)

![image-20240605193316037](./assets/image-20240605193316037.png)

![image-20240605193146433](./assets/image-20240605193146433.png)
只显示日期不显示时间，用DATE(……)

 ![image-20240605193714669](./assets/image-20240605193714669.png)
用于一个月、一年以内的订单，

![image-20240605194342617](./assets/image-20240605194342617.png)
求相差多少天。

![image-20240605195846525](./assets/image-20240605195846525.png)
求出活了多少天，假如活到80岁还剩多少天。

![image-20240605200750920](./assets/image-20240605200750920.png)
细节  

![image-20240607121110071](./assets/image-20240607121110071.png)
当前的一些时间

![image-20240607122346031](./assets/image-20240607122346031.png)

![image-20240607122359880](./assets/image-20240607122359880.png)

### 加密函数

![image-20240607122523845](./assets/image-20240607122523845.png)

![image-20240607125834996](./assets/image-20240607125834996.png)

![image-20240607130155629](./assets/image-20240607130155629.png)

![image-20240607130436856](./assets/image-20240607130436856.png)  

### 流程控制函数 

![image-20240607130615230](./assets/image-20240607130615230.png)

![image-20240607131501430](./assets/image-20240607131501430.png)![image-20240607131120084](./assets/image-20240607131120084.png)
![image-20240607131321105](./assets/image-20240607131321105.png)
![image-20240607131456212](./assets/image-20240607131456212.png)



## 多表查询

###  多表笛卡尔积 

![image-20240607140156259](./assets/image-20240607140156259.png)



![image-20240611164943297](./assets/image-20240611164943297.png)



![image-20240611170126152](./assets/image-20240611170126152.png)
不加条件就是行相乘

![image-20240611170332507](./assets/image-20240611170332507.png)

![image-20240611170645986](./assets/image-20240611170645986.png)

![image-20240611170746418](./assets/image-20240611170746418.png)
查询条件至少不能少于表的个数-1，否则会出现笛卡尔集。比如说3个表查询那至少有2个查询条件

![image-20240611171319592](./assets/image-20240611171319592.png)

### 自连接

![image-20240611174633987](./assets/image-20240611174633987.png)

一张表当作两张表来使用

![image-20240611180513253](./assets/image-20240611180513253.png)

### 子查询 

![image-20240611180617694](./assets/image-20240611180617694.png)

![image-20240611181214042](./assets/image-20240611181214042.png)
第7 行这个就是单选子查询

![image-20240611181600628](./assets/image-20240611181600628.png)
查询和10号部门相同的岗位但是不包括10号部门

### 临时表

![image-20240611193026698](./assets/image-20240611193026698.png)

![image-20240611201436738](./assets/image-20240611201436738.png)

### all和any

![image-20240611204423130](./assets/image-20240611204423130.png)

![image-20240611204147609](./assets/image-20240611204147609.png)

![1718109799771](./assets/1718109799771.png)

### 多列子查询 

![image-20240611205114602](./assets/image-20240611205114602.png)

![image-20240611204902137](./assets/image-20240611204902137.png)

### 子查询练习

![image-20240611205536323](./assets/image-20240611205536323.png)

![image-20240611205453163](./assets/image-20240611205453163.png)

![image-20240611205756195](./assets/image-20240611205756195.png)

![image-20240611205652438](./assets/image-20240611205652438.png)

![image-20240611205839273](./assets/image-20240611205839273.png)

![image-20240611210332658](./assets/image-20240611210332658.png)

![image-20240611210626821](./assets/image-20240611210626821.png)

## 表复制和去重

![image-20240612112648764](./assets/image-20240612112648764.png)

![image-20240612112704216](./assets/image-20240612112704216.png)

复制表的记录

![image-20240612112924461](./assets/image-20240612112924461.png)
复制表的结构信息！

distince只是查询的时候暂时去重
![image-20240612121729939](./assets/image-20240612121729939.png)

## 合并查询 

![image-20240612122143709](./assets/image-20240612122143709.png)

![image-20240612122325099](./assets/image-20240612122325099.png)

![image-20240612122349666](./assets/image-20240612122349666.png)

![image-20240612122437217](./assets/image-20240612122437217.png)

## 外连接

![image-20240612173126813](./assets/image-20240612173126813.png)

![image-20240612173816786](./assets/image-20240612173816786.png)

![image-20240612173918524](./assets/image-20240612173918524.png)

![image-20240612174156126](./assets/image-20240612174156126.png)

![image-20240612175722660](./assets/image-20240612175722660.png)

![image-20240612180000338](./assets/image-20240612180000338.png)

![image-20240612181024990](./assets/image-20240612181024990.png)

![image-20240612180655338](./assets/image-20240612180655338.png)

左边这个表如果没有和右边这个表有匹配的，会显示左这个表所有的记录

## sql约束

![image-20240612181203573](./assets/image-20240612181203573.png)

![image-20240612181225521](./assets/image-20240612181225521.png)

![image-20240612181738624](./assets/image-20240612181738624.png)

### 主键

![image-20240612182051711](./assets/image-20240612182051711.png)

### unique

![image-20240612182717290](./assets/image-20240612182717290.png)

![image-20240612182758999](./assets/image-20240612182758999.png)

![image-20240612182946905](./assets/image-20240612182946905.png)

### 外键 

![image-20240612183506489](./assets/image-20240612183506489.png)

![image-20240612183840547](./assets/image-20240612183840547.png)

![image-20240612183936664](./assets/image-20240612183936664.png)

![image-20240612184113493](./assets/image-20240612184113493.png)

![image-20240612185242842](./assets/image-20240612185242842.png)
主表必须具有主键约束或是unique约束。

![image-20240612185923532](./assets/image-20240612185923532.png)

### check

![image-20240612223044193](./assets/image-20240612223044193.png)

![image-20240612222746018](./assets/image-20240612222746018.png)
加约束条件，性别是能是男或女，工资必须在1000到2000之间

### 练习

![image-20240612223026266](./assets/image-20240612223026266.png)

![image-20240612223445750](./assets/image-20240612223445750.png)

![image-20240612223700683](./assets/image-20240612223700683.png)

![image-20240612224017662](./assets/image-20240612224017662.png)

## 自增长

![image-20240612224235580](./assets/image-20240612224235580.png)

![image-20240612224409638](./assets/image-20240612224409638.png)

![image-20240612224914791](./assets/image-20240612224914791.png)

![image-20240612224755716](./assets/image-20240612224755716.png)

## 索引优化速度

![image-20240612230108508](./assets/image-20240612230108508.png)

![image-20240612225925720](./assets/image-20240612225925720.png)

![image-20240612230002884](./assets/image-20240612230002884.png)

## mysql索引详解

![image-20240612231655124](./assets/image-20240612231655124.png)

![](./assets/image-20240612231454598.png)

![image-20240613152720366](./assets/image-20240613152720366.png)

### 添加索引

![image-20240613152802899](./assets/image-20240613152802899-1718263684086-1.png)

![image-20240613153557249](./assets/image-20240613153557249.png)
添加唯一索引，不重复为0，重复为1

![image-20240613153838231](./assets/image-20240613153838231.png)
添加普通索引的两种方式

![image-20240613154110013](./assets/image-20240613154110013.png)
添加主键索引

### 删除索引

![image-20240613154504975](./assets/image-20240613154504975.png)



**修改索引就是先删除在添加**

### 查看索引

![image-20240613160738920](./assets/image-20240613160738920.png)

### 练习

![image-20240613160840535](./assets/image-20240613160840535.png)

### 索引规则

![image-20240613161034436](./assets/image-20240613161034436.png)



## MySql事务

![image-20240613161213979](./assets/image-20240613161213979.png)

![image-20240613161958422](./assets/image-20240613161958422.png)
事物需求的引出

![image-20240613162541228](./assets/image-20240613162541228.png)

![image-20240613165326479](./assets/image-20240613165326479.png)

```mysql
CREATE TABLE t26(
				id INT,
				`name` VARCHAR(255)
)

INSERT INTO t26 VALUES(100,'tom')
SELECT * FROM t26
INSERT INTO t26 VALUES(200,'jack')
DELETE FROM t26 WHERE id=200

START TRANSACTION
SAVEPOINT a
INSERT INTO t26 VALUES(200,'jack')
SAVEPOINT b
ROLLBACK TO a
```

![image-20240613165102379](./assets/image-20240613165102379.png)
c1去连接数据库后做了一系列的操作，如果不提交，c2是有一些操作看不到的，这涉及到了隔离级别的问题

![image-20240613170315810](./assets/image-20240613170315810.png)
需要innodb的存储引擎才可以使用
![image-20240613170822468](./assets/image-20240613170822468.png)

set autocommit=off也可开启事务

## 事务的隔离级别

隔离级别是和事务相关的，离开事务就不要谈隔离级别

![image-20240613171127385](./assets/image-20240613171127385.png)

![image-20240613171703547](./assets/image-20240613171703547.png)

![image-20240613171747050](./assets/image-20240613171747050.png)
四种隔离级别
Mysql隔离级别定义了**事务与事务之间的隔离程度**
加锁：表正在被另一个事物操作的时候，就不会去操作了。这个就叫加锁

### 隔离级别演示

![image-20240614132958593](./assets/image-20240614132958593.png)

![image-20240614133443300](./assets/image-20240614133443300.png)
执行SELECT @@tx_isolation; 可以查看当前的隔离级别。当前这个隔离级别是“可重复读的”（默认)

![image-20240614134021191](./assets/image-20240614134021191.png)
隔离级别设置成Read Uncommitted

![image-20240614134607414](./assets/image-20240614134607414.png)
启动事务

![image-20240614141100837](./assets/image-20240614141100837.png)

![image-20240614155524895](./assets/image-20240614155524895.png)
加锁后就会卡住，等好边提交完才会继续向下执行

![image-20240614155821311](./assets/image-20240614155821311.png)

![image-20240614155911314](./assets/image-20240614155911314.png)
也可以在配置文件中设置默认的隔离级别
![image-20240614155951241](./assets/image-20240614155951241.png)

![image-20240614160103878](./assets/image-20240614160103878.png)

![1718352121348](./assets/1718352121348.png)
原子性：像这里要么都成功，要么都失败！

### 练习

![image-20240614160604398](./assets/image-20240614160604398.png)

## 存储引擎

![image-20240614161258764](./assets/image-20240614161258764.png)

![image-20240614162042461](./assets/image-20240614162042461.png)

![image-20240614162104250](./assets/image-20240614162104250.png)

![image-20240614163108678](./assets/image-20240614163108678.png)

![image-20240614163449022](./assets/image-20240614163449022.png)

![image-20240614163530991](./assets/image-20240614163530991.png)

![image-20240614164615769](./assets/image-20240614164615769.png)

![image-20240614164820571](./assets/image-20240614164820571.png)

## 视图

![image-20240614165235554](./assets/image-20240614165235554.png)

![image-20240614165253682](./assets/image-20240614165253682.png)

![image-20240614170039061](./assets/image-20240614170039061.png)

![image-20240614170110110](./assets/image-20240614170110110.png)

![image-20240614170159281](./assets/image-20240614170159281.png)

![image-20240614170329960](./assets/image-20240614170329960.png)

![image-20240614170729938](./assets/image-20240614170729938.png)

![image-20240614171623591](./assets/image-20240614171623591.png)

![image-20240614172413516](./assets/image-20240614172413516.png)

![image-20240614172730850](./assets/image-20240614172730850.png)

![image-20240614173026751](./assets/image-20240614173026751.png)

## mysql用户管理

![image-20240614173352177](./assets/image-20240614173352177.png)

![image-20240614173420737](./assets/image-20240614173420737.png)

![image-20240614174222904](./assets/image-20240614174222904.png)

![image-20240614174242429](./assets/image-20240614174242429.png)

![image-20240614175434283](./assets/image-20240614175434283.png)

### 设置权限 

![image-20240614175552360](./assets/image-20240614175552360.png)


![image-20240614175721750](./assets/image-20240614175721750.png)

![image-20240614180211814](./assets/image-20240614180211814.png)

![image-20240614180327514](./assets/image-20240614180327514.png)

![image-20240614180427758](./assets/image-20240614180427758.png)

![image-20240614181205376](./assets/image-20240614181205376.png)

![image-20240614181807385](./assets/image-20240614181807385.png)

![image-20240614182352170](./assets/image-20240614182352170.png)

![image-20240614182549330](./assets/image-20240614182549330.png)

![image-20240614182837015](./assets/image-20240614182837015.png)

### Mysql作业练习

![image-20240614183220942](./assets/image-20240614183220942.png)

![image-20240615141615480](./assets/image-20240615141615480.png)

![image-20240615141859956](./assets/image-20240615141859956.png)

![image-20240615142036468](./assets/image-20240615142036468.png)

![image-20240615142217267](./assets/image-20240615142217267.png)

![image-20240615144938340](./assets/image-20240615144938340.png)

![image-20240615145433072](./assets/image-20240615145433072.png)

![image-20240615145628331](./assets/image-20240615145628331.png)

![image-20240615145830265](./assets/image-20240615145830265.png)

![image-20240615145859052](./assets/image-20240615145859052.png)

![image-20240615145944596](./assets/image-20240615145944596.png)

![image-20240615150332302](./assets/image-20240615150332302.png)

![image-20240615151016193](./assets/image-20240615151016193.png)

![image-20240615151118091](./assets/image-20240615151118091.png)

![image-20240615151413679](./assets/image-20240615151413679.png)

![image-20240615151500410](./assets/image-20240615151500410.png)

![image-20240616091228359](./assets/image-20240616091228359.png)

![image-20240616091428826](./assets/image-20240616091428826.png)

![image-20240616091713848](./assets/image-20240616091713848.png)

![image-20240616091936055](./assets/image-20240616091936055.png)

![image-20240616092133709](./assets/image-20240616092133709.png)

![image-20240616092301291](./assets/image-20240616092301291.png)

![image-20240616092536272](./assets/image-20240616092536272.png)

![image-20240616092600823](./assets/image-20240616092600823.png)

![image-20240616092822142](./assets/image-20240616092822142.png)

![image-20240616093221075](./assets/image-20240616093221075.png)

![image-20240616093501137](./assets/image-20240616093501137.png)

![image-20240616094425065](./assets/image-20240616094425065.png)

技术就是窗户纸，捅破了往里看发现什么都没有，捅不破就觉得它很神秘

![image-20240616094715455](./assets/image-20240616094715455.png)

![image-20240616094750725](./assets/image-20240616094750725.png)

![image-20240616095154344](./assets/image-20240616095154344.png)

![image-20240616095345884](./assets/image-20240616095345884.png)

![image-20240616095710155](./assets/image-20240616095710155.png)

![image-20240616100046361](./assets/image-20240616100046361.png)

![image-20240616100314153](./assets/image-20240616100314153.png)

![image-20240616100435620](./assets/image-20240616100435620.png)

