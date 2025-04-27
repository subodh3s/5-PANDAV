<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Quiz App</title>
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap');

    body {
      margin: 0;
      padding: 0;
      font-family: 'Poppins', sans-serif;
      height: 100vh;
      background: #0f2027;
      color: #fff;
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;
    }

    /* Animated lines background */
    .lines-bg {
      position: absolute;
      width: 100%;
      height: 100%;
      overflow: hidden;
      z-index: 0;
    }

    .line {
      position: absolute;
      width: 2px;
      height: 100%;
      background: rgba(255, 255, 255, 0.05);
      animation: moveLines 10s linear infinite;
    }

    @keyframes moveLines {
      from {
        transform: translateY(0) rotate(45deg);
      }
      to {
        transform: translateY(-100%) rotate(45deg);
      }
    }

    .quiz-container {
      position: relative;
      background: rgba(255, 255, 255, 0.1);
      padding: 40px;
      border-radius: 20px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
      max-width: 700px;
      width: 200%;
      z-index: 10;
      backdrop-filter: blur(10px);
    }

    .question {
      font-size: 28px;
      font-weight: bold;
      margin-bottom: 25px;
    }

    .options-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 15px;
    }

    .option {
      background: rgba(255, 255, 255, 0.1);
      padding: 20px;
      border-radius: 14px;
      border: 1px solid transparent;
      cursor: pointer;
      transition: 0.3s;
      position: relative;
      color: #fff;
      font-size: 18px;
      text-align: center;
    }

    .option:hover {
      border-color: #4fc3f7;
      background: rgba(255, 255, 255, 0.2);
      box-shadow: 0 0 15px rgba(79, 195, 247, 0.7);
      transform: translateY(-2px);
    }

    .option input[type="radio"] {
      display: none;
    }

    .option input[type="radio"]:checked + span::after {
      content: "✔";
      position: absolute;
      top: 8px;
      right: 10px;
      color: #4fc3f7;
      font-size: 18px;
    }

    .option span {
      display: inline-block;
      width: 100%;
    }

    button {
      margin-top: 20px;
      padding: 12px;
      width: 100%;
      border: none;
      border-radius: 8px;
      background: #4fc3f7;
      color: #000;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
      transition: background 0.3s ease;
    }

    button:hover {
      background: #81d4fa;
    }

    #result {
      margin-top: 20px;
      font-size: 20px;
      font-weight: bold;
      color: #00e676;
      text-align: center;
    }

    .progress-bar {
      background: rgba(255, 255, 255, 0.2);
      height: 8px;
      border-radius: 5px;
      overflow: hidden;
      margin-bottom: 15px;
    }

    .progress {
      height: 100%;
      width: 0%;
      background: #4fc3f7;
      transition: width 0.4s ease;
    }

    .timer {
      font-size: 14px;
      color: #b3e5fc;
      margin-bottom: 15px;
      text-align: right;
    }
  </style>
</head>
<body>

<div class="lines-bg">
  <script>
    for (let i = 0; i < 20; i++) {
      const line = document.createElement('div');
      line.className = 'line';
      line.style.left = `${Math.random() * 100}%`;
      line.style.animationDuration = `${5 + Math.random() * 10}s`;
      document.body.querySelector('.lines-bg').appendChild(line);
    }
  </script>
</div>

<div class="quiz-container">
  <div class="progress-bar"><div class="progress" id="progress"></div></div>
  <div id="timer" class="timer"></div>
  <div id="question" class="question"></div>
  <div id="options" class="options-grid"></div>
  <button id="nextBtn" onclick="submitAnswer()">Next</button>
  <div id="result"></div>
</div>

<!-- Success Sound -->
<audio id="winSound" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_c6b7b4cfb0.mp3?filename=success-1-6297.mp3"></audio>

