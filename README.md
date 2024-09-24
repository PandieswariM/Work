To validate a URL when a user clicks an `<a>` tag in AngularJS and handle the link accordingly (either opening the link if valid or showing a toast message if invalid), follow these steps:

### Step 1: Create a method in the controller to validate the URL and handle the click
You can create a method in your AngularJS controller that will validate the URL using a regular expression and perform the action based on the result.

### Example:

#### HTML:
```html
<!DOCTYPE html>
<html ng-app="myApp">
<head>
  <title>URL Validation with Toast in AngularJS</title>
  <!-- Include any required CSS for toast (you can use Angular Material or any other toast library) -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/angular-material/1.1.12/angular-material.min.css">
</head>
<body ng-controller="LinkController">

  <a href="#" ng-click="validateAndOpenLink('https://example.com')">Click here to open URL</a>

  <!-- Toast notification container -->
  <md-toast ng-if="showToast">
    <span>{{ toastMessage }}</span>
  </md-toast>

  <!-- AngularJS and Angular Material scripts -->
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.7.9/angular.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/angular-material/1.1.12/angular-material.min.js"></script>

  <script>
    angular.module('myApp', ['ngMaterial'])
      .controller('LinkController', function($scope, $window, $timeout, $mdToast) {
        
        // Regular expression for validating URLs
        const urlPattern = /^(https?:\/\/)?([\da-z.-]+)\.([a-z.]{2,6})([/\w .-]*)*\/?$/;

        // Method to validate and open link
        $scope.validateAndOpenLink = function(url) {
          if (urlPattern.test(url)) {
            // If URL is valid, open it in a new tab
            $window.open(url, '_blank');
          } else {
            // If URL is invalid, show a toast message
            $scope.toastMessage = 'Invalid URL';
            $scope.showToast = true;

            // Hide toast after 3 seconds
            $timeout(function() {
              $scope.showToast = false;
            }, 3000);
          }
        };
      });
  </script>
</body>
</html>
```

### Explanation:
1. **ng-click**: The `<a>` tag uses `ng-click` to call the `validateAndOpenLink` method and passes the URL to validate.
   
2. **validateAndOpenLink**:
    - It uses a regular expression (`urlPattern`) to validate the URL.
    - If valid, it opens the URL in a new tab using `$window.open(url, '_blank')`.
    - If invalid, it displays a toast message using `$mdToast`.

3. **Toast Notification**:
    - Angular Material's toast (`<md-toast>`) is displayed for 3 seconds when the URL is invalid.

4. **Libraries**:
   - AngularJS and Angular Material are included for basic toast notifications. You can replace Angular Material with any other toast library if preferred.

This implementation allows you to handle URL validation and display a toast message for invalid URLs, all while preventing the invalid link from opening.
