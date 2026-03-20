<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy New Year 2026</title>
    <style>
        body {
            margin: 0;
            background: #000;
            overflow: hidden;
            color: white;
            font-family: 'Arial', sans-serif;
        }

        #canvas {
            display: block;
        }

        .message-container {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            pointer-events: none;
            z-index: 10;
        }

        h1 {
            font-size: 5rem;
            margin: 0;
            text-transform: uppercase;
            letter-spacing: 10px;
            animation: glow 2s ease-in-out infinite alternate;
            text-shadow: 0 0 10px #ff0000, 0 0 20px #ff0000, 0 0 30px #ff0000;
        }

        .year {
            font-size: 8rem;
            display: block;
            color: #ffd700;
            text-shadow: 0 0 10px #ffd700, 0 0 30px #ff8c00;
        }

        @keyframes glow {
            from { opacity: 0.8; transform: scale(1); }
            to { opacity: 1; transform: scale(1.05); }
        }
    </style>
</head>
<body>

    <div class="message-container">
        <h1>Happy New Year</h1>
        <span class="year"><i>2026</i></span>
    </div>

    <canvas id="canvas"></canvas>

    <script>
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');

        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        window.addEventListener('resize', () => {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        });

        class Particle {
            constructor(x, y, color, velocity) {
                this.x = x;
                this.y = y;
                this.color = color;
                this.velocity = velocity;
                this.alpha = 1;
                this.friction = 0.95;
            }

            draw() {
                ctx.save();
                ctx.globalAlpha = this.alpha;
                ctx.beginPath();
                ctx.arc(this.x, this.y, 2, 0, Math.PI * 2, false);
                ctx.fillStyle = this.color;
                ctx.fill();
                ctx.restore();
            }

            update() {
                this.velocity.x *= this.friction;
                this.velocity.y *= this.friction;
                this.velocity.y += 0.05; // Gravity
                this.x += this.velocity.x;
                this.y += this.velocity.y;
                this.alpha -= 0.01;
            }
        }

        let particles = [];

        function createFirework(x, y) {
            const particleCount = 100;
            const angleStep = (Math.PI * 2) / particleCount;
            const colors = ['#ff0000', '#ffd700', '#ffffff', '#ff4500', '#00ff00'];
            const color = colors[Math.floor(Math.random() * colors.length)];

            for (let i = 0; i < particleCount; i++) {
                particles.push(new Particle(x, y, color, {
                    x: Math.cos(angleStep * i) * (Math.random() * 8),
                    y: Math.sin(angleStep * i) * (Math.random() * 8)
                }));
            }
        }

        function animate() {
            requestAnimationFrame(animate);
            ctx.fillStyle = 'rgba(0, 0, 0, 0.1)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            particles.forEach((particle, index) => {
                if (particle.alpha > 0) {
                    particle.update();
                    particle.draw();
                } else {
                    particles.splice(index, 1);
                }
            });

            // Random auto-fireworks
            if (Math.random() < 0.05) {
                createFirework(Math.random() * canvas.width, Math.random() * canvas.height * 0.7);
            }
        }

        // Trigger explosion on click
        canvas.addEventListener('click', (event) => {
            createFirework(event.clientX, event.clientY);
        });

        animate();
    </script>
</body>
</html>
