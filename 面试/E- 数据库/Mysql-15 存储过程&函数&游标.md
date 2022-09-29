#### 什么是存储过程

存储过程是一组为了完成特定功能的SQL语句集合，使用存储过程的目的是将常用的复杂的工作预先用SQL语句写好并用一个指定名称存储起来，这个过程经编译和优化存储在数据库服务器中，因此称为存储过程。

存储过程的特点：

+   封装性
+   可增强的SQL语句的功能和灵活性
+   可减少网络流量
+   高性能
+   提高数据库的安全性和数据完整性
+   使数据独立



#### 创建存储过程

```mysql
CREATE PROCEDURE <过程名> ( [过程参数[,…] ] ) <过程体>
[过程参数[,…] ] 格式
[ IN | OUT | INOUT ] <参数名> <类型>
```

+   过程名，存储过程的名称

+   过程体，在 MySQL 中，服务器处理 SQL 语句默认是以分号作为语句结束标志的。然而，在创建存储过程时，存储过程体可能包含有多条 SQL 语句，这些 SQL 语句如果仍以分号作为语句结束符，那么 MySQL 服务器在处理时会以遇到的第一条 SQL 语句结尾处的分号作为整个程序的结束符，而不再去处理存储过程体中后面的 SQL 语句，这样显然不行。为解决以上问题，通常使用 **DELIMITER** 命令将结束命令修改为其他字符。语法格式如下：

    ```mysql
    DELIMITER $$
    BEGIN
    ...
    END $$
    
    ```

    

### 变量

#### 局部变量

```mysql
局部变量自定义，在begin/end块中有效
# 声明变量 declare var_name type [default varvalue];
# declare nick_name varchar(20) default 'chan';
```

#### 用户变量

```mysql
# 语法
# @var_name
# set @var_name=12;
```

#### 会话变量

```mysql
# @@session.var_name
```

#### 全局变量

```mysql
# @@global.var_name
```

## 出参入参

```mysql
DROP PROCEDURE IF EXISTS my_func;
DELIMITER $$
CREATE PROCEDURE my_func(in u_id int,inout u_name VARCHAR(20))
BEGIN
	SELECT concat(father," ",u_name) into u_name from users where users.u_id=u_id;
END $$

set @u_name="Jack";
CALL my_func(0,@u_name);
SELECT @u_name;
```



## 流程控制

#### IF ELSE THEN语句

```mysql
IF score >= 90 THEN SET s_level='A'; # 用IF开始
ELSEIF score >= 80 THEN SET s_level='B';
ELSE SET s_level='C';
END IF; # 用END借结束
```

#### CASE语句

```mysql
DROP PROCEDURE if EXISTS my_foo;
DELIMITER $$
CREATE PROCEDURE my_foo(in score int)
BEGIN
    declare idx int DEFAULT 0;
		declare level VARCHAR(20) DEFAULT "F";
		set idx=score;
		
		CASE  # 开始CASE
		when idx >= 90 THEN set level="A";
		when idx >= 80 THEN set level="B";
		when idx >= 70 THEN set level="C";
		else set level="D";
		END CASE; # 结束CASE
		SELECT level;
END $$

CALL my_foo(89);
```

或者这种精确匹配

```mysql
DROP PROCEDURE if EXISTS my_foo;
DELIMITER $$
CREATE PROCEDURE my_foo(in score int)
BEGIN
    declare idx int DEFAULT 0;
		declare level VARCHAR(20) DEFAULT "F";
		set idx=score;
		
		CASE idx
		when 0 THEN set level="A";
		when 1 THEN set level="B";
		when 2 THEN set level="C";
		else set level="D";
		END CASE;
		SELECT level;
END $$

CALL my_foo(2);
```

#### 循环语句

**while --- end while**

基本语法格式：

```mysql
while 条件 do
循环体
end while;

while idx < 5 do
	SELECT level;
	set idx=idx+1;
end while;
```

**repeat ... endrepeat**

```mysql
repeat
    --循环体
until 循环条件  
end repeat;

REPEAT
SELECT idx;
SET idx=idx+1;
UNTIL idx >= 10 
END REPEAT;
```

**loop ... end loop**

```mysql
label: LOOP

    IF idx = 3 THEN
        SELECT idx;
        ITERATE # ITERATE 的作用和continue的作用类似
    END IF;

    IF idx = 5 THEN
        LEAVE label;
    END IF;

END LOOP label;
```



#### 存储函数

基本语法结构：

```mysql
CREATE FUNCTION func_name ([param_name type[,...]])
RETURNS type
[characteristic ...] 
BEGIN
	routine_body
END;

### 例子
DROP FUNCTION if EXISTS my_func;
DELIMITER $$
CREATE FUNCTION my_func(score int) RETURNS VARCHAR(20)
BEGIN
	RETURN (SELECT father from users LIMIT 1);
END $$

### 调用
SELECT my_func(2);
```



#### 游标

游标就是把数据按照指定要求提取出相应的数据集，然后逐条进行数据处理。

**声明游标**

```mysql
declare c_name cursor for select_statement;
declare continue handler for not found set done=1; # 定义结束标志
```

**游标OPEN语句**

```mysql
open c_name;
```

**游标FETCH语句**

```mysql
fetch c_name into f_name; # 读取数据
```

**游标close语句**

```mysql
CLOSE c_name;
```

**例子**

```mysql
# 类似于迭代器
DROP PROCEDURE if EXISTS my_foo;
DELIMITER $$
CREATE PROCEDURE my_foo()
BEGIN
    declare id_sum int DEFAULT 0;
		declare p_id int DEFAULT 0;
		declare done int DEFAULT 0;
		
		declare c_id cursor for select u_id from users; # 定义游标
		declare continue handler for not found set done=1; # 定义结束标志
		
		OPEN c_id;
		
		repeat
		fetch c_id into p_id;
		set id_sum=id_sum+p_id;
		until done=1 # 结束循环
		end repeat;
		
		select id_sum;
		CLOSE c_id;
END $$

CALL my_foo();
```

