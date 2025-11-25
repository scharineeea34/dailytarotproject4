<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Daily Tarot — Pink Oracle</title>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <header class="site-header">
    <div class="brand">
      <div class="logo">🔮</div>
      <h1>Pink Oracle</h1>
    </div>
    <p class="tag">Daily Tarot • Yes / No • Three‑Card Reading</p>
  </header>

  <main class="container">
    <section id="daily" class="card-section">
      <h2>Daily Tarot</h2>
      <p class="muted">Your deterministic daily insight (based on date + user)</p>
      <div class="controls">
        <label for="username">Username (optional)</label>
        <input id="username" placeholder="e.g. scharineeea34" />
        <button id="daily-btn" class="btn">Reveal Today's Card</button>
      </div>
      <div id="daily-result" class="results"></div>
    </section>

    <section id="yesno" class="card-section">
      <h2>Yes / No Oracle</h2>
      <p class="muted">Ask a yes/no question, then draw one card.</p>
      <div class="controls">
        <input id="yesno-question" placeholder="Type your yes/no question..." />
        <button id="yesno-btn" class="btn">Draw</button>
      </div>
      <div id="yesno-result" class="results"></div>
    </section>

    <section id="threecard" class="card-section">
      <h2>Three‑Card Reading</h2>
      <p class="muted">Past · Present · Future</p>
      <div class="controls">
        <label for="shuffle">Shuffle mode</label>
        <select id="shuffle">
          <option value="random">Random</option>
          <option value="seeded">Deterministic (date + user)</option>
        </select>
        <button id="three-btn" class="btn">Reveal Three Cards</button>
      </div>
      <div id="three-result" class="results three-cards"></div>
    </section>

    <section class="about card-section">
      <h3>Notes</h3>
      <ul>
        <li>Daily reading is deterministic by default: same for a given username + date.</li>
        <li>Cards can show upright or reversed meanings.</li>
        <li>Replace the art with your images by editing script.js card objects.</li>
      </ul>
    </section>
  </main>

  <footer class="site-footer">
    <small>Made with pink vibes • Open-source</small>
  </footer>

  <script src="script.js"></script>
</body>
</html>
