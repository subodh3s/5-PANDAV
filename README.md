
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quiz App</title>
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap');body {
  font-family: 'Poppins', sans-serif;
  background: linear-gradient(135deg, #89f7fe, #66a6ff);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  overflow: hidden;
  position: relative;
}
.quiz-container {
  background: rgba(255, 255, 255, 0.95);
  padding: 40px 30px;
  border-radius: 20px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
  width: 90%;
  max-width: 450px;
  text-align: center;
  animation: popUp 0.8s ease;
}
@keyframes popUp {
  0% { transform: scale(0.8); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}
.question {
  font-size: 24px;
  font-weight: 700;
  color: #222;
  margin-bottom: 25px;
}
.option {
  background: #f7f7f7;
  margin: 10px 0;
  padding: 15px 20px;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 16px;
  font-weight: 500;
  display: flex;
  align-items: center;
  border: 2px solid transparent;
}
.option:hover {
  background: #e0f0ff;
  border-color: #4a90e2;
  transform: translateY(-2px);
}
input[type="radio"] {
  margin-right: 10px;
  transform: scale(1.3);
  accent-color: #4a90e2;
}
button {
  margin-top: 25px;
  padding: 14px;
  width: 100%;
  background: linear-gradient(to right, #4a90e2, #89f7fe);
  color: white;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-size: 18px;
  font-weight: 700;
  letter-spacing: 1px;
  transition: all 0.3s ease;
}
button:hover {
  background: linear-gradient(to right, #66a6ff, #a2d4ff);
  color: #333;
}
#result {
  margin-top: 20px;
  font-size: 22px;
  color: #27ae60;
  font-weight: bold;
  animation: fadeIn 0.8s ease forwards;
}
@keyframes fadeIn {
  from {opacity: 0;}
  to {opacity: 1;}
}
.progress-bar {
  width: 100%;
  background: #ddd;
  height: 10px;
  border-radius: 5px;
  margin-bottom: 20px;
  overflow: hidden;
}
.progress {
  height: 100%;
  width: 0%;
  background: linear-gradient(to right, #4a90e2, #89f7fe);
  transition: width 0.5s ease;
}
.timer {
  font-size: 16px;
  margin-bottom: 20px;
  color: #4a90e2;
  font-weight: 600;
}

  </style>
</head>
<body>
  <div class="quiz-container">
    <div class="progress-bar">
      <div class="progress" id="progress"></div>
    </div>
    <div id="timer" class="timer"></div>
    <div id="question" class="question"></div>
    <div id="options"></div>
    <button onclick="submitAnswer()">Submit</button>
    <div id="result"></div>
  </div>  <script>
    const quizData = [
      {
        question: "What is the capital of France?",
        options: ["Paris", "London", "Rome", "Berlin"],
        answer: "Paris"
      },
      {
        question: "Which planet is known as the Red Planet?",
        options: ["Earth", "Mars", "Jupiter", "Saturn"],
        answer: "Mars"
      },
      {
        question: "Who wrote 'Hamlet'?",
        options: ["Charles Dickens", "William Shakespeare", "Jane Austen", "Mark Twain"],
        answer: "William Shakespeare"
      }
    ];

    let currentQuestion = 0;
    let score = 0;
    let timerInterval;
    let timeLeft = 20;

    function loadQuestion() {
      clearInterval(timerInterval);
      timeLeft = 20;
      document.getElementById("timer").innerText = `Time Left: ${timeLeft}s`;
      timerInterval = setInterval(updateTimer, 1000);

      const q = quizData[currentQuestion];
      document.getElementById("question").innerText = q.question;
      const optionsDiv = document.getElementById("options");
      optionsDiv.innerHTML = "";
      q.options.forEach(option => {
        optionsDiv.innerHTML += `<div class='option'><label><input type='radio' name='option' value='${option}'> ${option}</label></div>`;
      });
      updateProgress();
    }

    function updateTimer() {
      timeLeft--;
      document.getElementById("timer").innerText = `Time Left: ${timeLeft}s`;
      if (timeLeft <= 0) {
        clearInterval(timerInterval);
        submitAnswer(true);
      }
    }

    function submitAnswer(timeout = false) {
      clearInterval(timerInterval);
      const selected = document.querySelector("input[name='option']:checked");
      if (!selected && !timeout) return alert("Please select an option!");
      if (selected && selected.value === quizData[currentQuestion].answer) {
        score++;
      }
      currentQuestion++;
      if (currentQuestion < quizData.length) {
        loadQuestion();
      } else {
        showResult();
      }
    }

    function updateProgress() {
      const progress = document.getElementById('progress');
      const percent = (currentQuestion / quizData.length) * 100;
      progress.style.width = percent + '%';
    }

    function showResult() {
      document.querySelector(".quiz-container").innerHTML = `<h2>Your Score: ${score}/${quizData.length}</h2><p style='margin-top:20px;'>Thanks for playing!</p>`;
      launchConfetti();
    }

    function launchConfetti() {
      confetti({
        particleCount: 200,
        spread: 70,
        origin: { y: 0.6 },
        colors: ['#4a90e2', '#66a6ff', '#a2d4ff']
      });
    }

    loadQuestion();
  </script></body>
</html>
