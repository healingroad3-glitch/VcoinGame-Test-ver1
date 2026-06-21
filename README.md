<!doctype html>
<html lang=ko>
<head>
<meta charset="utf-8">
<meta name="viewport"content="width=device-width,initial-scale=1">
<title>V코인 키우기ver.9</title>
<style>
body{text-align:center;
     font-family:sans-serif;
     background:#f5f5f5;
     padding:15px;
     max-width:600px;
     margin:auto;}
h1{color:black;}
#coinbtn{width:250px;
         height:90px;
         font-size:26px;
         border:none;
         border-radius:20px;
         margin:10px;
         cursor:pointer;}
#coinbtn:active{transform:scale(0.95);}.shopbtn{width:90%;
         max-width:400px;
         padding:15px;
         margin:8px;
         font-size:18px;
         border-radius:10px;}
#stats{font-size:22px;
       margin:20px auto;
       background:white;
       padding:15px;
       border-radius:15px;
       max-width:450px;}
#shop{margin-top:20px;
      background:white;
      padding:15px;
      border-radius:15px;
      max-width:500px;
      margin-left:auto;
      margin-right:auto;}
#progress{width:90%;
          max-width:400px;
          height:30px;}
#minerArea{position:relative;
           width:250px;
           height:150px;
           margin:20px auto;}
#character{position:absolute;
           left:40px;
           top:40px;
           font-size:60px;}
#pickaxe{position:absolute;
         left:90px;
         top:40px;
         font-size:40px;
         transform-origin:left bottom;
         animation:mine 0.8s infinite;
#coinAnim{position:absolute;
          right:20px;
          top:50px;
          font-size:40px;
          animation:coinMove 1s infinite;}#musicPlayerArea button{display:block;
                        width:90%;
                        margin:5px auto;}
@keyframes mine{0%{transform:rotate(0deg);}
50%
{transform:rotate(-40deg);}
100%
{transform:rotate(0deg);}}

@keyframes coinMove{0%{transform:translateY(0);}
50%
{transform:translateY(-10px);}    
100%
{transform:translateY(0);}}    
</style>
</head>
<body>
<h1>🪙V코인 키우기</h1>
<div id="stats">     
코인 : <span id="coin">0</span><br>
레벨 : <span id="level">1</span><br>
경험치 :<progress id="progress" value="0" max="10"></progress><br>
<p>⚙️자동 채굴기 :<span id="minerCount">0</span>개</p>
<p>⛏️수동 채굴기 :<span id="powerCount">0</span></p>
<p>🎵구매한 BGM:<span id="musicList">없음</span></p></div>
<div id="minerArea">
<div id="character">🧑‍🏭</div>
<div id="pickaxe">⛏️</div>
<div id="coinAnim">🪙</div></div>

<button id="coinbtn"onclick="earncoin()">🪙코인 얻기</button>
<p id="message"></p>
<div id="shop"style="display:none;">
<h2>🏪 상점</h2>
<button class="shopbtn" onclick="buypower()">⛏️수동 채굴기 힘 증가(100코인)</button><br>
<button class="shopbtn" onclick="buyAutominer()">⚙️자동 채굴기 구매(200코인)</button><br>
<div id="musicShop"style="display:none;">
<h2>🎵 BGM 상점</h2>
<button class="shopbtn" onclick="buyMusic1()">🎵 BGM 1 (5000코인)</button><br>
<button class="shopbtn" onclick="buyMusic2()">🎶 BGM 2 (10000코인)</button><br>
<button class="shopbtn" onclick="buyMusic3()">🎼 BGM 3 (15000코인)</button><br>
<button class="shopbtn" onclick="buyMusic4()">🎹 BGM 4 (20000코인)</button><br>
<div id="musicPlayerArea" style="display:none;"></div>
<button id="play1"style="display:none;"onclick="BGM1()">🎵BGM 1</button>
<button id="play2"style="display:none;"onclick="BGM2()">🎶BGM 2</button>
<button id="play3"style="display:none;"onclick="BGM3()">🎼BGM 3</button>
<button id="play4"style="display:none;"onclick="BGM4()">🎹BGM 4</button>
<div id="musicControl" style="display:none;">
<button onclick="stopBGM()">⏹️ 음악 정지</button></div>
</div>
</div>
</div>
    
