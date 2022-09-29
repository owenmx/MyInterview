```mysql
SHOW TABLES;
DROP TABLE tb_dept1;

DROP TABLE IF EXISTS department ;

CREATE TABLE department (
    id INT(11) PRIMARY KEY,
		name VARCHAR(22) NOT NULL,
		level int
)ENGINE=INNODB;

DROP TABLE IF EXISTS leader;
CREATE TABLE leader (
    id INT(11) PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) UNIQUE,
    deptId INT(11),
    salary FLOAT DEFAULT 20.0,
		CHECK (salary > 0),
    CONSTRAINT fk_emp_dept1 FOREIGN KEY(deptId) REFERENCES department(id)
);

ALTER TABLE leader CHANGE COLUMN salary
salary float DEFAULT NULL;

SHOW CREATE TABLE leader;
DESC leader;
ALTER TABLE leader DROP INDEX name;

ALTER TABLE leader DROP FOREIGN KEY fk_emp_dept1;
ALTER TABLE leader ADD CONSTRAINT fk_emp_dept1 FOREIGN KEY(deptId) REFERENCES department(id);

INSERT INTO department VALUES (1,"City",1);
INSERT INTO department VALUES (2,"AI",2);
INSERT INTO department VALUES (3,"Data",2);

INSERT INTO leader(id,name,deptId) VALUES (10,"Zhang Junbo",2);
INSERT INTO leader(name,deptId,salary) VALUES ("Zheng Yu",1,1000);
INSERT INTO leader(name,deptId,salary) VALUES ("Bao jie",2,200);


SELECT * FROM department;
SELECT * FROM leader;

set @age=12;
SELECT CASE WHEN @age>50 THEN "Old man" WHEN @age>30 THEN "Middle man" WHEN @age>20 THEN "Young man" WHEN @age>0 THEN "Children" ELSE "Dead" END AS COL ;

SELECT CASE WHEN WEEKDAY(NOW())=0 THEN '星期一' WHEN WEEKDAY(NOW())=1 THEN '星期二'  WHEN WEEKDAY(NOW())=2 THEN '星期三' WHEN WEEKDAY(NOW())=3 THEN '星期四' WHEN WEEKDAY(NOW())=4 THEN'星期五' WHEN WEEKDAY(NOW())=5 THEN '星期六' WHEN WEEKDAY(NOW())=6 THEN '星期天' END AS COLUMN1,NOW(),WEEKDAY(NOW()),DAYNAME(NOW());


USE w3cshool;
SET NAMES utf8;
SET FOREIGN_KEY_CHECKS = 0;

DROP TABLE IF EXISTS `employee_tbl`;
CREATE TABLE `employee_tbl` (
  `id` int(11) NOT NULL,
  `name` char(10) NOT NULL DEFAULT '',
  `date` datetime NOT NULL,
  `singin` tinyint(4) NOT NULL DEFAULT '0' COMMENT '登录次数',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

BEGIN;
INSERT INTO `employee_tbl` VALUES ('1', '小明', '2016-04-22 15:25:33', '1'), ('2', '小王', '2016-04-20 15:25:47', '3'), ('3', '小丽', '2016-04-19 15:26:02', '2'), ('4', '小王', '2016-04-07 15:26:14', '4'), ('5', '小明', '2016-04-11 15:26:40', '4'), ('6', '小明', '2016-04-04 15:26:54', '2');
COMMIT;

SET FOREIGN_KEY_CHECKS = 1;


update students set age=24 where student_id=123;
update students set age=28 where student_id=129;
update students set age=21 where student_id=139;
update students set age=22  where student_id=149;
update students set age=22 where student_id=159;
update students set age=23 where student_id=160;
update students set age=28 where student_id=161;
update students set age=26 where student_id=162;
update students set age=22 where student_id=163;

update students set hobby="swim" where student_id=14;
update students set hobby="swim" where student_id=123;
update students set hobby="play" where student_id=129;
update students set hobby="play" where student_id=139;
update students set hobby="swim" where student_id=149;
update students set hobby="swim" where student_id=159;
update students set hobby="swim" where student_id=160;
update students set hobby="swim" where student_id=161;
update students set hobby="play" where student_id=162;
update students set hobby="play" where student_id=163;


CREATE DATABASE if not EXISTS my_db;
SELECT table_name from information_schema.`TABLES` WHERE TABLE_SCHEMA='yiibaidb';

RENAME TABLE yiibaidb.employees to my_db.employees;
RENAME TABLE yiibaidb.items to my_db.items;
RENAME TABLE yiibaidb.offices to my_db.offices;
RENAME TABLE yiibaidb.orderdetails to my_db.orderdetails;
RENAME TABLE yiibaidb.orders to my_db.orders;
RENAME TABLE yiibaidb.payments to my_db.payments;
RENAME TABLE yiibaidb.productlines to my_db.productlines;
RENAME TABLE yiibaidb.products to my_db.products;
RENAME TABLE yiibaidb.tokens to my_db.tokens;


CREATE TABLE `users` (
  `u_id` int(10),
	`father` varchar(10),
	`country` varchar(19),
  PRIMARY KEY (`u_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8

