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
```
<!-- Numeric Input Field with enterkeyhint -->
<input type="number" inputmode="numeric" enterkeyhint="next" ng-model="numericField" ng-keydown="focusNext($event, 'dropdownField')">

<!-- Dropdown Field -->
<select id="dropdownField" ng-model="dropdownField">
    <option value="">Select an option</option>
    <option value="option1">Option 1</option>
    <option value="option2">Option 2</option>
</select>

<!-- Text Field -->
<input type="text" ng-model="textField">
```
```
$scope.focusNext = function(event, nextFieldId) {
    if (event.key === "Enter") { // Detect the Enter key
        event.preventDefault(); // Prevent the default action
        document.getElementById(nextFieldId).focus(); // Set focus to the dropdown
    }
};
```
