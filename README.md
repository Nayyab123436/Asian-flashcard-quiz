# Asian-flashcard-quiz

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Asia Flashcard Quiz</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=Inter:wght@400;500;600&display=swap');

  :root {
    --bg: #0f1a2e;
    --surface: #162035;
    --card-front: #1b2a45;
    --card-back: #0e2a1f;
    --accent: #f0a500;
    --accent2: #3be8a0;
    --text: #e8eaf0;
    --muted: #7a8aaa;
    --correct: #3be8a0;
    --wrong: #f05060;
    --border: rgba(255,255,255,0.08);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Inter', sans-serif;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 24px 16px 48px;
  }

  header {
    text-align: center;
    margin-bottom: 32px;
  }

  header h1 {
    font-family: 'Playfair Display', serif;
    font-size: clamp(1.8rem, 5vw, 2.8rem);
    color: var(--accent);
    letter-spacing: 0.01em;
    line-height: 1.2;
  }

  header p {
    color: var(--muted);
    margin-top: 8px;
    font-size: 0.9rem;
  }

  /* Region Filter */
  .filter-bar {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    justify-content: center;
    margin-bottom: 28px;
    max-width: 700px;
  }

  .filter-btn {
    padding: 6px 14px;
    border-radius: 20px;
    border: 1px solid var(--border);
    background: var(--surface);
    color: var(--muted);
    font-size: 0.78rem;
    font-family: 'Inter', sans-serif;
    cursor: pointer;
    transition: all 0.2s;
    font-weight: 500;
  }

  .filter-btn:hover { border-color: var(--accent); color: var(--accent); }
  .filter-btn.active { background: var(--accent); color: #0f1a2e; border-color: var(--accent); font-weight: 600; }

  /* Progress */
  .progress-bar-wrap {
    width: 100%;
    max-width: 560px;
    height: 4px;
    background: var(--border);
    border-radius: 4px;
    margin-bottom: 10px;
  }
  .progress-bar-fill {
    height: 100%;
    background: var(--accent);
    border-radius: 4px;
    transition: width 0.4s ease;
  }

  .stats {
    display: flex;
    gap: 24px;
    font-size: 0.8rem;
    color: var(--muted);
    margin-bottom: 24px;
  }
  .stats span { display: flex; align-items: center; gap: 5px; }
  .stats .dot { width: 8px; height: 8px; border-radius: 50%; display: inline-block; }
  .dot-correct { background: var(--correct); }
  .dot-wrong { background: var(--wrong); }

  /* Card Scene */
  .scene {
    width: 100%;
    max-width: 480px;
    height: 240px;
    perspective: 1200px;
    cursor: pointer;
    margin-bottom: 20px;
  }

  .card {
    width: 100%;
    height: 100%;
    position: relative;
    transform-style: preserve-3d;
    transition: transform 0.55s cubic-bezier(0.4,0,0.2,1);
  }

  .card.flipped { transform: rotateY(180deg); }

  .card-face {
    position: absolute;
    inset: 0;
    border-radius: 18px;
    backface-visibility: hidden;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 28px 32px;
    border: 1px solid var(--border);
  }

  .card-front {
    background: var(--card-front);
  }

  .card-back {
    background: var(--card-back);
    transform: rotateY(180deg);
    border-color: rgba(59,232,160,0.15);
  }

  .card-label {
    font-size: 0.68rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: var(--muted);
    margin-bottom: 10px;
    font-weight: 600;
  }

  .card-country {
    font-family: 'Playfair Display', serif;
    font-size: clamp(1.8rem, 5vw, 2.6rem);
    text-align: center;
    color: var(--text);
    line-height: 1.2;
  }

  .card-emoji {
    font-size: 2.4rem;
    margin-bottom: 10px;
  }

  .card-region {
    font-size: 1.5rem;
    font-weight: 700;
    color: var(--accent2);
    font-family: 'Playfair Display', serif;
    text-align: center;
    margin-bottom: 6px;
  }

  .card-flag {
    font-size: 3rem;
    margin-bottom: 6px;
  }

  .card-hint {
    font-size: 0.78rem;
    color: var(--muted);
    margin-top: 12px;
  }

  /* Buttons */
  .btn-row {
    display: flex;
    gap: 12px;
    margin-bottom: 16px;
  }

  .btn {
    padding: 10px 28px;
    border-radius: 10px;
    border: none;
    font-family: 'Inter', sans-serif;
    font-size: 0.88rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.18s;
  }

  .btn-correct {
    background: var(--correct);
    color: #0f1a2e;
  }
  .btn-correct:hover { opacity: 0.88; transform: translateY(-1px); }

  .btn-wrong {
    background: var(--wrong);
    color: #fff;
  }
  .btn-wrong:hover { opacity: 0.88; transform: translateY(-1px); }

  .btn-skip {
    background: var(--surface);
    color: var(--muted);
    border: 1px solid var(--border);
  }
  .btn-skip:hover { border-color: var(--accent); color: var(--accent); }

  .btn-flip {
    background: var(--accent);
    color: #0f1a2e;
    padding: 10px 32px;
    margin-bottom: 16px;
  }
  .btn-flip:hover { opacity: 0.88; transform: translateY(-1px); }

  /* Navigation */
  .nav-row {
    display: flex;
    align-items: center;
    gap: 16px;
    color: var(--muted);
    font-size: 0.82rem;
    margin-bottom: 28px;
  }

  .nav-btn {
    background: var(--surface);
    border: 1px solid var(--border);
    color: var(--muted);
    font-size: 1rem;
    padding: 6px 14px;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.18s;
    font-family: 'Inter', sans-serif;
  }
  .nav-btn:hover:not(:disabled) { border-color: var(--accent); color: var(--accent); }
  .nav-btn:disabled { opacity: 0.3; cursor: default; }

  /* Result Screen */
  .result-screen {
    display: none;
    flex-direction: column;
    align-items: center;
    gap: 16px;
    text-align: center;
    max-width: 480px;
    width: 100%;
  }
  .result-screen.visible { display: flex; }

  .result-score {
    font-family: 'Playfair Display', serif;
    font-size: 4rem;
    color: var(--accent);
    line-height: 1;
  }
  .result-label { color: var(--muted); font-size: 0.88rem; }

  .result-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    width: 100%;
    margin: 8px 0;
  }

  .result-box {
    background: var(--surface);
    border-radius: 12px;
    padding: 14px;
    border: 1px solid var(--border);
  }

  .result-box .num {
    font-family: 'Playfair Display', serif;
    font-size: 2rem;
  }

  .result-box .lbl { font-size: 0.75rem; color: var(--muted); }

  .btn-restart {
    background: var(--accent);
    color: #0f1a2e;
    padding: 12px 36px;
    border-radius: 12px;
    border: none;
    font-size: 0.92rem;
    font-weight: 700;
    cursor: pointer;
    font-family: 'Inter', sans-serif;
    transition: all 0.18s;
  }
  .btn-restart:hover { opacity: 0.88; transform: translateY(-1px); }

  /* Quiz Area */
  .quiz-area { width: 100%; max-width: 480px; display: flex; flex-direction: column; align-items: center; }

  .feedback-tag {
    font-size: 0.75rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    padding: 3px 10px;
    border-radius: 6px;
    margin-bottom: 10px;
    opacity: 0;
    transition: opacity 0.3s;
  }
  .feedback-tag.show { opacity: 1; }
  .feedback-tag.correct { background: rgba(59,232,160,0.15); color: var(--correct); }
  .feedback-tag.wrong { background: rgba(240,80,96,0.15); color: var(--wrong); }

  @media (max-width: 480px) {
    .scene { height: 210px; }
    .btn { padding: 9px 20px; font-size: 0.82rem; }
  }
