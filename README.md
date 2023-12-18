ji<!-- your.component.html -->
<div>
  <label for="designationFilter">Designation:</label>
  <select id="designationFilter" [(ngModel)]="designationFilter" (change)="externalFilterChanged()">
    <option value="">All</option>
    <!-- Populate with unique designations from your data -->
  </select>

  <label for="activeFilter">Active:</label>
  <select id="activeFilter" [(ngModel)]="activeFilter" (change)="externalFilterChanged()">
    <option value="">All</option>
    <option value="true">Yes</option>
    <option value="false">No</option>
  </select>
</div>

<ag-grid-angular
  [gridOptions]="gridOptions"
  [columnDefs]="columnDefs"
  [rowData]="rowData"
  [isExternalFilterPresent]="isExternalFilterPresent"
  [doesExternalFilterPass]="doesExternalFilterPass"
>
</ag-grid-angular>


..,.


// your.component.ts
export class YourComponent {
  // ...

  // External filters
  designationFilter = '';
  activeFilter = '';

  externalFilterChanged() {
    this.gridOptions.api.onFilterChanged();
  }

  isExternalFilterPresent() {
    return (
      this.designationFilter !== '' ||
      this.activeFilter !== ''
    );
  }

  doesExternalFilterPass(node) {
    return (
      (this.designationFilter === '' || node.data.designation === this.designationFilter) &&
      (this.activeFilter === '' || node.data.active === this.activeFilter)
    );
  }
}


....

// your.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-your',
  templateUrl: 'your.component.html',
  styleUrls: ['your.component.css']
})
export class YourComponent {
  // AG Grid options
  gridOptions = {
    // ... other options
  };

  // AG Grid column definitions
  columnDefs = [
    { headerName: 'ID', field: 'id' },
    { headerName: 'Name', field: 'name' },
    { headerName: 'Designation', field: 'designation' },
    { headerName: 'Age', field: 'age' },
    { headerName: 'DOB', field: 'dob' },
    { headerName: 'DOJ', field: 'doj' },
    { headerName: 'Active', field: 'active' },
    // ... other columns
  ];

  // Sample data
  rowData = [
    // Your data here
  ];
}
.,.



const dateFilterParams = {
      greaterThanOrEqual: this.fromDate,
      lessThanOrEqual: this.toDate,
      filterType: 'date',
    };




// YourComponent.ts

import { Component } from '@angular/core';
import { GridOptions } from 'ag-grid-community';

@Component({
  // component details...
})
export class YourComponent {
  gridOptions: GridOptions;
  rowData: any[]; // Your data array
  fromDate: Date;
  toDate: Date;

  // Set up grid options, columns, and other configurations...

  onGridReady(params) {
    // Initialize grid options, set data, etc.
    this.gridOptions = params.api;
    this.gridOptions.setRowData(this.rowData);
  }

  filterByDateRange() {
    const dateFilterParams = {
      greaterThanOrEqual: this.fromDate,
      lessThanOrEqual: this.toDate,
      filterType: 'date',
    };

    this.gridOptions.setFilterModel({
      dob: dateFilterParams,
    });
    this.gridOptions.onFilterChanged();
  }
}



