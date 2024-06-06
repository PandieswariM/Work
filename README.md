

    <?php
    if ($_SERVER["REQUEST_METHOD"] == "POST") {
        $textfieldValue = trim($_POST['textfield']);
        // Process the form data as needed
        if (!empty($textfieldValue)) {
            echo "Form submitted with value: " . htmlspecialchars($textfieldValue);
        } else {
            echo "Textfield is empty!";
        }

        <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Textfield and Button</title>
    <style>
        .disabled-button {
            opacity: 0.5;
        }
    </style>
</head>
<body>
    <form action="" method="post">
        <input type="text" id="textfield" name="textfield" value="<?php echo isset($_POST['textfield']) ? htmlspecialchars($_POST['textfield']) : ''; ?>">
        <button id="submitButton" type="submit" class="<?php echo empty($_POST['textfield']) ? 'disabled-button' : ''; ?>">Submit</button>
    </form>
    }
    ?>
</body>
</html>

