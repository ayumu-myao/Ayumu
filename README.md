[track-calc.html](https://github.com/user-attachments/files/27750033/track-calc.html)
# Ayumu<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<title>Track Calc</title>
<link rel="manifest" href="./manifest.json">
<meta name="theme-color" content="#0a0a10">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="TrackCalc">
<link rel="apple-touch-icon" href="./icon-192.png">
<style>
:root {
  --bg:#0a0a10; --surface:#12121b; --card:#181824; --card2:#1e1e2c;
  --accent:#f5c400; --accent2:#ff6b35; --accent3:#4ab4ff;
  --green:#3ddc84; --text:#e8e8f0; --muted:#6b6b88; --border:#242434; --red:#ff5555;
}
*{box-sizing:border-box;margin:0;padding:0;}
body{background:var(--bg);color:var(--text);font-family:-apple-system,BlinkMacSystemFont,'Helvetica Neue',sans-serif;min-height:100vh;padding:16px;}

/* topbar */
.topbar{max-width:980px;margin:0 auto 14px;display:flex;align-items:center;gap:8px;flex-wrap:wrap;}
.app-title{font-size:1.05rem;font-weight:800;color:var(--accent);letter-spacing:.12em;text-transform:uppercase;flex-shrink:0;}
.venue-chip{display:flex;align-items:center;gap:6px;background:var(--card);border:1px solid var(--border);border-radius:20px;padding:5px 12px;flex:1;min-width:120px;}
.venue-chip .dot{width:6px;height:6px;border-radius:50%;background:var(--muted);flex-shrink:0;}
.venue-chip .dot.active{background:var(--green);}
.venue-name{font-size:.8rem;color:var(--text);flex:1;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.btn{padding:7px 13px;border-radius:8px;border:1.5px solid var(--border);background:transparent;color:var(--muted);font-size:.78rem;font-weight:600;cursor:pointer;font-family:inherit;transition:all .15s;white-space:nowrap;}
.btn:hover{border-color:var(--accent3);color:var(--accent3);}
.btn.primary{border-color:var(--accent);color:var(--accent);background:rgba(245,196,0,.06);}
.btn.primary:hover{background:rgba(245,196,0,.14);}
.btn.lang{border-color:var(--accent3);color:var(--accent3);font-weight:800;letter-spacing:.05em;}

/* track bar */
.track-bar{max-width:980px;margin:0 auto 12px;display:flex;align-items:center;gap:8px;}
.track-bar label{font-size:.68rem;color:var(--muted);text-transform:uppercase;letter-spacing:.08em;}
.seg{display:flex;gap:4px;}
.seg button{padding:7px 14px;border:1.5px solid var(--border);background:transparent;color:var(--muted);border-radius:8px;cursor:pointer;font-size:.86rem;font-weight:700;font-family:inherit;transition:all .15s;}
.seg button.active{border-color:var(--accent);color:var(--accent);background:rgba(245,196,0,.08);}

/* layout */
.layout{max-width:980px;margin:0 auto;display:grid;grid-template-columns:1fr 1fr;gap:10px;}
@media(max-width:640px){.layout{grid-template-columns:1fr;}}
.col{display:flex;flex-direction:column;gap:10px;}
.card{background:var(--card);border-radius:14px;padding:16px 18px;border:1px solid var(--border);}
.card-label{font-size:.62rem;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:.1em;margin-bottom:14px;}

/* sliders */
.s-row{margin-bottom:16px;}
.s-row:last-child{margin-bottom:0;}
.s-head{display:flex;justify-content:space-between;align-items:baseline;margin-bottom:7px;}
.s-name{font-size:.76rem;color:var(--muted);}
.s-val{font-size:1rem;font-weight:700;color:var(--accent);font-variant-numeric:tabular-nums;}
.s-unit{font-size:.62rem;color:var(--muted);margin-left:2px;}
input[type=range]{-webkit-appearance:none;width:100%;height:3px;border-radius:2px;background:var(--border);outline:none;cursor:pointer;}
input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:16px;height:16px;border-radius:50%;background:var(--accent);cursor:pointer;box-shadow:0 0 0 3px rgba(245,196,0,.15);}
input[type=range]:active::-webkit-slider-thumb{box-shadow:0 0 0 6px rgba(245,196,0,.25);}
.tire-select{width:100%;background:var(--card2);color:var(--text);border:1px solid var(--border);border-radius:8px;padding:10px 12px;font:inherit;font-size:.9rem;cursor:pointer;-webkit-appearance:none;appearance:none;background-image:url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'><path fill='%23f5c500' d='M0 0l5 6 5-6z'/></svg>");background-repeat:no-repeat;background-position:right 12px center;padding-right:32px;}

/* gear chip */
.gear-chip{text-align:center;background:var(--surface);border-radius:10px;padding:10px;margin-top:12px;}
.gear-big{font-size:1.8rem;font-weight:800;color:var(--accent2);letter-spacing:.04em;font-variant-numeric:tabular-nums;}
.gear-sub{font-size:.66rem;color:var(--muted);margin-top:2px;}

/* first lap input */
.lap-input-wrap{display:flex;align-items:center;gap:8px;background:var(--surface);border:1.5px solid var(--border);border-radius:10px;padding:10px 14px;transition:border-color .15s;}
.lap-input-wrap:focus-within{border-color:var(--accent);}
.lap-input-wrap input{flex:1;background:transparent;border:none;outline:none;font-size:1.6rem;font-weight:800;color:var(--accent);font-variant-numeric:tabular-nums;font-family:inherit;width:100%;text-align:right;}
.lap-input-wrap .unit{font-size:.75rem;color:var(--muted);flex-shrink:0;}

/* weather */
.weather-btns{display:flex;gap:5px;margin-top:8px;}
.weather-btns button{flex:1;padding:7px 4px;border:1.5px solid var(--border);background:transparent;border-radius:8px;cursor:pointer;font-size:1rem;transition:all .15s;}
.weather-btns button.active{border-color:var(--accent);background:rgba(245,196,0,.08);}

/* speed stats */
.speed-block{text-align:center;padding:12px 0 14px;border-bottom:1px solid var(--border);margin-bottom:12px;}
.speed-num{font-size:3.8rem;font-weight:800;color:var(--accent);line-height:1;font-variant-numeric:tabular-nums;letter-spacing:-.02em;}
.speed-unit{font-size:.72rem;color:var(--muted);margin-top:4px;text-transform:uppercase;letter-spacing:.1em;}
.stats{display:grid;grid-template-columns:repeat(3,1fr);gap:7px;margin-bottom:10px;}
.stat{background:var(--surface);border-radius:10px;padding:9px 11px;}
.stat-l{font-size:.56rem;color:var(--muted);text-transform:uppercase;letter-spacing:.05em;margin-bottom:3px;}
.stat-v{font-size:1.05rem;font-weight:700;color:var(--text);font-variant-numeric:tabular-nums;}
.stat-s{font-size:.58rem;color:var(--muted);}
.badges{display:flex;flex-wrap:wrap;gap:5px;}
.badge{font-size:.62rem;padding:3px 8px;border-radius:20px;font-weight:600;}
.badge.ok{background:rgba(61,220,132,.1);color:var(--green);}
.badge.warn{background:rgba(255,107,53,.1);color:var(--accent2);}
.badge.info{background:rgba(74,180,255,.1);color:var(--accent3);}

/* event tabs */
.ev-tabs{display:flex;gap:4px;margin-bottom:14px;border-bottom:1px solid var(--border);padding-bottom:10px;}
.ev-tab{flex:1;padding:7px 4px;border:1.5px solid var(--border);background:transparent;color:var(--muted);border-radius:8px;cursor:pointer;font-size:.8rem;font-weight:700;font-family:inherit;transition:all .15s;}
.ev-tab.active{border-color:var(--accent);color:var(--accent);background:rgba(245,196,0,.08);}
.ev-tab:hover:not(.active){border-color:var(--muted);color:var(--text);}

/* event detail */
.ev-header{display:flex;align-items:baseline;gap:12px;margin-bottom:10px;}
.ev-total-time{font-size:2.8rem;font-weight:800;color:var(--text);font-variant-numeric:tabular-nums;line-height:1;background:transparent;border:none;border-bottom:2px solid transparent;outline:none;font-family:inherit;width:100%;padding:0;transition:border-color .15s;cursor:text;}
.ev-total-time:focus{border-bottom-color:var(--accent);}
.ev-total-time::placeholder{color:var(--muted);}
.ev-meta-col{display:flex;flex-direction:column;gap:3px;}
.ev-meta-row{font-size:.7rem;color:var(--muted);}
.ev-meta-row span{color:var(--text);font-weight:600;}

/* event summary */
.ev-summary{display:flex;flex-direction:column;gap:4px;margin-bottom:14px;}
.ev-summary-row{display:flex;justify-content:space-between;align-items:center;padding:5px 10px;background:var(--surface);border-radius:7px;border-left:2px solid transparent;cursor:pointer;transition:background .1s;}
.ev-summary-row:hover{background:var(--card2);}
.ev-summary-row.active{border-left-color:var(--accent);}
.ev-summary-row.fly-ev{border-left-color:var(--accent2);}
.ev-sum-name{font-size:.72rem;color:var(--muted);}
.ev-sum-laps{font-size:.65rem;color:var(--muted);}
.ev-sum-time{font-size:.9rem;font-weight:700;color:var(--text);font-variant-numeric:tabular-nums;}

/* split table */
.split-wrap{overflow-x:auto;}
.split-table{width:100%;border-collapse:collapse;}
.split-table th{font-size:.57rem;color:var(--muted);text-transform:uppercase;letter-spacing:.06em;text-align:left;padding:4px 8px;border-bottom:1px solid var(--border);}
.split-table th:not(:first-child){text-align:right;}
.split-table td{font-size:.8rem;padding:6px 8px;border-bottom:1px solid var(--border);font-variant-numeric:tabular-nums;}
.split-table td:not(:first-child){text-align:right;}
.split-table tr:last-child td{border-bottom:none;}
.split-table tr.lap-start td{color:var(--accent);}
.split-table tr.half-row td{color:var(--muted);font-size:.74rem;}
.split-table td.elapsed{color:var(--accent3);}
.split-table td.lap-num{font-weight:700;color:var(--text);}
.start-marker{font-size:.6rem;color:var(--accent2);margin-left:4px;}

/* history */
.history-section{max-width:980px;margin:12px auto 0;}
.history-toggle{width:100%;display:flex;align-items:center;justify-content:space-between;padding:12px 18px;background:var(--card);border:1px solid var(--border);border-radius:12px;cursor:pointer;font-family:inherit;transition:border-color .15s;}
.history-toggle:hover{border-color:var(--accent3);}
.history-toggle span{font-size:.7rem;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:.1em;}
.history-toggle .h-count{font-size:.7rem;color:var(--accent3);}
.history-toggle .h-arrow{font-size:.7rem;color:var(--muted);transition:transform .2s;}
.history-toggle.open .h-arrow{transform:rotate(180deg);}
.history-body{display:none;background:var(--card);border:1px solid var(--border);border-top:none;border-radius:0 0 12px 12px;overflow:hidden;}
.history-body.open{display:block;}
.h-empty{padding:20px;text-align:center;font-size:.8rem;color:var(--muted);}
.h-row{display:grid;grid-template-columns:auto 1fr auto auto;gap:12px;align-items:center;padding:10px 16px;border-bottom:1px solid var(--border);cursor:pointer;transition:background .12s;}
.h-row:last-child{border-bottom:none;}
.h-row:hover{background:var(--card2);}
.h-date{font-size:.68rem;color:var(--muted);white-space:nowrap;}
.h-info{display:flex;flex-direction:column;gap:2px;overflow:hidden;}
.h-venue{font-size:.8rem;color:var(--text);white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.h-detail{font-size:.65rem;color:var(--muted);}
.h-time{font-size:.85rem;font-weight:700;color:var(--accent);font-variant-numeric:tabular-nums;white-space:nowrap;}
.h-del{background:transparent;border:none;color:var(--muted);cursor:pointer;font-size:.75rem;padding:4px;border-radius:6px;transition:color .1s;}
.h-del:hover{color:var(--red);}
</style>
</head>
<body>

<!-- topbar -->
<div class="topbar">
  <div class="app-title">Track Calc</div>
  <div class="venue-chip">
    <div class="dot" id="locDot"></div>
    <div class="venue-name" id="venueName">-</div>
  </div>
  <button class="btn" id="locBtn" onclick="getLocation()">-</button>
  <button class="btn primary" id="saveBtn" onclick="saveRecord()">-</button>
  <button class="btn lang" id="langBtn" onclick="toggleLang()">EN</button>
</div>

<!-- track bar -->
<div class="track-bar">
  <label id="trackLabel">-</label>
  <div class="seg">
    <button class="active" onclick="setTrack(this,250)">250 m</button>
    <button onclick="setTrack(this,333)">333 m</button>
    <button onclick="setTrack(this,400)">400 m</button>
    <button onclick="setTrack(this,500)">500 m</button>
  </div>
</div>

<!-- main layout -->
<div class="layout">
  <!-- LEFT -->
  <div class="col">
    <div class="card">
      <div class="card-label" data-i18n="gear.title"></div>
      <div class="s-row">
        <div class="s-head">
          <span class="s-name" data-i18n="gear.front"></span>
          <span><span class="s-val" id="vFront">49</span><span class="s-unit">T</span></span>
        </div>
        <input type="range" id="front" min="45" max="70" value="49" step="1" oninput="calc()">
      </div>
      <div class="s-row">
        <div class="s-head">
          <span class="s-name" data-i18n="gear.rear"></span>
          <span><span class="s-val" id="vRear">14</span><span class="s-unit">T</span></span>
        </div>
        <input type="range" id="rear" min="10" max="20" value="14" step="1" oninput="calc()">
      </div>
      <div class="s-row">
        <div class="s-head">
          <span class="s-name" data-i18n="tire.circ"></span>
          <span><span class="s-val" id="vTireC">2096</span><span class="s-unit">mm</span></span>
        </div>
        <select id="tireC" onchange="calc()" class="tire-select">
          <option value="2080">700×19C (2080mm)</option>
          <option value="2086">700×20C (2086mm)</option>
          <option value="2096" selected>700×23C (2096mm)</option>
          <option value="2105">700×25C (2105mm)</option>
          <option value="2136">700×28C (2136mm)</option>
          <option value="2155">700×32C (2155mm)</option>
          <option value="2174">700×35C (2174mm)</option>
        </select>
      </div>
      <div class="s-row">
        <div class="s-head">
          <span class="s-name" data-i18n="tire.width"></span>
          <span><span class="s-val" id="vTireW">23</span><span class="s-unit">mm</span></span>
        </div>
        <input type="range" id="tireW" min="16" max="45" value="23" step="1" oninput="calc()">
      </div>
      <div class="gear-chip">
        <div class="gear-big" id="gearLabel">49 × 14</div>
        <div class="gear-sub" id="gearSub"></div>
      </div>
    </div>

    <div class="card">
      <div class="card-label" data-i18n="perf.title"></div>
      <div class="s-row">
        <div class="s-head">
          <span class="s-name" data-i18n="perf.cadence"></span>
          <span><span class="s-val" id="vCad">105</span><span class="s-unit">rpm</span></span>
        </div>
        <input type="range" id="cad" min="80" max="145" value="105" step="1" oninput="calc()">
      </div>
      <div class="s-row" style="margin-bottom:0;">
        <div class="s-head" style="margin-bottom:8px;">
          <span class="s-name" data-i18n="perf.firstLap"></span>
          <span class="s-unit" data-i18n="unit.sec"></span>
        </div>
        <div class="lap-input-wrap">
          <input type="number" id="firstLap" min="10" max="120" step="0.001" value="22.500" oninput="calc()">
          <span class="unit" data-i18n="unit.sec"></span>
        </div>
      </div>
    </div>

    <div class="card">
      <div class="card-label" data-i18n="env.title"></div>
      <div class="s-row">
        <div class="s-head">
          <span class="s-name" data-i18n="env.altitude"></span>
          <span><span class="s-val" id="vAlt">0</span><span class="s-unit">m</span></span>
        </div>
        <input type="range" id="alt" min="0" max="3000" value="0" step="10" oninput="calc()">
      </div>
      <div class="s-row">
        <div class="s-head">
          <span class="s-name" data-i18n="env.temp"></span>
          <span><span class="s-val" id="vTemp">20</span><span class="s-unit">°C</span></span>
        </div>
        <input type="range" id="temp" min="-5" max="45" value="20" step="1" oninput="calc()">
      </div>
    </div>
  </div>

  <!-- RIGHT -->
  <div class="col">
    <div class="card">
      <div class="card-label" data-i18n="stats.speedTitle"></div>
      <div class="speed-block">
        <div class="speed-num" id="speedNum">--.-</div>
        <div class="speed-unit">km / h</div>
      </div>
      <div class="stats">
        <div class="stat"><div class="stat-l" data-i18n="stats.1st"></div><div class="stat-v" id="st1st">--</div><div class="stat-s" data-i18n="unit.sec"></div></div>
        <div class="stat"><div class="stat-l" data-i18n="stats.fly"></div><div class="stat-v" id="stFly">--</div><div class="stat-s" data-i18n="unit.secLap"></div></div>
        <div class="stat"><div class="stat-l" data-i18n="stats.half"></div><div class="stat-v" id="stHalf">--</div><div class="stat-s" data-i18n="unit.secHalf"></div></div>
        <div class="stat"><div class="stat-l" data-i18n="stats.rho"></div><div class="stat-v" id="stRho">--</div><div class="stat-s" data-i18n="stats.rhoSub"></div></div>
        <div class="stat"><div class="stat-l" data-i18n="stats.corr"></div><div class="stat-v" id="stCorr">--</div><div class="stat-s" data-i18n="stats.corrSub"></div></div>
        <div class="stat"><div class="stat-l" data-i18n="stats.1km"></div><div class="stat-v" id="st1km">--</div><div class="stat-s" data-i18n="unit.sec"></div></div>
      </div>
      <div class="badges" id="badges"></div>
    </div>

    <div class="card">
      <div class="card-label" data-i18n="ev.title"></div>
      <div class="ev-tabs">
        <button class="ev-tab"        data-ev="200"  onclick="setEvent(this)">200m</button>
        <button class="ev-tab"        data-ev="500"  onclick="setEvent(this)">500m</button>
        <button class="ev-tab active" data-ev="1000" onclick="setEvent(this)">1km</button>
        <button class="ev-tab"        data-ev="3000" onclick="setEvent(this)">3km</button>
        <button class="ev-tab"        data-ev="4000" onclick="setEvent(this)">4km</button>
      </div>
      <div class="ev-summary" id="evSummary"></div>
      <div id="evDetail">
        <div class="ev-header">
          <div id="evTotalTime" class="ev-total-time">--</div>
          <div class="ev-meta-col" id="evMetaCol"></div>
        </div>
        <div class="split-wrap">
          <table class="split-table">
            <thead>
              <tr>
                <th data-i18n="split.lap"></th>
                <th data-i18n="split.half"></th>
                <th data-i18n="split.dist"></th>
                <th data-i18n="split.split"></th>
                <th data-i18n="split.elapsed"></th>
              </tr>
            </thead>
            <tbody id="splitBody"></tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- history -->
<div class="history-section">
  <button class="history-toggle" id="histToggle" onclick="toggleHistory()">
    <span data-i18n="hist.title"></span>
    <div style="display:flex;align-items:center;gap:8px;">
      <span class="h-count" id="histCount">0</span>
      <span class="h-arrow">▼</span>
    </div>
  </button>
  <div class="history-body" id="histBody">
    <div class="h-empty" id="hEmpty" data-i18n="hist.empty"></div>
    <div id="hList"></div>
  </div>
</div>

<script>
// ── audio ─────────────────────────────────────────────────────
let _actx = null;
const _prevVals = {};

function _getCtx() {
  if (!_actx) _actx = new (window.AudioContext || window.webkitAudioContext)();
  if (_actx.state === 'suspended') _actx.resume();
  return _actx;
}

function playClick() {
  const ctx = _getCtx();
  const now = ctx.currentTime;

  // bike freewheel ratchet click — pawl hitting tooth
  // pure metallic tick, no low body, very dry & sharp
  const dur = 0.004;
  const bufLen = Math.floor(ctx.sampleRate * dur);
  const buf = ctx.createBuffer(1, bufLen, ctx.sampleRate);
  const d = buf.getChannelData(0);
  // instant attack, fast exponential decay baked in
  for (let i = 0; i < bufLen; i++) {
    d[i] = (Math.random() * 2 - 1) * Math.exp(-i / (ctx.sampleRate * 0.0007));
  }
  const ns = ctx.createBufferSource(); ns.buffer = buf;

  // metallic resonant peak — pawl/tooth strike
  const bp = ctx.createBiquadFilter();
  bp.type = 'bandpass'; bp.frequency.value = 3800; bp.Q.value = 9;

  // remove any low body — pure tick
  const hp = ctx.createBiquadFilter();
  hp.type = 'highpass'; hp.frequency.value = 2200;

  const g = ctx.createGain();
  g.gain.setValueAtTime(1.0, now);
  g.gain.exponentialRampToValueAtTime(0.0001, now + dur);

  ns.connect(hp); hp.connect(bp); bp.connect(g); g.connect(ctx.destination);
  ns.start(now); ns.stop(now + dur + 0.001);

  // short crisp tap haptic
  if (navigator.vibrate) navigator.vibrate(3);
}

// fire on every integer step change across all range inputs
document.addEventListener('input', e => {
  if (e.target.type !== 'range') return;
  const id  = e.target.id;
  const val = +e.target.value;
  if (_prevVals[id] !== val) { _prevVals[id] = val; playClick(); }
});

// ── i18n ─────────────────────────────────────────────────────
const I18N = {
  ja: {
    'gear.title':'ギア設定', 'gear.front':'フロント', 'gear.rear':'リア',
    'gear.ratio':'ギア比', 'gear.dev':'開発距離',
    'tire.circ':'タイヤ周長', 'tire.width':'タイヤ太さ',
    'perf.title':'パフォーマンス', 'perf.cadence':'ケイデンス（フライングラップ）',
    'perf.firstLap':'1周目タイム（手動入力）',
    'env.title':'環境条件', 'env.altitude':'標高', 'env.temp':'気温',
    'env.pressure':'気圧（推定）',
    'stats.speedTitle':'フライング速度',
    'stats.1st':'1周目', 'stats.fly':'フライング', 'stats.half':'半周',
    'stats.rho':'空気密度比', 'stats.rhoSub':'vs 標準',
    'stats.corr':'速度補正', 'stats.corrSub':'空気効果',
    'stats.1km':'1km換算',
    'unit.sec':'秒', 'unit.secLap':'秒/周', 'unit.secHalf':'秒/半周',
    'ev.title':'種目別タイム', 'ev.fly':'⚡ フライングスタート', 'ev.stand':'🚦 スタンディングスタート',
    'ev.laps':'周', 'ev.halves':'本',
    'split.lap':'周', 'split.half':'ハーフ', 'split.dist':'距離',
    'split.split':'スプリット', 'split.elapsed':'累積',
    'hist.title':'履歴', 'hist.empty':'まだ記録がありません',
    'topbar.venue':'バンク未設定', 'topbar.locate':'📍 位置取得', 'topbar.save':'記録する',
    'track.label':'周長',
    'badge.altitude':'高地', 'badge.highP':'高気圧',
    'badge.tail':'追い風', 'badge.head':'向かい風', 'badge.wet':'路面ウェット',
    'ev.200':'200m', 'ev.500':'500m', 'ev.1000':'1km', 'ev.3000':'3km', 'ev.4000':'4km',
    'weather.sunny':'晴れ','weather.cloudy':'曇り','weather.rain':'雨','weather.wind':'強風',
    'rev.title':'逆算 — 目標タイムから必要ケイデンスを求める',
    'rev.event':'種目', 'rev.targetTime':'目標タイム', 'rev.timeHint':'例: 1:08.000',
    'rev.reqCad':'必要ケイデンス', 'rev.reqSpeed':'必要速度',
    'rev.reqFlyLap':'フライングラップ', 'rev.reqHalf':'半周',
    'rev.feasibility':'実現性', 'rev.feasOk':'達成可能', 'rev.feasHard':'高負荷', 'rev.feasOver':'過負荷',
  },
  en: {
    'gear.title':'Gear', 'gear.front':'Front', 'gear.rear':'Rear',
    'gear.ratio':'Gear ratio', 'gear.dev':'Development',
    'tire.circ':'Tire circ.', 'tire.width':'Tire width',
    'perf.title':'Performance', 'perf.cadence':'Cadence (flying lap)',
    'perf.firstLap':'1st lap time (manual)',
    'env.title':'Conditions', 'env.altitude':'Altitude', 'env.temp':'Temp',
    'env.pressure':'Pressure (est.)',
    'stats.speedTitle':'Flying Speed',
    'stats.1st':'1st Lap', 'stats.fly':'Flying', 'stats.half':'Half Lap',
    'stats.rho':'Air Density', 'stats.rhoSub':'vs std',
    'stats.corr':'Speed Corr.', 'stats.corrSub':'air effect',
    'stats.1km':'1km equiv.',
    'unit.sec':'sec', 'unit.secLap':'s/lap', 'unit.secHalf':'s/half',
    'ev.title':'Event Times', 'ev.fly':'⚡ Flying start', 'ev.stand':'🚦 Standing start',
    'ev.laps':'laps', 'ev.halves':'halves',
    'split.lap':'Lap', 'split.half':'Half', 'split.dist':'Dist',
    'split.split':'Split', 'split.elapsed':'Elapsed',
    'hist.title':'History', 'hist.empty':'No records yet',
    'topbar.venue':'No venue', 'topbar.locate':'📍 Location', 'topbar.save':'Save',
    'track.label':'Track',
    'badge.altitude':'Altitude', 'badge.highP':'High P',
    'badge.tail':'Tailwind', 'badge.head':'Headwind', 'badge.wet':'Wet surface',
    'ev.200':'200m', 'ev.500':'500m', 'ev.1000':'1km', 'ev.3000':'3km', 'ev.4000':'4km',
    'weather.sunny':'Clear','weather.cloudy':'Cloudy','weather.rain':'Rain','weather.wind':'Windy',
    'rev.title':'Reverse — Required Cadence from Target Time',
    'rev.event':'Event', 'rev.targetTime':'Target Time', 'rev.timeHint':'e.g. 1:08.000',
    'rev.reqCad':'Required Cadence', 'rev.reqSpeed':'Required Speed',
    'rev.reqFlyLap':'Flying Lap', 'rev.reqHalf':'Half Lap',
    'rev.feasibility':'Feasibility', 'rev.feasOk':'Achievable', 'rev.feasHard':'High Load', 'rev.feasOver':'Overload',
  },
};

let lang = 'ja';

function t(key) { return I18N[lang]?.[key] ?? I18N.ja[key] ?? key; }

function applyLang() {
  document.documentElement.lang = lang === 'ja' ? 'ja' : 'en';
  document.querySelectorAll('[data-i18n]').forEach(el => { el.textContent = t(el.dataset.i18n); });
  $('venueName').textContent = venueName || t('topbar.venue');
  $('locBtn').textContent    = t('topbar.locate');
  $('saveBtn').textContent   = t('topbar.save');
  $('trackLabel').textContent = t('track.label');
  $('langBtn').textContent   = lang === 'ja' ? 'EN' : 'JP';
  if (lastCalc) {
    renderAllEvents(lastCalc.firstT, lastCalc.flyLap, lastCalc.vF);
    renderEventDetail(lastCalc);
    updateGearSub(lastCalc.gr, lastCalc.dev);
  }
  calcReverse();
  renderHistory();
}

function toggleLang() {
  lang = lang === 'ja' ? 'en' : 'ja';
  applyLang();
}

// ── constants ─────────────────────────────────────────────────
const P0 = 1013.25, T0 = 288.15;

const WEATHER_KEYS = ['sunny','cloudy','rain','wind'];
const WEATHER_PEN  = { sunny:0, cloudy:0, rain:0.015, wind:0 };

const EVENTS = [
  { key:'200',  nameKey:'ev.200',  dist:200,   fly:true,  color:'var(--accent2)' },
  { key:'500',  nameKey:'ev.500',  dist:500,   fly:false, color:'var(--accent3)' },
  { key:'1000', nameKey:'ev.1000', dist:1000,  fly:false, color:'var(--accent)'  },
  { key:'3000', nameKey:'ev.3000', dist:3000,  fly:false, color:'var(--green)'   },
  { key:'4000', nameKey:'ev.4000', dist:4000,  fly:false, color:'var(--accent)'  },
];

// ── state ─────────────────────────────────────────────────────
let trackLen    = 250;
let weatherKey  = 'sunny';
let activeEvKey = '1000';
let activeRevKey = '1000';
let venueName   = null;
let lastCalc    = null;

// ── utils ─────────────────────────────────────────────────────
const $ = id => document.getElementById(id);

function fmt(s) {
  if (!isFinite(s) || s <= 0) return '--';
  if (s < 60) return s.toFixed(3) + '"';
  const m = Math.floor(s / 60);
  return `${m}'${(s % 60).toFixed(3).padStart(6,'0')}"`;
}

function syncRange(el) {
  const p = ((+el.value - +el.min) / (+el.max - +el.min) * 100).toFixed(1) + '%';
  el.style.background = `linear-gradient(to right,var(--accent) 0%,var(--accent) ${p},var(--border) ${p})`;
}

// ── controls ──────────────────────────────────────────────────
function setTrack(btn, len) {
  document.querySelectorAll('.seg button').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  trackLen = len;
  calc();
}

function setWeather(btn) {
  document.querySelectorAll('.weather-btns button').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  weatherKey = btn.dataset.w;
  if (weatherKey === 'wind') $('wind').value = 15;
  calc();
}

function setEvent(btn) {
  document.querySelectorAll('.ev-tab').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  activeEvKey = btn.dataset.ev;
  renderEventDetail(lastCalc);
}

// ── core calc ─────────────────────────────────────────────────
function calc() {
  const front  = +$('front').value;
  const rear   = +$('rear').value;
  const cad    = +$('cad').value;
  const firstT = parseFloat($('firstLap').value) || 22.5;
  const alt    = +$('alt').value;
  const temp   = +$('temp').value;
  const tireC  = +$('tireC').value;
  const tireW  = +$('tireW').value;
  const WHEEL  = tireC / 1000;

  // 標準大気圧公式: P(h) = P0 × (1 - 2.25577e-5 × h)^5.25588
  const pres = P0 * Math.pow(1 - 2.25577e-5 * alt, 5.25588);

  $('vFront').textContent = front;
  $('vRear').textContent  = rear;
  $('vCad').textContent   = cad;
  $('vAlt').textContent   = alt;
  $('vTemp').textContent  = temp;
  $('vTireC').textContent = tireC;
  $('vTireW').textContent = tireW;
  if ($('vPres')) $('vPres').textContent = pres.toFixed(0);

  document.querySelectorAll('input[type=range]').forEach(syncRange);

  const gr  = front / rear;
  const dev = gr * WHEEL;
  $('gearLabel').textContent = `${front} × ${rear}`;
  updateGearSub(gr, dev);

  const v0   = (cad / 60) * gr * WHEEL;
  const rhoR = (pres / P0) * (T0 / (temp + 273.15));
  const airF = Math.pow(rhoR, -1/3);
  const vF   = v0 * airF;
  const vKmh = vF * 3.6;

  const flyLap  = trackLen / vF;
  const halfLap = (trackLen / 2) / vF;

  $('speedNum').textContent = vKmh.toFixed(1);
  $('st1st').textContent    = firstT.toFixed(3);
  $('stFly').textContent    = flyLap.toFixed(3);
  $('stHalf').textContent   = halfLap.toFixed(3);
  $('stRho').textContent    = rhoR.toFixed(4);
  $('stCorr').textContent   = (airF >= 1 ? '+' : '') + ((airF-1)*100).toFixed(2) + '%';
  $('st1km').textContent    = (1000/vF).toFixed(3);

  const bEl = $('badges');
  bEl.innerHTML = '';
  if (alt >= 500) badge(bEl, `⛰ ${alt}m −${((1-rhoR)*100).toFixed(1)}%`, 'ok');

  lastCalc = { front, rear, cad, firstT, alt, temp, tireC, tireW, WHEEL, gr, dev, vKmh, vF, flyLap, halfLap };

  renderAllEvents(firstT, flyLap, vF);
  renderEventDetail(lastCalc);
  calcReverse();
}

function updateGearSub(gr, dev) {
  $('gearSub').textContent = `${t('gear.ratio')} ${gr.toFixed(3)}　${t('gear.dev')} ${dev.toFixed(2)} m/rev`;
}


// ── reverse calc ───────────────────────────────────────────────
function setRevEvent(btn) {
  document.querySelectorAll('#revTabs .ev-tab').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  activeRevKey = btn.dataset.ev;
  calcReverse();
}

function parseTargetTime(s) {
  s = s.trim();
  // mm:ss.sss or ss.sss
  const mMatch = s.match(/^(\d+):(\d+(?:\.\d*)?)$/);
  if (mMatch) return parseInt(mMatch[1]) * 60 + parseFloat(mMatch[2]);
  const sMatch = s.match(/^(\d+(?:\.\d*)?)["″]?$/);
  if (sMatch) return parseFloat(sMatch[1]);
  return NaN;
}

function calcReverse() {
  const ev = EVENTS.find(e => e.key === activeRevKey);
  if (!ev || !lastCalc) return;
  if (!$('revTarget')) return;

  const targetT = parseTargetTime($('revTarget').value);
  if (!isFinite(targetT) || targetT <= 0) {
    ['revCad','revSpeed','revFlyLap','revHalf','revFeas'].forEach(id => $(id).textContent = '--');
    return;
  }

  const { front, rear, alt, temp, firstT, WHEEL } = lastCalc;
  const gr = front / rear;

  // air correction factor (same as forward calc)
  const pres = P0 * Math.pow(1 - 2.25577e-5 * alt, 5.25588);
  const rhoR = (pres / P0) * (T0 / (temp + 273.15));
  const airF = Math.pow(rhoR, -1/3);

  // required flying lap time from target total time
  let reqFlyLap;
  if (ev.fly) {
    reqFlyLap = targetT * trackLen / ev.dist;
  } else {
    const nLaps = ev.dist / trackLen;
    if (nLaps <= 1) {
      reqFlyLap = targetT; // single lap
    } else {
      reqFlyLap = (targetT - firstT) / (nLaps - 1);
    }
  }

  if (reqFlyLap <= 0) {
    $('revCad').textContent = '無理';
    return;
  }

  // required speed
  const reqVf  = trackLen / reqFlyLap;
  // back-correct through air resistance to get base v0
  const reqV0  = reqVf / airF;
  // required cadence
  const reqCad = reqV0 * 60 / (gr * WHEEL);
  const reqKmh = reqVf * 3.6;
  const reqHalf = (trackLen / 2) / reqVf;

  $('revCad').textContent    = reqCad.toFixed(1);
  $('revSpeed').textContent  = reqKmh.toFixed(1);
  $('revFlyLap').textContent = reqFlyLap.toFixed(3);
  $('revHalf').textContent   = reqHalf.toFixed(3);

  // feasibility (rough guide based on cadence range)
  const feasEl = $('revFeas');
  const statEl = $('revFeasStat');
  if (reqCad < 80) {
    feasEl.textContent = '← ギア軽すぎ';
    feasEl.style.color = 'var(--accent3)';
  } else if (reqCad <= 110) {
    feasEl.textContent = t('rev.feasOk') + ' ✓';
    feasEl.style.color = 'var(--green)';
  } else if (reqCad <= 130) {
    feasEl.textContent = t('rev.feasHard') + ' ⚠';
    feasEl.style.color = 'var(--accent)';
  } else {
    feasEl.textContent = t('rev.feasOver') + ' ✕';
    feasEl.style.color = 'var(--red)';
  }
}

function badge(parent, txt, cls) {
  const b = document.createElement('span');
  b.className = `badge ${cls}`;
  b.textContent = txt;
  parent.appendChild(b);
}

// ── event rendering ────────────────────────────────────────────
function evTime(ev, firstT, flyLap, vF) {
  if (ev.fly) return ev.dist / vF;
  return firstT + (ev.dist / trackLen - 1) * flyLap;
}

function lapCountStr(dist) {
  const n = dist / trackLen;
  return (Number.isInteger(n) ? n : n.toFixed(1)) + ' ' + t('ev.laps');
}

function renderAllEvents(firstT, flyLap, vF) {
  const sum = $('evSummary');
  sum.innerHTML = '';
  EVENTS.forEach(ev => {
    const time = evTime(ev, firstT, flyLap, vF);
    const row  = document.createElement('div');
    row.className = 'ev-summary-row' + (ev.key === activeEvKey ? ' active' : '') + (ev.fly ? ' fly-ev' : '');
    row.innerHTML = `
      <span class="ev-sum-name" style="color:${ev.fly?'var(--accent2)':'var(--muted)'};">${t(ev.nameKey)}${ev.fly?' ⚡':''}</span>
      <span class="ev-sum-laps">${lapCountStr(ev.dist)}</span>
      <span class="ev-sum-time">${fmt(time)}</span>
    `;
    row.addEventListener('click', () => {
      activeEvKey = ev.key;
      document.querySelectorAll('.ev-tab').forEach(b => b.classList.toggle('active', b.dataset.ev === ev.key));
      renderAllEvents(firstT, flyLap, vF);
      renderEventDetail(lastCalc);
    });
    sum.appendChild(row);
  });
}

function renderEventDetail(c) {
  if (!c) return;
  const ev = EVENTS.find(e => e.key === activeEvKey);
  if (!ev) return;

  const time = evTime(ev, c.firstT, c.flyLap, c.vF);
  $('evTotalTime').textContent = fmt(time);
  $('evTotalTime').style.color = ev.color;

  const halfLen     = trackLen / 2;
  const totalHalves = ev.dist / halfLen;
  const laps        = ev.dist / trackLen;
  $('evMetaCol').innerHTML = `
    <div class="ev-meta-row">${t(ev.nameKey)}　<span>${Number.isInteger(laps) ? laps : laps.toFixed(2)} ${t('ev.laps')}</span></div>
    <div class="ev-meta-row">${t('split.half')} <span>${Math.ceil(totalHalves)} ${t('ev.halves')}</span></div>
    <div class="ev-meta-row">${ev.fly ? t('ev.fly') : t('ev.stand')}</div>
  `;

  const tbody = $('splitBody');
  tbody.innerHTML = '';
  let elapsed = 0, lapNum = 1, halfInLap = 0;
  const totalFullHalves = Math.floor(ev.dist / halfLen);
  const remainder = ev.dist % halfLen;

  for (let h = 0; h < totalFullHalves; h++) {
    const isFirstHalf = h === 0;
    const splitT = isFirstHalf && !ev.fly ? c.firstT / 2 : c.halfLap;
    elapsed += splitT;
    halfInLap++;
    const distSoFar = (h+1) * halfLen;
    const isLapEnd = halfInLap === 2 || trackLen === halfLen;
    if (isLapEnd) halfInLap = 0;

    const tr = document.createElement('tr');
    tr.className = isLapEnd ? 'lap-start' : 'half-row';
    tr.innerHTML = `
      <td class="lap-num">${isLapEnd ? lapNum++ : ''}</td>
      <td>${h+1}${isFirstHalf && !ev.fly ? '<span class="start-marker">ST</span>' : ''}</td>
      <td>${distSoFar}m</td>
      <td>${splitT.toFixed(3)}"</td>
      <td class="elapsed">${fmt(elapsed)}</td>
    `;
    tbody.appendChild(tr);
  }

  if (remainder > 0.1) {
    const partT = (remainder / halfLen) * c.halfLap;
    elapsed += partT;
    const tr = document.createElement('tr');
    tr.className = 'half-row';
    tr.innerHTML = `<td></td><td>+${remainder.toFixed(0)}m</td><td>${ev.dist}m</td><td>${partT.toFixed(3)}"</td><td class="elapsed">${fmt(elapsed)}</td>`;
    tbody.appendChild(tr);
  }
}

// ── geolocation ────────────────────────────────────────────────
async function getLocation() {
  const btn = $('locBtn');
  btn.textContent = '…';
  btn.disabled = true;
  $('locDot').classList.remove('active');
  if (!navigator.geolocation) { alert('Not supported'); btn.textContent = t('topbar.locate'); btn.disabled = false; return; }
  try {
    const pos = await new Promise((res,rej) =>
      navigator.geolocation.getCurrentPosition(res,rej,{enableHighAccuracy:true,timeout:12000}));
    const {latitude:lat, longitude:lon, altitude:gpsAlt} = pos.coords;

    // 施設名取得 と 標高取得 を並列実行
    const [name, elev] = await Promise.all([
      findVelodrome(lat,lon).then(n => n || reverseGeocode(lat,lon)),
      gpsAlt != null ? Promise.resolve(Math.round(gpsAlt)) : fetchElevation(lat,lon),
    ]);

    venueName = name || `${lat.toFixed(4)}, ${lon.toFixed(4)}`;
    $('venueName').textContent = venueName;
    $('locDot').classList.add('active');

    // 標高をスライダーに反映
    if (elev != null) {
      const altEl = $('alt');
      altEl.value = Math.min(Math.max(Math.round(elev/10)*10, 0), 3000);
      calc();
    }
  } catch(e) { $('venueName').textContent = t('topbar.venue'); }
  btn.textContent = t('topbar.locate');
  btn.disabled = false;
}

// 標高API (Open-Elevation, 無料・無認証)
async function fetchElevation(lat,lon) {
  try {
    const r = await fetch(`https://api.open-elevation.com/api/v1/lookup?locations=${lat},${lon}`);
    const d = await r.json();
    return d.results?.[0]?.elevation ?? null;
  } catch(_) {}
  // fallback: OpenTopoData
  try {
    const r = await fetch(`https://api.opentopodata.org/v1/srtm90m?locations=${lat},${lon}`);
    const d = await r.json();
    return d.results?.[0]?.elevation ?? null;
  } catch(_) {}
  return null;
}

async function findVelodrome(lat,lon) {
  try {
    // 競輪場・ベロドローム・サイクリングトラックを検索
    const q = `[out:json][timeout:10];
(
  node["sport"="cycling"](around:5000,${lat},${lon});
  way["sport"="cycling"](around:5000,${lat},${lon});
  node["leisure"="track"]["sport"="cycling"](around:5000,${lat},${lon});
  way["leisure"="sports_centre"]["sport"="cycling"](around:5000,${lat},${lon});
  node["name"~"競輪|ベロドローム|velodrome",i](around:5000,${lat},${lon});
  way["name"~"競輪|ベロドローム|velodrome",i](around:5000,${lat},${lon});
);
out body 3;`;
    const r = await fetch('https://overpass-api.de/api/interpreter',{method:'POST',body:q});
    const d = await r.json();
    if (d.elements?.length > 0) {
      const el = d.elements[0];
      return el.tags?.['name:ja'] || el.tags?.name || null;
    }
  } catch(_){}
  return null;
}

async function reverseGeocode(lat,lon) {
  try {
    const r = await fetch(`https://nominatim.openstreetmap.org/reverse?lat=${lat}&lon=${lon}&format=json&zoom=17`,
      {headers:{'Accept-Language':'ja,en','User-Agent':'TrackCalcApp/1.0'}});
    const d = await r.json();
    return d.address?.amenity||d.address?.leisure||d.address?.city||d.address?.town||d.address?.village||d.display_name?.split(',')[0]||null;
  } catch(_){}
  return null;
}

// ── history ────────────────────────────────────────────────────
function saveRecord() {
  if (!lastCalc) return;
  const rec = {
    id: Date.now(),
    date: new Date().toLocaleString(lang==='ja'?'ja-JP':'en-US',{month:'2-digit',day:'2-digit',hour:'2-digit',minute:'2-digit'}),
    venue: venueName || '-', trackLen, weather: t(`weather.${weatherKey}`),
    ...lastCalc,
    events: EVENTS.map(ev => ({ name:t(ev.nameKey), time:evTime(ev,lastCalc.firstT,lastCalc.flyLap,lastCalc.vF) })),
  };
  const hist = getHistory();
  hist.unshift(rec);
  if (hist.length > 100) hist.pop();
  localStorage.setItem('trackHist', JSON.stringify(hist));
  renderHistory();
  const btn = $('saveBtn'), orig = btn.textContent;
  btn.textContent = lang==='ja' ? '✓ 保存完了' : '✓ Saved';
  setTimeout(()=>btn.textContent=orig, 1200);
}

function getHistory() { try { return JSON.parse(localStorage.getItem('trackHist')||'[]'); } catch { return []; } }

function toggleHistory() {
  $('histToggle').classList.toggle('open');
  $('histBody').classList.toggle('open');
}

function renderHistory() {
  const hist = getHistory();
  $('histCount').textContent = `${hist.length}${lang==='ja'?'件':''}`;
  $('hEmpty').style.display  = hist.length ? 'none' : 'block';
  const list = $('hList');
  list.innerHTML = '';
  hist.forEach(rec => {
    const km1 = rec.events?.find(e => e.name==='1km'||e.name==='1km TT');
    const km4 = rec.events?.find(e => e.name==='4km');
    const row = document.createElement('div');
    row.className = 'h-row';
    row.innerHTML = `
      <div class="h-date">${rec.date}</div>
      <div class="h-info">
        <div class="h-venue">${rec.venue} <span style="color:var(--muted);font-size:.65rem;">${rec.trackLen}m</span></div>
        <div class="h-detail">${rec.front}×${rec.rear} (${(rec.front/rec.rear).toFixed(2)}) | ${rec.cad}rpm | ${rec.vKmh?.toFixed(1)}km/h | ${rec.weather}</div>
      </div>
      <div style="display:flex;flex-direction:column;gap:2px;text-align:right;">
        ${km1?`<div class="h-time" style="font-size:.72rem;color:var(--muted);">1km ${fmt(km1.time)}</div>`:''}
        ${km4?`<div class="h-time">${fmt(km4.time)}</div>`:''}
      </div>
      <button class="h-del" title="delete" onclick="deleteRecord(${rec.id},event)">✕</button>
    `;
    row.addEventListener('click', e => { if (!e.target.classList.contains('h-del')) loadRecord(rec); });
    list.appendChild(row);
  });
}

function deleteRecord(id,e) {
  e.stopPropagation();
  localStorage.setItem('trackHist', JSON.stringify(getHistory().filter(r=>r.id!==id)));
  renderHistory();
}

function loadRecord(rec) {
  trackLen = rec.trackLen;
  document.querySelectorAll('.seg button').forEach(b => b.classList.toggle('active', parseInt(b.textContent)===rec.trackLen));
  const set=(id,v)=>{ if ($(id)) $(id).value=v; };
  set('front',rec.front); set('rear',rec.rear); set('cad',rec.cad);
  set('firstLap',rec.firstT); set('alt', rec.alt ?? 0); set('temp',rec.temp);
  if (rec.venue && rec.venue !== '-') { venueName=rec.venue; $('venueName').textContent=venueName; }
  calc();
}

// ── init ───────────────────────────────────────────────────────
applyLang();
calc();
renderHistory();

// ── PWA service worker ─────────────────────────────────────────
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('./sw.js').catch(err => console.warn('SW reg failed', err));
  });
}
</script>
</body>
</html>
