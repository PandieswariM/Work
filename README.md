```
<!DOCTYPE html>
<html lang="en" ng-app="dragApp">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AngularJS Draggable with Auto Scrolling</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body ng-controller="DragController">
  <div id="scrollContainer">
    <div class="draggable-item"
         ng-repeat="item in items"
         ng-draggable="true"
         drag-start="onDragStart($event, item)"
         drag="onDrag($event, item)"
         drag-end="onDragEnd($event, item)">
      {{item}}
    </div>
  </div>

  <!-- Include AngularJS and ngDraggable -->
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/ngDraggable/0.1.11/ngDraggable.min.js"></script>
  <script src="app.js"></script>
</body>
</html>

#scrollContainer {
  width: 100%;
  height: 200px;
  overflow-x: auto;
  white-space: nowrap;
  position: relative;
  border: 1px solid #ccc;
  padding: 10px;
  box-sizing: border-box;
}

.draggable-item {
  display: inline-block;
  width: 100px;
  height: 100px;
  background-color: lightblue;
  margin-right: 10px;
  text-align: center;
  line-height: 100px;
  cursor: move;
  user-select: none;
}

var app = angular.module('dragApp', ['ngDraggable']);

app.controller('DragController', function($scope, $element, $timeout) {
    $scope.items = ['Item 1', 'Item 2', 'Item 3', 'Item 4', 'Item 5', 'Item 6', 'Item 7', 'Item 8'];

    const scrollContainer = document.getElementById('scrollContainer');
    let autoScrollInterval;
    
    const scrollThreshold = 50;  // Distance from edge to trigger scrolling
    const scrollSpeed = 10;      // Scroll speed (increase for faster scrolling)

    $scope.onDragStart = function(event, item) {
        stopAutoScroll(); // Stop any existing scroll
    };

    $scope.onDrag = function(event, item) {
        const dragItemRect = event.element[0].getBoundingClientRect();
        const containerRect = scrollContainer.getBoundingClientRect();

        // Detect if the drag item is near the right edge
        if (dragItemRect.right > containerRect.right - scrollThreshold) {
            startAutoScroll('right');
        } 
        // Detect if the drag item is near the left edge
        else if (dragItemRect.left < containerRect.left + scrollThreshold) {
            startAutoScroll('left');
        } 
        // Stop scrolling when the drag item is away from edges
        else {
            stopAutoScroll();
        }
    };

    $scope.onDragEnd = function(event, item) {
        stopAutoScroll();
    };

    function startAutoScroll(direction) {
        // Prevent starting multiple intervals
        if (autoScrollInterval) return;

        autoScrollInterval = $timeout(function scroll() {
            if (direction === 'right') {
                scrollContainer.scrollLeft += scrollSpeed;
            } else if (direction === 'left') {
                scrollContainer.scrollLeft -= scrollSpeed;
            }

            // Keep the interval running for continuous scrolling
            autoScrollInterval = $timeout(scroll, 20);
        }, 20);
    }

    function stopAutoScroll() {
        if (autoScrollInterval) {
            $timeout.cancel(autoScrollInterval);
            autoScrollInterval = null;
        }
    }
});

```
