$scope.selectedIndex = 0;
$scope.isSecondTabDisabled = true;

$scope.$watch('selectedIndex', function(current, old) {
  if ($scope.isSecondTabDisabled && current === 1) {
    $scope.selectedIndex = old;
  }
});


.....

<md-tabs md-selected="selectedIndex">
  <md-tab label="Tab 1">
    <!-- Tab 1 content -->
  </md-tab>
  <md-tab label="Tab 2" ng-disabled="isSecondTabDisabled">
    <!-- Tab 2 content -->
  </md-tab>
  <md-tab label="Tab 3">
    <!-- Tab 3 content -->
  </md-tab>
</md-tabs>
