# Balloon.pop
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Magic Path</title>

<style>
*{
  box-sizing:border-box;
}

body{
  margin:0;
  font-family:Arial,sans-serif;
  background:linear-gradient(135deg,#10152f,#253b70);
  color:white;
  text-align:center;
  min-height:100vh;
}

h1{
  margin:20px 0 5px;
  font-size:32px;
}

.subtitle{
  opacity:.8;
  margin-bottom:15px;
}

#stats{
  display:flex;
  justify-content:center;
  gap:12px;
  flex-wrap:wrap;
  margin:10px;
}

.stat{
  background:rgba(255,255,255,.12);
  padding:10px 18px;
  border-radius:15px;
}

#message{
  min-height:25px;
  margin:10px;
  font-weight:bold;
}

#board{
  width:min(92vw,430px);
  margin:15px auto;
  display:grid;
  grid-template-columns:repeat(5,1fr);
  gap:8px;
}

.cell{
  aspect-ratio:1;
  border:0;
  border-radius:14px;
  background:#3d4d78;
  box-shadow:0 5px 10px rgba(0,0,0,.25);
  cursor:pointer;
  transition:.15s;
}

.cell:active{
  transform:scale(.92);
}

.cell.path{
  background:#ffd166;
}

.cell.correct{
  background:#06d6a0;
}

.cell.wrong{
  background:#ef476f;
}

button{
  border:0;
  border-radius:14px;
  padding:13px 24px;
  margin:7px;
  font-size:17px;
  font-weight:bold;
  cursor:pointer;
  background:#ffffff;
  color:#18213f;
}

#startBtn{
  background:#ffd166;
}

#levelText{
  font-size:21px;
  font-weight:bold;
}

.small{
  opacity:.7;
  font-size:13px;
  margin:15px;
}
</style>
</head>

<body>

<h1>🪄 Magic Path</h1>

<div class="subtitle">
  Remember the magic path!
</div>

<div id="stats">

  <div class="stat">
    ⭐ Level: <span id="level">1</span>
  </div>

  <div class="stat">
    🏆 Best: <span id="best">0</span>
  </div>

</div>

<div id="levelText">
  Ready?
</div>

<div id="message">
  Press Start Game
</div>

<div id="board"></div>

<button id="startBtn">
  ▶ Start Game
</button>

<button id="restartBtn">
  🔄 Restart
</button>

<div class="small">
  Watch the golden path carefully, then tap the same squares.
</div>

<script>

const board = document.getElementById("board");
const levelEl = document.getElementById("level");
const bestEl = document.getElementById("best");
const message = document.getElementById("message");
const levelText = document.getElementById("levelText");
const startBtn = document.getElementById("startBtn");
const restartBtn = document.getElementById("restartBtn");

let level = 1;
let path = [];
let playerPath = [];
let accepting = false;

let best = Number(localStorage.getItem("magicPathBest") || 0);
bestEl.textContent = best;

function createBoard(){

  board.innerHTML = "";

  for(let i=0;i<25;i++){

    const cell = document.createElement("button");

    cell.className = "cell";
    cell.dataset.index = i;

    cell.addEventListener("click",function(){
      tapCell(i,cell);
    });

    board.appendChild(cell);
  }
}

function randomPath(){

  const needed = Math.min(3 + level, 15);

  let result = [];

  while(result.length < needed){

    const n = Math.floor(Math.random()*25);

    if(!result.includes(n)){
      result.push(n);
    }
  }

  return result;
}

function showPath(){

  accepting = false;

  message.textContent = "👀 Remember the path!";

  path.forEach(index => {

    board.children[index].classList.add("path");

  });

  const showTime = Math.max(900, 2200 - level*80);

  setTimeout(() => {

    path.forEach(index => {

      board.children[index].classList.remove("path");

    });

    accepting = true;

    message.textContent = "🧠 Now recreate the path!";

  },showTime);
}

function tapCell(index,cell){

  if(!