<script>
const quizData = [
  { question: "Capital of France?", options: ["Paris", "Berlin", "Madrid", "Rome"], answer: "Paris" },
  { question: "Red Planet?", options: ["Earth", "Mars", "Venus", "Jupiter"], answer: "Mars" },
  { question: "Author of Hamlet?", options: ["Twain", "Shakespeare", "Dickens", "Austen"], answer: "Shakespeare" },
  { question: "Largest Continent", options: ["Asia", "Africa", "North America", "Europe"], answer: "Asia" },
  { question: "Capital of Belarus?", options: ["Kathmandu", "Minsk", "Chinsinau", "Kyiv"], answer: "Minsk" },
  { question: "Largest Lake?", options: ["Baikal", "Fewa Lake", "Caspian Sea", "Lake Superior"], answer: "Baikal" },
  { question: "Longest River?", options: ["Amazon", "Kaligandaki", "Nile", "Mississippi"], answer: "Nile" },
  { question: "Dumbest Animal?", options: ["Koala", "Ostrich", "Kiwi", "Sloth"], answer: "Ostrich" },
  { question: "Best Teacher?", options: ["Pratham", "Shiva", "Bishnu", "Pawan"], answer: "Pratham" },
  { question: "Who hit fastest 50 in IPL?", options: ["Virat Kohli", "MS Dhoni", "Yashasvi Jaiswal", "Rohit Sharma"], answer: "Yashasvi Jaiswal" },
  { question: "Cristiano Ronaldo total Goals?", options: ["999", "929", "969", "933"], answer: "933" },
  { question: "Most MOTO GP titles?", options: ["Valentinio Rossi", "	Marc Márquez", "Alex Márquez", "Giacomo Agostini"], answer: "Giacomo Agostini" },
  { question: "Largest Island?", options: ["Greenland", "New Guinea", "Hawaii", "Honshu"], answer: "Greenland" },
  { question: "Strongest Man?", options: ["Dwayne Johnson", "Bill Kazmaier", "Tarun Gill", "Tom Stoltman"], answer: "Tom Stoltman" },
  { question: "Heart of Computer?", options: ["Moniter", "Motherboard", "CPU", "Mouse"], answer: "CPU" },
  { question: "Oldest Temple?", options: ["Changu Narayan", "Temple of Hatshepsut", "Mundeswari Temple", "Göbekli Tepe"], answer: "Göbekli Tepe" }
];

let current = 0, score = 0, timer, timeLeft = 20;

function loadQuestion() {
  clearInterval(timer);
  timeLeft = 20;
  document.getElementById("timer").innerText = `Time Left: ${timeLeft}s`;
  timer = setInterval(() => {
    timeLeft--;
    document.getElementById("timer").innerText = `Time Left: ${timeLeft}s`;
    if (timeLeft <= 0) {
      clearInterval(timer);
      submitAnswer(true);
    }
  }, 1000);

  const q = quizData[current];
  document.getElementById("question").innerText = q.question;
  const options = document.getElementById("options");
  options.innerHTML = "";
  q.options.forEach(opt => {
    const id = `opt-${opt}`;
    options.innerHTML += `
      <label class="option">
        <input type="radio" name="option" value="${opt}" id="${id}" />
        <span>${opt}</span>
      </label>
    `;
  });

  document.getElementById("progress").style.width = `${(current / quizData.length) * 100}%`;
}

function submitAnswer(timeout = false) {
  clearInterval(timer);
  const selected = document.querySelector("input[name='option']:checked");
  if (!selected && !timeout) return alert("Please select an option!");
  if (selected && selected.value === quizData[current].answer) score++;
  current++;
  current < quizData.length ? loadQuestion() : showResult();
}

function showResult() {
  document.querySelector(".quiz-container").innerHTML = `
    <h2>Your Score: ${score}/${quizData.length}</h2>
    <p style="margin-top: 10px;">Well done!</p>
  `;
  confetti({ particleCount: 200, spread: 60, origin: { y: 0.6 } });

  const winSound = document.getElementById("winSound");
  winSound.play();
}

loadQuestion();
</script>

</body>
</html>
