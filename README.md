<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NEO DJ SOUND</title>
    <style>
        :root {
            --neon-blue: #00f7ff;
            --neon-pink: #ff00ff;
            --dark-bg: #0a0a1a;
        }
        
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow: hidden; /* Блокируем прокрутку */
            background: var(--dark-bg);
            font-family: 'Rajdhani', sans-serif;
            cursor: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="%23ff00ff" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/></svg>'), auto;
        }

        #particles-js {
            position: absolute;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        .container {
            position: relative;
            z-index: 2;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
        }

        h1 {
            color: var(--neon-blue);
            text-shadow: 0 0 10px var(--neon-blue);
            font-size: 3rem;
            margin-bottom: 2rem;
            letter-spacing: 5px;
            text-transform: uppercase;
            animation: glow 2s infinite alternate;
        }

        @keyframes glow {
            from { text-shadow: 0 0 10px var(--neon-blue); }
            to { text-shadow: 0 0 20px var(--neon-pink), 0 0 30px var(--neon-blue); }
        }

        .neo-btn {
            position: relative;
            padding: 20px 60px;
            background: transparent;
            color: var(--neon-blue);
            border: 2px solid var(--neon-blue);
            font-size: 1.5rem;
            text-transform: uppercase;
            letter-spacing: 4px;
            cursor: pointer;
            border-radius: 50px;
            overflow: hidden;
            box-shadow: 0 0 20px rgba(0, 247, 255, 0.3);
            transition: all 0.3s;
        }

        .neo-btn:hover {
            color: var(--neon-pink);
            border-color: var(--neon-pink);
            box-shadow: 0 0 30px rgba(255, 0, 255, 0.5);
        }

        .neo-btn:active {
            transform: translateY(5px);
            box-shadow: 0 5px 15px rgba(255, 0, 255, 0.7);
        }

        /* Вертикальные волны (15 полос) */
        .dj-waves {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: flex-end;
            gap: 2px;
            z-index: 0;
            padding-bottom: 50px;
        }

        .wave-bar {
            width: 40px;
            height: 10px;
            background: rgba(0, 247, 255, 0.2);
            border-radius: 5px 5px 0 0;
            transition: height 0.1s;
            box-shadow: 0 0 10px rgba(0, 247, 255, 0.3);
        }

        .click-effect {
            position: absolute;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: radial-gradient(circle, var(--neon-pink), transparent);
            pointer-events: none;
            transform: translate(-50%, -50%);
            animation: ripple 1s linear forwards;
            filter: blur(1px);
        }

        @keyframes ripple {
            0% { transform: scale(0); opacity: 1; }
            100% { transform: scale(20); opacity: 0; }
        }

        .powered {
            position: absolute;
            bottom: 20px;
            color: rgba(255,255,255,0.3);
            font-size: 0.8rem;
            z-index: 3;
        }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@500;700&display=swap" rel="stylesheet">
</head>
<body>
    <div id="particles-js"></div>
    
    <!-- Вертикальные волны (15 полос) -->
    <div class="dj-waves" id="djWaves"></div>
    
    <div class="container">
        <h1>NEO DJ</h1>
        <button class="neo-btn" id="playBtn">START MIX</button>
    </div>

    <div class="powered">NEO-DJ SYSTEM v3.0</div>

    <audio id="audio" loop>
        <source src="a.mp3" type="audio/mpeg">
    </audio>

    <script src="https://cdn.jsdelivr.net/particles.js/2.0.0/particles.min.js"></script>
    <script>
        // Создаем 15 вертикальных волн
        const djWaves = document.getElementById('djWaves');
        for (let i = 0; i < 15; i++) {
            const wave = document.createElement('div');
            wave.className = 'wave-bar';
            djWaves.appendChild(wave);
        }
        const waveBars = document.querySelectorAll('.wave-bar');

        // Инициализация частиц
        particlesJS("particles-js", {
            particles: {
                number: { value: 80, density: { enable: true, value_area: 800 } },
                color: { value: ["#00f7ff", "#ff00ff"] },
                shape: { type: "circle" },
                opacity: { value: 0.7, random: true },
                size: { value: 3, random: true },
                line_linked: { 
                    enable: true, 
                    distance: 150, 
                    color: "#00f7ff", 
                    opacity: 0.4, 
                    width: 1 
                },
                move: { 
                    enable: true, 
                    speed: 2, 
                    direction: "none", 
                    random: true, 
                    straight: false, 
                    out_mode: "out" 
                }
            },
            interactivity: {
                detect_on: "canvas",
                events: {
                    onhover: { enable: true, mode: "repulse" },
                    onclick: { enable: true, mode: "push" }
                }
            }
        });

        // Клики по фону
        document.addEventListener('click', function(e) {
            if (e.target.id !== 'playBtn') {
                createClickEffect(e.clientX, e.clientY);
            }
        });

        function createClickEffect(x, y) {
            const effect = document.createElement('div');
            effect.className = 'click-effect';
            effect.style.left = x + 'px';
            effect.style.top = y + 'px';
            document.body.appendChild(effect);
            setTimeout(() => effect.remove(), 1000);
        }

        // Аудио система
        const playBtn = document.getElementById('playBtn');
        const audio = document.getElementById('audio');
        let audioContext, analyser, dataArray;
        
        playBtn.addEventListener('click', function() {
            if (audio.paused) {
                startAudio();
                playBtn.textContent = 'STOP MIX';
            } else {
                stopAudio();
                playBtn.textContent = 'START MIX';
            }
        });

        function startAudio() {
            audio.play();
            
            // Инициализация AudioContext
            audioContext = new (window.AudioContext || window.webkitAudioContext)();
            analyser = audioContext.createAnalyser();
            const source = audioContext.createMediaElementSource(audio);
            source.connect(analyser);
            analyser.connect(audioContext.destination);
            
            analyser.fftSize = 64;
            dataArray = new Uint8Array(analyser.frequencyBinCount);
            
            animateWaves();
        }

        function stopAudio() {
            audio.pause();
            audio.currentTime = 0;
            
            // Сброс волн
            waveBars.forEach(bar => {
                bar.style.height = '10px';
            });
        }

        function animateWaves() {
            if (audio.paused) return;
            
            requestAnimationFrame(animateWaves);
            analyser.getByteFrequencyData(dataArray);
            
            // Обновляем 15 волн
            for (let i = 0; i < waveBars.length; i++) {
                const value = dataArray[i % dataArray.length] / 2;
                waveBars[i].style.height = `${10 + value}px`;
                waveBars[i].style.background = i % 2 === 0 
                    ? 'rgba(0, 247, 255, 0.5)' 
                    : 'rgba(255, 0, 255, 0.5)';
            }
        }

        // Первый клик по странице активирует аудио
        document.addEventListener('click', function initAudio() {
            audioContext = new (window.AudioContext || window.webkitAudioContext)();
            document.removeEventListener('click', initAudio);
        }, { once: true });
    </script>
</body>
</html>
