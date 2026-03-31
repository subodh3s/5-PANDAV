<!DOCTYPE html>
<html>
<head>
  <title>Tic Tac Toe AI</title>
  <style>
    body {
      font-family: Arial;
      text-align: center;
      background: #f5f5f5;
    }

    h1 {
      margin-top: 20px;
    }

    #board {
      width: 300px;
      margin: 20px auto;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 5px;
    }

    .cell {
      width: 100px;
      height: 100px;
      font-size: 2rem;
      background: white;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      border: 2px solid #333;
      transition: 0.2s;
    }

    .cell:hover {
      background: #ddd;
    }

    button {
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
    }
  </style>
</head>
<body>

<h1>Tic Tac Toe (AI)</h1>
<div id="board"></div>
<button onclick="restart()">Restart</button>

<script>
  const boardElement = document.getElementById("board");
  let board = ["", "", "", "", "", "", "", "", ""];
  let human = "X";
  let ai = "O";
  let gameOver = false;

  function createBoard() {
    boardElement.innerHTML = "";
    board.forEach((cell, index) => {
      const div = document.createElement("div");
      div.classList.add("cell");
      div.innerText = cell;
      div.addEventListener("click", () => humanMove(index));
      boardElement.appendChild(div);
    });
  }

  function humanMove(index) {
    if (board[index] !== "" || gameOver) return;

    board[index] = human;
    createBoard();

    if (checkWinner(board, human)) {
      alert("You win!");
      gameOver = true;
      return;
    }

    if (isDraw()) {
      alert("Draw!");
      gameOver = true;
      return;
    }

    setTimeout(aiMove, 300);
  }

  function aiMove() {
    let bestScore = -Infinity;
    let move;

    for (let i = 0; i < 9; i++) {
      if (board[i] === "") {
        board[i] = ai;
        let score = minimax(board, 0, false);
        board[i] = "";
        if (score > bestScore) {
          bestScore = score;
          move = i;
        }
      }
    }

    board[move] = ai;
    createBoard();

    if (checkWinner(board, ai)) {
      alert("AI wins!");
      gameOver = true;
      return;
    }

    if (isDraw()) {
      alert("Draw!");
      gameOver = true;
    }
  }

  function minimax(newBoard, depth, isMaximizing) {
    if (checkWinner(newBoard, ai)) return 1;
    if (checkWinner(newBoard, human)) return -1;
    if (!newBoard.includes("")) return 0;

    if (isMaximizing) {
      let bestScore = -Infinity;
      for (let i = 0; i < 9; i++) {
        if (newBoard[i] === "") {
          newBoard[i] = ai;
          let score = minimax(newBoard, depth + 1, false);
          newBoard[i] = "";
          bestScore = Math.max(score, bestScore);
        }
      }
      return bestScore;
    } else {
      let bestScore = Infinity;
      for (let i = 0; i < 9; i++) {
        if (newBoard[i] === "") {
          newBoard[i] = human;
          let score = minimax(newBoard, depth + 1, true);
          newBoard[i] = "";
          bestScore = Math.min(score, bestScore);
        }
      }
      return bestScore;
    }
  }

  function checkWinner(b, player) {
    const wins = [
      [0,1,2],[3,4,5],[6,7,8],
      [0,3,6],[1,4,7],[2,5,8],
      [0,4,8],[2,4,6]
    ];

    return wins.some(combo =>
      combo.every(i => b[i] === player)
    );
  }

  function isDraw() {
    return board.every(cell => cell !== "");
  }

  function restart() {
    board = ["", "", "", "", "", "", "", "", ""];
    gameOver = false;
    createBoard();
  }

  createBoard();
</script>

</body>
</html>
