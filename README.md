Here is the equivalent code in AngularJS:

```
<!DOCTYPE html>
<html lang="en" ng-app="myApp">
<head>
  <meta charset="utf-8">
  <meta http-equiv="x-ua-compatible" content="ie=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <title>Cropper.js</title>
  <link rel="stylesheet" href="../css/cropper.css">
  <style>
    .container {
      margin: 20px auto;
      max-width: 640px;
    }
    img {
      max-width: 100%;
    }
  </style>
</head>
<body>
  <div class="container" ng-controller="CropperController">
    <h1>Cropper with a range of aspect ratio</h1>
    <div>
      <img id="image" src="../images/picture.jpg" alt="Picture" ng-cropper="options">
    </div>
  </div>
  <script src="../js/cropper.js"></script>
  <script src="(link unavailable)"></script>
  <script>
    angular.module('myApp', []).controller('CropperController', function($scope) {
      $scope.options = {
        ready: function() {
          var cropper = this.cropper;
          var containerData = cropper.getContainerData();
          var cropBoxData = cropper.getCropBoxData();
          var aspectRatio = cropBoxData.width / cropBoxData.height;
          var newCropBoxWidth;
          if (aspectRatio < 0.5 || aspectRatio > 1.5) {
            newCropBoxWidth = cropBoxData.height * ((0.5 + 1.5) / 2);
            cropper.setCropBoxData({
              left: (containerData.width - newCropBoxWidth) / 2,
              width: newCropBoxWidth
            });
          }
        },
        cropmove: function() {
          var cropper = this.cropper;
          var cropBoxData = cropper.getCropBoxData();
          var aspectRatio = cropBoxData.width / cropBoxData.height;
          if (aspectRatio < 0.5) {
            cropper.setCropBoxData({
              width: cropBoxData.height * 0.5
            });
          } else if (aspectRatio > 1.5) {
            cropper.setCropBoxData({
              width: cropBoxData.height * 1.5
            });
          }
        }
      };
    });
  </script>
</body>
</html>
```

Note that I've added the `ng-app` directive to the `html` element, and the `ng-controller` directive to the `div` element containing the image. I've also defined a `CropperController` controller that sets up the Cropper.js options.

I've also replaced the `window.addEventListener('DOMContentLoaded', ...)` code with the `ng-cropper` directive, which is a built-in AngularJS directive for working with Cropper.js.

Make sure to include the AngularJS script file and the Cropper.js script file in your HTML file.
