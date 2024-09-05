Not mention placeholder text for this field in the requirement PDF.


Brother, this is a change task, if you look into this task, I will commit it
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Consent Information</title>
    <link rel="stylesheet" href="styles.css">
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap" rel="stylesheet">
</head>
<body>
    <div class="custom-card-container">
        <div class="custom-card">
            <div class="date-section">
                <div class="date-item">
                    <span class="date-label">Start Date:</span>
                    <span class="date-value">01/01/2024</span>
                </div>
                <div class="date-item">
                    <span class="date-label">End Date:</span>
                    <span class="date-value">31/12/2024</span>
                </div>
                <div class="inr-item">
                    <span class="inr-label">Total INR:</span>
                    <span class="inr-value">₹ 15,000</span>
                </div>
            </div>
            <div class="points-section">
                <div class="points-star">
                    <span class="points-value">120</span>
                </div>
            </div>
        </div>
    </div>
</body>
</html>
```
```
/* styles.css */
body {
    font-family: 'Roboto', sans-serif;
    background-color: #f0f2f5;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
}

.custom-card-container {
    width: 350px;
}

.custom-card {
    background-color: #fff;
    border-radius: 15px;
    padding: 20px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.date-section {
    text-align: left;
}

.date-item, .inr-item {
    margin-bottom: 10px;
}

.date-label, .inr-label {
    font-weight: 500;
    color: #555;
}

.date-value, .inr-value {
    font-weight: bold;
    color: #333;
}

.points-section {
    display: flex;
    align-items: center;
    justify-content: center;
}

.points-star {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 80px;
    height: 80px;
    background-color: #ffcc00;
    clip-path: polygon(50% 0%, 61% 35%, 98% 35%, 68% 57%, 79% 91%, 50% 70%, 21% 91%, 32% 57%, 2% 35%, 39% 35%);
}

.points-value {
    font-size: 18px;
    font-weight: bold;
    color: #333;
}
```
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Consent Information</title>
    <link rel="stylesheet" href="styles.css">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
</head>
<body>
    <div class="card-container">
        <div class="card">
            <h2>Consent Information</h2>
            <div class="card-content">
                <div class="info-item">
                    <i class="icon">&#128197;</i>
                    <span class="label">Start Date:</span>
                    <span class="value">01/01/2024</span>
                </div>
                <div class="info-item">
                    <i class="icon">&#128197;</i>
                    <span class="label">End Date:</span>
                    <span class="value">31/12/2024</span>
                </div>
                <div class="info-item">
                    <i class="icon">&#11088;</i>
                    <span class="label">Total Target Points:</span>
                    <span class="value">120</span>
                </div>
                <div class="info-item">
                    <i class="icon">&#8377;</i>
                    <span class="label">Total INR:</span>
                    <span class="value">₹ 15,000</span>
                </div>
            </div>
        </div>
    </div>
</body>
</html>
```
```
/* styles.css */
body {
    font-family: 'Poppins', sans-serif;
    background: linear-gradient(to right, #6a11cb, #2575fc);
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
}

.card-container {
    width: 350px;
}

.card {
    background-color: #fff;
    border-radius: 15px;
    padding: 20px;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
    text-align: center;
}

h2 {
    margin-bottom: 20px;
    color: #444;
    font-weight: 600;
}

.card-content {
    display: flex;
    flex-direction: column;
    gap: 15px;
}

.info-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 8px;
    background-color: #f9f9f9;
}

.icon {
    font-size: 18px;
    color: #6a11cb;
    margin-right: 10px;
}

.label {
    font-weight: 600;
    color: #555;
    flex-grow: 1;
    text-align: left;
}

.value {
    color: #333;
}
```
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Consent Information</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <div class="card">
            <h2>Consent Information</h2>
            <div class="info">
                <div class="item">
                    <span class="label">Start Date:</span>
                    <span class="value">01/01/2024</span>
                </div>
                <div class="item">
                    <span class="label">End Date:</span>
                    <span class="value">31/12/2024</span>
                </div>
                <div class="item">
                    <span class="label">Total Target Points:</span>
                    <span class="value">120</span>
                </div>
                <div class="item">
                    <span class="label">Total INR:</span>
                    <span class="value">₹ 15,000</span>
                </div>
            </div>
        </div>
    </div>
</body>
</html>
```
```
/* styles.css */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Arial', sans-serif;
}

body {
    background-color: #f3f4f6;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.container {
    max-width: 400px;
    width: 100%;
    padding: 20px;
}

.card {
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    padding: 20px;
    text-align: center;
}

h2 {
    margin-bottom: 20px;
    color: #333;
}

.info {
    text-align: left;
}

.item {
    display: flex;
    justify-content: space-between;
    margin-bottom: 15px;
}

.label {
    font-weight: bold;
    color: #555;
}

.value {
    color: #777;
}
```
