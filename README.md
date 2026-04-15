<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Creator Search Insights</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0f;
    --surface: #13131a;
    --card: #1a1a24;
    --border: #2a2a3a;
    --accent: #fe2c55;
    --accent2: #25f4ee;
    --text: #f0f0f8;
    --muted: #6b6b88;
    --up: #25f4ee;
    --down: #fe2c55;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* NOISE TEXTURE */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.03'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
    opacity: 0.4;
  }

  /* HEADER */
  header {
    position: sticky;
    top: 0;
    z-index: 100;
    background: rgba(10,10,15,0.85);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--border);
    padding: 0 2rem;
    height: 60px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .logo {
    display: flex;
    align-items: center;
    gap: 10px;
    font-family: 'Bebas Neue', cursive;
    font-size: 1.4rem;
    letter-spacing: 1px;
  }

  .logo-icon {
    width: 32px;
    height: 32px;
    background: var(--accent);
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
  }

  .logo span { color: var(--accent2); }

  nav { display: flex; gap: 8px; }

  nav button {
    background: none;
    border: 1px solid transparent;
    color: var(--muted);
    padding: 6px 14px;
    border-radius: 20px;
    cursor: pointer;
    font-family: 'DM Sans', sans-serif;
    font-size: 0.82rem;
    transition: all 0.2s;
  }

  nav button:hover, nav button.active {
    border-color: var(--border);
    color: var(--text);
    background: var(--card);
  }

  nav button.active { border-color: var(--accent); color: var(--accent); }

  /* HERO SEARCH */
  .hero {
    padding: 3rem 2rem 2rem;
    max-width: 900px;
    margin: 0 auto;
    position: relative;
    z-index: 1;
  }

  .hero h1 {
    font-family: 'Bebas Neue', cursive;
    font-size: clamp(2.5rem, 6vw, 4rem);
    line-height: 1;
    margin-bottom: 0.3rem;
  }

  .hero h1 span { color: var(--accent); }

  .hero p {
    color: var(--muted);
    font-size: 0.9rem;
    margin-bottom: 1.5rem;
  }

  .search-bar {
    display: flex;
    gap: 10px;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 8px 8px 8px 16px;
    transition: border-color 0.2s;
  }

  .search-bar:focus-within { border-color: var(--accent); }

  .search-bar input {
    flex: 1;
    background: none;
    border: none;
    outline: none;
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-size: 1rem;
  }

  .search-bar input::placeholder { color: var(--muted); }

  .search-btn {
    background: var(--accent);
    border: none;
    color: white;
    padding: 10px 22px;
    border-radius: 8px;
    cursor: pointer;
    font-family: 'DM Sans', sans-serif;
    font-weight: 600;
    font-size: 0.9rem;
    transition: opacity 0.2s, transform 0.1s;
  }

  .search-btn:hover { opacity: 0.85; }
  .search-btn:active { transform: scale(0.97); }

  /* FILTERS */
  .filters {
    display: flex;
    gap: 8px;
    margin-top: 1rem;
    flex-wrap: wrap;
  }

  .filter-chip {
    background: var(--card);
    border: 1px solid var(--border);
    color: var(--muted);
    padding: 5px 12px;
    border-radius: 20px;
    font-size: 0.78rem;
    cursor: pointer;
    transition: all 0.15s;
    display: flex;
    align-items: center;
    gap: 5px;
  }

  .filter-chip:hover, .filter-chip.active {
    border-color: var(--accent2);
    color: var(--accent2);
  }

  /* MAIN GRID */
  .main {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 2rem 4rem;
    position: relative;
    z-index: 1;
  }

  .section-title {
    font-family: 'Bebas Neue', cursive;
    font-size: 1.3rem;
    letter-spacing: 1px;
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    gap: 10px;
    color: var(--muted);
  }

  .section-title::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  .section-title span { color: var(--text); }

  /* TRENDING TABLE */
  .trend-table {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    overflow: hidden;
    margin-bottom: 2.5rem;
  }

  .trend-table table {
    width: 100%;
    border-collapse: collapse;
  }

  .trend-table th {
    padding: 12px 16px;
    text-align: left;
    font-size: 0.72rem;
    color: var(--muted);
    font-weight: 500;
    letter-spacing: 0.5px;
    text-transform: uppercase;
    border-bottom: 1px solid var(--border);
    background: var(--surface);
  }

  .trend-table td {
    padding: 14px 16px;
    font-size: 0.88rem;
    border-bottom: 1px solid rgba(42,42,58,0.5);
  }

  .trend-table tr:last-child td { border-bottom: none; }

  .trend-table tr:hover td { background: rgba(255,255,255,0.02); }

  .rank {
    color: var(--muted);
    font-size: 0.75rem;
    font-weight: 600;
    width: 40px;
  }

  .keyword-cell {
    font-weight: 500;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .kw-tag {
    background: rgba(254,44,85,0.1);
    color: var(--accent);
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 0.7rem;
    font-weight: 600;
  }

  .kw-tag.trend { background: rgba(37,244,238,0.1); color: var(--accent2); }

  .volume-bar {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .bar-bg {
    flex: 1;
    height: 4px;
    background: var(--border);
    border-radius: 99px;
    max-width: 120px;
  }

  .bar-fill {
    height: 100%;
    border-radius: 99px;
    background: linear-gradient(90deg, var(--accent2), var(--accent));
  }

  .change {
    font-size: 0.78rem;
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 3px;
  }

  .change.up { color: var(--up); }
  .change.down { color: var(--down); }

  /* CARDS GRID */
  .cards-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 1rem;
    margin-bottom: 2.5rem;
  }

  .insight-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 1.2rem;
    cursor: pointer;
    transition: transform 0.2s, border-color 0.2s;
    position: relative;
    overflow: hidden;
  }

  .insight-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    opacity: 0;
    transition: opacity 0.2s;
  }

  .insight-card:hover { transform: translateY(-2px); border-color: rgba(254,44,85,0.3); }
  .insight-card:hover::before { opacity: 1; }

  .card-category {
    font-size: 0.68rem;
    font-weight: 600;
    letter-spacing: 0.8px;
    text-transform: uppercase;
    color: var(--accent2);
    margin-bottom: 0.5rem;
  }

  .card-title {
    font-weight: 600;
    font-size: 0.95rem;
    margin-bottom: 0.4rem;
  }

  .card-meta {
    display: flex;
    gap: 12px;
    margin-top: 0.8rem;
    flex-wrap: wrap;
  }

  .meta-item {
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  .meta-label { font-size: 0.65rem; color: var(--muted); text-transform: uppercase; letter-spacing: 0.5px; }
  .meta-value { font-size: 0.85rem; font-weight: 600; }

  .sparkline {
    margin-top: 1rem;
    height: 40px;
    display: flex;
    align-items: flex-end;
    gap: 3px;
  }

  .spark-bar {
    flex: 1;
    background: rgba(37,244,238,0.2);
    border-radius: 3px 3px 0 0;
    transition: background 0.2s;
  }

  .spark-bar.high { background: rgba(37,244,238,0.6); }
  .spark-bar.peak { background: var(--accent2); }

  /* STATS ROW */
  .stats-row {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 1rem;
    margin-bottom: 2.5rem;
  }

  .stat-box {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1.2rem;
  }

  .stat-label { font-size: 0.72rem; color: var(--muted); text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 0.4rem; }
  .stat-value { font-family: 'Bebas Neue', cursive; font-size: 2rem; line-height: 1; }
  .stat-sub { font-size: 0.72rem; color: var(--muted); margin-top: 0.2rem; }
  .stat-sub span { color: var(--up); }

  /* LOADING STATE */
  .loading {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 3rem;
    color: var(--muted);
    gap: 10px;
    font-size: 0.9rem;
  }

  .spinner {
    width: 20px;
    height: 20px;
    border: 2px solid var(--border);
    border-top-color: var(--accent);
    border-radius: 50%;
    animation: spin 0.7s linear infinite;
  }

  @keyframes spin { to { transform: rotate(360deg); } }

  /* SEARCH RESULTS */
  .results-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 1rem;
  }

  .results-count {
    font-size: 0.82rem;
    color: var(--muted);
  }

  .results-count span { color: var(--accent2); font-weight: 600; }

  /* TAG CLOUD */
  .tag-cloud {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 2.5rem;
  }

  .trend-tag {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 6px 12px;
    font-size: 0.82rem;
    cursor: pointer;
    transition: all 0.15s;
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .trend-tag:hover { border-color: var(--accent); color: var(--accent); }

  .trend-tag .vol { font-size: 0.68rem; color: var(--muted); }

  /* AI RESULT BOX */
  #ai-result {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 1.5rem;
    margin-bottom: 2rem;
    display: none;
    animation: fadeIn 0.3s ease;
  }

  @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }

  #ai-result .ai-header {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 1rem;
    font-size: 0.8rem;
    color: var(--accent2);
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }

  #ai-result .ai-body {
    font-size: 0.9rem;
    line-height: 1.7;
    color: var(--text);
  }

  #ai-result .ai-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    margin-top: 1rem;
  }

  #ai-result .ai-tag {
    background: rgba(37,244,238,0.08);
    border: 1px solid rgba(37,244,238,0.2);
    color: var(--accent2);
    padding: 3px 10px;
    border-radius: 20px;
    font-size: 0.75rem;
  }

  @media (max-width: 700px) {
    .stats-row { grid-template-columns: repeat(2, 1fr); }
    nav { display: none; }
    .hero h1 { font-size: 2.2rem; }
  }
