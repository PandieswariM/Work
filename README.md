DELIMITER //

CREATE PROCEDURE DynamicSelect(
    IN p_userid VARCHAR(255),
    IN p_password VARCHAR(255),
    OUT p_row_count INT
)
BEGIN
    SET @sql_query = CONCAT('SELECT COUNT(*) FROM ea_users WHERE userid = ''', p_userid, ''' AND password = ''', p_password, ''' AND isactive = 1');

    PREPARE dynamic_statement FROM @sql_query;
    EXECUTE dynamic_statement INTO p_row_count;
    DEALLOCATE PREPARE dynamic_statement;
END //

DELIMITER ;





CALL DynamicSelect('user123', 'pass123', @row_count);
SELECT @row_count;
