```
<div ng-app="myApp" ng-controller="myController">
    <input type="text" tabindex="1" ng-model="name" placeholder="Name" ng-keydown="moveFocus($event, 'dropdown1')">
    
    <select id="dropdown1" tabindex="2" ng-model="dropdown1" ng-keydown="moveFocus($event, 'dropdown2')">
        <option value="" disabled selected>Select an option</option>
        <option value="1">Option 1</option>
        <option value="2">Option 2</option>
    </select>
    
    <select id="dropdown2" tabindex="3" ng-model="dropdown2">
        <option value="" disabled selected>Select an option</option>
        <option value="A">Option A</option>
        <option value="B">Option B</option>
    </select>
</div>
```
```
angular.module('myApp', [])
.controller('myController', function($scope, $timeout) {
    $scope.moveFocus = function(event, nextFieldId) {
        if (event.key === "Enter") { // Capture Enter key
            $timeout(function() {
                document.getElementById(nextFieldId).focus();
            });
            event.preventDefault();
        }
    };
});
```
