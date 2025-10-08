<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Quiz Application</title>
    <style>
        /* CSS for basic styling */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f7f9;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
        }

        #quiz-container {
            background-color: #ffffff;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            width: 90%;
            max-width: 600px;
            text-align: center;
        }

        #question-element {
            font-size: 1.4em;
            margin-bottom: 25px;
            color: #333;
            font-weight: 600;
        }

        .options-container {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-bottom: 30px;
        }

        .option-btn {
            background-color: #e0e7ff;
            color: #4338ca;
            border: none;
            padding: 15px 20px;
            text-align: left;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s, transform 0.1s;
            font-size: 1em;
            font-weight: 500;
        }

        .option-btn:hover {
            background-color: #c7d2fe;
        }

        .option-btn.selected {
            background-color: #4f46e5;
            color: white;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }

        .control-btn {
            background-color: #10b981;
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1.1em;
            font-weight: 600;
            transition: background-color 0.3s;
        }

        .control-btn:hover {
            background-color: #059669;
        }

        #result-container {
            margin-top: 20px;
            font-size: 1.5em;
            font-weight: 700;
            color: #10b981;
        }

        /* Styling for correct/incorrect feedback (hidden initially) */
        .option-btn.correct {
            background-color: #34d399; /* Green */
            color: white;
            font-weight: bold;
        }
        .option-btn.incorrect {
            background-color: #f87171; /* Red */
            color: white;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <div id="quiz-container">
        <h1>Interactive Quiz</h1>
        <div id="question-element"></div>
        
        <div class="options-container" id="options-container">
            </div>

        <button class="control-btn" id="next-btn" onclick="nextQuestion()" disabled>Next Question</button>

        <div id="result-container" style="display: none;"></div>
    </div>

    <script>
        // JavaScript for Quiz Logic
        const quizData = [
            {
                question: "What does HTML stand for?",
                options: ["Hyper Text Markup Language", "High-Level Text Management", "Hyperlink and Text Markup", "Home Tool Markup Language"],
                answer: "Hyper Text Markup Language"
            },
            {
                question: "Which language is used for styling web pages?",
                options: ["JavaScript", "Python", "CSS", "SQL"],
                answer: "CSS"
            },
            {
                question: "What is the primary function of JavaScript?",
                options: ["To store data on a server", "To add interactivity to a webpage", "To define the structure of a webpage", "To manage network protocols"],
                answer: "To add interactivity to a webpage"
            },
            {
                question: "Which IBM technology is often used for data analysis and machine learning?",
                options: ["Watson", "Lotus Notes", "Tivoli", "WebSphere"],
                answer: "Watson"
            }
        ];

        let currentQuestionIndex = 0;
        let score = 0;
        let isAnswered = false;

        const questionEl = document.getElementById('question-element');
        const optionsContainer = document.getElementById('options-container');
        const nextButton = document.getElementById('next-btn');
        const resultContainer = document.getElementById('result-container');

        // --- Core Quiz Functions ---

        function startQuiz() {
            currentQuestionIndex = 0;
            score = 0;
            isAnswered = false;
            nextButton.textContent = "Next Question";
            nextButton.disabled = true;
            resultContainer.style.display = 'none';
            document.querySelector('h1').textContent = 'Interactive Quiz';
            
            displayQuestion();
        }

        function displayQuestion() {
            // Check if quiz is finished
            if (currentQuestionIndex >= quizData.length) {
                showResult();
                return;
            }

            const currentQuiz = quizData[currentQuestionIndex];
            questionEl.textContent = ${currentQuestionIndex + 1}. ${currentQuiz.question};
            optionsContainer.innerHTML = '';
            isAnswered = false;
            nextButton.disabled = true;

            // Create option buttons
            currentQuiz.options.forEach(option => {
                const button = document.createElement('button');
                button.textContent = option;
                button.classList.add('option-btn');
                button.onclick = () => selectOption(button, option, currentQuiz.answer);
                optionsContainer.appendChild(button);
            });
        }

        function selectOption(selectedButton, selectedAnswer, correctAnswer) {
            if (isAnswered) return; // Prevent changing selection after answering

            isAnswered = true;
            nextButton.disabled = false;
            
            // Disable all buttons to prevent further selection
            Array.from(optionsContainer.children).forEach(btn => btn.disabled = true);

            // Check if the answer is correct
            if (selectedAnswer === correctAnswer) {
                score++;
                selectedButton.classList.add('correct');
            } else {
                selectedButton.classList.add('incorrect');
                // Highlight the correct answer
                Array.from(optionsContainer.children).find(btn => btn.textContent === correctAnswer).classList.add('correct');
            }

            // Change button text for the last question
            if (currentQuestionIndex === quizData.length - 1) {
                nextButton.textContent = "Show Results";
            }
        }

        function nextQuestion() {
            currentQuestionIndex++;
            displayQuestion();
        }

        function showResult() {
            questionEl.textContent = "Quiz Finished!";
            optionsContainer.innerHTML = '';
            nextButton.textContent = "Restart Quiz";
            nextButton.onclick = startQuiz;
            nextButton.disabled = false;

            resultContainer.style.display = 'block';
            resultContainer.innerHTML = Your Score: ${score} out of ${quizData.length} questions correct.;

            document.querySelector('h1').textContent = 'Results';
        }

        // Start the quiz when the page loads
        document.addEventListener('DOMContentLoaded', startQuiz);
    </script>
</body>
</html>
