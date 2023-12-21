import { Component } from '@angular/core';

enum DateRange {
  Today = 'today',
  ThisWeek = 'thisWeek',
  ThisMonth = 'thisMonth',
  ThisYear = 'thisYear',
}

interface DateRangeOption {
  label: string;
  value: DateRange;
  getDateRange: () => { startDate: Date; endDate: Date };
}

@Component({
  selector: 'app-date-range-dropdown',
  template: `
    <label for="dateRange">Select Date Range:</label>
    <select id="dateRange" [(ngModel)]="selectedRange">
      <option *ngFor="let option of dateRangeOptions" [value]="option.value">{{ option.label }}</option>
    </select>

    <div *ngIf="selectedRange">
      <p>Start Date: {{ selectedOption.startDate | date:'mediumDate' }}</p>
      <p>End Date: {{ selectedOption.endDate | date:'mediumDate' }}</p>
    </div>
  `,
})
export class DateRangeDropdownComponent {
  selectedRange: DateRange;
  
  dateRangeOptions: DateRangeOption[] = [
    { label: 'Today', value: DateRange.Today, getDateRange: this.getTodayRange.bind(this) },
    { label: 'This Week', value: DateRange.ThisWeek, getDateRange: this.getWeekRange.bind(this) },
    { label: 'This Month', value: DateRange.ThisMonth, getDateRange: this.getMonthRange.bind(this) },
    { label: 'This Year', value: DateRange.ThisYear, getDateRange: this.getYearRange.bind(this) },
  ];

  getTodayRange(): { startDate: Date; endDate: Date } {
    const today = new Date();
    return { startDate: today, endDate: today };
  }

  getWeekRange(): { startDate: Date; endDate: Date } {
    const today = new Date();
    const startDate = new Date(today);
    startDate.setDate(today.getDate() - today.getDay());
    const endDate = new Date(startDate);
    endDate.setDate(startDate.getDate() + 6);
    return { startDate, endDate };
  }

  getMonthRange(): { startDate: Date; endDate: Date } {
    const today = new Date();
    const startDate = new Date(today.getFullYear(), today.getMonth(), 1);
    const endDate = new Date(today.getFullYear(), today.getMonth() + 1, 0);
    return { startDate, endDate };
  }

  getYearRange(): { startDate: Date; endDate: Date } {
    const today = new Date();
    const startDate = new Date(today.getFullYear(), 0, 1);
    const endDate = new Date(today.getFullYear(), 11, 31);
    return { startDate, endDate };
  }

  get selectedOption(): DateRangeOption {
    return this.dateRangeOptions.find(option => option.value === this.selectedRange);
  }
}


..,...

ALTER TABLE ea_employee
MODIFY COLUMN noofleave DECIMAL(10,2);


ALTER TABLE ea_employee
MODIFY COLUMN noofleave FLOAT;
