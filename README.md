<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Happy Birthday Ghazala</title>
<style>
    * { margin:0; padding:0; box-sizing:border-box; }
    body, html { width:100%; height:100%; font-family: 'Comic Sans MS', cursive; overflow:hidden; display:flex; justify-content:center; align-items:center; background: linear-gradient(to top, #0b0c2a, #1c1f4c); position:relative; }
    canvas { position:absolute; top:0; left:0; width:100%; height:100%; z-index:0; }

    .btn { padding:15px 30px; background:#ff6699; border:none; border-radius:50px; color:white; font-size:1.2rem; cursor:pointer; margin-top:20px; position:relative; z-index:2; }
    .btn:hover { transform:scale(1.1); transition:0.2s; }

    .cake-container { display:none; flex-direction:column; align-items:center; z-index:2; position:relative; }

    .cake { width:150px; height:150px; background:#ff6699; border-radius:0 0 20px 20px; position:relative; margin-top:30px; opacity:0; transform:scale(0); transition:all 1s ease; }
    .cake::before { content:""; position:absolute; top:-20px; left:50%; transform:translateX(-50%); width:30px; height:20px; background:yellow; border-radius:50%; }

    .doodle { position:absolute; font-size:24px; animation:float 4s infinite; }
    @keyframes float { 0% { transform: translateY(0); opacity:1; } 50% { transform: translateY(-20px); opacity:0.7; } 100% { transform: translateY(0); opacity:1; } }

    .birthday-card { display:none; position:absolute; top:50%; left:50%; transform:translate(-50%, -50%); background:#ffe6f0; border-radius:20px; padding:40px; text-align:center; color:#ff3366; box-shadow:0 10px 30px rgba(0,0,0,0.3); z-index:3; }
    .birthday-card h1 { font-size:2rem; margin-bottom:15px; }
    .birthday-card p { font-size:1.2rem; line-height:1.5; }
    .heart { color:red; display:inline-block; animation:heartbeat 1s infinite; }
    @keyframes heartbeat { 0%,100%{transform:scale(1);}50%{transform:scale(1.3);} }
</style>
</head>
<body>

<canvas id="fireworks"></canvas>

<button id="startBtn" class="btn" style="display:none;">Click Here 🎉</button>

<div class="cake-container" id="cakeContainer">
    <div class="cake" id="cake"></div>
</div>

<button id="cardBtn" class="btn" style="display:none;">For You 🎁</button>

<div class="birthday-card" id="birthdayCard">
    <h1>Happy Birthday, Ghazala <span class="heart">❤️</span></h1>
    <p>May Allah turn all your IN SHA ALLAH into ALHAMDULILLAH.<br>
    May you be the Happiest Person in the World.<br>
    May Allah protect you from evil eyes and give you many more reasons to Smile.</p>
</div>

<script>
/* Fireworks Setup */
const canvas = document.getElementById('fireworks');
const ctx = canvas.getContext('2d');
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let fireworks = [];
let particles = [];

class Firework {
    constructor(x,y,targetY){
        this.x=x; this.y=y; this.targetY=targetY; this.speed=2+Math.random()*3; this.exploded=false;
    }
    update(){
        if(this.y > this.targetY) this.y -= this.speed;
        else if(!this.exploded){ this.explode(); this.exploded=true; }
    }
    explode(){
        for(let i=0;i<30;i++) particles.push(new Particle(this.x,this.y));
    }
    draw(){ ctx.beginPath(); ctx.arc(this.x,this.y,2,0,Math.PI*2); ctx.fillStyle="white"; ctx.fill(); }
}

class Particle{
    constructor(x,y){ this.x=x; this.y=y; this.speedX=(Math.random()-0.5)*5; this.speedY=(Math.random()-0.5)*5; this.alpha=1; this.size=2; }
    update(){ this.x+=this.speedX; this.y+=this.speedY; this.alpha-=0.02; }
    draw(){ ctx.globalAlpha=this.alpha; ctx.beginPath(); ctx.arc(this.x,this.y,this.size,0,Math.PI*2); ctx.fillStyle="#ff6699"; ctx.fill(); ctx.globalAlpha=1; }
}

function animate(){
    ctx.fillStyle="rgba(11,12,42,0.2)";
    ctx.fillRect(0,0,canvas.width,canvas.height);
    if(Math.random()<0.05) fireworks.push(new Firework(Math.random()*canvas.width, canvas.height, Math.random()*canvas.height/2));
    fireworks.forEach((f,i)=>{ f.update(); f.draw(); if(f.exploded) fireworks.splice(i,1); });
    particles.forEach((p,i)=>{ p.update(); p.draw(); if(p.alpha<=0) particles.splice(i,1); });
    requestAnimationFrame(animate);
}
animate();

/* Show start button */
setTimeout(()=> { document.getElementById('startBtn').style.display='block'; }, 2000);

/* Start button click */
document.getElementById('startBtn').addEventListener('click',()=>{
    document.getElementById('startBtn').style.display='none';
    const cake=document.getElementById('cake');
    const container=document.getElementById('cakeContainer');
    container.style.display='flex';
    setTimeout(()=>{ cake.style.opacity=1; cake.style.transform='scale(1)'; },500);

    // Doodles
    const doodles=['🎈','🎉','✨','💖','🍰','🌟'];
    for(let i=0;i<15;i++){
        const d=document.createElement('div');
        d.className='doodle';
        d.innerText=doodles[Math.floor(Math.random()*doodles.length)];
        d.style.left=Math.random()*window.innerWidth+'px';
        d.style.top=Math.random()*window.innerHeight+'px';
        d.style.fontSize=15+Math.random()*30+'px';
        container.appendChild(d);
    }

    // Show "For You" button
    setTimeout(()=>{ document.getElementById('cardBtn').style.display='block'; },1500);
});

/* Card button click */
document.getElementById('cardBtn').addEventListener('click',()=>{
    document.getElementById('cakeContainer').style.display='none';
    document.getElementById('cardBtn').style.display='none';
    document.getElementById('birthdayCard').style.display='block';
});
</script>
</body>
</html>
