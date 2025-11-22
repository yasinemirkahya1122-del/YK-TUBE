<!doctype html>
<html lang="tr">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>YK TEAM</title>

<style>
    :root{
        --bg:#0a0c0d;
        --panel:#1b1d20;
        --text:#e4e4e4;
        --green:#00ff95;
        --blue:#00c8ff;
    }
    body,html{
        margin:0;
        padding:0;
        height:100%;
        overflow:hidden;
        background:var(--bg);
        font-family: 'Segoe UI', Tahoma, sans-serif;
        color:var(--text);
    }

    .fade-in{ animation:fadeIn 1s forwards; }
    @keyframes fadeIn{ from{opacity:0;} to{opacity:1;} }

    /* ŞİFRE EKRANI */
    #loginScreen{
        position:absolute;
        inset:0;
        display:flex;
        flex-direction:column;
        align-items:center;
        justify-content:center;
        background:black;
        z-index:50;
    }
    #loginPanel{
        background:rgba(255,255,255,0.07);
        padding:30px;
        border-radius:18px;
        backdrop-filter: blur(10px);
        box-shadow:0 0 30px rgba(0,255,110,0.3);
        animation:fadeIn 1s;
        text-align:center;
    }
    #loginPanel input{
        width:260px;
        padding:12px;
        border-radius:8px;
        border:0;
        margin-top:12px;
        background:#111;
        color:#fff;
        font-size:17px;
    }
    #loginPanel button{
        width:260px;
        padding:12px;
        border-radius:8px;
        border:0;
        margin-top:14px;
        background:var(--green);
        color:#000;
        font-size:17px;
        font-weight:700;
        cursor:pointer;
    }

    /* ANA EKRAN */
    #mainScreen{
        display:none;
        height:100%;
        flex-direction:column;
        align-items:center;
        animation:fadeIn 1s;
        position:relative;
        z-index:2; /* CANVAS ÜSTÜNDE */
        pointer-events:auto;
    }

    .title{
        font-size:80px;
        margin-top:20px;
        font-weight:900;
        letter-spacing:6px;
        text-shadow:0 0 20px rgba(0,255,140,0.6);
        z-index:3;
    }

    .searchBox{
        margin-top:10px;
        width:min(900px,90%);
        background:rgba(255,255,255,0.06);
        padding:16px;
        border-radius:30px;
        display:flex;
        gap:14px;
        align-items:center;
        box-shadow:0 0 20px rgba(0,0,0,0.4);
        backdrop-filter:blur(8px);
        z-index:3;
    }

    .searchBox input{
        flex:1;
        border:0;
        outline:0;
        background:transparent;
        font-size:18px;
        color:white;
    }
    .searchBox button{
        background:var(--blue);
        color:black;
        padding:10px 20px;
        border-radius:14px;
        border:0;
        cursor:pointer;
        font-weight:700;
    }

    /* BUTON PANEL */
    .buttonPanel{
        margin-top:25px;
        display:flex;
        gap:40px;
        background:rgba(255,255,255,0.05);
        padding:22px 40px;
        border-radius:20px;
        box-shadow:0 0 30px rgba(0,0,0,0.5);
        z-index:3;
    }
    .bigBtn{
        display:flex;
        flex-direction:column;
        align-items:center;
        color:white;
        cursor:pointer;
        text-decoration:none;
    }
    .bigCircle{
        width:90px;
        height:90px;
        border-radius:50%;
        background:radial-gradient(circle, #fff 0%, #aaa 25%, #555 100%);
        display:flex;
        justify-content:center;
        align-items:center;
        box-shadow:0 0 25px rgba(255,255,255,0.4), inset 0 0 12px #000;
    }
    .innerCircle{
        width:46px;
        height:46px;
        border-radius:50%;
        background:var(--green);
        box-shadow:0 0 20px var(--green);
    }
    .btnLabel{ margin-top:6px; font-weight:600; }

    /* MATRIX CANVAS - ARKA PLAN */
    #matrixCanvas{
        position:absolute;
        top:0;
        left:0;
        width:100%;
        height:100%;
        z-index:0;         /* EN ARKA */
        pointer-events:none; /* TIKLAMAYI ENGELLEME */
    }
