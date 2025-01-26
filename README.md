就一乐子
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>雨界</title>
    <style>
        /* 全局样式 */
        body {
            background-color: #000;
            color: #fff;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        /* 开始界面样式 */
        #start-screen {
            text-align: center;
            padding: 20px;
            background-color: rgba(0, 0, 0, 0.7);
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(255, 255, 255, 0.3);
        }

        #start-screen h1 {
            font-size: 3em;
            margin-bottom: 20px;
        }

        #start-screen p {
            font-size: 1.2em;
            margin-bottom: 30px;
        }

        #start-screen button {
            padding: 15px 30px;
            font-size: 1.2em;
            background-color: #007BFF;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        #start-screen button:hover {
            background-color: #0056b3;
        }

        /* 游戏界面样式 */
        #game-screen {
            display: none;
            padding: 20px;
            background-color: rgba(0, 0, 0, 0.7);
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(255, 255, 255, 0.3);
            width: 80%;
            max-width: 600px;
        }

        /* 下雨特效样式 */
        .rain {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
            overflow: hidden;
        }

        .drop {
            position: absolute;
            width: 1px;
            height: 10px;
            background-color: rgba(255, 255, 255, 0.8);
            animation: fall linear infinite;
            opacity: 0.7;
        }

        @keyframes fall {
            to {
                transform: translateY(100vh);
            }
        }

        /* 事件日志样式 */
        #event-log {
            border: 1px solid #555;
            padding: 10px;
            height: 250px;
            overflow-y: auto;
            margin-bottom: 20px;
            background-color: rgba(0, 0, 0, 0.4);
            border-radius: 5px;
        }

        /* 资源状态样式 */
        #resource-status {
            margin-bottom: 20px;
            display: flex;
            justify-content: space-around;
            align-items: center;
            flex-wrap: wrap;
        }

        #resource-status p {
            margin: 5px;
            font-size: 1.1em;
        }

        /* 互动按钮样式 */
        #interaction-buttons {
            margin-bottom: 20px;
            display: flex;
            justify-content: center;
            gap: 15px;
        }

        #interaction-buttons button {
            padding: 12px 25px;
            font-size: 1.1em;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        #interaction-buttons button:first-child {
            background-color: #28a745;
        }

        #interaction-buttons button:first-child:hover {
            background-color: #218838;
        }

        #interaction-buttons button:last-child {
            background-color: #dc3545;
        }

        #interaction-buttons button:last-child:hover {
            background-color: #c82333;
        }

        /* 得到东西动效样式 */
        .gain-effect {
            color: #28a745;
            animation: gain 1s ease;
        }

        /* 失去东西动效样式 */
        .loss-effect {
            color: #dc3545;
            animation: loss 1s ease;
        }

        @keyframes gain {
            0% {
                opacity: 0;
                transform: scale(0.5);
            }
            50% {
                opacity: 1;
                transform: scale(1.2);
            }
            100% {
                opacity: 1;
                transform: scale(1);
            }
        }

        @keyframes loss {
            0% {
                opacity: 0;
                transform: scale(1);
            }
            50% {
                opacity: 1;
                transform: scale(0.8);
            }
            100% {
                opacity: 1;
                transform: scale(1);
            }
        }

        /* 历史记录样式 */
        #history-log {
            border: 1px solid #555;
            padding: 10px;
            height: 150px;
            overflow-y: auto;
            margin-top: 20px;
            background-color: rgba(0, 0, 0, 0.4);
            border-radius: 5px;
        }

        /* 重来按钮样式 */
        #restart-button {
            display: none;
            padding: 12px 25px;
            font-size: 1.1em;
            background-color: #ffc107;
            color: #000;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
            margin: 20px auto 0;
        }

        #restart-button:hover {
            background-color: #e0a800;
        }

        /* 响应式设计 */
        @media (max-width: 600px) {
            #game-screen {
                width: 95%;
            }

            #resource-status {
                flex-direction: column;
                align-items: flex-start;
            }
        }
    </style>
</head>

