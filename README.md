# nse-monitor
NSE Momentum Monitor
[nse_momentum_monitor_v2_4.html](https://github.com/user-attachments/files/27548310/nse_momentum_monitor_v2_4.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>NSE Momentum Monitor</title>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;600;700&family=Syne:wght@400;500;600;700&display=swap" rel="stylesheet"/>
<style>
:root {
  --bg: #060910;
  --s1: #0d1117;
  --s2: #131922;
  --s3: #1c2433;
  --border: #1e2d40;
  --border2: #2a3f58;
  --accent: #00d4ff;
  --accent-dim: rgba(0,212,255,0.12);
  --green: #00e5a0;
  --green-dim: rgba(0,229,160,0.12);
  --red: #ff4d6d;
  --red-dim: rgba(255,77,109,0.12);
  --amber: #ffb830;
  --amber-dim: rgba(255,184,48,0.12);
  --purple: #a78bfa;
  --purple-dim: rgba(167,139,250,0.12);
  --text: #e2eaf5;
  --muted: #5a7a9a;
  --mono: 'IBM Plex Mono', monospace;
  --sans: 'Syne', sans-serif;
}
*{box-sizing:border-box;margin:0;padding:0;}
html{scroll-behavior:smooth;}
body{background:var(--bg);color:var(--text);font-family:var(--sans);min-height:100vh;overflow-x:hidden;}

/* grid bg */
body::after{
  content:'';position:fixed;inset:0;
  background-image:linear-gradient(var(--border) 1px,transparent 1px),linear-gradient(90deg,var(--border) 1px,transparent 1px);
  background-size:40px 40px;opacity:0.25;pointer-events:none;z-index:0;
}

header{
  position:relative;z-index:10;
  background:var(--s1);border-bottom:1px solid var(--border);
  padding:1rem 2rem;display:flex;align-items:center;justify-content:space-between;
}
.logo{font-family:var(--mono);font-size:14px;font-weight:700;letter-spacing:0.15em;}
.logo-nse{color:var(--accent);}
.logo-sep{color:var(--muted);margin:0 8px;}
.logo-sub{color:var(--muted);font-weight:400;font-size:11px;}
.header-right{display:flex;align-items:center;gap:16px;}
.live-pill{
  background:var(--green-dim);border:1px solid var(--green);border-radius:99px;
  padding:3px 12px;font-family:var(--mono);font-size:10px;color:var(--green);
  display:flex;align-items:center;gap:6px;letter-spacing:0.08em;
}
.pulse{width:6px;height:6px;border-radius:50%;background:var(--green);animation:pulse 1.5s infinite;}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1);}50%{opacity:0.4;transform:scale(0.8);}}
.time-display{font-family:var(--mono);font-size:11px;color:var(--muted);}

.page{position:relative;z-index:1;max-width:1140px;margin:0 auto;padding:2rem;}

/* section headers */
.sec-head{
  display:flex;align-items:center;gap:10px;margin-bottom:1rem;
}
.sec-tag{
  font-family:var(--mono);font-size:10px;color:var(--accent);
  background:var(--accent-dim);border:1px solid rgba(0,212,255,0.25);
  border-radius:4px;padding:2px 8px;letter-spacing:0.1em;
}
.sec-title{font-family:var(--mono);font-size:12px;color:var(--muted);letter-spacing:0.06em;}

/* add stock panel */
.add-panel{
  background:var(--s1);border:1px solid var(--border);border-radius:16px;
  padding:1.5rem;margin-bottom:1.5rem;
}
.add-fields{display:grid;grid-template-columns:2fr 1fr 1fr 1fr;gap:10px;margin-bottom:10px;}
.field label{display:block;font-family:var(--mono);font-size:10px;color:var(--muted);letter-spacing:0.08em;margin-bottom:5px;}
.field input{
  width:100%;background:var(--bg);border:1px solid var(--border);
  color:var(--text);font-family:var(--mono);font-size:12px;
  padding:9px 12px;border-radius:8px;outline:none;transition:border 0.2s;
}
.field input:focus{border-color:var(--accent);}
.add-actions{display:grid;grid-template-columns:1fr 2fr;gap:10px;}
.btn-add{
  background:var(--accent);color:#000;font-family:var(--mono);font-weight:700;
  font-size:12px;border:none;padding:10px;border-radius:8px;cursor:pointer;
  letter-spacing:0.08em;transition:opacity 0.2s;
}
.btn-add:hover{opacity:0.85;}
.btn-scan-all{
  background:transparent;border:1px solid var(--purple);color:var(--purple);
  font-family:var(--mono);font-size:12px;padding:10px;border-radius:8px;
  cursor:pointer;letter-spacing:0.06em;transition:all 0.2s;
}
.btn-scan-all:hover{background:var(--purple-dim);}
.btn-scan-all:disabled{opacity:0.35;cursor:not-allowed;}

