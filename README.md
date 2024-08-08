```
<!DOCTYPE html>
<html ng-app="myApp">
<head>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/angular-material/1.1.12/angular-material.min.css">
</head>
<body ng-controller="MainController">

  <md-button ng-click="showDialog($event)">Open Dialog</md-button>

  <script type="text/ng-template" id="dialogTemplate.html">
    <md-dialog aria-label="List dialog" ng-cloak>
      <md-dialog-content>
        <h2 class="md-title">Search Items</h2>
        <md-autocomplete
          md-selected-item="dialog.selectedItem"
          md-search-text="dialog.searchText"
          md-items="item in dialog.querySearch(dialog.searchText)"
          md-item-text="item.display"
          md-min-length="0"
          placeholder="Search for an item">
          <md-item-template>
            <span>{{item.display}}</span>
          </md-item-template>
          <md-not-found>
            No matches found.
          </md-not-found>
        </md-autocomplete>
      </md-dialog-content>
      <md-dialog-actions>
        <md-button ng-click="dialog.closeDialog()">Close</md-button>
      </md-dialog-actions>
    </md-dialog>
  </script>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.6.9/angular.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/angular-animate/1.6.9/angular-animate.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/angular-aria/1.6.9/angular-aria.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/angular-material/1.1.12/angular-material.min.js"></script>
  <script src="app.js"></script>

</body>
</html>
```
```
angular.module('myApp', ['ngMaterial'])
  .controller('MainController', function($scope, $mdDialog) {
    $scope.showDialog = function(ev) {
      $mdDialog.show({
        controller: DialogController,
        templateUrl: 'dialogTemplate.html',
        parent: angular.element(document.body),
        targetEvent: ev,
        clickOutsideToClose: true,
        fullscreen: $scope.customFullscreen // Only for -xs, -sm breakpoints.
      });
    };

    function DialogController($scope, $mdDialog) {
      var dialog = this;
      
      dialog.searchText = '';
      dialog.selectedItem = null;

      // Example list of items
      dialog.items = [
        { display: 'Apple' },
        { display: 'Banana' },
        { display: 'Cherry' },
        { display: 'Date' },
        { display: 'Elderberry' }
      ];

      // Simple search function
      dialog.querySearch = function(searchText) {
        return searchText ? dialog.items.filter(createFilterFor(searchText)) : dialog.items;
      };

      // Filter function
      function createFilterFor(query) {
        var lowercaseQuery = angular.lowercase(query);
        return function filterFn(item) {
          return angular.lowercase(item.display).indexOf(lowercaseQuery) !== -1;
        };
      }

      dialog.closeDialog = function() {
        $mdDialog.hide();
      };

      // Manually trigger the digest cycle on search text change
      $scope.$watch('dialog.searchText', function(newVal, oldVal) {
        if (newVal !== oldVal) {
          $scope.$applyAsync();
        }
      });
    }
  });

  ```
