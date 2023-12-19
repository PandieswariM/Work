DELIMITER //

CREATE PROCEDURE GetEmployeeData(
    IN p_designationrecid INT,
    IN p_gender VARCHAR(255),
    IN p_active INT,
    IN p_startdate DATE,
    IN p_enddate DATE,
    IN sSorting VARCHAR(50)
)
BEGIN
    DECLARE sSortingQuery VARCHAR(500) DEFAULT '';

    SET sSortingQuery = CONCAT('ORDER BY ', sSorting, '');

    SET @sQuery = CONCAT('SELECT
        EA.EmployeeRecId                        AS EmployeeRecId,
        EA.EmpId                                AS Employeeid,
        EA.EmpName                              AS Employeename,
        D.Designation                           AS Designation,
        BG.BloodGroup                           AS Bloodgroup,
        EA.Age                                  AS Age,
        DATE_FORMAT(EA.Dob, "%d/%m/%Y")         AS Dob,
        EA.Gender                               AS Gender,
        EA.EmailId                              AS Emailid,
        EA.MobileNo                             AS Mobilenumber,
        DATE_FORMAT(EA.JoiningDate, "%d/%m/%Y") AS Dateofjoining,
        IF(EA.IsActive = 1, ''Yes'', ''No'')    AS Active,
        EA.IsDeleted                            AS IsDeleted,
        EA.NoOfLeave                            AS Noofleave,
        EA.Address                              AS Address
    FROM
        EA_Employee                 AS EA
        LEFT JOIN EA_Designation    AS D  ON EA.DesignationRecId = D.DesignationRecId
        LEFT JOIN EA_BloodGroup     AS BG ON EA.BloodGroupRecId = BG.BloodGroupRecId
        WHERE
            (IFNULL(p_designationrecid, EA.DesignationRecId) = EA.DesignationRecId)
            AND (IFNULL(p_gender, EA.Gender) = EA.Gender)
            AND (IFNULL(p_active, EA.IsActive) = EA.IsActive)
            AND (IFNULL(p_startdate, EA.JoiningDate) <= EA.JoiningDate AND IFNULL(p_enddate, EA.JoiningDate) >= EA.JoiningDate)
        ', IFNULL(sSortingQuery, ''));

    PREPARE stmt FROM @sQuery;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;


DELIMITER //

CREATE PROCEDURE GetEmployeeData(
    IN p_designationrecid INT,
    IN p_gender VARCHAR(255),
    IN p_active INT,
    IN p_startdate DATE,
    IN p_enddate DATE,
    IN p_sorting VARCHAR(50)
)
BEGIN
    DECLARE sQuery VARCHAR(5000) DEFAULT '';
    DECLARE sSortingQuery VARCHAR(500) DEFAULT '';

    SET sSortingQuery = CONCAT('ORDER BY ', p_sorting, '');

    SET sQuery = CONCAT('
        SELECT
            EA.EmployeeRecId AS EmployeeRecId,
            EA.EmpId AS Employeeid,
            EA.EmpName AS Employeename,
            D.Designation AS Designation,
            BG.BloodGroup AS Bloodgroup,
            EA.Age AS Age,
            DATE_FORMAT(EA.Dob, "%d/%m/%Y") AS Dob,
            EA.Gender AS Gender,
            EA.EmailId AS Emailid,
            EA.MobileNo AS Mobilenumber,
            DATE_FORMAT(EA.JoiningDate, "%d/%m/%Y") AS Dateofjoining,
            IF(EA.IsActive = 1, "Yes", "No") AS Active,
            EA.IsDeleted AS IsDeleted,
            EA.NoOfLeave AS Noofleave,
            EA.Address AS Address
        FROM
            EA_Employee AS EA
            LEFT JOIN EA_Designation AS D ON EA.DesignationRecId = D.DesignationRecId
            LEFT JOIN EA_BloodGroup AS BG ON EA.BloodGroupRecId = BG.BloodGroupRecId
        WHERE
            (IFNULL(', IFNULL(p_designationrecid, 'NULL'), ', designationrecid) = designationrecid)
            AND (IFNULL(', IFNULL(p_gender, 'NULL'), ', gender) = gender)
            AND (IFNULL(', IFNULL(p_active, 'NULL'), ', active) = active)
            AND (IFNULL(', IFNULL(p_startdate, 'NULL'), ', doj) <= doj AND IFNULL(', IFNULL(p_enddate, 'NULL'), ', doj) >= doj
    ', IFNULL(sSortingQuery, ''));

    PREPARE stmt FROM sQuery;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;
