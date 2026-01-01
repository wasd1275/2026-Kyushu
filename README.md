<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>日本之旅 Japan Trip</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* --- 1. 日式極簡 & 手機優先基礎設定 --- */
        :root {
            --bg-color: #F9F9F7; /* 和紙白 */
            --card-bg: #FFFFFF;
            --text-main: #2C2C2C; /* 墨黑 */
            --text-sub: #8E8E8E;
            --accent-blue: #3E62AD; /* 藍染 */
            --accent-red: #D64F4F; /* 朱紅 */
            --accent-green: #6A8A6D; /* 抹茶 */
            --tag-food: #FFE8E8;
            --tag-buy: #E8F4FF;
            --tag-res: #F0F7E6;
            --shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
            --radius: 16px;
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        
        body {
            font-family: "Noto Sans JP", "Helvetica Neue", sans-serif;
            background-color: #eee;
            margin: 0;
            padding: 0;
            color: var(--text-main);
            display: flex;
            justify-content: center;
            min-height: 100vh;
        }

        /* 模擬手機 App 容器 */
        #app-container {
            width: 100%;
            max-width: 480px; /* 手機寬度限制 */
            background-color: var(--bg-color);
            position: relative;
            padding-bottom: 80px; /* 預留底部導航空間 */
            min-height: 100vh;
            box-shadow: 0 0 20px rgba(0,0,0,0.1);
        }

        /* --- 2. Header & 天氣 --- */
        header {
            padding: 20px;
            background: var(--card-bg);
            position: sticky;
            top: 0;
            z-index: 100;
            border-bottom: 1px solid rgba(0,0,0,0.05);
        }
        
        h1 { margin: 0; font-size: 20px; font-weight: 700; letter-spacing: 1px; }
        .sub-title { font-size: 12px; color: var(--text-sub); margin-top: 4px; }

        /* --- 3. 行程卡片設計 --- */
        .day-header {
            padding: 24px 20px 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .date-badge {
            background: var(--text-main);
            color: #fff;
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: bold;
        }

        .weather-widget {
            font-size: 12px;
            color: var(--text-sub);
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .card {
            background: var(--card-bg);
            margin: 12px 20px;
            padding: 16px;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            position: relative;
            border-left: 4px solid transparent;
            transition: transform 0.2s;
        }

        .card:active { transform: scale(0.98); }

        /* 卡片分類顏色 */
        .type-spot { border-left-color: var(--accent-blue); }
        .type-food { border-left-color: var(--accent-red); }
        .type-transport { border-left-color: var(--text-sub); background: #F2F2F2; }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            margin-bottom: 8px;
        }

        .card-time { font-size: 13px; font-weight: 700; color: var(--accent-blue); }
        .card-title { font-size: 16px; font-weight: 700; margin: 4px 0; }
        
        .map-btn {
            background: none;
            border: 1px solid #ddd;
            border-radius: 50%;
            width: 32px;
            height: 32px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--accent-blue);
            cursor: pointer;
        }

        /* 導遊攻略分析樣式 */
        .guide-text { font-size: 14px; line-height: 1.6; color: #555; }
        
        .tag {
            display: inline-block;
            padding: 2px 8px;
            border-radius: 4px;
            font-size: 11px;
            margin: 2px 0;
            font-weight: bold;
        }
        .tag.food { background: var(--tag-food); color: #D64F4F; }
        .tag.buy { background: var(--tag-buy); color: #3E62AD; }
        .tag.res { background: var(--tag-res); color: #5B7F2D; border: 1px dashed #5B7F2D; }

        /* --- 4. 工具頁面 (Flight/Budget) --- */
        .tools-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
            padding: 20px;
        }
        
        .tool-card {
            background: var(--card-bg);
            padding: 20px;
            border-radius: var(--radius);
            text-align: center;
            box-shadow: var(--shadow);
            cursor: pointer;
        }
        
        .tool-icon { font-size: 24px; margin-bottom: 8px; color: var(--accent-blue); }
        .tool-name { font-size: 14px; font-weight: bold; }

        /* 預算條 */
        .budget-bar-container { padding: 0 20px; margin-top: 10px; }
        .budget-track { height: 8px; background: #eee; border-radius: 4px; overflow: hidden; }
        .budget-fill { height: 100%; background: var(--accent-green); width: 65%; }
        .budget-text { display: flex; justify-content: space-between; font-size: 12px; margin-top: 4px; color: var(--text-sub); }

        /* --- 5. 底部導航欄 --- */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            width: 100%;
            max-width: 480px;
            background: rgba(255,255,255,0.95);
            backdrop-filter: blur(10px);
            border-top: 1px solid #eee;
            display: flex;
            justify-content: space-around;
            padding: 12px 0;
            z-index: 200;
        }

        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            font-size: 10px;
            color: #ccc;
            cursor: pointer;
        }

        .nav-item.active { color: var(--text-main); }
        .nav-item i { font-size: 20px; margin-bottom: 4px; }

        /* 頁面切換控制 */
        .page { display: none; }
        .page.active { display: block; }

    </style>
</head>
<body>

<div id="app-container">
    
    <div id="page-itinerary" class="page active">
        <header>
            <h1>東京・五天四夜</h1>
            <div class="sub-title">2024.04.01 - 2024.04.05</div>
        </header>

        <div id="itinerary-content"></div>
    </div>

    <div id="page-tools" class="page">
        <header>
            <h1>旅行工具箱</h1>
            <div class="sub-title">Tools & Info</div>
        </header>

        <div class="tools-grid">
            <div class="tool-card">
                <div class="tool-icon"><i class="fas fa-plane"></i></div>
                <div class="tool-name">航班資訊</div>
                <div style="font-size:10px; color:#999; margin-top:4px;">JL098 / JL099</div>
            </div>
            <div class="tool-card">
                <div class="tool-icon"><i class="fas fa-bed"></i></div>
                <div class="tool-name">住宿資訊</div>
                <div style="font-size:10px; color:#999; margin-top:4px;">淺草里士滿飯店</div>
            </div>
            <div class="tool-card" style="color: var(--accent-red);">
                <div class="tool-icon"><i class="fas fa-phone-alt"></i></div>
                <div class="tool-name">緊急聯絡</div>
                <div style="font-size:10px; margin-top:4px;">119 / 110</div>
            </div>
            <div class="tool-card">
                <div class="tool-icon"><i class="fas fa-calculator"></i></div>
                <div class="tool-name">記帳本</div>
            </div>
        </div>

        <div class="budget-bar-container">
            <h3 style="font-size:16px; margin-bottom:8px;">總預算控制</h3>
            <div class="budget-track">
                <div class="budget-fill"></div>
            </div>
            <div class="budget-text">
                <span>已支出 ¥65,000</span>
                <span>剩餘 ¥35,000</span>
            </div>
        </div>
    </div>

    <nav class="bottom-nav">
        <div class="nav-item active" onclick="switchTab('itinerary', this)">
            <i class="fas fa-map-marked-alt"></i>
            <span>行程</span>
        </div>
        <div class="nav-item" onclick="switchTab('tools', this)">
            <i class="fas fa-suitcase"></i>
            <span>工具</span>
        </div>
    </nav>
</div>

<script>
    // --- 資料區：您只需要修改這裡的 JSON 資料 ---
    const tripData = [
        {
            date: "Day 1 (4/1)",
            weather: "晴時多雲 18°C",
            icon: "fa-sun",
            events: [
                {
                    time: "10:00",
                    type: "transport", // spot, food, transport
                    title: "抵達東京成田機場",
                    desc: "搭乘 Skyliner 前往上野，記得先在網路買票，QR Code 預約代號：#SKY-2024-JP"
                },
                {
                    time: "12:30",
                    type: "food",
                    title: "阿美橫丁吃午餐",
                    desc: "必吃美食：鐵火丼與章魚燒。推薦店家：湊川水產。"
                },
                {
                    time: "15:00",
                    type: "spot",
                    title: "淺草寺雷門",
                    desc: "必買伴手禮：人形燒。記得要在雷門大燈籠下拍照。求籤如果是凶，記得綁在架子上。"
                }
            ]
        },
        {
            date: "Day 2 (4/2)",
            weather: "陰天 15°C",
            icon: "fa-cloud",
            events: [
                {
                    time: "09:00",
                    type: "spot",
                    title: "明治神宮",
                    desc: "散步感受芬多精，注意：鳥居進出要鞠躬。"
                },
                {
                    time: "12:00",
                    type: "food",
                    title: "AFURI 拉麵 原宿店",
                    desc: "必點菜單：柚子鹽拉麵。用餐時間可能排隊30分鐘。"
                },
                {
                    time: "14:00",
                    type: "spot",
                    title: "Shibuya Sky",
                    desc: "重要預約代號：#SHI-889900，請提早10分鐘入場。必拍角度：角落的手扶梯。"
                }
            ]
        }
    ];

    // --- 核心功能邏輯 ---

    // 1. 自動分析導遊關鍵字並上色
    function analyzeText(text) {
        // 定義關鍵字規則 (Regex)
        let processed = text;
        
        // 必吃/必點 -> 紅色標籤
        processed = processed.replace(/(必吃美食|必點菜單)[：:]?(\s*\S+)/g, '<span class="tag food">$1$2</span>');
        
        // 必買 -> 藍色標籤
        processed = processed.replace(/(必買伴手禮)[：:]?(\s*\S+)/g, '<span class="tag buy">$1$2</span>');
        
        // 預約代號 -> 綠色虛線框
        processed = processed.replace(/(預約代號|訂位編號)[：:]?(\s*[A-Za-z0-9-#]+)/g, '<span class="tag res">$1$2</span>');
        
        return processed;
    }

    // 2. 渲染行程
    function renderItinerary() {
        const container = document.getElementById('itinerary-content');
        let html = '';

        tripData.forEach(day => {
            html += `
                <div class="day-header">
                    <div class="date-badge">${day.date}</div>
                    <div class="weather-widget">
                        <i class="fas ${day.icon}"></i> ${day.weather}
                    </div>
                </div>
            `;

            day.events.forEach(event => {
                // 自動生成 Google Map 連結
                const mapLink = `https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(event.title + " 日本")}`;
                
                // 分析文字
                const highlightDesc = analyzeText(event.desc);

                html += `
                    <div class="card type-${event.type}">
                        <div class="card-header">
                            <div>
                                <div class="card-time">${event.time}</div>
                                <div class="card-title">${event.title}</div>
                            </div>
                            <button class="map-btn" onclick="window.open('${mapLink}', '_blank')">
                                <i class="fas fa-map-marker-alt"></i>
                            </button>
                        </div>
                        <div class="guide-text">${highlightDesc}</div>
                    </div>
                `;
            });
        });

        container.innerHTML = html;
    }

    // 3. Tab 切換功能
    function switchTab(pageId, navElement) {
        // 隱藏所有頁面
        document.querySelectorAll('.page').forEach(el => el.classList.remove('active'));
        // 顯示目標頁面
        document.getElementById('page-' + pageId).classList.add('active');
        
        // 更新 Nav 樣式
        document.querySelectorAll('.nav-item').forEach(el => el.classList.remove('active'));
        navElement.classList.add('active');
    }

    // 初始化
    renderItinerary();

</script>

</body>
</html>
