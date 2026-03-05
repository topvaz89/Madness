# Drive Mad Game v1.0.0

<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Drive Mad Game</title>
    <style>
        body { margin: 0; overflow: hidden; }
        canvas { background: #f0f0f0; display: block; margin: auto; }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="800" height="600"></canvas>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const gravity = 0.5;
        const carWidth = 50;
        const carHeight = 30;

        let car = {
            x: canvas.width / 2 - carWidth / 2,
            y: canvas.height - carHeight - 10,
            width: carWidth,
            height: carHeight,
            speedX: 0,
            speedY: 0,
            controls: { left: false, right: false, up: false }
        };

        // Game state
        let obstacles = [];
        let score = 0;
        let level = 1;

        // Initial obstacles
        function createObstacles() {
            for (let i = 0; i < 5; i++) {
                obstacles.push({
                    x: Math.random() * (canvas.width - 50),
                    y: Math.random() * (canvas.height - 200),
                    width: 50,
                    height: 50
                });
            }
        }

        // Collision detection
        function isColliding(car, obstacle) {
            return car.x < obstacle.x + obstacle.width &&
                   car.x + car.width > obstacle.x &&
                   car.y < obstacle.y + obstacle.height &&
                   car.y + car.height > obstacle.y;
        }

        // Control handling
        window.addEventListener('keydown', (e) => {
            switch(e.key) {
                case 'ArrowLeft':
                    car.controls.left = true;
                    break;
                case 'ArrowRight':
                    car.controls.right = true;
                    break;
                case 'ArrowUp':
                    car.controls.up = true;
                    break;
            }
        });

        window.addEventListener('keyup', (e) => {
            switch(e.key) {
                case 'ArrowLeft':
                    car.controls.left = false;
                    break;
                case 'ArrowRight':
                    car.controls.right = false;
                    break;
                case 'ArrowUp':
                    car.controls.up = false;
                    break;
            }
        });

        function update() {
            if (car.controls.left) car.speedX -= 0.5;
            if (car.controls.right) car.speedX += 0.5;
            if (car.controls.up) car.speedY -= 2;

            car.speedY += gravity;
            car.x += car.speedX;
            car.y += car.speedY;

            // Boundaries
            if (car.x < 0) car.x = 0;
            if (car.x + car.width > canvas.width) car.x = canvas.width - car.width;
            if (car.y + car.height > canvas.height) {
                car.y = canvas.height - car.height;
                car.speedY = 0;
            }

            // Check for collisions
            for (let obstacle of obstacles) {
                if (isColliding(car, obstacle)) {
                    score -= 10; // Penalize
                    // Reset car
                    car.x = canvas.width / 2 - carWidth / 2;
                    car.y = canvas.height - carHeight - 10;
                    car.speedX = 0;
                    car.speedY = 0;
                }
            }
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = 'blue';
            ctx.fillRect(car.x, car.y, car.width, car.height);
            ctx.fillStyle = 'red';
            for (let obstacle of obstacles) {
                ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
            }
            ctx.fillStyle = 'black';
            ctx.fillText('Score: ' + score, 10, 20);
        }

        function gameLoop() {
            update();
            draw();
            requestAnimationFrame(gameLoop);
        }

        createObstacles();
        gameLoop();
    </script>
</body>
</html>
## Release Notes
Initial release of Drive Mad Game - A fun arcade-style driving game where you navigate obstacles and try to achieve the highest score. Features include gravity physics, collision detection, keyboard controls (Arrow keys), and scoring system.
