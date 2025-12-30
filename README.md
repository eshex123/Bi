<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Bingo Game</title>
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
            padding: 20px;
            color: #333;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            padding: 30px;
        }

        header {
            text-align: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 3px solid #764ba2;
        }

        h1 {
            color: #764ba2;
            font-size: 2.8rem;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
        }

        .game-description {
            color: #666;
            font-size: 1.1rem;
            max-width: 800px;
            margin: 0 auto;
            line-height: 1.6;
        }

        .game-area {
            display: grid;
            grid-template-columns: 1fr 300px;
            gap: 30px;
            margin-bottom: 30px;
        }

        @media (max-width: 768px) {
            .game-area {
                grid-template-columns: 1fr;
            }
        }

        .bingo-card {
            background: white;
            padding: 25px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
        }

        .card-title {
            text-align: center;
            color: #764ba2;
            margin-bottom: 20px;
            font-size: 1.8rem;
        }

        .bingo-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            margin: 0 auto;
            max-width: 500px;
        }

        .bingo-cell {
            aspect-ratio: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            background: #f8f9fa;
            border: 2px solid #e9ecef;
            border-radius: 10px;
            font-size: 1.4rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            user-select: none;
        }

        .bingo-cell:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        .bingo-cell.free {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
        }

        .bingo-cell.marked {
            background: linear-gradient(135deg, #4ade80, #22c55e);
            color: white;
            border-color: #16a34a;
            transform: scale(0.95);
        }

        .bingo-header {
            background: linear-gradient(135deg, #764ba2, #667eea);
            color: white;
            font-weight: bold;
            font-size: 1.6rem;
        }

        .control-panel {
            background: white;
            padding: 25px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .number-display {
            text-align: center;
            padding: 30px;
            background: linear-gradient(135deg, #667eea, #764ba2);
            border-radius: 15px;
            color: white;
            margin-bottom: 10px;
        }

        .current-number {
            font-size: 4rem;
            font-weight: bold;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
        }

        .number-label {
            font-size: 1.2rem;
            opacity: 0.9;
            margin-top: 10px;
        }

        .called-numbers {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            max-height: 200px;
            overflow-y: auto;
        }

        .called-numbers h3 {
            color: #764ba2;
            margin-bottom: 10px;
        }

        .number-list {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
        }

        .called-number {
            background: white;
            border: 2px solid #667eea;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
        }

        .called-number.recent {
            background: #667eea;
            color: white;
            transform: scale(1.1);
        }

        .controls {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        button {
            padding: 15px 25px;
            border: none;
            border-radius: 10px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .btn-new-number {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
        }

        .btn-new-number:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.3);
        }

        .btn-new-game {
            background: linear-gradient(135deg, #10b981, #059669);
            color: white;
        }

        .btn-new-game:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(16, 185, 129, 0.3);
        }

        .btn-check-bingo {
            background: linear-gradient(135deg, #f59e0b, #d97706);
            color: white;
        }

        .btn-check-bingo:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(245, 158, 11, 0.3);
        }

        .game-info {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }

        .info-card {
            background: white;
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
        }

        .info-card h3 {
            color: #764ba2;
            margin-bottom: 15px;
            font-size: 1.3rem;
        }

        .bingo-message {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0);
            background: linear-gradient(135deg, #4ade80, #22c55e);
            color: white;
            padding: 40px 60px;
            border-radius: 20px;
            font-size: 3rem;
            font-weight: bold;
            text-align: center;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            z-index: 1000;
            transition: transform 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
            display: none;
        }

        .bingo-message.show {
            display: block;
            transform: translate(-50%, -50%) scale(1);
        }

        .bingo-pattern {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 5px;
            max-width: 200px;
            margin: 15px auto;
        }

        .pattern-cell {
            aspect-ratio: 1;
            background: #e9ecef;
            border-radius: 5px;
        }

        .pattern-cell.active {
            background: #22c55e;
        }

        .rules ul {
            padding-left: 20px;
            line-height: 1.8;
        }

        .rules li {
            margin-bottom: 8px;
        }

        footer {
            text-align: center;
            margin-top: 40px;
            padding-top: 20px;
            border-top: 2px solid #e9ecef;
            color: #666;
            font-size: 0.9rem;
        }

        @keyframes confetti {
            0% { transform: translateY(-100px) rotate(0deg); }
            100% { transform: translateY(100vh) rotate(360deg); }
        }

        .confetti {
            position: fixed;
            width: 15px;
            height: 15px;
            background: #ff6b6b;
            top: -20px;
            z-index: 999;
            animation: confetti 5s linear infinite;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>🎯 Interactive Bingo Game</h1>
            <p class="game-description">
                Click on numbers as they're called. Mark 5 in a row (horizontal, vertical, or diagonal) to win!
            </p>
        </header>

        <div class="game-area">
            <div class="bingo-card">
                <h2 class="card-title">Your Bingo Card</h2>
                <div class="bingo-grid" id="bingoGrid"></div>
            </div>

            <div class="control-panel">
                <div class="number-display">
                    <div class="current-number" id="currentNumber">--</div>
                    <div class="number-label">Current Number</div>
                </div>

                <div class="called-numbers">
                    <h3>Called Numbers</h3>
                    <div class="number-list" id="calledNumbers"></div>
                </div>

                <div class="controls">
                    <button class="btn-new-number" onclick="callNewNumber()">
                        <span>🎲</span> Call Next Number
                    </button>
                    <button class="btn-check-bingo" onclick="checkBingo()">
                        <span>✅</span> Check for Bingo
                    </button>
                    <button class="btn-new-game" onclick="newGame()">
                        <span>🔄</span> New Game
                    </button>
                </div>
            </div>
        </div>

        <div class="game-info">
            <div class="info-card">
                <h3>How to Play</h3>
                <div class="rules">
                    <ul>
                        <li>Numbers are called randomly from 1-75</li>
                        <li>Click on numbers on your card when they're called</li>
                        <li>Mark the center FREE space automatically</li>
                        <li>Get 5 marked spaces in a row to win!</li>
                        <li>Rows can be horizontal, vertical, or diagonal</li>
                    </ul>
                </div>
            </div>

            <div class="info-card">
                <h3>Winning Patterns</h3>
                <div class="bingo-pattern">
                    <div class="pattern-cell active"></div>
                    <div class="pattern-cell active"></div>
                    <div class="pattern-cell active"></div>
                    <div class="pattern-cell active"></div>
                    <div class="pattern-cell active"></div>
                </div>
                <p style="text-align: center; margin-top: 10px;">Any straight line wins!</p>
            </div>

            <div class="info-card">
                <h3>Game Stats</h3>
                <p>Numbers Called: <span id="numbersCalled">0</span></p>
                <p>Numbers Marked: <span id="numbersMarked">0</span></p>
                <p>Time Playing: <span id="gameTime">00:00</span></p>
            </div>
        </div>

        <footer>
            <p>Made with ❤️ | Bingo Game | Refresh page for a new card</p>
        </footer>
    </div>

    <div class="bingo-message" id="bingoMessage">BINGO!</div>

    <script>
        class BingoGame {
            constructor() {
                this.numbers = Array.from({length: 75}, (_, i) => i + 1);
                this.calledNumbers = [];
                this.cardNumbers = [];
                this.markedNumbers = new Set();
                this.bingoGrid = [];
                this.gameTime = 0;
                this.timer = null;
                this.gameActive = true;
                
                this.init();
            }

            init() {
                this.generateBingoCard();
                this.setupEventListeners();
                this.startTimer();
                this.markedNumbers.add('FREE');
                this.updateStats();
            }

            generateBingoCard() {
                // Bingo columns: B(1-15), I(16-30), N(31-45), G(46-60), O(61-75)
                const ranges = [
                    {start: 1, end: 15},
                    {start: 16, end: 30},
                    {start: 31, end: 45},
                    {start: 46, end: 60},
                    {start: 61, end: 75}
                ];

                this.cardNumbers = [];
                const bingoGrid = document.getElementById('bingoGrid');
                bingoGrid.innerHTML = '';

                // Create header
                'BINGO'.split('').forEach(letter => {
                    const headerCell = document.createElement('div');
                    headerCell.className = 'bingo-cell bingo-header';
                    headerCell.textContent = letter;
                    bingoGrid.appendChild(headerCell);
                });

                // Generate numbers for each column
                for (let col = 0; col < 5; col++) {
                    const range = ranges[col];
                    const columnNumbers = [];
                    
                    // Generate 5 unique numbers for this column
                    const availableNumbers = Array.from(
                        {length: range.end - range.start + 1}, 
                        (_, i) => i + range.start
                    );
                    
                    for (let i = 0; i < 5; i++) {
                        const randomIndex = Math.floor(Math.random() * availableNumbers.length);
                        columnNumbers.push(availableNumbers.splice(randomIndex, 1)[0]);
                    }

                    this.cardNumbers.push(columnNumbers);
                }

                // Transpose to get rows
                this.bingoGrid = [];
                for (let row = 0; row < 5; row++) {
                    this.bingoGrid[row] = [];
                    for (let col = 0; col < 5; col++) {
                        this.bingoGrid[row][col] = this.cardNumbers[col][row];
                    }
                }

                // Mark center as FREE
                this.bingoGrid[2][2] = 'FREE';

                // Create grid cells
                for (let row = 0; row < 5; row++) {
                    for (let col = 0; col < 5; col++) {
                        const cellValue = this.bingoGrid[row][col];
                        const cell = document.createElement('div');
                        cell.className = 'bingo-cell';
                        
                        if (cellValue === 'FREE') {
                            cell.className += ' free';
                            cell.textContent = 'FREE';
                        } else {
                            cell.textContent = cellValue;
                        }

                        cell.dataset.row = row;
                        cell.dataset.col = col;
                        cell.addEventListener('click', () => this.toggleMark(row, col));
                        bingoGrid.appendChild(cell);
                    }
                }
            }

            setupEventListeners() {
                document.addEventListener('keydown', (e) => {
                    if (e.key === ' ') {
                        e.preventDefault();
                        this.callNewNumber();
                    } else if (e.key === 'Enter') {
                        this.checkBingo();
                    } else if (e.key === 'r' || e.key === 'R') {
                        this.newGame();
                    }
                });
            }

            startTimer() {
                this.timer = setInterval(() => {
                    this.gameTime++;
                    const minutes = Math.floor(this.gameTime / 60).toString().padStart(2, '0');
                    const seconds = (this.gameTime % 60).toString().padStart(2, '0');
                    document.getElementById('gameTime').textContent = `${minutes}:${seconds}`;
                }, 1000);
            }

            callNewNumber() {
                if (!this.gameActive || this.calledNumbers.length >= 75) return;

                let newNumber;
                do {
                    newNumber = Math.floor(Math.random() * 75) + 1;
                } while (this.calledNumbers.includes(newNumber));

                this.calledNumbers.push(newNumber);
                this.updateNumberDisplay(newNumber);
                this.updateCalledNumbers();
                this.updateStats();

                // Auto-mark if number is on card
                for (let row = 0; row < 5; row++) {
                    for (let col = 0; col < 5; col++) {
                        if (this.bingoGrid[row][col] === newNumber) {
                            this.markCell(row, col);
                        }
                    }
                }
            }

            updateNumberDisplay(number) {
                const numberDisplay = document.getElementById('currentNumber');
                numberDisplay.textContent = number;
                numberDisplay.style.transform = 'scale(1.2)';
                
                setTimeout(() => {
                    numberDisplay.style.transform = 'scale(1)';
                }, 300);
            }

            updateCalledNumbers() {
                const container = document.getElementById('calledNumbers');
                container.innerHTML = '';
                
                const recentNumbers = this.calledNumbers.slice(-20);
                
                recentNumbers.forEach((num, index) => {
                    const numberElement = document.createElement('div');
                    numberElement.className = 'called-number';
                    if (index === recentNumbers.length - 1) {
                        numberElement.classList.add('recent');
                    }
                    numberElement.textContent = num;
                    container.appendChild(numberElement);
                });
                
                // Scroll to bottom
                container.scrollTop = container.scrollHeight;
            }

            toggleMark(row, col) {
                if (!this.gameActive || this.bingoGrid[row][col] === 'FREE') return;
                
                const cell = document.querySelector(`[data-row="${row}"][data-col="${col}"]`);
                const number = this.bingoGrid[row][col];
                
                if (this.markedNumbers.has(number)) {
                    this.markedNumbers.delete(number);
                    cell.classList.remove('marked');
                } else {
                    this.markCell(row, col);
                }
                
                this.updateStats();
            }

            markCell(row, col) {
                const cell = document.querySelector(`[data-row="${row}"][data-col="${col}"]`);
                const number = this.bingoGrid[row][col];
                
                if (!this.markedNumbers.has(number)) {
                    this.markedNumbers.add(number);
                    cell.classList.add('marked');
                    
                    // Add animation effect
                    cell.style.transform = 'scale(0.95)';
                    setTimeout(() => {
                        cell.style.transform = 'scale(1)';
                    }, 150);
                }
            }

            checkBingo() {
                if (!this.gameActive) return;

                // Check rows
                for (let row = 0; row < 5; row++) {
                    let rowComplete = true;
                    for (let col = 0; col < 5; col++) {
                        const number = this.bingoGrid[row][col];
                        if (!this.markedNumbers.has(number)) {
                            rowComplete = false;
                            break;
                        }
                    }
                    if (rowComplete) {
                        this.declareBingo('row');
                        return;
                    }
                }

                // Check columns
                for (let col = 0; col < 5; col++) {
                    let colComplete = true;
                    for (let row = 0; row < 5; row++) {
                        const number = this.bingoGrid[row][col];
                        if (!this.markedNumbers.has(number)) {
                            colComplete = false;
                            break;
                        }
                    }
                    if (colComplete) {
                        this.declareBingo('column');
                        return;
                    }
                }

                // Check diagonals
                let diag1Complete = true;
                let diag2Complete = true;
                
                for (let i = 0; i < 5; i++) {
                    if (!this.markedNumbers.has(this.bingoGrid[i][i])) {
                        diag1Complete = false;
                    }
                    if (!this.markedNumbers.has(this.bingoGrid[i][4 - i])) {
                        diag2Complete = false;
                    }
                }

                if (diag1Complete || diag2Complete) {
                    this.declareBingo('diagonal');
                    return;
                }

                alert('No Bingo yet! Keep playing!');
            }

            declareBingo(pattern) {
                this.gameActive = false;
                clearInterval(this.timer);
                
                const message = document.getElementById('bingoMessage');
                message.classList.add('show');
                
                // Create confetti effect
                this.createConfetti();
                
                // Play sound (optional - would need audio file)
                // const audio = new Audio('bingo-sound.mp3');
                // audio.play();
                
                setTimeout(() => {
                    message.classList.remove('show');
                }, 5000);
            }

            createConfetti() {
                const colors = ['#ff6b6b', '#4ecdc4', '#ffe66d', '#667eea', '#764ba2'];
                
                for (let i = 0; i < 150; i++) {
                    const confetti = document.createElement('div');
                    confetti.className = 'confetti';
                    confetti.style.left = Math.random() * 100 + 'vw';
                    confetti.style.background = colors[Math.floor(Math.random() * colors.length)];
                    confetti.style.width = Math.random() * 15 + 5 + 'px';
                    confetti.style.height = Math.random() * 15 + 5 + 'px';
                    confetti.style.animationDuration = Math.random() * 3 + 2 + 's';
                    confetti.style.animationDelay = Math.random() * 1 + 's';
                    
                    document.body.appendChild(confetti);
                    
                    setTimeout(() => {
                        confetti.remove();
                    }, 6000);
                }
            }

            updateStats() {
                document.getElementById('numbersCalled').textContent = this.calledNumbers.length;
                document.getElementById('numbersMarked').textContent = this.markedNumbers.size - 1; // Exclude FREE
            }

            newGame() {
                this.calledNumbers = [];
                this.markedNumbers = new Set(['FREE']);
                this.gameTime = 0;
                this.gameActive = true;
                
                clearInterval(this.timer);
                this.startTimer();
                
                this.generateBingoCard();
                this.updateNumberDisplay('--');
                this.updateCalledNumbers();
                this.updateStats();
                
                // Remove any existing confetti
                document.querySelectorAll('.confetti').forEach(el => el.remove());
            }
        }

        // Initialize game when page loads
        let game;
        window.addEventListener('DOMContentLoaded', () => {
            game = new BingoGame();
        });

        // Global functions for button clicks
        function callNewNumber() {
            game.callNewNumber();
        }

        function checkBingo() {
            game.checkBingo();
        }

        function newGame() {
            game.newGame();
        }
    </script>
</body>
</html>
