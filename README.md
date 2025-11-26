<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Snow Globe</title>

<style>
    body {
        margin: 0;
        overflow: hidden;
        background: black;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        user-select: none;
        touch-action: none;
    }

    #globeWrapper {
        position: relative;
        width: 90vw;
        max-width: 420px;
        aspect-ratio: 1/1;
        border-radius: 50%;
        overflow: hidden;
        box-shadow:
            0 0 40px rgba(255,255,255,0.2) inset,
            0 0 80px rgba(255,255,255,0.2);
    }

    /* 光泽效果 */
    #globeWrapper::before {
        content: "";
        position: absolute;
        top: 0; left: 0;
        width: 100%; height: 100%;
        border-radius: 50%;
        pointer-events: none;
        background: radial-gradient(circle at 30% 30%,
            rgba(255,255,255,0.25),
            rgba(255,255,255,0) 45%);
        z-index: 20;
    }

    #btn {
        position: absolute;
        top: 45%;
        left: 50%;
        transform: translateX(-50%);
        padding: 12px 20px;
        border-radius: 10px;
        background: white;
        color: black;
        font-size: 16px;
        border: none;
        z-index: 50;
    }
</style>
</head>
<body>

<div id="globeWrapper">
    <canvas id="snowCanvas"></canvas>
    <button id="btn">Tap to Enable Motion</button>
</div>

<script>
// ---------- Setup ----------
const canvas = document.getElementById("snowCanvas");
const ctx = canvas.getContext("2d");

const wrapper = document.getElementById("globeWrapper");
canvas.width = wrapper.clientWidth;
canvas.height = wrapper.clientHeight;

// ---------- Load snowflake images ----------
const imageFiles = ["snow1.png", "snow2.png", "snow3.png"];
const images = imageFiles.map(src => {
    const img = new Image();
    img.src = src;
    return img;
});

// ---------- Create Snow ----------
const NUM = 150;
let snowflakes = [];

function createSnow() {
    for (let i = 0; i < NUM; i++) {
        snowflakes.push({
            img: images[Math.floor(Math.random() * images.length)],
            x: Math.random() * canvas.width,
            y: Math.random() * canvas.height,
            size: Math.random() * 10 + 4, // 比较小的雪花，更自然
            dx: (Math.random() - 0.5) * 0.3, 
            dy: Math.random() * 0.7 + 0.3
        });
    }
}
createSnow();

let tiltX = 0;
let tiltY = 0;

// ---------- Draw Snow ----------
function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    snowflakes.forEach(flake => {

        // 类似你最初代码：轻盈的流动
        flake.x += flake.dx + tiltX * 0.08;
        flake.y += flake.dy + tiltY * 0.10;

        // 微漂浮扰动（让雪更自然）
        flake.x += (Math.random() - 0.5) * 0.2;
        flake.y += (Math.random() - 0.5) * 0.1;

        // 屏幕循环（防止聚团）
        if (flake.y > canvas.height) flake.y = -flake.size;
        if (flake.x > canvas.width) flake.x = 0;
        if (flake.x < 0) flake.x = canvas.width;

        ctx.drawImage(flake.img, flake.x, flake.y, flake.size, flake.size);
    });

    requestAnimationFrame(draw);
}
draw();

// ---------- Motion Permission ----------
const btn = document.getElementById("btn");
btn.addEventListener("click", async () => {
    btn.style.display = "none";

    if (typeof DeviceMotionEvent.requestPermission === "function") {
        let res = await DeviceMotionEvent.requestPermission();
        if (res === "granted") startMotion();
        else alert("Permission denied");
    } else {
        startMotion();
    }
});

function startMotion() {
    window.addEventListener("deviceorientation", (e) => {
        tiltX = e.gamma || 0; // 左右倾斜
        tiltY = e.beta  || 0; // 上下倾斜
    });
}
</script>

</body>
</html>
