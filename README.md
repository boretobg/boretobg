# Hello there! <img src="https://user-images.githubusercontent.com/1303154/88677602-1635ba80-d120-11ea-84d8-d263ba5fc3c0.gif" width="30px"> 
### Student at Software University and Technical University of Sofia. Loves playing video games and dreams of making them one day.
<img align="left" alt="boretobg's GitHub Stats" src="https://github-readme-stats.vercel.app/api?username=boretobg&count_private=true&theme=tokyonight&hide=prs&show_icons=true" />
<img aling="left" alt="boretobg | gif" width="270px" border="5" src="https://miro.medium.com/max/1360/0*7Q3yvSIv_t0ioJ-Z.gif" />

### Connect with me here:

[<img align="left" alt="boretobg | Facebook" width="22px" src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/Facebook_Logo_%282019%29.png/1024px-Facebook_Logo_%282019%29.png" />][facebook]
[<img align="left" alt="boretobg | Instagram" width="22px" src="https://assets.stickpng.com/images/580b57fcd9996e24bc43c521.png" />][instagram]
[<img align="left" alt="boretobg | Youtube" width="22px" src="https://i.pinimg.com/originals/de/1c/91/de1c91788be0d791135736995109272a.png" />][youtube]
[<img align="left" alt="boretobg | Spotify" width="22px" src="https://www.freepnglogos.com/uploads/spotify-logo-png/file-spotify-logo-png-4.png" />][spotify]

[facebook]: https://facebook.com/boretobg
[instagram]: https://instagram.com/bobbystefanov
[youtube]: https://www.youtube.com/paddingtonyt
[spotify]: https://open.spotify.com/user/21kbmgahty4nyq4tycetkhn5i

<!DOCTYPE html>
<html>
<head>
  <title>Basic Snake HTML Game</title>
  <meta charset="UTF-8">
  <style>
  html, body {
    height: 100%;
    margin: 0;
  }

  body {
    background: black;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  canvas {
    border: 1px solid white;
  }
  </style>
</head>
<body>
<canvas width="400" height="400" id="game"></canvas>
<script>
var canvas = document.getElementById('game');
var context = canvas.getContext('2d');

var grid = 16;
var count = 0;

var snake = {
  x: 160,
  y: 160,

  // snake velocity. moves one grid length every frame in either the x or y direction
  dx: grid,
  dy: 0,

  // keep track of all grids the snake body occupies
  cells: [],

  // length of the snake. grows when eating an apple
  maxCells: 4
};
var apple = {
  x: 320,
  y: 320
};

// get random whole numbers in a specific range
// @see https://stackoverflow.com/a/1527820/2124254
function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min)) + min;
}

// game loop
function loop() {
  requestAnimationFrame(loop);

  // slow game loop to 15 fps instead of 60 (60/15 = 4)
  if (++count < 4) {
    return;
  }

  count = 0;
  context.clearRect(0,0,canvas.width,canvas.height);

  // move snake by it's velocity
  snake.x += snake.dx;
  snake.y += snake.dy;

  // wrap snake position horizontally on edge of screen
  if (snake.x < 0) {
    snake.x = canvas.width - grid;
  }
  else if (snake.x >= canvas.width) {
    snake.x = 0;
  }

  // wrap snake position vertically on edge of screen
  if (snake.y < 0) {
    snake.y = canvas.height - grid;
  }
  else if (snake.y >= canvas.height) {
    snake.y = 0;
  }

  // keep track of where snake has been. front of the array is always the head
  snake.cells.unshift({x: snake.x, y: snake.y});

  // remove cells as we move away from them
  if (snake.cells.length > snake.maxCells) {
    snake.cells.pop();
  }

  // draw apple
  context.fillStyle = 'red';
  context.fillRect(apple.x, apple.y, grid-1, grid-1);

  // draw snake one cell at a time
  context.fillStyle = 'green';
  snake.cells.forEach(function(cell, index) {

    // drawing 1 px smaller than the grid creates a grid effect in the snake body so you can see how long it is
    context.fillRect(cell.x, cell.y, grid-1, grid-1);

    // snake ate apple
    if (cell.x === apple.x && cell.y === apple.y) {
      snake.maxCells++;

      // canvas is 400x400 which is 25x25 grids
      apple.x = getRandomInt(0, 25) * grid;
      apple.y = getRandomInt(0, 25) * grid;
    }

    // check collision with all cells after this one (modified bubble sort)
    for (var i = index + 1; i < snake.cells.length; i++) {

      // snake occupies same space as a body part. reset game
      if (cell.x === snake.cells[i].x && cell.y === snake.cells[i].y) {
        snake.x = 160;
        snake.y = 160;
        snake.cells = [];
        snake.maxCells = 4;
        snake.dx = grid;
        snake.dy = 0;

        apple.x = getRandomInt(0, 25) * grid;
        apple.y = getRandomInt(0, 25) * grid;
      }
    }
  });
}

// listen to keyboard events to move the snake
document.addEventListener('keydown', function(e) {
  // prevent snake from backtracking on itself by checking that it's
  // not already moving on the same axis (pressing left while moving
  // left won't do anything, and pressing right while moving left
  // shouldn't let you collide with your own body)

  // left arrow key
  if (e.which === 37 && snake.dx === 0) {
    snake.dx = -grid;
    snake.dy = 0;
  }
  // up arrow key
  else if (e.which === 38 && snake.dy === 0) {
    snake.dy = -grid;
    snake.dx = 0;
  }
  // right arrow key
  else if (e.which === 39 && snake.dx === 0) {
    snake.dx = grid;
    snake.dy = 0;
  }
  // down arrow key
  else if (e.which === 40 && snake.dy === 0) {
    snake.dy = grid;
    snake.dx = 0;
  }
});

// start the game
requestAnimationFrame(loop);
</script>
</body>
</html>