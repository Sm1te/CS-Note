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

