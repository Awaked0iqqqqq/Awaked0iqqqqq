
[index.html](https://github.com/user-attachments/files/25663540/index.html)
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ki Clicker - Ultimate Edition</title>
    <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;700;900&family=Rajdhani:wght@500;700;900&display=swap" rel="stylesheet">
    
    <style>
        /* =========================================
           VARIABLES & RESET
        ========================================= */
        :root {
            --dbz-orange: #ff8800;
            --dbz-gold: #ffd700;
            --dbz-blue: #0088ff;
            --shenron-green: #00ffaa;
            --bg-dark: rgba(10, 15, 20, 0.75);
            --bg-light: #ece8e1;
            --aura-color: #ff8800;
            --font-title: 'Nunito', sans-serif; 
            --font-ui: 'Rajdhani', sans-serif;
        }

        * { box-sizing: border-box; user-select: none; margin: 0; padding: 0; }

        body {
            background-color: #000;
            /* background-image retiré ici pour laisser place à la vidéo */
            color: var(--bg-light); font-family: var(--font-ui);
            height: 100vh; overflow: hidden;
            display: grid;
            grid-template-columns: 320px 1fr 380px;
            grid-template-rows: 90px 1fr;
            grid-template-areas: 
                "header header header"
                "sidebar main upgrades";
            position: relative;
        }

        /* STYLE DE LA VIDÉO EN ARRIÈRE-PLAN */
        #bg-video {
            position: fixed;
            right: 0;
            bottom: 0;
            min-width: 100%;
            min-height: 100%;
            width: auto;
            height: auto;
            z-index: -2;
            object-fit: cover;
        }

        .video-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5); /* Assombrit un peu la vidéo pour la lisibilité */
            z-index: -1;
        }

        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: rgba(255, 136, 0, 0.5); border-radius: 10px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--dbz-orange); }

        /* ... (Le reste de ton CSS reste identique) ... */
        header {
            grid-area: header; background: rgba(0, 0, 0, 0.5); backdrop-filter: blur(15px);
            border-bottom: 2px solid var(--dbz-orange);
            display: flex; justify-content: space-between; align-items: center; padding: 0 40px; z-index: 100;
            box-shadow: 0 10px 30px rgba(255, 136, 0, 0.1);
        }

        .logo-container {
            font-family: var(--font-title); font-size: 32px; font-weight: 900;
            color: var(--dbz-orange); text-transform: uppercase;
            text-shadow: 0 0 15px rgba(255, 136, 0, 0.6); letter-spacing: 2px;
        }

        .db-container {
            display: flex; align-items: center; gap: 20px; background: rgba(0,0,0,0.6);
            padding: 12px 25px; border-radius: 30px; border: 1px solid rgba(255, 136, 0, 0.3);
            box-shadow: inset 0 0 15px rgba(0,0,0,0.8);
        }

        .orb-dots { display: flex; gap: 10px; }
        .orb-dot {
            width: 20px; height: 20px; background: rgba(255,255,255,0.1); border-radius: 50%;
            transition: all 0.4s ease; border: 1px solid #333;
        }
        .orb-dot.active { 
            background: radial-gradient(circle, #ffcc00, #ff6600); border-color: #ff9900;
            box-shadow: 0 0 15px #ff9900; transform: scale(1.2); 
        }

        .shenron-btn {
            background: linear-gradient(135deg, var(--shenron-green), #00b377);
            color: #000; border: none; border-radius: 20px; padding: 8px 20px;
            font-family: var(--font-title); font-size: 16px; font-weight: 900;
            cursor: pointer; display: none; transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(0, 255, 170, 0.4); animation: pulseShenron 1.5s infinite;
        }
        @keyframes pulseShenron { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.05); } }

        aside.sidebar {
            grid-area: sidebar; background: var(--bg-dark); backdrop-filter: blur(12px);
            border-right: 1px solid rgba(255, 255, 255, 0.05);
            display: flex; flex-direction: column; padding: 25px; gap: 20px;
        }

        .panel-title {
            font-family: var(--font-title); font-size: 22px; font-weight: 900;
            color: #fff; margin-bottom: 15px; text-transform: uppercase;
            letter-spacing: 2px; text-align: center; border-bottom: 2px solid var(--aura-color);
            padding-bottom: 5px; transition: border-color 0.5s;
        }

        .stats-box {
            background: rgba(0,0,0,0.5); padding: 20px; border-radius: 16px;
            border: 1px solid rgba(255,255,255,0.05); font-size: 16px; box-shadow: 0 4px 15px rgba(0,0,0,0.4);
        }
        .stats-box div { display: flex; justify-content: space-between; margin-bottom: 12px; border-bottom: 1px dashed rgba(255,255,255,0.1); padding-bottom: 4px; font-weight: bold;}
        .stats-box div:last-child { border: none; margin-bottom: 0; padding-bottom: 0;}
        .stats-val { color: var(--dbz-orange); font-family: var(--font-title); font-size: 18px; font-weight: 900;}

        .trophy-room {
            flex: 1 1 0; overflow-y: auto; background: rgba(0,0,0,0.3); padding: 15px; border-radius: 16px; border: 1px solid rgba(255,255,255,0.05);
            display: flex; flex-direction: column; gap: 10px;
        }
        .trophy { padding: 10px; border-radius: 8px; font-weight: bold; text-align: center; transition: all 0.5s ease; font-size: 14px; flex-shrink: 0; }
        .trophy.locked { background: rgba(255,255,255,0.05); color: #555; border: 1px dashed #555; filter: grayscale(100%); }
        .trophy.unlocked { background: rgba(255, 215, 0, 0.1); color: var(--dbz-gold); border: 1px solid var(--dbz-gold); box-shadow: 0 0 10px rgba(255,215,0,0.3); }

        .options-row { display: flex; gap: 10px; margin-top: auto; justify-content: center; flex-wrap: wrap; flex-shrink: 0;}
        .opt-btn { background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); color: #ccc; padding: 8px 15px; border-radius: 20px; cursor: pointer; font-size: 13px; font-weight: bold; transition: all 0.2s;}
        .opt-btn:hover { color: #fff; background: rgba(255,255,255,0.15); border-color: var(--dbz-orange); }

        main {
            grid-area: main; display: flex; flex-direction: column; align-items: center; justify-content: center; position: relative;
        }

        .ki-display { text-align: center; margin-bottom: 20px; z-index: 10; background: rgba(0,0,0,0.6); padding: 20px 50px; border-radius: 30px; backdrop-filter: blur(10px); box-shadow: 0 10px 30px rgba(0,0,0,0.5); border: 1px solid var(--aura-color); transition: border-color 0.5s;}
        .ki-title { font-family: var(--font-title); font-size: 20px; color: #aaa; text-transform: uppercase; font-weight: 900; letter-spacing: 2px;}
        .ki-amount { font-family: var(--font-title); font-size: 80px; font-weight: 900; line-height: 1.1; color: #fff; text-shadow: 0 0 30px var(--aura-color); margin: 10px 0; transition: text-shadow 0.5s;}
        .kps-display { font-size: 22px; color: var(--aura-color); font-weight: bold; font-family: var(--font-title); transition: color 0.5s;}

        .goku-container { position: relative; width: 300px; height: 300px; display: flex; justify-content: center; align-items: center; margin: 30px 0; cursor: pointer; }
        .aura-glow { position: absolute; width: 100%; height: 100%; background: radial-gradient(circle, var(--aura-color) 0%, transparent 65%); animation: pulseAura 3s infinite ease-in-out; border-radius: 50%; pointer-events: none; opacity: 0.4; transition: background 0.5s;}
        @keyframes pulseAura { 0%, 100% { transform: scale(0.9); opacity: 0.3; } 50% { transform: scale(1.2); opacity: 0.6; } }

        #goku-img { width: 250px; height: 250px; object-fit: cover; border-radius: 50%; z-index: 2; border: 4px solid var(--aura-color); transition: all 0.1s cubic-bezier(0.175, 0.885, 0.32, 1.275); filter: drop-shadow(0 0 20px var(--aura-color)); }
        #goku-img:active { transform: scale(0.9); filter: drop-shadow(0 0 10px var(--aura-color)); }

        .rank-name-display { font-family: var(--font-title); font-size: 28px; font-weight: 900; color: #fff; text-transform: uppercase; letter-spacing: 2px; background: linear-gradient(90deg, transparent, rgba(0,0,0,0.8), transparent); padding: 5px 40px; border-radius: 20px; text-shadow: 0 0 10px var(--aura-color); transition: text-shadow 0.5s;}

        .autoclick-panel {
            background: rgba(0,0,0,0.6); border: 1px solid rgba(255,255,255,0.1); border-radius: 15px; padding: 10px 15px; display: flex; flex-direction: column; gap: 10px; margin-top: 20px; z-index: 10; width: 260px; backdrop-filter: blur(5px);
        }
        .ac-top { display: flex; justify-content: space-between; align-items: center; }
        .ac-title { font-family: var(--font-title); font-size: 14px; font-weight: 900; color: #aaa; letter-spacing: 1px; }
        .ac-toggle { background: #333; border: 1px solid #555; color: #fff; padding: 4px 15px; border-radius: 20px; cursor: pointer; font-family: var(--font-ui); font-weight: bold; transition: all 0.3s; }
        .ac-toggle.active { background: var(--shenron-green); border-color: var(--shenron-green); color: #000; box-shadow: 0 0 10px rgba(0,255,170,0.5); }
        .ac-upgrade {
            background: transparent; border: 1px dashed rgba(255,255,255,0.3); color: #ccc; padding: 8px; border-radius: 8px; font-family: var(--font-ui); font-size: 13px; font-weight: bold; cursor: pointer; transition: all 0.2s; text-align: center;
        }
        .ac-upgrade:hover:not(.disabled):not(.maxed) { border-color: var(--dbz-orange); color: var(--dbz-orange); background: rgba(255,136,0,0.1); }
        .ac-upgrade.disabled { opacity: 0.4; cursor: not-allowed; }
        .ac-upgrade.maxed { border-color: var(--dbz-gold); color: var(--dbz-gold); border-style: solid; cursor: default; }

        .rage-btn { margin-top: 20px; background: rgba(255, 0, 0, 0.1); border: 2px solid #ff0000; color: #ff0000; padding: 15px 40px; border-radius: 30px; font-family: var(--font-title); font-size: 20px; font-weight: 900; text-transform: uppercase; cursor: pointer; transition: all 0.3s; display: none; box-shadow: 0 0 15px rgba(255, 0, 0, 0.3); z-index: 10; }
        .rage-btn:hover:not(:disabled) { background: #ff0000; color: #fff; box-shadow: 0 0 30px #ff0000; transform: translateY(-3px); }
        .rage-btn:disabled { border-color: rgba(255,255,255,0.2); color: rgba(255,255,255,0.3); cursor: not-allowed; box-shadow: none; }
        .rage-active #goku-img { animation: shake 0.2s infinite; border-color: #ff0000; filter: drop-shadow(0 0 40px #ff0000); }
        @keyframes shake { 0%, 100% { transform: translate(2px, 2px) scale(1.02); } 50% { transform: translate(-2px, -2px) scale(1.02); } }

        aside.upgrades {
            grid-area: upgrades; background: var(--bg-dark); backdrop-filter: blur(12px); border-left: 1px solid rgba(255, 255, 255, 0.05); padding: 25px; display: flex; flex-direction: column; gap: 15px; overflow-y: auto; min-height: 0;
        }

        .prestige-btn {
            background: linear-gradient(135deg, var(--aura-color), #fff); color: #000; border: none; border-radius: 20px; padding: 15px; font-family: var(--font-title); font-size: 20px; font-weight: 900; text-transform: uppercase; cursor: pointer; transition: all 0.3s ease; text-align: center; box-shadow: 0 5px 20px rgba(255, 255, 255, 0.3); flex-shrink: 0; margin-bottom: 10px; margin-top: 0;
        }
        .prestige-btn:hover:not(:disabled) { transform: translateY(-3px); box-shadow: 0 10px 30px var(--aura-color); }
        .prestige-btn:disabled { background: rgba(255,255,255,0.1); color: #666; cursor: not-allowed; box-shadow: none; }

        .upgrade-card {
            background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 16px; padding: 15px; cursor: pointer; transition: all 0.3s ease; position: relative; overflow: hidden; flex-shrink: 0;
        }
        .upgrade-card:hover:not(.disabled) { background: rgba(255, 136, 0, 0.15); border-color: var(--dbz-orange); transform: translateX(-5px); box-shadow: 0 8px 20px rgba(0,0,0,0.5); }
        .upgrade-card.disabled { opacity: 0.4; cursor: not-allowed; filter: grayscale(100%); }
        
        .upg-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 5px; }
        .upg-name { font-weight: 900; font-size: 16px; color: #fff; font-family: var(--font-title); }
        .upg-lvl { background: rgba(255,255,255,0.15); color: #fff; padding: 2px 8px; border-radius: 12px; font-size: 11px; font-weight: bold;}
        .upg-desc { font-size: 13px; color: #ccc; margin-bottom: 8px; font-weight: bold;}
        .upg-cost { font-family: var(--font-title); font-size: 20px; color: var(--dbz-orange); font-weight: 900; }

        .float-text { position: fixed; color: #fff; font-family: var(--font-title); font-size: 26px; font-weight: 900; pointer-events: none; animation: floatUp 0.8s ease-out forwards; text-shadow: 0 3px 10px var(--aura-color); z-index: 1000; }
        @keyframes floatUp { 0% { opacity: 1; transform: translateY(0) scale(1); } 100% { opacity: 0; transform: translateY(-80px) scale(1.3); } }

        .dragon-ball-collect { position: fixed; width: 60px; height: 60px; cursor: pointer; z-index: 5000; filter: drop-shadow(0 0 15px #ff9900); animation: floatOrb 2s infinite ease-in-out; border-radius: 50%; object-fit: cover; box-shadow: inset -5px -5px 15px rgba(0,0,0,0.5); }
        @keyframes floatOrb { 0%, 100% { transform: translateY(0) scale(1); } 50% { transform: translateY(-20px) scale(1.1); } }

        .overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.9); backdrop-filter: blur(20px); z-index: 9999; display: none; flex-direction: column; align-items: center; justify-content: center; text-align: center; }
        .overlay-title { font-family: var(--font-title); font-size: 50px; font-weight: 900; color: var(--shenron-green); text-transform: uppercase; text-shadow: 0 0 30px rgba(0,255,170,0.5); margin-bottom: 10px; }
        
        .wish-container { display: flex; gap: 25px; margin-top: 40px; }
        .wish-card { background: rgba(20,20,20,0.8); border: 2px solid var(--shenron-green); padding: 30px 20px; border-radius: 15px; cursor: pointer; transition: all 0.3s; width: 220px; box-shadow: 0 0 15px rgba(0,255,170,0.1);}
        .wish-card:hover:not(.wish-max) { transform: translateY(-10px); background: rgba(0,255,170,0.1); box-shadow: 0 15px 30px rgba(0,255,170,0.3); }
        .wish-card h3 { font-family: var(--font-title); font-size: 22px; font-weight: 900; margin-bottom: 15px; color: #fff;}
        .wish-card p { font-size: 14px; color: #ccc; font-weight: bold;}
        .wish-max { opacity: 0.4; cursor: default; border-color: #555; filter: grayscale(100%);}
        
        .close-btn { position: absolute; top: 40px; right: 50px; font-size: 50px; color: #fff; cursor: pointer; background: none; border: none; font-family: var(--font-title); transition: color 0.2s; font-weight: bold;}
        .close-btn:hover { color: var(--dbz-orange); }

        #shenron-img { width: 300px; filter: drop-shadow(0 0 30px var(--shenron-green)); margin-bottom: 20px; animation: pulseShenron 3s infinite;}

        #cheat-menu {
            position: fixed; top: 20%; left: 20%; width: 320px; background: rgba(15, 20, 25, 0.95); border: 2px solid var(--dbz-orange); border-radius: 12px; z-index: 10000; display: none; flex-direction: column; box-shadow: 0 15px 40px rgba(0,0,0,0.8), 0 0 20px rgba(255,136,0,0.4); font-family: var(--font-ui); color: white; backdrop-filter: blur(10px);
        }
        #cheat-header {
            background: linear-gradient(90deg, var(--dbz-orange), #ff4400); color: #000; padding: 12px 15px; font-weight: 900; font-family: var(--font-title); cursor: grab; border-top-left-radius: 10px; border-top-right-radius: 10px; display: flex; justify-content: space-between; align-items: center;
        }
        #cheat-header:active { cursor: grabbing; }
        .cheat-body { padding: 20px; display: flex; flex-direction: column; gap: 12px; }
        .cheat-btn {
            background: rgba(255,255,255,0.05); color: #fff; border: 1px solid var(--dbz-orange); padding: 10px; cursor: pointer; border-radius: 8px; font-weight: bold; font-family: var(--font-ui); transition: all 0.2s; display: flex; justify-content: space-between;
        }
        .cheat-btn:hover { background: var(--dbz-orange); color: #000; transform: scale(1.02); }
    </style>
</head>
<body>

    <video autoplay muted loop playsinline id="bg-video">
        <source src="ton-video.mp4" type="video/mp4">
    </video>
    <div class="video-overlay"></div>

    <header>
        <div class="logo-container">KI CLICKER <span style="color:#fff; font-size: 18px; font-weight: 700;">// ULTIMATE</span></div>
        <div class="db-container">
            <span style="font-family: var(--font-title); font-size: 20px; font-weight: 900; color: #aaa;">DRAGON BALLS</span>
            <div class="orb-dots" id="db-dots">
                <div class="orb-dot"></div><div class="orb-dot"></div><div class="orb-dot"></div>
                <div class="orb-dot"></div><div class="orb-dot"></div><div class="orb-dot"></div><div class="orb-dot"></div>
            </div>
            <button class="shenron-btn" id="summon-btn" onclick="openShenron()">🐉 INVOQUER SHENRON</button>
        </div>
    </header>

    <aside class="sidebar">
        <div>
            <div class="panel-title">STATISTIQUES GLOBALES</div>
            <div class="stats-box">
                <div><span>Ki Total à Vie</span> <span class="stats-val" id="stat-total-ki">0</span></div>
                <div><span>Clics</span> <span class="stats-val" id="stat-clicks">0</span></div>
                <div><span>Multiplicateur Global</span> <span class="stats-val" style="color:var(--shenron-green);" id="stat-mult">x1</span></div>
                <div><span>Puissance Rage</span> <span class="stats-val" style="color:#ff4444;" id="stat-rage-power">x10</span></div>
            </div>
        </div>
        
        <div class="panel-title" style="margin-bottom: 5px;">SALLE DES TROPHÉES</div>
        <div class="trophy-room" id="trophy-room">
            <div class="trophy locked" id="trophy-1">🏆 10K Ki Total amassé</div>
            <div class="trophy locked" id="trophy-ki-1m">🏆 1M Ki Total amassé</div>
            <div class="trophy locked" id="trophy-ki-1b">🏆 1B Ki Total amassé</div>
            <div class="trophy locked" id="trophy-ki-1t">🏆 1T Ki Total amassé</div>
            <div class="trophy locked" id="trophy-2">👆 1 000 Clics manuels</div>
            <div class="trophy locked" id="trophy-click-10k">👆 10 000 Clics manuels</div>
            <div class="trophy locked" id="trophy-click-50k">👆 50 000 Clics manuels</div>
            <div class="trophy locked" id="trophy-3">🐉 Vœu exaucé par Shenron</div>
            <div class="trophy locked" id="trophy-shenron-all">🐉 Tous les vœux exaucés</div>
            <div class="trophy locked" id="trophy-rage-unlock">🔥 Mode Rage Débloqué</div>
            <div class="trophy locked" id="trophy-ac-max">🖱️ Auto-clic Niveau MAX</div>
            <div class="trophy locked" id="trophy-4">⚡ Atteindre le Super Sayan</div>
            <div class="trophy locked" id="trophy-god">⚡ Atteindre le Sayan Divin</div>
            <div class="trophy locked" id="trophy-ui">⚡ Maîtriser l'Ultra Instinct</div>
        </div>
        
        <div class="options-row">
            <button class="opt-btn" onclick="toggleAudio()">Musique : <span id="audio-status">OFF</span></button>
            <button class="opt-btn" onclick="hardReset()">Réinitialiser</button>
        </div>
    </aside>

    <main id="main-game">
        <div class="ki-display">
            <div class="ki-title">Puissance Actuelle</div>
            <div class="ki-amount" id="ki-amount">0</div>
            <div class="kps-display">ÉNERGIE : + <span id="kps-amount">0</span> / sec</div>
            <div style="font-size: 14px; color: #aaa; margin-top: 10px; font-weight: bold;">FORCE DE CLIC : + <span id="kpc-amount">1</span></div>
        </div>

        <div class="goku-container" id="goku-main" onclick="clickKi(event)">
            <div class="aura-glow"></div>
            <img id="goku-img" src="goku0.png" onerror="this.src='https://via.placeholder.com/250/000/fff?text=Goku+Image'" alt="Guerrier">
        </div>
        
        <div class="rank-name-display" id="rank-name">Forme de Base</div>
        
        <div class="autoclick-panel">
            <div class="ac-top">
                <span class="ac-title">🖱️ AUTO-CLIC</span>
                <button class="ac-toggle" id="auto-click-btn" onclick="toggleAutoClicker()">OFF</button>
            </div>
            <button class="ac-upgrade" id="auto-click-upg-btn" onclick="upgradeAutoClicker()">
                Améliorer (Niv <span id="ac-level">0</span>/5) — <span id="ac-cost">1000</span> Ki
            </button>
        </div>

        <button class="rage-btn" id="rage-btn" onclick="activateRage()">🔥 MODE RAGE 🔥</button>
    </main>

    <aside class="upgrades" id="upgrades-container">
        
        <button class="prestige-btn" id="prestige-btn" onclick="document.getElementById('eveil-menu').style.display='flex'">⚡ ÉVEIL SPIRITUEL</button>

        <div class="panel-title" style="margin-bottom:0; margin-top:10px; border: none; flex-shrink: 0;">ENTRAÎNEMENT & TECHNIQUES</div>
        
        <div class="upgrade-card" id="upg-card-c" onclick="buyUpg('c')">
            <div class="upg-header"><span class="upg-name">ARTS MARTIAUX</span><span class="upg-lvl" id="count-c">0/100</span></div>
            <div class="upg-desc">+1 Ki / Clic</div>
            <div class="upg-cost"><span id="cost-c">10</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-s" onclick="buyUpg('s')">
            <div class="upg-header"><span class="upg-name">LIVRAISON DE LAIT</span><span class="upg-lvl" id="count-s">0/20</span></div>
            <div class="upg-desc">+1 Ki / Sec</div>
            <div class="upg-cost"><span id="cost-s">50</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-m" onclick="buyUpg('m')">
            <div class="upg-header"><span class="upg-name">TOUR KARINE</span><span class="upg-lvl" id="count-m">0/15</span></div>
            <div class="upg-desc">+10 Ki / Sec</div>
            <div class="upg-cost"><span id="cost-m">500</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-st" onclick="buyUpg('st')">
            <div class="upg-header"><span class="upg-name" style="color:#00e5ff;">SALLE DE L'ESPRIT</span><span class="upg-lvl" id="count-st">0/10</span></div>
            <div class="upg-desc">+100 Ki / Sec</div>
            <div class="upg-cost"><span id="cost-st">15,000</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-g" onclick="buyUpg('g')">
            <div class="upg-header"><span class="upg-name" style="color:#ff4655;">MACHINE À GRAVITÉ</span><span class="upg-lvl" id="count-g">0/10</span></div>
            <div class="upg-desc">+50 Ki / Clic</div>
            <div class="upg-cost"><span id="cost-g">50,000</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-k" onclick="buyUpg('k')">
            <div class="upg-header"><span class="upg-name" style="color:#ffd700;">MAÎTRISE DU KIKOHA</span><span class="upg-lvl" id="count-k">0/5</span></div>
            <div class="upg-desc">+500 Ki/s & +200/Clic</div>
            <div class="upg-cost"><span id="cost-k">500,000</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-kio" onclick="buyUpg('kio')">
            <div class="upg-header"><span class="upg-name" style="color:#ff3333;">TECHNIQUE DE KAIO</span><span class="upg-lvl" id="count-kio">0/10</span></div>
            <div class="upg-desc">+2,500 Ki/s & +1,000/Clic</div>
            <div class="upg-cost"><span id="cost-kio">2.5M</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-ss" onclick="buyUpg('ss')">
            <div class="upg-header"><span class="upg-name" style="color:#ffea00;">CELLULES SAIYAN</span><span class="upg-lvl" id="count-ss">0/10</span></div>
            <div class="upg-desc">+15,000 Ki/s</div>
            <div class="upg-cost"><span id="cost-ss">15.0M</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-pot" onclick="buyUpg('pot')">
            <div class="upg-header"><span class="upg-name" style="color:#ffcc00;">BOUCLES POTARAS</span><span class="upg-lvl" id="count-pot">0/5</span></div>
            <div class="upg-desc">+100,000 Ki/s & +50,000/Clic</div>
            <div class="upg-cost"><span id="cost-pot">100.0M</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-god" onclick="buyUpg('god')">
            <div class="upg-header"><span class="upg-name" style="color:#ff0055;">RITUEL DIVIN</span><span class="upg-lvl" id="count-god">0/10</span></div>
            <div class="upg-desc">+500,000 Ki/s</div>
            <div class="upg-cost"><span id="cost-god">750.0M</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-hak" onclick="buyUpg('hak')">
            <div class="upg-header"><span class="upg-name" style="color:#9900ff;">ÉNERGIE HAKAI</span><span class="upg-lvl" id="count-hak">0/10</span></div>
            <div class="upg-desc">+2,500,000 Ki / Clic</div>
            <div class="upg-cost"><span id="cost-hak">5.0B</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-ui" onclick="buyUpg('ui')">
            <div class="upg-header"><span class="upg-name" style="color:#silver;">MOUVEMENT INCONSCIENT</span><span class="upg-lvl" id="count-ui">0/5</span></div>
            <div class="upg-desc">+20,000,000 Ki/s</div>
            <div class="upg-cost"><span id="cost-ui">50.0B</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-ue" onclick="buyUpg('ue')">
            <div class="upg-header"><span class="upg-name" style="color:#8a2be2;">INSTINCT DE CONQUÊTE</span><span class="upg-lvl" id="count-ue">0/5</span></div>
            <div class="upg-desc">+100,000,000 Ki / Clic</div>
            <div class="upg-cost"><span id="cost-ue">250.0B</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-zen" onclick="buyUpg('zen')">
            <div class="upg-header"><span class="upg-name" style="color:#00ffff;">ENSEIGNEMENT DES ANGES</span><span class="upg-lvl" id="count-zen">0/5</span></div>
            <div class="upg-desc">+500,000,000 Ki/s</div>
            <div class="upg-cost"><span id="cost-zen">1.5T</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-sdb" onclick="buyUpg('sdb')">
            <div class="upg-header"><span class="upg-name" style="color:#ffd700;">ÉNERGIE MULTIVERS</span><span class="upg-lvl" id="count-sdb">0/3</span></div>
            <div class="upg-desc">+5B Ki/s & +2B/Clic</div>
            <div class="upg-cost"><span id="cost-sdb">10.0T</span> Ki</div>
        </div>

        <div class="upgrade-card" id="upg-card-omni" onclick="buyUpg('omni')">
            <div class="upg-header"><span class="upg-name" style="color:#ffffff; text-shadow: 0 0 10px #fff;">PUISSANCE OMNIPOTENTE</span><span class="upg-lvl" id="count-omni">0/1</span></div>
            <div class="upg-desc">+50,000,000,000 Ki/s</div>
            <div class="upg-cost"><span id="cost-omni">100.0T</span> Ki</div>
        </div>
    </aside>

    <div id="cheat-menu">
        <div id="cheat-header">
            <span>💻 TERMINAL DES DIEUX</span>
            <span style="cursor:pointer;" onclick="document.getElementById('cheat-menu').style.display='none'">✖</span>
        </div>
        <div class="cheat-body">
            <button class="cheat-btn" onclick="adminCheatKi()"><span>💰 Ajouter +1M Ki</span> <span>[Inject]</span></button>
            <button class="cheat-btn" onclick="adminCheatPrestige()"><span>⚡ Forcer un Éveil</span> <span>[Rank Up]</span></button>
            <button class="cheat-btn" onclick="adminCheatDB()"><span>🐉 Max Dragon Balls</span> <span>[Spawn]</span></button>
            <button class="cheat-btn" onclick="adminCheatMult()"><span>🔥 Multiplicateur +10</span> <span>[Boost]</span></button>
        </div>
    </div>

    <div class="overlay" id="shenron-menu">
        <button class="close-btn" onclick="document.getElementById('shenron-menu').style.display='none'">×</button>
        <img src="shenron.png" id="shenron-img" onerror="this.src='https://via.placeholder.com/300/2ecc71/000?text=Shenron'" alt="Shenron">
        <div class="overlay-title">JE SUIS SHENRON...</div>
        <p style="font-size: 22px; color: #ccc; font-family: var(--font-title); font-weight: bold;">DIS-MOI TON SOUHAIT ET JE L'EXAUCERAI.</p>
        <div class="wish-container" id="wish-container"></div>
    </div>

    <div class="overlay" id="eveil-menu">
        <button class="close-btn" onclick="document.getElementById('eveil-menu').style.display='none'">×</button>
        <div class="overlay-title" style="color: var(--aura-color); text-shadow: 0 0 30px var(--aura-color);">ÉVEIL SPIRITUEL</div>
        <p style="font-size: 20px; text-align: center; max-width: 550px; margin-bottom: 30px; color: #ddd; font-weight: bold;">
            Atteignez un nouveau stade de puissance. Vos Ki et entraînements seront réinitialisés, mais votre <b>multiplicateur global sera triplé (x3) définitivement</b> !
        </p>
        <div style="font-size: 22px; margin-bottom: 30px; font-weight: bold;">Coût de l'Éveil : <span id="prestige-cost-display" style="color: var(--dbz-orange); font-family: var(--font-title); font-size: 50px; font-weight: 900; display: block; text-align: center; margin-top: 10px;"></span></div>
        <button class="prestige-btn" style="width: 350px; background: var(--aura-color); color:#fff; margin-bottom:0;" onclick="doPrestige()">ACCOMPLIR L'ÉVEIL</button>
    </div>

    <audio id="bg-music" loop><source src="musique.mp3" type="audio/mpeg"></audio>

    <script>
        // Le script JS est exactement le même qu'avant
        // (Variables, fonctions save/load, clickKi, upgrades, etc.)
        let ki = 0, totalKi = 0, totalClicks = 0;
        let kpc = 1, kps = 0, mult = 1, rIdx = 0;
        
        let costC = 10, costS = 50, costM = 500, costST = 15000, costG = 50000, costK = 500000;
        let countC = 0, countS = 0, countM = 0, countST = 0, countG = 0, countK = 0;
        
        let costKio = 2500000, costSs = 15000000, costPot = 100000000, costGod = 750000000, costHak = 5000000000;
        let costUi = 50000000000, costUe = 250000000000, costZen = 1500000000000, costSdb = 10000000000000, costOmni = 100000000000000;
        let countKio = 0, countSs = 0, countPot = 0, countGod = 0, countHak = 0;
        let countUi = 0, countUe = 0, countZen = 0, countSdb = 0, countOmni = 0;

        let dbCount = 0, dbBoostMult = 1;
        let isRage = false, rageMult = 1, rageBasePower = 10;
        let wishMultiUsed = false, wishKiUsed = false;
        
        let autoClickerActive = false;
        let autoClickInterval = null;
        let autoClickerLevel = 0;
        let autoClickerCost = 1000;
        const autoClickIntervals = [500, 333, 250, 166, 125, 100]; 

        const pCosts = [1e3, 1e4, 1e5, 1e6, 1e7, 1e8, 1e9, 1e10, 1e11, 1e12, 1e13, 1e14, 1e15, 1e16, 1e17, 1e18, 1e19, 1e20, 1e21, 1e22, 1e23];
        const transfoNames = ["Forme de Base", "Dépassement", "Super Sayan", "Super Sayan 2", "Super Sayan 3", "Sayan Divin", "Gold Great Ape", "Super Sayan Rosé", "Puissance Mystique", "Sayan Primal", "Évolution Bestiale", "Ultra Instinct Signe", "Ultra Instinct Maîtrisé", "Ultra Ego", "Aura de Destruction", "Lumière Angélique", "Maître Dimensionnel", "Gardien du Multivers", "Énergie Créatrice", "Divinité Suprême", "Guerrier Omnipotent"];
        const colors = ["#ff9900", "#00ff00", "#0000ff", "#32cd32", "#00ffff", "#ff0000", "#ffffff", "#ff00ff", "#8a2be2", "#ff1493", "#ff4500", "#ffd700", "#adff2f", "#7fffd4", "#00fa9a", "#1e90ff", "#ff69b4", "#9400d3", "#00ced1", "#ffb6c1", "#e0ffff"];

        const audio = document.getElementById('bg-music'); let audioEnabled = false;
        function toggleAudio() { if(!audioEnabled) { audio.play(); audioEnabled = true; document.getElementById('audio-status').innerText = "ON"; } else { audio.pause(); audioEnabled = false; document.getElementById('audio-status').innerText = "OFF"; } }

        function formatNumber(num) { 
            if(num >= 1e12) return (num / 1e12).toFixed(2) + "T";
            if(num >= 1e9) return (num / 1e9).toFixed(2) + "B"; 
            if(num >= 1e6) return (num / 1e6).toFixed(2) + "M"; 
            if(num >= 1e3) return (num / 1e3).toFixed(1) + "K"; 
            return Math.floor(num).toLocaleString(); 
        }
        function getActualGain(base) { return Math.floor(base * mult * rageMult * dbBoostMult); }

        function saveGame() {
            const data = { 
                ki, totalKi, totalClicks, kpc, kps, mult, rIdx, 
                costC, costS, costM, costST, costG, costK, 
                countC, countS, countM, countST, countG, countK, 
                costKio, costSs, costPot, costGod, costHak, costUi, costUe, costZen, costSdb, costOmni,
                countKio, countSs, countPot, countGod, countHak, countUi, countUe, countZen, countSdb, countOmni,
                dbCount, rageBasePower, wishMultiUsed, wishKiUsed,
                autoClickerLevel, autoClickerCost
            };
            localStorage.setItem('valDBZSave', JSON.stringify(data));
        }

        function loadGame() {
            const saved = localStorage.getItem('valDBZSave');
            if(saved) {
                const d = JSON.parse(saved);
                ki = d.ki||0; totalKi = d.totalKi||0; totalClicks = d.totalClicks||0;
                kpc = d.kpc||1; kps = d.kps||0; mult = d.mult||1; rIdx = d.rIdx||0;
                costC = d.costC||10; costS = d.costS||50; costM = d.costM||500;
                costST = d.costST||15000; costG = d.costG||50000; costK = d.costK||500000;
                countC = d.countC||0; countS = d.countS||0; countM = d.countM||0;
                countST = d.countST||0; countG = d.countG||0; countK = d.countK||0;
                costKio = d.costKio||2500000; costSs = d.costSs||15000000; costPot = d.costPot||100000000;
                costGod = d.costGod||750000000; costHak = d.costHak||5000000000; costUi = d.costUi||50000000000;
                costUe = d.costUe||250000000000; costZen = d.costZen||1500000000000; costSdb = d.costSdb||10000000000000;
                costOmni = d.costOmni||100000000000000;
                countKio = d.countKio||0; countSs = d.countSs||0; countPot = d.countPot||0;
                countGod = d.countGod||0; countHak = d.countHak||0; countUi = d.countUi||0;
                countUe = d.countUe||0; countZen = d.countZen||0; countSdb = d.countSdb||0;
                countOmni = d.countOmni||0;
                dbCount = d.dbCount||0; rageBasePower = d.rageBasePower||10;
                wishMultiUsed = d.wishMultiUsed||false; wishKiUsed = d.wishKiUsed||false;
                autoClickerLevel = d.autoClickerLevel || 0;
                autoClickerCost = d.autoClickerCost || 1000;
            }
            updateVisuals(); updateUI(); updateOrbsUI();
        }

        function hardReset() { if(confirm("⚠️ Détruire ce multivers et recommencer ?")) { localStorage.removeItem('valDBZSave'); location.reload(); } }

        function clickKi(e) {
            let gain = getActualGain(kpc); ki += gain; totalKi += gain; totalClicks++;
            if (e && e.clientX) {
                createFloatingText(e.clientX, e.clientY, `+${formatNumber(gain)}`);
            } else {
                let gokuRect = document.getElementById('goku-main').getBoundingClientRect();
                let cx = gokuRect.left + (gokuRect.width / 2) + (Math.random() * 80 - 40);
                let cy = gokuRect.top + (gokuRect.height / 2) + (Math.random() * 80 - 40);
                createFloatingText(cx, cy, `+${formatNumber(gain)}`);
                let img = document.getElementById('goku-img');
                img.style.transform = "scale(0.95)";
                setTimeout(() => img.style.transform = "scale(1)", 100);
            }
            updateUI();
        }

        function updateAutoClickerState() {
            const btn = document.getElementById('auto-click-btn');
            clearInterval(autoClickInterval);
            if (autoClickerActive) {
                btn.classList.add('active');
                btn.innerText = `ON`;
                let speed = autoClickIntervals[autoClickerLevel];
                autoClickInterval = setInterval(() => { clickKi(null); }, speed);
            } else {
                btn.classList.remove('active');
                btn.innerText = `OFF`;
            }
        }

        function toggleAutoClicker() { autoClickerActive = !autoClickerActive; updateAutoClickerState(); }

        function upgradeAutoClicker() {
            if (ki >= autoClickerCost && autoClickerLevel < 5) {
                ki -= autoClickerCost;
                autoClickerLevel++;
                autoClickerCost = Math.round(autoClickerCost * 2.5);
                if (autoClickerActive) updateAutoClickerState();
                updateUI(); saveGame();
            }
        }

        function createFloatingText(x, y, text) {
            const el = document.createElement('div'); el.className = 'float-text'; el.innerText = text;
            el.style.left = (x - 20 + Math.random() * 40) + 'px'; el.style.top = (y - 20 + Math.random() * 40) + 'px';
            document.body.appendChild(el); setTimeout(() => el.remove(), 800);
        }

        function buyUpg(type) {
            if (type === 'c' && ki >= costC && countC < 100) { ki -= costC; countC++; kpc++; costC = Math.round(costC * 1.5); }
            if (type === 's' && ki >= costS && countS < 20) { ki -= costS; countS++; kps++; costS = Math.round(costS * 1.6); }
            if (type === 'm' && ki >= costM && countM < 15) { ki -= costM; countM++; kps += 10; costM = Math.round(costM * 1.7); }
            if (type === 'st' && ki >= costST && countST < 10) { ki -= costST; countST++; kps += 100; costST = Math.round(costST * 1.8); }
            if (type === 'g' && ki >= costG && countG < 10) { ki -= costG; countG++; kpc += 50; costG = Math.round(costG * 1.8); }
            if (type === 'k' && ki >= costK && countK < 5) { ki -= costK; countK++; kps += 500; kpc += 200; costK = Math.round(costK * 2.0); }
            if (type === 'kio' && ki >= costKio && countKio < 10) { ki -= costKio; countKio++; kps += 2500; kpc += 1000; costKio = Math.round(costKio * 2.2); }
            if (type === 'ss' && ki >= costSs && countSs < 10) { ki -= costSs; countSs++; kps += 15000; costSs = Math.round(costSs * 2.5); }
            if (type === 'pot' && ki >= costPot && countPot < 5) { ki -= costPot; countPot++; kps += 100000; kpc += 50000; costPot = Math.round(costPot * 2.8); }
            if (type === 'god' && ki >= costGod && countGod < 10) { ki -= costGod; countGod++; kps += 500000; costGod = Math.round(costGod * 3.0); }
            if (type === 'hak' && ki >= costHak && countHak < 10) { ki -= costHak; countHak++; kpc += 2500000; costHak = Math.round(costHak * 3.0); }
            if (type === 'ui' && ki >= costUi && countUi < 5) { ki -= costUi; countUi++; kps += 20000000; costUi = Math.round(costUi * 3.5); }
            if (type === 'ue' && ki >= costUe && countUe < 5) { ki -= costUe; countUe++; kpc += 100000000; costUe = Math.round(costUe * 3.5); }
            if (type === 'zen' && ki >= costZen && countZen < 5) { ki -= costZen; countZen++; kps += 500000000; costZen = Math.round(costZen * 4.0); }
            if (type === 'sdb' && ki >= costSdb && countSdb < 3) { ki -= costSdb; countSdb++; kps += 5000000000; kpc += 2000000000; costSdb = Math.round(costSdb * 5.0); }
            if (type === 'omni' && ki >= costOmni && countOmni < 1) { ki -= costOmni; countOmni++; kps += 50000000000; costOmni = Math.round(costOmni * 10.0); }
            updateUI(); saveGame();
        }

        function checkTrophies() {
            if (totalKi >= 10000) document.getElementById('trophy-1').className = 'trophy unlocked';
            if (totalKi >= 1000000) document.getElementById('trophy-ki-1m').className = 'trophy unlocked';
            if (totalKi >= 1000000000) document.getElementById('trophy-ki-1b').className = 'trophy unlocked';
            if (totalKi >= 1000000000000) document.getElementById('trophy-ki-1t').className = 'trophy unlocked';
            if (totalClicks >= 1000) document.getElementById('trophy-2').className = 'trophy unlocked';
            if (totalClicks >= 10000) document.getElementById('trophy-click-10k').className = 'trophy unlocked';
            if (totalClicks >= 50000) document.getElementById('trophy-click-50k').className = 'trophy unlocked';
            if (wishMultiUsed || wishKiUsed || rageBasePower > 10) document.getElementById('trophy-3').className = 'trophy unlocked';
            if (wishMultiUsed && wishKiUsed && rageBasePower >= 50) document.getElementById('trophy-shenron-all').className = 'trophy unlocked';
            if (autoClickerLevel >= 5) document.getElementById('trophy-ac-max').className = 'trophy unlocked';
            if (rIdx >= 1) document.getElementById('trophy-rage-unlock').className = 'trophy unlocked';
            if (rIdx >= 2) document.getElementById('trophy-4').className = 'trophy unlocked';
            if (rIdx >= 5) document.getElementById('trophy-god').className = 'trophy unlocked';
            if (rIdx >= 12) document.getElementById('trophy-ui').className = 'trophy unlocked';
        }

        function updateUI() {
            document.getElementById('ki-amount').innerText = formatNumber(ki);
            document.getElementById('kps-amount').innerText = formatNumber(getActualGain(kps));
            document.getElementById('kpc-amount').innerText = formatNumber(getActualGain(kpc));
            document.getElementById('stat-total-ki').innerText = formatNumber(totalKi);
            document.getElementById('stat-clicks').innerText = totalClicks.toLocaleString();
            document.getElementById('stat-mult').innerText = "x" + mult;
            document.getElementById('stat-rage-power').innerText = "x" + rageBasePower;
            document.getElementById('cost-c').innerText = formatNumber(costC); document.getElementById('count-c').innerText = countC+"/100";
            document.getElementById('cost-s').innerText = formatNumber(costS); document.getElementById('count-s').innerText = countS+"/20";
            document.getElementById('cost-m').innerText = formatNumber(costM); document.getElementById('count-m').innerText = countM+"/15";
            document.getElementById('cost-st').innerText = formatNumber(costST); document.getElementById('count-st').innerText = countST+"/10";
            document.getElementById('cost-g').innerText = formatNumber(costG); document.getElementById('count-g').innerText = countG+"/10";
            document.getElementById('cost-k').innerText = formatNumber(costK); document.getElementById('count-k').innerText = countK+"/5";
            document.getElementById('cost-kio').innerText = formatNumber(costKio); document.getElementById('count-kio').innerText = countKio+"/10";
            document.getElementById('cost-ss').innerText = formatNumber(costSs); document.getElementById('count-ss').innerText = countSs+"/10";
            document.getElementById('cost-pot').innerText = formatNumber(costPot); document.getElementById('count-pot').innerText = countPot+"/5";
            document.getElementById('cost-god').innerText = formatNumber(costGod); document.getElementById('count-god').innerText = countGod+"/10";
            document.getElementById('cost-hak').innerText = formatNumber(costHak); document.getElementById('count-hak').innerText = countHak+"/10";
            document.getElementById('cost-ui').innerText = formatNumber(costUi); document.getElementById('count-ui').innerText = countUi+"/5";
            document.getElementById('cost-ue').innerText = formatNumber(costUe); document.getElementById('count-ue').innerText = countUe+"/5";
            document.getElementById('cost-zen').innerText = formatNumber(costZen); document.getElementById('count-zen').innerText = countZen+"/5";
            document.getElementById('cost-sdb').innerText = formatNumber(costSdb); document.getElementById('count-sdb').innerText = countSdb+"/3";
            document.getElementById('cost-omni').innerText = formatNumber(costOmni); document.getElementById('count-omni').innerText = countOmni+"/1";
            
            document.getElementById('upg-card-c').className = (ki < costC || countC >= 100) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-s').className = (ki < costS || countS >= 20) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-m').className = (ki < costM || countM >= 15) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-st').className = (ki < costST || countST >= 10) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-g').className = (ki < costG || countG >= 10) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-k').className = (ki < costK || countK >= 5) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-kio').className = (ki < costKio || countKio >= 10) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-ss').className = (ki < costSs || countSs >= 10) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-pot').className = (ki < costPot || countPot >= 5) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-god').className = (ki < costGod || countGod >= 10) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-hak').className = (ki < costHak || countHak >= 10) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-ui').className = (ki < costUi || countUi >= 5) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-ue').className = (ki < costUe || countUe >= 5) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-zen').className = (ki < costZen || countZen >= 5) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-sdb').className = (ki < costSdb || countSdb >= 3) ? 'upgrade-card disabled' : 'upgrade-card';
            document.getElementById('upg-card-omni').className = (ki < costOmni || countOmni >= 1) ? 'upgrade-card disabled' : 'upgrade-card';

            let currentCost = pCosts[rIdx] || "MAX";
            let pBtn = document.getElementById('prestige-btn');
            if (rIdx >= 20) { pBtn.disabled = true; pBtn.innerText = "PUISSANCE MAXIMALE"; }
            else { pBtn.disabled = (ki < currentCost); pBtn.innerText = pBtn.disabled ? `ÉVEIL (${formatNumber(currentCost)})` : "⚡ ÉVEIL DISPONIBLE !"; }
            document.getElementById('prestige-cost-display').innerText = typeof currentCost === "number" ? currentCost.toLocaleString() + " Ki" : currentCost;

            if (rIdx >= 1) { document.getElementById('rage-btn').style.display = 'block'; }
            if (rageBasePower >= 10 && !isRage && document.getElementById('rage-btn').innerText !== "RÉCUPÉRATION...") {
                document.getElementById('rage-btn').innerText = `🔥 MODE RAGE (X${rageBasePower}) 🔥`;
            }

            let upgBtn = document.getElementById('auto-click-upg-btn');
            if (autoClickerLevel >= 5) {
                upgBtn.innerHTML = `VITESSE MAX ATTEINTE (10/s)`;
                upgBtn.className = 'ac-upgrade maxed';
            } else {
                document.getElementById('ac-level').innerText = autoClickerLevel;
                document.getElementById('ac-cost').innerText = formatNumber(autoClickerCost);
                upgBtn.className = (ki < autoClickerCost) ? 'ac-upgrade disabled' : 'ac-upgrade';
            }
            checkTrophies();
        }

        function updateVisuals() {
            let activeIdx = Math.min(rIdx, 20);
            document.getElementById('goku-img').src = "goku" + activeIdx + ".png";
            document.getElementById('rank-name').innerText = transfoNames[activeIdx];
            document.documentElement.style.setProperty('--aura-color', colors[activeIdx] || "#ffffff");
        }

        function doPrestige() {
            let currentCost = pCosts[rIdx];
            if (ki >= currentCost && rIdx < 20) {
                ki = 0; kpc = 1; kps = 0; 
                countC=0; countS=0; countM=0; countST=0; countG=0; countK=0;
                costC=10; costS=50; costM=500; costST=15000; costG=50000; costK=500000;
                countKio=0; countSs=0; countPot=0; countGod=0; countHak=0; countUi=0; countUe=0; countZen=0; countSdb=0; countOmni=0;
                costKio=2500000; costSs=15000000; costPot=100000000; costGod=750000000; costHak=5000000000;
                costUi=50000000000; costUe=250000000000; costZen=1500000000000; costSdb=10000000000000; costOmni=100000000000000;
                mult *= 3; rIdx++;
                updateVisuals(); document.getElementById('eveil-menu').style.display = 'none';
                saveGame(); updateUI();
            }
        }

        function activateRage() {
            if (isRage) return;
            isRage = true; rageMult = rageBasePower; document.getElementById('main-game').classList.add('rage-active');
            let btn = document.getElementById('rage-btn'); btn.disabled = true; btn.innerText = "RAGE ACTIVE !!";
            updateUI();
            setTimeout(() => {
                isRage = false; rageMult = 1; document.getElementById('main-game').classList.remove('rage-active');
                btn.innerText = "RÉCUPÉRATION..."; updateUI();
                setTimeout(() => { btn.disabled = false; btn.innerText = `🔥 MODE RAGE (X${rageBasePower}) 🔥`; }, 30000);
            }, 15000);
        }

        function spawnDragonBall() {
            const db = document.createElement('img'); db.src = 'db.png'; 
            db.onerror = function(){ this.src='https://via.placeholder.com/60/ff9900/fff?text=DB'; };
            db.className = 'dragon-ball-collect';
            db.style.left = Math.random() * (window.innerWidth - 150) + 50 + 'px';
            db.style.top = Math.random() * (window.innerHeight - 150) + 50 + 'px';
            db.onclick = () => {
                if(dbCount < 7) {
                    dbCount++; updateOrbsUI(); dbBoostMult = 2;
                    setTimeout(() => { dbBoostMult = 1; updateUI(); }, 10000); saveGame();
                }
                db.remove();
            };
            document.body.appendChild(db); setTimeout(() => { if(db.parentNode) db.remove(); }, 15000);
        }

        function updateOrbsUI() {
            const dots = document.getElementById('db-dots').children;
            for(let i=0; i<7; i++) { if(i < dbCount) dots[i].classList.add('active'); else dots[i].classList.remove('active'); }
            document.getElementById('summon-btn').style.display = (dbCount >= 7) ? 'block' : 'none';
        }

        function openShenron() {
            if (dbCount >= 7) {
                const container = document.getElementById('wish-container');
                container.innerHTML = `
                    <div class="wish-card ${wishMultiUsed ? 'wish-max' : ''}" onclick="${wishMultiUsed ? '' : "applyWish('multi')"}">
                        <h3>PUISSANCE</h3><p>${wishMultiUsed ? 'MAXIMUM ATTEINT' : 'Multiplicateur Global +2 (Permanent)'}</p>
                    </div>
                    <div class="wish-card ${wishKiUsed ? 'wish-max' : ''}" onclick="${wishKiUsed ? '' : "applyWish('ki')"}">
                        <h3>FORTUNE</h3><p>${wishKiUsed ? 'MAXIMUM ATTEINT' : '+50% de tout votre Ki historique'}</p>
                    </div>
                    <div class="wish-card ${rageBasePower >= 50 ? 'wish-max' : ''}" onclick="${rageBasePower >= 50 ? '' : "applyWish('rage')"}">
                        <h3>FUREUR</h3><p>${rageBasePower >= 50 ? 'MAXIMUM ATTEINT' : 'Améliore la Rage : x' + (rageBasePower + 10)}</p>
                    </div>
                `;
                document.getElementById('shenron-menu').style.display = 'flex';
            }
        }

        function applyWish(type) {
            if (type === 'multi') { mult += 2; wishMultiUsed = true; } 
            else if (type === 'ki') { let b = totalKi * 0.5; ki += b; totalKi += b; wishKiUsed = true; } 
            else if (type === 'rage') { rageBasePower = Math.min(rageBasePower + 10, 50); }
            dbCount = 0; updateOrbsUI(); document.getElementById('shenron-menu').style.display = 'none';
            saveGame(); updateUI();
        }

        function makeDraggable(menuId, headerId) {
            const menu = document.getElementById(menuId);
            const header = document.getElementById(headerId);
            let isDragging = false, offsetX = 0, offsetY = 0;
            header.addEventListener('mousedown', (e) => {
                isDragging = true;
                offsetX = e.clientX - menu.getBoundingClientRect().left;
                offsetY = e.clientY - menu.getBoundingClientRect().top;
            });
            document.addEventListener('mousemove', (e) => {
                if (!isDragging) return;
                menu.style.left = (e.clientX - offsetX) + 'px';
                menu.style.top = (e.clientY - offsetY) + 'px';
            });
            document.addEventListener('mouseup', () => { isDragging = false; });
        }
        makeDraggable('cheat-menu', 'cheat-header');

        const konamiCode = ["ArrowUp", "ArrowUp", "ArrowDown", "ArrowDown", "ArrowLeft", "ArrowRight", "ArrowLeft", "ArrowRight", "b", "a", "Enter"]; let kPos = 0;
        document.addEventListener('keydown', (e) => {
            if(e.key === konamiCode[kPos]) {
                kPos++; if(kPos === konamiCode.length) { document.getElementById('cheat-menu').style.display='flex'; kPos = 0; }
            } else { kPos = 0; }
        });

        function adminCheatKi() { ki += 1000000; totalKi += 1000000; createFloatingText(window.innerWidth / 2, window.innerHeight / 2, "+1M KI (CHEAT)"); updateUI(); saveGame(); }
        function adminCheatPrestige() { if (rIdx < 20) { ki = pCosts[rIdx]; doPrestige(); } else { alert("Max !"); } }
        function adminCheatDB() { dbCount = 7; updateOrbsUI(); saveGame(); }
        function adminCheatMult() { mult += 10; updateUI(); saveGame(); }

        setInterval(() => { if(kps > 0) { let gain = getActualGain(kps) / 10; ki += gain; totalKi += gain; } updateUI(); }, 100);
        setInterval(() => { saveGame(); }, 5000);
        setInterval(() => { if(Math.random() > 0.4) spawnDragonBall(); }, 45000);

        window.onload = loadGame;
    </script>
</body>
</html>
