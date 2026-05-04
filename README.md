[daily-theme-song.html](https://github.com/user-attachments/files/27344663/daily-theme-song.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>每日主题曲</title>
<style>
  :root {
    --bg: #0d0d1a;
    --card-bg: #1a1a35;
    --text: #e0e0f0;
    --accent: #ff6b9d;
  }
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    font-family: "PingFang SC", "Microsoft YaHei", "Noto Sans SC", sans-serif;
    background: var(--bg); color: var(--text);
    min-height: 100vh; overflow-x: hidden;
  }
  #bgCanvas {
    position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    pointer-events: none; z-index: 0;
  }
  .container {
    position: relative; z-index: 1;
    max-width: 700px; margin: 0 auto; padding: 20px 16px 50px;
    display: flex; flex-direction: column; align-items: center;
  }

  /* Header */
  .header { text-align: center; margin: 30px 0 10px; }
  .header .icon { font-size: 56px; display: block; }
  .header h1 {
    font-size: clamp(26px, 5vw, 42px); margin-top: 8px;
    background: linear-gradient(135deg, #ff6b9d, #c44dff, #4da6ff, #44eebb);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
    background-clip: text; animation: hueShift 4s linear infinite;
    background-size: 200% 200%;
  }
  @keyframes hueShift {
    0% { filter: hue-rotate(0deg); }
    100% { filter: hue-rotate(360deg); }
  }
  .header .date-badge {
    display: inline-block; margin-top: 6px; padding: 4px 16px;
    border-radius: 20px; background: rgba(255,255,255,0.06);
    font-size: 13px; color: #8888aa; letter-spacing: 2px;
  }

  /* Input area */
  .input-area {
    display: flex; gap: 10px; margin: 20px 0; width: 100%; max-width: 400px;
  }
  .input-area input {
    flex: 1; padding: 12px 20px; border-radius: 50px;
    border: 1px solid rgba(255,255,255,0.2);
    background: rgba(255,255,255,0.05); color: #fff;
    font-size: 16px; outline: none; font-family: inherit;
    transition: border-color 0.3s;
  }
  .input-area input:focus { border-color: var(--accent); }
  .input-area input::placeholder { color: #555; }
  .input-area button {
    padding: 12px 24px; border-radius: 50px; border: none;
    background: linear-gradient(135deg, #ff6b9d, #c44dff);
    color: #fff; font-size: 16px; cursor: pointer; font-family: inherit;
    font-weight: bold; letter-spacing: 1px; transition: all 0.3s;
    white-space: nowrap;
  }
  .input-area button:hover { transform: translateY(-2px); box-shadow: 0 8px 25px rgba(196, 77, 255, 0.4); }

  /* Music Card */
  .card-wrapper { width: 100%; max-width: 400px; perspective: 1000px; margin: 10px 0; }
  .music-card {
    width: 100%; border-radius: 20px; padding: 28px 24px; position: relative;
    overflow: hidden; animation: cardAppear 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
    box-shadow: 0 20px 60px rgba(0,0,0,0.5);
    transition: transform 0.3s;
  }
  .music-card:hover { transform: translateY(-4px); }
  @keyframes cardAppear {
    from { opacity: 0; transform: scale(0.8) translateY(30px); }
    to { opacity: 1; transform: scale(1) translateY(0); }
  }
  .card-shine {
    position: absolute; top: -50%; left: -50%; width: 200%; height: 200%;
    background: radial-gradient(circle at 30% 30%, rgba(255,255,255,0.08) 0%, transparent 50%);
    pointer-events: none;
  }
  .card-header {
    display: flex; justify-content: space-between; align-items: flex-start;
    position: relative; z-index: 1;
  }
  .card-genre {
    font-size: 32px; font-weight: bold; letter-spacing: 1px;
    text-shadow: 0 2px 10px rgba(0,0,0,0.3);
  }
  .card-bpm {
    font-size: 12px; padding: 4px 10px; border-radius: 12px;
    background: rgba(255,255,255,0.15); letter-spacing: 1px;
  }
  .card-mood {
    font-size: 15px; margin-top: 4px; opacity: 0.85; letter-spacing: 1px;
    position: relative; z-index: 1;
  }
  .card-era {
    display: inline-block; margin-top: 8px; padding: 3px 10px;
    border-radius: 12px; font-size: 12px; letter-spacing: 1px;
    background: rgba(255,255,255,0.1); position: relative; z-index: 1;
  }
  .card-dedication {
    margin-top: 16px; padding-top: 14px;
    border-top: 1px solid rgba(255,255,255,0.15);
    font-size: 13px; letter-spacing: 1px; opacity: 0.8;
    position: relative; z-index: 1; line-height: 1.6;
  }
  .card-name {
    margin-top: 12px; text-align: right; font-size: 13px;
    opacity: 0.6; letter-spacing: 1px; position: relative; z-index: 1;
  }
  .card-eq {
    margin-top: 14px; display: flex; gap: 3px; align-items: flex-end;
    height: 40px; position: relative; z-index: 1;
  }
  .card-eq .bar {
    flex: 1; border-radius: 2px 2px 0 0;
    animation: eqBounce 0.6s ease-in-out infinite alternate;
    background: rgba(255,255,255,0.4);
  }

  /* Share section */
  .share-area { margin-top: 16px; display: flex; gap: 10px; flex-wrap: wrap; justify-content: center; }
  .btn-share {
    padding: 10px 20px; border-radius: 50px; border: 1px solid rgba(255,255,255,0.25);
    background: rgba(255,255,255,0.05); color: #ccc; cursor: pointer;
    font-size: 14px; letter-spacing: 1px; font-family: inherit;
    transition: all 0.3s;
  }
  .btn-share:hover { background: rgba(255,255,255,0.12); border-color: rgba(255,255,255,0.4); }
  .btn-share.copied { border-color: #44eebb; color: #44eebb; }

  /* Music Wall */
  .wall-section { width: 100%; max-width: 700px; margin-top: 40px; }
  .wall-title {
    font-size: 18px; letter-spacing: 2px; text-align: center;
    color: #8888aa; margin-bottom: 16px;
  }
  .wall-scroll {
    display: flex; gap: 12px; overflow-x: auto; padding: 8px 4px 16px;
    scroll-behavior: smooth; -webkit-overflow-scrolling: touch;
  }
  .wall-scroll::-webkit-scrollbar { height: 4px; }
  .wall-scroll::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.15); border-radius: 2px; }
  .wall-card {
    flex-shrink: 0; width: 140px; padding: 14px; border-radius: 14px;
    text-align: center; font-size: 12px; letter-spacing: 1px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
  }
  .wall-card .w-name { font-size: 14px; font-weight: bold; margin-bottom: 4px; }
  .wall-card .w-genre { font-size: 11px; opacity: 0.8; }
  .wall-card .w-mood { font-size: 10px; opacity: 0.6; margin-top: 2px; }
  .wall-empty { color: #555; text-align: center; width: 100%; padding: 20px; font-size: 14px; }

  .hidden { display: none !important; }

  @media (max-width: 500px) {
    .music-card { padding: 20px 16px; }
    .card-genre { font-size: 24px; }
    .wall-card { width: 110px; padding: 10px; }
  }
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>

<div class="container">
  <div class="header">
    <span class="icon">🎵</span>
    <h1>每日主题曲</h1>
    <span class="date-badge" id="dateBadge"></span>
  </div>

  <div class="input-area">
    <input type="text" id="nameInput" placeholder="输入你的名字…" maxlength="20" autocomplete="off">
    <button onclick="generate()">🎶 生成</button>
  </div>

  <div class="card-wrapper hidden" id="cardWrapper">
    <div class="music-card" id="musicCard">
      <div class="card-shine"></div>
      <div class="card-header">
        <span class="card-genre" id="cardGenre"></span>
        <span class="card-bpm" id="cardBpm"></span>
      </div>
      <div class="card-mood" id="cardMood"></div>
      <div class="card-era" id="cardEra"></div>
      <div class="card-eq" id="cardEq"></div>
      <div class="card-dedication" id="cardDedication"></div>
      <div class="card-name" id="cardName"></div>
    </div>
  </div>

  <div class="share-area hidden" id="shareArea">
    <button class="btn-share" onclick="copyLink()">🔗 复制分享链接</button>
    <button class="btn-share" onclick="generate()">🎲 换一首</button>
  </div>

  <div class="wall-section" id="wallSection">
    <div class="wall-title">🎧 今日音乐墙</div>
    <div class="wall-scroll" id="wallScroll"></div>
  </div>
</div>

<script>
// ==================== DATA ====================
const genres = ['Lo-fi Hip Hop', '摇滚', '电子', '古典交响', '爵士', 'R&B', '民谣', '嘻哈',
  'City Pop', '新浪潮', 'K-Pop', '独立音乐', '放克', '蓝调', '合成器流行', '梦幻流行'];
const moods = ['元气满满', '温柔治愈', '热血沸腾', '慵懒松弛', '浪漫梦幻', '自信霸气', '怀旧感伤', '神秘深邃', '自由飞扬', '甜蜜心动'];
const eras = ['60s 复古', '70s 迪斯科', '80s 金曲', '90s 经典', '00s 流行', '10s 独立', '20s 未来'];
const colorPalettes = [
  ['#ff6b9d', '#c44dff'], ['#e74c3c', '#f39c12'], ['#7b2fff', '#00d4ff'],
  ['#d4a574', '#8b5e3c'], ['#3c8dbc', '#2c3e50'], ['#e91e63', '#ff9800'],
  ['#4caf50', '#8bc34a'], ['#ff5722', '#ffc107'], ['#9c27b0', '#e040fb'],
  ['#00bcd4', '#009688'], ['#ff4081', '#536dfe'], ['#607d8b', '#455a64'],
  ['#ff9800', '#f44336'], ['#1976d2', '#64b5f6'], ['#e040fb', '#7c4dff'],
  ['#80deea', '#26c6da']
];
const lyricTemplates = [
  '今天的世界，<b>{name}</b> 是主角。<br>当 {genre} 的节奏响起，<br>整个宇宙都跟着 {mood} 地摇摆。',
  '在 {era} 的旋律里，<br><b>{name}</b> 找到了属于自己的频率。<br>{bpm} BPM —— 刚好是心跳加速的速度。',
  '这是一首写给 <b>{name}</b> 的歌。<br>{mood} 的前奏、{genre} 的副歌，<br>循环播放一整天。',
  '耳机分你一半 🎧<br><b>{name}</b> 的今日主题曲正在播放：<br>一首 {mood} 的 {genre}。',
  '没有什么比 {era} 的 {genre}<br>更能诠释 <b>{name}</b> 今天的心情。<br>音量调大，世界静音。',
  '今天的 <b>{name}</b>，<br>是一首 {bpm} BPM 的 {genre}。<br>节奏不快不慢，刚好和幸福同步。',
  '当 {genre} 遇到 {mood}，<br>就是 <b>{name}</b> 今天的主题曲配方。<br>请搭配 {era} 的滤镜食用。',
  '系统已为 <b>{name}</b> 自动匹配：<br>{era} · {genre} · {mood}<br>🎶 正在加载你的专属BGM…'
];

// ==================== HASH ====================
function hashStr(str) {
  let h = 5381;
  for (let i = 0; i < str.length; i++) {
    h = ((h << 5) + h + str.charCodeAt(i)) | 0;
  }
  return Math.abs(h);
}

function getToday() {
  const d = new Date();
  return d.getFullYear() + '-' + String(d.getMonth() + 1).padStart(2, '0') + '-' + String(d.getDate()).padStart(2, '0');
}

function nameToMusic(name) {
  const key = name.trim() + '_' + getToday();
  const h = hashStr(key);
  return {
    genre: genres[h % genres.length],
    mood: moods[(h >> 3) % moods.length],
    era: eras[(h >> 6) % eras.length],
    bpm: 60 + (h % 120),
    palette: colorPalettes[(h >> 9) % colorPalettes.length],
    lyricIdx: (h >> 12) % lyricTemplates.length
  };
}

// ==================== BACKGROUND CANVAS ====================
const canvas = document.getElementById('bgCanvas');
const ctx = canvas.getContext('2d');
let bgParticles = [], eqBars = [];
let eqActive = false, eqColors = ['#ff6b9d','#c44dff'];

function resizeCanvas() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}
resizeCanvas();
window.addEventListener('resize', resizeCanvas);

for (let i = 0; i < 60; i++) {
  bgParticles.push({
    x: Math.random() * canvas.width, y: Math.random() * canvas.height,
    r: Math.random() * 1.2 + 0.3, vy: -Math.random() * 0.3 - 0.1,
    alpha: Math.random() * 0.5 + 0.15
  });
}
for (let i = 0; i < 30; i++) {
  eqBars.push({ h: Math.random() * 80 + 10, target: Math.random() * 80 + 10, speed: 0.02 + Math.random() * 0.05 });
}

function animateBg() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Floating particles
  for (const p of bgParticles) {
    p.y += p.vy;
    if (p.y < -10) { p.y = canvas.height + 10; p.x = Math.random() * canvas.width; }
    ctx.fillStyle = `rgba(255,255,255,${p.alpha})`;
    ctx.beginPath(); ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2); ctx.fill();
  }

  // EQ bars (bottom)
  if (eqActive) {
    const barW = canvas.width / eqBars.length;
    for (let i = 0; i < eqBars.length; i++) {
      const b = eqBars[i];
      b.h += (b.target - b.h) * b.speed;
      if (Math.random() < 0.05) b.target = Math.random() * 120 + 20;
      const grad = ctx.createLinearGradient(0, canvas.height, 0, canvas.height - b.h);
      grad.addColorStop(0, eqColors[0]); grad.addColorStop(1, eqColors[1]);
      ctx.fillStyle = grad;
      ctx.fillRect(i * barW + 1, canvas.height - b.h, barW - 2, b.h);
    }
  }

  requestAnimationFrame(animateBg);
}
animateBg();

