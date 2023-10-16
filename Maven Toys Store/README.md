## Mexico Toys Sales

Mexico Toys sales merupakan dataset yang terdiri dari 4 tabel yaitu sales, product, inventory, dan stores yang dapat diunduh [disini](https://www.mavenanalytics.io/data-playground?search=toy). Dibawah ini adalah query SQL untuk menjawab pertanyaan terkait dataset.

### 1. Berapa total sales dari semua toko?
```sql
SELECT 
  SUM(s.units*p.product_price) AS total_Sales
FROM sales s 
JOIN products p ON s.product_id = p.product_id;
```

total_sales|
-----------|
14444572.35|

### 2. Berapa sales tiap tahun?
```sql
SELECT
  date_part('year', s.order_date) AS YEAR,
  SUM(s.units*p.product_price) AS total_Sales
FROM sales s 
JOIN products p  ON s.product_id = p.product_id
GROUP BY 1;
```
year  |total_sales|
------|-----------|
2017| 7482498.08|
2018| 6962074.27|

### 3. Berapa total transaksi per kategori?
```sql
SELECT
  p.product_category,
  count(s.product_id) AS total_transaction
FROM sales s 
JOIN products p ON s.product_id = p.product_id
GROUP BY 1
```
product_category |total_transaction|
-----------------|-----------------|
Art & Crafts     |           220673|
Electronics      |            99025|
Games            |           157006|
Sports & Outdoors|           131331|
Toys             |           221227|

### 4. Berapa total unit terjual per kategori?
```sql
SELECT 
  p.product_category,
  SUM(s.units) AS total_unit_sold
FROM sales s 
JOIN products p ON s.product_id = p.product_id
GROUP BY 1
```
product_category |total_unit_sold|
-----------------|---------------|
Art & Crafts     |         325574|
Electronics      |         134075|
Games            |         194673|
Sports & Outdoors|         169043|
Toys             |         267200|

### 5. Berapa total sales per kategori?
```sql
SELECT
  p.product_category,
  SUM(s.units * p.product_price) AS total_sales
FROM sales s 
JOIN products p ON s.product_id = p.product_id
GROUP BY 1
````

product_category |total_sales|
-----------------|-----------|
Art & Crafts     | 2705364.26|
Electronics      | 2246771.25|
Games            | 2226836.27|
Sports & Outdoors| 2172359.57|
Toys             | 5093241.00|

### 6. Bagaimana total sales berdasarkan lokasi toko?
```sql
SELECT
  s2.store_location,
  SUM(s.units*p.product_price) AS total_sales
FROM sales s 
JOIN stores s2 ON s.store_id = s2.store_id
JOIN products p ON s.product_id = p.product_id
GROUP BY 1
ORDER BY 2 DESC;
```

store_location|total_sales|
--------------|-----------|
Downtown      | 8219596.27|
Commercial    | 3279139.52|
Residential   | 1656113.98|
Airport       | 1289722.58|

### 7. Berapa profit yang dihasilkan?
```sql
SELECT 
  SUM(s.units*p.product_price) - SUM(s.units*product_cost) AS total_profit
FROM sales s 
JOIN products p ON s.product_id = p.product_id;
```
total_profit|
------------|
  4014029.00|

### 8. 5 toko dengan penjualan tertinggi
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

### 9. 5 toko dengan profit tertinggi
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

### 10. 5 produk penjualan tertinggi
```sql
SELECT 
  product_name,
  SUM(product_price*units) AS total_sales
FROM sales s
JOIN products p ON s.product_id = p.product_id
GROUP BY 1
ORDER BY 2 DESC 
LIMIT 5;
```

product_name |total_sales|
-------------|-----------|
Lego Bricks  | 2388882.63|
Colorbuds    | 1564476.32|
Magic Sand   |  968962.02|
Action Figure|  926748.42|
Rubik's Cube |  912983.28|

### 11. Berapa total stock yang ada di masing-masing toko?
```sql
SELECT 
  s.store_name,
  SUM(stock_on_hand) AS total_stock
FROM inventory i 
JOIN stores s ON i.store_id = s.store_id
GROUP BY 1
ORDER BY 2;
```
store_name                   |total_stock|
-----------------------------|-----------|
Maven Toys Guanajuato 1      |        367|
Maven Toys Puebla 3          |        412|
Maven Toys Puebla 2          |        420|
Maven Toys Xalapa 1          |        435|
Maven Toys Saltillo 1        |        437|
-----------------------------|-----------|
Maven Toys Morelia 1         |        738|
Maven Toys Culiacan 1        |        738|
Maven Toys Hermosillo 1      |        751|
Maven Toys Hermosillo 3      |        756|
Maven Toys Chihuahua 2       |        874|
Maven Toys Ciudad de Mexico 2|        915|
50 rows fetched, showing some data.

### 12. Apa saja produk yang memiliki stok di bawah ambang batas tertentu di setiap toko (misal ambang batas = 10?
```sql
SELECT 
  s.store_name,
  p.product_name,
  i.stock_on_hand
FROM inventory i 
JOIN stores s 
ON i.store_id = s.store_id 
JOIN products p 
ON i.product_id = p.product_id 
WHERE i.stock_on_hand <=10
ORDER BY 1,3;
```

store_name                   |product_name         |stock_on_hand|
-----------------------------|---------------------|-------------|
Maven Toys Aguascalientes 1  |Hot Wheels 5-Pack    |            0|
Maven Toys Aguascalientes 1  |Playfoam             |            0|
Maven Toys Aguascalientes 1  |Foam Disk Launcher   |            0|
Maven Toys Aguascalientes 1  |Mini Ping Pong Set   |            0|
Maven Toys Aguascalientes 1  |Chutes & Ladders     |            1|
----------------------------|---------------------|---------------|
Maven Toys Campeche 1        |Jenga                |            1|
Maven Toys Campeche 1        |Uno Card Game        |            2|
Maven Toys Campeche 1        |Kids Makeup Kit      |            3|
-----------------------------|---------------------|-------------|
Maven Toys Zacatecas 1       |Supersoaker Water Gun|            9|
Maven Toys Zacatecas 1       |Dino Egg             |            9|
Maven Toys Zacatecas 1       |Dart Gun             |            9|
Maven Toys Zacatecas 1       |Plush Pony           |            9|
Maven Toys Zacatecas 1       |Splash Balls         |           10|

650 rows fetched, showing some data.


### 13. Berapa margin keuntungan dari masing-masing produk?
```sql
SELECT
  product_name,
  product_price,
  product_cost,
ROUND((product_price - product_cost) / product_cost * 100, 2) AS margin_percentage
FROM products
GROUP BY 1,2,3
ORDER BY 4 DESC;
```
product_name         |product_price|product_cost|margin_percentage|
---------------------+-------------+------------+-----------------+
Jenga                |         9.99|        2.99|           234.11|
Mini Basketball Hoop |        24.99|        8.99|           177.98|
Playfoam             |        10.99|        3.99|           175.44|
Plush Pony           |        19.99|        8.99|           122.36|
Colorbuds            |        14.99|        6.99|           114.45|
--------------------|--------------|------------|-----------------|
PlayDoh Playset      |        24.99|       20.99|            19.06|
Teddy Bear           |        12.99|       10.99|            18.20|
Magic Sand           |        15.99|       13.99|            14.30|
Lego Bricks          |        39.99|       34.99|            14.29|
Splash Balls         |         8.99|        7.99|            12.52|
Rubik's Cube         |        19.99|       17.99|            11.12|
Dino Egg             |        10.99|        9.99|            10.01|

35 rows fetched, showing some data

### 14. Berapa margin keuntungan dari masing-masing kategori?
```sql
SELECT
  product_category,
  ROUND(AVG(product_price), 2) AS avg_price_per_product,
  ROUND(AVG(product_cost), 2) AS avg_cost_per_product,
  CONCAT(ROUND(((AVG(product_price) - AVG(product_cost)) / AVG(product_cost)) * 100, 2), '%') AS margin
FROM products
GROUP BY 1
ORDER BY 4 DESC;
````
product_category |avg_price_per_product|avg_cost_per_product|margin|
-----------------|---------------------|--------------------|------|
Sports & Outdoors|                15.28|               10.28|48.66%|
Games            |                12.37|                8.37|47.82%|
Art & Crafts     |                13.12|                8.99|45.88%|
Electronics      |                20.66|               14.32|44.22%|
Toys             |                15.99|               11.66|37.17%|

### 15. Bagaimana total sales per bulan dibandingkan bulan sebelumnya?
```sql
WITH sales_per_month AS (
SELECT 
  date_part('month', order_date) AS MONTH,
  SUM(s.units*p.product_price) AS total_sales
FROM sales s
JOIN products p ON s.product_id = p.product_id
GROUP BY 1),

prev_month AS (
SELECT
  MONTH,
  total_Sales,
LAG(total_sales, 1) OVER(ORDER BY month) AS previous_month_Sales
FROM sales_per_month)

SELECT
  *,
CONCAT(ROUND((total_sales - previous_month_sales) / previous_month_sales * 100, 2), '%') AS vs_previous_month
FROM prev_month;
```
month|total_sales|previous_month_sales|vs_previous_month|
-----|-----------|--------------------|-----------------|
  1.0| 1289751.13|                    |                 |
  2.0| 1263983.84|          1289751.13|-2.00%           |
  3.0| 1473000.83|          1263983.84|16.54%           |
  4.0| 1508764.05|          1473000.83|2.43%            |
  5.0| 1497689.39|          1508764.05|-0.73%           |
  6.0| 1470279.47|          1497689.39|-1.83%           |
  7.0| 1384383.09|          1470279.47|-5.84%           |
  8.0| 1150299.80|          1384383.09|-16.91%          |
  9.0| 1244038.52|          1150299.80|8.15%            |
 10.0|  623874.39|          1244038.52|-49.85%          |
 11.0|  661304.15|           623874.39|6.00%            |
 12.0|  877203.69|           661304.15|32.65%           |

### 16. Apa produk yang memiliki stok tertinggi di setiap toko?
```sql
WITH rank_by_stock AS (
SELECT
  *,
  RANK() OVER(PARTITION BY store_id ORDER BY stock_on_hand DESC) AS ranks
FROM inventory)

SELECT
  s.store_name,
  p.product_name,
  r.stock_on_hand
FROM rank_by_stock r
JOIN products p ON r.product_id = p.product_id 
JOIN stores s ON r.store_id = s.store_id
WHERE r.ranks = 1
ORDER BY 1;
```
store_name                   |product_name      |stock_on_hand|
-----------------------------|------------------|-------------|
Maven Toys Aguascalientes 1  |Magic Sand        |           67|
Maven Toys Campeche 1        |Deck Of Cards     |           73|
Maven Toys Campeche 2        |Magic Sand        |           90|
Maven Toys Chetumal 1        |Deck Of Cards     |           61|
Maven Toys Chihuahua 1       |Playfoam          |           48|
Maven Toys Chihuahua 2       |Deck Of Cards     |           94|
-----------------------------|------------------|-------------|
Maven Toys Toluca 1          |Dinosaur Figures  |           67|
Maven Toys Toluca 2          |Dinosaur Figures  |           66|
Maven Toys Tuxtla Gutierrez 1|Magic Sand        |           63|
Maven Toys Villahermosa 1    |PlayDoh Can       |          131|
Maven Toys Xalapa 1          |PlayDoh Can       |           47|
Maven Toys Xalapa 2          |Deck Of Cards     |           90|
Maven Toys Zacatecas 1       |PlayDoh Can       |           92|

52 rows fetched, showing some data

### 17. Apa produk dengan jumlah barang penjualan tertinggi masing-masing toko?
```sql
SELECT
  store_name,
  product_name,
  total_sold
FROM (
  SELECT
    store_id,
    product_id,
    SUM(units) AS total_sold,
    RANK() OVER(PARTITION BY store_id ORDER BY SUM(units) DESC) AS ranking
  FROM sales
  GROUP BY 1,2)r
JOIN stores s ON s.store_id = r.store_id
JOIN products p ON p.product_id = r.product_id
WHERE ranking = 1
ORDER BY 1;
```
store_name                   |product_name      |total_sold|
-----------------------------+------------------+----------+
Maven Toys Aguascalientes 1  |Colorbuds         |      2096|
Maven Toys Campeche 1        |Mini Ping Pong Set|      4244|
Maven Toys Campeche 2        |Barrel O' Slime   |      1849|
Maven Toys Chetumal 1        |Colorbuds         |      1922|
Maven Toys Chihuahua 1       |Mini Ping Pong Set|      2055|
Maven Toys Chihuahua 2       |Mini Ping Pong Set|      4229|
----------------------------|--------------------|---------|
Maven Toys Toluca 1          |Barrel O' Slime   |      4218|
Maven Toys Toluca 2          |Splash Balls      |      1657|
Maven Toys Tuxtla Gutierrez 1|Barrel O' Slime   |      2501|
Maven Toys Villahermosa 1    |PlayDoh Can       |      2682|
Maven Toys Xalapa 1          |Colorbuds         |      2160|
Maven Toys Xalapa 2          |PlayDoh Can       |      3310|
Maven Toys Zacatecas 1       |Barrel O' Slime   |      2138|
