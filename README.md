<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>📖 متتبع قراءة القرآن</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
        }

        h1 {
            text-align: center;
            color: #1e3c72;
            margin-bottom: 10px;
            font-size: 2.5em;
        }

        .subtitle {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
        }

        .progress-section {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 30px;
            text-align: center;
        }

        .progress-bar {
            width: 100%;
            height: 30px;
            background: rgba(255,255,255,0.3);
            border-radius: 15px;
            overflow: hidden;
            margin: 15px 0;
        }

        .progress-fill {
            height: 100%;
            background: #fff;
            transition: width 0.5s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #764ba2;
            font-weight: bold;
        }

        .stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }

        .stat-box {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 10px;
            text-align: center;
            border: 2px solid #e9ecef;
        }

        .stat-number {
            font-size: 2em;
            font-weight: bold;
            color: #1e3c72;
        }

        .stat-label {
            color: #666;
            font-size: 0.9em;
        }

        .controls {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        button {
            padding: 10px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1em;
            transition: all 0.3s;
        }

        .btn-primary {
            background: #1e3c72;
            color: white;
        }

        .btn-primary:hover {
            background: #2a5298;
            transform: translateY(-2px);
        }

        .btn-danger {
            background: #dc3545;
            color: white;
        }

        .btn-danger:hover {
            background: #c82333;
        }

        .search-box {
            flex: 1;
            min-width: 200px;
            padding: 10px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 1em;
        }

        .sura-list {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .sura-item {
            display: flex;
            align-items: center;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 10px;
            border: 2px solid transparent;
            transition: all 0.3s;
        }

        .sura-item:hover {
            border-color: #1e3c72;
            transform: translateX(-5px);
        }

        .sura-item.completed {
            background: #d4edda;
            border-color: #28a745;
        }

        .sura-number {
            width: 40px;
            height: 40px;
            background: #1e3c72;
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            margin-left: 15px;
        }

        .sura-info {
            flex: 1;
        }

        .sura-name {
            font-size: 1.2em;
            font-weight: bold;
            color: #333;
        }

        .sura-english {
            font-size: 0.9em;
            color: #666;
        }

        .sura-controls {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .checkbox-wrapper {
            display: flex;
            align-items: center;
            gap: 5px;
            cursor: pointer;
        }

        .checkbox-wrapper input[type="checkbox"] {
            width: 20px;
            height: 20px;
            cursor: pointer;
        }

        .ayah-input {
            width: 80px;
            padding: 8px;
            border: 2px solid #ddd;
            border-radius: 5px;
            text-align: center;
            font-size: 1em;
        }

        .ayah-input:focus {
            border-color: #1e3c72;
            outline: none;
        }

        .save-indicator {
            position: fixed;
            top: 20px;
            left: 20px;
            background: #28a745;
            color: white;
            padding: 10px 20px;
            border-radius: 5px;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .save-indicator.show {
            opacity: 1;
        }

        @media (max-width: 600px) {
            .container {
                padding: 15px;
            }
            
            h1 {
                font-size: 1.8em;
            }
            
            .sura-item {
                flex-direction: column;
                text-align: center;
                gap: 10px;
            }
            
            .sura-number {
                margin-left: 0;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📖 متتبع قراءة القرآن الكريم</h1>
        <p class="subtitle">سجل تقدمك في قراءة السور وحفظ موضعك</p>

        <div class="progress-section">
            <h2>تقدمك في القراءة</h2>
            <div class="progress-bar">
                <div class="progress-fill" id="progressFill">0%</div>
            </div>
            <p id="progressText">0 من 114 سورة</p>
        </div>

        <div class="stats">
            <div class="stat-box">
                <div class="stat-number" id="completedCount">0</div>
                <div class="stat-label">سور مكتملة</div>
            </div>
            <div class="stat-box">
                <div class="stat-number" id="remainingCount">114</div>
                <div class="stat-label">سور متبقية</div>
            </div>
            <div class="stat-box">
                <div class="stat-number" id="currentStreak">0</div>
                <div class="stat-label">أيام متتالية</div>
            </div>
        </div>

        <div class="controls">
            <input type="text" class="search-box" id="searchBox" placeholder="🔍 ابحث عن سورة...">
            <button class="btn-primary" onclick="markAllRead()">✓ تحديد الكل مقروء</button>
            <button class="btn-danger" onclick="resetAll()">↺ إعادة ضبط</button>
        </div>

        <div class="sura-list" id="suraList"></div>
    </div>

    <div class="save-indicator" id="saveIndicator">✓ تم الحفظ تلقائياً</div>

    <script>
        const suras = [
            {number: 1, name: "الفاتحة", english: "Al-Fatiha", verses: 7},
            {number: 2, name: "البقرة", english: "Al-Baqarah", verses: 286},
            {number: 3, name: "آل عمران", english: "Aal-E-Imran", verses: 200},
            {number: 4, name: "النساء", english: "An-Nisa", verses: 176},
            {number: 5, name: "المائدة", english: "Al-Ma'idah", verses: 120},
            {number: 6, name: "الأنعام", english: "Al-An'am", verses: 165},
            {number: 7, name: "الأعراف", english: "Al-A'raf", verses: 206},
            {number: 8, name: "الأنفال", english: "Al-Anfal", verses: 75},
            {number: 9, name: "التوبة", english: "At-Tawbah", verses: 129},
            {number: 10, name: "يونس", english: "Yunus", verses: 109},
            {number: 11, name: "هود", english: "Hud", verses: 123},
            {number: 12, name: "يوسف", english: "Yusuf", verses: 111},
            {number: 13, name: "الرعد", english: "Ar-Ra'd", verses: 43},
            {number: 14, name: "إبراهيم", english: "Ibrahim", verses: 52},
            {number: 15, name: "الحجر", english: "Al-Hijr", verses: 99},
            {number: 16, name: "النحل", english: "An-Nahl", verses: 128},
            {number: 17, name: "الإسراء", english: "Al-Isra", verses: 111},
            {number: 18, name: "الكهف", english: "Al-Kahf", verses: 110},
            {number: 19, name: "مريم", english: "Maryam", verses: 98},
            {number: 20, name: "طه", english: "Ta-Ha", verses: 135},
            {number: 21, name: "الأنبياء", english: "Al-Anbiya", verses: 112},
            {number: 22, name: "الحج", english: "Al-Hajj", verses: 78},
            {number: 23, name: "المؤمنون", english: "Al-Mu'minun", verses: 118},
            {number: 24, name: "النور", english: "An-Nur", verses: 64},
            {number: 25, name: "الفرقان", english: "Al-Furqan", verses: 77},
            {number: 26, name: "الشعراء", english: "Ash-Shu'ara", verses: 227},
            {number: 27, name: "النمل", english: "An-Naml", verses: 93},
            {number: 28, name: "القصص", english: "Al-Qasas", verses: 88},
            {number: 29, name: "العنكبوت", english: "Al-Ankabut", verses: 69},
            {number: 30, name: "الروم", english: "Ar-Rum", verses: 60},
            {number: 31, name: "لقمان", english: "Luqman", verses: 34},
            {number: 32, name: "السجدة", english: "As-Sajda", verses: 30},
            {number: 33, name: "الأحزاب", english: "Al-Ahzab", verses: 73},
            {number: 34, name: "سبأ", english: "Saba", verses: 54},
            {number: 35, name: "فاطر", english: "Fatir", verses: 45},
            {number: 36, name: "يس", english: "Ya-Sin", verses: 83},
            {number: 37, name: "الصافات", english: "As-Saffat", verses: 182},
            {number: 38, name: "ص", english: "Sad", verses: 88},
            {number: 39, name: "الزمر", english: "Az-Zumar", verses: 75},
            {number: 40, name: "غافر", english: "Ghafir", verses: 85},
            {number: 41, name: "فصلت", english: "Fussilat", verses: 54},
            {number: 42, name: "الشورى", english: "Ash-Shura", verses: 53},
            {number: 43, name: "الزخرف", english: "Az-Zukhruf", verses: 89},
            {number: 44, name: "الدخان", english: "Ad-Dukhan", verses: 59},
            {number: 45, name: "الجاثية", english: "Al-Jathiya", verses: 37},
            {number: 46, name: "الأحقاف", english: "Al-Ahqaf", verses: 35},
            {number: 47, name: "محمد", english: "Muhammad", verses: 38},
            {number: 48, name: "الفتح", english: "Al-Fath", verses: 29},
            {number: 49, name: "الحجرات", english: "Al-Hujurat", verses: 18},
            {number: 50, name: "ق", english: "Qaf", verses: 45},
            {number: 51, name: "الذاريات", english: "Adh-Dhariyat", verses: 60},
            {number: 52, name: "الطور", english: "At-Tur", verses: 49},
            {number: 53, name: "النجم", english: "An-Najm", verses: 62},
            {number: 54, name: "القمر", english: "Al-Qamar", verses: 55},
            {number: 55, name: "الرحمن", english: "Ar-Rahman", verses: 78},
            {number: 56, name: "الواقعة", english: "Al-Waqi'a", verses: 96},
            {number: 57, name: "الحديد", english: "Al-Hadid", verses: 29},
            {number: 58, name: "المجادلة", english: "Al-Mujadila", verses: 22},
            {number: 59, name: "الحشر", english: "Al-Hashr", verses: 24},
            {number: 60, name: "الممتحنة", english: "Al-Mumtahanah", verses: 13},
            {number: 61, name: "الصف", english: "As-Saff", verses: 14},
            {number: 62, name: "الجمعة", english: "Al-Jumu'ah", verses: 11},
            {number: 63, name: "المنافقون", english: "Al-Munafiqun", verses: 11},
            {number: 64, name: "التغابن", english: "At-Taghabun", verses: 18},
            {number: 65, name: "الطلاق", english: "At-Talaq", verses: 12},
            {number: 66, name: "التحريم", english: "At-Tahrim", verses: 12},
            {number: 67, name: "الملك", english: "Al-Mulk", verses: 30},
            {number: 68, name: "القلم", english: "Al-Qalam", verses: 52},
            {number: 69, name: "الحاقة", english: "Al-Haqqah", verses: 52},
            {number: 70, name: "المعارج", english: "Al-Ma'arij", verses: 44},
            {number: 71, name: "نوح", english: "Nuh", verses: 28},
            {number: 72, name: "الجن", english: "Al-Jinn", verses: 28},
            {number: 73, name: "المزمل", english: "Al-Muzzammil", verses: 20},
            {number: 74, name: "المدثر", english: "Al-Muddaththir", verses: 56},
            {number: 75, name: "القيامة", english: "Al-Qiyamah", verses: 40},
            {number: 76, name: "الإنسان", english: "Al-Insan", verses: 31},
            {number: 77, name: "المرسلات", english: "Al-Mursalat", verses: 50},
            {number: 78, name: "النبأ", english: "An-Naba", verses: 40},
            {number: 79, name: "النازعات", english: "An-Nazi'at", verses: 46},
            {number: 80, name: "عبس", english: "Abasa", verses: 42},
            {number: 81, name: "التكوير", english: "At-Takwir", verses: 29},
            {number: 82, name: "الإنفطار", english: "Al-Infitar", verses: 19},
            {number: 83, name: "المطففين", english: "Al-Mutaffifin", verses: 36},
            {number: 84, name: "الإنشقاق", english: "Al-Inshiqaq", verses: 25},
            {number: 85, name: "البروج", english: "Al-Buruj", verses: 22},
            {number: 86, name: "الطارق", english: "At-Tariq", verses: 17},
            {number: 87, name: "الأعلى", english: "Al-A'la", verses: 19},
            {number: 88, name: "الغاشية", english: "Al-Ghashiyah", verses: 26},
            {number: 89, name: "الفجر", english: "Al-Fajr", verses: 30},
            {number: 90, name: "البلد", english: "Al-Balad", verses: 20},
            {number: 91, name: "الشمس", english: "Ash-Shams", verses: 15},
            {number: 92, name: "الليل", english: "Al-Layl", verses: 21},
            {number: 93, name: "الضحى", english: "Ad-Duha", verses: 11},
            {number: 94, name: "الشرح", english: "Ash-Sharh", verses: 8},
            {number: 95, name: "التين", english: "At-Tin", verses: 8},
            {number: 96, name: "العلق", english: "Al-Alaq", verses: 19},
            {number: 97, name: "القدر", english: "Al-Qadr", verses: 5},
            {number: 98, name: "البينة", english: "Al-Bayyinah", verses: 8},
            {number: 99, name: "الزلزلة", english: "Az-Zilzal", verses: 8},
            {number: 100, name: "العاديات", english: "Al-Adiyat", verses: 11},
            {number: 101, name: "القارعة", english: "Al-Qari'ah", verses: 11},
            {number: 102, name: "التكاثر", english: "At-Takathur", verses: 8},
            {number: 103, name: "العصر", english: "Al-Asr", verses: 3},
            {number: 104, name: "الهمزة", english: "Al-Humazah", verses: 9},
            {number: 105, name: "الفيل", english: "Al-Fil", verses: 5},
            {number: 106, name: "قريش", english: "Quraysh", verses: 4},
            {number: 107, name: "الماعون", english: "Al-Ma'un", verses: 7},
            {number: 108, name: "الكوثر", english: "Al-Kawthar", verses: 3},
            {number: 109, name: "الكافرون", english: "Al-Kafirun", verses: 6},
            {number: 110, name: "النصر", english: "An-Nasr", verses: 3},
            {number: 111, name: "المسد", english: "Al-Masad", verses: 5},
            {number: 112, name: "الإخلاص", english: "Al-Ikhlas", verses: 4},
            {number: 113, name: "الفلق", english: "Al-Falaq", verses: 5},
            {number: 114, name: "الناس", english: "An-Nas", verses: 6}
        ];

        let suraData = JSON.parse(localStorage.getItem('quranTracker')) || {};

        function init() {
            renderList();
            updateStats();
            
            document.getElementById('searchBox').addEventListener('input', (e) => {
                renderList(e.target.value);
            });
        }

        function renderList(searchTerm = '') {
            const list = document.getElementById('suraList');
            list.innerHTML = '';

            suras.forEach(sura => {
                if (searchTerm && !sura.name.includes(searchTerm) && !sura.english.toLowerCase().includes(searchTerm.toLowerCase())) {
                    return;
                }

                const data = suraData[sura.number] || { completed: false, currentAyah: '' };
                
                const item = document.createElement('div');
                item.className = `sura-item ${data.completed ? 'completed' : ''}`;
                
                item.innerHTML = `
                    <div class="sura-number">${sura.number}</div>
                    <div class="sura-info">
                        <div class="sura-name">${sura.name}</div>
                        <div class="sura-english">${sura.english} • ${sura.verses} آية</div>
                    </div>
                    <div class="sura-controls">
                        <label class="checkbox-wrapper">
                            <input type="checkbox" 
                                   ${data.completed ? 'checked' : ''} 
                                   onchange="toggleComplete(${sura.number})">
                            <span>تمت القراءة</span>
                        </label>
                        <div>
                            <label>الآية:</label>
                            <input type="number" 
                                   class="ayah-input" 
                                   value="${data.currentAyah}" 
                                   placeholder="1-${sura.verses}"
                                   min="1" 
                                   max="${sura.verses}"
                                   onchange="updateAyah(${sura.number}, this.value)"
                                   title="أدخل رقم الآية التي وصلت إليها">
                        </div>
                    </div>
                `;
                
                list.appendChild(item);
            });
        }

        function toggleComplete(number) {
            if (!suraData[number]) suraData[number] = {};
            suraData[number].completed = !suraData[number].completed;
            saveData();
            renderList(document.getElementById('searchBox').value);
            updateStats();
        }

        function updateAyah(number, value) {
            if (!suraData[number]) suraData[number] = {};
            suraData[number].currentAyah = value;
            saveData();
        }

        function saveData() {
            localStorage.setItem('quranTracker', JSON.stringify(suraData));
            showSaveIndicator();
        }

        function showSaveIndicator() {
            const indicator = document.getElementById('saveIndicator');
            indicator.classList.add('show');
            setTimeout(() => indicator.classList.remove('show'), 2000);
        }

        function updateStats() {
            const completed = Object.values(suraData).filter(d => d.completed).length;
            const percentage = Math.round((completed / 114) * 100);
            
            document.getElementById('completedCount').textContent = completed;
            document.getElementById('remainingCount').textContent = 114 - completed;
            document.getElementById('progressFill').style.width = percentage + '%';
            document.getElementById('progressFill').textContent = percentage + '%';
            document.getElementById('progressText').textContent = `${completed} من 114 سورة (${percentage}%)`;
            
            const today = new Date().toDateString();
            const lastRead = localStorage.getItem('lastReadDate');
            let streak = parseInt(localStorage.getItem('readStreak') || '0');
            
            if (lastRead !== today && completed > 0) {
                if (lastRead === new Date(Date.now() - 86400000).toDateString()) {
                    streak++;
                } else if (lastRead !== today) {
                    streak = 1;
                }
                localStorage.setItem('lastReadDate', today);
                localStorage.setItem('readStreak', streak);
            }
            document.getElementById('currentStreak').textContent = streak;
        }

        function markAllRead() {
            if (confirm('هل أنت متأكد من تحديد جميع السور كمقروءة؟')) {
                suras.forEach(sura => {
                    if (!suraData[sura.number]) suraData[sura.number] = {};
                    suraData[sura.number].completed = true;
                });
                saveData();
                renderList(document.getElementById('searchBox').value);
                updateStats();
            }
        }

        function resetAll() {
            if (confirm('هل أنت متأكد من إعادة ضبط جميع البيانات؟ سيتم حذف كل تقدمك!')) {
                suraData = {};
                localStorage.removeItem('quranTracker');
                localStorage.removeItem('lastReadDate');
                localStorage.removeItem('readStreak');
                renderList(document.getElementById('searchBox').value);
                updateStats();
            }
        }

        init();
    </script>
</body>
</html>
