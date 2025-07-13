<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>تسجيل الدخول برقم الهاتف</title>

  <!-- Firebase -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.24.0/firebase-app.js";
    import { getDatabase, ref, get, child } from "https://www.gstatic.com/firebasejs/9.24.0/firebase-database.js";

    const firebaseConfig = {
      apiKey: "AIzaSyBksUQfseucTTUTrM8o-54kQizUYrcXlQQ",
      authDomain: "abu-shawqi-bouquets.firebaseapp.com",
      databaseURL: "https://abu-shawqi-bouquets-default-rtdb.firebaseio.com",
      projectId: "abu-shawqi-bouquets",
      storageBucket: "abu-shawqi-bouquets.appspot.com",
      messagingSenderId: "624365660732",
      appId: "1:624365660732:web:da0b73e43cfde85a863139"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    window.login = async function () {
      const phone = document.getElementById("phone").value.trim();
      if (phone === "") {
        alert("📱 يرجى إدخال رقم الهاتف");
        return;
      }

      const dbRef = ref(db);
      const snapshot = await get(child(dbRef, "allowedPhones/" + phone));
      if (snapshot.exists()) {
        alert("✅ مرحبًا بك! تم تسجيل الدخول.");
        window.location.href = "home.html"; // توجيه إلى الصفحة المطلوبة
      } else {
        alert("❌ هذا الرقم غير مصرح له بالدخول.");
      }
    };
  </script>

  <style>
    body {
      font-family: 'Cairo', sans-serif;
      background: #f5f5f5;
      padding: 40px;
      text-align: center;
    }
    input, button {
      padding: 12px;
      margin: 10px;
      font-size: 16px;
      width: 80%;
      max-width: 300px;
    }
  </style>
</head>
<body>

  <h2>تسجيل الدخول برقم الهاتف</h2>
  <input type="text" id="phone" placeholder="مثال: +201234567890" />
  <br>
  <button onclick="login()">🚪 دخول</button>

</body>
</html>
