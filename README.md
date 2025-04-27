<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Quiz App</title>
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap');

  body {
  background-color: #f4f4f4;
  font-family: 'Arial', sans-serif;
  display: flex;
  justify-content: center;   /* center horizontally */
  align-items: center;       /* center vertically */
  height: 100vh;             /* full viewport height */
  margin: 0;                 /* remove default margin */
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
  background: #fff;
  padding: 20px 40px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  width: 400px;
  text-align: left; /* optional: for left-aligned text inside */
}

    .question {
      font-size: 28px;
      font-weight: bold;
      margin-bottom: 20px;
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
      box-shadow: 0 0 15px rgba(79, 195, 247, 0.7)
      transform: transformY(-2px);
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
  <!-- 20 lines at different positions -->
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

<script>
const quizData = [
  { question: "Capital of France?", options: ["Paris", "Berlin", "Madrid", "Rome"], answer: "Paris" },
  { question: "Red Planet?", options: ["Earth", "Mars", "Venus", "Jupiter"], answer: "Mars" },
  { question: "Author of Hamlet?", options: ["Twain", "Shakespeare", "Dickens", "Austen"], answer: "Shakespeare" },
  { question: "Father of Ai?", options: ["Patanjali Baba", "John McCarthy", "Charles Babbage", "Albert Einstein"], answer: "John McCarthy" },
  { question: "Best computer teacher of CLEBS?", options: ["Pratham", "Shiva", "Bishnu", "Devi"], answer: "Pratham" },
  { question: "Capital of Belarus?", options: ["Minsk", "Vilnius", "Brasília", "Chisinau"], answer: "Minsk" },
  { question: "Largest Lake?", options: ["Caspian Sea", "Baikal", "Lake Superior", "Ontario"], answer: "Baikal" },
  { question: "Longest River?", options: ["Amazon", "Nile", "Kaligandaki", "Mississippi"], answer: "Nile" },
  { question: "Gas used to Extinguish Fire?", options: ["Nitrogen", "Oxygen", "Carbon Dioxide", "Hydrogen"], answer: "Carbon Dioxide" },
  { question: "National Animal of Australia?", options: ["Kangaroo", "Panda", "Zebra", "Giraffe"], answer: "Kangaroo" },
  { question: "What does Entomology deals with?", options: ["The study of Insects", "The study of Behaviour of Human Beings", "The study of rocks", "The study of Nature"], answer: "The study of Insects" },
  { question: " Hitler's party is known as?", options: ["Labour Party", "Nazi Party", "Ku-Klux-Klan", "Democratic Party"], answer: "Nazi Party" },
  { question: " When the First Afghan War took place in?", options: ["1969", "1839", "1843", "1848"], answer: "1839" },
  { question: "Largest Island?", options: ["New Guinea", "Greenland", "Andaman Nicobar", "Hawaii"], answer: "Greenland" },
  { question: "Hottest Continent?", options: ["South Asia", "Africa", "Australia", "North America"], answer: "Africa" },
  { question: "Largest Continent?", options: ["South Asia", "Africa", "Australia", "North America"], answer: "South Asia" },
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
}

loadQuestion();
</script>

</body>

