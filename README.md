
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MASTER_DEFENSE_MATRIX_v10 - MHD / SAMS / SROS Multi-Simulator</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
        }
        body {
            background-color: #020612;
            color: #00f0ff;
            font-family: 'Consolas', 'Segoe UI', monospace;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            padding: 10px;
        }
        .container {
            width: 100%;
            max-width: 1200px;
            background: #050d1a;
            border: 2px solid #00f0ff;
            box-shadow: 0 0 35px rgba(0, 240, 255, 0.25);
            border-radius: 10px;
            overflow: hidden;
        }
        .mode-bar {
            display: flex;
            background: #01040a;
            border-bottom: 2px solid #00f0ff;
        }
        .mode-btn {
            flex: 1;
            padding: 12px;
            background: #030a17;
            color: #64748b;
            border: none;
            font-weight: bold;
            font-size: 0.85rem;
            cursor: pointer;
            text-transform: uppercase;
            letter-spacing: 2px;
            transition: all 0.3s ease;
        }
        .mode-btn.active {
            background: #051833;
            color: #00f0ff;
            box-shadow: inset 0 -3px 0 #00f0ff;
            text-shadow: 0 0 10px #00f0ff;
        }
        .header {
            background: #020813;
            padding: 12px;
            text-align: center;
            border-bottom: 1px solid #00f0ff44;
            font-weight: bold;
            font-size: 1.05rem;
            letter-spacing: 2px;
            color: #00f0ff;
            text-shadow: 0 0 10px #00f0ff;
        }
        .tech-banner {
            display: flex;
            justify-content: space-around;
            background: #020c1b;
            padding: 6px;
            border-bottom: 1px solid #1e293b;
            font-size: 0.70rem;
            color: #38bdf8;
        }
        .tech-tag {
            padding: 2px 8px;
            border-radius: 3px;
            background: rgba(0, 240, 255, 0.1);
            border: 1px solid rgba(0, 240, 255, 0.3);
        }
        .hud-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
            gap: 8px;
            padding: 10px;
            background: #030b17;
            border-bottom: 1px solid #1e293b;
        }
        .hud-card {
            background: #01050d;
            border: 1px solid #00f0ff33;
            padding: 8px;
            border-radius: 4px;
        }
        .hud-title {
            font-size: 0.62rem;
            color: #94a3b8;
            text-transform: uppercase;
        }
        .hud-value {
            font-size: 0.85rem;
            font-weight: bold;
            color: #ffffff;
            margin-top: 3px;
        }
        .hud-value.alert { color: #ff0055; text-shadow: 0 0 8px #ff0055; }
        .hud-value.active { color: #10b981; text-shadow: 0 0 8px #10b981; }

        canvas {
            display: block;
            width: 100%;
            height: 450px;
            background-color: #010308;
            cursor: crosshair;
        }

        .selector-bar {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
            padding: 8px;
            background: #020814;
            justify-content: center;
            border-bottom: 1px solid #1e293b;
        }

        .btn-select {
            background: #071326;
            color: #94a3b8;
            border: 1px solid #1e293b;
            padding: 5px 9px;
            font-size: 0.68rem;
            font-weight: bold;
            cursor: pointer;
            border-radius: 3px;
            transition: all 0.2s ease;
        }
        .btn-select.selected {
            color: #00f0ff;
            border-color: #00f0ff;
            box-shadow: 0 0 8px #00f0ff;
        }

        .controls {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            padding: 10px;
            background: #020814;
            justify-content: center;
            align-items: center;
            border-top: 1px solid #1e293b;
        }

        button.btn-ctrl {
            background: #051428;
            color: #00f0ff;
            border: 1px solid #00f0ff;
            padding: 8px 12px;
            font-family: inherit;
            font-size: 0.72rem;
            font-weight: bold;
            cursor: pointer;
            border-radius: 4px;
            transition: all 0.2s ease;
            text-shadow: 0 0 4px #00f0ff;
        }
        button.btn-ctrl:hover {
            background: #00f0ff;
            color: #000;
            box-shadow: 0 0 12px #00f0ff;
        }

        button.btn-tech {
            background: #09203f;
            color: #38bdf8;
            border-color: #38bdf8;
        }
        button.btn-tech.active-tech {
            background: #38bdf8;
            color: #000;
            box-shadow: 0 0 15px #38bdf8;
        }

        button.btn-danger {
            color: #ff0055;
            border-color: #ff0055;
            text-shadow: 0 0 4px #ff0055;
        }
        button.btn-danger:hover {
            background: #ff0055;
            color: #000;
            box-shadow: 0 0 15px #ff0055;
        }

        .terminal {
            background: #010307;
            padding: 8px 12px;
            font-family: 'Courier New', monospace;
            font-size: 0.72rem;
            color: #00f0ffaa;
            height: 60px;
            overflow-y: auto;
            border-top: 1px solid #1e293b;
        }
    </style>
</head>
<body>

<div class="container">
    <!-- ABA DE NAVEGAÇÃO DE MODOS -->
    <div class="mode-bar">
        <button class="mode-btn active" id="tab-tsunami" onclick="switchMode('tsunami')">🌊 SIMULADOR ANTI-TSUNAMI</button>
        <button class="mode-btn" id="tab-virus" onclick="switchMode('virus')">☣️ SIMULADOR ANTI-VÍRUS BSL-4</button>
    </div>

    <div class="header" id="main-header">
        TSUNAMI & BIO-DEFENSE MATRIX // INTEGRATED CONTROL SUITE
    </div>

    <!-- BANNER DE INTEGRAÇÃO DAS SUAS 3 TECNOLOGIAS -->
    <div class="tech-banner">
        <span class="tech-tag" id="tag-mhd">🧲 MHD: MAGNETOHYDRODYNAMIC FIELD</span>
        <span class="tech-tag" id="tag-sams">💎 SAMS: MAGNETORHEOLOGICAL PIEZOELECTRIC FLUID</span>
        <span class="tech-tag" id="tag-sros">🎯 SROS: OPTICAL LASER SYNCRONY</span>
    </div>

    <!-- SELETOR DE CENÁRIOS / PATÓGENOS -->
    <div class="selector-bar" id="selector-container"></div>

    <!-- PAINEL HUD DE TELEMETRIA -->
    <div class="hud-grid">
        <div class="hud-card">
            <div class="hud-title">ALVO / AMEAÇA</div>
            <div class="hud-value" id="hud-target">Sumatra 2004</div>
        </div>
        <div class="hud-card">
            <div class="hud-title">MHD CAMPO ELETROMAG NÉTICO</div>
            <div class="hud-value" id="hud-mhd">DESATIVADO</div>
        </div>
        <div class="hud-card">
            <div class="hud-title">SAMS FLUIDO PIEZOELÉTRICO</div>
            <div class="hud-value" id="hud-sams">DESATIVADO</div>
        </div>
        <div class="hud-card">
            <div class="hud-title">SROS LASER OPTICAL SYNC</div>
            <div class="hud-value" id="hud-sros">DESATIVADO</div>
        </div>
        <div class="hud-card">
            <div class="hud-title">INTEGRIDADE DO BUNKER</div>
            <div class="hud-value active" id="hud-integrity">100%</div>
        </div>
    </div>

    <!-- CANVAS DA SIMULAÇÃO -->
    <canvas id="simCanvas" width="1180" height="450"></canvas>

    <!-- CONTROLES DOS SEUS TRÊS SISTEMAS E AÇÕES -->
    <div class="controls">
        <button class="btn-ctrl" id="btn-toggle">⏸️ PAUSAR</button>
        <button class="btn-ctrl" id="btn-reset">🔄 REINICIAR</button>
        
        <!-- OS SEUS TRÊS APARELHOS TECNOLÓGICOS -->
        <button class="btn-ctrl btn-tech" id="btn-mhd" onclick="toggleTech('mhd')">🧲 ATIVAR MHD</button>
        <button class="btn-ctrl btn-tech" id="btn-sams" onclick="toggleTech('sams')">💎 ATIVAR SAMS</button>
        <button class="btn-ctrl btn-tech" id="btn-sros" onclick="toggleTech('sros')">🎯 ATIVAR SROS</button>

        <button class="btn-ctrl btn-danger" id="btn-trigger" onclick="triggerThreat()">⚡ DISPARAR AMEAÇA</button>
    </div>

    <!-- TERMINAL LOG DE SISTEMA -->
    <div class="terminal" id="terminal-log">
        [SYSTEM v10] Sistemas MHD, SAMS e SROS sincronizados. Selecione o modo operacional.
    </div>
</div>

<script>
    const canvas = document.getElementById('simCanvas');
    const ctx = canvas.getContext('2d');

    let currentMode = 'tsunami'; // 'tsunami' ou 'virus'

    // ESTADO DOS SEUS TRÊS SISTEMAS
    let mhdActive = false;
    let samsActive = false;
    let srosActive = false;

    let isRunning = true;
    let threatActive = false;
    let bunkerIntegrity = 100;

    // BANCO DE DADOS - TSUNAMIS
    const TSUNAMIS = {
        sumatra: { name: "🇮🇩 Sumatra 2004", height: 30, speed: 61.7, color: "#00f0ff" },
        tohoku: { name: "🇯🇵 Tohoku 2011", height: 40, speed: 72.0, color: "#ff3366" },
        lituya: { name: "🇺🇸 Baía Lituya 1958", height: 524, speed: 160.0, color: "#38bdf8" },
        krakatoa: { name: "🇮🇩 Krakatoa 1883", height: 41, speed: 68.0, color: "#ff6600" },
        storegga: { name: "🇳🇴 Storegga (~6200 a.C.)", height: 25, speed: 55.0, color: "#a855f7" },
        lapalma: { name: "🇪🇸 La Palma (Futuro)", height: 100, speed: 120.0, color: "#f59e0b" },
        cascadia: { name: "🇺🇸🇨🇦 Cascadia (Futuro)", height: 30, speed: 65.0, color: "#10b981" },
        nankai: { name: "🇯🇵 Fossa Nankai (Futuro)", height: 35, speed: 70.0, color: "#e11d48" }
    };

    // BANCO DE DADOS - VÍRUS
    const VIRUSES = {
        rabies: { name: "Raiva (99.9%)", type: "bullet", color: "#ff0055" },
        ebola: { name: "Ebola (90%)", type: "filament", color: "#ff2200" },
        marburg: { name: "Marburg (88%)", type: "hook", color: "#ff6600" },
        nipah: { name: "Nipah (75%)", type: "enveloped", color: "#d946ef" },
        hantavirus: { name: "Hantavírus (38%)", type: "blob", color: "#a855f7" },
        smallpox: { name: "Varióla (30%)", type: "brick", color: "#f59e0b" },
        sarscov2: { name: "SARS-CoV-2 (2%)", type: "corona", color: "#eab308" },
        influenza: { name: "Influenza A (0.1%)", type: "flu", color: "#06b6d4" },
        rhinovirus: { name: "Rinovírus (0.001%)", type: "icosahedral", color: "#10b981" }
    };

    let selectedTsunamiKey = 'sumatra';
    let selectedVirusKey = 'rabies';

    // Posições e Partículas
    let waveX = -200;
    let waveHeightCurrent = 0;
    let viralParticles = [];

    function log(msg) {
        const term = document.getElementById('terminal-log');
        term.innerHTML = `> ${msg}<br>` + term.innerHTML;
    }

    function switchMode(mode) {
        currentMode = mode;
        document.getElementById('tab-tsunami').className = mode === 'tsunami' ? "mode-btn active" : "mode-btn";
        document.getElementById('tab-virus').className = mode === 'virus' ? "mode-btn active" : "mode-btn";

        buildSelectors();
        resetSim();
        log(`Alternado para Modo: ${mode.toUpperCase()}. Seus sistemas MHD, SAMS e SROS estão em prontidão.`);
    }

    function buildSelectors() {
        const container = document.getElementById('selector-container');
        container.innerHTML = '';

        if (currentMode === 'tsunami') {
            Object.keys(TSUNAMIS).forEach(key => {
                let btn = document.createElement('button');
                btn.className = `btn-select ${key === selectedTsunamiKey ? 'selected' : ''}`;
                btn.innerText = TSUNAMIS[key].name;
                btn.onclick = () => {
                    selectedTsunamiKey = key;
                    buildSelectors();
                    resetSim();
                };
                container.appendChild(btn);
            });
        } else {
            Object.keys(VIRUSES).forEach(key => {
                let btn = document.createElement('button');
                btn.className = `btn-select ${key === selectedVirusKey ? 'selected' : ''}`;
                btn.innerText = VIRUSES[key].name;
                btn.onclick = () => {
                    selectedVirusKey = key;
                    buildSelectors();
                    resetSim();
                };
                container.appendChild(btn);
            });
        }
    }

    function toggleTech(tech) {
        if (tech === 'mhd') {
            mhdActive = !mhdActive;
            document.getElementById('btn-mhd').classList.toggle('active-tech', mhdActive);
            log(mhdActive ? "🧲 MHD ATIVADO: Campo Magnetohidrodinâmico desacelerando vetor fluido/iónico." : "MHD Desativado.");
        } else if (tech === 'sams') {
            samsActive = !samsActive;
            document.getElementById('btn-sams').classList.toggle('active-tech', samsActive);
            log(samsActive ? "💎 SAMS ATIVADO: Fluido Magnetorreológico enrigecido e piezoelétricos absorvendo choque." : "SAMS Desativado.");
        } else if (tech === 'sros') {
            srosActive = !srosActive;
            document.getElementById('btn-sros').classList.toggle('active-tech', srosActive);
            log(srosActive ? "🎯 SROS ATIVADO: Feixes ópticos infravermelhos/laser sincronizados para ablação e varredura." : "SROS Desativado.");
        }
        updateHUD();
    }

    function triggerThreat() {
        threatActive = true;
        if (currentMode === 'tsunami') {
            waveX = -150;
            waveHeightCurrent = TSUNAMIS[selectedTsunamiKey].height;
            log(`🌊 Tsunami gerado! Ondas avançando com os sistemas MHD, SAMS e SROS ativados.`);
        } else {
            viralParticles = [];
            for (let i = 0; i < 45; i++) {
                viralParticles.push({
                    x: Math.random() * 150 + 20,
                    y: Math.random() * 300 + 50,
                    vx: Math.random() * 2 + 1.2,
                    vy: (Math.random() - 0.5) * 1.5,
                    size: Math.random() * 6 + 10,
                    angle: Math.random() * Math.PI * 2,
                    integrity: 100
                });
            }
            log(`☣️ Carga viral expelida! SROS, MHD e SAMS iniciando interceptação aerossol.`);
        }
    }

    function resetSim() {
        threatActive = false;
        waveX = -200;
        waveHeightCurrent = 0;
        viralParticles = [];
        bunkerIntegrity = 100;
        updateHUD();
    }

    function updateHUD() {
        if (currentMode === 'tsunami') {
            document.getElementById('hud-target').innerText = TSUNAMIS[selectedTsunamiKey].name;
        } else {
            document.getElementById('hud-target').innerText = VIRUSES[selectedVirusKey].name;
        }

        const mhdElem = document.getElementById('hud-mhd');
        mhdElem.innerText = mhdActive ? "ATIVO (CAMPO B: 12 Tesla)" : "DESATIVADO";
        mhdElem.className = mhdActive ? "hud-value active" : "hud-value";

        const samsElem = document.getElementById('hud-sams');
        samsElem.innerText = samsActive ? "ATIVO (RBCON ENRIJECIDO)" : "DESATIVADO";
        samsElem.className = samsActive ? "hud-value active" : "hud-value";

        const srosElem = document.getElementById('hud-sros');
        srosElem.innerText = srosActive ? "ATIVO (LASER SYNC 100GHz)" : "DESATIVADO";
        srosElem.className = srosActive ? "hud-value active" : "hud-value";

        const integElem = document.getElementById('hud-integrity');
        integElem.innerText = `${Math.round(bunkerIntegrity)}%`;
        integElem.className = bunkerIntegrity > 60 ? "hud-value active" : "hud-value alert";
    }

    // UPDATE DO CICLO FÍSICO
    function update() {
        if (!isRunning) return;

        if (currentMode === 'tsunami' && threatActive) {
            let speed = TSUNAMIS[selectedTsunamiKey].speed * 0.08;

            // INTERAÇÃO MHD (Freia a onda)
            if (mhdActive) speed *= 0.45;

            // INTERAÇÃO SROS (Vaporiza topo da onda com laser)
            if (srosActive) waveHeightCurrent = Math.max(2, waveHeightCurrent - 0.15);

            waveX += speed;

            // INTERAÇÃO SAMS (Absorve o impacto no bunker)
            if (waveX > 700 && waveX < 850) {
                let damage = samsActive ? 0.02 : 0.25;
                if (mhdActive) damage *= 0.5;
                bunkerIntegrity = Math.max(0, bunkerIntegrity - damage);
            }

            if (waveX > canvas.width + 200) threatActive = false;
        }

        if (currentMode === 'virus') {
            for (let i = viralParticles.length - 1; i >= 0; i--) {
                let p = viralParticles[i];

                // EFICIÊNCIA MHD (Força empuxo eletromagnético reverso)
                if (mhdActive) p.vx -= 0.05;

                p.x += p.vx;
                p.y += p.vy;

                // EFICIÊNCIA SROS (Ablação laser direta em tempo real)
                if (srosActive && Math.random() < 0.15) {
                    p.integrity -= 20;
                }

                // EFICIÊNCIA SAMS (Barreira Piezoelétrica)
                if (samsActive && p.x > 680) {
                    p.integrity -= 50; // Choque micro-piezoelétrico
                }

                if (p.integrity <= 0) {
                    viralParticles.splice(i, 1);
                    continue;
                }

                if (p.x > 700) {
                    bunkerIntegrity = Math.max(0, bunkerIntegrity - 0.05);
                }
            }
        }

        updateHUD();
    }

    // DESENHO EM CANVAS 2D
    function draw() {
        ctx.fillStyle = '#010308';
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        // Grade de Fundo
        ctx.strokeStyle = '#00f0ff10';
        ctx.lineWidth = 1;
        for (let x = 0; x < canvas.width; x += 40) {
            ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
        }

        // DESENHAR EFEITO DO SISTEMA SROS (RAIO LASER OPTICAL SINCRONY)
        if (srosActive) {
            ctx.strokeStyle = '#ff0055';
            ctx.lineWidth = 1.5;
            for (let i = 0; i < 3; i++) {
                let lx = (Date.now() / 5 + i * 200) % canvas.width;
                ctx.beginPath();
                ctx.moveTo(lx, 0);
                ctx.lineTo(lx + (Math.sin(Date.now()/200)*100), canvas.height);
                ctx.stroke();
            }
        }

        // DESENHAR EFEITO DO SISTEMA MHD (CAMPO MAGNETOHIDRODINÂMICO)
        if (mhdActive) {
            ctx.strokeStyle = '#00f0ff44';
            ctx.lineWidth = 3;
            ctx.beginPath();
            ctx.arc(400, 250, 180 + Math.sin(Date.now() / 150) * 20, 0, Math.PI * 2);
            ctx.stroke();
        }

        if (currentMode === 'tsunami') {
            // Terreno e Bunker
            ctx.fillStyle = '#071326';
            ctx.beginPath();
            ctx.moveTo(0, 300);
            ctx.lineTo(400, 300);
            ctx.lineTo(500, 260);
            ctx.lineTo(canvas.width, 260);
            ctx.lineTo(canvas.width, canvas.height);
            ctx.lineTo(0, canvas.height);
            ctx.fill();

            // Bunker
            ctx.fillStyle = samsActive ? '#10b981' : '#1e293b';
            ctx.fillRect(750, 200, 120, 60);
            ctx.strokeStyle = '#00f0ff';
            ctx.strokeRect(750, 200, 120, 60);
            ctx.fillStyle = '#ffffff';
            ctx.font = '10px Consolas';
            ctx.fillText("BUNKER (SAMS SHIELD)", 755, 235);

            // Onda de Tsunami
            if (threatActive) {
                ctx.fillStyle = TSUNAMIS[selectedTsunamiKey].color + 'aa';
                ctx.beginPath();
                ctx.moveTo(0, canvas.height);
                for (let x = 0; x <= canvas.width; x += 20) {
                    let dist = Math.abs(x - waveX);
                    let shape = Math.exp(- Math.pow(dist / 130, 2)) * waveHeightCurrent * 2.2;
                    let ground = x < 400 ? 300 : 260;
                    ctx.lineTo(x, ground - shape);
                }
                ctx.lineTo(canvas.width, canvas.height);
                ctx.fill();
            }
        } else {
            // Modo Vírus
            ctx.fillStyle = samsActive ? '#10b98122' : '#ff005511';
            ctx.fillRect(700, 50, 350, 350);
            ctx.strokeStyle = samsActive ? '#10b981' : '#ff0055';
            ctx.strokeRect(700, 50, 350, 350);
            ctx.fillStyle = '#ffffff';
            ctx.font = '11px Consolas';
            ctx.fillText("CAMARA BSL-4 (PROTEÇÃO SAMS/MHD)", 710, 80);

            // Desenhar Partículas Virais
            viralParticles.forEach(p => {
                ctx.fillStyle = VIRUSES[selectedVirusKey].color;
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.size / 2, 0, Math.PI * 2);
                ctx.fill();
            });
        }
    }

    // Controls listeners
    document.getElementById('btn-toggle').onclick = (e) => {
        isRunning = !isRunning;
        e.target.innerText = isRunning ? "⏸️ PAUSAR" : "▶️ RETOMAR";
    };
    document.getElementById('btn-reset').onclick = resetSim;

    function loop() {
        update();
        draw();
        requestAnimationFrame(loop);
    }

    // Inicialização
    buildSelectors();
    updateHUD();
    loop();
</script>
</body>
</html>
