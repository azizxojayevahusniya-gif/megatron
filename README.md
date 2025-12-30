
<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<title>SalomChat</title>
<style>
:root {
  --primary: #2563eb;
  --secondary: #1e40af;
  --bg: #f1f5f9;
  --card: #ffffff;
  --text: #0f172a;
  --muted: #475569;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: system-ui, -apple-system, Segoe UI, Roboto, sans-serif;
  background: var(--bg);
  color: var(--text);
}

/* HEADER */
header {
  background: linear-gradient(135deg, var(--primary), var(--secondary));
  color: white;
  padding: 40px 20px;
  text-align: center;
}

header h1 {
  font-size: 36px;
  margin-bottom: 10px;
}

header p {
  font-size: 18px;
  opacity: 0.9;
}

/* MAIN */
main {
  max-width: 900px;
  margin: 60px auto 60px;
  padding: 0 20px;
}

/* CARD */
.card {
  background: var(--card);
  border-radius: 18px;
  padding: 30px;
  box-shadow: 0 20px 40px rgba(0,0,0,0.08);
}

/* SEARCH */
.search {
  display: grid;
  grid-template-columns: 1fr 160px 140px;
  gap: 12px;
  margin-top: 30px;
}

.search input,
.search select,
.search button {
  padding: 14px;
  font-size: 16px;
  border-radius: 12px;
  border: 1px solid #cbd5e1;
}

.search button {
  background: var(--primary);
  color: white;
  border: none;
  cursor: pointer;
}

.search button:hover {
  background: var(--secondary);
}

/* RESULT */
#result {
  margin-top: 30px;
  line-height: 1.8;
}

.result-card {
  display: grid;
  grid-template-columns: 300px 1fr;
  gap: 25px;
  align-items: start;
  margin-top: 20px;
}

.result-card img {
  width: 100%;
  border-radius: 14px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.15);
}

.result-card h3 {
  margin-top: 0;
}

.loading {
  color: var(--muted);
}

/* FOOTER */
footer {
  text-align: center;
  color: var(--muted);
  font-size: 14px;
  padding: 30px 10px;
  margin-top: 40px;
  border-top: 1px solid #cbd5e1;
}

/* RESPONSIVE */
@media (max-width: 800px) {
  .search {
    grid-template-columns: 1fr;
  }
  .result-card {
    grid-template-columns: 1fr;
  }
}
</style>
</head>
<body>

<header>
  <h1>💬 SalomChat</h1>
  <p>Narsa haqida rasm va ma’lumot beruvchi platforma (O‘zbekcha, Inglizcha, Nemischa)</p>
</header>

<main>
  <div class="card">
    <h2>🔍 Qidiruv</h2>
    <p>So‘z yoki nom yozing va Wikipedia’dan ishonchli ma’lumot oling.</p>

    <div class="search">
      <input type="text" id="query" placeholder="Masalan: Computer">
      <select id="lang">
        <option value="en">English</option>
        <option value="uz">O‘zbekcha</option>
        <option value="de">Deutsch</option>
      </select>
      <button onclick="search()">Qidirish</button>
    </div>

    <div id="result"></div>
  </div>
</main>

<footer>
  © 2025 SalomChat · Wikipedia ochiq ma’lumotlari asosida
</footer>

<script>
function search() {
  const query = document.getElementById("query").value.trim();
  const lang = document.getElementById("lang").value;
  const result = document.getElementById("result");

  if (!query) {
    result.innerHTML = "<p class='loading'>So‘z kiriting...</p>";
    return;
  }

  result.innerHTML = "<p class='loading'>⏳ Yuklanmoqda...</p>";

  fetch(`https://${lang}.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(query)}`)
    .then(res => res.json())
    .then(data => {
      if (!data.extract) {
        result.innerHTML = "<p>Ma’lumot topilmadi</p>";
        return;
      }

      const imgHTML = data.thumbnail ? `<img src="${data.thumbnail.source}" alt="${data.title}">` : "";

      result.innerHTML = `
        <div class="result-card">
          ${imgHTML}
          <div>
            <h3>${data.title}</h3>
            <p>${data.extract}</p>
          </div>
        </div>
      `;
    })
    .catch(() => {
      result.innerHTML = "<p>Xatolik yuz berdi</p>";
    });
}
</script>

</body>
</html>
  
