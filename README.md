```
DELIMITER $$
DROP PROCEDURE IF EXISTS sp_APP_GetConsentMGTAcptCustomer$$
/*********************************************************************************************
* Created By    : Pandieswari M - Annam Software (P) Ltd.
* Name          : sp_APP_GetConsentMGTAcptCustomer
* Created On    : 23/08/2024
* Description   : To get consent management accepted retailer
* Reviewed By   :
**********************************************************************************************/

CREATE PROCEDURE `sp_APP_GetConsentMGTAcptCustomer`(
     IN iConsentMGTRecId    INT(11),
     IN iAppUserRecId       INT(11),
     IN sAppType            VARCHAR(20),
     IN sFilterStatus       VARCHAR(10),
     IN iFinancialYearRecId INT(11)
)
BEGIN
    DECLARE sTerritoryRecId     VARCHAR(1000) DEFAULT '';
    DECLARE sCustTypeCondition  VARCHAR(100) DEFAULT '';
    DECLARE sCondition          TEXT DEFAULT '';
    DECLARE sCustCondition      TEXT DEFAULT '';
    DECLARE sStsCondition       TEXT DEFAULT '';
    DECLARE sCode               TEXT DEFAULT '';
    DECLARE bIsAllLocation      INT DEFAULT 0;
    DECLARE sFOTRecId           VARCHAR(1000) DEFAULT '';
    DECLARE sMDOType            VARCHAR(20) DEFAULT '';
    DECLARE query               TEXT;

    -- Fetch necessary data
    SELECT Code INTO sCode
    FROM ConsentMGT AS CM
    LEFT JOIN ConsentMGTCustType AS CMT ON CMT.ConsentMGTCustTypeRecId = CM.ConsentMGTCustTypeRecId
    WHERE CM.ConsentMGTRecId = iConsentMGTRecId;

    SELECT COUNT(*) INTO bIsAllLocation
    FROM ConsentMGTRule
    WHERE ConsentMGTRecId = iConsentMGTRecId AND IFNULL(IsAllLocation, 0) = 1;

    IF sCode NOT IN ('DT', 'ALL', 'FUPLD') THEN
        SET sCustTypeCondition = CONCAT(' AND RT.Code IN ("', sCode, '") ');
    END IF;

    -- Determine app type and set conditions
    CASE sAppType
        WHEN 'TSM' THEN
            SELECT fn_APP_GetTerritoryForUser(iAppUserRecId) INTO sTerritoryRecId;
            SET sCustCondition = CONCAT(sCondition, ' AND CCT.Code = "FUPLD"');
            SET sCondition = CONCAT(sCondition, ' AND CCT.Code != "FUPLD"');
        WHEN 'MDO' THEN
            SELECT fn_APP_GetMDOType(iAppUserRecId) INTO sMDOType;
            IF sMDOType = 'FS' THEN
                SELECT fn_APP_GetMDOTerritoryForUser(iAppUserRecId) INTO sTerritoryRecId;
            ELSE
                SELECT fn_APP_GetFOTForUser(iAppUserRecId) INTO sFOTRecId;
            END IF;
            SET sCustCondition = CONCAT(sCondition, ' AND CCT.Code = "FUPLD"');
            SET sCondition = CONCAT(sCondition, ' AND (CCT.Code = "ALL")');
    END CASE;

    -- Dynamic SQL for Retailer/Distributor Insertion
    IF sTerritoryRecId <> '' THEN
        SET @Query = CONCAT('INSERT INTO tmpCustomerDetails (RetailerRecId, Code, ConsentMGTRecId)
            SELECT R.RetailerRecId, RT.Code, ', iConsentMGTRecId, '
            FROM Retailer AS R
            LEFT JOIN RetailerType AS RT ON R.RetailerTypeRecId = RT.RetailerTypeRecId
            WHERE IFNULL(R.IsDeleted, 0) = 0 AND IFNULL(R.IsActive, 0) = 1 AND R.TerritoryRecId IN (', sTerritoryRecId, ')', sCustTypeCondition, '
            GROUP BY R.RetailerRecId ORDER BY R.BusinessName');

        PREPARE stmt FROM @Query;
        EXECUTE stmt;
        DEALLOCATE PREPARE stmt;

        IF sCode IN ('DT', 'ALL', 'FUPLD') THEN
            SET @Query = CONCAT('INSERT INTO tmpCustomerDetails (DistributorRecId, Code, ConsentMGTRecId)
                SELECT D.DistributorRecId, "DT", ', iConsentMGTRecId, '
                FROM Distributor AS D
                WHERE IFNULL(D.IsDeleted, 0) = 0 AND IFNULL(D.IsActive, 0) = 1 AND D.TerritoryRecId IN (', sTerritoryRecId, ')
                GROUP BY D.DistributorRecId ORDER BY D.BusinessName');

            PREPARE stmt FROM @Query;
            EXECUTE stmt;
            DEALLOCATE PREPARE stmt;
        END IF;
    ELSEIF sFOTRecId <> '' THEN
        SET @Query = CONCAT('INSERT INTO tmpCustomerDetails (RetailerRecId, Code)
            SELECT R.RetailerRecId, RT.Code
            FROM Retailer AS R
            LEFT JOIN RetailerType AS RT ON R.RetailerTypeRecId = RT.RetailerTypeRecId
            LEFT JOIN CustomerFOTLink AS CFL ON CFL.RetailerRecId = R.RetailerRecId
            WHERE IFNULL(R.IsDeleted, 0) = 0 AND IFNULL(R.IsActive, 0) = 1 AND CFL.FOTRecId IN (', sFOTRecId, ')', sCustTypeCondition, '
            GROUP BY R.RetailerRecId ORDER BY R.BusinessName');

        PREPARE stmt FROM @Query;
        EXECUTE stmt;
        DEALLOCATE PREPARE stmt;

        IF sCode IN ('DT', 'ALL', 'FUPLD') THEN
            SET @Query = CONCAT('INSERT INTO tmpCustomerDetails (DistributorRecId, Code)
                SELECT D.DistributorRecId, "DT"
                FROM Distributor AS D
                LEFT JOIN CustomerFOTLink AS CFL ON CFL.DistributorRecId = D.DistributorRecId
                WHERE IFNULL(D.IsDeleted, 0) = 0 AND IFNULL(D.IsActive, 0) = 1 AND CFL.FOTRecId IN (', sFOTRecId, ')
                GROUP BY D.DistributorRecId ORDER BY D.BusinessName');

            PREPARE stmt FROM @Query;
            EXECUTE stmt;
            DEALLOCATE PREPARE stmt;
        END IF;
    END IF;

    -- Insert into tmpCustomerRules table
    INSERT INTO tmpCustomerRules (RetailerRecId, DistributorRecId, TerritoryRecId, RegionRecId, ZoneRecId, ConsentMGTRecId, Code)
    SELECT tmp.RetailerRecId, tmp.DistributorRecId, T.TerritoryRecId, T.RegionRecId, RG.ZoneRecId, tmp.ConsentMGTRecId, tmp.Code
    FROM tmpCustomerDetails AS tmp
    LEFT JOIN Retailer AS R ON R.RetailerRecId = tmp.RetailerRecId
    LEFT JOIN Distributor AS D ON D.DistributorRecId = tmp.DistributorRecId
    LEFT JOIN Territory AS T ON T.TerritoryRecId = R.TerritoryRecId OR T.TerritoryRecId = D.TerritoryRecId
    LEFT JOIN Region AS RG ON RG.RegionRecId = T.RegionRecId
    WHERE (tmp.RetailerRecId IS NOT NULL AND R.RetailerRecId IS NOT NULL) 
          OR (tmp.DistributorRecId IS NOT NULL AND D.DistributorRecId IS NOT NULL);

    -- Add index to tmpCustomerRules table
    ALTER TABLE tmpCustomerRules ADD INDEX IX_tmpCustomerRules_TerritoryRecId(TerritoryRecId), ADD INDEX IX_tmpCustomerRules_RegionRecId(RegionRecId), ADD INDEX IX_tmpCustomerRules_ZoneRecId(ZoneRecId);

    -- Handle IsAccepted Condition
    IF sFilterStatus <> '' THEN
        SET sStsCondition = CASE sFilterStatus
                                WHEN 'ACPT' THEN ' AND IsAccepted = 1'
                                WHEN 'NACPT' THEN ' AND IsAccepted = 0'
                                ELSE ''
                            END;
    END IF;

    -- Update temporary table with total values
    SET SQL_SAFE_UPDATES = 0;
    UPDATE tmpConsentMGT AS tmp
    LEFT JOIN CustomerTarget AS CT ON CT.UserRecId = tmp.CustomerRecId AND CT.AppType = tmp.CustomerCode
    ```
    SET TotalTargetkgl = COALESCE(CT.Points, 0), TotalTargetINR = COALESCE(CT.Value, 0)
    WHERE tmp.CustomerRecId IS NOT NULL AND CT.F
