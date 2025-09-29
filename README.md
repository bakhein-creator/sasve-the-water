<!DOCTYPE html>
<html lang="ko">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <title>강릉 가뭄: 물을 소중히</title>
   <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
   <style>
       @import url('https://fonts.googleapis.com/css2?family=Jua&display=swap');
      
       body {
           font-family: 'Jua', sans-serif;
           background-color: #f0f8ff;
           display: flex;
           justify-content: center;
           align-items: center;
           height: 100vh;
           margin: 0;
           flex-direction: column;
           overflow: hidden;
           color: #333;
       }


       .game-container {
           display: flex;
           flex-direction: column;
           align-items: center;
           background-color: #fff;
           border: 8px solid #4a90e2;
           border-radius: 20px;
           box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
           padding: 20px;
           width: 90%;
           max-width: 800px;
           box-sizing: border-box;
           position: relative;
       }


       .game-title {
           font-size: 3rem;
           color: #1e88e5;
           margin-bottom: 10px;
           text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
       }
      
       .game-subtitle {
           font-size: 1.5rem;
           color: #64b5f6;
           margin-bottom: 20px;
           text-align: center;
       }


       .controls {
           display: flex;
           justify-content: center;
           width: 100%;
           margin-bottom: 20px;
           font-size: 1.2rem;
           text-align: center;
       }
      
       .control-item {
           display: flex;
           flex-direction: column;
           align-items: center;
           color: #555;
       }
      
       .control-key {
           background-color: #e0e0e0;
           padding: 5px 10px;
           border-radius: 5px;
           font-family: 'Courier New', Courier, monospace;
           font-weight: bold;
           margin-top: 5px;
       }


       #gameCanvas {
           border: 2px dashed #99d9ea;
           background-color: #e3f2fd;
           border-radius: 10px;
           width: 100%;
           height: 600px;
           position: relative;
       }
      
       .ui-container {
           display: flex;
           justify-content: space-between;
           width: 100%;
           margin-top: 20px;
           align-items: center;
           flex-wrap: wrap;
       }
      
       .ui-item {
           display: flex;
           flex-direction: column;
           align-items: flex-start;
       }
      
       .ui-item h3 {
           margin: 0 0 5px 0;
           font-size: 1rem;
           color: #555;
       }


       .gauge-container-vertical {
           position: absolute;
           right: 10px;
           top: 50%;
           transform: translateY(-50%);
           width: 30px;
           height: 80%;
           background-color: #e0e0e0;
           border-radius: 15px;
           overflow: hidden;
           border: 2px solid #ccc;
           display: flex;
           flex-direction: column-reverse;
       }


       #leakageGauge {
           width: 100%;
           background: linear-gradient(0deg, #ffcc80, #ef5350);
           height: 0%;
           transition: height 0.1s linear;
       }
      
       .time-display {
           width: 25%;
           font-size: 1.5rem;
           color: #ef5350;
           text-align: right;
           font-weight: bold;
       }
      
       .best-time-display {
           font-size: 1.5rem;
           color: #4CAF50;
           font-weight: bold;
       }


       .game-message {
           position: absolute;
           top: 50%;
           left: 50%;
           transform: translate(-50%, -50%);
           background-color: rgba(255, 255, 255, 0.9);
           padding: 30px;
           border-radius: 15px;
           box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.3);
           text-align: center;
           z-index: 10;
           display: none;
           flex-direction: column;
           align-items: center;
       }


       .message-title {
           font-size: 2.5rem;
           margin-bottom: 10px;
       }
      
       .message-text {
           font-size: 1.5rem;
           margin-bottom: 20px;
       }
      
       .restart-button, .start-button {
           padding: 10px 20px;
           font-size: 1.2rem;
           cursor: pointer;
           background-color: #4CAF50;
           color: white;
           border: none;
           border-radius: 10px;
           box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
           transition: transform 0.2s, background-color 0.2s;
       }
      
       .restart-button:hover, .start-button:hover {
           background-color: #45a049;
           transform: translateY(-2px);
       }
      
       .water-droplet-logo {
           position: absolute;
           width: 30px;
           height: 30px;
       }
      
       /* 댐과 집 */
       .dam {
           position: absolute;
           top: 10px;
           left: 50%;
           transform: translateX(-50%);
           width: 100px;
           height: 50px;
           background-color: #1e88e5;
           border-radius: 10px 10px 0 0;
           display: flex;
           justify-content: center;
           align-items: center;
           color: white;
           font-size: 1.2rem;
       }
      
       .house {
           position: absolute;
           bottom: 10px;
           left: 50%;
           transform: translateX(-50%);
           width: 80px;
           height: 80px;
           background-color: #ff9800;
           border-radius: 10px;
           display: flex;
           justify-content: center;
           align-items: center;
           color: white;
           font-size: 2rem;
       }
      
       .house::before {
           content: '🏠';
       }
      
       .water-carrier {
           position: absolute;
           width: 60px;
           height: 60px;
           display: flex;
           justify-content: center;
           align-items: center;
           font-size: 3rem;
           transition: transform 0.1s linear;
       }


       .balance-bar {
           position: absolute;
           top: -20px;
           width: 80px;
           height: 8px;
           background-color: #e0e0e0;
           border-radius: 4px;
       }
      
       .balance-fill {
           height: 100%;
           background-color: #4CAF50;
           width: 100%;
           border-radius: 4px;
           transform-origin: center;
           transition: width 0.1s linear;
       }
   </style>
