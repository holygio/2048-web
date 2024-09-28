<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>2048 Game</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>2048 Game</h1>
  <div id="game-container"></div>
  <div id="score">Score: 0</div>
  <script src="script.js"></script>
</body>
</html>
body {
  font-family: 'Arial', sans-serif;
  text-align: center;
  background-color: #faf8ef;
  margin: 0;
  padding: 0;
}

h1 {
  margin-top: 20px;
}

#game-container {
  width: 400px;
  height: 400px;
  margin: 20px auto;
  background-color: #bbada0;
  border-radius: 10px;
  display: grid;
  grid-template: repeat(4, 1fr) / repeat(4, 1fr);
  gap: 10px;
  padding: 10px;
}

.tile {
  background-color: #cdc1b4;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 55px;
  font-weight: bold;
  color: #776e65;
}

.tile-2 { background-color: #eee4da; }
.tile-4 { background-color: #ede0c8; }
.tile-8 { background-color: #f2b179; color: #f9f6f2; }
.tile-16 { background-color: #f59563; color: #f9f6f2; }
.tile-32 { background-color: #f67c5f; color: #f9f6f2; }
.tile-64 { background-color: #f65e3b; color: #f9f6f2; }
.tile-128 { background-color: #edcf72; color: #f9f6f2; }
.tile-256 { background-color: #edcc61; color: #f9f6f2; }
.tile-512 { background-color: #edc850; color: #f9f6f2; }
.tile-1024 { background-color: #edc53f; color: #f9f6f2; }
.tile-2048 { background-color: #edc22e; color: #f9f6f2; }

#score {
  font-size: 24px;
  margin-top: 10px;
}
document.addEventListener('DOMContentLoaded', () => {
  const gridDisplay = document.getElementById('game-container');
  const scoreDisplay = document.getElementById('score');
  const width = 4;
  let squares = [];
  let score = 0;

  // Create the playing board
  function createBoard() {
    for (let i = 0; i < width * width; i++) {
      let square = document.createElement('div');
      square.innerHTML = '';
      square.classList.add('tile');
      gridDisplay.appendChild(square);
      squares.push(square);
    }
    generateNewTile();
    generateNewTile();
  }

  createBoard();

  // Generate a new tile
  function generateNewTile() {
    let randomNumber = Math.floor(Math.random() * squares.length);
    if (squares[randomNumber].innerHTML == '') {
      squares[randomNumber].innerHTML = 2;
      squares[randomNumber].classList.add('tile-2');
    } else {
      generateNewTile();
    }
  }

  // Implement move functions
  // Swipe right, left, up, down
  // Combine tiles
  // Update the score
  // Check for win or game over

  // Keycode events
  function control(e) {
    if (e.keyCode === 39) {
      keyRight();
    } else if (e.keyCode === 37) {
      keyLeft();
    } else if (e.keyCode === 38) {
      keyUp();
    } else if (e.keyCode === 40) {
      keyDown();
    }
  }
  document.addEventListener('keyup', control);

  // Implement movement and combining functions
});
function moveRight() {
  for (let i = 0; i < 16; i += 4) {
    let totalOne = squares[i].innerHTML || 0;
    let totalTwo = squares[i + 1].innerHTML || 0;
    let totalThree = squares[i + 2].innerHTML || 0;
    let totalFour = squares[i + 3].innerHTML || 0;
    let row = [parseInt(totalOne), parseInt(totalTwo), parseInt(totalThree), parseInt(totalFour)];

    let filteredRow = row.filter(num => num);
    let missing = 4 - filteredRow.length;
    let zeros = Array(missing).fill(0);
    let newRow = zeros.concat(filteredRow);

    squares[i].innerHTML = newRow[0] ? newRow[0] : '';
    squares[i + 1].innerHTML = newRow[1] ? newRow[1] : '';
    squares[i + 2].innerHTML = newRow[2] ? newRow[2] : '';
    squares[i + 3].innerHTML = newRow[3] ? newRow[3] : '';
  }
}
function combineRow() {
  for (let i = 0; i < 15; i++) {
    if (squares[i].innerHTML === squares[i + 1].innerHTML) {
      let combinedTotal = parseInt(squares[i].innerHTML) + parseInt(squares[i + 1].innerHTML);
      squares[i].innerHTML = combinedTotal;
      squares[i + 1].innerHTML = '';
      score += combinedTotal;
      scoreDisplay.innerHTML = 'Score: ' + score;
    }
  }
}
