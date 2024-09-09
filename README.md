```
<!DOCTYPE html>
<html ng-app="myApp">
<head>
    <title>Cordova AngularJS App</title>
    <script src="cordova.js"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
    <script src="app.js"></script> <!-- Your AngularJS app file -->
</head>
<body ng-controller="SettingsController">
    <button ng-click="openLocationSettings()">Open Location Settings</button>
</body>
</html>
```

```
var app = angular.module('myApp', []);

app.controller('SettingsController', ['$scope', function($scope) {
    $scope.openLocationSettings = function() {
        if (window.cordova) {
            if (cordova.platformId === 'android') {
                window.location.href = 'intent:#Intent;action=android.settings.LOCATION_SOURCE_SETTINGS;end';
            } else if (cordova.platformId === 'ios') {
                window.open('app-settings:');
            } else {
                alert("This function is only available on Android and iOS.");
            }
        } else {
            alert("Cordova is not available. Please run this on a real device.");
        }
    };
}]);

document.addEventListener('deviceready', function() {
    angular.bootstrap(document.body, ['myApp']);
}, false);
```