</head>
<body>


<div class="game-container">
   <h1 class="game-title">강릉 가뭄</h1>
   <p class="game-subtitle">스페이스바를 빠르게 눌러 균형을 유지하세요!</p>


   <div class="controls">
       <div class="control-item">
           <span class="control-key">Space</span>
           <p>균형 잡고 이동</p>
       </div>
   </div>
  
   <div id="gameCanvas">
       <div class="dam">댐</div>
       <div class="water-carrier" id="waterCarrier">
           🐷
           <div class="balance-bar">
               <div class="balance-fill" id="balanceFill"></div>
           </div>
       </div>
       <div class="house"></div>
       <div class="gauge-container-vertical">
           <div id="leakageGauge"></div>
       </div>
   </div>
  
   <div class="ui-container">
       <div class="ui-item">
           <h3>최고 기록</h3>
           <div class="best-time-display" id="bestTimeDisplay"></div>
       </div>
       <div class="ui-item">
           <h3>현재 시간</h3>
           <div class="time-display" id="timeDisplay"></div>
       </div>
   </div>
  
   <div class="game-message" id="introMessage">
       <h2 class="message-title">게임 설명</h2>
       <p class="message-text">
           강릉에 가뭄이 찾아왔습니다. 댐에서 물을 길어 집까지 운반해야 합니다.
           <br>
           자동으로 앞으로 이동하는 물통의 균형을 유지해야 합니다.
           <br>
           **스페이스바**를 빠르게 연타해서 균형을 잡으세요.
           <br>
           물통이 기울면 물이 새어 게이지가 차오릅니다. 게이지가 끝까지 차거나, 스페이스바를 꾹 누르고 있으면 게임 오버!
       </p>
       <button class="start-button" id="startButton">게임 시작</button>
   </div>


   <div class="game-message" id="gameOverMessage">
       <h2 class="message-title" id="messageTitle">게임 오버</h2>
       <p class="message-text" id="messageText"></p>
       <button class="restart-button" id="restartButton">다시 시작</button>
   </div>
</div>