/* stats bar */
.stats-bar{
  display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:1.5rem;
}
.stat-card{
  background:var(--s1);border:1px solid var(--border);border-radius:12px;
  padding:1rem;text-align:center;
}
.stat-val{font-family:var(--mono);font-size:22px;font-weight:700;color:var(--text);}
.stat-val.green{color:var(--green);}
.stat-val.red{color:var(--red);}
.stat-val.amber{color:var(--amber);}
.stat-label{font-family:var(--mono);font-size:10px;color:var(--muted);margin-top:4px;letter-spacing:0.06em;}

/* stock rows */
.stock-row{
  background:var(--s1);border:1px solid var(--border);border-radius:14px;
  margin-bottom:10px;overflow:hidden;transition:border-color 0.2s;
}
.stock-row:hover{border-color:var(--border2);}
.stock-row.verdict-high{border-left:3px solid var(--green);}
.stock-row.verdict-watch{border-left:3px solid var(--amber);}
.stock-row.verdict-avoid{border-left:3px solid var(--red);}
.stock-row.verdict-over{border-left:3px solid var(--muted);}

.stock-top{
  display:grid;grid-template-columns:2fr 1fr 1fr 1fr 1.4fr 0.8fr auto;
  gap:8px;align-items:center;padding:1rem 1.25rem;cursor:pointer;
}
.sname{font-family:var(--mono);font-size:14px;font-weight:700;}
.ssector{font-size:11px;color:var(--muted);margin-top:2px;font-family:var(--mono);}
.scmp{font-family:var(--mono);font-size:14px;font-weight:600;}
.sentry{font-family:var(--mono);font-size:12px;color:var(--muted);}
.sqty{font-size:10px;opacity:0.7;margin-top:2px;}
.spnl{font-family:var(--mono);font-weight:700;line-height:1.2;}
.spnl-amt{font-size:13px;}
.spnl-pct{font-size:10px;opacity:0.85;margin-top:2px;font-weight:600;}
.spnl.pos{color:var(--green);}
.spnl.neg{color:var(--red);}
.thit{font-size:9px;background:var(--green);color:#000;padding:2px 6px;border-radius:3px;margin-left:6px;font-weight:700;letter-spacing:0.05em;vertical-align:middle;}
.target-hit{box-shadow:0 0 0 1px var(--green) inset;}
.pnl-card.pos{border-color:var(--green);}
.pnl-card.neg{border-color:var(--red);}
.pnl-card.pos .tc-val{color:var(--green);}
.pnl-card.neg .tc-val{color:var(--red);}
.verdbadge{
  font-family:var(--mono);font-size:9px;font-weight:700;
  padding:4px 8px;border-radius:6px;letter-spacing:0.07em;text-align:center;white-space:nowrap;
}
.vb-high{background:var(--green-dim);color:var(--green);border:1px solid rgba(0,229,160,0.3);}
.vb-watch{background:var(--amber-dim);color:var(--amber);border:1px solid rgba(255,184,48,0.3);}
.vb-avoid{background:var(--red-dim);color:var(--red);border:1px solid rgba(255,77,109,0.3);}
.vb-over{background:rgba(90,122,154,0.12);color:var(--muted);border:1px solid rgba(90,122,154,0.3);}
.vb-pending{background:var(--purple-dim);color:var(--purple);border:1px solid rgba(167,139,250,0.3);}
.sscore{font-family:var(--mono);font-size:13px;font-weight:600;}
.row-actions{display:flex;gap:6px;align-items:center;}
.btn-analyze{
  background:transparent;border:1px solid var(--border2);color:var(--muted);
  font-family:var(--mono);font-size:10px;padding:5px 10px;border-radius:6px;
  cursor:pointer;transition:all 0.2s;white-space:nowrap;
}
.btn-analyze:hover{border-color:var(--accent);color:var(--accent);}
.btn-rm{background:transparent;border:none;color:var(--muted);cursor:pointer;font-size:15px;padding:2px 4px;transition:color 0.2s;}
.btn-rm:hover{color:var(--red);}

/* expanded analysis */
.analysis-body{display:none;border-top:1px solid var(--border);background:var(--s2);}
.analysis-body.open{display:block;}
.analysis-inner{padding:1.5rem;}

.target-cards{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:1.25rem;}
.tc{border-radius:10px;padding:1rem;text-align:center;}
.tc.t1{background:var(--green-dim);border:1px solid rgba(0,229,160,0.25);}
.tc.t2{background:var(--purple-dim);border:1px solid rgba(167,139,250,0.25);}
.tc.sl{background:var(--red-dim);border:1px solid rgba(255,77,109,0.25);}
.tc-lbl{font-family:var(--mono);font-size:10px;color:var(--muted);letter-spacing:0.08em;margin-bottom:5px;}
.tc-val{font-family:var(--mono);font-size:18px;font-weight:700;}
.t1 .tc-val{color:var(--green);}
.t2 .tc-val{color:var(--purple);}
.sl .tc-val{color:var(--red);}
.tc-sub{font-size:11px;color:var(--muted);margin-top:3px;font-family:var(--mono);}

.info-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:1.25rem;}
.ig{background:var(--s3);border:1px solid var(--border);border-radius:8px;padding:0.85rem;}
.ig-lbl{font-family:var(--mono);font-size:10px;color:var(--muted);letter-spacing:0.07em;margin-bottom:5px;}
.ig-val{font-family:var(--mono);font-size:12px;color:var(--text);}
.ig-val.g{color:var(--green);}
.ig-val.r{color:var(--red);}
.ig-val.a{color:var(--amber);}

