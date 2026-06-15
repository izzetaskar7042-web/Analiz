<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no"/>
<title>MatchIQ</title>
<meta name="theme-color" content="#7c3aed"/>
<meta name="apple-mobile-web-app-capable" content="yes"/>
<meta name="apple-mobile-web-app-title" content="MatchIQ"/>
<style>
*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%;background:#020817;color:#f1f5f9;font-family:-apple-system,sans-serif;overscroll-behavior:none}
::-webkit-scrollbar{display:none}
#header{position:fixed;top:0;left:0;right:0;z-index:200;background:#0a0f1eee;backdrop-filter:blur(12px);border-bottom:1px solid #1e293b;padding:env(safe-area-inset-top,0px) 16px 0}
.header-inner{display:flex;justify-content:space-between;align-items:center;height:56px}
.logo-text{font-size:17px;font-weight:800}.logo-text span{color:#a78bfa}
.logo-sub{font-size:10px;color:#475569}
.plan-badge{font-size:11px;font-weight:700;padding:3px 10px;border-radius:20px}
.btn-upgrade{background:linear-gradient(90deg,#7c3aed,#a78bfa);color:#fff;border:none;border-radius:10px;padding:7px 14px;font-size:12px;font-weight:700;cursor:pointer}
#main{padding-top:56px;padding-bottom:80px;min-height:100vh;overflow-y:auto}
.page{display:none;padding:16px}.page.active{display:block}
#bottomnav{position:fixed;bottom:0;left:0;right:0;z-index:200;background:#0a0f1eee;backdrop-filter:blur(12px);border-top:1px solid #1e293b;display:flex;padding:8px 0 env(safe-area-inset-bottom,8px)}
.nav-btn{flex:1;display:flex;flex-direction:column;align-items:center;gap:3px;background:none;border:none;cursor:pointer;padding:4px 0}
.nav-btn .nav-icon{font-size:20px}
.nav-btn .nav-label{font-size:10px;font-weight:600;color:#475569}
.nav-btn.active .nav-label{color:#a78bfa}
.card{background:#0f172a;border:1px solid #1e293b;border-radius:14px;overflow:hidden;margin-bottom:12px}
.card-header{padding:14px 16px;display:flex;justify-content:space-between;align-items:center;cursor:pointer}
.card-body{padding:0 16px 16px;display:none}.card-body.open{display:block}
.match-league{font-size:10px;color:#64748b;margin-bottom:3px}
.match-title{font-size:15px;font-weight:700}
.match-score{color:#f59e0b}
.odds-row{display:flex;gap:6px}
.odd-box{flex:1;background:#1e293b;border-radius:8px;padding:7px 0;text-align:center}
.odd-label{font-size:10px;color:#64748b}
.odd-val{font-size:14px;font-weight:700}
.prob-bar{display:flex;height:7px;border-radius:4px;overflow:hidden;gap:2px;margin:8px 0 4px}
.prob-labels{display:flex;justify-content:space-between;font-size:10px}
.inner-tabs{display:flex;gap:6px;margin:10px 0}
.inner-tab{flex:1;background:transparent;border:1px solid #1e293b;border-radius:7px;padding:6px 0;font-size:11px;font-weight:700;color:#64748b;cursor:pointer}
.inner-tab.active{background:#1e293b;color:#f1f5f9;border-color:#334155}
.h2h-row{display:flex;justify-content:space-between;align-items:center;background:#1e293b;border-radius:8px;padding:8px 12px;margin-bottom:6px}
.h2h-date{font-size:11px;color:#64748b}
.h2h-score{font-size:13px;font-weight:700}
.h2h-result{font-size:11px;font-weight:600;padding:2px 8px;border-radius:10px}
.ai-panel{background:linear-gradient(135deg,#1a1a2e,#16213e);border:1px solid #7c3aed;border-radius:12px;padding:16px;margin-top:8px}
.ai-title{color:#a78bfa;font-size:14px;font-weight:700;margin-bottom:10px}
.ai-output{color:#e2e8f0;font-size:13px;line-height:1.7;white-space:pre-wrap}
.btn-ai{background:linear-gradient(90deg,#7c3aed,#a78bfa);color:#fff;border:none;border-radius:8px;padding:9px 18px;font-size:13px;font-weight:700;cursor:pointer}
.btn-locked{background:#1e293b;color:#64748b;border:1px solid #334155;border-radius:8px;padding:9px 18px;font-size:13px;font-weight:700;cursor:pointer}
.spinner{display:inline-block;animation:spin 1s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}
.stats-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px;margin-bottom:20px}
.stat-box{background:#0f172a;border:1px solid #1e293b;border-radius:12px;padding:12px;text-align:center}
.stat-icon{font-size:18px;margin-bottom:4px}
.stat-val{font-size:15px;font-weight:800}
.stat-lbl{font-size:10px;color:#64748b;margin-top:2px}
.upcoming-card{background:#0f172a;border:1px solid #1e293b;border-radius:12px;padding:14px;margin-bottom:10px}
.btn-analyze{background:linear-gradient(90deg,#7c3aed,#a78bfa);color:#fff;border:none;border-radius:8px;padding:6px 14px;font-size:12px;font-weight:700;cursor:pointer;float:right;margin-top:-22px}
#pricing-modal{display:none;position:fixed;inset:0;background:#000c;z-index:500;overflow-y:auto;padding:20px 16px}
#pricing-modal.open{display:block}
.modal-box{background:#0f172a;border:1px solid #1e293b;border-radius:20px;padding:24px 20px;max-width:480px;margin:0 auto}
.plan-card{background:#1e293b;border:2px solid #334155;border-radius:14px;padding:20px;margin-bottom:12px;position:relative}
.plan-card.popular{border-color:#f59e0b}
.plan-card.elite{border-color:#8b5cf6}
.popular-badge{position:absolute;top:-11px;left:50%;transform:translateX(-50%);background:#f59e0b;color:#000;font-size:10px;font-weight:800;padding:2px 12px;border-radius:20px;white-space:nowrap}
.plan-feature{font-size:12px;color:#94a3b8;margin-bottom:6px;display:flex;gap:7px}
.plan-locked{font-size:12px;color:#475569;margin-bottom:6px;display:flex;gap:7px}
.btn-plan{width:100%;border-radius:10px;padding:10px 0;font-size:14px;font-weight:700;cursor:pointer;margin-top:14px;border:none}
#toast{position:fixed;bottom:90px;left:50%;transform:translateX(-50%) translateY(20px);background:#1e293b;color:#f1f5f9;padding:10px 20px;border-radius:20px;font-size:13px;font-weight:600;opacity:0;transition:all 0.3s;z-index:999;white-space:nowrap;border:1px solid #334155;pointer-events:none}
#toast.show{opacity:1;transform:translateX(-50%) translateY(0)}
.section-title{font-size:11px;color:#64748b;margin-bottom:10px;text-transform:uppercase;letter-spacing:.5px}
.cta-banner{background:linear-gradient(135deg,#1a1a2e,#16213e);border:1px solid #7c3aed;border-radius:14px;padding:16px;margin-top:20px;display:flex;justify-content:space-between;align-items:center;cursor:pointer}
</style>
</head>
<body>
<div id="header">
<div class="header-inner">
<div style="display:flex;align-items:center;gap:8px">
<span style="font-size:22px">⚽</span>
<div><div class="logo-text">Match<span>IQ</span></div><div class="logo-sub">Profesyonel Analiz</div></div>
</div>
<div style="display:flex;gap:8px;align-items:center">
<div id="plan-badge" class="plan-badge"></div>
<button class="btn-upgrade" onclick="openPricing()">⬆ Yükselt</button>
</div>
</div>
</div>

<div id="main">
<div id="page-gecmis" class="page active">
<div class="stats-grid">
<div class="stat-box"><div class="stat-icon">🎯</div><div class="stat-val">2.4K+</div><div class="stat-lbl">Analiz</div></div>
<div class="stat-box"><div class="stat-icon">📊</div><div class="stat-val">%71</div><div class="stat-lbl">Doğruluk</div></div>
<div class="stat-box"><div class="stat-icon">⚡</div><div class="stat-val">Aktif</div><div class="stat-lbl">AI Tahmin</div></div>
</div>
<div class="section-title">Son Maçlar — Detay için tıkla</div>
<div id="match-list"></div>
<div id="cta-free" class="cta-banner" onclick="openPricing()" style="display:none">
<div><div style="color:#a78bfa;font-weight:700">🚀 Pro'ya Geç</div><div style="font-size:11px;color:#64748b;margin-top:3px">AI analiz + oran karşılaştırma</div></div>
<div style="font-size:17px;font-weight:800">₺149/ay →</div>
</div>
</div>

<div id="page-upcoming" class="page">
<div class="section-title">Yaklaşan Maçlar</div>
<div id="analyze-panel" style="display:none"></div>
<div id="upcoming-list"></div>
</div>

<div id="page-uyelik" class="page">
<div style="text-align:center;margin-bottom:24px">
<div style="font-size:32px;margin-bottom:8px">👑</div>
<div style="font-size:20px;font-weight:800">Planını Seç</div>
<div style="font-size:13px;color:#64748b;margin-top:4px">Daha iyi analizler için yüksel</div>
</div>
<div id="pricing-cards"></div>
</div>

<div id="page-ayarlar" class="page">
<div style="text-align:center;padding:40px 0 20px">
<div style="font-size:48px;margin-bottom:12px">⚙️</div>
<div style="font-size:18px;font-weight:700;margin-bottom:6px">Ayarlar</div>
</div>
<div class="card" style="overflow:visible">
<div style="padding:16px">
<div style="font-size:11px;color:#64748b;margin-bottom:12px">Mevcut Plan</div>
<div style="display:flex;justify-content:space-between;align-items:center">
<div id="settings-plan-name" style="font-size:16px;font-weight:700"></div>
<button onclick="openPricing()" class="btn-upgrade">Plan Değiştir</button>
</div>
</div>
</div>
</div>
</div>

<div id="bottomnav">
<button class="nav-btn active" onclick="switchPage('gecmis',this)"><span class="nav-icon">📁</span><span class="nav-label">Geçmiş</span></button>
<button class="nav-btn" onclick="switchPage('upcoming',this)"><span class="nav-icon">🗓</span><span class="nav-label">Yaklaşan</span></button>
<button class="nav-btn" onclick="switchPage('uyelik',this)"><span class="nav-icon">👑</span><span class="nav-label">Üyelik</span></button>
<button class="nav-btn" onclick="switchPage('ayarlar',this)"><span class="nav-icon">⚙️</span><span class="nav-label">Ayarlar</span></button>
</div>

<div id="pricing-modal">
<div class="modal-box">
<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:20px">
<div style="font-size:20px;font-weight:800">Üyelik Planları</div>
<button onclick="closePricing()" style="background:#1e293b;color:#94a3b8;border:none;border-radius:8px;width:32px;height:32px;cursor:pointer;font-size:16px">✕</button>
</div>
<div id="modal-pricing-cards"></div>
</div>
</div>

<div id="toast"></div>

<script>
const MATCHES=[{id:1,home:"Galatasaray",away:"Fenerbahçe",league:"Süper Lig",date:"10 May 2025",score:"2-1",homeWin:42,draw:28,awayWin:30,odds:{home:2.1,draw:3.4,away:3.2},similarOdds:[{match:"Real Madrid vs Barcelona",date:"05 Nis 2025",result:"Ev Sahibi",score:"3-1"},{match:"Milan vs Inter",date:"22 Mar 2025",result:"Beraberlik",score:"1-1"}],h2h:[{date:"15 Ara 2024",score:"1-1",result:"Beraberlik"},{date:"20 May 2024",score:"3-0",result:"Galatasaray"},{date:"10 Ara 2023",score:"0-2",result:"Fenerbahçe"}]},{id:2,home:"Beşiktaş",away:"Trabzonspor",league:"Süper Lig",date:"08 May 2025",score:"0-0",homeWin:38,draw:32,awayWin:30,odds:{home:2.4,draw:3.1,away:2.9},similarOdds:[{match:"Juventus vs Napoli",date:"12 Nis 2025",result:"Beraberlik",score:"0-0"}],h2h:[{date:"22 Kas 2024",score:"2-1",result:"Beşiktaş"},{date:"05 Nis 2024",score:"1-3",result:"Trabzonspor"}]},{id:3,home:"Başakşehir",away:"Kayserispor",league:"Süper Lig",date:"06 May 2025",score:"3-1",homeWin:55,draw:22,awayWin:23,odds:{home:1.75,draw:3.8,away:4.5},similarOdds:[{match:"Bayern vs Bremen",date:"20 Nis 2025",result:"Ev Sahibi",score:"4-0"}],h2h:[{date:"14 Eki 2024",score:"2-0",result:"Başakşehir"},{date:"03 Mar 2024",score:"1-1",result:"Beraberlik"}]}];
const UPCOMING=[{id:10,home:"Galatasaray",away:"Beşiktaş",league:"Süper Lig",date:"15 Haz 2026",time:"21:00",odds:{home:2.2,draw:3.3,away:3.0}},{id:11,home:"Fenerbahçe",away:"Trabzonspor",league:"Süper Lig",date:"16 Haz 2026",time:"20:00",odds:{home:1.9,draw:3.5,away:3.8}},{id:12,home:"Real Madrid",away:"Atletico Madrid",league:"La Liga",date:"17 Haz 2026",time:"22:00",odds:{home:1.85,draw:3.6,away:4.0}}];
const PLANS=[{id:"free",name:"Ücretsiz",price:"₺0",period:"/ay",color:"#64748b",features:["5 maç analizi/gün","Geçmiş maçlar (7 gün)","Temel istatistikler"],locked:["AI Analiz","Oran Karşılaştırma","VIP Tahminler"]},{id:"pro",name:"Pro ⭐",price:"₺149",period:"/ay",color:"#f59e0b",popular:true,features:["Sınırsız analiz","Geçmiş maçlar (1 yıl)","AI Analiz","Oran Karşılaştırma"],locked:["VIP Tahminler","7/24 Destek"]},{id:"elite",name:"Elite 👑",price:"₺299",period:"/ay",color:"#8b5cf6",features:["Her şey Pro'da +","VIP Tahminler","7/24 Destek","Kişisel rapor"],locked:[]}];
let userPlan="free",openCards={},cardTabs={},aiResults={},aiLoading={};
window.onload=()=>{renderMatches();renderUpcoming();renderPricingCards("pricing-cards");renderPricingCards("modal-pricing-cards");updatePlanUI()};
function switchPage(n,btn){document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));document.getElementById("page-"+n).classList.add("active");document.querySelectorAll(".nav-btn").forEach(b=>b.classList.remove("active"));btn.classList.add("active");document.getElementById("main").scrollTop=0}
function openPricing(){document.getElementById("pricing-modal").classList.add("open")}
function closePricing(){document.getElementById("pricing-modal").classList.remove("open")}
function selectPlan(id){userPlan=id;updatePlanUI();closePricing();renderMatches();renderPricingCards("pricing-cards");renderPricingCards("modal-pricing-cards");showToast("✅ "+PLANS.find(x=>x.id===id).name+" planına geçildi!")}
function updatePlanUI(){const p=PLANS.find(x=>x.id===userPlan);const b=document.getElementById("plan-badge");b.textContent=p.name;b.style.color=p.color;b.style.background=p.color+"18";b.style.border="1px solid "+p.color+"44";document.getElementById("settings-plan-name").textContent=p.name;document.getElementById("settings-plan-name").style.color=p.color;const c=document.getElementById("cta-free");if(c)c.style.display=userPlan==="free"?"flex":"none"}
function renderPricingCards(cid){const el=document.getElementById(cid);if(!el)return;el.innerHTML=PLANS.map(p=>`<div class="plan-card ${p.popular?'popular':''} ${p.id==='elite'?'elite':''}">${p.popular?'<div class="popular-badge">EN POPÜLER</div>':''}<div style="font-size:16px;font-weight:800;color:${p.color};margin-bottom:4px">${p.name}</div><div style="margin:8px 0 14px"><span style="font-size:26px;font-weight:800">${p.price}</span><span style="font-size:12px;color:#64748b">${p.period}</span></div>${p.features.map(f=>`<div class="plan-feature"><span style="color:#34d399">✓</span>${f}</div>`).join("")}${p.locked.map(f=>`<div class="plan-locked"><span>✕</span>${f}</div>`).join("")}<button class="btn-plan" onclick="selectPlan('${p.id}')" style="background:${p.id==='free'?'#334155':'linear-gradient(90deg,'+p.color+','+p.color+'bb)'};color:${p.id==='free'?'#94a3b8':'#fff'}">${p.id===userPlan?"Mevcut Plan ✓":"Seç"}</button></div>`).join("")}
function renderMatches(){document.getElementById("match-list").innerHTML=MATCHES.map(m=>matchCardHTML(m)).join("")}
function matchCardHTML(m){const isOpen=openCards[m.id];const tab=cardTabs[m.id]||"h2h";const aiResult=aiResults[m.id];const isLoading=aiLoading[m.id];return`<div class="card"><div class="card-header" onclick="toggleCard(${m.id})"><div><div class="match-league">${m.league} • ${m.date}</div><div class="match-title">${m.home} <span class="match-score">${m.score}</span> ${m.away}</div></div><div style="display:flex;gap:6px;align-items:center"><div class="odd-box" style="min-width:36px"><div class="odd-label">1</div><div class="odd-val" style="color:#34d399">${m.odds.home}</div></div><div class="odd-box" style="min-width:36px"><div class="odd-label">X</div><div class="odd-val" style="color:#fbbf24">${m.odds.draw}</div></div><div class="odd-box" style="min-width:36px"><div class="odd-label">2</div><div class="odd-val" style="color:#f87171">${m.odds.away}</div></div><span style="color:${isOpen?'#a78bfa':'#475569'};font-size:16px;margin-left:4px">${isOpen?'▲':'▼'}</span></div></div><div class="card-body ${isOpen?'open':''}"><div style="font-size:10px;color:#64748b;margin-bottom:5px">Kazanma Olasılığı</div><div class="prob-bar"><div style="width:${m.homeWin}%;background:#34d399;border-radius:4px 0 0 4px"></div><div style="width:${m.draw}%;background:#fbbf24"></div><div style="width:${m.awayWin}%;background:#f87171;border-radius:0 4px 4px 0"></div></div><div class="prob-labels"><span style="color:#34d399">${m.home} %${m.homeWin}</span><span style="color:#fbbf24">Ber. %${m.draw}</span><span style="color:#f87171">${m.away} %${m.awayWin}</span></div><div class="inner-tabs"><button class="inner-tab ${tab==='h2h'?'active':''}" onclick="setTab(${m.id},'h2h')">H2H</button><button class="inner-tab ${tab==='odds'?'active':''}" onclick="setTab(${m.id},'odds')">${userPlan==='free'?'🔒 Oranlar':'Oranlar'}</button><button class="inner-tab ${tab==='ai'?'active':''}" onclick="setTab(${m.id},'ai')">🤖 AI</button></div>${tab==='h2h'?`<div>${m.h2h.map(h=>`<div class="h2h-row"><span class="h2h-date">${h.date}</span><span class="h2h-score">${h.score}</span><span class="h2h-result" style="color:${h.result==='Beraberlik'?'#fbbf24':'#34d399'};background:${h.result==='Beraberlik'?'#fbbf2415':'#34d39915'}">${h.result}</span></div>`).join("")}</div>`:''}${tab==='odds'?(userPlan==='free'?`<div style="text-align:center;padding:20px"><div style="font-size:32px;margin-bottom:8px">🔒</div><div style="font-size:14px;font-weight:700;margin-bottom:6px">Pro Özelliği</div><button class="btn-ai" onclick="openPricing()">⬆ Pro'ya Geç</button></div>`:`<div>${m.similarOdds.map(s=>`<div class="h2h-row"><div><div style="font-size:13px;font-weight:600">${s.match}</div><div style="font-size:11px;color:#64748b">${s.date}</div></div><div style="text-align:right"><div style="font-weight:700">${s.score}</div><div style="font-size:11px;color:#a78bfa">${s.result}</div></div></div>`).join("")}</div>`):''}${tab==='ai'?`<div class="ai-panel"><div class="ai-title">🤖 Yapay Zeka Analizi</div>${isLoading?`<div style="color:#a78bfa;font-size:13px"><span class="spinner">⚙️</span> Analiz ediliyor...</div>`:aiResult?`<div class="ai-output">${aiResult}</div>`:userPlan==='free'?`<button class="btn-locked" onclick="openPricing()">🔒 Pro'ya Geç</button>`:`<button class="btn-ai" onclick="runAI(${m.id})">⚡ Analizi Başlat</button>`}</div>`:''}></div></div>`}
function toggleCard(id){openCards[id]=!openCards[id];renderMatches()}
function setTab(id,tab){cardTabs[id]=tab;renderMatches()}
async function runAI(matchId){const m=MATCHES.find(x=>x.id===matchId);if(!m)return;aiLoading[matchId]=true;renderMatches();try{const res=await fetch("https://api.anthropic.com/v1/messages",{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify({model:"claude-sonnet-4-6",max_tokens:1000,messages:[{role:"user",content:`Profesyonel futbol analisti olarak analiz et:\nMaç: ${m.home} vs ${m.away}\nLig: ${m.league}\nOranlar: Ev ${m.odds.home} | X ${m.odds.draw} | Dep ${m.odds.away}\nEv Kazanma: %${m.homeWin} | Beraberlik: %${m.draw} | Dep: %${m.awayWin}\nH2H: ${m.h2h.map(h=>h.score+' ('+h.result+')').join(', ')}\n\n1. Form analizi\n2. Oran değerlendirmesi\n3. Senaryolar\n4. Tahmin + güven skoru\nKısa Türkçe yaz.`}]})});const data=await res.json();aiResults[matchId]=data.content?.map(b=>b.text||"").join("")||"Analiz alınamadı."}catch{aiResults[matchId]="⚠️ Hata oluştu."}aiLoading[matchId]=false;renderMatches()}
function renderUpcoming(){document.getElementById("upcoming-list").innerHTML=UPCOMING.map(m=>`<div class="upcoming-card"><div style="display:flex;justify-content:space-between;align-items:flex-start"><div><div style="font-size:10px;color:#64748b;margin-bottom:4px">${m.league} • ${m.date} ${m.time}</div><div style="font-size:14px;font-weight:700;margin-bottom:10px">${m.home} vs ${m.away}</div></div><button class="btn-analyze" onclick="analyzeUpcoming(${m.id})">Analiz Et</button></div><div class="odds-row"><div class="odd-box"><div class="odd-label">1</div><div class="odd-val" style="color:#34d399">${m.odds.home}</div></div><div class="odd-box"><div class="odd-label">X</div><div class="odd-val" style="color:#fbbf24">${m.odds.draw}</div></div><div class="odd-box"><div class="odd-label">2</div><div class="odd-val" style="color:#f87171">${m.odds.away}</div></div></div></div>`).join("")}
function analyzeUpcoming(id){if(userPlan==="free"){openPricing();return}const m=UPCOMING.find(x=>x.id===id);const p=document.getElementById("analyze-panel");p.style.display="block";p.innerHTML=`<div style="background:#0f172a;border:1px solid #7c3aed;border-radius:14px;padding:16px;margin-bottom:14px"><div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px"><div style="font-size:15px;font-weight:700">${m.home} vs ${m.away}</div><button onclick="document.getElementById('analyze-panel').style.display='none'" style="background:none;border:none;color:#64748b;cursor:pointer;font-size:18px">✕</button></div><div class="ai-panel"><div class="ai-title">🤖 AI Analizi</div><button class="btn-ai" onclick="runUpcomingAI(${id})">⚡ Başlat</button></div></div>`}
async function runUpcomingAI(id){const m=UPCOMING.find(x=>x.id===id);const p=document.getElementById("analyze-panel").querySelector(".ai-panel");p.innerHTML=`<div class="ai-title">🤖 AI Analizi</div><div style="color:#a78bfa;font-size:13px"><span class="spinner">⚙️</span> Analiz ediliyor...</div>`;try{const res=await fetch("https://api.anthropic.com/v1/messages",{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify({model:"claude-sonnet-4-6",max_tokens:1000,messages:[{role:"user",content:`Yaklaşan maç analizi:\n${m.home} vs ${m.away} | ${m.league} | ${m.date} ${m.time}\nOranlar: Ev ${m.odds.home} | X ${m.odds.draw} | Dep ${m.odds.away}\nKısa Türkçe analiz yap, tahmin ver.`}]})});const data=await res.json();p.innerHTML=`<div class="ai-title">🤖 AI Analizi</div><div class="ai-output">${data.content?.map(b=>b.text||"").join("")||"Analiz alınamadı."}</div>`}catch{p.innerHTML=`<div class="ai-title">🤖 AI Analizi</div><div style="color:#f87171">⚠️ Hata oluştu.</div>`}}
function showToast(msg){const t=document.getElementById("toast");t.textContent=msg;t.classList.add("show");setTimeout(()=>t.classList.remove("show"),2500)}
</script>
</body>
</html>
