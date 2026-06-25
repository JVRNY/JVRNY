
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
  <title>JVRNY - 3D Gacha</title>
  <style>
    :root {
      --bg: #0a0a1a;
      --gold: #f7c744;
      --purple: #c084fc;
      --blue: #60a5fa;
      --common: #9ca3af;
    }
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      background: var(--bg);
      overflow: hidden;
      font-family: 'Segoe UI', system-ui, sans-serif;
      touch-action: manipulation;
      user-select: none;
      -webkit-user-select: none;
      -webkit-tap-highlight-color: transparent;
      height: 100vh;
      width: 100vw;
      position: fixed;
    }
    #gameCanvas {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 1;
    }
    #ui {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 10;
      pointer-events: none;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;
      padding: 20px 16px;
    }
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      width: 100%;
      pointer-events: auto;
    }
    .title {
      font-size: clamp(1.5rem, 8vw, 2.4rem);
      font-weight: 900;
      letter-spacing: 6px;
      color: #fff;
      text-shadow: 0 0 20px rgba(247,199,68,0.7);
    }
    .currency {
      display: flex;
      align-items: center;
      gap: 8px;
      background: rgba(20,20,40,0.7);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      padding: 12px 20px;
      border-radius: 40px;
      border: 1px solid rgba(255,255,255,0.15);
      color: #fff;
      font-weight: 700;
      font-size: 1.1rem;
      letter-spacing: 1px;
    }
    .currency span { color: var(--gold); font-size: 1.3rem; margin-right: 4px; }
    .footer {
      width: 100%;
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
      justify-content: center;
      align-items: center;
      pointer-events: auto;
      padding-bottom: 10px;
    }
    .btn {
      background: rgba(30,30,60,0.75);
      backdrop-filter: blur(15px);
      -webkit-backdrop-filter: blur(15px);
      border: 1px solid rgba(255,255,255,0.25);
      padding: 14px 28px;
      border-radius: 50px;
      color: #fff;
      font-weight: 700;
      font-size: clamp(0.9rem, 4vw, 1.2rem);
      letter-spacing: 2px;
      cursor: pointer;
      transition: all 0.2s ease;
      box-shadow: 0 8px 24px rgba(0,0,0,0.4);
      pointer-events: auto;
      touch-action: manipulation;
    }
    .btn:active {
      transform: scale(0.94);
      background: rgba(60,60,100,0.8);
    }
    .btn-gold {
      border-color: var(--gold);
      color: var(--gold);
      background: rgba(247,199,68,0.12);
    }
    .btn-purple {
      border-color: var(--purple);
      color: var(--purple);
      background: rgba(192,132,252,0.1);
    }
    .btn:disabled {
      opacity: 0.5;
      pointer-events: none;
      filter: grayscale(0.5);
    }
    .result-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 50;
      display: flex;
      align-items: center;
      justify-content: center;
      background: rgba(5,5,20,0.8);
      backdrop-filter: blur(8px);
      -webkit-backdrop-filter: blur(8px);
      pointer-events: none;
      opacity: 0;
      transition: opacity 0.3s ease;
    }
    .result-overlay.active {
      opacity: 1;
      pointer-events: auto;
    }
    .result-card {
      background: rgba(20,20,45,0.9);
      border-radius: 32px;
      padding: 30px 25px;
      text-align: center;
      border: 2px solid rgba(255,255,255,0.2);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      box-shadow: 0 20px 50px rgba(0,0,0,0.7);
      max-width: 320px;
      width: 85%;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 15px;
    }
    .rarity-badge {
      font-size: 1rem;
      font-weight: 800;
      letter-spacing: 3px;
      padding: 6px 20px;
      border-radius: 20px;
      background: rgba(255,255,255,0.08);
    }
    .rarity-Common { color: var(--common); border: 1px solid var(--common); }
    .rarity-Rare { color: var(--blue); border: 1px solid var(--blue); }
    .rarity-Epic { color: var(--purple); border: 1px solid var(--purple); }
    .rarity-Legendary { color: var(--gold); border: 1px solid var(--gold); box-shadow: 0 0 20px var(--gold); }
    .chara-name {
      font-size: 1.8rem;
      font-weight: 900;
      letter-spacing: 2px;
      color: #fff;
    }
    .btn-collect {
      margin-top: 10px;
      padding: 12px 36px;
      background: rgba(255,255,255,0.15);
      border: 1px solid rgba(255,255,255,0.3);
      border-radius: 30px;
      color: #fff;
      font-weight: 700;
      letter-spacing: 2px;
      font-size: 1rem;
      cursor: pointer;
      transition: 0.2s;
    }
    .btn-collect:active { background: rgba(255,255,255,0.25); }
    .inventory-btn {
      position: absolute;
      top: 20px;
      right: 20px;
      background: none;
      border: none;
      color: #fff;
      font-size: 1.8rem;
      cursor: pointer;
      pointer-events: auto;
      filter: drop-shadow(0 0 8px rgba(255,255,255,0.5));
      z-index: 20;
    }
    .inventory-panel {
      position: fixed;
      bottom: 0;
      left: 0;
      width: 100%;
      max-height: 60vh;
      background: rgba(10,10,30,0.95);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      border-radius: 30px 30px 0 0;
      padding: 25px 20px;
      z-index: 40;
      transform: translateY(100%);
      transition: transform 0.3s ease;
      overflow-y: auto;
      color: #fff;
      display: flex;
      flex-direction: column;
      gap: 10px;
      border-top: 1px solid rgba(255,255,255,0.2);
    }
    .inventory-panel.open {
      transform: translateY(0%);
    }
    .inventory-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 12px 15px;
      background: rgba(255,255,255,0.05);
      border-radius: 16px;
      font-size: 1rem;
    }
    .inv-name { font-weight: 700; }
    .inv-count { color: var(--gold); }
    .close-inv {
      align-self: flex-end;
      background: rgba(255,255,255,0.1);
      border: none;
      color: #fff;
      padding: 8px 18px;
      border-radius: 20px;
      font-weight: 700;
    }
    @media (max-width: 600px) {
      .btn { padding: 12px 20px; }
      .footer { gap: 8px; }
    }
  </style>
