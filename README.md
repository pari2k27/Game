<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Colorful Click Counter Game</title>

    <style>
        :root {
            --purple: #8b5cf6;
            --pink: #ec4899;
            --orange: #f59e0b;
            --green: #22c55e;
            --blue: #38bdf8;
            --dark: #0f172a;
            --dark-2: #111827;
            --card: rgba(15, 23, 42, 0.7);
            --panel: rgba(255, 255, 255, 0.08);
            --text: #f8fafc;
            --muted: #dbeafe;
            --shadow: rgba(15, 23, 42, 0.38);
        }

        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background:
                radial-gradient(circle at top left, rgba(236, 72, 153, 0.35), transparent 28%),
                radial-gradient(circle at bottom right, rgba(59, 130, 246, 0.35), transparent 25%),
                linear-gradient(135deg, #0f172a 0%, #1e1b4b 35%, #111827 100%);
            font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
            color: var(--text);
        }

        #root {
            width: 100%;
            display: flex;
            justify-content: center;
            padding: 32px 16px;
        }

        .game-shell {
            width: min(100%, 560px);
            background: linear-gradient(135deg, rgba(15, 23, 42, 0.86), rgba(30, 41, 59, 0.82));
            border: 1px solid rgba(255, 255, 255, 0.12);
            border-radius: 28px;
            box-shadow: 0 25px 70px rgba(17, 24, 39, 0.4);
            padding: 30px 22px;
            position: relative;
            overflow: hidden;
        }

        .game-shell::before,
        .game-shell::after {
            content: "";
            position: absolute;
            border-radius: 50%;
            filter: blur(18px);
            opacity: 0.7;
        }

        .game-shell::before {
            width: 220px;
            height: 220px;
            background: rgba(168, 85, 247, 0.18);
            top: -80px;
            right: -40px;
        }

        .game-shell::after {
            width: 190px;
            height: 190px;
            background: rgba(59, 130, 246, 0.16);
            bottom: -70px;
            left: -40px;
        }

        .game-header {
            position: relative;
            z-index: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            margin-bottom: 22px;
        }

        .game-header img {
            width: 52px;
            height: 52px;
            filter: drop-shadow(0 10px 20px rgba(168, 85, 247, 0.5));
        }

        h1 {
            margin: 0;
            font-size: clamp(1.8rem, 3vw, 2.9rem);
            font-weight: 800;
            letter-spacing: 0.03em;
            text-transform: uppercase;
            background: linear-gradient(135deg, #f9a8d4, #c4b5fd, #7dd3fc);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .score-panel {
            position: relative;
            z-index: 1;
            background: linear-gradient(135deg, rgba(139, 92, 246, 0.18), rgba(59, 130, 246, 0.12));
            border: 1px solid rgba(255, 255, 255, 0.14);
            border-radius: 22px;
            padding: 20px 18px;
            margin-bottom: 22px;
        }

        .score-row,
        .reset-row {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            flex-wrap: wrap;
            font-size: 1.05rem;
            color: var(--muted);
            margin: 10px 0;
        }

        .score-row img,
        .reset-row img,
        .instruction img {
            width: 30px;
            height: 30px;
            object-fit: contain;
        }

        #click-count,
        #reset-count {
            display: inline-block;
            min-width: 52px;
            text-align: center;
            font-size: clamp(2.2rem, 4vw, 3rem);
            font-weight: 900;
            color: #f8fafc;
        }

        .controls {
            position: relative;
            z-index: 1;
            display: flex;
            justify-content: center;
            gap: 14px;
            flex-wrap: wrap;
            margin-bottom: 18px;
        }

        button {
            border: none;
            border-radius: 14px;
            padding: 16px 22px;
            font-size: 1.02rem;
            font-weight: 800;
            letter-spacing: 0.02em;
            cursor: pointer;
            transition: transform 0.2s ease, box-shadow 0.2s ease, filter 0.2s ease;
            box-shadow: 0 14px 28px rgba(17, 24, 39, 0.25);
        }

        button:hover {
            transform: translateY(-3px) scale(1.02);
            filter: brightness(1.08);
        }

        #click-button {
            background: linear-gradient(135deg, var(--purple), var(--pink));
            color: white;
        }

        #reset-button {
            background: linear-gradient(135deg, #f97316, #ef4444);
            color: white;
        }

        .instruction {
            position: relative;
            z-index: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin: 18px 0 12px;
            padding: 14px 16px;
            border-radius: 16px;
            background: rgba(255, 255, 255, 0.06);
            border: 1px solid rgba(255, 255, 255, 0.08);
            color: #e2e8f0;
            text-align: center;
        }

        #win-message {
            position: relative;
            z-index: 1;
            min-height: 44px;
            margin: 16px 0 0;
            text-align: center;
            font-size: clamp(1.5rem, 4vw, 2.3rem);
            font-weight: 900;
            background: linear-gradient(135deg, #10b981, #34d399, #a7f3d0);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 10px 24px rgba(16, 185, 129, 0.18);
        }

        @media (max-width: 480px) {
            .game-shell {
                padding: 24px 16px;
            }

            .controls {
                flex-direction: column;
            }

            button {
                width: 100%;
            }
        }
    </style>
</head>

<body>
    <div id="root">
        <div class="game-shell">
            <div class="game-header">
                <img src="business-goal.png" alt="Goal Icon">
                <h1>Click Counter</h1>
            </div>

            <div class="score-panel">
                <div class="score-row">
                    <img src="trophy.png" alt="Score Icon">
                    <span>Score</span>
                    <span id="click-count">0</span>
                </div>

                <div class="reset-row">
                    <img src="image.png" alt="Reset Icon">
                    <span>Resets</span>
                    <span id="reset-count">0</span>
                </div>
            </div>

            <div class="controls">
                <button id="click-button">⚡ Click to Score</button>
                <button id="reset-button">↺ Reset</button>
            </div>

            <div class="instruction">
                <img src="image%20copy.png" alt="Instruction Icon">
                <span>Click the button to increase your score.</span>
            </div>

            <p id="win-message"></p>
        </div>
    </div>

    <script>
        const clickButton = document.getElementById("click-button");
        const resetButton = document.getElementById("reset-button");
        const clickCount = document.getElementById("click-count");
        const resetCount = document.getElementById("reset-count");
        const winMessage = document.getElementById("win-message");

        let count = 0;
        let resetCounter = 0;

        clickButton.addEventListener("click", () => {
            count++;
            clickCount.textContent = count;

            if (count === 10) {
                winMessage.textContent = "🎉 You Won! 🏆";
            }
        });

        resetButton.addEventListener("click", () => {
            count = 0;
            resetCounter++;

            clickCount.textContent = count;
            resetCount.textContent = resetCounter;
            winMessage.textContent = "";
        });
    </script>
</body>

</html>