INSERT INTO `users` values(14,"Zhang Jun","China");
INSERT INTO `users` values(160,"Wang Jun","Korea");
INSERT INTO `users` values(161,"Li Jun","U.S.");


select * from runoob_tbl;
select * from tcount_tbl;

select * from tcount_tbl as t1 inner left join runoob_tbl as t2;

select * from (
	select b.runoob_id,b.runoob_author as b_author,a.runoob_author as a_author,a.runoob_count,b.runoob_title,b.submission_date from tcount_tbl as a left outer join runoob_tbl as b on a.runoob_author=b.runoob_author 
	union 
	select b.runoob_id,b.runoob_author as b_author,a.runoob_author as a_author,a.runoob_count,b.runoob_title,b.submission_date from tcount_tbl as a right outer join runoob_tbl as b on a.runoob_author=b.runoob_author
	)new_tb where a_author is null or b_author is null;
	
	
CREATE DATABASE tmp;
use tmp;
CREATE TABLE `product` (    `id` int(11) NOT NULL AUTO_INCREMENT,    `product_name` varchar(30) CHARACTER SET utf8 NOT NULL,    `price` float NOT NULL,    PRIMARY KEY (`id`)   ) ENGINE=InnoDB;

CREATE TABLE `comment` (    `id` int(11) NOT NULL AUTO_INCREMENT,    `entity_id` int(11) NOT NULL,    `content` varchar(100) CHARACTER SET utf8 NOT NULL,    PRIMARY KEY (`id`)   ) ENGINE=InnoDB;

INSERT INTO `product` (`id`, `product_name`, `price`) VALUES   (1, '肉松饼', 5),   (2, '可乐', 5),   (3, '鸡翅', 12),   (4, '杯子', 42);   INSERT INTO `comment` (`id`, `entity_id`, `content`) VALUES   (1, 1, '味道还不错'),   (2, 1, '还行啊'),   (3, 3, '很实用哦');  

