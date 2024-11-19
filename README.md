<div id="app"></div>
<!DOCTYPE html>
<html lang="ar">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>لعبة إكس-أو</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        text-align: center;
        background-color: #f4f4f4;
        padding: 20px;
      }

      h1 {
        margin-bottom: 20px;
      }

      .board {
        display: grid;
        grid-template-columns: repeat(3, 100px);
        grid-template-rows: repeat(3, 100px);
        gap: 10px;
        margin: 20px auto;
      }

      .cell {
        width: 100px;
        height: 100px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 32px;
        background-color: #fff;
        border: 2px solid #333;
        cursor: pointer;
      }

      .cell:hover {
        background-color: #f0f0f0;
      }

      #message {
        margin-top: 20px;
        font-size: 18px;
      }

      button {
        padding: 10px 20px;
        font-size: 16px;
        cursor: pointer;
        background-color: #4caf50;
        color: white;
        border: none;
        border-radius: 5px;
        margin-top: 20px;
      }

      button:hover {
        background-color: #45a049;
      }
    </style>
  </head>
  <body>
    <h1>لعبة إكس-أو</h1>
    <div class="board" id="board">
      <!-- سيتم إضافة الخلايا هنا عبر JavaScript -->
    </div>

    <div id="message"></div>
    <button onclick="restartGame()">إعادة اللعب</button>

    <script>
      let currentPlayer = 'X';
      let gameBoard = ['', '', '', '', '', '', '', '', ''];
      let gameOver = false;

      // إنشاء خلايا اللوحة
      const boardElement = document.getElementById('board');
      for (let i = 0; i < 9; i++) {
        const cell = document.createElement('div');
        cell.classList.add('cell');
        cell.setAttribute('data-index', i);
        cell.addEventListener('click', handleCellClick);
        boardElement.appendChild(cell);
      }

      // التعامل مع النقرات على الخلايا
      function handleCellClick(event) {
        const index = event.target.getAttribute('data-index');
        if (gameBoard[index] !== '' || gameOver) return;

        // تحديث الخلية بالنص المناسب (X أو O)
        gameBoard[index] = currentPlayer;
        event.target.innerText = currentPlayer;

        // التحقق من الفائز
        if (checkWinner()) {
          document.getElementById(
            'message'
          ).innerText = `${currentPlayer} فاز!`;
          gameOver = true;
          return;
        }

        // التحقق من التعادل
        if (gameBoard.every((cell) => cell !== '')) {
          document.getElementById('message').innerText = 'التعادل!';
          gameOver = true;
          return;
        }

        // تبديل اللاعب
        currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
      }

      // التحقق من الفائز
      function checkWinner() {
        const winPatterns = [
          [0, 1, 2],
          [3, 4, 5],
          [6, 7, 8],
          [0, 3, 6],
          [1, 4, 7],
          [2, 5, 8],
          [0, 4, 8],
          [2, 4, 6],
        ];

        return winPatterns.some((pattern) => {
          const [a, b, c] = pattern;
          return (
            gameBoard[a] === gameBoard[b] &&
            gameBoard[b] === gameBoard[c] &&
            gameBoard[a] !== ''
          );
        });
      }

      // إعادة اللعبة
      function restartGame() {
        gameBoard = ['', '', '', '', '', '', '', '', ''];
        gameOver = false;
        currentPlayer = 'X';
        document.getElementById('message').innerText = '';
        const cells = document.querySelectorAll('.cell');
        cells.forEach((cell) => (cell.innerText = ''));
      }
    </script>
  </body>
</html>
