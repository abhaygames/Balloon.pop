# Balloon.pop<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Balloon Pop 🎈</title>

<style>
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:linear-gradient(#bdeeff,#f8fbff);
  text-align:center;
  overflow:hidden;
}

h1{
  color:#246;
  margin:18px 0 8px;
}

#info{
  font-size:20px;
  font-weight:bold;
  margin-bottom:10px;
}

#game{
  position:relative;
  height:75vh;
  min-height:450px;
  overflow:hidden;
  border-top:3px solid white;
}

.balloon{
  position:absolute;
  width:65px;
  height:80px;
  border-radius:50% 50% 45% 45%;
  cursor:pointer;
  box-shadow:inset -8px -10px 15px rgba(0,0,0,.15);
  animation:floatUp 4s linear forwards;
}

.balloon:after{
  content:"";
  position:absolute;
  width:2px;
  height:55px;
  background:#555;
  left:50%;
  top:78px;
}

@keyframes floatUp{
  from{bottom:-100px}
  to{bottom:110%}
}

button{
  background:#1976d2;
  color:white;
  border:0;
  padding:12px 25px;
  border-radius:12px;
  font-size:17px;
  margin:8px;
}
</style>
</head>

<body>

<h1>🎈 Balloon Pop 🎈</h1>

<div id="info">
⭐ Score: <span id="score">0</span>
</div>

<button onclick="startGame()">Start Game</button>

<div id="game"></div>

<script>

let score = 0;
let gameTimer;

const colors = [
  "#ff4d6d",
  "#ffbe0b",
  "#3a86ff",
  "#8338ec",
  "#06d6a0"
];

function startGame(){

  score = 0;
  document.getElementById("score").textContent = score;

  document.getElementById("game").innerHTML = "";

  clearInterval(gameTimer);

  gameTimer = setInterval(createBalloon,700);
}

function createBalloon(){

  const game = document.getElementById("game");

  const balloon = document.createElement("div");

  balloon.className = "balloon";

  balloon.style.background =
    colors[Math.floor(Math.random()*colors.length)];

  balloon.style.left =
    Math.random()*85 + "%";

  balloon.onclick = function(){

    score++;

    document.getElementById("score").textContent = score;

    balloon.remove();

    if(score >= 20){

      clearInterval(gameTimer);

      setTimeout(function(){

        alert("🎉 Amazing! You scored 20! 🏆");

      },100);
    }
  };

  game.appendChild(balloon);

  setTimeout(function(){

    if(balloon.parentNode){
      balloon.remove();
    }

  },4000);
}

</script>

</body>
</html>
