### 数据库《mysql必知必会笔记》

```mysql
create database demo;  # 创建数据库
# drop database demo;  # 删除数据库
show databases;

# 创建表
create table demo.test
(
	barcode text,
    goodsname text,
    price int
);

describe demo.test;  -- 查看表的结构
show tables;  -- 显示所有的表、

alter table demo.test
add column itemnumber int primary key auto_increment;
-- 向表中添加一列数据
insert into demo.test
(barcode,goodsname,price)
values('0001','本',3);


create database demo2;
-- 使用数据库
use demo2;

show databases;
show tables;
show columns from demo2;

create database demo3;
use demo3;
create table demo3.importhead
(
listnumber INT,
supplierid INT,
stocknumber INT,
-- 在字段importype定义为INT后面，按照mysql创建表的语法，加了默认值1
importype INT default 1,
quantity decimal(10,3),
importvalue decimal(10,2),
recorder INT,
recordingdate datetime
);

describe importhead;

insert into demo3.importhead
(
	listnumber,
    supplierid,
    stocknumber,
    quantity,
    importvalue,
    recorder,
    recordingdate
)
values
(
3456,
1,
1,
10,
100,
1,
'2021-4-22'
);
select * from demo.importhead;

create database demo4;  -- 创建数据库
create table demo4.customers  -- 创建表
(
cust_id int not NULL auto_increment,
cust_name char(50) not null,
cust_address char(50) NULL,
cust_city char(50) null,
cust_state char(50) null,
cust_zip char(10) null,
cust_country char(50) null,
cust_contact char(50) null,
cust_email char(255) null,
primary key(cust_id)
)engine=InnoDB;

select * from demo4.customers;

create table demo4.ordertimes
(
order_num int not null,
order_item int not null,
prod_id char(10) not null,
quanity int not null,
iten_price decimal(8,2) not null,
primary key(order_num,order_item)
)engine=InnoDB;

describe demo4.ordertimes;

create table demo4.orders
(
order_num int not null auto_increment,
order_date datetime not null,
cust_id int not null,
primary key(order_num)
)engine=InnoDB;

create table demo4.products
(
prod_id char(10) not null,
vent_id int not null,
prod_name char(255) not null,
prod_price decimal(8,2) not null,
prod_desc text null,
primary key(prod_id)
)engine=InnoDB;

create table demo4.vendors
(
vend_id int not null auto_increment,
vend_name char(50) not null,
vend_address char(50) null,
vend_city char(50) null,
vend_state char(5) null,
vend_zip  char(10) null,
vend_country char(50) null,
primary key(vend_id)
)engine=InnoDB;

create table demo4.productnotes
(
note_id int not null auto_increment,
prod_id char(10) not null,
note_date datetime not null,
note_text text null,
primary key(note_id),
fulltext(note_text)
)engine=MyISAM;

describe demo4.products;
describe demo4.customers;
describe demo4.orders;
describe demo4.ordertimes;

show databases;  -- 显示虽有的数据库
use demo4;  -- 必须先使用use才能对数据库进行操作
show tables;  -- 显示数据库中的表的信息
show columns from customers;  -- 显示某个表的列的信息


-- 检索数据
-- select语句
-- 1、检索单个列
select prod_name from products;

-- 2、检索多个列
select prod_name,prod_id,prod_price from products;

-- 3、检索所有列
select * from products;

-- 4、检索不同的行
describe products;
select vent_id from products;  -- 它会返回所有的行，包括重复行


-- 5、去掉重复的行，使用dinstinct
select dinstinct vent_id from products;

-- 6、限制结果，select语句返回所有匹配的行，为了返回第一行或者第几行，可以使用limit
select prod_name from products
limit 5;  -- 表示返回不超过5行

-- 7、可以指定检索的开始和需要的行数
select prod_name
from products
limit 5,5;  -- 第一个5是开始的行数，第二个为要检索的行数，即从第5行开始，检索5行


-- 8、使用完全限定的表名，即在要检索的数据之前加上表名限定
select products.prod_name from products;


-- 排序检索数据
-- 1、排序数据
-- 对于实际的应用中，我们需要将检索出来的数据按照某种方式进行排序
-- 可以使用order by语句进行排序
select prod_name from products order by prod_name;  -- 按照产品名称对检索的数据排序

-- 2、按照多个列的数据进行排序
select prod_id,prod_name,prod_price 
from products
order by prod_price,prod_name;
-- 上述语句根据价格、名称、id检索出来的数据再按照价格和名称排序显示alter

-- 3、指定排序方向
-- 默认情况下，是按照升序排序的，我们可以指定降序排序
select prod_id,prod_name,prod_price
from products
order by prod_price desc;  -- 按照价格从高到低降序排列

-- 4、如果想对多个列升序或者降序排列，每一列都要指定顺序
select prod_id,prod_name,prod_price
from products
order by prod_price desc,prod_name desc;

-- 5、使用order by和limit可以检索出一个列中最大或者最小的值
select prod_price 
from products
order by prod_price
limit 1;  -- 找出价格最高的产品

-- 过滤数据：只检索所需数据需要指定搜索条件
-- 1、使用where语句
select prod_name,prod_price
from products
where prod_price = 2.50; -- 找出价格为2.5的所有的列

-- 注意：当使用order by和where语句的时候，需要将order by放到where语句之后，否则会出错
-- 2、where子句常见的操作符
-- = 等于
-- <> 不等于
-- != 不等于
-- between 在两者之间

-- 3、检查单个值
select prod_name,prod_price
from products
where prod_name = 'fuses';  -- 检索名称为fuses的列

select prod_name,prod_price
from producrs
where prod_price < 10;  -- 检索价格低于10

select prod_name,prod_price
from products
where prod_price <= 10;

-- 4、不匹配检查
-- 检索出不是由供应商1003制造的所有产品
select vent_id,prod_name
from products
where vent_id<> 1003;

select vent_id,prod_name
from products
where vent_id != 1003;

-- 5、范围值检查
select prod_price,prod_name 
from products
where prod_price between 5 and 10; -- 查找价格在5到10之间的数据alter

-- 6、空值检查
-- null:无值，它与字段包含0、空字符串或者仅仅包含空格不同
select prod_name
from products
where prod_price is NULL;

-- 数据过滤
-- 组合where子句
-- 1、AND操作符：通过对不止一列进行过滤
select prod_id,prod_price,prod_name
from products
where vent_id =1003 and prod_price <=10;
-- 上述语句检索由供应商1003制造并且价格小于等于10的产品名称和价格

-- 2、or操作符：匹配任意条件
select prod_name,prod_price
from products
where vent_id = 1002 or vent_id = 1003;

-- 3、计算次序
-- where可以包含任意数目的and和or操作符，允许两者结合以进行复杂和高级的过滤
-- 例如：需要检索价格为10以上的由1002或者1003供应的产品
select prod_name,prod_price 
from products
where vent_id=1002 or vent_id = 1003 and prod_price >=10;
-- 这里计算次序不对，检索出来的数据价格有小于10的，为了保证次序，需要添加（）
select prod_name,prod_price 
from products
where (vent_id=1002 or vent_id = 1003) and prod_price >=10;

-- 4、In操作符：指定条件范围，范围中的每一个条件都可以进行匹配
select prod_name,prod_price 
from products
where vent_id in(1002,1003)
order by prod_name;
-- IN的功能与or类似，但IN操作符执行清单速度更快

-- 5、NOT操作符：否定它之后的所跟的任何条件
select prod_name,prod_price
from products
where vent_id not in(1002,1003)
order by prod_name;


-- 用通配符进行过滤
-- 通配符：用来匹配值的一部分特殊字符
-- 搜索模式：由字面值、通配符或者两者组合构成的搜索条件
-- 为了搜索子句中使用通配符，必须使用like操作符，like指示mysql后面跟的
-- 搜索模式利用通配符匹配，而不是直接相等匹配进行比较

-- 1、百分号（%）通配符：表示任何字符出现任意次数
-- 为了找出所有以词jet开头的产品，可以使用下面的语句
select prod_name,prod_id
from products
where prod_name like 'jet%';

-- 通配符可以在搜索模式中任意位置使用，并且可以使用多个通配符
select prod_id,prod_name
from products
where prod_name like '%anvil%';

-- 通配符还可以放在中间
select prod_id,prod_name
from products
where prod_name like 's%e';
-- 注意：注意尾空格，当保存词anvil时，若后面多了一个空格，则'%anvil'将不会匹配
-- 注意：通配符虽然能匹配任何字符，但是null例外

-- 2、下划线（_)通配符：用途与百分号一样，但是只能用来匹配单个字符
select prod_id,prod_name
from products
where prod_name like '_ ton anvil';  -- 只会匹配一个字符，例如2 ton anvil

-- 用正则表达式进行搜索
/*
正则表达式：作用就是进行文本匹配
*/

-- 1、基本字符匹配
select prod_name
from products
where prod_name regexp '1000'
order by prod_name;
/*
regexp后面跟的内容按正则表达式处理
*/

-- 2、进行or匹配
select prod_name
from products
where prod_name regexp '1000|2000'
order by prod_name;
/*
功能类似于or，多个or条件可以写为'1000|2000|3000'
*/
 -- 3、匹配几个字符之一
 /*
 只想匹配特定的字符，可以通过指定一组[和]括起来的字符来完成
 */
 select prod_name
 from products
 where prod_name regexp '[123] Ton'
 order by prod_name;
 /*
 上述语句使用正则表达式[123]，定义一组字符，他的意思是匹配1或者2或者
 3
 */
 
 -- 4、匹配范围
 -- 集合可以用来匹配的一个或者多个字符
 select prod_name
 from products
 where prod_name regexp '[1-5] Ton'
 order by prod_name;
 /*
 这里使用了正则表达式[1-5]，表示一个范围，匹配1到5
 */
 
 -- 5、匹配特殊字符
 /*
 为了匹配特殊字符，必须使用\\为前导，\\-表示查找-，\\.表示alter查找.
 */
 select vend_name
 from vendors
 where vend_name regexp '\\.'
 order by vend_name;  -- 找出名字中后面包含.的数据
 -- 注意：匹配\时,需要在前面加上\\，即\\\表示匹配\
 
 
 -- 创建计算字段
 /*
 我们经常需要检索直接从数据库中检索出转换、计算或格式化过的数据
 而不是检索出数据，计算字段是运行在select语句内创建的
 字段：基本上与列的意思相近，经常互换使用
 */
 
 -- 1、拼接字段
 -- 拼接：将值联结到一起构成单个值
 -- concat（）联结两个列的函数
 select concat(vend_name,' (',vend_country,')')
 from vendors
 order by vend_name;
/*
上述语句将供应商的名称与国家联结成一个字符串
*/

-- 还可以先通过RTrim（）函数来删除数据右侧多余的空格来整理数据
select concat(rtrim(vend_name),'(',vend_country')')
from vendors
order by vend_name;

-- 2、使用别名：select拼接得到一个新的字段，但是它没有名字，可以通过起别名
-- 别名：是一个字段或者值的替换名，用as关键字赋予
select concat(RTrim(vend_name),'(',Rtrim(vend_country),')')
as vend_title
from vendors
order by vend_name;
/*
上述语句的作用是将供应商的名称和国家组成一个新的字段，并命名为
vend_title，并按照供应商名称排序
*/


-- 3、执行算数运算
-- 计算字段的另一个作用就是对检索出来的数据执行算术计算
-- 汇总物品的价格
select prod_id,quantity,item_price,
quantity * item_price as expend_price
from ordertimes
where order_num=2005;
# 以上句子实现的是，检索产品编号为2005的产品，并计算汇总的价格


-- 使用数据处理函数
/*
函数的主要类别有：文本函数、数值函数、日期和时间函数、系统函数
*/

-- 1、文本处理函数
-- 之前讨论过的RTrim（）去除字符串的空格就是文本函数，这里介绍Upper()函数，将文本转换为大写
select vend_name,Upper(vend_name) as vend_name_upcase
from vendors
order by vend_name;
-- 上述语句执行搜索vendors表中的名单，并转换为大写之后，按名字排序
/*
常见的文本处理函数：left（）--返回串左边的字符
length（）：返回串的长度
locate（）：找出串的一个字串
lower（）：将串转换为小写
Ltrim（）：去掉串左边的空格
Right（）：返回串右边的字符
soundex（）：返回串的soundex函数
*/ 

select cust_name,cust_contact
from customers
where soundex(cust_contact)=soundex('Y Lie');
-- 上述语句将会查找出所有读音和Y Lie相同的cust_contact


-- 2、日期和时间处理函数
-- 不管是插入、或者更新表值还是用where过滤子句，日期格式必须为yyyy-mm-dd
select cust_id,order_num
from orders
where order_date = '2005-09-01';
-- 上述语句会出现的问题是：当日期是2005-09-01 11：30：24时，就会检索失败
-- 解决办法：使用Date（）函数，它仅指示日期部分
select cust_id,order_num
from orders
where Date(order_date)='2005-09-01';

-- 当需要检索某一个月的产品时，做法
select cust_id,order_num
from orders
where Date(order_date) between '2005-09-01' and '2005-09-30';
-- 简单的做法是
select cust_id,order_num
from orders
where Year(order_date)=2005 and Month(order_date)=9;


-- 3、数值处理函数
/*
常见的数值处理函数：
Abs（）：返回一个数的绝对值
Cos（）：返回一个角度的余弦
Exp（）：返回一个数的指数值
*/


-- 汇总数据
-- 1、聚集函数：运行在行组上，计算和返回单个值的函数
/*
常见的函数：
AVG（）：返回某列的平均值
count（）：返回某列的行数
MAX（）：返回某列的最大值
MIN（）：返回某列的最小值
sum（）：返回某列的和
*/

-- AVG（）函数：对表中行数计数，并计算特定列值之和，求出该列的平均值
select avg(prod_price) as avg_price
from products;

-- avg()函数也可以确定特定列或行的平均值
select avg(prod_price) as avg_price
from products
where vend_id = 1003;

-- count（）函数：
/*
两种使用方式：
1、使用count（*）对表中行的数目进行计数，不管列表包含的的是空值（null）还是非空值
2、使用count（column）对特定列中具有值的行进行计数，忽略null值
*/
select count(*) as num_cust
from products;
-- 上述语句对所有的行进行统计

select count(cust_email) as num_cust
from customers;
-- 上述语句只对有电子邮件的列统计

-- max（）函数：返回指定列中的最大值，需要指出列
select max(prod_price) as max_price
from products;

-- min()函数与max（）函数相反

-- sum（）函数
select sum(quantity) as items_ordered
from ordertimes
where order_num=2005;


-- 2、聚集不同的值
/*
以上的聚集函数都可以如下使用：
1、对所有的行执行计算，指定all参数或者不给参数（因为all是默认行为）
2、只包含不同的值，可以用dinstinct
*/

-- 3、组合聚集函数
select count(*) as num_items,
min(prod_price) as price_min,
max(prod_price) as price_max,
avg(prod_price) as price_avg
from products;


-- 分组数据
-- 1、创建分组
select vend_id,count(*) as num_prods
from products
group by vend_id;

-- 2、过滤分组
select cust_id,count(*) as orders
from orders
group by cust_id
having count(*) > 2;
/*
having和where的差距：where在数据分组前进行过滤，having在数据分组后进行过滤
*/

-- 3、分组和排序
select order_num,sum(quantity * item_price) as ordertotal
from ordertimes
group by order_num
having sum(quantity * item_price) >=50;
-- 检索出订单价格大于等于50的订单号和总计订单价格，这里得到的数据只是按照order_num排好序

select order_num,sum(quantity * item_price) as ordertotal
from ordertimes
group by order_num
having sum(quantity * item_price) >=50
order by ordertotal; 
-- 相比于上一条语句，我们在根据order_num拍好序之后，又按照ordertotal排序


-- 使用子查询
-- 数据库的表都是关系表，当我们需要进行查询某一个信息时，可能要检索多个表，我们需要将多个查询组合起来
select cust_name,cust_contact
from customers
where cust_id in(select cust_id 
					from orders
                    where order_num in (select order_num
										from orderitems
                                        where prod_id = 'TN2'));
                                        
-- 4、作为计算字段使用子查询


-- 联结表
/*
关系表：关系表的设计就是保证把信息分解成多个表，一类数据一个表，各表通过常用的值互相关联
外键：外键为某一个表的一列，它包含另一个表的主键值，定义了两个表之间的关系
*/

-- 1、创建联结
-- 方法：规定要联结的表以及他们如何关联
select vend_name,prod_name,prod_price 
from vendors,products
where vendors.vend_id = products.vend_id
order by vend_name,prod_name;
-- 上述语句将表vendors和products关联起来，关联句子就是vend_id在两个表中都一致，注意where子句必不可少，否则将会出现笛卡尔积，将第一个表中的每一行于第二个表中的每一行匹配


-- 内部联结
select vend_name,prod_name,prod_price
from vendors inner join products
on vendors.vend_id = products.vend_id;
-- 推荐使用内部联结

-- 联结多个表
select prod_name,vend_name,prod_price,quantity
from ordertimes,products,vendors
where products.vend_id = vendors.vend_id
and ordertimes.prod_id = products.prod_id
and order_num=2005;


-- 创建高级联结
-- 1、使用表别名
/*
优点：
1、缩短sql语句
2、允许单条select语句中多次使用相同的表
*/

select cust_name.cust_contact
from customers as c,orders as o,ordertimes as oi
where c.cust_id = o.cust_id
and oi.order_num = o.order_num
and prod_id = 'TNT2';

-- 2、使用不同类型的联结
-- 2.1、自联结
-- 对于某个产品（id为DTNTR），发现它有问题，想要查询生产该商品的供应商生产的其他产品的情况
select prod_id,prod_name
from products
where vend_id =(select vend_id 
				from products
                where prod_id = 'DTNTR');
                
                
-- 使用自联结实现
select p1.prod_id,p1.prod_name
from products as p1,products as p2
where p1.vend_id = p2.vend_id
and p2.vend_id = 'DTNTR';



-- 组合查询
/*
mysql允许执行多条查询（多个select），并将结果作为单个查询结果返回，这些组合查询通常称为并或者复合查询
使用情况：
1、在单个查询中从不同的表返回类似结构的数据
2、对单个表执行多个查询，按单个查询返回数据
*/

-- 1、创建组合查询：可以使用union操作符来组合数条sql查询
-- 需要查询价格小于等于5的所有物品的列表，而且还想查询包括供应商1001和1002生产的所有物品（不考虑价格）
select vend_id,prod_id,prod_price
from products
where prod_price <= 5
union
select vend_id,prod_id,prod_price
from products
where vend_id in(1001,1002);

/*
union规则：
1、必须由两条或两条语句以上的select语句组成，语句之间使用关键字union分隔
2、union中的每一个查询必须包含相同的列、表达式、或者聚集函数
3、列数据必须兼容
*/

-- 2、union包含或者取消重复的行
/*
union默认会去除重复的行，如果需要，可以改变，如果想匹配所有的行，可以使用union all，而不是union
*/
select vend_id,prod_id,prod_price
from products
where prod_price <= 5
union all
select vend_id,prod_id,prod_price
from products
where vend_id in(1001,1002);

-- 对组合查询结果进行排序
-- select语句的输出用order by子句排序，在使用union组合查询时，只能使用一条order by语句，而且必须出出现在最后一条select之后
select vend_id,prod_id,prod_price
from products
where prod_price <= 5
union
select vend_id,prod_id,prod_price
from products
where vend_id IN(1001,1002)
order by vend_id,prod_price;


-- 全文本搜索
-- 进行全文本搜索
select note_text
from productnotes
where match(note_text) against('rabbit');
/*
match(note_text)指示mysql针对指定的列进行搜索，against（'rabbit')指定词rabbit作为搜索文本，包含rabbit的行将会被返回
使用完整的match（）说明：传递给match（）的值必须与fulltext（）定义中相同，如果指定多个列，则必须列出他们
搜索不区分大小写：除非使用BINARY方式，否则全文搜索不区分大小写
*/

-- 上述语句可以使用like实现
select note_text
from productnotes
where note_text like '%rabbit%';
-- 但是得到的文本不是按照词出现的顺序排列的，而全文搜索会自动进行排序

-- 使用查询扩展
/*
当我们进行全文搜索的时候得到的只是我们想要查找的词和文本，而使用查询扩展不仅返回我们需要的词，还会包含相关词的文本主要的步骤分为三步：
1、首先，进行一个基本的全文搜索，找出与搜索条件匹配的所有行
2、其次，mysql检查这些匹配行并选择有用的词
3、在进行全文搜索，不仅包含条件匹配的词，还包括有用的词
*/
select note_text
from productnotes
where match(note_text) against('anvil' with query expansion);


-- 插入数据
/*
INSERT用来插入数据，插入可以用几种方式使用：
1、插入完整的行
2、插入行的一部分
3、插入多行
4、插入某些查询的结果
*/

-- 1、插入完整的行
-- 把数据插入表中的最简单的方法是使用基本的insert方法，它要求指定表名和被插入到新行中的值
insert into customers
values(null,
'Peo E.Lapew',
'100 main street',
'Los angles',
'ca',
'90046',
'usa',
null,
null);

-- 插入语句一般不会输出，上述语句一般不安全，可以采用如下的方法
insert into customers(cust_name,
cust_address,
cust_city,
cust_state,
cust_zip,
cust_country,
cust_contact,
cust_email)
values(null,
'Peo E.Lapew',
'100 main street',
'Los angles',
'ca',
'90046',
'usa',
null,
null);

-- 2、插入多个行
insert into customers(cust_name,
cust_address,
cust_city,
cust_state,
cust_zip,
cust_country)
values(
'Peo E.Lapew',
'100 main street',
'Los angles',
'ca',
'90046',
'usa',
null,
null);
insert into customers(cust_name,
cust_address,
cust_city,
cust_state,
cust_zip,
cust_country)
values(
'M.martian',
'42 Galaxy way',
'New york',
'NY',
'11213',
'usa');

-- 或者可以改写为：
insert into customers(cust_name,
cust_address,
cust_city,
cust_state,
cust_zip,
cust_country)
values(
'Peo E.Lapew',
'100 main street',
'Los angles',
'ca',
'90046',
'usa',
null,
null),
(
'M.martian',
'42 Galaxy way',
'New york',
'NY',
'11213',
'usa');

-- 插入检索出来的数据
insert into customers(cust_name,
cust_address,
cust_city,
cust_state,
cust_zip,
cust_country)
select cust_name,
cust_address,
cust_city,
cust_state,
cust_zip,
cust_country
from custnew;  
-- 从表custnew中检索出来的数据插入到customers中


-- 更新和删除数据
-- 1、使用update语句，可以更新表中特定列，更新表中所有行
/*
注意：不要省略where，如果稍不注意就会更新整个表
基本的update语句由三部分组成：
1、要更新的表
2、列名和他们的新值
3、确定要更新行的过滤条件
*/
update customers
set cust_email = 'elemer@fudd.com'
where cust_id =1005;
-- 上述语句将cusyomers表中id为1005的email修改，如果不指定where的话，将会把表中所有的email都修改

-- 更新多个列
update customers
set cust_name = 'The Fudds',
cust_email='elemer@fudd.com'
where cust_id = 1005;

-- 为了删除某个列的值，可设置他为NULL
update customers
set cust_email=NULL
where cust_id = 1005;

-- 2、删除数据
/*
使用delete语句从一个表中删除数据，可以用两种方式删除：
从表中删除特定的行
从表中删除所有的行
同理：where不要省略，否则将会删除所有的行
*/
delete from customers
where cust_id = 10006;
-- delete删除的是表中的内容，但是不删除表本身


-- 创建和操纵表
-- 1、创建表
/*
两种方式：
1、使用具有交互式创建和管理表的工具
2、表也可以用mysql语句操纵
*/
-- 表创建基础
/*
利用create table创建表，必须给出下列信息：
1、新表的名字，在关键字create table之后给出
2、表列的名字和定义，用逗号分隔开
*/

create table customers
(
cust_id int not null auto_increment,
cust_name char(50) not null
)

-- 2、使用null值
-- 允许null值的列也允许在插入行时不给出该列的值，不允许null值的列不接受该列没有值的行，即在插入和更新行时，该列必须有值


-- 3、主键：主键值必须唯一

-- 4、更新表
-- 可以使用alter table语句，这是更改表的结构，必须给出下面的信息：
/*
1、在alter table之后要给出要更改的表名（该表必须存在，否则出错）
2、所做更改的列表
*/
alter table vendors
add vend_phone char(20);
-- 给表增加一个列为vend_phone的列，必须明确其数据类型

-- 5、删除表
-- drop table即可
drop table customers2;  # 删除表

-- 6、重命名表
-- rename table
rename table customers2 to customers;

-- 可以对多个表进行重命名
rename table backup_customers to customers,
backup_vendors to vendors,
backup_products to products;


-- 使用视图
-- 1、创建视图
/*
视图用create view来创建
使用show_create view viewname 来查看创建视图的语句
使用drop来删除视图，其语法为：drop view viewname;
更新视图时，可以使用drop再用create，也可以直接用create or replace view，如果要更新的视图不存在，则第二条语句会创建一个视图；如果要更新的视图存在，则第二条会替换
*/
create view productcustomers as 
select cust_name,cust_contact,prod_id
from customers,orders,orderitems
where customers.cust_id=orders.cust_id
and orderitems.order_num = orders.order_num;


-- 2、用视图重新格式化检索出的数据
-- 如果经常需要使用下面的语句，则可以创建视图
select Concat(RTrim(vend_name),'(',RTrim(vend_country),')') as vend_title
from vendors
order by vend_name;

-- 创建视图
create view vendorlocation as
select Concat(RTrim(vend_name),'(',RTrim(vend_country),')') as vend_title
from vendors
order by vend_name;

-- 3、用视图过滤不需要的数据
create view customeremaillist as 
select cust_id,cust_name,cust_email
from customers
where cust_email is not null;
-- 检索的时候将没有email的客户过滤

-- 4、使用视图与计算字段
create view orderitemsexpand as 
select order_num,
prod_id,
quantity,
item_price,
quantity * item_price as expanded_price
from orderitems;

-- 使用存储过程
-- 1、使用存储过程
-- mysql执行存储过程的语句为call，call接收存储过程的名字以及需要传递给他们的任意参数
call productpricing(@proicelow,
@pricehigh,
@priceaverage);

-- 2、创建存储过程
create procedure productpricing()
begin
	select avg(prod_price) as priceaverage
	from products;
end;
-- 调用存出过程
call productpricing();  -- 因为存储过程是一个函数，所以调用的时候需要加（）

-- 3、删除存储过程
drop procedure productpricing;
-- 注意这里不加（）

-- 4、使用参数
/*
一般存储过程并不显示结果，而是把结果返回给你指定的变量
*/
create procedure productpricing(
out p1 decimal(8,2),
out ph decimal(8,2),
out pa decimal(8,2)
)
begin
select min(prod_price)
into p1
from products;
select max(prod_price)
into ph
from products;
select avg(prod_price)
into pa
from products;
end;

-- 关键字out指出相应的参数用来从存出过程传出一个值（返回给调用者），mysql支持in（传递给存出过程）、out（从存储过程传出）、INOUT（对存储过程传入和传出）

-- 检查存储过程
-- 创建一个存储过程的create语句，使用show create procedure语句


-- 使用游标
/*
使用简单的select语句，没有办法得到第一行、下一行或者前十行
游标是一个存储在mysql服务器上的数据库查询，它不是一条select语句，而是被该语句检索出来的结果集，在存储了游标之后，应用程序可以根据需要滚动或者浏览器中的数据。
使用游标前的注意事项：
1、使用之前必须声明，这个过程实际上没有检索数据，他只是定义要使用的selectyuju
2、一旦声明以后，必须打开游标使用
3、对于填有数据的游标，根据需要取出各行
4、结束游标使用时，必须关闭游标
*/
-- 1、创建游标：使用declare语句创建
create procedure processsorders(
begin
	declare ordernumbers cursor
    for 
    select order_num from orders;
end;

-- 2、打开游标
open ordernumbers;

-- 3、关闭游标
close ordernumbers;

create procedure processorders()
begin
	declare ordernumbers cursor for
    select order_num from orders;
	
    open ordernumbers;
    
    close oedernumbers;
end;


-- 4、使用游标数据
-- 在一个游标被打开后，可以使用fetch语句分别访问它的每一行，fetch指定检索什么数据，检索出来的数据存储在什么地方
create procedure processorders()
begin
declare o int;
	declare ordernumbers cursor for
    select order_num from orders;
	
    open ordernumbers;
    fetch ordernumbers into o;
    
    close oedernumbers;
end;


-- 使用触发器
-- 触发器就是在某个表更改时发生自动处理

-- 1、创建触发器
/*
创建触发器时，需要给出四条信息：
1、唯一的触发器名
2、触发器关联的表
3、触发器应该响应的活动（delete、insert或者update）
4、触发器何时执行（处理之前或者处理之后）
*/
-- 触发器使用create t'rigger创建
create trigger newproduct after insert on products
for each row select 'product added';  -- 每插入一行或者多行信息到product之中，product added就会显示一次
-- 上述语句创建了名为trigger的触发器，触发器仅支持表，不支持视图

-- 2、删除触发器
drop trigger newproduct;
```