</style>
</head>
<body>

<header>
  <div class="logo">
    <div class="logo-icon">🎵</div>
    CREATOR<span>SEARCH</span>INSIGHTS
  </div>
  <nav>
    <button class="active" onclick="setTab(this,'trending')">Tendances</button>
    <button onclick="setTab(this,'hashtags')">Hashtags</button>
    <button onclick="setTab(this,'sounds')">Sons</button>
    <button onclick="setTab(this,'niches')">Niches</button>
  </nav>
</header>

<div class="hero">
  <h1>TROUVE DES TENDANCES<br><span>VIRALES</span> TIKTOK</h1>
  <p>Recherche des mots-clés, hashtags et sujets tendances pour maximiser ta portée</p>
  <div class="search-bar">
    <input type="text" id="searchInput" placeholder="Ex: recette, fitness, voyage, mode..." 
           onkeydown="if(event.key==='Enter') doSearch()">
    <button class="search-btn" onclick="doSearch()">🔍 Analyser</button>
  </div>
  <div class="filters">
    <div class="filter-chip active" onclick="toggleChip(this)">🌍 France</div>
    <div class="filter-chip" onclick="toggleChip(this)">🌎 Monde</div>
    <div class="filter-chip active" onclick="toggleChip(this)">📅 7 jours</div>
    <div class="filter-chip" onclick="toggleChip(this)">📅 30 jours</div>
    <div class="filter-chip" onclick="toggleChip(this)">✨ Viral seulement</div>
    <div class="filter-chip" onclick="toggleChip(this)">🎯 Niche</div>
  </div>