</style>
</head>
<body>

<header>
  <h1>🌏 Asia Flashcard Quiz</h1>
  <p>48 countries · 5 regions · Flip to reveal the answer</p>
</header>

<div class="filter-bar" id="filterBar"></div>

<div class="progress-bar-wrap">
  <div class="progress-bar-fill" id="progressFill" style="width:0%"></div>
</div>

<div class="stats">
  <span><span class="dot dot-correct"></span><span id="statCorrect">0</span> Correct</span>
  <span><span class="dot dot-wrong"></span><span id="statWrong">0</span> Wrong</span>
  <span id="statCard">Card 1 / 48</span>
</div>

<div class="quiz-area" id="quizArea">
  <div class="feedback-tag" id="feedbackTag"></div>

  <div class="scene" id="scene" onclick="flipCard()">
    <div class="card" id="card">
      <div class="card-face card-front" id="cardFront">
        <div class="card-label">Country</div>
        <div class="card-emoji" id="cardFlag">🌏</div>
        <div class="card-country" id="cardCountry">Loading...</div>
        <div class="card-hint">Click to reveal region</div>
      </div>
      <div class="card-face card-back" id="cardBack">
        <div class="card-label">Region</div>
        <div class="card-flag" id="cardFlagBack">🌏</div>
        <div class="card-region" id="cardRegion">—</div>
        <div class="card-hint" id="cardExtra"></div>
      </div>
    </div>
  </div>

  <button class="btn btn-flip" onclick="flipCard()" id="flipBtn">👆 Flip Card</button>

  <div class="btn-row" id="answerBtns" style="display:none">
    <button class="btn btn-correct" onclick="markAnswer(true)">✓ Got it</button>
    <button class="btn btn-wrong" onclick="markAnswer(false)">✗ Missed</button>
  </div>

  <div class="nav-row">
    <button class="nav-btn" id="prevBtn" onclick="goTo(-1)" disabled>← Prev</button>
    <span id="navLabel">1 / 48</span>
    <button class="nav-btn" id="nextBtn" onclick="goTo(1)">Next →</button>
    <button class="btn-skip btn btn-skip" onclick="goTo(1)">Skip</button>
  </div>
