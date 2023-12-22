import { Component } from '@angular/core';

@Component({
  selector: 'app-date-range-picker',
  templateUrl: './date-range-picker.component.html',
  styleUrls: ['./date-range-picker.component.css']
})
export class DateRangePickerComponent {
  fromDate: Date;
  toDate: Date;

  onFromDateChange(event: any) {
    this.toDate = new Date(event);
  }

  onToDateChange(event: any) {
    this.fromDate = new Date(event);
  }
}




<div>
  <label for="fromDate">From Date:</label>
  <input
    id="fromDate"
    ngxDaterangepickerMd
    [(ngModel)]="fromDate"
    (ngModelChange)="onFromDateChange($event)"
  />
</div>

<div>
  <label for="toDate">To Date:</label>
  <input
    id="toDate"
    ngxDaterangepickerMd
    [(ngModel)]="toDate"
    (ngModelChange)="onToDateChange($event)"
    [minDate]="fromDate"
  />
</div>




-- Modify the procedure to get total leave for all employees
DELIMITER //

CREATE PROCEDURE GetAllEmployeesTotalLeave()
BEGIN
    SELECT 
        e.employeeid,
        e.name,
        e.address,
        COALESCE(SUM(CASE WHEN lt.leavetype = 'CL' THEN ll.noofleave ELSE 0 END), 0) as CLTotalLeave,
        COALESCE(SUM(CASE WHEN lt.leavetype = 'LOP' THEN ll.noofleave ELSE 0 END), 0) as LOPTotalLeave,
        COALESCE(SUM(ll.noofleave), 0) as TotalNoOfLeave
    FROM employee e
    LEFT JOIN leave_log ll ON e.employeeid = ll.employeeid
    LEFT JOIN leavetype lt ON ll.leavetyperecid = lt.leavetyperecid
    GROUP BY e.employeeid, e.name, e.address;
END //

DELIMITER ;




-- Modify the procedure to get total leave for all employees without using GROUP BY
DELIMITER //

CREATE PROCEDURE GetAllEmployeesTotalLeave()
BEGIN
    SELECT 
        e.employeeid,
        e.name,
        e.address,
        CASE WHEN lt.leavetype = 'CL' THEN ll.noofleave ELSE 0 END as CLTotalLeave,
        CASE WHEN lt.leavetype = 'LOP' THEN ll.noofleave ELSE 0 END as LOPTotalLeave,
        ll.noofleave as TotalNoOfLeave
    FROM employee e
    LEFT JOIN leave_log ll ON e.employeeid = ll.employeeid
    LEFT JOIN leavetype lt ON ll.leavetyperecid = lt.leavetyperecid;
END //

DELIMITER ;
