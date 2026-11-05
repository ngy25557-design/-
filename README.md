<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>🎂 糕手蛋生日 - 夢幻蛋糕工坊</title>
    <style>
        /* 🎨 核心色卡與全域變數 */
        :root {
            --c-berry: #66281f;     /* 深褐/漿果 */
            --c-rose: #994d47;      /* 玫瑰紅 */
            --c-almond: #9d8361;    /* 杏仁奶油 */
            --c-beige: #bfb9a3;     /* 甜米色 */
            --c-cream: #f7f6f0;     /* 奶油白 */
            --c-bg: #e6e5d5;        /* 背景 */
        }

        * { box-sizing: border-box; user-select: none; -webkit-tap-highlight-color: transparent; }
        body {
            margin: 0; padding: 0; height: 100vh; overflow: hidden;
            background: var(--c-bg); font-family: 'Segoe UI', 'Quicksand', 'PingFang TC', system-ui, sans-serif;
            color: var(--c-berry);
        }
        .page { display: none; width: 100%; height: 100%; flex-direction: column; position: absolute; top: 0; left: 0; }
        .page.active { display: flex; }

        /* 立體糖果按鈕 */
        .btn {
            background: var(--c-rose); color: #fff; border: none; border-radius: 40px;
            padding: 16px 45px; font-size: 1.2rem; font-weight: bold; letter-spacing: 1px;
            box-shadow: 0 6px 0 var(--c-berry); cursor: pointer; transition: all 0.1s;
            display: inline-flex; align-items: center; justify-content: center;
        }
        .btn:active:not(:disabled) { transform: translateY(6px); box-shadow: 0 0 0 var(--c-berry); }
        .btn:disabled { background: var(--c-beige); box-shadow: 0 6px 0 #a09b88; cursor: not-allowed; opacity: 0.8; }
        .btn-outline { background: var(--c-cream); color: var(--c-rose); border: 2px solid var(--c-rose); box-shadow: 0 6px 0 var(--c-beige); }
        .btn-outline:active:not(:disabled) { box-shadow: 0 0 0 var(--c-beige); }

        /* ================= 頁面 1：首頁 ================= */
        #page-home { justify-content: center; align-items: center; background: linear-gradient(145deg, var(--c-bg), #dcd9c6); overflow: hidden; }
        .balloon { position: absolute; bottom: -100px; width: 60px; height: 80px; border-radius: 50%; background: var(--c-rose); animation: floatUp 8s linear infinite; opacity: 0.6; }
        .balloon::after { content: ''; position: absolute; bottom: -20px; left: 28px; width: 4px; height: 20px; background: rgba(102, 40, 31, 0.4); }
        .b2 { background: var(--c-beige); left: 20%; animation-duration: 11s; animation-delay: 2s; }
        .b3 { background: var(--c-almond); left: 80%; animation-duration: 9s; animation-delay: 1s; }
        @keyframes floatUp { 0% { bottom: -100px; transform: translateX(0); } 50% { transform: translateX(20px); } 100% { bottom: 110vh; transform: translateX(-20px); } }
        
        .home-title { font-size: 3.5rem; font-family: 'Comic Sans MS', cursive, sans-serif; color: var(--c-berry); margin-bottom: 20px; text-shadow: 3px 3px 0 #fff; text-align: center; z-index: 10; }
        .bgm-toggle { position: absolute; top: 20px; right: 20px; width: 50px; height: 50px; border-radius: 50%; background: var(--c-cream); border: 2px solid var(--c-rose); font-size: 1.5rem; display: flex; justify-content: center; align-items: center; cursor: pointer; z-index: 20; box-shadow: 0 4px 0 var(--c-beige); }

        /* ================= 頁面 2：選胚 ================= */
        #page-base { background: var(--c-bg); padding-bottom: 80px; }
        .page-header { text-align: center; padding: 30px 0; font-size: 1.8rem; font-weight: bold; }
        .base-list { display: flex; gap: 20px; justify-content: center; align-items: center; flex: 1; flex-wrap: wrap; padding: 20px; }
        .base-item {
            width: 140px; height: 140px; background: var(--c-cream); border-radius: 20px;
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            box-shadow: 0 8px 15px rgba(157, 131, 97, 0.2); border: 4px solid transparent; cursor: pointer;
        }
        .base-item.active { border-color: var(--c-rose); transform: scale(1.05); }
        .base-item.bounce { animation: bounce 0.3s ease; }
        @keyframes bounce { 0%, 100% { transform: scale(1.05); } 50% { transform: scale(0.95); } }
        .base-item canvas { width: 80px; height: 80px; margin-bottom: 10px; }
        .fixed-bottom { position: absolute; bottom: 0; left: 0; width: 100%; padding: 15px; background: rgba(247, 246, 240, 0.9); text-align: center; border-top: 2px solid var(--c-beige); z-index: 10; }

        /* ================= 頁面 3：裝飾 ================= */
        #page-decorate { flex-direction: row; background: var(--c-bg); }
        .canvas-area { flex: 7; display: flex; justify-content: center; align-items: center; background: var(--c-cream); position: relative; }
        #cakeCanvas { width: 95%; max-width: 800px; aspect-ratio: 1/1; touch-action: none; }
        
        .action-bar { position: absolute; top: 15px; left: 15px; display: flex; gap: 10px; }
        .icon-btn { width: 44px; height: 44px; border-radius: 50%; background: var(--c-cream); border: 2px solid var(--c-rose); color: var(--c-rose); font-size: 1.2rem; display: flex; justify-content: center; align-items: center; box-shadow: 0 3px 0 var(--c-beige); cursor: pointer; }
        .icon-btn:active { transform: translateY(3px); box-shadow: none; }

        .sidebar { flex: 3; max-width: 320px; min-width: 140px; background: var(--c-cream); border-left: 2px solid var(--c-beige); display: flex; flex-direction: column; }
        .sidebar-header { padding: 15px; text-align: center; font-weight: bold; background: #f0efe8; border-bottom: 2px solid var(--c-beige); }
        .accordion-container { flex: 1; overflow-y: auto; }
        .accordion-container::-webkit-scrollbar { width: 6px; }
        .accordion-container::-webkit-scrollbar-thumb { background: var(--c-beige); border-radius: 10px; }
        
        .acc-item { border-bottom: 1px solid var(--c-beige); }
        .acc-header { padding: 15px; font-weight: bold; background: var(--c-cream); display: flex; justify-content: space-between; cursor: pointer; font-size: 0.95rem; }
        .acc-header::after { content: '▼'; font-size: 0.8rem; color: var(--c-almond); transition: transform 0.3s; }
        .acc-item.active .acc-header::after { transform: rotate(180deg); }
        .acc-content { max-height: 0; overflow: hidden; transition: max-height 0.3s ease; background: #faf9f5; padding: 0 15px; }
        .acc-item.active .acc-content { max-height: 500px; padding: 15px; }
        
        .options-grid { display: flex; flex-wrap: wrap; gap: 10px; }
        .opt-btn { width: 44px; height: 44px; border-radius: 50%; background: #fff; box-shadow: 0 3px 5px rgba(0,0,0,0.1); display: flex; justify-content: center; align-items: center; font-size: 1.4rem; cursor: pointer; border: 2px solid transparent; }
        .opt-btn.active { border-color: var(--c-rose); transform: scale(1.1); }
        .sub-grid { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 10px; }
        .sub-btn { padding: 6px 10px; background: #fff; border: 1px solid var(--c-beige); border-radius: 10px; font-size: 0.75rem; font-weight: bold; cursor: pointer; color: var(--c-berry); }
        .sub-btn.active { background: var(--c-rose); color: #fff; border-color: var(--c-rose); }
        .sidebar-footer { padding: 15px; text-align: center; border-top: 2px solid var(--c-beige); }

        /* ================= 頁面 4：選場景 ================= */
        #page-scene { background: var(--c-bg); justify-content: center; align-items: center; padding-bottom: 80px;}
        .scene-list { display: flex; gap: 20px; flex-wrap: wrap; justify-content: center; margin-bottom: 30px; }
        .scene-card { width: 160px; height: 160px; border-radius: 20px; display: flex; flex-direction: column; justify-content: center; align-items: center; box-shadow: 0 8px 15px rgba(0,0,0,0.1); cursor: pointer; border: 4px solid transparent; font-weight: bold; font-size: 1.1rem; color: var(--c-berry); transition: 0.2s;}
        .scene-card.active { border-color: var(--c-rose); transform: scale(1.05); }
        .sc-restaurant { background: linear-gradient(145deg, #4a2c2c, #2c1e1e); color: #fff; }
        .sc-home { background: linear-gradient(145deg, #fff0e6, #e6e5d5); }
        .sc-beach { background: linear-gradient(145deg, #87CEEB, #F4D03F); }
        .sc-emoji { font-size: 4rem; margin-bottom: 10px; }

        /* ================= 頁面 5：最終展示 ================= */
        #page-final { justify-content: center; align-items: center; position: relative; overflow: hidden; }
        .final-bg { position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1; transition: background 0.5s; }
        .table-el { position: absolute; bottom: 0; left: 50%; transform: translateX(-50%); z-index: 2; transition: all 0.5s; }
        .table.restaurant { background: linear-gradient(to bottom, #7a2f2a, #4a1813); border-radius: 50% 50% 0 0 / 20% 20% 0 0; width: 90%; height: 35%; box-shadow: inset 0 10px 20px rgba(255,255,255,0.1); }
        .table.home { background: rgba(255,255,255,0.6); border: 4px solid rgba(255,255,255,0.9); backdrop-filter: blur(8px); width: 85%; height: 30%; border-radius: 50% 50% 0 0 / 10% 10% 0 0; }
        .rug-el { position: absolute; bottom: 0; left: 0; width: 100%; height: 55%; background: var(--c-beige); z-index: 1; border-radius: 50% 50% 0 0 / 20% 20% 0 0; display: none; }
        .table.beach { background: repeating-linear-gradient(90deg, var(--c-almond), var(--c-almond) 20px, #8a7151 20px, #8a7151 22px); width: 85%; height: 25%; border-radius: 10px; }
        
        /* 最終 Canvas 隱藏，用於合成 */
        #finalCanvas { display: none; }
        #cakeImg { position: absolute; z-index: 5; width: 60%; max-width: 400px; bottom: 35%; left: 50%; transform: translate(-50%, -1000px); filter: drop-shadow(0 25px 15px rgba(102, 40, 31, 0.3)); transition: transform 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
        #cakeImg.show { transform: translate(-50%, 0); }
        
        .final-ui { position: absolute; top: 20px; width: 100%; display: flex; justify-content: space-between; padding: 0 20px; z-index: 10; }
        .final-title { background: rgba(255,255,255,0.8); padding: 10px 30px; border-radius: 30px; border: 2px solid var(--c-beige); font-size: 1.5rem; font-weight: bold; position: absolute; top: 20px; left: 50%; transform: translateX(-50%); z-index: 10; white-space: nowrap; }
        .final-actions { position: absolute; bottom: 30px; display: flex; gap: 15px; z-index: 10; width: 100%; justify-content: center; }

        @media (max-width: 768px) {
            .home-title { font-size: 2.2rem; }
            .base-item { width: 110px; height: 110px; font-size: 0.9rem; }
            .sidebar { min-width: 130px; }
            .acc-header { padding: 10px; font-size: 0.85rem; }
            .opt-btn { width: 36px; height: 36px; font-size: 1.2rem; }
            .scene-card { width: 110px; height: 110px; font-size: 0.9rem; }
            .sc-emoji { font-size: 2.5rem; }
            .final-title { font-size: 1.2rem; padding: 8px 15px; }
            .btn { padding: 12px 25px; font-size: 1rem; }
        }
    </style>
</head>
<body>

    <!-- 頁面 0：首頁 -->
    <div id="page-home" class="page active">
        <div class="balloon"></div>
        <div class="balloon b2"></div>
        <div class="balloon b3"></div>
        <div class="bgm-toggle" id="bgmBtn" onclick="toggleBGM()">🎵</div>
        <h1 class="home-title">🎂 糕手蛋生日 🎈</h1>
        <p style="z-index:10; font-size:1.2rem; margin-bottom:30px; color:var(--c-almond);">一起來製作專屬的夢幻蛋糕吧！</p>
        <button class="btn" style="z-index:10;" onclick="goToPage('page-base')">開始製作蛋糕</button>
    </div>

    <!-- 頁面 1：選蛋糕胚 -->
    <div id="page-base" class="page">
        <div class="page-header">🍰 第一步：選擇蛋糕胚</div>
        <div class="base-list">
            <div class="base-item active" data-base="plain" onclick="selectBase(this)">
                <canvas width="80" height="80" class="mini-base" data-type="plain"></canvas>
                <span>素蛋糕胚</span>
            </div>
            <div class="base-item" data-base="cream" onclick="selectBase(this)">
                <canvas width="80" height="80" class="mini-base" data-type="cream"></canvas>
                <span>普通奶油</span>
            </div>
            <div class="base-item" data-base="choco" onclick="selectBase(this)">
                <canvas width="80" height="80" class="mini-base" data-type="choco"></canvas>
                <span>巧克力奶油</span>
            </div>
        </div>
        <div class="fixed-bottom">
            <button class="btn" onclick="goToPage('page-decorate')">下一頁 ➔</button>
        </div>
    </div>

    <!-- 頁面 2：裝飾工作台 -->
    <div id="page-decorate" class="page">
        <div class="canvas-area">
            <canvas id="cakeCanvas" width="800" height="800"></canvas>
            <div class="action-bar">
                <div class="icon-btn" onclick="undo()" title="撤銷">↩️</div>
                <div class="icon-btn" onclick="clearCake()" title="清空">🗑️</div>
            </div>
        </div>
        <div class="sidebar">
            <div class="sidebar-header">🧁 配料箱</div>
            <div class="accordion-container" id="accordion">
                <!-- 1. 奶油 -->
                <div class="acc-item active" data-type="cream">
                    <div class="acc-header" onclick="toggleAcc(this)">擠花奶油 (7色)</div>
                    <div class="acc-content">
                        <div class="options-grid" id="cream-options"></div>
                    </div>
                </div>
                <!-- 2. 鮮花 -->
                <div class="acc-item" data-type="flower">
                    <div class="acc-header" onclick="toggleAcc(this)">鮮花配料</div>
                    <div class="acc-content">
                        <div class="options-grid">
                            <div class="opt-btn" onclick="selectTool('flower', 'sakura', this)">🌸</div>
                            <div class="opt-btn" onclick="selectTool('flower', 'osmanthus', this)">🌼</div>
                            <div class="opt-btn" onclick="selectTool('flower', 'rose', this)">🌹</div>
                        </div>
                    </div>
                </div>
                <!-- 3. 水果 -->
                <div class="acc-item" data-type="fruit">
                    <div class="acc-header" onclick="toggleAcc(this)">新鮮水果</div>
                    <div class="acc-content">
                        <div class="options-grid">
                            <div class="opt-btn" onclick="selectTool('fruit', 'strawberry_whole', this)">🍓</div>
                            <div class="opt-btn" onclick="selectTool('fruit', 'grape_whole', this)">🍇</div>
                            <div class="opt-btn" onclick="selectTool('fruit', 'mango', this)">🥭</div>
                        </div>
                        <div class="sub-grid" id="fruit-subs">
                            <div class="sub-btn active" onclick="selectTool('fruit', 'strawberry_whole', this)">完整草莓</div>
                            <div class="sub-btn" onclick="selectTool('fruit', 'strawberry_slice', this)">草莓切片</div>
                            <div class="sub-btn" onclick="selectTool('fruit', 'grape_whole', this)">完整提子</div>
                            <div class="sub-btn" onclick="selectTool('fruit', 'grape_slice', this)">提子切片</div>
                        </div>
                    </div>
                </div>
                <!-- 4. 堅果 -->
                <div class="acc-item" data-type="nuts">
                    <div class="acc-header" onclick="toggleAcc(this)">堅果碎</div>
                    <div class="acc-content">
                        <div class="options-grid">
                            <div class="opt-btn" onclick="selectTool('nuts', 'almond', this)">🌰</div>
                        </div>
                        <p style="font-size:0.7rem; color:#888; margin-top:5px;">*撒上綜合堅果碎</p>
                    </div>
                </div>
                <!-- 5. 巧克力 -->
                <div class="acc-item" data-type="choco">
                    <div class="acc-header" onclick="toggleAcc(this)">巧克力</div>
                    <div class="acc-content">
                        <div class="sub-grid">
                            <div class="sub-btn active" onclick="selectTool('choco', 'sauce', this)">巧克力醬</div>
                            <div class="sub-btn" onclick="selectTool('choco', 'chips', this)">巧克力碎</div>
                        </div>
                    </div>
                </div>
                <!-- 6. 粉類 -->
                <div class="acc-item" data-type="powder">
                    <div class="acc-header" onclick="toggleAcc(this)">漸層撒粉</div>
                    <div class="acc-content">
                        <div class="options-grid">
                            <div class="opt-btn" onclick="selectTool('powder', 'sugar', this)" style="background:var(--c-bg)">✨</div>
                            <div class="opt-btn" onclick="selectTool('powder', 'matcha', this)" style="color:#88cc88">🍵</div>
                            <div class="opt-btn" onclick="selectTool('powder', 'cocoa', this)" style="color:#8b5a2b">☕</div>
                        </div>
                    </div>
                </div>
                <!-- 7. 冰淇淋 -->
                <div class="acc-item" data-type="icecream">
                    <div class="acc-header" onclick="toggleAcc(this)">冰淇淋球</div>
                    <div class="acc-content">
                        <div class="options-grid">
                            <div class="opt-btn" onclick="selectTool('icecream', 'vanilla', this)">🍨</div>
                            <div class="opt-btn" onclick="selectTool('icecream', 'choco', this)">🍧</div>
                            <div class="opt-btn" onclick="selectTool('icecream', 'matcha', this)">🍦</div>
                        </div>
                    </div>
                </div>
                <!-- 8. 糖果 -->
                <div class="acc-item" data-type="candy">
                    <div class="acc-header" onclick="toggleAcc(this)">彩色糖果碎</div>
                    <div class="acc-content">
                        <div class="options-grid">
                            <div class="opt-btn" onclick="selectTool('candy', 'sprinkle', this)">🍬</div>
                        </div>
                    </div>
                </div>
                <!-- 9. 餅乾 -->
                <div class="acc-item" data-type="cookie">
                    <div class="acc-header" onclick="toggleAcc(this)">餅乾</div>
                    <div class="acc-content">
                        <div class="sub-grid">
                            <div class="sub-btn active" onclick="selectTool('cookie', 'whole', this)">完整餅乾</div>
                            <div class="sub-btn" onclick="selectTool('cookie', 'crushed', this)">餅乾碎</div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="sidebar-footer">
                <button class="btn" style="width:100%;" onclick="goToPage('page-scene')">選場景 ➔</button>
            </div>
        </div>
    </div>

    <!-- 頁面 3：選場景 -->
    <div id="page-scene" class="page">
        <div class="page-header">🌍 選擇分享場景</div>
        <div class="scene-list">
            <div class="scene-card sc-restaurant" onclick="selectScene('restaurant', this)">
                <div class="sc-emoji">🍽️</div>
                <span>高級餐廳</span>
            </div>
            <div class="scene-card sc-home" onclick="selectScene('home', this)">
                <div class="sc-emoji">🛋️</div>
                <span>溫馨小家</span>
            </div>
            <div class="scene-card sc-beach" onclick="selectScene('beach', this)">
                <div class="sc-emoji">🏖️</div>
                <span>陽光沙灘</span>
            </div>
        </div>
        <div class="fixed-bottom">
            <button class="btn" id="finishBtn" disabled onclick="finishGame()">完成製作 ✨</button>
        </div>
    </div>

    <!-- 頁面 4：最終展示 -->
    <div id="page-final" class="page">
        <div class="final-bg" id="finalBg"></div>
        <div class="rug-el" id="finalRug"></div>
        <div class="table-el" id="finalTable"></div>
        <img id="cakeImg" src="" alt="你的蛋糕">
        
        <div class="final-title">🎉 糕手蛋，生日快樂！</div>
        
        <div class="final-actions">
            <button class="btn btn-outline" onclick="location.reload()">🔄 再做一個</button>
            <button class="btn" onclick="saveImage()">📸 保存圖片</button>
        </div>
    </div>

<script>
    // ================= 狀態與變數 =================
    let gameState = {
        baseType: 'plain',
        layers: [],
        currentTool: { type: 'cream', sub: '#FFB3C6' },
        scene: null
    };

    const canvas = document.getElementById('cakeCanvas');
    const ctx = canvas.getContext('2d');
    const CW = 800, CH = 800;
    const creamColors = ['#FFB3C6', '#B0E9D9', '#FFF3B0', '#D9C6F0', '#B8DFF5', '#FFD4B5', '#F7A9C0'];

    // ================= 背景音樂 (Web Audio API 合成) =================
    let audioCtx = null;
    let isPlayingBGM = false;
    let bgmInterval = null;
    function toggleBGM() {
        const btn = document.getElementById('bgmBtn');
        if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        
        if (isPlayingBGM) {
            clearInterval(bgmInterval);
            btn.textContent = '🔇';
            isPlayingBGM = false;
        } else {
            btn.textContent = '🎵';
            isPlayingBGM = true;
            playMelody();
            bgmInterval = setInterval(playMelody, 4000);
        }
    }
    function playMelody() {
        // 簡易生日快樂歌
        const notes = [262, 262, 294, 262, 349, 330, 262, 262, 294, 262, 392, 349];
        let time = audioCtx.currentTime;
        notes.forEach((freq, i) => {
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = 'sine';
            osc.frequency.value = freq;
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            gain.gain.setValueAtTime(0, time);
            gain.gain.linearRampToValueAtTime(0.1, time + 0.05);
            gain.gain.exponentialRampToValueAtTime(0.001, time + 0.4);
            osc.start(time);
            osc.stop(time + 0.5);
            time += 0.4; // 節拍
        });
    }

    // ================= 頁面切換 =================
    function goToPage(pageId) {
        document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
        document.getElementById(pageId).classList.add('active');
        if (pageId === 'page-decorate') initCanvas();
    }

    // ================= 頁面 1：選胚 =================
    function selectBase(el) {
        document.querySelectorAll('.base-item').forEach(c => c.classList.remove('active'));
        el.classList.add('active');
        el.classList.add('bounce');
        setTimeout(() => el.classList.remove('bounce'), 300);
        gameState.baseType = el.dataset.base;
    }
    // 初始化預覽圖
    window.onload = () => {
        document.querySelectorAll('.mini-base').forEach(cv => {
            const c = cv.getContext('2d');
            const type = cv.dataset.type;
            c.beginPath(); c.ellipse(40, 40, 30, 20, 0, 0, Math.PI*2);
            c.fillStyle = type === 'choco' ? '#7a2f2a' : (type === 'cream' ? '#f7f6f0' : '#f2e2c4');
            c.fill(); c.strokeStyle = '#bfb9a3'; c.lineWidth = 2; c.stroke();
        });
        // 初始化奶油選項
        const cp = document.getElementById('cream-options');
        creamColors.forEach((color, i) => {
            const div = document.createElement('div');
            div.className = `opt-btn ${i === 0 ? 'active' : ''}`;
            div.style.backgroundColor = color;
            div.onclick = () => selectTool('cream', color, div);
            cp.appendChild(div);
        });
    };

    // ================= 頁面 2：裝飾 =================
    function toggleAcc(header) {
        const item = header.parentElement;
        const isActive = item.classList.contains('active');
        document.querySelectorAll('.acc-item').forEach(i => i.classList.remove('active'));
        if (!isActive) item.classList.add('active');
    }

    function selectTool(type, sub, el) {
        gameState.currentTool = { type, sub };
        // 移除同組高亮
        const group = el.closest('.acc-content');
        group.querySelectorAll('.opt-btn, .sub-btn').forEach(b => b.classList.remove('active'));
        el.classList.add('active');
    }

    function initCanvas() {
        ctx.setTransform(1, 0, 0, 1, 0, 0);
        drawCake();
    }

    // 防抖動紀錄
    let lastTapTime = 0;
    let lastTapX = 0;
    let lastTapY = 0;

    canvas.addEventListener('pointerdown', (e) => {
        const rect = canvas.getBoundingClientRect();
        const x = (e.clientX - rect.left) * (CW / rect.width);
        const y = (e.clientY - rect.top) * (CH / rect.height);

        // 防誤觸與防抖 (100ms內且距離小於30px則忽略)
        const now = Date.now();
        const dist = Math.hypot(x - lastTapX, y - lastTapY);
        if (now - lastTapTime < 100 && dist < 30) return;
        lastTapTime = now;
        lastTapX = x;
        lastTapY = y;

        // 限制在蛋糕頂部區域
        const dx = x - 400;
        const dy = (y - 400) / 0.6;
        if (dx*dx + dy*dy > 250*250) return; 

        gameState.layers.push({ type: gameState.currentTool.type, sub: gameState.currentTool.sub, x, y });
        drawCake();
    });

    function undo() {
        if (gameState.layers.length > 0) {
            gameState.layers.pop();
            drawCake();
        }
    }

    function clearCake() {
        if (confirm('確定要清空所有裝飾嗎？')) {
            gameState.layers = [];
            drawCake();
        }
    }

    // ================= 核心繪製 =================
    function drawCake() {
        ctx.clearRect(0, 0, CW, CH);
        
        // 繪製蛋糕胚
        ctx.save();
        ctx.beginPath(); ctx.ellipse(400, 550, 250, 60, 0, 0, Math.PI * 2); ctx.fillStyle = 'rgba(102, 40, 31, 0.1)'; ctx.fill();
        ctx.beginPath(); ctx.ellipse(400, 450, 240, 140, 0, 0, Math.PI, false); ctx.lineTo(640, 400); ctx.ellipse(400, 400, 240, 140, 0, 0, Math.PI, true);
        ctx.fillStyle = gameState.baseType === 'choco' ? '#7a2f2a' : (gameState.baseType === 'cream' ? '#f7f6f0' : '#f2e2c4');
        ctx.fill();
        ctx.beginPath(); ctx.ellipse(400, 400, 240, 140, 0, 0, Math.PI * 2);
        ctx.fillStyle = gameState.baseType === 'choco' ? '#994d47' : (gameState.baseType === 'cream' ? '#f7f6f0' : '#f2e2c4');
        ctx.fill();
        ctx.restore();

        // 繪製配料 (按加入順序疊加)
        gameState.layers.forEach(layer => {
            ctx.save();
            ctx.shadowColor = 'rgba(102, 40, 31, 0.25)';
            ctx.shadowBlur = 6;
            ctx.shadowOffsetY = 4;

            if (layer.type === 'cream') drawCream(layer.x, layer.y, layer.sub);
            else if (layer.type === 'flower') drawFlower(layer.x, layer.y, layer.sub);
            else if (layer.type === 'fruit') drawFruit(layer.x, layer.y, layer.sub);
            else if (layer.type === 'nuts') drawNuts(layer.x, layer.y);
            else if (layer.type === 'choco') drawChoco(layer.x, layer.y, layer.sub);
            else if (layer.type === 'powder') drawPowder(layer.x, layer.y, layer.sub);
            else if (layer.type === 'icecream') drawIceCream(layer.x, layer.y, layer.sub);
            else if (layer.type === 'candy') drawCandy(layer.x, layer.y);
            else if (layer.type === 'cookie') drawCookie(layer.x, layer.y, layer.sub);
            ctx.restore();
        });
    }

    function drawCream(x, y, color) {
        ctx.fillStyle = color;
        ctx.beginPath(); ctx.ellipse(x, y, 20, 15, 0, 0, Math.PI * 2); ctx.fill();
        ctx.beginPath(); ctx.moveTo(x - 12, y - 2); ctx.quadraticCurveTo(x - 5, y - 30, x, y - 35); ctx.quadraticCurveTo(x + 5, y - 30, x + 12, y - 2); ctx.fill();
        ctx.fillStyle = 'rgba(255,255,255,0.7)'; ctx.beginPath(); ctx.ellipse(x - 6, y - 8, 5, 8, -0.5, 0, Math.PI*2); ctx.fill();
    }
    function drawFlower(x, y, sub) {
        if (sub === 'sakura') {
            ctx.fillStyle = '#FFC0CB';
            for(let i=0;i<5;i++) { let angle = (i/5)*Math.PI*2; ctx.beginPath(); ctx.ellipse(x + Math.cos(angle)*8, y + Math.sin(angle)*8, 7, 11, angle, 0, Math.PI*2); ctx.fill(); }
            ctx.fillStyle = '#994d47'; ctx.beginPath(); ctx.arc(x, y, 4, 0, Math.PI*2); ctx.fill();
        } else if (sub === 'osmanthus') {
            ctx.fillStyle = '#FFD700';
            for(let i=0;i<8;i++) { ctx.beginPath(); ctx.arc(x + (Math.random()-0.5)*30, y + (Math.random()-0.5)*30, 3, 0, Math.PI*2); ctx.fill(); }
        } else {
            ctx.fillStyle = '#994d47'; ctx.beginPath(); ctx.arc(x, y, 14, 0, Math.PI*2); ctx.fill();
            ctx.strokeStyle = '#66281f'; ctx.lineWidth = 3; ctx.beginPath(); ctx.arc(x, y, 8, 0, Math.PI*2); ctx.stroke();
        }
    }
    function drawFruit(x, y, sub) {
        if (sub === 'strawberry_whole') {
            ctx.fillStyle = '#994d47'; ctx.beginPath(); ctx.moveTo(x, y+18); ctx.bezierCurveTo(x-18, y+5, x-18, y-18, x, y-12); ctx.bezierCurveTo(x+18, y-18, x+18, y+5, x, y+18); ctx.fill();
            ctx.fillStyle = '#9d8361'; ctx.beginPath(); ctx.moveTo(x, y-12); ctx.lineTo(x-8, y-20); ctx.lineTo(x+8, y-20); ctx.fill();
            ctx.fillStyle = '#FFF'; ctx.beginPath(); ctx.arc(x-6, y, 1.5, 0, Math.PI*2); ctx.arc(x+6, y+3, 1.5, 0, Math.PI*2); ctx.fill();
        } else if (sub === 'strawberry_slice') {
            ctx.fillStyle = '#994d47'; ctx.beginPath(); ctx.moveTo(x, y-12); ctx.lineTo(x-15, y+12); ctx.lineTo(x+15, y+12); ctx.closePath(); ctx.fill();
            ctx.fillStyle = '#f7f6f0'; ctx.beginPath(); ctx.moveTo(x, y-5); ctx.lineTo(x-8, y+10); ctx.lineTo(x+8, y+10); ctx.closePath(); ctx.fill();
        } else if (sub === 'grape_whole') {
            ctx.fillStyle = '#66281f'; ctx.beginPath(); ctx.arc(x, y, 14, 0, Math.PI*2); ctx.fill();
            ctx.fillStyle = 'rgba(255,255,255,0.4)'; ctx.beginPath(); ctx.arc(x-5, y-5, 5, 0, Math.PI*2); ctx.fill();
        } else if (sub === 'grape_slice') {
            ctx.fillStyle = '#9d8361'; ctx.beginPath(); ctx.ellipse(x, y, 16, 10, 0, 0, Math.PI*2); ctx.fill();
            ctx.fillStyle = '#bfb9a3'; ctx.beginPath(); ctx.ellipse(x, y, 10, 5, 0, 0, Math.PI*2); ctx.fill();
        } else if (sub === 'mango') {
            ctx.fillStyle = '#FFBF00'; ctx.fillRect(x-9, y-9, 18, 18);
            ctx.fillStyle = '#FFF3B0'; ctx.fillRect(x-4, y-4, 8, 8);
        }
    }
    function drawNuts(x, y) {
        const colors = ['#9d8361', '#66281f', '#bfb9a3', '#4a1813'];
        for(let i=0; i<20; i++) { ctx.fillStyle = colors[Math.floor(Math.random()*colors.length)]; ctx.beginPath(); ctx.arc(x + (Math.random()-0.5)*45, y + (Math.random()-0.5)*45, 2 + Math.random()*4, 0, Math.PI*2); ctx.fill(); }
    }
    function drawChoco(x, y, sub) {
        if (sub === 'sauce') {
            ctx.fillStyle = '#4a1813'; ctx.beginPath(); ctx.ellipse(x, y, 25, 18, Math.random()*Math.PI, 0, Math.PI*2); ctx.fill();
            ctx.fillStyle = 'rgba(255,255,255,0.15)'; ctx.beginPath(); ctx.ellipse(x-6, y-5, 10, 4, 0, 0, Math.PI*2); ctx.fill();
        } else {
            for(let i=0; i<15; i++) { ctx.fillStyle = '#66281f'; ctx.fillRect(x + (Math.random()-0.5)*40, y + (Math.random()-0.5)*40, 6, 6); }
        }
    }
    function drawPowder(x, y, sub) {
        let color = 'rgba(247, 246, 240, 0.9)';
        if (sub === 'matcha') color = 'rgba(139,195,74,0.85)';
        if (sub === 'cocoa') color = 'rgba(102, 40, 31,0.85)';
        let grad = ctx.createRadialGradient(x, y, 0, x, y, 65);
        grad.addColorStop(0, color); grad.addColorStop(1, 'rgba(0,0,0,0)');
        ctx.fillStyle = grad; ctx.beginPath(); ctx.arc(x, y, 65, 0, Math.PI*2); ctx.fill();
        for(let i=0; i<40; i++) { let r = Math.random() * 65; let theta = Math.random() * Math.PI * 2; ctx.fillStyle = color; ctx.beginPath(); ctx.arc(x + r * Math.cos(theta), y + r * Math.sin(theta), 1.5, 0, Math.PI*2); ctx.fill(); }
    }
    function drawIceCream(x, y, sub) {
        let c1 = '#f7f6f0', c2 = '#e6e5d5';
        if (sub === 'choco') { c1 = '#9d8361'; c2 = '#66281f'; }
        if (sub === 'matcha') { c1 = '#B0E9D9'; c2 = '#88cc88'; }
        ctx.fillStyle = c1; ctx.beginPath(); ctx.arc(x, y, 22, 0, Math.PI*2); ctx.fill();
        ctx.fillStyle = c2; ctx.beginPath(); ctx.arc(x-8, y+8, 14, 0, Math.PI*2); ctx.fill(); ctx.beginPath(); ctx.arc(x+8, y+8, 14, 0, Math.PI*2); ctx.fill();
        ctx.fillStyle = 'rgba(255,255,255,0.5)'; ctx.beginPath(); ctx.arc(x-5, y-5, 6, 0, Math.PI*2); ctx.fill();
    }
    function drawCandy(x, y) {
        const colors = ['#994d47', '#bfb9a3', '#9d8361', '#e6e5d5', '#f7f6f0'];
        for(let i=0; i<15; i++) { ctx.fillStyle = colors[Math.floor(Math.random()*colors.length)]; let angle = Math.random() * Math.PI * 2; let dist = Math.random() * 35; ctx.fillRect(x + Math.cos(angle)*dist, y + Math.sin(angle)*dist, 5, 5); }
    }
    function drawCookie(x, y, sub) {
        if (sub === 'whole') {
            ctx.fillStyle = '#9d8361'; ctx.beginPath(); ctx.arc(x, y, 22, 0, Math.PI*2); ctx.fill();
            ctx.fillStyle = '#66281f'; ctx.beginPath(); ctx.arc(x-8, y-8, 3.5, 0, Math.PI*2); ctx.arc(x+8, y-2, 3.5, 0, Math.PI*2); ctx.arc(x-2, y+10, 3.5, 0, Math.PI*2); ctx.fill();
        } else {
            for(let i=0; i<20; i++) { ctx.fillStyle = '#66281f'; ctx.fillRect(x + (Math.random()-0.5)*45, y + (Math.random()-0.5)*45, 4, 4); }
        }
    }

    // ================= 頁面 3 & 4：場景與最終展示 =================
    function selectScene(scene, el) {
        gameState.scene = scene;
        document.querySelectorAll('.scene-card').forEach(c => c.classList.remove('active'));
        el.classList.add('active');
        document.getElementById('finishBtn').disabled = false;
    }

    function finishGame() {
        if (!gameState.scene) return;
        goToPage('page-final');
        
        const bg = document.getElementById('finalBg');
        const table = document.getElementById('finalTable');
        const rug = document.getElementById('finalRug');
        const cakeImg = document.getElementById('cakeImg');

        table.className = 'table-el table'; // Reset
        rug.style.display = 'none';

        if (gameState.scene === 'restaurant') {
            bg.style.background = 'linear-gradient(145deg, #2c1e1e, #4a2c2c)'; 
            table.classList.add('restaurant');
        } else if (gameState.scene === 'home') {
            bg.style.background = 'linear-gradient(145deg, #fff0e6, #e6e5d5)'; 
            table.classList.add('home'); rug.style.display = 'block';
        } else if (gameState.scene === 'beach') {
            bg.style.background = 'linear-gradient(180deg, #87CEEB 0%, #E0F6FF 60%, #F4D03F 100%)'; 
            table.classList.add('beach');
        }

        // 導出當前 Canvas 蛋糕
        cakeImg.src = canvas.toDataURL();
        
        // 觸發從天而降動畫
        setTimeout(() => cakeImg.classList.add('show'), 100);
    }

    // 保存圖片功能 (合成背景、桌子與蛋糕)
    function saveImage() {
        const exportCanvas = document.createElement('canvas');
        exportCanvas.width = 800; exportCanvas.height = 1000;
        const eCtx = exportCanvas.getContext('2d');

        // 1. 畫背景
        if (gameState.scene === 'restaurant') {
            let g = eCtx.createLinearGradient(0, 0, 800, 1000); g.addColorStop(0, '#2c1e1e'); g.addColorStop(1, '#4a2c2c');
            eCtx.fillStyle = g; eCtx.fillRect(0, 0, 800, 1000);
        } else if (gameState.scene === 'home') {
            let g = eCtx.createLinearGradient(0, 0, 800, 1000); g.addColorStop(0, '#fff0e6'); g.addColorStop(1, '#e6e5d5');
            eCtx.fillStyle = g; eCtx.fillRect(0, 0, 800, 1000);
            eCtx.fillStyle = '#bfb9a3'; eCtx.beginPath(); eCtx.ellipse(400, 1000, 600, 400, 0, 0, Math.PI*2); eCtx.fill(); // 地毯
        } else {
            let g = eCtx.createLinearGradient(0, 0, 0, 1000); g.addColorStop(0, '#87CEEB'); g.addColorStop(0.6, '#E0F6FF'); g.addColorStop(1, '#F4D03F');
            eCtx.fillStyle = g; eCtx.fillRect(0, 0, 800, 1000);
        }

        // 2. 畫桌子
        if (gameState.scene === 'restaurant') {
            eCtx.fillStyle = '#7a2f2a'; eCtx.beginPath(); eCtx.ellipse(400, 1000, 600, 350, 0, 0, Math.PI*2); eCtx.fill();
        } else if (gameState.scene === 'home') {
            eCtx.fillStyle = 'rgba(255,255,255,0.6)'; eCtx.beginPath(); eCtx.ellipse(400, 1000, 550, 300, 0, 0, Math.PI*2); eCtx.fill();
        } else {
            eCtx.fillStyle = '#9d8361'; eCtx.fillRect(50, 750, 700, 250);
        }

        // 3. 畫蛋糕
        const img = new Image();
        img.onload = () => {
            eCtx.drawImage(img, 100, 200, 600, 600);

            // 4. 加水印或文字
            eCtx.fillStyle = '#66281f';
            eCtx.font = 'bold 40px sans-serif';
            eCtx.textAlign = 'center';
            eCtx.fillText('🎂 糕手蛋，生日快樂！ 🎈', 400, 100);

            // 5. 觸發下載
            const link = document.createElement('a');
            link.download = 'birthday_cake.png';
            link.href = exportCanvas.toDataURL('image/png');
            link.click();
        };
        img.src = canvas.toDataURL();
    }
</script>
</body>
</html>