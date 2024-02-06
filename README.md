@HostListener('document:click', ['$event'])
handleClick(event: Event) {
  const clickedInside = this.elRef.nativeElement.contains(event.target);
  
  if (!clickedInside) {
    // Handle the outside click here
    console.log('Clicked outside the div');
  }
}
