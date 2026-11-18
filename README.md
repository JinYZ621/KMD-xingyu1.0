<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#050b17">
<title>星域回响：边境协议 | KMD 卡米达</title>
<style>
:root{
 --bg:#050b17;--panel:#0b1628;--panel2:#101f35;--line:rgba(112,174,255,.18);
 --blue:#61b5ff;--cyan:#69e2ef;--text:#e9f3ff;--muted:#91a7c2;
 --gold:#f5c76a;--red:#ff7185;--green:#73e0b0;
}
*{box-sizing:border-box}
body{margin:0;background:radial-gradient(ellipse at 50% -20%,#142e50 0%,#071224 42%,#040914 100%);color:var(--text);font-family:Inter,"Microsoft YaHei",Arial,sans-serif;min-height:100vh}
button{font:inherit;cursor:pointer}
button:disabled{opacity:.4;cursor:not-allowed}
.app{max-width:1500px;margin:auto;padding:18px}
.topbar{display:flex;justify-content:space-between;align-items:center;gap:16px;padding:14px 18px;background:rgba(7,17,32,.86);border:1px solid var(--line);border-radius:14px;box-shadow:0 12px 50px #0005;position:sticky;top:8px;z-index:20;backdrop-filter:blur(14px)}
.brand{display:flex;align-items:center;gap:12px}.emblem{width:42px;height:42px;border:1px solid var(--blue);display:grid;place-items:center;color:var(--blue);font-weight:900;transform:rotate(45deg);box-shadow:0 0 22px #348bdf55}.emblem span{transform:rotate(-45deg)}
.brand h1{font-size:15px;letter-spacing:2px;margin:0}.brand small{display:block;color:var(--muted);font-size:10px;letter-spacing:2px;margin-top:4px}
.top-actions{display:flex;align-items:center;gap:8px;flex-wrap:wrap}.resource{padding:8px 11px;background:#0c1b30;border:1px solid var(--line);border-radius:8px;font-size:12px;white-space:nowrap}.resource b{color:var(--cyan);margin-left:5px}
.layout{display:grid;grid-template-columns:220px minmax(0,1fr);gap:16px;margin-top:16px}
.sidebar,.mainpanel,.rightpanel{background:rgba(8,19,36,.92);border:1px solid var(--line);border-radius:14px}
.sidebar{padding:14px;height:max-content;position:sticky;top:100px}
.navbtn{width:100%;border:1px solid transparent;background:transparent;color:var(--muted);padding:13px 12px;text-align:left;border-radius:8px;margin:3px 0;transition:.2s}
.navbtn:hover,.navbtn.active{color:var(--text);background:#112744;border-color:#2d6598}
.navbtn span{display:inline-block;width:26px;color:var(--blue)}
.side-note{margin:20px 5px 5px;padding:12px;border-top:1px solid var(--line);color:var(--muted);font-size:11px;line-height:1.7}
.mainpanel{padding:20px;min-height:720px;overflow:hidden}
.view{display:none}.view.active{display:block}
.eyebrow{color:var(--blue);font-size:10px;letter-spacing:3px;font-weight:700}
h2{font-size:27px;margin:9px 0 7px;letter-spacing:1px}
.subtitle{color:var(--muted);font-size:13px;line-height:1.8;margin:0 0 20px}
.hero{min-height:220px;padding:24px;border:1px solid var(--line);border-radius:12px;background:linear-gradient(105deg,#0d2441dd,#0b1729aa),radial-gradient(circle at 85% 50%,#1c6593,#071324 50%);position:relative;overflow:hidden}
.hero:after{content:"";position:absolute;width:280px;height:280px;border:1px solid #75c5ff44;border-radius:50%;right:7%;top:-35px;box-shadow:0 0 0 30px #6abfff0c,0 0 0 65px #6abfff09}
.hero h2{font-size:clamp(26px,4vw,42px);position:relative;z-index:1}.hero p{max-width:560px;color:#b6c9df;line-height:1.9;font-size:13px;position:relative;z-index:1}
.hero .btn{position:relative;z-index:2}
.btn{border:1px solid #3d85bd;background:#123456;color:#dff2ff;padding:10px 14px;border-radius:7px;font-size:12px;transition:.2s}
.btn:hover{background:#1a4b77;border-color:var(--blue);box-shadow:0 0 18px #348bdf33}
.btn.primary{background:linear-gradient(120deg,#267ec1,#1b5b91);border-color:#69baff}
.btn.danger{border-color:#8d4054;background:#391b2b;color:#ffc5cf}
.btn.gold{border-color:#8d7542;background:#392f1b;color:#ffe3a1}
.btn.ghost{background:transparent}
.btnrow{display:flex;gap:8px;flex-wrap:wrap;margin-top:12px}
.grid{display:grid;gap:12px}.grid.two{grid-template-columns:repeat(2,minmax(0,1fr))}.grid.three{grid-template-columns:repeat(3,minmax(0,1fr))}.grid.four{grid-template-columns:repeat(4,minmax(0,1fr))}
.card{padding:15px;background:linear-gradient(145deg,#0d1d32,#091426);border:1px solid var(--line);border-radius:11px;min-width:0}
.card h3{font-size:14px;margin:0 0 8px}.card p{font-size:12px;color:var(--muted);line-height:1.75;margin:5px 0}
.stat{font-size:24px;font-weight:800;color:var(--cyan);margin:6px 0}.stat small{font-size:11px;color:var(--muted);font-weight:400}
.sectionhead{display:flex;justify-content:space-between;align-items:end;gap:10px;margin:22px 0 11px}.sectionhead h3{margin:0;font-size:15px}.sectionhead small{color:var(--muted);font-size:11px}
.progress{height:7px;background:#172840;border-radius:10px;overflow:hidden;margin:8px 0}.progress i{display:block;height:100%;background:linear-gradient(90deg,#278bce,#72d9e8);border-radius:10px}
.meter{display:flex;justify-content:space-between;color:var(--muted);font-size:11px}
.map{position:relative;min-height:380px;border:1px solid var(--line);border-radius:12px;overflow:hidden;background:radial-gradient(ellipse at 50% 45%,#16345a 0%,#0a1b32 45%,#06101e 100%)}
.map:before{content:"";position:absolute;inset:0;background-image:radial-gradient(#b8dcff 1px,transparent 1px),radial-gradient(#7dbbff .7px,transparent .7px);background-size:83px 83px,43px 43px;background-position:10px 8px,23px 17px;opacity:.35}
.mapline{position:absolute;height:1px;background:linear-gradient(90deg,transparent,#5aaeea88,transparent);transform-origin:left center}
.system{position:absolute;z-index:2;transform:translate(-50%,-50%);border:0;background:transparent;color:white;text-align:center;min-width:76px}
.planet{width:34px;height:34px;margin:0 auto 7px;border-radius:50%;display:block;border:1px solid #b8e2ff;box-shadow:0 0 20px #54baff88;background:radial-gradient(circle at 30% 25%,#d3f4ff,#3889c8 45%,#102b53 75%)}
.system.unknown .planet{background:radial-gradient(circle at 30% 25%,#f6e1ad,#a36a43 48%,#34203b 78%);border-color:#e9c68a}
.system.locked{opacity:.35;filter:grayscale(1)}
.system.selected .planet{outline:2px solid var(--gold);outline-offset:4px}
.system b{font-size:11px;text-shadow:0 2px 5px #000}
.system small{display:block;color:#9eb6d1;font-size:9px;margin-top:3px}
.log{max-height:230px;overflow:auto;font-size:11px;line-height:1.8;color:#a9bdd3}
.log div{padding:6px 0;border-bottom:1px solid #ffffff0b}
.tag{display:inline-block;border:1px solid var(--line);border-radius:20px;padding:4px 8px;font-size:10px;color:#9dcfff}
.good{color:var(--green)}.warn{color:var(--gold)}.bad{color:var(--red)}
.listrow{display:flex;justify-content:space-between;align-items:center;gap:12px;padding:11px 0;border-bottom:1px solid #ffffff10}
.listrow:last-child{border:0}.listrow strong{font-size:12px}.listrow small{display:block;color:var(--muted);font-size:10px;margin-top:4px}
.bigicon{font-size:28px;margin-bottom:8px}
.footer{text-align:center;color:#61758f;font-size:10px;padding:18px}
.toast{position:fixed;right:20px;bottom:20px;z-index:50;padding:12px 16px;background:#102945;border:1px solid #4c9bd4;border-radius:9px;color:#e9f5ff;font-size:12px;box-shadow:0 10px 35px #0008;opacity:0;transform:translateY(10px);transition:.25s;pointer-events:none}
.toast.show{opacity:1;transform:translateY(0)}
.modalback{position:fixed;inset:0;background:#020711bb;z-index:40;display:none;place-items:center;padding:15px}
.modalback.show{display:grid}.modal{max-width:500px;width:100%;padding:22px;background:#0b192c;border:1px solid #4b8cbd;border-radius:14px;box-shadow:0 20px 80px #000b}
.modal h3{margin:0 0 10px}.modal p{font-size:13px;color:var(--muted);line-height:1.8}
@media(max-width:950px){.layout{grid-template-columns:1fr}.sidebar{position:static;display:flex;overflow:auto;gap:5px;padding:8px}.navbtn{white-space:nowrap;width:auto;margin:0;padding:10px}.side-note{display:none}.grid.four{grid-template-columns:repeat(2,1fr)}}
@media(max-width:600px){.app{padding:8px}.topbar{padding:10px;align-items:flex-start;flex-direction:column}.top-actions{width:100%;display:grid;grid-template-columns:repeat(3,1fr)}.resource{text-align:center;padding:7px 3px;font-size:10px}.mainpanel{padding:13px;min-height:600px}.grid.two,.grid.three{grid-template-columns:1fr}.hero{padding:18px}.hero:after{right:-120px}.sectionhead{align-items:flex-start;flex-direction:column}.map{min-height:340px}.system{min-width:60px}.planet{width:28px;height:28px}.system b{font-size:9px}}

</style>
</head>
<body>
<div class="app">
<header class="topbar">
 <div class="brand"><div class="emblem"><span>K</span></div><div><h1>KMD · 星域回响</h1><small>边境协议 / FRONTIER PROTOCOL</small></div></div>
 <div class="top-actions">
  <div class="resource">⚡ 能源<b id="res-energy">1200</b></div>
  <div class="resource">▰ 合金<b id="res-alloy">850</b></div>
  <div class="resource">◆ 晶体<b id="res-crystal">300</b></div>
  <div class="resource">◈ 研究<b id="res-data">0</b></div>
  <button class="btn ghost" onclick="resetGame()" title="清空本地存档">重置存档</button>
 </div>
</header>
<div class="layout">
<aside class="sidebar">
 <button class="navbtn active" data-view="command"><span>⌂</span>指挥中心</button>
 <button class="navbtn" data-view="map"><span>◎</span>星域地图</button>
 <button class="navbtn" data-view="fleet"><span>◈</span>舰队管理</button>
 <button class="navbtn" data-view="research"><span>⌬</span>科技研究</button>
 <button class="navbtn" data-view="missions"><span>⚑</span>任务档案</button>
 <button class="navbtn" data-view="battle"><span>⚔</span>战术模拟</button>
 <button class="navbtn" data-view="codex"><span>▤</span>星际档案</button>
 <div class="side-note">KMD GAME LAB<br>单机策略经营 Demo<br><br>自动本地存档已启用</div>
</aside>
<main class="mainpanel">
<section class="view active" id="view-command">
 <div class="hero">
  <div class="eyebrow">COMMAND DECK / 远征舰队第七支队</div>
  <h2>边境指挥中心</h2>
  <p>指挥官，欢迎回到「深空之眼」。边境星域出现异常信号，舰队需要补给、扩张与科技支援。你的每一个决策，都将改变远征进程。</p>
  <div class="btnrow"><button class="btn primary" onclick="showView('map')">打开星域地图 →</button><button class="btn" onclick="claimDaily()">领取补给</button></div>
 </div>
 <div class="sectionhead"><h3>舰队状态</h3><small id="date-label">远征纪元 01 · 第 1 日</small></div>
 <div class="grid four">
  <div class="card"><div class="eyebrow">舰队战力</div><div class="stat" id="power-stat">120 <small>PT</small></div><p>当前综合作战能力</p></div>
  <div class="card"><div class="eyebrow">控制星域</div><div class="stat" id="systems-stat">1 <small>/ 6</small></div><p>已建立前哨站的星系</p></div>
  <div class="card"><div class="eyebrow">舰员士气</div><div class="stat" id="morale-stat">86<small>%</small></div><div class="progress"><i id="morale-bar" style="width:86%"></i></div></div>
  <div class="card"><div class="eyebrow">科研进度</div><div class="stat" id="research-stat">0<small> RP</small></div><p>累计获得研究数据</p></div>
 </div>
 <div class="grid two" style="margin-top:13px">
  <div class="card"><div class="sectionhead"><h3>当前行动</h3><span class="tag">优先事项</span></div><h3 id="objective-title">调查阿尔法信号</h3><p id="objective-text">前往阿尔法星系，调查异常信号源。</p><div class="btnrow"><button class="btn primary" onclick="showView('missions')">查看任务</button></div></div>
  <div class="card"><div class="sectionhead"><h3>舰队简报</h3><small>实时记录</small></div><div class="log" id="command-log"></div></div>
 </div>
</section>

<section class="view" id="view-map">
 <div class="eyebrow">SECTOR NAVIGATION</div><h2>边境星域地图</h2><p class="subtitle">选择星系查看情报。探索需要消耗能源，部分星系存在未知风险。</p>
 <div class="map" id="star-map">
  <div class="mapline" style="left:20%;top:30%;width:40%;transform:rotate(17deg)"></div>
  <div class="mapline" style="left:47%;top:40%;width:34%;transform:rotate(-28deg)"></div>
  <div class="mapline" style="left:20%;top:30%;width:55%;transform:rotate(38deg)"></div>
  <button class="system selected" style="left:20%;top:30%" data-system="sol"><i class="planet"></i><b>索尔</b><small>母星系</small></button>
  <button class="system unknown" style="left:48%;top:43%" data-system="alpha"><i class="planet"></i><b>阿尔法</b><small>信号源</small></button>
  <button class="system locked" style="left:78%;top:22%" data-system="vega"><i class="planet"></i><b>织女星</b><small>未探索</small></button>
  <button class="system locked" style="left:76%;top:72%" data-system="orion"><i class="planet"></i><b>猎户座</b><small>未探索</small></button>
  <button class="system locked" style="left:34%;top:78%" data-system="nyx"><i class="planet"></i><b>尼克斯</b><small>未探索</small></button>
  <button class="system locked" style="left:52%;top:13%" data-system="frontier"><i class="planet"></i><b>边界站</b><small>未探索</small></button>
 </div>
 <div class="grid two" style="margin-top:13px">
  <div class="card" id="system-info"><div class="eyebrow">SYSTEM INTEL</div><h3>索尔星系</h3><p>人类远征舰队的初始基地。拥有稳定的资源补给与基础设施。</p><span class="tag good">已控制</span></div>
  <div class="card"><div class="eyebrow">航行指令</div><h3 id="selected-name">索尔星系</h3><p id="selected-desc">选择一个星系以查看详情。</p><div class="btnrow"><button class="btn primary" id="explore-btn" onclick="exploreSelected()">派遣探索 · 80 能源</button><button class="btn" onclick="showView('missions')">任务档案</button></div></div>
 </div>
</section>

<section class="view" id="view-fleet">
 <div class="eyebrow">FLEET COMMAND</div><h2>舰队管理</h2><p class="subtitle">维护旗舰与护航舰队。升级舰船可提升探索效率和战斗能力。</p>
 <div class="grid two">
  <div class="card"><div class="bigicon">🚀</div><div class="eyebrow">FLAGSHIP / 旗舰</div><h3>深空之眼</h3><p>第七远征舰队旗舰，具备远程探索与战术指挥能力。</p><div class="meter"><span>船体耐久</span><b id="hull-label">100 / 100</b></div><div class="progress"><i id="hull-bar" style="width:100%"></i></div><div class="meter"><span>护盾能量</span><b id="shield-label">80 / 80</b></div><div class="progress"><i id="shield-bar" style="width:100%"></i></div></div>
  
  <div class="card"><div class="eyebrow">FLEET OVERVIEW</div><h3>舰队编制</h3><div class="listrow"><div><strong>深空之眼 · 指挥舰</strong><small>旗舰 / 远征指挥</small></div><span class="tag good">在线</span></div><div class="listrow"><div><strong>曙光 · 护卫舰</strong><small>护航 / 防御</small></div><span class="tag good">在线</span></div><div class="listrow"><div><strong>游隼 · 侦察艇</strong><small>侦察 / 资源采集</small></div><span class="tag good">在线</span></div><div class="listrow"><div><strong>舰员</strong><small>当前编制</small></div><b id="crew-label">24 / 30</b></div></div>
 </div>
 <div class="sectionhead"><h3>舰船升级</h3><small>消耗资源强化舰队</small></div>
 <div class="grid three">
  <div class="card"><h3>强化装甲</h3><p>提升旗舰船体耐久上限。</p><div class="tag">合金 120 · 能源 80</div><div class="btnrow"><button class="btn" onclick="upgrade('hull')">升级装甲</button></div></div>
  <div class="card"><h3>扩容护盾</h3><p>提升护盾容量与生存能力。</p><div class="tag">合金 100 · 晶体 50</div><div class="btnrow"><button class="btn" onclick="upgrade('shield')">升级护盾</button></div></div>
  <div class="card"><h3>武器系统</h3><p>提高舰队综合战斗力。</p><div class="tag">合金 160 · 晶体 35</div><div class="btnrow"><button class="btn" onclick="upgrade('weapon')">升级武器</button></div></div>
 </div>
 <div class="sectionhead"><h3>舰队维护</h3></div>
 <div class="card"><p>消耗能源与合金维修旗舰，恢复受损船体。</p><div class="btnrow"><button class="btn primary" onclick="repairFleet()">维修舰队 · 90 能源 / 60 合金</button></div></div>
</section>

<section class="view" id="view-research">
 <div class="eyebrow">TECHNOLOGY DIVISION</div><h2>科技研究</h2><p class="subtitle">研究新技术，解锁更高效的资源生产、舰队能力与星际探索。</p>
 <div class="grid three">
  <div class="card"><div class="bigicon">⚙</div><h3>高效能源核心</h3><p>提升能源采集效率。</p><div class="tag">研究数据 30</div><div class="btnrow"><button class="btn" onclick="research('energy')">开始研究</button></div><small id="tech-energy" class="good"></small></div>
  <div class="card"><div class="bigicon">🛡</div><h3>相位护盾</h3><p>强化旗舰防护系统。</p><div class="tag">研究数据 45</div><div class="btnrow"><button class="btn" onclick="research('shield')">开始研究</button></div><small id="tech-shield" class="good"></small></div>
  <div class="card"><div class="bigicon">⌖</div><h3>深空扫描阵列</h3><p>降低探索成本，发现更多资源。</p><div class="tag">研究数据 40</div><div class="btnrow"><button class="btn" onclick="research('scan')">开始研究</button></div><small id="tech-scan" class="good"></small></div>
 </div>
 <div class="sectionhead"><h3>科研中心</h3><small>研究数据由探索与任务获得</small></div>
 <div class="card"><div class="meter"><span>累计研究数据</span><b id="data-total">0 RP</b></div><div class="progress"><i id="data-bar" style="width:0%"></i></div><p>继续探索星系、完成任务以获取研究数据。</p></div>
</section>

<section class="view" id="view-missions">
 <div class="eyebrow">MISSION CONTROL</div><h2>任务档案</h2><p class="subtitle">完成远征任务获得资源、研究数据与星域控制权。</p>
 <div class="card"><div class="eyebrow">MAIN OPERATION / 主线行动</div><h3 id="main-mission-title">第一章：来自阿尔法的信号</h3><p id="main-mission-desc">派遣舰队前往阿尔法星系，查明异常信号来源。</p><div class="progress"><i id="mission-progress" style="width:0%"></i></div><div class="meter"><span>任务进度</span><b id="mission-progress-label">0 / 3</b></div></div>
 <div class="sectionhead"><h3>可执行行动</h3></div>
 <div class="grid two">
  <div class="card"><h3>日常补给巡航</h3><p>整理航线并回收沿途漂浮物资。</p><span class="tag">奖励：能源 +120 / 合金 +60</span><div class="btnrow"><button class="btn" onclick="runMission('supply')">执行巡航</button></div></div>
  <div class="card"><h3>残骸信号分析</h3><p>分析已发现的未知信号，获得科研数据。</p><span class="tag">奖励：研究数据 +18</span><div class="btnrow"><button class="btn" onclick="runMission('data')">分析信号</button></div></div>
 </div>
 <div class="sectionhead"><h3>行动记录</h3></div><div class="card log" id="mission-log"></div>
</section>

<section class="view" id="view-battle">
 <div class="eyebrow">TACTICAL SIMULATION</div><h2>战术模拟室</h2><p class="subtitle">挑战边境海盗训练舰。战斗采用回合制，胜利可获得资源与研究数据。</p>
 <div class="grid two">
  <div class="card"><div class="eyebrow">YOUR FLAGSHIP</div><div class="bigicon">🚀</div><h3>深空之眼</h3><div class="meter"><span>船体</span><b id="battle-hull">100</b></div><div class="progress"><i id="battle-hull-bar" style="width:100%"></i></div><div class="meter"><span>护盾</span><b id="battle-shield">80</b></div><div class="progress"><i id="battle-shield-bar" style="width:100%"></i></div></div>
  <div class="card"><div class="eyebrow">HOSTILE TARGET</div><div class="bigicon">☠</div><h3>边境掠夺者</h3><div class="meter"><span>敌舰耐久</span><b id="enemy-hp">100</b></div><div class="progress"><i id="enemy-bar" style="width:100%"></i></div><p id="battle-status">模拟战尚未开始。</p></div>
 </div>
 <div class="card" style="margin-top:13px"><div class="sectionhead"><h3>战术指令</h3><span class="tag">每回合行动一次</span></div><div class="btnrow"><button class="btn primary" id="battle-start" onclick="startBattle()">开始模拟战</button><button class="btn" id="attack-btn" onclick="battleAction('attack')" disabled>主炮攻击</button><button class="btn" id="defend-btn" onclick="battleAction('defend')" disabled>护盾防御</button><button class="btn gold" id="special-btn" onclick="battleAction('special')" disabled>超载齐射</button></div><div class="log" id="battle-log" style="margin-top:12px"></div></div>
</section>

<section class="view" id="view-codex">
 <div class="eyebrow">ARCHIVE / DATABASE</div><h2>星际档案</h2><p class="subtitle">远征舰队数据库，记录已知星域、舰船与关键事件。</p>
 <div class="grid two">
  <div class="card"><div class="bigicon">🌌</div><h3>零点坍缩</h3><p>十二年前，银河边境发生大规模空间异常。多个殖民星系与中央网络失去联系，事件原因至今未知。</p></div>
  <div class="card"><div class="bigicon">🛰</div><h3>第七远征舰队</h3><p>由旗舰「深空之眼」统领，负责重建边境通讯、调查异常信号并建立安全航线。</p></div>
  <div class="card"><div class="bigicon">🔷</div><h3>未知晶体</h3><p>一种在边境星域发现的高能物质，可用于高级护盾与深空设备研发。</p></div>
  <div class="card"><div class="bigicon">⚠</div><h3>边境掠夺者</h3><p>活跃于失联星域的武装势力，常袭击运输船与无人采集设施。</p></div>
 </div>
 <div class="sectionhead"><h3>已知星系</h3></div><div class="card" id="codex-systems"></div>
</section>
</main>
</div>
<div class="footer">KMD 卡米达 · 星域回响：边境协议 · 虚构原创单机网页游戏 Demo · 游戏进度保存在当前浏览器</div>
</div>
<div class="toast" id="toast"></div>
<div class="modalback" id="modalback" onclick="if(event.target===this)closeModal()"><div class="modal"><h3 id="modal-title">提示</h3><p id="modal-text"></p><div class="btnrow"><button class="btn primary" onclick="closeModal()">确认</button></div></div></div>
<script>
const SAVE_KEY="kmd_star_echo_save_v1";
const initialState={
 day:1,energy:1200,alloy:850,crystal:300,data:0,morale:86,
 hull:100,maxHull:100,shield:80,maxShield:80,weapon:1,power:120,crew:24,
 selected:"sol",explored:["sol"],controlled:["sol"],missionStep:0,
 upgrades:{hull:0,shield:0,weapon:0},tech:{energy:false,shield:false,scan:false},
 dailyClaimed:false,logs:["远征舰队抵达索尔星系。","指挥系统上线，等待指令。"],
 missionLogs:["第一章：来自阿尔法的信号已建立。"],

 battle:{active:false,enemy:100,enemyMax:100,defending:false,won:false}
};
let state=loadGame();
let selectedSystem="sol";
let toastTimer;
const systems={
 sol:{name:"索尔星系",desc:"人类远征舰队的初始基地。拥有稳定的资源补给与基础设施。",cost:0,reward:null},
 alpha:{name:"阿尔法星系",desc:"异常信号的发源地。扫描显示存在古老设施与不明能量反应。",cost:80,reward:{energy:180,alloy:100,crystal:45,data:25}},
 vega:{name:"织女星系",desc:"富含高纯度合金的小行星带，可能存在自动采矿设施。",cost:110,reward:{energy:120,alloy:220,crystal:20,data:15}},
 orion:{name:"猎户座星云",desc:"浓密星云遮蔽了传感器，未知舰船活动频繁。",cost:130,reward:{energy:200,alloy:80,crystal:80,data:30}},
 nyx:{name:"尼克斯暗区",desc:"暗物质异常区域，扫描到罕见晶体信号。风险与收益都很高。",cost:150,reward:{energy:100,alloy:100,crystal:150,data:35}},
 frontier:{name:"边界空间站",desc:"失联多年的中继空间站。重启后有望恢复边境通讯。",cost:170,reward:{energy:260,alloy:150,crystal:60,data:45}}
};
function loadGame(){try{const s=JSON.parse(localStorage.getItem(SAVE_KEY));return s?{...structuredClone(initialState),...s,upgrades:{...initialState.upgrades,...s.upgrades},tech:{...initialState.tech,...s.tech},battle:{...initialState.battle,...s.battle}}:structuredClone(initialState)}catch(e){return structuredClone(initialState)}}
function saveGame(){localStorage.setItem(SAVE_KEY,JSON.stringify(state))}
function notify(msg){const el=document.getElementById("toast");el.textContent=msg;el.classList.add("show");clearTimeout(toastTimer);toastTimer=setTimeout(()=>el.classList.remove("show"),2300)}
function modal(title,text){document.getElementById("modal-title").textContent=title;document.getElementById("modal-text").textContent=text;document.getElementById("modalback").classList.add("show")}
function closeModal(){document.getElementById("modalback").classList.remove("show")}
function addLog(msg){state.logs.unshift("第"+state.day+"日 · "+msg);state.logs=state.logs.slice(0,30)}
function addMissionLog(msg){state.missionLogs.unshift("第"+state.day+"日 · "+msg);state.missionLogs=state.missionLogs.slice(0,30)}
function canPay(cost){return Object.keys(cost).every(k=>state[k]>=cost[k])}
function pay(cost){Object.keys(cost).forEach(k=>state[k]-=cost[k])}
function gain(reward){Object.keys(reward).forEach(k=>state[k]=(state[k]||0)+reward[k])}
function showView(name){
 document.querySelectorAll(".view").forEach(v=>v.classList.toggle("active",v.id==="view-"+name));
 document.querySelectorAll(".navbtn").forEach(b=>b.classList.toggle("active",b.dataset.view===name));
 if(name==="map")renderMap();if(name==="battle")renderBattle();if(name==="codex")renderCodex();
}
document.querySelectorAll(".navbtn").forEach(b=>b.addEventListener("click",()=>showView(b.dataset.view)));
document.querySelectorAll(".system").forEach(b=>b.addEventListener("click",()=>selectSystem(b.dataset.system)));
function selectSystem(id){
 selectedSystem=id;state.selected=id;const s=systems[id];const explored=state.explored.includes(id);
  
 document.querySelectorAll(".system").forEach(b=>{b.classList.toggle("selected",b.dataset.system===id);b.classList.toggle("locked",!state.explored.includes(b.dataset.system)&&b.dataset.system!=="sol")});
 document.getElementById("selected-name").textContent=s.name;
 document.getElementById("selected-desc").textContent=explored?"该星系已完成初步探索。可以再次派遣舰队进行资源采集。":s.desc+" 探索消耗："+(state.tech.scan?Math.max(40,s.cost-30):s.cost)+" 能源。";
 document.getElementById("system-info").innerHTML='<div class="eyebrow">SYSTEM INTEL</div><h3>'+s.name+'</h3><p>'+s.desc+'</p><span class="tag '+(state.controlled.includes(id)?"good":"warn")+'">'+(state.controlled.includes(id)?"已建立控制":"未控制")+'</span>';
 document.getElementById("explore-btn").textContent=explored?"再次采集资源 · "+Math.max(40,Math.floor(s.cost*.55))+" 能源":"派遣探索 · "+(state.tech.scan?Math.max(40,s.cost-30):s.cost)+" 能源";
 document.getElementById("explore-btn").disabled=id==="sol";
}
function exploreSelected(){
 const id=selectedSystem,s=systems[id],first=!state.explored.includes(id);
 if(id==="sol")return;
 const cost=first?(state.tech.scan?Math.max(40,s.cost-30):s.cost):Math.max(40,Math.floor(s.cost*.55));
 if(state.energy<cost){notify("能源不足，无法派遣舰队。");return}
 pay({energy:cost});
 if(first){
  state.explored.push(id);state.controlled.push(id);gain(s.reward);
  state.morale=Math.min(100,state.morale+3);
  addLog("成功探索"+s.name+"，获得资源与研究数据。");
  addMissionLog("已探索"+s.name+"，获得 "+s.reward.data+" 点研究数据。");
  if(id==="alpha"&&state.missionStep===0){state.missionStep=1;addMissionLog("阿尔法信号已确认，解锁下一阶段调查。")}
  if(id==="frontier"&&state.missionStep>=2){state.missionStep=3}
  notify("探索成功："+s.name+"，资源已入库。");
 }else{
  const r={energy:Math.floor(50+Math.random()*90),alloy:Math.floor(35+Math.random()*65),crystal:Math.floor(8+Math.random()*22),data:Math.floor(5+Math.random()*12)};
  gain(r);addLog("在"+s.name+"完成资源采集。");addMissionLog("资源采集完成："+s.name);
  notify("采集完成，获得额外资源。");
 }
 state.day++;saveGame();renderAll();selectSystem(id);
}

function claimDaily(){
 if(state.dailyClaimed){notify("今日补给已领取，下一日再来。");return}
 gain({energy:180,alloy:70,crystal:25});state.morale=Math.min(100,state.morale+2);state.dailyClaimed=true;
 addLog("领取远征补给：能源 +180、合金 +70、晶体 +25。");saveGame();renderAll();notify("远征补给已送达！");
}
function upgrade(type){
 const costs={hull:{alloy:120,energy:80},shield:{alloy:100,crystal:50},weapon:{alloy:160,crystal:35}};
 const c=costs[type];if(!canPay(c)){notify("资源不足，无法升级。");return}
 pay(c);state.upgrades[type]++;
 if(type==="hull"){state.maxHull+=25;state.hull=Math.min(state.maxHull,state.hull+25)}
 if(type==="shield"){state.maxShield+=20;state.shield=Math.min(state.maxShield,state.shield+20)}
 if(type==="weapon")state.weapon++;
 state.power+=type==="weapon"?35:20;addLog("完成舰船系统升级："+({hull:"强化装甲",shield:"扩容护盾",weapon:"武器系统"}[type])+"。");
 saveGame();renderAll();notify("升级完成，舰队战力提升。");
}
function repairFleet(){
 const missing=state.maxHull-state.hull;if(missing<=0){notify("旗舰状态良好，无需维修。");return}
 if(!canPay({energy:90,alloy:60})){notify("维修资源不足。");return}
 pay({energy:90,alloy:60});state.hull=state.maxHull;state.shield=state.maxShield;addLog("旗舰完成维修，船体与护盾恢复。");saveGame();renderAll();notify("舰队维修完成。");
}
function research(type){
 if(state.tech[type]){notify("该科技已经研发完成。");return}
 const cost={energy:0,alloy:0,crystal:0,data:{energy:30,shield:45,scan:40}[type]};
 if(state.data<cost.data){notify("研究数据不足。请先探索星系或执行分析任务。");return}
 state.data-=cost.data;state.tech[type]=true;
 if(type==="energy")state.energy+=160;
 if(type==="shield"){state.maxShield+=30;state.shield+=30;state.power+=15}
 if(type==="scan")state.power+=10;
 addLog("科技研发完成："+({energy:"高效能源核心",shield:"相位护盾",scan:"深空扫描阵列"}[type])+"。");
 saveGame();renderAll();notify("科技研发成功！");
}
function runMission(type){
 if(type==="supply"){
 
  if(state.energy<30){notify("能源不足，无法执行巡航。");return}
  pay({energy:30});gain({energy:120,alloy:60});state.day++;addMissionLog("日常补给巡航完成，获得能源与合金。");
 }else{
  if(state.explored.length<2){notify("至少探索一个新星系后才能分析信号。");return}
  gain({data:18});state.day++;addMissionLog("信号分析完成，研究数据 +18。");
 }
 state.dailyClaimed=false;saveGame();renderAll();notify("任务完成，奖励已入库。");
}
function startBattle(){
 if(state.battle.active){notify("战斗已经进行中。");return}
 if(state.hull<=0){notify("旗舰耐久为零，请先维修。");return}
 state.battle={active:true,enemy:100+Math.floor(state.weapon*12),enemyMax:100+Math.floor(state.weapon*12),defending:false,won:false};
 addLog("进入战术模拟，与边境掠夺者交战。");renderBattle();saveGame();notify("模拟战开始！");
}
function battleAction(action){
 const b=state.battle;if(!b.active)return;
 let damage=0;
 if(action==="attack"){damage=22+state.weapon*7+Math.floor(Math.random()*9);b.enemy=Math.max(0,b.enemy-damage);battleLog("主炮命中敌舰，造成 "+damage+" 点伤害。")}
 if(action==="special"){
  if(state.energy<80){notify("能源不足，无法启动超载齐射。");return}
  state.energy-=80;damage=42+state.weapon*10+Math.floor(Math.random()*12);b.enemy=Math.max(0,b.enemy-damage);battleLog("超载齐射释放，造成 "+damage+" 点重创。");
 }
 if(action==="defend"){b.defending=true;state.shield=Math.min(state.maxShield,state.shield+15);battleLog("护盾进入防御姿态，恢复 15 点护盾。")}
 if(b.enemy<=0){
  b.active=false;b.won=true;gain({energy:150,alloy:100,crystal:25,data:20});state.morale=Math.min(100,state.morale+5);state.power+=5;
  battleLog("敌舰已被击败！模拟战胜利，奖励已发放。");addLog("战术模拟胜利，缴获资源与研究数据。");
 }else{
  const incoming=12+Math.floor(Math.random()*12);
  if(b.defending){const absorbed=Math.min(state.shield,incoming*2);state.shield-=absorbed;battleLog("防御姿态吸收敌方攻击。");b.defending=false}
  else if(state.shield>0){const absorbed=Math.min(state.shield,incoming);state.shield-=absorbed;const rest=incoming-absorbed;if(rest>0)state.hull=Math.max(0,state.hull-rest);battleLog("敌舰反击，造成 "+incoming+" 点压力。")}
  else{state.hull=Math.max(0,state.hull-incoming);battleLog("护盾耗尽，船体受到 "+incoming+" 点损伤。")}
  if(state.hull<=0){b.active=false;battleLog("旗舰失去战斗能力，模拟战结束。");addLog("战术模拟失败，旗舰需要维修。");}
 }
 saveGame();renderAll();renderBattle();
}
function battleLog(msg){const el=document.getElementById("battle-log");const d=document.createElement("div");d.textContent=msg;el.prepend(d);while(el.children.length>12)el.lastChild.remove()}
function renderBattle(){
 const b=state.battle;
 document.getElementById("battle-hull").textContent=state.hull+" / "+state.maxHull;
 document.getElementById("battle-shield").textContent=state.shield+" / "+state.maxShield;
 document.getElementById("battle-hull-bar").style.width=(state.hull/state.maxHull*100)+"%";
 document.getElementById("battle-shield-bar").style.width=(state.shield/state.maxShield*100)+"%";
 document.getElementById("enemy-hp").textContent=b.enemy+" / "+b.enemyMax;
 document.getElementById("enemy-bar").style.width=(b.enemy/b.enemyMax*100)+"%";
 document.getElementById("battle-status").textContent=b.active?"交战中：选择本回合战术指令。":b.won?"模拟战胜利，敌舰已被击败。":"模拟战尚未开始或已结束。";
 document.getElementById("battle-start").disabled=b.active;
 ["attack","defend","special"].forEach(x=>document.getElementById(x+"-btn").disabled=!b.active);
}
function renderMap(){
 document.querySelectorAll(".system").forEach(b=>{const id=b.dataset.system;b.classList.toggle("locked",!state.explored.includes(id)&&id!=="sol")});
 selectSystem(state.selected||"sol");
}
function renderCodex(){
 document.getElementById("codex-systems").innerHTML=Object.entries(systems).map(([id,s])=>'<div class="listrow"><div><strong>'+s.name+'</strong><small>'+s.desc+'</small></div><span class="tag '+(state.explored.includes(id)?"good":"")+'">'+(state.explored.includes(id)?"已探索":"未探索")+'</span></div>').join("");
}
function renderAll(){
 ["energy","alloy","crystal","data"].forEach(k=>document.getElementById("res-"+k).textContent=state[k]);
 document.getElementById("date-label").textContent="远征纪元 01 · 第 "+state.day+" 日";
 document.getElementById("power-stat").innerHTML=state.power+' <small>PT</small>';
 document.getElementById("systems-stat").innerHTML=state.controlled.length+' <small>/ 6</small>';
 document.getElementById("morale-stat").innerHTML=state.morale+'<small>%</small>';
 document.getElementById("morale-bar").style.width=state.morale+"%";
 document.getElementById("research-stat").innerHTML=state.data+'<small> RP</small>';
 document.getElementById("hull-label").textContent=state.hull+" / "+state.maxHull;
 document.getElementById("shield-label").textContent=state.shield+" / "+state.maxShield;
 document.getElementById("hull-bar").style.width=state.hull/state.maxHull*100+"%";
 document.getElementById("shield-bar").style.width=state.shield/state.maxShield*100+"%";
 document.getElementById("crew-label").textContent=state.crew+" / 30";
 
 document.getElementById("data-total").textContent=state.data+" RP";
 document.getElementById("data-bar").style.width=Math.min(100,state.data/100*100)+"%";
 ["energy","shield","scan"].forEach(k=>document.getElementById("tech-"+k).textContent=state.tech[k]?"✓ 已研发完成":"");
 document.getElementById("command-log").innerHTML=state.logs.map(x=>"<div>"+escapeHTML(x)+"</div>").join("");
 document.getElementById("mission-log").innerHTML=state.missionLogs.map(x=>"<div>"+escapeHTML(x)+"</div>").join("");
 const steps=state.missionStep;document.getElementById("mission-progress").style.width=(steps/3*100)+"%";
 document.getElementById("mission-progress-label").textContent=steps+" / 3";
 document.getElementById("main-mission-title").textContent=steps===0?"第一章：来自阿尔法的信号":steps===1?"第二章：边境航线":"第三章：重启边界";
 document.getElementById("main-mission-desc").textContent=steps===0?"派遣舰队前往阿尔法星系，查明异常信号来源。":steps===1?"继续探索至少两个新星系，收集异常数据。":steps===2?"探索边界空间站，尝试恢复星际中继网络。":"边界中继已恢复，远征阶段目标达成！";
 document.getElementById("objective-title").textContent=steps===0?"调查阿尔法信号":steps===1?"扩展边境航线":steps===2?"重启边界空间站":"远征阶段目标达成";
 document.getElementById("objective-text").textContent=steps===0?"前往阿尔法星系，调查异常信号源。":steps===1?"再探索两个星系，收集更多研究数据。":steps===2?"前往边界空间站，恢复星际通讯。":"边境中继网络已恢复，你可以继续自由探索与发展。";
 if(steps===1&&state.explored.length>=3)state.missionStep=2;
 if(steps===2&&state.explored.includes("frontier"))state.missionStep=3;
 renderBattle();saveGame();
}
function escapeHTML(s){return String(s).replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]))}
function resetGame(){
 if(!confirm("确定重置全部游戏进度？此操作不可撤销。"))return;
 localStorage.removeItem(SAVE_KEY);state=structuredClone(initialState);selectedSystem="sol";location.reload();
}
document.addEventListener("keydown",e=>{if(e.key==="Escape")closeModal()});
renderAll();renderMap();
</script>
</body>
</html>



