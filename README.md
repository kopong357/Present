<html lang="th">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Tense Challenge Game</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

    body {
      background: linear-gradient(135deg, #4facfe, #00f2fe);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
    }

    .game-container {
      background: white;
      width: 100%;
      max-width: 700px;
      border-radius: 20px;
      padding: 30px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.2);
      text-align: center;
    }

    h1 {
      color: #333;
      margin-bottom: 10px;
    }

    .score {
      font-size: 20px;
      color: #444;
      margin-bottom: 20px;
    }

    .question-box {
      background: #f5f7ff;
      padding: 25px;
      border-radius: 15px;
      margin-bottom: 20px;
    }

    .question {
      font-size: 24px;
      color: #222;
      margin-bottom: 20px;
    }

    .choices {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 15px;
    }

    button {
      padding: 15px;
      border: none;
      border-radius: 12px;
      font-size: 18px;
      cursor: pointer;
      transition: 0.3s;
    }

    .choice-btn {
      background: #4facfe;
      color: white;
    }

    .choice-btn:hover {
      background: #2196f3;
      transform: scale(1.03);
    }

    .result {
      margin-top: 20px;
      font-size: 22px;
      font-weight: bold;
      min-height: 30px;
    }

    .next-btn {
      margin-top: 20px;
      background: #28a745;
      color: white;
      display: none;
    }

    .next-btn:hover {
      background: #218838;
    }

    .game-over {
      display: none;
    }

    .restart-btn {
      background: #ff9800;
      color: white;
      margin-top: 20px;
    }

    .restart-btn:hover {
      background: #f57c00;
    }

    @media(max-width: 600px) {
      .choices {
        grid-template-columns: 1fr;
      }

      .question {
        font-size: 20px;
      }
    }
  </style>
</head>
<body>

  <div class="game-container">
    <h1>🎮 Tense Challenge Game</h1>
    <div class="score">คะแนน: <span id="score">0</span></div>

    <div id="game-area">
      <div class="question-box">
        <div class="question" id="question">Loading...</div>

        <div class="choices" id="choices"></div>

        <div class="result" id="result"></div>

        <button class="next-btn" id="nextBtn">ข้อถัดไป</button>
      </div>
    </div>

    <div class="game-over" id="gameOver">
      <h2>🎉 จบเกมแล้ว!</h2>
      <p id="finalScore"></p>
      <button class="restart-btn" onclick="restartGame()">เล่นใหม่</button>
    </div>
  </div>

  <script>
    const questions = [
      {
        question: "She ____ to school every day.",
        choices: ["go", "goes", "went", "going"],
        answer: "goes"
      },
      {
        question: "I ____ TV yesterday.",
        choices: ["watch", "watched", "watches", "watching"],
        answer: "watched"
      },
      {
        question: "They ____ dinner right now.",
        choices: ["eat", "are eating", "ate", "eats"],
        answer: "are eating"
      },
      {
        question: "We ____ to Japan next year.",
        choices: ["travel", "traveled", "will travel", "travels"],
        answer: "will travel"
      },
      {
        question: "He ____ his homework already.",
        choices: ["finish", "finished", "has finished", "finishing"],
        answer: "has finished"
      }
    ];

    let currentQuestion = 0;
    let score = 0;

    const questionEl = document.getElementById("question");
    const choicesEl = document.getElementById("choices");
    const resultEl = document.getElementById("result");
    const scoreEl = document.getElementById("score");
    const nextBtn = document.getElementById("nextBtn");
    const gameOverEl = document.getElementById("gameOver");
    const gameAreaEl = document.getElementById("game-area");
    const finalScoreEl = document.getElementById("finalScore");

    function loadQuestion() {
      resultEl.textContent = "";
      nextBtn.style.display = "none";

      const q = questions[currentQuestion];
      questionEl.textContent = q.question;

      choicesEl.innerHTML = "";

      q.choices.forEach(choice => {
        const btn = document.createElement("button");
        btn.textContent = choice;
        btn.classList.add("choice-btn");
        btn.onclick = () => checkAnswer(choice);
        choicesEl.appendChild(btn);
      });
    }

    function checkAnswer(choice) {
      const correct = questions[currentQuestion].answer;
      const buttons = document.querySelectorAll(".choice-btn");

      buttons.forEach(btn => btn.disabled = true);

      if (choice === correct) {
        resultEl.textContent = "✅ ถูกต้อง!";
        resultEl.style.color = "green";
        score += 10;
      } else {
        resultEl.textContent = `❌ ผิด! คำตอบที่ถูกคือ: ${correct}`;
        resultEl.style.color = "red";
      }

      scoreEl.textContent = score;
      nextBtn.style.display = "inline-block";
    }

    nextBtn.addEventListener("click", () => {
      currentQuestion++;

      if (currentQuestion < questions.length) {
        loadQuestion();
      } else {
        endGame();
      }
    });

    function endGame() {
      gameAreaEl.style.display = "none";
      gameOverEl.style.display = "block";
      finalScoreEl.textContent = `คะแนนรวมของคุณ: ${score} คะแนน`;
    }

    function restartGame() {
      currentQuestion = 0;
      score = 0;
      scoreEl.textContent = score;
      gameAreaEl.style.display = "block";
      gameOverEl.style.display = "none";
      loadQuestion();
    }

    loadQuestion();
  </script>

</body>
</html>