</style>
</head>

<body>

<!-- Şifre ekranı -->
<div id="loginScreen">
    <div id="loginPanel">
        <h2>Giriş</h2>
        <p>Şifreyi girin:</p>
        <input type="password" id="passwordInput" placeholder="Şifre">
        <button onclick="checkPassword()">Giriş Yap</button>
        <p id="error" style="color:red;margin-top:10px;display:none;">Şifre yanlış!</p>
    </div>
</div>

<!-- Ana ekran -->
<div id="mainScreen" class="fade-in">
    <canvas id="matrixCanvas"></canvas>

    <div class="title">YK</div>
<div class="h2">İnternette Güvenli Gezinmenin Kaynağı</div>
<div class="h2">©YK Team Tüm Hakları Saklıdır</div>

    <form class="searchBox" action="https://www.google.com/search" method="GET" target="_blank">
        <input type="text" name="q" placeholder="Google üzerinde arayın...">
        <button type="submit">Ara</button>
    </form>

    <div class="buttonPanel">
        <!-- BURAYA LİNKLERİNİ YAZ -->
        <a class="bigBtn" href="https://sahmaran.fun/login" target="_blank">
            <div class="bigCircle"><div class="innerCircle" style="background:#fff;"></div></div>
            <div class="btnLabel">Panel 1</div>
        </a>

        <a class="bigBtn" href="https://froze.lol/login" target="_blank">
            <div class="bigCircle"><div class="innerCircle" style="background:var(--green);"></div></div>
            <div class="btnLabel" style="color:var(--green)">Panel 2</div>
        </a>

        <a class="bigBtn" href="https://ykteamkayit.wordpress.com" target="_blank">
            <div class="bigCircle"><div class="innerCircle" style="background:var(--blue);"></div></div>
            <div class="btnLabel" style="color:var(--blue)">YK Team</div>
        </a>
    </div>
</div>
<script>
/* =========================
   ŞİFRE SİSTEMİ
========================= */
const CORRECT_PASS = "ykteamanaekran";

if(localStorage.getItem("ykteam_login_ok") === "1"){
    document.getElementById("loginScreen").style.display = "none";
    document.getElementById("mainScreen").style.display = "flex";
}

function checkPassword(){
    const pass = document.getElementById("passwordInput").value;
    if(pass === CORRECT_PASS){
        localStorage.setItem("ykteam_login_ok", "1");
        document.getElementById("loginScreen").style.display = "none";
        document.getElementById("mainScreen").style.display = "flex";
    } else {
        document.getElementById("error").style.display = "block";
    }
}

/* =========================
   MATRIX EFEKTİ
========================= */
const canvas = document.getElementById("matrixCanvas");
const ctx = canvas.getContext("2d");

function resize(){
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
}
resize();
window.onresize = resize;

const chars = "01";
const fontSize = 18;
const cols = Math.floor(canvas.width / fontSize);
let drops = Array(cols).fill(1);

function drawMatrix(){
    ctx.fillStyle = "rgba(0,0,0,0.06)";
    ctx.fillRect(0,0,canvas.width,canvas.height);

    ctx.fillStyle = "#00ff66";
    ctx.font = fontSize + "px monospace";

    for(let i=0;i<drops.length;i++){
        const text = chars[Math.floor(Math.random()*chars.length)];
        const x = i*fontSize;
        const y = drops[i]*fontSize;

        ctx.fillText(text,x,y);

        if(y > canvas.height && Math.random() > 0.98){
            drops[i] = 0;
        }
        drops[i]++;
    }
}
setInterval(drawMatrix,40);
</script>

</body>
</html>
