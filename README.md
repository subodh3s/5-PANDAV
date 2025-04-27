<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Quiz App with Timer</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="quiz-container">
    <div class="top-bar">
      <div class="question-number">Question <span id="q-number">1</span></div>
      <div class="timer">Time left: <span id="time">15</span>s</div>
    </div>
    <h2 class="question" id="question">Question goes here</h2>
    <div class="options">
      <div class="option-box" data-option="a" id="a_text">Option A</div>
      <div class="option-box" data-option="b" id="b_text">Option B</div>
      <div class="option-box" data-option="c" id="c_text">Option C</div>
      <div class="option-box" data-option="d" id="d_text">Option D</div>
    </div>
    <button id="submit">Submit</button>
  </div>
  <script src="script.js"></script>
</body>
</html>
body {
  margin: 0;
  padding: 0;
  font-family: 'Arial', sans-serif;
  background: #121212;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  color: white;
}

.quiz-container {
  background: rgba(255, 255, 255, 0.05);
  padding: 30px;
  border-radius: 15px;
  width: 90%;
  max-width: 600px;
  backdrop-filter: blur(10px);
  box-shadow: 0 10px 30px rgba(0,0,0,0.3);
  box-sizing: border-box;
  text-align: center;
}

.top-bar {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  font-size: 16px;
}

.question {
  font-size: 22px;
  margin-bottom: 20px;
}

.options {
  display: grid;
  gap: 15px;
}

.option-box {
  background: #1e1e1e;
  padding: 15px;
  border-radius: 10px;
  cursor: pointer;
  transition: 0.3s;
  border: 2px solid transparent;
}

.option-box:hover {
  background: #2c2c2c;
}

.option-box.selected {
  border-color: #4caf50;
  background: #2e7d32;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  background: #4caf50;
  border: none;
  border-radius: 6px;
  color: white;
  font-size: 16px;
  cursor: pointer;
}

button:hover {
  background: #388e3c;
}
const quizData = [
  {
    question: "Which language runs in a browser?",
    a: "Java",
    b: "C",
    c: "Python",
    d: "JavaScript",
    correct: "d",
  },
  {
    question: "What does CSS stand for?",
    a: "Central Style Sheets",
    b: "Cascading Style Sheets",
    c: "Cascading Simple Sheets",
    d: "Computer Style Sheet",
    correct: "b",
  },
];

let currentQuiz = 0;
let score = 0;
let selectedAnswer = "";
let time = 15;
let timerInterval;

const questionEl = document.getElementById("question");
const options = document.querySelectorAll(".option-box");
const submitBtn = document.getElementById("submit");
const timeEl = document.getElementById("time");
const qNumEl = document.getElementById("q-number");

function loadQuiz() {
  clearInterval(timerInterval);
  time = 15;
  updateTimer();

  const current = quizData[currentQuiz];
  questionEl.innerText = current.question;
  qNumEl.innerText = currentQuiz + 1;
  document.getElementById("a_text").innerText = current.a;
  document.getElementById("b_text").innerText = current.b;
  document.getElementById("c_text").innerText = current.c;
  document.getElementById("d_text").innerText = current.d;

  deselectOptions();
  startTimer();
}

function startTimer() {
  timerInterval = setInterval(() => {
    time--;
    updateTimer();
    if (time === 0) {
      clearInterval(timerInterval);
      autoSubmit();
    }
  }, 1000);
}

function updateTimer() {
  timeEl.innerText = time;
}

function deselectOptions() {
  options.forEach(opt => opt.classList.remove("selected"));
  selectedAnswer = "";
}

options.forEach(option => {
  option.addEventListener("click", () => {
    deselectOptions();
    option.classList.add("selected");
    selectedAnswer = option.dataset.option;
  });
});

submitBtn.addEventListener("click", () => {
  if (!selectedAnswer) {
    alert("Select an answer!");
    return;
  }
  clearInterval(timerInterval);
  checkAnswer();
});

function checkAnswer() {
  if (selectedAnswer === quizData[currentQuiz].correct) {
    score++;
  }
  nextQuestion();
}

function autoSubmit() {
  alert("Time's up!");
  nextQuestion();
}

function nextQuestion() {
  currentQuiz++;
  if (currentQuiz < quizData.length) {
    loadQuiz();
  } else {
    showResults();
  }
}

function showResults() {
  document.querySelector(".quiz-container").innerHTML = `
    <h2>You've scored ${score} out of ${quizData.length}</h2>
    <button onclick="location.reload()">Try Again</button>
  `;
}

loadQuiz();

