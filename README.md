101<!doctype html>
<html lang="tr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Canlı Chat</title>
  <meta name="color-scheme" content="light dark" />
  <style>
    /* ======= Modern Reset ======= */
    *,*::before,*::after{box-sizing:border-box}
    html:focus-within{scroll-behavior:smooth}
    body,h1,h2,h3,p,ul{margin:0}
    body{min-height:100svh;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto,Arial;background:radial-gradient(1000px 400px at 120% -10%, rgba(99,102,241,.28), transparent 60%),radial-gradient(900px 400px at -10% 110%, rgba(59,130,246,.28), transparent 60%),var(--bg,#0b1020);color:var(--fg,#e8ecff)}
    img{max-width:100%;display:block}
    button,input,textarea{font:inherit}
    :root{
      --bg:#0b1020; --panel:#0f152b; --soft:#121a36; --fg:#e8ecff; --muted:#b4bfe4;
      --brand:#6366f1; --brand-2:#3b82f6; --ring:color-mix(in oklab,var(--brand),white 35%);
      --ok:#10b981; --danger:#ef4444; --warn:#f59e0b; --card:linear-gradient(180deg,rgba(255,255,255,.04),rgba(255,255,255,.02));
      --br:14px; --shadow:0 10px 30px rgba(0,0,0,.35);
    }
    @media (prefers-color-scheme:light){
      :root{ --bg:#f7f8ff; --panel:#ffffff; --soft:#eff3ff; --fg:#0c1222; --muted:#4b556b; --shadow:0 10px 30px rgba(9,12,22,.08); --card:linear-gradient(180deg,rgba(0,0,0,.03),rgba(0,0,0,.02)); }
    }

    /* ======= Layout ======= */
    .wrap{display:grid;grid-template-rows:auto 1fr auto;min-height:100svh}
    header{position:sticky;top:0;z-index:10;backdrop-filter:saturate(140%) blur(8px);background:color-mix(in oklab,var(--bg),transparent 35%);border-bottom:1px solid color-mix(in oklab,var(--soft),white 12%)}
    .nav{max-width:1100px;margin:auto;display:flex;align-items:center;gap:1rem;padding:14px 18px}
    .brand{display:flex;align-items:center;gap:.7rem;font-weight:800}
    .brand-badge{display:inline-grid;place-items:center;width:28px;height:28px;border-radius:9px;background:linear-gradient(160deg,var(--brand),var(--brand-2));color:white}
    .nav-actions{margin-left:auto;display:flex;align-items:center;gap:.6rem}
    .btn{--b:var(--brand);display:inline-flex;align-items:center;gap:.5rem;border:1px solid color-mix(in oklab,var(--b),white 20%);background:linear-gradient(180deg,color-mix(in oklab,var(--b),white 8%),var(--b));color:white;font-weight:700;padding:.6rem .85rem;border-radius:12px;cursor:pointer;box-shadow:0 6px 20px color-mix(in oklab,var(--b),black 70%);transition:transform .15s ease,box-shadow .15s ease}
    .btn:hover{transform:translateY(-1px);box-shadow:0 10px 24px color-mix(in oklab,var(--b),black 68%)}
    .btn.secondary{--b:var(--brand-2)}
    .btn.ghost{background:transparent;color:var(--fg);border:1px dashed color-mix(in oklab,var(--soft),white 20%)}

    main{max-width:1100px;margin:auto;display:grid;grid-template-columns:260px 1fr;gap:16px;padding:16px}
    @media (max-width: 860px){ main{grid-template-columns:1fr} .sidebar{order:2} .chat{order:1} }

    /* ======= Sidebar ======= */
    .panel{background:var(--card);border:1px solid color-mix(in oklab,var(--panel),white 10%);backdrop-filter:blur(6px);border-radius:var(--br);box-shadow:var(--shadow)}
    .sidebar{padding:14px}
    .field{display:grid;gap:.35rem;margin-bottom:.8rem}
    label{font-size:.9rem;color:var(--muted)}
    input,select{background:color-mix(in oklab,var(--panel),black 2%);border:1px solid color-mix(in oklab,var(--soft),white 14%);border-radius:12px;color:var(--fg);padding:.6rem .75rem}
    input:focus,select:focus,textarea:focus{outline:3px solid var(--ring);outline-offset:2px}

    .users{margin-top:.8rem;border-top:1px dashed color-mix(in oklab,var(--soft),white 14%);padding-top:.6rem}
    .user{display:flex;align-items:center;gap:.5rem;padding:.35rem .2rem;border-radius:10px}
    .dot{width:8px;height:8px;border-radius:999px;background:var(--ok);box-shadow:0 0 0 3px color-mix(in oklab,var(--ok),white 70%) inset}

    /* ======= Chat ======= */
    .chat{display:grid;grid-template-rows:auto 1fr auto;gap:10px}
    .chat-head{display:flex;align-items:center;gap:.5rem;padding:12px 14px}
    .room-badge{display:inline-flex;align-items:center;gap:.5rem;background:color-mix(in oklab,var(--brand),white 86%);color:color-mix(in oklab,var(--brand),black 30%);border:1px solid color-mix(in oklab,var(--brand),white 40%);padding:.35rem .6rem;border-radius:999px;font-weight:800}
    .messages{padding:12px;overflow:auto;display:grid;gap:8px}
    .msg{display:grid;gap:4px;padding:10px 12px;border-radius:12px;background:color-mix(in oklab,var(--panel),black 2%);border:1px solid color-mix(in oklab,var(--soft),white 12%)}
    .me{background:linear-gradient(180deg,color-mix(in oklab,var(--brand-2),white 10%),var(--brand-2));color:white;border-color:transparent}
    .meta{display:flex;gap:.6rem;align-items:center;font-size:.8rem;color:var(--muted)}
    .text{word-wrap:break-word;white-space:pre-wrap}
    .sys{background:transparent;border:1px dashed color-mix(in oklab,var(--soft),white 16%);color:var(--muted);font-style:italic}

    .composer{display:grid;grid-template-columns:1fr auto;gap:8px;padding:12px;border-top:1px solid color-mix(in oklab,var(--soft),white 12%)}
    textarea{resize:none;min-height:46px;max-height:180px;background:color-mix(in oklab,var(--panel),black 2%);border:1px solid color-mix(in oklab,var(--soft),white 14%);border-radius:12px;color:var(--fg);padding:.7rem .8rem}
    .typing{height:18px;color:var(--muted);font-size:.85rem;padding:0 12px}

    footer{max-width:1100px;margin:20px auto;padding:0 16px 24px;color:var(--muted);display:flex;justify-content:space-between;gap:12px}
    .link{color:color-mix(in oklab,var(--brand),white 15%);text-decoration:none}
  </style>
</head>
<body>
<div class="wrap">
  <header>
    <div class="nav">
      <div class="brand">
        <div class="brand-badge">💬</div>
        <div>Canlı Chat</div>
      </div>
      <div class="nav-actions">
        <button id="themeBtn" class="btn ghost" aria-pressed="false" title="Tema değiştir">🌗 Tema</button>
        <button id="connectBtn" class="btn secondary">Bağlan</button>
      </div>
    </div>
  </header>

  <main>
    <!-- Sidebar -->
    <aside class="sidebar panel">
      <div class="field">
        <label for="username">Kullanıcı adı</label>
        <input id="username" type="text" maxlength="24" placeholder="Adın" />
      </div>
      <div class="field">
        <label for="room">Oda</label>
        <input id="room" type="text" maxlength="32" value="genel" />
      </div>
      <div class="field">
        <label for="wsurl">Sunucu (WS/WSS)</label>
        <input id="wsurl" type="url" value="ws://localhost:8080" />
      </div>
      <div style="display:flex; gap:8px;">
        <button id="joinBtn" class="btn">Odaya Katıl</button>
        <button id="leaveBtn" class="btn ghost">Ayrıl</button>
      </div>

      <div class="users">
        <div style="display:flex; align-items:center; gap:.5rem; margin-bottom:.4rem;">
          <strong>Çevrimiçi</strong>
          <span id="userCount" style="color:var(--muted)"></span>
        </div>
        <div id="userList"></div>
      </div>
    </aside>

    <!-- Chat Area -->
    <section class="chat panel" aria-live="polite">
      <div class="chat-head">
        <span class="room-badge"># <span id="roomName">genel</span></span>
        <span id="status" class="typing" style="margin-left:auto;">Bağlı değil</span>
      </div>
      <div id="messages" class="messages" role="log" aria-live="polite" aria-relevant="additions"></div>
      <div class="typing" id="typing"></div>
      <form id="composer" class="composer" autocomplete="off">
        <textarea id="text" placeholder="Mesaj yaz..." required></textarea>
        <button id="sendBtn" class="btn" type="submit" aria-label="Gönder">Gönder ➤</button>
      </form>
    </section>
  </main>

  <footer>
    <div>✔️ WebSocket ile gerçek zamanlı, oda bazlı canlı chat.</div>
    <a class="link" href="#" onclick="alert('Chat’i kendi sitene gömmek için bu dosyayı <iframe> ile de kullanabilirsin.');return false;">Gömme ipucu</a>
  </footer>
</div>

<script>
(() => {
  // ======= Ayarlar / Durum =======
  const els = {
    username: document.getElementById('username'),
    room: document.getElementById('room'),
    wsurl: document.getElementById('wsurl'),
    joinBtn: document.getElementById('joinBtn'),
    leaveBtn: document.getElementById('leaveBtn'),
    connectBtn: document.getElementById('connectBtn'),
    themeBtn: document.getElementById('themeBtn'),
    roomName: document.getElementById('roomName'),
    status: document.getElementById('status'),
    messages: document.getElementById('messages'),
    typing: document.getElementById('typing'),
    composer: document.getElementById('composer'),
    text: document.getElementById('text'),
    userList: document.getElementById('userList'),
    userCount: document.getElementById('userCount'),
    sendBtn: document.getElementById('sendBtn')
  };

  const state = {
    ws: null,
    me: null,
    connected: false,
    users: [],
    typingPeers: new Set(),
    typingTimer: null,
    reconnectAttempts: 0,
    desiredJoin: null, // {username, room}
  };

  // ======= Yardımcılar =======
  const escapeHTML = (str) => str.replace(/[&<>"']/g, m => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':"&quot;","'":"&#39;"}[m]));
  const fmtTime = (iso) => new Date(iso).toLocaleTimeString([], {hour:'2-digit', minute:'2-digit'});
  const scrollToBottom = () => { els.messages.lastElementChild?.scrollIntoView({behavior:'smooth', block:'end'}); };

  function beep(ok=true){
    try{
      const ctx = new (window.AudioContext||window.webkitAudioContext)();
      const o = ctx.createOscillator();
      const g = ctx.createGain();
      o.type = ok ? 'triangle' : 'square';
      o.frequency.value = ok ? 880 : 220;
      o.connect(g); g.connect(ctx.destination);
      g.gain.value = 0.02;
      o.start();
      setTimeout(()=>{o.stop();ctx.close();}, 120);
    }catch{}
  }

  function setStatus(text, good=false){
    els.status.textContent = text;
    els.status.style.color = good ? 'var(--ok)' : 'var(--muted)';
  }

  function setUsers(list){
    state.users = list || [];
    els.userList.innerHTML = '';
    for (const u of state.users){
      const div = document.createElement('div');
      div.className = 'user';
      div.innerHTML = `<span class="dot"></span><span>${escapeHTML(u.username)}</span>`;
      els.userList.appendChild(div);
    }
    els.userCount.textContent = `(${state.users.length})`;
  }

  function addSystem(text){
    const item = document.createElement('div');
    item.className = 'msg sys';
    item.innerHTML = `<div class="meta">Sistem • ${fmtTime(new Date().toISOString())}</div><div class="text">${escapeHTML(text)}</div>`;
    els.messages.appendChild(item); scrollToBottom();
  }

  function addMessage({user, text, ts, mine=false}){
    const item = document.createElement('div');
    item.className = 'msg' + (mine ? ' me' : '');
    item.innerHTML = `
      <div class="meta"><strong>${escapeHTML(user.username)}</strong> • ${fmtTime(ts)}</div>
      <div class="text">${escapeHTML(text)}</div>
    `;
    els.messages.appendChild(item);
    if (!mine) beep(true);
    scrollToBottom();
  }

  function showTyping(){
    if (state.typingPeers.size === 0){ els.typing.textContent = ''; return; }
    const names = [...state.typingPeers].slice(0,3).map(u=>u.username);
    const more = state.typingPeers.size > 3 ? ` ve ${state.typingPeers.size-3} kişi` : '';
    els.typing.textContent = `${names.join(', ')} yazıyor${more}…`;
  }

  // ======= WS Bağlantısı =======
  function connect(){
    if (state.ws && (state.ws.readyState === WebSocket.OPEN || state.ws.readyState === WebSocket.CONNECTING)) return;

    const url = els.wsurl.value.trim();
    if (!/^wss?:\/\//i.test(url)){ alert('Geçerli bir WS/WSS adresi girin.'); return; }

    state.ws = new WebSocket(url);
    setStatus('Bağlanılıyor…');

    state.ws.addEventListener('open', () => {
      state.connected = true;
      state.reconnectAttempts = 0;
      setStatus('Bağlandı', true);
      els.connectBtn.textContent = 'Kes';
      // Eğer kullanıcı join etmek istiyorsa
      const j = state.desiredJoin || {
        username: (localStorage.getItem('username') || 'Misafir'),
        room: (localStorage.getItem('room') || 'genel')
      };
      els.username.value = j.username;
      els.room.value = j.room;
      join(j.username, j.room);
    });

    state.ws.addEventListener('message', (ev) => {
      const data = JSON.parse(ev.data);
      if (data.type === 'hello'){ /* hoşgeldin */ return; }
      if (data.type === 'joined'){
        state.me = data.me;
        els.roomName.textContent = data.room;
        addSystem(`"${data.room}" odasına katıldın.`);
        setUsers(data.users);
        return;
      }
      if (data.type === 'user-joined'){
        setUsers(data.users);
        addSystem(`${data.user.username} katıldı.`);
        return;
      }
      if (data.type === 'user-left'){
        setUsers(data.users);
        addSystem(`${data.user.username} ayrıldı.`);
        return;
      }
      if (data.type === 'chat'){
        addMessage({user: data.user, text: data.text, ts: data.ts, mine: state.me && data.user.id === state.me.id});
        return;
      }
      if (data.type === 'ack'){
        // İsteyen: gönderim başarı onayı
        return;
      }
      if (data.type === 'typing'){
        if (data.isTyping){
          state.typingPeers.add({id:data.user.id, username:data.user.username});
        } else {
          for (const u of [...state.typingPeers]) if (u.id === data.user.id) state.typingPeers.delete(u);
        }
        showTyping();
        return;
      }
      if (data.type === 'ping'){
        state.ws?.send(JSON.stringify({type:'pong'}));
        return;
      }
    });

    state.ws.addEventListener('close', () => {
      state.connected = false;
      setStatus('Bağlı değil');
      els.connectBtn.textContent = 'Bağlan';
      addSystem('Bağlantı kapandı. Yeniden bağlanmayı deniyorum…');
      // Backoff (maks 10sn)
      const t = Math.min(10000, 800 * Math.pow(1.8, state.reconnectAttempts++));
      setTimeout(connect, t);
    });

    state.ws.addEventListener('error', () => {
      setStatus('Hata oluştu');
    });
  }

  function disconnect(){
    state.desiredJoin = null;
    state.ws?.close(1000, 'Kullanıcı kapattı');
    state.ws = null;
    state.connected = false;
    els.connectBtn.textContent = 'Bağlan';
  }

  function join(username, room){
    if (!state.ws || state.ws.readyState !== WebSocket.OPEN) {
      state.desiredJoin = { username, room };
      connect(); // bağlanınca otomatik join
      return;
    }
    username = username.trim() || 'Misafir';
    room = room.trim() || 'genel';
    localStorage.setItem('username', username);
    localStorage.setItem('room', room);
    state.ws.send(JSON.stringify({type:'join', username, room}));
  }

  function send(text){
    if (!state.ws || state.ws.readyState !== WebSocket.OPEN) return;
    const tempId = crypto.randomUUID?.() || String(Math.random());
    state.ws.send(JSON.stringify({type:'chat', text, tempId}));
  }

  // ======= Eventler =======
  els.connectBtn.addEventListener('click', () => {
    state.connected ? disconnect() : connect();
  });

  els.joinBtn.addEventListener('click', () => {
    join(els.username.value, els.room.value);
  });

  els.leaveBtn.addEventListener('click', () => {
    addSystem(`"${els.room.value || 'genel'}" odasından ayrıldın.`);
    // Ayrılmak: başka boş bir odaya katılmak gibi ele alıyoruz
    join(els.username.value, '_lobi_' + Math.random().toString(36).slice(2,6));
  });

  let typingTimeout;
  function typingChanged(){
    if (!state.ws || state.ws.readyState !== WebSocket.OPEN) return;
    state.ws.send(JSON.stringify({type:'typing', isTyping:true}));
    clearTimeout(typingTimeout);
    typingTimeout = setTimeout(() => {
      state.ws?.send(JSON.stringify({type:'typing', isTyping:false}));
    }, 1200);
  }

  els.text.addEventListener('input', typingChanged);

  els.composer.addEventListener('submit', (e) => {
    e.preventDefault();
    const text = els.text.value.trim();
    if (!text) return;
    send(text);
    els.text.value = '';
    typingChanged();
  });

  // Tema
  const THEME_KEY = 'chat-theme';
  function applyTheme(mode){
    if (mode === 'dark'){
      document.documentElement.style.setProperty('--bg','#0b1020');
      document.documentElement.style.setProperty('--fg','#e8ecff');
    } else {
      document.documentElement.style.setProperty('--bg','#f7f8ff');
      document.documentElement.style.setProperty('--fg','#0c1222');
    }
    localStorage.setItem(THEME_KEY, mode);
  }
  els.themeBtn.addEventListener('click', () => {
    const cur = localStorage.getItem(THEME_KEY) || (matchMedia('(prefers-color-scheme: dark)').matches ? 'dark':'light');
    applyTheme(cur === 'dark' ? 'light' : 'dark');
  });
  // ilk yüklemede
  applyTheme(localStorage.getItem(THEME_KEY) || (matchMedia('(prefers-color-scheme: dark)').matches ? 'dark':'light'));

  // Otomatik bağlan (isteğe bağlı; kullanıcıdan kalan değerlerle)
  els.username.value = localStorage.getItem('username') || '';
  els.room.value = localStorage.getItem('room') || 'genel';

  // Bağlantıyı başlatmak istemezsen bu satırı kaldırabilirsin:
  // connect();
})();
</script>
</body>
</html>
<!doctype html>
<html lang="tr">
<head>
  <meta charset="utf-8">
  <title>İletişim 101 Slayt</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js/dist/reveal.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js/dist/theme/white.css">
</head>
<body>
<div class="reveal">
  <div class="slides">
    <section><h1>İletişim 101</h1><p>Temel Kavramlar ve Uygulamalar</p></section>
    <section><h2>1. Giriş</h2><p>İletişim, kaynak ve alıcı arasında bilgi aktarımıdır.</p></section>
    <section><h2>2. İletişim Türleri</h2>
      <ul><li>Sözlü</li><li>Sözsüz</li><li>Yazılı</li><li>Dijital</li></ul>
    </section>
    <section><h2>3. İletişim Süreci</h2><p>Kaynak → Mesaj → Kanal → Alıcı → Geri Bildirim</p></section>
    <section><h2>4. Etkili İletişim</h2><ul><li>Empati</li><li>Dinleme</li><li>Açıklık</li><li>Beden dili</li></ul></section>
    <section><h2>5. Engeller</h2><ul><li>Ön yargı</li><li>Dikkat dağınıklığı</li><li>Kültürel fark</li></ul></section>
    <section><h2>6. Uygulama</h2><p>Mesaj netliği testi + beden dili checklist</p></section>
    <section><h2>7. Sonuç</h2><p>Etkili iletişim güveni artırır ve yanlış anlamaları azaltır.</p></section>
  </div>
</div>
<script src="https://cdn.jsdelivr.net/npm/reveal.js/dist/reveal.js"></script>
<script>Reveal.initialize();</script>
</body>
</html>
<a href="https://chat.vercel.ai/">
  <img alt="Next.js 14 and App Router-ready AI chatbot." src="app/(chat)/opengraph-image.png">
  <h1 align="center">Chat SDK</h1>
</a>

<p align="center">
    Chat SDK is a free, open-source template built with Next.js and the AI SDK that helps you quickly build powerful chatbot applications.
</p>

<p align="center">
  <a href="https://chat-sdk.dev"><strong>Read Docs</strong></a> ·
  <a href="#features"><strong>Features</strong></a> ·
  <a href="#model-providers"><strong>Model Providers</strong></a> ·
  <a href="#deploy-your-own"><strong>Deploy Your Own</strong></a> ·
  <a href="#running-locally"><strong>Running locally</strong></a>
</p>
<br/>

## Features

- [Next.js](https://nextjs.org) App Router
  - Advanced routing for seamless navigation and performance
  - React Server Components (RSCs) and Server Actions for server-side rendering and increased performance
- [AI SDK](https://sdk.vercel.ai/docs)
  - Unified API for generating text, structured objects, and tool calls with LLMs
  - Hooks for building dynamic chat and generative user interfaces
  - Supports xAI (default), OpenAI, Fireworks, and other model providers
- [shadcn/ui](https://ui.shadcn.com)
  - Styling with [Tailwind CSS](https://tailwindcss.com)
  - Component primitives from [Radix UI](https://radix-ui.com) for accessibility and flexibility
- Data Persistence
  - [Neon Serverless Postgres](https://vercel.com/marketplace/neon) for saving chat history and user data
  - [Vercel Blob](https://vercel.com/storage/blob) for efficient file storage
- [Auth.js](https://authjs.dev)
  - Simple and secure authentication

## Model Providers

This template ships with [xAI](https://x.ai) `grok-2-1212` as the default chat model. However, with the [AI SDK](https://sdk.vercel.ai/docs), you can switch LLM providers to [OpenAI](https://openai.com), [Anthropic](https://anthropic.com), [Cohere](https://cohere.com/), and [many more](https://sdk.vercel.ai/providers/ai-sdk-providers) with just a few lines of code.

## Deploy Your Own

You can deploy your own version of the Next.js AI Chatbot to Vercel with one click:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fvercel%2Fai-chatbot&env=AUTH_SECRET&envDescription=Learn+more+about+how+to+get+the+API+Keys+for+the+application&envLink=https%3A%2F%2Fgithub.com%2Fvercel%2Fai-chatbot%2Fblob%2Fmain%2F.env.example&demo-title=AI+Chatbot&demo-description=An+Open-Source+AI+Chatbot+Template+Built+With+Next.js+and+the+AI+SDK+by+Vercel.&demo-url=https%3A%2F%2Fchat.vercel.ai&products=%5B%7B%22type%22%3A%22integration%22%2C%22protocol%22%3A%22ai%22%2C%22productSlug%22%3A%22grok%22%2C%22integrationSlug%22%3A%22xai%22%7D%2C%7B%22type%22%3A%22integration%22%2C%22protocol%22%3A%22storage%22%2C%22productSlug%22%3A%22neon%22%2C%22integrationSlug%22%3A%22neon%22%7D%2C%7B%22type%22%3A%22integration%22%2C%22protocol%22%3A%22storage%22%2C%22productSlug%22%3A%22upstash-kv%22%2C%22integrationSlug%22%3A%22upstash%22%7D%2C%7B%22type%22%3A%22blob%22%7D%5D)

## Running locally

You will need to use the environment variables [defined in `.env.example`](.env.example) to run Next.js AI Chatbot. It's recommended you use [Vercel Environment Variables](https://vercel.com/docs/projects/environment-variables) for this, but a `.env` file is all that is necessary.

> Note: You should not commit your `.env` file or it will expose secrets that will allow others to control access to your various AI and authentication provider accounts.

1. Install Vercel CLI: `npm i -g vercel`
2. Link local instance with Vercel and GitHub accounts (creates `.vercel` directory): `vercel link`
3. Download your environment variables: `vercel env pull`

```bash
pnpm install
pnpm dev
```

Your app template should now be running on [localhost:3000](http://localhost:3000).