</div>

<div class="main">

  <!-- AI RESULT -->
  <div id="ai-result">
    <div class="ai-header">⚡ Analyse IA</div>
    <div class="ai-body" id="ai-body"></div>
    <div class="ai-tags" id="ai-tags"></div>
  </div>

  <!-- STATS -->
  <div class="stats-row">
    <div class="stat-box">
      <div class="stat-label">Tendances actives</div>
      <div class="stat-value">2,847</div>
      <div class="stat-sub"><span>+12%</span> vs semaine passée</div>
    </div>
    <div class="stat-box">
      <div class="stat-label">Vues générées</div>
      <div class="stat-value">14.2B</div>
      <div class="stat-sub"><span>+8.3%</span> cette semaine</div>
    </div>
    <div class="stat-box">
      <div class="stat-label">Hashtags trending</div>
      <div class="stat-value">934</div>
      <div class="stat-sub"><span>+21%</span> en hausse</div>
    </div>
    <div class="stat-box">
      <div class="stat-label">Créateurs actifs</div>
      <div class="stat-value">528K</div>
      <div class="stat-sub"><span>+5%</span> nouveaux</div>
    </div>
  </div>

  <!-- TRENDING KEYWORDS -->
  <div class="section-title"><span>MOTS-CLÉS TENDANCES</span></div>
  <div class="trend-table">
    <table>
      <thead>
        <tr>
          <th>#</th>
          <th>Mot-clé</th>
          <th>Volume de recherche</th>
          <th>Difficulté</th>
          <th>Évolution 7j</th>
          <th>Potentiel</th>
        </tr>
      </thead>
      <tbody id="trending-body"></tbody>
    </table>
  </div>

  <!-- TRENDING TOPICS CARDS -->
  <div class="section-title"><span>SUJETS EN EXPLOSION</span></div>
  <div class="cards-grid" id="topics-grid"></div>

  <!-- TAG CLOUD -->
  <div class="section-title"><span>HASHTAGS POPULAIRES</span></div>
  <div class="tag-cloud" id="tag-cloud"></div>

</div>

