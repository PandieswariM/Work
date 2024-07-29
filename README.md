To use Cropper.js in your AngularJS project, you can follow these steps:

1. Install Cropper.js using npm or bower:

npm:

```
npm install cropperjs
```

bower:

```
bower install cropper
```

1. Include Cropper.js and its CSS file in your HTML file:

```
<link rel="stylesheet" href="node_modules/cropperjs/dist/cropper.css">
<script src="node_modules/cropperjs/dist/cropper.js"></script>
```

1. Add an image element to your HTML file:

```
<img id="image" src="your-image-url" alt="Image">
```

1. Create a directive in AngularJS to wrap the Cropper.js functionality:

```
angular.module('yourApp').directive('imageCropper', function() {
  return {
    restrict: 'A',
    link: function(scope, element) {
      var cropper = new Cropper(element[0], {
        aspectRatio: 16 / 9,
        crop: function(event) {
          console.log(event.detail.x);
          console.log(event.detail.y);
          console.log(event.detail.width);
          console.log(event.detail.height);
          console.log(event.detail.rotate);
          console.log(event.detail.scaleX);
          console.log(event.detail.scaleY);
        }
      });
    }
  };
});
```

1. Use the directive in your HTML file:

```
<img id="image" src="your-image-url" alt="Image" image-cropper>
```

This will initialize Cropper.js on the image element and allow you to crop the image.

Note: Make sure to replace "your-image-url" with the actual URL of your image. Also, you can customize the Cropper.js options to fit your needs.





...

To save the cropped image to a $scope variable when the crop button is clicked, you can use the `getCroppedCanvas()` method of Cropper.js and then convert the canvas to a data URL.

Here's an updated version of the directive:

```
angular.module('yourApp').directive('imageCropper', function() {
  return {
    restrict: 'A',
    link: function(scope, element) {
      var cropper = new Cropper(element[0], {
        aspectRatio: 16 / 9,
        crop: function(event) {
          console.log(event.detail.x);
          console.log(event.detail.y);
          console.log(event.detail.width);
          console.log(event.detail.height);
          console.log(event.detail.rotate);
          console.log(event.detail.scaleX);
          console.log(event.detail.scaleY);
        }
      });

      scope.cropImage = function() {
        var canvas = cropper.getCroppedCanvas();
        var imageDataURL = canvas.toDataURL();
        scope.croppedImage = imageDataURL;
      };
    }
  };
});
```

Then, in your HTML file, you can add a button to trigger the `cropImage()` function:

```
<img id="image" src="your-image-url" alt="Image" image-cropper>
<button ng-click="cropImage()">Crop</button>
```

Now, when the crop button is clicked, the `cropImage()` function will be called, and the cropped image will be saved to the `$scope.croppedImage` variable.

Note: The `$scope.croppedImage` variable will hold the data URL of the cropped image, which can be used to display the image or send it to a server for further processing.
