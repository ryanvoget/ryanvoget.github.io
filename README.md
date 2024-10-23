<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pivot Table Example</title>
  <style>
    body {
      font-family: Arial, sans-serif;
    }

    table {
      border-collapse: collapse;
      width: 100%;
    }

    table, th, td {
      border: 1px solid black;
      padding: 8px;
      text-align: left;
    }

    th {
      cursor: pointer;
      background-color: #f2f2f2;
    }

    .filter-input {
      margin-bottom: 10px;
    }

    .summary-row {
      font-weight: bold;
      background-color: #e0e0e0;
    }
  </style>
</head>
<body>

<h2>Pivot Table Example</h2>

<div>
  <label for="filter">Filter by Category:</label>
  <input type="text" id="filter" class="filter-input" placeholder="Enter category">
</div>

<table id="pivot-table">
  <thead>
    <tr>
      <th onclick="sortTable(0)">Category</th>
      <th onclick="sortTable(1)">Subcategory</th>
      <th onclick="sortTable(2)">Amount</th>
    </tr>
  </thead>
  <tbody id="table-body">
    <!-- Data Rows -->
  </tbody>
  <tfoot>
    <tr class="summary-row">
      <td colspan="2">Total</td>
      <td id="total-amount"></td>
    </tr>
  </tfoot>
</table>

<script>
  // Sample data for pivot table
  const data = [
    { category: 'Fruit', subcategory: 'Apple', amount: 120 },
    { category: 'Fruit', subcategory: 'Banana', amount: 80 },
    { category: 'Fruit', subcategory: 'Orange', amount: 100 },
    { category: 'Vegetable', subcategory: 'Carrot', amount: 50 },
    { category: 'Vegetable', subcategory: 'Broccoli', amount: 60 },
    { category: 'Meat', subcategory: 'Chicken', amount: 150 },
    { category: 'Meat', subcategory: 'Beef', amount: 200 }
  ];

  let filteredData = [...data];

  // Function to render the table rows
  function renderTable(data) {
    const tbody = document.getElementById('table-body');
    tbody.innerHTML = '';

    let totalAmount = 0;

    data.forEach(row => {
      const tr = document.createElement('tr');
      tr.innerHTML = `
        <td>${row.category}</td>
        <td>${row.subcategory}</td>
        <td>${row.amount}</td>
      `;
      totalAmount += row.amount;
      tbody.appendChild(tr);
    });

    document.getElementById('total-amount').textContent = totalAmount;
  }

  // Initial render
  renderTable(filteredData);

  // Sorting function
  function sortTable(n) {
    const table = document.getElementById('pivot-table');
    let switching = true, shouldSwitch, switchCount = 0;
    let direction = 'asc';

    while (switching) {
      switching = false;
      const rows = table.rows;
      for (let i = 1; i < (rows.length - 1); i++) {
        shouldSwitch = false;
        const x = rows[i].getElementsByTagName('TD')[n];
        const y = rows[i + 1].getElementsByTagName('TD')[n];

        if (direction === 'asc' && x.innerHTML.toLowerCase() > y.innerHTML.toLowerCase()) {
          shouldSwitch = true;
          break;
        } else if (direction === 'desc' && x.innerHTML.toLowerCase() < y.innerHTML.toLowerCase()) {
          shouldSwitch = true;
          break;
        }
      }

      if (shouldSwitch) {
        rows[i].parentNode.insertBefore(rows[i + 1], rows[i]);
        switching = true;
        switchCount++;
      } else {
        if (switchCount === 0 && direction === 'asc') {
          direction = 'desc';
          switching = true;
        }
      }
    }
  }

  // Filter function
  document.getElementById('filter').addEventListener('input', (event) => {
    const filterValue = event.target.value.toLowerCase();
    filteredData = data.filter(row => row.category.toLowerCase().includes(filterValue));
    renderTable(filteredData);
  });
</script>

</body>
</html>
