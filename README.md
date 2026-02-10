<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <title>Valentine Challenge</title>
   <style>
       * { 
           margin: 0; 
           padding: 0; 
           box-sizing: border-box; 
           font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
       }
       
       body { 
           min-height: 100vh; 
           display: flex; 
           justify-content: center; 
           align-items: center; 
           background: linear-gradient(135deg, #fff0f5 0%, #ffe4e9 50%, #ffd1dc 100%);
           overflow: hidden; 
           position: relative;
       }

       /* Background ambient hearts */
       .bg-heart {
           position: absolute;
           font-size: 20px;
           color: rgba(255, 182, 193, 0.3);
           animation: floatUp 15s infinite linear;
           pointer-events: none;
           z-index: 0;
       }

       @keyframes floatUp {
           0% {
               transform: translateY(100vh) rotate(0deg);
               opacity: 0;
           }
           10% {
               opacity: 0.6;
           }
           90% {
               opacity: 0.6;
           }
           100% {
               transform: translateY(-10vh) rotate(360deg);
               opacity: 0;
           }
       }
       
       .page { 
           position: absolute; 
           top: 0; 
           left: 0; 
           width: 100%; 
           height: 100%; 
           display: flex; 
           flex-direction: column; 
           justify-content: center; 
           align-items: center; 
           transition: opacity 0.6s, transform 0.6s; 
           z-index: 10;
       }
       
       .hidden { 
           opacity: 0; 
           pointer-events: none; 
           transform: scale(0.95); 
       }
       
       .card { 
           background: rgba(255, 255, 255, 0.85); 
           backdrop-filter: blur(12px);
           -webkit-backdrop-filter: blur(12px);
           padding: 40px 50px; 
           border-radius: 30px; 
           text-align: center; 
           color: #8b5a6e; 
           box-shadow: 
               0 8px 32px rgba(255, 158, 181, 0.2),
               inset 0 0 20px rgba(255, 255, 255, 0.6);
           border: 1px solid rgba(255, 255, 255, 0.5);
           max-width: 90%;
           width: 450px;
       }
       
       h1 { 
           margin-bottom: 20px; 
           font-size: 1.8rem; 
           font-weight: 500;
           color: #8b5a6e;
       }

       p {
           color: #8b5a6e;
           margin-bottom: 20px;
           font-size: 1.1rem;
       }
       
       button { 
           margin: 8px; 
           padding: 12px 30px; 
           font-size: 1.1rem; 
           border: none; 
           border-radius: 50px; 
           cursor: pointer; 
           background: linear-gradient(135deg, #ffb7c5, #ff9eb5); 
           color: #fff; 
           transition: all 0.3s ease; 
           box-shadow: 
               0 4px 15px rgba(255, 158, 181, 0.4),
               0 0 0 1px rgba(255, 255, 255, 0.3) inset;
           font-weight: 500;
       }
       
       button:hover { 
           transform: translateY(-2px);
           box-shadow: 
               0 6px 20px rgba(255, 158, 181, 0.5),
               0 0 0 1px rgba(255, 255, 255, 0.5) inset;
       }

       button:active {
           transform: translateY(0);
       }
       
       .keypad { 
           display: grid; 
           grid-template-columns: repeat(3, 70px); 
           gap: 10px; 
           margin-top: 20px; 
           justify-content: center;
       }
       
       .keypad button { 
           font-size: 1.3rem; 
           padding: 15px;
           margin: 0;
           border-radius: 15px;
       }
       
       input[type="text"] { 
           padding: 12px 20px; 
           font-size: 1.2rem; 
           border-radius: 50px; 
           border: 2px solid rgba(255, 183, 197, 0.3); 
           text-align: center; 
           margin: 15px 0; 
           width: 220px; 
           background: rgba(255, 255, 255, 0.9);
           color: #8b5a6e;
           outline: none;
           transition: all 0.3s ease;
       }

       input[type="text"]:focus {
           border-color: #ffb7c5;
           box-shadow: 0 0 0 3px rgba(255, 183, 197, 0.2);
       }
       
       .message { 
           margin-top: 15px; 
           font-size: 1.1rem; 
           min-height: 30px;
           font-weight: 500;
       }

       .message:empty {
           display: none;
       }
       
       .aura { 
           color: #ff69b4; 
           text-shadow: 0 0 10px rgba(255, 105, 180, 0.5);
           animation: pulse 1.5s infinite; 
           font-weight: 600;
       }
       
       @keyframes pulse { 
           0%, 100% { 
               text-shadow: 0 0 10px rgba(255, 105, 180, 0.5);
               transform: scale(1);
           } 
           50% { 
               text-shadow: 0 0 20px rgba(255, 105, 180, 0.8);
               transform: scale(1.02);
           } 
       }

       .emoji-btn {
           font-size: 2rem;
           padding: 10px 20px;
           background: rgba(255, 255, 255, 0.6);
           border: 2px solid rgba(255, 183, 197, 0.3);
           transition: all 0.3s ease;
       }

       .emoji-btn:hover {
           background: rgba(255, 183, 197, 0.2);
           transform: scale(1.1);
       }

       .emoji-btn.selected {
           opacity: 0.5;
           background: rgba(255, 183, 197, 0.4);
           transform: scale(0.95);
       }

       /* Victory Screen */
       .victory-screen {
           position: fixed;
           top: 0;
           left: 0;
           width: 100%;
           height: 100%;
           background: linear-gradient(135deg, #fff0f5 0%, #ffe4e9 100%);
           display: none;
           justify-content: center;
           align-items: center;
           flex-direction: column;
           z-index: 100;
           opacity: 0;
           transition: opacity 1s ease;
       }

       .victory-screen.active {
           display: flex;
           opacity: 1;
       }

       .victory-content {
           text-align: center;
           z-index: 10;
           padding: 2rem;
       }

       .victory-heart {
           font-size: 5rem;
           margin-bottom: 1rem;
           animation: heartbeat 1.5s ease-in-out infinite;
           display: inline-block;
       }

       @keyframes heartbeat {
           0%, 100% { transform: scale(1); }
           25% { transform: scale(1.1); }
           50% { transform: scale(1); }
           75% { transform: scale(1.05); }
       }

       .victory-message {
           font-size: 1.5rem;
           color: #8b5a6e;
           font-weight: 500;
           background: rgba(255, 255, 255, 0.9);
           padding: 2rem 3rem;
           border-radius: 20px;
           box-shadow: 0 10px 40px rgba(255, 158, 181, 0.3);
           backdrop-filter: blur(10px);
       }

       .floating-heart {
           position: absolute;
           font-size: 24px;
           animation: floatVictory 6s ease-in infinite;
           opacity: 0.8;
           pointer-events: none;
       }

       @keyframes floatVictory {
           0% {
               transform: translateY(100vh) scale(0) rotate(0deg);
               opacity: 0;
           }
           10% {
               opacity: 0.8;
               transform: scale(1);
           }
           90% {
               opacity: 0.8;
           }
           100% {
               transform: translateY(-100vh) scale(0.5) rotate(720deg);
               opacity: 0;
           }
       }

       @media (max-width: 480px) {
           .card {
               padding: 30px 25px;
               width: 92%;
           }
           h1 {
               font-size: 1.5rem;
           }
           .keypad {
               grid-template-columns: repeat(3, 60px);
           }
           .keypad button {
               padding: 12px;
           }
       }
   </style>
</head>
<body>

   <!-- Ambient Background Hearts -->
   <div id="ambient-hearts"></div>

   <!-- Page 1 -->
   <div class="page" id="page1">
       <div class="card">
           <h1>Hey Eva! 💕</h1>
           <p>I have small tests so we can reach the final level</p>
           <button onclick="nextPage(2)">Ready 💗</button>
       </div>
   </div>

   <!-- Page 2 - Birthday -->
   <div class="page hidden" id="page2">
       <div class="card">
           <h1>My Birthday?</h1>
           <input type="text" id="birthdayInput" placeholder="DD/MM/YYYY" readonly>
           <div class="keypad">
               <button onclick="addKey('1')">1</button>
               <button onclick="addKey('2')">2</button>
               <button onclick="addKey('3')">3</button>
               <button onclick="addKey('4')">4</button>
               <button onclick="addKey('5')">5</button>
               <button onclick="addKey('6')">6</button>
               <button onclick="addKey('7')">7</button>
               <button onclick="addKey('8')">8</button>
               <button onclick="addKey('9')">9</button>
               <button onclick="addKey('0')">0</button>
               <button onclick="addKey('/')">/</button>
               <button onclick="clearInput()">C</button>
           </div>
           <button onclick="checkBirthday()">Submit</button>
           <div class="message" id="birthdayMsg"></div>
       </div>
   </div>

   <!-- Page 3 - Favorite Color -->
   <div class="page hidden" id="page3">
       <div class="card">
           <h1>My Favorite Color?</h1>
           <input type="text" id="colorInput" placeholder="Type your answer">
           <button onclick="checkColor()">Submit</button>
           <div class="message" id="colorMsg"></div>
       </div>
   </div>

   <!-- Page 4 - Sisters -->
   <div class="page hidden" id="page4">
       <div class="card">
           <h1>How many sisters do I have?</h1>
           <div style="display: flex; flex-wrap: wrap; justify-content: center;">
               <button onclick="checkSisters(this)">2</button>
               <button onclick="checkSisters(this)">3</button>
               <button onclick="checkSisters(this)">1</button>
               <button onclick="checkSisters(this)">4</button>
           </div>
           <div class="message" id="sistersMsg"></div>
       </div>
   </div>

   <!-- Page 5 - First Kiss -->
   <div class="page hidden" id="page5">
       <div class="card">
           <h1>Where was our first kiss?</h1>
           <input type="text" id="kissInput" placeholder="Type your answer">
           <button onclick="checkKiss()">Submit</button>
           <div class="message" id="kissMsg"></div>
       </div>
   </div>

   <!-- Page 6 - What I Like Most -->
   <div class="page hidden" id="page6">
       <div class="card">
           <h1>What do I like most about you?</h1>
           <p>Pick 2 emojis</p>
           <div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 10px;">
               <button class="emoji-btn" onclick="selectEmoji(this)">🍑</button>
               <button class="emoji-btn" onclick="selectEmoji(this)">🐱</button>
               <button class="emoji-btn" onclick="selectEmoji(this)">🍒</button>
               <button class="emoji-btn" onclick="selectEmoji(this)">👁️</button>
               <button class="emoji-btn" onclick="selectEmoji(this)">🫦</button>
               <button class="emoji-btn" onclick="selectEmoji(this)">😁</button>
           </div>
           <div class="message" id="emojiMsg"></div>
       </div>
   </div>

   <!-- Page 7 - Final -->
   <div class="page hidden" id="page7">
       <div class="card">
           <h1>Would you be my Valentine?</h1>
           <button onclick="finalAnswer(true)">YES ❤️</button>
           <button onclick="finalAnswer(false)">NO 🖤</button>
           <div class="message" id="finalMsg"></div>
       </div>
   </div>

   <!-- Victory Screen -->
   <div class="victory-screen" id="victoryScreen">
       <div class="victory-content">
           <div class="victory-heart">💕</div>
           <div class="victory-message" id="victoryMessage">
               You've stolen my heart… 💖✨
           </div>
       </div>
       <div id="victory-hearts"></div>
   </div>

   <script>
       // Create ambient background hearts
       function createAmbientHearts() {
           const container = document.getElementById('ambient-hearts');
           const heartSymbols = ['💗', '💖', '💕', '🌸'];
           
           for (let i = 0; i < 15; i++) {
               const heart = document.createElement('div');
               heart.className = 'bg-heart';
               heart.textContent = heartSymbols[Math.floor(Math.random() * heartSymbols.length)];
               heart.style.left = Math.random() * 100 + '%';
               heart.style.animationDelay = Math.random() * 15 + 's';
               heart.style.fontSize = (Math.random() * 20 + 15) + 'px';
               container.appendChild(heart);
           }
       }

       createAmbientHearts();

       let selectedEmojis = [];
       
       function nextPage(n){
           document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'));
           document.getElementById('page' + n).classList.remove('hidden');
       }

       function addKey(k){
           const input = document.getElementById('birthdayInput');
           if (input.value.length < 10) {
               input.value += k;
           }
       }
       
       function clearInput(){
           document.getElementById('birthdayInput').value = '';
       }
       
       function checkBirthday(){
           const val = document.getElementById('birthdayInput').value;
           if(val === '20/01/2003'){
               document.getElementById('birthdayMsg').innerHTML = '✅ Correct!';
               setTimeout(() => nextPage(3), 800);
           } else {
               document.getElementById('birthdayMsg').innerHTML = '❌ Try again!';
           }
       }

       function checkColor(){
           const val = document.getElementById('colorInput').value.trim().toLowerCase();
           if(val === 'black'){
               document.getElementById('colorMsg').innerHTML = '✅ Correct!';
               setTimeout(() => nextPage(4), 800);
           } else {
               document.getElementById('colorMsg').innerHTML = '❌ Try again!';
           }
       }

       function checkSisters(btn){
           if(btn.innerText === '2'){
               document.getElementById('sistersMsg').innerHTML = '✅ Correct!';
               setTimeout(() => nextPage(5), 800);
           } else {
               document.getElementById('sistersMsg').innerHTML = '❌ Try again!';
           }
       }

       function checkKiss(){
           const val = document.getElementById('kissInput').value.toLowerCase();
           const valid = ['beach', 'sea', 'sand', 'la plage', 'ramla dyal lab7ar', 'lab7ar', 'ocean', 'shore'];
           if(valid.some(v => val.includes(v))){
               document.getElementById('kissMsg').innerHTML = '✅ Correct!';
               setTimeout(() => nextPage(6), 800);
           } else {
               document.getElementById('kissMsg').innerHTML = '❌ Try again!';
           }
       }

       function selectEmoji(btn){
           const emoji = btn.innerText;
           
           if(selectedEmojis.includes(emoji)){
               selectedEmojis = selectedEmojis.filter(e => e !== emoji);
               btn.classList.remove('selected');
           } else if(selectedEmojis.length < 2){
               selectedEmojis.push(emoji);
               btn.classList.add('selected');
           } else {
               // If already 2 selected, remove the first one and add new
               const firstBtn = document.querySelector('.emoji-btn.selected');
               if (firstBtn) {
                   const firstEmoji = firstBtn.innerText;
                   selectedEmojis = selectedEmojis.filter(e => e !== firstEmoji);
                   firstBtn.classList.remove('selected');
               }
               selectedEmojis.push(emoji);
               btn.classList.add('selected');
           }
           
           if(selectedEmojis.length === 2){
               if(selectedEmojis.includes('🍑') && selectedEmojis.includes('👁️')){
                   document.getElementById('emojiMsg').innerHTML = '✅ Correct!';
                   setTimeout(() => nextPage(7), 800);
               } else {
                   document.getElementById('emojiMsg').innerHTML = '❌ Not quite...';
               }
           } else {
               document.getElementById('emojiMsg').innerHTML = 'Pick exactly 2 emojis';
           }
       }

       function finalAnswer(isYes){
           const victoryScreen = document.getElementById('victoryScreen');
           const victoryMessage = document.getElementById('victoryMessage');
           
           if(isYes){
               victoryMessage.innerHTML = "You've stolen my heart… 💖✨<br><small>Happy Valentine's Day!</small>";
           } else {
               victoryMessage.innerHTML = "Siri, tqawdi 😏🔥<br><small>But I still love you!</small>";
           }
           
           // Fade out current page
           document.querySelectorAll('.page').forEach(p => {
               p.style.opacity = '0';
               p.style.transform = 'scale(0.9)';
           });
           
           setTimeout(() => {
               victoryScreen.classList.add('active');
               startVictoryHearts();
           }, 600);
       }

       function startVictoryHearts() {
           const container = document.getElementById('victory-hearts');
           const colors = ['#ffb7c5', '#ff9eb5', '#ffd1dc', '#ffc0cb', '#ff69b4'];
           const heartSymbols = ['💗', '💖', '💕', '💓', '💝'];
           
           // Create hearts continuously
           setInterval(() => {
               const heart = document.createElement('div');
               heart.className = 'floating-heart';
               heart.textContent = heartSymbols[Math.floor(Math.random() * heartSymbols.length)];
               heart.style.left = Math.random() * 100 + '%';
               heart.style.color = colors[Math.floor(Math.random() * colors.length)];
               heart.style.fontSize = (Math.random() * 30 + 20) + 'px';
               heart.style.animationDuration = (Math.random() * 3 + 4) + 's';
               
               container.appendChild(heart);
               
               // Remove after animation
               setTimeout(() => {
                   heart.remove();
               }, 7000);
           }, 300);
       }
   </script>
</body>
</html>
