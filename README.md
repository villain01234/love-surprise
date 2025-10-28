# love-surprise<
!doctype html>
<html lang="hi">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Villaim Surprise 💥</title>
<style>
  :root{
    --bg:#040406;
    --pink:#ff77b9;
    --heart:#ff0066;
    --white:#f6f6f8;
  }
  html,body{
    margin:0;height:100%;
    background:var(--bg);
    font-family: 'Poppins', sans-serif;
    color:var(--white);
    overflow:hidden;
  }
  .stage{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;flex-direction:column;}

  /* Neon Flicker + Floating Watermark */
  .watermark{
    position:absolute;
    left:50%;top:50%;
    transform:translate(-50%,-50%) rotate(-3deg);
    font-size:160px;
    font-weight:900;
    text-transform:uppercase;
    letter-spacing:8px;
    color:#ff007a;
    text-shadow:
      0 0 20px #ff0080,
      0 0 40px #ff007a,
      0 0 80px #ff0060,
      0 0 120px #ff007a;
    opacity:0.06;
    pointer-events:none;
    user-select:none;
    animation: flicker 6s infinite, float 10s ease-in-out infinite;
    filter: blur(1px);
  }

  @keyframes flicker {
    0%, 19%, 21%, 23%, 25%, 54%, 56%, 100% {opacity:0.06;}
    20%, 22%, 24%, 55% {opacity:0.16;}
    50% {opacity:0.10;}
  }

  @keyframes float {
    0% {transform:translate(-50%,-50%) rotate(-3deg);}
    50% {transform:translate(-50%,-54%) rotate(-1deg);}
    100% {transform:translate(-50%,-50%) rotate(-3deg);}
  }

  /* Start screen */
  .start-screen{background:linear-gradient(180deg,#0a0a12,#111124);}
  .start-card{padding:24px 30px;border-radius:14px;text-align:center;background:rgba(255,255,255,0.03);box-shadow:0 10px 30px rgba(0,0,0,0.7);}
  .btn{background:linear-gradient(90deg,#ff6bb8,#ff8a66);color:white;border:none;padding:10px 18px;border-radius:10px;cursor:pointer;font-weight:700;}

  /* Warning stage */
  .warning{width:100%;height:100%;animation: blinkBG 1s infinite steps(2);}
  @keyframes blinkBG{0%{background:#5a0000;}50%{background:#200;}100%{background:#000;}}
  .terminal{color:#fff;font-family:"Courier New",monospace;font-size:18px;padding:20px;background:rgba(0,0,0,0.4);border-radius:10px;z-index:2;}
  .cursor{display:inline-block;width:10px;background:#fff;animation:blink 1s steps(1) infinite;}
  @keyframes blink{50%{opacity:0;}}

  /* Reveal */
  .reveal{background:radial-gradient(circle at 30% 20%,rgba(255,120,160,0.08),transparent 20%),radial-gradient(circle at 70% 80%,rgba(255,40,120,0.06),transparent 20%),var(--bg);}
  .message-card{text-align:center;z-index:3;}
  .message-card h1{font-size:48px;color:var(--pink);margin:0;text-shadow:0 0 15px rgba(255,0,120,0.3);}
  .message-card h2{font-size:34px;margin:6px 0;color:var(--white);}
  .action-btn{background:linear-gradient(90deg,#ff8ab3,#ff6b8a);color:white;border:0;padding:12px 20px;border-radius:12px;font-weight:700;cursor:pointer;box-shadow:0 10px 30px rgba(255, 90, 150, 0.12);}
  .heart{position:absolute;width:18px;height:18px;transform:rotate(-45deg);background:var(--heart);border-radius:18px 18px 0 0;animation:floatUp linear infinite;}
  .heart:before,.heart:after{content:"";position:absolute;width:18px;height:18px;background:var(--heart);border-radius:50%;}
  .heart:before{top:-9px;left:0;}
  .heart:after{left:9px;top:0;}
  @keyframes floatUp{0%{transform:translateY(0)rotate(-45deg)scale(0.8);opacity:1;}100%{transform:translateY(-110vh)rotate(-45deg)scale(1.05);opacity:0;}}
</style>
</head>
<body>

<div class="watermark">VILLAIM</div>

<!-- Start -->
<div id="start" class="stage start-screen">
  <div class="start-card">
    <h2>Secret Surprise 😈</h2>
    <p>Click करो, और कुछ अप्रत्याशित होगा...</p>
    <button id="startBtn" class="btn">Click to Start</button>
  </div>
</div>

<!-- Warning -->
<div id="warning" class="stage warning" style="display:none;">
  <div class="terminal">
    <div id="t1"></div>
    <div id="t2"></div>
    <div id="t3"></div>
    <div id="t4"></div>
  </div>
</div>

<!-- Reveal -->
<div id="reveal" class="stage reveal" style="display:none;">
  <div class="message-card">
    <h1>💖 Actually...</h1>
    <h2>I like you, Villaim 😳</h2>
    <button id="playBtn" class="action-btn">Play a Song 🎵</button>
  </div>
</div>

<script>
const start=document.getElementById('start');
const warning=document.getElementById('warning');
const reveal=document.getElementById('reveal');

const beep=(f=600,d=150)=>{
  try{
    const ctx=new (window.AudioContext||window.webkitAudioContext)();
    const o=ctx.createOscillator(),g=ctx.createGain();
    o.connect(g);g.connect(ctx.destination);
    o.frequency.value=f;o.start();
    setTimeout(()=>o.stop(),d);
  }catch{}
};
const sleep=ms=>new Promise(r=>setTimeout(r,ms));
async function type(el,text,speed=40){
  el.textContent="";
  for(let i=0;i<text.length;i++){
    el.textContent+=text[i];
    await sleep(speed);
  }
}

document.getElementById('startBtn').onclick=async()=>{
  start.style.display='none';warning.style.display='flex';
  const lines=[
    "Scanning device for secrets...",
    "⚠️ Unknown virus detected.",
    "Decrypting hidden file VILLAIM.txt...",
    "..."
  ];
  for(let i=0;i<lines.length;i++){
    await type(document.getElementById('t'+(i+1)),lines[i]);
    beep(700+Math.random()*300,100);
    await sleep(400);
  }
  await sleep(1000);
  warning.style.display='none';
  reveal.style.display='flex';
  spawnHearts();
};

function spawnHearts(){
  const container=document.body;
  for(let i=0;i<50;i++){
    const h=document.createElement('div');
    h.className='heart';
    const size=10+Math.random()*25;
    h.style.width=h.style.height=size+'px';
    h.style.left=Math.random()*100+'%';
    h.style.top='100%';
    h.style.animationDuration=(6+Math.random()*5)+'s';
    container.appendChild(h);
    setTimeout(()=>container.removeChild(h),10000);
  }
}

document.getElementById('playBtn').onclick=()=>{
  window.open("https://youtu.be/DLX62G4lc44","_blank");
};
</script>
</body>
</html>
