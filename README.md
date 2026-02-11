<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>A Special Message... 💙</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@400;700;900&display=swap');

        body {
            background-color: #dbeafe;
            background-image: radial-gradient(#60a5fa 1.5px, transparent 1.5px);
            background-size: 30px 30px;
            font-family: 'Outfit', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            overflow: hidden;
            touch-action: manipulation;
        }

        .bg-float {
            position: fixed;
            font-size: 1.5rem;
            opacity: 0.3;
            pointer-events: none;
            z-index: 1;
            animation: float-up 6s linear forwards;
        }

        @keyframes float-up {
            from { transform: translateY(110vh) rotate(0deg); }
            to { transform: translateY(-15vh) rotate(360deg); }
        }

        .card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border: 4px solid #1e40af;
            box-shadow: 10px 10px 0px #60a5fa;
            padding: 30px;
            border-radius: 28px;
            text-align: center;
            max-width: 400px;
            width: 85%;
            position: absolute;
            z-index: 10;
        }

        h1 { color: #1e3a8a; font-size: 1.5rem; font-weight: 900; margin: 0 0 15px 0; }
        p { font-size: 1.1rem; line-height: 1.4; color: #1e40af; font-weight: 500; }

        .gif-box img {
            width: 100%;
            max-width: 180px;
            border: 3px solid #1e40af;
            border-radius: 20px;
            margin-bottom: 15px;
        }

        .btn-container { display: flex; justify-content: center; align-items: center; gap: 12px; margin-top: 20px; }

        button {
            padding: 14px 28px;
            font-size: 1.1rem;
            font-weight: 700;
            border: 3px solid #1e3a8a;
            border-radius: 18px;
            cursor: pointer;
            box-shadow: 4px 4px 0px #1e3a8a;
            background-color: #3b82f6;
            color: white;
            transition: transform 0.1s;
        }

        button:active { transform: translate(2px, 2px); box-shadow: 2px 2px 0px #1e3a8a; }
        #noBtn { background-color: #ef4444; border-color: #991b1b; box-shadow: 4px 4px 0px #991b1b; }
        .hidden { display: none !important; }

        .falling-tulip {
            position: absolute;
            font-size: 50px;
            cursor: pointer;
            z-index: 20;
            animation: fall linear forwards;
        }

        @keyframes fall {
            to { transform: translateY(110vh) rotate(360deg); }
        }

        .score-board { font-size: 2rem; font-weight: 900; color: #1e3a8a; }
    </style>
</head>
<body onload="initDecorations()">

    <audio id="bgMusic" loop preload="auto">
        <source src="https://files.catbox.moe/r5s340.mp3" type="audio/mpeg">
    </audio>
    <audio id="popSound" preload="auto">
        <source src="https://assets.mixkit.co/active_storage/sfx/2571/2571-preview.mp3" type="audio/mpeg">
    </audio>

    <div class="card" id="slide1">
        <div class="gif-box"><img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExOHpueGZ6ZzRyc3R6ZzRyc3R6ZzRyc3R6ZzRyc3R6ZzRyc3R6JmVwPXYxX2ludGVybmFsX2dpZl9ieV9pZContext&ep=v1_gifs_search&rid=giphy.gif&ct=g"></div>
        <h1>A Special Surprise... 💙</h1>
        <p>Pass the tulip challenge to see your message!</p>
        <button onclick="startJourney()">I'm Ready! ✨</button>
    </div>

    <div class="card hidden" id="slide2">
        <h1>Wait lang...</h1>
        <p>Anong "BANG" ang masarap?? <br><b>Ediii Pabembanggg!! >_< 💙</b></p>
        <button onclick="startMiniGame()">Play Game 🎮</button>
    </div>

    <div class="card hidden" id="gameUI">
        <h1>Catch 5 Tulips! 🌷</h1>
        <div class="score-board" id="score">0</div>
        <p>Tap the falling tulips!</p>
    </div>

    <div class="card hidden" id="slide3">
        <div class="gif-box"><img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExMmdueGZ6ZzRyc3R6ZzRyc3R6ZzRyc3R6ZzRyc3R6ZzRyc3R6JmVwPXYxX2ludGVybmFsX2dpZl9ieV9pZContext&ep=v1_gifs_search&rid=giphy.gif&ct=g"></div>
        <h1 id="questionText">Will you be my Valentine? 🌹</h1>
        <div class="btn-container">
            <button id="yesBtn" onclick="celebrate()">YES! 🌈</button>
            <button id="noBtn">No? 🥺</button>
        </div>
    </div>

    <div class="card hidden" id="slide4">
        <h1>YEHEYYY! 🎉</h1>
        <p>ngani lng sa haa puhonay nasab ang mga handmade hehe digital nasab ta!</p>
        <p>Bembang kasakin sa 14! hehe 💙</p>
        <div style="font-size: 80px; margin: 10px;">💐</div>
        <p>plawerss ohh labyoyy!</p>
        <button onclick="location.reload()">Replay? 🔄</button>
    </div>

    <script>
        let score = 0;
        let gameActive = false;
        let yesSize = 1.1;
        const messages = ["Tarungg??", "Pahapak ka!", "Ay halaa syaa? 🥺", "Bahalaka...", "dimoko lab... 😭"];
        let msgIndex = 0;

        function initDecorations() {
            setInterval(createFloatyThing, 700);
        }

        function createFloatyThing() {
            const el = document.createElement('div');
            el.className = 'bg-float';
            el.innerHTML = Math.random() > 0.5 ? '🌷' : '💙';
            el.style.left = Math.random() * 100 + 'vw';
            const duration = Math.random() * 4 + 4;
            el.style.animationDuration = duration + 's';
            el.style.fontSize = (Math.random() * 1 + 1) + 'rem';
            document.body.appendChild(el);
            setTimeout(() => el.remove(), duration * 1000);
        }

        function startJourney() {
            const music = document.getElementById('bgMusic');
            // Force play audio
            music.play().catch(error => {
                console.log("Audio play failed. Most browsers need a click first!", error);
            });
            nextSlide(1);
        }

        function nextSlide(n) {
            document.getElementById(`slide${n}`).classList.add('hidden');
            const next = n === 2 ? 'gameUI' : `slide${n+1}`;
            document.getElementById(next).classList.remove('hidden');
        }

        function startMiniGame() {
            document.getElementById('slide2').classList.add('hidden');
            document.getElementById('gameUI').classList.remove('hidden');
            gameActive = true;
            spawnTulip();
        }

        function spawnTulip() {
            if (!gameActive) return;
            const t = document.createElement('div');
            t.className = 'falling-tulip';
            t.innerHTML = '🌷';
            t.style.left = Math.random() * 80 + 10 + 'vw';
            t.style.top = '-60px';
            t.style.animationDuration = '2.5s';
            
            t.onclick = () => {
                score++;
                document.getElementById('score').innerText = score;
                const pop = document.getElementById('popSound');
                pop.currentTime = 0;
                pop.play();
                t.remove();
                if (score >= 5) {
                    gameActive = false;
                    document.getElementById('gameUI').classList.add('hidden');
                    document.getElementById('slide3').classList.remove('hidden');
                }
            };
            document.body.appendChild(t);
            setTimeout(() => { if(t.parentNode) t.remove(); }, 2500);
            setTimeout(spawnTulip, 800);
        }

        const noBtn = document.getElementById('noBtn');
        const yesBtn = document.getElementById('yesBtn');

        function handleNo() {
            document.getElementById('popSound').play();
            document.getElementById('questionText').innerText = messages[msgIndex];
            msgIndex = (msgIndex + 1) % messages.length;
            yesSize += 0.4;
            yesBtn.style.fontSize = `${yesSize}rem`;
            yesBtn.style.padding = `${yesSize * 15}px ${yesSize * 25}px`;
            noBtn.style.position = 'fixed';
            noBtn.style.left = Math.random() * 70 + 10 + 'vw';
            noBtn.style.top = Math.random() * 70 + 10 + 'vh';
        }

        noBtn.addEventListener('click', handleNo);

        function celebrate() {
            document.getElementById('slide3').classList.add('hidden');
            document.getElementById('slide4').classList.remove('hidden');
            for(let i=0; i<50; i++) {
                const s = document.createElement('div');
                s.style.position = 'fixed';
                s.style.left = Math.random()*100+'vw';
                s.style.top = '100vh';
                s.innerHTML = Math.random() > 0.5 ? '💙' : '🌷';
                s.style.fontSize = '30px';
                s.style.zIndex = '100';
                s.animate([{transform:'translateY(0)'},{transform:'translateY(-110vh) rotate(360deg)'}], 2000);
                document.body.appendChild(s);
            }
        }
    </script>
</body>
</html>