<script>
   const canvas = document.getElementById('gameCanvas');
   const leakageGauge = document.getElementById('leakageGauge');
   const timeDisplay = document.getElementById('timeDisplay');
   const bestTimeDisplay = document.getElementById('bestTimeDisplay');
   const introMessage = document.getElementById('introMessage');
   const gameOverMessage = document.getElementById('gameOverMessage');
   const messageTitle = document.getElementById('messageTitle');
   const messageText = document.getElementById('messageText');
   const restartButton = document.getElementById('restartButton');
   const startButton = document.getElementById('startButton');
   const waterCarrier = document.getElementById('waterCarrier');
   const balanceFill = document.getElementById('balanceFill');
  
   const initialBestTime = 8.4;
   let bestTime = localStorage.getItem('bestTime');


   let gaugeValue = 0;
   const maxGauge = 100;
   let gameStartTime;
   let gameInterval;
   let timerInterval;


   let carrierY;
   const carrierSpeed = 0.5;
  
   let balance = 100;
   const maxBalance = 100;
   const balanceDecayRate = 0.5;
   const balanceGainRate = 10;
  
   let lastDropletTime = 0;
   const dropletInterval = 200;


   let spacebarPressed = false;
   let spacebarPressStartTime = 0;
   const maxPressTime = 500;


   function initGame() {
       if (!bestTime || parseFloat(bestTime) > initialBestTime) {
           bestTime = initialBestTime.toString();
           localStorage.setItem('bestTime', bestTime);
       }
       bestTimeDisplay.textContent = `${parseFloat(bestTime).toFixed(2)}초`;
       introMessage.style.display = 'flex';
       canvas.style.display = 'none';
   }


   function startGame() {
       gaugeValue = 0;
       balance = maxBalance;
       gameStartTime = Date.now();
      
       if (leakageGauge) {
           leakageGauge.style.height = '0%';
       }
       balanceFill.style.width = '100%';
       timeDisplay.textContent = '0.00초';
       gameOverMessage.style.display = 'none';
       introMessage.style.display = 'none';
       canvas.style.display = 'block';


       const damRect = document.querySelector('.dam').getBoundingClientRect();
       const canvasRect = canvas.getBoundingClientRect();
       carrierY = damRect.bottom - canvasRect.top;
       waterCarrier.style.top = `${carrierY}px`;
      
       const waterDroplets = document.querySelectorAll('.water-droplet-logo');
       waterDroplets.forEach(droplet => droplet.remove());


       gameInterval = setInterval(gameLoop, 10);
       timerInterval = setInterval(updateTimer, 10);
   }
  
   function updateTimer() {
       const timeElapsed = (Date.now() - gameStartTime) / 1000;
       timeDisplay.textContent = `${timeElapsed.toFixed(2)}초`;
   }


   function gameLoop() {
       gaugeValue += (100 - balance) / 100 * 0.1;
       balance = Math.max(0, balance - balanceDecayRate);
       balanceFill.style.width = `${balance}%`;


       if (leakageGauge) {
           leakageGauge.style.height = `${gaugeValue}%`;
       }


       carrierY += carrierSpeed;
       waterCarrier.style.top = `${carrierY}px`;


       const currentTime = Date.now();
       if (currentTime - lastDropletTime > dropletInterval && (100 - balance) > 50) {
           createWaterDroplet();
           lastDropletTime = currentTime;
       }
      
       if (gaugeValue >= maxGauge) {
           endGame('게이지 초과! 물이 넘쳐버렸어요.', false);
       }
       if (balance <= 0) {
           endGame('균형을 잃었어요! 물통을 놓쳤습니다.', false);
       }
      
       if (spacebarPressed && (Date.now() - spacebarPressStartTime > maxPressTime)) {
            endGame('스페이스바를 너무 오래 눌렀어요! 물통을 망가뜨렸습니다.', false);
       }


       const houseRect = document.querySelector('.house').getBoundingClientRect();
       const carrierRect = waterCarrier.getBoundingClientRect();
       if (carrierRect.bottom >= houseRect.top) {
           endGame('임무 성공! 물을 안전하게 운반했습니다.', true);
       }
   }
  
   function createWaterDroplet() {
       const droplet = document.createElement('div');
       droplet.classList.add('water-droplet-logo');
       droplet.innerHTML = `<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
           <path d="M50,10 C70,10 85,25 85,45 C85,70 50,90 50,90 C50,90 15,70 15,45 C15,25 30,10 50,10 Z" fill="#4a90e2"/>
           <circle cx="65" cy="35" r="5" fill="#fff" opacity="0.8"/>
       </svg>`;
      
       const carrierRect = waterCarrier.getBoundingClientRect();
       const canvasRect = canvas.getBoundingClientRect();


       droplet.style.left = `${carrierRect.left + carrierRect.width / 2}px`;
       droplet.style.top = `${carrierRect.bottom}px`;
      
       canvas.appendChild(droplet);


       const dropSpeed = 2;
       function dropLoop() {
           const currentTop = parseFloat(droplet.style.top);
           droplet.style.top = `${currentTop + dropSpeed}px`;
          
           if (currentTop > canvasRect.bottom) {
               droplet.remove();
           } else {
               requestAnimationFrame(dropLoop);
           }
       }
       requestAnimationFrame(dropLoop);
   }


   function endGame(message, isWin) {
       clearInterval(gameInterval);
       clearInterval(timerInterval);
      
       const bestTimeDisplayValue = bestTime ? `${parseFloat(bestTime).toFixed(2)}초` : '없음';


       if (isWin) {
           const timeTaken = (Date.now() - gameStartTime) / 1000;
           const formattedTime = timeTaken.toFixed(2);
          
           if (timeTaken < parseFloat(bestTime)) {
               messageTitle.textContent = '최고 기록 달성!';
               messageTitle.style.color = '#4CAF50';
               messageText.textContent = `${formattedTime}초 만에 물을 안전하게 운반했어요!\n낭비된 물: ${gaugeValue.toFixed(2)}%\n최고 기록: ${formattedTime}초`;
              
               bestTime = timeTaken.toString();
               localStorage.setItem('bestTime', bestTime);
               bestTimeDisplay.textContent = `${formattedTime}초`;
           } else {
               messageTitle.textContent = '게임 승리!';
               messageTitle.style.color = '#4CAF50';
               messageText.textContent = `${formattedTime}초 만에 물을 안전하게 운반했어요!\n낭비된 물: ${gaugeValue.toFixed(2)}%\n최고 기록: ${bestTimeDisplayValue}`;
           }


       } else {
           messageTitle.textContent = '게임 오버';
           messageText.textContent = message + `\n낭비된 물: ${gaugeValue.toFixed(2)}%\n최고 기록: ${bestTimeDisplayValue}`;
           messageTitle.style.color = '#ef5350';
       }
      
       gameOverMessage.style.display = 'flex';
   }
  
   document.addEventListener('keydown', (e) => {
       if (e.key === ' ' && !spacebarPressed) {
           spacebarPressed = true;
           spacebarPressStartTime = Date.now();
           balance = Math.min(maxBalance, balance + balanceGainRate);
       }
   });


   document.addEventListener('keyup', (e) => {
       if (e.key === ' ') {
           spacebarPressed = false;
       }
   });


   restartButton.addEventListener('click', startGame);
   startButton.addEventListener('click', startGame);


   window.onload = initGame;
</script>


</body>
</html>



