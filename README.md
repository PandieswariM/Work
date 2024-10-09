```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vertical Box Design - Pro Look</title>
    
    <!-- Link to Font Awesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">

    <style>
        /* Main container for holding all boxes */
        .container {
            display: flex;
            flex-direction: column; /* Vertical layout */
            align-items: center;
            padding: 40px;
            background-color: #f0f4f8; /* Soft light background */
            height: 100vh;
            justify-content: center;
        }

        /* General box styling */
        .box {
            border-radius: 15px;
            width: 200px;
            text-align: center;
            padding: 30px 20px;
            color: #fff; /* White text for contrast */
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1); /* Soft shadow */
            margin-bottom: 25px; /* Space between boxes */
            transition: transform 0.3s, box-shadow 0.3s;
            background: linear-gradient(135deg, #ffffff, #f0f4f8); /* Subtle gradient */
        }

        /* Hover effect for boxes */
        .box:hover {
            transform: translateY(-5px); /* Lift effect */
            box-shadow: 0 12px 25px rgba(0, 0, 0, 0.15); /* Deeper shadow */
        }

        /* Different background colors for each box */
        .voucher {
            background: linear-gradient(135deg, #6dd5ed, #2193b0); /* Light blue gradient */
        }

        .bank {
            background: linear-gradient(135deg, #f9d423, #ff4e50); /* Orange/red gradient */
        }

        .upi {
            background: linear-gradient(135deg, #56ab2f, #a8e063); /* Green gradient */
        }

        /* Icon styling */
        .icon i {
            font-size: 40px; /* Large icon size */
            margin-bottom: 15px;
        }

        /* Label styling */
        .label {
            font-size: 18px;
            font-weight: bold;
            text-transform: uppercase;
        }
    </style>
</head>
<body>

    <!-- Main container holding the boxes -->
    <div class="container">
        <!-- Box 1: Voucher -->
        <div class="box voucher">
            <div class="icon">
                <i class="fas fa-ticket-alt"></i> <!-- Font Awesome icon for voucher -->
            </div>
            <div class="label">Voucher</div>
        </div>

        <!-- Box 2: Bank DTP -->
        <div class="box bank">
            <div class="icon">
                <i class="fas fa-university"></i> <!-- Font Awesome icon for bank -->
            </div>
            <div class="label">Bank DTP</div>
        </div>

        <!-- Box 3: UPI -->
        <div class="box upi">
            <div class="icon">
                <i class="fas fa-mobile-alt"></i> <!-- Font Awesome icon for UPI -->
            </div>
            <div class="label">UPI</div>
        </div>
    </div>

</body>
</html>
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vertical Box Container with Icons</title>
    
    <!-- Link to Font Awesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">

    <style>
        /* Styling the main container */
        .container {
            display: flex;
            flex-direction: column; /* Vertical layout */
            align-items: center;
            padding: 20px;
            background-color: #e0f7fa; /* Light blue background */
            height: 100vh; /* Full height */
            justify-content: center; /* Center vertically */
        }

        /* General styling for each box */
        .box {
            border-radius: 10px;
            width: 150px;
            text-align: center;
            padding: 20px;
            color: white; /* White text color */
            box-shadow: 2px 2px 12px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
            transition: transform 0.2s, background-color 0.3s;
        }

        /* Hover effect for scaling */
        .box:hover {
            transform: scale(1.05);
        }

        /* Individual styles for each box */
        .box.voucher {
            background-color: #4caf50; /* Green for Voucher */
            border: 3px solid #388e3c; /* Dark green border */
        }

        .box.bank {
            background-color: #2196f3; /* Blue for Bank */
            border: 3px solid #1976d2; /* Dark blue border */
        }

        .box.upi {
            background-color: #ff9800; /* Orange for UPI */
            border: 3px solid #f57c00; /* Dark orange border */
        }

        /* Icon styling */
        .icon i {
            font-size: 40px; /* Large icon size */
            margin-bottom: 10px;
        }

        /* Label styling */
        .label {
            font-size: 16px;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <!-- Main container to hold the boxes -->
    <div class="container">
        <!-- Box 1: Voucher -->
        <div class="box voucher">
            <div class="icon">
                <i class="fas fa-ticket-alt"></i> <!-- Font Awesome icon for voucher -->
            </div>
            <div class="label">Voucher</div>
        </div>

        <!-- Box 2: Bank DTP -->
        <div class="box bank">
            <div class="icon">
                <i class="fas fa-university"></i> <!-- Font Awesome icon for bank -->
            </div>
            <div class="label">Bank DTP</div>
        </div>

        <!-- Box 3: UPI -->
        <div class="box upi">
            <div class="icon">
                <i class="fas fa-mobile-alt"></i> <!-- Font Awesome icon for UPI -->
            </div>
            <div class="label">UPI</div>
        </div>
    </div>

</body>
</html>
```
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vertical Box Design - Blue Theme</title>
    
    <!-- Link to Font Awesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">

    <style>
        /* Main container to hold all boxes */
        .container {
            display: flex;
            flex-direction: column; /* Vertical layout */
            align-items: center;
            padding: 40px;
            background-color: #f9fbfc; /* Very light background */
            height: 100vh;
            justify-content: center; /* Center vertically */
        }

        /* General box styling */
        .box {
            border-radius: 15px;
            width: 220px;
            text-align: center;
            padding: 25px 15px;
            color: #007bff; /* Blue text color */
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1); /* Subtle shadow */
            margin-bottom: 20px; /* Space between boxes */
            transition: transform 0.3s, box-shadow 0.3s;
        }

        /* Hover effect for boxes */
        .box:hover {
            transform: translateY(-5px); /* Slight lift effect */
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15); /* Deeper shadow on hover */
        }

        /* Different light color gradients for each box */
        .voucher {
            background: linear-gradient(135deg, #e0f7fa, #b2ebf2); /* Light cyan gradient */
        }

        .bank {
            background: linear-gradient(135deg, #fff3e0, #ffe0b2); /* Light orange gradient */
        }

        .upi {
            background: linear-gradient(135deg, #e1bee7, #f8bbd0); /* Light pink gradient */
        }

        /* Icon styling */
        .icon i {
            font-size: 40px; /* Large icon size */
            margin-bottom: 10px;
            color: #007bff; /* Blue icon color */
        }

        /* Label styling */
        .label {
            font-size: 18px;
            font-weight: bold;
            text-transform: uppercase;
            color: #007bff; /* Blue label color */
        }
    </style>
</head>
<body>

    <!-- Main container holding the boxes -->
    <div class="container">
        <!-- Box 1: Voucher -->
        <div class="box voucher">
            <div class="icon">
                <i class="fas fa-ticket-alt"></i> <!-- Font Awesome icon for voucher -->
            </div>
            <div class="label">Voucher</div>
        </div>

        <!-- Box 2: Bank DTP -->
        <div class="box bank">
            <div class="icon">
                <i class="fas fa-university"></i> <!-- Font Awesome icon for bank -->
            </div>
            <div class="label">Bank DTP</div>
        </div>

        <!-- Box 3: UPI -->
        <div class="box upi">
            <div class="icon">
                <i class="fas fa-mobile-alt"></i> <!-- Font Awesome icon for UPI -->
            </div>
            <div class="label">UPI</div>
        </div>
    </div>

</body>
</html>
```
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Redeem Points</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f9f9f9;
            margin: 0;
            padding: 20px;
        }

        .container {
            max-width: 600px;
            margin: auto;
            background: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            padding: 20px;
        }

        .image-container {
            width: 100%;
            height: 250px;
            overflow: hidden;
        }

        .image-container img {
            width: 100%;
            height: auto;
        }

        .info {
            padding: 10px 0;
        }

        .header {
            display: flex;
            justify-content: space-between;
            margin: 10px 0;
        }

        h1 {
            margin: 0;
            font-size: 24px;
            color: #333;
        }

        .customer-name {
            font-size: 18px;
            color: #333;
            text-align: right; /* Align customer name to the right */
        }

        .points {
            display: flex;
            justify-content: space-between;
            margin: 10px 0;
            color: #555;
        }

        .description {
            margin: 10px 0;
            height: 100px; /* Fixed height for consistent layout */
            overflow: hidden; /* Hide overflow */
            text-overflow: ellipsis; /* Add ellipsis for overflow */
            display: -webkit-box;
            -webkit-box-orient: vertical;
            -webkit-line-clamp: 5; /* Limit lines to 5 */
            color: #777;
        }

        .redeem {
            margin: 20px 0;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }

        input[type="number"] {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }

        .proceed-button {
            background-color: #28a745;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }

        .proceed-button:hover {
            background-color: #218838;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="image-container">
            <img src="your-image-url.jpg" alt="Image Description" />
        </div>
        <div class="info">
            <div class="header">
                <h1 id="name">Item Name</h1>
                <div class="customer-name" id="customer-name">Customer Name</div>
            </div>
            <div class="points">
                <span>Min Points: <strong id="min-points">100</strong></span>
                <span>Max Points: <strong id="max-points">500</strong></span>
            </div>
            <div class="description" id="description">
                Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
                Phasellus imperdiet, nulla et dictum interdum, nisi lorem egestas odio, vitae scelerisque enim ligula venenatis dolor. 
                Maecenas nisl est, ultrices nec congue eget, auctor vitae massa.
            </div>
            <div class="redeem">
                <label for="redeem-points">Redeem Points:</label>
                <input type="number" id="redeem-points" min="1" max="500" placeholder="Enter points to redeem" />
            </div>
            <button class="proceed-button">Proceed</button>
        </div>
    </div>
</body>
</html>
```