</div>

<div class="result-screen" id="resultScreen">
  <div style="font-size:3rem">🎉</div>
  <div class="result-score" id="resultScore">0%</div>
  <div class="result-label">Quiz Complete!</div>
  <div class="result-grid">
    <div class="result-box">
      <div class="num" style="color:var(--correct)" id="resCorrect">0</div>
      <div class="lbl">Correct</div>
    </div>
    <div class="result-box">
      <div class="num" style="color:var(--wrong)" id="resWrong">0</div>
      <div class="lbl">Missed</div>
    </div>
    <div class="result-box">
      <div class="num" style="color:var(--accent)" id="resSkipped">0</div>
      <div class="lbl">Skipped</div>
    </div>
    <div class="result-box">
      <div class="num" id="resTotal">0</div>
      <div class="lbl">Total Cards</div>
    </div>
  </div>
  <button class="btn-restart" onclick="restartQuiz()">🔄 Restart Quiz</button>
</div>

<script>
const ALL_COUNTRIES = [
  // South Asia
  { country: "Afghanistan", region: "South Asia", flag: "🇦🇫", capital: "Kabul" },
  { country: "Bangladesh", region: "South Asia", flag: "🇧🇩", capital: "Dhaka" },
  { country: "Bhutan", region: "South Asia", flag: "🇧🇹", capital: "Thimphu" },
  { country: "India", region: "South Asia", flag: "🇮🇳", capital: "New Delhi" },
  { country: "Maldives", region: "South Asia", flag: "🇲🇻", capital: "Malé" },
  { country: "Nepal", region: "South Asia", flag: "🇳🇵", capital: "Kathmandu" },
  { country: "Pakistan", region: "South Asia", flag: "🇵🇰", capital: "Islamabad" },
  { country: "Sri Lanka", region: "South Asia", flag: "🇱🇰", capital: "Sri Jayawardenepura Kotte" },

  // East Asia
  { country: "China", region: "East Asia", flag: "🇨🇳", capital: "Beijing" },
  { country: "Japan", region: "East Asia", flag: "🇯🇵", capital: "Tokyo" },
  { country: "Mongolia", region: "East Asia", flag: "🇲🇳", capital: "Ulaanbaatar" },
  { country: "North Korea", region: "East Asia", flag: "🇰🇵", capital: "Pyongyang" },
  { country: "South Korea", region: "East Asia", flag: "🇰🇷", capital: "Seoul" },
  { country: "Taiwan", region: "East Asia", flag: "🇹🇼", capital: "Taipei" },

  // Southeast Asia
  { country: "Brunei", region: "Southeast Asia", flag: "🇧🇳", capital: "Bandar Seri Begawan" },
  { country: "Cambodia", region: "Southeast Asia", flag: "🇰🇭", capital: "Phnom Penh" },
  { country: "East Timor (Timor-Leste)", region: "Southeast Asia", flag: "🇹🇱", capital: "Dili" },
  { country: "Indonesia", region: "Southeast Asia", flag: "🇮🇩", capital: "Jakarta" },
  { country: "Laos", region: "Southeast Asia", flag: "🇱🇦", capital: "Vientiane" },
  { country: "Malaysia", region: "Southeast Asia", flag: "🇲🇾", capital: "Kuala Lumpur" },
  { country: "Myanmar (Burma)", region: "Southeast Asia", flag: "🇲🇲", capital: "Naypyidaw" },
  { country: "Philippines", region: "Southeast Asia", flag: "🇵🇭", capital: "Manila" },
  { country: "Singapore", region: "Southeast Asia", flag: "🇸🇬", capital: "Singapore" },
  { country: "Thailand", region: "Southeast Asia", flag: "🇹🇭", capital: "Bangkok" },
  { country: "Vietnam", region: "Southeast Asia", flag: "🇻🇳", capital: "Hanoi" },

  // West Asia (Middle East)
  { country: "Armenia", region: "West Asia", flag: "🇦🇲", capital: "Yerevan" },
  { country: "Azerbaijan", region: "West Asia", flag: "🇦🇿", capital: "Baku" },
  { country: "Bahrain", region: "West Asia", flag: "🇧🇭", capital: "Manama" },
  { country: "Cyprus", region: "West Asia", flag: "🇨🇾", capital: "Nicosia" },
  { country: "Georgia", region: "West Asia", flag: "🇬🇪", capital: "Tbilisi" },
  { country: "Iran", region: "West Asia", flag: "🇮🇷", capital: "Tehran" },
  { country: "Iraq", region: "West Asia", flag: "🇮🇶", capital: "Baghdad" },
  { country: "Israel", region: "West Asia", flag: "🇮🇱", capital: "Jerusalem" },
  { country: "Jordan", region: "West Asia", flag: "🇯🇴", capital: "Amman" },
  { country: "Kuwait", region: "West Asia", flag: "🇰🇼", capital: "Kuwait City" },
  { country: "Lebanon", region: "West Asia", flag: "🇱🇧", capital: "Beirut" },
  { country: "Oman", region: "West Asia", flag: "🇴🇲", capital: "Muscat" },
  { country: "Qatar", region: "West Asia", flag: "🇶🇦", capital: "Doha" },
  { country: "Saudi Arabia", region: "West Asia", flag: "🇸🇦", capital: "Riyadh" },
  { country: "Syria", region: "West Asia", flag: "🇸🇾", capital: "Damascus" },
  { country: "Turkey", region: "West Asia", flag: "🇹🇷", capital: "Ankara" },
  { country: "United Arab Emirates", region: "West Asia", flag: "🇦🇪", capital: "Abu Dhabi" },
  { country: "Yemen", region: "West Asia", flag: "🇾🇪", capital: "Sanaa" },

  // Central Asia
  { country: "Kazakhstan", region: "Central Asia", flag: "🇰🇿", capital: "Astana" },
  { country: "Kyrgyzstan", region: "Central Asia", flag: "🇰🇬", capital: "Bishkek" },
  { country: "Tajikistan", region: "Central Asia", flag: "🇹🇯", capital: "Dushanbe" },
  { country: "Turkmenistan", region: "Central Asia", flag: "🇹🇲", capital: "Ashgabat" },
  { country: "Uzbekistan", region: "Central Asia", flag: "🇺🇿", capital: "Tashkent" },
];

