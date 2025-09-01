<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vocabulary Game: English Expressions</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            min-height: 100vh;
            padding: 20px;
            color: #333;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #6a11cb, #2575fc);
            color: white;
            padding: 30px;
            text-align: center;
            position: relative;
        }

        .header::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="20" cy="20" r="2" fill="rgba(255,255,255,0.1)"/><circle cx="80" cy="30" r="1.5" fill="rgba(255,255,255,0.1)"/><circle cx="40" cy="70" r="1" fill="rgba(255,255,255,0.1)"/><circle cx="90" cy="80" r="2.5" fill="rgba(255,255,255,0.1)"/></svg>');
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            position: relative;
            z-index: 1;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 10px;
            padding: 20px;
            background: #f8f9fa;
            flex-wrap: wrap;
        }

        .nav-btn {
            padding: 12px 24px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .nav-btn.active {
            background: linear-gradient(135deg, #6a11cb, #2575fc);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(106, 17, 203, 0.4);
        }

        .nav-btn:not(.active) {
            background: white;
            color: #666;
        }

        .nav-btn:hover:not(.active) {
            background: #e9ecef;
            transform: translateY(-1px);
        }

        .game-section {
            display: none;
            padding: 30px;
            min-height: 400px;
        }

        .game-section.active {
            display: block;
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .word-bank {
            background: linear-gradient(135deg, #f5f7fa, #e8f4f2);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 25px;
            border: 2px solid #6a11cb;
            box-shadow: 0 4px 15px rgba(106, 17, 203, 0.2);
        }

        .word-bank h3 {
            color: #6a11cb;
            margin-bottom: 15px;
            text-align: center;
            font-size: 1.4em;
        }

        .word-options {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            justify-content: center;
        }

        .word-option {
            background: linear-gradient(135deg, #6a11cb, #2575fc);
            color: white;
            padding: 10px 18px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 16px;
            box-shadow: 0 3px 10px rgba(106, 17, 203, 0.3);
            transition: all 0.3s ease;
            cursor: default;
        }

        .word-option:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(106, 17, 203, 0.4);
        }

        .question {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 20px;
            border-left: 5px solid #6a11cb;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
        }

        .question h3 {
            color: #6a11cb;
            margin-bottom: 15px;
            font-size: 1.3em;
        }

        .fill-blank {
            background: #fff;
            border: 2px solid #ddd;
            border-radius: 8px;
            padding: 8px 12px;
            font-size: 16px;
            margin: 0 5px;
            min-width: 120px;
            transition: border-color 0.3s ease;
        }

        .fill-blank.correct {
            border-color: #4CAF50;
            background: #e8f5e8;
        }

        .fill-blank.incorrect {
            border-color: #f44336;
            background: #ffeaea;
        }

        .options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .option {
            padding: 15px 20px;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .option:hover {
            border-color: #6a11cb;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(106, 17, 203, 0.2);
        }

        .option.selected {
            background: #6a11cb;
            color: white;
            border-color: #6a11cb;
        }

        .option.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .option.incorrect {
            background: #f44336;
            color: white;
            border-color: #f44336;
        }

        .matching-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-top: 20px;
        }

        .match-column h4 {
            text-align: center;
            margin-bottom: 15px;
            color: #6a11cb;
            font-size: 1.2em;
        }

        .match-item {
            padding: 15px;
            margin: 8px 0;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .match-item:hover {
            border-color: #6a11cb;
            transform: translateY(-1px);
            box-shadow: 0 4px 15px rgba(106, 17, 203, 0.2);
        }

        .match-item.selected {
            background: #e8f4f2;
            border-color: #6a11cb;
        }

        .match-item.matched {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
            cursor: default;
        }

        .check-btn {
            background: linear-gradient(135deg, #6a11cb, #2575fc);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 20px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(106, 17, 203, 0.3);
        }

        .check-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(106, 17, 203, 0.4);
        }

        .feedback {
            margin-top: 15px;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
            text-align: center;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .score {
            position: fixed;
            top: 20px;
            right: 20px;
            background: linear-gradient(135deg, #6a11cb, #2575fc);
            color: white;
            padding: 15px 20px;
            border-radius: 25px;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(106, 17, 203, 0.3);
            z-index: 1000;
        }

        @media (max-width: 768px) {
            .matching-container {
                grid-template-columns: 1fr;
                gap: 20px;
            }
            
            .nav-buttons {
                flex-direction: column;
                align-items: center;
            }
            
            .nav-btn {
                width: 200px;
            }
            
            .score {
                position: static;
                margin: 20px auto;
                display: block;
                width: fit-content;
            }
        }
    </style>
</head>
<body>
    <div class="score">Score: <span id="score">0</span>/18</div>
    
    <div class="container">
        <div class="header">
            <h1>📚 English Expressions Game</h1>
            <p>Test your knowledge of these useful English expressions!</p>
        </div>

        <div class="nav-buttons">
            <button class="nav-btn active" onclick="showSection('fill-gaps')">Fill in the Gaps</button>
            <button class="nav-btn" onclick="showSection('matching')">Match Meanings</button>
            <button class="nav-btn" onclick="showSection('multiple-choice')">Multiple Choice</button>
        </div>

        <!-- Fill in the Gaps Section -->
        <div id="fill-gaps" class="game-section active">
            <div class="word-bank">
                <h3>📝 Word Bank - Choose from these words:</h3>
                <div class="word-options">
                    <span class="word-option">short-handed</span>
                    <span class="word-option">close down</span>
                    <span class="word-option">scenery</span>
                    <span class="word-option">find</span>
                    <span class="word-option">basically</span>
                    <span class="word-option">break up</span>
                    <span class="word-option">safe</span>
                    <span class="word-option">main</span>
                    <span class="word-option">enough</span>
                </div>
            </div>

            <div class="question">
                <h3>Question 1:</h3>
                <p>The restaurant had to <input type="text" class="fill-blank" data-answer="close down" placeholder="answer"> after years of declining business.</p>
                <div class="feedback" id="feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 2:</h3>
                <p>We were <input type="text" class="fill-blank" data-answer="short-handed" placeholder="answer"> at work today because three people called in sick.</p>
                <div class="feedback" id="feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 3:</h3>
                <p>The mountain <input type="text" class="fill-blank" data-answer="scenery" placeholder="answer"> was absolutely breathtaking.</p>
                <div class="feedback" id="feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 4:</h3>
                <p>I can't <input type="text" class="fill-blank" data-answer="find" placeholder="answer"> my keys anywhere - have you seen them?</p>
                <div class="feedback" id="feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 5:</h3>
                <p><input type="text" class="fill-blank" data-answer="basically" placeholder="answer">, the plan is to finish the project by Friday.</p>
                <div class="feedback" id="feedback-5" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 6:</h3>
                <p>They decided to <input type="text" class="fill-blank" data-answer="break up" placeholder="answer"> after dating for two years.</p>
                <div class="feedback" id="feedback-6" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 7:</h3>
                <p>Make sure you store the chemicals in a <input type="text" class="fill-blank" data-answer="safe" placeholder="answer"> place away from children.</p>
                <div class="feedback" id="feedback-7" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 8:</h3>
                <p>The <input type="text" class="fill-blank" data-answer="main" placeholder="answer"> reason for his success was his dedication to practice.</p>
                <div class="feedback" id="feedback-8" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 9:</h3>
                <p>We don't have <input type="text" class="fill-blank" data-answer="enough" placeholder="answer"> time to finish this today.</p>
                <div class="feedback" id="feedback-9" style="display: none;"></div>
            </div>

            <button class="check-btn" onclick="checkFillBlanks()">Check Answers</button>
        </div>

        <!-- Matching Section -->
        <div id="matching" class="game-section">
            <h2 style="text-align: center; margin-bottom: 20px; color: #6a11cb;">Match the expressions with their meanings</h2>
            <div class="matching-container">
                <div class="match-column">
                    <h4>Expressions</h4>
                    <div class="match-item" data-word="short-handed" onclick="selectMatch(this)">short-handed</div>
                    <div class="match-item" data-word="close down" onclick="selectMatch(this)">close down</div>
                    <div class="match-item" data-word="scenery" onclick="selectMatch(this)">scenery</div>
                    <div class="match-item" data-word="find" onclick="selectMatch(this)">find</div>
                    <div class="match-item" data-word="basically" onclick="selectMatch(this)">basically</div>
                    <div class="match-item" data-word="break up" onclick="selectMatch(this)">break up</div>
                    <div class="match-item" data-word="safe" onclick="selectMatch(this)">safe</div>
                    <div class="match-item" data-word="main" onclick="selectMatch(this)">main</div>
                    <div class="match-item" data-word="enough" onclick="selectMatch(this)">enough</div>
                </div>
                <div class="match-column">
                    <h4>Meanings</h4>
                    <div class="match-item" data-meaning="enough" onclick="selectMatch(this)">Sufficient or adequate amount</div>
                    <div class="match-item" data-meaning="close down" onclick="selectMatch(this)">To cease operation permanently</div>
                    <div class="match-item" data-meaning="scenery" onclick="selectMatch(this)">Natural features of a landscape</div>
                    <div class="match-item" data-meaning="basically" onclick="selectMatch(this)">In the most important ways</div>
                    <div class="match-item" data-meaning="short-handed" onclick="selectMatch(this)">Having fewer workers than needed</div>
                    <div class="match-item" data-meaning="find" onclick="selectMatch(this)">To discover or locate something</div>
                    <div class="match-item" data-meaning="main" onclick="selectMatch(this)">Most important or principal</div>
                    <div class="match-item" data-meaning="break up" onclick="selectMatch(this)">To end a relationship</div>
                    <div class="match-item" data-meaning="safe" onclick="selectMatch(this)">Protected from danger or harm</div>
                </div>
            </div>
            <div class="feedback" id="matching-feedback" style="display: none;"></div>
        </div>

        <!-- Multiple Choice Section -->
        <div id="multiple-choice" class="game-section">
            <div class="question">
                <h3>Question 1: What does "short-handed" mean?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Having small hands</div>
                    <div class="option" onclick="selectOption(this, true)">Having fewer workers than needed</div>
                    <div class="option" onclick="selectOption(this, false)">Working quickly</div>
                    <div class="option" onclick="selectOption(this, false)">Being unprepared</div>
                </div>
                <div class="feedback" id="mc-feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 2: "Close down" means:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">To reduce in size</div>
                    <div class="option" onclick="selectOption(this, false)">To become friends with someone</div>
                    <div class="option" onclick="selectOption(this, true)">To cease operation permanently</div>
                    <div class="option" onclick="selectOption(this, false)">To move to a smaller location</div>
                </div>
                <div class="feedback" id="mc-feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 3: "Scenery" refers to:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Theater decorations</div>
                    <div class="option" onclick="selectOption(this, true)">Natural features of a landscape</div>
                    <div class="option" onclick="selectOption(this, false)">A type of painting</div>
                    <div class="option" onclick="selectOption(this, false)">Urban architecture</div>
                </div>
                <div class="feedback" id="mc-feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 4: "Find" means:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">To search for something</div>
                    <div class="option" onclick="selectOption(this, true)">To discover or locate something</div>
                    <div class="option" onclick="selectOption(this, false)">To create something new</div>
                    <div class="option" onclick="selectOption(this, false)">To lose something temporarily</div>
                </div>
                <div class="feedback" id="mc-feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 5: "Basically" is used to indicate:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">A complex explanation</div>
                    <div class="option" onclick="selectOption(this, true)">In the most important ways</div>
                    <div class="option" onclick="selectOption(this, false)">A scientific concept</div>
                    <div class="option" onclick="selectOption(this, false)">A detailed analysis</div>
                </div>
                <div class="feedback" id="mc-feedback-5" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 6: "Break up" can mean:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">To repair something</div>
                    <div class="option" onclick="selectOption(this, false)">To start a relationship</div>
                    <div class="option" onclick="selectOption(this, true)">To end a relationship</div>
                    <div class="option" onclick="selectOption(this, false)">To take a break from work</div>
                </div>
                <div class="feedback" id="mc-feedback-6" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 7: "Safe" means:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Risky or dangerous</div>
                    <div class="option" onclick="selectOption(this, true)">Protected from danger or harm</div>
                    <div class="option" onclick="selectOption(this, false)">A storage container</div>
                    <div class="option" onclick="selectOption(this, false)">A type of lock</div>
                </div>
                <div class="feedback" id="mc-feedback-7" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 8: "Main" refers to:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, true)">Most important or principal</div>
                    <div class="option" onclick="selectOption(this, false)">A type of pipe</div>
                    <div class="option" onclick="selectOption(this, false)">The ocean</div>
                    <div class="option" onclick="selectOption(this, false)">A minor detail</div>
                </div>
                <div class="feedback" id="mc-feedback-8" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 9: "Enough" means:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Too much</div>
                    <div class="option" onclick="selectOption(this, false)">Insufficient</div>
                    <div class="option" onclick="selectOption(this, true)">Sufficient or adequate amount</div>
                    <div class="option" onclick="selectOption(this, false)">Excessive</div>
                </div>
                <div class="feedback" id="mc-feedback-9" style="display: none;"></div>
            </div>
        </div>
    </div>

    <script>
        let score = 0;
        let selectedWord = null;
        let selectedMeaning = null;
        let matchedPairs = [];

        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.game-section').forEach(section => {
                section.classList.remove('active');
            });
            
            // Remove active class from all buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionId).classList.add('active');
            
            // Add active class to clicked button
            event.target.classList.add('active');
        }

        function checkFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            let correctCount = 0;
            
            blanks.forEach((blank, index) => {
                const userAnswer = blank.value.toLowerCase().trim();
                const correctAnswer = blank.dataset.answer.toLowerCase();
                
                if (userAnswer === correctAnswer) {
                    blank.classList.remove('incorrect');
                    blank.classList.add('correct');
                    correctCount++;
                } else {
                    blank.classList.remove('correct');
                    blank.classList.add('incorrect');
                }
                
                // Show feedback for each question
                const feedback = document.getElementById(`feedback-${index+1}`);
                if (userAnswer === correctAnswer) {
                    feedback.textContent = '✅ Correct!';
                    feedback.className = 'feedback correct';
                } else {
                    feedback.textContent = `❌ Incorrect. The correct answer is: "${blank.dataset.answer}"`;
                    feedback.className = 'feedback incorrect';
                }
                feedback.style.display = 'block';
            });
            
            updateScore();
        }

        function selectMatch(element) {
            if (element.classList.contains('matched')) return;
            
            if (element.dataset.word) {
                // Word selected
                if (selectedWord) selectedWord.classList.remove('selected');
                selectedWord = element;
                element.classList.add('selected');
            } else {
                // Meaning selected
                if (selectedMeaning) selectedMeaning.classList.remove('selected');
                selectedMeaning = element;
                element.classList.add('selected');
            }
            
            // Check if we have both word and meaning selected
            if (selectedWord && selectedMeaning) {
                checkMatch();
            }
        }

        function checkMatch() {
            const feedback = document.getElementById('matching-feedback');
            
            if (selectedWord.dataset.word === selectedMeaning.dataset.meaning) {
                // Correct match
                selectedWord.classList.remove('selected');
                selectedWord.classList.add('matched');
                selectedMeaning.classList.remove('selected');
                selectedMeaning.classList.add('matched');
                
                matchedPairs.push(selectedWord.dataset.word);
                
                feedback.textContent = '✅ Correct match!';
                feedback.className = 'feedback correct';
                
                // Check if all matches are complete
                if (matchedPairs.length === 9) {
                    feedback.textContent = '🎉 Congratulations! You matched all terms correctly!';
                }
            } else {
                // Incorrect match
                feedback.textContent = '❌ Incorrect match. Try again.';
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
            selectedWord = null;
            selectedMeaning = null;
            
            updateScore();
        }

        function selectOption(element, isCorrect) {
            // Remove selection from all options in this question
            const options = element.parentElement.querySelectorAll('.option');
            options.forEach(opt => {
                opt.classList.remove('selected');
                opt.classList.remove('correct');
                opt.classList.remove('incorrect');
            });
            
            // Mark the selected option
            element.classList.add('selected');
            
            const questionNumber = element.closest('.question').querySelector('h3').textContent.match(/\d+/)[0];
            const feedback = document.getElementById(`mc-feedback-${questionNumber}`);
            
            if (isCorrect) {
                element.classList.add('correct');
                feedback.textContent = '✅ Correct!';
                feedback.className = 'feedback correct';
            } else {
                element.classList.add('incorrect');
                feedback.textContent = '❌ Incorrect. Try again.';
                feedback.className = 'feedback incorrect';
                
                // Show the correct answer
                options.forEach(opt => {
                    if (opt.onclick.toString().includes('true')) {
                        opt.classList.add('correct');
                    }
                });
            }
            
            feedback.style.display = 'block';
            updateScore();
        }

        function updateScore() {
            // Calculate score for fill-in-the-blank
            let fillScore = 0;
            document.querySelectorAll('.fill-blank.correct').forEach(blank => {
                if (!blank.classList.contains('counted')) {
                    fillScore++;
                    blank.classList.add('counted');
                }
            });
            
            // Calculate score for matching (each pair is 1 point)
            const matchScore = matchedPairs.length;
            
            // Calculate score for multiple choice (each correct is 1 point)
            let mcScore = 0;
            document.querySelectorAll('.option.correct.selected').forEach(opt => {
                mcScore++;
            });
            
            // Total score
            score = fillScore + matchScore + mcScore;
            document.getElementById('score').textContent = score;
        }
    </script>
</body>
</html>