.scores-row{display:grid;grid-template-columns:repeat(7,1fr);gap:8px;margin-bottom:1.25rem;}
.sc-item{text-align:center;}
.sc-lbl{font-family:var(--mono);font-size:9px;color:var(--muted);letter-spacing:0.04em;margin-bottom:6px;}
.sc-ring{
  width:42px;height:42px;border-radius:50%;margin:0 auto;
  display:flex;align-items:center;justify-content:center;
  font-family:var(--mono);font-size:13px;font-weight:700;border:2px solid var(--border);
}
.sc-ring.g{border-color:var(--green);color:var(--green);background:var(--green-dim);}
.sc-ring.a{border-color:var(--amber);color:var(--amber);background:var(--amber-dim);}
.sc-ring.r{border-color:var(--red);color:var(--red);background:var(--red-dim);}

.report-box{
  background:var(--bg);border:1px solid var(--border);border-radius:10px;
  padding:1.25rem;font-family:var(--mono);font-size:11px;line-height:1.8;
  color:#7a9ab8;white-space:pre-wrap;max-height:420px;overflow-y:auto;
}
.report-box .g{color:var(--green);}
.report-box .r{color:var(--red);}
.report-box .a{color:var(--amber);}
.report-box .ac{color:var(--accent);}
.report-meta{font-family:var(--mono);font-size:10px;color:var(--muted);margin-top:8px;text-align:right;}

.loading-box{text-align:center;padding:2.5rem;font-family:var(--mono);font-size:12px;color:var(--muted);}
.spin{display:inline-block;width:18px;height:18px;border:2px solid var(--border2);border-top-color:var(--accent);border-radius:50%;animation:spin 0.7s linear infinite;vertical-align:middle;margin-right:8px;}
@keyframes spin{to{transform:rotate(360deg);}}

.empty{text-align:center;padding:5rem 2rem;font-family:var(--mono);font-size:12px;color:var(--muted);}
.empty-icon{font-size:36px;opacity:0.2;margin-bottom:1rem;}

.refresh-note{font-family:var(--mono);font-size:10px;color:var(--muted);text-align:right;margin-top:1rem;}

.portfolio-bar{
  display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:1rem;
  padding:14px;border:1px solid var(--border);background:var(--card);border-radius:6px;
}
.pf-card{text-align:center;}
.pf-lbl{font-family:var(--mono);font-size:10px;color:var(--muted);letter-spacing:0.08em;margin-bottom:4px;}
.pf-val{font-family:var(--mono);font-size:18px;font-weight:700;}
.pf-val.pos{color:var(--green);}
.pf-val.neg{color:var(--red);}

.col-heads{
  display:grid;grid-template-columns:2fr 1fr 1fr 1fr 1.4fr 0.8fr auto;
  gap:8px;padding:0.4rem 1.25rem;
  font-family:var(--mono);font-size:10px;color:var(--muted);letter-spacing:0.08em;
  margin-bottom:6px;
}

@media(max-width:768px){
  .add-fields{grid-template-columns:1fr 1fr;}
  .add-actions{grid-template-columns:1fr;}
  .stats-bar{grid-template-columns:1fr 1fr;}
  .portfolio-bar{grid-template-columns:1fr 1fr;}
  .col-heads{display:none;}
  .stock-top{grid-template-columns:1fr 1fr;gap:6px;}
  .info-grid{grid-template-columns:1fr 1fr;}
  .scores-row{grid-template-columns:repeat(4,1fr);}
  .target-cards{grid-template-columns:1fr;}
}
</style>
</head>
<body>

<header>
  <div>
    <div class="logo">
      <span class="logo-nse">NSE</span>
      <span class="logo-sep">//</span>
      <span>MOMENTUM MONITOR</span>
    </div>
    <div class="logo-sub" style="font-family:var(--mono);font-size:10px;margin-top:3px;color:var(--muted);">POWERED BY CLAUDE AI · FREE · NO API KEY NEEDED</div>
  </div>
  <div class="header-right">
    <div class="live-pill"><span class="pulse"></span>LIVE</div>
    <div class="time-display" id="clockDisplay"></div>
  </div>
</header>

