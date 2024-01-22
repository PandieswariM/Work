// your.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-your-component',
  template: `
    <ag-grid-angular
      [rowData]="yourData"
      [columnDefs]="yourColumnDefs"
    ></ag-grid-angular>
  `,
})
export class YourComponent {
  yourData: any[]; // Your data array
  yourColumnDefs: any[]; // Your column definitions

  constructor() {
    this.yourColumnDefs = [
      {
        headerName: 'Your Header',
        field: 'total',
        cellStyle: {
          'text-align': 'left', // Align text as needed
        },
        valueFormatter: (params) => {
          const totalText = params.data.total.split(':')[0];
          const value = params.data.total.split(':')[1].trim();
          return `<span style="color: red;">${totalText}</span>: <span style="color: black;">${value}</span>`;
        },
      },
      // Add other columns as needed
    ];
  }
}
