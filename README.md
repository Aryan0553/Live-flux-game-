# 🌈 LiveFlux – Motion Playground

LiveFlux is an interactive particle simulation built with **HTML5 Canvas** and **Vanilla JavaScript**. It creates a visually engaging motion playground where particles react dynamically to mouse movement, clicks, and mobile device orientation.

---

## 🚀 Features

- 🎨 Colorful particle system using HSL colors
- 🖱️ Mouse movement controls gravity direction
- 👆 Click to spawn particle clusters
- 📱 Mobile support with device tilt (gyroscope)
- 🌊 Smooth trailing animation effect
- ⚡ Real-time physics-based motion

---

## 🛠️ Tech Stack

- HTML5
- CSS3
- JavaScript (Canvas API)

---

## 🎯 How It Works

- Particles are generated with random velocity, size, and color
- Gravity dynamically changes based on user interaction
- Canvas redraws with a fade effect to create motion trails
- Optimized particle limit for performance

---

## ▶️ Usage

1. Clone the repository  
2. Open `index.html` in your browser  
3. Move your mouse or click to interact  
4. On mobile, tilt your device for gravity control  

---

## 📂 Project Code

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>LiveFlux – Motion Playground</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
html,body{
    margin:0;
    padding:0;
    background:#050505;
    overflow:hidden;
    font-family:Arial, sans-serif;
}
#info{
    position:fixed;
    top:15px;
    left:50%;
    transform:translateX(-50%);
    color:#0ff;
    background:rgba(0,0,0,0.6);
    padding:10px 20px;
    border-radius:20px;
    font-size:14px;
    z-index:10;
}
canvas{ display:block; }
</style>
</head>

<body>

<div id="info">
🖱️ Move mouse • 👆 Click to spawn • 📱 Tilt device (mobile)
</div>

<canvas id="canvas"></canvas>

<script>
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

canvas.width = innerWidth;
canvas.height = innerHeight;

window.addEventListener("resize",()=>{
    canvas.width = innerWidth;
    canvas.height = innerHeight;
});

let particles = [];
let gravityX = 0;
let gravityY = 0.2;

class Particle{
    constructor(x,y){
        this.x=x;
        this.y=y;
        this.vx=(Math.random()-0.5)*6;
        this.vy=(Math.random()-0.5)*6;
        this.size=Math.random()*6+3;
        this.color=`hsl(${Math.random()*360},100%,60%)`;
    }
    update(){
        this.vx += gravityX;
        this.vy += gravityY;
        this.x += this.vx;
        this.y += this.vy;

        if(this.x<0||this.x>canvas.width) this.vx*=-0.9;
        if(this.y<0||this.y>canvas.height) this.vy*=-0.9;
    }
    draw(){
        ctx.beginPath();
        ctx.arc(this.x,this.y,this.size,0,Math.PI*2);
        ctx.fillStyle=this.color;
        ctx.fill();
    }
}

canvas.addEventListener("click",(e)=>{
    for(let i=0;i<20;i++){
        particles.push(new Particle(e.clientX,e.clientY));
    }
});

document.addEventListener("mousemove",(e)=>{
    gravityX=(e.clientX/canvas.width-0.5)*0.2;
    gravityY=(e.clientY/canvas.height-0.5)*0.2;
});

window.addEventListener("deviceorientation",(e)=>{
    gravityX = e.gamma/100;
    gravityY = e.beta/100;
});

function animate(){
    ctx.fillStyle="rgba(0,0,0,0.2)";
    ctx.fillRect(0,0,canvas.width,canvas.height);

    particles.forEach((p)=>{
        p.update();
        p.draw();
        if(particles.length>1000) particles.shift();
    });

    requestAnimationFrame(animate);
}
animate();
</script>

</body>
</html>