// ==================== GENERATE ====================
function generate() {
  const name = document.getElementById('nameInput').value.trim();
  if (!name) { alert('请输入你的名字哦～'); return; }

  const music = nameToMusic(name);
  const template = lyricTemplates[music.lyricIdx];
  const dedication = template
    .replace('{name}', name)
    .replace('{genre}', music.genre)
    .replace('{mood}', music.mood)
    .replace('{era}', music.era)
    .replace('{bpm}', music.bpm);

  // Update card
  document.getElementById('cardGenre').textContent = music.genre;
  document.getElementById('cardBpm').textContent = music.bpm + ' BPM';
  document.getElementById('cardMood').textContent = '💭 ' + music.mood;
  document.getElementById('cardEra').textContent = '📻 ' + music.era;
  document.getElementById('cardDedication').innerHTML = dedication;
  document.getElementById('cardName').textContent = '—— ' + name + ' 的今日主题曲';

  // Card colors
  const card = document.getElementById('musicCard');
  card.style.background = `linear-gradient(135deg, ${music.palette[0]}22, ${music.palette[1]}44, ${music.palette[0]}22)`;
  card.style.border = `1px solid ${music.palette[1]}44`;
  document.getElementById('cardBpm').style.background = music.palette[1] + '33';
  document.getElementById('cardEra').style.background = music.palette[0] + '33';

  // EQ bars in card
  const eqDiv = document.getElementById('cardEq');
  eqDiv.innerHTML = Array.from({length: 20}, (_, i) => {
    const h = 8 + Math.abs(Math.sin(i * 0.7 + Date.now() * 0.001)) * 28;
    return `<div class="bar" style="height:${h}px;animation-delay:${i * 0.04}s;
      background:${i % 2 === 0 ? music.palette[0] : music.palette[1]};opacity:${0.4 + i/20 * 0.5};"></div>`;
  }).join('');

  // Activate bg EQ
  eqActive = true;
  eqColors = music.palette;

  // Show card + share
  document.getElementById('cardWrapper').classList.remove('hidden');
  document.getElementById('shareArea').classList.remove('hidden');
  // Re-trigger animation
  const wrapper = document.getElementById('cardWrapper');
  wrapper.style.animation = 'none'; wrapper.offsetHeight;
  wrapper.style.animation = '';

  // Update URL
  const url = new URL(window.location);
  url.searchParams.set('name', name);
  window.history.replaceState({}, '', url);

  // Save to music wall
  saveToWall(name, music);

  // Update date badge
  updateDateBadge();
}

