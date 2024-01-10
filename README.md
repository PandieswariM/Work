// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { LoginComponent } from './login/login.component';
import { EmployeeComponent } from './employee/employee.component';
import { AuthGuard } from './auth.guard';

const routes: Routes = [
  { path: 'login', component: LoginComponent },
  { path: 'employee', component: EmployeeComponent, canActivate: [AuthGuard] },
  { path: '', redirectTo: '/login', pathMatch: 'full' },
  // Other routes...
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { 










<!-- login.component.html -->
<form(ngSubmit)="onSubmit(username.value, password.value)">
  <labelfor="username">Username:</label>
  <input type="text" id="username" #username required>

  <labelfor="password">Password:</label>
  <input type="password" id="password" #password required>

  <buttontype="submit">Login</button>
</form>





// login.component.ts
import { Component } from '@angular/core';
import { Router } from '@angular/router';
import { AuthService } from '../auth.service';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {
  constructor(private authService: AuthService, private router: Router) {}

  onSubmit(username: string, password: string): void {
    if (this.authService.login(username, password)) {
      this.router.navigate(['/employee']);
    } else {
      // Handle unsuccessful login (e.g., show an error message)
    }
  }
}





// auth.guard.ts
import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot): boolean {
    const isLoggedIn = this.authService.isLoggedIn();

    if (!isLoggedIn) {
      this.router.navigate(['/login']);
      return false;
    }
    return true;
  }
}



// auth.service.ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  private loggedIn = false;

  login(username: string, password: string): boolean {
    // Your authentication logic here
    // For simplicity, let's assume successful login for any non-empty username and password
    this.loggedIn = username.trim() !== '' && password.trim() !== '';
    return this.loggedIn;
  }

  isLoggedIn(): boolean {
    return this.loggedIn;
  }
}








..,.




import { Component } from '@angular/core';

@Component({
  selector: 'app-your-grid',
  templateUrl: './your-grid.component.html',
  styleUrls: ['./your-grid.component.css']
})
export class YourGridComponent {
  // Your column definitions
  columnDefs = [
    { headerName: 'Column 1', field: 'col1' },
    { headerName: 'Column 2', field: 'col2' },
    // Add more columns as needed
  ];

  // Your row data
  rowData = [
    { col1: 'Value 1', col2: 'Value 2' },
    // Add more rows as needed
  ];

  // Your grid options
  gridOptions = {
    // Other grid options...

    // Define row class rules
    rowClassRules: {
      'last-row': (params) => {
        const lastRowIndex = params.api.getDisplayedRowCount() - 1;
        return params.rowIndex === lastRowIndex;
      },
    },
  };
}



.last-row {
  background-color: lightgray;  // Set your preferred background color
  position: sticky;
  bottom: 0;
  z-index: 1;
}
