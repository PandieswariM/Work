
```
SELECT FORMAT(ROUND(467886.468, 2), 2) AS formatted_number;


SELECT 
    IF(value >= 1000000, CONCAT(ROUND(value / 1000000, 2), ' m'), 
    IF(value >= 100000, CONCAT(ROUND(value / 100000, 2), ' lac'), 
    IF(value >= 1000, CONCAT(ROUND(value / 1000, 2), ' k'), value))) AS formatted_value
FROM (SELECT 467886.468 AS value) AS t;  -- Replace 467886.468 with your desired value
```


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

```
SELECT 
    value,
    CONCAT(
        ROUND(value / 
            IF(value >= 1000000, 1000000, 
                IF(value >= 100000, 100000, 1000)), 
            2), 
        IF(value >= 1000000, ' mill', 
            IF(value >= 100000, ' lac', ' k'))
    ) AS formatted_value
FROM numbers;
```
