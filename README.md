<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ManchSphere | Personal Hub</title>
    <style>
        :root { --primary: #00f2ff; --accent: #ff0055; --bg: #050505; --panel: #111; }
        body, html { margin: 0; padding: 0; height: 100%; background: var(--bg); color: white; font-family: sans-serif; overflow: hidden; }
        #auth-screen { display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100vh; }
        .login-card { background: var(--panel); padding: 40px; border-radius: 20px; border: 1px solid #222; text-align: center; }
        input { width: 250px; padding: 15px; margin: 10px 0; border-radius: 10px; border: 1px solid #333; background: #000; color: var(--primary); }
        .btn { padding: 15px 40px; border-radius: 10px; border: none; background: var(--primary); color: black; font-weight: bold; cursor: pointer; }
        #app { display: none; height: 100vh; grid-template-rows: auto 1fr auto; }
        .header { padding: 15px; border-bottom: 1px solid #222; display: flex; justify-content: space-between; }
        .content { overflow-y: auto; height: 100%; }
        .search-area { padding: 20px; text-align: center; }
        .search-row { display: flex; gap: 5px; max-width: 600px; margin: 10px auto; }
        .video-feed { height: 100%; overflow-y: scroll; scroll-snap-type: y mandatory; }
        .video-box { height: 100%; scroll-snap-align: start; position: relative; background: #000; }
        video { width: 100%; height: 100%; object-fit: cover; }
        .bottom-nav { background: #000; border-top: 1px solid #222; height: 60px; display: flex; justify-content: space-around; align-items: center; }
        .nav-item { font-size: 24px; cursor: pointer; opacity: 0.5; }
        .nav-item.active { opacity: 1; color: var(--primary); }
    </style>
</head>
<body>
    <div id="auth-screen">
        <div class="login-card">
            <h1>ManchSphere</h1>
            <input type="text" id="urlInput" placeholder="Введите manch.com">
            <br><button class="btn" onclick="login()">ВОЙТИ</button>
        </div>
    </div>
    <div id="app">
        <div class="header"><span style="color:var(--primary)">MANCH.COM</span><span>Манч</span></div>
        <div class="content" id="main-content">
            <div id="view-home" class="view">
                <div class="search-area">
                    <h2>Умный Поиск</h2>
                    <div class="search-row">
                        <input type="text" id="globalSearch" style="flex:1" placeholder="Найти всё что угодно...">
                        <button class="btn" onclick="doSearch()">🔍</button>
                    </div>
                    <h2>Загрузчик</h2>
                    <div class="search-row">
                        <input type="text" id="dlLink" style="flex:1" placeholder="Вставь ссылку из Telegram...">
                        <button class="btn" onclick="doDownload()">⬇️</button>
                    </div>
                </div>
            </div>
            <div id="view-video" class="view" style="display:none; height:100%;">
                <div class="video-feed" id="vFeed"></div>
            </div>
        </div>
        <div class="bottom-nav">
            <div class="nav-item active" onclick="switchTab('home', this)">🏠</div>
            <div class="nav-item" onclick="switchTab('video', this)">📱</div>
        </div>
    </div>
    <script>
        const vids = ["https://www.w3schools.com/html/mov_bbb.mp4", "https://www.w3schools.com/html/movie.mp4"];
        function login() {
            if(document.getElementById('urlInput').value.toLowerCase()==='manch.com') {
                localStorage.setItem('m_auth', 'true'); startApp();
            } else { alert("Неверно!"); }
        }
        function startApp() {
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app').style.display='grid';
            initVideos();
        }
        function switchTab(tab, el) {
            document.querySelectorAll('.view').forEach(v => v.style.display='none');
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            document.getElementById('view-'+tab).style.display='block';
            el.classList.add('active');
        }
        function doSearch() {
            window.open("https://www.google.com/search?q=" + document.getElementById('globalSearch').value, "_blank");
        }
        function doDownload() {
            window.open(document.getElementById('dlLink').value, "_blank");
        }
        function initVideos() {
            const f = document.getElementById('vFeed'); f.innerHTML='';
            vids.forEach(s => {
                const b = document.createElement('div'); b.className='video-box';
                b.innerHTML=`<video src="${s}" loop muted onclick="this.paused?this.play():this.pause()"></video>`;
                f.appendChild(b);
            });
            const obs = new IntersectionObserver((es)=>{es.forEach(e=>{const v=e.target.querySelector('video'); if(e.isIntersecting)v.play();else v.pause();})},{threshold:0.8});
            document.querySelectorAll('.video-box').forEach(b=>obs.observe(b));
        }
        window.onload = () => { if(localStorage.getItem('m_auth')) startApp(); };
    </script>
</body>
</html>
