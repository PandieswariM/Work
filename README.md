// your-service.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class YourService {
  constructor(private http: HttpClient) {}

  getDataForItem(item: any): Observable<any> {
    // Adjust your API endpoint or data fetching logic
    return this.http.get<any>(`your-api-endpoint/${item}`);
  }
}




// your-component.component.ts
import { Component, OnInit } from '@angular/core';
import { YourService } from './your-service.service';
import { forkJoin } from 'rxjs';

@Component({
  selector: 'app-your-component',
  templateUrl: './your-component.component.html',
  styleUrls: ['./your-component.component.css'],
})
export class YourComponent implements OnInit {
  results: any[];

  constructor(private yourService: YourService) {}

  ngOnInit(): void {
    const observables = [];

    // Assume you have an array of items to iterate over
    const items = [/* your array of items */];

    items.forEach((item) => {
      // Create an Observable for each item
      const observable = this.yourService.getDataForItem(item);

      // Add the Observable to the array
      observables.push(observable);
    });

    // Use forkJoin to wait for all Observables to complete
    forkJoin(observables).subscribe(
      (results) => {
        // Handle the results here
        this.results = results;
        console.log(results);
      },
      (error) => {
        // Handle errors here
        console.error(error);
      }
    );
  }
}




// app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';

import { YourComponent } from './your-component/your-component.component';
import { YourService } from './your-component/your-service.service';

@NgModule({
  declarations: [
    YourComponent,
    // Other components here
  ],
  imports: [
    BrowserModule,
    HttpClientModule,
    // Other modules here
  ],
  providers: [YourService],
  bootstrap: [YourComponent],
})
export class AppModule {}