<script>
const TRENDING_DATA = [
  { kw: "recette rapide", vol: 98, diff: "Facile", change: +142, cat: "Food" },
  { kw: "before after transformation", vol: 94, diff: "Moyen", change: +89, cat: "Fitness" },
  { kw: "outfit du jour", vol: 91, diff: "Facile", change: +67, cat: "Mode" },
  { kw: "astuce vie quotidienne", vol: 88, diff: "Facile", change: +203, cat: "LifeHack" },
  { kw: "routine matin", vol: 85, diff: "Moyen", change: +44, cat: "Lifestyle" },
  { kw: "gaming tips", vol: 82, diff: "Difficile", change: -12, cat: "Gaming" },
  { kw: "voyage pas cher", vol: 79, diff: "Moyen", change: +156, cat: "Travel" },
  { kw: "makeuptutorial", vol: 76, diff: "Moyen", change: +38, cat: "Beauty" },
  { kw: "finance perso", vol: 72, diff: "Difficile", change: +91, cat: "Finance" },
  { kw: "animal mignon", vol: 70, diff: "Facile", change: +22, cat: "Pets" },
];

const TOPICS = [
  { cat:"FOOD", title:"Recettes 60 secondes", views:"2.4B", growth:"+180%", comp:"Faible", spark:[3,5,4,7,6,9,8,10,9,8] },
  { cat:"LIFESTYLE", title:"Morning routine esthétique", views:"1.8B", growth:"+134%", comp:"Moyen", spark:[4,3,6,5,8,7,9,8,10,9] },
  { cat:"FASHION", title:"Thrift flip / DIY mode", views:"980M", growth:"+98%", comp:"Faible", spark:[2,4,3,6,5,8,7,9,8,10] },
  { cat:"FITNESS", title:"Transformation 30 jours", views:"3.1B", growth:"+210%", comp:"Élevé", spark:[5,6,8,7,9,8,10,9,8,10] },
  { cat:"TRAVEL", title:"Hidden gems France", views:"640M", growth:"+76%", comp:"Faible", spark:[3,4,3,5,6,7,8,9,10,9] },
  { cat:"FINANCE", title:"Gagner de l'argent en ligne", views:"1.2B", growth:"+145%", comp:"Moyen", spark:[6,7,8,9,8,10,9,8,7,10] },
];

const HASHTAGS = [
  { tag:"#pourtoi", vol:"842B" }, { tag:"#fyp", vol:"720B" }, { tag:"#viral", vol:"380B" },
  { tag:"#recette", vol:"28B" }, { tag:"#astuce", vol:"19B" }, { tag:"#lifestyle", vol:"45B" },
  { tag:"#france", vol:"22B" }, { tag:"#fashion", vol:"89B" }, { tag:"#fitness", vol:"67B" },
  { tag:"#tendance", vol:"14B" }, { tag:"#tuto", vol:"31B" }, { tag:"#humour", vol:"56B" },
  { tag:"#food", vol:"94B" }, { tag:"#travel", vol:"48B" }, { tag:"#motivation", vol:"37B" },
  { tag:"#beaute", vol:"25B" }, { tag:"#DIY", vol:"18B" }, { tag:"#gaming", vol:"73B" },
];

function renderTable() {
  const tbody = document.getElementById('trending-body');
  tbody.innerHTML = TRENDING_DATA.map((d, i) => `
    <tr>
      <td class="rank">${i+1}</td>
      <td class="keyword-cell">
        ${d.kw}
        <span class="kw-tag ${d.change > 100 ? 'trend' : ''}">${d.cat}</span>
      </td>
      <td>
        <div class="volume-bar">
          <span style="font-size:0.8rem;min-width:30px">${d.vol}K</span>
          <div class="bar-bg"><div class="bar-fill" style="width:${d.vol}%"></div></div>
        </div>
      </td>
      <td style="font-size:0.8rem;color:var(--muted)">${d.diff}</td>
      <td><span class="change ${d.change > 0 ? 'up' : 'down'}">${d.change > 0 ? '▲' : '▼'} ${Math.abs(d.change)}%</span></td>
      <td><span style="font-size:0.8rem;color:${d.change > 100 ? 'var(--accent2)' : d.change > 50 ? 'var(--text)' : 'var(--muted)'}">${d.change > 100 ? '🔥 Élevé' : d.change > 30 ? '📈 Bon' : '➡ Moyen'}</span></td>
    </tr>
  `).join('');
}

