To apply Cropper.js and implement image cropper functionality, you can follow these steps:

1. Include Cropper.js and its CSS file in your HTML file:
```
<link rel="stylesheet" href="(link unavailable)">
<script src="(link unavailable)"></script>
```

1. Add an image element to your HTML file:

```
<img id="image" src="your-image-url" alt="Image">
```

1. Initialize Cropper.js on the image element:

```
const image = document.getElementById('image');
const cropper = new Cropper(image, {
  aspectRatio: 16 / 9,
  crop(event) {
    console.log(event.detail.x);
    console.log(event.detail.y);
    console.log(event.detail.width);
    console.log(event.detail.height);
    console.log(event.detail.rotate);
    console.log(event.detail.scaleX);
    console.log(event.detail.scaleY);
  }
});
```

1. You can then use the `getCroppedCanvas()` method to get the cropped image:

```
const canvas = cropper.getCroppedCanvas();
```

1. To display the cropped image, you can set it as the source of an image element:

```
const croppedImage = document.getElementById('cropped-image');
croppedImage.src = canvas.toDataURL();
```

Remember to replace "your-image-url" with the actual URL of your image. You can also customize the aspect ratio, crop box size, and other options according to your needs.

Here's a working example:

```
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="(link unavailable)">
</head>
<body>
  <img id="image" src="your-image-url" alt="Image">
  <button id="crop">Crop</button>
  <img id="cropped-image" alt="Cropped Image">
  <script src="(link unavailable)"></script>
  <script>
    const image = document.getElementById('image');
    const cropper = new Cropper(image, {
      aspectRatio: 16 / 9,
      crop(event) {
        console.log(event.detail.x);
        console.log(event.detail.y);
        console.log(event.detail.width);
        console.log(event.detail.height);
        console.log(event.detail.rotate);
        console.log(event.detail.scaleX);
        console.log(event.detail.scaleY);
      }
    });
    document.getElementById('crop').addEventListener('click', () => {
      const canvas = cropper.getCroppedCanvas();
      const croppedImage = document.getElementById('cropped-image');
      croppedImage.src = canvas.toDataURL();
    });
  </script>
</body>
</html>
```
