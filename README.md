md-tabs md-selected="selectedIndex"
  <md-tab label="Tab 1">
    <!-- Tab 1 content -->
  </md-tab
  <md-tab label="Tab 2" ng-disabled="isSecondTabDisabled">
    <!-- Tab 2 content -->
  </md-tab
  <md-tab label="Tab 3">
    <!-- Tab 3 content -->
  </md-tab
md-tabs


$scope.selectedIndex = 0;
$scope.isSecondTabDisabled = true;

$scope.$watch('selectedIndex', function(current, old) {
  if ($scope.isSecondTabDisabled && current === 1) {
    $scope.selectedIndex = old;
  }
});


.....



app.directive('disableTab', function() {
  return {
    link: function(scope, element, attrs) {
      scope.$watch(attrs.disableTab, function(value) {
        if (value) {
          element.attr('disabled', 'disabled');
          element.addClass('disabled-tab');
        } else {
          element.removeAttr('disabled');
          element.removeClass('disabled-tab');
        }
      });
    }
  };
});

<md-tabs>
  <md-tab label="Tab 1">
    <!-- Tab 1 content -->
  </md-tab>
  <md-tab label="Tab 2" disable-tab="isSecondTabDisabled">
    <!-- Tab 2 content -->
  </md-tab>
  <md-tab label="Tab 3">
    <!-- Tab 3 content -->
  </md-tab>
</md-tabs>

.disabled-tab {
  pointer-events: none;
  opacity: 0.6;
}






