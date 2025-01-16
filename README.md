<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart pH Analyzer</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f9;
            text-align: center;
        }
        .header {
            background-color: #4CAF50;
            color: white;
            padding: 15px 0;
        }
        .container {
            max-width: 700px;
            margin: 20px auto;
            padding: 20px;
            background: white;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            border-radius: 8px;
        }
        .file-upload {
            border: 2px dashed #4CAF50;
            border-radius: 8px;
            padding: 20px;
            cursor: pointer;
            margin: 20px 0;
        }
        .file-upload:hover {
            background-color: #e8f5e9;
        }
        select, button {
            margin: 10px 0;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ddd;
            width: 100%;
        }
        button {
            background-color: #4CAF50;
            color: white;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        .result {
            margin-top: 20px;
            padding: 10px;
            background-color: #f9fbe7;
            border: 1px solid #c5e1a5;
            border-radius: 8px;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Smart pH Analyzer</h1>
        <p>Upload your indicator image, select the test type, and get results instantly!</p>
    </div>
    <div class="container">
        <div class="file-upload" onclick="document.getElementById('imageInput').click();">
            <p>Click here to upload or drag and drop your image.</p>
        </div>
        <input type="file" id="imageInput" accept="image/*" style="display: none;" onchange="previewImage()">
        <select id="testType">
            <option value="" disabled selected>Select the type of test</option>
            <option value="urine">Urine Infection Test</option>
            <option value="menstruation">Menstruation Infection Test</option>
            <option value="water">Water Potability Test</option>
            <option value="soil">Soil Classification</option>
            <option value="food">Food or Detergent Test</option>
        </select>
        <button onclick="analyzeImage()">Analyze</button>
        <div id="result" class="result" style="display: none;">
            <h3>Result</h3>
            <p id="rgb-values"></p>
            <p id="analysis-result"></p>
        </div>
    </div>
    <script>
        let uploadedImage = null;
        function previewImage() {
            const fileInput = document.getElementById('imageInput');
            if (fileInput.files.length === 0) {
                alert('Please upload an image.');
                return;
            }
            const reader = new FileReader();
            reader.onload = function(event) {
                uploadedImage = event.target.result;
                alert('Image uploaded successfully!');
            };
            reader.readAsDataURL(fileInput.files[0]);
        }
        function analyzeImage() {
            const testType = document.getElementById('testType').value;
            if (!uploadedImage) {
                alert('Please upload an image first.');
                return;
            }
            if (!testType) {
                alert('Please select a test type.');
                return;
            }
            const dummyRGB = [120, 100, 200]; // Simulated RGB values for demonstration
            const result = getAnalysis(dummyRGB, testType);
            document.getElementById('rgb-values').innerText = `RGB Values: (${dummyRGB[0]}, ${dummyRGB[1]}, ${dummyRGB[2]})`;
            document.getElementById('analysis-result').innerText = result;
            document.getElementById('result').style.display = 'block';
        }
        function getAnalysis(rgb, testType) {
            const [r, g, b] = rgb;
            if (testType === "urine") {
                if (r > 150) return "Infection detected in urine!";
                return "Urine is healthy.";
            }
            if (testType === "menstruation") {
                if (g > 120) return "Menstrual infection detected!";
                return "No infection during menstruation.";
            }
            if (testType === "water") {
                if (b > 180) return "Water is drinkable.";
                return "Water is not drinkable.";
            }
            if (testType === "soil") {
                if (r < 100) return "Soil is acidic.";
                if (b > 150) return "Soil is alkaline.";
                return "Soil is neutral.";
            }
            if (testType === "food") {
                if (r > 200) return "High acidity in food!";
                if (b > 200) return "Food/detergent is alkaline.";
                return "Food is neutral.";
            }
            return "Unknown test type.";
        }
    </script>
</body>
</html>