function renderCards() {
  const grid = document.getElementById('topics-grid');
  grid.innerHTML = TOPICS.map(t => {
    const maxV = Math.max(...t.spark);
    const bars = t.spark.map((v, i) => {
      const h = Math.round((v/maxV)*100);
      const cls = v === maxV ? 'peak' : v > maxV*0.7 ? 'high' : '';
      return `<div class="spark-bar ${cls}" style="height:${h}%"></div>`;
    }).join('');
    return `
    <div class="insight-card">
      <div class="card-category">${t.cat}</div>
      <div class="card-title">${t.title}</div>
      <div class="card-meta">
        <div class="meta-item"><div class="meta-label">Vues totales</div><div class="meta-value">${t.views}</div></div>
        <div class="meta-item"><div class="meta-label">Croissance</div><div class="meta-value" style="color:var(--up)">${t.growth}</div></div>
        <div class="meta-item"><div class="meta-label">Concurrence</div><div class="meta-value">${t.comp}</div></div>
      </div>
      <div class="sparkline">${bars}</div>
    </div>`;
  }).join('');
}

function renderTags() {
  const cloud = document.getElementById('tag-cloud');
  cloud.innerHTML = HASHTAGS.map(h => `
    <div class="trend-tag" onclick="fillSearch('${h.tag}')">
      ${h.tag} <span class="vol">${h.vol}</span>
    </div>
  `).join('');
}

function fillSearch(val) {
  document.getElementById('searchInput').value = val.replace('#','');
  doSearch();
}

function toggleChip(el) {
  el.classList.toggle('active');
}

function setTab(btn, tab) {
  document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
}

async function doSearch() {
  const q = document.getElementById('searchInput').value.trim();
  if (!q) return;

  const box = document.getElementById('ai-result');
  const body = document.getElementById('ai-body');
  const tags = document.getElementById('ai-tags');

  box.style.display = 'block';
  body.innerHTML = '<div class="loading"><div class="spinner"></div> Analyse IA en cours...</div>';
  tags.innerHTML = '';

  try {
    const resp = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        model: "claude-sonnet-4-20250514",
        max_tokens: 1000,
        system: `Tu es un expert en stratégie de contenu TikTok. 
Réponds UNIQUEMENT en JSON avec ce format exact:
{
  "analyse": "2-3 phrases sur le potentiel viral de ce sujet sur TikTok",
  "idees": ["idée de contenu 1", "idée de contenu 2", "idée de contenu 3"],
  "hashtags": ["#hashtag1", "#hashtag2", "#hashtag3", "#hashtag4", "#hashtag5"],
  "moment_ideal": "meilleure heure/jour pour poster",
  "score_viral": 85
}
Aucun texte en dehors du JSON.`,
        messages: [{ role: "user", content: `Analyse le sujet TikTok: "${q}"` }]
      })
    });

    const data = await resp.json();
    const text = data.content.map(i => i.text || '').join('');
    const clean = text.replace(/```json|```/g, '').trim();
    const parsed = JSON.parse(clean);

    body.innerHTML = `
      <div style="display:flex;align-items:center;gap:10px;margin-bottom:1rem">
        <span style="font-size:1.5rem">🎯</span>
        <div>
          <div style="font-size:0.72rem;color:var(--muted);text-transform:uppercase;letter-spacing:.5px">Score viral estimé</div>
          <div style="font-size:1.4rem;font-weight:700;color:${parsed.score_viral > 70 ? 'var(--up)' : 'var(--accent)'}">${parsed.score_viral}/100</div>
        </div>
      </div>
      <p style="margin-bottom:1rem">${parsed.analyse}</p>
      <div style="font-size:0.8rem;color:var(--muted);margin-bottom:.5rem;font-weight:600;text-transform:uppercase;letter-spacing:.5px">💡 Idées de contenu</div>
      <ul style="padding-left:1.2rem;display:flex;flex-direction:column;gap:.4rem">
        ${parsed.idees.map(i => `<li style="font-size:.88rem">${i}</li>`).join('')}
      </ul>
      <div style="margin-top:1rem;font-size:0.8rem;color:var(--muted)">⏰ Meilleur moment : <span style="color:var(--text)">${parsed.moment_ideal}</span></div>
    `;

    tags.innerHTML = parsed.hashtags.map(h => `<span class="ai-tag">${h}</span>`).join('');

  } catch(e) {
    body.innerHTML = `<span style="color:var(--muted)">Impossible d'analyser ce sujet pour le moment.</span>`;
  }
}

// INIT
renderTable();
renderCards();
renderTags();
</script>
</body>
</html>
