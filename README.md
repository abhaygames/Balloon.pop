# Balloon.pop
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Magic Path</title>

<style>
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:linear-gradient(135deg,#172554,#4c1d95);
  color:white;
  text-align:center;
}

h1{
  margin:20px 0 5px;
}

#info{
  font-size:20px;
  margin:15px;
}

#board{
  width:90vw;
  max-width:400px;
  margin:20px auto;
  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:10px;
}

.cell{
  height:75px;
  border:0;
  border-radius:15px;
  background:#475569;
}

.cell.show{
  background:#facc15;
}

.cell.correct{
  background:#22c55e;
}

.cell.wrong{
  background:#ef4444;
}

button{
  font-size:18px;
  font-weight:bold;
  border:0;
  border-radius:12px;
  padding:12px 22px;
  margin:8px;
}

#start{
  background:#facc15;
}

#message{
  min-height:28px;
  font-weight:bold;
}
</style>
</head>

<body>

<h1>🪄 Magic Path</h1>

<div id="info">
⭐ Level: <span id="level">1</span>
</div>

<div id="message">
Press Start Game 🎮
</div>

<div id="board"></div>

<button id="start">▶ Start Game</button>

<script>

var level = 1;
var path = [];
var player = [];
var playing = false;

var board = document.getElementById("board");
var message = document.getElementById("message");
var levelText = document.getElementById("level");
var startButton = document.getElementById("start");

function makeBoard(){

  board.innerHTML = "";

  for(var i=0;i<16;i++){

    var cell = document.createElement("button");

    cell.className = "cell";

    cell.setAttribute("data-id",i);

    cell.onclick = function(){

      chooseCell(
        Number(this.getAttribute("data-id")),
        this
      );

    };

    board.appendChild(cell);
  }
}

function makePath(){

  path = [];

  var amount = Math.min(3 + level,10);

  while(path.length < amount){

    var number = Math.floor(Math.random()*16);

    if(path.indexOf(number) === -1){

      path.push(number);

    }
  }
}

function startGame(){

  level = 1;

  levelText.innerHTML = level;

  startLevel();
}

function startLevel(){

  player = [];

  playing = false;

  makeBoard();

  makePath();

  message.innerHTML = "👀 Remember the yellow path!";

  for(var i=0;i<path.length;i++){

    board.children[path[i]].classList.add("show");

  }

  setTimeout(function(){

    for(var i=0;i<path.length;i++){

      board.children[path[i]].classList.remove("show");

    }

    playing = true;

    message.innerHTML = "🧠 Now tap the path!";

  },2000);

}

function chooseCell(id,cell){

  if(!playing){

    return;

  }

  var position = player.length;

  if(id === path[position]){

    cell.classList.add("correct");

    player.push(id);

    if(player.length === path.length){

      playing = false;

      message.innerHTML = "🎉 Perfect! Next level!";

      setTimeout(function(){

        level++;

        levelText.innerHTML = level;

        startLevel();

      },1000);

    }

  }else{

    cell.classList.add("wrong");

    playing = false;

    message.innerHTML = "💥 Wrong! Try again.";

  }

}

startButton.onclick = startGame;

makeBoard();

</script>

</body>
</html>
