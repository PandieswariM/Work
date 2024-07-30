You can use the `$mdDialog` service in AngularJS to detect when the dialog is fully open. Here's an example:

```
$mdDialog.show({
  // Your dialog options
}).then(function() {
  // Dialog is fully open, do some steps here
  console.log('Dialog is fully open');
});
```

If you want to perform actions when the dialog is opened, you can use the `open` event:

```
$mdDialog.show({
  // Your dialog options
}).then(function() {
  // Dialog is shown
  console.log('Dialog is shown');
}, function() {
  // Dialog is hidden
  console.log('Dialog is hidden');
});
```

Alternatively, you can use the `$mdDialog` events to detect when the dialog is fully open:

```
$rootScope.$on('$mdDialog.open', function() {
  console.log('Dialog is opening');
});

$rootScope.$on('$mdDialog.opened', function() {
  console.log('Dialog is fully open');
});
```

Note that the `$mdDialog` events are emitted on the `$rootScope`, so you need to listen for them on the `$rootScope` or a child scope.

When using Cropper.js, you need to set the `viewMode` option to `1` or `2` to enable cropping. Also, make sure to set the `aspectRatio` option to the desired ratio or `NaN` for free aspect ratio.

Here's an example:

```
var image = document.getElementById('image');
var cropper = new Cropper(image, {
  viewMode: 1,
  aspectRatio: NaN, // or set a specific ratio, e.g., 16/9
  autoCropArea: 1,
  cropBoxResizable: true,
  cropBoxMovable: true,
});
```

If the image still doesn't resize, try setting the `data-width` and `data-height` attributes on the image element:

```
<img id="image" src="your-image-url" data-width="100%" data-height="200px" />
```

This should allow the image to resize within the container while cropping.
