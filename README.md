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
  DECLARE stmt TEXT;
  
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

  -- Fetch the code
  SELECT Code INTO sCode
  FROM ConsentMGT AS CM
  LEFT JOIN ConsentMGTCustType AS CMT ON CMT.ConsentMGTCustTypeRecId = CM.ConsentMGTCustTypeRecId
  WHERE CM.ConsentMGTRecId = iConsentMGTRecId;

  -- Check if all locations
  SELECT COUNT(*) INTO bIsAllLocation
  FROM ConsentMGTRule
  WHERE ConsentMGTRecId = iConsentMGTRecId AND IFNULL(IsAllLocation, 0) = 1;

  -- Set customer type condition based on the code
  IF IFNULL(sCode, '') NOT IN ('DT', 'ALL', 'FUPLD') THEN
    SET sCustTypeCondition = CONCAT(' AND RT.Code IN ("', sCode, '") ');
  END IF;

  -- Set conditions based on app type
  IF sAppType IN ('MDO', 'TSM') THEN
    IF sAppType = 'TSM' THEN
      SELECT fn_APP_GetTerritoryForUser(iAppUserRecId) INTO sTerritoryRecId;
      SET sCustCondition = ' AND CCT.Code = "FUPLD"';
      SET sCondition = ' AND CCT.Code != "FUPLD"';
    ELSEIF sAppType = 'MDO' THEN
      SELECT fn_APP_GetMDOType(iAppUserRecId) INTO sMDOType;
      IF sMDOType = 'FS' THEN
        SELECT fn_APP_GetMDOTerritoryForUser(iAppUserRecId) INTO sTerritoryRecId;
      ELSE
        SELECT fn_APP_GetFOTForUser(iAppUserRecId) INTO sFOTRecId;
      END IF;
      SET sCustCondition = ' AND CCT.Code = "FUPLD"';
      SET sCondition = ' AND (CCT.Code = "ALL")';
    END IF;

    -- Execute dynamic queries based on conditions
    IF sTerritoryRecId != '' THEN
      SET @Query = CONCAT(
        'INSERT INTO tmpCustomerDetails (RetailerRecId, Code, ConsentMGTRecId)
        SELECT R.RetailerRecId, RT.Code, ', iConsentMGTRecId, '
        FROM Retailer AS R
        LEFT JOIN RetailerType AS RT ON R.RetailerTypeRecId = RT.RetailerTypeRecId
        WHERE IFNULL(R.IsDeleted, 0) = 0 
        AND IFNULL(R.IsActive, 0) = 1 
        AND R.TerritoryRecId IN (', sTerritoryRecId, ')', sCustTypeCondition, 
        ' GROUP BY R.RetailerRecId
        ORDER BY R.BusinessName;');

      PREPARE stmt FROM @Query;
      EXECUTE stmt;
      DEALLOCATE PREPARE stmt;
    ELSEIF sFOTRecId != '' THEN
      SET @Query = CONCAT(
        'INSERT INTO tmpCustomerDetails (RetailerRecId, Code)
        SELECT R.RetailerRecId, RT.Code
        FROM Retailer AS R
        LEFT JOIN RetailerType AS RT ON R.RetailerTypeRecId = RT.RetailerTypeRecId
        LEFT JOIN CustomerFOTLink AS CFL ON CFL.RetailerRecId = R.RetailerRecId
        WHERE IFNULL(R.IsDeleted, 0) = 0 
        AND IFNULL(R.IsActive, 0) = 1 
        AND CFL.FOTRecId IN (', sFOTRecId, ')', sCustTypeCondition, 
        ' GROUP BY R.RetailerRecId
        ORDER BY R.BusinessName;');

      PREPARE stmt FROM @Query;
      EXECUTE stmt;
      DEALLOCATE PREPARE stmt;
    END IF;
  END IF;

  -- Add indexes to improve query performance
  ALTER TABLE tmpCustomerDetails ADD INDEX IX_tmpCustomerDetails_RetailerRecId(RetailerRecId);
  ALTER TABLE tmpCustomerDetails ADD INDEX IX_tmpCustomerDetails_DistributorRecId(DistributorRecId);
  ALTER TABLE tmpCustomerRules ADD INDEX IX_tmpCustomerRules_TerritoryRecId(TerritoryRecId);
  ALTER TABLE tmpCustomerRules ADD INDEX IX_tmpCustomerRules_RegionRecId(RegionRecId);
  ALTER TABLE tmpCustomerRules ADD INDEX IX_tmpCustomerRules_ZoneRecId(ZoneRecId);

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

  -- Filter status condition
  IF sFilterStatus <> '' THEN
    IF sFilterStatus = 'ACPT' THEN
      SET sStsCondition = " AND IsAccepted = '1' ";
    ELSEIF sFilterStatus = 'NACPT' THEN
      SET sStsCondition = " AND IsAccepted = '0' ";
    END IF;
  END IF;

  -- Update target values
  SET SQL_SAFE_UPDATES = 0;
  UPDATE tmpConsentMGT AS tmp
  LEFT JOIN CustomerTarget AS CT ON CT.UserRecId = tmp.CustomerRecId AND CT.AppType = tmp.CustomerCode
  SET TotalTargetkgl = CT.Points, TotalTargetINR = CT.Value
  WHERE tmp.CustomerRecId IS NOT NULL AND CT.FinancialYearRecId = iFinancialYearRecId;
  SET SQL_SAFE_UPDATES = 1;

  -- Final result query
  SET @FinalQuery = CONCAT(
    'SELECT CustomerRecId, CustomerName, CustomerID, CustomerType, CustomerCode, Name, IsAccepted, 
    IFNULL(TotalTargetkgl, 0) AS TotalTargetkgl, IFNULL(TotalTargetINR, 0) AS TotalTargetINR 
    FROM tmpConsentMGT 
    WHERE CustomerRecId IS NOT NULL', sStsCondition, 
    ' GROUP BY CustomerRecId 
    ORDER BY IsAccepted DESC, CustomerName ASC');

  PREPARE stmt FROM @FinalQuery;
  EXECUTE stmt;
  DEALLOCATE PREPARE stmt;

  -- Clean up temporary tables
  DROP TEMPORARY TABLE IF EXISTS tmpCustomerDetails;
  DROP TEMPORARY TABLE IF EXISTS tmpCustomerRules;
  DROP TEMPORARY TABLE IF EXISTS tmpConsentMGT;
END$$
DELIMITER ;
```
