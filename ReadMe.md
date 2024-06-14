# Random Integer Generator

A web-based application for generating random integers within a specified range, excluding user-defined values. This project allows users to save settings using cookies and ensures no infinite loops by validating the input range and exclusions. Includes a simple and user-friendly interface with customizable minimum and maximum values, and handles edge cases gracefully.

## Features

- Generate random integers within a user-defined range.
- Exclude specific values from the random generation.
- Save settings with cookies for persistent configuration.
- Handle edge cases to avoid infinite loops.
- Simple and user-friendly interface.

## Usage

1. **Clone the Repository**

    ```bash
    git clone git@github.com:dglass710/random-int-generator.git
    cd random-int-generator
    ```

2. **Open the HTML File**

    Open `index.html` in your web browser to use the Random Integer Generator.

## HTML Code

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Random Integer Generator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        .container {
            max-width: 500px;
            margin: 0 auto;
        }
        label {
            display: block;
            margin: 10px 0 5px;
        }
        input[type="text"] {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
        }
        button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        h1 {
            text-align: center;
            margin-top: 40px;
        }
    </style>
</head>
<body>
    <div class="container">
        <label for="min">Minimum Value:</label>
        <input type="text" id="min" placeholder="Enter minimum value">

        <label for="max">Maximum Value:</label>
        <input type="text" id="max" placeholder="Enter maximum value">

        <label for="excluded">Excluded Values (comma-separated):</label>
        <input type="text" id="excluded" placeholder="Enter values to exclude">

        <button onclick="saveSettings()">Save Settings</button>
        <button onclick="generateRandomNumber()">Generate Number</button>

        <h1 id="randomNumber">Random Number: </h1>
    </div>

    <script>
        function saveSettings() {
            const min = document.getElementById('min').value;
            const max = document.getElementById('max').value;
            let excluded = document.getElementById('excluded').value.split(',')
                .map(Number)
                .filter((value, index, self) => self.indexOf(value) === index && !isNaN(value))
                .sort((a, b) => a - b);
            document.cookie = `min=${min}; path=/`;
            document.cookie = `max=${max}; path=/`;
            document.cookie = `excluded=${excluded.join(',')}; path=/`;
        }

        function getCookie(name) {
            const value = `; ${document.cookie}`;
            const parts = value.split(`; ${name}=`);
            if (parts.length === 2) return parts.pop().split(';').shift();
        }

        function loadSettings() {
            const min = getCookie('min');
            const max = getCookie('max');
            const excluded = getCookie('excluded');
            if (min) document.getElementById('min').value = min;
            if (max) document.getElementById('max').value = max;
            if (excluded) document.getElementById('excluded').value = excluded;
        }

        function generateRandomNumber() {
            const min = parseInt(document.getElementById('min').value);
            const max = parseInt(document.getElementById('max').value);
            const excluded = document.getElementById('excluded').value.split(',').map(Number);

            if (isNaN(min) || isNaN(max)) {
                alert('Please enter valid minimum and maximum values.');
                return;
            }

            // Generate list of valid numbers
            const validNumbers = [];
            for (let i = min; i <= max; i++) {
                if (!excluded.includes(i)) {
                    validNumbers.push(i);
                }
            }

            // Check if there are any valid numbers
            if (validNumbers.length === 0) {
                document.getElementById('randomNumber').innerText = 'No valid numbers available in the given range.';
                return;
            }

            // Generate a random number from the valid numbers
            const randomNumber = validNumbers[Math.floor(Math.random() * validNumbers.length)];
            document.getElementById('randomNumber').innerText = `Random Number: ${randomNumber}`;
        }

        document.addEventListener('DOMContentLoaded', loadSettings);
    </script>
</body>
</html>
