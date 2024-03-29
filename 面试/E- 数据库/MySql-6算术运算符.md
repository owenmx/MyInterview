
$$
\text{MySql} 算术运算符
$$


| 运算符 | 作用               | 使用方法                           |
| ------ | ------------------ | ---------------------------------- |
| +      | 加法运算           | 用于获得一个或多个值的和           |
| -      | 减法运算           | 用于从一个值中减去另一个值         |
| *      | 乘法运算           | 使数字相乘，得到两个或多个值的乘积 |
| /      | 除法运算，返回商   | 用一个值除以另一个值得到商         |
| %，MOD | 求余运算，返回余数 | 用一个值除以另一个值得到余数       |


$$
\text{MySql} 逻辑运算符
$$

| 运算符      | 作用     |
| ----------- | -------- |
| NOT 或者 !  | 逻辑非   |
| AND 或者 && | 逻辑与   |
| OR 和 \|\|  | 逻辑或   |
| XOR         | 逻辑异或 |


$$
\text{MySql} 比较运算符
$$

| 运算符              | 作用                         |
| ------------------- | ---------------------------- |
| =                   | 等于                         |
| <=>                 | 安全的等于                   |
| <> 或者 !=          | 不等于                       |
| <=                  | 小于等于                     |
| >=                  | 大于等于                     |
| >                   | 大于                         |
| IS NULL 或者 ISNULL | 判断一个值是否为空           |
| IS NOT NULL         | 判断一个值是否不为空         |
| BETWEEN AND         | 判断一个值是否落在两个值之间 |


$$
\text{MySql} 运算符优先级
$$


| 优先级由低到高排列 | 运算符                                                       |
| ------------------ | ------------------------------------------------------------ |
| 1                  | =(赋值运算）、:=                                             |
| 2                  | II、OR                                                       |
| 3                  | XOR                                                          |
| 4                  | &&、AND                                                      |
| 5                  | NOT                                                          |
| 6                  | BETWEEN、CASE、WHEN、THEN、ELSE                              |
| 7                  | =(比较运算）、<=>、>=、>、<=、<、<>、!=、 IS、LIKE、REGEXP、IN |
| 8                  | \|                                                           |
| 9                  | &                                                            |
| 10                 | <<、>>                                                       |
| 11                 | -(减号）、+                                                  |
| 12                 | *、/、%                                                      |
| 13                 | ^                                                            |
| 14                 | -(负号）、〜（位反转）                                       |
| 15                 | !                                                            |