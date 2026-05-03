```html
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PA TOKENS CASINO</title>

<style>
body{
margin:0;
font-family:Arial;
background:#0a0a0a;
color:white;
}

header{
display:flex;
justify-content:space-between;
padding:12px;
background:#111;
border-bottom:1px solid #222;
position:sticky;
top:0;
}

#bal{
color:#00ff99;
font-weight:bold;
}

.container{
padding:12px;
padding-bottom:90px;
}

.card{
background:#151515;
border:1px solid #222;
border-radius:12px;
padding:12px;
margin:10px 0;
}

input{
width:100%;
padding:10px;
margin:5px 0;
border-radius:8px;
border:1px solid #333;
background:#111;
color:white;
box-sizing:border-box;
}

button{
padding:10px;
margin:5px 0;
background:#1d1d1d;
color:white;
border:1px solid #333;
border-radius:8px;
cursor:pointer;
width:100%;
}

button:hover{background:#2a2a2a;}

.reel{
font-size:40px;
display:inline-block;
width:60px;
text-align:center;
}

.screen{display:none;}
.active{display:block;}

.nav{
position:fixed;
bottom:0;
left:0;
right:0;
display:flex;
background:#111;
border-top:1px solid #222;
}

.nav button{
flex:1;
border:none;
background:none;
padding:12px;
color:white;
}

.green{background:#00c853 !important;color:#000;}
.red{background:#d50000 !important;color:#fff;}

.small{font-size:13px;opacity:0.8;}
</style>
</head>

<body>

<header>
<div>🎰 PA TOKENS</div>
<div id="bal"></div>
</header>

<div class="container">

<!-- CASINO -->
<div id="casino" class="screen active">

<div class="card">
<h3>🎰 Слот</h3>
<input id="slotBet" placeholder="ставка">

<div>
<span class="reel" id="r1">🍒</span>
<span class="reel" id="r2">🍒</span>
<span class="reel" id="r3">🍒</span>
</div>

<button id="spinBtn" class="green" onclick="slot()">Крутить</button>
</div>

<div class="card">
<h3>🔴⚫ Фортуна</h3>
<input id="fortuneBet" placeholder="ставка">
<button onclick="fortune('red')">🔴 Красное</button>
<button onclick="fortune('black')">⚫ Чёрное</button>
<p id="fortuneText"></p>
</div>

</div>

<!-- BANK -->
<div id="bank" class="screen">
<div class="card">
<h3>🏦 Банк</h3>
<button onclick="deposit()">+1000</button>
<button onclick="withdraw()">Вывести</button>
</div>
</div>

<!-- DAILY -->
<div id="daily" class="screen">
<div class="card">
<h3>🎁 Daily задания</h3>
<p id="dailyText"></p>
<button onclick="daily()">Забрать награду</button>

<div class="small">
1: 5000тысяч<br>
2: секретный бонус<br>
3: 10000тысяч
</div>
</div>

<div class="card">
<h3>🎟 Промокод</h3>
<input id="promoInput" placeholder="введите промокод">
<button onclick="usePromo()">Активировать</button>
<p id="promoText"></p>
</div>
</div>

<!-- PROFILE -->
<div id="profile" class="screen">
<div class="card">
<h3>👤 Профиль</h3>
<p>Ник: <span id="nick"></span></p>
<input id="nameInput" placeholder="новый ник">
<button onclick="changeName()">Сменить</button>
</div>
</div>

</div>

<!-- NAV -->
<div class="nav">
<button onclick="show('casino')">🎰</button>
<button onclick="show('bank')">🏦</button>
<button onclick="show('daily')">🎁</button>
<button onclick="show('profile')">👤</button>
</div>

<script>

let user={
name:localStorage.getItem("n")||"Player",
tokens:+localStorage.getItem("t")||2000,
bank:0,
lastDaily:+localStorage.getItem("d")||0,
streak:+localStorage.getItem("s")||0
};

function save(){
localStorage.setItem("t",user.tokens);
localStorage.setItem("n",user.name);
}

function show(id){
document.querySelectorAll(".screen").forEach(s=>s.classList.remove("active"));
document.getElementById(id).classList.add("active");
}

function update(){
document.getElementById("bal").innerText="💰 "+user.tokens;
document.getElementById("nick").innerText=user.name;

let now=Date.now();
let day=86400000;

if(now-user.lastDaily>=day){
document.getElementById("dailyText").innerText="🎁 доступно";
}else{
let left=day-(now-user.lastDaily);
document.getElementById("dailyText").innerText="⏳ "+Math.floor(left/3600000)+"ч";
}

save();
}

/* SLOT */
function slot(){

let btn=document.getElementById("spinBtn");
btn.classList.remove("red","green");
btn.classList.add("green");

let bet=+document.getElementById("slotBet").value;
if(!bet||bet<=0||bet>user.tokens)return;

user.tokens-=bet;

let r1=document.getElementById("r1");
let r2=document.getElementById("r2");
let r3=document.getElementById("r3");

let s=["🍒","🍋","💎","7️⃣"];

let int=setInterval(()=>{
r1.innerText=s[Math.random()*4|0];
r2.innerText=s[Math.random()*4|0];
r3.innerText=s[Math.random()*4|0];
},60);

setTimeout(()=>{

clearInterval(int);

let res=[
s[Math.random()*4|0],
s[Math.random()*4|0],
s[Math.random()*4|0]
];

r1.innerText=res[0];
r2.innerText=res[1];
r3.innerText=res[2];

let win=0;
let lose=true;

if(res[0]==res[1]&&res[1]==res[2]){
win=bet*10;
lose=false;
}
else if(res[0]==res[1]||res[1]==res[2]){
win=bet*3;
lose=false;
}

user.tokens+=win;

if(lose){
btn.classList.add("red");
btn.classList.remove("green");
}else{
btn.classList.add("green");
btn.classList.remove("red");
}

update();

},1000);
}

/* FORTUNE */
function fortune(color){

let bet=+document.getElementById("fortuneBet").value;
if(!bet||bet<=0||bet>user.tokens)return;

user.tokens-=bet;

let res=Math.random()<0.5?"red":"black";

if(res===color){
user.tokens+=bet*2;
document.getElementById("fortuneText").innerText="WIN";
}else{
document.getElementById("fortuneText").innerText="LOSE";
}

update();
}

/* BANK */
function deposit(){
if(user.tokens<1000)return;
user.tokens-=1000;
user.bank+=1000;
update();
}

function withdraw(){
user.tokens+=user.bank;
user.bank=0;
update();
}

/* DAILY */
function daily(){

let now=Date.now();
let day=86400000;

if(now-user.lastDaily<day)return;

if(now-user.lastDaily<day*2) user.streak++;
else user.streak=1;

let reward=5000 + user.streak*5000;

user.tokens+=reward;
user.lastDaily=now;

localStorage.setItem("d",user.lastDaily);
localStorage.setItem("s",user.streak);

update();
}

/* PROMO (СКРЫТЫЙ) */
function usePromo(){

let code=document.getElementById("promoInput").value.trim();

let promos={
"PA_abm33":1500000,
"zxc_dodiki":100000,
"VIPPLAYER115":115000,
"VIPPLAYER67":67000,
"VIPPLAYER":100000,
"powerlay":30000,
"akikaki":20000,
"braznik":15000,
"lady_bag":15000
};

if(promos[code]){
user.tokens+=promos[code];
document.getElementById("promoText").innerText="Активирован!";
}else{
document.getElementById("promoText").innerText="Неверный код";
}

update();
}

/* NAME */
function changeName(){
let n=document.getElementById("nameInput").value;
if(!n)return;
user.name=n;
update();
}

update();

</script>

</body>
</html>
```