<button class="shopbtn" onclick="savegame()">💾게임 저장</button>
<button class="shopbtn" onclick="resetGame()">🔄게임 초기화</button>
<script>
let Coin=0;
let Level=1;
let Exp=0;
let power=1;
let autominer=0;
let automineunlock=false;
let bgm1=false;
let bgm2=false;
let bgm3=false;
let bgm4=false;

function earncoin(){Coin+=power; Exp+=power;
levelcheck();
updateScreen();}

function levelcheck(){
            while(Exp>= Level*10){
                Exp -= Level*10;
                Level++;
                power++;
                document.getElementById("message").innerHTML ="🎉 레벨업! Lv." + Level;
            }
            if(Level>= 10 && !automineunlock){
                automineunlock = true;
                document.getElementById("message").innerHTML =
                "🔓 자동 채굴기 잠금 해제!";
            }
            if(Level>= 10){
                document.getElementById("shop").style.display = "block";
            }
            if(Level>= 30){
                document.getElementById("musicShop").style.display = "block";
            }
        }

        function buypower(){
            if(Coin>=100){
                Coin-=100;
                power++;
                document.getElementById("message").innerHTML="⛏️수동 채굴기 힘 증가! 현재 채굴력: " +power;
                updateScreen();
            }
            else{
                document.getElementById("message").innerHTML="❌코인이 부족합니다!";
            }
        }
        function buyAutominer(){
            if(Coin>=200 && automineunlock){
                Coin-=200;
                autominer++;
                document.getElementById("message").innerHTML="⚙️자동 채굴기 구매! 🤖자동 채굴기+1";
                updateScreen();
            }
            else if(!automineunlock){
                document.getElementById("message").innerHTML="🔒자동 채굴기가 잠겨 있습니다!";
            }
            else{
                document.getElementById("message").innerHTML="❌코인이 부족합니다!";
            }
        }
        function buyMusic1(){
            if(Coin>=5000 && !bgm1){
                Coin-=5000;
                bgm1=true;
                document.getElementById("musicPlayerArea").style.display="block";
                document.getElementById("message").innerHTML="🎵 BGM 1 해금!";
                updateScreen();
            }
            else{
                document.getElementById("message").innerHTML="❌코인이 부족하거나 이미 구매한 음악입니다!";
            }
        }
        function buyMusic2(){
            if(Coin>=10000 && !bgm2){
                Coin-=10000;
                bgm2=true;
                document.getElementById("musicPlayerArea").style.display="block";
                document.getElementById("message").innerHTML="🎶 BGM 2 해금!";
                updateScreen();
            }
            else{
                document.getElementById("message").innerHTML="❌코인이 부족하거나 이미 구매한 음악입니다!";
            }
        }
        function buyMusic3(){
            if(Coin>=15000 && !bgm3){
                Coin-=15000;
                bgm3=true;
                document.getElementById("musicPlayerArea").style.display="block";
                document.getElementById("message").innerHTML="🎼 BGM 3 해금!";
                updateScreen();
            }
            else{
                document.getElementById("message").innerHTML="❌코인이 부족하거나 이미 구매한 음악입니다!";
            }
        }
        function buyMusic4(){
            if(Coin>=20000 && !bgm4){
                Coin-=20000;
                bgm4=true;
                document.getElementById("musicPlayerArea").style.display="block";
                document.getElementById("message").innerHTML="🎹 BGM 4 해금!";
                updateScreen();
            }
            else{
                document.getElementById("message").innerHTML="❌코인이 부족하거나 이미 구매한 음악입니다!";
            }
        }
        function playBGM(file){
            let player = document.getElementById("bgmPlayer");
            player.src = file;
            player.loop = true;
            player.play();
            document.getElementById("musicControl").style.display="block";
        }
        function stopBGM(){
            let player = document.getElementById("bgmPlayer");
            player.pause();
            player.currentTime = 0;
            document.getElementById("message").innerHTML="⏹️ 음악 정지";
        }
        function BGM1(){
            if(bgm1){playBGM("bgm1.mp3");}else{document.getElementById("message").innerHTML= "🔒BGM1을 먼저 구매하세요!";}
        }
        function BGM2(){
            if(bgm2){playBGM("bgm2.mp3");}else{document.getElementById("message").innerHTML= "🔒BGM2을 먼저 구매하세요!";}
        }
        function BGM3(){
            if(bgm3){playBGM("bgm3.mp3");}else{document.getElementById("message").innerHTML= "🔒BGM3을 먼저 구매하세요!";}
        }
        function BGM4(){
            if(bgm4){playBGM("bgm4.mp3");}else{document.getElementById("message").innerHTML= "🔒BGM4을 먼저 구매하세요!";}
        }
        function updateScreen(){
            document.getElementById("coin").innerHTML = Coin;
            document.getElementById("level").innerHTML = Level;
            document.getElementById("minerCount").innerHTML = autominer;
            document.getElementById("powerCount").innerHTML = power;
            document.getElementById("progress").value = Exp;
            document.getElementById("progress").max = Level * 10;
            document.getElementById("play1").style.display =bgm1 ? "inline-block" : "none";
            document.getElementById("play2").style.display =bgm2 ? "inline-block" : "none";
            document.getElementById("play3").style.display =bgm3 ? "inline-block" : "none";
            document.getElementById("play4").style.display =bgm4 ? "inline-block" : "none";
            let musicText = [];
            if(bgm1) musicText.push("BGM1");
            if(bgm2) musicText.push("BGM2");
            if(bgm3) musicText.push("BGM3");
            if(bgm4) musicText.push("BGM4");
            document.getElementById("musicList").innerHTML =
            musicText.length > 0 ? musicText.join(", ") : "없음";
        }

        function savegame(){
            localStorage.setItem("Coin", Coin);
            localStorage.setItem("Level", Level);
            localStorage.setItem("Exp", Exp);
            localStorage.setItem("power", power);
            localStorage.setItem("autominer", autominer);
            localStorage.setItem("automineunlock", automineunlock);
            localStorage.setItem("bgm1", bgm1);
            localStorage.setItem("bgm2", bgm2);
            localStorage.setItem("bgm3", bgm3);
            localStorage.setItem("bgm4", bgm4);
            document.getElementById("message").innerHTML ="💾 게임이 저장되었습니다!";
        }
        
        function autosave(){
            savegame();
        }

        function loadgame(){
            if(localStorage.getItem("Coin")){
                Coin=parseInt(localStorage.getItem("Coin"));
            }
            if(localStorage.getItem("Level")){
                Level=parseInt(localStorage.getItem("Level"));
            }
            if(localStorage.getItem("Exp")){
                Exp=parseInt(localStorage.getItem("Exp"));
            }
            if(localStorage.getItem("power")){
                power=parseInt(localStorage.getItem("power"));
            }
            if(localStorage.getItem("autominer")){
                autominer=parseInt(localStorage.getItem("autominer"));
            }
            if(localStorage.getItem("automineunlock")){
                automineunlock=(localStorage.getItem("automineunlock") === "true");
            }
            if(localStorage.getItem("bgm1")){
                bgm1=(localStorage.getItem("bgm1")==="true");
            }
            if(localStorage.getItem("bgm2")){
                bgm2=(localStorage.getItem("bgm2")==="true");
            }
            if(localStorage.getItem("bgm3")){
                bgm3=(localStorage.getItem("bgm3")==="true");
            }
            if(localStorage.getItem("bgm4")){
                bgm4=(localStorage.getItem("bgm4")==="true");
            }
        }
        function resetGame(){
            if(confirm("게임을 초기화 하시겠습니까?")){
                localStorage.clear();
                location.reload();
            }
        }
        setInterval(function(){
            if(autominer>0){
                Coin+=autominer;
                Exp+=autominer;
                levelcheck();
                updateScreen();
            }
        },1000);
        loadgame();
        updateScreen();
        levelcheck();
        autosave();
    </script>
    <audio id="bgmPlayer"></audio>
    </body>
</html>