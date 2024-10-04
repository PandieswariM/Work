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
