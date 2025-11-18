
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>シンプルなタイマーサイト</title>
    <style>
        /* CSSでデザインを設定 */
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            background-color: #f4f4f9;
            margin: 0;
        }

        #timer-container {
            background-color: #ffffff;
            padding: 40px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            text-align: center;
        }

        #display {
            font-size: 4em;
            font-weight: bold;
            color: #333;
            margin-bottom: 20px;
        }

        .input-group {
            margin-bottom: 15px;
        }

        input[type="number"] {
            padding: 8px;
            width: 80px;
            text-align: center;
            border: 1px solid #ccc;
            border-radius: 5px;
            margin: 0 5px;
        }

        button {
            padding: 10px 20px;
            margin: 5px;
            font-size: 1em;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            color: white;
            transition: background-color 0.3s;
        }

        #start-btn { background-color: #4CAF50; } /* 緑 */
        #start-btn:hover { background-color: #45a049; }

        #stop-btn { background-color: #f44336; } /* 赤 */
        #stop-btn:hover { background-color: #da190b; }

        #reset-btn { background-color: #2196F3; } /* 青 */
        #reset-btn:hover { background-color: #0b7dda; }

    </style>
</head>
<body>

    <div id="timer-container">
        <h2>カウントダウンタイマー</h2>
        
        <div class="input-group">
            <input type="number" id="minutes-input" value="5" min="0"> 分
            <input type="number" id="seconds-input" value="0" min="0" max="59"> 秒
        </div>

        <div id="display">05:00</div>

        <button id="start-btn">スタート</button>
        <button id="stop-btn" disabled>ストップ</button>
        <button id="reset-btn">リセット</button>
    </div>

    <script>
        // JavaScriptでタイマーの動作を設定
        const display = document.getElementById('display');
        const minutesInput = document.getElementById('minutes-input');
        const secondsInput = document.getElementById('seconds-input');
        const startBtn = document.getElementById('start-btn');
        const stopBtn = document.getElementById('stop-btn');
        const resetBtn = document.getElementById('reset-btn');

        let totalSeconds = 0;
        let timerInterval = null;
        let isRunning = false;

        // 時間を MM:SS 形式に整形する関数
        function formatTime(seconds) {
            const min = String(Math.floor(seconds / 60)).padStart(2, '0');
            const sec = String(seconds % 60).padStart(2, '0');
            return `${min}:${sec}`;
        }

        // タイマー表示を更新する関数
        function updateDisplay() {
            display.textContent = formatTime(totalSeconds);
        }

        // カウントダウン処理
        function countdown() {
            if (totalSeconds <= 0) {
                stopTimer();
                display.textContent = "時間終了！";
                // タイマー終了時の処理（アラートなど）
                alert("タイマーが終了しました！");
                return;
            }
            totalSeconds--;
            updateDisplay();
        }

        // タイマーを開始する関数
        function startTimer() {
            if (isRunning) return;

            // 入力値を取得し、秒単位の合計を計算
            const minutes = parseInt(minutesInput.value) || 0;
            const seconds = parseInt(secondsInput.value) || 0;

            totalSeconds = minutes * 60 + seconds;

            if (totalSeconds <= 0) {
                alert("0より大きい時間を設定してください。");
                return;
            }

            isRunning = true;
            startBtn.disabled = true;
            stopBtn.disabled = false;
            minutesInput.disabled = true;
            secondsInput.disabled = true;
            
            updateDisplay();
            timerInterval = setInterval(countdown, 1000); // 1秒ごとに countdown 関数を実行
        }

        // タイマーを停止する関数
        function stopTimer() {
            isRunning = false;
            startBtn.disabled = false;
            stopBtn.disabled = true;
            clearInterval(timerInterval);
        }

        // タイマーをリセットする関数
        function resetTimer() {
            stopTimer();
            // 入力値をデフォルト値に戻す
            minutesInput.value = 5;
            secondsInput.value = 0;
            // 表示を初期値に戻す
            totalSeconds = 5 * 60;
            updateDisplay();
            minutesInput.disabled = false;
            secondsInput.disabled = false;
        }

        // イベントリスナーを設定
        startBtn.addEventListener('click', startTimer);
        stopBtn.addEventListener('click', stopTimer);
        resetBtn.addEventListener('click', resetTimer);

        // ページロード時に初期表示を設定
        resetTimer(); 

    </script>
</body>
</html>
