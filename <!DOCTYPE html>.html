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
        background: linear-gradient(#0a0f33, #1e2a55);
        color: white;
        font-family: sans-serif;
        text-align: center;
    }
    #btn {
        position: absolute;
        top: 40%;
        left: 50%;
        transform: translateX(-50%);
        padding: 12px 20px;
        border-radius: 10px;
        background: white;
        color: black;
        font-size: 16px;
        border: none;
        display: block;
    }
</style>
</head>
<body>

<canvas id="snowCanvas"></canvas>
<button id="btn">Tap to Enable Motion</button>

<script>
// ---------- Canvas Setup ----------
const canvas = document.getElementById("snowCanvas");
const ctx = canvas.getContext("2d");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let snowflakes = [];
const NUM = 150;

// ---------- Create Snow ----------
for (let i = 0; i < NUM; i++) {
    snowflakes.push({
        x: Math.random() * canvas.width,
        y: Math.random() * canvas.height,
        r: Math.random() * 3 + 1,
        dx: 0,
        dy: Math.random() * 1 + 0.5,
    });
}

let tiltX = 0;  // 陀螺仪X
let tiltY = 0;  // 陀螺仪Y

// ---------- Draw Snow ----------
function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    snowflakes.forEach(flake => {
        // 受到手机角度影响 → 类似雪景球倾斜的感觉
        flake.x += flake.dx + tiltX * 0.2;
        flake.y += flake.dy + tiltY * 0.3;

        // 循环
        if (flake.y > canvas.height) flake.y = -5;
        if (flake.x > canvas.width) flake.x = 0;
        if (flake.x < 0) flake.x = canvas.width;

        ctx.beginPath();
        ctx.arc(flake.x, flake.y, flake.r, 0, Math.PI * 2);
        ctx.fillStyle = "white";
        ctx.fill();
    });

    requestAnimationFrame(draw);
}

draw();

// ---------- Motion Permission ----------
const btn = document.getElementById("btn");

btn.addEventListener("click", async () => {
    btn.style.display = "none";

    if (typeof DeviceMotionEvent.requestPermission === "function") {
        // iOS
        const response = await DeviceMotionEvent.requestPermission();
        if (response === "granted") startMotion();
        else alert("Permission denied");
    } else {
        // Android
        startMotion();
    }
});

function startMotion() {
    window.addEventListener("deviceorientation", (e) => {
        // gamma → 左右倾斜
        // beta → 前后倾斜
        tiltX = e.gamma || 0;
        tiltY = e.beta || 0;
    });
}
</script>

</body>
</html>
