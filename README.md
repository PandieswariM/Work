```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tracking Info</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>

  <div class="tracking-container">
    <div class="tracking-item">
      <label>Courier:</label>
      <span class="courier">FedEx Express</span>
    </div>

    <div class="tracking-item">
      <label>Tracking ID:</label>
      <span class="tracking-id">1234567890</span>
      <button class="copy-btn" onclick="copyToClipboard()">&#128203;</button>
    </div>

    <div class="tracking-item">
      <label>Tracking URL:</label>
      <a href="https://www.fedex.com/en-us/home.html" class="tracking-url" target="_blank">https://www.fedex.com/en-us/home.html</a>
    </div>
  </div>

  <script>
    function copyToClipboard() {
      var trackingId = document.querySelector('.tracking-id').textContent;
      navigator.clipboard.writeText(trackingId).then(() => {
        alert('Tracking ID copied!');
      });
    }
  </script>

</body>
</html>
```
```
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f5f5f5;
}

.tracking-container {
  width: 400px;
  padding: 20px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.tracking-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.tracking-item label {
  font-weight: bold;
  width: 100px;
}

.courier, .tracking-id, .tracking-url {
  font-size: 14px;
  color: #333;
}

.tracking-url {
  text-decoration: none;
  color: #007bff;
  max-width: 200px;
  word-wrap: break-word;
}

.tracking-id {
  max-width: 150px;
  word-wrap: break-word;
}

.copy-btn {
  background-color: transparent;
  border: none;
  font-size: 16px;
  cursor: pointer;
  margin-left: 10px;
}

.copy-btn:hover {
  color: #007bff;
}
```
