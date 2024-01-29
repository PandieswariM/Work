<!-- Your component's template -->
<input type="checkbox" [(ngModel)]="maleChecked" (change)="onCheckboxChange()"> Male
<input type="checkbox" [(ngModel)]="femaleChecked" (change)="onCheckboxChange()"> Female

<!-- Display checkbox state in console -->
{{checkboxState | json}}


...



// Your component's TypeScript file
import { Component } from '@angular/core';

@Component({
  selector: 'app-your-component',
  templateUrl: './your-component.component.html',
  styleUrls: ['./your-component.component.css']
})
export class YourComponent {
  maleChecked = true;
  femaleChecked = true;
  checkboxState = { male: this.maleChecked, female: this.femaleChecked };

  onCheckboxChange() {
    this.checkboxState = { male: this.maleChecked, female: this.femaleChecked };
    console.log(this.checkboxState);
  }
}
