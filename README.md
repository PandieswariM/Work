SET @row_number = 0;

SELECT 
    (@row_number:=@row_number + 1) AS index_count,
    person_details.*
FROM 
    person_details
LIMIT 10;