// ==================== MUSIC WALL ====================
function saveToWall(name, music) {
  const today = getToday();
  let wall = JSON.parse(localStorage.getItem('musicWall') || '{}');
  if (!wall[today]) wall[today] = [];
  // Remove duplicate name for today
  wall[today] = wall[today].filter(e => e.name !== name);
  wall[today].unshift({ name, genre: music.genre, mood: music.mood, palette: music.palette });
  wall[today] = wall[today].slice(0, 30);
  // Clean old days
  const keys = Object.keys(wall).sort();
  while (keys.length > 7) { delete wall[keys.shift()]; }
  localStorage.setItem('musicWall', JSON.stringify(wall));
  renderWall();
}

function renderWall() {
  const today = getToday();
  const wall = JSON.parse(localStorage.getItem('musicWall') || '{}');
  const entries = wall[today] || [];
  const scroll = document.getElementById('wallScroll');

  if (entries.length === 0) {
    scroll.innerHTML = '<div class="wall-empty">还没有人来过，做今天第一个留下旋律的人吧 🎵</div>';
    return;
  }

  scroll.innerHTML = entries.map(e => `
    <div class="wall-card" style="background:linear-gradient(135deg,${e.palette[0]}33,${e.palette[1]}44);border:1px solid ${e.palette[1]}33;">
      <div class="w-name">🎵 ${escapeHTML(e.name)}</div>
      <div class="w-genre">${e.genre}</div>
      <div class="w-mood">${e.mood}</div>
    </div>
  `).join('');
}

function escapeHTML(str) {
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

// ==================== SHARE ====================
function copyLink() {
  const url = window.location.href;
  navigator.clipboard.writeText(url).then(() => {
    const btn = document.querySelector('.btn-share');
    btn.textContent = '✅ 链接已复制！';
    btn.classList.add('copied');
    setTimeout(() => { btn.textContent = '🔗 复制分享链接'; btn.classList.remove('copied'); }, 2000);
  }).catch(() => {
    prompt('复制这个链接分享给朋友：', url);
  });
}

// ==================== INIT ====================
function updateDateBadge() {
  const d = new Date();
  const weekdays = ['日', '一', '二', '三', '四', '五', '六'];
  document.getElementById('dateBadge').textContent =
    `📅 ${d.getFullYear()}年${d.getMonth() + 1}月${d.getDate()}日 星期${weekdays[d.getDay()]}`;
}
updateDateBadge();
renderWall();

// Check URL params
const params = new URLSearchParams(window.location.search);
const urlName = params.get('name');
if (urlName) {
  document.getElementById('nameInput').value = urlName;
  generate();
}

// Enter key
document.getElementById('nameInput').addEventListener('keydown', e => {
  if (e.key === 'Enter') generate();
});
</script>
</body>
</html>
