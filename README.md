# Cheat.V2
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Xit Injector</title>
    <style>
        /* --- CSS UNIFICADO --- */
        * { box-sizing: border-box; margin: 0; padding: 0; }
        
        body {
            background-color: #121212;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow: hidden; /* Evita scroll quando o painel abre */
        }

        /* Painel Principal */
        .panel {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 320px;
            background-color: #1a1a1a;
            border: 2px solid #ff0000;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(255, 0, 0, 0.4);
            z-index: 99999;
            display: none; /* Começa escondido */
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translate(-50%, -45%); }
            to { opacity: 1; transform: translate(-50%, -50%); }
        }

        .header {
            background: linear-gradient(90deg, #cc0000, #ff0000);
            color: white;
            padding: 12px 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-weight: bold;
            cursor: grab;
            user-select: none;
            border-top-left-radius: 8px;
            border-top-right-radius: 8px;
        }

        .header span { text-transform: uppercase; letter-spacing: 1px; }

        .close-btn {
            background: none;
            border: none;
            color: white;
            font-size: 18px;
            cursor: pointer;
            font-weight: bold;
        }

        .content {
            padding: 20px;
        }

        /* Opções */
        .option-group {
            margin-bottom: 15px;
            background: #252525;
            padding: 10px;
            border-radius: 6px;
            border: 1px solid #333;
        }

        .toggle-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
        }

        .label-text {
            color: #eee;
            font-size: 14px;
            font-weight: 500;
        }

        /* Switch Customizado */
        .switch {
            position: relative;
            display: inline-block;
            width: 40px;
            height: 20px;
        }

        .switch input { opacity: 0; width: 0; height: 0; }

        .slider {
            position: absolute;
            cursor: pointer;
            top: 0; left: 0; right: 0; bottom: 0;
            background-color: #444;
            transition: .4s;
            border-radius: 20px;
        }

        .slider:before {
            position: absolute;
            content: "";
            height: 14px;
            width: 14px;
            left: 3px;
            bottom: 3px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }

        input:checked + .slider { background-color: #ff0000; }
        input:checked + .slider:before { transform: translateX(20px); }

        /* Sliders e Inputs */
        .controls {
            display: flex;
            gap: 10px;
            align-items: center;
        }

        input[type="range"] {
            flex: 1;
            accent-color: #ff0000;
            height: 4px;
        }

        input[type="number"] {
            width: 60px;
            background: #111;
            border: 1px solid #555;
            color: white;
            padding: 4px;
            border-radius: 4px;
            text-align: center;
        }

        /* Botão Injetar */
        .inject-btn {
            width: 100%;
            padding: 15px;
            background: #ff0000;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            text-transform: uppercase;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
            transition: all 0.2s;
            margin-top: 10px;
        }

        .inject-btn:active { transform: scale(0.98); }
        
        .inject-btn.active {
            background: #00aa00;
            box-shadow: 0 0 15px #00aa00;
        }

        .status-bar {
            margin-top: 15px;
            text-align: center;
            font-size: 12px;
            color: #888;
            min-height: 18px;
        }

        /* Botão Flutuante para Abrir */
        .float-btn {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 50px;
            height: 50px;
            background: #ff0000;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            font-size: 24px;
            box-shadow: 0 4px 15px rgba(255,0,0,0.6);
            cursor: pointer;
            z-index: 100000;
            border: 2px solid white;
        }

        /* Mira (Exemplo visual) */
        .crosshair {
            position: fixed;
            top: 50%;
            left: 50%;
            width: 20px;
            height: 20px;
            border: 2px solid red;
            transform: translate(-50%, -50%);
            pointer-events: none;
            z-index: 999999;
            display: none;
        }
        .crosshair::before, .crosshair::after {
            content: '';
            position: absolute;
            background: red;
        }
        .crosshair::before { top: 9px; left: -5px; width: 30px; height: 2px; }
        .crosshair::after { left: 9px; top: -5px; height: 30px; width: 2px; }

    </style>
</head>
<body>

    <!-- Elemento Visual da Mira -->
    <div id="crosshair" class="crosshair"></div>

    <!-- Botão para Abrir o Menu -->
    <div id="open-btn" class="float-btn">🎮</div>

    <!-- Painel do XIT -->
    <div id="panel" class="panel">
        <div class="header" id="drag-header">
            <span>XIT PANEL</span>
            <button id="close-btn" class="close-btn">✖</button>
        </div>
        
        <div class="content">
            
            <!-- Aimbot -->
            <div class="option-group">
                <div class="toggle-row">
                    <span class="label-text">Aimbot</span>
                    <label class="switch">
                        <input type="checkbox" id="chk-aim">
                        <span class="slider"></span>
                    </label>
                </div>
                <div class="controls">
                    <span style="font-size:10px; color:#aaa;">Fov</span>
                    <input type="range" min="10" max="180" value="90" id="rng-aim">
                </div>
            </div>

            <!-- ESP / Wallhack -->
            <div class="option-group">
                <div class="toggle-row">
                    <span class="label-text">Wallhack (ESP)</span>
                    <label class="switch">
                        <input type="checkbox" id="chk-esp">
                        <span class="slider"></span>
                    </label>
                </div>
            </div>

            <!-- Speed Hack -->
            <div class="option-group">
                <div class="toggle-row">
                    <span class="label-text">Speed Multiplier</span>
                    <input type="number" value="1.5" step="0.1" min="1" id="inp-speed">
                </div>
            </div>

            <button id="btn-inject" class="inject-btn">INJETAR</button>
            <div id="status-msg" class="status-bar">Pronto.</div>
        </div>
    </div>

    <script>
        // --- JAVASCRIPT UNIFICADO ---

        // Elementos DOM
        const panel = document.getElementById('panel');
        const openBtn = document.getElementById('open-btn');
        const closeBtn = document.getElementById('close-btn');
        const dragHeader = document.getElementById('drag-header');
        const injectBtn = document.getElementById('btn-inject');
        const statusMsg = document.getElementById('status-msg');
        const crosshair = document.getElementById('crosshair');

        // Inputs
        const chkAim = document.getElementById('chk-aim');
        const rngAim = document.getElementById('rng-aim');
        const chkEsp = document.getElementById('chk-esp');
        const inpSpeed = document.getElementById('inp-speed');

        // Estado
        let isInjected = false;

        // --- FUNÇÕES DE INTERFACE ---

        openBtn.addEventListener('click', () => {
            panel.style.display = 'block';
            openBtn.style.display = 'none';
        });

        closeBtn.addEventListener('click', () => {
            panel.style.display = 'none';
            openBtn.style.display = 'flex';
        });

        // Arrastar Painel (Drag & Drop simples)
        let isDragging = false;
        let offsetX, offsetY;

        dragHeader.addEventListener('mousedown', startDrag);
        dragHeader.addEventListener('touchstart', startDrag, {passive: false});

        function startDrag(e) {
            isDragging = true;
            const clientX = e.type.includes('mouse') ? e.clientX : e.touches[0].clientX;
            const clientY = e.type.includes('mouse') ? e.clientY : e.touches[0].clientY;
            
            const rect = panel.getBoundingClientRect();
            offsetX = clientX - rect.left;
            offsetY = clientY - rect.top;

            panel.style.transform = 'none'; // Remove centralização para mover livremente
            panel.style.left = rect.left + 'px';
            panel.style.top = rect.top + 'px';
        }

        window.addEventListener('mousemove', moveDrag);
        window.addEventListener('touchmove', moveDrag, {passive: false});

        function moveDrag(e) {
            if (!isDragging) return;
            e.preventDefault();
            const clientX = e.type.includes('mouse') ? e.clientX : e.touches[0].clientX;
            const clientY = e.type.includes('mouse') ? e.clientY : e.touches[0].clientY;
            
            panel.style.left = (clientX - offsetX) + 'px';
            panel.style.top = (clientY - offsetY) + 'px';
        }

        window.addEventListener('mouseup', () => isDragging = false);
        window.addEventListener('touchend', () => isDragging = false);


        // --- LÓGICA DE INJEÇÃO ---

        function updateStatus(msg, color = '#888') {
            statusMsg.textContent = msg;
            statusMsg.style.color = color;
        }

        function applyFeatures() {
            // 1. Aimbot Logic
            if (chkAim.checked) {
                crosshair.style.display = 'block';
                // Ajusta tamanho da mira baseado no Fov (apenas visual neste exemplo)
                const size = parseInt(rngAim.value) / 5; 
                crosshair.style.width = size + 'px';
                crosshair.style.height = size + 'px';
            } else {
                crosshair.style.display = 'none';
            }

            // 2. ESP Logic
            if (chkEsp.checked) {
                document.body.style.filter = "contrast(150%) hue-rotate(10deg)";
            } else {
                document.body.style.filter = "none";
            }

            // 3. Speed Hack Logic
            const speed = parseFloat(inpSpeed.value);
            document.documentElement.style.setProperty('--speed-mult', speed);
            // Aplica velocidade global nas animações CSS se houver
            document.querySelectorAll('*').forEach(el => {
                const currentTrans = getComputedStyle(el).transitionDuration;
                if(currentTrans !== '0s') {
                   el.style.transitionDuration = `${parseFloat(currentTrans)/speed}s`;
                }
            });
        }

        // Atualiza inputs em tempo real
        chkAim.addEventListener('change', applyFeatures);
        rngAim.addEventListener('input', applyFeatures);
        chkEsp.addEventListener('change', applyFeatures);
        inpSpeed.addEventListener('input', applyFeatures);

        // Botão Injetar
        injectBtn.addEventListener('click', () => {
            if (!isInjected) {
                // Injetando
                updateStatus("Injetando...", "#ffcc00");
                
                setTimeout(() => {
                    applyFeatures(); // Garante que as configs atuais sejam aplicadas
                    
                    isInjected = true;
                    injectBtn.textContent = "INJETADO";
                    injectBtn.classList.add('active');
                    updateStatus("Sucesso! Modificadores ativos.", "#00ff00");
                    
                    // Efeito sonoro simples (opcional)
                    // new Audio('pop.mp3').play().catch(e=>{}); 
                }, 600);
            } else {
                // Remover Injeção (Desinjetar)
                resetInjection();
            }
        });

        function resetInjection() {
            isInjected = false;
            injectBtn.textContent = "INJETAR";
            injectBtn.classList.remove('active');
            
            // Limpa efeitos visuais
            crosshair.style.display = 'none';
            document.body.style.filter = "none";
            document.documentElement.style.removeProperty('--speed-mult');
            
            // Reseta estilos inline aplicados (limpeza básica)
            document.querySelectorAll('*').forEach(el => {
                el.style.transitionDuration = '';
            });

            updateStatus("Removido. Clique para injetar novamente.", "#888");
        }

    </script>
</body>
</html>
