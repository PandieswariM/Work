
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

  selectedItemsText: string = '';

  onCheckboxChange() {
    const selectedItems = this.items.filter(item => item.selected).map(item => item.name);
    this.selectedItemsText = selectedItems.join(', ');
  }
}



<divclass="dropdown">
  <divclass="dropdown-menu" aria-labelledby="dropdownMenuButton" style="min-height: 150px; overflow-y: auto;">
    <divclass="checkbox" *ngFor="let item of items">
      <label>
        <input type="checkbox" [(ngModel)]="item.selected" (change)="onCheckboxChange()"> {{ item.name }}
      </label>
    </div>
  </div>



 ..........
  <inputtype="text" class="form-control" [(ngModel)]="selectedItemsText" readonly>
</div>








..,


import { Component } from '@angular/core';

@Component({
  selector: 'app-your-component',
  templateUrl: './your-component.component.html',
  styleUrls: ['./your-component.component.css']
})
export class YourComponent {
  dropdownOptions = [
    { name: 'Item 1', selected: false },
    { name: 'Item 2', selected: false },
    { name: 'Item 3', selected: false },
    // Add more items as needed
  ];

  showDropdown = false;

  toggleDropdown() {
    this.showDropdown = !this.showDropdown;
  }

  onCheckboxChange(option: any) {
    option.selected = !option.selected;
  }

  getSelectedOptions() {
    return this.dropdownOptions
      .filter(option => option.selected)
      .map(option => option.name)
      .join(', ');
  }

  onSubmit() {
    const selectedNames = this.getSelectedOptions();
    console.log('Selected Names:', selectedNames);
  }
}



<divclass="dropdown">
  <input type="text" [value]="getSelectedOptions()" readonly (click)="toggleDropdown()" />

  <divclass="dropdown-content" *ngIf="showDropdown">
    <div *ngFor="let option of dropdownOptions">
      <label>
        <input type="checkbox" [checked]="option.selected" (change)="onCheckboxChange(option)" />
        {{ option.name }}
        .......
        
      </label>
    </div>
  ......

  </div>
</div>
......



<button(click)="onSubmit()">Submit</button>



.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown-content {
  display: block;
  position: absolute;
  background-color: #f9f9f9;
  min-width: 160px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  z-index: 1;
}