<div class="page">

  <!-- Add stock -->
  <div class="add-panel">
    <div class="sec-head">
      <span class="sec-tag">01</span>
      <span class="sec-title">ADD STOCK TO WATCHLIST</span>
    </div>
    <div class="add-fields">
      <div class="field"><label>NSE SYMBOL</label><input id="iSym" placeholder="e.g. IBULLSLTD" oninput="this.value=this.value.toUpperCase()"/></div>
      <div class="field"><label>ENTRY PRICE ₹</label><input id="iEntry" type="number" placeholder="19.50" step="0.05"/></div>
      <div class="field"><label>QUANTITY</label><input id="iQty" type="number" placeholder="100" step="1"/></div>
      <div class="field"><label>YOUR TARGET ₹</label><input id="iTarget" type="number" placeholder="23.00" step="0.05"/></div>
    </div>
    <div class="add-actions">
      <button class="btn-add" onclick="addStock()">+ ADD STOCK</button>
      <button class="btn-scan-all" id="scanAllBtn" onclick="analyzeAll()">⟳ &nbsp;RUN AI ANALYSIS ON ALL STOCKS</button>
    </div>
  </div>

  <!-- Portfolio summary -->
  <div class="portfolio-bar" id="portfolioBar" style="display:none;">
    <div class="pf-card"><div class="pf-lbl">INVESTED</div><div class="pf-val" id="pfInv">₹0</div></div>
    <div class="pf-card"><div class="pf-lbl">CURRENT VALUE</div><div class="pf-val" id="pfCur">₹0</div></div>
    <div class="pf-card"><div class="pf-lbl">TOTAL P&amp;L</div><div class="pf-val" id="pfPnl">₹0</div></div>
    <div class="pf-card"><div class="pf-lbl">RETURN %</div><div class="pf-val" id="pfPct">—</div></div>
  </div>

  <!-- Stats bar -->
  <div class="stats-bar" id="statsBar" style="display:none;">
    <div class="stat-card"><div class="stat-val" id="stTotal">0</div><div class="stat-label">TOTAL STOCKS</div></div>
    <div class="stat-card"><div class="stat-val green" id="stHigh">0</div><div class="stat-label">HIGH MOMENTUM</div></div>
    <div class="stat-card"><div class="stat-val amber" id="stWatch">0</div><div class="stat-label">WATCHLIST</div></div>
    <div class="stat-card"><div class="stat-val red" id="stAvoid">0</div><div class="stat-label">AVOID / EXTENDED</div></div>
  </div>

  <!-- Watchlist -->
  <div class="sec-head">
    <span class="sec-tag">02</span>
    <span class="sec-title">YOUR WATCHLIST</span>
  </div>

  <div class="col-heads">
    <div>STOCK</div><div>CMP / DAY</div><div>ENTRY × QTY</div><div>P&amp;L ₹/%</div><div>VERDICT</div><div>SCORE</div><div></div>
  </div>

  <div id="stockList"></div>
  <div class="refresh-note" id="refreshNote"></div>

</div>

<script>
let stocks = JSON.parse(localStorage.getItem('nse_v2') || '[]');
let clockTmr, refreshTmr;

// clock
function updateClock(){
  const now = new Date();
  document.getElementById('clockDisplay').textContent =
    now.toLocaleTimeString('en-IN',{hour:'2-digit',minute:'2-digit',second:'2-digit',hour12:false}) + ' IST';
}
setInterval(updateClock,1000); updateClock();

function save(){ localStorage.setItem('nse_v2', JSON.stringify(stocks)); }

function addStock(){
  const sym = document.getElementById('iSym').value.trim().toUpperCase();
  const entry = parseFloat(document.getElementById('iEntry').value);
  const qty = parseInt(document.getElementById('iQty').value);
  const target = parseFloat(document.getElementById('iTarget').value);
  if(!sym||!entry||!qty||!target){alert('Please fill all 4 fields');return;}
  if(stocks.find(s=>s.sym===sym)){alert(sym+' already in watchlist');return;}
  stocks.push({sym,entry,qty,target,cmp:null,change:null,analysis:null,parsed:null,loading:false,open:false,ts:null});
  save(); render();
  ['iSym','iEntry','iQty','iTarget'].forEach(id=>document.getElementById(id).value='');
  fetchPrice(sym);
}

function removeStock(sym){
  if(!confirm('Remove '+sym+'?'))return;
  stocks=stocks.filter(s=>s.sym!==sym);
  save(); render();
}

function toggle(sym){
  const s=stocks.find(x=>x.sym===sym);
  if(s){s.open=!s.open;render();}
}

// fetch price via Yahoo Finance with robust multi-proxy fallback + retry
const _sleep = ms => new Promise(r => setTimeout(r, ms));

