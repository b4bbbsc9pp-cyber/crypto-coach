<!DOCTYPE html>

<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Crypto Coach</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { background: #050510; color: #e0e0f0; font-family: monospace; font-size: 13px; min-height: 100vh; }
.header { background: #07071a; border-bottom: 1px solid #13133a; padding: 10px 14px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 10; }
.header-title { color: #00ff9d; font-weight: 700; letter-spacing: 2px; font-size: 14px; }
.header-sub { font-size: 9px; margin-top: 2px; }
.tabs { display: flex; background: #07071a; border-bottom: 1px solid #13133a; position: sticky; top: 53px; z-index: 10; }
.tab { background: transparent; border: none; border-bottom: 2px solid transparent; color: #3a3a6a; padding: 9px 14px; font-size: 10px; font-weight: 700; font-family: monospace; cursor: pointer; letter-spacing: 1px; }
.tab.active { background: #0a1228; border-bottom: 2px solid #00ff9d; color: #00ff9d; }
.timer-bar { margin-left: auto; padding: 8px 10px; display: flex; align-items: center; }
.timer-track { width: 28px; height: 4px; background: #13133a; border-radius: 2px; overflow: hidden; }
.timer-fill { height: 100%; background: #00ff9d33; transition: width 1s linear; }
.body { padding: 12px; }
.card { background: #08081c; border: 1px solid #13133a; border-radius: 6px; padding: 12px; margin-bottom: 8px; }
.stat-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px; }
.stat-box { background: #09091f; border-radius: 4px; padding: 8px 10px; }
.stat-label { color: #2a2a4a; font-size: 9px; margin-bottom: 3px; }
.stat-val { font-size: 13px; font-weight: 700; }
.msg-card { border-radius: 6px; padding: 12px; margin-bottom: 12px; border-left-width: 4px; border-left-style: solid; }
.msg-title-row { display: flex; align-items: center; gap: 8px; margin-bottom: 10px; }
.msg-emoji { font-size: 20px; }
.msg-title { font-weight: 700; font-size: 13px; }
.msg-body { color: #c0c0d8; font-size: 13px; line-height: 1.8; margin-bottom: 12px; }
.bar-wrap { background: #050510; border-radius: 5px; padding: 8px 12px; margin-bottom: 12px; border: 1px solid #13133a; }
.bar-top { display: flex; justify-content: space-between; margin-bottom: 5px; font-size: 10px; }
.bar-track { background: #0d0d25; border-radius: 2px; height: 6px; overflow: hidden; }
.bar-fill { height: 100%; transition: width 1s ease; }
.bar-bot { display: flex; justify-content: space-between; margin-top: 4px; font-size: 9px; color: #2a2a4a; }
.btn-row { display: flex; gap: 8px; flex-wrap: wrap; }
.btn { border-radius: 4px; padding: 7px 14px; font-size: 11px; font-weight: 700; font-family: monospace; cursor: pointer; border-width: 1px; border-style: solid; }
.btn-g { background: #00ff9d20; border-color: #00ff9d55; color: #00ff9d; }
.btn-r { background: #ff4d6d20; border-color: #ff4d6d55; color: #ff4d6d; }
.btn-y { background: #ffd70020; border-color: #ffd70055; color: #ffd700; }
.btn-sm { font-size: 10px !important; padding: 5px 10px !important; }
.link-btn { display: inline-block; text-decoration: none; border-radius: 4px; padding: 7px 14px; font-size: 11px; font-weight: 700; font-family: monospace; background: #00ff9d20; border: 1px solid #00ff9d55; color: #00ff9d; }
.link-btn-sm { display: inline-block; text-decoration: none; border-radius: 4px; padding: 5px 10px; font-size: 10px; font-weight: 700; font-family: monospace; background: #00ff9d20; border: 1px solid #00ff9d55; color: #00ff9d; }
.data-grid { display: grid; grid-template-columns: repeat(3,1fr); gap: 6px; margin-bottom: 8px; }
.data-box { background: #09091f; border-radius: 3px; padding: 4px 8px; }
.data-label { color: #2a2a4a; font-size: 9px; }
.score-badge { border-radius: 3px; padding: 1px 6px; font-size: 9px; font-weight: 700; border-width: 1px; border-style: solid; }
.empty { text-align: center; color: #2a2a4a; padding: 30px; }
.trade-row { display: flex; justify-content: space-between; font-size: 10px; margin-bottom: 5px; border-bottom: 1px solid #0d0d20; padding-bottom: 4px; }
::-webkit-scrollbar { width: 3px; }
::-webkit-scrollbar-track { background: #050510; }
::-webkit-scrollbar-thumb { background: #13133a; }
</style>
</head>
<body>

<div class="header">
  <div>
    <div class="header-title">◈ CRYPTO COACH</div>
    <div class="header-sub" id="hSub" style="color:#2a2a4a">Caricamento...</div>
  </div>
  <div style="text-align:right">
    <div id="hVal" style="font-weight:700;font-size:13px;color:#00ff9d">$100.00</div>
    <div id="hPnl" style="font-size:9px;color:#00ff9d">+$0.00 (0.0%)</div>
  </div>
</div>

<div class="tabs">
  <button class="tab active" onclick="go('coach')">🎯 COACH</button>
  <button class="tab" onclick="go('market')">📊 MERCATO</button>
  <button class="tab" onclick="go('wallet')">💼 WALLET</button>
  <div class="timer-bar"><div class="timer-track"><div class="timer-fill" id="tFill" style="width:100%"></div></div></div>
</div>

<div id="tab-coach" class="body"></div>
<div id="tab-market" class="body" style="display:none"></div>
<div id="tab-wallet" class="body" style="display:none"></div>

<script>
var BUDGET = 100, REFRESH = 30;
var cash = BUDGET, holdings = {}, trades = [], tab = 'coach', timerSec = REFRESH, isLive = false;

// Token config — Kraken pair names
var TOKENS = [
  { symbol:'SOL',  name:'Solana',       kraken:'SOLUSD',  mcap:82000 },
  { symbol:'JUP',  name:'Jupiter',      kraken:'JUPUSD',  mcap:1200  },
  { symbol:'WIF',  name:'dogwifhat',    kraken:'WIFUSD',  mcap:1900  },
  { symbol:'BONK', name:'Bonk',         kraken:'BONKUSD', mcap:1300  },
  { symbol:'JTO',  name:'Jito',         kraken:'JTOUSD',  mcap:800   },
  { symbol:'PYTH', name:'Pyth Network', kraken:'PYTHUSD', mcap:600   },
  { symbol:'RAY',  name:'Raydium',      kraken:'RAYUSD',  mcap:980   },
  { symbol:'DOGE', name:'Dogecoin',     kraken:'XDGUSD',  mcap:24800 },
];

var FALLBACK = [
  { symbol:'SOL',  name:'Solana',       price:148.20, chg:3.2,  vol:4200, score:71, mcap:82000 },
  { symbol:'JUP',  name:'Jupiter',      price:0.58,   chg:8.4,  vol:180,  score:78, mcap:1200  },
  { symbol:'WIF',  name:'dogwifhat',    price:0.91,   chg:11.2, vol:320,  score:82, mcap:1900  },
  { symbol:'BONK', name:'Bonk',         price:0.0000142,chg:6.8,vol:280, score:74, mcap:1300  },
  { symbol:'JTO',  name:'Jito',         price:1.84,   chg:4.1,  vol:95,   score:68, mcap:800   },
  { symbol:'PYTH', name:'Pyth Network', price:0.22,   chg:5.3,  vol:75,   score:70, mcap:600   },
  { symbol:'RAY',  name:'Raydium',      price:2.41,   chg:7.6,  vol:140,  score:75, mcap:980   },
  { symbol:'DOGE', name:'Dogecoin',     price:0.168,  chg:2.1,  vol:920,  score:62, mcap:24800 },
];

var tokens = JSON.parse(JSON.stringify(FALLBACK));

function fmtP(p) {
  if (!p) return '—';
  if (p < 0.000001) return '$' + p.toExponential(2);
  if (p < 0.0001) return '$' + p.toFixed(8);
  if (p < 0.01) return '$' + p.toFixed(6);
  if (p < 1) return '$' + p.toFixed(4);
  if (p >= 1000) return '$' + p.toLocaleString('it-IT',{maximumFractionDigits:2});
  return '$' + p.toFixed(2);
}

function scoreColor(s) { return s >= 70 ? '#00ff9d' : s >= 45 ? '#ffd700' : '#ff4d6d'; }

function calcScore(chg, vol, mcap) {
  var s = 50;
  if (chg > 15) s += 15; else if (chg > 8) s += 9; else if (chg > 3) s += 4;
  else if (chg < -15) s -= 15; else if (chg < -8) s -= 9; else if (chg < -3) s -= 4;
  var r = vol / mcap;
  if (r > 0.4) s += 15; else if (r > 0.2) s += 9; else if (r > 0.08) s += 4; else if (r < 0.02) s -= 8;
  if (mcap < 500) s += 8; else if (mcap < 2000) s += 4; else if (mcap > 20000) s -= 4;
  return Math.max(5, Math.min(95, Math.round(s)));
}

function holdVal() {
  return tokens.reduce(function(s,t) { return s + (holdings[t.symbol] ? holdings[t.symbol].qty * t.price : 0); }, 0);
}
function totalVal() { return cash + holdVal(); }
function pnlVal() { return totalVal() - BUDGET; }

async function fetchPrices() {
  try {
    var pairs = TOKENS.map(function(t) { return t.kraken; }).join(',');
    var res = await fetch('https://api.kraken.com/0/public/Ticker?pair=' + pairs);
    if (!res.ok) throw new Error('HTTP ' + res.status);
    var data = await res.json();
    if (data.error && data.error.length) throw new Error(data.error[0]);
    var result = data.result;
    var updated = TOKENS.map(function(tok) {
      // Find result key matching our pair
      var key = Object.keys(result).find(function(k) { return k.toUpperCase().indexOf(tok.symbol) !== -1; });
      if (!key) return tokens.find(function(t) { return t.symbol === tok.symbol; }) || tok;
      var d = result[key];
      var price = parseFloat(d.c[0]);
      var open = parseFloat(d.o);
      var chg = open > 0 ? ((price - open) / open * 100) : 0;
      var vol = parseFloat(d.v[1]) * price / 1e6;
      return Object.assign({}, tok, {
        price: price,
        chg: parseFloat(chg.toFixed(2)),
        vol: parseFloat(vol.toFixed(1)),
        score: calcScore(chg, vol, tok.mcap)
      });
    });
    tokens = updated;
    isLive = true;
  } catch(e) {
    isLive = false;
  }
  render();
}

function go(t) {
  tab = t;
  ['coach','market','wallet'].forEach(function(id, i) {
    document.getElementById('tab-' + id).style.display = id === t ? 'block' : 'none';
    document.querySelectorAll('.tab')[i].className = 'tab' + (id === t ? ' active' : '');
  });
  render();
}

function buyToken(sym, usd) {
  var tok = tokens.find(function(t) { return t.symbol === sym; });
  if (!tok || cash < usd) return;
  var qty = usd / tok.price;
  if (holdings[sym]) {
    var tot = holdings[sym].qty + qty;
    holdings[sym] = { qty: tot, avg: (holdings[sym].qty * holdings[sym].avg + qty * tok.price) / tot, name: tok.name };
  } else {
    holdings[sym] = { qty: qty, avg: tok.price, name: tok.name };
  }
  cash = parseFloat((cash - usd).toFixed(4));
  trades.unshift({ type:'BUY', symbol:sym, price:tok.price, usd:usd, time:new Date().toLocaleTimeString('it-IT') });
  render();
}

function sellToken(sym, frac) {
  var tok = tokens.find(function(t) { return t.symbol === sym; });
  var h = holdings[sym];
  if (!tok || !h) return;
  var qty = h.qty * frac;
  var proceeds = qty * tok.price;
  var pnl = parseFloat((proceeds - qty * h.avg).toFixed(4));
  if (frac >= 1) delete holdings[sym];
  else holdings[sym] = Object.assign({}, h, { qty: h.qty - qty });
  cash = parseFloat((cash + proceeds).toFixed(4));
  trades.unshift({ type: frac < 1 ? 'SELL½' : 'SELL', symbol:sym, price:tok.price, usd:proceeds, pnl:pnl, time:new Date().toLocaleTimeString('it-IT') });
  render();
}

function getMsgs() {
  var msgs = [];
  Object.keys(holdings).forEach(function(sym) {
    var h = holdings[sym], tk = tokens.find(function(t) { return t.symbol === sym; });
    if (!tk) return;
    var pct = ((tk.price - h.avg) / h.avg) * 100;
    var val = h.qty * tk.price, inv = h.qty * h.avg, gain = (val - inv).toFixed(2);
    if (pct >= 40) msgs.push({ id:sym+'_moon', pri:1, col:'#ff4d6d', sym:sym, emoji:'🚀', title:sym+' +'+pct.toFixed(0)+'% — VENDI ORA', body:'Hai trasformato $'+inv.toFixed(0)+' in $'+val.toFixed(0)+'. Guadagno reale di $'+gain+'. Questi movimenti si invertono in pochi minuti.', act:'sell_all', cta:'VENDI TUTTO' });
    else if (pct >= 20) msgs.push({ id:sym+'_take', pri:2, col:'#00ff9d', sym:sym, emoji:'✅', title:sym+' +'+pct.toFixed(0)+'% — Incassa', body:'Entrato a '+fmtP(h.avg)+', ora '+fmtP(tk.price)+'. Guadagno $'+gain+' su $'+inv.toFixed(0)+' investiti. Vendi almeno la metà.', act:'sell_half', cta:'VENDI METÀ', cta2:'VENDI TUTTO' });
    else if (pct >= 10) msgs.push({ id:sym+'_hold', pri:3, col:'#ffd700', sym:sym, emoji:'👀', title:sym+' +'+pct.toFixed(0)+'% — Tieni ancora', body:'Stai guadagnando $'+gain+'. Target ideale +20/30%. Aspetta, ma se scende sotto l\'entrata esci subito.' });
    else if (pct > 0) msgs.push({ id:sym+'_wait', pri:5, col:'#444', sym:sym, emoji:'⏳', title:sym+' +'+pct.toFixed(1)+'% — Troppo poco', body:'Aspetta almeno +10% prima di considerare l\'uscita.' });
    else if (pct <= -20) msgs.push({ id:sym+'_stop', pri:1, col:'#ff4d6d', sym:sym, emoji:'🛑', title:sym+' -'+Math.abs(pct).toFixed(0)+'% — Taglia la perdita', body:'Stai perdendo $'+Math.abs(gain)+'. Con $100 speculativi non conviene aspettare. Esci e preserva il capitale.', act:'sell_all', cta:'ESCI ORA' });
    else if (pct <= -10) msgs.push({ id:sym+'_warn', pri:2, col:'#ff4d6d', sym:sym, emoji:'⚠️', title:sym+' -'+Math.abs(pct).toFixed(0)+'% — Attenzione', body:'Stai perdendo $'+Math.abs(gain)+'. Se continua oltre -20% esci senza pensarci.', act:'sell_all', cta:'VENDI E TAGLIA' });
    else msgs.push({ id:sym+'_neg', pri:4, col:'#ffd700', sym:sym, emoji:'👁', title:sym+' -'+Math.abs(pct).toFixed(1)+'% — Oscillazione', body:'Perdita lieve. Se scende oltre -10% rivaluta l\'uscita.' });
  });
  if (cash >= 20 && Object.keys(holdings).length < 4) {
    var best = tokens.filter(function(t) { return t.score >= 70 && !holdings[t.symbol]; }).sort(function(a,b) { return b.score - a.score; })[0];
    if (best) msgs.push({ id:'buy_'+best.symbol, pri:2, col:'#00ff9d', sym:best.symbol, emoji:'🔥', title:best.symbol+' — Segnale forte', body:(best.chg>=0?'+':'')+best.chg.toFixed(1)+'% oggi, score '+best.score+'/100. Hai $'+cash.toFixed(0)+' liberi. Vai su Jupiter e compra.', act:'jupiter', cta:'🌐 APRI JUPITER → COMPRA '+best.symbol, jupSym:best.symbol });
  }
  if (!msgs.length) msgs.push({ id:'calm', pri:9, col:'#333', sym:null, emoji:'🌙', title:'Mercato tranquillo', body:'Nessun segnale forte ora. Il coach aggiorna ogni 30 secondi.' });
  return msgs.sort(function(a,b) { return a.pri - b.pri; });
}

function updateHeader() {
  var p = pnlVal(), tv = totalVal(), c = p >= 0 ? '#00ff9d' : '#ff4d6d';
  document.getElementById('hVal').style.color = c;
  document.getElementById('hVal').textContent = '$' + tv.toFixed(2);
  document.getElementById('hPnl').style.color = c;
  document.getElementById('hPnl').textContent = (p>=0?'+':'') + '$' + p.toFixed(2) + ' (' + ((p/BUDGET)*100).toFixed(1) + '%)';
  var sub = document.getElementById('hSub');
  sub.style.color = isLive ? '#00ff9d' : '#ffd700';
  sub.textContent = (isLive ? '● LIVE Kraken' : '● Dati riferimento') + ' · aggiorna in ' + timerSec + 's';
}

function renderCoach() {
  var p = pnlVal(), hv = holdVal(), pc = p>=0?'#00ff9d':'#ff4d6d';
  var h = '<div class="card" style="background:#060614;border-left:3px solid #13133a;margin-bottom:14px"><div style="color:#2a2a4a;font-size:9px;letter-spacing:2px;margin-bottom:10px">LA TUA SITUAZIONE</div><div class="stat-grid">';
  h += '<div class="stat-box"><div class="stat-label">CASH</div><div class="stat-val" style="color:#e0e0f0">$'+cash.toFixed(2)+'</div></div>';
  h += '<div class="stat-box"><div class="stat-label">IN TOKEN</div><div class="stat-val" style="color:#888">$'+hv.toFixed(2)+'</div></div>';
  h += '<div class="stat-box"><div class="stat-label">GUADAGNO</div><div class="stat-val" style="color:'+pc+'">'+(p>=0?'+':'')+'$'+p.toFixed(2)+'</div></div>';
  h += '</div></div>';

  getMsgs().forEach(function(msg) {
    var tk = tokens.find(function(t) { return t.symbol === msg.sym; });
    var ho = holdings[msg.sym];
    var pct = (tk && ho) ? ((tk.price - ho.avg) / ho.avg * 100) : null;
    var bg = msg.pri===1 ? '#0d0710' : msg.col==='#00ff9d' ? '#06100a' : '#08081c';
    h += '<div class="msg-card" style="border-left-color:'+msg.col+';background:'+bg+'">';
    h += '<div class="msg-title-row"><span class="msg-emoji">'+msg.emoji+'</span><span class="msg-title" style="color:'+msg.col+'">'+msg.title+'</span></div>';
    h += '<div class="msg-body">'+msg.body+'</div>';
    if (ho && tk && pct !== null) {
      var bw = Math.min(100,Math.max(2,50+pct)), bc = pct>=0?'#00ff9d':'#ff4d6d';
      h += '<div class="bar-wrap"><div class="bar-top"><span style="color:#555">Entrato a '+fmtP(ho.avg)+'</span><span style="color:'+bc+';font-weight:700">ora '+fmtP(tk.price)+' ('+(pct>=0?'+':'')+pct.toFixed(1)+'%)</span></div>';
      h += '<div class="bar-track"><div class="bar-fill" style="width:'+bw+'%;background:'+bc+'"></div></div>';
      h += '<div class="bar-bot"><span>$'+(ho.qty*ho.avg).toFixed(2)+' investiti</span><span style="color:'+bc+'">$'+(ho.qty*tk.price).toFixed(2)+' ora</span></div></div>';
    }
    h += '<div class="btn-row">';
    if (msg.act==='sell_all' && ho && tk) h += '<button class="btn btn-r" onclick="sellToken(\''+msg.sym+'\',1)">'+msg.cta+'</button>';
    if (msg.act==='sell_half' && ho && tk) h += '<button class="btn btn-g" onclick="sellToken(\''+msg.sym+'\',0.5)">✂️ '+msg.cta+'</button><button class="btn btn-r" onclick="sellToken(\''+msg.sym+'\',1)">🔴 '+msg.cta2+'</button>';
    if (msg.act==='jupiter') h += '<a class="link-btn" href="https://jup.ag/swap/SOL-'+msg.jupSym+'" target="_blank">'+msg.cta+'</a>';
    h += '</div></div>';
  });
  document.getElementById('tab-coach').innerHTML = h;
}

function renderMarket() {
  var sorted = tokens.slice().sort(function(a,b) { return b.score - a.score; });
  var h = '';
  sorted.forEach(function(t) {
    var ho = holdings[t.symbol], sc = scoreColor(t.score), cc = t.chg>=0?'#00ff9d':'#ff4d6d';
    h += '<div class="card" style="border-left:3px solid '+sc+';opacity:'+(t.score<40?0.5:1)+'">';
    h += '<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">';
    h += '<div><span style="color:#e0e0f0;font-weight:700;font-size:13px">'+t.symbol+'</span><span style="color:#333;font-size:10px;margin-left:6px">'+t.name+'</span>'+(ho?'<span style="color:#ffd700;font-size:9px;margin-left:6px">★</span>':'')+'</div>';
    h += '<span class="score-badge" style="background:'+sc+'18;border-color:'+sc+'44;color:'+sc+'">'+t.score+'/100</span></div>';
    h += '<div class="data-grid">';
    h += '<div class="data-box"><div class="data-label">PREZZO</div><div style="font-size:11px;font-family:monospace">'+fmtP(t.price)+'</div></div>';
    h += '<div class="data-box"><div class="data-label">24H</div><div style="font-size:11px;font-weight:700;color:'+cc+'">'+(t.chg>=0?'▲':'▼')+Math.abs(t.chg).toFixed(1)+'%</div></div>';
    h += '<div class="data-box"><div class="data-label">VOLUME</div><div style="font-size:11px;font-family:monospace">$'+t.vol+'M</div></div>';
    h += '</div>';
    if (!ho && t.score >= 65 && cash >= 20) h += '<a class="link-btn link-btn-sm" href="https://jup.ag/swap/SOL-'+t.symbol+'" target="_blank">🌐 COMPRA SU JUPITER</a>';
    if (ho) h += '<button class="btn btn-r btn-sm" onclick="sellToken(\''+t.symbol+'\',1)">🔴 SEGNA VENDUTO</button>';
    h += '</div>';
  });
  document.getElementById('tab-market').innerHTML = h;
}

function renderWallet() {
  var p = pnlVal(), hv = holdVal(), tv = totalVal(), pc = p>=0?'#00ff9d':'#ff4d6d';
  var h = '<div class="stat-grid" style="margin-bottom:12px">';
  h += '<div class="stat-box"><div class="stat-label">CASH</div><div class="stat-val" style="color:#e0e0f0">$'+cash.toFixed(2)+'</div></div>';
  h += '<div class="stat-box"><div class="stat-label">TOKEN</div><div class="stat-val" style="color:#888">$'+hv.toFixed(2)+'</div></div>';
  h += '<div class="stat-box"><div class="stat-label">P&L</div><div class="stat-val" style="color:'+pc+'">'+(p>=0?'+':'')+'$'+p.toFixed(2)+'</div></div>';
  h += '</div>';
  h += '<div class="card" style="border-left:3px solid #ffd700"><div style="color:#ffd700;font-size:10px;margin-bottom:6px">⚡ COME FUNZIONA</div><div style="color:#555;font-size:11px;line-height:1.8">1. Coach vede segnale forte<br>2. Tocchi il link verde → apre Jupiter<br>3. Jupiter è già connesso al tuo wallet<br>4. Inserisci importo → Face ID → eseguito<br>5. Torna qui e segna il trade</div></div>';

  var keys = Object.keys(holdings);
  if (!keys.length) {
    h += '<div class="card"><div class="empty"><div style="font-size:24px;margin-bottom:6px">💼</div>Nessun trade ancora</div></div>';
  } else {
    keys.forEach(function(sym) {
      var ho = holdings[sym], tk = tokens.find(function(t) { return t.symbol === sym; });
      var cp = tk ? tk.price : ho.avg, val = ho.qty*cp, hpnl = val-ho.qty*ho.avg, hpct = ((hpnl/(ho.qty*ho.avg))*100).toFixed(1), pc2 = hpnl>=0?'#00ff9d':'#ff4d6d';
      h += '<div class="card" style="border-left:3px solid '+pc2+'">';
      h += '<div style="display:flex;justify-content:space-between;margin-bottom:8px"><span style="color:#e0e0f0;font-weight:700">'+sym+' <span style="color:#444;font-weight:400">'+ho.name+'</span></span><span style="color:'+pc2+';font-weight:700">'+(hpnl>=0?'+':'')+'$'+hpnl.toFixed(2)+' ('+hpct+'%)</span></div>';
      h += '<div class="data-grid"><div class="data-box"><div class="data-label">VALORE</div><div style="font-size:11px">$'+val.toFixed(2)+'</div></div><div class="data-box"><div class="data-label">ENTRATA</div><div style="font-size:11px">'+fmtP(ho.avg)+'</div></div><div class="data-box"><div class="data-label">ORA</div><div style="font-size:11px;color:'+pc2+'">'+fmtP(cp)+'</div></div></div>';
      h += '<div class="btn-row"><button class="btn btn-r btn-sm" onclick="sellToken(\''+sym+'\',1)">🔴 SEGNA VENDUTO</button><button class="btn btn-y btn-sm" onclick="sellToken(\''+sym+'\',0.5)">✂️ SEGNA METÀ</button></div></div>';
    });
  }

  if (trades.length) {
    h += '<div class="card"><div style="color:#2a2a4a;font-size:9px;letter-spacing:2px;margin-bottom:8px">STORICO</div>';
    trades.slice(0,15).forEach(function(t) {
      var tc = t.type==='BUY'?'#00ff9d':'#ff4d6d';
      h += '<div class="trade-row"><span style="color:'+tc+'">'+(t.type==='BUY'?'▲':'▼')+' '+t.symbol+'</span><span style="color:#555">$'+t.usd.toFixed(2)+'</span>'+(t.pnl!=null?'<span style="color:'+(t.pnl>=0?'#00ff9d':'#ff4d6d')+'">'+(t.pnl>=0?'+':'')+'$'+t.pnl.toFixed(2)+'</span>':'<span></span>')+'<span style="color:#1a1a3a">'+t.time+'</span></div>';
    });
    h += '</div>';
  }

  document.getElementById('tab-wallet').innerHTML = h;
}

function render() {
  updateHeader();
  if (tab==='coach') renderCoach();
  if (tab==='market') renderMarket();
  if (tab==='wallet') renderWallet();
}

setInterval(function() {
  timerSec = Math.max(0, timerSec-1);
  document.getElementById('tFill').style.width = (timerSec/REFRESH*100)+'%';
  updateHeader();
}, 1000);

setInterval(function() {
  timerSec = REFRESH;
  fetchPrices();
}, REFRESH*1000);

fetchPrices();
render();
</script>

</body>
</html>
