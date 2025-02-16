<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy Birthday Sansrita!</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            text-align: center;
            background: linear-gradient(to top right, #ffccff, #ccffff);
            overflow: hidden;
        }
        h1 {
            margin-top: 20px;
            font-size: 2em;
            color: #ff4081;
            text-shadow: 2px 2px #fff;
        }
        #candles {
            margin-top: 50px;
            display: flex;
            justify-content: center;
            gap: 20px;
            flex-wrap: wrap;
        }
        .candle {
            width: 20px;
            height: 100px;
            background: orange;
            border-radius: 5px;
            position: relative;
            cursor: pointer;
        }
        .candle::before {
            content: '';
            position: absolute;
            top: -20px;
            left: 50%;
            transform: translateX(-50%);
            width: 10px;
            height: 20px;
            background: yellow;
            border-radius: 50%;
            box-shadow: 0 0 15px 5px yellow;
            animation: flicker 1s infinite alternate;
        }
        @keyframes flicker {
            from {opacity: 1;}
            to {opacity: 0.6;}
        }
        #balloons {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            pointer-events: none;
        }
        .balloon {
            position: absolute;
            bottom: -100px;
            width: 50px;
            height: 70px;
            background: red;
            border-radius: 50%;
            animation: floatUp 5s linear infinite;
        }
        @keyframes floatUp {
            0% { transform: translateY(0); }
            100% { transform: translateY(-110vh); }
        }
    </style>
</head>
<body>
    <h1>Wishing you a day as magical as you are, Sansrita!<br>May your dreams float higher than these balloons<br>and your happiness shine brighter than these candles! 🎊</h1>
    <div id="candles"></div>
    <div id="balloons"></div>

    <audio id="song" src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" autoplay></audio>

    <script>
        const candlesContainer = document.getElementById('candles');
        const balloonsContainer = document.getElementById('balloons');
        const song = document.getElementById('song');

        // Generate candles
        for (let i = 0; i < 20; i++) {
            const candle = document.createElement('div');
            candle.className = 'candle';
            candlesContainer.appendChild(candle);
        }

        // Generate balloons
        function createBalloons() {
            for (let i = 0; i < 30; i++) {
                const balloon = document.createElement('div');
                balloon.className = 'balloon';
                balloon.style.left = `${Math.random() * 100}vw`;
                balloon.style.background = `hsl(${Math.random() * 360}, 70%, 60%)`;
                balloonsContainer.appendChild(balloon);

                setTimeout(() => {
                    balloon.remove();
                }, 5000);
            }
        }
        setInterval(createBalloons, 2000);

        // Request microphone access
        navigator.mediaDevices.getUserMedia({ audio: true }).then(stream => {
            const audioContext = new AudioContext();
            const source = audioContext.createMediaStreamSource(stream);
            const analyser = audioContext.createAnalyser();
            source.connect(analyser);
            analyser.fftSize = 256;
            const bufferLength = analyser.frequencyBinCount;
            const dataArray = new Uint8Array(bufferLength);

            function detectBlow() {
                analyser.getByteFrequencyData(dataArray);
                let sum = 0;
                for (let i = 0; i < bufferLength; i++) {
                    sum += dataArray[i];
                }
                const average = sum / bufferLength;

                if (average > 50) {
                    // Simulate candle blowout
                    document.querySelectorAll('.candle').forEach(candle => {
                        candle.style.background = '#555';
                        candle.style.boxShadow = 'none';
                        candle.style.transition = 'background 1s';
                        candle.style.setProperty('--flame', 'none');
                    });
                    createBalloons();
                }
                requestAnimationFrame(detectBlow);
            }
            detectBlow();
        }).catch(error => {
            alert('Microphone permission is required to blow out the candles!');
        });
    </script>
</body>
</html>
