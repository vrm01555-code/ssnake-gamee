<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Snake Game 🐍</title>
  <style>
    body { display:flex; flex-direction:column; justify-content:center; align-items:center; height:100vh; background:#111; color:#fff; }
    canvas { background:#222; border:2px solid #0f0; margin-bottom:10px; }
    button, select { padding:8px 16px; margin:5px; border:none; border-radius:5px; background:#0f0; color:#000; font-weight:bold; cursor:pointer; }
    button:hover, select:hover { background:#6f6; }
  </style>
</head>
<body>
  <h2>🐍 Snake Game</h2>
  <label for="difficulty">Chọn độ khó:</label>
  <select id="difficulty">
    <option value="300">Dễ</option>
    <option value="200" selected>Trung bình</option>
    <option value="100">Khó</option>
  </select>
  <button id="startBtn">▶️ Bắt đầu</button>
  <canvas id="game" width="400" height="400" style="display:none;"></canvas>
  <button id="restartBtn" style="display:none;">🔄 Chơi lại</button>

  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");
    const startBtn = document.getElementById("startBtn");
    const restartBtn = document.getElementById("restartBtn");
    const difficultySelect = document.getElementById("difficulty");

    const box = 20; 
    let snake, food, dir, score, game, speed;

    function init() {
      snake = [{x: 9 * box, y: 10 * box}];
      food = {
        x: Math.floor(Math.random() * 19) * box,
        y: Math.floor(Math.random() * 19) * box
      };
      dir = null;
      score = 0;
      restartBtn.style.display = "none";
      if (game) clearInterval(game);
      game = setInterval(draw, speed);
    }

    document.addEventListener("keydown", direction);
    function direction(event) {
      if(event.key === "ArrowLeft" && dir !== "RIGHT") dir = "LEFT";
      else if(event.key === "ArrowUp" && dir !== "DOWN") dir = "UP";
      else if(event.key === "ArrowRight" && dir !== "LEFT") dir = "RIGHT";
      else if(event.key === "ArrowDown" && dir !== "UP") dir = "DOWN";
    }

    function draw() {
      ctx.fillStyle = "#111";
      ctx.fillRect(0, 0, 400, 400);

      for(let i = 0; i < snake.length; i++) {
        ctx.fillStyle = i === 0 ? "#0f0" : "#6f6";
        ctx.fillRect(snake[i].x, snake[i].y, box, box);
      }

      ctx.fillStyle = "#f00";
      ctx.fillRect(food.x, food.y, box, box);

      let snakeX = snake[0].x;
      let snakeY = snake[0].y;

      if(dir === "LEFT") snakeX -= box;
      if(dir === "UP") snakeY -= box;
      if(dir === "RIGHT") snakeX += box;
      if(dir === "DOWN") snakeY += box;

      if(snakeX === food.x && snakeY === food.y) {
        score++;
        food = {
          x: Math.floor(Math.random() * 19) * box,
          y: Math.floor(Math.random() * 19) * box
        };
      } else {
        snake.pop();
      }

      let newHead = { x: snakeX, y: snakeY };

      if(snakeX < 0 || snakeY < 0 || snakeX >= 400 || snakeY >= 400 || collision(newHead, snake)) {
        clearInterval(game);
        restartBtn.style.display = "block";
        ctx.fillStyle = "#fff";
        ctx.font = "30px Arial";
        ctx.fillText("Game Over!", 120, 200);
        return;
      }

      snake.unshift(newHead);

      ctx.fillStyle = "#fff";
      ctx.font = "20px Arial";
      ctx.fillText("Điểm: " + score, 10, 390);
    }

    function collision(head, array) {
      for(let i = 0; i < array.length; i++) {
        if(head.x === array[i].x && head.y === array[i].y) {
          return true;
        }
      }
      return false;
    }

    startBtn.addEventListener("click", () => {
      speed = parseInt(difficultySelect.value);
      startBtn.style.display = "none";
      difficultySelect.style.display = "none";
      canvas.style.display = "block";
      init();
    });

    restartBtn.addEventListener("click", init);
  </script>
</body>
</html>
