<!DOCTYPE html>
<html ng-app="myApp">
<head>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/angular_material/1.1.24/angular-material.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.8.2/angular.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/angular_material/1.1.24/angular-material.min.js"></script>
</head>
<body>
    <div ng-controller="MyController">
        <md-autocomplete
            md-no-cache="true"
            md-selected-item="selectedCourier"
            md-search-text="searchText"
            md-items="item in filterItems(searchText)"
            md-item-text="item.name"
            placeholder="Search Courier">
            <md-item-template>
                <span md-highlight-text="searchText">{{item.name}}</span>
            </md-item-template>
            <md-not-found>
                No matches found
            </md-not-found>
        </md-autocomplete>
    </div>

    <script>
        angular.module('myApp', ['ngMaterial'])
            .controller('MyController', function($scope) {
                $scope.couriername = [
                    { courierrecid: 1, name: "ram" },
                    { courierrecid: 2, name: "raja" }
                ];

                $scope.searchText = '';
                $scope.selectedCourier = null;

                $scope.filterItems = function(query) {
                    return query ? $scope.couriername.filter(createFilterFor(query)) : $scope.couriername;
                };

                function createFilterFor(query) {
                    var lowercaseQuery = angular.lowercase(query);
                    return function filterFn(item) {
                        return (item.name.toLowerCase().indexOf(lowercaseQuery) !== -1);
                    };
                }
            });
    </script>
</body>
</html>
