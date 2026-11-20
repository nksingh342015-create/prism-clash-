# prism-clash-                 index.html
manifest.json
sw.js
icons/
  icon-192.png
  icon-512.png
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <meta name="theme-color" content="#0b0e1c" />
  <meta name="background-color" content="#070913" />
  <meta name="description" content="PRISM CLASH — Cosmic tactical 108-card shedding game configured for GitHub Pages PWA installation." />
  <meta name="mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
  <meta name="apple-mobile-web-app-title" content="Prism Clash" />
  <link rel="manifest" href="./manifest.json" />
  <link id="pwa-apple-icon" rel="apple-touch-icon" href="./icons/icon-192.png" />
  <title>PRISM CLASH — Cosmic Card Arena</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;600;700;800;900&family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-theme: radial-gradient(circle at 50% 20%, #1e1b4b 0%, #0f172a 55%, #020617 100%);
      --card-crimson: linear-gradient(145deg, #ff3b5c 0%, #be123c 100%);
      --card-gold: linear-gradient(145deg, #fbbf24 0%, #d97706 100%);
      --card-emerald: linear-gradient(145deg, #10b981 0%, #047857 100%);
      --card-cobalt: linear-gradient(145deg, #3b82f6 0%, #1d4ed8 100%);
      --card-prism: conic-gradient(from 45deg, #ff3b5c, #fbbf24, #10b981, #3b82f6, #a855f7, #ff3b5c);
    }

    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
      user-select: none;
    }

    html, body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      overflow: hidden;
      font-family: 'Outfit', sans-serif;
      background: #020617;
      color: #f8fafc;
      touch-action: manipulation;
    }

    .game-bg {
      background: var(--bg-theme);
      transition: background 0.5s ease;
    }

    .bg-nebula { --bg-theme: radial-gradient(circle at 50% 25%, #2e1065 0%, #0f172a 60%, #020617 100%); }
    .bg-cyber { --bg-theme: radial-gradient(circle at 50% 25%, #064e3b 0%, #090d16 60%, #020617 100%); }
    .bg-solar { --bg-theme: radial-gradient(circle at 50% 25%, #7c2d12 0%, #18181b 60%, #09090b 100%); }
    .bg-abyss { --bg-theme: radial-gradient(circle at 50% 25%, #1e3a8a 0%, #090d16 60%, #020617 100%); }

    /* Card Styling */
    .prism-card {
      position: relative;
      border-radius: 14px;
      box-shadow: 0 10px 22px rgba(0, 0, 0, 0.5), inset 0 1px 1px rgba(255, 255, 255, 0.45);
      border: 3px solid rgba(255, 255, 255, 0.92);
      transition: transform 0.22s cubic-bezier(0.22, 1, 0.36, 1), box-shadow 0.22s ease, filter 0.2s ease;
      flex-shrink: 0;
      overflow: hidden;
    }

    .no-anim .prism-card,
    .no-anim * {
      transition: none !important;
      animation: none !important;
    }

    .card-red { background: var(--card-crimson); }
    .card-yellow { background: var(--card-gold); }
    .card-green { background: var(--card-emerald); }
    .card-blue { background: var(--card-cobalt); }
    .card-wild {
      background: #0f172a;
      border-color: #f8fafc;
    }

    .card-wild::before {
      content: '';
      position: absolute;
      inset: -12px;
      background: var(--card-prism);
      opacity: 0.92;
      z-index: 0;
    }

    .card-inner-oval {
      position: absolute;
      inset: 12% 10%;
      background: rgba(15, 23, 42, 0.22);
      border-radius: 999px;
      transform: rotate(-22deg);
      border: 2px solid rgba(255, 255, 255, 0.35);
      z-index: 1;
    }

    .card-wild .card-inner-oval {
      background: rgba(2, 6, 23, 0.78);
      border-color: rgba(255, 255, 255, 0.7);
    }

    /* Card Backs */
    .card-back {
      background: linear-gradient(135deg, #0f172a 0%, #1e1b4b 100%);
      border: 2.5px solid #cbd5e1;
      box-shadow: 0 6px 14px rgba(0, 0, 0, 0.55);
    }
    .card-back-prism { background: linear-gradient(135deg, #1e1b4b, #4c1d95, #0f172a); border-color: #a855f7; }
    .card-back-solar { background: linear-gradient(135deg, #451a03, #9a3412, #1c1917); border-color: #f59e0b; }
    .card-back-matrix { background: linear-gradient(135deg, #022c22, #065f46, #090d16); border-color: #10b981; }

    /* Interactive Hand Cards */
    .hand-card {
      width: 78px;
      height: 116px;
      cursor: pointer;
    }
    @media (min-width: 640px) {
      .hand-card {
        width: 96px;
        height: 142px;
      }
    }
    @media (min-width: 1024px) {
      .hand-card {
        width: 106px;
        height: 156px;
      }
    }

    .hand-card.playable {
      box-shadow: 0 0 18px rgba(255, 255, 255, 0.65), 0 10px 22px rgba(0, 0, 0, 0.6);
      transform: translateY(-10px);
    }
    .hand-card.playable:hover {
      transform: translateY(-22px) scale(1.06);
      z-index: 40 !important;
      box-shadow: 0 0 26px rgba(250, 204, 21, 0.9), 0 16px 28px rgba(0, 0, 0, 0.7);
    }
    .hand-card.unplayable {
      filter: brightness(0.65) saturate(0.8);
    }
    .hand-card.unplayable:hover {
      transform: translateY(-4px);
    }

    @keyframes shakeCard {
      0%, 100% { transform: translateX(0); }
      20% { transform: translateX(-8px) rotate(-4deg); }
      40% { transform: translateX(8px) rotate(4deg); }
      60% { transform: translateX(-6px) rotate(-2deg); }
      80% { transform: translateX(6px) rotate(2deg); }
    }
    .shake-invalid {
      animation: shakeCard 0.35s ease-in-out !important;
      box-shadow: 0 0 22px #ef4444 !important;
    }

    @keyframes popBanner {
      0% { transform: translate(-50%, -50%) scale(0.4); opacity: 0; }
      20% { transform: translate(-50%, -50%) scale(1.12); opacity: 1; }
      80% { transform: translate(-50%, -50%) scale(1); opacity: 1; }
      100% { transform: translate(-50%, -65%) scale(0.95); opacity: 0; }
    }
    .action-banner {
      animation: popBanner 1.35s cubic-bezier(0.22, 1, 0.36, 1) forwards;
    }

    @keyframes pulseGlow {
      0%, 100% { transform: scale(1); box-shadow: 0 0 15px rgba(244, 63, 94, 0.7), 0 0 30px rgba(250, 204, 21, 0.5); }
      50% { transform: scale(1.07); box-shadow: 0 0 25px rgba(244, 63, 94, 0.95), 0 0 45px rgba(250, 204, 21, 0.85); }
    }
    .prism-btn-pulse {
      animation: pulseGlow 1.1s infinite ease-in-out;
    }

    @keyframes spinSlow {
      from { transform: rotate(0deg); }
      to { transform: rotate(360deg); }
    }
    @keyframes spinSlowRev {
      from { transform: rotate(360deg); }
      to { transform: rotate(0deg); }
    }
    .dir-cw { animation: spinSlow 9s linear infinite; }
    .dir-ccw { animation: spinSlowRev 9s linear infinite; }

    .hand-scroll::-webkit-scrollbar {
      height: 6px;
    }
    .hand-scroll::-webkit-scrollbar-track {
      background: rgba(15, 23, 42, 0.4);
      border-radius: 999px;
    }
    .hand-scroll::-webkit-scrollbar-thumb {
      background: rgba(148, 163, 184, 0.45);
      border-radius: 999px;
    }

    .glass-panel {
      background: rgba(15, 23, 42, 0.82);
      backdrop-filter: blur(14px);
      -webkit-backdrop-filter: blur(14px);
      border: 1px solid rgba(255, 255, 255, 0.14);
    }

    .text-stroke {
      text-shadow: 0 2px 4px rgba(0, 0, 0, 0.85), 0 0 2px rgba(0, 0, 0, 0.9);
    }

    #confetti-canvas {
      position: fixed;
      inset: 0;
      pointer-events: none;
      z-index: 90;
    }
  </style>
</head>
<body class="game-bg bg-nebula select-none">

  <canvas id="confetti-canvas"></canvas>

  <!-- ==================== MAIN MENU SCREEN ==================== -->
  <div id="screen-menu" class="fixed inset-0 z-20 flex flex-col items-center justify-between p-4 sm:p-6 overflow-y-auto">
    <!-- Top Utility Bar -->
    <div class="w-full max-w-5xl flex items-center justify-between">
      <button onclick="openModal('modal-profile')" class="glass-panel flex items-center gap-2.5 px-3.5 py-2 rounded-2xl hover:bg-slate-800/90 transition active:scale-95">
        <span id="menu-player-avatar" class="text-2xl">🚀</span>
        <div class="text-left">
          <div id="menu-player-name" class="text-sm font-bold text-white leading-tight">Nova</div>
          <div class="text-[11px] text-indigo-300 font-medium">Edit Profile</div>
        </div>
      </button>

      <div class="flex items-center gap-2">
        <button id="btn-install-app" onclick="triggerPwaInstall()" class="hidden glass-panel px-3.5 py-2 rounded-2xl border border-emerald-400/50 bg-emerald-500/15 hover:bg-emerald-500/25 text-emerald-200 transition active:scale-95 text-xs sm:text-sm font-extrabold flex items-center gap-1.5 shadow-lg">
          <span>📲</span> <span>Install App</span>
        </button>
        <button id="btn-quick-mute" onclick="toggleMute()" title="Toggle Mute" class="glass-panel p-2.5 rounded-2xl hover:bg-slate-800/90 transition active:scale-95 text-lg">
          🔊
        </button>
        <button onclick="openModal('modal-settings')" class="glass-panel px-3.5 py-2.5 rounded-2xl hover:bg-slate-800/90 transition active:scale-95 text-sm font-bold flex items-center gap-1.5">
          <span>⚙️</span> <span class="hidden sm:inline">Settings</span>
        </button>
      </div>
    </div>

    <!-- Center Logo & Actions -->
    <div class="flex flex-col items-center my-auto py-4 w-full max-w-md text-center">
      <div class="mb-3 relative">
        <svg class="w-24 h-24 sm:w-28 sm:h-28 drop-shadow-[0_0_25px_rgba(168,85,247,0.65)]" viewBox="0 0 120 120" fill="none" xmlns="http://www.w3.org/2000/svg">
          <defs>
            <linearGradient id="prismGrad1" x1="10" y1="10" x2="110" y2="110" gradientUnits="userSpaceOnUse">
              <stop offset="0%" stop-color="#FF3B5C"/>
              <stop offset="33%" stop-color="#FBBF24"/>
              <stop offset="66%" stop-color="#10B981"/>
              <stop offset="100%" stop-color="#3B82F6"/>
            </linearGradient>
            <linearGradient id="prismInner" x1="60" y1="15" x2="60" y2="105" gradientUnits="userSpaceOnUse">
              <stop offset="0%" stop-color="#FFFFFF" stop-opacity="0.95"/>
              <stop offset="100%" stop-color="#C084FC" stop-opacity="0.35"/>
            </linearGradient>
          </defs>
          <polygon points="60,8 110,38 110,82 60,112 10,82 10,38" fill="#0F172A" stroke="url(#prismGrad1)" stroke-width="6"/>
          <polygon points="60,20 96,60 60,100 24,60" fill="url(#prismGrad1)" opacity="0.88"/>
          <polygon points="60,20 96,60 60,60" fill="url(#prismInner)" opacity="0.75"/>
          <circle cx="60" cy="60" r="14" fill="#0F172A" stroke="#FFFFFF" stroke-width="3"/>
          <path d="M60 50 L63 57 L70 60 L63 63 L60 70 L57 63 L50 60 L57 57 Z" fill="#FBBF24"/>
        </svg>
      </div>

      <h1 class="text-4xl sm:text-6xl font-black tracking-tight bg-gradient-to-r from-rose-400 via-amber-300 via-emerald-300 to-sky-400 bg-clip-text text-transparent drop-shadow">
        PRISM CLASH
      </h1>
      <p class="text-slate-300 text-xs sm:text-sm tracking-widest uppercase font-semibold mt-1 mb-6">
        Cosmic Tactical Card Arena
      </p>

      <div class="w-full space-y-3">
        <button onclick="startQuickPlay()" class="w-full py-4 px-6 rounded-2xl bg-gradient-to-r from-rose-500 via-amber-500 to-emerald-500 hover:brightness-110 text-slate-950 font-black text-lg sm:text-xl shadow-[0_8px_25px_rgba(244,63,94,0.45)] transition transform active:scale-95 flex items-center justify-center gap-2">
          <span>⚡</span> QUICK PLAY (CLASSIC 4P)
        </button>

        <button onclick="openModal('modal-modes')" class="w-full py-3.5 px-6 rounded-2xl bg-indigo-600 hover:bg-indigo-500 text-white font-extrabold text-base sm:text-lg shadow-lg transition transform active:scale-95 flex items-center justify-center gap-2">
          <span>🎮</span> SELECT GAME MODE
        </button>

        <button onclick="openCustomMatch()" class="w-full py-3.5 px-6 rounded-2xl glass-panel hover:bg-slate-800/90 text-white font-bold text-base transition transform active:scale-95 flex items-center justify-center gap-2">
          <span>🛠️</span> CUSTOM MATCH & HOUSE RULES
        </button>

        <!-- PWA Status & Studio Banner -->
        <div id="pwa-menu-banner" class="glass-panel w-full rounded-2xl p-3 border border-indigo-400/25 flex items-center justify-between gap-3 text-left">
          <div class="flex items-center gap-2.5 min-w-0">
            <span id="pwa-status-icon" class="text-xl shrink-0">🌐</span>
            <div class="min-w-0">
              <div id="pwa-status-title" class="text-xs font-extrabold text-white truncate">GitHub Pages PWA Ready</div>
              <div id="pwa-status-sub" class="text-[11px] text-slate-300 truncate">Checking Service Worker & Install...</div>
            </div>
          </div>
          <button id="btn-banner-install" onclick="openPwaInfoModal()" class="shrink-0 px-3 py-1.5 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white font-extrabold text-xs transition active:scale-95">
            PWA Studio
          </button>
        </div>
      </div>

      <div class="grid grid-cols-3 gap-2.5 w-full mt-4">
        <button onclick="openModal('modal-rules')" class="glass-panel py-3 px-2 rounded-2xl hover:bg-slate-800/90 transition active:scale-95 flex flex-col items-center gap-1">
          <span class="text-xl">📖</span>
          <span class="text-xs font-bold">How to Play</span>
        </button>
        <button onclick="openStatsModal()" class="glass-panel py-3 px-2 rounded-2xl hover:bg-slate-800/90 transition active:scale-95 flex flex-col items-center gap-1">
          <span class="text-xl">🏆</span>
          <span class="text-xs font-bold">Stats & Medals</span>
        </button>
        <button onclick="openModal('modal-about')" class="glass-panel py-3 px-2 rounded-2xl hover:bg-slate-800/90 transition active:scale-95 flex flex-col items-center gap-1">
          <span class="text-xl">✨</span>
          <span class="text-xs font-bold">Deck & Info</span>
        </button>
      </div>
    </div>

    <div class="text-[11px] text-slate-400 font-medium">
      108-Card Rules • Colour-Blind Accessible • GitHub Pages PWA Ready
    </div>
  </div>

  <!-- ==================== GAMEPLAY SCREEN ==================== -->
  <div id="screen-game" class="fixed inset-0 z-10 hidden flex-col justify-between p-2 sm:p-4">
    <!-- Top HUD & Opponents -->
    <div class="w-full max-w-6xl mx-auto flex flex-col gap-2">
      <div class="flex items-center justify-between gap-2">
        <div class="flex items-center gap-2">
          <button onclick="pauseGame()" class="glass-panel px-3 py-2 rounded-xl hover:bg-slate-800 font-bold text-xs sm:text-sm flex items-center gap-1.5 active:scale-95">
            <span>⏸️</span> <span class="hidden sm:inline">Pause</span>
          </button>
          <div class="glass-panel px-3 py-1.5 rounded-xl text-xs font-bold text-indigo-200 flex items-center gap-2">
            <span id="hud-mode-badge">Classic</span>
            <span class="text-slate-500">|</span>
            <span id="hud-round-badge">Round 1</span>
            <span id="hud-target-badge" class="hidden sm:inline text-amber-300"></span>
          </div>
        </div>

        <div id="hud-house-rules" class="hidden md:flex items-center gap-1.5 flex-wrap justify-center"></div>

        <div class="flex items-center gap-1.5">
          <button onclick="openModal('modal-rules')" class="glass-panel p-2 rounded-xl hover:bg-slate-800 text-xs font-bold active:scale-95" title="Rules">📖</button>
          <button id="btn-game-mute" onclick="toggleMute()" class="glass-panel p-2 rounded-xl hover:bg-slate-800 text-xs font-bold active:scale-95" title="Mute">🔊</button>
        </div>
      </div>

      <div id="opponents-container" class="flex items-center justify-center gap-2 sm:gap-4 flex-wrap py-1"></div>
    </div>

    <!-- Center Arena -->
    <div class="relative flex-1 flex flex-col items-center justify-center my-1">
      <div id="turn-indicator" class="mb-2 sm:mb-3 px-4 py-1.5 rounded-full glass-panel text-xs sm:text-sm font-extrabold tracking-wide text-center shadow-lg transition-all">
        Your Turn!
      </div>

      <div class="relative flex items-center justify-center gap-6 sm:gap-12 p-4 sm:p-6">
        <div id="direction-ring" class="dir-cw pointer-events-none absolute w-56 h-56 sm:w-72 sm:h-72 rounded-full border-2 border-dashed border-white/20 flex items-center justify-between px-1">
          <span class="text-white/40 text-lg font-black">↻</span>
          <span class="text-white/40 text-lg font-black">↻</span>
        </div>

        <!-- Draw Pile -->
        <div class="flex flex-col items-center gap-1.5 z-10">
          <button id="btn-draw-pile" onclick="humanDrawCard()" class="relative group focus:outline-none active:scale-95 transition transform">
            <div id="draw-pile-visual" class="w-20 h-28 sm:w-24 sm:h-36 rounded-2xl card-back flex flex-col items-center justify-center relative">
              <div class="w-10 h-10 sm:w-12 sm:h-12 rounded-full bg-white/10 border border-white/30 flex items-center justify-center">
                <span class="text-lg sm:text-xl">✦</span>
              </div>
              <span id="draw-pile-count" class="mt-1.5 text-[11px] font-black tracking-wider text-slate-200 bg-black/50 px-2 py-0.5 rounded-full">108</span>
            </div>
            <div id="draw-pile-glow" class="hidden absolute -inset-1.5 rounded-2xl border-2 border-amber-400 animate-pulse pointer-events-none"></div>
          </button>
          <span class="text-[11px] font-bold text-slate-300 uppercase tracking-wider">Draw</span>
        </div>

        <!-- Active Colour Indicator -->
        <div class="flex flex-col items-center gap-2 z-10">
          <div id="active-color-orb" class="w-12 h-12 sm:w-14 sm:h-14 rounded-2xl border-2 border-white shadow-lg flex flex-col items-center justify-center transition-all duration-300">
            <span id="active-color-icon" class="text-lg sm:text-xl leading-none">🔥</span>
            <span id="active-color-label" class="text-[9px] font-black uppercase tracking-wider mt-0.5">RED</span>
          </div>
          <div id="pending-stack-badge" class="hidden px-2.5 py-0.5 rounded-full bg-rose-600 text-white text-xs font-black animate-bounce shadow">
            +2 STACK
          </div>
          <div id="direction-label" class="text-[10px] font-bold text-slate-300 uppercase tracking-wider">
            Clockwise ↻
          </div>
        </div>

        <!-- Discard Pile -->
        <div class="flex flex-col items-center gap-1.5 z-10">
          <div id="discard-pile-slot" class="w-20 h-28 sm:w-24 sm:h-36 flex items-center justify-center"></div>
          <span class="text-[11px] font-bold text-slate-300 uppercase tracking-wider">Discard</span>
        </div>
      </div>

      <!-- Post-Draw Bar -->
      <div id="post-draw-bar" class="hidden mt-2 glass-panel px-4 py-2 rounded-2xl flex items-center gap-3 z-20 shadow-xl border border-amber-400/50">
        <span class="text-xs sm:text-sm font-bold text-amber-200">Playable card drawn!</span>
        <button onclick="playDrawnCard()" class="px-3.5 py-1.5 rounded-xl bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-black text-xs sm:text-sm active:scale-95">
          Play Now
        </button>
        <button onclick="passAfterDraw()" class="px-3.5 py-1.5 rounded-xl bg-slate-700 hover:bg-slate-600 text-white font-bold text-xs sm:text-sm active:scale-95">
          Keep & Pass
        </button>
      </div>
    </div>

    <div id="floating-banner-container" class="pointer-events-none fixed inset-0 z-40"></div>

    <!-- Bottom Hand Area -->
    <div class="w-full max-w-6xl mx-auto flex flex-col gap-2">
      <div