# java-DB-AdvancedSQL-join-excercises

Join excercises


1. Select all product names for suppliers from USA. 

2. Select order id, order date, employee first and last name for employee whose last name starts with 'D' and for order that was shipped to Germany.

3. Select all orders shipped by United Package for orders sooner than '1996-08-12'

4. Select shippers company name that doesn't have any orders assigned.

5. Select order ids that contains product from category 'Beverages' ordered '1996-08-14'.

6. Select product names and availability of products supplied by 'Lyngbysild' from category 'Seafood'

7. Select last names of employees assigned to 'Northern' region.

8. Select employee id and his boss last name for those who have orders with date later than '1996-07-18'.
if an employee doesn't have a boss don't include him.

9. List the employees in the warehouse with orders that are not shipped yet.

10. Calculate the price for each product with its name in each order after discount is applied. 





SELECT product_name FROM products
	JOIN suppliers ON products.product_id = suppliers.supplier_id
	WHERE suppliers.country = 'USA';
	
SELECT o.order_id, o.order_date, e.first_name, e.last_name FROM orders o
	LEFT JOIN employees e ON o.employee_id = e.employee_id
	WHERE e.last_name LIKE 'D%' AND o.ship_country = 'Germany';

SELECT o.*, s.company_name FROM orders o
	LEFT JOIN shippers s ON o.ship_via = s.shipper_id
	WHERE s.company_name = 'United Package' AND o.order_date < '1996-08-12';
	
SELECT s.company_name FROM shippers s 
	LEFT JOIN orders o ON s.shipper_id = o.ship_via
	WHERE o.ship_via IS NULL;
	
SELECT o.order_id FROM order_details o
	LEFT JOIN products p ON o.product_id = p.product_id
	LEFT JOIN categories c ON p.category_id = c.category_id
	WHERE c.category_name = 'Beverages';
	
SELECT p.product_name, CASE WHEN units_in_stock > 0 THEN 'Available' 
		ELSE 'Out of Stock' END AS availability FROM products p
		LEFT JOIN suppliers s ON p.supplier_id = s.supplier_id
		LEFT JOIN categories c ON p.category_id = c.category_id
		WHERE s.company_name = 'Lyngbysild' AND c.category_name = 'Seafood';
		
SELECT DISTINCT e.last_name FROM employees e
	LEFT JOIN employee_territories et ON e.employee_id = et.employee_id
	LEFT JOIN territories t ON et.territory_id = t.territory_id
	LEFT JOIN region r ON t.region_id = r.region_id
	WHERE r.region_description = 'Northern';
	
SELECT e.employee_id, e.reports_to FROM employees e
	LEFT JOIN orders o ON e.employee_id = o.employee_id
	WHERE o.order_date > '1996-07-18' AND e.reports_to IS NOT NULL;
	
	
SELECT e.first_name, e.last_name FROM employees e
	LEFT JOIN orders o ON e.employee_id = o.employee_id
	WHERE o.shipped_date IS NULL;
	
SELECT p.product_name, ROUND(CAST(p.unit_price - (p.unit_price * o.discount) AS NUMERIC), 2) AS discounted_price
	FROM products p
	JOIN order_details o ON p.product_id = o.product_id;


