# hello
<!--  
诊所导航助手 · 最终版（到达目标诊室不显示箭头，只显示庆祝）
地形：走廊（1.jpg）→ 收费室（2.jpg）→ 9号（3.jpg）、10号（4.jpg）、11号（5.jpg）
箭头方向：
- 走廊：⬆️去收费室
- 收费室：➡️去9号
- 9号诊室：➡️去10号
- 10号诊室：➡️去11号
- 11号诊室：无箭头
重要：到达目标诊室时只显示🎉，不再显示向右箭头
-->
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.5">
    <title>诊所导航 · 最终版</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            background: #e6f2fa;
            font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 10px;
        }
        .app-box {
            max-width: 480px;
            width: 100%;
            background: white;
            border-radius: 28px;
            box-shadow: 0 10px 20px rgba(0,40,70,0.15);
            padding: 16px;
            border: 3px solid #ff9933;
        }
        
        /* 头部 */
        .title {
            font-size: 22px;
            font-weight: bold;
            color: #003f5c;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 8px;
            background: #fff0cf;
            padding: 8px 15px;
            border-radius: 40px;
        }
        .title span {
            font-size: 26px;
        }
        
        /* 护士按钮 */
        .nurse-btn {
            text-align: right;
            margin-bottom: 8px;
        }
        #nurseBtn {
            background: #4d79ff;
            color: white;
            border: none;
            padding: 5px 12px;
            border-radius: 25px;
            font-size: 14px;
            cursor: pointer;
        }
        
        /* 护士设置面板 */
        .nurse-panel {
            background: #e6f0ff;
            border-radius: 20px;
            padding: 15px;
            margin-bottom: 15px;
            border: 2px solid #3366cc;
            display: none;
        }
        .nurse-title {
            font-size: 18px;
            margin-bottom: 10px;
            color: #003399;
        }
        .nurse-room {
            display: flex;
            align-items: center;
            margin: 8px 0;
            background: white;
            padding: 8px;
            border-radius: 25px;
        }
        .nurse-room-name {
            font-size: 16px;
            width: 100px;
        }
        .nurse-input {
            font-size: 18px;
            width: 70px;
            text-align: center;
            border: 2px solid #4d79ff;
            border-radius: 12px;
            padding: 4px;
            margin-left: auto;
        }
        .nurse-unit {
            font-size: 14px;
            margin-left: 8px;
            margin-right: 8px;
        }
        .nurse-buttons {
            display: flex;
            gap: 8px;
            margin-top: 12px;
        }
        .save-btn, .close-btn {
            flex: 1;
            border: none;
            padding: 10px;
            border-radius: 25px;
            font-size: 18px;
            cursor: pointer;
            color: white;
        }
        .save-btn { background: #28a745; }
        .close-btn { background: #dc3545; }
        
        /* 输入框 */
        .search-box {
            background: #fff5e6;
            border-radius: 40px;
            border: 2px solid #ff944d;
            display: flex;
            align-items: center;
            padding: 3px 3px 3px 15px;
            margin-bottom: 15px;
        }
        .search-box input {
            flex: 1;
            border: none;
            background: transparent;
            padding: 10px 5px;
            font-size: 18px;
            outline: none;
            color: #003366;
        }
        .search-box input::placeholder {
            color: #b38b6d;
            font-size: 16px;
        }
        .search-box button {
            background: #ff751a;
            border: none;
            color: white;
            font-size: 18px;
            font-weight: bold;
            padding: 8px 18px;
            border-radius: 35px;
            cursor: pointer;
            border: 2px solid #cc5200;
        }
        
        /* 挂号信息卡片 */
        .ticket-card {
            background: #ffecb3;
            border-radius: 25px;
            padding: 15px;
            margin-bottom: 15px;
            border: 2px solid #ff8000;
        }
        .ticket-title {
            font-size: 18px;
            color: #004d66;
            margin-bottom: 8px;
        }
        .number-row {
            display: flex;
            align-items: center;
            justify-content: space-around;
            margin: 10px 0;
        }
        .current-num {
            background: #ff3333;
            color: white;
            font-size: 40px;
            font-weight: 900;
            padding: 5px 18px;
            border-radius: 30px;
            border: 2px solid white;
        }
        .wait-time {
            background: #00a36c;
            color: white;
            font-size: 26px;
            font-weight: bold;
            padding: 6px 18px;
            border-radius: 35px;
            display: inline-block;
        }
        
        /* 缴费提醒 */
        .payment-reminder {
            background: #ff6b6b;
            color: white;
            padding: 12px;
            border-radius: 20px;
            margin: 10px 0;
            font-size: 18px;
            text-align: center;
            font-weight: bold;
            border: 2px solid #c92a2a;
        }
        .payment-done {
            background: #51cf66;
            color: white;
            padding: 12px;
            border-radius: 20px;
            margin: 10px 0;
            font-size: 18px;
            text-align: center;
            font-weight: bold;
            border: 2px solid #2b8e3c;
            cursor: pointer;
        }
        
        /* 照片导航区域 */
        .photo-section {
            background: #667eea;
            border-radius: 25px;
            padding: 15px;
            margin: 15px 0;
            color: white;
        }
        .photo-header {
            font-size: 18px;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .photo-container {
            position: relative;
            width: 100%;
            height: 260px;
            background: #333;
            border-radius: 20px;
            overflow: hidden;
            border: 3px solid white;
            margin: 8px 0;
        }
        #navPhoto {
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: block;
        }
        .arrow-layer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }
        .nav-arrow {
            position: absolute;
            font-size: 50px;
            filter: drop-shadow(0 0 8px gold);
            animation: bounce 1.5s infinite;
            pointer-events: auto;
            cursor: pointer;
            z-index: 10;
            text-shadow: 1px 1px 5px black;
        }
        @keyframes bounce {
            0%, 100% { transform: translate(-50%, -50%) scale(1); }
            50% { transform: translate(-50%, -50%) scale(1.15); }
        }
        .location-tag {
            background: rgba(0,0,0,0.5);
            color: white;
            padding: 6px 15px;
            border-radius: 30px;
            font-size: 16px;
            display: inline-block;
            margin-bottom: 5px;
            border: 1px solid #ffaa00;
        }
        .nav-buttons {
            display: flex;
            gap: 8px;
            margin-top: 8px;
        }
        .nav-btn {
            flex: 1;
            background: white;
            color: #333;
            border: none;
            padding: 10px;
            border-radius: 30px;
            font-size: 18px;
            font-weight: bold;
            cursor: pointer;
            border: 2px solid #ff9933;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }
        .speak-btn {
            background: #4CAF50;
            color: white;
            border-color: #2E7D32;
        }
        
        /* 位置选择网格 */
        .location-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 4px;
            margin: 10px 0;
        }
        .loc-item {
            background: rgba(255,255,255,0.2);
            border: 1px solid white;
            border-radius: 25px;
            padding: 8px 2px;
            font-size: 20px;
            color: white;
            cursor: pointer;
            text-align: center;
            backdrop-filter: blur(5px);
        }
        .loc-item span {
            display: block;
            font-size: 11px;
            margin-top: 2px;
        }
        .loc-item.active {
            background: #ffaa00;
            border-color: #ff6600;
            color: #333;
        }
        
        /* 短信分享区域 */
        .share-section {
            background: #e8f0fe;
            border-radius: 20px;
            padding: 15px;
            margin: 15px 0;
            border: 2px solid #2196F3;
        }
        .share-title {
            font-size: 18px;
            color: #0d47a1;
            margin-bottom: 10px;
        }
        .phone-input {
            width: 100%;
            padding: 12px;
            font-size: 18px;
            border: 2px solid #2196F3;
            border-radius: 30px;
            margin: 8px 0;
            text-align: center;
        }
        .button-group {
            display: flex;
            gap: 8px;
        }
        .sms-button, .copy-button {
            flex: 1;
            border: none;
            padding: 12px;
            border-radius: 30px;
            font-size: 18px;
            font-weight: bold;
            cursor: pointer;
            color: white;
        }
        .sms-button { background: #2196F3; border: 2px solid #1565C0; }
        .copy-button { background: #ff9800; border: 2px solid #c66900; }
        
        /* 诊室列表 */
        .rooms-list {
            background: #cce6ff;
            border-radius: 25px;
            padding: 15px;
            margin-top: 15px;
        }
        .rooms-title {
            font-size: 18px;
            font-weight: bold;
            color: #00427a;
            margin-bottom: 12px;
        }
        .room-item {
            background: white;
            border-radius: 25px;
            padding: 10px;
            margin: 6px 0;
            display: flex;
            align-items: center;
            gap: 10px;
            border: 2px solid #ffaa33;
            font-size: 16px;
            cursor: pointer;
        }
        .room-num {
            font-size: 22px;
            font-weight: bold;
            color: #b33c00;
            min-width: 80px;
        }
        .room-item.highlight {
            background: #ffff99;
            border: 3px solid #ff6600;
        }
        
        /* 庆祝动画 */
        .celebration {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 80px;
            animation: bounce 1s infinite;
            z-index: 30;
            text-shadow: 0 0 30px gold;
            pointer-events: none;
        }
        
        .footer {
            font-size: 14px;
            color: #666;
            text-align: center;
            margin-top: 15px;
        }
    </style>
</head>
<body>
    <div class="app-box">
        <div class="title">
            <span>🏥</span> 诊所导航·最终版
        </div>
        
        <!-- 护士设置按钮 -->
        <div class="nurse-btn">
            <button id="nurseBtn">👩‍⚕️ 护士设置</button>
        </div>
        
        <!-- 护士设置面板 -->
        <div id="nursePanel" class="nurse-panel">
            <div class="nurse-title">⚙️ 更新当前看诊号码</div>
            <div id="nurseRooms"></div>
            <div class="nurse-buttons">
                <button id="saveNurseBtn" class="save-btn">💾 保存</button>
                <button id="closeNurseBtn" class="close-btn">✖️ 关闭</button>
            </div>
        </div>
        
        <!-- 症状输入 -->
        <div class="search-box">
            <input type="text" id="symptomInput" placeholder="输入症状..." value="胸痛">
            <button id="searchBtn">查询</button>
        </div>
        
        <!-- 挂号信息动态显示区 -->
        <div id="ticketArea"></div>
        
        <!-- 缴费状态区域 -->
        <div id="paymentArea"></div>
        
        <!-- 照片导航区域 -->
        <div class="photo-section">
            <div class="photo-header">
                <span>📸</span> 跟着照片走（点箭头）
            </div>
            
            <!-- 当前位置标签 -->
            <div class="location-tag" id="locationTag">🛤️ 走廊</div>
            
            <!-- 照片容器 - 这里用了你给的图床链接 -->
            <div class="photo-container">
                <img id="navPhoto" src="https://s41.ax1x.com/2026/03/03/pepxLI1.jpg" alt="实景">
                <div class="arrow-layer" id="arrowLayer"></div>
            </div>
            
            <!-- 语音按钮 -->
            <div style="margin: 8px 0;">
                <button class="nav-btn speak-btn" id="speakBtn">
                    🔊 听声音
                </button>
            </div>
            
            <!-- 快速位置选择 -->
            <div style="font-size: 14px; margin: 5px 0;">点一下您在哪：</div>
            <div class="location-grid" id="locationGrid">
                <div class="loc-item active" data-loc="走廊">🛤️<span>走廊</span></div>
                <div class="loc-item" data-loc="收费室">💰<span>缴费</span></div>
                <div class="loc-item" data-loc="9号诊室">❤️<span>胸痛</span></div>
                <div class="loc-item" data-loc="10号诊室">🌡️<span>发热</span></div>
                <div class="loc-item" data-loc="11号诊室">👶<span>妇儿</span></div>
            </div>
            
            <!-- 翻页按钮 -->
            <div class="nav-buttons">
                <button class="nav-btn" id="prevBtn">⬅️ 上一张</button>
                <button class="nav-btn" id="nextBtn">下一张 ➡️</button>
            </div>
        </div>
        
        <!-- 短信分享区域 -->
        <div class="share-section">
            <div class="share-title">📨 告诉子女</div>
            <input type="tel" id="childPhone" class="phone-input" placeholder="输入子女手机号" value="13800138000">
            <div class="button-group">
                <button onclick="sendSMS()" class="sms-button">✉️ 发短信</button>
                <button onclick="copyInfo()" class="copy-button">📋 复制</button>
            </div>
        </div>
        
        <!-- 所有诊室列表 -->
        <div class="rooms-list">
            <div class="rooms-title">📋 所有诊室 (点击直接挂号)</div>
            <div id="roomsContainer"></div>
        </div>
        
        <div class="footer">
            ⭐ 先到收费室缴费，再去诊室看病
        </div>
    </div>

    <script>
        // ===== 诊所数据 =====
        let queueNumbers = {
            '收费室': 5,
            '9号诊室': 15,
            '10号诊室': 8,
            '11号诊室': 22
        };
        
        // 缴费状态
        let hasPaid = false;
        
        const clinicData = [
            { 
                id: 1, 
                symptom: '冷汗,喘不上气,左肩膀疼,胳膊疼,恶心,想吐,心脏痛,心脏疼,针扎胸口一样的痛,针扎胸口一样的疼,心跳快,咳血,嘴唇发紫', 
                room: '9号诊室', 
                dept: '胸痛诊室', 
                avgWaitTime: 8, 
                icon: '❤️',
               必须先缴费: true
            },
            { 
                id: 2, 
                symptom: '鼻涕,咳嗽,嗓子痒,低烧,脑袋沉,感冒,痰', 
                room: '10号诊室', 
                dept: '发热诊室', 
                avgWaitTime: 6, 
                icon: '🌡️',
               必须先缴费: true
            },
            { 
                id: 3, 
                symptom: '生孩子,下腹痛', 
                room: '11号诊室', 
                dept: '妇女保健室', 
                avgWaitTime: 10, 
                icon: '👶',
               必须先缴费: true
            },
            { 
                id: 4, 
                symptom: '头痛,小孩', 
                room: '11号诊室', 
                dept: '儿科', 
                avgWaitTime: 5, 
                icon: '🤕',
               必须先缴费: true
            }
        ];

        // ===== 照片对应关系 =====
        const photos = {
            '走廊': 'https://s41.ax1x.com/2026/03/03/pepxLI1.jpg',
            '收费室': 'https://s41.ax1x.com/2026/03/03/pepxqaR.jpg',
            '9号诊室': 'https://s41.ax1x.com/2026/03/03/pepx7qJ.jpg',
            '10号诊室': 'https://s41.ax1x.com/2026/03/03/pepxTr4.jpg',
            '11号诊室': 'https://s41.ax1x.com/2026/03/03/pepxbZ9.jpg'
        };
        
        // ===== 箭头数据 =====
        const arrows = {
            // 走廊 -> 收费室
            '走廊': [
                { emoji: '⬆️', left: '50%', top: '70%', next: '收费室', text: '直走去收费室缴费' }
            ],
            
            // 收费室 -> 9号诊室
            '收费室': [
                { emoji: '➡️', left: '70%', top: '50%', next: '9号诊室', text: '右边9号诊室' }
            ],
            
            // 9号诊室 -> 10号诊室
            '9号诊室': [
                { emoji: '➡️', left: '70%', top: '50%', next: '10号诊室', text: '右边10号诊室' }
            ],
            
            // 10号诊室 -> 11号诊室
            '10号诊室': [
                { emoji: '➡️', left: '70%', top: '50%', next: '11号诊室', text: '右边11号诊室' }
            ],
            
            // 11号诊室 -> 无箭头
            '11号诊室': []
        };

        // ===== 状态变量 =====
        let currentLocation = '走廊';
        let targetRoom = null;

        // ===== DOM元素 =====
        const symptomInput = document.getElementById('symptomInput');
        const searchBtn = document.getElementById('searchBtn');
        const ticketArea = document.getElementById('ticketArea');
        const paymentArea = document.getElementById('paymentArea');
        const roomsContainer = document.getElementById('roomsContainer');
        const navPhoto = document.getElementById('navPhoto');
        const locationTag = document.getElementById('locationTag');
        const arrowLayer = document.getElementById('arrowLayer');
        const speakBtn = document.getElementById('speakBtn');
        const prevBtn = document.getElementById('prevBtn');
        const nextBtn = document.getElementById('nextBtn');
        const locationItems = document.querySelectorAll('.loc-item');
        const nurseBtn = document.getElementById('nurseBtn');
        const nursePanel = document.getElementById('nursePanel');
        const nurseRooms = document.getElementById('nurseRooms');
        const saveNurseBtn = document.getElementById('saveNurseBtn');
        const closeNurseBtn = document.getElementById('closeNurseBtn');

        // ===== 语音函数 =====
        function speak(text) {
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel();
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = 'zh-CN';
                utterance.rate = 0.8;
                window.speechSynthesis.speak(utterance);
            }
        }

        // ===== 护士设置功能 =====
        nurseBtn.addEventListener('click', () => {
            let html = '';
            const rooms = ['收费室', '9号诊室', '10号诊室', '11号诊室'];
            rooms.forEach(room => {
                html += `
                    <div class="nurse-room">
                        <div class="nurse-room-name">${room}</div>
                        <input type="number" id="nurse-${room}" value="${queueNumbers[room] || 1}" class="nurse-input">
                        <div class="nurse-unit">号</div>
                    </div>
                `;
            });
            nurseRooms.innerHTML = html;
            nursePanel.style.display = 'block';
        });

        saveNurseBtn.addEventListener('click', () => {
            const rooms = ['收费室', '9号诊室', '10号诊室', '11号诊室'];
            rooms.forEach(room => {
                const input = document.getElementById(`nurse-${room}`);
                if (input) queueNumbers[room] = parseInt(input.value) || 1;
            });
            showTicket(symptomInput.value);
            alert('✅ 号码已更新！');
            nursePanel.style.display = 'none';
        });

        closeNurseBtn.addEventListener('click', () => {
            nursePanel.style.display = 'none';
        });

        // ===== 短信分享功能 =====
        window.sendSMS = function() {
            const phone = document.getElementById('childPhone').value;
            if (!phone) { alert('请输入子女手机号'); return; }
            
            const symptom = symptomInput.value;
            let match = null;
            for (let item of clinicData) {
                if (item.symptom.includes(symptom) || symptom.includes(item.symptom.split(',')[0])) {
                    match = item;
                    break;
                }
            }
            
            if (!match) { alert('请先查询症状'); return; }
            const paidStatus = hasPaid ? '已缴费' : '未缴费';
            const currentNum = queueNumbers[match.room] || 10;
            const yourNum = currentNum + 4;
            const smsText = `老爸/老妈在诊所：\n${match.dept}(${match.room})\n号码：${yourNum}号\n缴费：${paidStatus}\n当前位置：${currentLocation}`;
            window.location.href = `sms:${phone}?body=${encodeURIComponent(smsText)}`;
        };

        window.copyInfo = function() {
            const phone = document.getElementById('childPhone').value;
            const symptom = symptomInput.value;
            
            let match = null;
            for (let item of clinicData) {
                if (item.symptom.includes(symptom) || symptom.includes(item.symptom.split(',')[0])) {
                    match = item;
                    break;
                }
            }
            
            if (!match) { alert('请先查询症状'); return; }
            const paidStatus = hasPaid ? '已缴费' : '未缴费';
            const currentNum = queueNumbers[match.room] || 10;
            const yourNum = currentNum + 4;
            const infoText = `【发给：${phone || '子女手机号'}】\n老爸/老妈在诊所：\n${match.dept}(${match.room})\n号码：${yourNum}号\n缴费：${paidStatus}\n当前位置：${currentLocation}`;
            const textarea = document.createElement('textarea');
            textarea.value = infoText;
            document.body.appendChild(textarea);
            textarea.select();
            document.execCommand('copy');
            document.body.removeChild(textarea);
            alert('✅ 信息已复制！');
        };

        // ===== 更新照片导航 =====
        function updateNav() {
            // 根据当前位置显示照片
            if (photos[currentLocation]) {
                navPhoto.src = photos[currentLocation];
            } else {
                navPhoto.src = 'https://s41.ax1x.com/2026/03/03/pepxLI1.jpg';
            }
            
            // 更新位置标签
            let icon = '📍';
            if (currentLocation === '走廊') icon = '🛤️';
            else if (currentLocation === '收费室') icon = '💰';
            else if (currentLocation === '9号诊室') icon = '❤️';
            else if (currentLocation === '10号诊室') icon = '🌡️';
            else if (currentLocation === '11号诊室') icon = '👶';
            locationTag.innerHTML = `${icon} ${currentLocation}`;
            
            // 清空并重新生成箭头
            arrowLayer.innerHTML = '';
            
            // 重要：如果当前位置就是目标诊室且已缴费，只显示庆祝，不显示箭头
            if (targetRoom && currentLocation === targetRoom && hasPaid) {
                // 显示庆祝动画
                const celebration = document.createElement('div');
                celebration.className = 'celebration';
                celebration.textContent = '🎉';
                arrowLayer.appendChild(celebration);
                speak('到' + targetRoom + '了，祝您早日康复');
            } else {
                // 否则正常显示箭头
                const locationArrows = arrows[currentLocation] || [];
                locationArrows.forEach(arrow => {
                    const arrowDiv = document.createElement('div');
                    arrowDiv.className = 'nav-arrow';
                    arrowDiv.style.left = arrow.left;
                    arrowDiv.style.top = arrow.top;
                    arrowDiv.style.transform = 'translate(-50%, -50%)';
                    arrowDiv.textContent = arrow.emoji;
                    arrowDiv.setAttribute('data-next', arrow.next);
                    arrowDiv.setAttribute('data-text', arrow.text);
                    
                    arrowDiv.addEventListener('click', function() {
                        const next = this.getAttribute('data-next');
                        if (next) {
                            currentLocation = next;
                            updateLocationActive();
                            updateNav();
                            speak('到' + next + '了');
                            
                            if (next === '收费室') {
                                speak('请先在这里缴费');
                            }
                        }
                    });
                    
                    arrowLayer.appendChild(arrowDiv);
                });
            }
            
            updateLocationActive();
        }
        
        function updateLocationActive() {
            locationItems.forEach(item => {
                if (item.dataset.loc === currentLocation) {
                    item.classList.add('active');
                } else {
                    item.classList.remove('active');
                }
            });
        }

        // ===== 症状匹配函数 =====
        function matchSymptom(input) {
            input = input.trim().toLowerCase();
            
            for (let item of clinicData) {
                const symptoms = item.symptom.split(',');
                for (let s of symptoms) {
                    if (s.trim().includes(input) || input.includes(s.trim())) {
                        return item;
                    }
                }
            }
            return null;
        }

        // ===== 更新缴费状态显示 =====
        function updatePaymentDisplay(match) {
            if (!match) return;
            
            if (!hasPaid) {
                paymentArea.innerHTML = `
                    <div class="payment-reminder">
                        ⚠️ 请先到收费室缴费，再去${match.room}看病
                        <button onclick="markPaid()" style="display: block; width: 100%; margin-top: 10px; padding: 8px; background: #51cf66; border: none; border-radius: 20px; color: white; font-size: 16px; font-weight: bold;">✅ 我已缴费</button>
                    </div>
                `;
            } else {
                paymentArea.innerHTML = `
                    <div class="payment-done" onclick="markUnpaid()">
                        ✅ 已缴费，点击可重新标记为未缴费
                    </div>
                `;
            }
        }
        
        window.markPaid = function() {
            hasPaid = true;
            const match = matchSymptom(symptomInput.value);
            updatePaymentDisplay(match);
            speak('缴费成功，可以去看诊了');
            updateNav(); // 重新更新导航，可能隐藏箭头
        };
        
        window.markUnpaid = function() {
            hasPaid = false;
            const match = matchSymptom(symptomInput.value);
            updatePaymentDisplay(match);
            updateNav(); // 重新更新导航，可能显示箭头
        };

        // ===== 挂号信息显示 =====
        function showTicket(symptom) {
            const match = matchSymptom(symptom);
            
            if (match) {
                targetRoom = match.room;
                const currentNum = queueNumbers[match.room] || 10;
                const yourNum = currentNum + 4;
                const waitMins = (yourNum - currentNum) * match.avgWaitTime;
                
                const now = new Date();
                now.setMinutes(now.getMinutes() + waitMins);
                const estTime = `${now.getHours().toString().padStart(2,'0')}:${now.getMinutes().toString().padStart(2,'0')}`;
                
                ticketArea.innerHTML = `
                    <div class="ticket-card">
                        <div class="ticket-title">🎫 挂号信息</div>
                        <div style="display: flex; align-items: center; justify-content: space-around;">
                            <div style="text-align: center;">
                                <div style="font-size: 14px;">当前</div>
                                <div style="font-size: 28px;">${currentNum}号</div>
                            </div>
                            <div style="font-size: 24px;">→</div>
                            <div style="text-align: center;">
                                <div style="font-size: 14px;">您的号码</div>
                                <div class="current-num">${yourNum}号</div>
                            </div>
                        </div>
                        <div style="margin: 10px 0; text-align: center;">
                            <div class="wait-time">预计 ${estTime}</div>
                            <div style="font-size: 14px; margin-top: 5px;">(约等${waitMins}分钟)</div>
                        </div>
                        <div style="background: #ffffcc; padding: 8px; border-radius: 20px; font-size: 14px; text-align: center;">
                            👉 您要去 ${match.room} ${match.dept}
                        </div>
                    </div>
                `;
                
                if (!hasPaid) {
                    paymentArea.innerHTML = `
                        <div class="payment-reminder">
                            ⚠️ 请先到收费室缴费，再去${match.room}看病
                            <button onclick="markPaid()" style="display: block; width: 100%; margin-top: 10px; padding: 8px; background: #51cf66; border: none; border-radius: 20px; color: white; font-size: 16px; font-weight: bold;">✅ 我已缴费</button>
                        </div>
                    `;
                } else {
                    paymentArea.innerHTML = `
                        <div class="payment-done" onclick="markUnpaid()">
                            ✅ 已缴费，点击可重新标记为未缴费
                        </div>
                    `;
                }
                
                document.querySelectorAll('.room-item').forEach(el => {
                    if (el.dataset.room === match.room) {
                        el.classList.add('highlight');
                    } else {
                        el.classList.remove('highlight');
                    }
                });
                
                if (hasPaid) {
                    speak(`您要去${match.room}，请跟着箭头走`);
                } else {
                    speak(`请先到收费室缴费，再去${match.room}`);
                }
                updateNav();
            } else {
                ticketArea.innerHTML = `
                    <div style="background: #ffe0e0; border-radius: 20px; padding: 20px; text-align: center; color: #b30000;">
                        没找到这个症状，请看下面诊室
                    </div>
                `;
            }
        }

        // ===== 渲染诊室列表 =====
        function renderRooms() {
            const uniqueRooms = {};
            clinicData.forEach(item => {
                if (!uniqueRooms[item.room]) {
                    uniqueRooms[item.room] = item;
                }
            });
            
            let html = '';
            Object.values(uniqueRooms).forEach(item => {
                html += `
                    <div class="room-item" data-symptom="${item.symptom.split(',')[0]}" data-room="${item.room}">
                        <div class="room-num">${item.room}</div>
                        <div>${item.dept} ${item.icon}</div>
                    </div>
                `;
            });
            roomsContainer.innerHTML = html;
        }

        // ===== 事件绑定 =====
        searchBtn.addEventListener('click', () => showTicket(symptomInput.value));
        symptomInput.addEventListener('keypress', (e) => { if (e.key === 'Enter') showTicket(symptomInput.value); });
        
        roomsContainer.addEventListener('click', (e) => {
            const item = e.target.closest('.room-item');
            if (item) {
                symptomInput.value = item.dataset.symptom;
                showTicket(item.dataset.symptom);
            }
        });
        
        locationItems.forEach(item => {
            item.addEventListener('click', function() {
                const loc = this.dataset.loc;
                currentLocation = loc;
                updateNav();
                speak('当前位置设为' + loc);
            });
        });
        
        speakBtn.addEventListener('click', () => {
            const locArrows = arrows[currentLocation] || [];
            
            if (targetRoom && currentLocation !== targetRoom) {
                if (!hasPaid && targetRoom !== '收费室') {
                    speak('请先去收费室缴费');
                    return;
                }
                
                let found = false;
                for (let arrow of locArrows) {
                    if (arrow.next === targetRoom) {
                        speak('请' + arrow.text);
                        found = true;
                        break;
                    }
                }
                if (!found) {
                    for (let arrow of locArrows) {
                        if (arrow.next === '收费室') {
                            speak('请先去' + arrow.text);
                            found = true;
                            break;
                        }
                    }
                }
                if (!found) speak('您现在在' + currentLocation);
            } else if (locArrows.length > 0) {
                let text = '您现在在' + currentLocation + '，';
                locArrows.forEach(a => text += '可以' + a.text + '去' + a.next + '，');
                speak(text);
            } else {
                speak('您现在在' + currentLocation);
            }
        });
        
        const locations = ['走廊', '收费室', '9号诊室', '10号诊室', '11号诊室'];
        prevBtn.addEventListener('click', () => {
            let idx = locations.indexOf(currentLocation);
            if (idx > 0) {
                currentLocation = locations[idx - 1];
                updateNav();
                speak('上一张，现在是' + currentLocation);
            } else {
                speak('已经是第一张了');
            }
        });
        
        nextBtn.addEventListener('click', () => {
            let idx = locations.indexOf(currentLocation);
            if (idx < locations.length - 1) {
                currentLocation = locations[idx + 1];
                updateNav();
                speak('下一张，现在是' + currentLocation);
            } else {
                speak('已经是最后一张了');
            }
        });

        // ===== 初始化 =====
        renderRooms();
        showTicket('胸痛');
        updateNav();
        
        setTimeout(() => {
            speak('欢迎使用诊所导航，请先到收费室缴费');
        }, 800);
    </script>
</body>
</html>
