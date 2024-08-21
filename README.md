```

<div id="scroll-box" style="overflow-x: scroll; white-space: nowrap; width: 300px;">
  <div style="display: inline-block; width: 1000px;">
    <!-- Your content here -->
    <div style="display: inline-block; width: 300px; height: 100px; background-color: red;"></div>
    <div style="display: inline-block; width: 300px; height: 100px; background-color: blue;"></div>
    <div style="display: inline-block; width: 300px; height: 100px; background-color: green;"></div>
  </div>
</div>

app.controller('ScrollController', function($scope, $interval) {
  let scrollDirection = 'right';
  const scrollSpeed = 5; // Adjust the speed here
  const scrollBox = document.getElementById('scroll-box');

  $interval(function() {
    if (scrollDirection === 'right') {
      scrollBox.scrollLeft += scrollSpeed;
      if (scrollBox.scrollLeft + scrollBox.clientWidth >= scrollBox.scrollWidth) {
        scrollDirection = 'left'; // Change direction at the end
      }
    } else {
      scrollBox.scrollLeft -= scrollSpeed;
      if (scrollBox.scrollLeft <= 0) {
        scrollDirection = 'right'; // Change direction at the beginning
      }
    }
  }, 30); // Adjust the interval here
});
```