async function fetchPrice(sym, isRetry){
  const s=stocks.find(x=>x.sym===sym);
  if(!s)return false;
  const yTicker=sym+'.NS';
  const baseUrl=`https://query1.finance.yahoo.com/v8/finance/chart/${yTicker}?interval=1d&range=5d`;
  // expanded proxy list for redundancy
  const proxies=[
    `https://api.allorigins.win/raw?url=${encodeURIComponent(baseUrl)}`,
    `https://corsproxy.io/?${encodeURIComponent(baseUrl)}`,
    `https://api.codetabs.com/v1/proxy?quest=${encodeURIComponent(baseUrl)}`,
    `https://thingproxy.freeboard.io/fetch/${baseUrl}`,
    `https://cors.eu.org/${baseUrl}`,
  ];
  // try each proxy, with 2 attempts per proxy on transient failure
  for(const proxyUrl of proxies){
    for(let attempt=0; attempt<2; attempt++){
      try{
        const res=await fetch(proxyUrl,{signal:AbortSignal.timeout(10000)});
        if(!res.ok){
          if(res.status===429||res.status===503){ await _sleep(500); continue; }
          break; // hard fail on this proxy, move to next
        }
        const data=await res.json();
        const q=data?.chart?.result?.[0];
        if(!q||!q.meta||q.meta.regularMarketPrice==null) break;
        const meta=q.meta;
        s.cmp=meta.regularMarketPrice;
        s.prevClose=meta.previousClose||meta.chartPreviousClose;
        s.change=s.prevClose?((s.cmp-s.prevClose)/s.prevClose*100):null;
        s.volume=meta.regularMarketVolume;
        s.high52=meta.fiftyTwoWeekHigh;
        s.low52=meta.fiftyTwoWeekLow;
        s.marketCap=meta.marketCap;
        s.stale=false;
        s.lastFetch=Date.now();
        save(); render();
        return true;
      }catch(e){
        await _sleep(300);
        continue;
      }
    }
  }
  // all proxies failed — mark stale silently (no console spam)
  s.stale=true;
  save(); render();
  // schedule one quiet retry in 30s if this wasn't already a retry
  if(!isRetry){
    setTimeout(()=>fetchPrice(sym,true), 30000);
  }
  return false;
}

async function fetchAllPrices(){
  let okCount=0, failCount=0;
  for(const s of stocks){
    const ok = await fetchPrice(s.sym);
    if(ok) okCount++; else failCount++;
    // small delay between stocks to avoid tripping proxy rate limits
    await _sleep(250);
  }
  const note = failCount>0
    ? `Prices refreshed at ${new Date().toLocaleTimeString('en-IN')} · ${okCount} updated, ${failCount} pending retry · Next refresh in 15 min`
    : `Prices refreshed at ${new Date().toLocaleTimeString('en-IN')} · Next refresh in 15 min`;
  document.getElementById('refreshNote').textContent=note;
}

async function analyzeStock(sym){
  const s=stocks.find(x=>x.sym===sym);
  if(!s)return;
  s.loading=true; s.open=true; render();

  await fetchPrice(sym);

  const cmp=s.cmp?'₹'+s.cmp.toFixed(2):'unknown';
  const chg=s.change?s.change.toFixed(2)+'%':'N/A';
  const vol=s.volume?s.volume.toLocaleString('en-IN'):'N/A';
  const h52=s.high52?'₹'+s.high52.toFixed(2):'N/A';
  const l52=s.low52?'₹'+s.low52.toFixed(2):'N/A';
  const near52h=s.high52&&s.cmp?(((s.high52-s.cmp)/s.high52)*100).toFixed(1)+'% below 52w high':'N/A';
  const rr=s.target&&s.cmp?
    (((s.target-s.entry)/(s.entry*0.05)).toFixed(2)+':1 (est)'):'N/A';

  const prompt=`You are a professional NSE momentum swing trading analyst bot.
Analyze this stock for high-probability 10-20% momentum in the next 2-3 weeks.

LIVE DATA:
- Symbol: ${sym} (NSE India)
- CMP: ${cmp}
- Today change: ${chg}
- Volume today: ${vol}
- 52-week High: ${h52}
- 52-week Low: ${l52}
- Distance from 52w high: ${near52h}
- Trader entry: ₹${s.entry}
- Trader quantity: ${s.qty||1} shares
- Trader target: ₹${s.target}
- Estimated R:R: ${rr}

Use web search to find: latest news, Q4 results, FII/DII activity, sector trend, RSI/EMA data, delivery%, analyst targets, any bulk/block deals for ${sym}.

Analyze ALL 16 parameters:
1. PRICE ACTION - 52w high proximity, resistance/support, higher highs/lows, consolidation
2. VOLUME ANALYSIS - vs 20-day avg, classify Weak/Normal/Strong/Explosive
3. RELATIVE STRENGTH - vs Nifty50 and sector index
4. DELIVERY PERCENTAGE - accumulation signal
5. MOVING AVERAGES - 20/50/200 EMA alignment. Price>20EMA>50EMA>200EMA?
6. RSI - current value, Weak<50 / Healthy 60-75 / Overbought>80
7. SECTOR MOMENTUM - sector strength and market sentiment
8. FUNDAMENTAL TRIGGER - results, revenue, profit, orders, news
9. INSTITUTIONAL ACTIVITY - FII/DII, bulk/block deals, promoter activity
10. MARKET CAP - Large/Mid/Small cap, momentum suitability
11. RISK ANALYSIS - stop loss zone, risk%, reward%, R:R ratio
12. ENTRY QUALITY - Early breakout/Retest/Extended/Ideal swing
13. MOMENTUM SCORES - rate each out of 10: Price Action, Volume, Relative Strength, Sector Strength, Institutional Activity, Risk Reward, Momentum Probability
14. FINAL VERDICT - HIGH MOMENTUM CANDIDATE / WATCHLIST STOCK / AVOID / OVEREXTENDED
15. TARGET ESTIMATION - 2-week target, 3-week target, probability of 10-20% move
16. Respond in EXACTLY this format first, then detailed analysis:

-----------------------------------
STOCK NAME: 
CMP: 
SECTOR: 
TREND: BULLISH / NEUTRAL / BEARISH
MOMENTUM SCORE: __/10
BREAKOUT LEVEL: 
SUPPORT LEVEL: 
STOP LOSS: 
VOLUME STATUS: 
RSI STATUS: 
DELIVERY STATUS: 
SECTOR STATUS: 
INSTITUTIONAL ACTIVITY: 
ENTRY QUALITY: IDEAL / GOOD / RISKY / OVEREXTENDED
2-WEEK TARGET: 
3-WEEK TARGET: 
RISK-REWARD RATIO: 
FINAL VERDICT: 
-----------------------------------

Score breakdown (exact format):
Price Action: X/10
Volume: X/10
Relative Strength: X/10
Sector Strength: X/10
Institutional Activity: X/10
Risk Reward: X/10
Momentum Probability: X/10

Then write a full paragraph analysis covering all findings. Be data-driven. Reject weak setups clearly.

IMPORTANT FORMATTING RULES:
- Do NOT use markdown bold (no asterisks like **bold**)
- Do NOT use bullet points or hashes for headers
- Use plain text only with the exact LABEL: value structure shown above
- Fill EVERY field including 2-WEEK TARGET, 3-WEEK TARGET, RISK-REWARD RATIO, and FINAL VERDICT — never leave them blank`;

  try{
    const res=await fetch('https://api.anthropic.com/v1/messages',{
      method:'POST',
      headers:{'Content-Type':'application/json'},
      body:JSON.stringify({
        model:'claude-sonnet-4-20250514',
        max_tokens:4000,
        tools:[{type:'web_search_20250305',name:'web_search'}],
        messages:[{role:'user',content:prompt}]
      })
    });
    const data=await res.json();
    const txt=data.content.filter(b=>b.type==='text').map(b=>b.text).join('\n');
    s.analysis=txt;
    s.ts=new Date().toLocaleTimeString('en-IN');
    parseReport(s,txt);
  }catch(e){
    s.analysis='Analysis error: '+e.message;
  }
  s.loading=false; save(); render();
}

