# quiz
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Indian Geography Quiz</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap');

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', sans-serif;
        }

        body {
            background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
            color: #fff;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .quiz-container {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 40px;
            width: 90%;
            max-width: 600px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            border-bottom: 2px solid rgba(255, 255, 255, 0.2);
            padding-bottom: 10px;
        }

        .header h2 {
            font-size: 1.2rem;
            color: #00d2ff;
        }

        .question-text {
            font-size: 1.5rem;
            font-weight: 600;
            margin-bottom: 20px;
            line-height: 1.4;
        }

        .options-container {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .option-btn {
            background: rgba(255, 255, 255, 0.05);
            border: 2px solid rgba(255, 255, 255, 0.2);
            padding: 15px 20px;
            border-radius: 8px;
            color: #fff;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: left;
        }

        .option-btn:hover:not(.disabled) {
            background: rgba(255, 255, 255, 0.2);
            transform: translateX(5px);
        }

        .option-btn.correct {
            background: rgba(46, 204, 113, 0.3);
            border-color: #2ecc71;
        }

        .option-btn.wrong {
            background: rgba(231, 76, 60, 0.3);
            border-color: #e74c3c;
        }

        .option-btn.disabled {
            cursor: not-allowed;
        }

        .feedback {
            margin-top: 15px;
            font-weight: 600;
            font-size: 1.1rem;
            min-height: 25px;
        }

        .feedback.correct-text { color: #2ecc71; }
        .feedback.wrong-text { color: #e74c3c; }

        .next-btn {
            background: #00d2ff;
            background: linear-gradient(to right, #3a7bd5, #3a6073);
            color: white;
            border: none;
            padding: 12px 30px;
            font-size: 1.1rem;
            border-radius: 8px;
            cursor: pointer;
            margin-top: 20px;
            float: right;
            display: none;
            transition: 0.3s;
        }

        .next-btn:hover {
            box-shadow: 0 0 15px rgba(58, 123, 213, 0.6);
        }

        /* Result Screen Styles */
        .result-container {
            text-align: center;
            display: none;
        }

        .result-container h1 {
            font-size: 2.5rem;
            color: #f1c40f;
            margin-bottom: 20px;
        }

        .result-container p {
            font-size: 1.3rem;
            margin: 10px 0;
        }

        .prize {
            font-size: 2rem;
            color: #2ecc71;
            font-weight: 600;
            margin: 20px 0;
            padding: 20px;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 10px;
            border: 1px dashed #2ecc71;
        }

        .restart-btn {
            background: #e67e22;
            color: white;
            border: none;
            padding: 12px 30px;
            font-size: 1.1rem;
            border-radius: 8px;
            cursor: pointer;
            margin-top: 20px;
            transition: 0.3s;
        }

        .restart-btn:hover {
            background: #d35400;
        }
    </style>
</head>
<body>

    <div class="quiz-container" id="quiz-box">
        <div class="header">
            <h2 id="question-tracker">Question 1 of 10</h2>
            <h2 id="score-tracker">Score: 0</h2>
        </div>

        <div class="question-text" id="question-text">
            Loading question...
        </div>

        <div class="options-container" id="options-container">
            <!-- Options will be generated here by JS -->
        </div>

        <div class="feedback" id="feedback"></div>

        <button class="next-btn" id="next-btn" onclick="nextQuestion()">Next Question ❯</button>
    </div>

    <div class="quiz-container result-container" id="result-box">
        <h1>QUIZ RESULT</h1>
        <p>Your Total Score</p>
        <h2 style="font-size: 2.5rem;" id="final-score">0 / 10</h2>
        
        <p>You have won</p>
        <div class="prize" id="prize-money">₹ 0</div>

        <button class="restart-btn" onclick="location.reload()">Play Again</button>
    </div>

    <script>
        const quizData = [
            { question: "Which is the highest mountain peak in India?", options: ["Nanda Devi", "Kanchenjunga", "Kamet", "Anamudi"], answer: 1 },
            { question: "Which river is known as the 'Sorrow of Bihar'?", options: ["Ganga", "Kosi", "Yamuna", "Godavari"], answer: 1 },
            { question: "In which state is the Sundarbans mainly located?", options: ["West Bengal", "Odisha", "Assam", "Bihar"], answer: 0 },
            { question: "Which is the largest state in India by area?", options: ["Madhya Pradesh", "Maharashtra", "Rajasthan", "Uttar Pradesh"], answer: 2 },
            { question: "Which mountain range separates India from the Tibetan Plateau?", options: ["Aravalli Range", "Western Ghats", "Himalayas", "Vindhya Range"], answer: 2 },
            { question: "Which is the largest freshwater lake in India?", options: ["Dal Lake", "Wular Lake", "Chilika Lake", "Loktak Lake"], answer: 1 },
            { question: "In which state is the Valley of Flowers National Park located?", options: ["Uttarakhand", "Himachal Pradesh", "Sikkim", "Arunachal Pradesh"], answer: 0 },
            { question: "Which Indian state has the longest coastline?", options: ["Gujarat", "Maharashtra", "Tamil Nadu", "Andhra Pradesh"], answer: 0 },
            { question: "Which desert is mainly located in Rajasthan?", options: ["Thar Desert", "Gobi Desert", "Ladakh Desert", "Deccan Desert"], answer: 0 },
            { question: "Which city is known as the 'Queen of the Arabian Sea'?", options: ["Kochi", "Mumbai", "Visakhapatnam", "Chennai"], answer: 0 }
        ];

        let currentQuestionIndex = 0;
        let score = 0;
        const alphabet = ['A', 'B', 'C', 'D'];

        const questionTracker = document.getElementById("question-tracker");
        const scoreTracker = document.getElementById("score-tracker");
        const questionText = document.getElementById("question-text");
        const optionsContainer = document.getElementById("options-container");
        const feedback = document.getElementById("feedback");
        const nextBtn = document.getElementById("next-btn");
        
        const quizBox = document.getElementById("quiz-box");
        const resultBox = document.getElementById("result-box");

        function loadQuestion() {
            // Reset state
            feedback.innerHTML = "";
            feedback.className = "feedback";
            nextBtn.style.display = "none";
            optionsContainer.innerHTML = "";

            const currentData = quizData[currentQuestionIndex];
            
            // Update trackers
            questionTracker.innerText = `Question ${currentQuestionIndex + 1} of ${quizData.length}`;
            scoreTracker.innerText = `Score: ${score}`;
            questionText.innerText = currentData.question;

            // Generate options
            currentData.options.forEach((option, index) => {
                const button = document.createElement("button");
                button.className = "option-btn";
                button.innerHTML = `<strong>${alphabet[index]}.</strong> ${option}`;
                button.onclick = () => selectOption(button, index);
                optionsContainer.appendChild(button);
            });
        }

        function selectOption(selectedButton, selectedIndex) {
            const currentData = quizData[currentQuestionIndex];
            const correctIndex = currentData.answer;
            const buttons = optionsContainer.children;

            // Disable all buttons
            for(let btn of buttons) {
                btn.classList.add("disabled");
                btn.onclick = null; // Remove click event
            }

            // Check if correct
            if(selectedIndex === correctIndex) {
                selectedButton.classList.add("correct");
                score++;
                feedback.innerText = "Correct! +1";
                feedback.className = "feedback correct-text";
            } else {
                selectedButton.classList.add("wrong");
                buttons[correctIndex].classList.add("correct");
                score--; // -1 for wrong answer as per your python logic
                feedback.innerHTML = `Wrong! -1 <br><span style="color:#aaa; font-size:0.9rem;">Correct answer was: ${currentData.options[correctIndex]}</span>`;
                feedback.className = "feedback wrong-text";
            }

            scoreTracker.innerText = `Score: ${score}`;
            
            // Show Next Button
            if (currentQuestionIndex === quizData.length - 1) {
                nextBtn.innerText = "See Results 🏆";
            }
            nextBtn.style.display = "block";
        }

        function nextQuestion() {
            currentQuestionIndex++;
            if (currentQuestionIndex < quizData.length) {
                loadQuestion();
            } else {
                showResults();
            }
        }

        function showResults() {
            quizBox.style.display = "none";
            resultBox.style.display = "block";

            document.getElementById("final-score").innerText = `${score} / 10`;

            let prize = 0;
            if (score === 10) prize = 10000;
            else if (score >= 8) prize = 5000;
            else if (score >= 6) prize = 2000;
            else if (score >= 4) prize = 1000;
            else if (score >= 1) prize = 500;
            else prize = 0;

            // Animate counting up for the prize money
            let currentPrize = 0;
            const prizeElement = document.getElementById("prize-money");
            const increment = prize / 50; // speed of count
            
            if (prize > 0) {
                const counter = setInterval(() => {
                    currentPrize += increment;
                    if(currentPrize >= prize) {
                        currentPrize = prize;
                        clearInterval(counter);
                    }
                    prizeElement.innerText = `₹ ${Math.floor(currentPrize).toLocaleString('en-IN')}`;
                }, 20);
            } else {
                prizeElement.innerText = `₹ 0`;
                prizeElement.style.color = "#e74c3c";
                prizeElement.style.borderColor = "#e74c3c";
            }
        }

        // Start Quiz
        loadQuestion();
    </script>
</body>
</html>
