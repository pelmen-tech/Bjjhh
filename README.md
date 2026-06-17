# Bjjhh
Same
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Муринский выживальщик</title>
    <style>
        :root {
            --bg-dark: #0f172a;
            --bg-card: #1e293b;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --accent-blue: #3b82f6;
            --accent-green: #22c55e;
            --accent-orange: #f59e0b;
            --accent-red: #ef4444;
            --border-color: #334155;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
        }

        body {
            background-color: var(--bg-dark);
            color: var(--text-main);
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            overflow-x: hidden;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .game-container {
            width: 100%;
            max-width: 1200px;
            height: 95vh;
            display: grid;
            grid-template-rows: auto 1fr auto;
            grid-template-columns: 2fr 1fr;
            grid-template-areas: 
                "hud hud"
                "map actions"
                "log actions";
            gap: 16px;
            padding: 16px;
        }

        /* HUD Styles */
        .hud {
            grid-area: hud;
            background: var(--bg-card);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 16px;
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 12px;
            align-items: center;
        }

        .hud-item {
            display: flex;
            flex-direction: column;
            gap: 4px;
        }

        .hud-label {
            font-size: 0.75rem;
            color: var(--text-muted);
            text-transform: uppercase;
            letter-spacing: 0.05em;
        }

        .hud-value {
            font-size: 1.25rem;
            font-weight: 700;
        }

        .bar-container {
            width: 100%;
            height: 8px;
            background-color: var(--border-color);
            border-radius: 4px;
            overflow: hidden;
            margin-top: 4px;
        }

        .bar {
            height: 100%;
            width: 0%;
            transition: width 0.5s ease-out, background-color 0.5s ease;
        }

        /* Map Styles */
        .map-section {
            grid-area: map;
            background: var(--bg-card);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 16px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        .map-title {
            align-self: flex-start;
            font-size: 1rem;
            font-weight: 600;
            color: var(--text-muted);
            margin-bottom: 12px;
        }

        .grid-map {
            display: grid;
            grid-template-columns: repeat(8, 1fr);
            grid-template-rows: repeat(8, 1fr);
            gap: 6px;
            width: 100%;
            max-width: 440px;
            aspect-ratio: 1;
            background-color: rgba(15, 23, 42, 0.6);
            border: 2px dashed var(--border-color);
            border-radius: 8px;
            padding: 8px;
        }

        .cell {
            background-color: rgba(51, 65, 85, 0.3);
            border-radius: 6px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            transition: transform 0.2s ease, background-color 0.3s ease;
            border: 1px solid rgba(255, 255, 255, 0.03);
            position: relative;
        }

        .cell.empty::after {
            content: '';
            width: 4px;
            height: 4px;
            background-color: var(--border-color);
            border-radius: 50%;
        }

        .cell.tower {
            background-color: #475569;
            box-shadow: inset 0 0 8px rgba(0, 0, 0, 0.5);
            animation: build-pop 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        .cell.shop {
            background-color: rgba(245, 158, 11, 0.2);
            border: 1px solid var(--accent-orange);
            animation: build-pop 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        .cell.park {
            background-color: rgba(34, 197, 94, 0.2);
            border: 1px solid var(--accent-green);
            animation: build-pop 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        @keyframes build-pop {
            0% { transform: scale(0.5); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }

        /* Actions Section Styles */
        .actions-section {
            grid-area: actions;
            background: var(--bg-card);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 16px;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .actions-title {
            font-size: 1rem;
            font-weight: 600;
            color: var(--text-muted);
            margin-bottom: 4px;
        }

        .action-btn {
            background: #334155;
            border: 1px solid var(--border-color);
            color: var(--text-main);
            padding: 12px 16px;
            border-radius: 8px;
            cursor: pointer;
            text-align: left;
            display: flex;
            flex-direction: column;
            gap: 4px;
            transition: all 0.2s ease;
        }

        .action-btn:hover:not(:disabled) {
            background: #475569;
            border-color: var(--text-muted);
            transform: translateY(-1px);
        }

        .action-btn:active:not(:disabled) {
            transform: translateY(1px);
        }

        .action-btn:disabled {
            opacity: 0.4;
            cursor: not-allowed;
        }

        .btn-title {
            font-weight: 700;
            font-size: 0.9rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .btn-cost {
            font-size: 0.8rem;
            color: var(--accent-orange);
            font-family: monospace;
        }

        .btn-desc {
            font-size: 0.75rem;
            color: var(--text-muted);
        }

        /* Log Section Styles */
        .log-section {
            grid-area: log;
            background: var(--bg-card);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 16px;
            display: flex;
            flex-direction: column;
            gap: 8px;
            height: 180px;
        }

        .log-title {
            font-size: 1rem;
            font-weight: 600;
            color: var(--text-muted);
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 4px;
        }

        .log-container {
            flex-grow: 1;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 6px;
            font-size: 0.85rem;
            padding-right: 4px;
        }

        .log-entry {
            line-height: 1.4;
            animation: slide-in 0.2s ease-out;
            padding-bottom: 4px;
            border-bottom: 1px dashed rgba(255, 255, 255, 0.05);
        }

        @keyframes slide-in {
            from { transform: translateY(5px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }

        .log-neutral { color: var(--text-muted); }
        .log-good { color: var(--accent-green); }
        .log-bad { color: var(--accent-red); }
        .log-info { color: var(--accent-blue); }

        /* Overlay Screens */
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: rgba(15, 23, 42, 0.9);
            backdrop-filter: blur(8px);
            z-index: 100;
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 20px;
            padding: 24px;
            text-align: center;
        }

        .overlay-title {
            font-size: 3rem;
            font-weight: 800;
        }

        .overlay-desc {
            font-size: 1.2rem;
            max-width: 600px;
            color: var(--text-muted);
            line-height: 1.6;
        }

        .restart-btn {
            background: var(--accent-blue);
            color: var(--text-main);
            border: none;
            padding: 14px 28px;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: 700;
            cursor: pointer;
            transition: background 0.2s ease;
        }

        .restart-btn:hover {
            background: #2563eb;
        }

        /* Scrollbar customizing */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: transparent;
        }
        ::-webkit-scrollbar-thumb {
            background: var(--border-color);
            border-radius: 3px;
        }
    </style>
</head>
<body>

    <div class="game-container">
        <!-- HUD -->
        <div class="hud">
            <div class="hud-item">
                <span class="hud-label">День</span>
                <span class="hud-value" id="hud-day">1 / 10</span>
            </div>
            <div class="hud-item">
                <span class="hud-label">Бюджет</span>
                <span class="hud-value" id="hud-budget" style="color: var(--accent-green);">100,000 ₽</span>
            </div>
            <div class="hud-item">
                <span class="hud-label">Население</span>
                <span class="hud-value" id="hud-pop">5,000 / 6,000</span>
            </div>
            <div class="hud-item">
                <span class="hud-label">Уровень услуг</span>
                <span class="hud-value" id="hud-services" style="color: var(--accent-orange);">10 / 100</span>
            </div>
            <div class="hud-item" style="grid-column: span 1;">
                <span class="hud-label">Счастье</span>
                <span class="hud-value" id="hud-happiness">60 / 100</span>
                <div class="bar-container">
                    <div class="bar" id="bar-happiness"></div>
                </div>
            </div>
            <div class="hud-item" style="grid-column: span 1;">
                <span class="hud-label">Очередь в метро</span>
                <span class="hud-value" id="hud-metro">25 / 100</span>
                <div class="bar-container">
                    <div class="bar" id="bar-metro"></div>
                </div>
            </div>
        </div>

        <!-- 2D Map -->
        <div class="map-section">
            <div class="map-title" id="stage-indicator">Этап 1: Первичная застройка</div>
            <div class="grid-map" id="grid-map"></div>
        </div>

        <!-- Actions Panel -->
        <div class="actions-section">
            <div class="actions-title">Панель оптимизации</div>
            
            <button class="action-btn" onclick="executeAction('build')">
                <span class="btn-title">Построить 25-этажку <span class="btn-cost">-20,000 ₽</span></span>
                <span class="btn-desc">+2000 вместимости, +800 жителей, увеличит плотность</span>
            </button>

            <button class="action-btn" onclick="executeAction('infra')">
                <span class="btn-title">Инфраструктура (Пивная/ПВЗ) <span class="btn-cost">-5,000 ₽</span></span>
                <span class="btn-desc">+10 уровень услуг района, снижает нагрузку на метро</span>
            </button>

            <button class="action-btn" onclick="executeAction('landscaping')">
                <span class="btn-title">Тактильное благоустройство <span class="btn-cost">-7,000 ₽</span></span>
                <span class="btn-desc">+15 временное счастье (на 3 дня), улучшит среду</span>
            </button>

            <button class="action-btn" onclick="executeAction('routes')">
                <span class="btn-title">Оптимизировать маршруты <span class="btn-cost">-3,000 ₽</span></span>
                <span class="btn-desc">-15 очередь в метро, водители поедут быстрее</span>
            </button>

            <button class="action-btn" onclick="executeAction('reform')">
                <span class="btn-title">Реформа ЖКХ <span class="btn-cost">Бесплатно</span></span>
                <span class="btn-desc">Риск: случайный эффект (+20 к счастью или +20 к очереди)</span>
            </button>
        </div>

        <!-- Event Logs -->
        <div class="log-section">
            <div class="log-title">Сводки оптимизации</div>
            <div class="log-container" id="log-container"></div>
        </div>
    </div>

    <!-- Modals (Victory/Defeat) -->
    <div class="overlay" id="overlay-screen">
        <div class="overlay-title" id="overlay-title">Вы проиграли</div>
        <div class="overlay-desc" id="overlay-desc">Причина поражения</div>
        <button class="restart-btn" onclick="resetGame()">Начать заново</button>
    </div>

    <script>
        // Game state
        let state = {
            day: 1,
            budget: 100000,
            population: 5000,
            housingCapacity: 6000,
            servicesScore: 10,
            happiness: 60,
            metroQueue: 25,
            tempHappinessTurns: 0,
            stage: 1,
            grid: []
        };

        const GRID_SIZE = 8;
        const totalCells = GRID_SIZE * GRID_SIZE;

        // Initialize and reset game
        function resetGame() {
            state = {
                day: 1,
                budget: 100000,
                population: 5000,
                housingCapacity: 6000,
                servicesScore: 10,
                happiness: 60,
                metroQueue: 25,
                tempHappinessTurns: 0,
                stage: 1,
                grid: Array(totalCells).fill('empty')
            };

            // Pre-populate grid to make map look interesting at start
            for (let i = 0; i < 4; i++) {
                state.grid[i * 12 + 2] = 'tower';
            }
            state.grid[15] = 'shop';
            state.grid[42] = 'park';

            document.getElementById('overlay-screen').style.display = 'none';
            document.getElementById('log-container').innerHTML = '';
            
            addLog("Вы назначены временным урбанистом-оптимизатором района. Ваша цель — продержаться 10 дней и не допустить транспортного или социального коллапса.", "info");
            render();
        }

        // Render game components
        function render() {
            // HUD Elements
            document.getElementById('hud-day').textContent = `${state.day} / 10`;
            document.getElementById('hud-budget').textContent = `${state.budget.toLocaleString()} ₽`;
            document.getElementById('hud-pop').textContent = `${state.population.toLocaleString()} / ${state.housingCapacity.toLocaleString()}`;
            document.getElementById('hud-services').textContent = `${state.servicesScore} / 100`;
            document.getElementById('hud-happiness').textContent = `${state.happiness} / 100`;
            document.getElementById('hud-metro').textContent = `${state.metroQueue} / 100`;

            // Progress bars
            const happinessBar = document.getElementById('bar-happiness');
            happinessBar.style.width = `${state.happiness}%`;
            if (state.happiness > 50) happinessBar.style.backgroundColor = 'var(--accent-green)';
            else if (state.happiness > 20) happinessBar.style.backgroundColor = 'var(--accent-orange)';
            else happinessBar.style.backgroundColor = 'var(--accent-red)';

            const metroBar = document.getElementById('bar-metro');
            metroBar.style.width = `${state.metroQueue}%`;
            if (state.metroQueue < 40) metroBar.style.backgroundColor = 'var(--accent-green)';
            else if (state.metroQueue < 75) metroBar.style.backgroundColor = 'var(--accent-orange)';
            else metroBar.style.backgroundColor = 'var(--accent-red)';

            // Stage title
            const stageText = document.getElementById('stage-indicator');
            if (state.stage === 1) {
                stageText.textContent = "Этап 1: Первичная застройка (Медленный рост, простор)";
                stageText.style.color = 'var(--accent-green)';
            } else if (state.stage === 2) {
                stageText.textContent = "Этап 2: Уплотнение (Стремительный рост, давка на входе)";
                stageText.style.color = 'var(--accent-orange)';
            } else {
                stageText.textContent = "Этап 3: Коллапс (Максимальная нагрузка систем)";
                stageText.style.color = 'var(--accent-red)';
            }

            // Map grid rendering
            const gridContainer = document.getElementById('grid-map');
            gridContainer.innerHTML = '';
            state.grid.forEach((type, index) => {
                const cell = document.createElement('div');
                cell.className = `cell ${type}`;
                if (type === 'tower') cell.textContent = '🏢';
                if (type === 'shop') cell.textContent = '🏪';
                if (type === 'park') cell.textContent = '🌳';
                gridContainer.appendChild(cell);
            });

            // Action button availability based on cash
            const buttons = document.querySelectorAll('.action-btn');
            buttons[0].disabled = state.budget < 20000;
            buttons[1].disabled = state.budget < 5000;
            buttons[2].disabled = state.budget < 7000;
            buttons[3].disabled = state.budget < 3000;
        }

        // Add log message
        function addLog(text, type = "neutral") {
            const container = document.getElementById('log-container');
            const entry = document.createElement('div');
            entry.className = `log-entry log-${type}`;
            entry.textContent = `[День ${state.day}] ${text}`;
            container.appendChild(entry);
            container.scrollTop = container.scrollHeight;
        }

        // Place a visual asset on grid
        function placeOnGrid(type) {
            const emptyIndices = [];
            state.grid.forEach((cell, idx) => {
                if (cell === 'empty') emptyIndices.push(idx);
            });

            if (emptyIndices.length > 0) {
                const randIndex = emptyIndices[Math.floor(Math.random() * emptyIndices.length)];
                state.grid[randIndex] = type;
            }
        }

        // Execute player choices
        function executeAction(actionType) {
            if (actionType === 'build') {
                state.budget -= 20000;
                state.housingCapacity += 2000;
                state.population += 800;
                placeOnGrid('tower');
                addLog("Построена новая 25-этажка 'Лазурные Пруды'. Ещё 800 человек успешно инвестировали в студии.", "info");
            } else if (actionType === 'infra') {
                state.budget -= 5000;
                state.servicesScore = Math.min(100, state.servicesScore + 10);
                placeOnGrid('shop');
                addLog("Сданы в аренду подвалы: открылась шаверма и три точки выдачи интернет-заказов.", "good");
            } else if (actionType === 'landscaping') {
                state.budget -= 7000;
                state.tempHappinessTurns = 3;
                placeOnGrid('park');
                addLog("Привезли рулонный газон и покрасили старые бордюры. Эстетический восторг продлится 3 дня.", "good");
            } else if (actionType === 'routes') {
                state.budget -= 3000;
                state.metroQueue = Math.max(0, state.metroQueue - 15);
                addLog("Маршруточники начали совершать обгоны по тротуарам. Очередь на метро временно сократилась.", "good");
            } else if (actionType === 'reform') {
                if (Math.random() > 0.5) {
                    state.happiness = Math.min(100, state.happiness + 20);
                    addLog("В рамках реформы ЖКХ дворники наконец-то убрали горы мусора. Невероятный праздник!", "good");
                } else {
                    state.metroQueue = Math.min(100, state.metroQueue + 20);
                    addLog("Реформа ЖКХ привела к сокращению автобусов. Очередь на маршрутку растянулась до КАДа.", "bad");
                }
            }

            nextTurn();
        }

        // Advanced Game Loop processing (next turn)
        function nextTurn() {
            // End of day calculations
            let revenue = state.population * 2;
            state.budget += revenue;

            // Increment Day
            state.day++;

            // Evaluate Stages
            if (state.day >= 4 && state.day <= 7) {
                state.stage = 2;
            } else if (state.day >= 8) {
                state.stage = 3;
            }

            // Passive population growth per stage
            let passiveGrowth = 0;
            if (state.stage === 2) {
                passiveGrowth = 300;
                state.population += passiveGrowth;
                addLog("Миграция: Новые арендаторы заполнили свободные диваны (+300 населения).", "neutral");
            } else if (state.stage === 3) {
                passiveGrowth = 600;
                state.population += passiveGrowth;
                addLog("Субурбанизация: Люди бегут из центра в дешёвые метры (+600 населения).", "neutral");
            }

            // Metro Queue Growth Formula based on stage and density
            let density = state.population / state.housingCapacity;
            let baseMetroGrowth = (state.stage === 1) ? 5 : (state.stage === 2) ? 12 : 20;
            let densityFactor = Math.max(0, Math.floor((density - 1.0) * 15));
            let servicesReduction = Math.floor(state.servicesScore * 0.15);
            
            let finalQueueChange = baseMetroGrowth + densityFactor - servicesReduction;
            state.metroQueue = Math.max(0, Math.min(100, state.metroQueue + finalQueueChange));

            if (finalQueueChange > 0) {
                addLog(`Транспорт: Из-за застройки очередь на метро выросла на ${finalQueueChange}.`, "bad");
            }

            // Temp happiness decrement
            let activeTempBonus = 0;
            if (state.tempHappinessTurns > 0) {
                activeTempBonus = 15;
                state.tempHappinessTurns--;
                if (state.tempHappinessTurns === 0) {
                    addLog("Временное благоустройство пришло в негодность: газоны вытоптаны.", "bad");
                }
            }

            // Happiness update formula
            let servicesBonus = Math.min(15, Math.floor(state.servicesScore * 0.15));
            state.happiness = Math.max(0, Math.min(100, Math.round(
                60 - (density * 18) - (state.metroQueue / 10) + servicesBonus + activeTempBonus
            )));

            // Round calculations
            state.population = Math.round(state.population);
            state.housingCapacity = Math.round(state.housingCapacity);

            render();

            // End game conditions check
            checkGameConditions();
        }

        // Check if conditions lead to victory or defeat
        function checkGameConditions() {
            const overlay = document.getElementById('overlay-screen');
            const title = document.getElementById('overlay-title');
            const desc = document.getElementById('overlay-desc');

            if (state.happiness <= 0) {
                title.textContent = "Социальный бунт!";
                desc.textContent = "Уровень счастья упал до нуля. Жители устроили митинг прямо посреди бесконечной пробки и перекрыли все выезды. Муниципальная администрация расформирована, вы уволены.";
                overlay.style.display = 'flex';
            } else if (state.metroQueue >= 100) {
                title.textContent = "Транспортный коллапс!";
                desc.textContent = "Очередь в метро превысила 100 метров и замкнулась в идеальный круг. Жители начали строить палаточные лагеря прямо у турникетов. Район признан зоной гуманитарного бедствия.";
                overlay.style.display = 'flex';
            } else if (state.day > 10) {
                title.textContent = "Победа оптимизатора!";
                desc.textContent = `Вам удалось пережить сложнейшие 10 дней! Вы сохранили баланс между сверхприбылью от застройки и недовольством электората. Вам выписана почётная грамота и премия в 500 рублей!`;
                overlay.style.display = 'flex';
            }
        }

        // Bootstrap
        window.onload = resetGame;
    </script>
</body>
</html>