const REGIONS = ["All", "South Asia", "East Asia", "Southeast Asia", "West Asia", "Central Asia"];
const REGION_COLORS = {
  "South Asia": "#f0a500",
  "East Asia": "#e84060",
  "Southeast Asia": "#3be8a0",
  "West Asia": "#7b6cf0",
  "Central Asia": "#38c4e8"
};

let deck = [];
let current = 0;
let flipped = false;
let scores = {};
let activeRegion = "All";

function buildDeck() {
  deck = activeRegion === "All"
    ? [...ALL_COUNTRIES]
    : ALL_COUNTRIES.filter(c => c.region === activeRegion);
  shuffle(deck);
  scores = {};
  deck.forEach((_, i) => scores[i] = null);
  current = 0;
  flipped = false;
}

function shuffle(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
}

function renderCard() {
  const d = deck[current];
  document.getElementById('cardFlag').textContent = d.flag;
  document.getElementById('cardCountry').textContent = d.country;
  document.getElementById('cardFlagBack').textContent = d.flag;
  document.getElementById('cardRegion').textContent = d.region;
  document.getElementById('cardRegion').style.color = REGION_COLORS[d.region] || '#3be8a0';
  document.getElementById('cardExtra').textContent = `Capital: ${d.capital}`;

  const c = document.getElementById('card');
  c.classList.remove('flipped');
  flipped = false;

  document.getElementById('flipBtn').style.display = 'block';
  document.getElementById('answerBtns').style.display = 'none';

  const fb = document.getElementById('feedbackTag');
  fb.className = 'feedback-tag';
  fb.textContent = '';

  const s = scores[current];
  if (s === true) {
    fb.className = 'feedback-tag correct show';
    fb.textContent = '✓ Got it!';
  } else if (s === false) {
    fb.className = 'feedback-tag wrong show';
    fb.textContent = '✗ Missed';
  }

  updateStats();
}

