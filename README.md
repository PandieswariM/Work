<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/angular-drag-and-drop-lists/2.1.0/angular-drag-and-drop-lists.min.js"></script>


```
<div ng-app="dragDropApp" ng-controller="MainCtrl">
    <div id="scrollable-container" style="width: 500px; height: 500px; overflow-y: scroll; border: 1px solid #000; position: relative;">
        <ul dnd-list="boxes" style="list-style-type: none; padding: 0;">
            <li ng-repeat="box in boxes" dnd-draggable="box" dnd-moved="boxes.splice($index, 1)" 
                dnd-effect-allowed="move" dnd-selected="selected = box" dnd-dragstart="onDragStart($event)"
                dnd-dragend="onDragEnd($event)"
                style="width: 400px; height: 400px; margin-bottom: 20px; background-color: #ddd; display: flex; align-items: center; justify-content: center; text-align: center;">
                {{box.name}}
            </li>
        </ul>
    </div>
</div>
```

```
<script>
    angular.module('dragDropApp', ['dndLists'])
        .controller('MainCtrl', function($scope, $element, $interval) {
            $scope.boxes = [
                { id: 1, name: 'Box 1' },
                { id: 2, name: 'Box 2' },
                { id: 3, name: 'Box 3' },
                { id: 4, name: 'Box 4' },
                { id: 5, name: 'Box 5' }
            ];

            let scrollInterval;
            let scrollableContainer = document.getElementById('scrollable-container');
            
            // Function to start auto-scrolling when dragging near edges
            $scope.onDragStart = function(event) {
                let containerRect = scrollableContainer.getBoundingClientRect();

                scrollInterval = $interval(function() {
                    let mouseY = event.clientY;

                    if (mouseY < containerRect.top + 50) {
                        // Scroll up
                        scrollableContainer.scrollTop -= 5;
                    } else if (mouseY > containerRect.bottom - 50) {
                        // Scroll down
                        scrollableContainer.scrollTop += 5;
                    }
                }, 30); // adjust the speed of scrolling as needed
            };

            // Function to stop auto-scrolling
            $scope.onDragEnd = function() {
                if (scrollInterval) {
                    $interval.cancel(scrollInterval);
                    scrollInterval = null;
                }
            };
        });
</script>
```

```
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/angular-drag-and-drop-lists/2.1.0/angular-drag-and-drop-lists.min.js"></script>
```

```
<script>
    angular.module('dragDropApp', ['dndLists'])
        .controller('MainCtrl', function($scope) {
            $scope.boxes = [
                { id: 1, name: 'Box 1' },
                { id: 2, name: 'Box 2' },
                { id: 3, name: 'Box 3' },
                { id: 4, name: 'Box 4' },
                { id: 5, name: 'Box 5' }
            ];
        });
</script>
```
```
<div ng-app="dragDropApp" ng-controller="MainCtrl">
    <div style="width: 500px; height: 500px; overflow-y: scroll; border: 1px solid #000; position: relative;">
        <ul dnd-list="boxes" style="list-style-type: none; padding: 0;">
            <li ng-repeat="box in boxes" dnd-draggable="box" dnd-moved="boxes.splice($index, 1)" 
                dnd-effect-allowed="move" dnd-selected="selected = box"
                style="width: 400px; height: 400px; margin-bottom: 20px; background-color: #ddd; display: flex; align-items: center; justify-content: center; text-align: center;">
                {{box.name}}
            </li>
        </ul>
    </div>
</div>

```
```
::-webkit-scrollbar {
    width: 8px;
}

::-webkit-scrollbar-thumb {
    background-color: darkgrey;
    border-radius: 10px;
}
```
