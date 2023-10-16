## Maven Toys Store 

#### 1. Berapa total sales dari semua toko?
```sql
SELECT 
  SUM(s.units*p.product_price) AS total_Sales
FROM sales s 
JOIN products p ON s.product_id = p.product_id;
```

total_sales|
-----------|
14444572.35|

#### 2. Berapa profit yang dihasilkan?
```sql
SELECT 
  SUM(s.units*p.product_price) - SUM(s.units*product_cost) AS total_profit
FROM sales s 
JOIN products p ON s.product_id = p.product_id;
```
total_profit|
------------|
  4014029.00|

#### 3. 5 toko dengan penjualan tertinggi
```sql
SELECT 
  st.store_name,
  SUM(s.units*p.product_price) AS total_Sales
FROM sales s 
JOIN products p ON s.product_id = p.product_id 
JOIN stores st ON s.store_id = st.store_id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

store_name                   |total_sales|
-----------------------------|-----------|
Maven Toys Ciudad de Mexico 2|  554553.43|
Maven Toys Guadalajara 3     |  449354.91|
Maven Toys Ciudad de Mexico 1|  433556.21|
Maven Toys Toluca 1          |  411157.32|
Maven Toys Monterrey 2       |  372998.82|

#### 4. 5 toko dengan profit tertinggi
```sql
SELECT
  st.store_name,
  SUM(s.units*p.product_price) - SUM(s.units*product_cost) AS total_profit
FROM sales s 
JOIN products p ON s.product_id = p.product_id 
JOIN stores st ON s.store_id = st.store_id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

store_name                   |total_profit|
-----------------------------|------------|
Maven Toys Ciudad de Mexico 2|   169856.00|
Maven Toys Guadalajara 3     |   121571.00|
Maven Toys Ciudad de Mexico 1|   111296.00|
Maven Toys Monterrey 2       |   106783.00|
Maven Toys Toluca 1          |   104612.00|
