<!DOCTYPE html>
<html lang="kk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Қазақ Тесті</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            max-width: 600px;
            width: 100%;
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 {
            font-size: 28px;
            margin-bottom: 10px;
        }

        .progress-bar {
            background: rgba(255,255,255,0.3);
            height: 8px;
            border-radius: 4px;
            margin-top: 15px;
            overflow: hidden;
        }

        .progress {
            background: white;
            height: 100%;
            width: 0%;
            border-radius: 4px;
            transition: width 0.3s ease;
        }

        .quiz-content {
            padding: 30px;
        }

        .question {
            font-size: 20px;
            color: #333;
            margin-bottom: 25px;
            line-height: 1.5;
        }

        .question-number {
            color: #667eea;
            font-weight: bold;
            font-size: 14px;
            text-transform: uppercase;
            margin-bottom: 10px;
            display: block;
        }

        .options {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .option {
            background: #f8f9fa;
            border: 2px solid #e9ecef;
            border-radius: 12px;
            padding: 18px 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .option:hover {
            border-color: #667eea;
            background: #f0f4ff;
            transform: translateX(5px);
        }

        .option.selected {
            border-color: #667eea;
            background: #667eea;
            color: white;
        }

        .option-letter {
            width: 32px;
            height: 32px;
            background: #667eea;
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            flex-shrink: 0;
        }

        .option.selected .option-letter {
            background: white;
            color: #667eea;
        }

        .buttons {
            display: flex;
            gap: 15px;
            margin-top: 30px;
        }

        .btn {
            flex: 1;
            padding: 15px 25px;
            border: none;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.3);
        }

        .btn-secondary {
            background: #e9ecef;
            color: #495057;
        }

        .btn-secondary:hover {
            background: #dee2e6;
        }

        .result-screen {
            display: none;
            text-align: center;
            padding: 40px;
        }

        .result-icon {
            font-size: 80px;
            margin-bottom: 20px;
        }

        .score {
            font-size: 48px;
            font-weight: bold;
            color: #667eea;
            margin: 20px 0;
        }

        .result-message {
            font-size: 18px;
            color: #666;
            margin-bottom: 30px;
        }

        .hidden {
            display: none;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .fade-in {
            animation: fadeIn 0.5s ease;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Негізгі экран -->
        <div id="start-screen" class="quiz-content" style="text-align: center;">
            <div style="font-size: 60px; margin-bottom: 20px;">🎓</div>
            <h2 style="color: #333; margin-bottom: 15px;">Қазақ тілінен тест</h2>
            <p style="color: #666; margin-bottom: 30px; line-height: 1.6;">
                Бұл тест қазақ тілінің грамматикасын тексеруге арналған. 
                Барлығы 5 сұрақ. Сәттілік!
            </p>
            <button class="btn btn-primary" onclick="startQuiz()" style="width: 100%;">
                Тестті бастау
            </button>
        </div>

        <!-- Тест экраны -->
        <div id="quiz-screen" class="hidden">
            <div class="header">
                <h1>📝 Тест</h1>
                <div class="progress-bar">
                    <div class="progress" id="progress"></div>
                </div>
            </div>
            <div class="quiz-content">
                <span class="question-number">Сұрақ <span id="current-q">1</span> / <span id="total-q">5</span></span>
                <div class="question" id="question-text"></div>
                <div class="options" id="options"></div>
                <div class="buttons">
                    <button class="btn btn-secondary" onclick="previousQuestion()" id="prev-btn">Алдыңғы</button>
                    <button class="btn btn-primary" onclick="nextQuestion()" id="next-btn">Келесі</button>
                </div>
            </div>
        </div>

        <!-- Нәтиже экраны -->
        <div id="result-screen" class="result-screen">
            <div class="result-icon" id="result-icon">🎉</div>
            <h2 style="color: #333; margin-bottom: 10px;">Тест аяқталды!</h2>
            <div class="score" id="score">0/5</div>
            <p class="result-message" id="result-message"></p>
            <button class="btn btn-primary" onclick="restartQuiz()" style="width: 100%;">
                Қайта бастау
            </button>
        </div>
    </div>

    <script>
        // Сұрақтар базасы
        const questions = [
            {
                question: "«Қазақ» сөзінің мағынасы не?",
                options: [
                    "Еркін, бостандық сүйгіш",
                    "Таулы жер",
                    "Су жағасында",
                    "Орманды жер"
                ],
                correct: 0
            },
            {
                question: "Қазақ тілінде неше септік бар?",
                options: [
                    "5",
                    "6",
                    "7",
                    "8"
                ],
                correct: 2
            },
            {
                question: "«Алматы» сөзінің мағынасы не?",
                options: [
                    "Алма орны",
                    "Алма ағашы",
                    "Алма беретін",
                    "Алмалы жер"
                ],
                correct: 0
            },
            {
                question: "Қазақ әліпбиінде неше әріп бар?",
                options: [
                    "30",
                    "33",
                    "42",
                    "28"
                ],
                correct: 2
            },
            {
                question: "«Күн» сөзінің көпше түрі қалай жазылады?",
                options: [
                    "Күннер",
                    "Күндер",
                    "Күнде",
                    "Күндіз"
                ],
                correct: 1
            }
        ];

        let currentQuestion = 0;
        let answers = [];
        let score = 0;

        function startQuiz() {
            document.getElementById('start-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            document.getElementById('quiz-screen').classList.add('fade-in');
            document.getElementById('total-q').textContent = questions.length;
            showQuestion();
        }

        function showQuestion() {
            const q = questions[currentQuestion];
            document.getElementById('current-q').textContent = currentQuestion + 1;
            document.getElementById('question-text').textContent = q.question;
            
            // Прогресс бар
            const progress = ((currentQuestion + 1) / questions.length) * 100;
            document.getElementById('progress').style.width = progress + '%';

            // Жауап нұсқалары
            const optionsDiv = document.getElementById('options');
            optionsDiv.innerHTML = '';
            
            const letters = ['A', 'B', 'C', 'D'];
            q.options.forEach((opt, index) => {
                const div = document.createElement('div');
                div.className = 'option';
                if (answers[currentQuestion] === index) {
                    div.classList.add('selected');
                }
                div.onclick = () => selectOption(index);
                div.innerHTML = `
                    <span class="option-letter">${letters[index]}</span>
                    <span>${opt}</span>
                `;
                optionsDiv.appendChild(div);
            });

            // Батырмаларды басқару
            document.getElementById('prev-btn').style.visibility = 
                currentQuestion === 0 ? 'hidden' : 'visible';
            document.getElementById('next-btn').textContent = 
                currentQuestion === questions.length - 1 ? 'Аяқтау' : 'Келесі';
        }

        function selectOption(index) {
            answers[currentQuestion] = index;
            showQuestion();
        }

        function nextQuestion() {
            if (currentQuestion < questions.length - 1) {
                currentQuestion++;
                showQuestion();
            } else {
                showResults();
            }
        }

        function previousQuestion() {
            if (currentQuestion > 0) {
                currentQuestion--;
                showQuestion();
            }
        }

        function showResults() {
            score = 0;
            answers.forEach((ans, idx) => {
                if (ans === questions[idx].correct) score++;
            });

            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').style.display = 'block';
            document.getElementById('result-screen').classList.add('fade-in');
            
            document.getElementById('score').textContent = `${score}/${questions.length}`;
            
            let message = '';
            let icon = '';
            if (score === questions.length) {
                message = 'Керемет! Сіз барлық сұраққа дұрыс жауап бердіңіз! 🌟';
                icon = '🏆';
            } else if (score >= questions.length * 0.7) {
                message = 'Жақсы нәтиже! Сіз қазақ тілін жақсы білесіз! 👍';
                icon = '🎉';
            } else if (score >= questions.length * 0.4) {
                message = 'Жақсы бастама, бірақ тағы да үйрену керек! 📚';
                icon = '🤔';
            } else {
                message = 'Қазақ тілін үйренуге уақыт келді! Қайта байқап көріңіз! 💪';
                icon = '📖';
            }
            
            document.getElementById('result-message').textContent = message;
            document.getElementById('result-icon').textContent = icon;
        }

        function restartQuiz() {
            currentQuestion = 0;
            answers = [];
            score = 0;
            document.getElementById('result-screen').style.display = 'none';
            document.getElementById('start-screen').classList.remove('hidden');
            document.getElementById('start-screen').classList.add('fade-in');
        }
    </script>
</body>
</html>
