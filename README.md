<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', sans-serif;
            transition: background 0.3s ease, color 0.3s ease;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: #f0f2f5;
        }

        body.dark-theme {
            background: #1a1a1a;
        }

        .calculator {
            background: rgba(255, 255, 255, 0.95);
            padding: 1.5rem;
            border-radius: 1.5rem;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            width: 320px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.3);
            position: relative;
        }

        .dark-theme .calculator {
            background: rgba(40, 40, 40, 0.95);
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: #ffffff;
        }

        .controls {
            position: absolute;
            top: 1rem;
            left: 1rem;
            display: flex;
            gap: 0.5rem;
        }

        .theme-toggle, .history-btn {
            background: #4a90e2;
            color: white;
            border: none;
            padding: 0.5rem 1rem;
            border-radius: 0.5rem;
            cursor: pointer;
            font-size: 0.9rem;
            transition: background 0.3s ease;
        }

        .history-btn {
            background: #9b59b6;
        }

        .theme-toggle:hover {
            background: #357abd;
        }

        .history-btn:hover {
            background: #8e44ad;
        }

        .dark-theme .theme-toggle {
            background: #00c853;
        }

        .dark-theme .history-btn {
            background: #8e44ad;
        }

        .display {
            width: 100%;
            height: 70px;
            border: none;
            background: transparent;
            color: #2d3436;
            text-align: right;
            padding: 1rem;
            font-size: 2rem;
            margin-bottom: 1rem;
            font-weight: 300;
            border-bottom: 2px solid #eee;
        }

        .dark-theme .display {
            color: #ffffff;
            border-bottom: 2px solid #444;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 0.8rem;
        }

        button {
            padding: 1rem;
            border: none;
            background: #f8f9fa;
            color: #2d3436;
            font-size: 1.1rem;
            cursor: pointer;
            border-radius: 0.8rem;
            transition: all 0.2s ease;
            font-weight: 500;
        }

        .dark-theme button {
            background: #444;
            color: #ffffff;
        }

        button:hover {
            background: #e9ecef;
            transform: translateY(-2px);
        }

        .dark-theme button:hover {
            background: #555;
        }

        .operator {
            background: #4a90e2;
            color: white;
        }

        .dark-theme .operator {
            background: #357abd;
        }

        .equals {
            background: #00c853;
            color: white;
        }

        .dark-theme .equals {
            background: #009624;
        }

        .clear {
            background: #ff5252;
            color: white;
        }

        .dark-theme .clear {
            background: #d50000;
        }

        .special {
            background: #f8f9fa;
            color: #4a90e2;
        }

        .dark-theme .special {
            background: #444;
            color: #4a90e2;
        }

        .history-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .history-content {
            background: white;
            padding: 1.5rem;
            border-radius: 1rem;
            max-width: 300px;
            max-height: 400px;
            overflow-y: auto;
        }

        .dark-theme .history-content {
            background: #2d2d2d;
            color: white;
        }

        .history-entry {
            padding: 0.5rem 0;
            border-bottom: 1px solid #eee;
        }

        .dark-theme .history-entry {
            border-color: #444;
        }

        .close-btn {
            float: right;
            cursor: pointer;
            padding: 0.5rem;
        }

        @media (max-width: 400px) {
            .calculator {
                width: 90%;
                padding: 1rem;
            }
            
            button {
                padding: 0.8rem;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="controls">
            <button class="theme-toggle" onclick="toggleTheme()">🌙 Dark</button>
            <button class="history-btn" onclick="showHistory()">📜 History</button>
        </div>
        <input type="text" class="display" readonly>
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">C</button>
            <button class="special" onclick="appendToDisplay('(')">(</button>
            <button class="special" onclick="appendToDisplay(')')">)</button>
            <button class="operator" onclick="appendToDisplay('/')">/</button>
            
            <button onclick="appendToDisplay('7')">7</button>
            <button onclick="appendToDisplay('8')">8</button>
            <button onclick="appendToDisplay('9')">9</button>
            <button class="operator" onclick="appendToDisplay('*')">×</button>
            
            <button onclick="appendToDisplay('4')">4</button>
            <button onclick="appendToDisplay('5')">5</button>
            <button onclick="appendToDisplay('6')">6</button>
            <button class="operator" onclick="appendToDisplay('-')">-</button>
            
            <button onclick="appendToDisplay('1')">1</button>
            <button onclick="appendToDisplay('2')">2</button>
            <button onclick="appendToDisplay('3')">3</button>
            <button class="operator" onclick="appendToDisplay('+')">+</button>
            
            <button onclick="appendToDisplay('0')">0</button>
            <button onclick="appendToDisplay('.')">.</button>
            <button onclick="deleteLastChar()">⌫</button>
            <button class="equals" onclick="calculate()">=</button>
        </div>
    </div>

    <div class="history-modal" id="historyModal">
        <div class="history-content">
            <span class="close-btn" onclick="closeHistory()">✕</span>
            <h3>Calculation History</h3>
            <div id="historyList"></div>
        </div>
    </div>

    <script>
        const display = document.querySelector('.display');
        const body = document.body;
        let calculationHistory = [];

        // Theme toggle
        function toggleTheme() {
            body.classList.toggle('dark-theme');
            const themeToggle = document.querySelector('.theme-toggle');
            if (body.classList.contains('dark-theme')) {
                themeToggle.textContent = '☀️ Light';
            } else {
                themeToggle.textContent = '🌙 Dark';
            }
        }

        // History functions
        function addToHistory(calculation) {
            calculationHistory.unshift(calculation);
            if (calculationHistory.length > 10) {
                calculationHistory.pop();
            }
        }

        function showHistory() {
            const modal = document.getElementById('historyModal');
            const historyList = document.getElementById('historyList');
            historyList.innerHTML = calculationHistory
                .map(entry => `<div class="history-entry">${entry}</div>`)
                .join('');
            modal.style.display = 'flex';
        }

        function closeHistory() {
            document.getElementById('historyModal').style.display = 'none';
        }

        // Calculator functions
        function appendToDisplay(value) {
            display.value += value;
        }

        function clearDisplay() {
            display.value = '';
        }

        function deleteLastChar() {
            display.value = display.value.slice(0, -1);
        }

        function calculate() {
            try {
                const result = eval(display.value);
                addToHistory(`${display.value} = ${result}`);
                display.value = result;
            } catch (error) {
                display.value = 'Error';
            }
        }

        // Close modal when clicking outside
        window.onclick = function(event) {
            const modal = document.getElementById('historyModal');
            if (event.target === modal) {
                closeHistory();
            }
        }

        // Keyboard support
        document.addEventListener('keydown', (event) => {
            const key = event.key;
            if (key >= '0' && key <= '9' || key === '.' || key === '+' || key === '-' || key === '*' || key === '/' || key === '(' || key === ')') {
                appendToDisplay(key);
            } else if (key === 'Enter') {
                calculate();
            } else if (key === 'Backspace') {
                deleteLastChar();
            } else if (key === 'Escape') {
                clearDisplay();
            }
        });
    </script>
</body>
</html>
