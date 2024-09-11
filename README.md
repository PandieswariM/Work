```
DELIMITER $$

DROP PROCEDURE IF EXISTS sp_APP_GetConsentMGTAcptCustomer$$
CREATE PROCEDURE `sp_APP_GetConsentMGTAcptCustomer`(
    IN iConsentMGTRecId INT(11),
    IN iAppUserRecId INT(11),
    IN sAppType VARCHAR(20),
    IN sFilterStatus VARCHAR(10),
    IN iFinancialYearRecId INT(11)
)
BEGIN
    DECLARE sTerritoryRecId VARCHAR(1000) DEFAULT '';
    DECLARE sCustTypeCondition VARCHAR(100) DEFAULT '';
    DECLARE sCondition TEXT DEFAULT '';
    DECLARE sCustCondition TEXT DEFAULT '';
    DECLARE sStsCondition TEXT DEFAULT '';
    DECLARE sCode TEXT DEFAULT '';
    DECLARE bIsAllLocation INT(11) DEFAULT 0;
    DECLARE sFOTRecId VARCHAR(1000) DEFAULT '';
    DECLARE sMDOType VARCHAR(20) DEFAULT '';

    DROP TEMPORARY TABLE IF EXISTS tmpCustomerDetails;
    CREATE TEMPORARY TABLE tmpCustomerDetails (
        RetailerRecId INT(11),
        DistributorRecId INT(11),
        Code VARCHAR(45),
        ConsentMGTRecId INT(11)
    );

    DROP TEMPORARY TABLE IF EXISTS tmpCustomerRules;
    CREATE TEMPORARY TABLE tmpCustomerRules (
        RetailerRecId INT(11),
        DistributorRecId INT(11),
        ConsentMGTRecId INT(11),
        TerritoryRecId INT(11),
        RegionRecId INT(11),
        ZoneRecId INT(11),
        Code VARCHAR(45)
    );

    DROP TEMPORARY TABLE IF EXISTS tmpConsentMGT;
    CREATE TEMPORARY TABLE tmpConsentMGT (
        CustomerRecId INT(11),
        CustomerName VARCHAR(500),
        CustomerID VARCHAR(50),
        CustomerType VARCHAR(50),
        CustomerCode VARCHAR(50),
        Name VARCHAR(1000),
        IsAccepted TINYINT(1),
        TotalTargetkgl DECIMAL(25, 4),
        TotalTargetINR DECIMAL(25, 4)
    );

    -- Retrieve Code and Location flag
    SELECT Code INTO sCode
    FROM ConsentMGT AS CM
    LEFT JOIN ConsentMGTCustType AS CMT ON CMT.ConsentMGTCustTypeRecId = CM.ConsentMGTCustTypeRecId
    WHERE CM.ConsentMGTRecId = iConsentMGTRecId;

    SELECT COUNT(*) INTO bIsAllLocation
    FROM ConsentMGTRule
    WHERE ConsentMGTRecId = iConsentMGTRecId AND IFNULL(IsAllLocation, 0) = 1;

    -- Set Customer Type Condition
    IF IFNULL(sCode, '') NOT IN ('DT', 'ALL', 'FUPLD') THEN
        SET sCustTypeCondition = CONCAT(' AND RT.Code IN ("', sCode, '") ');
    END IF;

    -- Process based on App Type (MDO or TSM)
    IF sAppType IN ("MDO", "TSM") THEN
        IF sAppType = "TSM" THEN
            SELECT fn_APP_GetTerritoryForUser(iAppUserRecId) INTO sTerritoryRecId;
            SET sCustCondition = CONCAT(sCondition, ' AND CCT.Code = "FUPLD"');
            SET sCondition = CONCAT(sCondition, ' AND CCT.Code != "FUPLD"');
        ELSE
            SELECT fn_APP_GetMDOType(iAppUserRecId) INTO sMDOType;
            IF sMDOType = "FS" THEN
                SELECT fn_APP_GetMDOTerritoryForUser(iAppUserRecId) INTO sTerritoryRecId;
            ELSE
                SELECT fn_APP_GetFOTForUser(iAppUserRecId) INTO sFOTRecId;
            END IF;
            SET sCustCondition = CONCAT(sCondition, ' AND CCT.Code = "FUPLD"');
            SET sCondition = CONCAT(sCondition, ' AND (CCT.Code = "ALL")');
        END IF;

        -- Process by Territory or FOT
        IF sTerritoryRecId != '' THEN
            SET @Query = CONCAT('INSERT INTO tmpCustomerDetails (RetailerRecId, Code, ConsentMGTRecId)
                SELECT R.RetailerRecId, RT.Code, ', iConsentMGTRecId, '
                FROM Retailer R
                LEFT JOIN RetailerType RT ON R.RetailerTypeRecId = RT.RetailerTypeRecId
                WHERE R.TerritoryRecId IN (', sTerritoryRecId, ')', sCustTypeCondition, '
                AND IFNULL(R.IsDeleted, 0) = 0 AND IFNULL(R.IsActive, 0) = 1
                GROUP BY R.RetailerRecId
                ORDER BY R.BusinessName;');
            
            PREPARE stmt FROM @Query;
            EXECUTE stmt;
            DEALLOCATE PREPARE stmt;

            -- Insert Distributors if applicable
            IF sCode IN ('DT', 'ALL', 'FUPLD') THEN
                SET @Query = CONCAT('INSERT INTO tmpCustomerDetails (DistributorRecId, Code, ConsentMGTRecId)
                    SELECT D.DistributorRecId, "DT", ', iConsentMGTRecId, '
                    FROM Distributor D
                    WHERE D.TerritoryRecId IN (', sTerritoryRecId, ')
                    AND IFNULL(D.IsDeleted, 0) = 0 AND IFNULL(D.IsActive, 0) = 1
                    GROUP BY D.DistributorRecId
                    ORDER BY D.BusinessName;');
                PREPARE stmt FROM @Query;
                EXECUTE stmt;
                DEALLOCATE PREPARE stmt;
            END IF;
        ELSEIF sFOTRecId != '' THEN
            SET @Query = CONCAT('INSERT INTO tmpCustomerDetails (RetailerRecId, Code)
                SELECT R.RetailerRecId, RT.Code
                FROM Retailer R
                LEFT JOIN RetailerType RT ON R.RetailerTypeRecId = RT.RetailerTypeRecId
                LEFT JOIN CustomerFOTLink CFL ON CFL.RetailerRecId = R.RetailerRecId
                WHERE CFL.FOTRecId IN (', sFOTRecId, ')', sCustTypeCondition, '
                AND IFNULL(R.IsDeleted, 0) = 0 AND IFNULL(R.IsActive, 0) = 1
                GROUP BY R.RetailerRecId
                ORDER BY R.BusinessName;');
            PREPARE stmt FROM @Query;
            EXECUTE stmt;
            DEALLOCATE PREPARE stmt;

            -- Insert Distributors if applicable
            IF sCode IN ('DT', 'ALL', 'FUPLD') THEN
                SET @Query = CONCAT('INSERT INTO tmpCustomerDetails (DistributorRecId, Code)
                    SELECT D.DistributorRecId, "DT"
                    FROM Distributor D
                    LEFT JOIN CustomerFOTLink CFL ON CFL.DistributorRecId = D.DistributorRecId
                    WHERE CFL.FOTRecId IN (', sFOTRecId, ')
                    AND IFNULL(D.IsDeleted, 0) = 0 AND IFNULL(D.IsActive, 0) = 1
                    GROUP BY D.DistributorRecId
                    ORDER BY D.BusinessName;');
                PREPARE stmt FROM @Query;
                EXECUTE stmt;
                DEALLOCATE PREPARE stmt;
            END IF;
        END IF;

        -- Index creation
        ALTER TABLE tmpCustomerDetails ADD INDEX IX_tmpCustomerDetails_RetailerRecId (RetailerRecId);
        ALTER TABLE tmpCustomerDetails ADD INDEX IX_tmpCustomerDetails_DistributorRecId (DistributorRecId);

        -- Populate Customer Rules
        INSERT INTO tmpCustomerRules (RetailerRecId, DistributorRecId, TerritoryRecId, RegionRecId, ZoneRecId, ConsentMGTRecId, Code)
        SELECT tmp.RetailerRecId, tmp.DistributorRecId, T.TerritoryRecId, T.RegionRecId, RG.ZoneRecId, tmp.ConsentMGTRecId, tmp.Code
        FROM tmpCustomerDetails tmp
        LEFT JOIN Retailer R ON R.RetailerRecId = tmp.RetailerRecId
        LEFT JOIN Distributor D ON D.DistributorRecId = tmp.DistributorRecId
        LEFT JOIN Territory T ON T.TerritoryRecId = R.TerritoryRecId OR T.TerritoryRecId = D.TerritoryRecId
        LEFT JOIN Region RG ON RG.RegionRecId = T.RegionRecId
        WHERE (tmp.RetailerRecId IS NOT NULL AND R.RetailerRecId IS NOT NULL)
        OR (tmp.DistributorRecId IS NOT NULL AND D.DistributorRecId IS NOT NULL);
        
        -- Index creation for Rules
        ALTER TABLE tmpCustomerRules ADD INDEX IX_tmpCustomerRules_TerritoryRecId (TerritoryRecId);
        ALTER TABLE tmpCustomerRules ADD INDEX IX_tmpCustomerRules_RegionRecId (RegionRecId);
        ALTER TABLE tmpCustomerRules ADD INDEX IX_tmpCustomerRules_ZoneRecId (ZoneRecId);
    END IF;

    -- Construct the query for Consent Management
    IF sCode = "FUPLD" THEN
        SET @TotQuery = CONCAT('INSERT INTO tmpConsentMGT(CustomerRecId, CustomerName, CustomerID, CustomerType, CustomerCode, Name, IsAccepted)
            SELECT R.RetailerRecId, R.BusinessName, R.ERPId
            R.RetailerRecId, "Retailer", RT.Code, IFNULL(CR.Name, "")
            FROM tmpCustomerRules tmp
            LEFT JOIN Retailer R ON R.RetailerRecId = tmp.RetailerRecId
            LEFT JOIN ConsentMGT C ON C.ConsentMGTRecId = tmp.ConsentMGTRecId
            LEFT JOIN ConsentRule CR ON CR.ConsentMGTRecId = C.ConsentMGTRecId
            LEFT JOIN RetailerType RT ON RT.RetailerTypeRecId = R.RetailerTypeRecId
            WHERE IFNULL(R.IsDeleted, 0) = 0 
            AND IFNULL(R.IsActive, 0) = 1
            GROUP BY R.RetailerRecId
            ORDER BY R.BusinessName;');
    ELSE
        SET @TotQuery = CONCAT('INSERT INTO tmpConsentMGT(CustomerRecId, CustomerName, CustomerID, CustomerType, CustomerCode, Name, IsAccepted)
            SELECT D.DistributorRecId, D.BusinessName, D.ERPId, "Distributor", "DT", IFNULL(CR.Name, "")
            FROM tmpCustomerRules tmp
            LEFT JOIN Distributor D ON D.DistributorRecId = tmp.DistributorRecId
            LEFT JOIN ConsentMGT C ON C.ConsentMGTRecId = tmp.ConsentMGTRecId
            LEFT JOIN ConsentRule CR ON CR.ConsentMGTRecId = C.ConsentMGTRecId
            WHERE IFNULL(D.IsDeleted, 0) = 0 
            AND IFNULL(D.IsActive, 0) = 1
            GROUP BY D.DistributorRecId
            ORDER BY D.BusinessName;');
    END IF;

    -- Execute dynamically constructed query
    PREPARE stmt FROM @TotQuery;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;

    -- Apply Filter Status Conditions
    IF sFilterStatus IS NOT NULL THEN
        SET sStsCondition = CASE
            WHEN sFilterStatus = 'Accepted' THEN 'AND CM.IsAccepted = 1'
            WHEN sFilterStatus = 'Rejected' THEN 'AND CM.IsAccepted = 0'
            ELSE ''
        END;
    END IF;

    -- Final Select Statement
    SET @finalQuery = CONCAT('SELECT * FROM tmpConsentMGT WHERE 1=1 ', sStsCondition, ';');
    PREPARE stmt FROM @finalQuery;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;

    -- Clean up Temporary Tables
    DROP TEMPORARY TABLE IF EXISTS tmpCustomerDetails;
    DROP TEMPORARY TABLE IF EXISTS tmpCustomerRules;
    DROP TEMPORARY TABLE IF EXISTS tmpConsentMGT;

END$$
DELIMITER ;
```
