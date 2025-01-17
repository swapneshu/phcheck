<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PH Testing and Analysis</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f2f2f2;
            color: #333;
        }
        header {
            background-color: #4CAF50;
            color: white;
            padding: 15px;
            text-align: center;
        }
        select, button {
            margin: 10px;
            padding: 10px;
        }
        .result {
            margin-top: 20px;
            text-align: center;
        }
        #test-result {
            font-size: 20px;
            color: #4CAF50;
        }
        #upload-section {
            text-align: center;
            margin-top: 20px;
        }
        #file-upload {
            padding: 10px;
        }
        .download-btn {
            background-color: #4CAF50;
            color: white;
            padding: 10px;
            border: none;
            cursor: pointer;
        }
        .download-btn:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <header>
        <h1>Welcome to PH Testing and Analysis</h1>
        <p>Choose a test from the menu below</p>
    </header>
    
    <div style="text-align:center;">
        <label for="test-selection">Select Test:</label>
        <select id="test-selection" onchange="showTestOptions()">
            <option value="">Select Test Category</option>
            <option value="ph">PH Test</option>
            <option value="health">Health Tests</option>
            <option value="household">Household Utilities</option>
            <option value="agriculture">Agriculture</option>
        </select>
    </div>

    <!-- Second Menu (appears after selecting the first one) -->
    <div id="test-options" style="text-align:center; display:none;">
        <label for="specific-test">Select Specific Test:</label>
        <select id="specific-test">
            <!-- Options will be added dynamically based on the first menu selection -->
        </select>
    </div>

    <!-- Upload Section -->
    <div id="upload-section" style="display:none;">
        <h3>Upload Your PH Strip Image:</h3>
        <input type="file" id="file-upload" accept="image/*">
        <button onclick="uploadImage()">Upload Image</button>
    </div>

    <!-- Results Section -->
    <div id="test-result" class="result"></div>

    <!-- Download Section -->
    <div id="download-section" style="text-align:center; display:none;">
        <button class="download-btn" onclick="downloadResult()">Download Result</button>
    </div>

    <script>
        // Function to handle selection of the test category
        function showTestOptions() {
            var testCategory = document.getElementById("test-selection").value;
            var options = document.getElementById("specific-test");
            var testOptionsDiv = document.getElementById("test-options");
            var uploadSection = document.getElementById("upload-section");
            
            testOptionsDiv.style.display = 'block';
            uploadSection.style.display = 'block';
            
            // Clear existing options
            options.innerHTML = '';

            if (testCategory === 'ph') {
                var option1 = document.createElement("option");
                option1.text = "PH Test";
                option1.value = "ph-test";
                options.add(option1);
            } else if (testCategory === 'health') {
                var option1 = document.createElement("option");
                option1.text = "Urine Infection Test";
                option1.value = "urine-infection";
                options.add(option1);
                var option2 = document.createElement("option");
                option2.text = "Menstrual Infection Test";
                option2.value = "menstrual-infection";
                options.add(option2);
            } else if (testCategory === 'household') {
                var option1 = document.createElement("option");
                option1.text = "Soap Test";
                option1.value = "soap-test";
                options.add(option1);
                var option2 = document.createElement("option");
                option2.text = "Shampoo Test";
                option2.value = "shampoo-test";
                options.add(option2);
            } else if (testCategory === 'agriculture') {
                var option1 = document.createElement("option");
                option1.text = "Soil PH Test";
                option1.value = "soil-ph";
                options.add(option1);
                var option2 = document.createElement("option");
                option2.text = "Water PH Test";
                option2.value = "water-ph";
                options.add(option2);
            }
        }

        // Function to upload and process the image
        function uploadImage() {
            var fileInput = document.getElementById('file-upload');
            var file = fileInput.files[0];

            if (!file) {
                alert("Please select a file.");
                return;
            }

            var formData = new FormData();
            formData.append('file', file);

            // Send the image to the Flask backend using AJAX
            fetch('/predict', {
                method: 'POST',
                body: formData
            })
            .then(response => response.json())
            .then(data => {
                if (data.predicted_ph) {
                    document.getElementById('test-result').innerHTML = `<h2>Predicted PH Value: ${data.predicted_ph}</h2>`;
                    document.getElementById('download-section').style.display = 'block';  // Show download button
                } else {
                    document.getElementById('test-result').innerHTML = `<h2>Error: Could not predict PH value</h2>`;
                }
            })
            .catch(error => {
                document.getElementById('test-result').innerHTML = `<h2>Error: ${error}</h2>`;
            });
        }

        // Function to generate the PDF for download
        function downloadResult() {
            var doc = new jsPDF();
            var resultText = document.getElementById('test-result').innerText;
            doc.text(resultText, 10, 10);
            doc.save('PH-Test-Result.pdf');
        }
    </script>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
</body>
</html>
