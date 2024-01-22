// your-grid.component.ts
import { Component } from '@angular/core';
import { AgGridAngular } from 'ag-grid-angular';

@Component({
  selector: 'app-your-grid',
  template: `
    <ag-grid-angular
      style="width: 100%; height: 400px;"
      class="ag-theme-alpine"
      [rowData]="rowData"
      [columnDefs]="columnDefs"
      (gridReady)="onGridReady($event)"
      [domLayout]="autoHeight"
    ></ag-grid-angular>
  `,
})
export class YourGridComponent {
  rowData = [
    { sno: 1, employeeName: 'John Doe', id: 101, age: 25 },
    // Add more data as needed
  ];

  columnDefs = [
    { headerName: 'SNO', field: 'sno' },
    { headerName: 'Employee Name', field: 'employeeName' },
    {
      headerName: 'ID',
      field: 'id',
      cellStyle: { color: 'red' },
      tooltipField: (params) => {
        // Check if it's the bottom pinned row
        if (params.node.rowPinned === 'bottom') {
          return `Total ID: ${params.value}`;
        } else {
          return `Individual ID: ${params.value}`;
        }
      },
    },
    { headerName: 'Age', field: 'age', tooltipField: 'age' },
    // Add more columns as needed
  ];

  autoHeight = 'autoHeight';

  onGridReady(params: any) {
    params.api.sizeColumnsToFit();

    // Add the total row
    const totalRow = {
      sno: 'Total:',
      employeeName: '',
      id: this.rowData.length,
      age: '',
    };

    // Add the total row to the end of the row data
    this.rowData.push(totalRow);

    // Pin the total row to the bottom
    params.api.setPinnedBottomRowData([totalRow]);
  }
}
