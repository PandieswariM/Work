SELECT
    ll.leaverecid,
    ll.noofleave,
    ll.comments,
    d.durationtype,
    lt.leavetype,
    CASE
        WHEN lt.leavetype = 'CL' THEN 5 - COALESCE(SUM(ll.noofleave) OVER (PARTITION BY ll.employeeid, lt.leavetype ORDER BY ll.fromdate), 0)
        WHEN lt.leavetype = 'LOP' THEN 5 - COALESCE(SUM(ll.noofleave) OVER (PARTITION BY ll.employeeid, lt.leavetype ORDER BY ll.fromdate), 0)
    END AS balanceleave
FROM
    leave_log ll
JOIN
    duration d ON ll.durationrecid = d.durationrecid
JOIN
    leavetype lt ON ll.leavetyperecid = lt.leavetyperecid
WHERE
    ll.employeeid = 101;
    
....


SELECT
    ll.leaverecid,
    ll.noofleave,
    ll.comments,
    d.durationtype,
    lt.leavetype,
    CASE
        WHEN lt.leavetype = 'CL' THEN 5 - COALESCE((
            SELECT SUM(noofleave)
            FROM leave_log sub_ll
            WHERE sub_ll.employeeid = ll.employeeid
                AND sub_ll.leavetyperecid = lt.leavetyperecid
                AND sub_ll.fromdate <= ll.fromdate
        ), 0)
        WHEN lt.leavetype = 'LOP' THEN 5 - COALESCE((
            SELECT SUM(noofleave)
            FROM leave_log sub_ll
            WHERE sub_ll.employeeid = ll.employeeid
                AND sub_ll.leavetyperecid = lt.leavetyperecid
                AND sub_ll.fromdate <= ll.fromdate
        ), 0)
    END AS balanceleave
FROM
    leave_log ll
JOIN
    duration d ON ll.durationrecid = d.durationrecid
JOIN
    leavetype lt ON ll.leavetyperecid = lt.leavetyperecid
WHERE
    ll.employeeid = 101;
    
