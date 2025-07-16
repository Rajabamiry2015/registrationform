<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>فرم ثبت‌نام دوره زبان فارسی</title>
  <style>
    body {
      font-family: sans-serif;
      background: #f0f4f8;
      margin: 0;
      padding: 20px;
    }
    h2 {
      text-align: center;
      color: #333;
    }
    form {
      max-width: 500px;
      margin: auto;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    input, select, button {
      width: 100%;
      padding: 10px;
      margin-top: 10px;
      border-radius: 5px;
      border: 1px solid #ccc;
      font-size: 16px;
    }
    button {
      background-color: #007bff;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
    .admin-section {
      margin-top: 40px;
      max-width: 700px;
      margin-left: auto;
      margin-right: auto;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      background: white;
    }
    th, td {
      padding: 8px;
      border: 1px solid #ccc;
      text-align: center;
    }
    #adminTable {
      display: none;
    }
  </style>
</head>
<body>
  <h2>فرم ثبت‌نام دوره آنلاین زبان فارسی</h2>
  <form id="signupForm">
    <label for="name">نام کامل:</label>
    <input type="text" id="name" required />

    <label for="phone">شماره تماس:</label>
    <input type="tel" id="phone" required />

    <label for="email">ایمیل:</label>
    <input type="email" id="email" required />

    <label for="level">سطح دوره:</label>
    <select id="level" required>
      <option value="">-- انتخاب کنید --</option>
      <option value="مبتدی">مبتدی</option>
      <option value="متوسط">متوسط</option>
      <option value="پیشرفته">پیشرفته</option>
    </select>

    <button type="submit">ثبت‌نام</button>
  </form>

  <div class="admin-section">
    <h2>🔐 فقط مدیر: لیست ثبت‌نام‌شدگان</h2>
    <button onclick="toggleTable()">نمایش/مخفی جدول</button>
    <table id="adminTable">
      <thead>
        <tr>
          <th>نام</th>
          <th>شماره تماس</th>
          <th>ایمیل</th>
          <th>سطح</th>
        </tr>
      </thead>
      <tbody id="userTableBody"></tbody>
    </table>
  </div>

  <script>
    const form = document.getElementById('signupForm');
    const tableBody = document.getElementById('userTableBody');
    const adminTable = document.getElementById('adminTable');

    function toggleTable() {
      adminTable.style.display = adminTable.style.display === 'none' ? 'table' : 'none';
    }

    function saveToLocalStorage(entry) {
      const entries = JSON.parse(localStorage.getItem('registrations')) || [];
      entries.push(entry);
      localStorage.setItem('registrations', JSON.stringify(entries));
    }

    function loadFromLocalStorage() {
      const entries = JSON.parse(localStorage.getItem('registrations')) || [];
      entries.forEach(entry => addEntryToTable(entry));
    }

    function addEntryToTable(entry) {
      const newRow = document.createElement('tr');
      newRow.innerHTML = `
        <td>${entry.name}</td>
        <td>${entry.phone}</td>
        <td>${entry.email}</td>
        <td>${entry.level}</td>
      `;
      tableBody.appendChild(newRow);
    }

    form.addEventListener('submit', function(e) {
      e.preventDefault();
      const name = document.getElementById('name').value.trim();
      const phone = document.getElementById('phone').value.trim();
      const email = document.getElementById('email').value.trim();
      const level = document.getElementById('level').value;

      if (name && phone && email && level) {
        const newEntry = { name, phone, email, level };
        saveToLocalStorage(newEntry);
        addEntryToTable(newEntry);
        form.reset();
        alert("ثبت‌نام با موفقیت انجام شد!");
      }
    });

    // بارگذاری اطلاعات هنگام لود صفحه
    window.onload = loadFromLocalStorage;
  </script>
</body>
</html>
