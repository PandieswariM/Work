DELIMITER //

CREATE PROCEDURE GetEmployeeData(
    IN p_designationrecid INT,
    IN p_gender VARCHAR(255),
    IN p_active INT,
    IN p_startdate DATE,
    IN p_enddate DATE
)
BEGIN
    SELECT *
    FROM your_table_name
    WHERE
        (IFNULL(p_designationrecid, designationrecid) = designationrecid)
        AND (IFNULL(p_gender, gender) = gender)
        AND (IFNULL(p_active, active) = active)
        AND doj BETWEEN p_startdate AND p_enddate
        AND doj BETWEEN '2023-01-03' AND '2023-06-03';
END //

DELIMITER ;
