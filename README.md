....


-- Create leavetype table
CREATE TABLE leavetype (
    leavetyperecid INT PRIMARY KEY,
    leavetype VARCHAR(50) NOT NULL
);

-- Insert data into leavetype table
INSERT INTO leavetype (leavetyperecid, leavetype) VALUES
(1, 'CL'),
(2, 'LOP');

-- Create duration table
CREATE TABLE duration (
    durationrecid INT PRIMARY KEY,
    durationtype VARCHAR(50) NOT NULL
);

-- Insert data into duration table
INSERT INTO duration (durationrecid, durationtype) VALUES
(1, 'Full-day'),
(2, 'Half day morning'),
(3, 'Half day evening');

-- Create leave_log table with foreign key references
CREATE TABLE leave_log (
    leaverecid INT PRIMARY KEY,
    durationrecid INT,
    employeeid INT,
    leavetyperecid INT,
    noofleave INT,
    comments VARCHAR(255),
    fromdate DATE,
    todate DATE,
    FOREIGN KEY (durationrecid) REFERENCES duration(durationrecid),
    FOREIGN KEY (leavetyperecid) REFERENCES leavetype(leavetyperecid)
);

-- Create stored procedure to insert a row if it doesn't exist
DELIMITER //

CREATE PROCEDURE InsertLeaveLog(
    IN leaverecid_param INT,
    IN durationrecid_param INT,
    IN employeeid_param INT,
    IN leavetyperecid_param INT,
    IN noofleave_param INT,
    IN comments_param VARCHAR(255),
    IN fromdate_param DATE,
    IN todate_param DATE
)
BEGIN
    DECLARE row_count INT;

    -- Check if the record already exists
    SELECT COUNT(*) INTO row_count
    FROM leave_log
    WHERE employeeid = employeeid_param
        AND durationrecid = durationrecid_param
        AND ((fromdate_param BETWEEN fromdate AND todate) OR (todate_param BETWEEN fromdate AND todate));

    -- Insert the record if it doesn't exist
    IF row_count = 0 THEN
        INSERT INTO leave_log (leaverecid, durationrecid, employeeid, leavetyperecid, noofleave, comments, fromdate, todate)
        VALUES (leaverecid_param, durationrecid_param, employeeid_param, leavetyperecid_param, noofleave_param, comments_param, fromdate_param, todate_param);
    END IF;
END //

DELIMITER ;
