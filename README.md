<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta name="theme-color" content="#0b0d19" />
  <meta name="description" content="Milo é um companheiro interativo que reage ao jogador durante a partida." />
  <link rel="manifest" href="manifest.webmanifest" />
  <title>Milo — seu companheiro de jogo</title>
  <style>
    :root {
      --bg: #090b14;
      --panel: rgba(19, 22, 39, .78);
      --panel-strong: #15192c;
      --line: rgba(255,255,255,.09);
      --text: #f7f8ff;
      --muted: #949ab3;
      --violet: #8c6bff;
      --violet-2: #b496ff;
      --cyan: #65e5f4;
      --pink: #ff72b6;
      --lime: #bdf47b;
      --shadow: 0 24px 70px rgba(0,0,0,.32);
    }

    * { box-sizing: border-box; }
    html { min-width: 320px; }
    body {
      margin: 0;
      color: var(--text);
      background:
        radial-gradient(circle at 12% 8%, rgba(119, 79, 255, .22), transparent 28rem),
        radial-gradient(circle at 90% 14%, rgba(73, 212, 230, .10), transparent 24rem),
        var(--bg);
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      min-height: 100vh;
      overflow-x: hidden;
    }

    button { font: inherit; }
    .app-shell { width: min(1240px, calc(100% - 36px)); margin: 0 auto; padding: 28px 0 40px; }
    .topbar { display: flex; align-items: center; justify-content: space-between; gap: 24px; margin-bottom: 34px; }
    .brand { display: flex; align-items: center; gap: 12px; letter-spacing: -.03em; }
    .brand-mark { width: 42px; height: 42px; border-radius: 14px; display: grid; place-items: center; background: linear-gradient(135deg, var(--violet), #5c45dc); box-shadow: 0 8px 30px rgba(140, 107, 255, .35); font-size: 21px; }
    .brand strong { font-size: 18px; }
    .brand span { color: var(--muted); font-size: 11px; display: block; margin-top: 3px; letter-spacing: .08em; text-transform: uppercase; }
    .top-actions { display: flex; align-items: center; gap: 10px; }
    .live-pill, .icon-btn { border: 1px solid var(--line); background: rgba(255,255,255,.04); color: var(--muted); }
    .live-pill { padding: 9px 13px; border-radius: 999px; font-size: 12px; display: inline-flex; align-items: center; gap: 8px; }
    .live-pill i { width: 7px; height: 7px; border-radius: 50%; background: var(--lime); box-shadow: 0 0 12px var(--lime); }
    .icon-btn { width: 38px; height: 38px; border-radius: 12px; cursor: pointer; transition: .2s ease; }
    .icon-btn:hover { color: var(--text); border-color: rgba(255,255,255,.22); transform: translateY(-1px); }

    .intro { margin-bottom: 28px; }
    .eyebrow { color: var(--cyan); text-transform: uppercase; letter-spacing: .18em; font-size: 11px; font-weight: 700; }
    h1 { font-size: clamp(35px, 5vw, 66px); line-height: .98; letter-spacing: -.065em; max-width: 720px; margin: 12px 0 13px; }
    h1 em { font-style: normal; color: var(--violet-2); }
    .intro p { color: var(--muted); max-width: 610px; line-height: 1.65; margin: 0; font-size: 15px; }

    .dashboard { display: grid; grid-template-columns: minmax(0, 1.56fr) minmax(310px, .84fr); gap: 18px; align-items: stretch; }
    .panel { border: 1px solid var(--line); background: var(--panel); box-shadow: var(--shadow); border-radius: 26px; backdrop-filter: blur(18px); }
    .game-panel { padding: 19px; min-height: 570px; }
    .panel-heading { display: flex; align-items: flex-start; justify-content: space-between; gap: 16px; padding: 2px 4px 16px; }
    .panel-heading h2 { font-size: 16px; margin: 0 0 5px; letter-spacing: -.02em; }
    .panel-heading p { margin: 0; color: var(--muted); font-size: 12px; }
    .mode-pill { color: var(--cyan); font-size: 11px; border: 1px solid rgba(101,229,244,.22); background: rgba(101,229,244,.08); padding: 7px 10px; border-radius: 8px; white-space: nowrap; }

    .scorebar { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 14px; }
    .stat { padding: 13px 14px; border: 1px solid var(--line); background: rgba(255,255,255,.035); border-radius: 16px; }
    .stat small { display: block; color: var(--muted); text-transform: uppercase; letter-spacing: .12em; font-size: 9px; margin-bottom: 6px; }
    .stat strong { display: block; font-size: 24px; letter-spacing: -.05em; }
    .stat strong span { font-size: 12px; color: var(--muted); font-weight: 500; letter-spacing: 0; }
    #comboStat { color: var(--pink); }

    .arena { position: relative; min-height: 372px; overflow: hidden; border-radius: 20px; border: 1px solid rgba(140,107,255,.25); background: linear-gradient(145deg, rgba(40,35,82,.8), rgba(12,19,39,.92)); cursor: crosshair; isolation: isolate; }
    .arena::before { content: ""; position: absolute; inset: 0; opacity: .42; background-image: linear-gradient(rgba(140,107,255,.1) 1px, transparent 1px), linear-gradient(90deg, rgba(140,107,255,.1) 1px, transparent 1px); background-size: 34px 34px; mask-image: linear-gradient(to bottom, black, transparent); z-index: -1; }
    .arena::after { content: ""; position: absolute; width: 220px; height: 220px; top: 10%; right: 12%; border-radius: 50%; background: rgba(115, 93, 255, .16); filter: blur(40px); z-index: -1; }
    .arena-label { position: absolute; left: 18px; top: 16px; display: flex; align-items: center; gap: 8px; color: rgba(255,255,255,.48); font-size: 10px; letter-spacing: .14em; text-transform: uppercase; pointer-events: none; }
    .arena-label i { display: block; width: 6px; height: 6px; border-radius: 50%; background: var(--pink); box-shadow: 0 0 10px var(--pink); }
    .target { position: absolute; width: 58px; height: 58px; margin: -29px 0 0 -29px; display: grid; place-items: center; border: 0; border-radius: 50%; color: white; background: radial-gradient(circle at 36% 29%, #fff 0 5%, #ffb5d6 7%, #ff5aa9 29%, #ac51ff 65%, #5d43dc); box-shadow: 0 0 0 7px rgba(255,114,182,.11), 0 0 30px rgba(255,114,182,.72); cursor: pointer; animation: target-in .26s cubic-bezier(.2,1.5,.5,1) both, target-pulse 1.4s ease-in-out infinite; z-index: 3; }
    .target:active { transform: scale(.82); }
    .target span { width: 13px; height: 13px; border: 2px solid rgba(255,255,255,.9); border-radius: 50%; }
    @keyframes target-in { from { opacity: 0; transform: scale(.3) rotate(-20deg); } to { opacity: 1; transform: scale(1) rotate(0); } }
    @keyframes target-pulse { 50% { box-shadow: 0 0 0 12px rgba(255,114,182,.06), 0 0 36px rgba(255,114,182,.86); } }
    .arena-center { position: absolute; inset: 0; display: grid; place-items: center; padding: 30px; text-align: center; pointer-events: none; }
    .start-card { max-width: 310px; pointer-events: auto; }
    .start-card .spark { font-size: 32px; margin-bottom: 8px; }
    .start-card h3 { margin: 0 0 7px; font-size: 22px; letter-spacing: -.04em; }
    .start-card p { color: var(--muted); font-size: 13px; line-height: 1.55; margin: 0 0 18px; }
    .primary { border: 0; color: #130e2c; background: linear-gradient(110deg, #a58aff, #d5c6ff); font-weight: 800; border-radius: 12px; padding: 12px 20px; cursor: pointer; box-shadow: 0 9px 22px rgba(140,107,255,.27); transition: transform .2s, box-shadow .2s; }
    .primary:hover { transform: translateY(-2px); box-shadow: 0 14px 28px rgba(140,107,255,.38); }
    .primary:active { transform: translateY(0); }
    .game-footer { display: flex; align-items: center; justify-content: space-between; gap: 14px; padding: 13px 4px 0; color: var(--muted); font-size: 11px; }
    .hint { display: flex; align-items: center; gap: 7px; }
    .key { padding: 4px 7px; color: var(--text); border: 1px solid var(--line); border-radius: 6px; background: rgba(255,255,255,.05); }

    .companion-panel { padding: 21px; display: flex; flex-direction: column; min-height: 570px; background: linear-gradient(165deg, rgba(28,26,54,.9), rgba(17,23,41,.8)); }
    .companion-top { display: flex; align-items: center; justify-content: space-between; }
    .companion-id { display: flex; align-items: center; gap: 10px; }
    .mini-avatar { position: relative; width: 38px; height: 38px; border-radius: 14px; background: linear-gradient(135deg, #ff83be, #8a61ff); display: grid; place-items: center; font-size: 18px; box-shadow: 0 7px 18px rgba(255,114,182,.2); }
    .companion-id strong { font-size: 14px; display: block; }
    .companion-id span { color: var(--lime); font-size: 10px; display: block; margin-top: 2px; }
    .online-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--lime); box-shadow: 0 0 10px var(--lime); }
    .companion-stage { flex: 1; min-height: 295px; display: grid; place-items: center; position: relative; }
    .companion-stage::before { content: ""; position: absolute; width: 210px; height: 55px; bottom: 37px; border-radius: 50%; background: rgba(109,84,255,.18); filter: blur(15px); }
    .character { position: relative; width: 185px; height: 185px; border-radius: 47% 47% 42% 42%; background: linear-gradient(145deg, #ffd0e7 2%, #fa8fbe 45%, #a66cff 100%); box-shadow: inset -18px -19px 25px rgba(97,49,180,.22), inset 15px 12px 22px rgba(255,255,255,.28), 0 25px 40px rgba(105,66,232,.3); animation: float 3.2s ease-in-out infinite; }
    .character::before, .character::after { content: ""; position: absolute; top: -34px; width: 67px; height: 78px; border-radius: 12px 58px 12px 58px; background: linear-gradient(145deg, #ffb8d8, #a46bff); z-index: -1; }
    .character::before { left: 4px; transform: rotate(-31deg); }
    .character::after { right: 4px; transform: scaleX(-1) rotate(-31deg); }
    @keyframes float { 50% { transform: translateY(-8px) rotate(1deg); } }
    .face { position: absolute; inset: 0; }
    .eye { position: absolute; top: 67px; width: 19px; height: 25px; border-radius: 50%; background: #29203f; }
    .eye::after { content: ""; position: absolute; width: 6px; height: 8px; top: 4px; left: 4px; border-radius: 50%; background: white; }
    .eye.left { left: 51px; } .eye.right { right: 51px; }
    .nose { position: absolute; top: 102px; left: calc(50% - 7px); width: 14px; height: 10px; border-radius: 55% 55% 55% 55%; background: #9b4384; }
    .mouth { position: absolute; top: 112px; left: calc(50% - 16px); width: 32px; height: 15px; border-bottom: 3px solid #762d78; border-radius: 0 0 50% 50%; }
    .blush { position: absolute; top: 101px; width: 27px; height: 12px; border-radius: 50%; background: rgba(255,116,158,.45); filter: blur(2px); }
    .blush.left { left: 27px; } .blush.right { right: 27px; }
    .character.excited { animation: excited .38s ease-in-out 2; }
    .character.excited .eye { height: 19px; top: 70px; }
    @keyframes excited { 50% { transform: translateY(-14px) rotate(-4deg) scale(1.05); } }
    .bubble { position: relative; margin: 0 auto; width: min(100%, 330px); padding: 14px 16px; border-radius: 17px; background: rgba(255,255,255,.08); border: 1px solid rgba(255,255,255,.1); color: #eeeaff; line-height: 1.5; font-size: 13px; text-align: center; }
    .bubble::after { content: ""; position: absolute; left: 50%; bottom: -7px; width: 13px; height: 13px; background: rgba(255,255,255,.08); border-right: 1px solid rgba(255,255,255,.1); border-bottom: 1px solid rgba(255,255,255,.1); transform: translateX(-50%) rotate(45deg); }
    .quick-title { color: var(--muted); font-size: 10px; text-transform: uppercase; letter-spacing: .14em; margin: 26px 0 10px; }
    .quick-actions { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
    .quick { border: 1px solid var(--line); border-radius: 11px; padding: 9px 10px; color: #d9d9ed; background: rgba(255,255,255,.035); cursor: pointer; text-align: left; font-size: 11px; transition: .2s; }
    .quick:hover { border-color: rgba(165,138,255,.55); background: rgba(140,107,255,.12); transform: translateY(-1px); }
    .mic-row { display: flex; gap: 9px; margin-top: 12px; }
    .mic-btn { flex: 1; display: flex; align-items: center; justify-content: center; gap: 8px; border: 1px solid rgba(101,229,244,.18); color: var(--cyan); background: rgba(101,229,244,.06); border-radius: 11px; padding: 11px 12px; cursor: pointer; font-size: 12px; }
    .mic-btn:hover { background: rgba(101,229,244,.11); }
    .mic-btn.listening { color: #131a27; background: var(--cyan); animation: listen 1.1s infinite; }
    @keyframes listen { 50% { box-shadow: 0 0 0 6px rgba(101,229,244,.12); } }
    .mute-btn { width: 44px; border: 1px solid var(--line); color: var(--muted); background: rgba(255,255,255,.04); border-radius: 11px; cursor: pointer; }

    .features { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin-top: 16px; }
    .feature { border: 1px solid var(--line); border-radius: 17px; background: rgba(255,255,255,.03); padding: 15px; }
    .feature-icon { width: 29px; height: 29px; display: grid; place-items: center; border-radius: 9px; background: rgba(140,107,255,.16); margin-bottom: 11px; }
    .feature h3 { font-size: 12px; margin: 0 0 5px; }
    .feature p { color: var(--muted); font-size: 11px; line-height: 1.5; margin: 0; }
    .footer-note { text-align: center; color: #5e6379; font-size: 11px; margin-top: 24px; }
    .sr-only { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0,0,0,0); white-space: nowrap; border: 0; }
    [hidden] { display: none !important; }

    @media (max-width: 860px) {
      .dashboard { grid-template-columns: 1fr; }
      .companion-panel { min-height: auto; }
      .companion-stage { min-height: 275px; }
    }
    @media (max-width: 560px) {
      .app-shell { width: min(100% - 24px, 1240px); padding-top: 17px; }
      .topbar { margin-bottom: 26px; }
      .top-actions .live-pill { display: none; }
      h1 { font-size: 43px; }
      .game-panel, .companion-panel { padding: 14px; border-radius: 20px; }
      .arena { min-height: 330px; }
      .features { grid-template-columns: 1fr; }
      .feature { display: grid; grid-template-columns: auto 1fr; column-gap: 10px; align-items: start; }
      .feature-icon { grid-row: span 2; margin-bottom: 0; }
      .game-footer { align-items: flex-start; flex-direction: column; }
    }
  </style>
</head>
<body>
  <main class="app-shell">
    <header class="topbar">
      <div class="brand">
        <div class="brand-mark">✦</div>
        <div><strong>PLAYMATE</strong><span>companheiro de partida</span></div>
      </div>
      <div class="top-actions">
        <div class="live-pill"><i></i> sistema online</div>
        <button class="icon-btn" id="resetButton" title="Reiniciar melhor pontuação" aria-label="Reiniciar melhor pontuação">↻</button>
      </div>
    </header>

    <section class="intro">
      <div class="eyebrow">apresentando o Milo</div>
      <h1>Jogue. Reaja.<br><em>Nunca jogue sozinho.</em></h1>
      <p>Um companheiro que acompanha sua partida, comemora seus combos, percebe quando você precisa de ajuda e conversa com você em tempo real.</p>
    </section>

    <section class="dashboard" aria-label="Área principal do Playmate">
      <article class="panel game-panel">
        <div class="panel-heading">
          <div><h2>Neon Reflex</h2><p>Toque nos alvos antes que desapareçam.</p></div>
          <div class="mode-pill">modo treino · 30s</div>
        </div>
        <div class="scorebar" aria-live="polite">
          <div class="stat"><small>pontuação</small><strong id="scoreStat">0</strong></div>
          <div class="stat"><small>combo</small><strong id="comboStat">0<span>x</span></strong></div>
          <div class="stat"><small>tempo</small><strong id="timeStat">30<span>s</span></strong></div>
        </div>
        <div class="arena" id="arena" role="application" aria-label="Arena do jogo Neon Reflex">
          <div class="arena-label"><i></i> arena ao vivo</div>
          <button class="target" id="target" hidden aria-label="Alvo neon"><span></span></button>
          <div class="arena-center" id="arenaCenter">
            <div class="start-card">
              <div class="spark">✦</div>
              <h3>Pronto para o reflexo?</h3>
              <p>O Milo vai narrar sua partida. Acerte os alvos, mantenha o combo e veja como ele reage.</p>
              <button class="primary" id="startButton">Começar partida</button>
            </div>
          </div>
        </div>
        <div class="game-footer"><div class="hint"><span class="key">clique</span> no alvo brilhante</div><div>melhor: <strong id="bestStat">0</strong> pontos</div></div>
      </article>

      <aside class="panel companion-panel">
        <div class="companion-top">
          <div class="companion-id"><div class="mini-avatar">✦</div><div><strong>Milo</strong><span>● acompanhando você</span></div></div>
          <div class="online-dot" aria-label="Milo online"></div>
        </div>
        <div class="companion-stage">
          <div class="character" id="character" aria-label="Milo, seu companheiro"><div class="face"><div class="eye left"></div><div class="eye right"></div><div class="blush left"></div><div class="blush right"></div><div class="nose"></div><div class="mouth"></div></div></div>
        </div>
        <div class="bubble" id="bubble" aria-live="polite">Oi! Eu sou o Milo. Vamos jogar uma partida?</div>
        <div class="quick-title">fale com o Milo</div>
        <div class="quick-actions">
          <button class="quick" data-message="Milo, me dá uma dica!">💡 Me dá uma dica</button>
          <button class="quick" data-message="Milo, você está pronto?">⚡ Está pronto?</button>
          <button class="quick" data-message="Milo, comemora comigo!">🎉 Comemora comigo</button>
          <button class="quick" data-message="Milo, como estou indo?">📊 Como estou indo?</button>
        </div>
        <div class="mic-row">
          <button class="mic-btn" id="micButton">◉ Falar com Milo</button>
          <button class="mute-btn" id="muteButton" title="Ativar/desativar voz" aria-label="Ativar/desativar voz">🔊</button>
        </div>
      </aside>
    </section>

    <section class="features" aria-label="Recursos do Playmate">
      <div class="feature"><div class="feature-icon">◈</div><h3>Reações em tempo real</h3><p>O humor do Milo muda conforme sua performance.</p></div>
      <div class="feature"><div class="feature-icon">⌁</div><h3>Conversa por voz</h3><p>Use o microfone para chamar o Milo durante a partida.</p></div>
      <div class="feature"><div class="feature-icon">↗</div><h3>Feito para evoluir</h3><p>Pronto para conectar com um jogo real via overlay ou API.</p></div>
    </section>
    <div class="footer-note">Protótipo Playmate · uma experiência original de companheiro de jogo</div>
  </main>

  <script>
    const state = {
      score: 0,
      combo: 0,
      time: 30,
      running: false,
      muted: false,
      best: Number(localStorage.getItem('playmate-best') || 0),
      interval: null,
      hits: 0,
      recognition: null
    };

    const $ = (selector) => document.querySelector(selector);
    const scoreStat = $('#scoreStat');
    const comboStat = $('#comboStat');
    const timeStat = $('#timeStat');
    const bestStat = $('#bestStat');
    const target = $('#target');
    const arena = $('#arena');
    const arenaCenter = $('#arenaCenter');
    const bubble = $('#bubble');
    const character = $('#character');
    const startButton = $('#startButton');
    const micButton = $('#micButton');
    const muteButton = $('#muteButton');

    bestStat.textContent = state.best;

    function render() {
      scoreStat.textContent = state.score;
      comboStat.innerHTML = `${state.combo}<span>x</span>`;
      timeStat.innerHTML = `${state.time}<span>s</span>`;
      bestStat.textContent = state.best;
    }

    function speak(text, mood = 'happy') {
      bubble.textContent = text;
      character.classList.remove('excited');
      void character.offsetWidth;
      if (mood === 'excited') character.classList.add('excited');
      if (!state.muted && 'speechSynthesis' in window) {
        window.speechSynthesis.cancel();
        const utterance = new SpeechSynthesisUtterance(text);
        utterance.lang = 'pt-BR';
        utterance.rate = mood === 'excited' ? 1.08 : .98;
        utterance.pitch = mood === 'excited' ? 1.22 : 1.08;
        window.speechSynthesis.speak(utterance);
      }
    }

    function tone(frequency = 500, duration = .08) {
      if (state.muted) return;
      try {
        const context = new (window.AudioContext || window.webkitAudioContext)();
        const oscillator = context.createOscillator();
        const gain = context.createGain();
        oscillator.frequency.value = frequency;
        oscillator.type = 'sine';
        gain.gain.setValueAtTime(.035, context.currentTime);
        gain.gain.exponentialRampToValueAtTime(.001, context.currentTime + duration);
        oscillator.connect(gain).connect(context.destination);
        oscillator.start();
        oscillator.stop(context.currentTime + duration);
      } catch (_) { /* áudio é um extra opcional */ }
    }

    function placeTarget() {
      const padding = 11;
      const x = padding + Math.random() * (100 - padding * 2);
      const y = padding + Math.random() * (100 - padding * 2);
      target.style.left = `${x}%`;
      target.style.top = `${y}%`;
      target.hidden = false;
    }

    function startGame() {
      if (state.running) return;
      state.score = 0;
      state.combo = 0;
      state.time = 30;
      state.hits = 0;
      state.running = true;
      arenaCenter.hidden = true;
      render();
      placeTarget();
      tone(620, .12);
      speak('Valendo! Mostra esse reflexo, eu estou de olho.', 'excited');
      state.interval = setInterval(() => {
        state.time -= 1;
        render();
        if (state.time <= 0) endGame();
      }, 1000);
    }

    function endGame() {
      clearInterval(state.interval);
      state.running = false;
      target.hidden = true;
      arenaCenter.hidden = false;
      const isNewBest = state.score > state.best;
      if (isNewBest) {
        state.best = state.score;
        localStorage.setItem('playmate-best', state.best);
      }
      $('#startButton').textContent = 'Jogar novamente';
      render();
      if (isNewBest && state.score > 0) {
        speak(`Novo recorde! ${state.score} pontos. Eu sabia que você conseguia!`, 'excited');
      } else if (state.score > 100) {
        speak(`Fim de jogo! ${state.score} pontos. Foi uma ótima partida!`, 'happy');
      } else {
        speak(`Fim de jogo! ${state.score} pontos. Na próxima a gente chega mais longe.`, 'happy');
      }
    }

    function miss() {
      if (!state.running) return;
      state.combo = 0;
      render();
      speak('Quase! Respira e mira no próximo.', 'happy');
      tone(220, .08);
      placeTarget();
    }

    target.addEventListener('click', (event) => {
      event.stopPropagation();
      if (!state.running) return;
      state.hits += 1;
      state.combo += 1;
      state.score += 10 + Math.min(state.combo * 2, 20);
      render();
      tone(480 + Math.min(state.combo * 30, 260), .07);
      placeTarget();
      if (state.combo === 3) speak('Boa! Você encontrou o ritmo.', 'happy');
      else if (state.combo === 5) speak('Combo de cinco! Agora ninguém segura.', 'excited');
      else if (state.combo > 0 && state.combo % 10 === 0) speak(`${state.combo} de combo! Isso foi absurdo!`, 'excited');
      else if (state.hits === 1) speak('Aí! Começou bem.', 'happy');
    });

    arena.addEventListener('click', (event) => {
      if (event.target === arena || event.target.classList.contains('arena-label')) miss();
    });
    startButton.addEventListener('click', startGame);

    document.querySelectorAll('.quick').forEach((button) => {
      button.addEventListener('click', () => {
        const message = button.dataset.message;
        if (message.includes('dica')) {
          speak(state.running ? 'Fica de olho nas bordas e não deixa o combo cair.' : 'Começa uma partida e eu vou te dando dicas pelo caminho.');
        } else if (message.includes('pronto')) {
          speak(state.running ? 'Sempre! Continua assim, campeão.' : 'Nascido pronto. Aperta começar e vem comigo!', 'excited');
        } else if (message.includes('comemora')) {
          speak(state.score > 0 ? `Aêê! ${state.score} pontos e contando!` : 'A comemoração começa depois do primeiro alvo. Vai!', 'excited');
        } else {
          speak(state.running ? `Você está com ${state.score} pontos e combo de ${state.combo}. Está mandando bem!` : `Seu melhor placar é ${state.best}. Vamos bater esse recorde?`);
        }
      });
    });

    muteButton.addEventListener('click', () => {
      state.muted = !state.muted;
      muteButton.textContent = state.muted ? '🔇' : '🔊';
      muteButton.setAttribute('aria-label', state.muted ? 'Ativar voz' : 'Desativar voz');
      if (!state.muted) speak('Voz ativada. Estou te ouvindo!');
      else bubble.textContent = 'Voz desativada. Ainda posso falar por texto.';
    });

    function setupSpeech() {
      const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
      if (!SpeechRecognition) {
        micButton.addEventListener('click', () => speak('Seu navegador não liberou o microfone, mas você pode falar comigo pelos botões rápidos.'));
        return;
      }
      state.recognition = new SpeechRecognition();
      state.recognition.lang = 'pt-BR';
      state.recognition.interimResults = false;
      state.recognition.onstart = () => { micButton.classList.add('listening'); micButton.textContent = '◉ Estou ouvindo...'; };
      state.recognition.onend = () => { micButton.classList.remove('listening'); micButton.textContent = '◉ Falar com Milo'; };
      state.recognition.onerror = () => speak('Não consegui ouvir. Tenta de novo, mais perto do microfone.');
      state.recognition.onresult = (event) => {
        const said = event.results[0][0].transcript.toLowerCase();
        if (said.includes('começar') || said.includes('jogar') || said.includes('vamos')) {
          speak('Bora! A partida vai começar.', 'excited');
          if (!state.running) startGame();
        } else if (said.includes('parar') || said.includes('pausar')) {
          if (state.running) endGame();
          else speak('A partida já está parada.');
        } else if (said.includes('dica') || said.includes('ajuda')) {
          speak(state.running ? 'Procura o brilho e clica rápido. O próximo alvo pode aparecer em qualquer lugar.' : 'Aperta começar e eu te acompanho.');
        } else if (said.includes('pontuação') || said.includes('placar') || said.includes('indo')) {
          speak(`Seu placar agora é ${state.score}, com combo de ${state.combo}.`);
        } else {
          speak(`Você disse “${said}”. Gostei! Fala começar, dica ou placar para eu reagir.`);
        }
      };
      micButton.addEventListener('click', () => {
        try { state.recognition.start(); } catch (_) { /* reconhecimento já iniciado */ }
      });
    }

    $('#resetButton').addEventListener('click', () => {
      state.best = 0;
      localStorage.removeItem('playmate-best');
      render();
      speak('Recorde zerado. Uma nova história começa agora!');
    });

    setupSpeech();
    render();
  </script>
</body>
</html>
