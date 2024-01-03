// Add a property to track selected row(s)
public selectedRows: any[] = [];

// ...

// Update selectedRows property when row selection changes
onSelectionChanged(event: any): void {
    this.selectedRows = this.EmployeeGridApi.getSelectedRows();
}

// ...

// In your ngOnInit or constructor, set up a selectionChanged event listener
ngOnInit() {
    // ... (your existing code)

    // Add a selectionChanged event listener
    this.EmployeeGridOptions.onSelectionChanged = this.onSelectionChanged.bind(this);
}

// ...

// In your HTML, use the selectedRows property to conditionally disable the button
<button class="btn btn-success btn-sm update-button" [disabled]="shouldDisableButton()">Update</button>

// ...

// Add a method to check if the button should be disabled
shouldDisableButton(): boolean {
    // Check if there are selected rows and if the 'Active' value is 'Yes'
    return this.selectedRows.length === 1 && this.selectedRows[0].Active === 'Yes';
}
-->

UpdateCellrenderer(params: any): string {
    const noOfLeave = params.data.Noofleave || 0;
    const isButtonDisabled = noOfLeave > 10 ? 'disabled' : '';

    return `<button class="btn btn-success btn-sm update-button" ${isButtonDisabled}>Update</button>`;
}