async function analyzeAll(){
  const btn=document.getElementById('scanAllBtn');
  btn.disabled=true; btn.textContent='⟳  ANALYZING... PLEASE WAIT';
  for(const s of stocks) await analyzeStock(s.sym);
  btn.disabled=false; btn.textContent='⟳  RUN AI ANALYSIS ON ALL STOCKS';
}

function parseReport(s,txt){
  // strip markdown bold/italic so regex can match labels reliably
  const clean=txt.replace(/\*\*/g,'').replace(/\*/g,'');
  // generic getter: matches "LABEL: value" up to end of line, case-insensitive
  // ^ or newline before the label so SECTOR doesn't match SECTOR STATUS
  const g=(k)=>{
    const m=clean.match(new RegExp('(?:^|\\n)\\s*'+k+'\\s*[:\\-]\\s*([^\\n]+)','i'));
    return m?m[1].trim().replace(/[`*_]+$/,'').trim():null;
  };
  const scores={};
  const scoreKeys=['Price Action','Volume','Relative Strength','Sector Strength','Institutional Activity','Risk Reward','Momentum Probability'];
  scoreKeys.forEach(k=>{
    const m=clean.match(new RegExp(k+'\\s*[:\\-]\\s*(\\d+(?:\\.\\d+)?)\\s*\\/\\s*10','i'));
    scores[k]=m?parseFloat(m[1]):null;
  });
  s.parsed={
    trend:g('TREND'),score:g('MOMENTUM SCORE'),
    breakout:g('BREAKOUT LEVEL'),support:g('SUPPORT LEVEL'),sl:g('STOP LOSS'),
    volume:g('VOLUME STATUS'),rsi:g('RSI STATUS'),delivery:g('DELIVERY STATUS'),
    sector:g('SECTOR STATUS'),institutional:g('INSTITUTIONAL ACTIVITY'),
    entry:g('ENTRY QUALITY'),t2w:g('2-WEEK TARGET'),t3w:g('3-WEEK TARGET'),
    rrr:g('RISK-REWARD RATIO'),verdict:g('FINAL VERDICT'),sectorName:g('SECTOR'),
    scores
  };
  // debug: see what got parsed
  console.log('Parsed for',s.sym,s.parsed);
}

function verdictKey(v){
  if(!v)return'pending';
  if(v.includes('HIGH'))return'high';
  if(v.includes('WATCH'))return'watch';
  if(v.includes('AVOID'))return'avoid';
  if(v.includes('OVER'))return'over';
  return'pending';
}

function scoreClass(n){return n>=7?'g':n>=5?'a':'r';}

function highlight(txt){
  return txt
    .replace(/(HIGH MOMENTUM CANDIDATE)/g,'<span class="g">$1</span>')
    .replace(/(WATCHLIST STOCK)/g,'<span class="a">$1</span>')
    .replace(/(AVOID|OVEREXTENDED)/g,'<span class="r">$1</span>')
    .replace(/\b(BULLISH|STRONG|EXPLOSIVE|IDEAL)\b/g,'<span class="g">$1</span>')
    .replace(/\b(BEARISH|WEAK|AVOID)\b/g,'<span class="r">$1</span>')
    .replace(/\b(NEUTRAL|NORMAL|MODERATE)\b/g,'<span class="a">$1</span>')
    .replace(/(---+)/g,'<span style="color:var(--border2)">$1</span>');
}

function updateStats(){
  const total=stocks.length;
  const high=stocks.filter(s=>verdictKey(s.parsed?.verdict)==='high').length;
  const watch=stocks.filter(s=>verdictKey(s.parsed?.verdict)==='watch').length;
  const avoid=stocks.filter(s=>['avoid','over'].includes(verdictKey(s.parsed?.verdict))).length;
  document.getElementById('stTotal').textContent=total;
  document.getElementById('stHigh').textContent=high;
  document.getElementById('stWatch').textContent=watch;
  document.getElementById('stAvoid').textContent=avoid;
  document.getElementById('statsBar').style.display=total>0?'grid':'none';

  // portfolio totals (only stocks with live CMP)
  let invested=0, current=0, hasLive=false;
  for(const s of stocks){
    const q=s.qty||1;
    invested += s.entry*q;
    if(s.cmp){ current += s.cmp*q; hasLive=true; }
    else current += s.entry*q; // fall back to entry if no live price
  }
  const pnl = current - invested;
  const pct = invested>0 ? (pnl/invested)*100 : 0;
  const cls = pnl>=0 ? 'pos' : 'neg';
  const pfBar = document.getElementById('portfolioBar');
  pfBar.style.display = total>0 ? 'grid' : 'none';
  document.getElementById('pfInv').textContent = '₹'+invested.toLocaleString('en-IN',{maximumFractionDigits:0});
  document.getElementById('pfCur').textContent = hasLive ? '₹'+current.toLocaleString('en-IN',{maximumFractionDigits:0}) : '—';
  const pfPnl=document.getElementById('pfPnl');
  pfPnl.textContent = hasLive ? (pnl>=0?'+₹':'-₹')+Math.abs(pnl).toLocaleString('en-IN',{maximumFractionDigits:0}) : '—';
  pfPnl.className = 'pf-val ' + (hasLive?cls:'');
  const pfPct=document.getElementById('pfPct');
  pfPct.textContent = hasLive ? (pct>=0?'+':'')+pct.toFixed(2)+'%' : '—';
  pfPct.className = 'pf-val ' + (hasLive?cls:'');
}

function render(){
  updateStats();
  const list=document.getElementById('stockList');
  if(stocks.length===0){
    list.innerHTML='<div class="empty"><div class="empty-icon">◈</div>Add your first stock above to begin monitoring</div>';
    return;
  }
  list.innerHTML=stocks.map(s=>{
    const vk=verdictKey(s.parsed?.verdict);
    const qty=s.qty||1;
    const pnlPct=s.cmp?((s.cmp-s.entry)/s.entry*100):null;
    const pnlAmt=s.cmp?((s.cmp-s.entry)*qty):null;
    const pnlPctStr=pnlPct!=null?(pnlPct>=0?'+':'')+pnlPct.toFixed(2)+'%':'—';
    const pnlAmtStr=pnlAmt!=null?(pnlAmt>=0?'+₹':'-₹')+Math.abs(pnlAmt).toFixed(0):'—';
    const pnlClass=pnlPct==null?'':pnlPct>=0?'pos':'neg';
    const targetHit=s.cmp&&s.target&&s.cmp>=s.target;
    const scoreNum=s.parsed?.score?parseFloat(s.parsed.score):null;
    const vbMap={high:'vb-high',watch:'vb-watch',avoid:'vb-avoid',over:'vb-over',pending:'vb-pending'};
    const vbLabel={high:'HIGH MOMENTUM',watch:'WATCHLIST',avoid:'AVOID',over:'OVEREXTENDED',pending:'PENDING'};
    const scoreKeys=['Price Action','Volume','Relative Strength','Sector Strength','Institutional Activity','Risk Reward','Momentum Probability'];
    const scoreLabels=['PRICE','VOLUME','REL STR','SECTOR','INST','R:R','PROB'];

    return `<div class="stock-row verdict-${vk} ${targetHit?'target-hit':''}" id="sr_${s.sym}">
      <div class="stock-top" onclick="toggle('${s.sym}')">
        <div>
          <div class="sname">${s.sym}${targetHit?' <span class="thit">🎯 TARGET HIT</span>':''}</div>
          <div class="ssector">${s.parsed?.sectorName||'NSE'}</div>
        </div>
        <div>
          <div class="scmp">${s.cmp?'₹'+s.cmp.toFixed(2):'—'}</div>
          <div class="ssector" style="color:${s.change==null?'var(--muted)':s.change>=0?'var(--green)':'var(--red)'}">${s.change!=null?(s.change>=0?'+':'')+s.change.toFixed(2)+'%':'—'} day</div>
        </div>
        <div class="sentry">
          <div>₹${s.entry.toFixed(2)}</div>
          <div class="sqty">× ${qty} qty</div>
        </div>
        <div class="spnl ${pnlClass}">
          <div class="spnl-amt">${pnlAmtStr}</div>
          <div class="spnl-pct">${pnlPctStr}</div>
        </div>
        <div><span class="verdbadge ${vbMap[vk]}">${vbLabel[vk]}</span></div>
        <div class="sscore" style="color:${scoreNum==null?'var(--muted)':scoreNum>=7?'var(--green)':scoreNum>=5?'var(--amber)':'var(--red)'}">${scoreNum!=null?scoreNum.toFixed(1)+'/10':'—'}</div>
        <div class="row-actions">
          <button class="btn-analyze" onclick="event.stopPropagation();analyzeStock('${s.sym}')">ANALYZE</button>
          <button class="btn-rm" onclick="event.stopPropagation();removeStock('${s.sym}')">✕</button>
        </div>
      </div>

      <div class="analysis-body ${s.open?'open':''}">
        ${s.loading?`<div class="loading-box"><span class="spin"></span>Fetching live data &amp; running 16-parameter AI analysis on ${s.sym}...</div>`:''}
        ${!s.loading&&s.analysis?`<div class="analysis-inner">
          <div class="target-cards">
            <div class="tc t1">
              <div class="tc-lbl">2-WEEK TARGET</div>
              <div class="tc-val">${s.parsed?.t2w||'—'}</div>
              <div class="tc-sub">your target: ₹${s.target}</div>
            </div>
            <div class="tc t2">
              <div class="tc-lbl">3-WEEK TARGET</div>
              <div class="tc-val">${s.parsed?.t3w||'—'}</div>
              <div class="tc-sub">R:R → ${s.parsed?.rrr||'—'}</div>
            </div>
            <div class="tc pnl-card ${pnlClass}">
              <div class="tc-lbl">YOUR P&amp;L</div>
              <div class="tc-val">${pnlAmtStr}</div>
              <div class="tc-sub">${pnlPctStr} · invested ₹${(s.entry*qty).toFixed(0)} · now ₹${s.cmp?(s.cmp*qty).toFixed(0):'—'}</div>
            </div>
          </div>

          <div class="info-grid">
            <div class="ig"><div class="ig-lbl">TREND</div><div class="ig-val ${s.parsed?.trend?.includes('BULL')?'g':s.parsed?.trend?.includes('BEAR')?'r':'a'}">${s.parsed?.trend||'—'}</div></div>
            <div class="ig"><div class="ig-lbl">VOLUME</div><div class="ig-val ${s.parsed?.volume?.match(/STRONG|EXPLOS/i)?'g':s.parsed?.volume?.match(/WEAK/i)?'r':'a'}">${s.parsed?.volume||'—'}</div></div>
            <div class="ig"><div class="ig-lbl">RSI STATUS</div><div class="ig-val ${s.parsed?.rsi?.match(/Health|60|65|70/i)?'g':s.parsed?.rsi?.match(/Over|80/i)?'a':s.parsed?.rsi?.match(/Weak|<50/i)?'r':''}">${s.parsed?.rsi||'—'}</div></div>
            <div class="ig"><div class="ig-lbl">ENTRY QUALITY</div><div class="ig-val ${s.parsed?.entry?.includes('IDEAL')?'g':s.parsed?.entry?.match(/RISKY|OVER/i)?'r':'a'}">${s.parsed?.entry||'—'}</div></div>
            <div class="ig"><div class="ig-lbl">SECTOR</div><div class="ig-val">${s.parsed?.sector||'—'}</div></div>
            <div class="ig"><div class="ig-lbl">INSTITUTIONAL</div><div class="ig-val">${s.parsed?.institutional||'—'}</div></div>
          </div>

          <div class="scores-row">
            ${scoreKeys.map((k,i)=>{
              const v=s.parsed?.scores?.[k];
              return `<div class="sc-item">
                <div class="sc-lbl">${scoreLabels[i]}</div>
                <div class="sc-ring ${v!=null?scoreClass(v):''}">${v!=null?v:'?'}</div>
              </div>`;
            }).join('')}
          </div>

          <div class="report-box">${highlight(s.analysis)}</div>
          <div class="report-meta">Analyzed: ${s.ts||'—'} &nbsp;·&nbsp; Prices: Yahoo Finance (free)</div>
        </div>`:''}
        ${!s.loading&&!s.analysis?`<div class="loading-box" style="color:var(--muted)">Click ANALYZE to run the full 16-parameter AI report on ${s.sym}</div>`:''}
      </div>
    </div>`;
  }).join('');
}

// auto refresh prices every 15 min
function startRefresh(){
  clearInterval(refreshTmr);
  fetchAllPrices();
  refreshTmr=setInterval(fetchAllPrices, 15*60*1000);
}

render();
if(stocks.length>0) startRefresh();
</script>
</body>
</html>
