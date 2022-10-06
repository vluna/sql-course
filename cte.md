# Common Table Expression (CTE)
>Also known as:
>- WITH clause/query
>- Subquery refactor

### What is it?
A temporary result, meaning that it works within the execution scope of a single SELECT, INSERT, UPDATE, DELETE, or CREATE VIEW statement (not to be confused with temporary tables*).

### Why use it?
CTE makes it easy to write and maintain complex queries. When the queries become complex, they usually get harder to read. CTE can break it down into smaller portions to help with readability and reuse the queries if necessary.


**Temporary tables get deleted when the current client session is terminated.*

### Example
Let's say you have a chain of stores and you want to know which stores are performing well and which ones are not. In order to figure this out, you want to know the average sales of all stores and see which ones are above and below the average. Breaking it down into small pieces, you want to know:
- The total amount of sales per store
- The average amount of sales across all stores
- Stores that are making more than the average sales for all stores
- Stores that are making below the average sales for all stores

---

### Solution
#### Without CTE
```
-- Total sales per store	
SELECT s.id, SUM(oi.list_price * oi.quantity) as total_sales
FROM sales.orders o
JOIN sales.order_items oi ON o.id = oi.order_id
JOIN sales.stores s ON o.store_id = s.id
GROUP BY s.id;

-- AVG sales with all stores
SELECT AVG(total_sales)
FROM (SELECT s.id, SUM(oi.list_price * oi.quantity) as total_sales
		FROM sales.orders o
		JOIN sales.order_items oi ON o.id = oi.order_id
		JOIN sales.stores s ON o.store_id = s.id
		GROUP BY s.id) sales;

-- Stores making above average sales
SELECT *
FROM (SELECT s.id, SUM(oi.list_price * oi.quantity) as total_sales
		FROM sales.orders o
		JOIN sales.order_items oi ON o.id = oi.order_id
		JOIN sales.stores s ON o.store_id = s.id
		GROUP BY s.id) sales
JOIN (SELECT AVG(total_sales) avg_total_sales
		FROM (SELECT s.id, SUM(oi.list_price * oi.quantity) as total_sales
			FROM sales.orders o
			JOIN sales.order_items oi ON o.id = oi.order_id
			JOIN sales.stores s ON o.store_id = s.id
			GROUP BY s.id) sales) avg_sales
	ON sales.total_sales > avg_sales.avg_total_sales;
```

#### With CTE
```
-- Total sales per store
WITH sales_per_store (store_id, total_sales) AS (
	SELECT s.id as store_id, SUM(oi.list_price * oi.quantity) as total_sales
	FROM sales.orders o
	JOIN sales.order_items oi ON o.id = oi.order_id
	JOIN sales.stores s ON o.store_id = s.id
	GROUP BY s.id),
	
-- AVG sales with all stores
	avg_stores_sales (avg_total_sales) AS (
		SELECT AVG(total_sales) avg_total_sales
		FROM sales_per_store)
		
-- Stores making above average sales
SELECT *
FROM sales_per_store
JOIN avg_stores_sales ON sales_per_store.total_sales > avg_stores_sales.avg_total_sales;
```
