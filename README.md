# POODIGY_WD_3[index.html](https://github.com/user-attachments/files/24740258/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prodigy Tic-Tac-Toe</title>
    <style>
        :root {
            --bg: #0f172a;
            --cell-bg: #1e293b;
            --text: #f8fafc;
            --x-color: #3b82f6;
            --o-color: #ef4444;
            --winner-bg: #22c55e;
        }

        body {
            background-color: var(--bg);
            color: var(--text);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }

        .status {
            font-size: 2rem;
            margin-bottom: 20px;
            font-weight: bold;
        }

        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 10px;
        }

        .cell {
            background-color: var(--cell-bg);
            width: 100px;
            height: 100px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3rem;
            font-weight: bold;
            cursor: pointer;
            border-radius: 12px;
            transition: all 0.2s;
        }

        .cell:hover { background-color: #334155; }
        .cell.x { color: var(--x-color); }
        .cell.o { color: var(--o-color); }

        .restart-btn {
            margin-top: 30px;
            padding: 12px 30px;
            font-size: 1.1rem;
            font-weight: bold;
            background-color: var(--winner-bg);
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: transform 0.2s;
        }

        .restart-btn:hover { transform: scale(1.05); }
    </style>
</head>
<body>

    <div class="status" id="status">Player X's Turn</div>
    
    <div class="board" id="board">
        <div data-index="0" class="cell"></div>
        <div data-index="1" class="cell"></div>
        <div data-index="2" class="cell"></div>
        <div data-index="3" class="cell"></div>
        <div data-index="4" class="cell"></div>
        <div data-index="5" class="cell"></div>
        <div data-index="6" class="cell"></div>
        <div data-index="7" class="cell"></div>
        <div data-index="8" class="cell"></div>
    </div>

    <button class="restart-btn" id="restartBtn">Reset Game</button>

    <script>
        const boardElement = document.getElementById('board');
        const statusDisplay = document.getElementById('status');
        const restartBtn = document.getElementById('restartBtn');
        const cells = document.querySelectorAll('.cell');

        let gameActive = true;
        let currentPlayer = "X";
        let gameState = ["", "", "", "", "", "", "", "", ""];

        const winningConditions = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
            [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
            [0, 4, 8], [2, 4, 6]             // Diagonals
        ];

        function handleCellClick(clickedCellEvent) {
            const clickedCell = clickedCellEvent.target;
            const clickedCellIndex = parseInt(clickedCell.getAttribute('data-index'));

            if (gameState[clickedCellIndex] !== "" || !gameActive) return;

            gameState[clickedCellIndex] = currentPlayer;
            clickedCell.innerText = currentPlayer;
            clickedCell.classList.add(currentPlayer.toLowerCase());

            handleResultValidation();
        }

        function handleResultValidation() {
            let roundWon = false;
            for (let i = 0; i <= 7; i++) {
                const winCondition = winningConditions[i];
                let a = gameState[winCondition[0]];
                let b = gameState[winCondition[1]];
                let c = gameState[winCondition[2]];
                if (a === '' || b === '' || c === '') continue;
                if (a === b && b === c) {
                    roundWon = true;
                    break;
                }
            }

            if (roundWon) {
                statusDisplay.innerText = `Player ${currentPlayer} Wins!`;
                gameActive = false;
                return;
            }

            let roundDraw = !gameState.includes("");
            if (roundDraw) {
                statusDisplay.innerText = "Draw!";
                gameActive = false;
                return;
            }

            currentPlayer = currentPlayer === "X" ? "O" : "X";
            statusDisplay.innerText = `Player ${currentPlayer}'s Turn`;
        }

        function handleRestartGame() {
            gameActive = true;
            currentPlayer = "X";
            gameState = ["", "", "", "", "", "", "", "", ""];
            statusDisplay.innerText = `Player X's Turn`;
            cells.forEach(cell => {
                cell.innerText = "";
                cell.classList.remove('x', 'o');
            });
        }

        cells.forEach(cell => cell.addEventListener('click', handleCellClick));
        restartBtn.addEventListener('click', handleRestartGame);
    </script>
</body>
</html>
