// Add a property to track selected row(s)
public selectedRows: any[] = [];

// ...

// Update selectedRows property when row selection changes
onSelectionChanged(event: any): void {
    this.selectedRows = this.EmployeeGridApi.getSelectedRows();
}

// ...

// In your ngOnInit or constructor, set up a selectionChanged event listener
ngOnInit() {
    // ... (your existing code)

    // Add a selectionChanged event listener
    this.EmployeeGridOptions.onSelectionChanged = this.onSelectionChanged.bind(this);
}

// ...

// In your HTML, use the selectedRows property to conditionally disable the button
<button class="btn btn-success btn-sm update-button" [disabled]="shouldDisableButton()">Update</button>

// ...

// Add a method to check if the button should be disabled
shouldDisableButton(): boolean {
    // Check if there are selected rows and if the 'Active' value is 'Yes'
    return this.selectedRows.length === 1 && this.selectedRows[0].Active === 'Yes';
}
-->

UpdateCellrenderer(params: any): string {
    const noOfLeave = params.data.Noofleave || 0;
    const isButtonDisabled = noOfLeave > 10 ? 'disabled' : '';

    return `<button class="btn btn-success btn-sm update-button" ${isButtonDisabled}>Update</button>`;
}




DELIMITER //

CREATE PROCEDURE GetLeaveBalance(IN employeeIdParam INT, IN yearParam INT)
BEGIN
    SELECT
        ll.leaverecid,
        ll.noofleave,
        ll.comments,
        d.durationtype,
        lt.leavetype,
        CASE
            WHEN lt.leavetype = 'CL' THEN 5 - IFNULL((
                SELECT SUM(noofleave)
                FROM leave_log sub_ll
                WHERE sub_ll.employeeid = ll.employeeid
                    AND sub_ll.leavetyperecid = lt.leavetyperecid
                    AND sub_ll.fromdate <= ll.fromdate
                    AND YEAR(sub_ll.fromdate) = yearParam
            ), 0)
            WHEN lt.leavetype = 'LOP' THEN 5 - IFNULL((
                SELECT SUM(noofleave)
                FROM leave_log sub_ll
                WHERE sub_ll.employeeid = ll.employeeid
                    AND sub_ll.leavetyperecid = lt.leavetyperecid
                    AND sub_ll.fromdate <= ll.fromdate
                    AND YEAR(sub_ll.fromdate) = yearParam
            ), 0)
        END AS balanceleave
    FROM
        leave_log ll
    JOIN
        duration d ON ll.durationrecid = d.durationrecid
    JOIN
        leavetype lt ON ll.leavetyperecid = lt.leavetyperecid
    WHERE
        ll.employeeid = employeeIdParam AND YEAR(ll.fromdate) = yearParam;
END //

DELIMITER ;




SELECT *
FROM your_table_name
WHERE YEAR(dob) = 2000;




import { DatePipe } from '@angular/common';

const datePipe = new DatePipe('en-US');
const inputDate = new Date('25/12/2023');

const formattedDate = datePipe.transform(inputDate, 'yyyy-MM-dd');
console.log(formattedDate);  // Output: 2023-12-25