use w3cshool;
CREATE TABLE `my_students` (
  `student_id` int(10) unsigned NOT NULL DEFAULT '0',
  `name` varchar(30) DEFAULT NULL,
  `sex` char(1) DEFAULT NULL,
  `birth` date DEFAULT NULL,
  `age` smallint(6) DEFAULT NULL,
  `death_time` time DEFAULT NULL,
  `friends` year(4) DEFAULT NULL,
  `hobby` set('play','swim') DEFAULT NULL,
  PRIMARY KEY (`student_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8


DROP VIEW my_view;
CREATE VIEW my_view AS SELECT * FROM students where sex='女';
SELECT * from my_view;
SELECT * from students;

delete from my_view where student_id=123;
use w3cshool;

DESC students;


ALTER TABLE students MODIFY student_id INT;
ALTER TABLE students MODIFY student_id INT AUTO_INCREMENT;
ALTER TABLE students AUTO_INCREMENT = 300;

ALTER TABLE students CHANGE student_id student_id INT(10) AUTO_INCREMENT PRIMARY KEY;

insert into students(name,sex,birth,age,death_time,friends,hobby) values ("Carry",'男','2020-01-23',21,'12:34:19',1999,'swim');

insert into students(name,sex,birth,age,death_time,friends,hobby) SELECT name,sex,birth,age,death_time,friends,hobby from students;

select * from students;

use w3cshool;
create table test(
a int ,
b int,
c int,
d int,
key index_abc(a,b,c)
)engine=InnoDB default charset=utf8;


DROP PROCEDURE IF EXISTS proc_initData;
DELIMITER $
CREATE PROCEDURE proc_initData()
BEGIN
DECLARE i INT DEFAULT 1;
WHILE i<=10000 DO
    INSERT INTO test(a,b,c,d) VALUES(i,i,i,i);
    SET i = i+1;
END WHILE;
END $
CALL proc_initData();

select * from test;

select * from information_schema.innodb_locks;

use w3cshool;

DROP PROCEDURE IF EXISTS my_func1;
DELIMITER $$
CREATE PROCEDURE my_func1 ()
BEGIN
  DECLARE nick_name varchar(12) DEFAULT '21';
	SELECT nick_name ;
END $$


DROP PROCEDURE IF EXISTS my_func;
DELIMITER $$
CREATE PROCEDURE my_func(in u_id int,out u_name VARCHAR(20))
BEGIN
	SELECT concat(father," ",u_name) into u_name from users where users.u_id=u_id;
END $$

set @u_name="Jack";
CALL my_func(0,@u_name);
SELECT @u_name;



use w3cshool;

DROP PROCEDURE if EXISTS my_foo;
DELIMITER $$
CREATE PROCEDURE my_foo()
BEGIN
    declare id_sum int DEFAULT 0;
		declare p_id int DEFAULT 0;
		declare done int DEFAULT 0;
		
		declare c_id cursor for select u_id from users;
		declare continue handler for not found set done=1;
		
		OPEN c_id;
		
		repeat
		fetch c_id into p_id;
		set id_sum=id_sum+p_id;
		until done=1
		end repeat;
		
		select id_sum;
		CLOSE c_id;
END $$

CALL my_foo();



DROP FUNCTION if EXISTS my_func;
DELIMITER $$
CREATE FUNCTION my_func(score int) RETURNS VARCHAR(20)
BEGIN
	DECLARE age int DEFAULT 10;
	SET age=129;
	RETURN (age);
END $$

SELECT my_func(2);


set autocommit=1;
drop TRIGGER if EXISTS after_insert_order;
delimiter $$
create trigger after_insert_order before insert on users for each row
begin
		IF NEW.u_id=14 THEN
			set NEW.country='German';
		END IF;
end $$

show tables;
insert into users values(41,'LIsa',"Thai");

select * from leader;
select * from users;


DROP TABLE IF EXISTS account;
CREATE TABLE account (acct_num INT, amount DECIMAL(10,2));
INSERT INTO account VALUES(137,14.98),(141,1937.50),(97,-100.00);


DROP TRIGGER IF EXISTS upd_check;
delimiter $$
CREATE TRIGGER ins_check AFTER INSERT ON account FOR EACH ROW
begin
		IF OLD.amount = 100 THEN
		SET OLD.amount = 20;
	  ELSEIF OLD.amount > 100 THEN
		SET OLD.amount = 100;
		END IF;
end $$


select * from account;
update account set amount=100 where acct_num=137;
INSERT INTO account VALUES(139,100);

select * from account;
```

