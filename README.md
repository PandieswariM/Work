
```
<div class="scroll-container" ng-controller="TodoController">
  <ul id="todo-list" class="todo-list">
    <li ng-repeat="item in todoList" 
        draggable="true" 
        ng-dragstart="onDragStart($event, item)" 
        ng-dragend="onDragEnd($event)"
        ng-drag="onDrag($event)">
      {{ item }}
    </li>
  </ul>
</div>

<div class="scroll-container" ng-controller="TodoController">
  <ul id="todo-list" class="todo-list">
    <li ng-repeat="item in todoList" 
        draggable="true" 
        ng-dragstart="onDragStart($event, item)" 
        ng-dragend="onDragEnd($event)"
        ng-drag="onDrag($event)">
      {{ item }}
    </li>
  </ul>
</div>
.scroll-container {
  height: 400px;
  overflow-y: auto;
  border: 1px solid #ccc;
}

.todo-list {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.todo-list li {
  padding: 10px;
  border-bottom: 1px solid #ddd;
  cursor: move;
}

app.controller('TodoController', function($scope, $timeout) {
  $scope.todoList = ['Item 1', 'Item 2', 'Item 3', 'Item 4', 'Item 5'];

  let scrollInterval;
  const scrollSpeed = 5;
  const edgeThreshold = 50;
  const scrollContainer = document.querySelector('.scroll-container');

  // Start drag event
  $scope.onDragStart = function(event, item) {
    // Set the dragged item (you can store this to handle drop logic)
    event.dataTransfer.setData('text', item);
  };

  // Handle drag movement to auto-scroll
  $scope.onDrag = function(event) {
    const boundingRect = scrollContainer.getBoundingClientRect();
    const pointerY = event.clientY;

    if (pointerY - boundingRect.top < edgeThreshold) {
      scrollInterval = $timeout(function scrollUp() {
        scrollContainer.scrollTop -= scrollSpeed;
        scrollInterval = $timeout(scrollUp, 10);
      }, 10);
    } else if (boundingRect.bottom - pointerY < edgeThreshold) {
      scrollInterval = $timeout(function scrollDown() {
        scrollContainer.scrollTop += scrollSpeed;
        scrollInterval = $timeout(scrollDown, 10);
      }, 10);
    } else {
      $timeout.cancel(scrollInterval);
    }
  };

  // Stop scrolling when drag ends
  $scope.onDragEnd = function(event) {
    $timeout.cancel(scrollInterval);
  };
});
```
