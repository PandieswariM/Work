```
<div layout="column">
  <md-input-container>
    <label>Text Field</label>
    <input type="text" ng-model="userInput" next-focus="#selectField" />
  </md-input-container>

  <md-input-container>
    <label>Select Field</label>
    <md-select id="selectField" ng-model="userSelection">
      <md-option value="1">Option 1</md-option>
      <md-option value="2">Option 2</md-option>
    </md-select>
  </md-input-container>
</div>

<div layout="column">
  <md-input-container>
    <label>Text Field</label>
    <input type="text" ng-model="userInput" next-focus="#selectField" />
  </md-input-container>

  <md-input-container>
    <label>Select Field</label>
    <md-select id="selectField" ng-model="userSelection">
      <md-option value="1">Option 1</md-option>
      <md-option value="2">Option 2</md-option>
    </md-select>
  </md-input-container>
</div>
```

```
<!DOCTYPE html>
<html ng-app="myApp">
<head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
</head>
<body ng-controller="MyController">
    <input type="file" id="imageInput" accept="image/*" onchange="angular.element(this).scope().convertToBase64(this.files)" />
    <br>
    <img ng-if="base64Image" ng-src="{{base64Image}}" alt="Uploaded Image" style="max-width: 300px; max-height: 300px;" />
    <script>
        var app = angular.module('myApp', []);
        app.controller('MyController', ['$scope', function($scope) {
            $scope.convertToBase64 = function(files) {
                if (files.length > 0) {
                    const file = files[0];
                    const reader = new FileReader();
                    
                    reader.onload = function(event) {
                        const base64String = event.target.result;
                        console.log(base64String); // This is your Base64 string
                        $scope.base64Image = base64String;
                        $scope.$apply();
                    };

                    reader.readAsDataURL(file);
                }
            };
        }]);
    </script>
</body>
</html>
```
```
<!DOCTYPE html>
<html lang="en" ng-app="myApp">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AngularJS Table with Template</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular-sanitize.min.js"></script>
    <style>
        /* Add some basic styles */
        table {
            width: 100%;
            border-collapse: collapse;
        }
        th, td {
            padding: 8px;
            text-align: left;
            border: 1px solid #ddd;
        }
    </style>
</head>
<body ng-controller="MainController">

    <h1>AngularJS Table with Template</h1>
    <table>
        <thead>
            <tr>
                <th>Name</th>
                <th>Action</th>
            </tr>
        </thead>
        <tbody>
            <tr ng-repeat="item in items">
                <td>{{ item.name }}</td>
                <td>
                    <div ng-bind-html="template(item)"></div>
                </td>
            </tr>
        </tbody>
    </table>

    <script>
        // Step 3: Create AngularJS Module and Controller
        angular.module('myApp', ['ngSanitize'])
            .controller('MainController', function($scope, $sce) {
                // Sample data
                $scope.items = [
                    { name: 'Item 1' },
                    { name: 'Item 2' },
                    { name: 'Item 3' }
                ];

                // Function to return HTML template for each item
                $scope.template = function(item) {
                    return $sce.trustAsHtml('<div ng-click="handleClick(' + JSON.stringify(item) + ')">Click Me</div>');
                };

                // Function to handle click event
                $scope.handleClick = function(item) {
                    alert('You clicked on ' + item.name);
                };
            });
    </script>
</body>
</html>
```
