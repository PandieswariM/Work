<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsive Box Layout</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<div class="big-box">
    <div class="small-box"></div>
    <div class="icon">+</div>
    <div class="small-box"></div>
    <div class="icon">=</div>
    <div class="small-box"></div>
</div>

</body>
</html>

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
}

.big-box {
    display: flex;
    justify-content: space-around;
    align-items: center;
    width: 80%;
    height: 200px;
    background-color: #fff;
    border: 2px solid #000;
    padding: 20px;
}

.small-box {
    flex: 1;
    background-color: lightblue;
    height: 100%;
    border: 2px solid #000;
    margin: 0 10px;
}

.icon {
    font-size: 2em;
    font-weight: bold;
    margin: 0 10px;
}
