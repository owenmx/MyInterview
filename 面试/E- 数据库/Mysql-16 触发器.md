#### 基本语法

```mysql
delimiter $$
CREATE TRIGGER 触发器名 BEFORE|AFTER 触发事件
ON 表名 FOR EACH ROW
BEGIN
        执行语句列表
END
$$


### 例子
CREATE TABLE account (acct_num INT, amount DECIMAL(10,2));
INSERT INTO account VALUES(137,14.98),(141,1937.50),(97,-100.00);


DROP TRIGGER IF EXISTS upd_check;
delimiter $$
CREATE TRIGGER upd_check BEFORE UPDATE ON account FOR EACH ROW
begin
		IF OLD.amount = 100 THEN
		SET NEW.amount = 20;
	  ELSEIF NEW.amount > 100 THEN
		SET NEW.amount = 100;
		END IF;
end $$

.
select * from account;
update account set amount=100 where acct_num=137;
```





#### Mysql触发器之 NEW与OLD解析

mysql触发器中，NEW和OLD关键字表示触发器所在表中，触发了触发器的那一行数据：

具体的：

在INSERT型触发器中，NEW用来表示将要BEFORE或者已经AFTER插入的新数据。**old表示插入之前的值，new表示新插入的值；**

在UPDATE型触发器中，OLD用来表示将要或已经被修改的原数据。

在DELETE型触发器中，OLD用来表示将要或已经被删除的原数据。

使用方法：NEW.columnName（columnName为相应数据的某一列名）

另外，OLD是只读的，而NEW则可以在触发器中使用SET复制，这样不会触发触发器，造成循环调用。





#### INSERT触发器

在 INSERT 语句执行之前或之后响应的触发器。

使用 INSERT 触发器需要注意以下几点：

-   在 INSERT 触发器代码内，可引用一个名为 NEW（不区分大小写）的虚拟表来访问被插入的行。
-   在 BEFORE INSERT 触发器中，NEW 中的值也可以被更新，即允许更改被插入的值（只要具有对应的操作权限）。
-   对于 AUTO_INCREMENT 列，NEW 在 INSERT 执行之前包含的值是 0，在 INSERT 执行之后将包含新的自动生成值。





#### UPDATE触发器

在 UPDATE 语句执行之前或之后响应的触发器。

使用 UPDATE 触发器需要注意以下几点：

-   在 UPDATE 触发器代码内，可引用一个名为 NEW（不区分大小写）的虚拟表来访问更新的值。
-   在 UPDATE 触发器代码内，可引用一个名为 OLD（不区分大小写）的虚拟表来访问 UPDATE 语句执行前的值。
-   在 BEFORE UPDATE 触发器中，NEW 中的值可能也被更新，即允许更改将要用于 UPDATE 语句中的值（只要具有对应的操作权限）。
-   OLD 中的值全部是只读的，不能被更新。


注意：当触发器设计对触发表自身的更新操作时，只能使用 BEFORE 类型的触发器，AFTER 类型的触发器将不被允许。





#### DELETE触发器

在 DELETE 语句执行之前或之后响应的触发器。

使用 DELETE 触发器需要注意以下几点：

-   在 DELETE 触发器代码内，可以引用一个名为 OLD（不区分大小写）的虚拟表来访问被删除的行。
-   OLD 中的值全部是只读的，不能被更新。