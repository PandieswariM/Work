<form (ngSubmit)="onSubmit()" #uploadForm="ngForm">
  <input type="file" name="excelFile" (change)="onFileChange($event)" />
  <button type="submit">Upload</button>
</form>



import { Component } from '@angular/core';

@Component({
  selector: 'app-upload',
  templateUrl: './upload.component.html',
  styleUrls: ['./upload.component.css'],
})
export class UploadComponent {
  selectedFile: File;

  onFileChange(event: any): void {
    this.selectedFile = event.target.files[0];
  }

  onSubmit(): void {
    // Send the file to the backend using a service
  }
}


import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({
  providedIn: 'root',
})
export class UploadService {
  constructor(private http: HttpClient) {}

  uploadFile(file: File): Observable<any> {
    const formData: FormData = new FormData();
    formData.append('excelFile', file, file.name);

    return this.http.post<any>('backend-api/upload', formData);
  }
}


onSubmit(): void {
  if (this.selectedFile) {
    this.uploadService.uploadFile(this.selectedFile).subscribe(
      (response) => {
        console.log('File uploaded successfully', response);
      },
      (error) => {
        console.error('Error uploading file', error);
      }
    );
  }
}



use Slim\Http\Request;
use Slim\Http\Response;

$app->post('/upload', function (Request $request, Response $response, array $args) {
    $uploadedFile = $request->getUploadedFiles()['excelFile'];
    
    // Process the uploaded file as needed (e.g., read content)
    
    return $response->withJson(['status' => 'success']);
});



use Slim\Http\Request;
use Slim\Http\Response;
use PHPExcel_IOFactory;

$app->post('/upload', function (Request $request, Response $response, array $args) {
    $uploadedFile = $request->getUploadedFiles()['excelFile'];

    // Check if a file was uploaded
    if ($uploadedFile->getError() === UPLOAD_ERR_OK) {
        // Save the uploaded file to a temporary location
        $tempFilePath = 'path/to/temp/folder/' . $uploadedFile->getClientFilename();
        $uploadedFile->moveTo($tempFilePath);

        // Include PHPExcel classes
        require 'vendor/autoload.php';

        // Read the Excel file using PHPExcel
        $inputFileType = PHPExcel_IOFactory::identify($tempFilePath);
        $objReader = PHPExcel_IOFactory::createReader($inputFileType);
        $objPHPExcel = $objReader->load($tempFilePath);

        // Access data from the sheet as needed
        $sheet = $objPHPExcel->getActiveSheet();
        $data = [];
        foreach ($sheet->getRowIterator() as $row) {
            $data[] = $row->getValues();
        }

        // Do something with $data (e.g., process, store, or return it)

        // Delete the temporary file
        unlink($tempFilePath);

        return $response->withJson(['status' => 'success', 'data' => $data]);
    } else {
        return $response->withJson(['status' => 'error', 'message' => 'File upload error']);
    }
});




....

import { Component } from '@angular/core';

@Component({
  selector: 'app-your-component',
  templateUrl: './your-component.component.html',
  styleUrls: ['./your-component.component.css']
})
export class YourComponent {
  onFileChange(event: any): void {
    const file = event.target.files[0];

    if (file) {
      this.saveFileToLocalFolder(file);
    }
  }

  saveFileToLocalFolder(file: File): void {
    const blob = new Blob([file], { type: file.type });
    const link = document.createElement('a');

    link.href = window.URL.createObjectURL(blob);
    link.download = file.name;

    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  }
}




.....


npm install xlsx

// component.ts
import * as XLSX from 'xlsx';

onFileChange(event: any) {
  const file = event.target.files[0];
  const reader = new FileReader();

  reader.onload = (e: any) => {
    const workbook = XLSX.read(e.target.result, { type: 'binary' });
    const sheetName = workbook.SheetNames[0];
    const sheet = workbook.Sheets[sheetName];
    const sheetData = XLSX.utils.sheet_to_json(sheet, { header: 1 });

    // Now you can send sheetData to the server
    this.httpClient.post('your-server-url/upload', { sheetData }).subscribe(response => {
      console.log(response);
    });
  };

  reader.readAsBinaryString(file);
}


// index.php
use Slim\Psr7\Request;
use Slim\Psr7\Response;
require_once 'vendor/autoload.php'; // adjust the path as needed

$app->post('/upload', function (Request $request, Response $response) {
    $uploadedFile = $request->getParsedBody()['sheetData'];

    // Now $uploadedFile contains the sheet data, and you can process it using PHPExcel
    require_once 'path/to/PHPExcel/Classes/PHPExcel/IOFactory.php';

    $objPHPExcel = PHPExcel_IOFactory::createReader('Excel2007')->load($uploadedFile);

    // Process $objPHPExcel as needed

    return $response->withJson(['success' => true, 'data' => $processedData]);
});

