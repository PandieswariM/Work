import { Component } from '@angular/core';

@Component({
  selector: 'app-date-picker',
  templateUrl: './date-picker.component.html',
  styleUrls: ['./date-picker.component.css']
})
export class DatePickerComponent {
  startDate: Date;
  endDate: Date;
  weekdaysCount: number;

  calculateWeekdays(): void {
    if (this.startDate && this.endDate) {
      let count = 0;
      let currentDate = new Date(this.startDate);

      while (currentDate <= this.endDate) {
        const dayOfWeek = currentDate.getDay();

        // Check if the current day is not a weekend (Sunday or Saturday)
        if (dayOfWeek !== 0 && dayOfWeek !== 6) {
          count++;
        }

        // Move to the next day
        currentDate.setDate(currentDate.getDate() + 1);
      }

      this.weekdaysCount = count;
    }
  }
}
