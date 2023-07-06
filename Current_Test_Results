
<html>
<head>
  <title>Current Test Results</title>
  <style>
    table {
      border-collapse: collapse;
    }

    th, td {
      border: 1px solid black;
      padding: 8px;
      text-align: left;
    }
  </style>
</head>
<body>
  <table>
    <tr>
      <th>Product Name</th>
      <th>Image</th>
    </tr>
    
    <!-- JavaScript code to read CSV file and populate the table -->
    <script>
      fetch('https://raw.githubusercontent.com/sumeishan/zinc/main/df.csv')
        .then(response => response.text())
        .then(csvData => {
          const rows = csvData.split('\n');
          const headers = rows[0].split(',');
          
          for (let i = 1; i < rows.length; i++) {
            const row = rows[i].split(',');
            const productName = row[headers.indexOf('Product Name')];
            const imageUrl = row[headers.indexOf('Image1')];
            const url = row[headers.indexOf('URL')];
            
            const tableRow = document.createElement('tr');
            const nameCell = document.createElement('td');
            const imageCell = document.createElement('td');
            const image = document.createElement('img');
            const link = document.createElement('a');
            
            nameCell.textContent = productName;
            link.href = url;
            image.src = imageUrl;
            image.alt = productName;
            link.appendChild(image);
            imageCell.appendChild(link);
            
            tableRow.appendChild(nameCell);
            tableRow.appendChild(imageCell);
            
            document.querySelector('table').appendChild(tableRow);
          }
        });
    </script>
  </table>
</body>
</html>
