<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>What's in My Medicine?</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f8fb;
      padding: 20px;
      text-align: center;
    }
    h1 {
      color: #2c3e50;
    }
    input {
      padding: 10px;
      width: 250px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    button {
      padding: 10px 15px;
      border: none;
      background-color: #3498db;
      color: white;
      border-radius: 5px;
      margin-left: 10px;
      cursor: pointer;
    }
    .result {
      margin-top: 30px;
      text-align: left;
      max-width: 600px;
      margin-left: auto;
      margin-right: auto;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    .error {
      color: red;
    }
  </style>
</head>
<body>
  <h1>What's in My Medicine?</h1>
  <p>Enter a drug name to find its official FDA label information.</p>
  <input type="text" id="drugName" placeholder="e.g. Advil" />
  <button onclick="fetchDrugInfo()">Search</button>
  <div id="output" class="result"></div>

  <script>
    function fetchDrugInfo() {
      const drug = document.getElementById('drugName').value.trim();
      const output = document.getElementById('output');
      output.innerHTML = '';

      if (!drug) {
        output.innerHTML = '<p class="error">Please enter a drug name.</p>';
        return;
      }

      fetch(`https://api.fda.gov/drug/label.json?search=openfda.brand_name:${drug}&limit=1`)
        .then(response => response.json())
        .then(data => {
          const info = data.results[0];
          const html = `
            <h2>${drug}</h2>
            <p><strong>Purpose:</strong> ${info.purpose ? info.purpose.join('<br>') : 'N/A'}</p>
            <p><strong>Uses:</strong> ${info.indications_and_usage ? info.indications_and_usage.join('<br>') : 'N/A'}</p>
            <p><strong>Warnings:</strong> ${info.warnings ? info.warnings.join('<br>') : 'N/A'}</p>
            <p><strong>Dosage:</strong> ${info.dosage_and_administration ? info.dosage_and_administration.join('<br>') : 'N/A'}</p>
          `;
          output.innerHTML = html;
        })
        .catch(err => {
          console.error(err);
          output.innerHTML = '<p class="error">No data found. Please try a different drug name.</p>';
        });
    }
  </script>
</body>
</html>
# whats-in-my-medicine
