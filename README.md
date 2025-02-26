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
            background: linear-gradient(to top right, #ffe6f0, #ccf5ff);
            overflow: hidden;
            font-family: 'Comic Sans MS', cursive, sans-serif;
        }
        #message {
            display: none;
            margin-top: 20px;
            font-size: 3em;
            color: #ff4081;
            text-shadow: 3px 3px 5px #000;
            background-color: rgba(255, 255, 255, 0.7);
            padding: 20px;
            border-radius: 15px;
        }
        #candles {
            margin-top: 50px;
            display: flex;
            justify-content: center;
            gap: 20px;
            flex-wrap: nowrap;
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
            to {opacity: 0.5;}
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
            width: 60px;
            height: 80px;
            background: red;
            border-radius: 50%;
            animation: floatUp 4s ease-in-out infinite;
            opacity: 0.7;
            transform: translateX(0);
            animation: floatUp 6s ease-in-out infinite, sway 2s ease-in-out infinite;
        }
        @keyframes floatUp {
            0% { transform: translateY(0); }
            100% { transform: translateY(-110vh); }
        }
        @keyframes sway {
            0% { transform: translateX(0); }
            50% { transform: translateX(20px); }
            100% { transform: translateX(0); }
        }
    </style>
</head>
<body>
    <div id="candles"></div>
    <div id="balloons"></div>
    <h1 id="message">Wishing you a day as magical as you are, Sansrita!<br>May your dreams float higher than these balloons<br>and your happiness shine brighter than these candles! 🎊</h1>

    <audio id="song" autoplay>
        <source src="https://drive.google.com/uc?id=1cft6GsrpnyIqnryTmmdbHta7AHG93NnI" type="audio/mpeg">
        Your browser does not support the audio element.
    </audio>

    <script>
        const candlesContainer = document.getElementById('candles');
        const balloonsContainer = document.getElementById('balloons');
        const message = document.getElementById('message');

        // Generate candles
        for (let i = 0; i < 10; i++) {
            const candle = document.createElement('div');
            candle.className = 'candle';
            candlesContainer.appendChild(candle);
        }

        // Generate interactive balloons
        function createBalloons() {
            for (let i = 0; i < 20; i++) {
                const balloon = document.createElement('div');
                balloon.className = 'balloon';
                balloon.style.left = `${Math.random() * 100}vw`;
                balloon.style.background = `hsl(${Math.random() * 360}, 70%, 60%)`;
                balloonsContainer.appendChild(balloon);

                setTimeout(() => {
                    balloon.remove();
                }, 6000);
            }
        }
        setInterval(createBalloons, 1500);

        // Request microphone access
        navigator.mediaDevices.getUserMedia({ audio: true }).then(stream => {
            const audioContext = new AudioContext();
            const source = audioContext.createMediaStreamSource(stream);
            const analyser = audioContext.createAnalyser();
            source.connect(analyser);
            analyser.fftSize = 256;
            const bufferLength = analyser.frequencyBinCount;
            const dataArray = new Uint8Array(bufferLength);

            let blownOut = false;

            function detectBlow() {
                analyser.getByteFrequencyData(dataArray);
                let sum = 0;
                for (let i = 0; i < bufferLength; i++) {
                    sum += dataArray[i];
                }
                const average = sum / bufferLength;

                if (average > 60 && !blownOut) {
                    blownOut = true;
                    // Simulate candle blowout
                    document.querySelectorAll('.candle').forEach(candle => {
                        candle.style.background = '#555';
                        candle.style.boxShadow = 'none';
                        candle.style.transition = 'background 1s';
                    });
                    createBalloons();
                    setTimeout(() => {
                        message.style.display = 'block';
                    }, 2000);
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