function flipCard() {
  if (!flipped) {
    document.getElementById('card').classList.add('flipped');
    flipped = true;
    document.getElementById('flipBtn').style.display = 'none';
    document.getElementById('answerBtns').style.display = 'flex';
  }
}

function markAnswer(correct) {
  scores[current] = correct;
  const fb = document.getElementById('feedbackTag');
  fb.className = correct ? 'feedback-tag correct show' : 'feedback-tag wrong show';
  fb.textContent = correct ? '✓ Got it!' : '✗ Missed';
  updateStats();

  const answered = Object.values(scores).filter(v => v !== null).length;
  if (answered === deck.length) {
    setTimeout(showResults, 600);
  } else {
    setTimeout(() => goTo(1), 500);
  }
}

function goTo(dir) {
  const next = current + dir;
  if (next < 0 || next >= deck.length) return;
  current = next;
  renderCard();
}

function updateStats() {
  const correct = Object.values(scores).filter(v => v === true).length;
  const wrong = Object.values(scores).filter(v => v === false).length;
  const total = deck.length;

  document.getElementById('statCorrect').textContent = correct;
  document.getElementById('statWrong').textContent = wrong;
  document.getElementById('statCard').textContent = `Card ${current + 1} / ${total}`;
  document.getElementById('navLabel').textContent = `${current + 1} / ${total}`;

  const answered = correct + wrong;
  document.getElementById('progressFill').style.width = `${(answered / total) * 100}%`;

  document.getElementById('prevBtn').disabled = current === 0;
  document.getElementById('nextBtn').disabled = current === total - 1;
}

function showResults() {
  const correct = Object.values(scores).filter(v => v === true).length;
  const wrong = Object.values(scores).filter(v => v === false).length;
  const skipped = deck.length - correct - wrong;
  const pct = Math.round((correct / deck.length) * 100);

  document.getElementById('quizArea').style.display = 'none';
  document.getElementById('resultScreen').classList.add('visible');
  document.getElementById('resultScore').textContent = `${pct}%`;
  document.getElementById('resCorrect').textContent = correct;
  document.getElementById('resWrong').textContent = wrong;
  document.getElementById('resSkipped').textContent = skipped;
  document.getElementById('resTotal').textContent = deck.length;
}

function restartQuiz() {
  document.getElementById('resultScreen').classList.remove('visible');
  document.getElementById('quizArea').style.display = 'flex';
  buildDeck();
  renderCard();
}

function buildFilterBar() {
  const bar = document.getElementById('filterBar');
  bar.innerHTML = '';
  REGIONS.forEach(r => {
    const btn = document.createElement('button');
    btn.className = 'filter-btn' + (r === activeRegion ? ' active' : '');
    const count = r === 'All' ? ALL_COUNTRIES.length : ALL_COUNTRIES.filter(c => c.region === r).length;
    btn.textContent = `${r} (${count})`;
    if (r !== 'All') btn.style.setProperty('--accent', REGION_COLORS[r]);
    btn.onclick = () => {
      activeRegion = r;
      document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      buildDeck();
      renderCard();
    };
    bar.appendChild(btn);
  });
}

// Init
buildFilterBar();
buildDeck();
renderCard();
</script>
</body>
</html>
