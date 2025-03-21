<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tic-Tac-Toe Game</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div class="container">
    <h1>Tic-Tac-Toe</h1>
    <div id="board">
      <div class="cell" data-index="0"></div>
      <div class="cell" data-index="1"></div>
      <div class="cell" data-index="2"></div>
      <div class="cell" data-index="3"></div>
      <div class="cell" data-index="4"></div>
      <div class="cell" data-index="5"></div>
      <div class="cell" data-index="6"></div>
      <div class="cell" data-index="7"></div>
      <div class="cell" data-index="8"></div>
    </div>
    <button id="resetBtn">Reset Game</button>
    <p id="status">Player X's Turn</p>
  </div>
  <script src="script.js"></script>
</body>
</html>body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f9;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.container {
  text-align: center;
}

h1 {
  color: #333;
}

#board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 5px;
  margin: 20px auto;
}

.cell {
  width: 100px;
  height: 100px;
  background-color: #fff;
  border: 2px solid #333;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
  font-weight: bold;
  cursor: pointer;
}

.cell.X {
  color: #007bff;
}

.cell.O {
  color: #ff4444;
}

#resetBtn {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  margin-top: 20px;
}

#resetBtn:hover {
  background-color: #0056b3;
}

#status {
  margin-top: 20px;
  font-size: 1.2rem;
  color: #333;
}const board = document.getElementById('board');
const cells = document.querySelectorAll('.cell');
const statusText = document.getElementById('status');
const resetBtn = document.getElementById('resetBtn');

let currentPlayer = 'X';
let gameState = ['', '', '', '', '', '', '', '', ''];
let gameActive = true;

const winningConditions = [
  [0, 1, 2], // Top row
  [3, 4, 5], // Middle row
  [6, 7, 8], // Bottom row
  [0, 3, 6], // Left column
  [1, 4, 7], // Middle column
  [2, 5, 8], // Right column
  [0, 4, 8], // Diagonal
  [2, 4, 6]  // Diagonal
];

function handleCellClick(event) {
  const clickedCell = event.target;
  const clickedCellIndex = parseInt(clickedCell.getAttribute('data-index'));

  if (gameState[clickedCellIndex] !== '' || !gameActive) {
    return;
  }

  gameState[clickedCellIndex] = currentPlayer;
  clickedCell.textContent = currentPlayer;
  clickedCell.classList.add(currentPlayer);

  checkForWinner();
}

function checkForWinner() {
  let roundWon = false;

  for (let i = 0; i < winningConditions.length; i++) {
    const [a, b, c] = winningConditions[i];
    if (gameState[a] === '' || gameState[b] === '' || gameState[c] === '') {
      continue;
    }
    if (gameState[a] === gameState[b] && gameState[b] === gameState[c]) {
      roundWon = true;
      break;
    }
  }

  if (roundWon) {
    statusText.textContent = `Player ${currentPlayer} Wins!`;
    gameActive = false;
    return;
  }

  if (!gameState.includes('')) {
    statusText.textContent = 'Draw!';
    gameActive = false;
    return;
  }

  currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
  statusText.textContent = `Player ${currentPlayer}'s Turn`;
}

function resetGame() {
  gameState = ['', '', '', '', '', '', '', '', ''];
  gameActive = true;
  currentPlayer = 'X';
  statusText.textContent = `Player ${currentPlayer}'s Turn`;
  cells.forEach(cell => {
    cell.textContent = '';
    cell.classList.remove('X', 'O');
  });
}

cells.forEach(cell => cell.addEventListener('click', handleCellClick));
resetBtn.addEventListener('click', resetGame)
