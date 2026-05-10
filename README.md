<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DiFindNemo 🐟 – Diagnosis Challenge</title>
<style>
/* ── Reset & Base ── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background: #f0f2f5;
  color: #1a1a2e;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* ── Nav bar ── */
nav {
background: #fff;
border-bottom: 1px solid #e0e0e0;
display: flex;
align-items: center;
justify-content: center;
gap: 28px;
padding: 0 20px;
height: 52px;
position: sticky;
top: 0;
z-index: 50;
box-shadow: 0 1px 4px rgba(0,0,0,.06);
}
.nav-link {
font-size: 0.82rem;
font-weight: 500;
color: #555;
text-decoration: none;
letter-spacing: 0.2px;
padding: 4px 2px;
border-bottom: 2px solid transparent;
transition: color .15s, border-color .15s;
cursor: pointer;
}
.nav-link:hover { color: #1a1a2e; border-bottom-color: #1a1a2e; }
.nav-link.active { color: #1a1a2e; border-bottom-color: #1a1a2e; font-weight: 600; }

/* ── Page wrapper ── */
.page {
flex: 1;
display: flex;
flex-direction: column;
align-items: center;
padding: 28px 16px 60px;
}

/* ── Main card ── */
.card {
background: #fff;
border-radius: 14px;
box-shadow: 0 2px 12px rgba(0,0,0,.08);
width: 100%;
max-width: 660px;
overflow: hidden;
}

/* ── Card header ── */
.card-header {
text-align: center;
padding: 26px 28px 18px;
border-bottom: 1px solid #eeeeee;
}
.header-eyebrow {
font-size: 0.72rem;
font-weight: 600;
letter-spacing: 1.5px;
text-transform: uppercase;
color: #888;
margin-bottom: 8px;
}
.header-title {
display: flex;
align-items: center;
justify-content: center;
gap: 10px;
margin-bottom: 4px;
}
.header-title h1 {
font-size: 1.7rem;
font-weight: 800;
color: #1a1a2e;
letter-spacing: -0.5px;
}
.turtle-icon { font-size: 1.6rem; }
.header-sub {
font-size: 0.8rem;
color: #999;
margin-bottom: 12px;
}
.case-nav {
display: flex;
align-items: center;
justify-content: space-between;
margin-top: 10px;
}
.case-nav-btn {
background: none;
border: 1px solid #ddd;
border-radius: 8px;
padding: 5px 12px;
font-size: 0.8rem;
color: #666;
cursor: pointer;
transition: all .15s;
}
.case-nav-btn:hover { background: #f5f5f5; color: #1a1a2e; }
.case-nav-btn:disabled { opacity: .3; cursor: default; }
.case-label {
font-size: 0.82rem;
font-weight: 600;
color: #1a1a2e;
}
.ep-tag {
display: inline-block;
background: #eef2ff;
color: #4361ee;
font-size: 0.68rem;
font-weight: 600;
letter-spacing: 0.5px;
border-radius: 20px;
padding: 3px 10px;
margin-top: 8px;
}

/* ── Attempts tracker (6 dots) ── */
.attempts-bar {
display: flex;
justify-content: center;
gap: 8px;
padding: 14px 28px 0;
}
.attempt-pip {
width: 11px; height: 11px;
border-radius: 50%;
background: #e0e0e0;
transition: background .3s;
}
.attempt-pip.used    { background: #e84455; }
.attempt-pip.won     { background: #3ecf6a; }
.attempt-pip.current { background: #4361ee; animation: blink 1.2s ease-in-out infinite; }
@keyframes blink { 0%,100%{opacity:1} 50%{opacity:.35} }

/* ── Divider ── */
.divider {
height: 1px;
background: #eee;
margin: 18px 0 0;
}

/* ── Scenario section ── */
.scenario-section { padding: 22px 28px 0; }
.section-label {
font-size: 0.68rem;
font-weight: 700;
letter-spacing: 1.8px;
text-transform: uppercase;
color: #aaa;
margin-bottom: 14px;
}
.clue-block {
margin-bottom: 10px;
animation: fadeSlide .35s ease;
}
@keyframes fadeSlide { from{opacity:0;transform:translateY(-8px)} to{opacity:1;transform:translateY(0)} }
.clue-row {
display: flex;
gap: 12px;
align-items: flex-start;
}
.clue-badge {
flex-shrink: 0;
width: 22px; height: 22px;
border-radius: 6px;
background: #eef2ff;
color: #4361ee;
font-size: 0.68rem;
font-weight: 700;
display: flex;
align-items: center;
justify-content: center;
margin-top: 1px;
}
.clue-text {
font-size: 0.95rem;
line-height: 1.65;
color: #2c2c3e;
}
.clue-separator {
height: 1px;
background: #f0f0f0;
margin: 10px 0;
}

/* ── Guess history ── */
.guess-history { padding: 16px 28px 0; display: flex; flex-direction: column; gap: 6px; }
.guess-row {
display: flex;
align-items: center;
gap: 10px;
padding: 8px 12px;
border-radius: 8px;
font-size: 0.86rem;
animation: fadeSlide .25s ease;
}
.guess-row.wrong   { background: #fff0f2; border: 1px solid #f9cdd2; color: #c0392b; }
.guess-row.correct { background: #f0fdf4; border: 1px solid #bbf7d0; color: #166534; }
.guess-row.skipped { background: #fafafa; border: 1px solid #e5e5e5; color: #888; font-style: italic; }
.guess-icon { font-size: 0.9rem; flex-shrink: 0; }
.guess-label { font-size: 0.7rem; color: #aaa; margin-right: 2px; }

/* ── Input section ── */
.input-section { padding: 18px 28px 0; }
.input-wrap {
display: flex;
flex-direction: column;
gap: 10px;
position: relative;
}
.input-field-row { display: flex; gap: 8px; }
#diagInput {
flex: 1;
border: 1.5px solid #ddd;
border-radius: 9px;
padding: 11px 14px;
font-size: 0.92rem;
color: #1a1a2e;
outline: none;
transition: border-color .2s;
font-family: inherit;
background: #fafafa;
}
#diagInput:focus { border-color: #4361ee; background: #fff; }
#diagInput::placeholder { color: #bbb; }
#submitBtn {
background: #1a1a2e;
color: #fff;
border: none;
border-radius: 9px;
padding: 11px 22px;
font-size: 0.88rem;
font-weight: 600;
cursor: pointer;
transition: background .15s, transform .1s;
font-family: inherit;
white-space: nowrap;
}
#submitBtn:hover:not(:disabled) { background: #2d2d4a; transform: translateY(-1px); }
#submitBtn:disabled { opacity: .4; cursor: not-allowed; transform: none; }
#skipBtn {
background: #fff;
border: 1.5px solid #ddd;
border-radius: 9px;
padding: 9px 14px;
font-size: 0.8rem;
color: #888;
cursor: pointer;
transition: all .15s;
font-family: inherit;
white-space: nowrap;
}
#skipBtn:hover:not(:disabled) { border-color: #f5a623; color: #f5a623; }
#skipBtn:disabled { opacity: .35; cursor: not-allowed; }

/* ── Autocomplete ── */
#suggestions {
position: absolute; top: calc(100% + 4px); left: 0;
right: 0;
background: #fff;
border: 1px solid #ddd;
border-radius: 10px;
box-shadow: 0 6px 20px rgba(0,0,0,.1);
z-index: 100;
display: none;
overflow: hidden;
max-height: 200px;
overflow-y: auto;
}
.sug-item {
padding: 9px 14px;
font-size: 0.86rem;
cursor: pointer;
color: #2c2c3e;
transition: background .1s;
display: flex; align-items: center; gap: 8px;
}
.sug-item:hover, .sug-item.sel { background: #f0f4ff; }
.sug-item mark { background: none; color: #4361ee; font-weight: 700; }

/* ── Action buttons row ── */
.action-row {
display: flex;
gap: 8px;
flex-wrap: wrap;
padding: 12px 28px 0;
}
.anki-btn {
background: #fff;
border: 1.5px solid #ddd;
border-radius: 8px;
padding: 7px 13px;
font-size: 0.78rem;
color: #555;
cursor: pointer;
transition: all .15s;
font-family: inherit;
display: flex; align-items: center; gap: 5px;
}
.anki-btn:hover { border-color: #4361ee; color: #4361ee; }

/* ── Result / Diagnosis Summary ── */
#summarySection {
margin: 16px 28px 0;
border: 1.5px solid #e0e0e0;
border-radius: 10px;
overflow: hidden;
display: none;
}
.summary-toggle {
width: 100%;
background: #f9f9f9;
border: none;
padding: 12px 16px;
text-align: left;
font-size: 0.88rem;
font-weight: 600;
color: #1a1a2e;
cursor: pointer;
display: flex; align-items: center; justify-content: space-between;
font-family: inherit;
transition: background .15s;
}
.summary-toggle:hover { background: #f0f0f0; }
.summary-body {
display: none;
padding: 14px 16px;
background: #fff;
border-top: 1px solid #eee;
}
.summary-body.open { display: block; }
.dx-reveal {
font-size: 0.78rem;
font-weight: 600;
letter-spacing: 1.2px;
text-transform: uppercase;
color: #aaa;
margin-bottom: 6px;
}
.dx-name { font-size: 1.25rem; font-weight: 800; color: #1a1a2e; margin-bottom: 8px; }
.dx-ep   { font-size: 0.76rem; color: #4361ee; font-weight: 600; margin-bottom: 10px; }
.dx-facts {
font-size: 0.84rem;
line-height: 1.65;
color: #555;
}
.dx-facts strong { color: #1a1a2e; }

/* ── Win/Lose banner ── */
#resultBanner {
display: none;
margin: 18px 28px 0;
border-radius: 10px;
padding: 14px 18px;
animation: fadeSlide .4s ease;
}
#resultBanner.win  { background: #f0fdf4; border: 1.5px solid #bbf7d0; }
#resultBanner.lose { background: #fff5f5; border: 1.5px solid #fed7d7; }
.banner-row { display: flex; align-items: center; gap: 10px; }
.banner-emoji { font-size: 1.5rem; }
.banner-title { font-size: 1rem; font-weight: 700; color: #1a1a2e; }
.banner-sub   { font-size: 0.8rem; color: #888; margin-top: 2px; }
.banner-actions { display: flex; gap: 8px; margin-top: 12px; flex-wrap: wrap; }
.banner-btn {
border: none; border-radius: 8px;
padding: 8px 16px; font-size: 0.82rem; font-weight: 600;
cursor: pointer; transition: all .15s; font-family: inherit;
}
.btn-share { background: #1a1a2e; color: #fff; }
.btn-share:hover { background: #2d2d4a; }
.btn-next  { background: #fff; border: 1.5px solid #ddd; color: #1a1a2e; }
.btn-next:hover { border-color: #1a1a2e; }
.btn-archive { background: #fff; border: 1.5px solid #ddd; color: #1a1a2e; }
.btn-archive:hover { border-color: #4361ee; color: #4361ee; }
.btn-restart { background: #fff; border: 1.5px solid #e67e22; color: #e67e22; }
.btn-restart:hover { background: #e67e22; color: #fff; }

/* ── Restart overlay ── */
#restartOverlay {
display: none; position: fixed; inset: 0;
background: rgba(0,0,0,.45); z-index: 300;
align-items: center; justify-content: center; padding: 20px;
}
#restartOverlay.open { display: flex; }
.restart-box {
background: #fff; border-radius: 16px;
width: 100%; max-width: 380px;
padding: 28px 24px 24px;
box-shadow: 0 8px 40px rgba(0,0,0,.22);
text-align: center;
}
.restart-box h3 { font-size: 1.05rem; font-weight: 700; margin: 0 0 6px; }
.restart-box p  { font-size: 0.84rem; color: #666; margin: 0 0 22px; line-height: 1.5; }
.restart-options { display: flex; flex-direction: column; gap: 10px; }
.restart-opt {
padding: 13px 16px; border-radius: 10px; border: none;
font-size: 0.88rem; font-weight: 600; cursor: pointer;
transition: all .15s; text-align: left; display: flex; gap: 10px; align-items: center;
}
.restart-opt-current {
background: #fff4ea; color: #c0621a; border: 1.5px solid #e67e22;
}
.restart-opt-current:hover { background: #e67e22; color: #fff; }
.restart-opt-all {
background: #fef0f0; color: #c0392b; border: 1.5px solid #e74c3c;
}
.restart-opt-all:hover { background: #e74c3c; color: #fff; }
.restart-opt-cancel {
background: #f5f5f5; color: #888; border: 1.5px solid #ddd;
justify-content: center;
}
.restart-opt-cancel:hover { background: #eee; color: #555; }
.restart-opt-icon { font-size: 1.1rem; flex-shrink: 0; }
.restart-opt-label { flex: 1; }
.restart-opt-label strong { display: block; margin-bottom: 2px; }
.restart-opt-label span { font-size: 0.77rem; font-weight: 400; opacity: .8; }

/* ── Footer ── */
.card-footer {
padding: 20px 28px;
text-align: center;
font-size: 0.73rem;
color: #bbb;
margin-top: 18px;
border-top: 1px solid #f0f0f0;
line-height: 1.6;
}
.creator-badge {
display: inline-flex; align-items: center; gap: 7px;
margin-bottom: 12px;
background: linear-gradient(135deg, #f09433, #e6683c, #dc2743, #cc2366, #bc1888);
color: #fff; border-radius: 20px;
padding: 7px 15px 7px 12px;
font-size: 0.78rem; font-weight: 600;
text-decoration: none; letter-spacing: 0.01em;
box-shadow: 0 2px 10px rgba(188,24,136,.25);
transition: transform .15s, box-shadow .15s;
}
.creator-badge:hover {
transform: translateY(-2px);
box-shadow: 0 4px 16px rgba(188,24,136,.35);
}
.creator-badge svg { flex-shrink: 0; }

/* ── Archive overlay ── */
#archiveOverlay {
display: none; position: fixed; inset: 0;
background: rgba(0,0,0,.45); z-index: 200;
align-items: center; justify-content: center; padding: 20px;
}
#archiveOverlay.open { display: flex; }
.overlay-box {
background: #fff; border-radius: 14px;
width: 100%; max-width: 620px; max-height: 82vh;
display: flex; flex-direction: column; overflow: hidden;
box-shadow: 0 8px 40px rgba(0,0,0,.2);
}
.overlay-head {
padding: 16px 20px; display: flex; justify-content: space-between; align-items: center;
border-bottom: 1px solid #eee;
}
.overlay-head h2 { font-size: 1rem; font-weight: 700; }
.overlay-close { background: none; border: none; font-size: 1.3rem; cursor: pointer; color: #888; line-height: 1; }
.archive-list { overflow-y: auto; padding: 12px; display: flex; flex-direction: column; gap: 8px; }
.arc-item {
display: flex; align-items: center; gap: 12px;
background: #fafafa; border: 1px solid #eee; border-radius: 10px;
padding: 11px 14px; cursor: pointer; transition: all .15s;
}
.arc-item:hover { border-color: #4361ee; background: #f0f4ff; }
.arc-item.done { border-left: 3px solid #3ecf6a; }
.arc-num-badge {
flex-shrink: 0; width: 34px; height: 34px; border-radius: 8px;
background: #eef2ff; color: #4361ee; font-weight: 700; font-size: 0.75rem;
display: flex; align-items: center; justify-content: center;
}
.arc-info { flex: 1; min-width: 0; }
.arc-ep    { font-size: 0.72rem; color: #4361ee; font-weight: 600; margin-bottom: 2px; }
.arc-clue  { font-size: 0.82rem; color: #555; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.arc-status { flex-shrink: 0; font-size: 0.72rem; font-weight: 600; }
.arc-status.done { color: #3ecf6a; }
.arc-status.new  { color: #ccc; }

/* ── Stats overlay ── */
#statsOverlay {
display: none; position: fixed; inset: 0;
background: rgba(0,0,0,.45); z-index: 200;
align-items: center; justify-content: center; padding: 20px;
}
#statsOverlay.open { display: flex; }
.stats-nums {
display: grid; grid-template-columns: repeat(4,1fr);
gap: 10px; padding: 20px 20px 14px;
}
.stat-cell { text-align: center; }
.stat-n { font-size: 1.9rem; font-weight: 800; color: #1a1a2e; line-height: 1; }
.stat-l { font-size: 0.7rem; color: #aaa; margin-top: 4px; }
.dist-title { padding: 0 20px 8px; font-size: 0.68rem; letter-spacing: 1.5px; text-transform: uppercase; color: #bbb; font-weight: 600; }
.dist-row { display: flex; align-items: center; gap: 8px; padding: 3px 20px; }
.dist-n { width: 14px; text-align: right; font-size: 0.8rem; color: #aaa; }
.dist-track { flex: 1; height: 22px; background: #f0f0f0; border-radius: 4px; overflow: hidden; }
.dist-fill  { height: 100%; background: #1a1a2e; border-radius: 4px; min-width: 22px; display: flex; align-items: center; justify-content: flex-end; padding-right: 7px; font-size: 0.75rem; color: #fff; font-weight: 600; transition: width .5s ease; }
.dist-fill.highlight { background: #3ecf6a; }

/* ── Toast ── */
#toast {
position: fixed; bottom: 28px; left: 50%; transform: translateX(-50%);
background: #1a1a2e; color: #fff;
border-radius: 9px; padding: 9px 18px; font-size: 0.85rem;
opacity: 0; pointer-events: none; transition: opacity .25s; z-index: 400;
white-space: nowrap;
}
#toast.show { opacity: 1; }

/* ── Cookie banner ── */
#cookieBanner {
position: fixed; bottom: 0; left: 0; right: 0;
background: #fff; border-top: 1px solid #eee;
padding: 14px 20px; display: none; align-items: center;
justify-content: center; gap: 14px; flex-wrap: wrap;
font-size: 0.8rem; color: #555; z-index: 300;
box-shadow: 0 -2px 10px rgba(0,0,0,.06);
}
#cookieBanner span { flex: 1; min-width: 220px; }
.cookie-btn {
border: none; border-radius: 7px; padding: 7px 16px;
font-size: 0.8rem; font-weight: 600; cursor: pointer; font-family: inherit;
}
.cookie-accept { background: #1a1a2e; color: #fff; }
.cookie-decline { background: #f0f0f0; color: #555; }

@media (max-width: 520px) {
.scenario-section, .guess-history, .input-section,
.action-row, #summarySection, #resultBanner, .card-footer { padding-left: 16px; padding-right: 16px; }
.attempts-bar { padding-left: 16px; padding-right: 16px; }
#suggestions { right: 0; }
.stats-nums { grid-template-columns: repeat(2,1fr); }
}
@keyframes shake {
0%,100%{transform:translateX(0)} 20%,60%{transform:translateX(-5px)} 40%,80%{transform:translateX(5px)}
}
</style>

</head>
<body>

<!-- ══════════ NAV ══════════ -->

<nav>
  <a class="nav-link active" onclick="showPage('game')">Play DiFindNemo</a>
  <a class="nav-link" onclick="openArchive()">Archives</a>
  <a class="nav-link" onclick="openStats()">Statistics</a>
  <a class="nav-link" onclick="openRestart()">🔄 Restart</a>
</nav>

<!-- ══════════ PAGE ══════════ -->

<div class="page">
<div class="card">

  <!-- Card header -->

  <div class="card-header">
    <div class="header-eyebrow" id="caseDate">Divine Intervention Podcast Notes</div>
    <div class="header-title">
      <span class="turtle-icon">🩺</span>
      <h1>DiFindNemo 🐟</h1>
    </div>
    <div class="header-sub">Case <span id="caseNum">1</span> of <span id="totalNum">82</span></div>
    <div class="case-nav">
      <button class="case-nav-btn" id="prevBtn" onclick="prevCase()">← Prev</button>
      <span class="case-label" id="caseLabelCenter">What's the Diagnosis?</span>
      <button class="case-nav-btn" id="nextBtn" onclick="nextCase()">Next →</button>
    </div>
    <div><span class="ep-tag" id="epTag">Topic</span></div>
  </div>

  <!-- Attempt pips -->

  <div class="attempts-bar" id="attemptPips"></div>
  <div class="divider"></div>

  <!-- Clinical scenario / clues -->

  <div class="scenario-section">
    <div class="section-label">Clinical Scenario</div>
    <div id="clueList"></div>
  </div>

  <!-- Guess history -->

  <div class="guess-history" id="guessHistory"></div>

  <!-- Input -->

  <div class="input-section" id="inputSection">
    <div class="input-wrap">
      <div style="font-size:.78rem;color:#aaa;margin-bottom:4px;">Enter your diagnosis below and press Submit</div>
      <div class="input-field-row">
        <input id="diagInput" type="text" autocomplete="off" spellcheck="false"
               placeholder="e.g. Achalasia, DiGeorge Syndrome…">
        <button id="skipBtn" onclick="skipAttempt()" title="Skip — reveal next clue">Skip ↓</button>
        <button id="submitBtn" onclick="submitGuess()">Submit</button>
      </div>
      <div id="suggestions"></div>
    </div>
  </div>

  <!-- Action buttons (Anki tags) -->

  <div class="action-row" id="actionRow">
    <button class="anki-btn" onclick="copyAnki(1)">📋 Copy Anki Tag (Step 1)</button>
    <button class="anki-btn" onclick="copyAnki(2)">📋 Copy Anki Tag (Step 2)</button>
  </div>

  <!-- Result banner -->

  <div id="resultBanner">
    <div class="banner-row">
      <span class="banner-emoji" id="bannerEmoji"></span>
      <div>
        <div class="banner-title" id="bannerTitle"></div>
        <div class="banner-sub" id="bannerSub"></div>
      </div>
    </div>
    <div class="banner-actions">
      <button class="banner-btn btn-share" onclick="shareResult()">📤 Share</button>
      <button class="banner-btn btn-next" onclick="nextCase()">Next Case →</button>
      <button class="banner-btn btn-restart" onclick="openRestart()">🔄 Restart</button>
      <button class="banner-btn btn-archive" onclick="openArchive()">Archives</button>
    </div>
  </div>

  <!-- Diagnosis Summary (expandable) -->

  <div id="summarySection">
    <button class="summary-toggle" onclick="toggleSummary()">
      <span>+ Diagnosis Summary</span>
      <span id="summaryChevron">▾</span>
    </button>
    <div class="summary-body" id="summaryBody">
      <div class="dx-reveal">Correct Diagnosis</div>
      <div class="dx-name" id="dxName"></div>
      <div class="dx-ep"   id="dxEp"></div>
      <div class="dx-facts" id="dxFacts"></div>
    </div>
  </div>

  <!-- Footer -->

  <div class="card-footer">
    <div>
      <a class="creator-badge" href="https://instagram.com/nemothereal" target="_blank" rel="noopener">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect x="2" y="2" width="20" height="20" rx="5" stroke="white" stroke-width="2"/>
          <circle cx="12" cy="12" r="4.5" stroke="white" stroke-width="2"/>
          <circle cx="17.5" cy="6.5" r="1" fill="white"/>
        </svg>
        Made by @nemothereal
      </a>
    </div>
    <em>*DiFindNemo 🐟 is for educational purposes only and does not constitute medical advice.<br>
    Based on Divine Intervention Podcast notes.</em>
  </div>

</div><!-- /card -->
</div><!-- /page -->

<!-- ══════════ ARCHIVE ══════════ -->

<div id="archiveOverlay">
  <div class="overlay-box">
    <div class="overlay-head">
      <h2>🗂 DiFindNemo Archives</h2>
      <button class="overlay-close" onclick="closeArchive()">×</button>
    </div>
    <div class="archive-list" id="archiveList"></div>
  </div>
</div>

<!-- ══════════ STATS ══════════ -->

<div id="statsOverlay">
  <div class="overlay-box" style="max-width:440px">
    <div class="overlay-head">
      <h2>📊 Statistics</h2>
      <button class="overlay-close" onclick="closeStats()">×</button>
    </div>
    <div class="stats-nums">
      <div class="stat-cell"><div class="stat-n" id="sPlayed">0</div><div class="stat-l">Played</div></div>
      <div class="stat-cell"><div class="stat-n" id="sWon">0</div><div class="stat-l">Won</div></div>
      <div class="stat-cell"><div class="stat-n" id="sWinPct">0%</div><div class="stat-l">Win %</div></div>
      <div class="stat-cell"><div class="stat-n" id="sStreak">0</div><div class="stat-l">Streak</div></div>
    </div>
    <div class="dist-title">Guess Distribution</div>
    <div id="distRows"></div>
    <div style="height:18px"></div>
  </div>
</div>

<!-- ══════════ RESTART MODAL ══════════ -->

<div id="restartOverlay">
  <div class="restart-box">
    <h3>🔄 Restart</h3>
    <p>What would you like to reset?</p>
    <div class="restart-options">
      <button class="restart-opt restart-opt-current" onclick="restartCurrentCase()">
        <span class="restart-opt-icon">📋</span>
        <span class="restart-opt-label">
          <strong>Restart This Case</strong>
          <span>Reset the current question back to clue #1</span>
        </span>
      </button>
      <button class="restart-opt restart-opt-all" onclick="restartAllCases()">
        <span class="restart-opt-icon">🗑️</span>
        <span class="restart-opt-label">
          <strong>Restart All Cases</strong>
          <span>Clear every case — start the entire game from scratch</span>
        </span>
      </button>
      <button class="restart-opt restart-opt-cancel" onclick="closeRestart()">Cancel</button>
    </div>
  </div>
</div>

<!-- ══════════ COOKIE BANNER ══════════ -->

<div id="cookieBanner">
  <span>🍪 DiFindNemo uses localStorage to save your game progress and stats. No data is sent anywhere.</span>
  <button class="cookie-btn cookie-accept" onclick="acceptCookies()">Accept &amp; Play</button>
  <button class="cookie-btn cookie-decline" onclick="dismissCookies()">Dismiss</button>
</div>

<div id="toast"></div>

<!-- ══════════ SCRIPT ══════════ -->

<script>
/* ───────────── CASES ───────────── */
const CASES = [
  {
    id:1, diagnosis:"Paroxysmal Nocturnal Hemoglobinuria",
    aliases:["pnh","paroxysmal nocturnal hemoglobinuria","paroxysmal nocturnal haemoglobinuria"],
    episode:"Ep. 136 – Immunology", topic:"Immunology",
    ankiStep1:"PNH::PIGA_mutation::complement_hemolysis",
    ankiStep2:"PNH::eculizumab::Neisseria_vaccine",
    clues:[
      "A middle-aged patient presents complaining of reddish-brown (cola-colored) urine that is most noticeable every morning upon waking.",
      "The patient also reports unexplained thrombotic events — including clots in the hepatic and portal veins — despite no obvious hypercoagulable risk factors. He has a background of aplastic anemia.",
      "CBC reveals low hemoglobin (7.1 g/dL) with elevated LDH, low haptoglobin, and elevated indirect bilirubin, consistent with intravascular hemolysis. Direct Coombs test is negative.",
      "Flow cytometry shows markedly decreased CD55 (DAF) and CD59 (Protectin) expression on red blood cells and granulocytes.",
      "The underlying mutation is in the PIGA gene, disrupting GPI anchor synthesis. Without GPI anchors, CD55 and CD59 cannot be displayed on RBC membranes — leaving them vulnerable to complement-mediated lysis.",
      "Treatment is eculizumab (anti-C5 monoclonal antibody), preventing MAC formation. Before starting, patients MUST be vaccinated against Neisseria and other encapsulated organisms."
    ],
    keyFacts:"PIGA mutation → no GPI anchors → no CD55/CD59 → complement attacks RBCs → hemolysis + thrombosis. Treat with eculizumab (anti-C5). Vaccinate against Neisseria first."
  },
  {
    id:2, diagnosis:"Immune Thrombocytopenic Purpura",
    aliases:["itp","immune thrombocytopenic purpura","immune thrombocytopenia","idiopathic thrombocytopenic purpura"],
    episode:"Ep. 136 – Immunology", topic:"Immunology",
    ankiStep1:"ITP::GPIIb_IIIa_antibodies::thrombocytopenia",
    ankiStep2:"ITP::romiplostim::eltrombopag::splenectomy",
    clues:[
      "A 34-year-old woman with a history of SLE presents with nosebleeds, easy bruising, and small red dots (petechiae) on her lower legs for the past 2 weeks.",
      "She reports prolonged bleeding from a minor skin cut. She has no fever, splenomegaly, or lymphadenopathy.",
      "CBC: WBC 7.2, Hgb 12.8, Platelets 8,000 (severely low). All other cell lines are normal. PT/PTT are normal.",
      "Bone marrow biopsy would show increased megakaryocytes — platelet production is normal, but peripheral destruction is accelerated.",
      "Autoantibodies against GPIIb/IIIa on platelet surfaces cause opsonization and destruction by splenic macrophages.",
      "Treatment: thrombopoietin receptor agonists (romiplostim, eltrombopag), IVIG, steroids. Splenectomy is definitive. Common in SLE patients."
    ],
    keyFacts:"Anti-GPIIb/IIIa antibodies → splenic platelet destruction → isolated thrombocytopenia. Common in SLE. Treat: TPO agonists (romiplostim/eltrombopag), IVIG, steroids, or splenectomy."
  },
  {
    id:3, diagnosis:"Bruton's Agammaglobulinemia",
    aliases:["brutons agammaglobulinemia","bruton's agammaglobulinemia","x-linked agammaglobulinemia","xla","burton's agammaglobulinemia","x linked agammaglobulinemia"],
    episode:"Ep. 173 – Immunodeficiency", topic:"Immunodeficiency",
    ankiStep1:"Brutons::BTK_mutation::X-linked::no_B_cells",
    ankiStep2:"Brutons::IVIG::absent_tonsils::6mo_protected",
    clues:[
      "A 3-year-old boy presents with his fifth episode of pneumonia this year, caused by Streptococcus pneumoniae. He has also had recurrent Giardia GI infections and multiple ear infections since age 6 months.",
      "Physical exam reveals absent tonsillar tissue and a very small spleen for his age. There is no lymphadenopathy.",
      "Serum immunoglobulins are nearly undetectable across all classes (IgG, IgA, IgM). Flow cytometry shows markedly decreased B cells in peripheral blood; T cell count is normal.",
      "The mutation is in Bruton's tyrosine kinase (BTK) on the X chromosome, preventing B cell maturation beyond the pro-B cell stage.",
      "Only boys are affected (X-linked recessive). Boys are protected for the first 6 months by maternal passive IgG — infections begin after maternal antibodies wane.",
      "Treatment: monthly IVIG to provide passive immunity. Live vaccines are contraindicated. Tiny/absent tonsils and spleen are hallmarks — no B cells means no lymphoid organ development."
    ],
    keyFacts:"X-linked BTK mutation → no B cell maturation → absent immunoglobulins → recurrent pyogenic infections. Absent tonsils/spleen. Boys only. Protected first 6 months by maternal IgG. Treat with IVIG."
  },
  {
    id:4, diagnosis:"Common Variable Immunodeficiency",
    aliases:["cvid","common variable immunodeficiency","common variable immune deficiency"],
    episode:"Ep. 173 – Immunodeficiency", topic:"Immunodeficiency",
    ankiStep1:"CVID::low_all_Ig::normal_B_cells::lymphoma_risk",
    ankiStep2:"CVID::IVIG::autoimmune::any_sex_adolescent",
    clues:[
      "A 24-year-old woman presents with her fourth serious respiratory infection in 18 months. She is female with no family history of immune problems.",
      "She received the pneumococcal vaccine 4 months ago but developed pneumococcal pneumonia last month. She also has vitiligo, alopecia, and has had autoimmune hemolytic anemia twice.",
      "Exam shows cervical lymphadenopathy — she is at significantly elevated risk for lymphoma and a biopsy is sent.",
      "Serum immunoglobulins are markedly low across all classes (IgG, IgA, IgM), but flow cytometry shows normal numbers of mature B cells in peripheral blood.",
      "Unlike Bruton's agammaglobulinemia, this condition affects any sex, presents in adolescence or young adulthood, and B cells ARE present but dysfunctional (fail to differentiate into plasma cells).",
      "Treatment: IVIG. Patients are at increased risk for lymphoma, autoimmune diseases (polyarteritis nodosa, vitiligo, alopecia), and unusual skin conditions."
    ],
    keyFacts:"B cells present but dysfunctional → low all Ig classes → same infections as Bruton's but in adolescents/adults of any sex. High risk for lymphoma and autoimmune disease. Treat with IVIG."
  },
  {
    id:5, diagnosis:"IgA Deficiency",
    aliases:["iga deficiency","selective iga deficiency","iga def"],
    episode:"Ep. 173 – Immunodeficiency", topic:"Immunodeficiency",
    ankiStep1:"IgA_def::most_common_immunodeficiency::anti-IgA_Ab",
    ankiStep2:"IgA_def::washed_blood::anaphylaxis::mucosal_infections",
    clues:[
      "A 30-year-old woman with a history of recurrent sinus and GI infections is admitted in acute respiratory distress with stridor and bronchospasm — beginning minutes after a blood transfusion was started.",
      "She has a history of multiple food allergies, frequent Giardia infections, and celiac disease.",
      "She is severely hypotensive with urticaria — this is an anaphylactic reaction. Epinephrine is given.",
      "Serum immunoglobulins: IgG normal, IgM normal, IgA is undetectable. She has anti-IgA antibodies, which triggered anaphylaxis to donor IgA in the blood product.",
      "IgA is the primary immunoglobulin of mucosal surfaces. Without it, patients get recurrent respiratory and GI infections, high rates of atopic disorders, and celiac disease.",
      "This is the MOST COMMON primary immunodeficiency. Future transfusions must use IgA-depleted or washed blood products. All other Ig classes are normal — unlike CVID."
    ],
    keyFacts:"Most common immunodeficiency. IgA absent → anaphylactic transfusion reactions (anti-IgA antibodies), recurrent mucosal infections (Giardia, respiratory), food allergies, celiac. Use IgA-depleted blood for future transfusions."
  },
  {
    id:6, diagnosis:"Ataxia Telangiectasia",
    aliases:["ataxia telangiectasia","at","ataxia-telangiectasia"],
    episode:"Ep. 173 – Immunodeficiency", topic:"Immunodeficiency",
    ankiStep1:"AT::ATM_chr11::DNA_repair::telangiectasias::cerebellar_ataxia",
    ankiStep2:"AT::no_xrays::low_IgA::lymphoma_risk::negative_Romberg",
    clues:[
      "A 6-year-old girl is brought in because she has been 'wobbly' walking over the past 2 years and is progressively losing motor milestones. She has recurrent sinopulmonary infections and mild intellectual disabilities.",
      "Her parents have noticed small red squiggly blood vessels in the whites of her eyes and on her nose.",
      "Physical exam reveals telangiectasias on the conjunctiva and bridge of the nose. The Romberg test is negative (cerebellar ataxia, not dorsal column disease).",
      "Labs show very low IgA, low IgG, and very low total WBC. She is hypersensitive to ionizing radiation — X-rays are contraindicated.",
      "The underlying mutation is in the ATM gene on chromosome 11, which regulates cellular response to double-stranded DNA breaks. This leads to chromosomal instability and lymphocyte apoptosis.",
      "Autosomal recessive. High risk of hematologic malignancies. Fatal early. Distinguish from Friedreich's ataxia (positive Romberg, GAA repeat, AR) and IgA deficiency (normal WBC). Treat: IVIG + antibiotics."
    ],
    keyFacts:"ATM gene (chr 11) → DNA repair defect → cerebellar ataxia + conjunctival telangiectasias + sinopulmonary infections + low IgA/WBC. No X-rays. High malignancy risk. Negative Romberg (cerebellar, not dorsal column)."
  },
  {
    id:7, diagnosis:"Severe Combined Immunodeficiency",
    aliases:["scid","severe combined immunodeficiency","severe combined immune deficiency"],
    episode:"Ep. 173 – Immunodeficiency", topic:"Immunodeficiency",
    ankiStep1:"SCID::absent_thymus::no_T_B_cells::ADA_deficiency",
    ankiStep2:"SCID::no_live_vaccines::BMT::PJP_from_birth",
    clues:[
      "A 4-month-old infant is admitted with Pneumocystis jirovecii pneumonia — his second life-threatening infection. He has also had recurrent oral thrush and CMV pneumonitis.",
      "He has failure to thrive, deep skin abscesses, and liver abscesses. Both T cell and B cell counts are nearly zero on flow cytometry.",
      "Chest X-ray reveals absent thymic shadow.",
      "Labs: severely low WBC, absent serum immunoglobulins. Problems began at birth (unlike Bruton's, where maternal IgG provides 6 months of protection).",
      "One cause is adenosine deaminase (ADA) deficiency — toxic metabolite accumulation causes lymphocyte apoptosis. Another is a mutation in the common gamma chain (IL-2 receptor), blocking all lymphoid development.",
      "Live vaccines are ABSOLUTELY CONTRAINDICATED (fatal infection will result). Definitive treatment: bone marrow transplant. Support with IVIG and prophylactic antibiotics. Lymph node biopsy shows hypoplastic paracortex (T cell zone)."
    ],
    keyFacts:"Combined T+B+NK cell deficiency from birth. Absent thymus. Causes: ADA deficiency or IL-2 gamma chain mutation. NO live vaccines ever. Paracortex hypoplasia on lymph node biopsy. Definitive treatment: bone marrow transplant."
  },
  {
    id:8, diagnosis:"Wiskott-Aldrich Syndrome",
    aliases:["wiskott-aldrich","wiskott aldrich","was","wiskott-aldrich syndrome"],
    episode:"Ep. 173 – Immunodeficiency", topic:"Immunodeficiency",
    ankiStep1:"WAS::WASP_gene::eczema_thrombocytopenia_infections::X-linked",
    ankiStep2:"WAS::high_IgE_IgA::small_platelets::HSCT",
    clues:[
      "A 2-year-old boy presents with a petechial rash scattered across his skin and eczematous patches on his flexural surfaces.",
      "He has recurrent ear infections and skin infections since infancy. CBC shows platelets of 18,000 with notably small platelet size.",
      "A maternal uncle died in childhood from a similar illness with bleeding and infection.",
      "Immunoglobulin levels: IgE markedly elevated, IgA elevated, IgG and IgM are low to normal. T cell function is also defective.",
      "The underlying mutation is in the WASP gene, which affects actin cytoskeleton reorganization in immune cells — disrupting immune synapse formation.",
      "X-linked recessive — affects only boys. Classic triad: eczema + thrombocytopenia (small platelets) + recurrent infections. High risk for autoimmune disorders and hematologic malignancies. Treatment: antibiotics, IVIG, hematopoietic stem cell transplant."
    ],
    keyFacts:"X-linked WASP mutation. Classic triad: eczema + thrombocytopenia (small platelets) + recurrent infections. High IgE/IgA, low IgG/IgM. High risk for leukemia/lymphoma and autoimmune disease. Treat with HSCT."
  },
  {
    id:9, diagnosis:"Leukocyte Adhesion Deficiency",
    aliases:["lad","leukocyte adhesion deficiency"],
    episode:"Ep. 173 – Immunodeficiency", topic:"Immunodeficiency",
    ankiStep1:"LAD::CD18_mutation::no_pus::high_WBC::delayed_cord",
    ankiStep2:"LAD::flow_cytometry_diagnosis::BMT::no_neutrophil_extravasation",
    clues:[
      "A 9-month-old infant has never developed pus at any site of infection despite multiple serious skin and soft tissue infections since birth.",
      "The infant's umbilical cord fell off at 7 weeks of age (normal: 1–2 weeks). He is admitted with apparent cellulitis — minimal redness, no abscess formation.",
      "CBC during active infection shows WBC of 52,000 (markedly elevated). Paradoxically: a huge leukocyte count with almost no visible tissue inflammation.",
      "Neutrophils cannot adhere to vascular endothelium or roll along it — they cannot migrate out of circulation into infected tissue.",
      "The molecular defect involves mutations in CD18 (beta-2 integrin), CD11a, sialyl-Lewis X, or E/P-selectin — all required for neutrophil extravasation from blood into tissue.",
      "Diagnosis: flow cytometry to detect absence of CD18/CD11 on neutrophil surface. No pus despite sky-high WBC is the pathognomonic hallmark. Definitive treatment: bone marrow transplant."
    ],
    keyFacts:"CD18 (beta-2 integrin) mutation → neutrophils can't adhere/migrate → no pus despite sky-high WBC. Delayed umbilical cord separation. Flow cytometry is diagnostic. Treat with BMT."
  },
  {
    id:10, diagnosis:"Chronic Granulomatous Disease",
    aliases:["cgd","chronic granulomatous disease"],
    episode:"Ep. 173 – Immunodeficiency", topic:"Immunodeficiency",
    ankiStep1:"CGD::NADPH_oxidase::catalase_pos_organisms::NBT_test",
    ankiStep2:"CGD::IFN_gamma_tx::Staph_Aspergillus::X-linked::GI_granulomas",
    clues:[
      "A 6-year-old boy has had recurrent lymphadenopathy, deep skin abscesses, and two liver abscesses since age 18 months.",
      "Cultures consistently grow Staphylococcus aureus and Aspergillus species. He also has granulomatous lesions in his colon on imaging — mimicking Crohn's disease.",
      "Neutrophil function testing: NADPH oxidase is absent, completely impairing the oxidative burst.",
      "Neutrophils cannot generate reactive oxygen species needed to kill catalase-positive organisms. The pathogens are phagocytosed but not destroyed.",
      "Diagnostic tests: nitroblue tetrazolium (NBT) test or dihydrorhodamine (DHR) flow cytometry — neutrophils show no oxidative activity.",
      "X-linked recessive (mostly boys). Vulnerable organisms are CATALASE-POSITIVE: Staph aureus, Aspergillus, Candida, Serratia, Nocardia, Pseudomonas. Treatment: prophylactic IFN-gamma and TMP-SMX."
    ],
    keyFacts:"NADPH oxidase deficiency → impaired oxidative burst → can't kill catalase(+) organisms. NBT/DHR test confirms. GI granulomas mimic Crohn's. X-linked. Treat with IFN-gamma + TMP-SMX prophylaxis."
  },
  {
    id:11, diagnosis:"Chediak-Higashi Syndrome",
    aliases:["chediak-higashi","chediak higashi","chs"],
    episode:"Ep. 173 – Immunodeficiency", topic:"Immunodeficiency",
    ankiStep1:"CHS::LYST_gene::giant_WBC_granules::partial_albinism",
    ankiStep2:"CHS::AR::fatal_childhood::HSCT::NK_cell_defect",
    clues:[
      "A 4-year-old child with partial albinism, blonde hair, and blue eyes is admitted for a severe Staphylococcal skin infection — his third hospitalization this year.",
      "Parents report repeated infections since infancy, progressive intellectual disabilities, and the child has always had abnormally light-colored hair and skin.",
      "Physical exam reveals nystagmus and hypotonia. NK cell function is severely reduced.",
      "Peripheral blood smear: giant, abnormally large granules are seen in ALL white blood cells — neutrophils, lymphocytes, monocytes. This is the pathognomonic finding.",
      "The LYST gene is mutated, impairing lysosomal trafficking. Neutrophils cannot properly form or release toxic granules (degranulation is defective), and melanosomes cannot distribute normally — causing partial albinism.",
      "Autosomal recessive. Fatal in childhood without treatment. High risk of hematologic malignancies. Key: giant WBC granules on blood smear + albinism + recurrent infections. Treat: HSCT before accelerated phase."
    ],
    keyFacts:"LYST mutation → defective lysosomal trafficking → giant WBC granules (pathognomonic) + partial albinism + recurrent infections + NK cell dysfunction. Fatal in childhood. AR. Treat with HSCT."
  },
  {
    id:12, diagnosis:"DiGeorge Syndrome",
    aliases:["digeorge","digeorge syndrome","22q11","catch-22","velocardiofacial syndrome"],
    episode:"Ep. 173 – Immunodeficiency", topic:"Immunodeficiency",
    ankiStep1:"DiGeorge::22q11::FISH::no_thymus_parathyroid::CATCH22",
    ankiStep2:"DiGeorge::hypocalcemia_seizures::T_cell_deficiency::CHD_most_common_death",
    clues:[
      "A newborn develops severe hypocalcemia with tetany and seizures within the first 24 hours of life.",
      "EKG shows a prolonged QT interval from hypocalcemia. Echo reveals a VSD and truncus arteriosus. The infant has dysmorphic facial features including micrognathia and low-set ears.",
      "Chest X-ray shows absent thymic shadow. The infant will be highly susceptible to viral and fungal infections.",
      "The defect is failure of the 3rd and 4th pharyngeal pouches to develop — resulting in absent thymus (no T cells) and absent parathyroids (no PTH → hypocalcemia).",
      "Genetic testing by FISH reveals a microdeletion of chromosome 22q11.2.",
      "CATCH-22: Cardiac defects (VSD, truncus arteriosus, tetralogy of Fallot, interrupted aortic arch), Abnormal facies, Thymic aplasia, Cleft palate + Cognitive deficits, Hypoparathyroidism. Most common cause of death: congenital heart disease. FISH is diagnostic."
    ],
    keyFacts:"22q11.2 deletion (FISH). Failure of 3rd/4th pharyngeal pouches → absent thymus (T cell deficit) + absent parathyroids (hypocalcemia). CATCH-22. Most common death: CHD. Absent thymic shadow on CXR."
  },
  {
    id:13, diagnosis:"Zollinger-Ellison Syndrome",
    aliases:["zollinger-ellison","zollinger ellison","zes","gastrinoma"],
    episode:"Ep. 185 – Gastroenterology", topic:"GI",
    ankiStep1:"ZES::gastrinoma::high_gastrin::unusual_ulcers",
    ankiStep2:"ZES::PPIs_surgery_octreotide::MEN1::secretin_test",
    clues:[
      "A 48-year-old man has had multiple recurrent peptic ulcers despite maximal acid-suppression therapy. The ulcers keep returning within weeks of healing.",
      "He also reports severe watery diarrhea in addition to epigastric pain.",
      "Endoscopy reveals peptic ulcers in unusual locations — in the jejunum and distal duodenum, far beyond where typical H. pylori ulcers occur.",
      "Fasting serum gastrin level: >1,000 pg/mL (normal <100). Secretin stimulation test paradoxically INCREASES gastrin levels further.",
      "CT scan reveals a 1.5 cm mass in the head of the pancreas — a gastrinoma secreting gastrin constitutively, causing massive hypersecretion of gastric acid.",
      "Treatment: high-dose PPIs and surgical resection of the gastrinoma. Octreotide (somatostatin analog) can suppress gastrin secretion. ~60% of gastrinomas are malignant. Associated with MEN1 (Para-Pan-Pit triad)."
    ],
    keyFacts:"Gastrinoma → uncontrolled gastrin → massive acid → recurrent ulcers in unusual locations + watery diarrhea. Secretin paradoxically ↑ gastrin. Treat: PPIs + surgery + octreotide. MEN1 association."
  },
  {
    id:14, diagnosis:"Pernicious Anemia",
    aliases:["pernicious anemia","pernicious anaemia"],
    episode:"Ep. 185 – Gastroenterology", topic:"GI",
    ankiStep1:"Pernicious_anemia::anti_IF_anti_parietal_cell::B12_deficiency",
    ankiStep2:"Pernicious_anemia::subacute_combined_degeneration::IM_B12::hypersegmented_neutrophils",
    clues:[
      "A 58-year-old woman presents with progressive fatigue, tingling in her hands and feet, and difficulty with balance. She also has a smooth, beefy-red tongue.",
      "She reports poor memory over the past year. Neurological exam: diminished proprioception and vibration sense (dorsal columns), plus exaggerated reflexes (corticospinal tract damage).",
      "Labs: Hgb 7.5 g/dL, MCV 112 fL (macrocytic). Peripheral smear: hypersegmented neutrophils (≥5 lobes). Methylmalonic acid and homocysteine are both elevated.",
      "The cause is autoimmune destruction of gastric parietal cells. Without parietal cells, intrinsic factor is not produced and B12 cannot be absorbed at the terminal ileum.",
      "Antibodies are directed against the H+/K+ ATPase, parietal cells themselves, or intrinsic factor directly. This is distinguished from dietary B12 deficiency by the presence of these antibodies.",
      "Neurological manifestation: subacute combined degeneration of the spinal cord (dorsal columns + lateral corticospinal tracts). Treatment: intramuscular B12 injections (bypasses the GI absorption defect)."
    ],
    keyFacts:"Anti-parietal cell / anti-intrinsic factor antibodies → no IF → B12 malabsorption → megaloblastic anemia + subacute combined degeneration (dorsal + lateral corticospinal tracts). Elevated MMA + homocysteine. Treat: IM B12."
  },
  {
    id:15, diagnosis:"Achalasia",
    aliases:["achalasia"],
    episode:"Ep. 185 – Gastroenterology", topic:"GI",
    ankiStep1:"Achalasia::LES_fails_to_relax::absent_peristalsis::birds_beak",
    ankiStep2:"Achalasia::Heller_myotomy::Chagas_secondary::manometry_gold_standard",
    clues:[
      "A 38-year-old woman has had progressive difficulty swallowing for 2 years — affecting both solids AND liquids from the very beginning (unlike esophageal strictures, which start with solids only).",
      "She regurgitates undigested food with no acid taste (no GERD), has lost 18 lbs, and occasionally wakes at night coughing from regurgitated material.",
      "Barium swallow shows the classic 'bird's beak' sign — smooth tapering at the distal esophagus with a massively dilated proximal esophagus.",
      "Esophageal manometry: absent peristalsis throughout the esophageal body AND failure of the lower esophageal sphincter (LES) to relax with swallowing (elevated resting LES pressure).",
      "Pathology: loss or degeneration of inhibitory (NO/VIP-releasing) myenteric (Auerbach) plexus neurons at the LES. Manometry is the gold standard for diagnosis.",
      "Secondary cause: Chagas disease (Trypanosoma cruzi destroys the enteric plexus) — also causes dilated cardiomyopathy and Hirschsprung-like megacolon. Treatment: pneumatic dilation, Heller myotomy, or botulinum toxin injection."
    ],
    keyFacts:"Loss of Auerbach's plexus → LES fails to relax + absent peristalsis → dysphagia to solids AND liquids. Bird's beak on barium. Manometry gold standard. Secondary: Chagas (T. cruzi). Treat: pneumatic dilation / Heller myotomy."
  },
  {
    id:16, diagnosis:"Boerhaave Syndrome",
    aliases:["boerhaave","boerhaave syndrome","esophageal rupture","spontaneous esophageal perforation"],
    episode:"Ep. 185 – Gastroenterology", topic:"GI",
    ankiStep1:"Boerhaave::esophageal_rupture::pneumomediastinum::Gastrografin_NOT_barium",
    ankiStep2:"Boerhaave::surgical_emergency::subcutaneous_emphysema::violent_vomiting",
    clues:[
      "A 52-year-old man with heavy alcohol use presents with sudden, excruciating chest and abdominal pain that began immediately after a violent vomiting episode.",
      "He is febrile (39.1°C), tachycardic, and hypotensive. He cannot swallow his own saliva and appears to be in extremis.",
      "On exam, you palpate a 'rice crispy' crackling sensation in the neck and supraclavicular region — subcutaneous emphysema.",
      "Chest X-ray and CT show pneumomediastinum — air tracking along the aorta, mediastinal structures, and pleural space. Left pleural effusion with food particles.",
      "Imaging was done with WATER-SOLUBLE contrast (Gastrografin) — NOT barium swallow, which is absolutely contraindicated because barium in the mediastinum causes fatal mediastinitis.",
      "Surgical emergency with >30% mortality. Full-thickness transmural perforation of the esophagus caused by sudden dramatic increase in intraesophageal pressure. Immediate surgical repair required. Contrast with Mallory-Weiss (partial thickness, no pneumomediastinum, self-limited)."
    ],
    keyFacts:"Full-thickness esophageal rupture from violent vomiting → pneumomediastinum + subcutaneous emphysema + septic shock. Use Gastrografin (NOT barium). Surgical emergency. Contrast: Mallory-Weiss = mucosa only, small bleed, self-limited."
  },
  {
    id:17, diagnosis:"Mallory-Weiss Tear",
    aliases:["mallory-weiss","mallory weiss","mallory-weiss tear","mallory weiss tear"],
    episode:"Ep. 185 – Gastroenterology", topic:"GI",
    ankiStep1:"Mallory-Weiss::GE_junction_mucosal_tear::retching::hematemesis",
    ankiStep2:"Mallory-Weiss::partial_thickness::self-limited::contrast_Boerhaave",
    clues:[
      "A 44-year-old man with heavy alcohol use presents with hematemesis after a prolonged episode of violent retching and vomiting.",
      "He reports that he vomited forcefully multiple times without blood, and then bright red blood appeared after the fifth or sixth episode.",
      "He is mildly tachycardic but hemodynamically stable. The volume of blood loss is relatively small.",
      "Upper endoscopy reveals a longitudinal mucosal laceration at the gastroesophageal junction — partial thickness (mucosa and submucosa only — not full thickness).",
      "There is no pneumomediastinum, no subcutaneous emphysema, and the patient is not in extremis — unlike Boerhaave syndrome.",
      "This is typically self-limited and stops with supportive care (fluids, anti-emetics). Risk factors: severe retching, vomiting, hiccups, seizures, or anything dramatically increasing intraesophageal pressure."
    ],
    keyFacts:"Forceful retching → longitudinal mucosal/submucosal tear at GE junction → small-volume hematemesis. Partial thickness (unlike Boerhaave's full-thickness perforation). Usually self-limited. No pneumomediastinum. Endoscopy is diagnostic."
  },
  {
    id:18, diagnosis:"Plummer-Vinson Syndrome",
    aliases:["plummer-vinson","plummer vinson","plummer-vinson syndrome","paterson-kelly"],
    episode:"Ep. 185 – Gastroenterology", topic:"GI",
    ankiStep1:"Plummer-Vinson::esophageal_web::iron_deficiency::dysphagia",
    ankiStep2:"Plummer-Vinson::SCC_risk::koilonychia::smooth_tongue::iron_tx",
    clues:[
      "A 52-year-old woman presents with difficulty swallowing solid food for several months. She says food 'gets stuck in her throat' — upper esophageal level.",
      "She also has a sore, smooth tongue (atrophic glossitis) and brittle, concave, spoon-shaped fingernails (koilonychia). She has been very fatigued.",
      "CBC: Hgb 7.8 g/dL, MCV 65 fL (microcytic, hypochromic). Iron studies: low serum iron, low ferritin, elevated TIBC — classic iron deficiency anemia.",
      "Upper endoscopy reveals a thin, web-like mucosal fold in the upper (cervical) esophagus causing partial obstruction. The web is dilated with the endoscope.",
      "This classic triad — dysphagia (cervical esophageal web) + iron deficiency anemia + atrophic glossitis (smooth tongue) — defines this condition.",
      "Treatment: iron supplementation and esophageal dilation. Important: carries an increased risk of squamous cell carcinoma of the pharynx and upper esophagus — requires surveillance."
    ],
    keyFacts:"Triad: dysphagia (cervical esophageal web, upper esophagus) + iron deficiency anemia (microcytic) + atrophic glossitis. Also: koilonychia. Increased SCC risk. Treat: iron supplementation + endoscopic dilation."
  },
  {
    id:19, diagnosis:"Esophageal Varices",
    aliases:["esophageal varices","variceal bleeding","esophageal variceal bleeding"],
    episode:"Ep. 185 – Gastroenterology", topic:"GI",
    ankiStep1:"Esophageal_varices::portal_HTN::octreotide_first_line::banding",
    ankiStep2:"Esophageal_varices::TIPS::propranolol_prevention::hepatic_encephalopathy_risk",
    clues:[
      "A 58-year-old man with 30 years of heavy alcohol use presents to the ER with massive hematemesis — vomiting approximately 1 liter of bright red blood.",
      "He is cachectic and jaundiced with palmar erythema, spider angiomas, gynecomastia, caput medusae, and a distended abdomen with ascites.",
      "Vital signs: BP 75/48, HR 132. Labs: elevated bilirubin, PT/INR 2.4, albumin 2.1, thrombocytopenia.",
      "IV octreotide (somatostatin analog) is initiated immediately — this is first-line for the acute phase to reduce portal pressure.",
      "Endoscopy reveals large, tortuous dilated veins at the gastroesophageal junction actively bleeding. Endoscopic band ligation is performed. Ceftriaxone is given to prevent bacterial translocation.",
      "Portal hypertension → portosystemic collaterals at GE junction → varices. Prevention: non-selective beta-blockers (propranolol). If refractory: TIPS procedure (can cause hyperammonemia/hepatic encephalopathy). Spironolactone for ascites."
    ],
    keyFacts:"Portal hypertension → esophageal varices → massive hematemesis. Signs of chronic liver disease (jaundice, spider angiomas, caput medusae, ascites). Acute: IV octreotide + endoscopic banding + prophylactic antibiotics. Prevention: propranolol. TIPS if refractory."
  },
  {
    id:20, diagnosis:"Zenker's Diverticulum",
    aliases:["zenkers diverticulum","zenker's diverticulum","zenker diverticulum","pharyngoesophageal diverticulum"],
    episode:"Ep. 185 – Gastroenterology", topic:"GI",
    ankiStep1:"Zenker::Killian_triangle::false_diverticulum::halitosis",
    ankiStep2:"Zenker::EGD_contraindicated::barium_swallow_dx::elderly_men::surgery",
    clues:[
      "A 72-year-old man presents with chief complaint of foul-smelling breath (halitosis) that persists despite meticulous dental hygiene.",
      "He also occasionally regurgitates old, undigested food he ate days ago — with no acid or bitter taste. He sometimes feels food 'sitting in his throat.' A gurgling sound can be heard in his neck when he swallows.",
      "He has recently developed hoarseness and a recurrent cough from aspiration.",
      "Barium swallow reveals a posterior outpouching of the upper esophagus arising from Killian's triangle — the area of muscular weakness between thyropharyngeus and cricopharyngeus fibers.",
      "This is a FALSE (pulsion) diverticulum — it contains only mucosa and submucosa (not all four layers). Increased hypopharyngeal pressure from cricopharyngeal dysfunction causes herniation at this weakness.",
      "EGD (esophagoscopy) is CONTRAINDICATED — the scope can perforate the pouch. Treat: surgical excision. Most common in elderly men. Contrast: traction diverticulum (mid-esophagus) is a TRUE diverticulum with all four layers."
    ],
    keyFacts:"Posterior outpouching at Killian's triangle (upper esophagus). False diverticulum (mucosa + submucosa only). Halitosis + regurgitation of old (non-acid) food + neck gurgling. Dx: barium swallow. EGD is CONTRAINDICATED. Treat: surgery."
  },
  {
    id:21, diagnosis:"Candida Esophagitis",
    aliases:["candida esophagitis","esophageal candidiasis","candidal esophagitis"],
    episode:"Ep. 185 – Gastroenterology", topic:"GI",
    ankiStep1:"Candida_esophagitis::AIDS_CD4_under_100::fluconazole::pseudohyphae",
    ankiStep2:"Candida_esophagitis::odynophagia::vs_CMV_ganciclovir::vs_HSV_acyclovir",
    clues:[
      "A patient with AIDS (CD4 count = 38 cells/μL) presents with 3 weeks of painful swallowing (odynophagia) and difficulty eating.",
      "Physical exam of the oropharynx reveals thick white plaques adherent to the tongue and hard palate (oral thrush).",
      "He has been rapidly losing weight and has mild dysphagia in addition to odynophagia.",
      "When no specific distinguishing features are given to differentiate esophagitis types, ALWAYS choose this as the most common cause in immunocompromised patients.",
      "Endoscopy: white plaques coating the esophageal mucosa. Biopsy: pseudohyphae and budding yeast forms — the hallmark.",
      "Treatment: fluconazole (systemic azole — first-line for esophageal disease, unlike oral candidiasis where topical nystatin suffices). Histology: hyphae = Candida; CMV = perinuclear halo/intranuclear inclusions (treat with ganciclovir); HSV = intranuclear inclusions with peripheral chromatin, no perinuclear halo (treat with acyclovir). Foscarnet for resistance."
    ],
    keyFacts:"AIDS + CD4<100 + odynophagia + white plaques = Candida esophagitis (most common). Treat: fluconazole. CMV: perinuclear halo → ganciclovir. HSV: intranuclear inclusions (peripheral chromatin) → acyclovir. Resistance to both → foscarnet."
  },
  {
    id:22, diagnosis:"Hirschsprung's Disease",
    aliases:["hirschsprung","hirschsprung's disease","hirschsprung disease","congenital aganglionic megacolon"],
    episode:"Ep. 186 – Gastroenterology", topic:"GI",
    ankiStep1:"Hirschsprung::neural_crest_migration::absent_ganglion_cells::RET_gene",
    ankiStep2:"Hirschsprung::squirt_sign::rectal_biopsy_gold_standard::Down_syndrome",
    clues:[
      "A 3-day-old term newborn has not passed meconium since birth. Normal infants pass meconium within 48 hours.",
      "The abdomen is progressively distending. The infant is not tolerating feeds and has had bilious emesis.",
      "Rectal examination is performed and is followed by an explosive expulsion of stool and gas — the classic 'squirt sign.'",
      "Barium enema reveals a narrowed distal aganglionic segment with a transition zone leading to dilated proximal colon.",
      "Rectal suction biopsy (gold standard) confirms absence of ganglion cells in the myenteric (Auerbach's) and submucosal (Meissner's) plexuses in the narrowed distal segment.",
      "Caused by failure of neural crest cells to migrate to the distal colon. RET gene loss-of-function mutation. Associated with Down syndrome. Chagas disease (T. cruzi) can mimic this by destroying the enteric plexus. Treatment: resect aganglionic segment + end-to-end anastomosis."
    ],
    keyFacts:"Failure of neural crest cell migration → absent Auerbach's/Meissner's plexus → aganglionic segment → obstruction. 'Squirt sign' on rectal exam. Biopsy is gold standard. RET mutation. Down syndrome + Chagas associations."
  },
  {
    id:23, diagnosis:"Intussusception",
    aliases:["intussusception"],
    episode:"Ep. 186 – Gastroenterology", topic:"GI",
    ankiStep1:"Intussusception::6mo_2yr::currant_jelly::target_sign_US",
    ankiStep2:"Intussusception::air_enema_tx::Meckel_lead_point::Dance_sign",
    clues:[
      "An 18-month-old boy is brought to the ED because he has been crying inconsolably in waves for several hours, drawing his knees up to his chest with each episode.",
      "Between episodes he appears lethargic and pale. His parents describe bloody, mucousy ('currant jelly') stools. He had a viral URI 2 weeks ago.",
      "Abdominal exam reveals a sausage-shaped mass in the right upper quadrant. The right lower quadrant feels abnormally empty ('Dance's sign').",
      "Abdominal ultrasound shows a 'target sign' / 'doughnut sign' — concentric rings representing telescoping of one bowel segment into the adjacent distal segment.",
      "Most common cause of intestinal obstruction in children aged 6 months to 2 years. Lead point here: hyperplasia of Peyer's patches in the terminal ileum following the viral illness. In older children, Meckel's diverticulum can be the lead point.",
      "Treatment: air enema — both diagnostic AND therapeutic. If air enema fails or perforation occurs → emergent surgery. If child becomes septic after air enema → think bowel perforation → urgent XR for free air + broad-spectrum antibiotics + OR."
    ],
    keyFacts:"Colicky pain in waves + currant jelly stools + sausage RUQ mass. Most common bowel obstruction in 6mo–2yr. Lead point: Peyer's patch hyperplasia (post-viral) or Meckel's diverticulum. Dx: US target sign. Tx: air enema (diagnostic + therapeutic)."
  },
  {
    id:24, diagnosis:"Appendicitis",
    aliases:["appendicitis","acute appendicitis"],
    episode:"Ep. 186 – Gastroenterology", topic:"GI",
    ankiStep1:"Appendicitis::McBurney::Rovsing::psoas_sign::carcinoid_site",
    ankiStep2:"Appendicitis::CT_adults_US_kids::iliohypogastric_nerve_risk::fecalith",
    clues:[
      "A 21-year-old man presents with 14 hours of abdominal pain that began around his umbilicus and has since migrated to his right lower quadrant.",
      "He is anorexic, has vomited twice, has a low-grade fever of 38.1°C, and pain is worsened by movement.",
      "Exam: tenderness at McBurney's point (2/3 from umbilicus to ASIS). Positive Rovsing's sign (LLQ palpation → RLQ pain), positive psoas sign (hip extension → RLQ pain).",
      "WBC 15,200. CT abdomen/pelvis with contrast: dilated appendix (>6mm), periappendiceal fat stranding, wall thickening, and a hyperdense appendicolith (fecalith).",
      "In adults: fecalith obstruction is the most common cause. In children: lymphoid hyperplasia after viral GI infections is more common. In children, ultrasound is preferred over CT.",
      "Treatment: IV fluids, broad-spectrum antibiotics (ampicillin + gentamicin + metronidazole, or ceftriaxone + metronidazole), laparoscopic appendectomy. Surgical complication: iliohypogastric nerve injury → burning sensation around the bladder/inguinal region. High-yield: appendix is the most common site of carcinoid tumors."
    ],
    keyFacts:"Periumbilical pain → RLQ migration + anorexia + fever + leukocytosis. McBurney's/Rovsing's/psoas signs. CT (adults), US (children). Tx: antibiotics + laparoscopic appendectomy. Appendix = most common carcinoid tumor site."
  },
  {
    id:25, diagnosis:"Ulcerative Colitis",
    aliases:["ulcerative colitis","uc"],
    episode:"Ep. 186 – Gastroenterology", topic:"GI",
    ankiStep1:"UC::continuous_mucosa_rectum::crypt_abscesses::pANCA::PSC",
    ankiStep2:"UC::toxic_megacolon::pyoderma_gangrenosum::lead_pipe::total_colectomy_cure",
    clues:[
      "A 26-year-old man presents with 7 months of bloody diarrhea (8–10 episodes/day) with urgency, tenesmus, and left lower quadrant cramping. He has lost 14 lbs.",
      "He also has bilateral knee/ankle arthritis (HLA-B27 positive) and recurrent eye inflammation (uveitis). He also has irregular painful skin ulcers on his legs.",
      "Colonoscopy: continuous mucosal inflammation starting from the rectum and extending proximally — no skip pattern. Crypt abscesses on biopsy.",
      "Barium enema: loss of haustra ('lead pipe' colon). Serum: p-ANCA positive. He also has primary sclerosing cholangitis (PSC) — elevated ALP and conjugated bilirubin.",
      "Disease is continuous (no skips), rectum ALWAYS involved, only mucosa ± submucosa affected (NOT transmural), and the colon cancer risk is very high (screen 8 years after first diagnosis).",
      "Most serious complication: toxic megacolon (massive dilation of transverse colon — surgical emergency, can also be caused by C. diff). Extra-intestinal: arthritis (HLA-B27), uveitis, pyoderma gangrenosum (not erythema nodosum), PSC. Cure: total proctocolectomy. Tx: 5-ASA, 6-MP/azathioprine, steroids, anti-TNF."
    ],
    keyFacts:"Continuous bloody diarrhea from rectum proximally (no skips). Mucosa/submucosa only. Crypt abscesses. p-ANCA. PSC association. Toxic megacolon (emergency). High colon CA risk. Pyoderma gangrenosum (not erythema nodosum). Cure: total proctocolectomy."
  },
  {
    id:26, diagnosis:"Crohn's Disease",
    aliases:["crohn's disease","crohn disease","crohns","regional enteritis"],
    episode:"Ep. 186 – Gastroenterology", topic:"GI",
    ankiStep1:"Crohns::skip_lesions::transmural::ASCA::string_sign::rectal_sparing",
    ankiStep2:"Crohns::oxalate_stones::B12_def::fistulas::erythema_nodosum::anti-TNF",
    clues:[
      "A 30-year-old woman has had 9 months of chronic, non-bloody, watery diarrhea with right lower quadrant pain and 22 lbs of weight loss.",
      "She has tender red-blue nodules on her shins (erythema nodosum), episodic uveitis, and painful aphthous mouth ulcers.",
      "Colonoscopy: 'skip lesions' — areas of normal mucosa alternating with inflamed segments. Terminal ileum severely involved. Rectum is SPARED.",
      "Biopsy: non-caseating granulomas and transmural inflammation (all bowel layers → fistula risk). Barium: 'string sign' in terminal ileum. Anti-Saccharomyces cerevisiae antibodies (ASCA) positive.",
      "Terminal ileum involvement causes: fat malabsorption, bile acid malabsorption → calcium oxalate kidney stones (flank pain to groin), vitamin B12 malabsorption → megaloblastic anemia, and fistula formation (colovesical, colovaginal).",
      "Key contrasts with UC: Crohn's = skip lesions, transmural, rectal sparing, non-caseating granulomas, string sign, ASCA+, erythema nodosum (not pyoderma gangrenosum), fistulas. Treatment: 5-ASA, steroids, infliximab (anti-TNF)."
    ],
    keyFacts:"Skip lesions, transmural, rectal sparing, non-caseating granulomas, ASCA+, string sign. Terminal ileum → oxalate stones + B12 deficiency + fat malabsorption. Erythema nodosum. Fistulas common. Treat: 5-ASA, steroids, anti-TNF (infliximab)."
  },
  {
    id:27, diagnosis:"Diverticulosis",
    aliases:["diverticulosis","colonic diverticula","colonic diverticulosis"],
    episode:"Ep. 186 – Gastroenterology", topic:"GI",
    ankiStep1:"Diverticulosis::painless_LGIB_elderly::false_diverticula::sigmoid",
    ankiStep2:"Diverticulosis::low_fiber::vs_diverticulitis_painful_fever::most_common_LGIB",
    clues:[
      "A 70-year-old woman calls her doctor after noticing a large amount of bright red blood in the toilet after a bowel movement. She feels otherwise well.",
      "She has NO abdominal pain, NO fever. She takes NSAIDs regularly for osteoarthritis and has had chronic constipation for years.",
      "Colonoscopy reveals multiple small outpouchings throughout the sigmoid and descending colon.",
      "These are FALSE diverticula — containing only mucosa and submucosa (not muscularis propria or serosa) — arising at natural weak points where vasa recta penetrate the bowel wall.",
      "Risk factors: low-fiber diet, chronic constipation, increased intraluminal pressure, advancing age. Most common in the left (sigmoid) colon.",
      "This is the most common cause of painless lower GI bleeding in the elderly. Key: painless (vs. diverticulitis = painful + fever + LLQ tenderness). When a diverticulum becomes infected (fecalith), it becomes diverticulitis. Most bleeds stop spontaneously."
    ],
    keyFacts:"Most common cause of PAINLESS lower GI bleed in elderly. False diverticula (mucosa + submucosa only) at weak points where vasa recta enter. Sigmoid most common. Risk: low fiber, constipation. Diverticulitis = painful + fever + LLQ pain."
  },
  {
    id:28, diagnosis:"Familial Adenomatous Polyposis",
    aliases:["fap","familial adenomatous polyposis","adenomatous polyposis coli"],
    episode:"Ep. 186 – Gastroenterology", topic:"GI",
    ankiStep1:"FAP::APC_chr5_AD::hundreds_polyps::100pct_colon_CA_by_40",
    ankiStep2:"FAP::Turcot_medulloblastoma::Gardner_soft_tissue::prophylactic_colectomy",
    clues:[
      "A 24-year-old man's colonoscopy (ordered because his father developed colon cancer at age 45) shows hundreds of adenomatous polyps carpeting his entire colon.",
      "His father also has a jaw osteoma and retinal pigmentation. The patient has no GI symptoms yet.",
      "Without prophylactic intervention, virtually 100% of patients with this condition will develop colorectal cancer by age 40.",
      "Germline mutation in the APC (adenomatous polyposis coli) tumor suppressor gene on chromosome 5q21. Autosomal dominant inheritance.",
      "Associations: APC mutation + posterior fossa medulloblastoma (dropping metastases to the cord) = Turcot syndrome. APC mutation + soft tissue tumors + retinal hyperplasia = Gardner's syndrome.",
      "Treatment: prophylactic total colectomy in early adulthood. Regular upper endoscopy (duodenal/periampullary cancer risk). Chromosomal instability pathway: APC → KRAS oncogene → DCC/SMAD → p53 → frank carcinoma."
    ],
    keyFacts:"APC tumor suppressor gene (chr 5q21, AD). Hundreds of polyps → 100% colon CA risk by 40. Prophylactic colectomy. Turcot (+ medulloblastoma). Gardner (+ soft tissue tumors + retinal pigmentation). Chromosomal instability pathway."
  },
  {
    id:29, diagnosis:"Septic Shock",
    aliases:["septic shock","sepsis with shock","septicemia"],
    episode:"Ep. 233 – Shock", topic:"Shock",
    ankiStep1:"Septic_shock::low_SVR_high_CO::warm_shock::norepinephrine_first_line",
    ankiStep2:"Septic_shock::qSOFA::lactate::ceftazidime_vanco::high_MVO2",
    clues:[
      "A 72-year-old man with diabetes and a long-term Foley catheter presents with fever (39.6°C), chills, altered mental status, and shortness of breath.",
      "Vital signs: BP 74/40, HR 138, RR 30. Urine is turbid and foul-smelling. He does not improve after 2L IV normal saline.",
      "Physical exam: the patient is WARM and flushed (vasodilated) despite being hypotensive — classic 'warm shock.'",
      "Hemodynamic profile: ↓SVR (vasodilation from cytokines — histamine, bradykinin, PGE), ↑CO (hyperdynamic heart), ↓PCWP, ↓CVP, ↑MVO2 (venous O2 content elevated because tissues aren't extracting efficiently). Lactate 5.8 mmol/L.",
      "qSOFA criteria met (≥2): altered mental status + RR>22 + SBP≤100. Cultures drawn, broad-spectrum antibiotics started (ceftazidime + vancomycin to cover Pseudomonas + MRSA).",
      "After fluid resuscitation fails to maintain MAP ≥65 mmHg → start vasopressors. FIRST-LINE: norepinephrine (alpha-1 + beta-1). In septic shock, CO is high and SVR is low — the exact opposite of cardiogenic shock."
    ],
    keyFacts:"Infection → cytokines → ↓SVR + ↑CO + ↑MVO2 (warm shock). qSOFA ≥2. Tx: cultures, broad-spectrum abx (antipseudomonal + anti-MRSA), IV fluids, then norepinephrine (first-line) if MAP <65 after fluids. Target lactate clearance."
  },
  {
    id:30, diagnosis:"Cardiogenic Shock",
    aliases:["cardiogenic shock"],
    episode:"Ep. 233 – Shock", topic:"Shock",
    ankiStep1:"Cardiogenic_shock::low_CO_high_SVR::high_PCWP::cold_shock",
    ankiStep2:"Cardiogenic_shock::dobutamine_milrinone::IABP::widened_pulse_pressure_milrinone",
    clues:[
      "A 62-year-old man with a large anterior STEMI 4 days ago suddenly develops respiratory failure and profound hypotension.",
      "He is COLD and clammy (vasoconstricted). He has JVD, bilateral crackles throughout both lung fields, and an S3 gallop — classic 'cold shock.' He does NOT respond to IV fluids.",
      "Vital signs: BP 70/52, HR 118. Echo shows EF of 15% with wall motion abnormalities in the LAD territory.",
      "Swan-Ganz catheter: ↓CO (failing heart), ↑SVR (compensatory vasoconstriction), ↑PCWP >18 mmHg (pulmonary congestion), ↑CVP, ↓MVO2 (tissues extracting all available O2).",
      "Both PCWP and CVP are ELEVATED (fluid backing up in both sides) — contrast with hypovolemic shock, where both are LOW. This is post-MI pump failure.",
      "Treatment: dobutamine (beta-1 agonist) or milrinone (PDE-3 inhibitor → ↑cAMP → ↑contractility + vasodilation). Milrinone: ↑CO (= ↑SBP) + ↓SVR (= ↓DBP) → WIDENS pulse pressure. Mechanical support: IABP or Impella. Emergent PCI."
    ],
    keyFacts:"Post-MI → ↓CO + ↑SVR + ↑PCWP + ↑CVP + ↓MVO2 (cold shock). Both RA/LA pressures elevated. Tx: dobutamine or milrinone (PDE-3i, widens pulse pressure) + emergent PCI + IABP/Impella. Contrast: septic = warm, ↑CO, ↓SVR."
  },
  {
    id:31, diagnosis:"Neurogenic Shock",
    aliases:["neurogenic shock","spinal shock"],
    episode:"Ep. 233 – Shock", topic:"Shock",
    ankiStep1:"Neurogenic_shock::bradycardia_hypotension::both_SVR_CO_low::unique",
    ankiStep2:"Neurogenic_shock::norepinephrine_atropine::warm_hypotension::sympathectomy",
    clues:[
      "A 27-year-old man sustains a C5 spinal cord injury in a high-speed diving accident. In the trauma bay he is hypotensive.",
      "His heart rate is 45 bpm — he is BRADYCARDIC despite being hypotensive. All other types of shock cause compensatory tachycardia — this is the key distinguishing feature.",
      "He is WARM and flushed (vasodilated periphery) despite profound hypotension — not cold and clammy like cardiogenic or hypovolemic shock.",
      "This is the ONLY type of shock where both SVR AND CO are simultaneously decreased. In all other shocks, CO and SVR oppose each other.",
      "Mechanism: injury above thoracic sympathetic outflow eliminates both cardiac sympathetics (↓HR, ↓contractility → ↓CO) AND vascular sympathetics (↓vascular tone → ↓SVR → blood pools in extremities). Only vagal tone remains.",
      "Treatment: vasopressors (phenylephrine or norepinephrine), atropine for bradycardia, cautious fluids, high-dose methylprednisolone. KEY: neurogenic shock = bradycardia + warm hypotension. ALL other shocks = tachycardia."
    ],
    keyFacts:"Spinal cord injury → loss of sympathetic outflow → ↓SVR AND ↓CO (both low — unique to this type). Bradycardia + warm hypotension (no compensatory tachycardia). Treat: vasopressors + atropine. Only shock where CO and SVR both fall simultaneously."
  },
  {
    id:32, diagnosis:"Pyloric Stenosis",
    aliases:["pyloric stenosis","hypertrophic pyloric stenosis","infantile hypertrophic pyloric stenosis"],
    episode:"Ep. 17 – Pediatrics", topic:"Pediatrics",
    ankiStep1:"Pyloric_stenosis::non-bilious_projectile_vomiting::4-8wk_male::olive_mass",
    ankiStep2:"Pyloric_stenosis::hypochloremic_hypokalemic_alkalosis::Ramstedt::erythromycin_risk",
    clues:[
      "A 4-week-old male infant is brought in for forceful, projectile non-bilious vomiting after every feeding for the past 5 days.",
      "The parents describe the infant as 'hungry' immediately after vomiting — he reaches for the breast right away ('hungry vomiting'). He is losing weight.",
      "You can see visible peristaltic waves moving left to right across the upper abdomen. A firm, olive-shaped mass is palpable in the right upper quadrant.",
      "Labs: metabolic alkalosis (low Cl, low K, high HCO3, high pH) — from repeated loss of HCl in vomit. This is a hypochloremic, hypokalemic metabolic alkalosis.",
      "Abdominal ultrasound confirms hypertrophied pyloric muscle: thickness >3mm, channel length >14mm. The pyloric canal is elongated and narrowed.",
      "Risk factors: firstborn male infant, erythromycin exposure in the first 2 weeks of life (increases pyloric muscle tone). Treatment: Ramstedt pyloromyotomy (surgical). MUST correct electrolyte abnormalities BEFORE surgery — correct the alkalosis, hypokalemia, and dehydration first."
    ],
    keyFacts:"4–8 week male infant, projectile non-bilious vomiting, hungry immediately after ('hungry vomiting'). Olive mass in RUQ. Hypochloremic hypokalemic metabolic alkalosis. US confirms (muscle >3mm). Erythromycin risk. Tx: correct electrolytes first, then Ramstedt pyloromyotomy."
  }
  ,{
    id:33, diagnosis:"Terminal Complement Deficiency",
    aliases:["terminal complement deficiency","complement deficiency","c5-c9 deficiency","mac deficiency"],
    episode:"Ep. 173 – Immunodeficiency", topic:"Immunodeficiency",
    ankiStep1:"Terminal_complement::C5-C9::MAC::Neisseria",
    ankiStep2:"Terminal_complement::recurrent_Neisseria::meningitidis::gonorrhoeae",
    clues:[
      "A 19-year-old college student presents with his third episode of Neisseria meningitidis meningitis in 5 years. He initially recovered well each time with antibiotics.",
      "Review of systems reveals he also had Neisseria gonorrhoeae bacteremia at age 17. He has no HIV risk factors and lives a healthy lifestyle.",
      "CBC, immunoglobulins, and T/B cell counts are all within normal limits. CH50 (total hemolytic complement) assay returns markedly low — nearly undetectable.",
      "Individual complement levels C1–C4 are normal. C5, C6, C7, C8, and C9 levels are found to be severely deficient.",
      "Without C5–C9, the Membrane Attack Complex (MAC) cannot be assembled. MAC is critical for lysing encapsulated gram-negative organisms, particularly Neisseria species.",
      "Management: prophylactic antibiotics, meningococcal and other encapsulated-organism vaccines. These patients are specifically vulnerable to Neisseria meningitidis and Neisseria gonorrhoeae infections."
    ],
    keyFacts:"C5–C9 deficiency → no MAC → recurrent Neisseria (meningitidis + gonorrhoeae). CH50 near zero. Tx: vaccine + prophylactic antibiotics."
  }
  ,{
    id:34, diagnosis:"Acute Retroviral Syndrome",
    aliases:["acute retroviral syndrome","acute hiv","primary hiv infection","acute hiv infection"],
    episode:"Ep. 237 – HIV", topic:"HIV/AIDS",
    ankiStep1:"Acute_HIV::mononucleosis-like::high_viral_load::window_period",
    ankiStep2:"Acute_HIV::p24_antigen::4th_gen_ELISA::NAAT",
    clues:[
      "A 24-year-old man presents with 2 weeks of fever, sore throat, fatigue, myalgias, and a diffuse maculopapular rash covering his trunk. He reports a new sexual partner 3 weeks ago.",
      "Lymph nodes are enlarged in the cervical, axillary, and inguinal regions. He also has oral ulcers. The illness resembles infectious mononucleosis.",
      "Monospot test is negative. EBV and CMV serology are negative. CBC shows a mild lymphopenia.",
      "This presentation occurs 2–4 weeks after HIV exposure and represents the period of highest viral load and greatest transmissibility.",
      "Standard HIV antibody tests (3rd generation) may be negative during this window period. A 4th-generation test (p24 antigen + antibody) or HIV RNA by NAAT will be positive.",
      "Treatment: initiate antiretroviral therapy (ART) as soon as possible. Treating during acute infection reduces reservoir size and limits immune damage."
    ],
    keyFacts:"2–4 weeks post-exposure: mono-like illness + rash. Highest viremia/transmissibility. 3rd-gen test negative (window); use 4th-gen or HIV RNA. Tx: start ART immediately."
  }
  ,{
    id:35, diagnosis:"Cryptococcal Meningitis",
    aliases:["cryptococcal meningitis","cryptococcus meningitis","cryptococcus neoformans meningitis"],
    episode:"Ep. 237 – HIV", topic:"HIV/AIDS",
    ankiStep1:"Cryptococcal_meningitis::CD4<100::india_ink::capsule",
    ankiStep2:"Cryptococcal_meningitis::amphotericin_B::fluconazole::opening_pressure",
    clues:[
      "A 38-year-old HIV-positive man not on ART presents with 3 weeks of progressive headache, photophobia, and neck stiffness. He has been feeling increasingly confused.",
      "His most recent CD4 count was 45 cells/µL. He has no focal neurologic deficits.",
      "LP reveals elevated opening pressure (>25 cmH2O), lymphocytic pleocytosis, low glucose, and high protein. India ink stain of CSF shows encapsulated yeast with a large polysaccharide capsule.",
      "CSF cryptococcal antigen (CrAg) is highly positive. Serum CrAg is also positive. Cultures grow Cryptococcus neoformans.",
      "The polysaccharide capsule of Cryptococcus inhibits phagocytosis and neutrophil recruitment. It's acquired from bird droppings (especially pigeons) in the environment.",
      "Treatment: Induction with Amphotericin B + flucytosine (2 weeks) → Consolidation with fluconazole → Maintenance fluconazole. Serial LPs or lumbar drain for elevated ICP."
    ],
    keyFacts:"CD4<100, HIV. Subacute meningitis + high ICP. India ink: encapsulated yeast. CrAg positive. Tx: AmB+5-FC induction → fluconazole. Manage ICP."
  }
  ,{
    id:36, diagnosis:"CMV Retinitis",
    aliases:["cmv retinitis","cytomegalovirus retinitis","cytomegalovirus eye"],
    episode:"Ep. 237 – HIV", topic:"HIV/AIDS",
    ankiStep1:"CMV_retinitis::CD4<50::pizza_pie::cotton_wool",
    ankiStep2:"CMV_retinitis::ganciclovir::valganciclovir::foscarnet",
    clues:[
      "A 41-year-old man with AIDS (not on ART) presents with 2 weeks of floaters, blurry vision, and a new visual field defect in the right eye. He has no eye pain.",
      "His CD4 count is 28 cells/µL. Visual acuity is 20/60 in the right eye.",
      "Fundoscopic examination shows areas of yellow-white exudates and hemorrhages in a 'pizza pie' or 'crumbled cheese and ketchup' appearance along the retinal vessels.",
      "There is no pain with eye movement (differentiating from optic neuritis). The lesions are perivascular and progress centrifugally.",
      "This is the most common serious ocular opportunistic infection in AIDS, caused by CMV (a herpesvirus). It is sight-threatening and can cause retinal detachment.",
      "Treatment: valganciclovir orally (preferred), or IV ganciclovir for severe cases. Alternative: foscarnet or cidofovir. Must also start/optimize ART."
    ],
    keyFacts:"CD4<50, AIDS. Painless vision loss + floaters. Fundoscopy: 'pizza pie' hemorrhages + exudates. CMV herpesvirus. Tx: valganciclovir/ganciclovir. Start ART."
  }
  ,{
    id:37, diagnosis:"Progressive Multifocal Leukoencephalopathy",
    aliases:["pml","progressive multifocal leukoencephalopathy","jc virus","john cunningham virus"],
    episode:"Ep. 237 – HIV", topic:"HIV/AIDS",
    ankiStep1:"PML::JC_virus::CD4<200::white_matter::no_enhancement",
    ankiStep2:"PML::no_specific_treatment::ART::natalizumab_risk",
    clues:[
      "A 46-year-old HIV-positive man presents with 4 weeks of progressive right-sided weakness, slurred speech, and cognitive decline. No headache or fever.",
      "CD4 count is 62 cells/µL. MRI brain shows multiple, asymmetric, non-enhancing white matter lesions in the subcortical regions, with no mass effect.",
      "Contrast does not enhance the lesions (no blood-brain barrier disruption). The lesions are in the white matter, not the cortex.",
      "CSF PCR is positive for JC virus (John Cunningham polyomavirus). This virus reactivates from latency in immunosuppressed patients.",
      "JC virus infects and destroys oligodendrocytes, causing demyelination. It is a reactivation of a common latent infection (most adults seropositive) — only causes disease in severe immunosuppression.",
      "There is no specific antiviral treatment for PML. The only effective therapy is immune reconstitution via ART. Associated with natalizumab use in MS patients."
    ],
    keyFacts:"CD4<200. Progressive focal neuro deficits. MRI: non-enhancing white matter lesions. JC virus on CSF PCR. Oligodendrocyte destruction → demyelination. Tx: ART only."
  }
  ,{
    id:38, diagnosis:"CNS Toxoplasmosis",
    aliases:["cns toxoplasmosis","cerebral toxoplasmosis","toxoplasma encephalitis","toxoplasmosis brain"],
    episode:"Ep. 237 – HIV", topic:"HIV/AIDS",
    ankiStep1:"Toxoplasmosis::CD4<100::ring_enhancing::sulfadiazine_pyrimethamine",
    ankiStep2:"Toxoplasmosis::cat_feces::reactivation::trimethoprim_prophylaxis",
    clues:[
      "A 35-year-old woman with HIV (CD4=55) presents with 2 weeks of headache, fever, and confusion. She also has left arm weakness that began 3 days ago.",
      "She owns several cats and often gardens. CT head shows multiple ring-enhancing lesions in the basal ganglia and cortex, with surrounding edema.",
      "MRI confirms multiple ring-enhancing lesions at the gray-white matter junction, particularly in the basal ganglia. There is surrounding vasogenic edema.",
      "Toxoplasma gondii serology (IgG) is positive, consistent with reactivation of prior infection in the setting of severe immunosuppression.",
      "The empiric treatment approach is tried first — if lesions improve after 2 weeks of anti-toxoplasma therapy, the diagnosis is confirmed. If no improvement, brain biopsy is performed.",
      "Treatment: sulfadiazine + pyrimethamine + leucovorin (folinic acid to prevent bone marrow suppression). Prophylaxis: TMP-SMX when CD4 <100."
    ],
    keyFacts:"CD4<100, HIV. Ring-enhancing lesions (basal ganglia). Cat/soil exposure → Toxoplasma reactivation. Treat empirically first (sulfadiazine+pyrimethamine+leucovorin). Prophylaxis: TMP-SMX."
  }
  ,{
    id:39, diagnosis:"Kaposi Sarcoma",
    aliases:["kaposi sarcoma","kaposi's sarcoma","ks","hiv kaposi"],
    episode:"Ep. 237 – HIV", topic:"HIV/AIDS",
    ankiStep1:"Kaposi_sarcoma::HHV8::AIDS_defining::violaceous_lesions",
    ankiStep2:"Kaposi_sarcoma::ART::liposomal_doxorubicin::CD4<200",
    clues:[
      "A 32-year-old HIV-positive man (CD4=88) presents with multiple painless, dark purple-red raised lesions on his skin — on his legs, face, and the roof of his mouth.",
      "The lesions do not blanch with pressure. He also reports blood in his stool and occasional shortness of breath.",
      "Biopsy of a skin lesion reveals spindle-shaped cells with slit-like vascular spaces and lymphocyte infiltration, consistent with a vascular neoplasm.",
      "The lesion biopsy stains positive for HHV-8 (Human Herpesvirus 8 / Kaposi Sarcoma-associated Herpesvirus, KSHV).",
      "This is an AIDS-defining malignancy. It can involve skin, mucous membranes, lungs (causing pleural effusions), and GI tract. Pulmonary KS causes hemoptysis and respiratory failure.",
      "Treatment: the most important intervention is starting/optimizing ART to boost CD4. Localized disease: radiation or topical alitretinoin. Systemic disease: liposomal doxorubicin."
    ],
    keyFacts:"HHV-8 (KSHV) + AIDS. Violaceous skin/mucosal lesions. Spindle cells on biopsy. AIDS-defining malignancy. Tx: ART (primary); liposomal doxorubicin for systemic disease."
  }
  ,{
    id:40, diagnosis:"Wernicke-Korsakoff Syndrome",
    aliases:["wernicke korsakoff","wernicke's encephalopathy","korsakoff psychosis","wernicke encephalopathy","thiamine deficiency encephalopathy"],
    episode:"Ep. 243 – Water Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"Wernicke::B1_thiamine::triad::mammillary_bodies",
    ankiStep2:"Wernicke::IV_thiamine_before_glucose::Korsakoff::confabulation",
    clues:[
      "A 52-year-old alcoholic man is brought to the ER confused and unsteady. Nursing staff note his eyes are moving abnormally — one eye is deviated inward.",
      "The classic triad is identified: confusion (encephalopathy), gait ataxia, and ophthalmoplegia (nystagmus, lateral gaze palsy, CN VI palsy).",
      "He is given IV dextrose in the ER before any other interventions, and his condition acutely worsens with new seizure activity.",
      "This deterioration occurred because glucose administration depletes remaining thiamine stores, precipitating acute Wernicke's. Thiamine MUST be given before glucose.",
      "MRI reveals hemorrhagic lesions in the mammillary bodies and periaqueductal gray matter — the hallmark imaging finding. PET shows decreased metabolic activity in these regions.",
      "If untreated, Wernicke's progresses to Korsakoff psychosis (permanent): anterograde amnesia + confabulation (making up stories to fill memory gaps). Treatment: IV thiamine immediately, then oral supplementation."
    ],
    keyFacts:"Thiamine (B1) deficiency in alcoholics. Triad: confusion+ophthalmoplegia+ataxia. Mammillary body/periaqueductal gray lesions. Give IV thiamine BEFORE glucose. Korsakoff: anterograde amnesia+confabulation."
  }
  ,{
    id:41, diagnosis:"Wet Beriberi",
    aliases:["wet beriberi","beriberi","cardiac beriberi","thiamine deficiency heart failure"],
    episode:"Ep. 243 – Water Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"Wet_beriberi::B1_thiamine::high_output_heart_failure::dilated_cardiomyopathy",
    ankiStep2:"Wet_beriberi::edema::alcoholic::IV_thiamine",
    clues:[
      "A 45-year-old chronic alcoholic presents with progressive leg swelling, shortness of breath on exertion, and fatigue for 3 months. He eats very little.",
      "On exam: bounding pulses, elevated JVP, bilateral pitting edema to the knees, S3 gallop, and cardiomegaly. Extremities are warm.",
      "Echocardiogram shows dilated cardiomyopathy with reduced ejection fraction (EF 25%). Cardiac output is paradoxically elevated (high-output state).",
      "Despite the high cardiac output, the heart is failing because of massive peripheral vasodilation. Tissues are extracting more oxygen — mixed venous O2 saturation is elevated.",
      "The cause is thiamine (B1) deficiency, impairing ATP production in cardiac muscle. B1 is essential for pyruvate dehydrogenase and the citric acid cycle.",
      "Treatment: IV thiamine leads to dramatic improvement. This form (wet beriberi) involves cardiac failure; dry beriberi involves peripheral neuropathy. Both from B1 deficiency."
    ],
    keyFacts:"B1 (thiamine) deficiency. High-output heart failure + dilated cardiomyopathy. Bounding pulses, warm extremities. Alcoholics/malnutrition. Tx: IV thiamine → dramatic improvement."
  }
  ,{
    id:42, diagnosis:"Pellagra",
    aliases:["pellagra","niacin deficiency","b3 deficiency","nicotinic acid deficiency"],
    episode:"Ep. 243 – Water Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"Pellagra::niacin_B3::4Ds::Hartnup_disease",
    ankiStep2:"Pellagra::isoniazid::carcinoid::niacin_supplementation",
    clues:[
      "A refugee from a corn-dependent region presents with a symmetric, hyperpigmented, scaly rash on sun-exposed areas of his neck (Casal's necklace), forearms, and hands.",
      "He also complains of persistent diarrhea for 2 months and has become increasingly forgetful and confused. His diet consists almost entirely of corn (maize).",
      "The four D's are identified: Dermatitis (sun-sensitive, symmetric), Diarrhea, Dementia (confusion, memory loss), and Death if untreated.",
      "Corn lacks tryptophan and contains niacin in a bound, unabsorbable form (niacytin). This causes niacin (B3) deficiency. Tryptophan is also needed to synthesize niacin endogenously.",
      "Other causes: isoniazid (INH) therapy — INH depletes B6, which is needed to convert tryptophan→niacin. Carcinoid tumor — tryptophan is shunted to make serotonin instead.",
      "Treatment: niacin (nicotinic acid) supplementation. The rash, diarrhea, and dementia resolve. Hartnup disease (defective tryptophan absorption) causes pellagra-like symptoms."
    ],
    keyFacts:"Niacin (B3) deficiency. 4 D's: Dermatitis+Diarrhea+Dementia+Death. Corn-based diets (niacytin inabsorbable). Also: INH, carcinoid. Casal's necklace rash. Tx: niacin."
  }
  ,{
    id:43, diagnosis:"Scurvy",
    aliases:["scurvy","vitamin c deficiency","ascorbic acid deficiency"],
    episode:"Ep. 243 – Water Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"Scurvy::vitamin_C::collagen::perifollicular_hemorrhage",
    ankiStep2:"Scurvy::corkscrew_hair::bleeding_gums::poor_wound_healing",
    clues:[
      "A 68-year-old man who lives alone and 'eats mostly crackers and canned soup' presents with gum bleeding, loose teeth, and poor wound healing after a minor cut 3 weeks ago.",
      "Exam shows swollen, bleeding gums, perifollicular hemorrhages (bleeding around hair follicles), and corkscrew (coiled) hairs on his legs. Old scars have broken down.",
      "He also has hemarthroses (bleeding into joints), causing painful swollen knees. His skin shows easy bruising and purpura.",
      "Vitamin C (ascorbic acid) is required for hydroxylation of proline and lysine in collagen synthesis. Without it, collagen is structurally weak — leading to vascular fragility, poor wound healing, and bleeding.",
      "Risk groups: elderly living alone, alcoholics, anyone with poor diet (no fruits/vegetables). X-ray may show periosteal hemorrhage in children causing 'Trummerfeld zone'.",
      "Treatment: vitamin C supplementation (oral). Symptoms resolve within days to weeks. The 'sailor's disease' — historically caused by no fresh food on long voyages."
    ],
    keyFacts:"Vitamin C deficiency. Collagen synthesis failure → bleeding gums, perifollicular hemorrhage, corkscrew hairs, poor wound healing, old scar breakdown. Elderly/alcoholics. Tx: vitamin C."
  }
  ,{
    id:44, diagnosis:"Folate Deficiency",
    aliases:["folate deficiency","folic acid deficiency","megaloblastic anemia folate"],
    episode:"Ep. 243 – Water Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"Folate::megaloblastic::neural_tube::no_neuro_symptoms",
    ankiStep2:"Folate::methotrexate::phenytoin::pregnancy::leucovorin",
    clues:[
      "A 29-year-old pregnant woman at 8 weeks gestation presents for prenatal care. She admits she did not take prenatal vitamins before conception.",
      "CBC shows macrocytic anemia (MCV 108 fL), hypersegmented neutrophils, and normal platelets. She reports fatigue and mild glossitis.",
      "Serum folate is low. Serum B12 level is normal. Homocysteine is elevated; methylmalonic acid (MMA) is normal.",
      "The normal MMA distinguishes folate deficiency from B12 deficiency (B12 deficiency causes elevated MMA + homocysteine; folate deficiency causes only elevated homocysteine).",
      "Importantly, there are NO neurological symptoms (subacute combined degeneration does NOT occur in folate deficiency). Neural tube defects (spina bifida, anencephaly) occur in the fetus if folate is deficient periconceptionally.",
      "Treatment: folic acid supplementation. All women of childbearing age should take 400–800 mcg/day. Drugs that cause folate deficiency: methotrexate, phenytoin, trimethoprim. Antidote: leucovorin."
    ],
    keyFacts:"Folate deficiency → megaloblastic anemia. High homocysteine, normal MMA (distinguish from B12). NO neuro symptoms. Neural tube defects in fetus. Drugs: MTX, phenytoin. Tx: folic acid."
  }
  ,{
    id:45, diagnosis:"Graves' Disease",
    aliases:["graves disease","graves' disease","toxic diffuse goiter","autoimmune hyperthyroidism"],
    episode:"Ep. 251 – Thyroid", topic:"Thyroid",
    ankiStep1:"Graves::TSH_receptor_antibody::exophthalmos::pretibial_myxedema",
    ankiStep2:"Graves::PTU::methimazole::radioactive_iodine::thyroidectomy",
    clues:[
      "A 28-year-old woman presents with 4 months of weight loss despite increased appetite, heat intolerance, palpitations, and anxiety. She also notes her eyes appear to be 'bulging out.'",
      "Exam: diffuse smooth goiter, resting tremor, warm moist skin, tachycardia (HR 108), brisk reflexes, and bilateral proptosis (exophthalmos). Pretibial myxedema is noted on her shins.",
      "TSH is undetectable (<0.01). Free T4 and T3 are elevated. Radioactive iodine uptake (RAIU) scan shows diffusely increased uptake throughout an enlarged gland.",
      "TSH-receptor stimulating antibodies (TRAb/TSI) are positive — the pathognomonic antibody. These IgG antibodies mimic TSH, constitutively activating thyroid hormone production.",
      "Exophthalmos is caused by TSH-receptor antibodies reacting with orbital fibroblasts, causing glycosaminoglycan deposition and inflammation behind the eyes — it persists even after thyroid treatment.",
      "Treatment options: methimazole (preferred over PTU — less hepatotoxicity; use PTU in 1st trimester), radioactive iodine (I-131), or thyroidectomy. Beta-blockers for symptomatic control."
    ],
    keyFacts:"TSH-receptor stimulating antibodies (TRAb/TSI). Diffuse goiter + hyperthyroid + exophthalmos + pretibial myxedema. Diffuse RAIU uptake. Tx: methimazole/PTU, RAI, or thyroidectomy."
  }
  ,{
    id:46, diagnosis:"Hashimoto's Thyroiditis",
    aliases:["hashimoto thyroiditis","hashimoto's thyroiditis","chronic lymphocytic thyroiditis","autoimmune hypothyroidism"],
    episode:"Ep. 251 – Thyroid", topic:"Thyroid",
    ankiStep1:"Hashimoto::anti-TPO::anti-thyroglobulin::Hurthle_cells",
    ankiStep2:"Hashimoto::hypothyroid::levothyroxine::lymphoma_risk",
    clues:[
      "A 42-year-old woman presents with fatigue, weight gain, cold intolerance, constipation, dry skin, and hair loss for 8 months. Her mother has the same condition.",
      "Exam: firm, non-tender, 'rubbery' goiter. Bradycardia (HR 56), delayed relaxation of deep tendon reflexes, periorbital puffiness, and coarse dry hair.",
      "TSH is elevated (12.4). Free T4 is low. Anti-TPO (anti-thyroid peroxidase) antibodies are markedly elevated. Anti-thyroglobulin antibodies are also positive.",
      "Thyroid biopsy (if done) would show lymphocytic infiltration with germinal center formation and Hürthle cell (oxyphilic) metaplasia — the hallmark pathology.",
      "This is the most common cause of hypothyroidism in iodine-sufficient countries. It is an autoimmune destruction of thyroid tissue. Associated with other autoimmune diseases.",
      "Treatment: levothyroxine (synthetic T4) lifelong. Target TSH 0.5–2.5. Increased risk of thyroid lymphoma (primary lymphoma of the thyroid) compared to general population."
    ],
    keyFacts:"Most common hypothyroid cause (iodine-sufficient). Anti-TPO + anti-thyroglobulin antibodies. Hürthle cells. Rubbery goiter. Tx: levothyroxine. Risk: thyroid lymphoma."
  }
  ,{
    id:47, diagnosis:"De Quervain's Thyroiditis",
    aliases:["de quervain thyroiditis","subacute granulomatous thyroiditis","painful thyroiditis","subacute thyroiditis"],
    episode:"Ep. 251 – Thyroid", topic:"Thyroid",
    ankiStep1:"DeQuervain::painful_thyroid::post_viral::ESR_elevated::self_limited",
    ankiStep2:"DeQuervain::transient_hyperthyroid_then_hypothyroid::NSAIDs::low_RAIU",
    clues:[
      "A 35-year-old woman presents with 3 weeks of anterior neck pain that radiates to the jaw and ear, following a viral upper respiratory infection 4 weeks ago.",
      "The thyroid gland is exquisitely tender on palpation and firm. She also has symptoms of hyperthyroidism: palpitations, sweating, and weight loss.",
      "TSH is suppressed, free T4 is elevated. ESR is dramatically elevated (>100 mm/hr). WBC is mildly elevated. Radioactive iodine uptake (RAIU) is nearly zero.",
      "The near-zero RAIU is key: inflammation destroys follicular cells, releasing preformed thyroid hormone into the blood. The gland is not actively making new hormone (hence no iodine uptake).",
      "The clinical course is classic: hyperthyroid phase (weeks) → hypothyroid phase (weeks to months) → return to euthyroid. Most patients (95%) recover fully.",
      "Treatment: NSAIDs for pain and inflammation. Steroids for severe cases. Beta-blockers for hyperthyroid symptoms. Thyroid hormone replacement if hypothyroid phase is prolonged."
    ],
    keyFacts:"Post-viral painful tender thyroid. Elevated ESR. Transient hyperthyroid→hypothyroid→euthyroid. Near-zero RAIU (preformed hormone leaks). Tx: NSAIDs ± steroids. Self-limited."
  }
  ,{
    id:48, diagnosis:"Thyroid Storm",
    aliases:["thyroid storm","thyrotoxic crisis","thyroid crisis"],
    episode:"Ep. 251 – Thyroid", topic:"Thyroid",
    ankiStep1:"Thyroid_storm::life_threatening::precipitant::PTU_then_iodine",
    ankiStep2:"Thyroid_storm::Burch_Wartofsky::propranolol::hydrocortisone::cholestyramine",
    clues:[
      "A 55-year-old woman with known but untreated Graves' disease is admitted after an emergency appendectomy. 12 hours post-op she develops HR 165, fever 40.2°C, agitation, and confusion.",
      "She vomits repeatedly and becomes increasingly delirious. BP is 160/90. She has a diffuse goiter and prominent exophthalmos on exam.",
      "TSH is undetectable, free T4 is extremely elevated. The Burch-Wartofsky scoring system (>45 points) confirms thyroid storm.",
      "The precipitant was surgery (physiologic stress). Other precipitants: infection, trauma, radioactive iodine treatment in undertreated patients, or contrast dye.",
      "The treatment order matters critically: (1) PTU — blocks new thyroid synthesis AND peripheral T4→T3 conversion. (2) Then iodine (Lugol's/SSKI) — given 1 hour AFTER PTU to block hormone release (Wolff-Chaikoff). (3) Propranolol — blocks peripheral effects. (4) Hydrocortisone — prevents adrenal crisis and blocks T4→T3. (5) Cholestyramine — binds thyroid hormone in gut.",
      "Mortality is 10–30% even with treatment. ICU admission essential. Cooling blankets for fever. Avoid aspirin (displaces T4 from binding proteins)."
    ],
    keyFacts:"Life-threatening hyperthyroid crisis precipitated by stress/surgery/infection. Tx sequence: PTU → iodine (1hr later) → propranolol → hydrocortisone → cholestyramine. ICU. Avoid aspirin."
  }
  ,{
    id:49, diagnosis:"Papillary Thyroid Cancer",
    aliases:["papillary thyroid cancer","papillary thyroid carcinoma","ptc"],
    episode:"Ep. 251 – Thyroid", topic:"Thyroid",
    ankiStep1:"Papillary_thyroid::BRAF::orphan_annie_nuclei::psammoma_bodies",
    ankiStep2:"Papillary_thyroid::radiation_exposure::lymphatic_spread::best_prognosis",
    clues:[
      "A 34-year-old woman who received radiation to the neck for Hodgkin's lymphoma at age 10 presents with a painless thyroid nodule discovered incidentally on ultrasound.",
      "Ultrasound shows a 2.1 cm hypoechoic nodule with microcalcifications and irregular borders. Fine needle aspiration (FNA) biopsy is performed.",
      "FNA cytology shows 'Orphan Annie eye' nuclei (empty-looking, ground-glass nuclei), nuclear grooves, and psammoma bodies (concentric calcifications).",
      "This is the most common thyroid cancer (80% of cases). Key mutation: BRAF V600E (most common) or RET/PTC rearrangements. Associated with prior radiation exposure.",
      "It spreads via lymphatics to cervical lymph nodes (not hematogenously). Despite lymph node involvement, prognosis remains excellent. It is multifocal in 20–80% of cases.",
      "Treatment: total thyroidectomy (hemithyroidectomy if <1cm). Radioactive iodine (I-131) ablation for remnant/metastatic disease. TSH suppression with levothyroxine. 10-year survival >95%."
    ],
    keyFacts:"Most common thyroid cancer. Radiation risk. Orphan Annie nuclei + psammoma bodies on FNA. BRAF mutation. Lymphatic spread. Excellent prognosis. Tx: thyroidectomy ± RAI."
  }
  ,{
    id:50, diagnosis:"Medullary Thyroid Cancer",
    aliases:["medullary thyroid cancer","medullary thyroid carcinoma","mtc","calcitonin secreting thyroid"],
    episode:"Ep. 251 – Thyroid", topic:"Thyroid",
    ankiStep1:"Medullary_thyroid::calcitonin::RET_mutation::C_cells::MEN2",
    ankiStep2:"Medullary_thyroid::amyloid_stroma::CEA::screen_family::adrenalectomy_first",
    clues:[
      "A 45-year-old man presents with a thyroid nodule and a family history of thyroid cancer. His sister had both a thyroid tumor and bilateral adrenal tumors.",
      "Serum calcitonin is markedly elevated. CEA is also elevated. Ultrasound shows a solid thyroid nodule in the upper pole. RET proto-oncogene mutation testing is positive.",
      "FNA biopsy shows sheets of spindle or polygonal cells with amyloid deposits in the stroma (Congo red positive). No follicles or papillae are seen.",
      "This cancer arises from parafollicular C-cells (not follicular epithelium), which normally secrete calcitonin. It does NOT take up radioactive iodine.",
      "Associated with MEN2A (medullary thyroid Ca + pheochromocytoma + parathyroid hyperplasia) and MEN2B (medullary thyroid Ca + pheochromocytoma + mucosal neuromas + marfanoid habitus).",
      "Treatment: total thyroidectomy. Must rule out pheochromocytoma FIRST (adrenalectomy before thyroidectomy if pheochromocytoma present, to prevent hypertensive crisis). Screen all first-degree relatives with RET testing."
    ],
    keyFacts:"Parafollicular C-cells. Calcitonin elevated. RET mutation. Amyloid stroma. MEN2A/2B. No RAI uptake. Rule out pheo FIRST before surgery. Screen family with RET testing."
  }
  ,{
    id:51, diagnosis:"Anaplastic Thyroid Cancer",
    aliases:["anaplastic thyroid cancer","anaplastic thyroid carcinoma","undifferentiated thyroid cancer"],
    episode:"Ep. 251 – Thyroid", topic:"Thyroid",
    ankiStep1:"Anaplastic_thyroid::worst_prognosis::rapidly_growing::TP53::elderly",
    ankiStep2:"Anaplastic_thyroid::local_invasion::dysphagia_stridor::palliative",
    clues:[
      "A 72-year-old woman presents with a rapidly growing, rock-hard neck mass that has doubled in size in 3 weeks. She has dysphagia, hoarseness, and inspiratory stridor.",
      "Exam shows a fixed, stony-hard mass that appears to invade surrounding structures. CT reveals the mass encasing the trachea and esophagus with bilateral lymphadenopathy.",
      "Biopsy shows highly pleomorphic cells with frequent mitoses, necrosis, and giant cells. No follicular architecture is present.",
      "This is the most aggressive and deadly thyroid cancer. Median survival is 5–6 months. It is invariably classified as Stage IV at diagnosis due to its aggressive behavior.",
      "It arises from dedifferentiation of well-differentiated thyroid cancer (often papillary or follicular). TP53 mutation is frequently involved. It does NOT take up RAI.",
      "Treatment is mostly palliative: external beam radiation + chemotherapy (dabrafenib+trametinib if BRAF-positive). Airway protection may require tracheostomy."
    ],
    keyFacts:"Worst prognosis thyroid cancer. Elderly women. Rapidly growing rock-hard mass. Invades trachea/esophagus. TP53 mutation. No RAI uptake. Stage IV at diagnosis. Palliative treatment."
  }
  ,{
    id:52, diagnosis:"Latent Tuberculosis",
    aliases:["latent tb","latent tuberculosis","ltbi","latent tuberculous infection"],
    episode:"Ep. 262 – Tuberculosis", topic:"Pulmonology",
    ankiStep1:"Latent_TB::positive_TST::no_symptoms::isoniazid_9_months",
    ankiStep2:"Latent_TB::IGRA::no_cough::chest_xray_normal::reactivation_risk",
    clues:[
      "A 35-year-old immigrant from Southeast Asia applies for a job at a hospital. Routine pre-employment screening includes tuberculin skin test (TST/PPD).",
      "The TST is read at 48–72 hours: 15 mm of induration. He is completely asymptomatic — no cough, night sweats, weight loss, or fever. He feels entirely well.",
      "Chest X-ray is normal. Sputum AFB smear and culture (if sent) are negative. An IGRA (QuantiFERON-TB Gold) test is also positive.",
      "This represents latent TB infection — the bacteria are walled off in granulomas but not actively replicating. The host immune system (primarily CD4 T-cells and macrophages) is keeping the infection controlled.",
      "Reactivation risk is highest with: HIV (most important), TNF-alpha inhibitors (biologics), diabetes, malnutrition, silicosis, end-stage renal disease, and malignancy.",
      "Treatment: isoniazid (INH) for 9 months (preferred), or rifampin for 4 months, or INH+rifapentine weekly for 3 months (3HP). Give pyridoxine (B6) with INH to prevent neuropathy."
    ],
    keyFacts:"Positive TST/IGRA, no symptoms, normal CXR. Granulomas wall off bacteria. Reactivation risks: HIV, TNF-alpha inhibitors. Tx: INH × 9 months + B6 (pyridoxine)."
  }
  ,{
    id:53, diagnosis:"Active Pulmonary Tuberculosis",
    aliases:["active tuberculosis","active pulmonary tb","pulmonary tuberculosis","tb disease"],
    episode:"Ep. 262 – Tuberculosis", topic:"Pulmonology",
    ankiStep1:"Active_TB::RIPE::upper_lobe_cavitation::AFB_smear::acid_fast",
    ankiStep2:"Active_TB::night_sweats::hemoptysis::Ghon_complex::DOT",
    clues:[
      "A 48-year-old homeless man presents with 3 months of productive cough, night sweats, unintentional 15 lb weight loss, and hemoptysis. He has no health insurance.",
      "Exam shows a thin, febrile man with dullness to percussion and decreased breath sounds at the right upper lobe. Sputum is blood-tinged.",
      "Chest X-ray: right upper lobe cavitary lesion with surrounding infiltrate. Upper lobe involvement is classic — better ventilation and higher O2 tension favors mycobacterial growth.",
      "Sputum AFB smear is positive for acid-fast bacilli (AFB). Confirmatory culture grows Mycobacterium tuberculosis over 6–8 weeks. Nucleic acid amplification test (NAAT) can give faster results.",
      "TB spreads person-to-person via respiratory droplets. Primary infection: Ghon complex (subpleural lesion + hilar lymphadenopathy). Most resolve; ~5% progress to active disease.",
      "Treatment: RIPE therapy — Rifampin, Isoniazid, Pyrazinamide, Ethambutol × 2 months, then Rifampin + Isoniazid × 4 months. DOT (directly observed therapy) ensures adherence. Give B6 with INH."
    ],
    keyFacts:"Cough + night sweats + weight loss + hemoptysis. Upper lobe cavitary lesion on CXR. AFB smear positive. Tx: RIPE × 2 months then RI × 4 months. DOT. B6 with INH."
  }
  ,{
    id:54, diagnosis:"TB Meningitis",
    aliases:["tb meningitis","tuberculous meningitis","tuberculosis meningitis"],
    episode:"Ep. 262 – Tuberculosis", topic:"Pulmonology",
    ankiStep1:"TB_meningitis::basilar::CN_palsies::lymphocytic_CSF::very_low_glucose",
    ankiStep2:"TB_meningitis::RIPE_plus_steroids::hydrocephalus::SIADH",
    clues:[
      "A 29-year-old Somali refugee presents with 3 weeks of progressive headache, fever, neck stiffness, and new double vision.",
      "Exam reveals CN VI palsy bilaterally (eyes cannot abduct), CN III palsy on the left, and papilledema. He is lethargic but arousable.",
      "LP: opening pressure 30 cmH2O, WBC 220 (80% lymphocytes), protein 150 mg/dL, glucose 20 mg/dL (serum glucose 90) — very low CSF glucose (ratio <0.3) is characteristic.",
      "The CSF glucose is strikingly low because M. tuberculosis consumes glucose and the inflammatory response impairs glucose transport across the blood-brain barrier.",
      "Basilar meningitis causes cranial nerve palsies (especially CN III, IV, VI) and hydrocephalus due to obstruction of CSF flow at the base of the brain. Vasculitis causes strokes.",
      "Treatment: RIPE regimen + dexamethasone (steroids reduce mortality and neurologic sequelae). Treat for 12 months total. Monitor for hydrocephalus requiring ventriculoperitoneal shunt."
    ],
    keyFacts:"Basilar meningitis. CN palsies + hydrocephalus. CSF: lymphocytes + very low glucose + high protein. AFB/NAAT on CSF. Tx: RIPE + dexamethasone × 12 months."
  }
  ,{
    id:55, diagnosis:"Pott's Disease",
    aliases:["pott's disease","potts disease","vertebral tuberculosis","tb spine","tuberculous spondylitis"],
    episode:"Ep. 262 – Tuberculosis", topic:"Pulmonology",
    ankiStep1:"Potts::vertebral_TB::thoracic_spine::gibbus_deformity::psoas_abscess",
    ankiStep2:"Potts::cold_abscess::anterior_vertebral::cord_compression::RIPE",
    clues:[
      "A 38-year-old Haitian man with a history of pulmonary TB (treated 10 years ago) presents with 6 months of progressive back pain and a new inability to walk.",
      "Exam reveals a kyphotic deformity ('gibbus') of the mid-thoracic spine. Lower extremity weakness and hyperreflexia suggest spinal cord compression.",
      "MRI spine: destruction of two adjacent vertebral bodies (T8–T9) with loss of the intervertebral disc space. There is a paravertebral fluid collection extending into the psoas muscle.",
      "The paravertebral collection is a 'cold abscess' — it contains caseous necrotic material but lacks the warmth, redness, or acute inflammation of a pyogenic abscess.",
      "TB spine characteristically affects the anterior vertebral bodies. The disc space is lost (unlike metastatic cancer which preserves it). Organisms spread hematogenously from a distant focus.",
      "Treatment: RIPE regimen for 12 months. Surgical decompression if neurological compromise is present or progresses. Drain cold abscesses if necessary."
    ],
    keyFacts:"Vertebral TB (thoracic most common). Adjacent vertebral body destruction + disc space loss. Gibbus deformity. Cold psoas abscess. Cord compression. Tx: RIPE × 12 months ± surgery."
  }
  ,{
    id:56, diagnosis:"Vitamin A Deficiency",
    aliases:["vitamin a deficiency","retinol deficiency","xerophthalmia","night blindness vitamin a"],
    episode:"Ep. 281 – Fat Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"Vitamin_A::night_blindness::xerophthalmia::Bitot_spots::rhodopsin",
    ankiStep2:"Vitamin_A::measles::fat_malabsorption::keratomalacia::teratogenic_high_dose",
    clues:[
      "A 4-year-old boy from rural Bangladesh presents with difficulty seeing in dim light and a dry, hazy cornea. His mother says he cannot see after dark.",
      "Exam shows conjunctival dryness and triangular, foamy, silvery-white deposits on the temporal bulbar conjunctiva (Bitot's spots). The cornea is dry and lustreless (xerophthalmia).",
      "If untreated, the cornea will ulcerate and liquefy (keratomalacia), causing permanent blindness. This is the leading cause of preventable blindness in children in the developing world.",
      "Vitamin A (retinol) is required for rhodopsin synthesis in rod photoreceptors (night vision), maintaining epithelial integrity, and immune function.",
      "Risk factors: malnutrition, fat malabsorption (Crohn's, celiac, cystic fibrosis), measles infection (measles → vitamin A supplementation recommended to reduce mortality).",
      "Treatment: high-dose vitamin A supplementation. Note: excess vitamin A is teratogenic (causes fetal cranial defects) and toxic (pseudotumor cerebri, hepatotoxicity, bone resorption)."
    ],
    keyFacts:"Retinol deficiency. Night blindness → Bitot's spots → xerophthalmia → keratomalacia (blindness). Rhodopsin synthesis impaired. Measles → supplement. High dose = teratogenic/toxic."
  }
  ,{
    id:57, diagnosis:"Rickets",
    aliases:["rickets","vitamin d deficiency children","nutritional rickets"],
    episode:"Ep. 281 – Fat Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"Rickets::vitamin_D3::children::bowlegs::rachitic_rosary",
    ankiStep2:"Rickets::low_calcium::hypophosphatemia::craniotabes::Harrison_groove",
    clues:[
      "A 2-year-old exclusively breastfed child living in northern Canada presents with bowed legs (genu varum) and delayed closure of the anterior fontanelle.",
      "Exam shows softening of the skull (craniotabes — skull feels like a ping-pong ball when pressed), widening of the wrists and ankles, and 'rachitic rosary' (beading at the costochondral junctions).",
      "A horizontal groove along the lower chest (Harrison's groove) marks the site of diaphragmatic pull on softened ribs. X-rays show cupped, frayed ('paintbrush') metaphyses and widened growth plates.",
      "Labs: low serum calcium, low phosphate, elevated alkaline phosphatase, elevated PTH (secondary hyperparathyroidism), and low 25-OH vitamin D.",
      "Vitamin D3 (cholecalciferol) is made in skin by UV light and converted in liver (25-OH) then kidney (1,25-OH, calcitriol). Calcitriol promotes intestinal Ca/PO4 absorption and bone mineralization.",
      "Treatment: vitamin D and calcium supplementation. Breastfed infants require vitamin D drops (400 IU/day). Also seen in fat malabsorption, renal disease, or liver disease."
    ],
    keyFacts:"Vitamin D deficiency in children. Bowed legs, rachitic rosary, craniotabes, Harrison's groove. Paintbrush metaphyses on XR. Low Ca+PO4, high ALP+PTH. Tx: vitamin D + Ca."
  }
  ,{
    id:58, diagnosis:"Vitamin K Deficiency",
    aliases:["vitamin k deficiency","vit k deficiency","hemorrhagic disease of newborn"],
    episode:"Ep. 281 – Fat Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"Vitamin_K::clotting_factors::prolonged_PT::hemorrhagic_newborn",
    ankiStep2:"Vitamin_K::warfarin_reversal::fat_malabsorption::2_3_7_9_10_PC_PS",
    clues:[
      "A 3-day-old exclusively breastfed newborn presents with bleeding from the umbilical stump, blood in the diaper (hematuria vs GI blood), and prolonged bleeding from the heel-stick site.",
      "The infant's mother refused the vitamin K injection at birth. PT is markedly prolonged. PTT is also prolonged. Platelet count and fibrinogen are normal.",
      "Breast milk is low in vitamin K. Neonatal gut flora (which produce vitamin K) are not yet established. The blood-brain barrier is immature, placing the infant at risk for intracranial hemorrhage.",
      "Vitamin K is required for gamma-carboxylation of coagulation factors II, VII, IX, X (and anticoagulant proteins C and S). Without it, these factors are non-functional.",
      "Warfarin also works by blocking vitamin K epoxide reductase, preventing recycling of vitamin K. Broad-spectrum antibiotics kill gut flora that produce vitamin K (acquired deficiency in adults).",
      "Treatment: vitamin K injection (prophylactic at birth is standard of care). In bleeding: IV vitamin K. For warfarin reversal: IV vitamin K + 4-factor PCC or FFP for immediate reversal."
    ],
    keyFacts:"Factors II, VII, IX, X (+ Prot C/S) require vitamin K. Prolonged PT+PTT, normal platelets. Newborn: breast milk low + no gut flora. Warfarin mechanism. Tx: vitamin K injection."
  }
  ,{
    id:59, diagnosis:"Delirium Tremens",
    aliases:["delirium tremens","dt","alcohol withdrawal delirium","severe alcohol withdrawal"],
    episode:"Ep. 289 – Alcoholic Liver Disease", topic:"Gastroenterology",
    ankiStep1:"DTs::72_hours::seizures::hallucinations::benzodiazepines",
    ankiStep2:"DTs::GABA_B_up_glutamate_N_down::autonomic_instability::ICU",
    clues:[
      "A 52-year-old chronic alcoholic is admitted for pneumonia. On hospital day 3, nursing staff find him combative, confused, and trembling violently. He is diaphoretic with a HR of 148.",
      "He is hallucinating — seeing snakes crawling on the walls (visual hallucinations) and hearing voices. His BP is 180/105. Temperature is 38.9°C.",
      "This occurs 48–96 hours after the last drink — the dangerous peak of alcohol withdrawal. Minor withdrawal (tremors, anxiety) begins at 6–12 hours; seizures at 12–48 hours.",
      "The pathophysiology: chronic alcohol use upregulates GABA-B receptors (causing tolerance) and downregulates glutamate (NMDA) receptors. On withdrawal, relative GABA deficiency and glutamate excess → hyperexcitability.",
      "Without treatment, mortality from DTs is ~35%. With treatment (ICU, benzos), mortality drops to ~5%. Complications: hyperthermia, rhabdomyolysis, arrhythmias, aspiration.",
      "Treatment: benzodiazepines (lorazepam or diazepam) IV — GABA agonists replace alcohol's GABA effect. CIWA protocol guides dosing. ICU admission. Correct thiamine (before glucose), electrolytes, and hydration."
    ],
    keyFacts:"Alcohol withdrawal at 48–96 hours. Autonomic instability + visual hallucinations + tremors + seizures. GABA down, glutamate up. Tx: benzodiazepines (CIWA protocol), thiamine, ICU."
  }
  ,{
    id:60, diagnosis:"Fetal Alcohol Syndrome",
    aliases:["fetal alcohol syndrome","fas","fetal alcohol spectrum disorder","fasd"],
    episode:"Ep. 289 – Alcoholic Liver Disease", topic:"Gastroenterology",
    ankiStep1:"FAS::alcohol_teratogen::smooth_philtrum::short_palpebral::IUGR",
    ankiStep2:"FAS::thin_upper_lip::microcephaly::intellectual_disability::no_safe_dose",
    clues:[
      "A pediatrician evaluates a 3-year-old girl for developmental delay and behavioral problems. Her adoptive parents report she was born to a mother with known alcohol use during pregnancy.",
      "Exam shows a child below the 5th percentile for height and weight. Dysmorphic features: smooth philtrum (absent groove between nose and lip), thin upper lip, and short palpebral fissures (small eye openings).",
      "Neuropsychological testing shows intellectual disability, attention-deficit problems, and poor impulse control. MRI brain shows corpus callosum abnormalities and reduced gray matter volume.",
      "Alcohol is the most common teratogen in the United States. It crosses the placenta freely and is especially damaging to the developing CNS during all trimesters.",
      "The mechanism: alcohol inhibits cell migration and induces apoptosis in developing neurons. It also causes placental vasoconstriction → fetal hypoxia.",
      "There is NO safe amount of alcohol during pregnancy. FAS is a permanent, irreversible diagnosis. Early intervention with special education and behavioral therapy improves outcomes."
    ],
    keyFacts:"Alcohol teratogen. Triad: (1) facial dysmorphia (smooth philtrum, thin lip, short palpebral fissures), (2) IUGR, (3) CNS dysfunction (ID, ADHD). No safe alcohol dose. Permanent."
  }
  ,{
    id:61, diagnosis:"Alcoholic Fatty Liver Disease",
    aliases:["alcoholic fatty liver","alcoholic steatosis","fatty liver disease","hepatic steatosis"],
    episode:"Ep. 289 – Alcoholic Liver Disease", topic:"Gastroenterology",
    ankiStep1:"Fatty_liver::reversible::alcohol::AST_ALT_2:1::steatosis",
    ankiStep2:"Fatty_liver::macrovesicular::hepatomegaly::asymptomatic::abstinence",
    clues:[
      "A 44-year-old man who drinks 8–10 beers per day is seen in clinic for an insurance physical. He has no complaints — no jaundice, abdominal pain, or fatigue.",
      "Exam reveals a mildly enlarged, non-tender liver (hepatomegaly). No splenomegaly, no ascites, no spider angiomata, no palmar erythema.",
      "LFTs: AST 78, ALT 45 (AST:ALT ratio ~2:1, classic for alcoholic liver disease). Bilirubin and albumin are normal. GGT is elevated.",
      "Liver ultrasound shows increased echogenicity ('bright liver') consistent with fatty infiltration. Liver biopsy (if done) shows macrovesicular steatosis — fat vacuoles push the nucleus to the cell periphery.",
      "Alcohol causes fatty liver by: (1) increasing NADH (alcohol→acetaldehyde→NADH by ADH), shifting fatty acid metabolism toward synthesis; (2) directly inhibiting fatty acid beta-oxidation.",
      "This is the most common and most reversible stage of alcoholic liver disease. Complete abstinence leads to resolution within 4–6 weeks. Without abstinence, it can progress to alcoholic hepatitis or cirrhosis."
    ],
    keyFacts:"Most common + reversible stage. Asymptomatic hepatomegaly. AST:ALT ~2:1. Macrovesicular steatosis. Bright liver on US. Reversible with abstinence within 4–6 weeks."
  }
  ,{
    id:62, diagnosis:"Methanol Poisoning",
    aliases:["methanol poisoning","methanol toxicity","methyl alcohol poisoning","wood alcohol poisoning"],
    episode:"Ep. 289 – Alcoholic Liver Disease", topic:"Gastroenterology",
    ankiStep1:"Methanol::formic_acid::anion_gap_acidosis::visual_loss::fomepizole",
    ankiStep2:"Methanol::osmol_gap_early::anion_gap_late::ethanol_antidote::dialysis",
    clues:[
      "A 38-year-old man is brought to the ER after drinking unknown liquor at a party. He is confused with HR 115, RR 28, and complains that his vision is blurry and he 'sees snowflakes.'",
      "He was initially intoxicated but now seems more obtunded than expected. ABG: pH 7.12, HCO3 10, pCO2 27 — severe anion-gap metabolic acidosis. Serum lactate is mildly elevated.",
      "Anion gap = 26 (elevated). Serum osmolality is elevated but the calculated osmolarity is lower — elevated osmol gap (early) transitioning to anion gap (late, as methanol is metabolized).",
      "Visual complaints ('snowstorm vision') with an inebriated patient and high anion gap acidosis should prompt immediate suspicion of methanol. The optic nerve is specifically targeted.",
      "Methanol is metabolized by alcohol dehydrogenase (ADH) → formaldehyde → formic acid. Formic acid inhibits cytochrome c oxidase (mitochondrial complex IV), causing cellular hypoxia and optic nerve damage.",
      "Treatment: fomepizole (4-MP) — preferential ADH inhibitor (first choice). Ethanol also works (competes with methanol for ADH) if fomepizole unavailable. Hemodialysis to remove methanol and formate. Folate (enhances formate metabolism)."
    ],
    keyFacts:"Methanol → formic acid (via ADH) → anion gap acidosis + optic nerve damage ('snowstorm' vision). Osmol gap early, then anion gap. Tx: fomepizole (or ethanol) + dialysis + folate."
  }
  ,{
    id:63, diagnosis:"Barrett's Esophagus",
    aliases:["barrett's esophagus","barretts esophagus","barrett esophagus","intestinal metaplasia esophagus"],
    episode:"Ep. 185 – GI: Esophagus & Stomach", topic:"Gastroenterology",
    ankiStep1:"Barretts::GERD::intestinal_metaplasia::columnar::adenocarcinoma_risk",
    ankiStep2:"Barretts::salmon_pink::goblet_cells::PPI::surveillance_EGD",
    clues:[
      "A 58-year-old obese man with 15 years of heartburn and regurgitation, despite taking omeprazole, undergoes upper endoscopy (EGD) for ongoing symptoms.",
      "EGD shows salmon-colored, velvety mucosa extending 4 cm above the gastroesophageal junction, replacing the normal pale squamous epithelium.",
      "Biopsy of the salmon-colored area shows intestinal metaplasia: columnar epithelium with goblet cells (normally absent from the esophagus). This is the defining histological finding.",
      "Barrett's esophagus is a response to chronic acid injury — the normal squamous epithelium of the esophagus transforms to acid-resistant columnar (intestinal-type) epithelium.",
      "This metaplasia carries a risk of progressing to esophageal adenocarcinoma (dysplasia → carcinoma). Annual surveillance endoscopy is required; frequency depends on degree of dysplasia.",
      "Treatment: high-dose PPI to reduce acid injury. Radiofrequency ablation for high-grade dysplasia. Esophagectomy for intramucosal carcinoma. Lifestyle changes: weight loss, elevate HOB."
    ],
    keyFacts:"Chronic GERD → squamous → intestinal metaplasia (goblet cells). Salmon-colored mucosa. Adenocarcinoma risk (not SCC). PPI + surveillance EGD. Ablation for high-grade dysplasia."
  }
  ,{
    id:64, diagnosis:"Esophageal Squamous Cell Carcinoma",
    aliases:["esophageal squamous cell carcinoma","esophageal scc","squamous cell carcinoma esophagus"],
    episode:"Ep. 185 – GI: Esophagus & Stomach", topic:"Gastroenterology",
    ankiStep1:"Esophageal_SCC::upper_middle_esophagus::alcohol_tobacco::dysphagia",
    ankiStep2:"Esophageal_SCC::Plummer_Vinson::achalasia::tylosis::poor_prognosis",
    clues:[
      "A 62-year-old man with a 40-pack-year smoking history and heavy alcohol use presents with 4 months of progressive dysphagia — first to solids, now liquids — and a 20 lb weight loss.",
      "He has no heartburn history. He is from a high-risk geographic area (Iran, China). EGD shows an irregular, ulcerated, friable mucosal mass in the mid-esophagus.",
      "Biopsy reveals squamous cell carcinoma — keratin pearls and intercellular bridges. The tumor is in the upper two-thirds of the esophagus.",
      "Risk factors are different from adenocarcinoma: tobacco + alcohol (most important), achalasia, Plummer-Vinson syndrome, tylosis (autosomal dominant palmoplantar keratoderma), lye ingestion.",
      "Adenocarcinoma occurs in the distal esophagus (GERD → Barrett's → adenocarcinoma). SCC occurs in the upper-to-middle esophagus. Both present with progressive dysphagia.",
      "Staging with CT and EUS (endoscopic ultrasound) guides treatment. Surgery (esophagectomy) ± chemoradiation. Prognosis is poor — most present at advanced stage (5-year survival <20%)."
    ],
    keyFacts:"Upper/mid esophagus. Tobacco + alcohol risk (vs. GERD for adenocarcinoma). Progressive dysphagia + weight loss. Keratin pearls on biopsy. Risk: achalasia, Plummer-Vinson, tylosis."
  }
  ,{
    id:65, diagnosis:"Lynch Syndrome",
    aliases:["lynch syndrome","hnpcc","hereditary nonpolyposis colorectal cancer","mismatch repair deficiency"],
    episode:"Ep. 186 – GI: Small & Large Bowel", topic:"Gastroenterology",
    ankiStep1:"Lynch::MMR_genes::MSH2_MLH1::Amsterdam_criteria::right_sided_colon",
    ankiStep2:"Lynch::endometrial_cancer::microsatellite_instability::colonoscopy_25",
    clues:[
      "A 38-year-old woman presents with rectal bleeding and change in bowel habits. Her father had colon cancer at age 41, and her paternal aunt had endometrial cancer at age 44.",
      "Colonoscopy reveals a right-sided colon cancer. She has no polyps elsewhere. The tumor is resected and sent for pathology.",
      "Tumor testing shows microsatellite instability (MSI-high) and absence of MLH1 and MSH2 protein expression on immunohistochemistry — consistent with mismatch repair (MMR) deficiency.",
      "Germline testing confirms a pathogenic MLH1 mutation. This is an autosomal dominant hereditary cancer syndrome caused by mutations in DNA mismatch repair genes (MLH1, MSH2, MSH6, PMS2).",
      "Unlike FAP, Lynch syndrome does NOT cause thousands of polyps. It causes early-onset colorectal cancer (right-sided predominance) and extracolonic cancers: endometrial (highest risk), ovarian, stomach, urinary tract.",
      "Amsterdam II criteria: 3 family members with Lynch-related cancer, 2 successive generations, 1 under age 50, 1 first-degree relative. Surveillance: colonoscopy beginning age 20–25, annual thereafter."
    ],
    keyFacts:"MMR gene mutations (MLH1, MSH2). MSI-high tumors. Early-onset right-sided CRC. Endometrial cancer risk highest extracolonic. NO polyposis (unlike FAP). Colonoscopy from age 25."
  }
  ,{
    id:66, diagnosis:"Peutz-Jeghers Syndrome",
    aliases:["peutz jeghers syndrome","peutz-jeghers","peutz jeghers","sth11 hamartoma"],
    episode:"Ep. 186 – GI: Small & Large Bowel", topic:"Gastroenterology",
    ankiStep1:"Peutz_Jeghers::STK11::hamartomas::mucocutaneous_pigmentation::small_bowel",
    ankiStep2:"Peutz_Jeghers::intussusception::GI_bleeding::GI_breast_ovarian_cancer",
    clues:[
      "A 19-year-old woman presents with recurrent episodes of colicky abdominal pain and occasional blood in her stool. She has dark blue-black spots on her lips, buccal mucosa, and fingers.",
      "The perioral and mucocutaneous pigmentation was present since childhood. Her father had a similar spotting pattern and died from colon cancer at age 45.",
      "Colonoscopy and upper endoscopy reveal multiple hamartomatous polyps throughout the GI tract, predominantly in the small bowel.",
      "An autosomal dominant mutation in the STK11/LKB1 tumor suppressor gene is responsible. The polyps are hamartomas (disorganized but mature tissue), not adenomas.",
      "Despite being hamartomas, there is an increased risk of GI malignancies (small bowel, colorectal, gastric) as well as extraintestinal cancers: breast, ovarian, cervical, testicular.",
      "Complications: GI bleeding, intussusception (polyp acts as a lead point — presents with colicky pain). Management: endoscopic surveillance, polypectomy when identified."
    ],
    keyFacts:"STK11/LKB1 mutation. Hamartomatous polyps (small bowel predominant). Mucocutaneous pigmentation (lips/mouth/fingers). Intussusception risk. Cancer risk: GI + breast/ovarian. Autosomal dominant."
  }
  ,{
    id:67, diagnosis:"Meckel's Diverticulum",
    aliases:["meckel's diverticulum","meckels diverticulum","meckel diverticulum"],
    episode:"Ep. 186 – GI: Small & Large Bowel", topic:"Gastroenterology",
    ankiStep1:"Meckel::rule_of_2s::omphalomesenteric::gastric_mucosa::painless_rectal_bleeding",
    ankiStep2:"Meckel::technetium_scan::ileum::children::diverticulitis_like",
    clues:[
      "A 3-year-old boy presents with sudden painless, large-volume bright red rectal bleeding. He looks pale and is tachycardic. There is no abdominal tenderness.",
      "Stool is maroon-colored. Nasogastric lavage is clear (no upper GI source). The child is hemodynamically unstable, requiring IV fluid resuscitation.",
      "A Technetium-99m pertechnetate scan (Meckel's scan) shows an ectopic focus of uptake in the right lower quadrant — indicating ectopic gastric mucosa.",
      "The ectopic gastric mucosa secretes acid, causing ulceration of adjacent ileal mucosa → painless bleeding. This is the most common cause of significant lower GI bleeding in children.",
      "Rule of 2s: 2% of the population, 2 feet from ileocecal valve, 2 inches long, 2 types of ectopic mucosa (gastric most common, pancreatic), presents within first 2 years of life, 2:1 male:female.",
      "Treatment: surgical resection of the diverticulum. May also present as diverticulitis (mimicking appendicitis) or intussusception in adults."
    ],
    keyFacts:"Rule of 2s. Omphalomesenteric duct remnant. Ectopic gastric mucosa → painless rectal bleeding in children. Meckel's scan (Tc-99m). Most common significant lower GI bleed in kids. Tx: surgery."
  }
  ,{
    id:68, diagnosis:"Diverticulitis",
    aliases:["diverticulitis","acute diverticulitis","colonic diverticulitis"],
    episode:"Ep. 186 – GI: Small & Large Bowel", topic:"Gastroenterology",
    ankiStep1:"Diverticulitis::LLQ_pain::fever::sigmoid::CT_scan::antibiotics",
    ankiStep2:"Diverticulitis::microperforation::Hinchey::complications::low_fiber",
    clues:[
      "A 68-year-old woman with known diverticulosis presents with 2 days of worsening left lower quadrant pain, fever (38.6°C), and constipation. She has nausea but no vomiting.",
      "Exam: LLQ tenderness with guarding, no rigidity. Mild leukocytosis (WBC 14.2). She is not jaundiced. No palpable mass.",
      "CT abdomen/pelvis with contrast shows sigmoid colonic wall thickening, pericolonic fat stranding, and a 4 cm pericolic fluid collection (abscess).",
      "Diverticulitis = microperforation of a diverticulum → localized pericolonic infection/inflammation. The sigmoid colon is affected in 90% of Western cases (due to high intraluminal pressures).",
      "Complications (Hinchey classification): I=pericolic abscess, II=pelvic abscess, III=generalized purulent peritonitis, IV=generalized fecal peritonitis. Fistulae (colovesical → pneumaturia, fecaluria) can occur.",
      "Treatment: mild = outpatient antibiotics (ciprofloxacin + metronidazole, or amoxicillin-clavulanate). Hinchey I–II abscess = IV antibiotics ± CT-guided drainage. Hinchey III–IV = emergency surgery (Hartmann's procedure)."
    ],
    keyFacts:"Sigmoid colon microperforation. LLQ pain + fever + leukocytosis. CT: colonic wall thickening + fat stranding ± abscess. Complication: fistula (colovesical). Tx: antibiotics ± drainage ± surgery."
  }
  ,{
    id:69, diagnosis:"Celiac Disease",
    aliases:["celiac disease","celiac sprue","gluten sensitive enteropathy","coeliac disease"],
    episode:"Ep. 186 – GI: Small & Large Bowel", topic:"Gastroenterology",
    ankiStep1:"Celiac::anti-tTG::anti-endomysial::HLA-DQ2_DQ8::villous_atrophy",
    ankiStep2:"Celiac::dermatitis_herpetiformis::T_cell_lymphoma::gluten_free_diet::dapsone",
    clues:[
      "A 28-year-old woman with a history of type 1 diabetes presents with chronic diarrhea, bloating, and a 12 lb weight loss over 6 months. She describes her stool as pale, bulky, and difficult to flush.",
      "She also has iron-deficiency anemia, vitamin D deficiency, and a pruritic vesicular rash on her elbows and knees (dermatitis herpetiformis).",
      "Serologic tests: anti-tissue transglutaminase (anti-tTG) IgA markedly elevated; anti-endomysial antibodies positive. Total IgA is normal.",
      "Upper endoscopy with small bowel biopsy (duodenum/jejunum) shows villous atrophy, crypt hyperplasia, and increased intraepithelial lymphocytes — the hallmark histological findings.",
      "Pathogenesis: gluten (gliadin component) triggers immune response in HLA-DQ2/DQ8 individuals → CD4 T-cell activation → mucosal damage → malabsorption.",
      "Treatment: strict lifelong gluten-free diet (no wheat, barley, rye). Dermatitis herpetiformis responds to dapsone. Increased risk of T-cell lymphoma (enteropathy-associated) with prolonged exposure."
    ],
    keyFacts:"Gluten sensitivity (HLA-DQ2/DQ8). Anti-tTG IgA + anti-endomysial antibodies. Villous atrophy + crypt hyperplasia. Dermatitis herpetiformis. T-cell lymphoma risk. Tx: gluten-free diet."
  }
  ,{
    id:70, diagnosis:"Colorectal Cancer",
    aliases:["colorectal cancer","colon cancer","rectal cancer","colonic adenocarcinoma"],
    episode:"Ep. 186 – GI: Small & Large Bowel", topic:"Gastroenterology",
    ankiStep1:"CRC::APC::adenoma_to_carcinoma::apple_core::CEA_surveillance",
    ankiStep2:"CRC::KRAS::left_obstructive::right_bleeding::colonoscopy_45",
    clues:[
      "A 62-year-old man presents with a 3-month history of change in bowel habits (alternating diarrhea and constipation), rectal bleeding, and a 15 lb weight loss.",
      "Digital rectal exam reveals a palpable mass. CBC shows iron deficiency anemia. CEA level is elevated at 28 ng/mL.",
      "Colonoscopy reveals a 4 cm partially obstructing, ulcerated mass in the sigmoid colon. Biopsy confirms adenocarcinoma.",
      "CT of chest/abdomen/pelvis: 3 liver lesions consistent with metastases. The molecular pathway: APC mutation → β-catenin dysregulation → KRAS mutation → SMAD4/TP53 loss → carcinoma.",
      "Left-sided colon cancer: obstructive symptoms, change in stool caliber, rectal bleeding. Right-sided colon cancer: iron deficiency anemia, bulky mass, less obstruction (wider lumen).",
      "Screening: colonoscopy beginning age 45 (average risk). Staging with CT. CEA used for monitoring recurrence (not screening). Treatment: surgery + FOLFOX chemotherapy ± bevacizumab for metastatic disease."
    ],
    keyFacts:"APC→KRAS→TP53 pathway. Left: obstruction + change in caliber. Right: IDA + mass. CEA for surveillance (not screening). Colonoscopy from age 45. Tx: surgery ± FOLFOX chemo."
  }
  ,{
    id:71, diagnosis:"MEN Type 1",
    aliases:["men1","men type 1","multiple endocrine neoplasia type 1","wermer syndrome"],
    episode:"Ep. 185 – GI: Esophagus & Stomach", topic:"Endocrinology",
    ankiStep1:"MEN1::3Ps::parathyroid_pituitary_pancreas::menin::chromosome_11",
    ankiStep2:"MEN1::hyperparathyroidism_first::prolactinoma::gastrinoma_ZE",
    clues:[
      "A 35-year-old man is evaluated for kidney stones (his second episode). He also reports headaches, visual field changes, and intractable peptic ulcer disease resistant to PPIs.",
      "Lab workup: hypercalcemia (Ca 11.8), elevated PTH, elevated prolactin, and elevated gastrin (>1000 pg/mL). MRI pituitary shows a 1.2 cm macroadenoma.",
      "Upper endoscopy shows multiple duodenal ulcers. Secretin stimulation test: gastrin rises paradoxically (>200 pg/mL increase) — confirming Zollinger-Ellison syndrome.",
      "The triad: parathyroid hyperplasia/adenoma (most common — causes hypercalcemia/stones), pituitary adenoma (prolactinoma most common), and pancreatic islet cell tumor (gastrinoma most common).",
      "Caused by loss-of-function mutation in MEN1 (menin tumor suppressor gene) on chromosome 11q13. Autosomal dominant. Hyperparathyroidism almost always presents first.",
      "Management: each component treated individually. Hyperparathyroidism: parathyroidectomy. Pituitary: dopamine agonist (cabergoline/bromocriptine). Pancreatic: PPI + surgical resection if localized."
    ],
    keyFacts:"MEN1 (menin gene, chr 11). 3 P's: Parathyroid (Ca stones) + Pituitary (prolactinoma) + Pancreas (gastrinoma/insulinoma). Hyperparathyroidism first. AD inheritance."
  }
  ,{
    id:72, diagnosis:"MEN Type 2A",
    aliases:["men2a","men type 2a","multiple endocrine neoplasia type 2a","sipple syndrome"],
    episode:"Ep. 251 – Thyroid", topic:"Endocrinology",
    ankiStep1:"MEN2A::RET::medullary_thyroid::pheochromocytoma::parathyroid",
    ankiStep2:"MEN2A::calcitonin::rule_out_pheo_first::adrenalectomy_before_thyroidectomy",
    clues:[
      "A 40-year-old woman is found to have an elevated calcitonin on routine labs. Her brother underwent bilateral adrenalectomy last year for pheochromocytoma.",
      "Further workup: calcitonin markedly elevated, 24-hour urine metanephrines elevated, calcium 10.9, PTH elevated — consistent with hyperparathyroidism.",
      "Genetic testing confirms a germline RET proto-oncogene gain-of-function mutation (codon 634). Thyroid FNA shows medullary thyroid carcinoma.",
      "The triad of MEN2A: medullary thyroid carcinoma (100% penetrance), pheochromocytoma (50%), and parathyroid hyperplasia (20–30%). All linked to RET mutations.",
      "Critical surgical sequence: if pheochromocytoma is present, it MUST be resected first (before thyroidectomy) — otherwise, surgical stimulation of an unresected pheochromocytoma causes hypertensive crisis.",
      "Prophylactic thyroidectomy is recommended in RET-positive family members, often in childhood. All first-degree relatives must be tested for RET mutation."
    ],
    keyFacts:"RET mutation. MEN2A: Medullary thyroid Ca (100%) + Pheo (50%) + Parathyroid (20%). Adrenalectomy BEFORE thyroidectomy. Screen family with RET testing. Prophylactic thyroidectomy."
  }
  ,{
    id:73, diagnosis:"MEN Type 2B",
    aliases:["men2b","men type 2b","multiple endocrine neoplasia type 2b"],
    episode:"Ep. 251 – Thyroid", topic:"Endocrinology",
    ankiStep1:"MEN2B::RET_codon918::mucosal_neuromas::marfanoid::no_parathyroid",
    ankiStep2:"MEN2B::most_aggressive_MTC::earliest_thyroidectomy::ganglioneuromas",
    clues:[
      "A 16-year-old boy with a tall, slender body habitus (but normal lens and aorta), bumpy lips, and lumps on his tongue is referred to endocrinology after his mother noticed a neck mass.",
      "Exam: multiple nodules on the tongue and lips (mucosal neuromas), prominent corneal nerve fibers on slit-lamp, and a firm thyroid nodule. He has a Marfanoid body type.",
      "Calcitonin is very elevated. 24-hr urine metanephrines are elevated (pheochromocytoma). Genetic testing shows RET codon 918 mutation.",
      "MEN2B triad: medullary thyroid carcinoma (earliest and most aggressive of all MEN-related MTCs) + pheochromocytoma + mucosal neuromas/ganglioneuromas (intestinal: causes constipation/megacolon).",
      "Unlike MEN2A, there is NO parathyroid disease. The Marfanoid habitus is present but without Marfan syndrome features (no lens dislocation, no aortic root dilation).",
      "MEN2B requires the earliest prophylactic thyroidectomy (within first 6 months of life for codon 918 mutations). Adrenalectomy first if pheochromocytoma is present."
    ],
    keyFacts:"RET codon 918. MEN2B: MTC (most aggressive, earliest onset) + Pheo + mucosal neuromas + Marfanoid. NO parathyroid. Thyroidectomy in first 6 months. No Marfan features (lens/aorta normal)."
  }
  ,{
    id:74, diagnosis:"Branchial Cleft Cyst",
    aliases:["branchial cleft cyst","branchial cyst","lateral neck cyst"],
    episode:"Ep. 280 – Branchial Arches", topic:"ENT/Embryology",
    ankiStep1:"Branchial_cleft::2nd_branchial::lateral_neck::SCM::painless_mass",
    ankiStep2:"Branchial_cleft::cholesterol_crystals::young_adult::surgical_excision",
    clues:[
      "A 22-year-old man presents with a painless, soft, fluctuant mass along the anterior border of the sternocleidomastoid (SCM) muscle that he first noticed 2 months ago.",
      "The mass is non-tender, compressible, and transilluminates. It has gradually enlarged. He has no fever, no weight loss, no dysphagia, and no prior infections.",
      "CT neck shows a well-circumscribed, fluid-filled cystic structure at the angle of the mandible, medial to the SCM and lateral to the carotid sheath.",
      "Aspiration yields turbid fluid with cholesterol crystals. Pathology of the cyst wall shows stratified squamous epithelium with lymphoid tissue in the wall.",
      "This is the most common branchial apparatus anomaly, derived from persistence of the second branchial cleft (pharyngeal groove). It represents failure of the cervical sinus of His to obliterate.",
      "Treatment: complete surgical excision (to prevent recurrence and infection). Must rule out cystic metastasis from oropharyngeal SCC (HPV+) in patients >40 years — biopsy first."
    ],
    keyFacts:"2nd branchial cleft remnant. Anterior SCM, painless fluctuant cyst, young adult. Cholesterol crystals. Squamous epithelium + lymphoid wall. Tx: surgical excision. Rule out cystic SCC in >40yr."
  }
  ,{
    id:75, diagnosis:"Pierre Robin Sequence",
    aliases:["pierre robin sequence","pierre robin syndrome","robin sequence"],
    episode:"Ep. 280 – Branchial Arches", topic:"ENT/Embryology",
    ankiStep1:"Pierre_Robin::micrognathia::glossoptosis::U_shaped_cleft_palate::airway",
    ankiStep2:"Pierre_Robin::1st_arch::Stickler_syndrome::prone_positioning::airway_management",
    clues:[
      "A newborn in the delivery room has immediate respiratory distress with cyanosis and audible stridor. The neonatologist notes an unusually small jaw.",
      "Exam: micrognathia (very small mandible), glossoptosis (tongue falls backward into the oropharynx), and a U-shaped cleft palate. The tongue is obstructing the airway.",
      "The U-shaped cleft palate (vs. V-shaped in isolated cleft palate) is characteristic. The sequence is: mandibular hypoplasia → tongue displacement posteriorly → prevents palatal shelves from fusing.",
      "This is a 'sequence' (one defect leading to a cascade of others) rather than a 'syndrome.' The primary defect is failure of mandibular development (1st branchial arch derivative).",
      "Associations: Stickler syndrome (most common — collagen mutation, myopia, deafness), Treacher Collins syndrome, and velocardiofacial syndrome.",
      "Immediate management: prone positioning (gravity moves tongue forward, opens airway). May require nasopharyngeal airway, laryngeal mask, or surgical airway. Mandible often catches up with growth."
    ],
    keyFacts:"Micrognathia + glossoptosis + U-shaped cleft palate. Airway emergency in newborn. 1st arch failure → tongue blocks palate fusion. Associated: Stickler syndrome. Tx: prone positioning, airway management."
  }
  ,{
    id:76, diagnosis:"Treacher Collins Syndrome",
    aliases:["treacher collins syndrome","mandibulofacial dysostosis","treacher collins"],
    episode:"Ep. 280 – Branchial Arches", topic:"ENT/Embryology",
    ankiStep1:"Treacher_Collins::TCOF1::1st_2nd_arch::downslanting_palpebral::bilateral_symmetric",
    ankiStep2:"Treacher_Collins::malar_hypoplasia::ear_anomalies::conductive_hearing_loss::AD",
    clues:[
      "A baby is born with symmetric bilateral facial abnormalities. The parents are concerned about the baby's facial appearance and note the baby seems not to respond to sounds.",
      "Exam: bilateral downslanting palpebral fissures, malar (cheekbone) hypoplasia giving a 'fish-like' face, coloboma (notch) of the lower eyelids, and bilateral external ear (pinna) anomalies with absent ear canals.",
      "Audiologic evaluation confirms bilateral conductive hearing loss due to absent/malformed external auditory canals and middle ear ossicles.",
      "Genetic testing reveals a TCOF1 (treacle) gene mutation on chromosome 5q31. This affects neural crest cell migration into the 1st and 2nd branchial arches during embryonic development.",
      "Unlike Pierre Robin, which is a sequence, Treacher Collins is a true syndrome with a specific gene mutation. It is autosomal dominant with variable expressivity.",
      "Management: multidisciplinary (craniofacial surgery, ENT, audiology, ophthalmology). Hearing aids early for language development. Reconstructive surgery for facial abnormalities. Intelligence is typically normal."
    ],
    keyFacts:"TCOF1 mutation. Bilateral symmetric: downslanting palpebral fissures + malar hypoplasia + lower lid coloboma + ear anomalies + conductive hearing loss. 1st+2nd arch. AD. Normal intelligence."
  }
  ,{
    id:77, diagnosis:"Dry Beriberi",
    aliases:["dry beriberi","peripheral neuropathy thiamine","b1 neuropathy"],
    episode:"Ep. 243 – Water Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"Dry_beriberi::B1_thiamine::peripheral_neuropathy::ascending::alcoholic",
    ankiStep2:"Dry_beriberi::glove_stocking::motor_sensory::muscle_wasting::thiamine",
    clues:[
      "A 58-year-old chronic alcoholic presents with 6 months of bilateral leg weakness, numbness, and 'pins and needles' in his feet and hands.",
      "Exam reveals diminished sensation in a 'glove-and-stocking' distribution, reduced deep tendon reflexes at the ankles, and mild wasting of the calf muscles.",
      "There is no cardiac involvement — heart rate, blood pressure, and heart size are normal. He has no edema.",
      "Nerve conduction studies show axonal neuropathy — decreased amplitude of sensory and motor potentials — consistent with peripheral nerve degeneration.",
      "This is dry beriberi: thiamine (B1) deficiency manifesting as peripheral neuropathy, without cardiac involvement. Thiamine is essential for the pyruvate dehydrogenase complex and α-ketoglutarate dehydrogenase, critical for ATP production in neurons.",
      "Treatment: thiamine supplementation (oral or IV). Neurological recovery can occur but may be incomplete if the deficiency has been prolonged."
    ],
    keyFacts:"B1 (thiamine) deficiency. Peripheral neuropathy (glove-stocking): sensory + motor. No cardiac involvement (contrast with wet beriberi). Axonal neuropathy. Alcoholics/malnutrition. Tx: thiamine."
  }
  ,{
    id:78, diagnosis:"Vitamin E Deficiency",
    aliases:["vitamin e deficiency","tocopherol deficiency","ataxia vitamin e"],
    episode:"Ep. 281 – Fat Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"Vitamin_E::antioxidant::hemolytic_anemia::ataxia::fat_malabsorption",
    ankiStep2:"Vitamin_E::posterior_column::spinocerebellar::Friedreich-like::abetalipoproteinemia",
    clues:[
      "A 14-year-old boy with cystic fibrosis and known fat malabsorption develops progressive difficulty walking and loss of coordination over 2 years.",
      "Exam shows ataxia, diminished proprioception and vibration sense (posterior column dysfunction), areflexia, and dysarthria. He has no family history of neurologic disease.",
      "Vitamin E level is undetectable. Serum vitamin A is also low. No neurologic disease gene mutation is found.",
      "The neurological picture resembles Friedreich's ataxia — but Friedreich's is caused by frataxin gene mutation, not nutritional deficiency. The spinocerebellar tracts and posterior columns are affected.",
      "Vitamin E is a fat-soluble antioxidant that protects cell membranes from lipid peroxidation. It is absorbed with dietary fat and transported on lipoproteins. Deficiency causes hemolytic anemia and neurodegeneration.",
      "Causes of deficiency: fat malabsorption (CF, cholestatic liver disease), abetalipoproteinemia (very rare — cannot form chylomicrons). Treatment: high-dose vitamin E supplementation."
    ],
    keyFacts:"Vitamin E deficiency (fat malabsorption: CF, cholestatic disease, abetalipoproteinemia). Spinocerebellar ataxia + posterior column signs. Hemolytic anemia. Friedreich's-like picture. Tx: vitamin E."
  }
  ,{
    id:79, diagnosis:"Hypervitaminosis A",
    aliases:["hypervitaminosis a","vitamin a toxicity","vitamin a overdose","retinol toxicity"],
    episode:"Ep. 281 – Fat Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"Hypervit_A::pseudotumor_cerebri::hepatotoxicity::teratogenic::bone_resorption",
    ankiStep2:"Hypervit_A::isotretinoin::polar_bear_liver::headache::papilledema",
    clues:[
      "A 26-year-old woman taking high-dose isotretinoin for severe acne presents with severe headache, nausea, and blurred vision.",
      "Exam shows papilledema bilaterally on fundoscopic exam. There are no focal neurological deficits.",
      "LP is performed: opening pressure is elevated at 32 cmH2O. CSF analysis is normal (no cells, normal glucose and protein). MRI brain shows no mass.",
      "This is pseudotumor cerebri (idiopathic intracranial hypertension) caused by vitamin A toxicity. Isotretinoin (a vitamin A derivative/retinoid) is a known cause.",
      "Other toxic effects of excess vitamin A: teratogenicity (cranial neural crest defects — women of childbearing age on isotretinoin require two forms of contraception), hepatotoxicity, bone pain/resorption, dry skin.",
      "Historical note: Arctic explorers who ate polar bear liver (extremely high in vitamin A) developed acute toxicity: headache, nausea, peeling skin, pseudotumor cerebri."
    ],
    keyFacts:"Vitamin A toxicity (isotretinoin, supplements, polar bear liver). Pseudotumor cerebri (papilledema, high ICP). Teratogenic (contraception mandatory). Hepatotoxicity, bone pain. Stop vitamin A."
  }
  ,{
    id:80, diagnosis:"Osteomalacia",
    aliases:["osteomalacia","adult rickets","vitamin d deficiency adults"],
    episode:"Ep. 281 – Fat Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"Osteomalacia::unmineralized_osteoid::pseudofractures::bone_pain::looser_zones",
    ankiStep2:"Osteomalacia::vitamin_D::low_Ca_PO4::high_ALP::Milkman_fractures",
    clues:[
      "A 55-year-old dark-skinned woman who wears full body covering for religious reasons presents with diffuse bone pain and muscle weakness for 8 months. She rarely goes outdoors.",
      "She is tender over the ribs, pelvis, and proximal femur with palpation. She has proximal muscle weakness (difficulty rising from a chair) and a waddling gait.",
      "Labs: serum calcium 7.8, phosphate 1.9, alkaline phosphatase 380 (elevated), PTH elevated. 25-OH vitamin D is very low (<10 ng/mL).",
      "X-rays show Looser zones (Milkman's fractures) — pseudofractures: radiolucent bands perpendicular to the cortex in the femoral neck, pubic rami, and scapulae.",
      "Osteomalacia = failure of bone mineralization in adults (rickets in children). The osteoid (bone matrix) is laid down but not mineralized due to low calcium and phosphate.",
      "Treatment: vitamin D3 (cholecalciferol) + calcium supplementation. Investigate underlying cause: fat malabsorption, renal osteodystrophy, liver disease, anti-epileptic drugs (phenytoin induces vitamin D metabolism)."
    ],
    keyFacts:"Vitamin D deficiency in adults. Unmineralized osteoid → bone pain + fractures. Looser zones/pseudofractures on XR. Low Ca+PO4, high ALP+PTH. Waddling gait, proximal weakness. Tx: vitamin D + Ca."
  }
  ,{
    id:81, diagnosis:"B6 (Pyridoxine) Deficiency",
    aliases:["pyridoxine deficiency","b6 deficiency","vitamin b6 deficiency","isoniazid neuropathy"],
    episode:"Ep. 243 – Water Soluble Vitamins", topic:"Vitamins",
    ankiStep1:"B6::isoniazid::peripheral_neuropathy::sideroblastic_anemia::homocysteine",
    ankiStep2:"B6::pyridoxine_supplement_with_INH::GABA_serotonin::seizures",
    clues:[
      "A 42-year-old man being treated for active tuberculosis (on isoniazid for 3 months) presents with bilateral tingling and numbness in his hands and feet.",
      "Exam shows reduced sensation in a stocking-glove distribution and diminished ankle reflexes. He was not prescribed any supplemental vitamins with his TB regimen.",
      "CBC shows microcytic anemia with a normal iron level. Peripheral blood smear shows ringed sideroblasts (iron-laden mitochondria surrounding the nucleus).",
      "Isoniazid (INH) inhibits pyridoxal kinase, preventing conversion of pyridoxine to its active form, pyridoxal-5-phosphate (PLP). This depletes functional B6.",
      "B6 (PLP) is a cofactor for: aminotransferases (ALT/AST), decarboxylases (DOPA → dopamine, 5-HTP → serotonin, glutamate → GABA), glycogen phosphorylase, and heme synthesis (ALA synthase). B6 deficiency → peripheral neuropathy + sideroblastic anemia + seizures (GABA deficiency).",
      "Prevention: always co-prescribe pyridoxine (B6) 25–50 mg/day with isoniazid. Also deficient in alcoholics and those on hydralazine or oral contraceptives."
    ],
    keyFacts:"INH blocks B6 activation → peripheral neuropathy + sideroblastic anemia ± seizures (GABA). Always give pyridoxine with INH. B6 is cofactor for aminotransferases, GABA/serotonin synthesis, heme synthesis."
  }
  ,{
    id:82, diagnosis:"Carcinoid Tumor",
    aliases:["carcinoid tumor","carcinoid syndrome","neuroendocrine tumor carcinoid"],
    episode:"Ep. 243 – Water Soluble Vitamins", topic:"Gastroenterology",
    ankiStep1:"Carcinoid::serotonin::flushing_diarrhea::wheezing::right_heart::5-HIAA",
    ankiStep2:"Carcinoid::ileum::pellagra::octreotide::liver_metastases::niacin_shunt",
    clues:[
      "A 55-year-old man presents with episodic flushing (facial redness lasting minutes), watery diarrhea, and wheezing. The episodes are triggered by alcohol and stress.",
      "Exam during an episode: bright red flushing of the face and neck, audible wheezing. He has right-sided heart murmurs — tricuspid regurgitation and pulmonary stenosis.",
      "24-hour urine 5-HIAA (5-hydroxyindoleacetic acid, a serotonin metabolite) is markedly elevated at 180 mg/day (normal <8). Serum chromogranin A is elevated.",
      "CT scan shows a small bowel mass (ileal primary — most common site for midgut carcinoids) with multiple liver metastases. Carcinoid syndrome only occurs with liver metastases (serotonin is normally degraded in the liver).",
      "Serotonin shunts tryptophan away from niacin synthesis, causing pellagra (3 D's: dermatitis, diarrhea, dementia). Right-sided heart disease occurs because serotonin is inactivated by the pulmonary vascular bed before reaching the left heart.",
      "Treatment: octreotide (somatostatin analogue) for symptom control and tumor growth. Surgical resection if possible. Everolimus/sunitinib for advanced disease."
    ],
    keyFacts:"Serotonin-secreting neuroendocrine tumor. Flushing + diarrhea + wheezing + right-sided valve disease. Elevated 5-HIAA. Pellagra (tryptophan shunted away from niacin). Liver mets required for syndrome. Tx: octreotide."
  }
  ,{
    id:83, diagnosis:"Down Syndrome",
    aliases:["down syndrome","downs syndrome","trisomy 21","down's syndrome"],
    episode:"Ep. 17 – Peds", topic:"Genetics/Pediatrics",
    ankiStep1:"Down::trisomy21::nondisjunction::endocardial_cushion::ALL",
    ankiStep2:"Down::Alzheimers_presenilin::single_palmar_crease::atlanto-axial::Robertsonian",
    clues:[
      "A 35-year-old woman gives birth to an infant who immediately appears hypotonic ('floppy baby') with a flat facial profile, upward-slanting palpebral fissures, and epicanthal folds.",
      "The infant has a small, flat nose, large tongue, and a single transverse palmar crease (simian crease) on both hands. Fingers appear short and stubby.",
      "Echocardiogram reveals an endocardial cushion defect (AV septal defect) — the most common congenital heart defect in this condition. Duodenal atresia ('double bubble' on X-ray) is also noted.",
      "Karyotype shows 47 chromosomes with three copies of chromosome 21. In 95% of cases, this results from maternal meiotic nondisjunction — risk increases sharply with maternal age.",
      "Long-term risks include: early-onset Alzheimer's disease (presenilin gene on chromosome 21), ALL ('ALL fall Down'), and atlantoaxial instability (requires cervical spine X-ray before contact sports).",
      "Maternal quad screen: elevated β-hCG and inhibin A, low AFP and unconjugated estriol. Can also result from Robertsonian translocation (carrier has 46 chromosomes but child has unbalanced 46+extra 21 material)."
    ],
    keyFacts:"Trisomy 21. Maternal nondisjunction (age-related). Flat facies, hypotonia, simian crease. Endocardial cushion defect + duodenal atresia. ALL risk. Alzheimer's (chr 21). Quad screen: high βhCG+inhibin A, low AFP+estriol."
  }
  ,{
    id:84, diagnosis:"Turner Syndrome",
    aliases:["turner syndrome","45x","45 x","monosomy x"],
    episode:"Ep. 17 – Peds", topic:"Genetics/Pediatrics",
    ankiStep1:"Turner::45X::webbed_neck::coarctation::streak_ovaries",
    ankiStep2:"Turner::horseshoe_kidney::bicuspid_aortic::lymphedema::GH_estrogen",
    clues:[
      "A 14-year-old girl is evaluated for primary amenorrhea and short stature. She has never had a menstrual period and has minimal breast development despite being in the expected age range for puberty.",
      "Physical exam reveals a short, stocky girl with a low posterior hairline, webbed neck (pterygium colli), and bilateral pitting edema of the hands and feet (lymphedema).",
      "Cardiac workup reveals a bicuspid aortic valve and coarctation of the aorta (hypertension in arms, hypotension in legs, rib notching on X-ray). Renal ultrasound shows a horseshoe kidney.",
      "Labs: FSH and LH are markedly elevated (hypergonadotropic hypogonadism). Estradiol is very low. Karyotype: 45,X. The ovaries have degenerated into streak gonads (no estrogen → no negative feedback → FSH/LH rise).",
      "This is the most common cause of primary amenorrhea. Intelligence is typically normal, though patients may have visuospatial difficulties.",
      "Treatment: growth hormone (GH) to improve height, then estrogen replacement therapy at puberty (age ~11–13) to induce secondary sexual characteristics and protect bone density. Fertility typically not possible (streak gonads)."
    ],
    keyFacts:"45,X. Short female, webbed neck, lymphedema. Bicuspid AV + coarctation. Horseshoe kidney. Streak gonads → hypergonadotropic hypogonadism. Primary amenorrhea. Tx: GH then estrogen. Normal intelligence."
  }
  ,{
    id:85, diagnosis:"Klinefelter Syndrome",
    aliases:["klinefelter syndrome","47xxy","klinefelters syndrome","klinefelter's syndrome"],
    episode:"Ep. 17 – Peds", topic:"Genetics/Pediatrics",
    ankiStep1:"Klinefelter::47XXY::hypogonadism::gynecomastia::infertility",
    ankiStep2:"Klinefelter::tall::small_testes::azoospermia::testosterone",
    clues:[
      "A 17-year-old boy is evaluated for gynecomastia and delayed pubertal development. He is the tallest boy in his class at 6'2\" despite average parental heights.",
      "He has sparse facial and body hair, small testes bilaterally, and a small phallus for his age. He has mild learning difficulties, particularly with reading and language.",
      "Testosterone is low. FSH and LH are elevated (hypergonadotropic hypogonadism). Semen analysis shows azoospermia (no sperm). Karyotype: 47,XXY.",
      "The extra X chromosome causes failure of spermatogenesis and reduced testosterone production, leading to hypogonadism, gynecomastia (from estrogen/testosterone imbalance), and infertility.",
      "The tall stature results from delayed epiphyseal closure (low testosterone means less sex hormone to close growth plates). This is the most common sex chromosome abnormality.",
      "Treatment: testosterone replacement therapy (starts at puberty) to develop secondary sexual characteristics, reduce gynecomastia, improve libido, and maintain bone density. Infertility is generally irreversible but ICSI may help."
    ],
    keyFacts:"47,XXY. Tall male. Small testes + gynecomastia + azoospermia. Hypergonadotropic hypogonadism (low T, high FSH/LH). Mild learning difficulties. Most common sex chromosome disorder. Tx: testosterone."
  }
  ,{
    id:86, diagnosis:"Fragile X Syndrome",
    aliases:["fragile x syndrome","fragile x","fmr1","fragile x mental retardation"],
    episode:"Ep. 17 – Peds", topic:"Genetics/Pediatrics",
    ankiStep1:"Fragile_X::CGG_repeat::FMR1::X_linked::macroorchidism",
    ankiStep2:"Fragile_X::long_face::large_ears::autism::ADHD::anticipation",
    clues:[
      "A 7-year-old boy is brought for evaluation of intellectual disability, attention problems, and social difficulties. His maternal uncle also had intellectual disability.",
      "Exam: large, protruding ears, a long narrow face, and a prominent jaw. His mother notes that his father is mildly affected — 'not quite right' but functional.",
      "Post-pubertal examination (or family history) reveals macroorchidism (enlarged testicles) — a hallmark finding in affected males.",
      "Genetic testing reveals CGG trinucleotide repeat expansion in the FMR1 gene on the X chromosome. Normal: <55 repeats. Premutation: 55–200. Full mutation (causes disease): >200 repeats.",
      "This condition shows genetic anticipation (worsens through generations) and X-linked inheritance — but it is unusual because carrier females are mildly affected and the repeat can expand during maternal transmission.",
      "Associations: intellectual disability (most common heritable cause in males), autism spectrum disorder, ADHD, and seizures. Female carriers may develop premature ovarian insufficiency or FXTAS (fragile X tremor-ataxia syndrome)."
    ],
    keyFacts:"CGG repeat in FMR1 gene. X-linked with anticipation. Long face + large ears + macroorchidism. Intellectual disability + autism + ADHD. Most common heritable cause of ID in males. Female carriers: POI, FXTAS."
  }
  ,{
    id:87, diagnosis:"Prader-Willi Syndrome",
    aliases:["prader willi syndrome","prader-willi syndrome","pws"],
    episode:"Ep. 17 – Peds", topic:"Genetics/Pediatrics",
    ankiStep1:"Prader_Willi::paternal_deletion_15q::hyperphagia::hypotonia::hypogonadism",
    ankiStep2:"Prader_Willi::maternal_UPD::obesity::short::cryptorchidism",
    clues:[
      "A 3-year-old boy who was a 'floppy baby' at birth (hypotonia, poor feeding, tube-fed for 3 months) now presents with insatiable appetite, rapid weight gain, and behavioral outbursts when food is withheld.",
      "Exam: obese child (BMI 30), almond-shaped eyes, small hands and feet, and undescended testes (cryptorchidism). His speech is delayed.",
      "His hypotonia has persisted since infancy, though it has improved somewhat. He has mild intellectual disability and behavioral problems including temper tantrums and obsessive behaviors.",
      "Genetic testing reveals deletion of the paternal chromosome 15q11-q13. (Alternatively, maternal uniparental disomy: both copies of chromosome 15 come from the mother, with no paternal copy.)",
      "The paternal region of 15q11-q13 is normally active (imprinted = silenced on maternal copy). When the paternal copy is deleted, neither copy is functional — leading to Prader-Willi. 'Prader has no PaPa.'",
      "Treatment: caloric restriction, structured environment (lock food), growth hormone (GH) therapy improves height, muscle tone, and body composition. Monitor for obstructive sleep apnea and type 2 diabetes."
    ],
    keyFacts:"Paternal 15q deletion (or maternal UPD 15). Neonatal hypotonia → hyperphagia/obesity. Almond eyes, small hands/feet, hypogonadism. Behavioral: food obsession. Tx: GH + strict diet. 'Prader has no PaPa.'"
  }
  ,{
    id:88, diagnosis:"Angelman Syndrome",
    aliases:["angelman syndrome","happy puppet syndrome"],
    episode:"Ep. 17 – Peds", topic:"Genetics/Pediatrics",
    ankiStep1:"Angelman::maternal_deletion_15q::ataxia::seizures::inappropriate_laughter",
    ankiStep2:"Angelman::paternal_UPD::small_face::absent_speech::EEG_spike",
    clues:[
      "A 2-year-old girl is brought for evaluation of severe developmental delay. She cannot speak any words but laughs frequently and spontaneously — often at inappropriate times.",
      "Parents describe a happy, excitable personality with frequent hand-flapping movements. She walks with a wide-based, puppet-like ataxic gait and has a history of multiple seizures.",
      "Exam: small head (microcephaly), widely spaced teeth, protruding tongue, and a characteristically happy demeanor. EEG shows large-amplitude spike-and-slow-wave complexes.",
      "Genetic testing reveals deletion of the maternal chromosome 15q11-q13. (Alternatively, paternal uniparental disomy: both chromosome 15s come from the father, with no maternal copy.)",
      "The maternal region of 15q11-q13 is normally active (UBE3A is maternally expressed in the brain). When deleted, UBE3A is non-functional → Angelman syndrome. 'AngelMan for Mom.'",
      "Compare with Prader-Willi (paternal 15q deletion): hypotonia, hyperphagia, obesity, hypogonadism. Both result from chromosome 15 imprinting but affect different genes."
    ],
    keyFacts:"Maternal 15q deletion (or paternal UPD 15). Ataxia + seizures + absent speech + inappropriate/excessive laughter. Happy demeanor ('happy puppet'). Small face. 'AngelMan = Mom.' Compare: Prader-Willi (paternal deletion)."
  }
  ,{
    id:89, diagnosis:"Tuberous Sclerosis",
    aliases:["tuberous sclerosis","tuberous sclerosis complex","tsc","bourneville disease"],
    episode:"Ep. 17 – Peds", topic:"Genetics/Pediatrics",
    ankiStep1:"Tuberous_sclerosis::TSC1_TSC2::ash_leaf::adenoma_sebaceum::cortical_tubers",
    ankiStep2:"Tuberous_sclerosis::West_syndrome::rhabdomyoma::renal_angiomyolipoma::shagreen_patch",
    clues:[
      "A 6-month-old boy is brought to the ED after his mother witnessed him having episodes of brief body jerks with eye deviation. EEG shows hypsarrhythmia (chaotic high-amplitude multifocal spikes).",
      "Skin exam under Wood's lamp reveals 3 hypopigmented leaf-shaped macules ('ash leaf spots'). A reddish-brown rash of papules is noted on his cheeks and nose bridge.",
      "Brain MRI shows multiple cortical tubers (calcified gray matter hamartomas) and subependymal nodules lining the ventricles. Cardiac ultrasound reveals a cardiac rhabdomyoma.",
      "The facial lesions are angiofibromas (not acne), classically called 'adenoma sebaceum.' Renal ultrasound reveals bilateral angiomyolipomas.",
      "This is an autosomal dominant neurocutaneous syndrome caused by mutations in TSC1 (hamartin, chromosome 9) or TSC2 (tuberin, chromosome 16). Both suppress mTOR pathway.",
      "Infantile spasms (West syndrome) is the classic seizure presentation. Antiseizure: ACTH or vigabatrin. Subependymal giant cell astrocytoma (SEGA) can obstruct CSF flow. Treatment: everolimus (mTOR inhibitor) for SEGAs and angiomyolipomas."
    ],
    keyFacts:"TSC1/TSC2 mutation. AD. Triad: ash leaf spots + angiofibromas (adenoma sebaceum) + cortical tubers. West syndrome (infantile spasms, hypsarrhythmia). Rhabdomyoma + renal angiomyolipoma. Tx: ACTH/vigabatrin; everolimus."
  }
  ,{
    id:90, diagnosis:"Von Gierke Disease",
    aliases:["von gierke disease","glycogen storage disease type 1","gsd1","glucose-6-phosphatase deficiency"],
    episode:"Ep. 17 – Peds", topic:"Genetics/Metabolic",
    ankiStep1:"Von_Gierke::G6Pase::liver_kidney::lactic_acidosis::severe_fasting_hypoglycemia",
    ankiStep2:"Von_Gierke::hepatomegaly::gout::xanthomas::cornstarch::AR",
    clues:[
      "A 6-month-old infant presents with a seizure after a 4-hour fast. He appears well-nourished but has a markedly protuberant abdomen. His parents are consanguineous.",
      "Blood glucose is 28 mg/dL. Exam confirms massive hepatomegaly. Kidneys are also enlarged on ultrasound. No splenomegaly.",
      "Labs: severe fasting hypoglycemia, elevated lactic acid, elevated uric acid (gout risk), hypertriglyceridemia, and elevated free fatty acids. No ketonuria.",
      "The key finding: after a glucagon challenge, blood glucose fails to rise — despite abundant glycogen in the liver. This confirms an inability to release glucose from glycogen or gluconeogenesis.",
      "The deficient enzyme is glucose-6-phosphatase (G6Pase) — normally located in the ER of liver and kidney cells. Without it, glucose-6-phosphate accumulates and cannot be released as free glucose. Autosomal recessive.",
      "Treatment: frequent small feeds + uncooked cornstarch (slow-digesting glucose source) to prevent hypoglycemia. No muscle involvement (G6Pase is not in muscle). Liver transplant for severe cases."
    ],
    keyFacts:"G6Pase deficiency (GSD type 1). Liver+kidney (no muscle). Severe fasting hypoglycemia + lactic acidosis + hyperuricemia + hyperlipidemia. Hepatomegaly. Glucagon challenge: no glucose rise. Tx: frequent feeds + cornstarch."
  }
  ,{
    id:91, diagnosis:"Pompe Disease",
    aliases:["pompe disease","glycogen storage disease type 2","gsd2","acid maltase deficiency","lysosomal storage disease glycogen"],
    episode:"Ep. 17 – Peds", topic:"Genetics/Metabolic",
    ankiStep1:"Pompe::acid_maltase::lysosomal::cardiomegaly::hypotonia",
    ankiStep2:"Pompe::death_before_2::AR::alglucosidase_alfa::no_hepatomegaly_in_classic",
    clues:[
      "A 3-month-old infant is admitted with progressive weakness, poor feeding, and failure to thrive. The parents report he has difficulty breathing, especially when lying flat.",
      "Exam: profound hypotonia ('floppy infant'), macroglossia, and massive cardiomegaly. Heart sounds are distant. CXR confirms cardiomegaly with pulmonary congestion.",
      "EKG shows a very short PR interval and massive QRS complexes. Echo confirms hypertrophic cardiomyopathy with severely reduced function.",
      "Muscle biopsy reveals vacuoles filled with glycogen in the cytoplasm and lysosomes. Enzymatic assay shows absent acid alpha-glucosidase (acid maltase) activity. Dried blood spot confirms.",
      "This is the only glycogen storage disease caused by a lysosomal enzyme deficiency. Glycogen accumulates within lysosomes throughout the body — most critically in heart, skeletal, and diaphragmatic muscle.",
      "Without treatment, the infantile form leads to death before age 2 from cardiorespiratory failure. Treatment: enzyme replacement therapy with alglucosidase alfa (recombinant acid alpha-glucosidase) dramatically improves survival."
    ],
    keyFacts:"Acid alpha-glucosidase (acid maltase) deficiency. ONLY GSD that is a lysosomal disease. Hypotonia + cardiomegaly + macroglossia in infants. Death before 2 without treatment. Tx: alglucosidase alfa (ERT). AR."
  }
  ,{
    id:92, diagnosis:"Duchenne Muscular Dystrophy",
    aliases:["duchenne muscular dystrophy","dmd","duchenne md","duchenne"],
    episode:"Ep. 19 – Neurology", topic:"Neurology/Genetics",
    ankiStep1:"DMD::dystrophin::X_linked::Gowers_sign::calf_pseudohypertrophy",
    ankiStep2:"DMD::CK_elevated::cardiomyopathy::respiratory_failure::steroids",
    clues:[
      "A 5-year-old boy is brought in because his parents noticed he struggles to climb stairs and falls frequently. He uses an unusual maneuver — placing his hands on his thighs and 'walking up' his own legs to stand from the floor.",
      "Exam reveals bilateral calf enlargement (pseudohypertrophy — firm, rubbery calves that look muscular but are filled with fat and fibrotic tissue). Proximal weakness is pronounced.",
      "Serum creatine kinase (CK) is markedly elevated (>10,000 IU/L). EMG shows a myopathic pattern. Genetic testing reveals a frameshift deletion in the DMD gene on the X chromosome.",
      "The DMD gene encodes dystrophin — a structural protein that anchors the sarcolemma to the cytoskeleton. Without it, muscle fibers are damaged during contraction and replaced by fat and fibrotic tissue.",
      "X-linked recessive — affects males almost exclusively. Carrier females may have mild muscle weakness or elevated CK. Boys are typically in a wheelchair by age 12.",
      "Treatment: corticosteroids (prednisone/deflazacort) slow progression. Newer therapies: exon-skipping (eteplirsen for exon 51 skip). Cardiac monitoring (dilated cardiomyopathy). Respiratory support (eventually non-invasive ventilation). Death from respiratory or cardiac failure in 20s–30s."
    ],
    keyFacts:"X-linked. Dystrophin gene (DMD). Proximal weakness + Gowers sign + calf pseudohypertrophy. CK very elevated. Frameshift deletion. Cardiomyopathy + respiratory failure. Steroids slow progression. Death 20s-30s."
  }
  ,{
    id:93, diagnosis:"Myasthenia Gravis",
    aliases:["myasthenia gravis","mg","neuromuscular junction autoimmune"],
    episode:"Ep. 19 – Neurology", topic:"Neurology",
    ankiStep1:"MG::AChR_antibodies::postsynaptic::fatigable_weakness::thymoma",
    ankiStep2:"MG::ptosis_diplopia::worse_at_end_of_day::pyridostigmine::CT_chest",
    clues:[
      "A 38-year-old woman presents with drooping eyelids (ptosis) and double vision (diplopia) that are notably worse at the end of the day and improve with rest.",
      "She also reports difficulty chewing, slurred speech, and trouble swallowing. Symptoms began mildly 3 months ago and have progressively worsened.",
      "Neurological exam: fatigable weakness — repeated muscle use causes increasing weakness. Ice pack test is positive: applying ice to the eyelid improves ptosis (cold slows ACh breakdown). Reflexes are normal.",
      "Anti-AChR antibodies are positive (in 85% of cases). EMG shows decremental response on repetitive stimulation. Anti-MuSK antibodies may be positive in seronegative cases.",
      "The postsynaptic nicotinic acetylcholine receptors at the neuromuscular junction are destroyed by autoantibodies. Unlike LEMS (presynaptic Ca2+ channel antibodies), strength does NOT improve with repeated stimulation.",
      "CT chest is mandatory — thymoma present in ~15% of patients. Thymectomy can improve or cure myasthenia gravis. Treatment: acetylcholinesterase inhibitors (pyridostigmine), steroids, immunosuppressants (azathioprine), IVIG for myasthenic crisis."
    ],
    keyFacts:"Anti-AChR antibodies. Postsynaptic NMJ. Fatigable weakness (worse at end of day). Ptosis + diplopia + dysphagia. Decremental EMG. Thymoma in 15% → CT chest. Tx: pyridostigmine, steroids, thymectomy."
  }
  ,{
    id:94, diagnosis:"Glioblastoma Multiforme",
    aliases:["glioblastoma multiforme","gbm","glioblastoma","grade iv glioma","butterfly glioma"],
    episode:"Ep. 19 – Neurology", topic:"Neurology/Oncology",
    ankiStep1:"GBM::GFAP::butterfly::pseudopalisading_necrosis::ring_enhancing",
    ankiStep2:"GBM::grade4::adults::EGFR::temozolomide::survival_15months",
    clues:[
      "A 62-year-old man presents with 6 weeks of progressive headaches (worse in the morning, with nausea), personality changes, and new-onset seizures.",
      "MRI brain with contrast reveals a large mass that crosses the corpus callosum, creating a 'butterfly' pattern. There is central ring enhancement with a necrotic core and significant surrounding edema.",
      "Neuropsychological testing reveals executive dysfunction and word-finding difficulties. A stereotactic biopsy is performed.",
      "Pathology: marked cellular pleomorphism, mitoses, microvascular proliferation (glomeruloid vessels), and pseudopalisading necrosis (tumor cells arranged around necrotic areas). GFAP positive.",
      "This is the most common primary brain tumor in adults and the most malignant (WHO Grade IV). Mutations: EGFR amplification, PTEN loss, TERT promoter mutation. IDH-wildtype carries the worst prognosis.",
      "Treatment: maximal safe surgical resection + radiotherapy + temozolomide (Stupp protocol). Bevacizumab for recurrence. Median survival: ~15 months despite treatment. It does NOT metastasize outside the CNS."
    ],
    keyFacts:"Most malignant primary brain tumor. Crosses corpus callosum ('butterfly'). Pseudopalisading necrosis + ring enhancement. GFAP+. Adults. IDH-wildtype worst prognosis. Tx: surgery + RT + temozolomide. Median survival 15 months."
  }
  ,{
    id:95, diagnosis:"Medulloblastoma",
    aliases:["medulloblastoma","pnet brain","cerebellar tumor child","primitive neuroectodermal tumor"],
    episode:"Ep. 19 – Neurology", topic:"Neurology/Oncology",
    ankiStep1:"Medulloblastoma::cerebellar_vermis::Homer_Wright_rosettes::children::hydrocephalus",
    ankiStep2:"Medulloblastoma::CSF_seeding::radiation_sensitive::WNT_pathway::posterior_fossa",
    clues:[
      "A 7-year-old boy presents with 3 months of morning vomiting, headaches, and progressive difficulty walking. He has become increasingly clumsy and falls to the right.",
      "Exam: truncal ataxia, papilledema (increased ICP), and dysmetria. He cannot perform heel-to-shin or finger-to-nose testing accurately.",
      "MRI brain reveals a large midline posterior fossa mass in the cerebellar vermis, with obstructive hydrocephalus (mass blocking the 4th ventricle).",
      "Pathology: densely cellular small round blue cells (SRBCT) arranged in Homer-Wright rosettes (rosettes surrounding a fibrillary neuropil center). Synaptophysin and GFAP positive.",
      "This is the most common malignant brain tumor in children and the most common posterior fossa tumor in children. It can seed the CSF (drop metastases throughout the spinal cord — 'sugar coating').",
      "Treatment: maximal safe resection + craniospinal irradiation + chemotherapy. Radiation-sensitive. WNT-activated subtype has the best prognosis. Risk of cognitive effects from craniospinal radiation in young children."
    ],
    keyFacts:"Most common malignant pediatric brain tumor. Cerebellar vermis (posterior fossa). Homer-Wright rosettes. Small round blue cells. CSF seeding ('drop mets'). Hydrocephalus. Tx: surgery + craniospinal RT + chemo."
  }
  ,{
    id:96, diagnosis:"Wallenberg Syndrome",
    aliases:["wallenberg syndrome","lateral medullary syndrome","pica syndrome","lateral medullary infarct"],
    episode:"Ep. 19 – Neurology", topic:"Neurology",
    ankiStep1:"Wallenberg::PICA::lateral_medulla::crossed_findings::dysphagia",
    ankiStep2:"Wallenberg::ipsilateral_face_contralateral_body::Horners::ataxia::hiccups",
    clues:[
      "A 68-year-old man with atrial fibrillation presents acutely with vertigo, nausea, hiccups, difficulty swallowing, and a hoarse voice.",
      "Neurological exam shows: ipsilateral loss of pain and temperature sensation on the face (left), contralateral loss of pain and temperature on the body (right), ipsilateral Horner's syndrome (ptosis, miosis, anhidrosis), and ipsilateral limb ataxia.",
      "There is loss of the gag reflex and dysphonia (CN IX/X palsy). Nystagmus is present. Notably, motor strength is fully preserved — no hemiplegia.",
      "MRI reveals a lateral medullary infarct. The vessel occluded is the Posterior Inferior Cerebellar Artery (PICA) or its parent, the vertebral artery.",
      "The 'crossed' pattern results from the anatomy: the spinothalamic tract (pain/temperature of the contralateral body) decussates in the spinal cord before reaching the medulla; the trigeminal nucleus (pain/temperature of the ipsilateral face) is in the lateral medulla.",
      "The sympathetic chain descending through the lateral medulla is interrupted, causing ipsilateral Horner's. The cerebellar peduncle injury causes ipsilateral ataxia. No motor hemiplegia because the corticospinal tract runs medially."
    ],
    keyFacts:"PICA occlusion → lateral medullary infarct. Crossed findings: ipsilateral face analgesia + contralateral body analgesia. Ipsilateral Horner + ataxia. Dysphagia + dysphonia (CN IX/X). NO hemiplegia. Hiccups."
  }
  ,{
    id:97, diagnosis:"BPPV",
    aliases:["bppv","benign paroxysmal positional vertigo","positional vertigo","benign positional vertigo"],
    episode:"Ep. 19 – Neurology", topic:"Neurology/ENT",
    ankiStep1:"BPPV::otolith::posterior_semicircular::Dix_Hallpike::Epley",
    ankiStep2:"BPPV::seconds::geotropic_nystagmus::most_common_vertigo::no_hearing_loss",
    clues:[
      "A 55-year-old woman presents with brief episodes of spinning sensation (vertigo) that occur when she rolls over in bed or turns her head quickly. Each episode lasts about 30–60 seconds.",
      "She has no hearing loss, no tinnitus, and no focal neurologic deficits. The vertigo resolves completely between episodes. She has had this for 3 weeks.",
      "The Dix-Hallpike maneuver is performed: head is turned to the right and the patient is brought supine quickly. After a 5-second latency, rotatory nystagmus (geotropic — toward the ground) appears and fatigues with repeated testing.",
      "The otoconia (calcium carbonate crystals) have become dislodged from the utricle and entered the posterior semicircular canal, causing abnormal fluid movement with head position changes.",
      "This is the most common cause of vertigo. Key differentiators: nystagmus fatigues (peripheral), short duration (<1 min), positional trigger, no hearing loss or tinnitus.",
      "Treatment: Epley maneuver (canalith repositioning) — highly effective (90% success). Semont maneuver is an alternative. Vestibular suppressants (meclizine) help symptoms but don't treat the cause."
    ],
    keyFacts:"Most common vertigo. Otolith (otoconia) in posterior semicircular canal. Dix-Hallpike: rotatory nystagmus with latency, fatigues. Episodes <1 min. No hearing loss. Tx: Epley maneuver."
  }
  ,{
    id:98, diagnosis:"Ectopic Pregnancy",
    aliases:["ectopic pregnancy","tubal pregnancy","ectopic"],
    episode:"Ep. 22 – OBGYN", topic:"OBGYN",
    ankiStep1:"Ectopic::ampulla::PID::LMP_6wk::beta_hcg::methotrexate",
    ankiStep2:"Ectopic::hemoperitoneum::shoulder_tip_pain::discriminatory_zone::salpingectomy",
    clues:[
      "A 28-year-old woman presents to the ED with lower abdominal pain and light vaginal bleeding. Her last menstrual period was 6 weeks ago, and a urine pregnancy test is positive.",
      "She has a history of pelvic inflammatory disease (PID) and smokes cigarettes. On exam, her BP is 100/65 and HR is 105 — mildly tachycardic. She has right adnexal tenderness.",
      "Transvaginal ultrasound (TVUS) shows no intrauterine gestational sac. β-hCG is 2,800 mIU/mL (above the 'discriminatory zone' of 2,000 mIU/mL where an IUP should be visible).",
      "TVUS reveals a small adnexal mass with free fluid in the cul-de-sac (hemoperitoneum). This patient's fallopian tube has been damaged by prior PID, leading to implantation in the ampulla of the fallopian tube.",
      "Risk factors: PID (scarred tubes), prior ectopic, smoking (reduces tubal motility), tubal ligation, IUD. Most common site: ampulla of the fallopian tube.",
      "Treatment: if hemodynamically stable and β-hCG <5,000, give methotrexate (inhibits trophoblast growth — contraindicated in kidney/liver disease). If unstable or ruptured → emergency salpingectomy. Rupture → life-threatening hemoperitoneum."
    ],
    keyFacts:"PID #1 risk factor. No IUP + positive βhCG + adnexal mass. Ampulla most common site. Discriminatory zone: 2000 mIU/mL. Stable + βhCG <5000: methotrexate. Unstable/ruptured: emergency salpingectomy."
  }
  ,{
    id:99, diagnosis:"Molar Pregnancy",
    aliases:["molar pregnancy","hydatidiform mole","complete hydatidiform mole","partial molar pregnancy"],
    episode:"Ep. 22 – OBGYN", topic:"OBGYN",
    ankiStep1:"Molar_pregnancy::snowstorm::very_high_bhcg::hyperemesis::preeclampsia_before_20w",
    ankiStep2:"Molar_pregnancy::complete::46XX::no_fetal_tissue::suction_curettage::choriocarcinoma",
    clues:[
      "A 19-year-old woman at 10 weeks gestation presents with severe nausea and vomiting (she has lost 10 pounds), vaginal bleeding, and blood pressure of 148/96 (elevated for gestational age).",
      "Her uterus is large-for-dates (size of 16 weeks). β-hCG is 250,000 mIU/mL — far above normal for gestational age.",
      "Transvaginal ultrasound shows no fetal pole and no gestational sac. Instead, there is a heterogeneous, echogenic mass filling the uterus with multiple cystic areas — the 'snowstorm' appearance.",
      "This is a complete hydatidiform mole: 2 sperm fertilize an empty egg (or 1 sperm fertilizes and duplicates), resulting in a 46,XX diploid karyotype. No fetal tissue. Trophoblastic tissue proliferates and produces massive βhCG.",
      "Partial mole: 2 sperm fertilize 1 egg → triploid (69,XXX or XXY), contains some fetal tissue, less βhCG, lower choriocarcinoma risk. Preeclampsia before 20 weeks is a classic clue pointing to a mole.",
      "Treatment: suction curettage (D&C). Serial weekly βhCG until undetectable. Patient must use contraception for 6 months (βhCG must be monitored to detect choriocarcinoma — rising or plateauing βhCG = gestational trophoblastic neoplasia). Tx: methotrexate."
    ],
    keyFacts:"Snowstorm uterus, very high βhCG, no fetal tissue (complete). Pre-20wk preeclampsia + hyperemesis. Complete: 46XX, 2 sperm + empty egg. Tx: suction curettage. Monitor βhCG × 6 months. Choriocarcinoma risk."
  }
  ,{
    id:100, diagnosis:"PCOS",
    aliases:["pcos","polycystic ovarian syndrome","polycystic ovary syndrome","stein leventhal syndrome"],
    episode:"Ep. 22 – OBGYN", topic:"OBGYN",
    ankiStep1:"PCOS::hyperandrogenism::anovulation::polycystic_ovaries::insulin_resistance",
    ankiStep2:"PCOS::LH_FSH_ratio::clomiphene::metformin::endometrial_cancer_risk",
    clues:[
      "A 24-year-old obese woman presents with irregular periods (every 45–60 days), unwanted hair growth on her chin and inner thighs, and difficulty getting pregnant for 1 year.",
      "Exam: BMI 33, acanthosis nigricans on the back of her neck and axillae, moderate hirsutism on the face, and mild acne. Blood pressure is elevated at 140/90.",
      "Labs: elevated free testosterone and LH, low SHBG, mildly elevated LH:FSH ratio (~3:1). Fasting glucose is 108. TSH and prolactin are normal.",
      "Pelvic ultrasound shows bilaterally enlarged ovaries with ≥12 small follicles arranged peripherally ('string of pearls') in each ovary. Endometrial thickness is increased.",
      "Diagnosis requires 2 of 3 Rotterdam criteria: (1) polycystic ovaries on ultrasound, (2) clinical/biochemical hyperandrogenism, (3) oligo/anovulation. Exclude secondary causes (thyroid, prolactinoma, congenital adrenal hyperplasia, androgen-secreting tumor).",
      "Treatment: weight loss + lifestyle (first-line). OCP (regulates cycles, lowers androgens via SHBG). Metformin (insulin resistance). Clomiphene citrate (induces ovulation for fertility). Long-term risk: endometrial cancer (unopposed estrogen from anovulation) — endometrial biopsy if prolonged PCOS with irregular bleeding."
    ],
    keyFacts:"2/3 Rotterdam criteria: polycystic ovaries + hyperandrogenism + anovulation. Insulin resistance + acanthosis nigricans. Elevated LH:FSH. 'String of pearls.' Endometrial cancer risk. Tx: weight loss, OCP, metformin, clomiphene."
  }
  ,{
    id:101, diagnosis:"Placenta Previa",
    aliases:["placenta previa","low lying placenta","placenta previa totalis"],
    episode:"Ep. 22 – OBGYN", topic:"OBGYN",
    ankiStep1:"Placenta_previa::painless_bleeding::3rd_trimester::C_section::no_vaginal_exam",
    ankiStep2:"Placenta_previa::prior_C_section::low_implantation::pelvic_rest::ultrasound_first",
    clues:[
      "A 34-year-old woman at 32 weeks gestation with a history of 2 prior cesarean sections calls her OB's office reporting sudden-onset painless, bright red vaginal bleeding. She has no abdominal pain or contractions.",
      "She is brought to L&D. Vital signs are stable. Fundal height is appropriate. No uterine tenderness. Fetal heart tracing is reactive.",
      "A vaginal exam is NOT performed — this could provoke massive hemorrhage by disrupting the placenta. Ultrasound is performed instead.",
      "Transvaginal ultrasound confirms that the placental edge overlies the internal cervical os (complete placenta previa). The placenta is implanted in the lower uterine segment.",
      "Risk factors: prior uterine surgery (C-sections cause scar tissue attracting low placentation), advanced maternal age, multiparity, prior curettage, smoking. Classic presentation: painless 3rd trimester bleeding.",
      "Management: hospitalization, pelvic rest (no intercourse), antenatal corticosteroids if <34 weeks. Planned cesarean delivery at 36–37 weeks. If massive hemorrhage at any gestational age → emergency C-section. NEVER perform a vaginal delivery."
    ],
    keyFacts:"Painless bright red 3rd-trimester bleeding. Placenta over cervical os. Prior C-section #1 risk factor. No vaginal exam (→ hemorrhage). Ultrasound first. Tx: pelvic rest, steroids, planned C-section at 36–37 weeks."
  }
  ,{
    id:102, diagnosis:"Abruptio Placentae",
    aliases:["abruptio placentae","placental abruption","abruption","placenta abruption"],
    episode:"Ep. 22 – OBGYN", topic:"OBGYN",
    ankiStep1:"Abruption::painful_bleeding::cocaine::HTN::board_like_uterus",
    ankiStep2:"Abruption::DIC::fetal_distress::emergency_delivery::concealed_hemorrhage",
    clues:[
      "A 29-year-old cocaine user at 34 weeks gestation is brought to the ED by EMS after the onset of sudden, severe abdominal pain and dark vaginal bleeding. She has not had prenatal care.",
      "Exam: blood pressure is 165/105, HR 115. The uterus is rigid and 'board-like' to palpation with exquisite tenderness. Fetal heart tracing shows late decelerations and minimal variability.",
      "Ultrasound shows a retroplacental hematoma (blood behind the placenta lifting it off the uterine wall). The placenta appears to be separated from the fundal area.",
      "Coagulation labs: fibrinogen is low, PT/PTT prolonged, D-dimer elevated — signs of consumptive coagulopathy (DIC) as placental thromboplastin is released into the maternal circulation.",
      "Premature separation of the normally-implanted placenta causes both maternal hemorrhage and fetal hypoxia. Risk factors: hypertension (most common), cocaine use, trauma, prior abruption, smoking.",
      "Management: if fetus is viable and showing distress → emergency cesarean delivery. If stable → may monitor. Blood transfusion and FFP for coagulopathy. Note: 'concealed' abruption (no visible bleeding but internal hemorrhage) is possible."
    ],
    keyFacts:"Painful 3rd-trimester bleeding + board-like uterus. HTN + cocaine = major risk factors. Retroplacental hematoma. DIC complication. Fetal distress. Tx: emergency C-section. Concealed hemorrhage possible (no external blood)."
  }
  ,{
    id:103, diagnosis:"Idiopathic Intracranial Hypertension",
    aliases:["idiopathic intracranial hypertension","pseudotumor cerebri","benign intracranial hypertension","iih"],
    episode:"Ep. 19 – Neurology", topic:"Neurology",
    ankiStep1:"IIH::obese_female::headache::papilledema::empty_sella",
    ankiStep2:"IIH::elevated_OP::normal_CSF::tetracycline_vitA::acetazolamide::optic_nerve_fenestration",
    clues:[
      "A 26-year-old obese woman taking tetracycline for acne presents with 6 weeks of daily headache that is worse in the morning, pulsatile, and associated with transient visual obscurations (brief visual blackouts).",
      "She also notices a whooshing sound in her ears (pulsatile tinnitus) and has had two episodes of brief double vision. She has no fever or neck stiffness.",
      "Fundoscopic exam reveals bilateral papilledema (blurred disc margins with elevated optic discs). Visual field testing shows enlarged blind spots.",
      "Brain MRI/MRV is normal — no mass, no hydrocephalus, no venous sinus thrombosis. An 'empty sella' or flattening of the posterior globes may be seen. LP: opening pressure 32 cmH2O; CSF analysis completely normal.",
      "This condition occurs most commonly in obese women of childbearing age. Triggers: tetracyclines (including doxycycline), vitamin A/retinoids (isotretinoin), corticosteroid withdrawal, OCPs.",
      "Treatment: weight loss (most important). Acetazolamide (reduces CSF production by inhibiting carbonic anhydrase). Serial LPs for acute relief. Optic nerve sheath fenestration if vision is threatened. VP shunt for refractory cases."
    ],
    keyFacts:"Obese young woman. Daily headache + papilledema + pulsatile tinnitus. Normal MRI, elevated LP opening pressure, normal CSF. Triggers: tetracyclines, vitamin A. Tx: weight loss, acetazolamide; optic nerve fenestration for vision loss."
  }
  ,{
    id:104, diagnosis:"Lambert-Eaton Myasthenic Syndrome",
    aliases:["lambert eaton","lems","lambert-eaton myasthenic syndrome","lambert eaton syndrome","presynaptic nmj"],
    episode:"Ep. 19 – Neurology", topic:"Neurology/Oncology",
    ankiStep1:"LEMS::presynaptic::VGCC_antibodies::proximal_weakness::small_cell_lung",
    ankiStep2:"LEMS::improves_with_repetition::paraneoplastic::dry_mouth::3,4-DAP",
    clues:[
      "A 62-year-old heavy smoker presents with proximal muscle weakness in his thighs and buttocks, making it difficult to climb stairs. He also reports a dry mouth.",
      "Neurological exam reveals: reflexes are initially absent, but after voluntary muscle contraction, the reflexes transiently return (post-exercise facilitation). This is the opposite of myasthenia gravis.",
      "EMG shows an incremental (facilitating) response on high-frequency repetitive stimulation — strength improves with repeated stimulation. CT chest reveals a hilar mass.",
      "Biopsy of the mass confirms small cell lung carcinoma (SCLC). Anti-voltage-gated calcium channel (VGCC) antibodies are found in the serum.",
      "This is a paraneoplastic syndrome: SCLC expresses VGCC (normally on presynaptic terminal). Antibodies against VGCC prevent calcium influx → impaired ACh vesicle release → weakness. Autonomic features (dry mouth, constipation, erectile dysfunction) are common.",
      "Treatment: treat the underlying tumor (SCLC — chemotherapy + radiation). 3,4-diaminopyridine (3,4-DAP) blocks K+ channels → prolongs depolarization → more Ca2+ influx → improved ACh release. Pyridostigmine may help. IVIG/plasma exchange for severe cases."
    ],
    keyFacts:"Presynaptic VGCC antibodies. Paraneoplastic (small cell lung Ca). Proximal weakness that IMPROVES with repetition (opposite of MG). Post-exercise facilitation of reflexes. Autonomic features (dry mouth). Tx: treat SCLC + 3,4-DAP."
  },
  {
    id:105, diagnosis:"Insulinoma",
    aliases:["insulinoma","islet cell tumor","pancreatic beta cell tumor","whipple triad"],
    episode:"Ep. 29", topic:"Endocrinology",
    ankiStep1:"Insulinoma::Whipple_triad::C-peptide_elevated",
    ankiStep2:"Insulinoma::hypoglycemia_symptoms_glucose_relief::elevated_insulin_and_C-peptide",
    clues:[
      "A 52-year-old woman presents with episodic diaphoresis, palpitations, and confusion that consistently occur before meals and upon waking in the morning.",
      "She reports that eating immediately relieves her symptoms within minutes. Over 6 months, she has gained 8 pounds because she snacks constantly to prevent episodes.",
      "During an observed fast, her blood glucose drops to 38 mg/dL and she becomes tremulous and anxious — symptoms that fully resolve after drinking orange juice.",
      "Serum insulin is inappropriately elevated during the hypoglycemic episode, and C-peptide is also elevated, ruling out exogenous insulin injection.",
      "A secretagogue screen (urine sulfonylurea screen) is negative. CT abdomen with contrast reveals a 1.4 cm hypervascular lesion in the tail of the pancreas.",
      "This is Whipple's triad: hypoglycemia plus symptoms of hypoglycemia plus resolution with glucose administration — the classic presentation of this insulin-secreting pancreatic neuroendocrine tumor."
    ],
    keyFacts:"Insulinoma presents with Whipple's triad. Elevated insulin AND elevated C-peptide distinguishes it from exogenous insulin (which suppresses C-peptide). Sulfonylureas also raise both but give a positive secretagogue screen. Treatment is surgical resection."
  },
  {
    id:106, diagnosis:"Porphyria Cutanea Tarda",
    aliases:["porphyria cutanea tarda","PCT","uroporphyrinogen decarboxylase deficiency"],
    episode:"Ep. 29", topic:"Metabolic/Dermatology",
    ankiStep1:"Porphyria_Cutanea_Tarda::UROD_deficiency::phlebotomy",
    ankiStep2:"Porphyria_Cutanea_Tarda::blistering_hands_hirsutism::elevated_liver_enzymes",
    clues:[
      "A 45-year-old man with a history of chronic alcohol use and hepatitis C presents with painful blistering lesions on the backs of both hands that worsen with sun exposure.",
      "He also notes hyperpigmentation of sun-exposed skin and increased facial hair growth that his wife has noticed over the past year.",
      "Physical examination reveals fragile skin, milia (small white cysts), and hypertrichosis on the temples and cheeks. Liver enzymes are mildly elevated.",
      "Urine collected in sunlight turns pink to dark brown. Urine porphyrin studies show markedly elevated uroporphyrins.",
      "The patient's condition is associated with alcohol use, hepatitis C, iron overload, and estrogen. It results from a deficiency of a specific enzyme in the heme synthesis pathway.",
      "This is the most common porphyria, caused by deficiency of uroporphyrinogen decarboxylase. Treatment involves phlebotomy to reduce iron stores and sun avoidance."
    ],
    keyFacts:"Porphyria cutanea tarda: blistering photosensitivity + hirsutism + hyperpigmentation. Caused by UROD deficiency. Associated with alcohol, hepatitis C, iron overload, estrogen. Urine turns pink in sunlight. Treatment: phlebotomy."
  },
  {
    id:107, diagnosis:"Lead Poisoning",
    aliases:["lead poisoning","lead toxicity","plumbism","lead intoxication"],
    episode:"Ep. 29", topic:"Toxicology",
    ankiStep1:"Lead_Poisoning::sideroblastic_anemia::DMSA_chelation",
    ankiStep2:"Lead_Poisoning::basophilic_stippling::Prussian_blue_stain_ringed_sideroblasts",
    clues:[
      "A 75-year-old man who has lived in a home built in 1928 presents with fatigue, abdominal cramping, and constipation. His young grandchildren who visit frequently have been found to have elevated blood lead levels.",
      "He also complains of wrist drop and peripheral neuropathy that has developed over several years. Blood smear shows basophilic stippling of red blood cells.",
      "A Prussian blue stain of a bone marrow aspirate shows basophilic inclusions arranged in a ring around the nucleus of erythroblasts.",
      "Ferritin is elevated. Total iron binding capacity is decreased. Transferrin saturation is elevated. This iron study pattern reflects iron loading with impaired utilization.",
      "Blood lead level is 65 mcg/dL. Radiographs of the long bones show dense metaphyseal bands (lead lines) in the children who visited the home.",
      "This heavy metal inhibits delta-aminolevulinic acid synthase and ferrochelatase, blocking heme synthesis and causing ringed sideroblasts. Treatment uses succimer (dimercaptosuccinic acid) or EDTA."
    ],
    keyFacts:"Lead poisoning: basophilic stippling, ringed sideroblasts on Prussian blue stain, elevated ferritin, decreased TIBC. Inhibits ALAS and ferrochelatase. Supplement B6 in sideroblastic anemia. Chelation with succimer (DMSA), EDTA, or dimercaprol (BAL)."
  },
  {
    id:108, diagnosis:"Pneumocystis Jirovecii Pneumonia",
    aliases:["pneumocystis jirovecii pneumonia","PCP pneumonia","pneumocystis pneumonia"],
    episode:"Ep. 29", topic:"Infectious Disease / Pulmonology",
    ankiStep1:"PCP_Pneumonia::silver_stain::TMP-SMX",
    ankiStep2:"PCP_Pneumonia::bilateral_ground_glass_HIV::LDH_elevated_BAL",
    clues:[
      "A 34-year-old man with known HIV infection presents with 3 weeks of progressive dyspnea on exertion, non-productive cough, and low-grade fevers.",
      "He has not been taking antiretroviral therapy regularly. His most recent CD4 count was 145 cells/mcL. Pulse oximetry is 88% on room air.",
      "Chest radiograph shows bilateral, diffuse, symmetric ground-glass infiltrates. CT chest confirms extensive bilateral ground-glass opacities in a perihilar distribution.",
      "Bronchoalveolar lavage fluid stained with silver (Gomori methenamine silver) stain reveals cup-shaped or crescent-shaped cysts. LDH from pulmonary fluid is markedly elevated.",
      "The alveolar-arterial oxygen gradient is 45 mmHg. Because it exceeds 35 mmHg, corticosteroids are added to the primary treatment regimen.",
      "This opportunistic fungal pathogen causes pneumonia in immunocompromised patients with CD4 counts below 200. Treatment is trimethoprim-sulfamethoxazole; prophylaxis begins when CD4 falls below 200."
    ],
    keyFacts:"Pneumocystis jirovecii pneumonia: bilateral ground-glass infiltrates in HIV patient with CD4 under 200. Diagnosis by silver stain of BAL fluid. LDH elevated. Add steroids if PaO2 under 70 or A-a gradient over 35. Treatment and prophylaxis: trimethoprim-sulfamethoxazole."
  },
  {
    id:109, diagnosis:"Carbon Monoxide Poisoning",
    aliases:["carbon monoxide poisoning","carbon monoxide toxicity","CO poisoning"],
    episode:"Ep. 29", topic:"Toxicology",
    ankiStep1:"Carbon_Monoxide_Poisoning::leftward_shift_oxyhemoglobin::hyperbaric_oxygen",
    ankiStep2:"Carbon_Monoxide_Poisoning::cherry_red_lips_winter::carboxyhemoglobin",
    clues:[
      "A 66-year-old woman is found unresponsive at home in January. Her husband reports they have been using a space heater in their bedroom with the windows closed due to freezing temperatures.",
      "Emergency medical services note she has an altered mental status and her lips have a distinctive bright red coloration. Her husband is also complaining of severe headache.",
      "Neurological exam shows confusion and mild ataxia. Initial oxygen saturation reads falsely normal at 98% on standard pulse oximetry, which cannot distinguish this toxin from oxygen on hemoglobin.",
      "Co-oximetry reveals a carboxyhemoglobin level of 32%. CT brain shows bilateral hyperintense lesions in the globus pallidus on MRI.",
      "The toxin binds hemoglobin with 200 times the affinity of oxygen, causing a leftward shift of the oxyhemoglobin dissociation curve and preventing oxygen delivery to tissues.",
      "This diagnosis requires measurement of carboxyhemoglobin levels. Treatment is 100% oxygen via non-rebreather mask or hyperbaric oxygen, which reduces the half-life of carboxyhemoglobin binding."
    ],
    keyFacts:"Carbon monoxide poisoning: cherry-red lips, headache, altered mental status in winter context. Pulse oximetry falsely normal. Diagnose with carboxyhemoglobin level. MRI shows globus pallidus lesions. Treatment: hyperbaric oxygen."
  },
  {
    id:110, diagnosis:"Hyperglycemic Hyperosmolar State",
    aliases:["hyperglycemic hyperosmolar state","hyperosmolar hyperglycemic state","HHS","HHNS","hyperosmolar nonketotic coma"],
    episode:"Ep. 29", topic:"Endocrinology",
    ankiStep1:"Hyperglycemic_Hyperosmolar_State::no_ketosis::T2DM",
    ankiStep2:"Hyperglycemic_Hyperosmolar_State::glucose_over_600_normal_pH::fluids_and_insulin",
    clues:[
      "A 72-year-old man with type 2 diabetes mellitus is brought to the emergency department by his family after 3 days of polyuria, polydipsia, and progressive confusion.",
      "He takes metformin and a sulfonylurea but has not been eating or drinking well due to a respiratory illness. He appears profoundly dehydrated with dry mucous membranes and poor skin turgor.",
      "Blood glucose is 920 mg/dL. Serum sodium is 128 mEq/L (corrected sodium is 143 mEq/L). Serum pH is 7.36 and bicarbonate is 22 mEq/L. There are no ketones in urine.",
      "Serum osmolality is 365 mOsm/kg. The patient becomes more confused shortly after rapid fluid resuscitation is begun. Cerebral edema from rapid glucose correction is suspected.",
      "Unlike diabetic ketoacidosis, this condition occurs in type 2 diabetes because residual insulin production suppresses glucagon and prevents ketogenesis, preserving a normal pH.",
      "This hyperglycemic emergency features extreme glucose elevation, hyperosmolarity, profound dehydration, and altered mental status WITHOUT ketoacidosis. Treatment is aggressive isotonic fluid resuscitation followed by insulin."
    ],
    keyFacts:"Hyperglycemic hyperosmolar state: glucose over 600, serum osmolality over 320, no ketosis, normal pH. Occurs in type 2 diabetes. Sodium correction: add 1.6 mEq for every 100 mg/dL glucose above normal. Cerebral edema risk with rapid correction. Treatment: fluids then insulin."
  },
  {
    id:111, diagnosis:"Stevens-Johnson Syndrome",
    aliases:["stevens-johnson syndrome","SJS","toxic epidermal necrolysis","erythema multiforme major"],
    episode:"Ep. 29", topic:"Dermatology",
    ankiStep1:"Stevens-Johnson_Syndrome::mucous_membrane_involvement::stop_offending_drug",
    ankiStep2:"Stevens-Johnson_Syndrome::targetoid_lesions_skin_sloughing::allopurinol_sulfonamides",
    clues:[
      "A 58-year-old man with gout presents to the emergency department with 3 days of fever, malaise, and a rapidly spreading blistering rash. He started allopurinol 10 days ago.",
      "Physical examination reveals targetoid (bull's-eye) skin lesions predominantly on the trunk and extremities, along with painful ulcers of the oral mucosa and conjunctival erythema.",
      "Skin sloughing involves approximately 12% of the total body surface area. Nikolsky sign is positive: gentle lateral pressure causes the epidermis to separate from the dermis.",
      "Ophthalmology is urgently consulted due to conjunctival involvement and risk of corneal scarring. The patient is transferred to a burn unit for wound care.",
      "Common causative medications include allopurinol, sulfonamides, anticonvulsants (lamotrigine, carbamazepine, phenytoin), and NSAIDs. The first and most critical step is stopping the offending drug.",
      "This severe mucocutaneous hypersensitivity reaction is defined by less than 10% body surface area involvement (greater than 30% is classified as toxic epidermal necrolysis). Treatment: stop drug, supportive burn care, ophthalmologic protection."
    ],
    keyFacts:"Stevens-Johnson syndrome: targetoid lesions + mucous membrane involvement + skin sloughing under 10% BSA. Positive Nikolsky sign. Causative drugs: allopurinol, sulfonamides, anticonvulsants. Stop the drug immediately. Over 30% BSA = toxic epidermal necrolysis."
  },
  {
    id:112, diagnosis:"Primary Adrenal Insufficiency",
    aliases:["primary adrenal insufficiency","addison disease","addison's disease","autoimmune adrenalitis"],
    episode:"Ep. 30", topic:"Endocrinology",
    ankiStep1:"Addison_Disease::autoimmune_adrenal_destruction::cosyntropin_test",
    ankiStep2:"Addison_Disease::hyperpigmentation_hyponatremia_hyperkalemia::hydrocortisone_fludrocortisone",
    clues:[
      "A 38-year-old woman presents with 6 months of progressive fatigue, weight loss, nausea, and dizziness upon standing. She also reports salt cravings and generalized weakness.",
      "Physical examination reveals hyperpigmentation of the skin creases, buccal mucosa, and new scars — areas that are typically not hyperpigmented in other conditions causing fatigue.",
      "Blood pressure is 88/52 mmHg supine with orthostatic drop. Laboratories show sodium 126 mEq/L, potassium 5.8 mEq/L, glucose 58 mg/dL, and a peripheral eosinophilia.",
      "ACTH level is markedly elevated at 890 pg/mL. Morning cortisol is low at 3 mcg/dL. Aldosterone is undetectable. Renin is elevated.",
      "Cosyntropin stimulation test shows no rise in cortisol despite stimulation, confirming the adrenal glands cannot respond. Anti-21-hydroxylase antibodies are positive.",
      "This autoimmune destruction of all three zones of the adrenal cortex causes deficiency of glucocorticoids AND mineralocorticoids. High ACTH cross-reacts with melanocortin receptors causing hyperpigmentation. Treatment requires both hydrocortisone and fludrocortisone."
    ],
    keyFacts:"Primary adrenal insufficiency (Addison disease): hyperpigmentation (high ACTH cross-reacts with MSH receptor), hyponatremia, hyperkalemia, hypoglycemia, eosinophilia. ACTH high. Cosyntropin test shows no response. Treatment: hydrocortisone + fludrocortisone. Stress-dose steroids for illness/surgery."
  },
  {
    id:113, diagnosis:"Waterhouse-Friderichsen Syndrome",
    aliases:["waterhouse-friderichsen syndrome","adrenal hemorrhage","meningococcal adrenal hemorrhage"],
    episode:"Ep. 30", topic:"Infectious Disease / Endocrinology",
    ankiStep1:"Waterhouse-Friderichsen_Syndrome::adrenal_hemorrhage::Neisseria_meningitidis",
    ankiStep2:"Waterhouse-Friderichsen_Syndrome::purpuric_rash_shock_hyperkalemia::meningococcemia",
    clues:[
      "A 19-year-old college student in a dormitory presents to the emergency department with sudden-onset severe headache, high fever, and a rapidly spreading non-blanching purpuric rash.",
      "Within hours of arrival, he develops profound hypotension unresponsive to fluid resuscitation, requiring vasopressors. Blood glucose is 44 mg/dL despite intravenous dextrose.",
      "Laboratory studies show sodium 128 mEq/L, potassium 6.2 mEq/L, and a non-anion gap metabolic acidosis. Cortisol level is critically low despite the hemodynamic stress.",
      "CT abdomen reveals bilateral adrenal gland hemorrhage with loss of normal adrenal architecture. Blood cultures are positive for a gram-negative diplococcus.",
      "The bacteria responsible — Neisseria meningitidis — causes massive bilateral adrenal hemorrhage, obliterating cortisol and aldosterone production simultaneously.",
      "This catastrophic complication of meningococcemia combines septic shock with primary adrenal insufficiency due to bilateral adrenal hemorrhage. Treatment requires ceftriaxone plus immediate stress-dose corticosteroids."
    ],
    keyFacts:"Waterhouse-Friderichsen syndrome: bilateral adrenal hemorrhage from Neisseria meningitidis causing acute adrenal insufficiency. Presents with purpuric rash, refractory shock, hypoglycemia, hyperkalemia, hyponatremia. Treatment: ceftriaxone + high-dose corticosteroids."
  },
  {
    id:114, diagnosis:"Rhabdomyolysis",
    aliases:["rhabdomyolysis","rhabdo","myoglobinuria","acute kidney injury from muscle breakdown"],
    episode:"Ep. 31", topic:"Nephrology / Toxicology",
    ankiStep1:"Rhabdomyolysis::myoglobin_renal_tubule_damage::hyperkalemia",
    ankiStep2:"Rhabdomyolysis::cola_colored_urine_CK_elevation::found_down",
    clues:[
      "A 27-year-old man is found on the floor of his apartment by a friend who had not heard from him in 13 hours. He was last seen at a bar drinking heavily.",
      "He is confused and complaining of severe pain and weakness in both legs. The skin over his thighs is tense and mottled. Urine output is only 20 mL/hour and the urine is dark brown, like cola.",
      "Electrocardiogram shows peaked T-waves. Potassium is 6.4 mEq/L, creatinine is 4.8 mg/dL, and creatine kinase is 85,000 IU/L.",
      "Urinalysis shows a large amount of blood on dipstick but only a few red blood cells on microscopy — a classic discrepancy caused by free myoglobin in the urine cross-reacting with the dipstick.",
      "The elevated potassium requires urgent management: calcium gluconate to stabilize the cardiac membrane, followed by insulin plus glucose and sodium bicarbonate to shift potassium intracellularly.",
      "Skeletal muscle breakdown releases massive amounts of myoglobin, potassium, phosphate, and urate into the circulation. Myoglobin precipitates in renal tubules causing acute tubular necrosis. Treatment is aggressive intravenous hydration."
    ],
    keyFacts:"Rhabdomyolysis: dark brown urine, markedly elevated creatine kinase, dipstick-positive blood with few red cells on microscopy (myoglobin). Causes hyperkalemia and acute kidney injury. Treat hyperkalemia with calcium gluconate first, then shift potassium intracellularly. Treatment: aggressive IV hydration."
  },
  {
    id:115, diagnosis:"Sickle Cell Disease",
    aliases:["sickle cell disease","sickle cell anemia","hemoglobin SS disease"],
    episode:"Ep. 31", topic:"Hematology",
    ankiStep1:"Sickle_Cell_Disease::glutamate_to_valine_mutation::hydroxyurea",
    ankiStep2:"Sickle_Cell_Disease::vaso-occlusive_crisis_osteomyelitis_Salmonella::penicillin_prophylaxis",
    clues:[
      "A 16-year-old African American male presents with severe bilateral leg pain, fever, and swelling of the hands and feet. He has had several similar episodes requiring hospitalization in the past.",
      "He takes daily penicillin and folic acid supplements. His spleen is not palpable — it has been gradually infarcted over years of repeated sickling episodes.",
      "Complete blood count shows hemoglobin of 7.2 g/dL, elevated reticulocyte count, and a peripheral smear with elongated, crescent-shaped red blood cells and Howell-Jolly bodies.",
      "He develops severe back pain and tenderness over his lumbar spine. MRI reveals signal changes in the vertebral bodies. Osteomyelitis is suspected. The most common causative organism in this disease is Salmonella species.",
      "The most common cause of sepsis in this population is Streptococcus pneumoniae, which is why encapsulated organism vaccines and penicillin prophylaxis are mandatory.",
      "This autosomal recessive hemoglobinopathy results from a glutamate-to-valine substitution in the beta-globin chain. Hydroxyurea (which inhibits ribonucleotide reductase and increases fetal hemoglobin) reduces crisis frequency."
    ],
    keyFacts:"Sickle cell disease: glutamate-to-valine mutation in beta-globin. Autosplenectomy by age 5 predisposes to encapsulated organism infections (most common sepsis: Streptococcus pneumoniae; most common osteomyelitis: Salmonella). Howell-Jolly bodies on smear. Treatment: hydroxyurea, penicillin prophylaxis, folate."
  },
  {
    id:116, diagnosis:"Subarachnoid Hemorrhage",
    aliases:["subarachnoid hemorrhage","SAH","berry aneurysm rupture","thunderclap headache"],
    episode:"Ep. 31", topic:"Neurology",
    ankiStep1:"Subarachnoid_Hemorrhage::berry_aneurysm_rupture::nimodipine",
    ankiStep2:"Subarachnoid_Hemorrhage::worst_headache_of_life_xanthochromia::ADPKD",
    clues:[
      "A 42-year-old woman presents to the emergency department reporting the worst headache of her life, which she describes as 'like being hit in the back of the head with a bat.' It reached maximum intensity within seconds.",
      "On examination, she has meningismus (neck stiffness) and photophobia. She is found to have bilateral flank masses on abdominal palpation noted incidentally during the exam.",
      "Non-contrast CT scan of the head is obtained immediately. It shows hyperdense (bright white) blood filling the basal cisterns and subarachnoid space.",
      "Because the CT was performed within 6 hours of symptom onset, blood is visible. If the CT had been negative, lumbar puncture would be performed looking for xanthochromia (yellow discoloration of the cerebrospinal fluid).",
      "The flank masses represent autosomal dominant polycystic kidney disease, which is associated with intracranial berry aneurysms — particularly at the anterior communicating artery.",
      "A calcium channel blocker (nimodipine) is administered to prevent cerebral vasospasm, the most feared delayed complication. This hemorrhage results from rupture of a saccular aneurysm in the circle of Willis."
    ],
    keyFacts:"Subarachnoid hemorrhage: thunderclap headache (worst of life), non-contrast CT shows hyperdense subarachnoid blood. If CT negative, lumbar puncture for xanthochromia. Associated with ADPKD and anterior communicating artery aneurysm. Nimodipine prevents vasospasm. Also associated with Marfan syndrome and Ehlers-Danlos."
  },
  {
    id:117, diagnosis:"Primary Hyperaldosteronism",
    aliases:["primary hyperaldosteronism","conn syndrome","conn's syndrome","aldosterone-producing adenoma"],
    episode:"Ep. 32", topic:"Endocrinology",
    ankiStep1:"Primary_Hyperaldosteronism::aldosterone_adenoma::spironolactone",
    ankiStep2:"Primary_Hyperaldosteronism::hypokalemia_metabolic_alkalosis_refractory_HTN::elevated_aldosterone_low_renin",
    clues:[
      "A 48-year-old woman with hypertension is referred for evaluation after her blood pressure remains uncontrolled on three antihypertensive agents. She takes lisinopril, amlodipine, and hydrochlorothiazide.",
      "She reports muscle weakness, cramps, and excessive urination. Labs show potassium of 2.9 mEq/L and bicarbonate of 31 mEq/L. She denies use of licorice or laxatives.",
      "Urine potassium-to-creatinine ratio is elevated, indicating renal potassium wasting rather than poor intake. Plasma aldosterone-to-renin ratio is markedly elevated at greater than 30.",
      "Serum aldosterone is elevated and plasma renin activity is suppressed. Adrenal CT reveals a 1.8 cm left adrenal adenoma. Adrenal vein sampling confirms left-sided excess aldosterone secretion.",
      "The aldosterone excess causes sodium and water retention (hypertension), renal potassium wasting (hypokalemia), and upregulation of the proton-ATPase pump in the collecting duct (metabolic alkalosis).",
      "This is the most common cause of secondary hypertension. The unilateral adenoma is surgically resected (adrenalectomy). Medical management with spironolactone or eplerenone is used for bilateral adrenal hyperplasia."
    ],
    keyFacts:"Primary hyperaldosteronism (Conn syndrome): hypokalemia + metabolic alkalosis + refractory hypertension. Elevated aldosterone, suppressed renin. Adrenal CT for adenoma; adrenal vein sampling for lateralization. Unilateral adenoma: adrenalectomy. Bilateral hyperplasia: spironolactone."
  },
  {
    id:118, diagnosis:"Herniated Nucleus Pulposus",
    aliases:["herniated nucleus pulposus","herniated disk","herniated disc","disc herniation","lumbar radiculopathy"],
    episode:"Ep. 32", topic:"Orthopedics / Neurology",
    ankiStep1:"Herniated_Disk::nucleus_pulposus_nerve_root_compression::physical_therapy_NSAIDs",
    ankiStep2:"Herniated_Disk::L5-S1_loss_Achilles_reflex::positive_straight_leg_raise",
    clues:[
      "A 45-year-old man who works as a warehouse manager presents with sudden-onset severe low back pain that radiates down his right leg to the heel and lateral foot following an episode of heavy lifting.",
      "The pain is sharp and burning, described as shooting 'like electricity' down the leg. It is worsened by coughing, sneezing, and the Valsalva maneuver.",
      "On examination, straight leg raise on the right reproduces the radiating leg pain at 40 degrees of hip flexion — a positive test indicating nerve root irritation.",
      "The Achilles tendon reflex is absent on the right. Plantar flexion strength is decreased. Sensation is diminished along the lateral foot and heel.",
      "The absent Achilles reflex and sensory loss in the lateral foot localizes the lesion to the S1 nerve root. The level of disc herniation is L5-S1 — the most commonly herniated level.",
      "Herniation of the nucleus pulposus compresses the adjacent nerve root. First-line treatment is physical therapy and NSAIDs. Bedrest and immediate surgery are not recommended. Surgery is reserved for progressive neurologic deficits or cauda equina syndrome."
    ],
    keyFacts:"Herniated disk: radicular pain worsened by Valsalva, positive straight leg raise. L5-S1 herniation: absent Achilles reflex, S1 distribution sensory loss, plantar flexion weakness. L4 herniation: absent patellar reflex. Treatment: physical therapy + NSAIDs. Surgery only for cauda equina or progressive deficit."
  },
  {
    id:119, diagnosis:"Pulmonary Arterial Hypertension",
    aliases:["pulmonary arterial hypertension","pulmonary hypertension","idiopathic pulmonary arterial hypertension","PAH"],
    episode:"Ep. 32", topic:"Pulmonology / Cardiology",
    ankiStep1:"Pulmonary_Arterial_Hypertension::BMPR2_mutation::endothelin_antagonists",
    ankiStep2:"Pulmonary_Arterial_Hypertension::loud_S2_right_heart_failure::right_heart_catheterization",
    clues:[
      "A 28-year-old woman presents with 8 months of progressive exertional dyspnea, fatigue, and near-syncope with exertion. She has no history of cardiopulmonary disease and is a non-smoker.",
      "Physical examination reveals a loud, accentuated pulmonic component of the second heart sound (P2). Jugular venous distension, right ventricular heave, and peripheral edema are noted.",
      "Echocardiogram shows right ventricular enlargement and hypertrophy with flattening of the interventricular septum (a D-shaped left ventricle). Estimated right ventricular systolic pressure is 68 mmHg.",
      "Right heart catheterization confirms a mean pulmonary artery pressure of 38 mmHg at rest — the diagnostic gold standard (cutoff is greater than 25 mmHg). Left heart pressures are normal.",
      "Genetic testing reveals a BMPR2 (bone morphogenetic protein receptor type 2) gene mutation. This is the most common genetic mutation in familial and idiopathic forms of this condition.",
      "This young woman has idiopathic pulmonary arterial hypertension, more common in females. Treatment includes endothelin receptor antagonists (bosentan, ambrisentan), phosphodiesterase-5 inhibitors (sildenafil), and prostacyclin analogs (epoprostenol)."
    ],
    keyFacts:"Pulmonary arterial hypertension: loud P2, right heart failure in young women. Diagnosed by right heart catheterization (mean PA pressure over 25 mmHg). BMPR2 mutation. Treatment: endothelin antagonists (bosentan), PDE-5 inhibitors (sildenafil), prostacyclins (epoprostenol). Most common cause overall: COPD."
  },
  {
    id:120, diagnosis:"Parkinson Disease",
    aliases:["parkinson disease","parkinson's disease","parkinsonism","substantia nigra degeneration"],
    episode:"Ep. 45", topic:"Neurology",
    ankiStep1:"Parkinson_Disease::substantia_nigra_dopamine_loss::lewy_bodies_alpha_synuclein",
    ankiStep2:"Parkinson_Disease::resting_tremor_cogwheel_rigidity_bradykinesia::levodopa_carbidopa",
    clues:[
      "A 68-year-old man is brought in by his wife who reports he has been walking with small shuffling steps, has difficulty initiating movement, and seems to have lost his arm swing while walking.",
      "On examination, there is a pill-rolling tremor of the right hand at rest that diminishes with voluntary movement. Passive range of motion of the wrist reveals a ratchet-like resistance.",
      "He has a mask-like facial expression with reduced blinking. His handwriting has become progressively smaller (micrographia). His wife notes he speaks more softly than before.",
      "The patient has orthostatic hypotension. Brain MRI is normal. The substantia nigra shows loss of its normal black pigmentation on gross pathology when examined at autopsy in similar cases.",
      "Histology of affected neurons reveals eosinophilic intracytoplasmic inclusions composed of alpha-synuclein protein (Lewy bodies). Dopamine-producing neurons in the substantia nigra pars compacta are selectively destroyed.",
      "This is the second most common neurodegenerative disease. Initial treatment options include dopamine agonists (pramipexole, ropinirole), MAO-B inhibitors (selegiline), amantadine, or COMT inhibitors. Levodopa-carbidopa is the most effective but saved for later due to the on-off phenomenon."
    ],
    keyFacts:"Parkinson disease: asymmetric resting pill-rolling tremor, cogwheel rigidity, bradykinesia, postural instability. Micrographia, hypophonia, masked facies. Lewy bodies (alpha-synuclein) in substantia nigra. Treatment: dopamine agonists, MAO-B inhibitors, amantadine, COMT inhibitors, levodopa-carbidopa (last line). Essential tremor worsens with movement and responds to propranolol."
  },
  {
    id:121, diagnosis:"Narcolepsy",
    aliases:["narcolepsy","narcolepsy with cataplexy","hypocretin deficiency","orexin deficiency"],
    episode:"Ep. 45", topic:"Neurology / Sleep Medicine",
    ankiStep1:"Narcolepsy::hypocretin_orexin_deficiency::modafinil_sodium_oxybate",
    ankiStep2:"Narcolepsy::cataplexy_sleep_paralysis_hallucinations::polysomnography_decreased_latency",
    clues:[
      "A 24-year-old man presents after his third minor motor vehicle accident caused by falling asleep at the wheel. He reports feeling an irresistible urge to sleep throughout the day despite 8 hours of nighttime sleep.",
      "He describes episodes of sudden bilateral leg weakness and collapse triggered by laughing or excitement — he once collapsed at a party when his team scored a goal. He remains conscious during these episodes.",
      "He also experiences vivid dream-like hallucinations while falling asleep (hypnagogic) and upon awakening (hypnopompic). Occasionally, he awakens fully alert but cannot move any muscle for 1-2 minutes.",
      "Polysomnography reveals an abnormally short sleep-onset rapid eye movement period (less than 15 minutes, normally greater than 90 minutes). Multiple Sleep Latency Test shows mean sleep onset under 8 minutes with two sleep-onset rapid eye movement periods.",
      "Cerebrospinal fluid analysis reveals markedly reduced hypocretin (orexin) levels — a neuropeptide produced by lateral hypothalamic neurons that promotes wakefulness.",
      "This sleep disorder is characterized by the tetrad of excessive daytime sleepiness, cataplexy, hypnagogic/hypnopompic hallucinations, and sleep paralysis. Treatment: modafinil (stimulant) for sleepiness; sodium oxybate for cataplexy."
    ],
    keyFacts:"Narcolepsy: excessive daytime sleepiness + cataplexy (emotion-triggered muscle weakness) + hypnagogic/hypnopompic hallucinations + sleep paralysis. Low CSF hypocretin/orexin. Polysomnography shows decreased REM latency. Treatment: modafinil for sleepiness, sodium oxybate for cataplexy."
  },
  {
    id:122, diagnosis:"Huntington Disease",
    aliases:["huntington disease","huntington's disease","huntington chorea","CAG repeat expansion"],
    episode:"Ep. 45", topic:"Neurology / Genetics",
    ankiStep1:"Huntington_Disease::CAG_repeat_chromosome_4_AD::caudate_atrophy",
    ankiStep2:"Huntington_Disease::choreiform_movements_behavioral_changes::tetrabenazine",
    clues:[
      "A 41-year-old man is brought by his wife for 14 months of progressive personality changes, irritability, and forgetfulness. His father died at age 46 after similar symptoms, and his paternal uncle was also affected.",
      "On examination, he has involuntary, irregular, dance-like movements of his face, trunk, and limbs that he cannot suppress. He makes frequent grimacing expressions he is not aware of.",
      "Neuropsychological testing reveals frontal lobe-type cognitive decline. He has poor impulse control and a flat affect. Depression is present.",
      "MRI brain shows marked atrophy of the caudate nuclei bilaterally, causing widening of the lateral ventricles with a 'butterfly' configuration on coronal views.",
      "Genetic testing reveals an expansion of CAG trinucleotide repeats greater than 36 on chromosome 4. The disease shows anticipation — each successive generation may develop symptoms earlier.",
      "This autosomal dominant neurodegenerative disorder has no disease-modifying treatment. Choreiform movements are treated with tetrabenazine (vesicular monoamine transporter 2 inhibitor) or haloperidol."
    ],
    keyFacts:"Huntington disease: CAG repeat expansion on chromosome 4, autosomal dominant with anticipation. Triad of chorea, behavioral changes, and dementia. Caudate atrophy on MRI. Treatment: tetrabenazine or haloperidol for chorea (dopamine depletion/blockade). Genetic counseling is critical."
  },
  {
    id:123, diagnosis:"Meningococcal Meningitis",
    aliases:["meningococcal meningitis","neisseria meningitidis meningitis","bacterial meningitis with petechiae","meningococcemia"],
    episode:"Ep. 45", topic:"Infectious Disease / Neurology",
    ankiStep1:"Meningococcal_Meningitis::Neisseria_meningitidis::rifampin_prophylaxis",
    ankiStep2:"Meningococcal_Meningitis::petechiae_purpura_meningismus::ceftriaxone",
    clues:[
      "A 20-year-old college student in a dormitory presents to the emergency department with sudden-onset high fever, severe headache, stiff neck, and photophobia that began within hours.",
      "Physical examination reveals a rapidly spreading non-blanching petechial and purpuric rash covering the trunk and extremities. Kernig and Brudzinski signs are both positive.",
      "Lumbar puncture reveals turbid cerebrospinal fluid under elevated opening pressure. Cerebrospinal fluid analysis shows white blood cell count of 2,800 (90% neutrophils), low glucose (28 mg/dL), and very high protein.",
      "Gram stain of the cerebrospinal fluid reveals gram-negative diplococci both intra- and extracellularly. Blood cultures are obtained and antibiotics are started immediately.",
      "Close contacts (including roommates) are given prophylaxis with rifampin, ciprofloxacin, or ceftriaxone. Pregnant close contacts receive ceftriaxone (rifampin and ciprofloxacin are contraindicated in pregnancy).",
      "This Gram-negative diplococcus is the most feared cause of bacterial meningitis in young adults, particularly in close-contact settings, due to its rapid progression to septicemia and death. Treatment: ceftriaxone plus vancomycin plus dexamethasone."
    ],
    keyFacts:"Meningococcal meningitis: fever + headache + petechial/purpuric rash + meningismus in young adults/college settings. Gram-negative diplococci in CSF. Most common cause of meningitis with purpura. Prophylaxis for close contacts: rifampin, ciprofloxacin, or ceftriaxone. Treatment: ceftriaxone + vancomycin + dexamethasone."
  },
  {
    id:124, diagnosis:"Mucormycosis",
    aliases:["mucormycosis","rhinocerebral mucormycosis","zygomycosis","mucor infection"],
    episode:"Ep. 31", topic:"Infectious Disease",
    ankiStep1:"Mucormycosis::aseptate_hyphae_90_degrees::DKA_diabetic",
    ankiStep2:"Mucormycosis::black_necrotic_eschar_sinusitis::amphotericin_B_debridement",
    clues:[
      "A 54-year-old woman with poorly controlled type 1 diabetes presents with severe facial pain, fever, and a black discoloration of her left nostril extending to the hard palate. She has been in diabetic ketoacidosis for the past 48 hours.",
      "CT of the sinuses shows opacification of the left maxillary and ethmoid sinuses with erosion of the adjacent bone. The black necrotic tissue extends through the nasal septum.",
      "Emergency nasal endoscopy reveals black eschar covering the left nasal turbinates. A biopsy is taken from the necrotic tissue and sent for histopathology.",
      "Histology reveals broad, ribbon-like, non-septate (aseptate) hyphae that branch at near right angles (90 degrees). This distinguishes this organism from Aspergillus, which has septate hyphae branching at 45-degree angles.",
      "The organism thrives in the acidic, high-glucose, iron-rich environment of diabetic ketoacidosis. It invades blood vessels, causing thrombosis and the characteristic black ischemic necrosis.",
      "This life-threatening angioinvasive fungal infection occurs predominantly in diabetic ketoacidosis and neutropenic patients. Treatment requires aggressive surgical debridement of all necrotic tissue plus intravenous liposomal amphotericin B."
    ],
    keyFacts:"Mucormycosis: black necrotic eschar in DKA patient. Aseptate hyphae branching at 90 degrees (vs. Aspergillus: septate, 45 degrees). Angioinvasive causing thrombosis. Treatment: emergency surgical debridement + amphotericin B. Also occurs in neutropenia and iron overload."
  },
  {
    id:125, diagnosis:"Cryptococcal Meningitis",
    aliases:["cryptococcal meningitis","cryptococcus neoformans meningitis","cryptococcosis"],
    episode:"Ep. 263", topic:"Infectious Disease / Neurology",
    ankiStep1:"Cryptococcal_Meningitis::india_ink_latex_agglutination::amphotericin_B_flucytosine",
    ankiStep2:"Cryptococcal_Meningitis::HIV_CD4_under_100_very_high_opening_pressure::fluconazole_maintenance",
    clues:[
      "A 32-year-old intravenous drug user with HIV presents with 3 days of progressively worsening headache, fever, and neck stiffness. He is not on antiretroviral therapy. His CD4 count is 42 cells/mcL.",
      "He appears drowsy and has mild photophobia. The headache is described as diffuse, severe, and constant. He has mild papilledema on fundoscopic exam.",
      "Lumbar puncture reveals opening pressure of 320 mmHg (very elevated). Cerebrospinal fluid shows high protein, very low glucose (approximately 10 mg/dL), and lymphocytic pleocytosis.",
      "India ink stain of the cerebrospinal fluid reveals encapsulated yeast with a large polysaccharide capsule surrounded by a clear halo. Latex agglutination test for cryptococcal antigen is strongly positive.",
      "The organism is acquired by inhalation of bird droppings (especially pigeon droppings) and causes disseminated infection in severely immunocompromised patients.",
      "This is the most common cause of meningitis in HIV-infected patients with CD4 under 100. Induction treatment is amphotericin B plus flucytosine for 2 weeks, followed by fluconazole consolidation for 8 weeks, then lifelong fluconazole prophylaxis."
    ],
    keyFacts:"Cryptococcal meningitis: most common meningitis in HIV patients with CD4 under 100. India ink stain shows encapsulated yeast. Latex agglutination for cryptococcal antigen is diagnostic. Very elevated CSF opening pressure. Treatment: amphotericin B + flucytosine induction, then fluconazole maintenance."
  },
  {
    id:126, diagnosis:"Neurocysticercosis",
    aliases:["neurocysticercosis","taenia solium brain infection","cysticercosis"],
    episode:"Ep. 263", topic:"Infectious Disease / Neurology",
    ankiStep1:"Neurocysticercosis::Taenia_solium_eggs::albendazole_steroids",
    ankiStep2:"Neurocysticercosis::seizures_calcified_brain_cysts::pork_tapeworm_eggs",
    clues:[
      "A 34-year-old immigrant from a rural area of Mexico presents to the neurology clinic after his second generalized tonic-clonic seizure in 3 months. He has no prior history of epilepsy.",
      "The patient reports no symptoms between seizures. He grew up on a farm where pigs were raised and slaughtered for food. He sometimes ate undercooked pork.",
      "MRI brain reveals multiple ring-enhancing cystic lesions throughout the cerebral cortex, some with a small eccentric hyperdense nodule (the scolex) visible inside — a pathognomonic 'hole with a dot' appearance.",
      "Some lesions appear calcified on CT scan, representing dead or dying parasites surrounded by calcium deposits. Perilesional edema is present, which explains the seizures.",
      "Importantly, the infection is acquired by ingesting eggs (not cysts) shed in the feces of a tapeworm-infected human — not from eating pork directly. The larvae then hatch and migrate to the brain.",
      "This parasitic brain infection is the leading cause of acquired epilepsy worldwide. Treatment: albendazole or mebendazole plus corticosteroids (to reduce inflammation from dying cysts) plus antiepileptic drugs."
    ],
    keyFacts:"Neurocysticercosis: most common cause of acquired epilepsy worldwide. Caused by Taenia solium egg ingestion (fecal-oral route, not pork directly). MRI shows ring-enhancing cysts with scolex ('hole with a dot'). Calcified lesions = chronic/dead cysts. Treatment: albendazole + steroids + antiepileptics."
  },
  {
    id:127, diagnosis:"Naegleria Fowleri Meningoencephalitis",
    aliases:["naegleria fowleri","primary amebic meningoencephalitis","PAM","fatal freshwater meningitis"],
    episode:"Ep. 263", topic:"Infectious Disease / Neurology",
    ankiStep1:"Naegleria_Fowleri::freshwater_swimming::amphotericin_B_fatal",
    ankiStep2:"Naegleria_Fowleri::frontal_lobe_amebic_encephalitis::olfactory_nerve_route",
    clues:[
      "A 14-year-old boy presents in August with sudden-onset fever, severe headache, stiff neck, and confusion 5 days after swimming in a warm freshwater lake in the southern United States.",
      "He is profoundly ill, combative, and has olfactory hallucinations (smelling things that are not there). He progresses to coma within 24 hours of hospital admission.",
      "Lumbar puncture shows markedly elevated opening pressure, hemorrhagic cerebrospinal fluid, elevated white blood cells with neutrophilic predominance, very low glucose, and high protein.",
      "Fresh examination of the cerebrospinal fluid under light microscopy reveals mobile trophozoites — the pathognomonic finding that distinguishes this infection from bacterial meningitis.",
      "The organism enters through the nasal mucosa during water forcefully entering the nose (as occurs with diving or swimming). It migrates along the olfactory nerve through the cribriform plate to reach the brain, causing necrosis of the frontal lobe.",
      "This rapidly fatal infection has a mortality rate near 100% despite treatment with amphotericin B, miltefosine, and azithromycin. Prevention involves avoiding diving into warm freshwater or wearing nose clips."
    ],
    keyFacts:"Naegleria fowleri: rapidly fatal primary amebic meningoencephalitis from warm freshwater exposure. Travels via olfactory nerve through cribriform plate. Mobile trophozoites in CSF. Frontal lobe necrosis. Near 100% fatal. Treatment attempted with amphotericin B + miltefosine."
  },
  {
    id:128, diagnosis:"African Sleeping Sickness",
    aliases:["african sleeping sickness","african trypanosomiasis","trypanosoma brucei","tsetse fly infection"],
    episode:"Ep. 263", topic:"Infectious Disease",
    ankiStep1:"African_Sleeping_Sickness::Trypanosoma_brucei_tsetse_fly::melarsoprol_CNS",
    ankiStep2:"African_Sleeping_Sickness::somnolence_weight_loss_elevated_IgM::pentamidine",
    clues:[
      "A 26-year-old graduate student presents 10 days after returning from a 3-month research trip to sub-Saharan Africa. He reports progressive somnolence, confusion, and refusal to eat over the past week.",
      "He recalls a painful insect bite on his forearm early in the trip, followed by a red, indurated skin lesion (chancre) that resolved spontaneously. He then developed fever, headaches, and swollen lymph nodes in the posterior cervical chain (Winterbottom's sign).",
      "He is now somnolent during the day and awake at night — a classic reversal of the normal sleep-wake cycle. He has lost 15 pounds since returning home.",
      "Lumbar puncture is performed. Cerebrospinal fluid shows markedly elevated immunoglobulin M antibodies against the pathogen. Trypomastigotes are visible on Giemsa-stained blood smears and cerebrospinal fluid.",
      "The organism is transmitted by the bite of the tsetse fly and initially causes a bloodstream infection (stage 1), then invades the central nervous system (stage 2) causing progressive encephalitis and death from starvation.",
      "Stage 1 (bloodstream only) is treated with pentamidine. Stage 2 (central nervous system involvement) requires melarsoprol, an arsenic-containing compound that crosses the blood-brain barrier."
    ],
    keyFacts:"African sleeping sickness: Trypanosoma brucei transmitted by tsetse fly. Stage 1: fever, lymphadenopathy (Winterbottom's sign), skin chancre. Stage 2: CNS invasion with sleep-wake reversal, somnolence, progressive encephalitis. Elevated CSF IgM. Stage 1 treatment: pentamidine. Stage 2 treatment: melarsoprol."
  },
  {
    id:129, diagnosis:"Viral Meningitis",
    aliases:["viral meningitis","aseptic meningitis","enteroviral meningitis","benign meningitis"],
    episode:"Ep. 263", topic:"Infectious Disease / Neurology",
    ankiStep1:"Viral_Meningitis::enterovirus_most_common::supportive_care",
    ankiStep2:"Viral_Meningitis::normal_glucose_lymphocytosis_normal_opening_pressure::summer_season",
    clues:[
      "A 22-year-old college student presents in July with 3 days of headache, fever, and mild neck stiffness. He is alert and oriented, mildly uncomfortable, but able to carry on a conversation.",
      "He had a gastrointestinal illness with diarrhea and vomiting one week prior. Several of his friends developed similar symptoms. There are no skin lesions or focal neurological deficits.",
      "Lumbar puncture reveals clear cerebrospinal fluid under normal opening pressure (180 mmHg). White blood cell count is 85 with 90% lymphocytes. Glucose is 58 mg/dL (normal). Protein is mildly elevated at 65 mg/dL.",
      "Gram stain and bacterial cultures are negative. Cerebrospinal fluid polymerase chain reaction is sent for enteroviruses, herpes simplex virus, and varicella-zoster virus.",
      "The polymerase chain reaction returns positive for enterovirus. This is the most common cause of viral (aseptic) meningitis, particularly in summer and fall outbreaks.",
      "Unlike bacterial meningitis, this condition is characterized by normal cerebrospinal fluid glucose, normal opening pressure, lymphocytic pleocytosis, and clinical benignity. Treatment is supportive care; full recovery is expected."
    ],
    keyFacts:"Viral meningitis: most common cause is enterovirus. CSF shows lymphocytic pleocytosis, NORMAL glucose, normal or mildly elevated opening pressure — distinguishes from bacterial (low glucose, neutrophilic, high pressure). Summer/fall epidemics. Treatment: supportive care only."
  },
  {
    id:130, diagnosis:"Actinic Keratosis",
    aliases:["actinic keratosis","solar keratosis","precancerous skin lesion","sun-damaged keratinocytes"],
    episode:"Ep. 102", topic:"Dermatology / Oncology",
    ankiStep1:"Actinic_Keratosis::UV_radiation_precancer::topical_5-fluorouracil",
    ankiStep2:"Actinic_Keratosis::rough_scaly_plaques_sun_exposed::squamous_cell_carcinoma_precursor",
    clues:[
      "A 68-year-old farmer who has worked outdoors without sun protection for 40 years presents with multiple rough, scaly, erythematous plaques on his forehead, cheeks, and dorsal forearms.",
      "The lesions feel like sandpaper on palpation. Individual lesions are 3-10 mm in diameter with a gritty surface that partially removes with gentle scraping but recurs. Some lesions are mildly pruritic.",
      "He has a history of several prior lesions that were removed from his face. His skin shows diffuse solar damage with telangiectasias and uneven pigmentation.",
      "Dermoscopy reveals a characteristic pattern of erythema and scaling. Biopsy of one lesion shows dysplasia of keratinocytes confined to the epidermis with a background of solar elastosis.",
      "This is the most common premalignant skin lesion. The primary risk factor is cumulative ultraviolet radiation exposure. Without treatment, approximately 5-10% of lesions progress over years.",
      "This precancerous condition is a precursor to squamous cell carcinoma of the skin. Most lesions self-resolve; treatment options include topical 5-fluorouracil, imiquimod, cryotherapy with liquid nitrogen, or photodynamic therapy."
    ],
    keyFacts:"Actinic keratosis: rough, scaly plaques on sun-exposed skin. Number-one risk factor is UV exposure. Precursor to squamous cell carcinoma. Treatment: topical 5-fluorouracil, imiquimod, cryotherapy. Unlike melanoma, actinic keratosis is NOT a melanocytic lesion."
  },
  {
    id:131, diagnosis:"Melanoma",
    aliases:["melanoma","malignant melanoma","cutaneous melanoma","acral lentiginous melanoma"],
    episode:"Ep. 102", topic:"Dermatology / Oncology",
    ankiStep1:"Melanoma::ABCDE_criteria::Breslow_depth_prognosis",
    ankiStep2:"Melanoma::UV_radiation_BRAF_mutation::wide_local_excision_sentinel_lymph_node",
    clues:[
      "A 52-year-old woman with a history of multiple severe sunburns as a teenager presents with a changing skin lesion on her left shoulder that her husband noticed has darkened and developed an irregular border over 8 months.",
      "Examination reveals a 9 mm pigmented lesion with asymmetry, irregularly notched borders, multiple shades of brown, black, and pink within the same lesion, and an area of central ulceration.",
      "Dermoscopy reveals atypical pigment network, regression structures, and irregular vascular patterns. The lesion is biopsied via excision biopsy with 1 mm margins for pathologic assessment.",
      "Histology reveals malignant melanocytes with nuclear atypia and pagetoid spread through the epidermis. The Breslow thickness is 2.1 mm, which is the single most important prognostic factor.",
      "Sentinel lymph node biopsy of the axillary nodes is performed given the Breslow thickness greater than 1 mm. One of three nodes contains micrometastatic disease.",
      "This malignant tumor of melanocytes carries the worst prognosis of all skin cancers. In African Americans, the acral lentiginous subtype occurs on the palms, soles, and nail beds. Treatment: wide local excision; advanced disease uses BRAF inhibitors (vemurafenib) if BRAF V600E mutation is present."
    ],
    keyFacts:"Melanoma: ABCDE criteria (Asymmetry, Border irregularity, Color variation, Diameter over 6 mm, Evolution). Breslow depth is the primary prognostic factor. Acral lentiginous type is most common in dark-skinned individuals (palms, soles, nailbeds). BRAF V600E mutation in 50% of cases. Treatment: wide local excision."
  },
  {
    id:132, diagnosis:"Basal Cell Carcinoma",
    aliases:["basal cell carcinoma","basal cell cancer","pearly papule skin cancer","rodent ulcer"],
    episode:"Ep. 102", topic:"Dermatology / Oncology",
    ankiStep1:"Basal_Cell_Carcinoma::UV_radiation_PTCH1_mutation::Mohs_surgery",
    ankiStep2:"Basal_Cell_Carcinoma::pearly_gray_papule_telangiectasia::locally_invasive_low_metastasis",
    clues:[
      "A 71-year-old retired lifeguard presents for evaluation of a slowly growing lesion on his right upper lip that has been present for 2 years. He had multiple blistering sunburns as a young man.",
      "On examination, there is a 7 mm pearly gray, translucent papule with rolled edges and prominent dilated blood vessels (telangiectasias) coursing over the surface. The center is slightly depressed.",
      "The lesion does not heal and occasionally bleeds with minor trauma. The patient applied topical steroids for 6 months without improvement.",
      "A punch biopsy reveals nests and cords of basaloid cells extending from the epidermis into the dermis, with characteristic peripheral palisading of nuclei and artifactual retraction clefts.",
      "This is the most common skin cancer in fair-skinned individuals. It is locally invasive and destructive but has an extremely low rate of distant metastasis (less than 0.1%).",
      "Treatment for small lesions is Mohs micrographic surgery, which achieves the highest cure rates while preserving healthy tissue. Vismodegib (hedgehog pathway inhibitor targeting PTCH1/Smoothened) is used for advanced disease."
    ],
    keyFacts:"Basal cell carcinoma: most common skin cancer. Pearly gray papule with telangiectasias and rolled edges on sun-exposed skin. PTCH1 mutation (hedgehog pathway). Locally invasive, rarely metastasizes. Treatment: Mohs surgery. Vismodegib for advanced disease."
  },
  {
    id:133, diagnosis:"Neuroblastoma",
    aliases:["neuroblastoma","adrenal neuroblastoma","N-myc amplification","sympathetic nervous system tumor"],
    episode:"Ep. 102", topic:"Pediatric Oncology",
    ankiStep1:"Neuroblastoma::N-myc_Homer-Wright_pseudorosettes::opsoclonus_myoclonus",
    ankiStep2:"Neuroblastoma::2yo_abdominal_mass_crosses_midline_calcifications::urinary_catecholamines",
    clues:[
      "A 2-year-old boy is brought to his pediatrician because his parents noticed his abdomen looks enlarged. He has also been irritable and has had fevers for 2 weeks.",
      "Blood pressure is 148/92 mmHg. On palpation, a large, firm, non-tender abdominal mass is identified that crosses the midline. The mass is hard and irregular.",
      "CT scan of the abdomen reveals a large heterogeneous mass arising from the right adrenal gland with calcifications throughout. The mass encases the aorta and crosses the midline. Chest CT shows pulmonary nodules consistent with metastatic disease.",
      "His parents notice he has been making unusual dancing eye movements (opsoclonus) and jerky arm movements (myoclonus) — a paraneoplastic phenomenon associated with this tumor.",
      "Urine studies show elevated vanillylmandelic acid and homovanillic acid — metabolites of catecholamines released by the tumor. Bone marrow biopsy shows tumor involvement.",
      "This is the most common extracranial solid tumor of childhood, arising from neural crest cells of the sympathetic nervous system. Histology: Homer-Wright pseudorosettes. Oncogene: N-myc amplification (poor prognosis). Contrast with Wilms tumor: does NOT cross midline, NOT calcified."
    ],
    keyFacts:"Neuroblastoma: most common extracranial solid tumor in children under 5. Arises from adrenal medulla or sympathetic chain. Mass crosses midline with calcifications. Elevated urinary VMA/HVA. Homer-Wright pseudorosettes on histology. N-myc amplification = poor prognosis. Opsoclonus-myoclonus paraneoplastic syndrome."
  },
  {
    id:134, diagnosis:"Wilms Tumor",
    aliases:["wilms tumor","nephroblastoma","WAGR syndrome","wilms' tumor"],
    episode:"Ep. 102", topic:"Pediatric Oncology",
    ankiStep1:"Wilms_Tumor::WT1_mutation_WAGR::nephroblastoma",
    ankiStep2:"Wilms_Tumor::unilateral_flank_mass_no_midline_crossing::hypertension_child",
    clues:[
      "A 3-year-old girl is brought to her pediatrician by her parents who noticed her abdomen has been growing larger over the past month while she has appeared healthy and playful otherwise.",
      "On examination, blood pressure is 140/90 mmHg. Abdominal palpation reveals a large, smooth, non-tender mass in the right flank. The mass does NOT cross the midline and is NOT hard or calcified.",
      "Urinalysis shows microscopic hematuria. CT abdomen with contrast shows a well-circumscribed intrarenal mass arising from the right kidney with clear distortion of the normal renal architecture.",
      "The physician notes the child has absent irises bilaterally (aniridia) and a left-sided undescended testicle in a sibling. This constellation suggests a specific genetic deletion syndrome.",
      "Histology of the resected specimen reveals triphasic morphology: blastema (small blue cells), stroma, and epithelial elements (tubular structures) — the classic appearance of this embryonal kidney tumor.",
      "WAGR syndrome (Wilms tumor, aniridia, genitourinary anomalies, and intellectual disability) results from deletion of chromosome 11p13 (WT1 gene). This is the most common renal tumor in children. Treatment: nephrectomy plus chemotherapy, with excellent prognosis."
    ],
    keyFacts:"Wilms tumor (nephroblastoma): most common renal tumor in children. Smooth unilateral flank mass that does NOT cross midline, no calcifications. WT1 gene deletion (11p13). WAGR syndrome (Wilms, Aniridia, Genitourinary anomalies, intellectual Retardation). Triphasic histology. Treatment: nephrectomy + chemotherapy."
  },
  {
    id:135, diagnosis:"Prolactinoma",
    aliases:["prolactinoma","prolactin-secreting pituitary adenoma","hyperprolactinemia","galactorrhea amenorrhea syndrome"],
    episode:"Ep. 102", topic:"Endocrinology / Neurology",
    ankiStep1:"Prolactinoma::dopamine_agonist_bromocriptine::bitemporal_hemianopsia",
    ankiStep2:"Prolactinoma::galactorrhea_amenorrhea_infertility::medical_management_first",
    clues:[
      "A 29-year-old woman presents with 8 months of amenorrhea, milky nipple discharge from both breasts despite not being pregnant or breastfeeding, and difficulty conceiving.",
      "She also reports bilateral temporal visual field narrowing that she first noticed while driving — she has had two near-collisions when cars in adjacent lanes were not visible to her.",
      "Serum prolactin level is 320 ng/mL (normal under 25 ng/mL). Thyroid-stimulating hormone is normal (ruling out hypothyroidism as a secondary cause of elevated prolactin). Beta-human chorionic gonadotropin is negative.",
      "MRI of the pituitary gland with gadolinium contrast shows a 14 mm macroadenoma arising from the pituitary gland that compresses the optic chiasm from below, explaining the bitemporal hemianopsia.",
      "Unlike other pituitary adenomas (growth hormone-secreting, adrenocorticotropic hormone-secreting), this tumor type is managed medically FIRST — surgery is NOT the initial treatment.",
      "This is the most common pituitary adenoma. Dopamine (prolactin-inhibiting factor) normally suppresses prolactin release. Treatment with cabergoline or bromocriptine (dopamine agonists) reduces tumor size and normalizes prolactin in over 80% of cases."
    ],
    keyFacts:"Prolactinoma: most common pituitary adenoma. Galactorrhea + amenorrhea + infertility in women; hypogonadism + erectile dysfunction in men. Bitemporal hemianopsia if macroadenoma compresses chiasm. Prolactin often over 200. First-line treatment: cabergoline or bromocriptine (NOT surgery first). Surgery reserved for failure of medical therapy."
  },
  {
    id:136, diagnosis:"Craniopharyngioma",
    aliases:["craniopharyngioma","rathke pouch tumor","suprasellar calcified mass"],
    episode:"Ep. 102", topic:"Pediatric Neurology / Oncology",
    ankiStep1:"Craniopharyngioma::Rathke_pouch::cholesterol_motor_oil_fluid",
    ankiStep2:"Craniopharyngioma::calcified_suprasellar_mass_bitemporal_hemianopsia::childhood_tumor",
    clues:[
      "A 10-year-old boy is brought to his pediatrician because of worsening headaches, declining school performance, and his parents' concern that he seems shorter than his classmates his age.",
      "Fundoscopic examination reveals bilateral papilledema. Visual field testing shows bitemporal hemianopsia. Endocrine evaluation shows growth hormone deficiency and central hypothyroidism.",
      "CT scan of the brain reveals a calcified suprasellar mass with both cystic and solid components arising above the sella turcica. MRI confirms the lesion is intimately related to the hypothalamus and optic chiasm.",
      "The cystic component of the tumor contains dark brown-green fluid with a characteristic motor-oil or machine-oil appearance when aspirated. This fluid contains cholesterol crystals.",
      "Histology reveals squamous epithelium with palisading at the periphery, central stellate cells, and dystrophic calcifications — the classic features of this benign but locally destructive tumor.",
      "This tumor arises from remnants of Rathke's pouch (the embryonic structure that gives rise to the anterior pituitary). Despite being histologically benign, it damages the hypothalamic-pituitary axis, causing panhypopituitarism. Treatment: surgical resection."
    ],
    keyFacts:"Craniopharyngioma: most common suprasellar tumor in children. Arises from Rathke's pouch remnants. Calcified cystic mass with cholesterol-rich 'motor oil' fluid. Bitemporal hemianopsia + panhypopituitarism + growth failure. Histology: squamous epithelium with palisading. Treatment: surgical resection."
  },
  {
    id:137, diagnosis:"Osteosarcoma",
    aliases:["osteosarcoma","osteogenic sarcoma","bone sarcoma","sunburst pattern bone tumor"],
    episode:"Ep. 102", topic:"Orthopedic Oncology",
    ankiStep1:"Osteosarcoma::codman_triangle_sunburst::retinoblastoma_pagets_teriparatide",
    ankiStep2:"Osteosarcoma::metaphysis_distal_femur_bone_pain_fever::neoadjuvant_chemotherapy",
    clues:[
      "A 15-year-old boy presents with 3 months of worsening pain and swelling around his right knee that initially he attributed to a sports injury. The pain is now constant and wakes him from sleep.",
      "He has a low-grade fever and a firm, tender swelling just above the right knee that is fixed to the underlying bone. The overlying skin is warm. He has lost 8 pounds.",
      "Plain radiograph of the right distal femur reveals an aggressive lytic and sclerotic lesion in the metaphysis with elevation of the periosteum forming a triangular shadow at the tumor margins (Codman's triangle) and spicules of new bone radiating outward in a sunburst pattern.",
      "MRI shows the lesion has not penetrated the cortex. CT chest shows two small pulmonary nodules, the most common site of metastatic disease.",
      "Biopsy reveals malignant mesenchymal cells producing osteoid matrix. Risk factors for this tumor include retinoblastoma (Rb gene mutation), Paget's disease of bone, and teriparatide (parathyroid hormone analog) administration.",
      "This is the most common primary malignant bone tumor, predominantly affecting adolescents in the metaphysis of long bones (distal femur most common). Treatment: neoadjuvant chemotherapy (methotrexate, cisplatin, doxorubicin) followed by limb-sparing surgery, then adjuvant chemotherapy."
    ],
    keyFacts:"Osteosarcoma: most common primary bone malignancy in adolescents. Metaphysis of distal femur most common location. Codman's triangle + sunburst pattern on X-ray. Pulmonary metastases most common. Risk factors: retinoblastoma (Rb mutation), Paget's disease, teriparatide. Treatment: neoadjuvant chemotherapy + surgery."
  },
  {
    id:138, diagnosis:"Pheochromocytoma",
    aliases:["pheochromocytoma","pheo","adrenal medulla catecholamine tumor","chromaffin cell tumor"],
    episode:"Ep. 102", topic:"Endocrinology",
    ankiStep1:"Pheochromocytoma::urine_metanephrines::alpha_block_before_beta_block",
    ankiStep2:"Pheochromocytoma::episodic_HTN_headache_diaphoresis_palpitations::chromaffin_cells",
    clues:[
      "A 38-year-old woman presents with episodic severe headaches, profuse sweating, palpitations, and panic attacks that last 15-30 minutes each and occur 2-3 times weekly. Her blood pressure during an episode reaches 210/118 mmHg.",
      "Between episodes, her blood pressure is normal. She has had two emergency department visits for hypertensive urgency. Her primary care physician has tried three antihypertensive agents without adequate control.",
      "24-hour urine collection shows markedly elevated metanephrines (620 mcg/day; normal under 400) and catecholamines. Plasma free metanephrines are also elevated.",
      "CT abdomen reveals a 4 cm heterogeneous, well-circumscribed right adrenal mass with central necrosis. This is the diagnostic modality of choice for localization once biochemical confirmation is made.",
      "Before surgical resection, the patient must be given phenoxybenzamine (non-selective alpha blocker) for 7-14 days to block alpha-1 and alpha-2 adrenergic receptors. ONLY AFTER adequate alpha blockade is established is a beta blocker added.",
      "This tumor arises from chromaffin cells of the adrenal medulla (modified postganglionic sympathetic neurons). The rule of 10s: 10% bilateral, 10% malignant, 10% extra-adrenal, 10% familial. Associations: MEN2A, MEN2B, Von Hippel-Lindau, neurofibromatosis type 1."
    ],
    keyFacts:"Pheochromocytoma: episodic hypertension + headache + diaphoresis + palpitations. Arises from chromaffin cells of adrenal medulla. Diagnose with urine metanephrines. Alpha block BEFORE beta block before surgery (avoid unopposed alpha stimulation). Rule of 10s. Associations: MEN2A, MEN2B, VHL, NF1."
  },
  {
    id:139, diagnosis:"Acute Lymphoblastic Leukemia",
    aliases:["acute lymphoblastic leukemia","ALL","acute lymphocytic leukemia","childhood leukemia"],
    episode:"Ep. 203", topic:"Hematology / Oncology",
    ankiStep1:"Acute_Lymphoblastic_Leukemia::TdT_positive_child::Down_syndrome_association",
    ankiStep2:"Acute_Lymphoblastic_Leukemia::bone_pain_lymphadenopathy_very_high_WBC::induction_chemotherapy",
    clues:[
      "A 5-year-old boy presents with 6 weeks of fatigue, pallor, recurrent nosebleeds, and bone pain that causes him to limp. His parents report he has been irritable and refuses to walk.",
      "On examination, he has hepatosplenomegaly, diffuse lymphadenopathy, and tenderness over multiple long bones. There are petechiae scattered across his trunk and extremities.",
      "Complete blood count shows white blood cells of 85,000/mcL with 80% blasts, hemoglobin of 6.8 g/dL, and platelets of 18,000/mcL. Peripheral smear shows lymphoblasts with scant cytoplasm and immature chromatin.",
      "Bone marrow biopsy shows greater than 20% lymphoblasts. Flow cytometry reveals the blasts express terminal deoxynucleotidyl transferase (TdT), CD10 (CALLA), CD19, and CD20, consistent with a precursor B-cell immunophenotype.",
      "Lumbar puncture is performed to evaluate for central nervous system involvement, and intrathecal chemotherapy is administered prophylactically. Cytogenetics show hyperdiploidy — a favorable prognostic feature.",
      "This is the most common childhood malignancy and the most common leukemia in children. The most common age is 2-5 years. Association: Down syndrome (trisomy 21). Treatment: intensive induction chemotherapy (vincristine, prednisone, asparaginase); overall cure rate exceeds 90% in children."
    ],
    keyFacts:"Acute lymphoblastic leukemia: most common childhood malignancy. Peak age 2-5 years. TdT positive, CD10 (CALLA) positive. Down syndrome association. Bone pain, lymphadenopathy, hepatosplenomegaly, pancytopenia with circulating blasts. CNS prophylaxis with intrathecal chemotherapy. Hyperdiploidy = favorable prognosis. Cure rate over 90% in children."
  },
  {
    id:140, diagnosis:"Acute Myeloid Leukemia",
    aliases:["acute myeloid leukemia","AML","acute myelogenous leukemia","auer rods leukemia"],
    episode:"Ep. 203", topic:"Hematology / Oncology",
    ankiStep1:"Acute_Myeloid_Leukemia::auer_rods_DIC::t15_17_ATRA",
    ankiStep2:"Acute_Myeloid_Leukemia::middle_age_adult_blasts_pancytopenia::MPO_positive",
    clues:[
      "A 55-year-old man presents with 3 weeks of fatigue, night sweats, easy bruising, and three episodes of epistaxis requiring nasal packing. He works in a rubber manufacturing facility with benzene exposure.",
      "On examination, he is pale and febrile. There is gingival hypertrophy with bleeding gums. Petechiae and ecchymoses are present. The spleen is mildly enlarged.",
      "Complete blood count shows white blood cells of 68,000/mcL with 65% blasts, hemoglobin of 7.1 g/dL, and platelets of 22,000/mcL. Peripheral smear shows large blasts with prominent nucleoli and needle-like azurophilic crystalline inclusions in the cytoplasm.",
      "The needle-shaped cytoplasmic inclusions are pathognomonic when they enter the bloodstream, triggering disseminated intravascular coagulation. Coagulation studies show prolonged prothrombin time, elevated D-dimer, and very low fibrinogen.",
      "Cytogenetics reveal t(15;17) translocation, creating the PML-RARalpha fusion gene. This specific subtype (acute promyelocytic leukemia) is uniquely treated with all-trans retinoic acid (a vitamin A derivative) that causes terminal differentiation of the blasts.",
      "This is the most common acute leukemia in adults. Risk factors: benzene, ionizing radiation, prior chemotherapy, Down syndrome. Most subtypes are treated with cytarabine plus anthracycline. The t(15;17) subtype responds to all-trans retinoic acid plus arsenic trioxide."
    ],
    keyFacts:"Acute myeloid leukemia: most common acute leukemia in adults. Auer rods in blasts — pathognomonic, cause DIC when released. t(15;17) = acute promyelocytic leukemia treated with all-trans retinoic acid (ATRA). Gingival hypertrophy common (monocytic subtype). MPO and myeloperoxidase positive. TdT negative (unlike ALL)."
  },
  {
    id:141, diagnosis:"Chronic Myeloid Leukemia",
    aliases:["chronic myeloid leukemia","CML","philadelphia chromosome","BCR-ABL leukemia"],
    episode:"Ep. 203", topic:"Hematology / Oncology",
    ankiStep1:"Chronic_Myeloid_Leukemia::t9_22_Philadelphia_chromosome::imatinib",
    ankiStep2:"Chronic_Myeloid_Leukemia::splenomegaly_very_high_WBC_low_LAP::BCR-ABL_fusion",
    clues:[
      "A 48-year-old man presents with 4 months of progressive fatigue, early satiety, and left upper quadrant fullness. He has lost 12 pounds without trying.",
      "On examination, there is massive splenomegaly extending to the umbilicus. The spleen is firm and non-tender. Complete blood count shows white blood cells of 185,000/mcL with a full spectrum of myeloid cells at all stages of maturation — from myeloblasts to mature granulocytes.",
      "Peripheral smear shows a left shift with myelocytes, metamyelocytes, bands, and neutrophils. Basophilia and eosinophilia are prominent. Platelets are elevated. Leukocyte alkaline phosphatase score is very low (distinguishes it from a leukemoid reaction).",
      "Bone marrow biopsy reveals hypercellularity with myeloid hyperplasia. Cytogenetics show the Philadelphia chromosome: a reciprocal translocation between chromosomes 9 and 22, t(9;22), creating the BCR-ABL fusion gene.",
      "BCR-ABL produces a constitutively active tyrosine kinase that drives uncontrolled myeloid proliferation. The disease may transform to acute leukemia (blast crisis) if untreated.",
      "This malignancy is treated with imatinib (Gleevec), a tyrosine kinase inhibitor that competitively blocks the BCR-ABL kinase domain. Imatinib dramatically changed the prognosis of this disease from months to years of survival."
    ],
    keyFacts:"Chronic myeloid leukemia: Philadelphia chromosome t(9;22), BCR-ABL fusion. Massive splenomegaly + very high WBC with full myeloid spectrum + low leukocyte alkaline phosphatase. Basophilia is characteristic. Blast crisis = transformation to acute leukemia. Treatment: imatinib (tyrosine kinase inhibitor)."
  },
  {
    id:142, diagnosis:"Chronic Lymphocytic Leukemia",
    aliases:["chronic lymphocytic leukemia","CLL","smudge cells leukemia","elderly lymphocytosis"],
    episode:"Ep. 203", topic:"Hematology / Oncology",
    ankiStep1:"Chronic_Lymphocytic_Leukemia::smudge_cells_elderly::recurrent_bacterial_infections",
    ankiStep2:"Chronic_Lymphocytic_Leukemia::CD5_positive_B_cells::hypogammaglobulinemia",
    clues:[
      "A 72-year-old retired teacher is found to have an incidental white blood cell count of 62,000/mcL on a routine blood panel. She is completely asymptomatic and feels well.",
      "On examination, she has bilateral, rubbery, non-tender cervical, axillary, and inguinal lymphadenopathy. The spleen tip is palpable. She has had 4 bacterial respiratory infections in the past year requiring antibiotics.",
      "Peripheral blood smear shows a marked lymphocytosis with small, mature-appearing lymphocytes and characteristic smudge cells (lymphocytes that rupture during slide preparation due to their fragility).",
      "Flow cytometry of the circulating lymphocytes reveals a clonal B-cell population co-expressing CD5 (normally a T-cell marker) with B-cell markers CD19, CD20, and CD23 — the hallmark immunophenotype.",
      "Serum protein electrophoresis shows markedly reduced immunoglobulin levels (hypogammaglobulinemia). The recurrent infections result from the fact that the proliferating B cells do not produce functional antibodies.",
      "This is the most common leukemia in adults in the Western world, predominantly in the elderly. It causes immunodeficiency through displacement of normal B cells. Treatment is watchful waiting for asymptomatic patients; chemoimmunotherapy (fludarabine, cyclophosphamide, rituximab) when symptomatic."
    ],
    keyFacts:"Chronic lymphocytic leukemia: most common adult leukemia in the West. Elderly patient, incidentally found lymphocytosis. Smudge cells on smear. CD5+ B-cell co-expressing CD19/CD20/CD23. Hypogammaglobulinemia → recurrent bacterial infections. Richter transformation to diffuse large B-cell lymphoma. Watch and wait if asymptomatic."
  },
  {
    id:143, diagnosis:"Hodgkin Lymphoma",
    aliases:["hodgkin lymphoma","hodgkin's lymphoma","reed-sternberg cell lymphoma","nodular sclerosing lymphoma"],
    episode:"Ep. 203", topic:"Hematology / Oncology",
    ankiStep1:"Hodgkin_Lymphoma::Reed-Sternberg_cells_CD15_CD30::bimodal_distribution",
    ankiStep2:"Hodgkin_Lymphoma::B_symptoms_mediastinal_mass_young_adult::ABVD_chemotherapy",
    clues:[
      "A 22-year-old woman presents with 4 months of painless swelling in the right side of her neck. She has also had drenching night sweats, unintentional weight loss of 10 pounds, and fevers over 38°C.",
      "She reports that drinking alcohol causes pain at the sites of her enlarged lymph nodes — a specific but unusual symptom. Examination reveals multiple firm, rubbery, non-tender lymph nodes in the right cervical and supraclavicular chains.",
      "CT chest reveals a large anterior mediastinal mass (the most common mediastinal mass in young adults, together with teratoma). CT abdomen-pelvis shows no retroperitoneal lymphadenopathy.",
      "Excisional biopsy of the largest cervical node reveals large bilobed cells with prominent 'owl's eye' nucleoli surrounded by a clear halo, set against a background of reactive lymphocytes, eosinophils, and plasma cells.",
      "Flow cytometry and immunohistochemistry show the malignant cells express CD15 and CD30, but NOT CD20 or CD45. This specific immunophenotype distinguishes the malignant cells from normal lymphocytes.",
      "Nodular sclerosing is the most common subtype. This lymphoma has a bimodal age distribution (late teens/20s and 50s-60s). The B symptoms (fever, night sweats, weight loss) confer a worse prognosis. Treatment: ABVD chemotherapy (doxorubicin, bleomycin, vinblastine, dacarbazine) +/- radiation."
    ],
    keyFacts:"Hodgkin lymphoma: Reed-Sternberg cells (owl's eye) CD15+ CD30+ (not CD20). B symptoms: fever, night sweats, weight loss. Bimodal distribution. Alcohol-induced pain at lymph node sites is pathognomonic. Nodular sclerosing is most common subtype. More lymphocytes = better prognosis. Treatment: ABVD chemotherapy."
  },
  {
    id:144, diagnosis:"Burkitt Lymphoma",
    aliases:["burkitt lymphoma","burkitt's lymphoma","t8;14 lymphoma","starry sky lymphoma","c-myc lymphoma"],
    episode:"Ep. 203", topic:"Hematology / Oncology",
    ankiStep1:"Burkitt_Lymphoma::t8_14_c-myc::starry_sky_histology",
    ankiStep2:"Burkitt_Lymphoma::jaw_mass_African_child_abdominal_mass_HIV::EBV_association",
    clues:[
      "An 8-year-old boy from sub-Saharan Africa presents with a rapidly growing, painful jaw mass that has developed over 3 weeks. His teacher noticed the swelling during the school week.",
      "On examination, there is a 6 cm bony-hard mass deforming the right mandible. Cervical lymphadenopathy is also present. He appears malnourished and febrile.",
      "CT scan shows a destructive lesion involving the mandible and maxilla. Tissue biopsy reveals a highly cellular neoplasm of intermediate-sized lymphoid cells with basophilic cytoplasm and numerous lipid vacuoles.",
      "Histology shows a 'starry sky' pattern — the pale macrophages engulfing the debris of rapidly dying tumor cells scattered among sheets of dark blue malignant lymphocytes create the impression of stars in a night sky.",
      "Cytogenetics reveal a translocation between chromosome 8 (where the c-Myc oncogene resides) and chromosome 14 (the immunoglobulin heavy chain locus): t(8;14). This places c-Myc under the control of a powerful immunoglobulin promoter.",
      "The African (endemic) form is associated with Epstein-Barr virus and presents as a jaw mass. The sporadic form presents as an abdominal mass. This is the fastest-growing human tumor (doubling time approximately 24 hours). Treatment: intensive combination chemotherapy with CNS prophylaxis."
    ],
    keyFacts:"Burkitt lymphoma: t(8;14), c-Myc amplification. Starry sky histology. Fastest-growing human tumor. African (endemic) form: EBV-associated jaw mass. Sporadic form: abdominal mass. HIV-associated form also exists. Treatment: intensive combination chemotherapy including rituximab. Tumor lysis syndrome is a major treatment risk."
  },
  {
    id:145, diagnosis:"Multiple Myeloma",
    aliases:["multiple myeloma","myeloma","plasma cell dyscrasia","CRAB myeloma"],
    episode:"Ep. 203", topic:"Hematology / Oncology",
    ankiStep1:"Multiple_Myeloma::CRAB_criteria::SPEP_monoclonal_spike",
    ankiStep2:"Multiple_Myeloma::rouleaux_punched_out_lytic_lesions::Bence_Jones_proteins",
    clues:[
      "A 68-year-old man presents with severe back pain that has worsened over 3 months. He also reports fatigue, recurrent pneumonia, and increasing thirst and urination. He notes his urine has been foamy.",
      "Laboratory studies: calcium 12.8 mg/dL, creatinine 2.4 mg/dL, hemoglobin 8.1 g/dL. Total protein is markedly elevated with albumin low — an elevated globulin gap. Urinalysis shows protein.",
      "Peripheral blood smear shows red blood cells stacked like coins (rouleaux formation), caused by immunoglobulin coating of the cell surface. Serum protein electrophoresis shows a sharp monoclonal spike (M-spike) in the gamma region.",
      "Urine protein electrophoresis shows Bence Jones proteins (free light chains — kappa or lambda). Skeletal survey reveals multiple punched-out lytic lesions throughout the skull, spine, and ribs without surrounding sclerosis.",
      "Bone marrow biopsy reveals greater than 10% clonal plasma cells with eccentric nuclei and a paranuclear Golgi hof (clock-face chromatin). The CRAB criteria are met: hyperCalcemia, Renal insufficiency, Anemia, and Bone lesions.",
      "This plasma cell malignancy is the second most common hematologic malignancy in adults. Treatment: bortezomib (proteasome inhibitor) + lenalidomide + dexamethasone, followed by autologous stem cell transplantation in eligible patients."
    ],
    keyFacts:"Multiple myeloma: CRAB criteria (hyperCalcemia, Renal insufficiency, Anemia, Bone lytic lesions). Rouleaux formation on smear. SPEP shows M-spike. Bence Jones proteins in urine. Punched-out lytic lesions without sclerosis. Bone marrow: over 10% plasma cells. Treatment: bortezomib + lenalidomide + dexamethasone."
  },
  {
    id:146, diagnosis:"Polycythemia Vera",
    aliases:["polycythemia vera","primary polycythemia","JAK2 mutation polycythemia","aquagenic pruritus"],
    episode:"Ep. 203", topic:"Hematology / Oncology",
    ankiStep1:"Polycythemia_Vera::JAK2_mutation::phlebotomy_hydroxyurea",
    ankiStep2:"Polycythemia_Vera::aquagenic_pruritus_plethora_low_EPO::myeloproliferative",
    clues:[
      "A 62-year-old man presents with 6 months of headaches, dizziness, and a ruddy red complexion. He reports intense itching of his entire body that consistently occurs after taking warm showers or baths.",
      "On examination, his face is a deep red-plum color (facial plethora). Blood pressure is 162/95 mmHg. The spleen is enlarged. A faint systolic bruit is heard in the left upper quadrant.",
      "Complete blood count shows hemoglobin 20.2 g/dL, hematocrit 61%, white blood cells 14,000/mcL, and platelets 520,000/mcL — elevation of all three cell lines (panmyelosis).",
      "Serum erythropoietin level is markedly suppressed (nearly undetectable), distinguishing primary from secondary polycythemia (in which erythropoietin is elevated in response to hypoxia).",
      "JAK2 V617F mutation analysis is positive. This gain-of-function mutation in the Janus kinase 2 signaling pathway causes erythropoietin-independent proliferation of erythroid (and myeloid) precursors.",
      "This myeloproliferative neoplasm carries risks of thrombosis (stroke, myocardial infarction, Budd-Chiari syndrome) and eventual transformation to myelofibrosis or acute myeloid leukemia. Treatment: therapeutic phlebotomy to keep hematocrit under 45% and aspirin; hydroxyurea for high-risk patients."
    ],
    keyFacts:"Polycythemia vera: JAK2 V617F mutation. Aquagenic pruritus (after shower) + facial plethora + splenomegaly. Panmyelosis (elevated red blood cells, white blood cells, and platelets). Low erythropoietin distinguishes from secondary polycythemia. Risk of thrombosis and Budd-Chiari syndrome. Treatment: phlebotomy + aspirin; hydroxyurea for high risk."
  },
  {
    id:147, diagnosis:"Hereditary Spherocytosis",
    aliases:["hereditary spherocytosis","spectrin deficiency anemia","osmotic fragility anemia","HS"],
    episode:"Ep. 180", topic:"Hematology",
    ankiStep1:"Hereditary_Spherocytosis::spectrin_ankyrin_defect::osmotic_fragility_test",
    ankiStep2:"Hereditary_Spherocytosis::lacks_central_pallor_MCHC_elevated::splenectomy",
    clues:[
      "A 24-year-old woman presents with fatigue and intermittent jaundice that has worsened over several years. She recalls being jaundiced as a newborn requiring phototherapy.",
      "Family history reveals her father and a sibling have also been diagnosed with anemia. On examination, the sclerae are icteric and there is mild splenomegaly.",
      "Complete blood count shows hemoglobin 9.8 g/dL, MCV 78 fL (microcytic), but MCHC is elevated at 38 g/dL (normal under 36) — a characteristic finding in this condition.",
      "Peripheral blood smear shows small, round red blood cells that lack the normal central pallor. Reticulocyte count is elevated at 8%, consistent with compensated hemolytic anemia.",
      "Indirect bilirubin is elevated and lactate dehydrogenase is elevated, consistent with intravascular hemolysis. An eosin-5-maleimide binding test (or osmotic fragility test) confirms the red blood cells are abnormally fragile when exposed to hypotonic solutions.",
      "This autosomal dominant condition results from mutations in spectrin, ankyrin, or band 3 proteins of the red blood cell membrane, causing the cell to lose its biconcave disc shape and become spherical. The spherocytes are trapped and destroyed in the spleen. Definitive treatment is splenectomy."
    ],
    keyFacts:"Hereditary spherocytosis: autosomal dominant, spectrin/ankyrin/band 3 mutation. Spherocytes without central pallor on smear. Elevated MCHC. Elevated osmotic fragility. Indirect hyperbilirubinemia. Splenomegaly. Diagnosed by eosin-5-maleimide test or osmotic fragility test. Treatment: splenectomy (curative). Give encapsulated organism vaccines before splenectomy."
  },
  {
    id:148, diagnosis:"Heparin-Induced Thrombocytopenia",
    aliases:["heparin-induced thrombocytopenia","HIT","heparin thrombocytopenia","platelet factor 4 antibody"],
    episode:"Ep. 180", topic:"Hematology",
    ankiStep1:"Heparin_Induced_Thrombocytopenia::anti-PF4_antibody::argatroban_direct_thrombin_inhibitor",
    ankiStep2:"Heparin_Induced_Thrombocytopenia::platelet_drop_50pct_day5_thrombosis::stop_heparin",
    clues:[
      "A 58-year-old man who underwent coronary artery bypass surgery 6 days ago is found to have a platelet count of 45,000/mcL, down from 210,000/mcL preoperatively. He is currently receiving unfractionated heparin infusion.",
      "He develops swelling, warmth, and pain in his left calf. Lower extremity Doppler ultrasound confirms a new deep vein thrombosis. Surprisingly, despite a very low platelet count, he has a thrombotic rather than bleeding complication.",
      "The 4-T score is calculated: greater than 50% platelet fall, onset day 5-10, new thrombosis, and no other obvious cause — the score is 7 (high probability).",
      "Laboratory testing shows anti-platelet factor 4/heparin complex antibodies (anti-PF4 antibodies) are strongly positive by enzyme-linked immunosorbent assay. Serotonin release assay confirms functional antibodies.",
      "The antibodies bind to the platelet factor 4-heparin complex, activating platelets and triggering both platelet consumption (thrombocytopenia) and a paradoxical hypercoagulable state (thrombosis).",
      "The first step is immediate discontinuation of all heparin products (including heparin flushes and low-molecular-weight heparin). The patient must be started on an alternative anticoagulant — argatroban or bivalirudin (direct thrombin inhibitors). Warfarin is NOT started until platelets recover."
    ],
    keyFacts:"Heparin-induced thrombocytopenia: platelet drop over 50% on days 5-10 of heparin therapy PLUS thrombosis (paradoxical). Anti-PF4/heparin antibodies (type II hypersensitivity). Stop ALL heparin immediately. Start direct thrombin inhibitor (argatroban or bivalirudin). Do NOT give warfarin until platelets recover above 150,000."
  },
  {
    id:149, diagnosis:"Von Willebrand Disease",
    aliases:["von willebrand disease","vWD","von willebrand factor deficiency","platelet adhesion defect"],
    episode:"Ep. 180", topic:"Hematology",
    ankiStep1:"Von_Willebrand_Disease::vWF_deficiency_platelet_adhesion::desmopressin",
    ankiStep2:"Von_Willebrand_Disease::mucocutaneous_bleeding_elevated_PTT::positive_ristocetin_cofactor",
    clues:[
      "A 19-year-old woman presents with concern about heavy menstrual bleeding since menarche, requiring multiple pad changes per hour on the first two days of her cycle. She also reports frequent nosebleeds and prolonged bleeding from minor cuts.",
      "She had prolonged bleeding after a dental extraction at age 14. Her mother and maternal aunt have similar bleeding histories. No medications are taken.",
      "Physical examination reveals scattered petechiae and several bruises at different stages of healing. Platelet count is normal at 245,000/mcL.",
      "Coagulation studies show prolonged bleeding time, mildly elevated partial thromboplastin time, and normal prothrombin time. Platelet aggregation studies show abnormal aggregation with ristocetin — a specific finding for this condition.",
      "Von Willebrand factor antigen level is reduced to 18% of normal. Von Willebrand factor activity (ristocetin cofactor assay) is also reduced. Factor VIII activity is mildly reduced because von Willebrand factor normally acts as a carrier protein for factor VIII.",
      "This is the most common inherited bleeding disorder (autosomal dominant). Von Willebrand factor mediates platelet adhesion to injured subendothelial collagen. Treatment: desmopressin (DDAVP), which releases von Willebrand factor from endothelial Weibel-Palade bodies; recombinant von Willebrand factor concentrate for severe cases."
    ],
    keyFacts:"Von Willebrand disease: most common inherited bleeding disorder. Mucocutaneous bleeding (menorrhagia, epistaxis, easy bruising). Normal platelet count, elevated bleeding time, mildly elevated PTT. Abnormal ristocetin aggregation. Reduced VWF antigen and activity. Treatment: desmopressin (releases VWF from endothelium) or VWF concentrate. Desmopressin also releases factor VIII."
  },
  {
    id:150, diagnosis:"Atrial Fibrillation",
    aliases:["atrial fibrillation","afib","AF","irregularly irregular rhythm"],
    episode:"Ep. 104", topic:"Cardiology",
    ankiStep1:"Atrial_Fibrillation::no_P_waves_irregularly_irregular::rate_control_rhythm_control",
    ankiStep2:"Atrial_Fibrillation::stroke_risk_CHA2DS2-VASc::anticoagulation",
    clues:[
      "A 72-year-old man with hypertension, type 2 diabetes, and a prior stroke presents to the emergency department with palpitations, dyspnea, and fatigue for 6 hours. His heart rate is 138 bpm and blood pressure is 118/74 mmHg.",
      "Cardiac examination reveals an irregular rhythm with varying pulse intensity. There are no murmurs. Lung examination is clear. He is not in acute distress.",
      "Electrocardiogram shows absent P-waves, an irregularly irregular ventricular rate, and a narrow QRS complex. No delta waves or short PR interval are present.",
      "Because symptoms began 6 hours ago — within 48 hours — cardioversion is an option, but given his prior stroke, his thromboembolic risk is extremely high. Rate control is initiated with intravenous metoprolol.",
      "His CHA2DS2-VASc score is calculated: Congestive heart failure (0), Hypertension (1), Age over 75 (0, he is 72), Diabetes (1), prior Stroke (2), Vascular disease (0), Age 65-74 (1), Female (0) = 5 points. Long-term anticoagulation with a direct oral anticoagulant (apixaban) is started.",
      "This most common sustained cardiac arrhythmia results from chaotic re-entry circuits in the atria. Rate control uses beta blockers or non-dihydropyridine calcium channel blockers (diltiazem, verapamil). Rhythm control uses amiodarone. Hemodynamically unstable patients require immediate synchronized cardioversion."
    ],
    keyFacts:"Atrial fibrillation: absent P-waves, irregularly irregular rhythm. Rate control: metoprolol, diltiazem, or digoxin. Rhythm control: amiodarone. Anticoagulate with CHA2DS2-VASc score of 2 or more in men. Unstable: synchronized cardioversion. If over 48 hours duration, rule out left atrial appendage thrombus with transesophageal echo before cardioversion."
  },
  {
    id:151, diagnosis:"Wolff-Parkinson-White Syndrome",
    aliases:["wolff-parkinson-white syndrome","WPW","pre-excitation syndrome","bundle of kent"],
    episode:"Ep. 104", topic:"Cardiology",
    ankiStep1:"Wolff-Parkinson-White::delta_wave_short_PR_bundle_of_Kent::procainamide",
    ankiStep2:"Wolff-Parkinson-White::DO_NOT_block_AV_node::accessory_pathway_ablation",
    clues:[
      "A 24-year-old athlete presents with recurrent episodes of sudden-onset racing heart, dizziness, and presyncope that terminate spontaneously after several minutes. Symptoms have occurred 4 times over the past year.",
      "Electrocardiogram performed during sinus rhythm shows a short PR interval (less than 120 milliseconds), a slurred upstroke of the QRS complex (delta wave), and a widened QRS.",
      "During a tachycardic episode, the ventricular rate reaches 220 bpm with a wide, bizarre QRS complex. The rhythm is irregular, suggesting atrial fibrillation conducting through the accessory pathway.",
      "Electrophysiology study confirms the presence of an accessory atrioventricular bypass tract (Bundle of Kent) that allows electrical impulses to bypass the atrioventricular node, causing ventricular pre-excitation.",
      "Administration of adenosine or any atrioventricular nodal blocking agent (beta blockers, calcium channel blockers, digoxin) is CONTRAINDICATED in this condition with atrial fibrillation, as it blocks the normal pathway and forces conduction exclusively down the accessory pathway, potentially causing ventricular fibrillation.",
      "Acute management of the wide-complex tachycardia with accessory pathway conduction uses procainamide (which slows conduction in the accessory pathway). Definitive treatment is catheter ablation of the accessory pathway."
    ],
    keyFacts:"Wolff-Parkinson-White syndrome: short PR interval + delta wave + widened QRS. Accessory pathway (Bundle of Kent) bypasses the AV node. NEVER block the AV node with adenosine/beta blockers/calcium channel blockers/digoxin in WPW with atrial fibrillation — risk of ventricular fibrillation. Acute treatment: procainamide. Definitive treatment: catheter ablation."
  },
  {
    id:152, diagnosis:"Aspirin-Exacerbated Respiratory Disease",
    aliases:["aspirin-exacerbated respiratory disease","AERD","aspirin triad","Samter's triad","aspirin sensitive asthma"],
    episode:"Ep. 29", topic:"Pulmonology / Immunology",
    ankiStep1:"AERD::COX_inhibition_excess_leukotrienes::montelukast_zileuton",
    ankiStep2:"AERD::asthma_nasal_polyps_aspirin_sensitivity::leukotriene_pathway",
    clues:[
      "A 36-year-old woman with moderate persistent asthma presents with worsening wheezing and severe bronchospasm that began 45 minutes after taking ibuprofen for a headache. She required emergency nebulizer treatments.",
      "On further history, she has experienced similar reactions to aspirin, naproxen, and diclofenac. She tolerates acetaminophen without difficulty.",
      "She has had progressive nasal obstruction and anosmia over the past 3 years. Nasal endoscopy reveals bilateral nasal polyps filling both nasal cavities. She reports needing to blow her nose constantly.",
      "Spirometry shows obstructive pattern that improves with bronchodilator. Skin testing for allergens is negative. Serum eosinophils are elevated. Total IgE is normal.",
      "The mechanism involves inhibition of cyclooxygenase-1 by aspirin and other non-steroidal anti-inflammatory drugs, which shunts arachidonic acid metabolism toward the 5-lipoxygenase pathway, producing excess leukotrienes that cause bronchoconstriction and nasal inflammation.",
      "This triad of asthma, nasal polyps, and non-steroidal anti-inflammatory drug sensitivity is treated with leukotriene receptor antagonists (montelukast) or leukotriene synthesis inhibitors (zileuton, a 5-lipoxygenase inhibitor). Aspirin desensitization may be performed in specialized centers."
    ],
    keyFacts:"Aspirin-exacerbated respiratory disease (Samter's triad): asthma + nasal polyps + NSAID sensitivity. COX inhibition shunts arachidonic acid to excess leukotriene production. All NSAIDs cross-react. Acetaminophen is safe. Other causes of nasal polyps: cystic fibrosis, Wegener's (GPA). Treatment: leukotriene receptor antagonists (montelukast) or zileuton."
  },
  {
    id:153, diagnosis:"Toxic Shock Syndrome",
    aliases:["toxic shock syndrome","TSS","superantigen mediated shock","staphylococcal toxic shock"],
    episode:"Ep. 30", topic:"Infectious Disease",
    ankiStep1:"Toxic_Shock_Syndrome::superantigen_T_cell_activation::fluids_antibiotics",
    ankiStep2:"Toxic_Shock_Syndrome::fever_rash_hypotension_desquamation::tampon_or_nasal_packing",
    clues:[
      "A 25-year-old woman presents to the emergency department with sudden-onset high fever to 40°C, diffuse sunburn-like erythroderma, and blood pressure of 62/40 mmHg refractory to 3 liters of intravenous fluids.",
      "On history, she reports she left a tampon in place for 5 days during her menstrual cycle because she forgot about it. She also has vomiting, watery diarrhea, myalgias, and a severe headache.",
      "Examination reveals a diffuse, macular, erythematous rash covering the trunk, face, and extremities. Mucous membranes are erythematous. Troponin, creatinine, and liver enzymes are all mildly elevated, suggesting multiorgan dysfunction.",
      "The retained tampon is removed. Blood, vaginal, and tampon cultures are sent. The clinical picture is consistent with toxin-mediated septic shock rather than bacteremic shock.",
      "The toxin produced by the causative organism (Staphylococcus aureus TSST-1) functions as a superantigen — it simultaneously binds the MHC class II molecule on antigen-presenting cells AND the T-cell receptor, activating up to 20% of all T-cells at once and causing a massive cytokine storm.",
      "One to two weeks after the acute illness, the palms and soles develop characteristic desquamation (peeling). Treatment includes fluid resuscitation, vasopressors, and clindamycin plus a beta-lactam antibiotic (clindamycin inhibits toxin production)."
    ],
    keyFacts:"Toxic shock syndrome: fever + diffuse erythroderma + hypotension + multiorgan dysfunction. Caused by Staphylococcus aureus (TSST-1) or Streptococcus pyogenes superantigens. Triggers: retained tampon, nasal packing, wound infection. Late desquamation of palms/soles. Treatment: remove source + fluids + vancomycin + clindamycin (inhibits toxin synthesis)."
  },
  {
    id:154, diagnosis:"Sjogren Syndrome",
    aliases:["sjogren syndrome","sjogren's syndrome","sicca syndrome","autoimmune exocrinopathy"],
    episode:"Ep. 29", topic:"Rheumatology",
    ankiStep1:"Sjogren_Syndrome::anti-SSA_anti-SSB_antibodies::lymphocytic_exocrine_infiltration",
    ankiStep2:"Sjogren_Syndrome::dry_eyes_dry_mouth_enlarged_parotid::increased_lymphoma_risk",
    clues:[
      "A 48-year-old woman presents with 2 years of sand-like gritty sensation in both eyes and difficulty swallowing dry foods such as crackers without drinking water. She also notes extreme dental decay despite good oral hygiene.",
      "She has bilateral painless parotid gland enlargement that her dentist first noticed. Joint pain in her fingers and wrists has been present for 6 months.",
      "Schirmer's test shows reduced tear production (less than 5 mm wetting in 5 minutes). Slit-lamp examination reveals rose bengal staining of the corneal epithelium, indicating keratoconjunctivitis sicca.",
      "Salivary gland biopsy of the lower lip reveals focal lymphocytic infiltration of the minor salivary glands (focus score greater than 1 per 4 mm² section of tissue) — the gold standard diagnostic finding.",
      "Serological testing is positive for anti-SSA (anti-Ro) and anti-SSB (anti-La) antibodies. Antinuclear antibody is positive at 1:640. Rheumatoid factor is also positive.",
      "This autoimmune exocrinopathy carries a 40-fold increased risk of marginal zone B-cell lymphoma (mucosa-associated lymphoid tissue type). Primary disease also affects the kidneys (renal tubular acidosis type 1) and lungs. Treatment: artificial tears, pilocarpine (muscarinic agonist to stimulate secretion), hydroxychloroquine."
    ],
    keyFacts:"Sjogren syndrome: keratoconjunctivitis sicca (dry eyes) + xerostomia (dry mouth) + parotid enlargement. Anti-SSA (Ro) and anti-SSB (La) antibodies. Lip biopsy shows focal lymphocytic infiltration. Increased risk of MALT lymphoma. Renal tubular acidosis type 1. Treatment: artificial tears, pilocarpine, hydroxychloroquine."
  },
  {
    id:155, diagnosis:"Minimal Change Disease",
    aliases:["minimal change disease","lipoid nephrosis","nil disease","nephrotic syndrome in children"],
    episode:"Ep. 29", topic:"Nephrology",
    ankiStep1:"Minimal_Change_Disease::effacement_foot_processes_EM::corticosteroids",
    ankiStep2:"Minimal_Change_Disease::nephrotic_syndrome_child_normal_light_microscopy::NSAID_Hodgkin",
    clues:[
      "A 4-year-old boy is brought to his pediatrician with 2 weeks of puffy eyelids in the morning, progressive abdominal swelling, and swelling of the scrotum. His mother reports his urine has been foamy.",
      "On examination, bilateral pitting periorbital edema, abdominal ascites, and bilateral lower extremity pitting edema are present. Blood pressure is normal.",
      "Urinalysis reveals heavy proteinuria (4+ protein). 24-hour urine protein is 4.2 grams (nephrotic range, over 3.5 grams). Albumin is 1.8 g/dL (low). Total cholesterol is 385 mg/dL (elevated — hyperlipidemia from compensatory hepatic lipoprotein synthesis).",
      "Renal biopsy shows normal glomeruli on light microscopy (hence the name) and no deposits on immunofluorescence. However, electron microscopy reveals diffuse effacement (fusion) of the podocyte foot processes.",
      "No red blood cell casts or hematuria are present, distinguishing this from nephritic syndrome. Complement levels are normal. Anti-nuclear antibody is negative.",
      "This is the most common cause of nephrotic syndrome in children (peak age 2-6 years). It is also associated with non-steroidal anti-inflammatory drugs, Hodgkin lymphoma, and bee stings in adults. Treatment: corticosteroids (prednisone) — over 90% achieve remission."
    ],
    keyFacts:"Minimal change disease: most common cause of nephrotic syndrome in children. Normal light microscopy + normal immunofluorescence + effaced foot processes on electron microscopy. Massive proteinuria without hematuria. Associated with NSAID use, Hodgkin lymphoma, bee stings in adults. Treatment: corticosteroids — excellent response."
  },
  {
    id:156, diagnosis:"G6PD Deficiency",
    aliases:["G6PD deficiency","glucose-6-phosphate dehydrogenase deficiency","favism","hemolytic anemia after oxidant exposure"],
    episode:"Ep. 29", topic:"Hematology",
    ankiStep1:"G6PD_Deficiency::Heinz_bodies_bite_cells::oxidant_stress_hemolysis",
    ankiStep2:"G6PD_Deficiency::primaquine_dapsone_fava_beans::X-linked_recessive",
    clues:[
      "A 22-year-old African American male soldier presents with acute onset of jaundice, dark urine, and fatigue 3 days after starting primaquine for malaria prophylaxis.",
      "On examination, he is icteric. Hemoglobin has dropped from 14.2 to 7.8 g/dL over 72 hours. Reticulocyte count is elevated at 12%. Indirect bilirubin and lactate dehydrogenase are elevated.",
      "Peripheral blood smear shows bite cells (red blood cells with a portion appearing 'bitten off' by the spleen removing denatured hemoglobin), and Heinz bodies (denatured hemoglobin precipitates) visible on crystal violet stain.",
      "The hemolysis self-limits after several days because only older red blood cells (with the lowest enzyme levels) are destroyed; younger reticulocytes have enough enzyme activity to survive oxidant stress.",
      "The underlying defect involves decreased activity of the enzyme that generates NADPH via the pentose phosphate pathway, which is essential for regenerating glutathione — the primary antioxidant protecting red blood cells from oxidant damage.",
      "This X-linked recessive condition provides partial protection against Plasmodium falciparum malaria in heterozygous females. Hemolytic triggers: primaquine, dapsone, rasburicase, sulfonamides, nitrofurantoin, fava beans, and infection."
    ],
    keyFacts:"G6PD deficiency: X-linked recessive, most common in African and Mediterranean populations. Episodic hemolytic anemia triggered by oxidant stress (antimalarials, sulfonamides, fava beans, infection). Bite cells and Heinz bodies on smear. Enzyme assay diagnostic (but measure after hemolytic episode as young RBCs have higher G6PD activity). Avoid triggers."
  },
  {
    id:157, diagnosis:"Alpha-1 Antitrypsin Deficiency",
    aliases:["alpha-1 antitrypsin deficiency","A1AT deficiency","alpha-1 antitrypsin","COPD in non-smoker"],
    episode:"Ep. 29", topic:"Pulmonology / Hepatology",
    ankiStep1:"Alpha-1_Antitrypsin_Deficiency::PiZZ_genotype::panacinar_emphysema",
    ankiStep2:"Alpha-1_Antitrypsin_Deficiency::young_COPD_cirrhosis::alpha-1_antitrypsin_replacement",
    clues:[
      "A 38-year-old man who has never smoked presents with progressive dyspnea on exertion and chronic cough for 3 years. He has no occupational exposures. His father died of liver disease in his 50s.",
      "Spirometry shows an obstructive pattern with forced expiratory volume in 1 second/forced vital capacity ratio of 58%. CT chest reveals panacinar (affecting all parts of the acinus) emphysema with a basilar predominance — unlike typical smoking-related emphysema which is predominantly upper lobe.",
      "Liver function tests reveal mildly elevated alanine transaminase and aspartate transaminase. Liver biopsy shows periodic acid–Schiff positive, diastase-resistant inclusions in hepatocytes (misfolded protein accumulated within the endoplasmic reticulum).",
      "Serum alpha-1 antitrypsin level is markedly reduced at 18 mg/dL (normal over 100 mg/dL). Genotyping reveals the homozygous PiZZ genotype — the most severe form of this condition.",
      "The deficient protein normally inhibits neutrophil elastase in the lungs. Without it, neutrophil elastase destroys alveolar walls (emphysema). In the liver, misfolded Z-variant protein accumulates, causing hepatocyte damage and cirrhosis.",
      "This autosomal codominant condition should be suspected in any young non-smoker with emphysema or in any patient with unexplained liver disease. Treatment: weekly intravenous replacement therapy (augmentation therapy) for pulmonary disease; liver transplantation for cirrhosis."
    ],
    keyFacts:"Alpha-1 antitrypsin deficiency: panacinar emphysema with basilar predominance in young non-smoker PLUS liver disease. PiZZ genotype most severe. Neutrophil elastase destroys alveolar walls unchecked. PAS-positive hepatocyte inclusions (misfolded protein). Diagnosis: serum alpha-1 antitrypsin level + genotyping. Treatment: augmentation therapy; liver transplant for cirrhosis."
  },
  {
    id:158, diagnosis:"Cavernous Sinus Thrombosis",
    aliases:["cavernous sinus thrombosis","septic cavernous sinus thrombosis","orbital apex syndrome"],
    episode:"Ep. 45", topic:"Infectious Disease / Neurology",
    ankiStep1:"Cavernous_Sinus_Thrombosis::CN3_4_6_loss::Staph_aureus_IV_antibiotics_heparin",
    ankiStep2:"Cavernous_Sinus_Thrombosis::periorbital_edema_proptosis_ophthalmoplegia::sinusitis_dental",
    clues:[
      "A 28-year-old woman presents with 4 days of high fever, severe headache, and right eye pain that has progressively worsened. She was treated for right sinusitis 2 weeks ago.",
      "On examination, the right eye is proptotic (bulging forward) with periorbital edema and marked conjunctival chemosis. The right eye is in a fixed midposition — she cannot move it in any direction.",
      "She has lost sensation over the forehead and cheek on the right (cranial nerve V1 and V2 distribution). Fundoscopy shows dilated, engorged retinal veins.",
      "CT with contrast and MRI brain with venography reveal thrombosis of the right cavernous sinus with extension to the superior ophthalmic vein. The cavernous sinus contains the oculomotor, trochlear, abducens, and trigeminal nerves V1 and V2.",
      "Blood cultures grow Staphylococcus aureus. The infection spread from the facial or sinus region through the valveless ophthalmic veins and emissary veins into the cavernous sinus — the danger triangle of the face.",
      "Cranial nerve VI (abducens) is the first to be affected because it lies free within the sinus rather than embedded in the lateral wall. Treatment: intravenous antibiotics (vancomycin) plus anticoagulation with heparin to prevent propagation."
    ],
    keyFacts:"Cavernous sinus thrombosis: ophthalmoplegia (cranial nerve III, IV, VI palsy) + proptosis + periorbital edema + sensory loss (V1, V2). CN VI is first affected (lies free in sinus). Caused by facial/sinus infection spreading via ophthalmic veins. Most common organism: Staphylococcus aureus. Treatment: IV antibiotics + heparin anticoagulation."
  },
  {
    id:159, diagnosis:"Essential Thrombocythemia",
    aliases:["essential thrombocythemia","primary thrombocythemia","JAK2 thrombocytosis","myeloproliferative thrombocytosis"],
    episode:"Ep. 203", topic:"Hematology / Oncology",
    ankiStep1:"Essential_Thrombocythemia::JAK2_very_high_platelets::thrombosis_AND_bleeding",
    ankiStep2:"Essential_Thrombocythemia::paradoxical_bleeding_thrombosis_dysfunctional_platelets::hydroxyurea",
    clues:[
      "A 55-year-old woman is found to have a platelet count of 1,450,000/mcL on routine bloodwork. She reports occasional tingling and burning in her fingertips and toes, and intermittent headaches.",
      "On examination, her spleen is mildly enlarged. She has no pallor or lymphadenopathy. She has had one episode of transient ischemic attack in the past year and one episode of heavy vaginal bleeding following minor dental work.",
      "She has both thrombotic AND hemorrhagic complications — an apparently paradoxical finding. The mechanism is that the massively elevated platelet number leads to acquired von Willebrand disease as von Willebrand factor multimers are cleaved by the large platelet mass.",
      "Bone marrow biopsy shows large, clustered, hyperlobulated megakaryocytes without fibrosis or increased blasts. JAK2 V617F mutation is detected in 50% of cases; calreticulin (CALR) mutation in an additional 25%.",
      "Serum erythropoietin is normal. Serum ferritin is normal (ruling out reactive thrombocytosis from iron deficiency). JAK2 V617F mutation is positive.",
      "This myeloproliferative neoplasm is distinguished from reactive thrombocytosis by the presence of a driver mutation. Low-risk patients are managed with aspirin. High-risk patients (age over 60 or prior thrombosis) are treated with hydroxyurea to reduce platelet count."
    ],
    keyFacts:"Essential thrombocythemia: extremely elevated platelets (often over 1,000,000) with paradoxical thrombosis AND bleeding (acquired von Willebrand disease). JAK2 V617F or CALR mutation. Clustered hyperlobulated megakaryocytes on biopsy. Distinguishing from reactive: driver mutation present. Treatment: aspirin for all; hydroxyurea for high-risk patients."
  },
  {
    id:160, diagnosis:"Primary Myelofibrosis",
    aliases:["primary myelofibrosis","myelofibrosis","agnogenic myeloid metaplasia","bone marrow fibrosis"],
    episode:"Ep. 203", topic:"Hematology / Oncology",
    ankiStep1:"Primary_Myelofibrosis::dacrocytes_dry_tap::extramedullary_hematopoiesis",
    ankiStep2:"Primary_Myelofibrosis::massive_splenomegaly_leukoerythroblastic::JAK2_mutation",
    clues:[
      "A 64-year-old man presents with progressive fatigue, weight loss, night sweats, and left-sided abdominal fullness and early satiety for 8 months. He cannot sleep on his left side due to discomfort.",
      "Physical examination reveals massive splenomegaly extending to the left iliac fossa. The liver is also enlarged. There is no significant lymphadenopathy.",
      "Complete blood count shows anemia (hemoglobin 8.4 g/dL), moderate leukocytosis with immature myeloid cells, and a variable platelet count. Peripheral smear shows teardrop-shaped red blood cells (dacrocytes) and circulating nucleated red blood cells — a leukoerythroblastic picture.",
      "Bone marrow aspiration yields no material — a 'dry tap' due to the fibrotic marrow. Bone marrow biopsy reveals dense reticulin and collagen fibrosis replacing the normal marrow architecture, with clustered, atypical megakaryocytes.",
      "JAK2 V617F mutation is detected. The massive splenomegaly results from extramedullary hematopoiesis — blood cell production in the spleen and liver compensating for the fibrotic bone marrow.",
      "This is the most aggressive of the classic myeloproliferative neoplasms. Treatment: ruxolitinib (JAK1/2 inhibitor) for symptomatic disease. Allogeneic stem cell transplantation is the only potentially curative option."
    ],
    keyFacts:"Primary myelofibrosis: massive splenomegaly + leukoerythroblastic blood picture + dacrocytes (teardrop cells) on smear + dry tap on bone marrow aspiration. Dense fibrosis on biopsy. JAK2 or CALR mutation. Extramedullary hematopoiesis. Treatment: ruxolitinib (JAK inhibitor). Only curative: allogeneic stem cell transplantation."
  },
  {
    id:161, diagnosis:"Hemophilia A",
    aliases:["hemophilia A","factor VIII deficiency","hemophilia","classic hemophilia"],
    episode:"Ep. 180", topic:"Hematology",
    ankiStep1:"Hemophilia_A::factor_VIII_deficiency_X-linked::elevated_PTT_normal_PT",
    ankiStep2:"Hemophilia_A::hemarthrosis_deep_tissue_bleeding::desmopressin_or_factor_VIII",
    clues:[
      "A 7-year-old boy is brought to the emergency department after developing severe right knee swelling and pain following a minor fall during soccer practice. He is crying and holding his knee in partial flexion.",
      "His mother reports he has had multiple similar episodes of joint swelling since toddlerhood, as well as prolonged bleeding after circumcision at birth and after a tooth extraction. His maternal uncle has similar bleeding problems.",
      "Joint aspiration of the knee yields frank hemarthrosis (blood within the joint). Radiograph shows soft tissue swelling and joint space widening from the blood.",
      "Coagulation studies show a markedly prolonged activated partial thromboplastin time of 82 seconds (normal 25-35 seconds) with a normal prothrombin time and normal platelet count.",
      "A mixing study with normal plasma corrects the prolonged activated partial thromboplastin time, indicating a factor deficiency (not an inhibitor). Specific factor assays reveal factor VIII activity of less than 1% (severe hemophilia).",
      "This X-linked recessive disorder causes deficiency of factor VIII, which is part of the intrinsic coagulation pathway. Treatment: recombinant factor VIII concentrate. Desmopressin (DDAVP) releases von Willebrand factor and factor VIII from endothelial cells and is useful in mild-to-moderate disease."
    ],
    keyFacts:"Hemophilia A: factor VIII deficiency, X-linked recessive. Deep tissue bleeding and hemarthrosis (unlike platelet disorders which cause mucocutaneous bleeding). Elevated PTT, normal PT, normal platelets. Mixing study corrects PTT. Treatment: recombinant factor VIII. Desmopressin for mild disease. Hemophilia B: factor IX deficiency (same presentation and genetics)."
  },
  {
    id:162, diagnosis:"Antiphospholipid Antibody Syndrome",
    aliases:["antiphospholipid antibody syndrome","APS","antiphospholipid syndrome","lupus anticoagulant"],
    episode:"Ep. 180", topic:"Rheumatology / Hematology",
    ankiStep1:"Antiphospholipid_Antibody_Syndrome::recurrent_miscarriages_thrombosis::warfarin_anticoagulation",
    ankiStep2:"Antiphospholipid_Antibody_Syndrome::lupus_anticoagulant_elevated_PTT_paradox::anti-cardiolipin",
    clues:[
      "A 32-year-old woman is referred after her third spontaneous miscarriage, all occurring after 10 weeks of gestation. She also had a deep vein thrombosis following a long flight 2 years ago.",
      "She has a history of systemic lupus erythematosus. She reports livedo reticularis (a lace-like purplish skin discoloration) on her arms and legs.",
      "Coagulation studies show an elevated activated partial thromboplastin time of 58 seconds that does NOT correct on mixing with normal plasma — the 'lupus anticoagulant' paradox: the test suggests anticoagulation but the patient is actually HYPERCOAGULABLE.",
      "Anti-cardiolipin antibodies (IgG, very high titer) and anti-beta-2 glycoprotein-I antibodies are positive. Thrombocytopenia is also present (platelets 88,000/mcL) despite the hypercoagulable state.",
      "The antibodies target phospholipid-binding proteins and cause thrombosis of the uteroplacental arteries (leading to miscarriage) and systemic arterial and venous thrombosis. The paradoxical in vitro anticoagulant effect occurs because the antibodies interfere with the phospholipid surface needed for the test.",
      "This autoimmune hypercoagulable state is treated with anticoagulation: heparin (or low-molecular-weight heparin) plus low-dose aspirin during pregnancy for miscarriage prevention; long-term warfarin for thrombosis prevention."
    ],
    keyFacts:"Antiphospholipid antibody syndrome: recurrent miscarriages + arterial/venous thrombosis + thrombocytopenia. Lupus anticoagulant paradox: elevated PTT in vitro but hypercoagulable in vivo. Anti-cardiolipin and anti-beta-2-glycoprotein-I antibodies. Associated with systemic lupus erythematosus. Treatment: heparin + low-dose aspirin in pregnancy; warfarin long-term for thrombosis."
  },
  {
    id:163, diagnosis:"Testicular Torsion",
    aliases:["testicular torsion","spermatic cord torsion","torsion of testicle"],
    episode:"Ep. 265", topic:"Urology",
    ankiStep1:"Testicular_Torsion::venous_outflow_obstruction::exploratory_surgery_within_6_hours",
    ankiStep2:"Testicular_Torsion::sudden_severe_scrotal_pain_no_systemic_sx::decreased_blood_flow_ultrasound",
    clues:[
      "A 16-year-old boy presents to the emergency department at 2 AM with sudden-onset severe left testicular and scrotal pain that woke him from sleep. He vomited twice on the way to the hospital.",
      "He has no fever, no urethral discharge, and no history of prior sexually transmitted infections. He is in obvious distress holding his groin. The left testicle is high-riding and in a horizontal position (transverse lie).",
      "The cremasteric reflex is absent on the left (normally, stroking the inner thigh causes the ipsilateral testicle to elevate). The right testicle hangs normally and has a normal cremasteric reflex.",
      "Doppler ultrasound of the scrotum is considered, but given the classic presentation and short time window, the surgical team decides to proceed directly to the operating room. Imaging should not delay surgery when clinical suspicion is high.",
      "The spermatic cord has twisted 720 degrees. The testicle is dark blue-purple from venous engorgement. After manual detorsion in the operating room, blood flow is restored and the testicle is viable.",
      "Bilateral orchiopexy (fixing both testes to the scrotal wall with sutures) is performed to prevent recurrence on either side — the bell-clapper deformity that predisposes to torsion is usually bilateral. If the testicle is not viable within 6 hours, orchiectomy is required."
    ],
    keyFacts:"Testicular torsion: sudden severe testicular pain, high-riding horizontal testicle, absent cremasteric reflex, no fever/discharge. Venous obstruction causes hemorrhagic infarction. Surgical emergency — go directly to OR if clinical suspicion high (do not wait for ultrasound). Salvage rate: 100% under 6 hours, near 0% after 24 hours. Treatment: bilateral orchiopexy."
  },
  {
    id:164, diagnosis:"Seminoma",
    aliases:["seminoma","testicular seminoma","germ cell tumor testis","radiosensitive testicular cancer"],
    episode:"Ep. 265", topic:"Urology / Oncology",
    ankiStep1:"Seminoma::radiosensitive_chemosensitive::no_AFP_marker",
    ankiStep2:"Seminoma::painless_testicular_mass_young_male::orchiectomy_through_inguinal_approach",
    clues:[
      "A 28-year-old man notices a painless, hard lump in his right testicle while showering. He has had a sensation of heaviness in the scrotum for several weeks but attributed it to overexertion at the gym.",
      "On examination, the right testicle is enlarged with a firm, non-transilluminating mass arising from within the testicular parenchyma. There is no epididymal tenderness and no hydrocele. Scrotal skin is normal.",
      "Scrotal ultrasound confirms a 3.2 cm hypoechoic intratesticular mass with increased vascularity. CT abdomen-pelvis reveals enlarged retroperitoneal lymph nodes at the level of the renal vessels — the primary lymphatic drainage of the testis.",
      "Tumor markers are obtained before surgery: beta-human chorionic gonadotropin is mildly elevated, alpha-fetoprotein is NORMAL (alpha-fetoprotein elevation would indicate a non-seminomatous germ cell tumor), and lactate dehydrogenase is mildly elevated.",
      "Radical inguinal orchiectomy (never transscrotal biopsy, which disrupts lymphatic drainage and worsens staging) reveals a uniform, cream-colored tumor. Histology shows large cells with clear cytoplasm, central nuclei, and lymphocytic infiltrate between lobules.",
      "This is the most common testicular germ cell tumor, representing 50% of cases. It is exquisitely radiosensitive and chemosensitive. Stage I is treated with surveillance or adjuvant radiation/carboplatin. Metastatic disease responds well to bleomycin, etoposide, and cisplatin chemotherapy."
    ],
    keyFacts:"Seminoma: most common testicular germ cell tumor. Painless testicular mass. Normal AFP (elevation indicates non-seminoma). Mildly elevated beta-hCG and LDH. Histology: large cells with clear cytoplasm and lymphocytic stroma. Exquisitely radiosensitive and chemosensitive. Approach: radical inguinal orchiectomy (never transscrotal). Treatment: surveillance, radiation, or cisplatin-based chemotherapy depending on stage."
  },
  {
    id:165, diagnosis:"Septic Arthritis",
    aliases:["septic arthritis","infectious arthritis","bacterial arthritis","joint infection"],
    episode:"Ep. 29", topic:"Infectious Disease / Rheumatology",
    ankiStep1:"Septic_Arthritis::arthrocentesis_WBC_over_50000::joint_washout_antibiotics",
    ankiStep2:"Septic_Arthritis::Staph_aureus_MCC::Salmonella_sickle_cell_Neisseria_young_adult",
    clues:[
      "A 45-year-old man with type 2 diabetes presents with 3 days of fever, chills, and severe pain and swelling of the right knee. He cannot bear weight on the leg. He had a knee arthroscopy 4 weeks ago.",
      "On examination, temperature is 38.9°C, heart rate 108 bpm. The right knee is markedly swollen, warm, erythematous, and exquisitely tender to any movement. The joint is held in slight flexion.",
      "Arthrocentesis is performed. The synovial fluid is turbid and yellow-green. Joint fluid white blood cell count is 85,000/mcL with 95% polymorphonuclear cells, glucose is low, and protein is elevated.",
      "Gram stain of the joint fluid reveals gram-positive cocci in clusters. Blood cultures are also drawn. Radiograph shows no bony destruction (this occurs in later stages if untreated).",
      "Joint fluid culture grows Staphylococcus aureus — the most common cause of septic arthritis in all adult patients. In young sexually active adults, Neisseria gonorrhoeae is the most common cause and characteristically causes migratory polyarthritis with skin pustules.",
      "Urgent joint washout (arthroscopic lavage and debridement) is performed in addition to intravenous antibiotics. The joint must be surgically decompressed to prevent avascular necrosis and cartilage destruction from proteases."
    ],
    keyFacts:"Septic arthritis: joint fluid WBC over 50,000 with neutrophilic predominance. Most common cause overall: Staphylococcus aureus. Most common in young sexually active adults: Neisseria gonorrhoeae (migratory arthritis + skin pustules). Sickle cell disease: Salmonella. Diagnosis and treatment: arthrocentesis + joint washout + antibiotics. Gram-negative gonorrhea treated with ceftriaxone + doxycycline."
  },
  {
    id:166, diagnosis:"Tricuspid Stenosis",
    aliases:["tricuspid stenosis","right heart valve stenosis","tricuspid valve obstruction"],
    episode:"Ep. 29", topic:"Cardiology",
    ankiStep1:"Tricuspid_Stenosis::giant_A_waves_JVP::diastolic_rumble_left_sternal_border",
    ankiStep2:"Tricuspid_Stenosis::rheumatic_fever_carcinoid::increased_preload_intensifies",
    clues:[
      "A 52-year-old woman with a history of rheumatic fever at age 12 presents with progressive dyspnea, fatigue, and abdominal swelling. She has known mitral stenosis.",
      "On examination, jugular venous pulsations are markedly elevated. Careful inspection of the jugular waveform reveals giant 'A waves' — exaggerated pulsations caused by forceful right atrial contraction against increased resistance.",
      "A diastolic rumbling murmur is heard at the left lower sternal border (4th intercostal space). It increases in intensity with inspiration (the Carvallo sign) and with maneuvers that increase venous return (leg raise, inhalation).",
      "The murmur does not radiate to the axilla (ruling out mitral stenosis radiation) and is best heard along the left parasternal border rather than at the apex.",
      "Echocardiography confirms thickening and restricted opening of the tricuspid valve leaflets with a mean gradient of 6 mmHg across the valve. The right atrium is markedly dilated.",
      "Right heart outflow obstruction prevents the right ventricle from filling adequately. Signs of right heart failure dominate: elevated jugular venous pressure, hepatomegaly, ascites, and peripheral edema. Treatment: diuretics for symptoms; surgical or balloon valvuloplasty for severe disease."
    ],
    keyFacts:"Tricuspid stenosis: giant JVP A-waves + diastolic rumble at left lower sternal border. Increases with inspiration (Carvallo sign) and increased preload. Most common cause: rheumatic fever (often with concurrent mitral stenosis). Also: carcinoid syndrome (right-sided lesions due to serotonin inactivation in lungs). Signs: right heart failure (ascites, edema, hepatomegaly)."
  },
  {
    id:167, diagnosis:"Contrast-Induced Nephropathy",
    aliases:["contrast-induced nephropathy","contrast nephropathy","iodinated contrast kidney injury","radiocontrast nephropathy"],
    episode:"Ep. 29", topic:"Nephrology",
    ankiStep1:"Contrast_Nephropathy::creatinine_rise_24-48hrs_post_contrast::N-acetylcysteine_hydration",
    ankiStep2:"Contrast_Nephropathy::risk_CKD_diabetes::hold_metformin",
    clues:[
      "A 68-year-old man with chronic kidney disease (baseline creatinine 1.9 mg/dL) and type 2 diabetes undergoes a CT pulmonary angiogram with iodinated contrast for evaluation of suspected pulmonary embolism.",
      "He received 1 liter of intravenous normal saline before and after the procedure. The following day, his creatinine is 2.8 mg/dL. Urinalysis shows muddy brown casts consistent with acute tubular necrosis.",
      "He reports decreased urine output. Urine sodium is elevated and fractional excretion of sodium is greater than 2%, consistent with intrinsic renal injury rather than prerenal azotemia.",
      "His metformin had been continued before the procedure. Given the acute kidney injury, metformin is now held — it cannot be cleared by the impaired kidneys and accumulates, risking life-threatening lactic acidosis.",
      "The mechanism involves direct tubular toxicity of iodinated contrast media plus osmotically-induced renal vasoconstriction reducing medullary blood flow, causing tubular ischemia and necrosis within 24-48 hours of exposure.",
      "Prevention strategies: isotonic saline hydration before and after contrast, N-acetylcysteine (antioxidant that also vasodilates renal arteries), using low-osmolar or iso-osmolar contrast agents, and minimizing contrast volume."
    ],
    keyFacts:"Contrast-induced nephropathy: creatinine rise within 24-48 hours after iodinated contrast. Risk factors: CKD, diabetes, heart failure, dehydration, high contrast volume. Prevention: pre/post hydration with normal saline + N-acetylcysteine. Hold metformin before contrast (lactic acidosis risk). Alternative N-acetylcysteine uses: acetaminophen overdose antidote, mucolytic in cystic fibrosis."
  },
  {
    id:168, diagnosis:"Hidradenitis Suppurativa",
    aliases:["hidradenitis suppurativa","acne inversa","apocrine sweat gland inflammation","recurrent skin abscesses"],
    episode:"Ep. 31", topic:"Dermatology",
    ankiStep1:"Hidradenitis_Suppurativa::apocrine_gland_inflammation::axilla_groin_recurrent_abscesses",
    ankiStep2:"Hidradenitis_Suppurativa::T2DM_obesity_smoking::clindamycin_adalimumab",
    clues:[
      "A 25-year-old woman with type 1 diabetes and obesity presents with a 4-year history of recurrent painful nodules and boils in her armpits and groin. She has had more than 20 such lesions requiring incision and drainage.",
      "Physical examination of the bilateral axillae reveals multiple fluctuant nodules, open comedones, sinus tracts connecting lesions, and thick rope-like scars from previous healed lesions.",
      "The lesions are malodorous. She also has nodules along the inframammary fold and perianal region. The areas affected are exclusively hair-bearing regions with apocrine sweat glands.",
      "Culture of the purulent drainage grows mixed flora including Staphylococcus aureus. She has been treated repeatedly with clindamycin with partial response, but lesions continue to recur in the same areas.",
      "The underlying mechanism involves follicular occlusion of apocrine gland ducts, leading to rupture and secondary bacterial infection, rather than primary bacterial infection. It is NOT simply a recurrent skin abscess.",
      "This chronic, relapsing inflammatory disorder of apocrine sweat glands is treated with topical clindamycin or oral doxycycline for mild disease, adalimumab (anti-tumor necrosis factor alpha) for moderate-to-severe disease, and surgical excision for refractory lesions. Smoking cessation and weight loss improve outcomes."
    ],
    keyFacts:"Hidradenitis suppurativa: recurrent painful abscesses in apocrine gland-bearing areas (axilla, groin, inframammary, perianal). Follicular occlusion — NOT primary bacterial infection. Sinus tracts and scarring. Associated with obesity, smoking, metabolic syndrome, Crohn's disease. Most common organism when superinfected: Staphylococcus aureus. Treatment: clindamycin, doxycycline, adalimumab, surgery."
  },
  {
    id:169, diagnosis:"Carcinoid Syndrome",
    aliases:["carcinoid syndrome","carcinoid tumor with metastasis","serotonin-secreting tumor","neuroendocrine tumor with carcinoid syndrome"],
    episode:"Ep. 29", topic:"Gastroenterology / Oncology",
    ankiStep1:"Carcinoid_Syndrome::5-HIAA_urine::right_sided_heart_lesions_serotonin",
    ankiStep2:"Carcinoid_Syndrome::flushing_diarrhea_bronchospasm_TIPS::liver_metastasis_required",
    clues:[
      "A 49-year-old woman presents with years of episodic facial flushing, watery diarrhea (up to 10 episodes daily), and wheezing not explained by asthma medications. Episodes are often triggered by alcohol and stress.",
      "On examination, her face is chronically erythematous. Cardiac auscultation reveals a harsh systolic murmur over the left lower sternal border and a diastolic murmur consistent with right-sided valve lesions.",
      "Echocardiogram confirms tricuspid regurgitation and pulmonic stenosis — both right-sided lesions. Left-sided valves are normal because pulmonary circulation metabolizes the causative mediator before it reaches the left heart.",
      "24-hour urine 5-hydroxyindoleacetic acid (5-HIAA) — the urinary metabolite of serotonin — is markedly elevated at 48 mg/day (normal under 6 mg/day). Abdominal CT reveals a small bowel mass with multiple hepatic metastases.",
      "The patient has developed pellagra (dermatitis, diarrhea, dementia) because the tumor uses tryptophan to produce excess serotonin, leaving insufficient tryptophan for niacin (vitamin B3) synthesis.",
      "Carcinoid syndrome only occurs when tumor has metastasized to the liver, because the liver normally metabolizes serotonin from gut tumors. The 'Be FDR' mnemonic covers the features: Bronchospasm + Flushing + Diarrhea + Right-sided heart lesions. Treatment: octreotide for symptom control; surgical resection when possible."
    ],
    keyFacts:"Carcinoid syndrome (Be FDR): Bronchospasm + Flushing + Diarrhea + Right-sided heart lesions (TIPS: tricuspid insufficiency + pulmonic stenosis). Urine 5-HIAA elevated. Only occurs after liver metastasis (liver metabolizes serotonin). Niacin deficiency (pellagra) because tryptophan diverted to serotonin. Treatment: octreotide. Diagnosis of primary tumor: abdominal CT; if not found, octreotide scintigraphy."
  },
  {
    id:170, diagnosis:"Achalasia",
    aliases:["achalasia","esophageal achalasia","lower esophageal sphincter achalasia","bird beak esophagus"],
    episode:"Ep. 29", topic:"Gastroenterology",
    ankiStep1:"Achalasia::LES_failure_to_relax_Auerbach_plexus::bird_beak_barium_swallow",
    ankiStep2:"Achalasia::dysphagia_solids_AND_liquids::manometry_high_LES_pressure",
    clues:[
      "A 42-year-old woman presents with 3 years of progressive difficulty swallowing that affects both solid foods AND liquids equally. She has lost 20 pounds because eating is uncomfortable.",
      "She reports regurgitation of undigested food, particularly when lying down at night. She has had several episodes of aspiration pneumonia. She denies heartburn.",
      "Upper endoscopy reveals a dilated esophagus filled with retained food material. The lower esophageal sphincter resists passage of the endoscope but can be traversed with gentle pressure — the 'pop-through' sign.",
      "Barium swallow shows a dilated esophagus that tapers to a smooth, symmetric narrowing at the gastroesophageal junction — the classic 'bird's beak' appearance. There is no mucosal irregularity.",
      "High-resolution esophageal manometry confirms the diagnosis: the lower esophageal sphincter fails to completely relax with swallowing, and there is aperistalsis (absent peristaltic waves) of the esophageal body.",
      "The Auerbach (myenteric) plexus ganglionic cells are destroyed, impairing coordinated peristalsis and lower esophageal sphincter relaxation. Primary treatment options include pneumatic balloon dilation, Heller myotomy (surgical cutting of the lower esophageal sphincter muscle), or peroral endoscopic myotomy."
    ],
    keyFacts:"Achalasia: dysphagia to solids AND liquids equally (vs. structural lesion: solids worse). Bird's beak sign on barium swallow. Manometry: elevated lower esophageal sphincter pressure + incomplete relaxation + aperistalsis. Destruction of Auerbach's plexus. Secondary achalasia: Chagas disease (Trypanosoma cruzi). Treatment: pneumatic dilation, Heller myotomy, or per-oral endoscopic myotomy (POEM)."
  },
  {
    id:171, diagnosis:"Nephrolithiasis",
    aliases:["nephrolithiasis","kidney stones","renal calculi","urolithiasis"],
    episode:"Ep. 29", topic:"Nephrology / Urology",
    ankiStep1:"Nephrolithiasis::calcium_oxalate_most_common::CT_gold_standard",
    ankiStep2:"Nephrolithiasis::colicky_flank_pain_hematuria_tamsulosin::struvite_uric_acid_cystine",
    clues:[
      "A 35-year-old man presents to the emergency department with sudden-onset severe left flank pain that radiates to the groin and left testicle. The pain comes in waves and is the worst he has ever experienced.",
      "He is writhing and unable to find a comfortable position. He reports nausea and one episode of vomiting. Urinalysis reveals gross hematuria and no white blood cells.",
      "CT scan of the abdomen and pelvis without contrast (gold standard for stone detection) reveals a 6 mm calculus at the left ureterovesical junction with moderate hydronephrosis proximal to the stone.",
      "The stone is hyperdense on CT, consistent with a calcium-containing stone. Urine calcium, oxalate, and uric acid levels are measured. He has had 3 prior kidney stones.",
      "Stone analysis from a prior episode showed calcium oxalate — the most common type (75-80%). The characteristic microscopic appearance is envelope-shaped (X-shaped) crystals. Struvite (coffin-shaped, from Proteus mirabilis infection) and uric acid (radiolucent, coffee-colored) stones are less common.",
      "Treatment: IV ketorolac for pain, intravenous fluids, and tamsulosin (alpha-1 blocker relaxes the ureter, facilitating stone passage). Stones under 5 mm usually pass spontaneously; larger stones may require extracorporeal shock wave lithotripsy or ureteroscopy."
    ],
    keyFacts:"Nephrolithiasis: severe colicky flank pain radiating to groin + hematuria. CT without contrast is gold standard. Most common type: calcium oxalate (envelope-shaped crystals). Struvite: coffin-shaped, from urease-producing Proteus (alkaline urine). Uric acid: radiolucent. Cystine: hexagonal crystals, cystinuria (COLA transporter defect). Treatment: fluids + tamsulosin. Extracorporeal shock wave lithotripsy for larger stones."
  },
  {
    id:172, diagnosis:"Idiopathic Intracranial Hypertension",
    aliases:["idiopathic intracranial hypertension","pseudotumor cerebri","benign intracranial hypertension"],
    episode:"Ep. 45", topic:"Neurology",
    ankiStep1:"Idiopathic_Intracranial_Hypertension::elevated_opening_pressure_normal_CT::acetazolamide",
    ankiStep2:"Idiopathic_Intracranial_Hypertension::obese_female_papilledema_visual_loss::vitamin_A_tetracyclines",
    clues:[
      "A 32-year-old woman with a body mass index of 38 presents with 4 months of daily throbbing headaches, pulsatile tinnitus (whooshing sound in her ears), and intermittent blurring of vision in both eyes.",
      "She started minocycline 6 months ago for acne treatment. Fundoscopic examination reveals bilateral papilledema — disc swelling from elevated intracranial pressure transmitted to the optic nerve sheaths.",
      "Visual field testing shows an enlarged blind spot and progressive constriction of peripheral vision. She has transient visual obscurations (brief graying of vision) lasting seconds when she bends over.",
      "CT brain with and without contrast shows no mass lesion and normal-sized ventricles — the ventricles should be dilated if there were a true hydrocephalus, but in this condition, the cerebrospinal fluid pressure is elevated with no obstruction.",
      "Lumbar puncture reveals an opening pressure of 340 mmHg (normal under 250 mmHg). Cerebrospinal fluid composition is completely normal — no cells, normal protein and glucose. The pressure alone is elevated.",
      "Risk factors include obesity, female sex, vitamin A derivatives (isotretinoin), tetracycline antibiotics (doxycycline, minocycline), and glucocorticoid withdrawal. The causative medication is stopped. Treatment: acetazolamide (carbonic anhydrase inhibitor reduces cerebrospinal fluid production) and weight loss."
    ],
    keyFacts:"Idiopathic intracranial hypertension: obese woman + headache + papilledema + pulsatile tinnitus + visual changes. CT normal (no mass, normal ventricles). LP: elevated opening pressure with normal CSF composition. Risk factors: obesity, female sex, vitamin A derivatives, tetracyclines. Treatment: acetazolamide + weight loss. Serial LPs or optic nerve sheath fenestration for visual loss."
  },
  {
    id:173, diagnosis:"Follicular Thyroid Carcinoma",
    aliases:["follicular thyroid carcinoma","follicular thyroid cancer","follicular thyroid tumor","hematogenous thyroid cancer"],
    episode:"Ep. 102", topic:"Endocrinology / Oncology",
    ankiStep1:"Follicular_Thyroid_Carcinoma::hematogenous_spread_bone_lung::RAS_mutation",
    ankiStep2:"Follicular_Thyroid_Carcinoma::FNA_insufficient_requires_excision::cold_nodule",
    clues:[
      "A 55-year-old woman is found to have a 2.2 cm thyroid nodule incidentally on a carotid ultrasound. She has no neck pain, dysphagia, or voice changes. TSH is normal.",
      "Thyroid ultrasound confirms a solid, hypoechoic nodule with irregular margins and microcalcifications in the right lower pole. Fine needle aspiration biopsy is performed.",
      "The cytology report is classified as 'follicular neoplasm' or Bethesda category IV — fine needle aspiration cannot distinguish follicular adenoma (benign) from this malignancy because the diagnosis requires evidence of capsular or vascular invasion visible only in the full excised specimen.",
      "She undergoes right hemithyroidectomy. Permanent histology reveals a well-differentiated follicular neoplasm with unequivocal vascular invasion of four vessels within and beyond the tumor capsule.",
      "Unlike papillary thyroid carcinoma (which spreads through lymphatics), this tumor spreads hematogenously — most commonly to bone (causing lytic lesions) and lungs. Lymph node involvement is uncommon.",
      "Radioactive iodine ablation (iodine-131) is administered after total thyroidectomy, as this tumor avidly takes up iodine (unlike medullary or anaplastic thyroid cancer). Thyroglobulin serves as the tumor marker for surveillance."
    ],
    keyFacts:"Follicular thyroid carcinoma: hematogenous spread to bone and lungs (contrast with papillary = lymphatic spread). Cold nodule. Fine needle aspiration cannot diagnose (shows follicular neoplasm — requires surgical excision). RAS mutation. Treatment: total thyroidectomy + radioactive iodine ablation. Surveillance: thyroglobulin levels."
  },
  {
    id:174, diagnosis:"Benign Prostatic Hyperplasia",
    aliases:["benign prostatic hyperplasia","BPH","prostatic enlargement","lower urinary tract symptoms male"],
    episode:"Ep. 265", topic:"Urology",
    ankiStep1:"Benign_Prostatic_Hyperplasia::alpha_1_blocker_tamsulosin::5_alpha_reductase_inhibitor",
    ankiStep2:"Benign_Prostatic_Hyperplasia::obstructive_lower_urinary_tract_symptoms::median_lobe_transition_zone",
    clues:[
      "A 68-year-old man presents with a 2-year history of urinary hesitancy, weak urine stream, incomplete bladder emptying, and nocturia (waking 3-4 times per night to urinate). He has to strain to initiate urination.",
      "He denies gross hematuria, pelvic pain, fever, or dysuria. Digital rectal examination reveals a smoothly enlarged, rubbery, non-tender prostate with preserved median sulcus. No nodules are palpable.",
      "Prostate-specific antigen is mildly elevated at 4.2 ng/mL (normal under 4.0 ng/mL), consistent with an enlarged prostate. Post-void residual urine volume measured by ultrasound is 180 mL (normal under 50 mL).",
      "Uroflowmetry shows a reduced peak urinary flow rate of 8 mL/second (normal over 15 mL/second). Cystoscopy reveals bladder trabeculation from chronically elevated detrusor pressures and narrowing of the prostatic urethra.",
      "The hyperplasia originates in the transition zone (central periurethral gland) — the central growth compresses the urethra. This is in contrast to prostate cancer, which arises in the peripheral zone (palpable on rectal examination).",
      "Medical treatment: alpha-1 blockers (tamsulosin, doxazosin) relax smooth muscle acutely; 5-alpha-reductase inhibitors (finasteride, dutasteride) reduce prostate volume over months. Surgical treatment: transurethral resection of the prostate for refractory disease."
    ],
    keyFacts:"Benign prostatic hyperplasia: obstructive lower urinary tract symptoms in older men. Transition zone hyperplasia compresses urethra. Digital rectal exam: smooth, rubbery, enlarged prostate. Treatment: alpha-1 blockers (tamsulosin — rapid relief) + 5-alpha-reductase inhibitors (finasteride — reduces size, months for effect). Surgery: transurethral resection of the prostate for refractory cases or acute urinary retention."
  },
  {
    id:175, diagnosis:"Epididymitis",
    aliases:["epididymitis","testicular epididymitis","infectious epididymitis","epididymo-orchitis"],
    episode:"Ep. 265", topic:"Urology / Infectious Disease",
    ankiStep1:"Epididymitis::Chlamydia_young_Ecoli_older::ceftriaxone_doxycycline",
    ankiStep2:"Epididymitis::gradual_scrotal_pain_fever::positive_cremasteric_reflex",
    clues:[
      "A 22-year-old sexually active man presents with 4 days of gradually worsening right scrotal pain and swelling accompanied by low-grade fever and urethral discharge.",
      "He does not use condoms consistently. His last sexual contact was 10 days ago with a new partner. He denies trauma. The pain developed slowly over days, unlike testicular torsion.",
      "Examination reveals tenderness localized to the posterior aspect of the right testicle (the epididymis). The testicle itself is in a normal anatomic position and the cremasteric reflex is intact.",
      "Urinalysis reveals pyuria and leukocyte esterase. Urethral swab and nucleic acid amplification testing are sent for Neisseria gonorrhoeae and Chlamydia trachomatis.",
      "Scrotal Doppler ultrasound shows increased blood flow to the epididymis — distinguishing epididymitis from testicular torsion (which shows decreased blood flow). The testicle shows normal blood flow.",
      "In young sexually active men under 35 years, Chlamydia trachomatis and Neisseria gonorrhoeae are the most common causes. Treatment: ceftriaxone (single dose) plus doxycycline for 10 days. In older men or after urologic procedures, Escherichia coli and other enteric organisms predominate; treat with fluoroquinolones."
    ],
    keyFacts:"Epididymitis: gradual scrotal pain + fever + urethral discharge + intact cremasteric reflex + increased blood flow on Doppler. Young men under 35: Chlamydia trachomatis and Neisseria gonorrhoeae (sexually transmitted). Older men: Escherichia coli (enteric). Treatment: young — ceftriaxone + doxycycline; older — fluoroquinolones. Contrast with testicular torsion: sudden pain, absent cremasteric reflex, decreased blood flow."
  },
  {
    id:176, diagnosis:"Osteochondroma",
    aliases:["osteochondroma","exostosis","cartilage-capped bony outgrowth","most common benign bone tumor"],
    episode:"Ep. 29", topic:"Orthopedics / Oncology",
    ankiStep1:"Osteochondroma::most_common_benign_bone_tumor::cartilage_cap_malignant_transformation",
    ankiStep2:"Osteochondroma::mushroom_shaped_metaphysis::hereditary_multiple_exostoses",
    clues:[
      "A 16-year-old boy presents with a slowly growing, painless bony lump on the outer side of his left knee that he first noticed 2 years ago. He denies any history of trauma. The lump has not changed significantly in size.",
      "Physical examination reveals a firm, non-tender, immobile bony protuberance arising from the distal femoral metaphysis. Overlying skin is normal. Range of motion of the knee is full.",
      "Radiograph of the left knee shows a sessile (broad-based) exostosis arising from the cortex of the distal femoral metaphysis, with the medullary cavity of the lesion continuous with the medullary cavity of the femur.",
      "The lesion has a cartilaginous cap visible on MRI measuring 4 mm in thickness. The cartilage cap should be under 15 mm in adults; a cap thicker than 15 mm raises concern for malignant transformation to chondrosarcoma.",
      "This is the most common benign bone tumor. It most commonly arises in the metaphysis of long bones, particularly around the knee (distal femur, proximal tibia).",
      "Most osteochondromas are solitary and do not require treatment unless causing symptoms (pain from bursa formation, nerve or vessel compression). Multiple hereditary exostoses (hereditary multiple osteochondromas) is an autosomal dominant condition with higher malignant transformation risk."
    ],
    keyFacts:"Osteochondroma: most common benign bone tumor. Painless, firm metaphyseal exostosis with continuous medullary cavity on X-ray. Cartilaginous cap — thickness over 15 mm in adults raises concern for chondrosarcoma. Most common location: distal femur/proximal tibia (around knee). Multiple lesions: hereditary multiple exostoses (autosomal dominant). Treatment: observation; excision if symptomatic."
  },
  {
    id:177, diagnosis:"Pernicious Anemia",
    aliases:["pernicious anemia","autoimmune gastritis B12 deficiency","intrinsic factor deficiency","anti-intrinsic factor antibody anemia"],
    episode:"Ep. 30", topic:"Hematology / Gastroenterology",
    ankiStep1:"Pernicious_Anemia::anti_intrinsic_factor_antibodies::subacute_combined_degeneration",
    ankiStep2:"Pernicious_Anemia::macrocytic_anemia_elevated_MMA::B12_injections",
    clues:[
      "A 58-year-old woman with hypothyroidism (Hashimoto's thyroiditis) presents with fatigue, shortness of breath on exertion, and difficulty walking due to unsteadiness. She also complains of tingling and numbness in both feet.",
      "On examination, she has a beefy red, smooth tongue (atrophic glossitis). Neurological examination reveals decreased vibration and proprioception sense in the lower extremities and bilateral Babinski signs.",
      "Complete blood count shows hemoglobin 8.4 g/dL with a markedly elevated MCV of 118 fL. Peripheral smear shows macro-ovalocytes and hypersegmented neutrophils (5-lobed nuclei) — pathognomonic for megaloblastic anemia.",
      "Serum vitamin B12 level is critically low at 68 pg/mL. Serum methylmalonic acid and homocysteine are both markedly elevated — elevated methylmalonic acid is specific for vitamin B12 deficiency (folate deficiency elevates homocysteine but not methylmalonic acid).",
      "Anti-intrinsic factor antibodies are positive — the diagnostic hallmark. Anti-parietal cell antibodies are also positive. Schilling test (if performed) would show reduced absorption of radiolabeled B12 that corrects with intrinsic factor supplementation.",
      "Autoimmune destruction of gastric parietal cells eliminates intrinsic factor production, preventing B12 absorption in the terminal ileum. The neurological damage (subacute combined degeneration of dorsal columns and corticospinal tracts) is irreversible if untreated. Treatment: intramuscular B12 injections (lifelong)."
    ],
    keyFacts:"Pernicious anemia: autoimmune destruction of parietal cells → no intrinsic factor → B12 malabsorption. Anti-intrinsic factor antibodies (diagnostic). Macrocytic anemia + subacute combined degeneration (dorsal columns + lateral corticospinal tracts). Elevated MMA AND homocysteine (folate deficiency raises only homocysteine). Association: other autoimmune diseases (Hashimoto's, vitiligo). Treatment: intramuscular B12 injections (lifelong)."
  },
  {
    id:178, diagnosis:"Nephrogenic Systemic Fibrosis",
    aliases:["nephrogenic systemic fibrosis","gadolinium fibrosis","nephrogenic fibrosing dermopathy"],
    episode:"Ep. 29", topic:"Nephrology / Dermatology",
    ankiStep1:"Nephrogenic_Systemic_Fibrosis::gadolinium_MRI_CKD::skin_fibrosis_joint_contracture",
    ankiStep2:"Nephrogenic_Systemic_Fibrosis::avoid_gadolinium_severe_CKD::no_proven_treatment",
    clues:[
      "A 54-year-old man with end-stage renal disease on hemodialysis presents with 6 weeks of progressive hardening and thickening of the skin of his arms and legs.",
      "On examination, the skin of the bilateral forearms and thighs is markedly indurated, with a peau d'orange (orange peel) texture and tethering to underlying tissue. The skin changes are symmetric and cannot be pinched.",
      "He is developing flexion contractures of his elbows, knees, and hips due to fibrosis of the underlying soft tissues and fascia. He received a gadolinium-enhanced MRI of the brain 8 weeks ago for evaluation of a neurological symptom.",
      "Skin biopsy reveals increased collagen deposition, fibroblast proliferation, and CD34-positive fibrocytes — a pattern consistent with gadolinium-induced fibrosis.",
      "The gadolinium contrast agent was not efficiently excreted by his non-functioning kidneys and accumulated in tissues, where it triggered a fibrotic response in skin, muscle, and internal organs.",
      "This rare but serious condition is caused by gadolinium-based contrast agents in patients with severe chronic kidney disease (estimated glomerular filtration rate under 30) or acute kidney injury. Prevention: avoid gadolinium in high-risk patients; use the lowest dose of stable macrocyclic gadolinium agents. There is no proven treatment."
    ],
    keyFacts:"Nephrogenic systemic fibrosis: gadolinium contrast in patients with severe CKD or on dialysis causes progressive skin fibrosis and joint contractures. Bilateral, symmetric, indurated skin — cannot be pinched. Internal organ involvement can occur. Prevention: avoid gadolinium when GFR under 30. No proven treatment (physical therapy for contractures). Contrast with scleroderma (which has Raynaud's, systemic features, and specific antibodies)."
  },
  {
    id:179, diagnosis:"Addisonian Crisis",
    aliases:["addisonian crisis","acute adrenal crisis","adrenal crisis","acute adrenal insufficiency"],
    episode:"Ep. 30", topic:"Endocrinology",
    ankiStep1:"Addisonian_Crisis::acute_adrenal_insufficiency::high_dose_hydrocortisone_immediately",
    ankiStep2:"Addisonian_Crisis::hypotension_hyponatremia_hyperkalemia_trigger::stress_dose_steroids",
    clues:[
      "A 44-year-old woman with known Addison disease presents to the emergency department via ambulance after developing severe weakness, vomiting, and loss of consciousness at home. She had a gastroenteritis illness for 3 days and ran out of her hydrocortisone prescription.",
      "Vital signs: blood pressure 58/30 mmHg despite 2 liters of normal saline, heart rate 128 bpm, temperature 38.4°C. She is profusely diaphoretic and confused.",
      "Labs: sodium 121 mEq/L, potassium 6.8 mEq/L, glucose 38 mg/dL, and bicarbonate 14 mEq/L. Random cortisol is critically low at 2 mcg/dL in the setting of severe physiologic stress.",
      "Electrocardiogram shows peaked T-waves and widening of the QRS complex from the hyperkalemia. Intravenous calcium gluconate is given to stabilize the cardiac membrane.",
      "The critical missing element is glucocorticoid coverage during a physiologic stressor (illness). Patients with adrenal insufficiency cannot mount the normal cortisol surge that maintains blood pressure during stress.",
      "Immediate treatment: intravenous hydrocortisone 100 mg bolus (do NOT wait for confirmatory testing), followed by continuous dextrose-containing saline infusion. This is a medical emergency — mortality without treatment is near 100%. Patients must be educated about sick-day rules (doubling doses during illness)."
    ],
    keyFacts:"Addisonian crisis: acute life-threatening adrenal insufficiency triggered by physiologic stress (illness, surgery, missed medication). Severe hypotension + hyponatremia + hyperkalemia + hypoglycemia. Do NOT wait for cosyntropin test — give intravenous hydrocortisone immediately. Hydrocortisone has intrinsic mineralocorticoid activity at high doses. Educate patients: double steroid dose when ill."
  },
  {
    id:180, diagnosis:"Sickle Cell Osteomyelitis",
    aliases:["sickle cell osteomyelitis","salmonella osteomyelitis","osteomyelitis in sickle cell disease"],
    episode:"Ep. 31", topic:"Infectious Disease / Hematology",
    ankiStep1:"Sickle_Cell_Osteomyelitis::Salmonella_MCC::differentiate_from_vaso-occlusive_crisis",
    ankiStep2:"Sickle_Cell_Osteomyelitis::bone_pain_fever_elevated_WBC::fluoroquinolones",
    clues:[
      "A 14-year-old boy with known sickle cell disease presents with 5 days of severe right thigh pain and fever to 39.2°C. He has had multiple prior vaso-occlusive pain crises, but this episode feels different — the pain is constant rather than episodic, and worsened over days.",
      "On examination, there is point tenderness, warmth, and swelling over the right mid-femur. He is unable to bear weight. White blood cell count is 18,500/mcL (higher than typical for a vaso-occlusive crisis).",
      "MRI of the right femur shows bone marrow edema and periosteal elevation in the diaphysis consistent with osteomyelitis. Blood cultures are obtained before antibiotic initiation.",
      "In most patients, the most common cause of osteomyelitis is Staphylococcus aureus. However, in patients with sickle cell disease, an unusual organism predominates due to functional asplenia increasing susceptibility.",
      "Blood cultures return positive for a gram-negative rod that is non-lactose fermenting on MacConkey agar, oxidase negative, and produces hydrogen sulfide. This organism is the same one implicated in enteric fever.",
      "Salmonella species cause osteomyelitis in sickle cell disease due to functional asplenia (the spleen normally filters encapsulated and intracellular organisms). The bone infarcts from sickling create ideal conditions for bacterial seeding. Treatment: fluoroquinolones (ciprofloxacin) or third-generation cephalosporins."
    ],
    keyFacts:"Osteomyelitis in sickle cell disease: most common causative organism is Salmonella (unlike the general population where Staphylococcus aureus predominates). Functional asplenia from repeated infarcts. Fever + focal bone pain + elevated WBC distinguishes from vaso-occlusive crisis. Treatment: fluoroquinolones or third-generation cephalosporins. Also note: Staphylococcus aureus is still the second most common cause in sickle cell patients."
  },
  {
    id:181, diagnosis:"Malignant Hyperthermia",
    aliases:["malignant hyperthermia","MH","ryanodine receptor mutation anesthesia","volatile anesthetic hyperthermia"],
    episode:"Ep. 45", topic:"Pharmacology / Anesthesiology",
    ankiStep1:"Malignant_Hyperthermia::RYR1_mutation_volatile_anesthetics::dantrolene",
    ankiStep2:"Malignant_Hyperthermia::intraoperative_hyperthermia_rigidity_rhabdomyolysis::succinylcholine",
    clues:[
      "A 28-year-old man with no prior medical history undergoes elective knee surgery under general anesthesia using sevoflurane and succinylcholine for intubation. Thirty minutes into the case, the anesthesiologist notices an unexplained rise in end-tidal CO2.",
      "The patient develops tachycardia (heart rate 142 bpm), rapidly rising core temperature now 41.8°C, and generalized skeletal muscle rigidity despite deepening anesthesia. The masseter muscle (jaw) is clenched so tightly the endotracheal tube cannot be moved.",
      "Arterial blood gas shows severe mixed respiratory and metabolic acidosis (pH 6.98). Creatine kinase is greater than 50,000 IU/L. Dark brown urine indicates myoglobinuria from muscle breakdown.",
      "The triggering agents are identified — succinylcholine (depolarizing neuromuscular blocker) and sevoflurane (a volatile halogenated anesthetic). Both agents trigger uncontrolled calcium release from the sarcoplasmic reticulum in genetically susceptible individuals.",
      "The underlying mutation is in the ryanodine receptor (RYR1) gene, causing dysfunctional sarcoplasmic reticulum calcium channels that release massive amounts of calcium into skeletal muscle cells when exposed to triggering agents.",
      "Emergency treatment: stop all triggering agents immediately, switch to non-triggering agents (propofol, benzodiazepines), administer dantrolene sodium intravenously (inhibits RYR1 calcium release channel), apply active cooling, and correct metabolic acidosis."
    ],
    keyFacts:"Malignant hyperthermia: intraoperative hyperthermia + rigidity + rhabdomyolysis + rising end-tidal CO2. Triggered by volatile anesthetics (halothane, sevoflurane) and succinylcholine. Autosomal dominant RYR1 mutation. Masseter spasm ('jaws of steel') is early sign. Treatment: STOP triggers + dantrolene (RYR1 inhibitor) + active cooling. Safe anesthetics: propofol, nitrous oxide, local anesthetics."
  },
  {
    id:182, diagnosis:"Neuroleptic Malignant Syndrome",
    aliases:["neuroleptic malignant syndrome","NMS","antipsychotic hyperthermia","dopamine antagonist hyperthermia"],
    episode:"Ep. 45", topic:"Pharmacology / Psychiatry",
    ankiStep1:"Neuroleptic_Malignant_Syndrome::dopamine_antagonist_hyperthermia::bromocriptine_dantrolene",
    ankiStep2:"Neuroleptic_Malignant_Syndrome::FALTER_mnemonic_lead_pipe_rigidity::stop_antipsychotic",
    clues:[
      "A 35-year-old man with schizophrenia presents to the emergency department with high fever of 40.5°C, severe generalized muscle rigidity, and altered mental status 5 days after his haloperidol dose was doubled by his outpatient psychiatrist.",
      "On examination, muscle tone is dramatically increased throughout — described as 'lead-pipe' rigidity (a constant resistance to passive movement, not the ratcheting cogwheel rigidity of parkinsonism). He is diaphoretic and appears confused.",
      "Laboratory studies show creatine kinase of 42,000 IU/L (elevated from muscle breakdown), white blood cells 18,000/mcL, elevated creatinine (acute kidney injury from myoglobinuria), and metabolic acidosis.",
      "Vital signs reveal autonomic instability — blood pressure fluctuates between 80/50 and 190/110 mmHg over 2 hours. Hyperthermia, diaphoresis, sialorrhea, and tachycardia represent a diffuse autonomic storm.",
      "The offending antipsychotic is stopped immediately. The mechanism involves acute dopamine blockade in the hypothalamus (impairing thermoregulation) and striatum (causing rigidity) and in the spinal cord (causing autonomic instability).",
      "Treatment: benzodiazepines for agitation, dantrolene for hyperthermia and rigidity, bromocriptine or amantadine (dopamine agonists to reverse dopamine blockade). Cooling measures and aggressive hydration for rhabdomyolysis. This is a medical emergency distinct from malignant hyperthermia."
    ],
    keyFacts:"Neuroleptic malignant syndrome: FALTER (Fever + Altered mental status + Leukocytosis + Tremor/rigidity + Elevated CK + autonomic instability with Rigidity). Caused by dopamine antagonists (antipsychotics, metoclopramide). Lead-pipe rigidity. Stop offending drug. Treatment: dantrolene + bromocriptine + benzodiazepines + cooling. Compare with serotonin syndrome: clonus/hyperreflexia, onset hours (not days)."
  },
  {
    id:183, diagnosis:"Serotonin Syndrome",
    aliases:["serotonin syndrome","serotonin toxicity","serotonin excess","selective serotonin reuptake inhibitor toxicity"],
    episode:"Ep. 45", topic:"Pharmacology / Toxicology",
    ankiStep1:"Serotonin_Syndrome::clonus_hyperreflexia_hyperthermia::cyproheptadine",
    ankiStep2:"Serotonin_Syndrome::rapid_onset_hours_SSRIs_MAOIs::Hunter_criteria",
    clues:[
      "A 42-year-old woman with depression and migraines presents to the emergency department 4 hours after taking sumatriptan (triptan) for a severe migraine. She takes sertraline and recently started linezolid for a skin infection.",
      "She is agitated and confused. On examination, temperature is 39.8°C. There is generalized tremor, hyperreflexia, and — most importantly — spontaneous and inducible clonus (rhythmic muscle contractions) of the ankles.",
      "The clonus is the key distinguishing finding: in neuroleptic malignant syndrome, reflexes are normal and there is no clonus, just lead-pipe rigidity. This patient has hyperreflexia and prominent clonus in addition to muscle rigidity.",
      "Her pupils are dilated (mydriasis) and she is diaphoretic. Vital signs show heart rate 138 bpm and blood pressure 158/96 mmHg. Skin is warm and flushed. Bowel sounds are hyperactive.",
      "The combination of serotonergic agents — sertraline (selective serotonin reuptake inhibitor), linezolid (monoamine oxidase A inhibitor that prevents serotonin breakdown), and sumatriptan (5-HT1B/1D agonist) — caused excessive serotonergic activity at the postsynaptic receptor.",
      "Treatment: stop all serotonergic agents, benzodiazepines for agitation and neuromuscular hyperactivity, cyproheptadine (serotonin receptor antagonist) as antidote, cooling for hyperthermia. Most cases resolve within 24 hours of stopping the offending agents."
    ],
    keyFacts:"Serotonin syndrome: rapid onset (hours) after serotonergic drug combination. Triad: altered mental status + autonomic instability + neuromuscular abnormalities (CLONUS and hyperreflexia are key). Mydriasis, hyperactive bowel sounds. Common triggers: SSRI + MAO inhibitor, triptans, linezolid, tramadol, fentanyl, St. John's wort. Treatment: cyproheptadine + benzodiazepines + stop offending drugs. Clonus distinguishes from NMS."
  },
  {
    id:184, diagnosis:"Normal Pressure Hydrocephalus",
    aliases:["normal pressure hydrocephalus","NPH","communicating hydrocephalus elderly","wet wacky wobbly"],
    episode:"Ep. 45", topic:"Neurology",
    ankiStep1:"Normal_Pressure_Hydrocephalus::wet_wacky_wobbly_triad::ventriculoperitoneal_shunt",
    ankiStep2:"Normal_Pressure_Hydrocephalus::enlarged_ventricles_normal_pressure_LP::magnetic_gait",
    clues:[
      "A 76-year-old man is brought by his family for evaluation of progressive forgetfulness, urinary incontinence despite no urinary tract infection, and difficulty walking over the past year.",
      "His family reports the walking difficulty is distinct — he walks with small, shuffling steps and his feet appear 'glued to the floor,' as if he cannot lift them. He falls frequently. He was initially thought to have Parkinson's disease, but his tremor workup was negative.",
      "Neuropsychological testing confirms frontal-subcortical dementia (slowed processing, poor executive function) without the memory loss typical of Alzheimer's disease. He is able to follow simple commands.",
      "CT brain shows markedly enlarged ventricles (ventriculomegaly) out of proportion to the degree of cortical atrophy — the key imaging finding. In Alzheimer's disease, the cortical atrophy matches the ventricular enlargement.",
      "Lumbar puncture is performed and reveals a normal opening pressure (less than 200 mmHg) despite the enlarged ventricles — the paradox of this condition. Removal of 30-50 mL of cerebrospinal fluid causes temporary improvement in gait (positive tap test).",
      "The clinical triad of wet (urinary incontinence), wacky (dementia), and wobbly (gait disturbance — magnetic gait) in an elderly patient with ventriculomegaly and normal cerebrospinal fluid pressure is classic. Treatment: ventriculoperitoneal shunt placement; gait improves most reliably, dementia may partially improve."
    ],
    keyFacts:"Normal pressure hydrocephalus: classic triad — Wet (urinary incontinence) + Wacky (dementia) + Wobbly (magnetic gait). Enlarged ventricles disproportionate to cortical atrophy on imaging. Lumbar puncture: normal opening pressure. Positive tap test (improvement after cerebrospinal fluid removal). Treatment: ventriculoperitoneal shunt — gait improves best, incontinence second, dementia least."
  },
  {
    id:185, diagnosis:"Varicocele",
    aliases:["varicocele","pampiniform plexus dilation","bag of worms scrotum","male infertility varicocele"],
    episode:"Ep. 265", topic:"Urology",
    ankiStep1:"Varicocele::bag_of_worms_pampiniform_plexus::left_sided_predominance",
    ankiStep2:"Varicocele::male_infertility_impaired_spermatogenesis::surgical_repair_embolization",
    clues:[
      "A 28-year-old man presents to a fertility clinic with his wife after 18 months of unsuccessful attempts to conceive. His wife has a normal fertility evaluation. He is otherwise healthy and has no history of mumps, trauma, or cryptorchidism.",
      "Semen analysis reveals decreased sperm motility (asthenospermia), decreased sperm count (oligospermia), and increased abnormal sperm morphology (teratospermia). He is azoospermic on a repeat sample.",
      "On physical examination while the patient is standing, a soft, compressible, worm-like mass is palpable in the left hemiscrotum superior and posterior to the left testicle. The mass disappears when the patient lies flat (empties on recumbency).",
      "Valsalva maneuver (bearing down) increases the size and prominence of the mass. The left testicle is slightly smaller than the right, suggesting atrophy from the chronic venous congestion.",
      "Scrotal Doppler ultrasound confirms dilated, tortuous veins of the pampiniform plexus greater than 3 mm in diameter with reversal of flow on Valsalva. The left testicular vein drains directly into the left renal vein at a right angle, predisposing to increased hydrostatic pressure.",
      "The elevated scrotal temperature from venous blood pooling impairs spermatogenesis (which requires temperatures 2°C lower than body temperature). Surgical ligation (varicocelectomy) or percutaneous embolization of the varicocele improves semen parameters and pregnancy rates."
    ],
    keyFacts:"Varicocele: bag-of-worms scrotum, predominantly LEFT-sided (left testicular vein drains into left renal vein at 90°, increasing pressure). Disappears when supine. Causes male infertility through elevated scrotal temperature impairing spermatogenesis. Sudden left-sided varicocele in older man: rule out left renal cell carcinoma (obstructing left renal vein). Treatment: varicocelectomy or percutaneous embolization."
  },
];

/* ───────────── STATE ───────────── */
let state = {
  caseIndex: getDailyIndex(),
  revealedClues: 1,
  attempts: [],
  status: 'playing',
  sugIdx: -1
};
let stats = loadStats();
let summaryOpen = false;

/* ───────────── INIT ───────────── */
function getDailyIndex() {
  const d = new Date();
  const day = Math.floor(d.getTime() / 86400000);
  return day % CASES.length;
}

function init() {
  // Try to restore today's state
  const key = 'dip_state_' + new Date().toDateString();
  try {
    const s = JSON.parse(localStorage.getItem(key));
    if (s) state = s;
  } catch(e) {}

  document.getElementById('totalNum').textContent = CASES.length;

  // Cookie banner — show only if not already accepted
  try {
    if (!localStorage.getItem('dip_cookies')) {
      document.getElementById('cookieBanner').style.display = 'flex';
    }
  } catch(e) {
    // localStorage blocked (sandboxed/private mode) — skip the banner entirely
  }

  renderAll();
  setupInput();
  setupKeyboard();
}

function renderAll() {
  const c = CASES[state.caseIndex];
  const d = new Date();

  document.getElementById('caseNum').textContent = state.caseIndex + 1;
  document.getElementById('caseDate').textContent = d.toLocaleDateString('en-US', { weekday:'long', year:'numeric', month:'long', day:'numeric' });
  document.getElementById('epTag').textContent = c.topic + ' — ' + c.episode;
  document.getElementById('prevBtn').disabled = state.caseIndex === 0;
  document.getElementById('nextBtn').disabled = state.caseIndex === CASES.length - 1;

  renderPips();
  renderClues();
  renderHistory();

  const over = state.status !== 'playing';
  document.getElementById('inputSection').style.display = over ? 'none' : 'block';
  document.getElementById('actionRow').style.display = 'flex';

  if (over) {
    renderResult();
  } else {
    document.getElementById('resultBanner').style.display = 'none';
    document.getElementById('summarySection').style.display = 'none';
  }
}

function renderPips() {
  const bar = document.getElementById('attemptPips');
  bar.innerHTML = '';
  for (let i = 0; i < 6; i++) {
    const pip = document.createElement('div');
    pip.className = 'attempt-pip';
    const a = state.attempts[i];
    if (a !== undefined) {
      pip.classList.add(isCorrect(a) ? 'won' : 'used');
    } else if (i === state.attempts.length && state.status === 'playing') {
      pip.classList.add('current');
    }
    bar.appendChild(pip);
  }
}

function renderClues() {
  const c = CASES[state.caseIndex];
  const numToShow = (state.status !== 'playing') ? c.clues.length : state.revealedClues;
  const list = document.getElementById('clueList');
  list.innerHTML = '';
  for (let i = 0; i < numToShow; i++) {
    const block = document.createElement('div');
    block.className = 'clue-block';
    block.innerHTML = `
      <div class="clue-row">
        <div class="clue-badge">${i + 1}</div>
        <div class="clue-text">${c.clues[i]}</div>
      </div>
      ${i < numToShow - 1 ? '<div class="clue-separator"></div>' : ''}
    `;
    list.appendChild(block);
  }
}

function renderHistory() {
  const h = document.getElementById('guessHistory');
  h.innerHTML = '';
  state.attempts.forEach((g, i) => {
    const row = document.createElement('div');
    const skip = g === '[Skipped]';
    const correct = !skip && isCorrect(g);
    row.className = `guess-row ${skip ? 'skipped' : correct ? 'correct' : 'wrong'}`;
    row.innerHTML = `
      <span class="guess-icon">${skip ? '—' : correct ? '✓' : '✗'}</span>
      <span><span class="guess-label">Attempt ${i+1}:</span> ${g}</span>
    `;
    h.appendChild(row);
  });
}

function renderResult() {
  const c = CASES[state.caseIndex];
  const won = state.status === 'won';

  // Banner
  const banner = document.getElementById('resultBanner');
  banner.style.display = 'block';
  banner.className = won ? 'win' : 'lose';
  document.getElementById('bannerEmoji').textContent  = won ? '🎉' : '😔';
  document.getElementById('bannerTitle').textContent = won
    ? `Correct! Diagnosed in ${state.attempts.length} attempt${state.attempts.length===1?'':'s'}!`
    : 'The answer has been revealed below.';
  document.getElementById('bannerSub').textContent = won
    ? 'Excellent clinical reasoning!'
    : `The correct diagnosis was: ${c.diagnosis}`;

  // Summary section
  const sum = document.getElementById('summarySection');
  sum.style.display = 'block';
  document.getElementById('dxName').textContent = c.diagnosis;
  document.getElementById('dxEp').textContent   = c.episode;
  document.getElementById('dxFacts').innerHTML  = '<strong>Key Facts:</strong> ' + c.keyFacts;

  // Auto-open summary on loss
  if (!won) openSummary();
}

/* ───────────── GAME LOGIC ───────────── */
function norm(s) { return s.toLowerCase().replace(/[^a-z0-9 ]/g,'').trim(); }

function isCorrect(g) {
  if (g === '[Skipped]') return false;
  const n = norm(g);
  const c = CASES[state.caseIndex];
  if (n === norm(c.diagnosis)) return true;
  return c.aliases.some(a => norm(a) === n);
}

function submitGuess() {
  if (state.status !== 'playing') return;
  const inp = document.getElementById('diagInput');
  const guess = inp.value.trim();
  if (!guess) { inp.style.animation='shake .4s ease'; setTimeout(()=>inp.style.animation='',400); return; }

  state.attempts.push(guess);
  const c = CASES[state.caseIndex];

  if (isCorrect(guess)) {
    state.status = 'won';
    updateStats(true, state.attempts.length);
  } else if (state.attempts.length >= 6) {
    state.status = 'lost';
    updateStats(false, state.attempts.length);
  } else {
    if (state.revealedClues < c.clues.length) state.revealedClues++;
  }

  inp.value = '';
  document.getElementById('suggestions').style.display = 'none';
  saveState();
  renderAll();
}

function skipAttempt() {
  if (state.status !== 'playing') return;
  const c = CASES[state.caseIndex];
  state.attempts.push('[Skipped]');
  if (state.attempts.length >= 6) {
    state.status = 'lost';
    updateStats(false, state.attempts.length);
  } else {
    if (state.revealedClues < c.clues.length) state.revealedClues++;
  }
  saveState();
  renderAll();
}

function prevCase() {
  if (state.caseIndex > 0) loadCase(state.caseIndex - 1);
}
function nextCase() {
  if (state.caseIndex < CASES.length - 1) loadCase(state.caseIndex + 1);
}
function openRestart() {
  document.getElementById('restartOverlay').classList.add('open');
}
function closeRestart() {
  document.getElementById('restartOverlay').classList.remove('open');
}
function restartCurrentCase() {
  closeRestart();
  const idx = state.caseIndex;
  localStorage.removeItem(`dip_case_${idx}`);
  state = { caseIndex: idx, revealedClues: 1, attempts: [], status: 'playing', sugIdx: -1 };
  summaryOpen = false;
  document.getElementById('summaryBody').classList.remove('open');
  document.getElementById('summaryChevron').textContent = '▾';
  renderAll();
  window.scrollTo({ top: 0, behavior: 'smooth' });
}
function restartAllCases() {
  closeRestart();
  // Remove all case saves
  for (let i = 0; i < CASES.length; i++) {
    localStorage.removeItem(`dip_case_${i}`);
  }
  // Also clear stats
  localStorage.removeItem('dip_stats');
  // Reload back to case 0
  state = { caseIndex: 0, revealedClues: 1, attempts: [], status: 'playing', sugIdx: -1 };
  summaryOpen = false;
  document.getElementById('summaryBody').classList.remove('open');
  document.getElementById('summaryChevron').textContent = '▾';
  renderAll();
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function loadCase(idx) {
  const key = `dip_case_${idx}`;
  try {
    const s = JSON.parse(localStorage.getItem(key));
    if (s) { state = s; }
    else throw '';
  } catch(e) {
    state = { caseIndex: idx, revealedClues: 1, attempts: [], status: 'playing', sugIdx: -1 };
  }
  summaryOpen = false;
  document.getElementById('summaryBody').classList.remove('open');
  document.getElementById('summaryChevron').textContent = '▾';
  renderAll();
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

/* ───────────── AUTOCOMPLETE ───────────── */
function setupInput() {
  const inp  = document.getElementById('diagInput');
  const sugs = document.getElementById('suggestions');
  const all  = CASES.map(c => c.diagnosis);

  inp.addEventListener('input', () => {
    const v = inp.value.trim().toLowerCase();
    if (v.length < 2) { sugs.style.display='none'; return; }
    const hits = all.filter(d => d.toLowerCase().includes(v)).slice(0, 7);
    if (!hits.length) { sugs.style.display='none'; return; }
    state.sugIdx = -1;
    sugs.innerHTML = hits.map(d => {
      const i = d.toLowerCase().indexOf(v);
      return `<div class="sug-item" onmousedown="pickSug('${d.replace(/'/g,"\\'")}')">🩺 ${d.slice(0,i)}<mark>${d.slice(i,i+v.length)}</mark>${d.slice(i+v.length)}</div>`;
    }).join('');
    sugs.style.display = 'block';
  });

  inp.addEventListener('blur', () => setTimeout(() => sugs.style.display='none', 150));
}

function setupKeyboard() {
  const inp  = document.getElementById('diagInput');
  const sugs = document.getElementById('suggestions');
  inp.addEventListener('keydown', e => {
    const items = sugs.querySelectorAll('.sug-item');
    if (e.key === 'ArrowDown') { state.sugIdx = Math.min(state.sugIdx+1, items.length-1); items.forEach((el,i)=>el.classList.toggle('sel',i===state.sugIdx)); e.preventDefault(); }
    else if (e.key === 'ArrowUp') { state.sugIdx = Math.max(state.sugIdx-1,-1); items.forEach((el,i)=>el.classList.toggle('sel',i===state.sugIdx)); e.preventDefault(); }
    else if (e.key === 'Enter') { if (state.sugIdx>=0 && items[state.sugIdx]) { inp.value=items[state.sugIdx].textContent.replace('🩺 ',''); sugs.style.display='none'; } else submitGuess(); }
    else if (e.key === 'Escape') sugs.style.display='none';
  });
}

function pickSug(dx) {
  document.getElementById('diagInput').value = dx;
  document.getElementById('suggestions').style.display = 'none';
  document.getElementById('diagInput').focus();
}

/* ───────────── ANKI TAGS ───────────── */
function copyAnki(step) {
  const c = CASES[state.caseIndex];
  const tag = step === 1 ? c.ankiStep1 : c.ankiStep2;
  navigator.clipboard.writeText(tag).then(() => showToast('Anki tag copied! ✓'));
}

/* ───────────── SUMMARY TOGGLE ───────────── */
function toggleSummary() {
  summaryOpen = !summaryOpen;
  document.getElementById('summaryBody').classList.toggle('open', summaryOpen);
  document.getElementById('summaryChevron').textContent = summaryOpen ? '▴' : '▾';
}
function openSummary() {
  summaryOpen = true;
  document.getElementById('summaryBody').classList.add('open');
  document.getElementById('summaryChevron').textContent = '▴';
}

/* ───────────── SHARE ───────────── */
function shareResult() {
  const c = CASES[state.caseIndex];
  const won = state.status === 'won';
  const grid = state.attempts.map(g => g==='[Skipped]' ? '⬜' : isCorrect(g) ? '🟩' : '🟥').join('');
  const txt = `🩺 DiFindNemo – Case #${state.caseIndex+1}\n${c.topic} | ${c.episode}\n${grid}\n${won ? state.attempts.length+'/6' : 'X/6'}`;
  navigator.clipboard.writeText(txt).then(() => showToast('Copied to clipboard!'));
}

/* ───────────── ARCHIVE ───────────── */
function openArchive() {
  const list = document.getElementById('archiveList');
  list.innerHTML = '';
  CASES.forEach((c, i) => {
    const done = !!localStorage.getItem(`dip_case_${i}`);
    const el = document.createElement('div');
    el.className = `arc-item ${done ? 'done' : ''}`;
    el.innerHTML = `
      <div class="arc-num-badge">#${i+1}</div>
      <div class="arc-info">
        <div class="arc-ep">${c.episode}</div>
        <div class="arc-clue">${c.clues[0]}</div>
      </div>
      <div class="arc-status ${done?'done':'new'}">${done?'✓':'▶'}</div>
    `;
    el.onclick = () => { loadCase(i); closeArchive(); };
    list.appendChild(el);
  });
  document.getElementById('archiveOverlay').classList.add('open');
}
function closeArchive() { document.getElementById('archiveOverlay').classList.remove('open'); }

/* ───────────── STATS ───────────── */
function openStats() {
  const s = stats;
  document.getElementById('sPlayed').textContent  = s.played;
  document.getElementById('sWon').textContent     = s.won;
  document.getElementById('sWinPct').textContent  = s.played ? Math.round(s.won/s.played*100)+'%' : '0%';
  document.getElementById('sStreak').textContent  = s.streak;

  const rows = document.getElementById('distRows');
  rows.innerHTML = '';
  const max = Math.max(...s.dist, 1);
  [1,2,3,4,5,6].forEach(n => {
    const v = s.dist[n-1]||0;
    const pct = Math.max(Math.round(v/max*100), 8);
    const isLast = s.lastWinAttempt === n;
    rows.innerHTML += `<div class="dist-row">
      <div class="dist-n">${n}</div>
      <div class="dist-track">
        <div class="dist-fill ${isLast?'highlight':''}" style="width:${pct}%">${v||''}</div>
      </div>
    </div>`;
  });
  document.getElementById('statsOverlay').classList.add('open');
}
function closeStats() { document.getElementById('statsOverlay').classList.remove('open'); }

/* ───────────── PERSIST ───────────── */
function saveState() {
  const today = new Date().toDateString();
  localStorage.setItem('dip_state_'+today, JSON.stringify(state));
  localStorage.setItem(`dip_case_${state.caseIndex}`, JSON.stringify(state));
}
function loadStats() {
  try { return JSON.parse(localStorage.getItem('dip_stats')) || defStats(); } catch(e) { return defStats(); }
}
function defStats() { return { played:0, won:0, streak:0, dist:[0,0,0,0,0,0], lastWinAttempt:null }; }
function updateStats(won, attempts) {
  stats.played++;
  if (won) { stats.won++; stats.streak++; stats.dist[attempts-1]=(stats.dist[attempts-1]||0)+1; stats.lastWinAttempt=attempts; }
  else      { stats.streak=0; stats.lastWinAttempt=null; }
  localStorage.setItem('dip_stats', JSON.stringify(stats));
}

/* ───────────── COOKIES ───────────── */
function acceptCookies()  { localStorage.setItem('dip_cookies','1'); document.getElementById('cookieBanner').style.display='none'; }
function dismissCookies() { document.getElementById('cookieBanner').style.display='none'; }

/* ───────────── TOAST ───────────── */
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg; t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2000);
}

/* ───────────── CLOSE OVERLAYS ON BACKDROP ───────────── */
document.addEventListener('click', e => {
  if (e.target.id === 'archiveOverlay')  closeArchive();
  if (e.target.id === 'statsOverlay')    closeStats();
  if (e.target.id === 'restartOverlay')  closeRestart();
});

/* ───────────── BOOT ───────────── */
document.addEventListener('DOMContentLoaded', init);
</script>

</body>
</html>
