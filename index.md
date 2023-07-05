 
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

    fetch('/Supplementation%20Spreadsheet%20-%20Sheet1.csv')
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

    // Function to calculate the number of measures and determine the appropriate measure word
    function getNumberOfMeasures(first_value = 40, dosagePerMeasure, measure) {
      var { n: recommended_dose, duration: recommended_duration } = calculate_supplementation(first_value); // Fixed variable name
      const dividend = Math.floor(recommended_dose / dosagePerMeasure); // Fixed variable name
      return dividend;
    }

    // Array of measure word pairs [singular, plural]
    const measureWordPairs = [
      ['gummy', 'gummies'],
      ['capsule', 'capsules'],
      ['drop', 'drops'],
      ['tablet', 'tablets'],
      ['spray', 'sprays']
    ];

    // Function to get the singular version of the measure word
    function getMeasureSingular(measure) {
      const pair = measureWordPairs.find(([singular, _]) => measure === singular);
      return pair ? pair[0] : measure;
    }

    // Function to get the plural version of the measure word
    function getMeasurePlural(measure) {
      const pair = measureWordPairs.find(([_ , plural]) => measure === plural);
      return pair ? pair[1] : measure;
    }

    // Function to calculate the total dosage
    function calculateTotalDosage(numberOfMeasures, dosagePerMeasure) {
      const totalDosage = numberOfMeasures * dosagePerMeasure;
      return totalDosage.toFixed(1);
    }

    // Function to calculate the supplementation values
    function calculate_supplementation(weight_in_lbs) {
      let recommended_dose = 0;
      let recommended_duration = "4 to 6 weeks, depending on severity";
      let weight_in_kg = weight_in_lbs * 0.45359237;
      recommended_dose = Math.round(weight_in_kg * 10) / 10;
      return { n: recommended_dose, duration: recommended_duration };
    }

    // Function to perform the calculation and display the result
    function calculate() {
      var value1 = parseInt(document.getElementById("input1").value);
      var value2 = parseInt(document.getElementById("input2").value);
      var { n: recommended_dose, duration: recommended_duration } = calculate_supplementation(value1);
      var resultDiv = document.getElementById("result");
      resultDiv.innerHTML = "Recommended Dosage: " + recommended_dose + " mg per day" + "<br>Recommended Duration: " + recommended_duration;

      // Clear existing table rows
      const tbody = document.getElementById('table-body'); // Use the id attribute set for tbody
      tbody.innerHTML = '';

      // Generate new table rows
      for (const dosage in dictionaryObject) {
        const { url, measure, origDosage, perMeasure } = dictionaryObject[dosage];

        const row = document.createElement('tr');
        const numberOfMeasures = getNumberOfMeasures(value1, perMeasure, measure);
        const suggestedDosage = numberOfMeasures === 1 ? getMeasureSingular(measure) : getMeasurePlural(measure);
        const originalSuggestedServing = `${origDosage} mg for every ${perMeasure} ${measure}`;

        row.innerHTML = `
          <td>${url}</td>
          <td>${numberOfMeasures} ${suggestedDosage} (Total Intake: ${calculateTotalDosage(numberOfMeasures, dosage)} mg)</td>
          <td>${originalSuggestedServing}</td>
        `;

        tbody.appendChild(row);
      }

      // Show the results table
      var table = document.querySelector('table');
      table.style.display = 'table';
    }
  </script>
</body>
</html>

