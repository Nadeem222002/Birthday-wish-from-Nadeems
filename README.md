<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy Birthday Sansrita!</title>
    <style>
        body {
            text-align: center;
            background: url('your-image-url-here') no-repeat center center fixed;
            background-size: cover;
            color: #fff;
            font-family: 'Comic Sans MS', cursive;
            margin: 0;
            padding: 0;
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }
        h1 {
            margin-top: 50px;
            color: #fff;
            text-shadow: 2px 2px #000;
            font-size: 3em;
            position: relative;
            z-index: 2;
        }
        .balloon {
            position: absolute;
            bottom: -100px;
            width: 50px;
            height: 70px;
            border-radius: 50% 50% 60% 60%;
            animation: floatBalloon 5s linear infinite;
        }
        @keyframes floatBalloon {
            0% { transform: translateY(0); opacity: 1; }
            100% { transform: translateY(-110vh); opacity: 0; }
        }
        .candle {
            position: absolute;
            bottom: 30px;
            width: 20px;
            height: 60px;
            background-color: #fff;
            border: 2px solid #000;
            border-radius: 5px;
            z-index: 1;
        }
        .flame {
            position: absolute;
            top: -20px;
            left: 5px;
            width: 10px;
            height: 20px;
            background: radial-gradient(circle, #ffcc00, #ff6600);
            border-radius: 50%;
            animation: flicker 0.5s infinite;
        }
        @keyframes flicker {
            0%, 100% { transform: scaleY(1); opacity: 1; }
            50% { transform: scaleY(1.3); opacity: 0.7; }
        }
        #message {
            position: absolute;
            top: 35%;
            width: 100%;
            font-size: 2em;
            color: #fff;
            text-shadow: 2px 2px #000;
            display: none;
            z-index: 3;
        }
    </style>
</head>
<body>
    <h1>🎂 Happy Birthday, Sansrita! 🎉</h1>
    <div class="candle">
        <div class="flame"></div>
    </div>
    <p id="blowMsg">Take a deep breath and blow out the candle to unlock your birthday magic! 🎤</p>

    <audio id="birthdaySong" src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-6.mp3" preload="auto"></audio>

    <script>
        function createBalloons() {
            const colors = ['#FF4D4D', '#FF66B2', '#66CCFF', '#99FF99', '#FFD700'];
            setInterval(() => {
                const balloon = document.createElement('div');
                balloon.classList.add('balloon');
                balloon.style.left = Math.random() * 100 + 'vw';
                balloon.style.background = `radial-gradient(circle at 30% 30%, ${colors[Math.floor(Math.random() * colors.length)]}, #000000)`;
                balloon.style.animationDuration = (Math.random() * 3 + 5) + 's';
                document.body.appendChild(balloon);
                setTimeout(() => balloon.remove(), 10000);
            }, 300);
        }

        function listenForBlow() {
            navigator.mediaDevices.getUserMedia({ audio: true }).then(function (stream) {
                const audioContext = new (window.AudioContext || window.webkitAudioContext)();
                const source = audioContext.createMediaStreamSource(stream);
                const analyser = audioContext.createAnalyser();
                source.connect(analyser);
                analyser.fftSize = 256;

                const bufferLength = analyser.frequencyBinCount;
                const dataArray = new Uint8Array(bufferLength);

                function detectBlow() {
                    analyser.getByteFrequencyData(dataArray);
                    let sum = dataArray.reduce((a, b) => a + b, 0);

                    if (sum > 5000) {
                        blowOutCandle();
                    } else {
                        requestAnimationFrame(detectBlow);
                    }
                }
                detectBlow();
            }).catch(function (err) {
                alert('Microphone access is required to blow out the candle!');
            });
        }

        function blowOutCandle() {
            document.querySelector('.flame').style.display = 'none';
            document.getElementById('blowMsg').innerText = '✨ Poof! The candle is out... and your magical birthday surprise begins! 🎉';
            document.getElementById('birthdaySong').play();

            setTimeout(() => {
                alert('🎈 Happy Birthday, Sansrita! 🎂 Wishing you a day as magical as you are! May your dreams float higher than these balloons and your happiness shine brighter than these candles! 🎊');
            }, 2000);
        }

        createBalloons();
        listenForBlow();
    </script>
</body>
</html>
