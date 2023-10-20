# Work


https://chat.openai.com/share/75b4dcfc-d1c5-4ef2-92a7-0801c74bb90e



https://chat.openai.com/share/2b7b989b-2788-4d33-bc90-54ff62080efa



https://chat.openai.com/share/320a9613-e1ec-44ac-8dc6-233b49a1b476

https://technicaaadda.blogspot.com/2020/04/code-for-aggrid-integration.html?m=1




https://chat.openai.com/share/93461722-25e7-4f88-9daf-fcacd065323f


https://chat.openai.com/share/93461722-25e7-4f88-9daf-fcacd065323f



https://chat.openai.com/share/e6c49d0b-b62e-4aa2-a503-6573eccb3adf





https://chat.openai.com/share/aa6f21ce-0a05-4040-a93d-946ce6ad85a6



https://chat.openai.com/share/e5c49263-c50c-42af-b65e-d219b092d5bb



https://chat.openai.com/share/6e0cd0b0-42ec-4390-999c-82f7cf197f46





https://chat.openai.com/share/2ef59f1e-ad92-4d81-ae7d-6ea6d882862e
https://chat.openai.com/share/2ef59f1e-ad92-4d81-ae7d-6ea6d882862e








// app.component.ts

import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  rowData = [
    { make: 'Toyota', model: 'Celica', price: 35000 },
    { make: 'Ford', model: 'Mondeo', price: 32000 },
    { make: 'Porsche', model: 'Boxter', price: 72000 }
  ];

  columnDefs = [
    { field: 'make', cellClass: this.getHighlightClass.bind(this) },
    { field: 'model', cellClass: this.getHighlightClass.bind(this) },
    { field: 'price', cellClass: this.getHighlightClass.bind(this) }
  ];

  gridOptions = {
    rowData: this.rowData,
    columnDefs: this.columnDefs,
    onGridReady: (params: any) => {
      params.api.sizeColumnsToFit();
    },
    onFirstDataRendered: (params: any) => {
      params.api.sizeColumnsToFit();
    }
  };

  getHighlightClass(params: any) {
    const searchText = this.searchText.toLowerCase();
    if (searchText && params.value.toString().toLowerCase().indexOf(searchText) >= 0) {
      return 'highlight';
    }
    return '';
  }

  searchText = '';

  onSearchChange() {
    this.gridOptions.api.setQuickFilter(this.searchText);
  }
}


https://chat.openai.com/share/f04da005-b0b5-45c7-81e2-349d5010176f



import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  rowData = [
    { make: 'Toyota', model: 'Celica', price: 35000 },
    { make: 'Ford', model: 'Mondeo', price: 32000 },
    { make: 'Porsche', model: 'Boxter', price: 72000 }
  ];

  columnDefs = [
    { field: 'make', cellRenderer: this.customCellRenderer },
    { field: 'model', cellRenderer: this.customCellRenderer },
    { field: 'price', cellRenderer: this.customCellRenderer }
  ];

  gridOptions = {
    rowData: this.rowData,
    columnDefs: this.columnDefs
  };

  onSearchChange(event: any) {
    const searchText = event.target.value;
    this.gridOptions.api.setQuickFilter(searchText);
  }

  customCellRenderer(params: any) {
    const searchText = params.context.searchText;
    const cellValue = params.value;
    if (searchText && cellValue.toLowerCase().includes(searchText.toLowerCase())) {
      return `<span class="highlight">${cellValue}</span>`;
    }
    return cellValue;
  }
}



......






<input type="text" (input)="onSearchChange($event)" placeholder="Search...">
<ag-grid-angular
    style="width: 500px; height: 200px;"
    class="ag-theme-alpine"
    [gridOptions]="gridOptions"
    [context]="{ searchText: searchText }">
</ag-grid-angular>

<div>
  <input [(ngModel)]="searchText" (keyup)="searchValueInGrid()" placeholder="Search...">
</div>
<div style="height: 200px;" class="ag-theme-alpine">
  <ag-grid-angular 
    style="width: 100%;" 
    class="ag-theme-alpine"
    [rowData]="rowData" 
    [columnDefs]="columnDefs"
    [defaultColDef]="defaultColDef"
    (gridReady)="onGridReady($event)">
  </ag-grid-angular>
</div>




import { Component } from '@angular/core';
import { GridApi } from 'ag-grid-community';

@Component({
  selector: 'app-your-component',
  templateUrl: './your-component.component.html',
  styleUrls: ['./your-component.component.css']
})
export class YourComponent {
  private gridApi: GridApi;
  rowData: any[];
  searchText: string = '';

  columnDefs = [
    { headerName: 'Make', field: 'make' },
    { headerName: 'Model', field: 'model' },
    { headerName: 'Price', field: 'price' }
  ];

  defaultColDef = {
    flex: 1,
    minWidth: 100,
    filter: true,
    sortable: true,
    floatingFilter: true
  };

  onGridReady(params: any) {
    this.gridApi = params.api;
  }

  searchValueInGrid() {
    if (this.gridApi && this.searchText !== '') {
      this.gridApi.setQuickFilter(this.searchText);
      this.highlightSearchText();
    } else {
      this.gridApi.setQuickFilter(null);
    }
  }

  highlightSearchText() {
    const filterInstance = this.gridApi.getFilterInstance('ag-TextColumnFilter');
    if (filterInstance) {
      filterInstance.getFrameworkComponentInstance().then((filter: any) => {
        filter.setParams({ applyButton: true });
        filter.onFloatingFilterChanged('onEnterKeyPressed');
      });
    }
  }

  ngOnInit() {
    this.rowData = [
      { make: 'Toyota', model: 'Celica', price: 35000 },
      { make: 'Ford', model: 'Mondeo', price: 32000 },
      { make: 'Porsche', model: 'Boxter', price: 72000 }
      // Additional data entries here
    ];
  }
}


