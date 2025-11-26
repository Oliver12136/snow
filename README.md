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
    }

    #globeWrapper {
        position: relative;
        width: 90vw;
        max-width: 420px;
        aspect-ratio: 1/1;
        border-radius: 50%;
        overflow: hidden;
        box-shadow:
            0 0 40px rgba(255,255,255,0.25) inset,
            0 0 80px rgba(255,255,255,0.2);
    }

    /* ❄️ 水晶外层光泽效果 */
    #globeWrapper::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background: radial-gradient(circle at 30% 30%,
            rgba(255,255,255,0.25),
            rgba(255,255,255,0) 40%);
        pointer-events: none;
        z-index: 20;
    }

    #btn {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
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
// ============ Canvas Setup ============
const canvas = document.getElementById("snowCanvas");
const ctx = canvas.getContext("2d");

const wrapper = document.getElementById("globeWrapper");
canvas.width = wrapper.clientWidth;
canvas.height = wrapper.clientHeight;

// ============ Load Snowflake Images ============
const images = ["snow1.png", "snow2.png", "snow3.png"].map(src => {
    let img = new Image();
    img.src = src;
    return img;
});

// ============ Create Snowflakes ============
// 初始状态：他们堆在底部（高度随机 约 bottom 20%）
const NUM = 120;
let snowflakes = [];

function createSnow() {
    for (let i = 0; i < NUM; i++) {
        let size = Math.random() * 22 + 10; // 随机大小
        snowflakes.push({
            img: images[Math.floor(Math.random() * images.length)],
            x: Math.random() * canvas.width,
            y: canvas.height - (Math.random() * canvas.height * 0.2), // 底部堆积
            size: size,
            dx: 0,
            dy: 0,
        });
    }
}

createSnow();

// ============ Gyroscope Variables ============
let tiltX = 0;
let tiltY = 0;

// ============ Draw Snow ============
function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    snowflakes.forEach(flake => {
        // --- 受手机倾斜影响 ---
        flake.x += tiltX * 0.2;
        flake.y += tiltY * 0.25;

        // --- 类似雪景球滚动 ---
        if (flake.x > canvas.width) flake.x = canvas.width;
        if (flake.x < 0) flake.x = 0;

        // 不让雪飞出去，只能在边缘滑
        if (flake.y > canvas.height - flake.size) flake.y = canvas.height - flake.size;
        if (flake.y < canvas.height * 0.1) flake.y = canvas.height * 0.1;

        ctx.drawImage(flake.img, flake.x, flake.y, flake.size, flake.size);
    });

    requestAnimationFrame(draw);
}

draw();


// ============ Motion Permission ============
const btn = document.getElementById("btn");

btn.addEventListener("click", async () => {
    btn.style.display = "none";

    if (typeof DeviceMotionEvent.requestPermission === "function") {
        // iOS 要额外权限
        let res = await DeviceMotionEvent.requestPermission();
        if (res === "granted") startMotion();
        else alert("Permission denied");
    } else {
        startMotion();
    }
});

function startMotion() {
    window.addEventListener("deviceorientation", (e) => {
        tiltX = e.gamma || 0;
        tiltY = e.beta  || 0;
    });
}
</script>

</body>
</html>
