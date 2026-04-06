# yiyangtan493-cmd.github.io
A coconut clicking game
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Coconut Clicker | Idle Clicker Game</title>
    <style>
        * {
            user-select: none;
            box-sizing: border-box;
        }

        body {
            margin: 0;
            min-height: 100vh;
            background: linear-gradient(145deg, #4a2f1d 0%, #2d1e12 100%);
            font-family: 'Segoe UI', 'Poppins', system-ui, -apple-system, 'Inter', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            position: relative;
            overflow-x: hidden;
        }

        .tree-left, .tree-right {
            position: fixed;
            bottom: 0;
            width: 200px;
            height: auto;
            pointer-events: none;
            z-index: 5;
            filter: drop-shadow(8px 12px 8px rgba(0,0,0,0.3));
        }
        .tree-left { left: 10px; }
        .tree-right { right: 10px; transform: scaleX(-1); }
        .tree-svg { width: 100%; height: auto; display: block; }

        .game-container {
            max-width: 750px;
            width: 100%;
            background: rgba(255,248,225,0.94);
            backdrop-filter: blur(8px);
            border-radius: 64px;
            box-shadow: 0 25px 45px rgba(0,0,0,0.4);
            overflow: hidden;
            transition: transform 0.1s ease;
            position: relative;
            z-index: 20;
            margin-bottom: 30px;
        }

        .game-scroll {
            max-height: 75vh;
            overflow-y: auto;
            padding: 20px 18px 30px;
            scrollbar-width: thin;
        }

        .counter-card {
            background: #fff7e8;
            border-radius: 60px;
            padding: 16px 20px;
            text-align: center;
            box-shadow: 0 6px 0 #c29b4a;
            margin-bottom: 20px;
        }

        .coconut-number {
            font-size: 2.5rem;
            font-weight: 800;
            background: linear-gradient(135deg, #b87333, #e7b42c);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .ascension-badge {
            background: #ffd966;
            border-radius: 40px;
            padding: 6px 12px;
            font-weight: bold;
            color: #7a3e1f;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            margin-top: 8px;
        }

        .click-area {
            background: #fbe9c3;
            border-radius: 80px;
            padding: 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 15px;
            margin-bottom: 20px;
            box-shadow: 0 8px 0 #b6863c;
        }

        .coconut-btn {
            background: none;
            border: none;
            font-size: 5rem;
            cursor: pointer;
            transition: transform 0.1s ease;
            filter: drop-shadow(6px 8px 0 #9b6e3a);
        }
        .coconut-btn:active { transform: scale(0.92); }

        .stats-panel {
            background: #fef3e0;
            border-radius: 36px;
            padding: 12px 18px;
            flex: 1;
        }

        .merchant-panel {
            background: #faeecd;
            border-radius: 48px;
            padding: 14px;
            margin-bottom: 20px;
        }

        .progress-bar {
            height: 12px;
            background: #ddc7a0;
            border-radius: 20px;
            overflow: hidden;
            margin: 10px 0;
        }

        .progress-fill {
            width: 0%;
            height: 100%;
            transition: width 0.2s linear;
        }

        .merchant-stock {
            display: flex;
            overflow-x: auto;
            gap: 12px;
            padding: 12px 4px;
        }

        .variant-card {
            min-width: 110px;
            background: #fff6e6;
            border-radius: 28px;
            padding: 12px 8px;
            text-align: center;
            border: 2px solid #e7bc7e;
        }

        .buy-btn {
            background: #4c8b3c;
            border: none;
            border-radius: 32px;
            color: white;
            padding: 6px 12px;
            font-weight: bold;
            cursor: pointer;
        }

        .upgrade-scroll {
            display: flex;
            overflow-x: auto;
            gap: 14px;
            padding: 8px 0;
        }

        .upgrade-item {
            background: #fffaec;
            border-radius: 28px;
            min-width: 130px;
            padding: 12px;
            text-align: center;
        }

        .action-bar {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 12px;
            margin: 16px 0;
        }

        .action-btn {
            background: #7f5539;
            color: white;
            border: none;
            border-radius: 60px;
            padding: 10px 18px;
            cursor: pointer;
            font-weight: bold;
        }

        .particle-layer {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 200;
        }

        .falling-coconut {
            position: absolute;
            font-size: 2rem;
            pointer-events: none;
            z-index: 250;
            filter: drop-shadow(2px 4px 6px rgba(0,0,0,0.3));
        }

        .ascension-glow {
            position: fixed;
            inset: 0;
            background: radial-gradient(circle at center, rgba(255,215,0,0.5) 0%, rgba(255,100,0,0) 80%);
            pointer-events: none;
            opacity: 0;
            transition: opacity 0.2s;
            z-index: 150;
        }

        .achievement-toast {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%) translateY(-100px);
            background: linear-gradient(135deg, #ffd700, #ffaa00);
            color: #3d2b1a;
            padding: 12px 24px;
            border-radius: 60px;
            font-weight: bold;
            font-size: 1rem;
            display: flex;
            align-items: center;
            gap: 12px;
            z-index: 1000;
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
            transition: transform 0.3s ease;
            border: 2px solid #fff;
        }
        .achievement-toast.show {
            transform: translateX(-50%) translateY(0);
        }
        .achievement-toast span { font-size: 1.8rem; }

        .achievements-panel {
            background: #fff7e8;
            border-radius: 48px;
            padding: 16px;
            margin-top: 20px;
        }
        .achievements-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
            gap: 12px;
            max-height: 300px;
            overflow-y: auto;
            padding: 8px;
        }
        .achievement-card {
            background: #f0e6d0;
            border-radius: 24px;
            padding: 12px;
            text-align: center;
            border: 2px solid #c9a87b;
        }
        .achievement-card.unlocked {
            background: linear-gradient(135deg, #ffecb3, #ffe0a3);
            border-color: #ffaa44;
            box-shadow: 0 0 10px rgba(255,170,68,0.5);
        }
        .achievement-card.locked { opacity: 0.6; filter: grayscale(0.3); }
        .achievement-icon { font-size: 2.2rem; }
        .achievement-name { font-weight: bold; font-size: 0.8rem; margin-top: 6px; }
        .achievement-desc { font-size: 0.65rem; color: #6b4c2c; }
        .achievement-reward { font-size: 0.7rem; color: #d4a017; font-weight: bold; margin-top: 4px; }

        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            backdrop-filter: blur(4px);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }
        .modal-card {
            background: #fff7e6;
            border-radius: 64px;
            max-width: 90%;
            width: 500px;
            padding: 24px;
            max-height: 80vh;
            overflow-y: auto;
        }
        .close-btn {
            background: #b36b3c;
            color: white;
            border: none;
            border-radius: 40px;
            padding: 8px 20px;
            cursor: pointer;
        }
        .sound-toggle {
            background: #7f5539;
            color: white;
            border: none;
            border-radius: 60px;
            padding: 8px 16px;
            cursor: pointer;
            font-size: 0.8rem;
            display: inline-flex;
            align-items: center;
            gap: 6px;
            margin-top: 6px;
        }
        .enable-sound-btn {
            background: #ffaa44;
            color: #3d2b1a;
            border: none;
            border-radius: 60px;
            padding: 10px 20px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 10px;
            animation: pulse 1s infinite;
        }
        @keyframes pulse { 0% { opacity: 0.7; } 50% { opacity: 1; } 100% { opacity: 0.7; } }

        @media (max-width: 900px) { .tree-left, .tree-right { width: 140px; } }
    </style>
</head>
<body>

<div class="tree-left">
    <svg class="tree-svg" viewBox="0 0 200 400" fill="none">
        <path d="M90 400 L85 200 Q83 150 95 100 L100 50 L105 100 Q117 150 115 200 L110 400 Z" fill="#6B4226"/>
        <path d="M100 50 Q40 20 10 60 Q40 40 100 55" fill="#2d6a2d"/>
        <path d="M100 50 Q160 20 190 60 Q160 40 100 55" fill="#2d6a2d"/>
        <circle cx="80" cy="65" r="8" fill="#8B5A2B"/>
        <circle cx="120" cy="70" r="7" fill="#8B5A2B"/>
        <circle cx="100" cy="85" r="9" fill="#9B6A3B"/>
    </svg>
</div>

<div class="tree-right">
    <svg class="tree-svg" viewBox="0 0 200 400" fill="none">
        <path d="M90 400 L85 200 Q83 150 95 100 L100 50 L105 100 Q117 150 115 200 L110 400 Z" fill="#6B4226"/>
        <path d="M100 50 Q40 20 10 60 Q40 40 100 55" fill="#2d6a2d"/>
        <path d="M100 50 Q160 20 190 60 Q160 40 100 55" fill="#2d6a2d"/>
        <circle cx="80" cy="65" r="8" fill="#8B5A2B"/>
        <circle cx="120" cy="70" r="7" fill="#8B5A2B"/>
        <circle cx="100" cy="85" r="9" fill="#9B6A3B"/>
    </svg>
</div>

<div class="game-container" id="gameContainer">
    <div class="game-scroll">
        <div class="counter-card">
            <div>🥥 COCONUTS</div>
            <div class="coconut-number" id="coconutDisplay">0</div>
            <div id="ascensionInfo" class="ascension-badge" style="display: none;">✨ Ascension Tier 0 · 1.0x</div>
            <div id="nextTierHint" style="font-size:0.7rem; margin-top:6px; color:#a5673f;"></div>
        </div>
        <div class="click-area">
            <button class="coconut-btn" id="coconutClickBtn">🥥</button>
            <div class="stats-panel">
                <div>💪 Click Power: <strong id="clickPowerVal">1</strong></div>
                <div>⚡ Total per Click: <strong id="totalPerClick">1</strong></div>
                <div>🖱️ Total Clicks: <strong id="totalClicks">0</strong></div>
                <div>✨ Equipped: <strong id="equippedName">Normal Coconut</strong></div>
                <button class="sound-toggle" id="soundToggleBtn">🔊 Sound ON</button>
                <button class="enable-sound-btn" id="enableSoundBtn">🎵 Click to Enable Sound</button>
            </div>
        </div>
        <div class="merchant-panel" id="merchantPanel">
            <div id="merchantTimerText">Merchant Arrives in 30s</div>
            <div class="progress-bar"><div class="progress-fill" id="merchantProgressFill" style="background:#3b82f6;"></div></div>
            <div id="merchantStockArea" style="display: none;"><div style="font-weight: bold;">🌴 MERCHANT STOCK 🌴</div><div class="merchant-stock" id="merchantStockList"></div></div>
            <div id="merchantEmptyMsg">Merchant will return soon...</div>
        </div>
        <div class="action-bar">
            <button class="action-btn" id="exportBtn">📤 Export</button>
            <button class="action-btn" id="importBtn">📥 Import</button>
            <button class="action-btn" id="collectionBtn">🎒 Equip Coconuts</button>
            <button class="action-btn" id="achievementsBtn">🏆 Achievements</button>
            <button class="action-btn" id="ascendBtn">🌟 Ascend</button>
        </div>
        <div style="background: #f8ebcf; border-radius: 48px; padding: 12px;">
            <div style="font-weight: bold;">⬆️ UPGRADES</div>
            <div class="upgrade-scroll" id="upgradesList"></div>
        </div>

        <div class="achievements-panel">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
                <div style="font-weight: bold;">🏆 ACHIEVEMENTS <span id="achievementCount">0/0</span></div>
                <button id="toggleAchievementsBtn" style="background: #b8860b; padding: 4px 12px;">▼ Hide</button>
            </div>
            <div id="achievementsGrid" class="achievements-grid"></div>
        </div>
    </div>
</div>

<div id="modalContainer"></div>
<div class="particle-layer" id="particleLayer"></div>
<div class="ascension-glow" id="ascensionGlow"></div>
<div id="achievementToast" class="achievement-toast"></div>

<script>
    // ========== AUDIO SYSTEM ==========
    let audioContext = null;
    let soundEnabled = true;
    let audioReady = false;

    function initAndEnableAudio() {
        if (audioContext) {
            if (audioContext.state === 'suspended') {
                audioContext.resume().then(() => {
                    audioReady = true;
                    document.getElementById("enableSoundBtn").style.display = "none";
                });
            }
            return;
        }
        try {
            audioContext = new (window.AudioContext || window.webkitAudioContext)();
            audioContext.resume().then(() => {
                audioReady = true;
                document.getElementById("enableSoundBtn").style.display = "none";
            });
        } catch(e) { console.log("Web Audio not supported"); }
    }

    function playSoftPop() {
        if (!soundEnabled || !audioReady || !audioContext) return;
        try {
            const now = audioContext.currentTime;
            const gainNode = audioContext.createGain();
            gainNode.gain.setValueAtTime(0.25, now);
            gainNode.gain.exponentialRampToValueAtTime(0.0001, now + 0.25);
            gainNode.connect(audioContext.destination);
            const oscillator = audioContext.createOscillator();
            oscillator.type = 'sine';
            oscillator.frequency.setValueAtTime(380, now);
            oscillator.frequency.exponentialRampToValueAtTime(280, now + 0.1);
            oscillator.connect(gainNode);
            oscillator.start(now);
            oscillator.stop(now + 0.12);
        } catch(e) {}
    }

    function playAchievementSound() {
        if (!soundEnabled || !audioReady || !audioContext) return;
        try {
            const now = audioContext.currentTime;
            const gain = audioContext.createGain();
            gain.gain.setValueAtTime(0.3, now);
            gain.gain.exponentialRampToValueAtTime(0.0001, now + 0.5);
            gain.connect(audioContext.destination);
            const osc = audioContext.createOscillator();
            osc.type = 'sine';
            osc.frequency.setValueAtTime(880, now);
            osc.frequency.exponentialRampToValueAtTime(660, now + 0.4);
            osc.connect(gain);
            osc.start(now);
            osc.stop(now + 0.5);
        } catch(e) {}
    }

    function playAscensionChime() {
        if (!soundEnabled || !audioReady || !audioContext) return;
        try {
            const now = audioContext.currentTime;
            const gain = audioContext.createGain();
            gain.gain.setValueAtTime(0.3, now);
            gain.gain.exponentialRampToValueAtTime(0.0001, now + 0.8);
            gain.connect(audioContext.destination);
            const osc = audioContext.createOscillator();
            osc.type = 'sine';
            osc.frequency.setValueAtTime(880, now);
            osc.frequency.exponentialRampToValueAtTime(440, now + 0.6);
            osc.connect(gain);
            osc.start(now);
            osc.stop(now + 0.7);
        } catch(e) {}
    }

    function playEpicSound() {
        if (!soundEnabled || !audioReady || !audioContext) return;
        try {
            const now = audioContext.currentTime;
            const gain = audioContext.createGain();
            gain.gain.setValueAtTime(0.5, now);
            gain.gain.exponentialRampToValueAtTime(0.0001, now + 1.5);
            gain.connect(audioContext.destination);
            const osc = audioContext.createOscillator();
            osc.type = 'sine';
            osc.frequency.setValueAtTime(220, now);
            osc.frequency.exponentialRampToValueAtTime(440, now + 0.8);
            osc.frequency.exponentialRampToValueAtTime(880, now + 1.2);
            osc.connect(gain);
            osc.start(now);
            osc.stop(now + 1.5);
        } catch(e) {}
    }

    function toggleSound() {
        soundEnabled = !soundEnabled;
        document.getElementById("soundToggleBtn").innerHTML = soundEnabled ? "🔊 Sound ON" : "🔇 Sound OFF";
    }
    window.toggleSound = toggleSound;

    // ========== ACHIEVEMENTS ==========
    const achievementsList = [
        { id: "first_click", name: "First Step", desc: "First click", icon: "👆", req: (s) => s.totalClicks >= 1, reward: 100, unlocked: false },
        { id: "clicker_100", name: "Dedicated Clicker", desc: "100 clicks", icon: "🖱️", req: (s) => s.totalClicks >= 100, reward: 500, unlocked: false },
        { id: "clicker_1000", name: "Click Master", desc: "1,000 clicks", icon: "⚡", req: (s) => s.totalClicks >= 1000, reward: 2000, unlocked: false },
        { id: "clicker_10000", name: "Legendary Clicker", desc: "10,000 clicks", icon: "🏆", req: (s) => s.totalClicks >= 10000, reward: 10000, unlocked: false },
        { id: "coconut_hoarder", name: "Coconut Hoarder", desc: "1M coconuts", icon: "💰", req: (s) => s.coconutCount >= 1000000, reward: 50000, unlocked: false },
        { id: "coconut_billionaire", name: "Coconut Billionaire", desc: "1B coconuts", icon: "💎", req: (s) => s.coconutCount >= 1000000000, reward: 5000000, unlocked: false },
        { id: "collector_5", name: "Collector", desc: "5 variants", icon: "📦", req: (s) => s.unlockedVariants.length >= 5, reward: 1000, unlocked: false },
        { id: "collector_10", name: "Master Collector", desc: "10 variants", icon: "🎁", req: (s) => s.unlockedVariants.length >= 10, reward: 5000, unlocked: false },
        { id: "collector_20", name: "Legendary Collector", desc: "20 variants", icon: "👑", req: (s) => s.unlockedVariants.length >= 20, reward: 25000, unlocked: false },
        { id: "collector_all", name: "Completionist", desc: "All variants", icon: "🏆", req: (s) => s.unlockedVariants.length >= 29, reward: 1000000, unlocked: false },
        { id: "dragon_finder", name: "Dragon Tamer", desc: "Buy Dragon Coconut", icon: "🐉", req: (s) => s.unlockedVariants.includes("Dragon Coconut"), reward: 5000000, unlocked: false },
        { id: "upgrade_5", name: "Upgrader", desc: "5 upgrades", icon: "⬆️", req: (s) => s.upgrades.filter(u => u.purchased).length >= 5, reward: 5000, unlocked: false },
        { id: "upgrade_all", name: "Fully Upgraded", desc: "All upgrades", icon: "💪", req: (s) => s.upgrades.every(u => u.purchased), reward: 50000, unlocked: false },
        { id: "ascension_1", name: "Awakened", desc: "Tier 1 ascension", icon: "✨", req: (s) => s.ascensionCount >= 1, reward: 100000, unlocked: false },
        { id: "ascension_3", name: "Coconut Deity", desc: "Tier 3 ascension", icon: "🧘", req: (s) => s.ascensionCount >= 3, reward: 5000000, unlocked: false },
        { id: "ascension_5", name: "Ultimate Coconut", desc: "Tier 5 ascension", icon: "🌟", req: (s) => s.ascensionCount >= 5, reward: 100000000, unlocked: false },
        { id: "merchant_lucky", name: "Lucky Shopper", desc: "Buy from merchant", icon: "🍀", req: (s) => s.merchantPurchases >= 1, reward: 10000, unlocked: false },
        { id: "power_1000", name: "Overpowered", desc: "1000 click power", icon: "⚡", req: (s) => s.clickPower >= 1000, reward: 50000, unlocked: false },
        { id: "power_10000", name: "Godlike Power", desc: "10000 click power", icon: "💥", req: (s) => s.clickPower >= 10000, reward: 500000, unlocked: false },
        { id: "click_million", name: "Million Clicks", desc: "1M clicks", icon: "🎉", req: (s) => s.totalClicks >= 1000000, reward: 10000000, unlocked: false }
    ];

    // ========== GAME DATA ==========
    const allVariants = [
        { name: "Normal Coconut", baseCost: 0, clickBonus: 0, rarity: 0, image: "🥥" },
        { name: "Dirt Coconut", baseCost: 250, clickBonus: 2, rarity: 1, image: "🪨🥥" },
        { name: "Mossy Coconut", baseCost: 500, clickBonus: 3, rarity: 1, image: "🌿🥥" },
        { name: "Cracked Coconut", baseCost: 2000, clickBonus: 5, rarity: 1, image: "🥥💔" },
        { name: "Muddy Coconut", baseCost: 4000, clickBonus: 7, rarity: 1, image: "🌧️🥥" },
        { name: "Sea Coconut", baseCost: 7000, clickBonus: 12, rarity: 1, image: "🌊🥥" },
        { name: "Windblown Coconut", baseCost: 9000, clickBonus: 30, rarity: 1, image: "💨🥥" },
        { name: "Plain Coconut", baseCost: 10000, clickBonus: 50, rarity: 1, image: "🥥" },
        { name: "Gold Coconut", baseCost: 12000, clickBonus: 75, rarity: 2, image: "🥇🥥" },
        { name: "Ice Coconut", baseCost: 15000, clickBonus: 100, rarity: 2, image: "❄️🥥" },
        { name: "Fire Coconut", baseCost: 16000, clickBonus: 125, rarity: 2, image: "🔥🥥" },
        { name: "Rock Coconut", baseCost: 17000, clickBonus: 160, rarity: 2, image: "⛰️🥥" },
        { name: "Storm Coconut", baseCost: 18000, clickBonus: 222, rarity: 2, image: "⛈️🥥" },
        { name: "Shadow Coconut", baseCost: 19000, clickBonus: 367, rarity: 2, image: "🌑🥥" },
        { name: "Lucky Coconut", baseCost: 20000, clickBonus: 500, rarity: 2, image: "🍀🥥" },
        { name: "Diamond Coconut", baseCost: 50000, clickBonus: 650, rarity: 3, image: "💎🥥" },
        { name: "Rainbow Coconut", baseCost: 55000, clickBonus: 750, rarity: 3, image: "🌈🥥" },
        { name: "Lava Coconut", baseCost: 60000, clickBonus: 900, rarity: 3, image: "🌋🥥" },
        { name: "Crystal Coconut", baseCost: 70000, clickBonus: 1100, rarity: 3, image: "🔮🥥" },
        { name: "Phantom Coconut", baseCost: 75000, clickBonus: 1250, rarity: 3, image: "👻🥥" },
        { name: "Cosmic Coconut", baseCost: 80000, clickBonus: 1350, rarity: 3, image: "🌌🥥" },
        { name: "Thunder Coconut", baseCost: 90000, clickBonus: 1500, rarity: 3, image: "⚡🥥" },
        { name: "Alien Coconut", baseCost: 100000, clickBonus: 2000, rarity: 4, image: "👽🥥" },
        { name: "Galactic Coconut", baseCost: 500000, clickBonus: 2500, rarity: 4, image: "🪐🥥" },
        { name: "Celestial Coconut", baseCost: 2000000, clickBonus: 5000, rarity: 4, image: "✨🥥" },
        { name: "Divine Coconut", baseCost: 3000000, clickBonus: 10000, rarity: 4, image: "👼🥥" },
        { name: "Void Coconut", baseCost: 5000000, clickBonus: 15000, rarity: 5, image: "🕳️🥥" },
        { name: "Eternal Coconut", baseCost: 750000, clickBonus: 20000, rarity: 5, image: "♾️🥥" },
        { name: "Dragon Coconut", baseCost: 10000000, clickBonus: 1000000, rarity: 6, image: "🐉🥥" }
    ];

    const allAscensionTiers = [
        { tier: 1, cost: 500_000_000, bonusMultiplier: 2.0, name: "First Awakening" },
        { tier: 2, cost: 2_000_000_000, bonusMultiplier: 3.5, name: "Coconut Master" },
        { tier: 3, cost: 10_000_000_000, bonusMultiplier: 5.0, name: "Coconut Deity" },
        { tier: 4, cost: 50_000_000_000, bonusMultiplier: 10.0, name: "Coconut God" },
        { tier: 5, cost: 100_000_000_000, bonusMultiplier: 25.0, name: "Ultimate Coconut" }
    ];

    let upgrades = [
        { name: "+1 per click", cost: 250, bonus: 1, purchased: false },
        { name: "+5 per click", cost: 5000, bonus: 5, purchased: false },
        { name: "+20 per click", cost: 10000, bonus: 20, purchased: false },
        { name: "+50 per click", cost: 50000, bonus: 50, purchased: false },
        { name: "+75 per click", cost: 75000, bonus: 75, purchased: false },
        { name: "+100 per click", cost: 100000, bonus: 100, purchased: false },
        { name: "+500 per click", cost: 500000, bonus: 500, purchased: false },
        { name: "+1000 per click", cost: 1000000, bonus: 1000, purchased: false },
        { name: "+5000 per click", cost: 5000000, bonus: 5000, purchased: false }
    ];

    // Game State
    let coconutCount = 0, clickPower = 1, totalClicks = 0, unlockedVariants = ["Normal Coconut"];
    let currentVariant = allVariants[0];
    let ascensionCount = 0, ascensionMultiplier = 1.0, secondsElapsed = 0, showMerchant = false, merchantVariants = [];
    let merchantPurchases = 0;
    let achievements = JSON.parse(JSON.stringify(achievementsList));
    let isAnimating = false;
    
    // ========== PARTICLE SYSTEM ==========
    let coconutParticles = [];
    let particleId = 0;
    const MAX_PARTICLES = 20;

    function spawnCoconutFromTree(side) {
        let startX, startY = 60;
        let directionX, directionY;
        if (side === 'left') {
            startX = 70 + (Math.random() - 0.5) * 40;
            directionX = Math.random() * 15 + 10;
            directionY = -Math.random() * 15 - 5;
        } else {
            startX = window.innerWidth - 100 + (Math.random() - 0.5) * 40;
            directionX = -Math.random() * 15 - 10;
            directionY = -Math.random() * 15 - 5;
        }
        coconutParticles.push({
            id: particleId++,
            x: startX, y: startY,
            vx: directionX, vy: directionY,
            rotation: Math.random() * 360,
            scale: 0.7 + Math.random() * 0.8,
            opacity: 0.9, life: 1
        });
    }

    function spawnCoconutsOnClick(count) {
        for (let i = 0; i < Math.min(count, MAX_PARTICLES - coconutParticles.length); i++) {
            spawnCoconutFromTree(i % 2 === 0 ? 'left' : 'right');
        }
    }

    function updateParticles() {
        for (let i = 0; i < coconutParticles.length; i++) {
            const p = coconutParticles[i];
            p.x += p.vx;
            p.y += p.vy;
            p.vy += 0.5;
            p.rotation += 12;
            p.life -= 0.02;
            p.opacity = p.life * 0.9;
            if (p.y > window.innerHeight + 100 || p.x < -100 || p.x > window.innerWidth + 100 || p.life <= 0) {
                coconutParticles.splice(i, 1);
                i--;
            }
        }
        let html = '';
        for (let p of coconutParticles) {
            html += `<div class="falling-coconut" style="transform: translate(${p.x}px, ${p.y}px) rotate(${p.rotation}deg) scale(${p.scale}); opacity: ${p.opacity};">🥥</div>`;
        }
        document.getElementById("particleLayer").innerHTML = html;
        requestAnimationFrame(updateParticles);
    }
    updateParticles();

    setInterval(() => {
        if (!isAnimating && coconutParticles.length < MAX_PARTICLES) {
            spawnCoconutFromTree(Math.random() > 0.5 ? 'left' : 'right');
        }
    }, 2000);

    // Helper Functions
    function getTotalPerClick() { return Math.floor((clickPower + currentVariant.clickBonus) * ascensionMultiplier); }

    function showAchievementToast(achievement) {
        const toast = document.getElementById("achievementToast");
        toast.innerHTML = `<span>${achievement.icon}</span> <div><strong>${achievement.name}</strong><br>+${achievement.reward.toLocaleString()} 🥥</div>`;
        toast.classList.add("show");
        playAchievementSound();
        setTimeout(() => toast.classList.remove("show"), 3000);
    }

    function checkAchievements() {
        let anyUnlocked = false;
        for (let ach of achievements) {
            if (!ach.unlocked) {
                let reqMet = false;
                if (ach.id === "first_click") reqMet = totalClicks >= 1;
                else if (ach.id === "clicker_100") reqMet = totalClicks >= 100;
                else if (ach.id === "clicker_1000") reqMet = totalClicks >= 1000;
                else if (ach.id === "clicker_10000") reqMet = totalClicks >= 10000;
                else if (ach.id === "coconut_hoarder") reqMet = coconutCount >= 1000000;
                else if (ach.id === "coconut_billionaire") reqMet = coconutCount >= 1000000000;
                else if (ach.id === "collector_5") reqMet = unlockedVariants.length >= 5;
                else if (ach.id === "collector_10") reqMet = unlockedVariants.length >= 10;
                else if (ach.id === "collector_20") reqMet = unlockedVariants.length >= 20;
                else if (ach.id === "collector_all") reqMet = unlockedVariants.length >= 29;
                else if (ach.id === "dragon_finder") reqMet = unlockedVariants.includes("Dragon Coconut");
                else if (ach.id === "upgrade_5") reqMet = upgrades.filter(u => u.purchased).length >= 5;
                else if (ach.id === "upgrade_all") reqMet = upgrades.every(u => u.purchased);
                else if (ach.id === "ascension_1") reqMet = ascensionCount >= 1;
                else if (ach.id === "ascension_3") reqMet = ascensionCount >= 3;
                else if (ach.id === "ascension_5") reqMet = ascensionCount >= 5;
                else if (ach.id === "merchant_lucky") reqMet = merchantPurchases >= 1;
                else if (ach.id === "power_1000") reqMet = clickPower >= 1000;
                else if (ach.id === "power_10000") reqMet = clickPower >= 10000;
                else if (ach.id === "click_million") reqMet = totalClicks >= 1000000;
                
                if (reqMet) {
                    ach.unlocked = true;
                    anyUnlocked = true;
                    coconutCount += ach.reward;
                    showAchievementToast(ach);
                }
            }
        }
        if (anyUnlocked) {
            renderAchievements();
            updateUI();
        }
    }

    function renderAchievements() {
        const container = document.getElementById("achievementsGrid");
        const unlockedCount = achievements.filter(a => a.unlocked).length;
        document.getElementById("achievementCount").innerText = `${unlockedCount}/${achievements.length}`;
        container.innerHTML = achievements.map(ach => `
            <div class="achievement-card ${ach.unlocked ? 'unlocked' : 'locked'}">
                <div class="achievement-icon">${ach.icon}</div>
                <div class="achievement-name">${ach.name}</div>
                <div class="achievement-desc">${ach.desc}</div>
                <div class="achievement-reward">+${ach.reward.toLocaleString()} 🥥</div>
                ${!ach.unlocked ? '<div style="font-size:0.6rem; color:#999;">🔒 Locked</div>' : '<div style="font-size:0.6rem; color:green;">✓ Unlocked!</div>'}
            </div>
        `).join('');
    }

    function updateUI() {
        document.getElementById("coconutDisplay").innerText = Math.floor(coconutCount).toLocaleString();
        document.getElementById("clickPowerVal").innerText = clickPower;
        document.getElementById("totalPerClick").innerText = getTotalPerClick();
        document.getElementById("totalClicks").innerText = totalClicks.toLocaleString();
        document.getElementById("equippedName").innerText = currentVariant.name;
        const ascDiv = document.getElementById("ascensionInfo");
        if (ascensionCount > 0) { ascDiv.style.display = "inline-flex"; ascDiv.innerHTML = `✨ Ascension Tier ${ascensionCount} · ${ascensionMultiplier.toFixed(1)}x`; } else ascDiv.style.display = "none";
        const nextTier = ascensionCount < allAscensionTiers.length ? allAscensionTiers[ascensionCount] : null;
        const hintDiv = document.getElementById("nextTierHint");
        if (nextTier) { hintDiv.innerHTML = `🌟 Next: ${nextTier.name} (${nextTier.bonusMultiplier}x) - Need ${nextTier.cost.toLocaleString()} coconuts`; hintDiv.style.color = coconutCount >= nextTier.cost ? "#4caf50" : "#a5673f"; } else { hintDiv.innerHTML = "🏆 MAX ASCENSION REACHED!"; }
        renderUpgrades(); 
        renderMerchantStock();
    }

    function renderUpgrades() { 
        const container = document.getElementById("upgradesList"); 
        container.innerHTML = ""; 
        upgrades.forEach((upg, idx) => { 
            const div = document.createElement("div"); 
            div.className = "upgrade-item"; 
            div.innerHTML = `<div><strong>${upg.name}</strong></div><div>🥥 ${upg.cost.toLocaleString()}</div><div>✨ +${upg.bonus}</div>${!upg.purchased ? `<button class="buy-btn" data-upgrade="${idx}">BUY</button>` : `<button disabled>✔ Purchased</button>`}`; 
            container.appendChild(div); 
            if (!upg.purchased) { const btn = div.querySelector(`[data-upgrade="${idx}"]`); if (btn) btn.addEventListener("click", () => buyUpgrade(idx)); } 
        }); 
    }
    
    function buyUpgrade(idx) { let upg = upgrades[idx]; if (!upg.purchased && coconutCount >= upg.cost) { coconutCount -= upg.cost; clickPower += upg.bonus; upg.purchased = true; updateUI(); spawnCoconutsOnClick(8); checkAchievements(); } }
    
    function generateMerchantStock() { 
        let available = allVariants.filter(v => !unlockedVariants.includes(v.name) && v.name !== "Normal Coconut"); 
        let stock = []; 
        for (let r = 1; r <= 5; r++) { 
            let variantsRarity = available.filter(v => v.rarity === r); 
            if (variantsRarity.length) stock.push(variantsRarity[Math.floor(Math.random() * variantsRarity.length)]); 
        } 
        let dragon = available.find(v => v.name === "Dragon Coconut"); 
        if (dragon && Math.random() < 0.05) stock.push(dragon); 
        return stock.slice(0, 5); 
    }
    
    function renderMerchantStock() { 
        const stockArea = document.getElementById("merchantStockArea"); 
        const emptyMsg = document.getElementById("merchantEmptyMsg"); 
        const listDiv = document.getElementById("merchantStockList"); 
        if (showMerchant && merchantVariants.length) { 
            stockArea.style.display = "block"; emptyMsg.style.display = "none"; listDiv.innerHTML = ""; 
            merchantVariants.forEach(v => { 
                let card = document.createElement("div"); card.className = "variant-card"; 
                card.innerHTML = `<div style="font-size:2rem;">${v.image}</div><div><strong>${v.name}</strong></div><div>🥥 ${v.baseCost.toLocaleString()}</div><div style="color:green;">+${v.clickBonus}</div><button class="buy-btn" data-variant="${v.name}">Buy</button>`; 
                listDiv.appendChild(card); 
                let btn = card.querySelector(`[data-variant="${v.name}"]`); 
                if (btn) { 
                    if (coconutCount >= v.baseCost) btn.addEventListener("click", () => buyVariant(v)); 
                    else { btn.disabled = true; btn.style.background = "#aaa"; } 
                } 
            }); 
        } else { stockArea.style.display = "none"; emptyMsg.style.display = "block"; } 
    }
    
    function buyVariant(v) { 
        if (coconutCount >= v.baseCost && !unlockedVariants.includes(v.name)) { 
            coconutCount -= v.baseCost; 
            unlockedVariants.push(v.name); 
            currentVariant = v; 
            merchantVariants = merchantVariants.filter(vv => vv.name !== v.name); 
            merchantPurchases++; 
            updateUI(); 
            spawnCoconutsOnClick(10); 
            checkAchievements(); 
        } 
    }

    function handleClick() { 
        let gain = getTotalPerClick(); 
        coconutCount += gain; 
        totalClicks += gain; 
        updateUI(); 
        spawnCoconutsOnClick(Math.min(gain / 50 + 5, 12));
        playSoftPop(); 
        checkAchievements();
        const container = document.getElementById("gameContainer"); 
        container.style.transform = `translate(${(Math.random()-0.5)*4}px, ${(Math.random()-0.5)*3}px)`;
        setTimeout(() => container.style.transform = "", 100);
    }

    let timerInterval; 
    function startGameTimer() { 
        timerInterval = setInterval(() => { 
            secondsElapsed++; 
            let cycle = secondsElapsed % 60;
            const fill = document.getElementById("merchantProgressFill");
            const txt = document.getElementById("merchantTimerText");
            if (cycle < 30) {
                showMerchant = false;
                txt.innerText = `🕒 Merchant arrives in ${30 - cycle}s`;
                fill.style.width = `${(cycle / 30) * 100}%`;
                fill.style.background = "#3b82f6";
                if (merchantVariants.length > 0) merchantVariants = [];
            } else {
                showMerchant = true;
                txt.innerText = `🌴 Merchant leaves in ${60 - cycle}s`;
                fill.style.width = `${((60 - cycle) / 30) * 100}%`;
                fill.style.background = "#f97316";
                if (cycle === 30 || (merchantVariants.length === 0 && showMerchant)) merchantVariants = generateMerchantStock();
            }
            renderMerchantStock();
        }, 1000); 
    }

    function playTier1Animation(cb) { isAnimating = true; playAscensionChime(); setTimeout(() => { isAnimating = false; if(cb) cb(); }, 800); }
    function playTier2Animation(cb) { isAnimating = true; playAscensionChime(); setTimeout(() => { isAnimating = false; if(cb) cb(); }, 800); }
    function playTier3Animation(cb) { isAnimating = true; playAscensionChime(); setTimeout(() => { isAnimating = false; if(cb) cb(); }, 800); }
    function playTier4Animation(cb) { isAnimating = true; playAscensionChime(); setTimeout(() => { isAnimating = false; if(cb) cb(); }, 800); }
    function playTier5Animation(cb) { isAnimating = true; playEpicSound(); setTimeout(() => { isAnimating = false; if(cb) cb(); }, 800); }

    function showAscensionModal() { 
        if (isAnimating) return;
        let tiers = ascensionCount < allAscensionTiers.length ? [allAscensionTiers[ascensionCount]] : [];
        let modal = document.getElementById("modalContainer"); 
        if(tiers.length === 0) { 
            modal.innerHTML = `<div class="modal-card"><h2>🏆 MAX ASCENSION</h2><button class="close-btn" id="closeAsc">Close</button></div>`; 
            modal.style.display = "flex"; 
            document.getElementById("closeAsc").onclick = () => modal.style.display = "none"; 
            return; 
        }
        let html = `<div class="modal-card"><h2>🌟 ASCENSION</h2><div>Current Tier: ${ascensionCount} (${ascensionMultiplier.toFixed(1)}x)</div>`;
        tiers.forEach(t => { let available = coconutCount >= t.cost; 
            html += `<div style="border:2px solid ${available ? '#ffaa44' : '#aaa'}; border-radius:24px; margin:16px 0; padding:16px;">
                <div style="font-size:1.3rem; font-weight:bold;">✨ ${t.name}</div>
                <div>➕ ${t.bonusMultiplier}x Multiplier</div>
                <div>🥥 Cost: ${t.cost.toLocaleString()}</div>
                ${available ? `<button class="action-btn" data-tier='${JSON.stringify(t)}' style="background:#d9534f; margin-top:12px;">ASCEND NOW</button>` : `<span style="color:#b33;">🔒 Need ${(t.cost - coconutCount).toLocaleString()} more</span>`}
            </div>`; 
        });
        html += `<button class="close-btn" id="closeAscModal">Close</button></div>`;
        modal.innerHTML = html; 
        modal.style.display = "flex";
        document.querySelectorAll("[data-tier]").forEach(btn => btn.addEventListener("click", () => { 
            let tier = JSON.parse(btn.dataset.tier); 
            if(coconutCount >= tier.cost) { 
                modal.style.display = "none"; 
                if (tier.tier === 1) playTier1Animation(() => completeAscension(tier));
                else if (tier.tier === 2) playTier2Animation(() => completeAscension(tier));
                else if (tier.tier === 3) playTier3Animation(() => completeAscension(tier));
                else if (tier.tier === 4) playTier4Animation(() => completeAscension(tier));
                else if (tier.tier === 5) playTier5Animation(() => completeAscension(tier));
                else completeAscension(tier);
            } 
        }));
        document.getElementById("closeAscModal")?.addEventListener("click", () => modal.style.display = "none");
    }
    
    function completeAscension(tier) {
        let glow = document.getElementById("ascensionGlow"); glow.style.opacity = "1"; 
        let div = document.createElement("div"); div.innerText = tier.name.toUpperCase(); div.style.cssText = "position:fixed; top:40%; left:0; right:0; text-align:center; font-size:3rem; font-weight:bold; color:gold; text-shadow:0 0 12px orange; z-index:300; pointer-events:none;"; document.body.appendChild(div); 
        setTimeout(() => div.remove(), 2500); setTimeout(() => glow.style.opacity = "0", 2500); 
        setTimeout(() => { 
            coconutCount = 0; clickPower = 1; totalClicks = 0; unlockedVariants = ["Normal Coconut"]; 
            currentVariant = allVariants[0]; upgrades.forEach(u => u.purchased = false); 
            ascensionCount = tier.tier; ascensionMultiplier = tier.bonusMultiplier; 
            updateUI(); renderMerchantStock(); checkAchievements(); 
        }, 1500); 
    }

    function exportSave() { 
        let saveObj = { coconutCount, clickPower, unlockedVariants, currentVariant: currentVariant.name, upgrades, ascensionCount, ascensionMultiplier, totalClicks, merchantPurchases, achievements: achievements.map(a => ({ id: a.id, unlocked: a.unlocked })) };
        let save = btoa(JSON.stringify(saveObj)); navigator.clipboard.writeText(save); alert("Save code copied!"); 
    }
    
    function importSave() { 
        let code = prompt("Paste your save code:"); 
        try { 
            let d = JSON.parse(atob(code)); 
            coconutCount = d.coconutCount; clickPower = d.clickPower; unlockedVariants = d.unlockedVariants; 
            currentVariant = allVariants.find(v => v.name === d.currentVariant) || allVariants[0]; 
            upgrades = d.upgrades; ascensionCount = d.ascensionCount; ascensionMultiplier = d.ascensionMultiplier; 
            totalClicks = d.totalClicks; merchantPurchases = d.merchantPurchases || 0;
            if (d.achievements) { d.achievements.forEach(saved => { let ach = achievements.find(a => a.id === saved.id); if (ach) ach.unlocked = saved.unlocked; }); }
            updateUI(); renderMerchantStock(); renderAchievements(); alert("Save loaded!"); 
        } catch(e) { alert("Invalid save code!"); } 
    }
    
    function showCollection() { 
        let modal = document.getElementById("modalContainer"); 
        let html = `<div class="modal-card"><h2>🥥 Coconut Collection</h2><div style="display:grid;grid-template-columns:repeat(auto-fill, minmax(100px,1fr)); gap:10px;">`; 
        allVariants.forEach(v => { 
            let u = unlockedVariants.includes(v.name); 
            html += `<div style="border:2px solid ${u ? '#e7bc7e' : '#aaa'}; border-radius:20px; padding:10px; text-align:center;">
                <div style="font-size:2rem;">${u ? v.image : "❓"}</div>
                <div style="font-size:0.75rem;">${u ? v.name : "???"}</div>
                ${u && currentVariant.name !== v.name ? `<button class="action-btn" data-equip="${v.name}" style="margin-top:6px;padding:4px 8px;">Equip</button>` : currentVariant.name === v.name ? "✔️" : ""}
            </div>`; 
        }); 
        html += `</div><button class="close-btn" id="colClose">Close</button></div>`; 
        modal.innerHTML = html; modal.style.display = "flex"; 
        document.querySelectorAll("[data-equip]").forEach(btn => btn.onclick = () => { 
            let v = allVariants.find(v => v.name === btn.dataset.equip); 
            if (v && unlockedVariants.includes(v.name)) { currentVariant = v; updateUI(); modal.style.display = "none"; } 
        }); 
        document.getElementById("colClose").onclick = () => modal.style.display = "none"; 
    }

    let achievementsVisible = true;
    document.getElementById("toggleAchievementsBtn").onclick = () => {
        achievementsVisible = !achievementsVisible;
        document.getElementById("achievementsGrid").style.display = achievementsVisible ? "grid" : "none";
        document.getElementById("toggleAchievementsBtn").innerHTML = achievementsVisible ? "▲ Hide" : "▼ Show";
    };

    document.getElementById("coconutClickBtn").onclick = handleClick;
    document.getElementById("soundToggleBtn").onclick = toggleSound;
    document.getElementById("enableSoundBtn").onclick = initAndEnableAudio;
    document.getElementById("exportBtn").onclick = exportSave;
    document.getElementById("importBtn").onclick = importSave;
    document.getElementById("collectionBtn").onclick = showCollection;
    document.getElementById("achievementsBtn").onclick = () => { 
        document.getElementById("achievementsGrid").style.display = "grid"; 
        achievementsVisible = true; 
        document.getElementById("toggleAchievementsBtn").innerHTML = "▲ Hide"; 
    };
    document.getElementById("ascendBtn").onclick = showAscensionModal;
    
    renderAchievements();
    updateUI(); 
    startGameTimer();
</script>
</body>
</html>
