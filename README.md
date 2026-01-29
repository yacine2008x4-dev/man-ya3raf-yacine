<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>من يعرف ياسين؟</title>
  <style>
    body {
      margin: 0;
      font-family: Tahoma, sans-serif;
      background: radial-gradient(circle, #0f2027, #203a43, #2c5364);
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .box {
      background: rgba(0,0,0,0.7);
      padding: 25px;
      width: 90%;
      max-width: 400px;
      border-radius: 15px;
      box-shadow: 0 0 25px #00ffff;
      text-align: center;
      animation: glow 2s infinite alternate;
    }

    @keyframes glow {
      from { box-shadow: 0 0 15px #00ffff; }
      to   { box-shadow: 0 0 35px #00ffff; }
    }

    h2 {
      margin-bottom: 20px;
    }

    button {
      display: block;
      width: 100%;
      margin: 10px 0;
      padding: 12px;
      border: none;
      border-radius: 10px;
      background: #00ffff;
      color: black;
      font-size: 16px;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      background: #00bbbb;
      transform: scale(1.03);
    }

    input {
      width: 100%;
      padding: 12px;
      border-radius: 10px;
      border: none;
      font-size: 16px;
      margin-bottom: 15px;
      text-align: center;
    }
  </style>
</head>
<body>

<div class="box" id="app"></div>

<script>
  const app = document.getElementById("app");
  let score = 0;
  let username = "";

  const questions = [
    {
      q: "📅 شهر ميلادي؟",
      options: ["2006", "2007", "2008", "2009"],
      answer: "2008"
    },
    {
      q: "🎬 هوايتي؟",
      options: ["تصوير", "مونتاج", "رياضة", "رسم"],
      answer: "مونتاج"
    },
    {
      q: "😴 ماذا أحب؟",
      options: ["أكل", "سفر", "نوم", "لعب"],
      answer: "نوم"
    },
    {
      q: "🐱 حيواني المفضل؟",
      options: ["كلاب", "طيور", "قطط", "أسود"],
      answer: "قطط"
    },
    {
      q: "👑 من أنا؟",
      options: ["محمد", "علي", "ياسين", "أحمد"],
      answer: "ياسين"
    }
  ];

  let index = 0;

  function start() {
    app.innerHTML = `
      <h2>🌀 من يعرف ياسين؟</h2>
      <input placeholder="اكتب اسمك" id="nameInput">
      <button onclick="saveName()">ابدأ التحدي ⚔️</button>
    `;
  }

  function saveName() {
    const input = document.getElementById("nameInput").value;
    if (!input) return alert("دخل اسمك 🙂");
    username = input;
    showQuestion();
  }

  function showQuestion() {
    const q = questions[index];
    let html = `<h2>${q.q}</h2>`;
    q.options.forEach(opt => {
      html += `<button onclick="check('${opt}')">${opt}</button>`;
    });
    app.innerHTML = html;
  }

  function check(choice) {
    if (choice === questions[index].answer) {
      score++;
    }
    index++;
    if (index < questions.length) {
      showQuestion();
    } else {
      finish();
    }
  }

  function finish() {
    let players = JSON.parse(localStorage.getItem("players")) || [];
    players.push({ name: username, score: score });
    players.sort((a,b) => b.score - a.score);
    localStorage.setItem("players", JSON.stringify(players));

    const rank = players.findIndex(p => p.name === username && p.score === score) + 1;

    app.innerHTML = `
      <h2>🔥 النتيجة النهائية</h2>
      <p>اسمك: <b>${username}</b></p>
      <p>نقاطك: <b>${score} / 5</b></p>
      <p>ترتيبك: <b>#${rank}</b></p>
      <button onclick="start()">إعادة 🔁</button>
    `;
  }

  start();
</script>

</body>
</html>
