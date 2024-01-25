SELECT 
  CASE WHEN TIMESTAMPDIFF(YEAR, date_column1, date_column2) > 0 THEN
    CONCAT(TIMESTAMPDIFF(YEAR, date_column1, date_column2), ' years ')
  ELSE '' END
  +
  CASE WHEN TIMESTAMPDIFF(MONTH, date_column1, date_column2) % 12 > 0 THEN
    CONCAT(TIMESTAMPDIFF(MONTH, date_column1, date_column2) % 12, ' months ')
  ELSE '' END AS date_difference
FROM your_table;
