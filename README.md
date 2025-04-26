<html>
<head>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            color: #333;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .quiz-container {
            background: #fff;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            width: 90%;
            max-width: 600px;
            padding: 20px;
            text-align: center;
            box-sizing: border-box;
        }

        .question {
            font-size: 1.2em;
            margin-bottom: 20px;
        }

        .options {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .option {
            padding: 10px;
            border: 2px solid #ccc;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s, color 0.3s;
        }

        .option:hover {
            background-color: #007bff;
            color: #fff;
        }

        .timer {
            font-size: 1.2em;
            margin-bottom: 20px;
            color: #ff5722;
        }

        .result {
            font-size: 1.5em;
            color: #4caf50;
            display: none;
        }

        .restart-btn {
            background-color: #007bff;
            color: #fff;
            border: none;
            padding: 10px 20px;
            font-size: 1em;
            border-radius: 8px;
            cursor: pointer;
            margin-top: 20px;
            display: none;
        }

        .restart-btn:hover {
            background-color: #0056b3;
        }
    </style>
</head>

<body>
    <div class="quiz-container">
        <div class="timer">Time Left: <span id="time">30</span>s</div>
        <div class="question">Question will appear here</div>
        <div class="options"></div>
        <div class="result">Your score: <span id="score">0</span></div>
        <button class="restart-btn">Restart Quiz</button>
    </div>
    <script>
        const quizData = [
            {
                question: "What is the capital of France?",
                options: ["Berlin", "Madrid", "Paris", "Lisbon"],
                answer: "Paris"
            },
            {
                question: "Which language is used for web development?",
                options: ["Python", "HTML", "Java", "C++"],
                answer: "HTML"
            },
            {
                question: "Who wrote 'Hamlet'?",
                options: ["Charles Dickens", "William Shakespeare", "Mark Twain", "Jane Austen"],
                answer: "William Shakespeare"
            },
            {
                question: "What is the largest planet in our solar system?",
                options: ["Earth", "Mars", "Jupiter", "Saturn"],
                answer: "Jupiter"
            },
            {
                question: "Which country is known as the Land of the Rising Sun?",
                options: ["China", "Japan", "South Korea", "India"],
                answer: "Japan"
            },
            {
                question: "Who is the best computer teacher?",
                options: ["Pratham", "Bishnu", "Shiva", "Pawan"],
                answer: "Pratham"
            },
            {
                question: "When did Pratham Sir joined CLEBS?",
                options: ["2065", "1969", "2003", "2081"],
                answer: "2081"
            },
             {
                question: "When was KP OLI born?",
                options: ["1957 August 11", "1969 April 1", "1952 February 22", "2025 April 29"],
                answer: "1952 February 22"
            },
             {
                question: "What is the main comonent of Computer?",
                options: ["Carburetor", "Air Filter", "CPU", "Clutch Plate"],
                answer: "CPU"
            },
            {
                question: "Which one of the following achieves the speed of 183 kmph in first gear?",
                options: ["Yamaha R1", "Aprillia RS V4", "GSX-R 1000", "CBR 1000RR-R FIREBLADE"],
                answer: "CBR 1000RR-R FIREBLADE"
            },
            {
                question: "Which is the richest Football club?",
                options: ["Aston Vilaa", "FC Barcelona", "Lazio FC", "Real Madrid CF"],
                answer: "Real Madrid CF"
            },
            {
                question: "When was Nepal decleared as Secular state?",
                options: ["2001 Januray 17", "2006 May 26", "2013 October 7", "2008 May 18"],
                answer: "2008 May 18"
            },
             {
                question: "When was Cristiano Ronaldo born?",
                options: ["2001 Januray 17", "2006 May 26", "1985 February 5", "2008 May 18"],
                answer: "1985 February 5"
            },
             {
                question: "Who invented Coca-Cola?",
                options: ["Jhon Stith Pemberton", "Celeb Bradham", "Charles Alderton", "Bob Stiller"],
                answer: "Jhon Stith Pemberton"
            },
             {
                question: "Who was the founder of Apple?",
                options: ["John von Neumann ", "Jeff Bezos", "Binod Chaudhary", "Steve Jobs"],
                answer: "Steve Jobs"
            },
             {
                question: "Which man has the most trophies in football?",
                options: ["Lional Messi", "Cristiano Ronaldo", "Harry Magurie", "N'Golo Kanté"],
                answer: "Lional Messi"
            },
            {
                question: "Who was the first Programmer?",
                options: ["Lady Ada Lovelace", "John Von Neumann", "Charles Babbage", "Albert Einstein"],
                answer: "Lady Ada Lovalace"
            },
              {
                question: "Who has the most wins in MOTO GP?",
                options: ["Valentinio Rossi 46", "Marc Márquez 93", "Giacomo Agostini 1", "Fabio Quartararo 20"],
                answer: "Giacomo Agostini 1"
            },
            {
                question: "When was World War I ended?",
                options: ["1869 July 23", "1914 July 28", "1941 August 21", "1491 January 1"],
                answer: "1914 July 28"
            },
             {
                question: "When was World War I ended?",
                options: ["1869 July 23", "1914 July 28", "1941 August 21", "1491 January 1"],
                answer: "1914 July 28"
            },
             {
                question: "Which of the following countries was first to use a symbol for Zero(0)?",
                options: ["United Kingdom", "India", "Japan", "China"],
                answer: "India"
            },
        ];

        let currentQuestion = 0;
        let score = 0;
        let timeLeft = 30;
        let timerInterval;
        const timerEl = document.getElementById('time');
        const questionEl = document.querySelector('.question');
        const optionsEl = document.querySelector('.options');
        const resultEl = document.querySelector('.result');
        const scoreEl = document.getElementById('score');
        const restartBtn = document.querySelector('.restart-btn');

        // Function to load the question
        function loadQuestion() {
            if (currentQuestion >= quizData.length) {
                endQuiz();
                return;
            }
            clearInterval(timerInterval);
            timeLeft = 30;
            timerEl.textContent = timeLeft;
            startTimer();
            const currentQuiz = quizData[currentQuestion];
            questionEl.textContent = currentQuiz.question;
            optionsEl.innerHTML = ''; // Clear previous options
            currentQuiz.options.forEach(option => {
                const button = document.createElement('button');
                button.classList.add('option');
                button.textContent = option;
                button.onclick = () => checkAnswer(option);
                optionsEl.appendChild(button);
            });
        }

        // Check the answer
        function checkAnswer(selectedOption) {
            if (selectedOption === quizData[currentQuestion].answer) {
                score++;
            }
            currentQuestion++;
            loadQuestion();
        }

        // Start the timer
        function startTimer() {
            timerInterval = setInterval(() => {
                timeLeft--;
                timerEl.textContent = timeLeft;
                if (timeLeft <= 0) {
                    clearInterval(timerInterval);
                    endQuiz();
                }
            }, 1000);
        }

        // End the quiz and show the results
        function endQuiz() {
            clearInterval(timerInterval);
            questionEl.style.display = 'none';
            optionsEl.style.display = 'none';
            resultEl.style.display = 'block';
            scoreEl.textContent = score;
            restartBtn.style.display = 'block';
        }

        // Restart the quiz
        restartBtn.addEventListener('click', () => {
            // Reset variables
            currentQuestion = 0;
            score = 0;
            timeLeft = 30;
            timerEl.textContent = timeLeft;

            // Reset the display
            questionEl.style.display = 'block';
            optionsEl.style.display = 'flex'; // Ensure options are displayed correctly
            resultEl.style.display = 'none';
            restartBtn.style.display = 'none';

            // Load the first question
            loadQuestion();
        });

        // Initialize the quiz with the first question
        loadQuestion();
    </script>
</body>

</html>
