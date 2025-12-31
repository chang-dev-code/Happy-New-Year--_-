# 🎶🎶🎶✌️
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Happy New Year Card</title>
  <style>
    :root{
      --bg1:#050816;
      --bg2:#0b1b3a;
      --gold:#ffd36a;
      --pink:#ff6bd6;
      --cyan:#6bf4ff;
      --cardW:min(340px, 86vw);
      --cardH: min(460px, 70vh);
    }
    *{box-sizing:border-box}
    html,body{height:100%; margin:0; font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial;}
    body{
      overflow:hidden;
      background: radial-gradient(1200px 700px at 50% 30%, #132b63 0%, var(--bg2) 35%, var(--bg1) 75%);
      color:#fff;
      user-select:none;
      -webkit-tap-highlight-color: transparent;
    }

    /* background stars */
    .stars{
      position:fixed; inset:0; pointer-events:none;
      background:
        radial-gradient(1px 1px at 10% 20%, rgba(255,255,255,.6) 40%, transparent 45%) ,
        radial-gradient(1px 1px at 30% 80%, rgba(255,255,255,.5) 40%, transparent 45%) ,
        radial-gradient(1px 1px at 70% 30%, rgba(255,255,255,.55) 40%, transparent 45%) ,
        radial-gradient(1px 1px at 85% 60%, rgba(255,255,255,.45) 40%, transparent 45%) ,
        radial-gradient(1px 1px at 55% 55%, rgba(255,255,255,.35) 40%, transparent 45%) ;
      opacity:.6;
      filter: drop-shadow(0 0 6px rgba(255,255,255,.15));
    }

    canvas#fx{
      position:fixed; inset:0;
      width:100%; height:100%;
      z-index:0;
    }

    /* Start screen */
    .start-screen{
      position:fixed; inset:0;
      display:grid; place-items:center;
      z-index:5;
      background: rgba(0,0,0,.35);
      backdrop-filter: blur(6px);
      transition: opacity .5s ease, transform .5s ease;
    }
    .start-card{
      text-align:center;
      padding:28px 22px;
      border-radius:18px;
      background: rgba(255,255,255,.06);
      border: 1px solid rgba(255,255,255,.14);
      box-shadow: 0 18px 50px rgba(0,0,0,.45);
      width:min(520px, 90vw);
    }
    .start-card h1{
      margin:0 0 10px;
      font-size: clamp(22px, 3.8vw, 34px);
      letter-spacing:.5px;
    }
    .start-card p{
      margin:0 0 18px;
      opacity:.9;
      line-height:1.45;
    }
    button{
      appearance:none;
      border:none;
      cursor:pointer;
      padding:12px 18px;
      border-radius:14px;
      font-weight:700;
      font-size:16px;
      color:#091021;
      background: linear-gradient(135deg, var(--gold), #fff2b5);
      box-shadow: 0 10px 30px rgba(255,211,106,.25);
      transition: transform .15s ease, filter .15s ease;
    }
    button:hover{ transform: translateY(-1px); filter: brightness(1.03); }
    button:active{ transform: translateY(1px); }

    /* Main container */
    .stage{
      position:fixed; inset:0;
      display:grid;
      place-items:center;
      z-index:2;
      pointer-events:none;
    }

    /* Card 3D */
    .card-wrap{
      width: var(--cardW);
      height: var(--cardH);
      perspective: 1200px;
      pointer-events:auto;
      transform: scale(.2);
      opacity:0;
      transition: transform .9s cubic-bezier(.2,1,.2,1), opacity .9s ease;
    }
    .card-wrap.show{
      transform: scale(1);
      opacity:1;
    }

    .card{
      width:100%;
      height:100%;
      position:relative;
      transform-style: preserve-3d;
    }

    /* front cover that opens */
    .cover{
       position:absolute; inset:0;
  transform-origin: left center;
  transform: rotateY(0deg);
  transition: transform 1s cubic-bezier(.2,1,.2,1);
  border-radius: 22px;
  overflow:hidden;

  /* bìa đỏ - OPAQUE (không xuyên thấu) */
  background: linear-gradient(135deg, #5a0012 0%, #c4002a 55%, #6f0016 100%);
  border: 1px solid rgba(255,255,255,.18);
  box-shadow: 0 20px 60px rgba(0,0,0,.55);
  backface-visibility: hidden;
}

/* hiệu ứng bóng nhẹ trên bìa (không làm trong suốt) */
.cover::after{
  content:"";
  position:absolute; inset:0;
  background:
    radial-gradient(700px 380px at 30% 20%, rgba(255,255,255,.18) 0%, transparent 55%),
    radial-gradient(900px 500px at 80% 80%, rgba(255,215,215,.10) 0%, transparent 60%);
  opacity: .9;
}

    .cover::after{
      content:"";
      position:absolute; inset:-80px;
      background: conic-gradient(from 90deg, rgba(255,211,106,.25), rgba(107,244,255,.12), rgba(255,107,214,.18), rgba(255,211,106,.25));
      filter: blur(22px);
      opacity:.55;
      animation: glow 6s linear infinite;
    }
    @keyframes glow{
      0%{ transform: rotate(0deg); }
      100%{ transform: rotate(360deg); }
    }

    .cover-content{
      position:relative;
      z-index:2;
      height:100%;
      padding:22px;
      display:flex;
      flex-direction:column;
      align-items:center;
      justify-content:center;
      gap:12px;
      text-align:center;
    }
    .badge{
      font-weight:800;
      letter-spacing:.18em;
      font-size:12px;
      opacity:.9;
      color: rgba(255,255,255,.92);
      text-transform:uppercase;
    }
    .title{
      font-size: clamp(26px, 5vw, 40px);
      font-weight:900;
      line-height:1.05;
      margin:0;
      text-shadow: 0 10px 35px rgba(0,0,0,.45);
    }
    .hint{
      margin:0;
      opacity:.9;
      font-size: 14px;
      line-height:1.4;
    }
    .pill{
      margin-top:10px;
      display:inline-flex;
      align-items:center;
      gap:8px;
      padding:10px 12px;
      border-radius:999px;
      background: rgba(0,0,0,.22);
      border: 1px solid rgba(255,255,255,.16);
      box-shadow: inset 0 0 0 1px rgba(255,255,255,.06);
      font-size:13px;
      opacity:.95;
    }
    .dot{
      width:10px; height:10px; border-radius:999px;
      background: var(--gold);
      box-shadow: 0 0 18px rgba(255,211,106,.6);
    }

    /* inside */
    .inside{
      position:absolute; inset:0;
      border-radius: 22px;
      overflow:hidden;
      background:
        radial-gradient(900px 500px at 80% 0%, rgba(255,255,255,.10) 0%, transparent 60%),
        linear-gradient(135deg, rgba(10,20,45,.95), rgba(8,12,28,.95));
      border: 1px solid rgba(255,255,255,.14);
      box-shadow: 0 20px 60px rgba(0,0,0,.55);
      transform: translateZ(-1px);
    }
    .inside-content{
      height:100%;
      padding:24px;
      display:flex;
      flex-direction:column;
      justify-content:center;
      align-items:center;
      text-align:center;
      gap:14px;
    }
    .big{
      font-size: clamp(30px, 6vw, 44px);
      font-weight:1000;
      margin:0;
      background: linear-gradient(90deg, var(--gold), #fff, var(--cyan), var(--pink));
      -webkit-background-clip:text;
      background-clip:text;
      color: transparent;
      filter: drop-shadow(0 16px 40px rgba(0,0,0,.55));
    }
    .sub{
      margin:0;
      opacity:.9;
      line-height:1.5;
      max-width: 28ch;
    }
    .sparkline{
      width:100%;
      height:2px;
      background: linear-gradient(90deg, transparent, rgba(255,211,106,.7), rgba(107,244,255,.6), rgba(255,107,214,.6), transparent);
      opacity:.8;
      border-radius:999px;
      margin: 6px 0 0;
    }

    /* Open state */
    .card-wrap.open .cover{
      transform: rotateY(-140deg);
    }

    /* small footer */
    .footer-tip{
      position:fixed;
      left:50%;
      transform: translateX(-50%);
      bottom: 14px;
      z-index:3;
      padding:8px 12px;
      font-size:12px;
      opacity:.85;
      border-radius:999px;
      background: rgba(0,0,0,.25);
      border: 1px solid rgba(255,255,255,.12);
      backdrop-filter: blur(6px);
      pointer-events:none;
    }

    /* hide start screen */
    .start-screen.hide{
      opacity:0;
      transform: scale(1.02);
      pointer-events:none;
    }

    /* ===== Music UI ===== */
    .music-ui{
      position: fixed;
      top: 14px;
      right: 14px;
      z-index: 10;
      display:flex;
      align-items:center;
      gap:10px;
      padding:10px 12px;
      border-radius: 14px;
      background: rgba(0,0,0,.25);
      border: 1px solid rgba(255,255,255,.12);
      backdrop-filter: blur(6px);
    }
    .music-ui button{
      padding:10px 12px;
      border-radius:12px;
      font-weight:800;
      font-size:14px;
    }
    .music-ui .vol{
      display:flex;
      align-items:center;
      gap:8px;
      font-size:12px;
      opacity:.95;
      color:#fff;
    }
    .music-ui input[type="range"]{
      width: 140px;
    }
  </style>
</head>
<body>
  <div class="stars"></div>
  <canvas id="fx"></canvas>

  <!-- ===== Music UI + Audio (đổi music.mp3 thành file nhạc của bạn) ===== -->
  <div id="musicUI" class="music-ui" style="display:none;">
    <button id="musicToggle">▶ Play</button>
    <div class="vol">
      <span>🔊</span>
      <input id="volume" type="range" min="0" max="1" step="0.01" value="0.6" />
      <span id="volLabel">60%</span>
    </div>
  </div>
  <audio id="bgm" src="Download.mp3" preload="auto" loop></audio>

  <div class="start-screen" id="startScreen">
    <div class="start-card">
      <h1>Thiệp Chúc Mừng Năm Mới ✨</h1>
      <p>Bấm <b>Start</b> để tạo thiệp. Bấm vào thiệp để mở</p>
      <button id="startBtn">Start</button>
    </div>
  </div>

  <div class="stage">
    <div class="card-wrap" id="cardWrap" aria-label="New Year Card" title="Bấm để mở/đóng thiệp">
      <div class="card">
        <div class="inside">
          <div class="inside-content">
            <h2 class="big">Happy New Year 2026</h2>
            <div class="sparkline"></div>
            <p class="sub">Chúc bạn năm nay có đủ sức khỏe để làm việc bạn yêu, đủ thời gian để yêu thương người bạn quý❤️💕 🥂</p>
            <p class="sub" style="opacity:.75;font-size:12px;margin-top:2px;">(Bấm xung quanh để có pháo hoa 🎆)</p>
          </div>
        </div>

        <div class="cover">
          <div class="cover-content">
            <div class="badge">NEW YEAR CARD</div>
            <h2 class="title">2026</h2>
            <p class="hint">Bấm vào thiệp để mở ra lời chúc</p>
            <div class="pill"><span class="dot"></span> Click quanh màn hình để bắn pháo hoa</div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="footer-tip" id="footerTip" style="display:none;">
    Tip: Bấm xung quanh để bắn pháo hoa 🎆
  </div>

<script>
/* =========================
   Fireworks (Canvas)
========================= */
const canvas = document.getElementById("fx");
const ctx = canvas.getContext("2d", { alpha: true });

let W, H, DPR;
function resize(){
  DPR = Math.max(1, Math.min(2, window.devicePixelRatio || 1));
  W = Math.floor(window.innerWidth);
  H = Math.floor(window.innerHeight);
  canvas.width = Math.floor(W * DPR);
  canvas.height = Math.floor(H * DPR);
  canvas.style.width = W + "px";
  canvas.style.height = H + "px";
  ctx.setTransform(DPR, 0, 0, DPR, 0, 0);
}
window.addEventListener("resize", resize);
resize();

const particles = [];
function rand(min, max){ return Math.random()*(max-min)+min; }

class Particle{
  constructor(x,y, vx,vy, life, hue, size){
    this.x=x; this.y=y;
    this.vx=vx; this.vy=vy;
    this.life=life;
    this.maxLife=life;
    this.hue=hue;
    this.size=size;
    this.gravity=0.06;
    this.drag=0.985;
  }
  step(){
    this.vx *= this.drag;
    this.vy *= this.drag;
    this.vy += this.gravity;
    this.x += this.vx;
    this.y += this.vy;
    this.life -= 1;
  }
  draw(){
    const t = Math.max(0, this.life / this.maxLife);
    ctx.globalAlpha = t;
    ctx.fillStyle = `hsla(${this.hue}, 95%, 65%, 1)`;
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.size * (0.6 + 0.8*t), 0, Math.PI*2);
    ctx.fill();

    // glow
    ctx.globalAlpha = t*0.25;
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.size * (2.2 + 3.2*t), 0, Math.PI*2);
    ctx.fill();
  }
}

function burst(x,y){
  const count = Math.floor(rand(60, 110));
  const hue = Math.floor(rand(0, 360));
  for(let i=0;i<count;i++){
    const a = rand(0, Math.PI*2);
    const sp = rand(1.8, 6.2);
    const vx = Math.cos(a)*sp;
    const vy = Math.sin(a)*sp;
    const life = Math.floor(rand(45, 90));
    const size = rand(1.2, 2.6);
    particles.push(new Particle(x,y,vx,vy,life,hue + rand(-18,18), size));
  }
}

function loop(){
  // fade trail
  ctx.globalCompositeOperation = "source-over";
  ctx.globalAlpha = 0.18;
  ctx.fillStyle = "rgba(0,0,0,1)";
  ctx.fillRect(0,0,W,H);

  ctx.globalCompositeOperation = "lighter";
  for(let i=particles.length-1;i>=0;i--){
    const p = particles[i];
    p.step();
    p.draw();
    if(p.life <= 0 || p.x < -50 || p.x > W+50 || p.y > H+60){
      particles.splice(i,1);
    }
  }
  requestAnimationFrame(loop);
}
loop();

/* =========================
   Start + Card open + MUSIC
========================= */
const startScreen = document.getElementById("startScreen");
const startBtn = document.getElementById("startBtn");
const cardWrap = document.getElementById("cardWrap");
const footerTip = document.getElementById("footerTip");

/* ===== MUSIC: elements ===== */
const bgm = document.getElementById("bgm");
const musicUI = document.getElementById("musicUI");
const musicToggle = document.getElementById("musicToggle");
const volume = document.getElementById("volume");
const volLabel = document.getElementById("volLabel");

// default volume
bgm.volume = Number(volume.value);
volLabel.textContent = Math.round(bgm.volume * 100) + "%";

volume.addEventListener("input", () => {
  bgm.volume = Number(volume.value);
  volLabel.textContent = Math.round(bgm.volume * 100) + "%";
});

function setToggleText(){
  musicToggle.textContent = bgm.paused ? "▶ Play" : "⏸ Pause";
}

musicToggle.addEventListener("click", async (e) => {
  e.stopPropagation();
  try{
    if(bgm.paused) await bgm.play();
    else bgm.pause();
  }catch(err){
    console.log("Play blocked:", err);
  }
  setToggleText();
});

let started = false;

async function showCard(){
  started = true;
  startScreen.classList.add("hide");
  footerTip.style.display = "block";

  // show music controls + try autoplay (Start click = user gesture nên thường chạy OK)
  musicUI.style.display = "flex";
  try { await bgm.play(); } catch(e) { console.log("Autoplay blocked:", e); }
  setToggleText();

  // forming effect
  requestAnimationFrame(() => cardWrap.classList.add("show"));

  // small celebratory bursts
  setTimeout(()=> burst(W*0.25, H*0.35), 250);
  setTimeout(()=> burst(W*0.75, H*0.35), 420);
  setTimeout(()=> burst(W*0.50, H*0.25), 620);
}

startBtn.addEventListener("click", (e)=>{
  e.stopPropagation();
  showCard();
});

cardWrap.addEventListener("click", (e)=>{
  e.stopPropagation();
  cardWrap.classList.toggle("open");

  // burst near card when toggling
  const rect = cardWrap.getBoundingClientRect();
  burst(rect.left + rect.width*0.2, rect.top + rect.height*0.25);
  burst(rect.left + rect.width*0.8, rect.top + rect.height*0.35);
});

/* =========================
   Click around = fireworks
========================= */
document.addEventListener("pointerdown", (e)=>{
  if(!started) return;

  // ignore click on card
  const path = e.composedPath ? e.composedPath() : [];
  if(path.includes(cardWrap)) return;

  burst(e.clientX, e.clientY);
});

// Auto fireworks
let autoTimer = null;
function startAuto(){
  if(autoTimer) return;
  autoTimer = setInterval(()=>{
    if(!started) return;
    burst(rand(W*0.15, W*0.85), rand(H*0.12, H*0.45));
  }, 2200);
}
startAuto();
</script>
</body>
</html>
