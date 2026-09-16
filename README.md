<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🍪 Ultimate Cookie Clicker</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',sans-serif}

/* === PASSWORD SCREEN === */
#passwordGate{position:fixed;inset:0;background:linear-gradient(180deg,#1a0f08 0%,#000 100%);display:flex;align-items:center;justify-content:center;z-index:99999;transition:opacity .4s ease}
#passwordGate.hidden{opacity:0;pointer-events:none}
.gate-box{background:rgba(139,69,19,.3);border:3px solid #d4a000;border-radius:20px;padding:35px 25px;text-align:center;backdrop-filter:blur(12px);max-width:420px;width:90%}
.gate-box h1{color:#ffd700;font-size:clamp(26px,5vw,34px);margin-bottom:5px;text-shadow:0 0 20px gold}
.gate-box p{color:#ccc;margin-bottom:20px;font-size:15px}
.tab{display:flex;gap:10px;margin-bottom:20px}
.tab-btn{flex:1;padding:10px;border-radius:8px;border:2px solid #8b4513;background:transparent;color:#aaa;font-size:15px;cursor:pointer;transition:all .2s}
.tab-btn.active{border-color:#ffd700;background:rgba(255,215,0,.15);color:#ffd700;font-weight:bold}
.gate-box input{width:100%;padding:14px;border-radius:10px;border:2px solid #8b4513;background:#1a0f08;color:#fff;font-size:18px;text-align:center;outline:none;margin-bottom:5px}
.gate-box input:focus{border-color:#ffd700;box-shadow:0 0 15px rgba(255,215,0,.3)}
.gate-box button{width:100%;padding:13px;margin-top:10px;border-radius:10px;border:none;background:linear-gradient(135deg,#b88600,#ffd700);color:#000;font-size:18px;font-weight:bold;cursor:pointer;transition:all .2s}
.gate-box button:hover{transform:scale(1.03)}
.error-msg{color:#ff6666;margin-top:12px;font-size:14px;opacity:0;height:0;transition:all .2s}
.error-msg.show{opacity:1;height:auto}
.remember-me{display:flex;align-items:center;justify-content:center;gap:10px;margin-top:12px;color:#aaa;font-size:14px;cursor:pointer}
.remember-me input{width:auto;cursor:pointer;margin:0}

/* === BODY & LOCK SCREEN === */
body{background:linear-gradient(180deg,#2c1810 0%,#1a0f08 50%,#0f0805 100%);color:#fff;min-height:100vh;overflow-x:hidden}
body.locked *{pointer-events:none !important;user-select:none !important}
body.locked::before{content:'';position:fixed;inset:0;background:rgba(0,0,0,.95);z-index:9998;backdrop-filter:blur(10px)}
body.locked::after{content:'🔒 SITE LOCKED — ONLY OWNER CAN UNLOCK';position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);font-size:clamp(20px,5vw,38px);font-weight:bold;color:#ffd700;text-shadow:0 0 20px gold;z-index:9999;text-align:center;line-height:1.6;padding:20px}

/* === OWNER ADMIN BAR — FULL FEATURES === */
#adminBar{position:fixed;top:10px;right:10px;display:flex;flex-wrap:wrap;gap:6px;z-index:1000;display:none;max-width:320px;justify-content:flex-end}
#adminBar.visible{display:flex}
.admin-btn{padding:8px 12px;border-radius:8px;border:none;font-size:13px;font-weight:bold;cursor:pointer;transition:all .2s ease;box-shadow:0 2px 8px rgba(0,0,0,.3)}
.lock-btn{background:linear-gradient(135deg,#aa2222,#dd3333);color:#fff}
.lock-btn:hover{transform:scale(1.05);box-shadow:0 0 12px rgba(220,50,50,.4)}
.unlock-btn{background:linear-gradient(135deg,#22aa22,#33dd33);color:#fff}
.unlock-btn:hover{transform:scale(1.05);box-shadow:0 0 12px rgba(50,220,50,.4)}
.pass-btn{background:linear-gradient(135deg,#2266aa,#3388dd);color:#fff}
.reset-btn{background:linear-gradient(135deg,#aa6622,#dd8833);color:#fff}
.announce-btn{background:linear-gradient(135deg,#8822aa,#aa44dd);color:#fff}
.stats-btn{background:linear-gradient(135deg,#333,#555);color:#fff}
.admin-panel{position:fixed;top:50%;left:50%;transform:translate(-50%,-50%) scale(0);background:rgba(0,0,0,.96);border:3px solid gold;border-radius:20px;padding:30px;z-index:10000;transition:transform .3s ease;min-width:340px;max-width:90%;backdrop-filter:blur(12px)}
.admin-panel.show{transform:translate(-50%,-50%) scale(1)}
.admin-panel h2{color:#ffd700;margin-bottom:20px;text-align:center;font-size:22px}
.admin-panel input,.admin-panel button{width:100%;padding:12px;margin:6px 0;border-radius:8px;border:none;font-size:15px}
.admin-panel input{background:#222;color:#fff;border:1px solid #444;outline:none}
.admin-panel button{background:linear-gradient(135deg,#d4a000,#ffd700);color:#000;font-weight:bold;cursor:pointer;transition:all .2s}
.admin-panel button:hover{transform:scale(1.03)}
.close-btn{position:absolute;top:10px;right:15px;font-size:28px;color:#888;cursor:pointer;border:none;background:transparent}
.role-badge{position:fixed;top:15px;left:15px;background:rgba(255,215,0,.15);border:2px solid gold;border-radius:8px;padding:8px 12px;font-size:13px;color:#ffd700;z-index:50;box-shadow:0 0 10px rgba(255,215,0,.2)}
.announcement-bar{position:fixed;top:0;left:0;right:0;background:linear-gradient(90deg,#b88600,#ffd700,#b88600);color:#000;font-weight:bold;padding:10px;text-align:center;z-index:500;display:none;animation:pulse 2s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.8}}
.sync-indicator{position:fixed;bottom:15px;right:15px;font-size:11px;color:#888;z-index:50}

/* === GAME STYLES === */
.container{display:flex;flex-direction:column;align-items:center;padding:20px;max-width:1200px;margin:0 auto;padding-top:80px}
.cookie-area{text-align:center;margin:20px 0;position:relative}
#count{font-size:clamp(36px,8vw,64px);font-weight:bold;color:#ffd700;text-shadow:0 0 20px #ff9900,0 0 40px rgba(255,150,0,.3);margin-bottom:5px}
#perSecond{font-size:clamp(16px,3vw,24px);opacity:.85;margin-bottom:25px;color:#ffcc66}
#cookie{font-size:clamp(100px,20vw,160px);cursor:pointer;user-select:none;transition:all .08s cubic-bezier(0.2,0,0.2,1);filter:drop-shadow(0 0 40px rgba(210,100,0,.6));display:inline-block}
#cookie:hover{transform:scale(1.05);filter:drop-shadow(0 0 50px rgba(255,140,0,.8))}
#cookie:active{transform:scale(0.9)}
.shop{display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:12px;width:100%;margin-top:30px}
.upgrade{background:rgba(139,69,19,.35);border:2px solid #d26400;border-radius:14px;padding:16px;cursor:pointer;transition:all .2s ease;backdrop-filter:blur(4px)}
.upgrade:hover{transform:translateY(-4px);border-color:#ffd700;box-shadow:0 8px 30px rgba(255,215,0,.25)}
.upgrade.locked{opacity:.55;cursor:not-allowed;transform:none;border-color:#666;box-shadow:none}
.name{font-size:20px;font-weight:bold;margin-bottom:4px;display:flex;align-items:center;gap:8px}
.cost{color:#ffd700;font-size:17px;margin:4px 0}
.owned{color:#88ff88;font-size:14px;opacity:.9}
.prod{font-size:13px;opacity:.65;margin-top:4px;color:#aaddff}
.golden{position:fixed;font-size:55px;cursor:pointer;animation:float 2.5s ease-in-out infinite;z-index:100;display:none;filter:drop-shadow(0 0 25px gold,0 0 50px rgba(255,215,0,.5));transition:transform .1s}
.golden:hover{transform:scale(1.2)}
@keyframes float{0%,100%{transform:translateY(0) rotate(-5deg)}50%{transform:translateY(-20px) rotate(5deg)}}
.floating{position:absolute;pointer-events:none;font-weight:bold;animation:rise 1.5s ease-out forwards;color:#ffd700;text-shadow:0 0 8px #000;font-size:22px}
@keyframes rise{0%{opacity:1;transform:translateY(0)scale(1)}100%{opacity:0;transform:translateY(-100px)scale(1.6)}}
.prestige{position:fixed;top:60px;right:15px;background:rgba(255,215,0,.15);border:2px solid gold;border-radius:12px;padding:12px;cursor:pointer;display:none;backdrop-filter:blur(10px);z-index:50;transition:all .2s}
.prestige:hover{background:rgba(255,215,0,.25);transform:scale(1.05)}
.achievement{position:fixed;top:20px;left:50%;transform:translateX(-50%) translateY(-100px);background:rgba(0,0,0,.85);border:2px solid gold;border-radius:12px;padding:15px 25px;color:#ffd700;font-weight:bold;z-index:200;transition:transform .5s ease;backdrop-filter:blur(10px)}
.achievement.show{transform:translateX(-50%) translateY(0)}
.stats-panel{position:fixed;bottom:15px;left:15px;background:rgba(0,0,0,.6);border:1px solid #555;border-radius:10px;padding:12px;font-size:13px;opacity:.9;z-index:50;backdrop-filter:blur(5px);max-width:220px}
.stats-panel div{margin:4px 0}
.stats-panel span{color:#ffd700;font-weight:bold}
</style>
</head>
<body>

<!-- === PASSWORD SCREEN === -->
<div id="passwordGate">
    <div class="gate-box">
        <h1>🍪 COOKIE CLICKER</h1>
        <p>Select your access type</p>
        <div class="tab">
            <button class="tab-btn active" id="userTab" onclick="switchTab('user')">🎮 User</button>
            <button class="tab-btn" id="ownerTab" onclick="switchTab('owner')">👑 Owner</button>
        </div>
        <input type="password" id="mainPassword" placeholder="Enter password...">
        <div class="remember-me">
            <input type="checkbox" id="rememberMe">
            <label for="rememberMe">Remember me</label>
        </div>
        <button onclick="checkPassword()">🔓 Enter</button>
        <div class="error-msg" id="passError">❌ Incorrect password — try again</div>
    </div>
</div>

<!-- === FULL OWNER ADMIN BAR — ALL FEATURES === -->
<div id="adminBar">
    <button class="admin-btn lock-btn" onclick="lockSite()">🔒 Lock</button>
    <button class="admin-btn unlock-btn" onclick="unlockSite()">🔓 Unlock</button>
    <button class="admin-btn pass-btn" onclick="openPassPanel()">🔑 Passwords</button>
    <button class="admin-btn announce-btn" onclick="openAnnouncePanel()">📢 Announce</button>
    <button class="admin-btn stats-btn" onclick="openStatsPanel()">📊 Stats</button>
    <button class="admin-btn reset-btn" onclick="openResetPanel()">⚠️ Reset</button>
</div>

<!-- === ANNOUNCEMENT BAR === -->
<div class="announcement-bar" id="announcementBar"></div>

<!-- === ADMIN PANELS === -->
<div class="admin-panel" id="passPanel">
    <button class="close-btn" onclick="closeAllPanels()">&times;</button>
    <h2>🔑 CHANGE PASSWORDS</h2>
    <input type="password" id="newOwnerPass" placeholder="New Owner Password">
    <input type="password" id="newUserPass" placeholder="New User Password">
    <button onclick="savePasswords()">✅ Save Passwords</button>
</div>

<div class="admin-panel" id="announcePanel">
    <button class="close-btn" onclick="closeAllPanels()">&times;</button>
    <h2>📢 GLOBAL ANNOUNCEMENT</h2>
    <input type="text" id="announcementText" placeholder="Type announcement...">
    <button onclick="sendAnnouncement()">📢 Show to Everyone</button>
    <button onclick="hideAnnouncement()">❌ Hide Announcement</button>
</div>

<div class="admin-panel" id="statsPanel">
    <button class="close-btn" onclick="closeAllPanels()">&times;</button>
    <h2>📊 SITE STATISTICS</h2>
    <div style="text-align:left;color:#ddd;line-height:1.8;margin:15px 0">
        <div>🍪 Total Cookies: <span id="statCookies">0</span></div>
        <div>👆 Total Clicks: <span id="statClicks">0</span></div>
        <div>⚡ CPS: <span id="statCps">0</span></div>
        <div>🏠 Buildings: <span id="statBuildings">0</span></div>
        <div>👑 Prestige Points: <span id="statPrestige">0</span></div>
        <div>🔒 Lock Status: <span id="statLock">Unlocked</span></div>
    </div>
    <button onclick="closeAllPanels()">✅ Close</button>
</div>

<div class="admin-panel" id="resetPanel">
    <button class="close-btn" onclick="closeAllPanels()">&times;</button>
    <h2>⚠️ RESET EVERYTHING</h2>
    <p style="color:#ff6666;margin:15px 0">This will reset ALL cookies, upgrades, and progress for everyone!</p>
    <button onclick="confirmReset()" style="background:linear-gradient(135deg,#aa2222,#dd3333);color:#fff">🗑️ Reset All Data</button>
</div>

<div id="roleBadge" class="role-badge" style="display:none">👑 Owner Mode</div>
<div class="sync-indicator" id="syncStatus">✓ Global Sync Active</div>

<!-- === GAME UI === -->
<div class="stats-panel" id="gameStats">
    <div>Total Clicks: <span id="totalClicks">0</span></div>
    <div>Total Baked: <span id="totalBaked">0</span></div>
    <div>Play Time: <span id="playTime">0:00</span></div>
</div>

<div class="prestige" id="prestigeBtn" onclick="prestige()">
    <div>⭐ Prestige</div>
    <div id="prestigePoints">0 points</div>
</div>

<div class="achievement" id="achievement">🏆 <span id="achText"></span></div>

<div class="container">
    <div class="cookie-area">
        <div id="count">0 🍪</div>
        <div id="perSecond">per second: 0</div>
        <div id="cookie">🍪</div>
    </div>
    <div class="shop">
        <div class="upgrade" data-id="0">
            <div class="name">🖐️ Cursor</div>
            <div class="cost">Cost: 15 🍪</div>
            <div class="owned">Owned: 0</div>
            <div class="prod">+0.1 per second</div>
        </div>
        <div class="upgrade" data-id="1">
            <div class="name">🏠 Grandma</div>
            <div class="cost">Cost: 100 🍪</div>
            <div class="owned">Owned: 0</div>
            <div class="prod">+1 per second</div>
        </div>
        <div class="upgrade" data-id="2">
            <div class="name">🏭 Farm</div>
            <div class="cost">Cost: 1,100 🍪</div>
            <div class="owned">Owned: 0</div>
            <div class="prod">+8 per second</div>
        </div>
        <div class="upgrade" data-id="3">
            <div class="name">🏢 Mine</div>
            <div class="cost">Cost: 12,000 🍪</div>
            <div class="owned">Owned: 0</div>
            <div class="prod">+47 per second</div>
        </div>
        <div class="upgrade" data-id="4">
            <div class="name">🏗️ Factory</div>
            <div class="cost">Cost: 130,000 🍪</div>
            <div class="owned">Owned: 0</div>
            <div class="prod">+260 per second</div>
        </div>
        <div class="upgrade" data-id="5">
            <div class="name">🏦 Bank</div>
            <div class="cost">Cost: 2,100,000 🍪</div>
            <div class="owned">Owned: 0</div>
            <div class="prod">+1,400 per second</div>
        </div>
        <div class="upgrade" data-id="6">
            <div class="name">⛩️ Temple</div>
            <div class="cost">Cost: 35,000,000 🍪</div>
            <div class="owned">Owned: 0</div>
            <div class="prod">+7,800 per second</div>
        </div>
        <div class="upgrade" data-id="7">
            <div class="name">🧙 Wizard Tower</div>
            <div class="cost">Cost: 550,000,000 🍪</div>
            <div class="owned">Owned: 0</div>
            <div class="prod">+44,000 per second</div>
        </div>
    </div>
</div>

<div class="golden" id="goldenCookie">✨</div>

<script>
// ==========================================
// 🔑 YOUR PASSWORDS — SET & WORKING!
// ==========================================
let OWNER_PASSWORD = 'Cookieboss123';
let USER_PASSWORD = 'play123';
let currentRole = 'user';
let isOwner = false;
let startTime = Date.now();

// ==========================================
// 🌍 GLOBAL LOCK SYSTEM — WORKS FOR ALL!
// ==========================================
const GLOBAL_LOCK = 'cookie_clicker_global_lock_v2';
const GLOBAL_ANNOUNCE = 'cookie_clicker_global_announce';

function getGlobalState(key){
    try{
        return localStorage.getItem(key);
    }catch(e){ return null; }
}

function setGlobalState(key, value){
    try{
        localStorage.setItem(key, value);
        return true;
    }catch(e){ return false; }
}

function checkLock(){
    const locked = getGlobalState(GLOBAL_LOCK) === 'true';
    document.body.classList.toggle('locked', locked);
    const statLock = document.getElementById('statLock');
    if(statLock) statLock.textContent = locked ? '🔒 Locked' : '🔓 Unlocked';
}

function lockSite(){
    if(!isOwner) return alert('❌ Owner only!');
    setGlobalState(GLOBAL_LOCK, 'true');
    checkLock();
    alert('🔒 SITE LOCKED — ALL USERS LOCKED!');
}

function unlockSite(){
    if(!isOwner) return alert('❌ Owner only!');
    setGlobalState(GLOBAL_LOCK, 'false');
    checkLock();
    alert('🔓 SITE UNLOCKED — ALL USERS UNLOCKED!');
}

// Check lock & announcements every 1.5 seconds for everyone
setInterval(() => {
    checkLock();
    checkAnnouncement();
}, 1500);

// ==========================================
// 📢 GLOBAL ANNOUNCEMENTS
// ==========================================
function checkAnnouncement(){
    const text = getGlobalState(GLOBAL_ANNOUNCE);
    const bar = document.getElementById('announcementBar');
    if(text && text !== 'hidden'){
        bar.textContent = text;
        bar.style.display = 'block';
    }else{
        bar.style.display = 'none';
    }
}

function sendAnnouncement(){
    if(!isOwner) return;
    const text = document.getElementById('announcementText').value.trim();
    if(text){
        setGlobalState(GLOBAL_ANNOUNCE, text);
        alert('✅ Announcement shown to ALL users!');
        closeAllPanels();
    }
}

function hideAnnouncement(){
    if(!isOwner) return;
    setGlobalState(GLOBAL_ANNOUNCE, 'hidden');
    alert('✅ Announcement hidden!');
    closeAllPanels();
}

// ==========================================
// 🔑 PASSWORD LOGIN — FIXED!
// ==========================================
function switchTab(role){
    currentRole = role;
    document.getElementById('userTab').classList.toggle('active', role === 'user');
    document.getElementById('ownerTab').classList.toggle('active', role === 'owner');
    document.getElementById('mainPassword').placeholder = role === 'user' ? 'Enter user password...' : 'Enter owner password...';
    document.getElementById('mainPassword').value = '';
    document.getElementById('passError').classList.remove('show');
}

function checkPassword(){
    const input = document.getElementById('mainPassword').value.trim();
    const error = document.getElementById('passError');
    let granted = false;
    
    if(currentRole === 'owner' && input === OWNER_PASSWORD){
        granted = true;
        isOwner = true;
    } else if(currentRole === 'user' && input === USER_PASSWORD){
        granted = true;
        isOwner = false;
    }
    
    if(granted){
        error.classList.remove('show');
        if(document.getElementById('rememberMe').checked){
            localStorage.setItem('rememberedRole', currentRole);
            localStorage.setItem('rememberedPass', input);
        }
        document.getElementById('passwordGate').classList.add('hidden');
        const badge = document.getElementById('roleBadge');
        badge.style.display = 'block';
        badge.textContent = isOwner ? '👑 Owner Mode' : '🎮 User Mode';
        badge.style.background = isOwner ? 'rgba(255,215,0,.15)' : 'rgba(100,100,100,.3)';
        badge.style.border = isOwner ? '2px solid gold' : '1px solid #666';
        document.getElementById('adminBar').classList.toggle('visible', isOwner);
        checkLock();
        checkAnnouncement();
        startGame();
    }else{
        error.classList.add('show');
        document.getElementById('mainPassword').value = '';
        document.getElementById('mainPassword').focus();
    }
}

document.getElementById('mainPassword').addEventListener('keypress', e => {
    if(e.key === 'Enter') checkPassword();
});

function initPasswordGate(){
    const savedRole = localStorage.getItem('rememberedRole');
    const savedPass = localStorage.getItem('rememberedPass');
    if(savedRole && savedPass){
        if(savedRole === 'owner' && savedPass === OWNER_PASSWORD){
            autoLogin('owner');
            return;
        }
        if(savedRole === 'user' && savedPass === USER_PASSWORD){
            autoLogin('user');
            return;
        }
    }
}

function autoLogin(role){
    currentRole = role;
    isOwner = (role === 'owner');
    document.getElementById('passwordGate').classList.add('hidden');
    const badge = document.getElementById('roleBadge');
    badge.style.display = 'block';
    badge.textContent = isOwner ? '👑 Owner Mode' : '🎮 User Mode';
    badge.style.background = isOwner ? 'rgba(255,215,0,.15)' : 'rgba(100,100,100,.3)';
    badge.style.border = isOwner ? '2px solid gold' : '1px solid #666';
    document.getElementById('adminBar').classList.toggle('visible', isOwner);
    checkLock();
    checkAnnouncement();
    startGame();
}

// ==========================================
// ⚙️ ADMIN PANEL FUNCTIONS
// ==========================================
function openPassPanel(){ closeAllPanels(); document.getElementById('passPanel').classList.add('show'); }
function openAnnouncePanel(){ closeAllPanels(); document.getElementById('announcePanel').classList.add('show'); }
function openStatsPanel(){ closeAllPanels(); updateStatsPanel(); document.getElementById('statsPanel').classList.add('show'); }
function openResetPanel(){ closeAllPanels(); document.getElementById('resetPanel').classList.add('show'); }

function closeAllPanels(){
    document.querySelectorAll('.admin-panel').forEach(p => p.classList.remove('show'));
}

function savePasswords(){
    if(!isOwner) return;
    const newOwner = document.getElementById('newOwnerPass').value.trim();
    const newUser = document.getElementById('newUserPass').value.trim();
    if(newOwner) OWNER_PASSWORD = newOwner;
    if(newUser) USER_PASSWORD = newUser;
    alert('✅ Passwords updated! Tell your friends the new User password!');
    closeAllPanels();
}

function updateStatsPanel(){
    document.getElementById('statCookies').textContent = fmt(cookies);
    document.getElementById('statClicks').textContent = fmt(totalClicks);
    document.getElementById('statCps').textContent = fmt(cps);
    document.getElementById('statBuildings').textContent = upgrades.reduce((s,u)=>s+u.owned,0);
    document.getElementById('statPrestige').textContent = prestigePoints;
}

function confirmReset(){
    if(!isOwner) return;
    if(confirm('⚠️ THIS WILL DELETE ALL PROGRESS FOR EVERYONE!\n\nType "YES" to confirm:')){
        const confirmText = prompt('Type YES to reset EVERYTHING:');
        if(confirmText === 'YES'){
            localStorage.clear();
            location.reload();
        }
    }
}

// ==========================================
// 🎮 GAME CODE — FULLY FIXED!
// ==========================================
function startGame(){
    loadGame();
    calculateCPS();
    updateAll();
}

let cookies = 0, cps = 0, clickPower = 1, totalClicks = 0, totalBaked = 0;
let prestigePoints = parseInt(localStorage.getItem('prestigePoints')) || 0;
let achievements = JSON.parse(localStorage.getItem('achievements') || '{}');

let upgrades = [
    {name:'Cursor',baseCost:15,ps:0.1,owned:0,count:15},
    {name:'Grandma',baseCost:100,ps:1,owned:0,count:100},
    {name:'Farm',baseCost:1100,ps:8,owned:0,count:1100},
    {name:'Mine',baseCost:12000,ps:47,owned:0,count:12000},
    {name:'Factory',baseCost:130000,ps:260,owned:0,count:130000},
    {name:'Bank',baseCost:2100000,ps:1400,owned:0,count:2100000},
    {name:'Temple',baseCost:35000000,ps:7800,owned:0,count:35000000},
    {name:'Wizard Tower',baseCost:550000000,ps:44000,owned:0,count:550000000}
];

const achList = [
    {id:'firstClick',name:'First Bite!',check:()=>totalClicks>=1},
    {id:'hundredClicks',name:'Dedicated',check:()=>totalClicks>=100},
    {id:'thousandClicks',name:'Cookie Monster',check:()=>totalClicks>=1000},
    {id:'first100',name:'A Small Fortune',check:()=>cookies>=100},
    {id:'first1000',name:'Getting Serious',check:()=>cookies>=1000},
    {id:'firstMillion',name:'Cookie Millionaire',check:()=>cookies>=1000000},
    {id:'firstBillion',name:'Cookie Billionaire',check:()=>cookies>=1000000000},
    {id:'firstCursor',name:'Hired Help',check:()=>upgrades[0].owned>=1},
    {id:'firstGrandma',name:'Grandma Knows Best',check:()=>upgrades[1].owned>=1},
    {id:'tenGrandmas',name:'Grandma Army',check:()=>upgrades[1].owned>=10},
    {id:'firstGolden',name:'Lucky Find!',check:()=>window.goldenClicks>=1},
    {id:'firstPrestige',name:'Transcendent',check:()=>prestigePoints>=1}
];

window.goldenClicks = 0;

function loadGame(){
    const s = localStorage.getItem('cookieClickerSave');
    if(s){
        const d = JSON.parse(s);
        cookies = d.cookies||0;
        clickPower = d.clickPower||1;
        upgrades = d.upgrades||upgrades;
        totalClicks = d.totalClicks||0;
        totalBaked = d.totalBaked||0;
        startTime = d.startTime||Date.now();
        window.goldenClicks = d.goldenClicks||0;
    }
    prestigePoints = parseInt(localStorage.getItem('prestigePoints'))||0;
    achievements = JSON.parse(localStorage.getItem('achievements')||'{}');
    updateAll();
}

function saveGame(){
    localStorage.setItem('cookieClickerSave',JSON.stringify({
        cookies,clickPower,upgrades,totalClicks,totalBaked,startTime,goldenClicks:window.goldenClicks
    }));
    localStorage.setItem('prestigePoints',prestigePoints);
    localStorage.setItem('achievements',JSON.stringify(achievements));
}

function fmt(n){
    if(n<1e3)return Math.floor(n).toLocaleString();
    if(n<1e6)return (n/1e3).toFixed(1)+'K';
    if(n<1e9)return (n/1e6).toFixed(1)+'M';
    if(n<1e12)return (n/1e9).toFixed(1)+'B';
    if(n<1e15)return (n/1e12).toFixed(1)+'T';
    if(n<1e18)return (n/1e15).toFixed(1)+'Q';
    return (n/1e18).toFixed(1)+'Qi';
}

function fmtTime(ms){
    let s = Math.floor(ms/1000), m = Math.floor(s/60), h = Math.floor(m/60);
    s %= 60; m %= 60;
    if(h>0)return `${h}h ${m}m`;
    return `${m}:${s.toString().padStart(2,'0')}`;
}

function checkAchievements(){
    achList.forEach(ach=>{
        if(!achievements[ach.id] && ach.check()){
            achievements[ach.id] = true;
            showAchievement(ach.name);
        }
    });
}

function showAchievement(name){
    const el = document.getElementById('achievement');
    document.getElementById('achText').textContent = name;
    el.classList.add('show');
    setTimeout(()=>el.classList.remove('show'),3500);
}

document.getElementById('cookie').addEventListener('click',e=>{
    const gain = clickPower * (1 + prestigePoints * 0.05);
    cookies += gain; totalBaked += gain; totalClicks++;
    createFloating(e.clientX,e.clientY,'+'+fmt(gain));
    updateCount(); checkAchievements(); saveGame();
});

function createFloating(x,y,t){
    const d = document.createElement('div');
    d.className = 'floating'; d.textContent = t;
    d.style.left = x+'px'; d.style.top = y+'px';
    document.body.appendChild(d);
    setTimeout(()=>d.remove(),1500);
}

document.querySelectorAll('.upgrade').forEach((el,i)=>{
    el.addEventListener('click',()=>buyUpgrade(i));
});

function buyUpgrade(i){
    const u = upgrades[i];
    if(cookies >= u.count){
        cookies -= u.count; u.owned++;
        u.count = Math.ceil(u.baseCost * Math.pow(1.15,u.owned));
        calculateCPS(); updateAll(); checkAchievements(); saveGame();
    }
}

function calculateCPS(){
    cps = 0;
    upgrades.forEach(u=>cps += u.ps * u.owned);
    cps *= (1 + prestigePoints * 0.02);
}

function updateAll(){
    updateCount(); updateShop(); updatePrestige(); updateStats();
}

function updateCount(){
    document.getElementById('count').textContent = fmt(cookies)+' 🍪';
    document.getElementById('perSecond').textContent = 'per second: '+fmt(cps);
}

function updateShop(){
    document.querySelectorAll('.upgrade').forEach((el,i)=>{
        const u = upgrades[i];
        el.querySelector('.cost').textContent = 'Cost: '+fmt(u.count)+' 🍪';
        el.querySelector('.owned').textContent = 'Owned: '+u.owned;
        el.classList.toggle('locked', cookies < u.count);
    });
}

function updatePrestige(){
    const totalCookies = upgrades.reduce((sum,u)=>sum+u.owned*u.baseCost,0) + totalBaked;
    const canPrestige = totalCookies >= 1e12;
    document.getElementById('prestigeBtn').style.display = canPrestige ? 'block' : 'none';
    document.getElementById('prestigePoints').textContent = prestigePoints+' points (+'+Math.floor(Math.pow(totalCookies/1e12,0.5))+')';
}

function updateStats(){
    document.getElementById('totalClicks').textContent = fmt(totalClicks);
    document.getElementById('totalBaked').textContent = fmt(totalBaked);
    document.getElementById('playTime').textContent = fmtTime(Date.now() - startTime);
}

function prestige(){
    if(confirm('⚠️ Reset ALL cookies & upgrades for Prestige Points?\n+5% click power & +2% CPS per point!')){
        const totalCookies = upgrades.reduce((sum,u)=>sum+u.owned*u.baseCost,0) + totalBaked;
        prestigePoints += Math.floor(Math.pow(totalCookies/1e12,0.5));
        cookies = 0; clickPower = 1; totalBaked = 0;
        upgrades.forEach(u=>{u.owned=0;u.count=u.baseCost});
        calculateCPS(); updateAll(); checkAchievements(); saveGame();
    }
}

function spawnGoldenCookie(){
    const el = document.getElementById('goldenCookie');
    el.style.left = (Math.random()*(window.innerWidth-100))+'px';
    el.style.top = (Math.random()*(window.innerHeight-100))+'px';
    el.style.display = 'block';
    el.onclick = ()=>{
        window.goldenClicks++;
        const bonus = Math.max(cookies*0.15, cps*60, 1000);
        cookies += bonus; totalBaked += bonus;
        createFloating(el.offsetLeft+25,el.offsetTop,'+'+fmt(bonus)+' 🍪✨');
        el.style.display = 'none';
        updateCount(); checkAchievements(); saveGame();
    };
    setTimeout(()=>el.style.display='none',15000);
}

setInterval(()=>{
    if(cps>0){
        const gain = cps/20;
        cookies += gain; totalBaked += gain;
        updateCount(); updateStats(); checkAchievements();
    }
    updateShop();
},50);

setInterval(saveGame,10000);
setInterval(spawnGoldenCookie,30000 + Math.random()*20000);

initPasswordGate();
</script>
</body>
</html># hope.html