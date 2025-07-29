# Tic-Tac-Toe
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Tic Tac Toe</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 50px;
    }

    h1 {
      margin-bottom: 20px;
      color: #333;
    }

    .board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      gap: 5px;
    }

    .cell {
      width: 100px;
      height: 100px;
      background-color: white;
      border: 2px solid #333;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 2.5rem;
      cursor: pointer;
    }

    .cell.x {
      color: #007bff;
    }

    .cell.o {
      color: #e74c3c;
    }

    .message {
      margin-top: 20px;
      text-align: center;
      display: none;
    }

    #restartButton {
      margin-top: 10px;
      padding: 10px 20px;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <h1>Tic Tac Toe</h1>
  <div class="board" id="board">
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
  </div>
  <div class="message" id="winningMessage">
    <p id="winningMessageText"></p>
    <button id="restartButton">Restart</button>
  </div>

  <script>
    const cellElements = document.querySelectorAll("[data-cell]");
    const board = document.getElementById("board");
    const winningMessage = document.getElementById("winningMessage");
    const winningMessageText = document.getElementById("winningMessageText");
    const restartButton = document.getElementById("restartButton");

    const WINNING_COMBINATIONS = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6],
    ];

    let oTurn;

    startGame();

    restartButton.addEventListener("click", startGame);

    function startGame() {
      oTurn = false;
      cellElements.forEach(cell => {
        cell.classList.remove("x", "o");
        cell.removeEventListener("click", handleClick);
        cell.addEventListener("click", handleClick, { once: true });
      });
      winningMessage.style.display = "none";
    }

    function handleClick(e) {
      const cell = e.target;
      const currentClass = oTurn ? "o" : "x";
      placeMark(cell, currentClass);
      if (checkWin(currentClass)) {
        endGame(false);
      } else if (isDraw()) {
        endGame(true);
      } else {
        swapTurns();
      }
    }

    function placeMark(cell, currentClass) {
      cell.classList.add(currentClass);
    }

    function swapTurns() {
      oTurn = !oTurn;
    }

    function checkWin(currentClass) {
      return WINNING_COMBINATIONS.some(combination => {
        return combination.every(index => {
          return cellElements[index].classList.contains(currentClass);
        });
      });
    }

    function isDraw() {
      return [...cellElements].every(cell => {
        return cell.classList.contains("x") || cell.classList.contains("o");
      });
    }

    function endGame(draw) {
      if (draw) {
        winningMessageText.innerText = "Draw!";
      } else {
        winningMessageText.innerText = `${oTurn ? "O" : "X"} Wins!`;
      }
      winningMessage.style.display = "block";
    }
  </script>
</body>
</html>
