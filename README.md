<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <title>Chúc Mừng Năm Mới</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background: black;
            font-family: Arial, sans-serif;
        }

        h1 {
            position: absolute;
            top: 35%;
            width: 100%;
            text-align: center;
            color: gold;
            font-size: 60px;
            text-shadow: 0 0 20px red;
            z-index: 2;
        }

        p {
            position: absolute;
            top: 50%;
            width: 100%;
            text-align: center;
            color: white;
            font-size: 28px;
            z-index: 2;
        }

        canvas {
            position: fixed;
            top: 0;
            left: 0;
        }
    </style>
</head>
<body>

<h1>🎉 CHÚC MỪNG NĂM MỚI 🎉
</h1>
<p>Chúc bạn năm mới An Khang – Thịnh Vượng – Thành Công!</p>

<canvas id="fireworks"></canvas>

<script>
    const canvas = document.getElementById("fireworks");
    const ctx = canvas.getContext("2d");

    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    window.onresize = () => {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
    };

    class Firework {
        constructor() {
            this.x = Math.random() * canvas.width;
            this.y = canvas.height;
            this.targetY = Math.random() * canvas.height / 2;
            this.speed = 5 + Math.random() * 3;
            this.exploded = false;
            this.particles = [];
        }

        update() {
            if (!this.exploded) {
                this.y -= this.speed;
                if (this.y <= this.targetY) {
                    this.explode();
                    this.exploded = true;
                }
            } else {
                this.particles.forEach(p => p.update());
            }
        }

        draw() {
            if (!this.exploded) {
                ctx.fillStyle = "white";
                ctx.beginPath();
                ctx.arc(this.x, this.y, 3, 0, Math.PI * 2);
                ctx.fill();
            } else {
                this.particles.forEach(p => p.draw());
            }
        }

        explode() {
            for (let i = 0; i < 50; i++) {
                this.particles.push(new Particle(this.x, this.y));
            }
        }
    }

    class Particle {
        constructor(x, y) {
            this.x = x;
            this.y = y;
            this.angle = Math.random() * Math.PI * 2;
            this.speed = Math.random() * 5 + 1;
            this.life = 100;
            this.color = `hsl(${Math.random() * 360},100%,50%)`;
        }

        update() {
            this.x += Math.cos(this.angle) * this.speed;
            this.y += Math.sin(this.angle) * this.speed;
            this.life--;
        }

        draw() {
            ctx.fillStyle = this.color;
            ctx.beginPath();
            ctx.arc(this.x, this.y, 3, 0, Math.PI * 2);
            ctx.fill();
        }
    }

    let fireworks = [];

    function animate() {
        ctx.fillStyle = "rgba(0,0,0,0.2)";
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        if (Math.random() < 0.05) {
            fireworks.push(new Firework());
        }

        fireworks.forEach((fw, index) => {
            fw.update();
            fw.draw();
        });

        requestAnimationFrame(animate);
    }

    animate();
</script>

</body>
</html>
