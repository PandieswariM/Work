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
