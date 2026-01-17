<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Tron GPT</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600&display=swap" rel="stylesheet">
  <style>
    :root{
      --accent:#00fff7;
      --bg1:#0f2027;
      --bg2:#000;
      --card-bg: rgba(0,255,247,0.04);
      --glass: rgba(255,255,255,0.02);
      --radius:12px;
      --mono: 'Orbitron', sans-serif;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family:var(--mono);
      background: radial-gradient(circle at top, var(--bg1), var(--bg2));
      color:var(--accent);
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      min-height:100vh;
      display:flex;
      flex-direction:column;
    }
    header{
      display:flex;
      align-items:center;
      justify-content:space-between;
      padding:18px 28px;
      border-bottom:1px solid rgba(0,255,247,0.12);
      backdrop-filter: blur(4px);
    }
    .logo{display:flex;align-items:center;gap:14px}
    .logo svg{width:48px;height:48px}
    h1{font-size:1.6rem;margin:0}
    main{max-width:1100px;margin:28px auto;padding:0 20px;flex:1;width:100%}
    .grid{display:grid;grid-template-columns:1fr 420px;gap:22px}
    @media (max-width:980px){.grid{grid-template-columns:1fr}}
    .card{
      background:linear-gradient(180deg, rgba(0,255,247,0.02), rgba(0,0,0,0.12));
      border:1px solid rgba(0,255,247,0.12);
      border-radius:var(--radius);
      padding:18px;
      box-shadow:0 6px 18px rgba(0,0,0,0.6);
    }
    label{display:block;font-size:0.85rem;color:var(--accent);margin-bottom:6px}
    textarea,input,select,button{
      width:100%;
      padding:12px;
      margin-top:8px;
      background:#000;
      color:var(--accent);
      border:1px solid rgba(0,255,247,0.12);
      border-radius:8px;
      font-family:var(--mono);
      font-size:0.95rem;
    }
    textarea{min-height:140px;resize:vertical}
    .row{display:flex;gap:10px;align-items:center}
    .small{width:120px}
    button{
      cursor:pointer;
      transition:all .18s ease;
      background:transparent;
      color:var(--accent);
      border:1px solid rgba(0,255,247,0.14);
    }
    button.primary{background:var(--accent);color:#000;border:none}
    button:active{transform:translateY(1px)}
    footer{text-align:center;padding:14px;border-top:1px solid rgba(0,255,247,0.06);font-size:0.9rem}
    .consent{
      display:flex;gap:10px;align-items:center;margin-top:10px;
      font-size:0.9rem;color:rgba(0,255,247,0.9)
    }
    .toggle{display:inline-flex;align-items:center;gap:8px}
    .status{font-weight:600}
    .log{font-family:monospace;font-size:0.85rem;color:#9ff;max-height:160px;overflow:auto;padding:8px;background:rgba(0,0,0,0.25);border-radius:8px;margin-top:10px}
  </style>
</head>
<body>
<header>
  <div class="logo">
    <svg viewBox="0 0 100 100" aria-hidden="true">
      <circle cx="50" cy="50" r="45" stroke="#00fff7" stroke-width="4" fill="none"/>
      <polygon points="50,15 80,75 20,75" fill="#00fff7"/>
    </svg>
    <div>
      <h1>Tron GPT</h1>
      <div style="font-size:0.85rem;color:rgba(0,255,247,0.8)">Secure demo • Backend required</div>
    </div>
  </div>
  <div style="display:flex;gap:12px;align-items:center">
    <div style="font-size:0.9rem;color:rgba(0,255,247,0.8)">Permissions</div>
    <button id="openConsent">Manage</button>
  </div>
</header>

<main>
  <div class="grid">
    <div>
      <div class="card" id="chatCard">
        <h2>AI Assistant</h2>
        <label for="chatInput">Ask Tron GPT anything</label>
        <textarea id="chatInput" placeholder="Type your question..."></textarea>
        <div class="row" style="margin-top:12px">
          <select id="chatModel" class="small">
            <option value="default">Default</option>
            <option value="creative">Creative</option>
          </select>
          <button id="sendChat" class="primary">Generate Response</button>
          <button id="clearChat">Clear</button>
        </div>
        <div class="log" id="chatLog" aria-live="polite"></div>
      </div>

      <div class="card" id="imageCard" style="margin-top:18px">
        <h2>Image Generation</h2>
        <label for="imagePrompt">Describe the image</label>
        <input id="imagePrompt" placeholder="e.g., neon city skyline at dusk, cinematic" />
        <div class="row" style="margin-top:10px">
          <button id="genImage">Generate Image</button>
          <button id="viewImages">View Results</button>
        </div>
        <div class="log" id="imageLog"></div>
      </div>

      <div class="card" id="audioCard" style="margin-top:18px">
        <h2>Audio / Voice</h2>
        <label for="audioText">Text to convert to audio</label>
        <input id="audioText" placeholder="Enter text to convert to audio" />
        <div class="row" style="margin-top:10px">
          <button id="genAudio">Generate Audio</button>
          <button id="playAudio">Play Last</button>
        </div>
        <div class="log" id="audioLog"></div>
      </div>
    </div>

    <div>
      <div class="card">
        <h2>Video Generation</h2>
        <label for="videoPrompt">Describe the video concept</label>
        <input id="videoPrompt" placeholder="Short concept, style, duration" />
        <div class="row" style="margin-top:10px">
          <button id="genVideo">Generate Video</button>
        </div>
        <div class="log" id="videoLog"></div>
      </div>

      <div class="card" style="margin-top:18px">
        <h2>Settings & Permissions</h2>
        <div class="consent">
          <div class="toggle">
            <input type="checkbox" id="allowImage" />
            <label for="allowImage">Image</label>
          </div>
          <div class="toggle">
            <input type="checkbox" id="allowAudio" />
            <label for="allowAudio">Audio</label>
          </div>
          <div class="toggle">
            <input type="checkbox" id="allowVideo" />
            <label for="allowVideo">Video</label>
          </div>
        </div>
        <div style="margin-top:12px;font-size:0.9rem;color:rgba(0,255,247,0.8)">
          Consent is stored locally. The server will still enforce policy and validation.
        </div>
      </div>
    </div>
  </div>
</main>

<footer>© 2026 Nyasha John • Tron GPT</footer>

<script>
  // --- Configuration: change only the base API path if your server uses a different route
  const API_BASE = '/api';

  // --- Utility: append log
  function appendLog(elId, text){
    const el = document.getElementById(elId);
    const p = document.createElement('div');
    p.textContent = `[${new Date().toLocaleTimeString()}] ${text}`;
    el.prepend(p);
  }

  // --- Load consent from localStorage
  document.getElementById('allowImage').checked = localStorage.getItem('allowImage') === 'true';
  document.getElementById('allowAudio').checked = localStorage.getItem('allowAudio') === 'true';
  document.getElementById('allowVideo').checked = localStorage.getItem('allowVideo') === 'true';

  ['allowImage','allowAudio','allowVideo'].forEach(id=>{
    document.getElementById(id).addEventListener('change', e=>{
      localStorage.setItem(id, e.target.checked);
      appendLog('chatLog', `Permission ${id} set to ${e.target.checked}`);
    });
  });

  // --- Chat
  document.getElementById('sendChat').addEventListener('click', async ()=>{
    const prompt = document.getElementById('chatInput').value.trim();
    if(!prompt){ appendLog('chatLog','Please enter a question.'); return; }
    appendLog('chatLog','Sending request to server...');
    try{
      const res = await fetch(`${API_BASE}/chat`, {
        method:'POST',
        headers:{'Content-Type':'application/json'},
        body: JSON.stringify({prompt})
      });
      if(!res.ok){ throw new Error('Server error'); }
      const data = await res.json();
      appendLog('chatLog', data.response || 'No response text.');
    }catch(err){
      appendLog('chatLog','Error: ' + err.message);
    }
  });

  document.getElementById('clearChat').addEventListener('click', ()=>{
    document.getElementById('chatInput').value = '';
    document.getElementById('chatLog').innerHTML = '';
  });

  // --- Image
  document.getElementById('genImage').addEventListener('click', async ()=>{
    if(!localStorage.getItem('allowImage') || localStorage.getItem('allowImage') !== 'true'){
      appendLog('imageLog','Image permission not granted locally.');
      return;
    }
    const prompt = document.getElementById('imagePrompt').value.trim();
    if(!prompt){ appendLog('imageLog','Please describe the image.'); return; }
    appendLog('imageLog','Requesting image generation...');
    try{
      const res = await fetch(`${API_BASE}/image`, {
        method:'POST',
        headers:{'Content-Type':'application/json'},
        body: JSON.stringify({prompt})
      });
      const data = await res.json();
      appendLog('imageLog', data.message || 'Image request submitted.');
    }catch(err){
      appendLog('imageLog','Error: ' + err.message);
    }
  });

  // --- Audio
  document.getElementById('genAudio').addEventListener('click', async ()=>{
    if(!localStorage.getItem('allowAudio') || localStorage.getItem('allowAudio') !== 'true'){
      appendLog('audioLog','Audio permission not granted locally.');
      return;
    }
    const text = document.getElementById('audioText').value.trim();
    if(!text){ appendLog('audioLog','Please enter text.'); return; }
    appendLog('audioLog','Requesting audio generation...');
    try{
      const res = await fetch(`${API_BASE}/audio`, {
        method:'POST',
        headers:{'Content-Type':'application/json'},
        body: JSON.stringify({text})
      });
      const data = await res.json();
      appendLog('audioLog', data.message || 'Audio request submitted.');
      // server may return a URL to play; handle accordingly
    }catch(err){
      appendLog('audioLog','Error: ' + err.message);
    }
  });

  // --- Video
  document.getElementById('genVideo').addEventListener('click', async ()=>{
    if(!localStorage.getItem('allowVideo') || localStorage.getItem('allowVideo') !== 'true'){
      appendLog('videoLog','Video permission not granted locally.');
      return;
    }
    const prompt = document.getElementById('videoPrompt').value.trim();
    if(!prompt){ appendLog('videoLog','Please describe the video concept.'); return; }
    appendLog('videoLog','Requesting video generation...');
    try{
      const res = await fetch(`${API_BASE}/video`, {
        method:'POST',
        headers:{'Content-Type':'application/json'},
        body: JSON.stringify({prompt})
      });
      const data = await res.json();
      appendLog('videoLog', data.message || 'Video request submitted.');
    }catch(err){
      appendLog('videoLog','Error: ' + err.message);
    }
  });

  // --- Simple consent modal opener (for demo)
  document.getElementById('openConsent').addEventListener('click', ()=>{
    const allowImage = confirm('Allow image generation from this browser? (OK = allow)');
    localStorage.setItem('allowImage', allowImage);
    appendLog('chatLog', `Image permission set to ${allowImage}`);
  });
</script>
</body>
</html>
