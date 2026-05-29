# cricket-game

<!DOCTYPE html>
<html>
<head>
<title>🏏 Cricket Game</title>

<style>
body{
    margin:0;
    overflow:hidden;
    font-family:Arial;
    background:linear-gradient(green, darkgreen);
    color:white;
    text-align:center;
}

h1{
    margin-top:10px;
}

#scoreboard{
    font-size:24px;
    margin:10px;
}

#ground{
    width:100%;
    height:500px;
    position:relative;
    overflow:hidden;
}

#bat{
    width:140px;
    height:20px;
    background:brown;
    position:absolute;
    bottom:40px;
    left:150px;
    border-radius:10px;
}

#ball{
    width:30px;
    height:30px;
    background:red;
    border-radius:50%;
    position:absolute;
    top:50px;
    left:180px;
}

#player{
    position:absolute;
    bottom:70px;
    left:170px;
    font-size:50px;
}

</style>
</head>

<body>

<h1>🏏 Cricket Game</h1>

<div id="scoreboard">
Score: <span id="score">0</span>
| Wickets: <span id="wickets">0</span>
</div>

<div id="message"></div>

<div id="ground">

<div id="player">🏏</div>
<div id="bat"></div>
<div id="ball"></div>

</div>

<audio id="hitSound">
<source src="https://www.soundjay.com/button/sounds/button-16.mp3">
</audio>

<script>

let bat = document.getElementById("bat");
let ball = document.getElementById("ball");

let scoreText = document.getElementById("score");
let wicketText = document.getElementById("wickets");
let message = document.getElementById("message");

let hitSound = document.getElementById("hitSound");

let batX = 150;
let ballX = Math.random() * 300;
let ballY = 50;

let score = 0;
let wickets = 0;

document.addEventListener("keydown", function(e){

    if(e.key=="ArrowLeft"){
        batX -= 25;
    }

    if(e.key=="ArrowRight"){
        batX += 25;
    }

    if(batX < 0){
        batX = 0;
    }

    if(batX > window.innerWidth - 140){
        batX = window.innerWidth - 140;
    }

    bat.style.left = batX + "px";
});

function resetBall(){

    ballY = 50;
    ballX = Math.random() * (window.innerWidth - 50);

    ball.style.left = ballX + "px";
}

function moveBall(){

    ballY += 6;
    ball.style.top = ballY + "px";

    let batLeft = batX;
    let batRight = batX + 140;

    let ballCenter = ballX + 15;

    // HIT
    if(ballY > 410 && ballCenter > batLeft && ballCenter < batRight){

        hitSound.play();

        let runOptions = [1,2,4,6];
        let run = runOptions[Math.floor(Math.random()*runOptions.length)];

        score += run;

        if(run == 4){
            message.innerHTML = "🔥 FOUR!";
        }
        else if(run == 6){
            message.innerHTML = "🚀 SIX!";
        }
        else{
            message.innerHTML = "🏃 " + run + " Runs";
        }

        scoreText.innerHTML = score;

        resetBall();
    }

    // MISS = WICKET
    if(ballY > 500){

        wickets++;

        wicketText.innerHTML = wickets;

        message.innerHTML = "❌ WICKET!";

        resetBall();

        if(wickets >= 3){

            alert("Game Over! Final Score: " + score);

            score = 0;
            wickets = 0;

            scoreText.innerHTML = score;
            wicketText.innerHTML = wickets;
        }
    }
}

setInterval(moveBall, 30);

</script>

</body>
</html>