To achieve this, you can use the `draggable` directive in AngularJS and the `ng-mouseup` and `ng-mousedown` directives to handle the dragging and dropping of the boxes.

Here's an example:

HTML:

```
<div class="big-container" ng-style="{height: '250px', width: '100%'}">
  <div class="box-container" ng-repeat="box in boxes" ng-style="{height: '200px', width: '200px'}" ng-mousedown="startDrag($event, box)" ng-mouseup="stopDrag($event, box)" ng-class="{dragging: box.dragging}">
    Box {{(link unavailable)}}
  </div>
</div>
```

JavaScript:

```
$scope.boxes = [
  {id: 1, dragging: false},
  {id: 2, dragging: false},
  {id: 3, dragging: false},
  {id: 4, dragging: false},
  {id: 5, dragging: false}
];

$scope.startDrag = function($event, box) {
  box.dragging = true;
};

$scope.stopDrag = function($event, box) {
  box.dragging = false;
};
```

CSS:

```
.big-container {
  position: relative;
}

.box-container {
  position: absolute;
  cursor: move;
}

.dragging {
  opacity: 0.5;
}
```

In this example, the `ng-mousedown` directive starts the drag operation, and the `ng-mouseup` directive stops it. The `ng-style` directive sets the height and width of the boxes, and the `ng-class` directive applies the `dragging` class when the box is being dragged.

Note that this is a basic example, and you may need to add additional logic to handle the actual dragging and dropping of the boxes, such as updating their positions and handling collisions.
