<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>تسجيل الدخول برقم الهاتف</title>

  <!-- Firebase SDKs -->
  <script type="module">
    // استيراد Firebase (باستخدام ES Modules)
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.24.0/firebase-app.js";
    import { getAuth, RecaptchaVerifier, signInWithPhoneNumber } from "https://www.gstatic.com/firebasejs/9.24.0/firebase-auth.js";

    // إعدادات Firebase
    const firebaseConfig = {
      apiKey: "AIzaSyBksUQfseucTTUTrM8o-54kQizUYrcXlQQ",
      authDomain: "abu-shawqi-bouquets.firebaseapp.com",
      databaseURL: "https://abu-shawqi-bouquets-default-rtdb.firebaseio.com",
      projectId: "abu-shawqi-bouquets",
      storageBucket: "abu-shawqi-bouquets.appspot.com",
      messagingSenderId: "624365660732",
      appId: "1:624365660732:web:da0b73e43cfde85a863139",
      measurementId: "G-F39RHCPTF5"
    };

    // تهيئة Firebase
    const app = initializeApp(firebaseConfig);
    const auth = getAuth(app);

    // تهيئة reCAPTCHA
    window.recaptchaVerifier = new RecaptchaVerifier('recaptcha-container', {
      size: 'normal',
      callback: (response) => {
        console.log("reCAPTCHA verified");
      }
    }, auth);

    let confirmationResult;

    window.sendOTP = function () {
      const phoneNumber = document.getElementById("phoneNumber").value;
      signInWithPhoneNumber(auth, phoneNumber, window.recaptchaVerifier)
        .then((result) => {
          confirmationResult = result;
          document.getElementById("login-form").style.display = "none";
          document.getElementById("verify-form").style.display = "block";
          alert("تم إرسال الكود إلى رقم الهاتف.");
        }).catch((error) => {
          alert("خطأ: " + error.message);
        });
    };

    window.verifyOTP = function () {
      const otp = document.getElementById("otp").value;
      confirmationResult.confirm(otp).then((result) => {
        const user = result.user;
        alert("تم تسجيل الدخول بنجاح! رقمك: " + user.phoneNumber);
        // يمكن توجيه المستخدم هنا إلى صفحة الحساب مثلاً
      }).catch((error) => {
        alert("الكود غير صحيح أو منتهي: " + error.message);
      });
    };
  </script>

  <style>
    body {
      font-family: 'Cairo', sans-serif;
      padding: 20px;
      background: #f4f4f4;
      text-align: center;
    }
    input, button {
      margin: 10px;
      padding: 10px;
      font-size: 16px;
      width: 80%;
      max-width: 300px;
    }
    #recaptcha-container {
      margin-top: 15px;
    }
  </style>
</head>
<body>

  <h2>تسجيل الدخول برقم الهاتف</h2>

  <div id="login-form">
    <input type="text" id="phoneNumber" placeholder="مثال: +201234567890" />
    <div id="recaptcha-container"></div>
    <button onclick="sendOTP()">إرسال الكود</button>
  </div>

  <div id="verify-form" style="display:none;">
    <input type="text" id="otp" placeholder="ادخل الكود المرسل" />
    <button onclick="verifyOTP()">تأكيد الكود</button>
  </div>

</body>
</html>
