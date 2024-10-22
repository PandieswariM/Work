```
Your order (ID: [Your Order ID]) containing apples and oranges has been dispatched and is on its way!
```
```
SELECT GROUP_CONCAT(Productname) AS all_products
FROM products;
```
```
SELECT 
    value,
    CONCAT(
        ROUND(value / IF(value >= 100000, 100000, 1000), 2), 
        IF(value >= 100000, ' lac', ' k')
    ) AS formatted_value
FROM numbers;
```
