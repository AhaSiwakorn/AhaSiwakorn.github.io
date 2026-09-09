# AhaSiwakorn.github.io
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Siwakorn Mukdaprakorn — 3D Portfolio</title>
  <meta name="description" content="Siwakorn Mukdaprakorn — 3D / Creative Coding Portfolio built with Three.js." />
  <style>
    :root {
      --bg: #05060a;
      --panel: rgba(11, 13, 20, 0.74);
      --line: rgba(255,255,255,.10);
      --text: #f5f7ff;
      --muted: #a5adbf;
      --cyan: #5ee7ff;
      --violet: #9a7cff;
      --green: #6dffbd;
    }
    * { box-sizing: border-box; }
    html { scroll-behavior: smooth; }
    body {
      margin: 0;
      background: var(--bg);
      color: var(--text);
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      overflow-x: hidden;
    }
    #app {
      position: fixed;
      inset: 0;
      z-index: 0;
    }
    canvas { display: block; }
    .grain {
      position: fixed;
      inset: 0;
      z-index: 1;
      pointer-events: none;
      opacity: .055;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 160 160' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.85' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='.75'/%3E%3C/svg%3E");
    }
    .ui {
      position: relative;
      z-index: 2;
      pointer-events: none;
    }
    .nav, .hero, .section, footer {
      pointer-events: auto;
    }
    .nav {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 22px 34px;
      background: linear-gradient(to bottom, rgba(5,6,10,.82), transparent);
      backdrop-filter: blur(6px);
    }
    .brand {
      font-weight: 700;
      letter-spacing: .08em;
      text-transform: uppercase;
      font-size: 13px;
    }
    .nav a {
      color: var(--muted);
      text-decoration: none;
      font-size: 12px;
      margin-left: 22px;
      transition: color .2s ease;
    }
    .nav a:hover { color: white; }
    .hero {
      min-height: 100vh;
      display: flex;
      align-items: flex-end;
      padding: 0 7vw 9vh;
    }
    .hero-inner { max-width: 760px; }
    .eyebrow {
      color: var(--cyan);
      font-size: 12px;
      letter-spacing: .22em;
      text-transform: uppercase;
      margin-bottom: 15px;
    }
    h1 {
      font-size: clamp(52px, 9vw, 132px);
      line-height: .9;
      letter-spacing: -.065em;
      margin: 0 0 24px;
      max-width: 1000px;
    }
    .hero p {
      max-width: 650px;
      color: #cbd1df;
      line-height: 1.7;
      font-size: 15px;
      margin: 0 0 28px;
    }
    .chips { display: flex; gap: 8px; flex-wrap: wrap; }
    .chip {
      border: 1px solid var(--line);
      background: rgba(255,255,255,.045);
      padding: 8px 12px;
      border-radius: 999px;
      color: #d7dceb;
      font-size: 11px;
      letter-spacing: .04em;
      backdrop-filter: blur(8px);
    }

    .section {
      width: min(1180px, calc(100% - 40px));
      margin: 0 auto;
      min-height: 100vh;
      padding: 120px 0;
      display: flex;
      align-items: center;
    }
    .section-inner {
      width: 100%;
      border: 1px solid var(--line);
      background: linear-gradient(145deg, rgba(16,18,27,.82), rgba(8,9,14,.58));
      border-radius: 28px;
      padding: 40px;
      box-shadow: 0 30px 90px rgba(0,0,0,.35);
      backdrop-filter: blur(12px);
    }
    .section-title {
      display: flex;
      justify-content: space-between;
      align-items: end;
      gap: 20px;
      margin-bottom: 26px;
    }
    .section-title h2 {
      margin: 0;
      font-size: clamp(30px, 4vw, 52px);
      letter-spacing: -.04em;
    }
    .section-title p {
      margin: 0;
      color: var(--muted);
      max-width: 460px;
      line-height: 1.6;
      font-size: 14px;
    }
    .model-grid {
      display: grid;
      grid-template-columns: 1.3fr .7fr;
      gap: 20px;
    }
    .card {
      border: 1px solid var(--line);
      border-radius: 20px;
      background: rgba(255,255,255,.025);
      padding: 22px;
      min-height: 220px;
    }
    .viewer {
      min-height: 570px;
      position: relative;
      overflow: hidden;
      padding: 0;
      background:
        radial-gradient(circle at 45% 40%, rgba(94,231,255,.10), transparent 25%),
        radial-gradient(circle at 70% 60%, rgba(154,124,255,.09), transparent 30%),
        rgba(0,0,0,.20);
    }
    .viewer canvas {
      width: 100% !important;
      height: 100% !important;
    }
    .viewer-ui {
      position: absolute;
      left: 18px;
      right: 18px;
      bottom: 18px;
      display: flex;
      justify-content: space-between;
      gap: 10px;
      pointer-events: none;
      font-size: 11px;
      color: #c6ccda;
    }
    .status {
      padding: 7px 10px;
      border-radius: 999px;
      background: rgba(0,0,0,.45);
      border: 1px solid var(--line);
      backdrop-filter: blur(8px);
    }
    .specs h3, .specs h4 { margin-top: 0; }
    .specs h3 { font-size: 20px; margin-bottom: 18px; }
    .spec {
      border-top: 1px solid var(--line);
      padding: 15px 0;
    }
    .spec b {
      display: block;
      font-size: 12px;
      margin-bottom: 5px;
    }
    .spec span {
      color: var(--muted);
      font-size: 12px;
      line-height: 1.5;
    }

    .sim-wrap {
      position: relative;
      height: 620px;
      overflow: hidden;
      border: 1px solid var(--line);
      border-radius: 20px;
      background: #030406;
    }
    #flockCanvas { width: 100%; height: 100%; }
    .sim-overlay {
      position: absolute;
      left: 18px;
      top: 18px;
      width: min(320px, calc(100% - 36px));
      padding: 18px;
      border: 1px solid var(--line);
      border-radius: 16px;
      background: rgba(3,4,6,.60);
      backdrop-filter: blur(10px);
    }
    .sim-overlay strong { font-size: 12px; letter-spacing: .1em; text-transform: uppercase; }
    .sim-overlay p { color: var(--muted); font-size: 12px; line-height: 1.6; margin: 8px 0 0; }
    .sim-controls {
      margin-top: 15px;
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }
    button {
      pointer-events: auto;
      border: 1px solid var(--line);
      background: rgba(255,255,255,.055);
      color: #e9edf8;
      padding: 8px 11px;
      border-radius: 10px;
      cursor: pointer;
      font: inherit;
      font-size: 11px;
    }
    button:hover { background: rgba(255,255,255,.09); }

    .shader-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
      align-items: stretch;
    }
    .shader-preview {
      height: 520px;
      border-radius: 20px;
      overflow: hidden;
      position: relative;
      border: 1px solid var(--line);
      background: #03040a;
    }
    #shaderCanvas { width: 100%; height: 100%; }
    .shader-label {
      position: absolute;
      left: 18px;
      bottom: 18px;
      padding: 8px 10px;
      border-radius: 999px;
      border: 1px solid var(--line);
      background: rgba(0,0,0,.45);
      backdrop-filter: blur(7px);
      font-size: 11px;
      color: #dbe2f2;
    }
    .code-panel {
      background: #070910;
      border-radius: 20px;
      border: 1px solid var(--line);
      padding: 22px;
      overflow: hidden;
    }
    .code-panel .dots { display: flex; gap: 6px; margin-bottom: 18px; }
    .dots i { width: 8px; height: 8px; border-radius: 50%; background: #313746; display: block; }
    pre {
      margin: 0;
      white-space: pre-wrap;
      color: #cbd4e7;
      font: 12px/1.75 ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
    }
    .kw { color: #d4a5ff; }
    .fn { color: #7fdfff; }
    .num { color: #9cffc8; }
    .comment { color: #667186; }
    footer {
      min-height: 50vh;
      display: grid;
      place-items: center;
      text-align: center;
      padding: 100px 20px;
      color: var(--muted);
    }
    footer h2 {
      color: white;
      font-size: clamp(36px, 6vw, 72px);
      margin: 0 0 12px;
      letter-spacing: -.05em;
    }
    footer p { margin: 0; }
    .hint {
      position: fixed;
      right: 24px;
      bottom: 24px;
      z-index: 3;
      font-size: 10px;
      color: #97a0b3;
      background: rgba(0,0,0,.45);
      border: 1px solid var(--line);
      padding: 8px 10px;
      border-radius: 999px;
      backdrop-filter: blur(8px);
      pointer-events: none;
    }
    @media (max-width: 820px) {
      .nav { padding: 18px 18px; }
      .nav nav { display: none; }
      .section { padding: 80px 0; width: min(100% - 20px, 1180px); }
      .section-inner { padding: 22px; border-radius: 20px; }
      .model-grid, .shader-grid { grid-template-columns: 1fr; }
      .viewer { min-height: 430px; }
      .sim-wrap { height: 510px; }
      .shader-preview { height: 420px; }
      .hero { padding: 0 24px 48px; }
      .hint { display: none; }
    }
  </style>
</head>
<body>
  <div id="app"></div>
  <div class="grain"></div>

  <div class="ui">
    <header class="nav">
      <div class="brand">SM / 3D PORTFOLIO</div>
      <nav>
        <a href="#model">PBR MODEL</a>
        <a href="#simulation">SIMULATION</a>
        <a href="#shader">SHADER</a>
      </nav>
    </header>

    <main>
      <section class="hero">
        <div class="hero-inner">
          <div class="eyebrow">Creative Coding · 3D · Real-time Graphics</div>
          <h1>Siwakorn<br />Mukdaprakorn</h1>
          <p>
            พอร์ตโฟลิโอ 3D แบบ real-time ที่รวมงานโมเดล PBR, interactive simulation
            และ custom shader เข้าด้วยกัน — ทุกองค์ประกอบตอบสนองต่อเมาส์และเรนเดอร์ด้วย Three.js
          </p>
          <div class="chips">
            <span class="chip">Three.js</span>
            <span class="chip">WebGL / GLSL</span>
            <span class="chip">PBR / GLTF</span>
            <span class="chip">Boids Simulation</span>
          </div>
        </div>
      </section>

      <section id="model" class="section">
        <div class="section-inner">
          <div class="section-title">
            <div>
              <div class="eyebrow">01 / PBR ASSET</div>
              <h2>Physically Based Material</h2>
            </div>
            <p>
              ตัวอย่าง GLB จาก Khronos glTF Sample Assets ใช้ Material แบบ PBR
              พร้อม environment lighting, tone mapping และ OrbitControls
            </p>
          </div>

          <div class="model-grid">
            <div class="card viewer" id="modelViewer">
              <div class="viewer-ui">
                <div class="status" id="modelStatus">Loading GLB…</div>
                <div class="status">Drag = Orbit · Wheel = Zoom</div>
              </div>
            </div>
            <div class="card specs">
              <h3>Asset Breakdown</h3>
              <div class="spec">
                <b>Format</b>
                <span>GLB / glTF 2.0</span>
              </div>
              <div class="spec">
                <b>Material</b>
                <span>Metallic-Roughness PBR</span>
              </div>
              <div class="spec">
                <b>Lighting</b>
                <span>HDR-style environment + key/fill lights</span>
              </div>
              <div class="spec">
                <b>Rendering</b>
                <span>ACES Filmic-style tone mapping, high output color space</span>
              </div>
              <div class="spec">
                <b>Interaction</b>
                <span>Mouse orbit / zoom / damping</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="simulation" class="section">
        <div class="section-inner">
          <div class="section-title">
            <div>
              <div class="eyebrow">02 / SIMULATION</div>
              <h2>Flock / Boids</h2>
            </div>
            <p>
              ฝูงอนุภาคจำลองด้วยกฎ Separation, Alignment และ Cohesion
              พร้อมแรงดึงดูดจากตำแหน่งเมาส์แบบ real-time
            </p>
          </div>

          <div class="sim-wrap">
            <canvas id="flockCanvas"></canvas>
            <div class="sim-overlay">
              <strong>Boids Simulation</strong>
              <p>
                ขยับเมาส์เพื่อสร้างแรงรบกวนให้ฝูงนก และลองเปลี่ยนโหมด
                เพื่อดูผลของกฎพฤติกรรมที่แตกต่างกัน
              </p>
              <div class="sim-controls">
                <button id="modeNormal">Normal</button>
                <button id="modeChaos">Chaos</button>
                <button id="modeCalm">Calm</button>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="shader" class="section">
        <div class="section-inner">
          <div class="section-title">
            <div>
              <div class="eyebrow">03 / ADVANCED SHADER</div>
              <h2>Procedural GLSL</h2>
            </div>
            <p>
              Custom vertex/fragment shader สำหรับสร้างพื้นผิว procedural
              โดยใช้ signed-distance style functions, domain warping, fresnel
              และ time-based animation
            </p>
          </div>

          <div class="shader-grid">
            <div class="shader-preview">
              <canvas id="shaderCanvas"></canvas>
              <div class="shader-label">GLSL · FRACTAL / FRESNEL / DOMAIN WARP</div>
            </div>

            <div class="code-panel">
              <div class="dots"><i></i><i></i><i></i></div>
<pre><span class="kw">float</span> <span class="fn">fbm</span>(vec3 p) {
  <span class="kw">float</span> value = <span class="num">0.0</span>;
  <span class="kw">float</span> amp = <span class="num">0.5</span>;

  <span class="kw">for</span> (<span class="kw">int</span> i = <span class="num">0</span>; i &lt; <span class="num">5</span>; i++) {
    value += amp * noise(p);
    p = p * <span class="num">2.02</span> + vec3(<span class="num">1.7</span>, <span class="num">9.2</span>, <span class="num">2.8</span>);
    amp *= <span class="num">0.5</span>;
  }
  <span class="kw">return</span> value;
}

<span class="comment">// domain warp + fresnel</span>
vec3 q = p + <span class="num">0.8</span> * warp(p, uTime);
<span class="kw">float</span> edge = <span class="fn">pow</span>(<span class="num">1.0</span> - max(dot(n, v), <span class="num">0.0</span>), <span class="num">3.0</span>);

<span class="kw">vec3</span> color = mix(
  vec3(<span class="num">0.05</span>, <span class="num">0.35</span>, <span class="num">0.9</span>),
  vec3(<span class="num">0.65</span>, <span class="num">0.15</span>, <span class="num">1.0</span>),
  fbm(q)
);

color += edge * vec3(<span class="num">0.2</span>, <span class="num">0.8</span>, <span class="num">1.0</span>);
<span class="kw">fragColor</span> = vec4(color, <span class="num">1.0</span>);</pre>
            </div>
          </div>
        </div>
      </section>

      <footer>
        <div>
          <div class="eyebrow">END OF PORTFOLIO</div>
          <h2>Let’s build something 3D.</h2>
          <p>Siwakorn Mukdaprakorn · Three.js · WebGL · GLSL</p>
        </div>
      </footer>
    </main>
  </div>

  <div class="hint">MOVE / DRAG / SCROLL TO INTERACT</div>

  <script type="importmap">
  {
    "imports": {
      "three": "https://cdn.jsdelivr.net/npm/three@0.180.0/build/three.module.js",
      "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.180.0/examples/jsm/"
    }
  }
  </script>

  <script type="module">
    import * as THREE from "three";
    import { OrbitControls } from "three/addons/controls/OrbitControls.js";
    import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";

    // ------------------------------------------------------------
    // Shared helpers
    // ------------------------------------------------------------
    const $ = (id) => document.getElementById(id);
    const clamp = (v, a, b) => Math.min(Math.max(v, a), b);

    // ------------------------------------------------------------
    // Floating background scene
    // ------------------------------------------------------------
    const app = $("app");
    const bgRenderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    bgRenderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    bgRenderer.setSize(window.innerWidth, window.innerHeight);
    bgRenderer.setAnimationLoop(renderBackground);
    app.appendChild(bgRenderer.domElement);

    const bgScene = new THREE.Scene();
    const bgCamera = new THREE.PerspectiveCamera(55, innerWidth / innerHeight, .1, 100);
    bgCamera.position.set(0, 0, 12);

    const bgGroup = new THREE.Group();
    bgScene.add(bgGroup);

    const starGeo = new THREE.BufferGeometry();
    const starCount = 900;
    const starPos = new Float32Array(starCount * 3);
    for (let i = 0; i < starCount; i++) {
      starPos[i * 3] = (Math.random() - .5) * 28;
      starPos[i * 3 + 1] = (Math.random() - .5) * 18;
      starPos[i * 3 + 2] = (Math.random() - .5) * 18;
    }
    starGeo.setAttribute("position", new THREE.BufferAttribute(starPos, 3));
    const stars = new THREE.Points(
      starGeo,
      new THREE.PointsMaterial({ color: 0xa8b3d6, size: .025, transparent: true, opacity: .55 })
    );
    bgGroup.add(stars);

    const orbGeo = new THREE.IcosahedronGeometry(3.8, 3);
    const orbMat = new THREE.ShaderMaterial({
      transparent: true,
      depthWrite: false,
      uniforms: { uTime: { value: 0 } },
      vertexShader: `
        varying vec3 vNormal;
        varying vec3 vWorld;
        uniform float uTime;
        void main() {
          vNormal = normalize(normalMatrix * normal);
          vec3 p = position;
          p += normal * (0.08 * sin(position.y * 2.0 + uTime));
          vec4 world = modelMatrix * vec4(p, 1.0);
          vWorld = world.xyz;
          gl_Position = projectionMatrix * viewMatrix * world;
        }
      `,
      fragmentShader: `
        varying vec3 vNormal;
        varying vec3 vWorld;
        uniform float uTime;
        void main() {
          vec3 v = normalize(cameraPosition - vWorld);
          float fres = pow(1.0 - max(dot(vNormal, v), 0.0), 3.2);
          float pulse = 0.5 + 0.5 * sin(uTime * 0.65 + vWorld.y * 1.2);
          vec3 c1 = vec3(0.03, 0.08, 0.18);
          vec3 c2 = vec3(0.28, 0.08, 0.45);
          vec3 col = mix(c1, c2, pulse);
          col += fres * vec3(0.08, 0.55, 0.9);
          gl_FragColor = vec4(col, 0.08 + fres * 0.14);
        }
      `
    });
    const orb = new THREE.Mesh(orbGeo, orbMat);
    orb.position.set(4.5, 1.2, -2.4);
    bgGroup.add(orb);

    const bgMouse = new THREE.Vector2();
    window.addEventListener("pointermove", (e) => {
      bgMouse.x = (e.clientX / innerWidth) * 2 - 1;
      bgMouse.y = -(e.clientY / innerHeight) * 2 + 1;
    });

    function renderBackground(t) {
      const time = t * 0.001;
      stars.rotation.y = time * 0.015;
      stars.rotation.x = Math.sin(time * .12) * .03;
      orb.rotation.y = time * .04;
      orbMat.uniforms.uTime.value = time;
      bgGroup.position.x += ((bgMouse.x * .35) - bgGroup.position.x) * .015;
      bgGroup.position.y += ((bgMouse.y * .2) - bgGroup.position.y) * .015;
      bgRenderer.render(bgScene, bgCamera);
    }

    // ------------------------------------------------------------
    // PBR GLB viewer
    // ------------------------------------------------------------
    const viewer = $("modelViewer");
    const modelRenderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    modelRenderer.setPixelRatio(Math.min(devicePixelRatio, 2));
    modelRenderer.setSize(viewer.clientWidth, viewer.clientHeight);
    modelRenderer.outputColorSpace = THREE.SRGBColorSpace;
    modelRenderer.toneMapping = THREE.ACESFilmicToneMapping;
    modelRenderer.toneMappingExposure = 1.08;
    viewer.appendChild(modelRenderer.domElement);

    const modelScene = new THREE.Scene();
    const modelCamera = new THREE.PerspectiveCamera(34, viewer.clientWidth / viewer.clientHeight, .01, 100);
    modelCamera.position.set(2.1, 1.25, 3.3);

    const modelRoot = new THREE.Group();
    modelScene.add(modelRoot);

    const hemi = new THREE.HemisphereLight(0x9bbcff, 0x101018, 2.4);
    modelScene.add(hemi);

    const key = new THREE.DirectionalLight(0xffffff, 4.5);
    key.position.set(4, 6, 4);
    modelScene.add(key);

    const rim = new THREE.PointLight(0x61dfff, 18, 10);
    rim.position.set(-3, 1.5, -2);
    modelScene.add(rim);

    const fill = new THREE.PointLight(0xa37bff, 12, 10);
    fill.position.set(2, -1, -3);
    modelScene.add(fill);

    const modelControls = new OrbitControls(modelCamera, modelRenderer.domElement);
    modelControls.enableDamping = true;
    modelControls.dampingFactor = 0.06;
    modelControls.minDistance = 1.5;
    modelControls.maxDistance = 7;
    modelControls.target.set(0, .45, 0);

    // Public Khronos Sample Model.
    // The loader is also configured to tolerate a fallback error state.
    const glbUrl = "https://raw.githubusercontent.com/KhronosGroup/glTF-Sample-Assets/main/Models/DamagedHelmet/glTF-Binary/DamagedHelmet.glb";
    const loader = new GLTFLoader();

    loader.load(
      glbUrl,
      (gltf) => {
        const root = gltf.scene;
        const box = new THREE.Box3().setFromObject(root);
        const size = box.getSize(new THREE.Vector3());
        const center = box.getCenter(new THREE.Vector3());
        const scale = 1.7 / Math.max(size.x, size.y, size.z);
        root.scale.setScalar(scale);
        root.position.sub(center.multiplyScalar(scale));
        root.position.y -= .35;
        root.traverse((obj) => {
          if (obj.isMesh) {
            obj.castShadow = true;
            obj.receiveShadow = true;
            if (obj.material) obj.material.envMapIntensity = 1.15;
          }
        });
        modelRoot.add(root);
        $("modelStatus").textContent = "GLB loaded · PBR active";
      },
      undefined,
      (err) => {
        console.error(err);
        $("modelStatus").textContent = "GLB failed to load · check internet / CORS";
      }
    );

    // A subtle floor reflection plane.
    const floor = new THREE.Mesh(
      new THREE.CircleGeometry(2.3, 96),
      new THREE.MeshBasicMaterial({ color: 0x0b0f18, transparent: true, opacity: .8 })
    );
    floor.rotation.x = -Math.PI / 2;
    floor.position.y = -.95;
    modelScene.add(floor);

    function renderModel(t) {
      const time = t * .001;
      modelControls.update();
      if (modelRoot.children.length) {
        modelRoot.rotation.y += 0.0018;
        modelRoot.position.y = Math.sin(time * .8) * .02;
      }
      rim.position.x = -3 + Math.sin(time * .7) * 1.3;
      modelRenderer.render(modelScene, modelCamera);
    }

    function resizeModel() {
      const w = viewer.clientWidth, h = viewer.clientHeight;
      modelCamera.aspect = w / h;
      modelCamera.updateProjectionMatrix();
      modelRenderer.setSize(w, h, false);
    }

    // ------------------------------------------------------------
    // Flock / Boids simulation
    // ------------------------------------------------------------
    const flockCanvas = $("flockCanvas");
    const fctx = flockCanvas.getContext("2d");
    let fw = 1, fh = 1, dpr = 1;
    let flockMode = "normal";
    const mouse = { x: 0, y: 0, active: false };

    const birds = [];
    const birdCount = 150;

    function makeBird(i) {
      const a = Math.random() * Math.PI * 2;
      const r = Math.random() * .42;
      return {
        x: Math.cos(a) * r + .5,
        y: Math.sin(a) * r + .5,
        vx: (Math.random() - .5) * .7,
        vy: (Math.random() - .5) * .7,
        size: 1.8 + Math.random() * 2.4,
        phase: Math.random() * Math.PI * 2
      };
    }
    for (let i = 0; i < birdCount; i++) birds.push(makeBird(i));

    function resizeFlock() {
      const rect = flockCanvas.getBoundingClientRect();
      dpr = Math.min(devicePixelRatio, 2);
      fw = rect.width;
      fh = rect.height;
      flockCanvas.width = fw * dpr;
      flockCanvas.height = fh * dpr;
      fctx.setTransform(dpr, 0, 0, dpr, 0, 0);
    }

    function resetFlock() {
      for (let i = 0; i < birds.length; i++) Object.assign(birds[i], makeBird(i));
    }

    function updateBirds(dt) {
      let sepWeight = 0.88, aliWeight = 0.52, cohWeight = 0.32, speedLimit = 1.7;
      if (flockMode === "chaos") {
        sepWeight = 1.45; aliWeight = 0.20; cohWeight = 0.22; speedLimit = 2.4;
      } else if (flockMode === "calm") {
        sepWeight = 0.38; aliWeight = 0.85; cohWeight = 0.52; speedLimit = 1.15;
      }

      const perception = Math.min(fw, fh) * 0.075;
      const perception2 = perception * perception;

      for (let i = 0; i < birds.length; i++) {
        const b = birds[i];
        let sx = 0, sy = 0, ax = 0, ay = 0, cx = 0, cy = 0, count = 0;

        for (let j = 0; j < birds.length; j++) {
          if (i === j) continue;
          const o = birds[j];
          let dx = o.x * fw - b.x * fw;
          let dy = o.y * fh - b.y * fh;
          const d2 = dx * dx + dy * dy;
          if (d2 < perception2 && d2 > 0.01) {
            const inv = 1 / Math.sqrt(d2);
            sx -= dx * inv;
            sy -= dy * inv;
            ax += o.vx;
            ay += o.vy;
            cx += o.x;
            cy += o.y;
            count++;
          }
        }

        if (count) {
          ax = ax / count - b.vx;
          ay = ay / count - b.vy;
          cx = (cx / count - b.x) * fw;
          cy = (cy / count - b.y) * fh;
          b.vx += (sx * sepWeight * .004 + ax * aliWeight * .035 + cx * cohWeight * .0007) * dt * 60;
          b.vy += (sy * sepWeight * .004 + ay * aliWeight * .035 + cy * cohWeight * .0007) * dt * 60;
        }

        // Mouse repulsion / attraction.
        if (mouse.active) {
          const mx = mouse.x - b.x * fw;
          const my = mouse.y - b.y * fh;
          const md2 = mx * mx + my * my;
          const radius = Math.min(fw, fh) * .19;
          if (md2 < radius * radius) {
            const dist = Math.sqrt(md2) + .001;
            const force = (1 - dist / radius);
            const dir = flockMode === "calm" ? 1 : -1;
            b.vx += (mx / dist) * force * dir * .018 * dt * 60;
            b.vy += (my / dist) * force * dir * .018 * dt * 60;
          }
        }

        // Soft boundaries.
        const bx = b.x * fw, by = b.y * fh;
        const edge = 90;
        if (bx < edge) b.vx += .018 * (edge - bx) / edge * dt * 60;
        if (bx > fw - edge) b.vx -= .018 * (bx - (fw - edge)) / edge * dt * 60;
        if (by < edge) b.vy += .018 * (edge - by) / edge * dt * 60;
        if (by > fh - edge) b.vy -= .018 * (by - (fh - edge)) / edge * dt * 60;

        // Tiny procedural drift.
        b.vx += Math.sin(performance.now() * .0007 + b.phase) * .0008 * dt * 60;
        b.vy += Math.cos(performance.now() * .0006 + b.phase) * .0008 * dt * 60;

        const s = Math.hypot(b.vx, b.vy);
        if (s > speedLimit) {
          b.vx = b.vx / s * speedLimit;
          b.vy = b.vy / s * speedLimit;
        }

        b.x += b.vx * dt / fw * 60;
        b.y += b.vy * dt / fh * 60;

        if (b.x < -.02) b.x = 1.02;
        if (b.x > 1.02) b.x = -.02;
        if (b.y < -.02) b.y = 1.02;
        if (b.y > 1.02) b.y = -.02;
      }
    }

    function drawFlock() {
      fctx.clearRect(0, 0, fw, fh);

      const grad = fctx.createRadialGradient(fw*.55, fh*.48, 0, fw*.55, fh*.48, Math.max(fw,fh)*.7);
      grad.addColorStop(0, "rgba(35,120,180,.10)");
      grad.addColorStop(1, "rgba(0,0,0,0)");
      fctx.fillStyle = grad;
      fctx.fillRect(0, 0, fw, fh);

      for (const b of birds) {
        const x = b.x * fw, y = b.y * fh;
        const angle = Math.atan2(b.vy, b.vx);
        const len = 8 + b.size * 1.8;
        fctx.save();
        fctx.translate(x, y);
        fctx.rotate(angle);
        fctx.beginPath();
        fctx.moveTo(len, 0);
        fctx.lineTo(-len * .65, len * .45);
        fctx.lineTo(-len * .2, 0);
        fctx.lineTo(-len * .65, -len * .45);
        fctx.closePath();
        fctx.fillStyle = "rgba(118,225,255,.82)";
        fctx.fill();
        fctx.restore();
      }
    }

    let lastFlock = performance.now();
    function animateFlock(now) {
      const dt = Math.min((now - lastFlock) / 1000, .033);
      lastFlock = now;
      updateBirds(dt);
      drawFlock();
      requestAnimationFrame(animateFlock);
    }
    requestAnimationFrame(animateFlock);

    flockCanvas.addEventListener("pointerenter", () => mouse.active = true);
    flockCanvas.addEventListener("pointerleave", () => mouse.active = false);
    flockCanvas.addEventListener("pointermove", (e) => {
      const r = flockCanvas.getBoundingClientRect();
      mouse.x = e.clientX - r.left;
      mouse.y = e.clientY - r.top;
    });
    $("modeNormal").onclick = () => flockMode = "normal";
    $("modeChaos").onclick = () => flockMode = "chaos";
    $("modeCalm").onclick = () => flockMode = "calm";

    // ------------------------------------------------------------
    // Advanced shader canvas
    // ------------------------------------------------------------
    const shaderCanvas = $("shaderCanvas");
    const shaderRenderer = new THREE.WebGLRenderer({ canvas: shaderCanvas, antialias: true });
    shaderRenderer.setPixelRatio(Math.min(devicePixelRatio, 2));
    shaderRenderer.outputColorSpace = THREE.SRGBColorSpace;

    const shaderScene = new THREE.Scene();
    const shaderCamera = new THREE.OrthographicCamera(-1, 1, 1, -1, 0, 1);

    const shaderUniforms = {
      uTime: { value: 0 },
      uPointer: { value: new THREE.Vector2(.5, .5) },
      uResolution: { value: new THREE.Vector2(1, 1) }
    };

    const shaderMaterial = new THREE.ShaderMaterial({
      uniforms: shaderUniforms,
      vertexShader: `
        varying vec2 vUv;
        void main() {
          vUv = uv;
          gl_Position = vec4(position.xy, 0.0, 1.0);
        }
      `,
      fragmentShader: `
        precision highp float;
        varying vec2 vUv;
        uniform float uTime;
        uniform vec2 uPointer;
        uniform vec2 uResolution;

        float hash(vec3 p) {
          p = fract(p * 0.3183099 + .1);
          p *= 17.0;
          return fract(p.x * p.y * p.z * (p.x + p.y + p.z));
        }

        float noise(vec3 x) {
          vec3 i = floor(x);
          vec3 f = fract(x);
          f = f*f*(3.0 - 2.0*f);

          return mix(
            mix(
              mix(hash(i + vec3(0,0,0)), hash(i + vec3(1,0,0)), f.x),
              mix(hash(i + vec3(0,1,0)), hash(i + vec3(1,1,0)), f.x), f.y
            ),
            mix(
              mix(hash(i + vec3(0,0,1)), hash(i + vec3(1,0,1)), f.x),
              mix(hash(i + vec3(0,1,1)), hash(i + vec3(1,1,1)), f.x), f.y
            ),
            f.z
          );
        }

        float fbm(vec3 p) {
          float v = 0.0;
          float a = 0.5;
          for (int i = 0; i < 5; i++) {
            v += a * noise(p);
            p = p * 2.02 + vec3(1.7, 9.2, 2.8);
            a *= 0.5;
          }
          return v;
        }

        mat2 rot(float a) {
          float c = cos(a), s = sin(a);
          return mat2(c, -s, s, c);
        }

        void main() {
          vec2 uv = (vUv - .5);
          uv.x *= uResolution.x / max(uResolution.y, 1.0);

          vec2 p0 = uv;
          p0 *= rot(uTime * .10);
          float r = length(p0);

          vec2 pointer = (uPointer - .5);
          pointer.x *= uResolution.x / max(uResolution.y, 1.0);

          vec2 p = p0 - pointer * .18;
          float warp = fbm(vec3(p * 2.6, uTime * .16));
          p += vec2(
            sin(p.y * 5.0 + uTime * .5),
            cos(p.x * 4.0 - uTime * .4)
          ) * (.06 + warp * .07);

          float d = length(p);
          float bands = sin(d * 24.0 - uTime * 1.7 + warp * 8.0);
          float glow = exp(-7.0 * abs(d - .24 - warp * .10));
          float fres = pow(1.0 - clamp(dot(normalize(vec3(p, .7)), vec3(0.0,0.0,1.0)), 0.0, 1.0), 2.7);

          vec3 a = vec3(.03, .15, .36);
          vec3 b = vec3(.34, .07, .72);
          vec3 c = vec3(.10, .95, .90);

          vec3 col = mix(a, b, smoothstep(-.7, .7, bands));
          col = mix(col, c, glow * .85);
          col += fres * vec3(.08, .6, 1.0);
          col *= 0.82 + 0.18 * sin(uTime + r * 9.0);

          float vignette = smoothstep(1.1, .12, r);
          col *= vignette;

          gl_FragColor = vec4(col, 1.0);
        }
      `
    });

    const quad = new THREE.Mesh(new THREE.PlaneGeometry(2, 2), shaderMaterial);
    shaderScene.add(quad);

    shaderCanvas.addEventListener("pointermove", (e) => {
      const r = shaderCanvas.getBoundingClientRect();
      shaderUniforms.uPointer.value.set((e.clientX - r.left) / r.width, 1 - (e.clientY - r.top) / r.height);
    });

    function resizeShader() {
      const r = shaderCanvas.getBoundingClientRect();
      shaderRenderer.setSize(r.width, r.height, false);
      shaderUniforms.uResolution.value.set(r.width * devicePixelRatio, r.height * devicePixelRatio);
    }

    function renderShader(t) {
      shaderUniforms.uTime.value = t * .001;
      shaderRenderer.render(shaderScene, shaderCamera);
    }

    // ------------------------------------------------------------
    // Main loop and scroll interaction
    // ------------------------------------------------------------
    function resizeAll() {
      bgCamera.aspect = innerWidth / innerHeight;
      bgCamera.updateProjectionMatrix();
      bgRenderer.setSize(innerWidth, innerHeight, false);
      resizeModel();
      resizeFlock();
      resizeShader();
    }
    window.addEventListener("resize", resizeAll);

    function loop(t) {
      renderModel(t);
      renderShader(t);
      requestAnimationFrame(loop);
    }
    resizeAll();
    requestAnimationFrame(loop);
  </script>
</body>
</html>
