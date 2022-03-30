# MySQL Basic Operation

## 1. Intro

Every row should have its own **primary key**

To choose/use database: 

**USE** dB_name

**SHOW** TABLES



## 2. Retrieve data

### 2.1 SELECT

Select a column  from table:

```mysql
SELECT prod_name FROM products;
```

MySQL syntax **cannot identify Caps** or not 

General us rows and cols with lowercase, operation keywords with capitals.



When SQL deal with our code, any **whitespace will be ignored.**



Select **multiple** cols:

```mysql
SELECT prod_id, prod_name, prod_price FROM products;
```

Select **all** cols:

```mysql
SELECT * FROM products;
```



**Distinct** to return distinct rows without duplication.(Apply to **all rows**)

```mysql
SELECT DISTINCT vend_id FROM products;
```



**Limit** result:

```mysql
SELECT prod_name FROM products LIMIT 5;
```

This code above select to return less or equal to **5 rows** from **row 0**

```mysql
SELECT prod_name FROM products LIMIT 5,5;
```

This code above select to return  **5 rows** from **row 5**



```mysql
SELECT products.prod_name FROM db.products
```



### 2.2 Select with Sorted data

```mysql
SELECT prod_name FROM products ORDER BY prod_names;
```



```mysql
SELECT prod_id, prod_name, prod_price FROM products ORDER BY prod_price, prod_name;
```

**First sort by price, and then name(only if they have the same price)**



Change sorting order: (Default : **ASC**)

```mysql
SELECT prod_id, prod_name, prod_price FROM products ORDER BY prod_price DESC;
```

```mysql
SELECT prod_id, prod_name, prod_price FROM products ORDER BY prod_price DESC, prod_name;
```

**DESC** only applies to the col in front of it.



Generally in MySQL, **A is equal to a**.

**Max** or **Min** value:

```mysql
SELECT prod_id, prod_name, prod_price FROM products ORDER BY prod_price DESC LIMIT 1;
SELECT prod_id, prod_name, prod_price FROM products ORDER BY prod_price LIMIT 1;
```



### 2.3 Filter data

**WHERE**

```mysql
SELECT prod_price FROM products WHERE prod_price = 5;
SELECT prod_name, prod_price FROM products WHERE prod_name = 'fuses';
SELECT prod_price FROM products WHERE prod_price <= 10;
SELECT vend_id, prod_price FROM products WHERE prod_price <> 5;
```

If you want to use ORDER BY with WHERE, you should **put ORDER BY after WHERE**



**Operations:**

| Operations | What it do              |
| ---------- | ----------------------- |
| =          | equal                   |
| <>         | not equal               |
| !=         | not equal               |
| <          | less                    |
| <=         | less or equal           |
| >          | greater                 |
| >=         | greater or equal        |
| BETWEEN    | SELECT between 2 values |

Check null:

```mysql
SELECT prod_name FROM products WHERE prod_price IS NULL;
```



### 2.4 Combined WHERE with logic operator

**AND/OR**

```mysql
SELECT prod_id, prod_price, prod_name FROM products WHERE vend_id = 1003 AND prod_price <= 10;
SELECT prod_id, prod_price, prod_name FROM products WHERE vend_id = 1003 OR prod_price != 10;
```



```mysql
SELECT prod_id, prod_price, prod_name FROM products WHERE vend_id = 1003 OR prod_price != 10 AND prod_name != 'washer';
```

In this code, SQL will **first deal with AND and then solve OR**

**Brackets > AND > OR**

```mysql
SELECT prod_id, prod_price, prod_name FROM products WHERE (vend_id = 1003 OR vend_id = 1002) AND prod_name != 'washer';
```



**IN** 

```mysql
SELECT prod_id, prod_price, prod_name FROM products WHERE vend_id IN (1001, 1200) AND prod_name != 'washer';
```



**NOT**

```mysql
SELECT prod_id, prod_price, prod_name FROM products WHERE vend_id NOT IN (1001, 1200) AND prod_name != 'washer';
```



### 2.5 LIKE

LIKE指示MySQL, 后跟的搜索模式利用通配符匹配而不是直接相等匹配进行比较

#### %

%表示任何字符出现任何次数

```mysql
SELECT prod_id, prod_name FROM products
WHERE prod_name LIKE 'jet%'
```

找出所有以词jet开头的产品

```mysql
SELECT prod_id, prod_name FROM products
WHERE prod_name LIKE '%anvil%'
```

表示匹配任何位置包含文本anvil的值，而不论之前或者之后出现的词

'%' 不能匹配 null

#### _

_只能匹配单个字符

```mysql
SELECT prod_id, prod_name FROM products
WHERE prod_name LIKE '_ anvil'
```



### 2.6正则表达式

#### LIKE/REGEXP

```mysql
SELECT prod_id, prod_name FROM products
WHERE prod_name LIKE '1000'
ORDER BY prod_name

SELECT prod_id, prod_name FROM products
WHERE prod_name REGEXP '1000'
ORDER BY prod_name

.000 在正则表达式中 .表示可以匹配任意一个字符
```

第一个不会返回数据，LIKE匹配整个列，如果被匹配的文本在列值中出现，LIKE将不会找到它，所以也不会返回, 除非使用通配符。

REGEXP在列值内进行匹配，如果找到了就会返回