</head>
<body>
  <canvas id="gameCanvas"></canvas>

  <div id="ui">
    <div class="header">
      <div class="title">JVRNY</div>
      <div class="currency">
        <span>✦</span> <span id="orb-count">100</span>
      </div>
    </div>
    <button class="inventory-btn" id="inventory-toggle">📦</button>
    <div class="footer">
      <button class="btn btn-gold" id="pull-single">PULL x1 <span style="color:#fff">10</span></button>
      <button class="btn btn-purple" id="pull-multi">PULL x10 <span style="color:#fff">90</span></button>
    </div>
  </div>

  <div class="result-overlay" id="result-overlay">
    <div class="result-card">
      <div class="rarity-badge" id="rarity-badge">RARITY</div>
      <div class="chara-name" id="chara-name">NAME</div>
      <button class="btn-collect" id="collect-btn">COLLECT</button>
    </div>
  </div>

  <div class="inventory-panel" id="inventory-panel">
    <button class="close-inv" id="close-inventory">✕ Close</button>
    <div id="inventory-list"></div>
  </div>

  <script type="importmap">
    {
      "imports": {
        "three": "https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js"
      }
    }
  </script>

  <script type="module">
    import * as THREE from 'three';

    // ──────────────────────── DOM ────────────────────────
    const canvas = document.getElementById('gameCanvas');
    const orbCountEl = document.getElementById('orb-count');
    const pullSingleBtn = document.getElementById('pull-single');
    const pullMultiBtn = document.getElementById('pull-multi');
    const resultOverlay = document.getElementById('result-overlay');
    const rarityBadge = document.getElementById('rarity-badge');
    const charaNameEl = document.getElementById('chara-name');
    const collectBtn = document.getElementById('collect-btn');
    const inventoryToggle = document.getElementById('inventory-toggle');
    const inventoryPanel = document.getElementById('inventory-panel');
    const inventoryList = document.getElementById('inventory-list');
    const closeInventoryBtn = document.getElementById('close-inventory');

    // ──────────────────────── THREE.JS SETUP ────────────────────────
    const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: false });
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.PCFSoftShadowMap;
    renderer.toneMapping = THREE.ACESFilmicToneMapping;
    renderer.toneMappingExposure = 1.1;

    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0x060612);
    scene.fog = new THREE.FogExp2(0x060612, 0.0008);

    const camera = new THREE.PerspectiveCamera(50, window.innerWidth / window.innerHeight, 0.3, 30);
    camera.position.set(0, 2.2, 8);
    camera.lookAt(0, 0.4, 0);

    // ──────────────────────── LIGHTING ────────────────────────
    const ambientLight = new THREE.AmbientLight(0x1a1a3a, 1.2);
    scene.add(ambientLight);

    const dirLight = new THREE.DirectionalLight(0xffffff, 1.4);
    dirLight.position.set(5, 8, 5);
    dirLight.castShadow = true;
    dirLight.shadow.mapSize.width = 1024;
    dirLight.shadow.mapSize.height = 1024;
    dirLight.shadow.camera.near = 0.5;
    dirLight.shadow.camera.far = 40;
    dirLight.shadow.camera.left = -10;
    dirLight.shadow.camera.right = 10;
    dirLight.shadow.camera.top = 10;
    dirLight.shadow.camera.bottom = -6;
    dirLight.shadow.bias = -0.0002;
    scene.add(dirLight);

    // Point lights for atmosphere
    const pointLight1 = new THREE.PointLight(0x4466aa, 2, 12);
    pointLight1.position.set(2, 1.5, 3);
    scene.add(pointLight1);
    const pointLight2 = new THREE.PointLight(0xaa4466, 1.8, 12);
    pointLight2.position.set(-2, 1.2, 2.5);
    scene.add(pointLight2);

    // ──────────────────────── ENVIRONMENT ────────────────────────
    // Ground
    const groundGeo = new THREE.CircleGeometry(12, 64);
    const groundMat = new THREE.MeshStandardMaterial({ color: 0x0d0d24, roughness: 0.7, metalness: 0.4 });
    const ground = new THREE.Mesh(groundGeo, groundMat);
    ground.rotation.x = -Math.PI / 2;
    ground.position.y = -1.1;
    ground.receiveShadow = true;
    scene.add(ground);

    // Decorative ring on ground
    const ringGeo = new THREE.TorusGeometry(2.8, 0.06, 32, 64);
    const ringMat = new THREE.MeshStandardMaterial({ color: 0x334466, roughness: 0.3, metalness: 0.8, emissive: 0x112233,
      emissiveIntensity: 0.4 });
    const ring = new THREE.Mesh(ringGeo, ringMat);
    ring.rotation.x = -Math.PI / 2;
    ring.position.y = -1.05;
    ring.receiveShadow = true;
    scene.add(ring);

    // Pedestal
    const pedestalBase = new THREE.Mesh(
      new THREE.CylinderGeometry(1.4, 1.6, 0.3, 32),
      new THREE.MeshStandardMaterial({ color: 0x1a1a2e, roughness: 0.4, metalness: 0.7 })
    );
    pedestalBase.position.y = -0.85;
    pedestalBase.receiveShadow = true;
    pedestalBase.castShadow = true;
    scene.add(pedestalBase);

    const pedestalTop = new THREE.Mesh(
      new THREE.CylinderGeometry(1.1, 1.35, 0.35, 32),
      new THREE.MeshStandardMaterial({ color: 0x2a2a44, roughness: 0.3, metalness: 0.8, emissive: 0x111122,
        emissiveIntensity: 0.25 })
    );
    pedestalTop.position.y = -0.55;
    pedestalTop.receiveShadow = true;
    pedestalTop.castShadow = true;
    scene.add(pedestalTop);

    // Floating particles background
    const bgParticlesGeo = new THREE.BufferGeometry();
    const bgCount = 400;
    const bgPositions = new Float32Array(bgCount * 3);
    for (let i = 0; i < bgCount; i++) {
      bgPositions[i * 3] = (Math.random() - 0.5) * 14;
      bgPositions[i * 3 + 1] = Math.random() * 6 + 0.5;
      bgPositions[i * 3 + 2] = (Math.random() - 0.5) * 8 + 1;
    }
    bgParticlesGeo.setAttribute('position', new THREE.BufferAttribute(bgPositions, 3));
    const bgParticlesMat = new THREE.PointsMaterial({
      color: 0x8899dd,
      size: 0.04,
      transparent: true,
      opacity: 0.55,
      blending: THREE.AdditiveBlending,
      depthWrite: false
    });
    const bgParticles = new THREE.Points(bgParticlesGeo, bgParticlesMat);
    scene.add(bgParticles);

    // ──────────────────────── GACHA CHARACTERS ────────────────────────
    const charaDefinitions = [
      { id: 0, name: 'Aether Sprite', rarity: 'Common', color: 0xcccccc, shape: 'sphere' },
      { id: 1, name: 'Stone Golem', rarity: 'Common', color: 0xaaaaaa, shape: 'box' },
      { id: 2, name: 'Wind Wisp', rarity: 'Common', color: 0xbbccdd, shape: 'torus' },
      { id: 3, name: 'Flame Cub', rarity: 'Common', color: 0xffaa66, shape: 'cone' },
      { id: 4, name: 'Aqua Droplet', rarity: 'Rare', color: 0x5599ff, shape: 'icosahedron' },
      { id: 5, name: 'Thorn Knight', rarity: 'Rare', color: 0x66aacc, shape: 'dodecahedron' },
      { id: 6, name: 'Crystal Fox', rarity: 'Rare', color: 0x99ccff, shape: 'octahedron' },
      { id: 7, name: 'Shadow Raven', rarity: 'Epic', color: 0xaa66ff, shape: 'torusKnot' },
      { id: 8, name: 'Star Empress', rarity: 'Epic', color: 0xcc88ff, shape: 'star' },
      { id: 9, name: 'Void Dragon', rarity: 'Legendary', color: 0xffd700, shape: 'dragon' },
      { id: 10, name: 'Celestial Seraph', rarity: 'Legendary', color: 0xffaa00, shape: 'seraph' }
    ];

    // Probability weights
    const rarityWeights = { Common: 60, Rare: 25, Epic: 10, Legendary: 5 };

    function weightedRandomRarity() {
      const total = Object.values(rarityWeights).reduce((a, b) => a + b, 0);
      let rand = Math.random() * total;
      for (const [rarity, weight] of Object.entries(rarityWeights)) {
        rand -= weight;
        if (rand <= 0) return rarity;
      }
      return 'Common';
    }

    function getRandomCharacter() {
      const rarity = weightedRandomRarity();
      const pool = charaDefinitions.filter(c => c.rarity === rarity);
      return pool[Math.floor(Math.random() * pool.length)];
    }

    // Create 3D model for character
    function createCharacterModel(def) {
      const group = new THREE.Group();
      const mat = new THREE.MeshStandardMaterial({
        color: def.color,
        roughness: 0.25,
        metalness: 0.6,
        emissive: def.color,
        emissiveIntensity: 0.35
      });
      const glowMat = new THREE.MeshBasicMaterial({
        color: def.color,
        transparent: true,
        opacity: 0.2,
        depthWrite: false
      });

      let geo;
      switch (def.shape) {
        case 'sphere': geo = new THREE.SphereGeometry(0.8, 32, 32); break;
        case 'box': geo = new THREE.BoxGeometry(1.3, 1.3, 1.3); break;
        case 'torus': geo = new THREE.TorusGeometry(0.65, 0.35, 16, 32); break;
        case 'cone': geo = new THREE.ConeGeometry(0.8, 1.6, 32); break;
        case 'icosahedron': geo = new THREE.IcosahedronGeometry(0.9, 0); break;
        case 'dodecahedron': geo = new THREE.DodecahedronGeometry(0.9, 0); break;
        case 'octahedron': geo = new THREE.OctahedronGeometry(0.85, 0); break;
        case 'torusKnot': geo = new THREE.TorusKnotGeometry(0.6, 0.25, 64, 8, 2, 3); break;
        case 'star':
          const starGroup = new THREE.Group();
          const spikeGeo = new THREE.ConeGeometry(0.25, 1.2, 8);
          for (let i = 0; i < 6; i++) {
            const spike = new THREE.Mesh(spikeGeo, mat);
            spike.position.y = 0.5;
            spike.rotation.x = (i / 6) * Math.PI * 2;
            spike.rotation.z = Math.PI / 2;
            starGroup.add(spike);
          }
          const core = new THREE.Mesh(new THREE.SphereGeometry(0.45, 16, 16), mat);
          starGroup.add(core);
          return starGroup;
        case 'dragon':
          const dragonGroup = new THREE.Group();
          const body = new THREE.Mesh(new THREE.CylinderGeometry(0.35, 0.5, 1.8, 8), mat);
          body.position.y = 0.2;
          dragonGroup.add(body);
          const head = new THREE.Mesh(new THREE.SphereGeometry(0.55, 16, 16), mat);
          head.position.y = 1.15;
          dragonGroup.add(head);
          const wings = new THREE.Mesh(new THREE.ConeGeometry(0.9, 0.2, 4), mat);
          wings.position.y = 0.6;
          wings.rotation.x = Math.PI / 2;
          dragonGroup.add(wings);
          return dragonGroup;
        case 'seraph':
          const seraphGroup = new THREE.Group();
          const mainSphere = new THREE.Mesh(new THREE.SphereGeometry(0.7, 32, 32), mat);
          seraphGroup.add(mainSphere);
          const ring1 = new THREE.Mesh(new THREE.TorusGeometry(0.9, 0.08, 16, 32), mat);
          ring1.rotation.x = Math.PI / 2;
          seraphGroup.add(ring1);
          const ring2 = new THREE.Mesh(new THREE.TorusGeometry(0.9, 0.08, 16, 32), mat);
          ring2.rotation.z = Math.PI / 2;
          seraphGroup.add(ring2);
          return seraphGroup;
        default: geo = new THREE.SphereGeometry(0.8, 32, 32);
      }

      if (geo) {
        const main = new THREE.Mesh(geo, mat);
        main.castShadow = true;
        main.receiveShadow = true;
        group.add(main);
        const glow = new THREE.Mesh(geo, glowMat);
        glow.scale.set(1.25, 1.25, 1.25);
        group.add(glow);
      }
      return group;
    }

    // ──────────────────────── GAME STATE ────────────────────────
    let orbs = 100;
    const pullCostSingle = 10;
    const pullCostMulti = 90;
    const inventory = new Map(); // id -> count
    let currentModel = null;
    let isPulling = false;
    let pullState = 'idle'; // idle, animating, displaying
    let animStartTime = 0;
    const ANIM_DURATION = 2.2; // seconds for full reveal
    let lastPulledChar = null;

    // ──────────────────────── AUDIO (simple) ────────────────────────
    let audioCtx = null;
    function initAudio() {
      if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    }
    function playChime(freq = 660, duration = 0.15) {
      if (!audioCtx) return;
      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.type = 'sine';
      osc.frequency.value = freq;
      gain.gain.setValueAtTime(0.2, audioCtx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + duration);
      osc.connect(gain);
      gain.connect(audioCtx.destination);
      osc.start();
      osc.stop(audioCtx.currentTime + duration);
    }
    function playPullSound() {
      playChime(440, 0.1);
      setTimeout(() => playChime(550, 0.1), 100);
      setTimeout(() => playChime(660, 0.12), 200);
    }
    function playRaritySound(rarity) {
      if (rarity === 'Legendary') {
        playChime(880, 0.3);
        setTimeout(() => playChime(1100, 0.4), 150);
      } else if (rarity === 'Epic') {
        playChime(770, 0.2);
        setTimeout(() => playChime(880, 0.2), 120);
      } else if (rarity === 'Rare') {
        playChime(660, 0.15);
      }
    }

    // ──────────────────────── UI UPDATE ────────────────────────
    function updateOrbDisplay() {
      orbCountEl.textContent = orbs;
      pullSingleBtn.disabled = (orbs < pullCostSingle || isPulling);
      pullMultiBtn.disabled = (orbs < pullCostMulti || isPulling);
    }

    function addToInventory(charDef) {
      const count = inventory.get(charDef.id) || 0;
      inventory.set(charDef.id, count + 1);
    }

    function renderInventory() {
      inventoryList.innerHTML = '';
      if (inventory.size === 0) {
        inventoryList.innerHTML = '<div class="inventory-item" style="justify-content:center; opacity:0.7;">No characters yet</div>';
        return;
      }
      for (const [id, count] of inventory) {
        const def = charaDefinitions.find(c => c.id === id);
        if (!def) continue;
        const item = document.createElement('div');
        item.className = 'inventory-item';
        item.innerHTML = `<span class="inv-name" style="color:${getRarityColor(def.rarity)}">${def.name}</span>
                          <span class="inv-count">x${count}</span>`;
        inventoryList.appendChild(item);
      }
    }

    function getRarityColor(rarity) {
      switch (rarity) {
        case 'Common': return '#9ca3af';
        case 'Rare': return '#60a5fa';
        case 'Epic': return '#c084fc';
        case 'Legendary': return '#f7c744';
        default: return '#fff';
      }
    }

    function showResult(charDef) {
      rarityBadge.textContent = charDef.rarity.toUpperCase();
      rarityBadge.className = 'rarity-badge rarity-' + charDef.rarity;
      charaNameEl.textContent = charDef.name;
      charaNameEl.style.color = getRarityColor(charDef.rarity);
      resultOverlay.classList.add('active');
      playRaritySound(charDef.rarity);
    }

    function hideResult() {
      resultOverlay.classList.remove('active');
      if (pullState === 'displaying') {
        pullState = 'idle';
        isPulling = false;
        updateOrbDisplay();
        if (currentModel) {
          scene.remove(currentModel);
          currentModel = null;
        }
      }
    }

    // ──────────────────────── PULL ANIMATION ────────────────────────
    function startPullAnimation(charDef) {
      isPulling = true;
      pullState = 'animating';
      animStartTime = performance.now() / 1000;
      lastPulledChar = charDef;

      // Remove previous model if any
      if (currentModel) {
        scene.remove(currentModel);
        currentModel = null;
      }

      // Create model at pedestal position hidden
      currentModel = createCharacterModel(charDef);
      currentModel.position.set(0, -0.2, 0);
      currentModel.scale.set(0.01, 0.01, 0.01);
      scene.add(currentModel);

      updateOrbDisplay();
      playPullSound();
    }

    function updateAnimation(now) {
      if (pullState !== 'animating' && pullState !== 'displaying') return;
      const elapsed = now - animStartTime;
      if (!currentModel) return;

      if (pullState === 'animating') {
        // Phase 1: gather energy (0 to 0.8s) -> scale up, rotate
        const t1 = Math.min(elapsed / 0.8, 1.0);
        const scale = 0.01 + t1 * 1.0;
        currentModel.scale.set(scale, scale, scale);
        currentModel.position.y = -0.2 + t1 * 0.6;
        currentModel.rotation.y += 0.02;
        currentModel.rotation.x = Math.sin(elapsed * 5) * 0.3;

        if (elapsed >= 0.8 && elapsed < ANIM_DURATION) {
          pullState = 'displaying';
          // Ensure full scale
          currentModel.scale.set(1, 1, 1);
          currentModel.position.y = 0.4;
          showResult(lastPulledChar);
        }
      } else if (pullState === 'displaying') {
        // Rotate slowly
        currentModel.rotation.y += 0.015;
        currentModel.position.y = 0.4 + Math.sin(elapsed * 3) * 0.1;
      }
    }

    // ──────────────────────── PULL LOGIC ────────────────────────
    function performPull(cost) {
      if (isPulling) return;
      if (orbs < cost) return;
      initAudio();
      orbs -= cost;
      const charDef = getRandomCharacter();
      addToInventory(charDef);
      startPullAnimation(charDef);
      updateOrbDisplay();
    }

    function pullSingle() { performPull(pullCostSingle); }
    function pullMulti() {
      if (isPulling) return;
      if (orbs < pullCostMulti) return;
      initAudio();
      // For multi, we pull 10 times but show only the last? Or a summary. For simplicity, pull 10 and show best rarity.
      // We'll pull sequentially but only animate the best? We'll do a quick summary: pull all, display best.
      const pulls = [];
      for (let i = 0; i < 10; i++) {
        const char = getRandomCharacter();
        pulls.push(char);
        addToInventory(char);
      }
      // Find best by rarity order
      const rarityOrder = { Common: 0, Rare: 1, Epic: 2, Legendary: 3 };
      pulls.sort((a, b) => rarityOrder[b.rarity] - rarityOrder[a.rarity]);
      const best = pulls[0];
      orbs -= pullCostMulti;
      startPullAnimation(best);
      updateOrbDisplay();
    }

    // ──────────────────────── EVENT LISTENERS ────────────────────────
    pullSingleBtn.addEventListener('click', pullSingle);
    pullMultiBtn.addEventListener('click', pullMulti);
    collectBtn.addEventListener('click', hideResult);
    resultOverlay.addEventListener('click', (e) => {
      if (e.target === resultOverlay) hideResult();
    });

    inventoryToggle.addEventListener('click', () => {
      renderInventory();
      inventoryPanel.classList.add('open');
    });
    closeInventoryBtn.addEventListener('click', () => {
      inventoryPanel.classList.remove('open');
    });
    inventoryPanel.addEventListener('click', (e) => {
      if (e.target === inventoryPanel) inventoryPanel.classList.remove('open');
    });

    // Touch support for buttons is native

    // ──────────────────────── ANIMATION LOOP ────────────────────────
    function animate() {
      requestAnimationFrame(animate);
      const now = performance.now() / 1000;

      // Update pull animation
      if (pullState === 'animating' || pullState === 'displaying') {
        updateAnimation(now);
      }

      // Rotate background particles
      bgParticles.rotation.y += 0.0005;
      bgParticles.rotation.x += 0.0002;
      const positions = bgParticles.geometry.attributes.position.array;
      for (let i = 0; i < bgCount; i++) {
        positions[i * 3 + 1] += 0.004;
        if (positions[i * 3 + 1] > 6) positions[i * 3 + 1] = 0.5;
      }
      bgParticles.geometry.attributes.position.needsUpdate = true;

      // Pulse ring
      ring.scale.setScalar(1 + Math.sin(now * 4) * 0.02);

      renderer.render(scene, camera);
    }

    // ──────────────────────── RESIZE ────────────────────────
    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    }); <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
  <title>JVRNY - 3D Gacha</title>
  <style>
    :root {
      --bg: #0a0a1a;
      --gold: #f7c744;
      --purple: #c084fc;
      --blue: #60a5fa;
      --common: #9ca3af;
    }
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      background: var(--bg);
      overflow: hidden;
      font-family: 'Segoe UI', system-ui, sans-serif;
      touch-action: manipulation;
      user-select: none;
      -webkit-user-select: none;
      -webkit-tap-highlight-color: transparent;
      height: 100vh;
      width: 100vw;
      position: fixed;
    }
    #gameCanvas {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 1;
    }
    #ui {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 10;
      pointer-events: none;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;
      padding: 20px 16px;
    }
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      width: 100%;
      pointer-events: auto;
    }
    .title {
      font-size: clamp(1.5rem, 8vw, 2.4rem);
      font-weight: 900;
      letter-spacing: 6px;
      color: #fff;
      text-shadow: 0 0 20px rgba(247,199,68,0.7);
    }
    .currency {
      display: flex;
      align-items: center;
      gap: 8px;
      background: rgba(20,20,40,0.7);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      padding: 12px 20px;
      border-radius: 40px;
      border: 1px solid rgba(255,255,255,0.15);
      color: #fff;
      font-weight: 700;
      font-size: 1.1rem;
      letter-spacing: 1px;
    }
    .currency span { color: var(--gold); font-size: 1.3rem; margin-right: 4px; }
    .footer {
      width: 100%;
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
      justify-content: center;
      align-items: center;
      pointer-events: auto;
      padding-bottom: 10px;
    }
    .btn {
      background: rgba(30,30,60,0.75);
      backdrop-filter: blur(15px);
      -webkit-backdrop-filter: blur(15px);
      border: 1px solid rgba(255,255,255,0.25);
      padding: 14px 28px;
      border-radius: 50px;
      color: #fff;
      font-weight: 700;
      font-size: clamp(0.9rem, 4vw, 1.2rem);
      letter-spacing: 2px;
      cursor: pointer;
      transition: all 0.2s ease;
      box-shadow: 0 8px 24px rgba(0,0,0,0.4);
      pointer-events: auto;
      touch-action: manipulation;
    }
    .btn:active {
      transform: scale(0.94);
      background: rgba(60,60,100,0.8);
    }
    .btn-gold {
      border-color: var(--gold);
      color: var(--gold);
      background: rgba(247,199,68,0.12);
    }
    .btn-purple {
      border-color: var(--purple);
      color: var(--purple);
      background: rgba(192,132,252,0.1);
    }
    .btn:disabled {
      opacity: 0.5;
      pointer-events: none;
      filter: grayscale(0.5);
    }
    .result-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 50;
      display: flex;
      align-items: center;
      justify-content: center;
      background: rgba(5,5,20,0.8);
      backdrop-filter: blur(8px);
      -webkit-backdrop-filter: blur(8px);
      pointer-events: none;
      opacity: 0;
      transition: opacity 0.3s ease;
    }
    .result-overlay.active {
      opacity: 1;
      pointer-events: auto;
    }
    .result-card {
      background: rgba(20,20,45,0.9);
      border-radius: 32px;
      padding: 30px 25px;
      text-align: center;
      border: 2px solid rgba(255,255,255,0.2);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      box-shadow: 0 20px 50px rgba(0,0,0,0.7);
      max-width: 320px;
      width: 85%;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 15px;
    }
    .rarity-badge {
      font-size: 1rem;
      font-weight: 800;
      letter-spacing: 3px;
      padding: 6px 20px;
      border-radius: 20px;
      background: rgba(255,255,255,0.08);
    }
    .rarity-Common { color: var(--common); border: 1px solid var(--common); }
    .rarity-Rare { color: var(--blue); border: 1px solid var(--blue); }
    .rarity-Epic { color: var(--purple); border: 1px solid var(--purple); }
    .rarity-Legendary { color: var(--gold); border: 1px solid var(--gold); box-shadow: 0 0 20px var(--gold); }
    .chara-name {
      font-size: 1.8rem;
      font-weight: 900;
      letter-spacing: 2px;
      color: #fff;
    }
    .btn-collect {
      margin-top: 10px;
      padding: 12px 36px;
      background: rgba(255,255,255,0.15);
      border: 1px solid rgba(255,255,255,0.3);
      border-radius: 30px;
      color: #fff;
      font-weight: 700;
      letter-spacing: 2px;
      font-size: 1rem;
      cursor: pointer;
      transition: 0.2s;
    }
    .btn-collect:active { background: rgba(255,255,255,0.25); }
    .inventory-btn {
      position: absolute;
      top: 20px;
      right: 20px;
      background: none;
      border: none;
      color: #fff;
      font-size: 1.8rem;
      cursor: pointer;
      pointer-events: auto;
      filter: drop-shadow(0 0 8px rgba(255,255,255,0.5));
      z-index: 20;
    }
    .inventory-panel {
      position: fixed;
      bottom: 0;
      left: 0;
      width: 100%;
      max-height: 60vh;
      background: rgba(10,10,30,0.95);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      border-radius: 30px 30px 0 0;
      padding: 25px 20px;
      z-index: 40;
      transform: translateY(100%);
      transition: transform 0.3s ease;
      overflow-y: auto;
      color: #fff;
      display: flex;
      flex-direction: column;
      gap: 10px;
      border-top: 1px solid rgba(255,255,255,0.2);
    }
    .inventory-panel.open {
      transform: translateY(0%);
    }
    .inventory-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 12px 15px;
      background: rgba(255,255,255,0.05);
      border-radius: 16px;
      font-size: 1rem;
    }
    .inv-name { font-weight: 700; }
    .inv-count { color: var(--gold); }
    .close-inv {
      align-self: flex-end;
      background: rgba(255,255,255,0.1);
      border: none;
      color: #fff;
      padding: 8px 18px;
      border-radius: 20px;
      font-weight: 700;
    }
    @media (max-width: 600px) {
      .btn { padding: 12px 20px; }
      .footer { gap: 8px; }
    }
  </style>
