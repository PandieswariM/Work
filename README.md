<!-- my.component.html -->

<div>
  <p>{{ myResponse?.message }}</p>
</div>



// my.component.ts

import { Component, OnInit } from '@angular/core';
import { MyService } from './my.service';

@Component({
  selector: 'app-my',
  templateUrl: './my.component.html',
  styleUrls: ['./my.component.css'],
})
export class MyComponent implements OnInit {
  myResponse: object;

  constructor(private myService: MyService) {}

  ngOnInit(): void {
    this.myService.fetchData().subscribe((response) => {
      this.myResponse = response;
      // Now 'this.myResponse' contains the response object from the backend
    });
  }
}


// my.service.ts

import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class MyService {
  private apiUrl = 'http://your-backend-url/api';

  constructor(private http: HttpClient) {}

  fetchData(): Observable<object> {
    return this.http.get<object>(`${this.apiUrl}/endpoint`);
  }
}


// index.php

require 'vendor/autoload.php';

$app = new \Slim\App();

$app->get('/api/endpoint', function ($request, $response) {
    $data = array(/* your data here */);

    // Encode the data as JSON
    $jsonEncodedData = json_encode($data);

    // Set the appropriate content type
    $response = $response->withHeader('Content-Type', 'application/json');

    // Send the JSON response
    $response->getBody()->write($jsonEncodedData);

    return $response;
});

$app->run();



.....

// index.php

require 'vendor/autoload.php';

$app = new \Slim\App();

$app->post('/api/endpoint', function ($request, $response) {



    $data = json_decode($request->getBody(), true);

    // Process the received data
    $message = isset($data['message']) ? $data['message'] : 'Default Message';

    $responseData = array('message' => $message);
    
    // Encode the data as JSON
    $jsonEncodedData = json_encode($responseData);

    // Set the appropriate content type
    $response = $response->withHeader('Content-Type', 'application/json');

    // Send the JSON response
    $response->getBody()->write($jsonEncodedData);

    return $response;
});

$app->run();