正则表达式不区分大小写， 可以使用BINARY来区分

```mysql
WHERE prod_name REGEXP BINARY 'JetPack .000'
```

#### OR |

```mysql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000|2000'
ORDER BY prod_name
```

#### 匹配几个字符之一

```MYSQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP '[123] ROBIN'
ORDER BY prod_name
```

能匹配1或2或3 = [1|2|3]

'1|2|3 ROBIN' = {1, 2, 3 ROBIN}

[^123]否定 取反

#### 匹配范围

[123456789] = [0-9]

[a-z]

```mysql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '[1-3] ROBIN'
ORDER BY prod_name
```

#### 匹配特殊字符

\\\前导 \\\\-表示查找'-' 同理'.'也是一样 

| \\\f     | 换页     |
| -------- | -------- |
| **\\\n** | **换行** |
| \\\r     | 回车     |
| \\\t     | 制表     |
| \\\v     | 纵向制表 |

![image-20220322163559936](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220322163559936.png)

#### 匹配多个实例

![image-20220322163639197](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220322163639197.png)

```MYSQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP '\\([0-9] sticks?\\)'
ORDER BY prod_name
```

**[0-9]匹配任意数字， \\\\(匹配)，sticks 后面的s + ? => s可以选**

```mysql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '[[:digit:]]{4}'
ORDER BY prod_name
```

**匹配任意数字，{4}表示任意数字出现4次，所以等于匹配四位数**

#### 定位符

![image-20220322164135046](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220322164135046.png)

找以一个数开头的所有产品：

```mysql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '^[:digit:\\.]'
ORDER BY prod_name
```

^有双重用途



## 3. 创造计算字段

#### 3.1 拼接

有的DBMS可以使用+ 或者 || 来实现拼接，MySQL要用Concat()来实现

<img src="C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220322170631965.png" alt="image-20220322170631965" style="zoom:67%;" />

```mysql
SELECT Concat(RTrim(vend_name)，'(', RTrim(vend_contry), ')')
FROM vendors
ORDER BY vend_name
```

RTrim() 可以去掉右边的所有空格，同理也有LTrim()



#### 3.2 Alias

```mysql
SELECT Concat(RTrim(vend_name)，'(', RTrim(vend_contry), ')') AS vend_title
FROM vendors
ORDER BY vend_name
```



#### 3.3 算数计算

```mysql
SELECT prod_id, quantity, item_price, quantity*item_prices AS expanded_price
FROM orderitems
WHERE order_num = 20005
```

![image-20220322172025511](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220322172025511.png)

### 4. 函数

#### 4.1 文本处理函数

| Left()      | 返回串左边的字符  |
| ----------- | ----------------- |
| Length()    | 返回串的长度      |
| Locate()    | 找出串的一个子串  |
| Lower()     | 转为小写          |
| LTrim()     | 去掉串左边的空格  |
| Right()     | 返回串右边的字符  |
| RTrim()     | 去掉串右边的空格  |
| Soundex()   | 返回串的SOUNDEX值 |
| SubString() | 返回子串的字符    |
| Upper()     | 转为大写          |

```mysql
SELECT cust_name， cust_contact FROM customers WHERE Soundex(cust_contact) = Soundex('robin')
```

#### 4.2 时间和日期处理函数

<img src="C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220329211548456.png" alt="image-20220329211548456" style="zoom:77%;" />

日期格式： YYYY-MM-DD

```mysql
SELECT cust_id, order_num FROM orders WHERE Date(order_date) BETWEEN '2021-09-01' AND '2021-10-01'
SELECT cust_id, order_num FROM orders WHERE Year(order_date) = 2021 AND Month(order_date) = 9
```

#### 4.3 数据处理函数

<img src="C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220329212605045.png" alt="image-20220329212605045" style="zoom:67%;" />



### 5. 汇总处理

| AVG()       | 平均值       |
| ----------- | ------------ |
| **COUNT()** | **某列行数** |
| **MAX()**   | **最大值**   |
| **MIN()**   | **最小值**   |
| **SUM()**   | **和**       |

```mysql
SELECT AVG(X) AS AVG_VAL FROM Y WHERE vend_id = 1003
```

```mysql
SELECT COUNT(X) AS Y FROM Z
SELECT COUNT(*) AS Y FROM Z
SELECT AVG(DISTINC prod_price) AS avg_price FROM X
```



### 6. 数据分组

**GROUP BY**

```mysql
SELECT book, COUNT(*) AS num_prods FROM products GROUP BY vend_id
```

GROUP BY必须出现在WHERE之后,ORDER BY之前

**HAVING	**

所有的where语句都可以使用having代替

having过滤分组，where过滤行

```mysql
SELECT cust_id, COUNT(*) AS orders FROM orders GROUP BY cust_id HAVING COUNT(*) >= 2
```

where在分组前过滤数据，HAVING在分组后过滤数据



**ORDER BY / GROUP BY**

![image-20220329214905680](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220329214905680.png)

```mysql
SELECT order_num, SUM(quantity * item_price) AS ordertotal FROM orderitems
GROUP BY order_num
HAVING SUM(ordertotal >= 50)
ORDER BY ordertotal
```



![image-20220329215338318](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220329215338318.png)

![image-20220329215355701](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220329215355701.png)





### 7. 子查询
