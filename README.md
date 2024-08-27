<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Stopwatch</title>
    <style>
        body {
            font-family: 'Courier New', Courier, monospace;
            background-color: #e0e0e0;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }

        .stopwatch {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            text-align: center;
            width: 300px;
            position: relative;
        }

        #time {
            font-size: 60px;
            margin-bottom: 20px;
            letter-spacing: 2px;
            animation: pulse 1s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .buttons {
            margin-top: 20px;
        }

        .buttons button {
            font-size: 18px;
            padding: 10px 15px;
            margin: 5px;
            border: 2px solid #333;
            background-color: transparent;
            color: #333;
            cursor: pointer;
            transition: background-color 0.3s, color 0.3s;
            border-radius: 5px;
        }

        .buttons button:hover {
            background-color: #333;
            color: white;
        }

        .lap-times {
            margin-top: 20px;
            max-height: 150px;
            overflow-y: auto;
            text-align: left;
            border-top: 1px solid #ddd;
            padding-top: 10px;
        }

        .lap-time {
            font-size: 18px;
            padding: 5px;
            border-bottom: 1px solid #ddd;
        }
    </style>
</head>
<body>

    <div class="stopwatch">
        <div id="time">00:00:00</div>
        <div class="buttons">
            <button id="start">Start</button>
            <button id="pause">Pause</button>
            <button id="reset">Reset</button>
            <button id="lap">Lap</button>
        </div>
        <div class="lap-times" id="lapTimes"></div>
    </div>

    <script>
        let startTime, elapsedTime = 0, timerInterval, lapCounter = 0;

        function timeToString(time) {
            let diffInHrs = time / 3600000;
            let hh = Math.floor(diffInHrs);

            let diffInMin = (diffInHrs - hh) * 60;
            let mm = Math.floor(diffInMin);

            let diffInSec = (diffInMin - mm) * 60;
            let ss = Math.floor(diffInSec);

            let formattedHH = hh.toString().padStart(2, "0");
            let formattedMM = mm.toString().padStart(2, "0");
            let formattedSS = ss.toString().padStart(2, "0");

            return `${formattedHH}:${formattedMM}:${formattedSS}`;
        }

        function print(txt) {
            document.getElementById("time").innerHTML = txt;
        }

        function start() {
            startTime = Date.now() - elapsedTime;
            timerInterval = setInterval(function printTime() {
                elapsedTime = Date.now() - startTime;
                print(timeToString(elapsedTime));
            }, 1000);
            showButton("PAUSE");
        }

        function pause() {
            clearInterval(timerInterval);
            showButton("PLAY");
        }

        function reset() {
            clearInterval(timerInterval);
            print("00:00:00");
            elapsedTime = 0;
            document.getElementById("lapTimes").innerHTML = '';
            lapCounter = 0;
            showButton("PLAY");
        }

        function lap() {
            lapCounter++;
            const lapTime = document.createElement("div");
            lapTime.className = "lap-time";
            lapTime.textContent = `Lap ${lapCounter}: ${timeToString(elapsedTime)}`;
            document.getElementById("lapTimes").appendChild(lapTime);
        }

        function showButton(buttonKey) {
            const startButton = document.getElementById("start");
            const pauseButton = document.getElementById("pause");

            if (buttonKey === "PLAY") {
                startButton.style.display = "inline-block";
                pauseButton.style.display = "none";
            } else {
                startButton.style.display = "none";
                pauseButton.style.display = "inline-block";
            }
        }

        document.getElementById("start").addEventListener("click", start);
        document.getElementById("pause").addEventListener("click", pause);
        document.getElementById("reset").addEventListener("click", reset);
        document.getElementById("lap").addEventListener("click", lap);

        showButton("PLAY");
    </script>

</body>
</html>
# prodigy-stop-watch-
