<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Dodge the Block (Mobile)</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { background: #111; display: flex; flex-direction: column; align-items: center; height: 100vh; }
    canvas { background: #222; border: 2px solid #fff; margin-top: 10px; }
    .controls {
      display: flex;
      justify-content: center;
      margin-top: 10px;
    }
    .btn {
      background: #00e676;
      border: none;
      color: #111;
      font-size: 24px;
      padding: 15px 25px;
      margin: 0 10px;
      border-radius: 10px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <canvas id="game" width="400" height="600"></canvas>
  <div class="controls">
    <button class="btn" onclick="moveLeft()">◀️</button>
    <button class="btn" onclick="moveRight()">▶️</button>
  </div>
  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");

    const player = { x: 180, y: 550, width: 40, height: 40, speed: 20 };
    let obstacles = [];
    let gameOver = false;
    let score = 0;

    function drawPlayer() {
      ctx.fillStyle = "#00e676";
      ctx.fillRect(player.x, player.y, player.width, player.height);
    }

    function drawObstacles() {
      ctx.fillStyle = "#ff1744";
      for (let obs of obstacles) {
        ctx.fillRect(obs.x, obs.y, obs.size, obs.size);
      }
    }

    function updateObstacles() {
      for (let obs of obstacles) {
        obs.y += obs.speed;
        if (obs.y > canvas.height) {
          obs.y = -obs.size;
          obs.x = Math.random() * (canvas.width - obs.size);
          score++;
          obs.speed += 0.05;
        }

        if (
          obs.x < player.x + player.width &&
          obs.x + obs.size > player.x &&
          obs.y < player.y + player.height &&
          obs.y + obs.size > player.y
        ) {
          gameOver = true;
        }
      }
    }

    function drawScore() {
      ctx.fillStyle = "white";
      ctx.font = "20px Arial";
      ctx.fillText("Score: " + score, 10, 30);
    }

    function gameLoop() {
      if (gameOver) {
        ctx.fillStyle = "white";
        ctx.font = "30px Arial";
        ctx.fillText("Game Over", 120, 300);
        ctx.font = "20px Arial";
        ctx.fillText("Final Score: " + score, 135, 340);
        return;
      }

      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawPlayer();
      drawObstacles();
      drawScore();
      updateObstacles();

      requestAnimationFrame(gameLoop);
    }

    function moveLeft() {
      if (player.x > 0) player.x -= player.speed;
    }

    function moveRight() {
      if (player.x + player.width < canvas.width) player.x += player.speed;
    }

    for (let i = 0; i < 5; i++) {
      obstacles.push({
        x: Math.random() * 360,
        y: Math.random() * -600,
        size: 40,
        speed: 4 + Math.random() * 4
      });
    }

    gameLoop();
  </script>
</body>
</html>
