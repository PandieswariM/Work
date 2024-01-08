
<divclass="dropdown">
  <buttonclass="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    Select Items
  </button>
  <divclass="dropdown-menu" aria-labelledby="dropdownMenuButton" style="min-height: 150px; overflow-y: auto;">
    <div class="checkbox" *ngFor="let item of items">
      <label>
        <input type="checkbox" [(ngModel)]="item.selected"> {{ item.name }}
      </label>
    </div>
  </div>
  <buttonclass="btn btn-primary" (click)="onSubmit()">Submit</button>
</div>

.........


import { Component } from '@angular/core';

@Component({
  selector: 'app-dropdown',
  templateUrl: './dropdown.component.html',
  styleUrls: ['./dropdown.component.css']
})
export class DropdownComponent {
  items = [
    { name: 'Item 1', selected: false },
    { name: 'Item 2', selected: false },
    { name: 'Item 3', selected: false },
    // Add more items as needed
  ];

  onSubmit() {
    const selectedItems = this.items.filter(item => item.selected).map(item => item.name);
    console.log('Selected Items:', selectedItems);
  }
}

