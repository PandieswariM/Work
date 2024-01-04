// Import necessary modules
import { Component } from '@angular/core';

@Component({
  selector: 'app-your-grid',
  templateUrl: './your-grid.component.html',
  styleUrls: ['./your-grid.component.css']
})
export class YourGridComponent {
  // Sample data
  rowData = [
    { id: 1, name: 'John', graduation: 'B.Sc', year: 2022, dob: '1990-01-01' },
    // Add more rows as needed
  ];

  // Column definitions
  columnDefs = [
    { headerName: 'ID', field: 'id' },
    { headerName: 'Name', field: 'name' },
    { headerName: 'Graduation', field: 'graduation' },
    { headerName: 'Year', field: 'year', hide: true }, // Initially hidden
    { headerName: 'DOB', field: 'dob' }
  ];

  // Flag to control column visibility
  isYearActive = true;

  // Toggle function to switch column visibility
  toggleYearColumn() {
    this.isYearActive = !this.isYearActive;
    this.columnDefs.find(column => column.field === 'year').hide = !this.isYearActive;
  }
}
