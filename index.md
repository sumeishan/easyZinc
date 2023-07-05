<html>
<head>
  <title>easyZinc</title>
  <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
  <div class="top-section">
    <header>
      <nav>
        <ul>
          <li><a href="#">About</a></li>
          <li><a href="#">For Providers</a></li>
          <li><a href="#">For Patients</a></li>
        </ul>
      </nav>
      <h1>easyZinc</h1>
      <h3>Research-Based Supplementation for Pediatric IBD Patients</h3>
    </header>
  </div>
 
  <div class="calculator">
    <h1>Supplementation Calculator</h1>
    <div class="calculator">
      <label for="input1">Weight (lbs):</label>
      <input type="number" id="input1">
      <br>
      <label for="input2">Age (years):</label>
      <input type="number" id="input2">
      <br>
      <button onclick="calculate()">Calculate</button>
      <div id="result"></div>
      <div>
        <h3 style="font-weight: bold;">References</h3>
        <ol>
          <li>American Academy of Pediatrics Committee on Nutrition. Liver disease. In: Pediatric Nutrition, 8th ed, Kleinman RE, Greer FR (Eds), American Academy of Pediatrics, 2019. p.1199.</li>
          <li>American Academy of Pediatrics Committee on Nutrition. Trace elements. In: Pediatric Nutrition, 8th ed, Kleinman RE, Greer FR (Eds), American Academy of Pediatrics, 2019. p.591.</li>
        </ol>
      </div>
    </div>
  </div>

  <h2>Results</h2>

  <table style="display: none;"> <!-- Hide the table by default -->
    <thead>
      <tr>
        <th>URL</th>
        <th>Suggested Dosage</th>
        <th>Original Suggested Serving Size</th>
      </tr>
    </thead>
    <tbody id="table-body"> <!-- Added an id attribute -->
      <!-- JavaScript code will generate rows here -->
    </tbody>
  </table>
  
  <script>
    // Your JavaScript code
    const dictionaryObject = {};

    fetch('https://raw.githubusercontent.com/sumeishan/zinc/main/Supplementation%20Spreadsheet%20-%20Sheet1.csv')
      .then(response => response.text())
      .then(data => {
        const rows = data.split('\n').slice(1); // Remove header row

        rows.forEach(row => {
          const [url, origDosage, perMeasure, measure, dosagePerMeasure] = row.split(',');

          dictionaryObject[dosagePerMeasure] = {
            url: url,
            measure: measure, 
            origDosage: origDosage,
            perMeasure: perMeasure
          };
        });

        console.log('Dictionary object:', dictionaryObject);
      })
      .catch(error => {
        console.error('Error fetching CSV:', error);
      });

    // Rest of your JavaScript code...
  </script>
</body>
</html>
