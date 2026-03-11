<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Logo Quiz Arena</title>

<style>
body{
  margin:0;
  font-family:Arial, sans-serif;
  background:#0f172a;
  color:white;
  text-align:center;
}

.container{
  max-width:1200px;
  margin:auto;
  padding:20px;
}

h1{font-size:42px;}

.logo{
  width:320px;
  height:320px;
  object-fit:contain;
  background:white;
  padding:20px;
  border-radius:20px;
}

input{
  padding:14px;
  width:350px;
  font-size:18px;
  margin-top:20px;
}

button{
  padding:14px 28px;
  font-size:18px;
  margin-top:15px;
  cursor:pointer;
}

#timer{
  font-size:40px;
  font-weight:bold;
  margin:20px;
  color:#38bdf8;
}

.correct{color:#22c55e;font-size:26px;}
.wrong{color:#ef4444;font-size:26px;}
</style>
</head>

<body>

<div class="container">

<h1>Logo Quiz Challenge</h1>

<div id="startScreen">
<input type="text" id="playerName" placeholder="Enter your name">
<br>
<button onclick="startQuiz()">Start</button>
</div>

<div id="quizScreen" style="display:none;">
<div id="timer">20</div>
<img id="logo" class="logo">
<br>
<input type="text" id="answer" placeholder="Type logo name">
<br>
<button onclick="submitAnswer()">Submit</button>
<p id="feedback"></p>
</div>

<div id="resultScreen" style="display:none;">
<h1 id="finalScore"></h1>
<ul id="board"></ul>
<button onclick="location.reload()">Play Again</button>
</div>

</div>

<script>
const logos = [
{
imgs:[
"https://cdn-icons-png.flaticon.com/512/226/226770.png",
"https://upload.wikimedia.org/wikipedia/commons/d/d7/Android_robot.svg"
],
ans:"android"
},
{
imgs:[
"https://cdn-icons-png.flaticon.com/512/5968/5968804.png"
],
ans:"duolingo"
},
{
imgs:[
"https://play-lh.googleusercontent.com/XhHn0pV3hokW2N7DbStO2Qd6hw2yffA9H9n1tFoZT3zh0ElBTtPlqvGjufH6G_QDqg"
],
ans:"gps map camera"
},
{
imgs:[
"https://upload.wikimedia.org/wikipedia/commons/3/3b/Wynk_Music_Logo.png"
],
ans:"wynk"
},
{
imgs:[
"https://play-lh.googleusercontent.com/0Z6kTpsuRWpu9X6lzNN2O0oSbYqK0PXmXqQyV2pAonyz1ME3iAFH4x63HzlOAAb2YtM"
],
ans:"hunter assassin"
},
{
imgs:[
"https://upload.wikimedia.org/wikipedia/en/5/5a/Hill_Climb_Racing_logo.png"
],
ans:"hill climb racing"
},
{
imgs:[
"https://cdn-icons-png.flaticon.com/512/124/124570.png"
],
ans:"flash"
},
{
imgs:[
"https://upload.wikimedia.org/wikipedia/commons/8/8d/Google_Pixel_logo.svg"
],
ans:"pixel"
},
{
imgs:[
"https://cdn-icons-png.flaticon.com/512/174/174881.png"
],
ans:"wordpress"
},
{
imgs:[
"https://upload.wikimedia.org/wikipedia/commons/6/6d/Motorola_logo.svg"
],
ans:"motorola"
},
{
imgs:[
"https://upload.wikimedia.org/wikipedia/commons/4/4d/OpenAI_Logo.svg"
],
ans:"openai"
},
{
imgs:[
"https://cdn-icons-png.flaticon.com/512/145/145808.png"
],
ans:"pinterest"
},
{
imgs:[
"https://play-lh.googleusercontent.com/0rYF9bBlt3F4iAfDdcQa1Ji4V859WUvDdDhcE7UZ5EKeosFVJeFt3PcTJS3BM4tiTqc"
],
ans:"groww"
},
{
imgs:[
"https://upload.wikimedia.org/wikipedia/en/7/7e/Mini_Militia_Doodle_Army_2_logo.png"
],
ans:"mini militia"
},
{
imgs:[
"https://cdn-icons-png.flaticon.com/512/5968/5968827.png",
"https://upload.wikimedia.org/wikipedia/commons/a/a0/Firefox_logo%2C_2019.svg"
],
ans:"firefox"
}
];

let current = 0;
let score = 0;
let timer;
let timeLeft = 20;

function startQuiz(){
  let name = document.getElementById("playerName").value;
  if(!name) return;

  document.getElementById("startScreen").style.display="none";
  document.getElementById("quizScreen").style.display="block";
  loadQuestion();
}

function loadQuestion(){
  if(current >= logos.length){
    endQuiz();
    return;
  }

  loadImageWithFallback(logos[current].imgs);

  document.getElementById("answer").value="";
  document.getElementById("feedback").innerText="";

  timeLeft = 20;
  updateTimer();

  timer = setInterval(()=>{
    timeLeft--;
    updateTimer();
    if(timeLeft === 0){
      clearInterval(timer);
      nextQuestion();
    }
  },1000);
}

function loadImageWithFallback(list){
  const img = document.getElementById("logo");
  let index = 0;

  img.onerror = function(){
    index++;
    if(index < list.length){
      img.src = list[index];
    }
  };

  img.src = list[index];
}

function updateTimer(){
  document.getElementById("timer").innerText = timeLeft + "s";
}

function submitAnswer(){
  clearInterval(timer);

  let user = document.getElementById("answer").value.toLowerCase().trim();

  if(user.includes(logos[current].ans)){
    score++;
    document.getElementById("feedback").innerHTML =
    "<span class='correct'>Correct</span>";
  }else{
    document.getElementById("feedback").innerHTML =
    "<span class='wrong'>Wrong</span>";
  }

  setTimeout(nextQuestion,1200);
}

function nextQuestion(){
  current++;
  loadQuestion();
}

function endQuiz(){
  document.getElementById("quizScreen").style.display="none";
  document.getElementById("resultScreen").style.display="block";

  document.getElementById("finalScore").innerText =
  "Score: " + score + " / " + logos.length;
}
</script>

</body>
</html>
