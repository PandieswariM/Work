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