<body>
    <!-- 开始界面 -->
    <div id="start-screen">
        <h1>雨界</h1>
        <p>你将在这里生存直至死亡或是离开。</p>
        <button onclick="startGame()">开始游戏</button>
    </div>

    <!-- 游戏界面 -->
    <div id="game-screen">
        <div class="rain" id="rain-effect"></div>
        <div id="event-log">
            <!-- 事件历史记录 -->
        </div>
        <div id="resource-status">
            <p>水: <span id="water">100</span></p>
            <p>食物: <span id="food">100</span></p>
            <p>电: <span id="electricity">100</span></p>
            <p>时间: <span id="time">1</span> 天</p>
        </div>
        <div id="interaction-buttons">
            <!-- 互动按钮 -->
        </div>
        <div id="history-log">
            <!-- 游戏历史记录 -->
        </div>
        <button id="restart-button" onclick="restartGame()">重来</button>
    </div>

    <script>
        // 资源初始值
        let water = 100;
        let food = 100;
        let electricity = 100;
        let time = 1;
        let isGameOver = false;
        let currentEvent = null;
        let isProcessing = false; // 用于防止多次点击
        let gameHistory = [];

        // 开始游戏
        function startGame() {
            document.getElementById('start-screen').style.display = 'none';
            document.getElementById('game-screen').style.display = 'block';
            createRainEffect();
            logEvent('游戏开始，祝你好运！');
            nextDay();
        }

        // 创建下雨特效
        function createRainEffect() {
            const rain = document.getElementById('rain-effect');
            for (let i = 0; i < 200; i++) {
                const drop = document.createElement('div');
                drop.classList.add('drop');
                drop.style.left = Math.random() * 100 + '%';
                drop.style.animationDuration = Math.random() * 2 + 1 + 's';
                rain.appendChild(drop);
            }
        }

        // 记录事件
        function logEvent(event) {
            const eventLog = document.getElementById('event-log');
            const p = document.createElement('p');
            p.textContent = `第 ${time} 天: ${event}`;
            eventLog.appendChild(p);
            eventLog.scrollTop = eventLog.scrollHeight;
        }

        // 记录游戏历史
        function logGameHistory(result) {
            const historyLog = document.getElementById('history-log');
            const p = document.createElement('p');
            p.textContent = `游戏结束，结果: ${result}，持续时间: ${time} 天`;
            historyLog.appendChild(p);
            historyLog.scrollTop = historyLog.scrollHeight;
            gameHistory.push({ result, time });
        }

        // 应用得到东西动效
        function applyGainEffect(spanElement) {
            spanElement.classList.add('gain-effect');
            setTimeout(() => {
                spanElement.classList.remove('gain-effect');
            }, 1000);
        }

        // 应用失去东西动效
        function applyLossEffect(spanElement) {
            spanElement.classList.add('loss-effect');
            setTimeout(() => {
                spanElement.classList.remove('loss-effect');
            }, 1000);
        }

        // 下一天
        function nextDay() {
            if (isGameOver || isProcessing) return;
            isProcessing = true;

            // 清除之前的互动按钮
            const interactionButtons = document.getElementById('interaction-buttons');
            interactionButtons.innerHTML = '';

            // 消耗资源
            water -= 10;
            food -= 10;
            electricity -= 5;
            time++;

            // 更新资源显示
            const waterSpan = document.getElementById('water');
            const foodSpan = document.getElementById('food');
            const electricitySpan = document.getElementById('electricity');
            waterSpan.textContent = water;
            foodSpan.textContent = food;
            electricitySpan.textContent = electricity;

            // 检查资源是否耗尽
            if (water <= 0 || food <= 0 || electricity <= 0) {
                logEvent('你因资源耗尽而死亡，游戏失败！');
                logGameHistory('失败');
                endGame();
                return;
            }

            // 随机事件
            const randomEvent = Math.floor(Math.random() * 4);
            switch (randomEvent) {
                case 0:
                    currentEvent = {
                        description: '发电站损坏，是否消耗 5 食物修补？',
                        yes: () => {
                            if (isProcessing) return;
                            isProcessing = true;
                            if (food >= 5) {
                                food -= 5;
                                foodSpan.textContent = food;
                                applyLossEffect(foodSpan);
                                electricity += 30;
                                electricitySpan.textContent = electricity;
                                applyGainEffect(electricitySpan);
                                logEvent('你消耗了 5 食物修补发电站，电力增加 30。');
                            } else {
                                logEvent('食物不足，无法修补发电站。');
                            }
                            setTimeout(() => {
                                isProcessing = false;
                                nextDay();
                            }, 1000);
                        },
                        no: () => {
                            if (isProcessing) return;
                            isProcessing = true;
                            logEvent('你选择不修补发电站，电力持续下降。');
                            electricity -= 20;
                            electricitySpan.textContent = electricity;
                            applyLossEffect(electricitySpan);
                            setTimeout(() => {
                                isProcessing = false;
                                nextDay();
                            }, 1000);
                        }
                    };
                    break;
                case 1:
                    currentEvent = {
                        description: '发现一个水源，但水质浑浊，是否消耗 2 电净化水？',
                        yes: () => {
                            if (isProcessing) return;
                            isProcessing = true;
                            if (electricity >= 2) {
                                electricity -= 2;
                                electricitySpan.textContent = electricity;
                                applyLossEffect(electricitySpan);
                                water += 50;
                                waterSpan.textContent = water;
                                applyGainEffect(waterSpan);
                                logEvent('你消耗了 2 电净化水，获得 50 单位水。');
                            } else {
                                logEvent('电力不足，无法净化水。');
                            }
                            setTimeout(() => {
                                isProcessing = false;
                                nextDay();
                            }, 1000);
                        },
                        no: () => {
                            if (isProcessing) return;
                            isProcessing = true;
                            logEvent('你选择不净化水，失去了获得水的机会。');
                            setTimeout(() => {
                                isProcessing = false;
                                nextDay();
                            }, 1000);
                        }
                    };
                    break;
                case 2:
                    currentEvent = {
                        description: '遇到一群野兽，是否消耗 10 水制造陷阱驱赶它们？',
                        yes: () => {
                            if (isProcessing) return;
                            isProcessing = true;
                            if (water >= 10) {
                                water -= 10;
                                waterSpan.textContent = water;
                                applyLossEffect(waterSpan);
                                logEvent('你消耗了 10 水制造陷阱，成功驱赶了野兽。');
                            } else {
                                logEvent('水不足，无法制造陷阱，野兽袭击导致食物减少 20。');
                                food -= 20;
                                foodSpan.textContent = food;
                                applyLossEffect(foodSpan);
                            }
                            setTimeout(() => {
                                isProcessing = false;
                                nextDay();
                            }, 1000);
                        },
                        no: () => {
                            if (isProcessing) return;
                            isProcessing = true;
                            logEvent('你选择不制造陷阱，野兽袭击导致食物减少 20。');
                            food -= 20;
                            foodSpan.textContent = food;
                            applyLossEffect(foodSpan);
                            setTimeout(() => {
                                isProcessing = false;
                                nextDay();
                            }, 1000);
                        }
                    };
                    break;
                case 3:
                    currentEvent = {
                        description: '发现一个废弃的房屋，是否消耗 15 食物和 5 电探索？',
                        yes: () => {
                            if (isProcessing) return;
                            isProcessing = true;
                            if (food >= 15 && electricity >= 5) {
                                food -= 15;
                                foodSpan.textContent = food;
                                applyLossEffect(foodSpan);
                                electricity -= 5;
                                electricitySpan.textContent = electricity;
                                applyLossEffect(electricitySpan);
                                const randomReward = Math.floor(Math.random() * 2);
                                if (randomReward === 0) {
                                    water += 30;
                                    waterSpan.textContent = water;
                                    applyGainEffect(waterSpan);
                                    logEvent('你消耗了 15 食物和 5 电探索房屋，获得 30 单位水。');
                                } else {
                                    logEvent('你消耗了 15 食物和 5 电探索房屋，但没有发现任何有用的东西。');
                                }
                            } else {
                                logEvent('资源不足，无法探索房屋。');
                            }
                            setTimeout(() => {
                                isProcessing = false;
                                nextDay();
                            }, 1000);
                        },
                        no: () => {
                            if (isProcessing) return;
                            isProcessing = true;
                            logEvent('你选择不探索房屋，错过了可能的奖励。');
                            setTimeout(() => {
                                isProcessing = false;
                                nextDay();
                            }, 1000);
                        }
                    };
                    break;
            }

            if (currentEvent) {
                logEvent(currentEvent.description);
                const yesButton = document.createElement('button');
                yesButton.textContent = '是';
                yesButton.onclick = currentEvent.yes;
                const noButton = document.createElement('button');
                noButton.textContent = '否';
                noButton.onclick = currentEvent.no;
                interactionButtons.appendChild(yesButton);
                interactionButtons.appendChild(noButton);
            }

            // 检查是否找到离开的道路
            if (Math.random() < 0.1) {
                logEvent('你找到了离开的道路，游戏获胜！');
                logGameHistory('胜利');
                endGame();
                return;
            }

            isProcessing = false;
        }

        // 结束游戏
        function endGame() {
            isGameOver = true;
            const interactionButtons = document.getElementById('interaction-buttons');
            interactionButtons.innerHTML = '';
            document.getElementById('restart-button').style.display = 'block';
        }

        // 重新开始游戏
        function restartGame() {
            water = 100;
            food = 100;
            electricity = 100;
            time = 1;
            isGameOver = false;
            isProcessing = false;
            currentEvent = null;
            const eventLog = document.getElementById('event-log');
            eventLog.innerHTML = '';
            const waterSpan = document.getElementById('water');
            const foodSpan = document.getElementById('food');
            const electricitySpan = document.getElementById('electricity');
            const timeSpan = document.getElementById('time');
            waterSpan.textContent = water;
            foodSpan.textContent = food;
            electricitySpan.textContent = electricity;
            timeSpan.textContent = time;
            document.getElementById('restart-button').style.display = 'none';
            logEvent('游戏重新开始，祝你好运！');
            nextDay();
        }
    </script>
</body>

</html>
