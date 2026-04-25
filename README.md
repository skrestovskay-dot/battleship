<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Battleship | Tick the Box — 6 game variants</title>
    <style>
        * {
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', 'Roboto', system-ui, sans-serif;
            background: linear-gradient(145deg, #1a5f7a 0%, #0e3e52 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            margin: 0;
        }

        .game-container {
            max-width: 1300px;
            width: 100%;
            background: #fef5e6;
            border-radius: 56px;
            box-shadow: 0 30px 45px rgba(0, 0, 0, 0.4);
            padding: 20px 25px 30px;
            transition: all 0.2s;
        }

        .tabs {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            margin-bottom: 25px;
            justify-content: center;
            border-bottom: 2px solid #ffcd94;
            padding-bottom: 12px;
        }

        .tab-btn {
            background: #e9cfac;
            border: none;
            padding: 10px 20px;
            font-size: 1rem;
            font-weight: bold;
            border-radius: 60px;
            cursor: pointer;
            transition: 0.2s;
            color: #3b2c1e;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .tab-btn.active {
            background: #f5a623;
            color: white;
            box-shadow: 0 4px 10px rgba(245,166,35,0.4);
            transform: scale(1.02);
        }

        .tab-btn:hover:not(.active) {
            background: #f7d9aa;
            transform: translateY(-2px);
        }

        h2 {
            text-align: center;
            font-size: 1.7rem;
            margin: 5px 0 5px;
            color: #1f5e7e;
        }

        .subtitle {
            text-align: center;
            font-size: 0.9rem;
            color: #5b7f92;
            margin-bottom: 20px;
        }

        .players-panel {
            display: flex;
            justify-content: space-between;
            gap: 20px;
            margin-bottom: 25px;
            flex-wrap: wrap;
        }

        .player-card {
            flex: 1;
            background: #ffffffcc;
            backdrop-filter: blur(4px);
            border-radius: 40px;
            padding: 12px 16px;
            text-align: center;
            box-shadow: 0 5px 12px rgba(0,0,0,0.1);
            transition: 0.2s;
            border: 2px solid transparent;
        }

        .player-card.active {
            background: #ffe3b5;
            border-color: #f5a623;
            box-shadow: 0 0 0 3px #ffd966;
        }

        .player-name {
            font-size: 1.5rem;
            font-weight: bold;
            letter-spacing: 1px;
        }

        .player-score {
            font-size: 2rem;
            font-weight: 800;
            color: #1e6f5c;
        }

        .turn-indicator {
            background: #2c5282;
            color: gold;
            padding: 4px 12px;
            border-radius: 60px;
            font-size: 0.75rem;
            font-weight: bold;
            display: inline-block;
        }

        .table-wrapper {
            overflow-x: auto;
            border-radius: 32px;
            background: #fff;
            padding: 8px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }

        .game-table {
            width: 100%;
            border-collapse: collapse;
            text-align: center;
            background: white;
            border-radius: 24px;
            font-size: 0.9rem;
        }

        .game-table th, .game-table td {
            border: 2px solid #2f6b8f;
            padding: 12px 5px;
            transition: 0.05s linear;
        }

        .game-table th {
            background: #2a5f7a;
            color: white;
            font-weight: 600;
            font-size: 1rem;
        }

        .game-table td {
            background-color: #fcf7ef;
            cursor: pointer;
            font-weight: bold;
            font-size: 1.4rem;
        }

        .game-table td:hover:not(.marked-hit):not(.marked-miss) {
            background-color: #ffe3a3;
        }

        .marked-hit {
            background-color: #2b8c5e !important;
            color: white;
            cursor: default;
            position: relative;
        }
        .marked-hit::after {
            content: "✓";
            font-size: 1.7rem;
            font-weight: bold;
        }

        .marked-miss {
            background-color: #b3442c !important;
            color: white;
            cursor: default;
            position: relative;
        }
        .marked-miss::after {
            content: "✗";
            font-size: 1.7rem;
            font-weight: bold;
        }

        .game-table td span {
            display: none;
        }

        .reset-btn {
            background: #e0a800;
            border: none;
            font-weight: bold;
            font-size: 1.2rem;
            padding: 12px 24px;
            border-radius: 60px;
            margin-top: 25px;
            width: 100%;
            cursor: pointer;
            color: #2d2b1f;
            transition: 0.2s;
        }

        .reset-btn:hover {
            background: #c48f0b;
            transform: scale(0.98);
            color: white;
        }

        .winner-message {
            text-align: center;
            margin-top: 15px;
            font-size: 1.5rem;
            font-weight: bold;
            background: gold;
            display: inline-block;
            width: auto;
            padding: 6px 28px;
            border-radius: 60px;
            color: #1f4e3c;
        }

        footer {
            text-align: center;
            font-size: 0.7rem;
            margin-top: 20px;
            color: #5f7e6b;
        }
        @media (max-width: 700px) {
            .game-table th, .game-table td { padding: 6px 2px; font-size: 0.7rem; }
            .player-name { font-size: 1rem; }
        }
    </style>
</head>
<body>
<div class="game-container">
    <div class="tabs" id="tabsContainer"></div>
    <h2 id="gameTitle">🎯 Weather vs Symptoms</h2>
    <div class="subtitle" id="gameSubtitle">Check the correct matches (hidden targets)</div>

    <div class="players-panel" id="playersPanel"></div>
    <div class="table-wrapper">
        <table class="game-table" id="gameTable">
            <thead id="tableHeader"></thead>
            <tbody id="tableBody"></tbody>
        </table>
    </div>
    <button class="reset-btn" id="resetGameBtn">🔄 New game / Reset board</button>
    <div id="winnerMessage" style="text-align: center;"></div>
    <footer>💡 Rules: Click a cell to guess. HIT (✓) = correct match → extra turn. MISS (✗) = turn passes. First to reach target score wins. Two players.</footer>
</div>

<script>
    // ======================== 6 GAME VARIANTS ========================
    const GAME_VARIANTS = [
        {
            name: "🌦️ Weather vs Symptoms",
            rows: ["Weather: rainy", "Weather: windy", "Weather: snowy"],
            cols: ["cough", "runny nose", "headache", "sore throat", "cold"],
            winScore: 5
        },
        {
            name: "🐶 Animals vs Food",
            rows: ["🐱 Cat", "🐶 Dog", "🐭 Mouse", "🐰 Rabbit"],
            cols: ["🥛 Milk", "🍖 Meat", "🧀 Cheese", "🥕 Carrot", "🐟 Fish"],
            winScore: 5
        },
        {
            name: "👨‍🍳 Professions vs Tools",
            rows: ["Doctor", "Chef", "Builder", "Painter", "Musician"],
            cols: ["🩺 Stethoscope", "🔪 Knife", "🔨 Hammer", "🎨 Brush", "🎸 Guitar"],
            winScore: 5
        },
        {
            name: "🌍 Countries vs Capitals",
            rows: ["France", "Germany", "Italy", "Spain", "Japan"],
            cols: ["Paris", "Berlin", "Rome", "Madrid", "Tokyo"],
            winScore: 5
        },
        {
            name: "🎬 Movies vs Characters",
            rows: ["Harry Potter", "Lord of the Rings", "Star Wars", "Avengers", "Pirates Caribbean"],
            cols: ["Harry", "Frodo", "Luke Skywalker", "Tony Stark", "Jack Sparrow"],
            winScore: 5
        },
        {
            name: "⚽ Sports vs Equipment",
            rows: ["Football", "Basketball", "Tennis", "Hockey", "Baseball"],
            cols: ["⚽ Ball", "🏀 Hoop", "🎾 Racket", "🏒 Stick", "⚾ Bat"],
            winScore: 5
        }
    ];

    let currentVariantIndex = 0;
    let correctCells = new Set();
    let board = [];
    let currentPlayer = 0;
    let scores = [0, 0];
    let gameActive = true;
    let winner = null;

    // Generate random correct pairs (winScore number of targets)
    function generateRandomPairs(rowsCount, colsCount, winScore) {
        let allPairs = [];
        for (let r = 0; r < rowsCount; r++) {
            for (let c = 0; c < colsCount; c++) {
                allPairs.push(`${r},${c}`);
            }
        }
        // Fisher-Yates shuffle
        for (let i = allPairs.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [allPairs[i], allPairs[j]] = [allPairs[j], allPairs[i]];
        }
        const selected = new Set();
        const targetsCount = Math.min(winScore, allPairs.length);
        for (let i = 0; i < targetsCount; i++) {
            selected.add(allPairs[i]);
        }
        return selected;
    }

    function initGame() {
        const variant = GAME_VARIANTS[currentVariantIndex];
        const rowCount = variant.rows.length;
        const colCount = variant.cols.length;
        const win = variant.winScore;

        correctCells = generateRandomPairs(rowCount, colCount, win);
        // Reset board
        board = [];
        for (let i = 0; i < rowCount; i++) {
            board[i] = [];
            for (let j = 0; j < colCount; j++) {
                board[i][j] = null;
            }
        }
        scores = [0, 0];
        currentPlayer = 0;
        gameActive = true;
        winner = null;
        const winnerDiv = document.getElementById("winnerMessage");
        if (winnerDiv) winnerDiv.innerHTML = "";
        renderUI();
    }

    function handleCellClick(rowIdx, colIdx) {
        if (!gameActive || winner !== null) {
            alert("Game over! Press 'New game' to start again.");
            return;
        }
        if (board[rowIdx][colIdx] !== null) {
            alert("This cell is already marked! Turn passes.");
            switchPlayer();
            renderUI();
            return;
        }
        const cellKey = `${rowIdx},${colIdx}`;
        const isHit = correctCells.has(cellKey);
        const variant = GAME_VARIANTS[currentVariantIndex];
        const winScore = variant.winScore;

        if (isHit) {
            // HIT
            board[rowIdx][colIdx] = 'hit';
            scores[currentPlayer] += 1;
            renderUI();
            if (scores[currentPlayer] >= winScore) {
                gameActive = false;
                winner = currentPlayer;
                const winnerName = currentPlayer === 0 ? "Player 1 🏆" : "Player 2 🏆";
                const winnerDiv = document.getElementById("winnerMessage");
                if (winnerDiv) winnerDiv.innerHTML = `<div class="winner-message">🏆 VICTORY! ${winnerName} 🏆</div>`;
                renderUI();
                return;
            }
            // Extra turn: do NOT switch player, just re-render
            // But we must ensure that if after render winner condition met - stop.
            if (scores[currentPlayer] >= winScore) {
                gameActive = false;
                winner = currentPlayer;
                const winnerName = currentPlayer === 0 ? "Player 1 🏆" : "Player 2 🏆";
                const winnerDiv = document.getElementById("winnerMessage");
                if (winnerDiv) winnerDiv.innerHTML = `<div class="winner-message">🏆 VICTORY! ${winnerName} 🏆</div>`;
                renderUI();
                return;
            }
        } else {
            // MISS
            board[rowIdx][colIdx] = 'miss';
            renderUI();
            switchPlayer();
        }
        // final check after possible switch
        if (scores[0] >= winScore || scores[1] >= winScore) {
            gameActive = false;
            winner = scores[0] >= winScore ? 0 : 1;
            const winnerName = winner === 0 ? "Player 1 🏆" : "Player 2 🏆";
            const winnerDiv = document.getElementById("winnerMessage");
            if (winnerDiv) winnerDiv.innerHTML = `<div class="winner-message">🏆 VICTORY! ${winnerName} 🏆</div>`;
        }
        renderUI();
    }

    function switchPlayer() {
        currentPlayer = currentPlayer === 0 ? 1 : 0;
    }

    function renderUI() {
        const variant = GAME_VARIANTS[currentVariantIndex];
        const rowsList = variant.rows;
        const colsList = variant.cols;
        const winScore = variant.winScore;

        // Players Panel
        const panel = document.getElementById("playersPanel");
        if (panel) {
            panel.innerHTML = `
                <div class="player-card ${currentPlayer === 0 && gameActive && winner === null ? 'active' : ''}">
                    <div class="player-name">⚓ PLAYER 1</div>
                    <div class="player-score">🎯 ${scores[0]}</div>
                    ${currentPlayer === 0 && gameActive && winner === null ? '<div class="turn-indicator">🔥 TURN</div>' : ''}
                    ${winner === 0 ? '<div style="color:goldenrod;">⭐ WINNER ⭐</div>' : ''}
                </div>
                <div class="player-card ${currentPlayer === 1 && gameActive && winner === null ? 'active' : ''}">
                    <div class="player-name">⛵ PLAYER 2</div>
                    <div class="player-score">🎯 ${scores[1]}</div>
                    ${currentPlayer === 1 && gameActive && winner === null ? '<div class="turn-indicator">🔥 TURN</div>' : ''}
                    ${winner === 1 ? '<div style="color:goldenrod;">⭐ WINNER ⭐</div>' : ''}
                </div>
            `;
        }

        // Table Header
        const thead = document.getElementById("tableHeader");
        if (thead) {
            let headerRow = "<tr><th>🧩 Category →</th>";
            for (let col of colsList) {
                headerRow += `<th>${col}</th>`;
            }
            headerRow += "</tr>";
            thead.innerHTML = headerRow;
        }

        // Table Body
        const tbody = document.getElementById("tableBody");
        if (!tbody) return;
        tbody.innerHTML = "";
        for (let r = 0; r < rowsList.length; r++) {
            const tr = document.createElement("tr");
            const tdFirst = document.createElement("td");
            tdFirst.style.backgroundColor = "#cfe3ef";
            tdFirst.style.fontWeight = "bold";
            tdFirst.style.cursor = "default";
            tdFirst.innerText = rowsList[r];
            tr.appendChild(tdFirst);

            for (let c = 0; c < colsList.length; c++) {
                const td = document.createElement("td");
                const status = board[r]?.[c];
                if (status === 'hit') {
                    td.classList.add("marked-hit");
                    td.style.cursor = "default";
                } else if (status === 'miss') {
                    td.classList.add("marked-miss");
                    td.style.cursor = "default";
                } else {
                    td.style.cursor = "pointer";
                    td.addEventListener("click", (function(row, col) {
                        return function() { handleCellClick(row, col); };
                    })(r, c));
                }
                const span = document.createElement("span");
                span.innerText = "";
                td.appendChild(span);
                tr.appendChild(td);
            }
            tbody.appendChild(tr);
        }

        // Disable clicks after game over
        if (!gameActive || winner !== null) {
            const allCells = document.querySelectorAll("#gameTable tbody td:not(:first-child)");
            allCells.forEach(cell => {
                if (!cell.classList.contains("marked-hit") && !cell.classList.contains("marked-miss")) {
                    cell.style.cursor = "not-allowed";
                    const newCell = cell.cloneNode(true);
                    cell.parentNode.replaceChild(newCell, cell);
                }
            });
        }

        // Update header texts
        const titleEl = document.getElementById("gameTitle");
        if (titleEl) titleEl.innerHTML = `🎲 ${variant.name}`;
        const subEl = document.getElementById("gameSubtitle");
        if (subEl) subEl.innerHTML = `Goal: get ${winScore} hits (green checks). Hit = extra turn.`;
    }

    function resetGame() {
        initGame();
    }

    function switchVariant(index) {
        currentVariantIndex = index;
        initGame();
        updateTabsUI();
    }

    function updateTabsUI() {
        const tabsContainer = document.getElementById("tabsContainer");
        if (!tabsContainer) return;
        tabsContainer.innerHTML = "";
        GAME_VARIANTS.forEach((variant, idx) => {
            const btn = document.createElement("button");
            btn.textContent = variant.name;
            btn.classList.add("tab-btn");
            if (idx === currentVariantIndex) btn.classList.add("active");
            btn.addEventListener("click", () => {
                if (idx !== currentVariantIndex) {
                    switchVariant(idx);
                }
            });
            tabsContainer.appendChild(btn);
        });
    }

    document.addEventListener("DOMContentLoaded", () => {
        updateTabsUI();
        initGame();
        const resetBtn = document.getElementById("resetGameBtn");
        if (resetBtn) resetBtn.addEventListener("click", resetGame);
    });
</script>
</body>
</html>
