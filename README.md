```
<div id="scrollContainer" ng-controller="DragController">
  <div class="draggable-item" 
       ng-repeat="item in items" 
       ng-draggable 
       drag-start="onDragStart($event, item)" 
       drag="onDrag($event, item)" 
       drag-end="onDragEnd($event, item)">
    {{item}}
  </div>
</div>

app.controller('DragController', function($scope, $element, $timeout) {
    const scrollContainer = document.getElementById('scrollContainer');

    let autoScrollInterval;
    const scrollSpeed = 5;

    $scope.onDragStart = function(event, item) {
        clearInterval(autoScrollInterval); // Stop any existing scroll
    };

    $scope.onDrag = function(event, item) {
        const dragItemRect = event.element[0].getBoundingClientRect();
        const containerRect = scrollContainer.getBoundingClientRect();

        if (dragItemRect.right > containerRect.right - 20) {
            autoScroll('right');
        } else if (dragItemRect.left < containerRect.left + 20) {
            autoScroll('left');
        } else {
            clearInterval(autoScrollInterval);
        }
    };

    $scope.onDragEnd = function(event, item) {
        clearInterval(autoScrollInterval);
    };

    function autoScroll(direction) {
        if (autoScrollInterval) return;

        autoScrollInterval = $timeout(function scroll() {
            if (direction === 'right') {
                scrollContainer.scrollLeft += scrollSpeed;
            } else if (direction === 'left') {
                scrollContainer.scrollLeft -= scrollSpeed;
            }

            autoScrollInterval = $timeout(scroll, 20); // repeat scroll every 20ms
        }, 20);
    }
});

```