</head>
<body>
  <canvas id="gameCanvas"></canvas>

  <div id="ui">
    <div class="header">
      <div class="title">JVRNY</div>
      <div class="currency">
        <span>✦</span> <span id="orb-count">100</span>
      </div>
    </div>
    <button class="inventory-btn" id="inventory-toggle">📦</button>
    <div class="footer">
      <button class="btn btn-gold" id="pull-single">PULL x1 <span style="color:#fff">10</span></button>
      <button class="btn btn-purple" id="pull-multi">PULL x10 <span style="color:#fff">90</span></button>
    </div>
  </div>

  <div class="result-overlay" id="result-overlay">
    <div class="result-card">
      <div class="rarity-badge" id="rarity-badge">RARITY</div>
      <div class="chara-name" id="chara-name">NAME</div>
      <button class="btn-collect" id="collect-btn">COLLECT</button>
    </div>
  </div>

  <div class="inventory-panel" id="inventory-panel">
    <button class="close-inv" id="close-inventory">✕ Close</button>
    <div id="inventory-list"></div>
  </div>

  <script type="importmap">
    {
      "imports": {
        "three": "https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js"
      }
    }
  </script>

  <script type="module">
    import * as THREE from 'three';

    // ──────────────────────── DOM ────────────────────────
    const canvas = document.getElementById('gameCanvas');
    const orbCountEl = document.getElementById('orb-count');
    const pullSingleBtn = document.getElementById('pull-single');
    const pullMultiBtn = document.getElementById('pull-multi');
    const resultOverlay = document.getElementById('result-overlay');
    const rarityBadge = document.getElementById('rarity-badge');
    const charaNameEl = document.getElementById('chara-name');
    const collectBtn = document.getElementById('collect-btn');
    const inventoryToggle = document.getElementById('inventory-toggle');
    const inventoryPanel = document.getElementById('inventory-panel');
    const inventoryList = document.getElementById('inventory-list');
    const closeInventoryBtn = document.getElementById('close-inventory');

    // ──────────────────────── THREE.JS SETUP ────────────────────────
    const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: false });
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.PCFSoftShadowMap;
    renderer.toneMapping = THREE.ACESFilmicToneMapping;
    renderer.toneMappingExposure = 1.1;

    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0x060612);
    scene.fog = new THREE.FogExp2(0x060612, 0.0008);

    const camera = new THREE.PerspectiveCamera(50, window.innerWidth / window.innerHeight, 0.3, 30);
    camera.position.set(0, 2.2, 8);
    camera.lookAt(0, 0.4, 0);

    // ──────────────────────── LIGHTING ────────────────────────
    const ambientLight = new THREE.AmbientLight(0x1a1a3a, 1.2);
    scene.add(ambientLight);

    const dirLight = new THREE.DirectionalLight(0xffffff, 1.4);
    dirLight.position.set(5, 8, 5);
    dirLight.castShadow = true;
    dirLight.shadow.mapSize.width = 1024;
    dirLight.shadow.mapSize.height = 1024;
    dirLight.shadow.camera.near = 0.5;
    dirLight.shadow.camera.far = 40;
    dirLight.shadow.camera.left = -10;
    dirLight.shadow.camera.right = 10;
    dirLight.shadow.camera.top = 10;
    dirLight.shadow.camera.bottom = -6;
    dirLight.shadow.bias = -0.0002;
    scene.add(dirLight);

    // Point lights for atmosphere
    const pointLight1 = new THREE.PointLight(0x4466aa, 2, 12);
    pointLight1.position.set(2, 1.5, 3);
    scene.add(pointLight1);
    const pointLight2 = new THREE.PointLight(0xaa4466, 1.8, 12);
    pointLight2.position.set(-2, 1.2, 2.5);
    scene.add(pointLight2);

    // ──────────────────────── ENVIRONMENT ────────────────────────
    // Ground
    const groundGeo = new THREE.CircleGeometry(12, 64);
    const groundMat = new THREE.MeshStandardMaterial({ color: 0x0d0d24, roughness: 0.7, metalness: 0.4 });
    const ground = new THREE.Mesh(groundGeo, groundMat);
    ground.rotation.x = -Math.PI / 2;
    ground.position.y = -1.1;
    ground.receiveShadow = true;
    scene.add(ground);

    // Decorative ring on ground
    const ringGeo = new THREE.TorusGeometry(2.8, 0.06, 32, 64);
    const ringMat = new THREE.MeshStandardMaterial({ color: 0x334466, roughness: 0.3, metalness: 0.8, emissive: 0x112233,
      emissiveIntensity: 0.4 });
    const ring = new THREE.Mesh(ringGeo, ringMat);
    ring.rotation.x = -Math.PI / 2;
    ring.position.y = -1.05;
    ring.receiveShadow = true;
    scene.add(ring);

    // Pedestal
    const pedestalBase = new THREE.Mesh(
      new THREE.CylinderGeometry(1.4, 1.6, 0.3, 32),
      new THREE.MeshStandardMaterial({ color: 0x1a1a2e, roughness: 0.4, metalness: 0.7 })
    );
    pedestalBase.position.y = -0.85;
    pedestalBase.receiveShadow = true;
    pedestalBase.castShadow = true;
    scene.add(pedestalBase);

    const pedestalTop = new THREE.Mesh(
      new THREE.CylinderGeometry(1.1, 1.35, 0.35, 32),
      new THREE.MeshStandardMaterial({ color: 0x2a2a44, roughness: 0.3, metalness: 0.8, emissive: 0x111122,
        emissiveIntensity: 0.25 })
    );
    pedestalTop.position.y = -0.55;
    pedestalTop.receiveShadow = true;
    pedestalTop.castShadow = true;
    scene.add(pedestalTop);

    // Floating particles background
    const bgParticlesGeo = new THREE.BufferGeometry();
    const bgCount = 400;
    const bgPositions = new Float32Array(bgCount * 3);
    for (let i = 0; i < bgCount; i++) {
      bgPositions[i * 3] = (Math.random() - 0.5) * 14;
      bgPositions[i * 3 + 1] = Math.random() * 6 + 0.5;
      bgPositions[i * 3 + 2] = (Math.random() - 0.5) * 8 + 1;
    }
    bgParticlesGeo.setAttribute('position', new THREE.BufferAttribute(bgPositions, 3));
    const bgParticlesMat = new THREE.PointsMaterial({
      color: 0x8899dd,
      size: 0.04,
      transparent: true,
      opacity: 0.55,
      blending: THREE.AdditiveBlending,
      depthWrite: false
    });
    const bgParticles = new THREE.Points(bgParticlesGeo, bgParticlesMat);
    scene.add(bgParticles);

    // ──────────────────────── GACHA CHARACTERS ────────────────────────
    const charaDefinitions = [
      { id: 0, name: 'Aether Sprite', rarity: 'Common', color: 0xcccccc, shape: 'sphere' },
      { id: 1, name: 'Stone Golem', rarity: 'Common', color: 0xaaaaaa, shape: 'box' },
      { id: 2, name: 'Wind Wisp', rarity: 'Common', color: 0xbbccdd, shape: 'torus' },
      { id: 3, name: 'Flame Cub', rarity: 'Common', color: 0xffaa66, shape: 'cone' },
      { id: 4, name: 'Aqua Droplet', rarity: 'Rare', color: 0x5599ff, shape: 'icosahedron' },
      { id: 5, name: 'Thorn Knight', rarity: 'Rare', color: 0x66aacc, shape: 'dodecahedron' },
      { id: 6, name: 'Crystal Fox', rarity: 'Rare', color: 0x99ccff, shape: 'octahedron' },
      { id: 7, name: 'Shadow Raven', rarity: 'Epic', color: 0xaa66ff, shape: 'torusKnot' },
      { id: 8, name: 'Star Empress', rarity: 'Epic', color: 0xcc88ff, shape: 'star' },
      { id: 9, name: 'Void Dragon', rarity: 'Legendary', color: 0xffd700, shape: 'dragon' },
      { id: 10, name: 'Celestial Seraph', rarity: 'Legendary', color: 0xffaa00, shape: 'seraph' }
    ];

    // Probability weights
    const rarityWeights = { Common: 60, Rare: 25, Epic: 10, Legendary: 5 };

    function weightedRandomRarity() {
      const total = Object.values(rarityWeights).reduce((a, b) => a + b, 0);
      let rand = Math.random() * total;
      for (const [rarity, weight] of Object.entries(rarityWeights)) {
        rand -= weight;
        if (rand <= 0) return rarity;
      }
      return 'Common';
    }

    function getRandomCharacter() {
      const rarity = weightedRandomRarity();
      const pool = charaDefinitions.filter(c => c.rarity === rarity);
      return pool[Math.floor(Math.random() * pool.length)];
    }

    // Create 3D model for character
    function createCharacterModel(def) {
      const group = new THREE.Group();
      const mat = new THREE.MeshStandardMaterial({
        color: def.color,
        roughness: 0.25,
        metalness: 0.6,
        emissive: def.color,
        emissiveIntensity: 0.35
      });
      const glowMat = new THREE.MeshBasicMaterial({
        color: def.color,
        transparent: true,
        opacity: 0.2,
        depthWrite: false
      });

      let geo;
      switch (def.shape) {
        case 'sphere': geo = new THREE.SphereGeometry(0.8, 32, 32); break;
        case 'box': geo = new THREE.BoxGeometry(1.3, 1.3, 1.3); break;
        case 'torus': geo = new THREE.TorusGeometry(0.65, 0.35, 16, 32); break;
        case 'cone': geo = new THREE.ConeGeometry(0.8, 1.6, 32); break;
        case 'icosahedron': geo = new THREE.IcosahedronGeometry(0.9, 0); break;
        case 'dodecahedron': geo = new THREE.DodecahedronGeometry(0.9, 0); break;
        case 'octahedron': geo = new THREE.OctahedronGeometry(0.85, 0); break;
        case 'torusKnot': geo = new THREE.TorusKnotGeometry(0.6, 0.25, 64, 8, 2, 3); break;
        case 'star':
          const starGroup = new THREE.Group();
          const spikeGeo = new THREE.ConeGeometry(0.25, 1.2, 8);
          for (let i = 0; i < 6; i++) {
            const spike = new THREE.Mesh(spikeGeo, mat);
            spike.position.y = 0.5;
            spike.rotation.x = (i / 6) * Math.PI * 2;
            spike.rotation.z = Math.PI / 2;
            starGroup.add(spike);
          }
          const core = new THREE.Mesh(new THREE.SphereGeometry(0.45, 16, 16), mat);
          starGroup.add(core);
          return starGroup;
        case 'dragon':
          const dragonGroup = new THREE.Group();
          const body = new THREE.Mesh(new THREE.CylinderGeometry(0.35, 0.5, 1.8, 8), mat);
          body.position.y = 0.2;
          dragonGroup.add(body);
          const head = new THREE.Mesh(new THREE.SphereGeometry(0.55, 16, 16), mat);
          head.position.y = 1.15;
          dragonGroup.add(head);
          const wings = new THREE.Mesh(new THREE.ConeGeometry(0.9, 0.2, 4), mat);
          wings.position.y = 0.6;
          wings.rotation.x = Math.PI / 2;
          dragonGroup.add(wings);
          return dragonGroup;
        case 'seraph':
          const seraphGroup = new THREE.Group();
          const mainSphere = new THREE.Mesh(new THREE.SphereGeometry(0.7, 32, 32), mat);
          seraphGroup.add(mainSphere);
          const ring1 = new THREE.Mesh(new THREE.TorusGeometry(0.9, 0.08, 16, 32), mat);
          ring1.rotation.x = Math.PI / 2;
          seraphGroup.add(ring1);
          const ring2 = new THREE.Mesh(new THREE.TorusGeometry(0.9, 0.08, 16, 32), mat);
          ring2.rotation.z = Math.PI / 2;
          seraphGroup.add(ring2);
          return seraphGroup;
        default: geo = new THREE.SphereGeometry(0.8, 32, 32);
      }

      if (geo) {
        const main = new THREE.Mesh(geo, mat);
        main.castShadow = true;
        main.receiveShadow = true;
        group.add(main);
        const glow = new THREE.Mesh(geo, glowMat);
        glow.scale.set(1.25, 1.25, 1.25);
        group.add(glow);
      }
      return group;
    }

    // ──────────────────────── GAME STATE ────────────────────────
    let orbs = 100;
    const pullCostSingle = 10;
    const pullCostMulti = 90;
    const inventory = new Map(); // id -> count
    let currentModel = null;
    let isPulling = false;
    let pullState = 'idle'; // idle, animating, displaying
    let animStartTime = 0;
    const ANIM_DURATION = 2.2; // seconds for full reveal
    let lastPulledChar = null;

    // ──────────────────────── AUDIO (simple) ────────────────────────
    let audioCtx = null;
    function initAudio() {
      if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    }
    function playChime(freq = 660, duration = 0.15) {
      if (!audioCtx) return;
      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.type = 'sine';
      osc.frequency.value = freq;
      gain.gain.setValueAtTime(0.2, audioCtx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + duration);
      osc.connect(gain);
      gain.connect(audioCtx.destination);
      osc.start();
      osc.stop(audioCtx.currentTime + duration);
    }
    function playPullSound() {
      playChime(440, 0.1);
      setTimeout(() => playChime(550, 0.1), 100);
      setTimeout(() => playChime(660, 0.12), 200);
    }
    function playRaritySound(rarity) {
      if (rarity === 'Legendary') {
        playChime(880, 0.3);
        setTimeout(() => playChime(1100, 0.4), 150);
      } else if (rarity === 'Epic') {
        playChime(770, 0.2);
        setTimeout(() => playChime(880, 0.2), 120);
      } else if (rarity === 'Rare') {
        playChime(660, 0.15);
      }
    }

    // ──────────────────────── UI UPDATE ────────────────────────
    function updateOrbDisplay() {
      orbCountEl.textContent = orbs;
      pullSingleBtn.disabled = (orbs < pullCostSingle || isPulling);
      pullMultiBtn.disabled = (orbs < pullCostMulti || isPulling);
    }

    function addToInventory(charDef) {
      const count = inventory.get(charDef.id) || 0;
      inventory.set(charDef.id, count + 1);
    }

    function renderInventory() {
      inventoryList.innerHTML = '';
      if (inventory.size === 0) {
        inventoryList.innerHTML = '<div class="inventory-item" style="justify-content:center; opacity:0.7;">No characters yet</div>';
        return;
      }
      for (const [id, count] of inventory) {
        const def = charaDefinitions.find(c => c.id === id);
        if (!def) continue;
        const item = document.createElement('div');
        item.className = 'inventory-item';
        item.innerHTML = `<span class="inv-name" style="color:${getRarityColor(def.rarity)}">${def.name}</span>
                          <span class="inv-count">x${count}</span>`;
        inventoryList.appendChild(item);
      }
    }

    function getRarityColor(rarity) {
      switch (rarity) {
        case 'Common': return '#9ca3af';
        case 'Rare': return '#60a5fa';
        case 'Epic': return '#c084fc';
        case 'Legendary': return '#f7c744';
        default: return '#fff';
      }
    }

    function showResult(charDef) {
      rarityBadge.textContent = charDef.rarity.toUpperCase();
      rarityBadge.className = 'rarity-badge rarity-' + charDef.rarity;
      charaNameEl.textContent = charDef.name;
      charaNameEl.style.color = getRarityColor(charDef.rarity);
      resultOverlay.classList.add('active');
      playRaritySound(charDef.rarity);
    }

    function hideResult() {
      resultOverlay.classList.remove('active');
      if (pullState === 'displaying') {
        pullState = 'idle';
        isPulling = false;
        updateOrbDisplay();
        if (currentModel) {
          scene.remove(currentModel);
          currentModel = null;
        }
      }
    }

    // ──────────────────────── PULL ANIMATION ────────────────────────
    function startPullAnimation(charDef) {
      isPulling = true;
      pullState = 'animating';
      animStartTime = performance.now() / 1000;
      lastPulledChar = charDef;

      // Remove previous model if any
      if (currentModel) {
        scene.remove(currentModel);
        currentModel = null;
      }

      // Create model at pedestal position hidden
      currentModel = createCharacterModel(charDef);
      currentModel.position.set(0, -0.2, 0);
      currentModel.scale.set(0.01, 0.01, 0.01);
      scene.add(currentModel);

      updateOrbDisplay();
      playPullSound();
    }

    function updateAnimation(now) {
      if (pullState !== 'animating' && pullState !== 'displaying') return;
      const elapsed = now - animStartTime;
      if (!currentModel) return;

      if (pullState === 'animating') {
        // Phase 1: gather energy (0 to 0.8s) -> scale up, rotate
        const t1 = Math.min(elapsed / 0.8, 1.0);
        const scale = 0.01 + t1 * 1.0;
        currentModel.scale.set(scale, scale, scale);
        currentModel.position.y = -0.2 + t1 * 0.6;
        currentModel.rotation.y += 0.02;
        currentModel.rotation.x = Math.sin(elapsed * 5) * 0.3;

        if (elapsed >= 0.8 && elapsed < ANIM_DURATION) {
          pullState = 'displaying';
          // Ensure full scale
          currentModel.scale.set(1, 1, 1);
          currentModel.position.y = 0.4;
          showResult(lastPulledChar);
        }
      } else if (pullState === 'displaying') {
        // Rotate slowly
        currentModel.rotation.y += 0.015;
        currentModel.position.y = 0.4 + Math.sin(elapsed * 3) * 0.1;
      }
    }

    // ──────────────────────── PULL LOGIC ────────────────────────
    function performPull(cost) {
      if (isPulling) return;
      if (orbs < cost) return;
      initAudio();
      orbs -= cost;
      const charDef = getRandomCharacter();
      addToInventory(charDef);
      startPullAnimation(charDef);
      updateOrbDisplay();
    }

    function pullSingle() { performPull(pullCostSingle); }
    function pullMulti() {
      if (isPulling) return;
      if (orbs < pullCostMulti) return;
      initAudio();
      // For multi, we pull 10 times but show only the last? Or a summary. For simplicity, pull 10 and show best rarity.
      // We'll pull sequentially but only animate the best? We'll do a quick summary: pull all, display best.
      const pulls = [];
      for (let i = 0; i < 10; i++) {
        const char = getRandomCharacter();
        pulls.push(char);
        addToInventory(char);
      }
      // Find best by rarity order
      const rarityOrder = { Common: 0, Rare: 1, Epic: 2, Legendary: 3 };
      pulls.sort((a, b) => rarityOrder[b.rarity] - rarityOrder[a.rarity]);
      const best = pulls[0];
      orbs -= pullCostMulti;
      startPullAnimation(best);
      updateOrbDisplay();
    }

    // ──────────────────────── EVENT LISTENERS ────────────────────────
    pullSingleBtn.addEventListener('click', pullSingle);
    pullMultiBtn.addEventListener('click', pullMulti);
    collectBtn.addEventListener('click', hideResult);
    resultOverlay.addEventListener('click', (e) => {
      if (e.target === resultOverlay) hideResult();
    });

    inventoryToggle.addEventListener('click', () => {
      renderInventory();
      inventoryPanel.classList.add('open');
    });
    closeInventoryBtn.addEventListener('click', () => {
      inventoryPanel.classList.remove('open');
    });
    inventoryPanel.addEventListener('click', (e) => {
      if (e.target === inventoryPanel) inventoryPanel.classList.remove('open');
    });

    // Touch support for buttons is native

    // ──────────────────────── ANIMATION LOOP ────────────────────────
    function animate() {
      requestAnimationFrame(animate);
      const now = performance.now() / 1000;

      // Update pull animation
      if (pullState === 'animating' || pullState === 'displaying') {
        updateAnimation(now);
      }

      // Rotate background particles
      bgParticles.rotation.y += 0.0005;
      bgParticles.rotation.x += 0.0002;
      const positions = bgParticles.geometry.attributes.position.array;
      for (let i = 0; i < bgCount; i++) {
        positions[i * 3 + 1] += 0.004;
        if (positions[i * 3 + 1] > 6) positions[i * 3 + 1] = 0.5;
      }
      bgParticles.geometry.attributes.position.needsUpdate = true;

      // Pulse ring
      ring.scale.setScalar(1 + Math.sin(now * 4) * 0.02);

      renderer.render(scene, camera);
    }

    // ──────────────────────── RESIZE ────────────────────────
    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    });

    // ──────────────────────── START ────────────────────────
    updateOrbDisplay();
    animate();

    console.log('🌟 JVRNY Gacha ready! Pull for legendary characters.');
  </script>
</body>
</html>

    // ──────────────────────── START ────────────────────────
    updateOrbDisplay();
    animate();

    console.log('🌟 JVRNY Gacha ready! Pull for legendary characters.');
  </script>
</body>
</html>
