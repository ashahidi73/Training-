<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <title>لعبة الخسوف التعليمية</title>
    <style>
        body {
            font-family: 'Tahoma', sans-serif;
            text-align: center;
            background-color: #1a1a2e;
            color: #e0e0e0;
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }
        h1 {
            color: #bb86fc;
            margin-bottom: 20px;
        }
        .game-container {
            background-color: #2e1a47;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
            max-width: 700px;
            width: 90%;
            margin-bottom: 20px;
        }
        #question {
            font-size: 1.5em;
            margin-bottom: 25px;
            color: #e0e0e0;
        }
        .options-container button {
            background-color: #03dac6;
            color: #1a1a2e;
            border: none;
            padding: 12px 25px;
            margin: 8px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1.1em;
            transition: background-color 0.3s ease, transform 0.2s ease;
        }
        .options-container button:hover {
            background-color: #01a296;
            transform: translateY(-2px);
        }
        #result {
            margin-top: 20px;
            font-size: 1.2em;
            font-weight: bold;
        }
        #score-display {
            font-size: 1.1em;
            margin-top: 15px;
            color: #bb86fc;
        }
        #explanation-image {
            max-width: 100%;
            height: auto;
            margin-top: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
            display: none; /* Hidden by default */
        }
        .navigation-buttons button {
            background-color: #6200ee;
            color: #fff;
            border: none;
            padding: 10px 20px;
            margin: 10px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.3s ease;
        }
        .navigation-buttons button:hover {
            background-color: #3700b3;
        }
    </style>
</head>
<body>
    <h1>🎮 لعبة اكتشف الخسوف!</h1>
    <div class="game-container">
        <p id="question">Loading question...</p>
        <div class="options-container" id="options-container">
            <!-- Buttons will be dynamically loaded here -->
        </div>
        <p id="result"></p>
        <p id="score-display">النقاط: 0</p>
        <img id="explanation-image" src="" alt="تفسير الخسوف">
        <div class="navigation-buttons">
            <button id="next-question-btn" onclick="nextQuestion()" style="display: none;">السؤال التالي</button>
            <button id="restart-game-btn" onclick="startGame()" style="display: none;">إبدأ من جديد</button>
        </div>
    </div>

    <script>
        const questions = [
            {
                question: "ما السبب الرئيسي لحدوث الخسوف القمري؟",
                options: [
                    { text: "وجود الشمس بين الأرض والقمر", correct: false },
                    { text: "وجود الأرض بين الشمس والقمر", correct: true, explanationImage: "lunar_eclipse.png" }, // تأكد أن هذا الاسم يطابق اسم ملف الصورة
                    { text: "وجود القمر بين الأرض والشمس", correct: false }
                ]
            },
            {
                question: "في أي طور يكون القمر عند حدوث الخسوف القمري؟",
                options: [
                    { text: "هلال", correct: false },
                    { text: "محاق", correct: false },
                    { text: "بدر", correct: true }
                ]
            },
            {
                question: "لماذا لا يحدث الخسوف القمري في كل شهر؟",
                options: [
                    { text: "بسبب سرعة دوران الأرض", correct: false },
                    { text: "بسبب ميلان مدار القمر حول الأرض", correct: true },
                    { text: "بسبب حجم الشمس الكبير", correct: false }
                ]
            }
        ];

        let currentQuestionIndex = 0;
        let score = 0;

        let questionElement;
        let optionsContainer;
        let resultElement;
        let scoreDisplay;
        let explanationImage;
        let nextQuestionBtn;
        let restartGameBtn;

        function initializeElements() {
            questionElement = document.getElementById("question");
            optionsContainer = document.getElementById("options-container");
            resultElement = document.getElementById("result");
            scoreDisplay = document.getElementById("score-display");
            explanationImage = document.getElementById("explanation-image");
            nextQuestionBtn = document.getElementById("next-question-btn");
            restartGameBtn = document.getElementById("restart-game-btn");
        }

        function startGame() {
            initializeElements();
            currentQuestionIndex = 0;
            score = 0;
            scoreDisplay.textContent = `النقاط: ${score}`;
            restartGameBtn.style.display = 'none';
            explanationImage.style.display = 'none';
            loadQuestion();
        }

        function loadQuestion() {
            resultElement.textContent = "";
            nextQuestionBtn.style.display = 'none';
            explanationImage.style.display = 'none';

            if (currentQuestionIndex >= questions.length) {
                endGame();
                return;
            }

            const currentQ = questions[currentQuestionIndex];
            questionElement.textContent = currentQ.question;
            optionsContainer.innerHTML = "";

            currentQ.options.forEach((option, index) => {
                const button = document.createElement("button");
                button.textContent = option.text;
                button.onclick = () => checkAnswer(option.correct, option.explanationImage);
                optionsContainer.appendChild(button);
            });
        }

        function checkAnswer(isCorrect, imageFilename) {
            Array.from(optionsContainer.children).forEach(button => {
                button.disabled = true;
            });

            if (isCorrect) {
                resultElement.textContent = "إجابتك صحيحة ✅ أحسنت!";
                score++;
                scoreDisplay.textContent = `النقاط: ${score}`;
                if (imageFilename) {
                    explanationImage.src = imageFilename; // هذا هو السطر المعدّل
                    explanationImage.style.display = 'block';
                }
            } else {
                resultElement.textContent = "❌ إجابتك خاطئة. حاول مرة أخرى!";
            }
            nextQuestionBtn.style.display = 'block';
        }

        function nextQuestion() {
            currentQuestionIndex++;
            loadQuestion();
        }

        function endGame() {
            questionElement.textContent = `لقد أنهيت اللعبة! مجموع نقاطك: ${score} من ${questions.length}`;
            optionsContainer.innerHTML = "";
            resultElement.textContent = "";
            nextQuestionBtn.style.display = 'none';
            restartGameBtn.style.display = 'block';
            explanationImage.style.display = 'none';
        }

        document.addEventListener('DOMContentLoaded', startGame);
    </script>
</body>
</html>
