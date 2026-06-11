<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cybersecurity & Steganography Quiz Center</title>
    <style>
        /* Matrix & Terminal Asosiy Stillari */
        :root {
            --matrix-green: #00ff41;
            --dark-bg: #0d0d0d;
            --card-bg: rgba(16, 16, 16, 0.9);
            --neon-glow: 0 0 10px rgba(0, 255, 65, 0.5), 0 0 20px rgba(0, 255, 65, 0.3);
            --error-red: #ff3333;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Courier New', Courier, monospace;
        }

        body {
            background-color: var(--dark-bg);
            color: #ffffff;
            overflow-x: hidden;
            padding-bottom: 50px;
        }

        /* Matrix Rain Canvas */
        #matrix-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            z-index: -1;
            opacity: 0.15;
        }

        /* Kiber Panel (Header) */
        .cyber-header {
            background: rgba(0, 0, 0, 0.85);
            border-bottom: 2px solid var(--matrix-green);
            padding: 20px;
            text-align: center;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: var(--neon-glow);
        }

        .cyber-title {
            color: var(--matrix-green);
            font-size: 1.8rem;
            text-shadow: var(--neon-glow);
            margin-bottom: 10px;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        /* Dashboard (Statistika, Taymer, Jonlar) */
        .cyber-dashboard {
            display: flex;
            justify-content: space-around;
            align-items: center;
            flex-wrap: wrap;
            max-width: 1000px;
            margin: 0 auto;
            background: rgba(20, 20, 20, 0.8);
            border: 1px solid var(--matrix-green);
            padding: 10px;
            border-radius: 5px;
        }

        .stat-box {
            font-size: 1.1rem;
            color: #ffffff;
            margin: 5px 15px;
        }

        .stat-box span {
            color: var(--matrix-green);
            font-weight: bold;
            text-shadow: var(--neon-glow);
        }

        #timer.warning {
            color: var(--error-red);
            animation: blink 1s infinite;
        }

        #firewall-hp {
            color: var(--error-red);
            font-size: 1.3rem;
        }

        /* Quiz Konteyneri */
        .quiz-container {
            max-width: 800px;
            margin: 30px auto;
            padding: 0 20px;
        }

        /* Savol Kartasi */
        .question-card {
            background: var(--card-bg);
            border: 1px solid rgba(0, 255, 65, 0.3);
            border-radius: 8px;
            padding: 25px;
            margin-bottom: 25px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
            transition: all 0.3s ease;
            display: none; /* Savollar bosqichma-bosqich ochiladi */
        }

        .question-card.active {
            display: block;
        }

        .question-text {
            font-size: 1.2rem;
            color: var(--matrix-green);
            margin-bottom: 20px;
            border-left: 3px solid var(--matrix-green);
            padding-left: 10px;
            line-height: 1.5;
        }

        /* Variantlar */
        .options-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .option-item {
            background: rgba(30, 30, 30, 0.6);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 5px;
            padding: 15px;
            cursor: pointer;
            transition: all 0.2s ease;
            font-size: 1rem;
            line-height: 1.4;
        }

        .option-item:hover {
            background: rgba(0, 255, 65, 0.1);
            border-color: var(--matrix-green);
            padding-left: 20px;
        }

        /* To'g'ri va Xato holatlar */
        .option-item.correct {
            background: rgba(0, 255, 65, 0.2) !important;
            border-color: var(--matrix-green) !important;
            color: var(--matrix-green);
            font-weight: bold;
            box-shadow: inset 0 0 10px rgba(0,255,65,0.2);
        }

        .option-item.wrong {
            background: rgba(255, 51, 51, 0.2) !important;
            border-color: var(--error-red) !important;
            color: var(--error-red);
            text-decoration: line-through;
        }

        .option-item.disabled {
            pointer-events: none;
            opacity: 0.6;
        }

        /* Terminal Natija Oynasi */
        .terminal-result {
            background: #000000;
            border: 2px solid var(--matrix-green);
            border-radius: 8px;
            padding: 30px;
            text-align: center;
            box-shadow: var(--neon-glow);
            display: none;
            margin-top: 40px;
        }

        .terminal-result h2 {
            color: var(--matrix-green);
            margin-bottom: 20px;
            text-transform: uppercase;
            letter-spacing: 3px;
        }

        .rank-badge {
            font-size: 1.6rem;
            font-weight: bold;
            margin: 20px 0;
            padding: 10px;
            display: inline-block;
            border: 1px dashed var(--matrix-green);
            color: var(--matrix-green);
            text-shadow: var(--neon-glow);
        }

        .restart-btn {
            background: transparent;
            color: var(--matrix-green);
            border: 1px solid var(--matrix-green);
            padding: 12px 30px;
            font-size: 1.1rem;
            cursor: pointer;
            text-transform: uppercase;
            font-family: inherit;
            margin-top: 20px;
            transition: all 0.3s;
        }

        .restart-btn:hover {
            background: var(--matrix-green);
            color: #000000;
            box-shadow: var(--neon-glow);
        }

        /* Animatsiyalar */
        @keyframes blink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.3; }
        }
    </style>
</head>
<body>

    <canvas id="matrix-canvas"></canvas>

    <header class="cyber-header">
        <div class="cyber-title">> CYBERSECURITY QUIZ CENTER_</div>
        <div class="cyber-dashboard">
            <div class="stat-box">PROGRESS: <span id="current-idx">1</span>/<span id="total-count">71</span></div>
            <div class="stat-box">ACCURACY: <span id="accuracy">100%</span></div>
            <div class="stat-box">FIREWALL HP: <span id="firewall-hp">█████</span></div>
            <div class="stat-box">SYSTEM TIMER: <span id="timer">60s</span></div>
        </div>
    </header>

    <main class="quiz-container">
        <div id="quiz-wrapper">
            
            <div class="question-card active" data-correct-index="0">
                <div class="question-text">1. Biometrik tizimlar nima uchun xizmat qiladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Insonning o'ziga xos tana a'zolari belgilari (biometrik ma'lumotlar) asosida uni aniqlash va tekshirish (identifikatsiya va verifikatsiya) uchun xizmat qiladi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Tarmoq trafigini tahlil qilish uchun</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Shifrlangan fayllarni ochish uchun</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Ma'lumotlarni zaxiralash uchun</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">2. Barmoq izidagi papillyar chiziqlar necha xil asosiy turga bo'linadi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) 2 xil</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) 3 xil (yoylar, ilmoqlar, buramalar)</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) 4 xil</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) 5 xil</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">3. Ko‘z to‘r pardasi (retina) biometrikasi nimaga asoslangan?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Ko'z qorachig'ining o'lchamiga</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Ko'z yosh kanallarining tuzilishiga</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Ko'z to'r pardasining orqa qismidagi qon tomirlarining o'ziga xos va betakror tuzilishiga (rasmiga)</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Ko'z rangiga</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">4. “Liveness detection” texnologiyasining asosiy vazifasi nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Tizimga taqdim etilayotgan biometrik belgi (yuz, barmoq) haqiqiy tirik insonga tegishli ekanligini (soxta foto yoki datchik emasligini) aniqlash</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Foydalanuvchining yoshini aniqlash</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Ovoz balandligini o'lchash</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Yurak urishini kuzatish</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">5. Iris biometrikasi ko‘zning qaysi qismini tahlil qiladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Ko'z to'r pardasini</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Ko'z shishasimon tanasini</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Ko'z muskullarini</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Ko'zning rangdor pardasini (iris)</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">6. Biometrik steganografiya nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Biometrik tizimlarni buzib kirish</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Maxfiy ma'lumotlarni biometrik ma'lumotlar (masalan, yuz tasviri, barmoq izi rasmi yoki ovoz fayli) ichiga sezilarsiz yashirish</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Shifrlangan parollarni saqlash</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Tarmoq paketlarini filtrlash</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">7. Biometrik ma'lumotni yashirishdan oldin xavfsizlik uchun nima qilinadi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Fayl hajmi kattalashtiriladi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Sarlavhasi o'zgartiriladi</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Qo'shimcha xavfsizlik qatlami yaratish uchun u kriptografik usullar bilan shifrlanadi (yoki siqiladi)</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Ochiq matnga o'tkaziladi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">8. Ovoz biometrikasida qaysi xususiyatlar tahlil qilinadi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Ovozning chastotasi, tembri, balandligi va nutqning dinamik xususiyatlari</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Faqat aytilgan so'zlar ro'yxati</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Ovoz faylining formati va hajmi</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Mikrofonning rusumi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">9. Barmoq izidagi “minutiyalar” nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Izning umumiy rang o'lchami</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Papillyar chiziqlarning uzilish, ayrilish yoki tugash kabi lokal o'ziga xos nuqtalari</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Sknerlash tezligi soniyalarda</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Chiziqlarning umumiy soni</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">10. Yuzni aniqlash tizimi (FRS) jarayonining birinchi bosqichi qaysi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Cb/Cr rang tahlili</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Yuz xususiyatlarini solishtirish</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Shablon yaratish</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Tasvirdan yuz qismini aniqlash (Face Detection)</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">11. Biometrik \"shablon\" (template) nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Foydalanuvchining pasport nusxasi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Oddiy rasm ko'rinishidagi fayl</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Biometrik belgidan ajratib olingan va xotirada samarali saqlash hamda solishtirish uchun mo'ljallangan matematik yoki raqamli model (kod)</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Dasturning grafik interfeysi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">12. Qo'l kafti biometrikasida nima o'rganiladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Kaft chiziqlari, terining tuzilishi, geometrik shakli va teri osti qon tomirlari (venalar) xaritasi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Faqat barmoqlarning uzunligi</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Tirnoqlarning shakli</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Qo'l harorati</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">13. Echo hiding usulida nima ishlatiladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Audio signal chastotasini pasaytirish</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Asl signalga inson qulog'i sezmaydigan darajada juda qisqa kechikishli aks-sado (echo) qo'shish orqali ma'lumot yashirish</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Audio sarlavhasini o'chirish</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Shovqin generatorlari</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">14. DWT (Discrete Wavelet Transform) usulining o'ziga xosligi nimada?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Ma'lumotni faqat vaqt sohasida yashirishi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Audio fayl formatini o'zgartirishi</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Signalni to'liq o'chirib tashlashi</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Signalni ham vaqt, ham chastota sohasida chastota quyi-yuqori komponentlariga ajratib, muhim bo'lmagan qismlarga ma'lumot joylashtirishi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">15. Metadata orqali ma’lumot yashirishda qaysi elementlardan foydalaniladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Faylning strukturaviy teglari (masalan, EXIF, ID3) yoki sarlavha qismidagi bo'sh (zahira) baytlardan</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Signal piksellarining o'zidan</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Audio amplituda qiymatidan</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Tarmoq IP manzillaridan</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">16. Ovoz signali nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Kompyuter xotirasidagi ixtiyoriy baytlar to'plami</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Vaqt bo'yicha uzluksiz o'zgarib turadigan akustik to'lqinlarning fizik va matematik ifodasi</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Faqatgina diskret qiymatli raqamlar</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Tasvirlarning o'zgarish chastotasi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">17. Analog signalning asosiy xususiyati nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Faqat nollar va birlardan tashkil topishi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Ma'lum bir vaqt oralig'ida uzilib-uzilib kelishi</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) U vaqt va amplituda bo'yicha uzluksiz (continuous) qiymatlarga ega bo'ladi</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Chekli va aniq sonli qiymatlarga ega bo'lishi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">18. Diskretizatsiya (sampling) nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Uzluksiz analog signalni ma'lum vaqt intervallarida o'lchash orqali diskret (raqamli) shaklga o'tkazish jarayoni</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Audio faylni internet orqali uzatish</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Signal spektrini toraytirish</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Audio xabarni matnga aylantirish</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">19. Audio signalda 44.1 kHz chastota nimani bildiradi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Signal hajmi 44.1 MB ekanligini</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Signal kuchi 44 desibel ekanligini</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Sekundiga 44 bit uzatilishini</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Bir sekund davomida analog signaldan 44100 marta namuna (sample) olinganligini</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">20. WAV formati audiolarni qanday holda saqlaydi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Kuchli siqilgan va sifati yo'qotilgan holatda</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Siqilmagan, xom PCM (Pulse Code Modulation) raqamli audio oqimi holatida</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Faqat matn ko'rinishidagi teglar holatida</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Shifrlangan arxiv fayli ichida</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">21. MP3 formatining asosiy afzalligi nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Ma'lumotni mutlaqo yo'qotishsiz saqlashi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Har qanday operatsion tizimda ochilishi</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Psixoakustik model yordamida inson qulog'i sezmaydigan chastotalarni olib tashlab, fayl hajmini keskin siqishi (lossy compression)</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Avtomatik shifrlash tizimining borligi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">22. Audio steganografiya nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Maxfiy ma'lumotlarni raqamli audio signallar (WAV, MP3) ichiga sezilarsiz tarzda yashirish metodologiyasi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Nutq signallarini tahrirlovchi dastur</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Musiqalar sifatini oshirish texnikasi</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Radioto'lqinlar xavfsizligi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">23. Audio LSB usulida nima o‘zgartiriladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Audio namunalarning eng katta baytlari</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Audio namunalarning (samples) eng kichik ahamiyatga ega bo'lgan oxirgi bitlari (Least Significant Bits)</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Ovozning balandlik koeffitsiyenti</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Fayl formati va sarlavhasi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">24. Audio LSB usulining eng katta kamchiligi nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Algoritmning sekin ishlashi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Fayl hajmini juda oshirib yuborishi</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Dasturiy ta'minotining yo'qligi</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Signalga qo'shiladigan arzimas shovqinlarga, qayta namunalashga (resampling) va siqish (MP3) jarayonlariga nisbatan juda zaifligi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">25. Fazani kodlash (Phase coding) usuli nimaga asoslanadi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Audio signalning faza xususiyatlaridagi mutloq o'zgarishlarni inson qulog'i yaxshi sezmasligidan foydalanib, maxfiy xabarni faza burchaklariga joylashtirishga</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Amplituda qiymatlarini ko'paytirishga</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Signal spektrini o'chirishga</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Ovoz tezligini o'zgartirishga</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">26. Tarmoq steganografiyasi nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Routerni parollash usuli</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Kompyuter tarmoqlarini viruslardan himoyalash</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Maxfiy xabarlarni tarmoq protokollari (TCP, IP, ICMP) paketlarining sarlavha maydonlari, vaqt intervallari yoki xizmat baytlari ichiga yashirib uzatish</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Wi-Fi signalini shifrlash</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">27. IP paket sarlavhasidagi qaysi maydonda ma'lumot yashirish mumkin?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) TTL maydonida</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Identification (Identifikator), Flags yoki Fragment Offset maydonlaridagi foydalanilmayotgan yoki zahira bitlarida</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Source IP manzilida</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Protocol maydonida</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">28. TCP paket sarlavhasidagi qaysi soha ma'lumot yashirish uchun ishlatiladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Boshlang'ich ketma-ketlik raqami (ISN - Initial Sequence Number) yoki Reserved (Zahira) maydonlari</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Source Port maydoni</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Checksum sohasi</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Window Size maydoni</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">29. Vaqtga asoslangan tarmoq steganografiyasi (Timing-based) qanday ishlaydi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Paketlar hajmini o'zgartirish orqali</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Paketlarni shifrlash orqali</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Paket sarlavhalarini o'chirish orqali</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Paketlarni jo'natish orasidagi vaqt intervallarini manipulyatsiya qilish orqali (masalan, qisqa interval "0", uzun interval "1")</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">30. DNS steganografiyasi qanday amalga oshiriladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) IP manzillarni bloklash orqali</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) DNS serverni o'chirib qo'yish orqali</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) DNS so'rovlari yoki javoblarining TXT, CNAME kabi rekord maydonlari yoki so'rov matni ichiga ma'lumotlarni kodlab joylashtirish orqali</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Shifrlangan protokollarni cheklash orqali</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">31. ICMP steganografiyasida ma'lumot qayerga joylashtiriladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) ICMP Echo Request/Reply paketlarining Data (ma'lumot) maydoniga (ixtiyoriy ma'lumot yozish mumkin bo'lgan qismga)</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Paket tipini belgilovchi baytga</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Tarmoq maskasiga</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Router manzili maydoniga</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">32. Stegoanaliz nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Ma'lumot yashirish algoritmi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Berilgan ob'ektlar (konteynerlar) ichida yashirin ma'lumot bor yoki yo'qligini aniqlash, uni tahlil qilish va kerak bo'lsa ochib olish (fosh qilish) fani va jarayoni</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Ma'lumotlarni shifrlash texnikasi</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Tarmoqni monitoring qilish tizimi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">33. Vizual stegoanaliz usuli nimaga asoslangan?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Faqat matematik hisob-kitoblarga</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Audio fayllarni eshitishga</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Tasvir fayli hajmini taqqoslashga</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Tasvirning bitli tekisliklarini ajratish orqali uning tuzilishidagi g'ayritabiiy vizual o'zgarishlar yoki artefaktlarni ko'z bilan qidirishga</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">34. Statistik stegoanaliz nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Tasvir piksellarini vizual solishtirish</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Ma'lumot oqimi tezligini aniqlash</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Matematik va statistik usullar (masalan, gistogramma tahlili, Xi-kvadrat testi) yordamida konteyner statistik xususiyatlarining (entropiya, chastota) o'zgarishini aniqlash</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Arxiv sarlavhalarini tekshirish</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">35. Gistogramma tahlili (Histogram analysis) stegoanalizda qanday qo'llaniladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) LSB usulida ma'lumot yashirilganda piksellar chastotasi gistogrammasidagi "juftliklar" tenglashib qolish (PoVs) qonuniyatini qidirish orqali</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Tasvir ranglarini yorqinroq qilish uchun</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Fayl sarlavhasidagi o'zgarishlarni tekshirish uchun</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Tarmoqdagi paketlar sonini chizish uchun</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">36. Rasm piksellarining rangi odatda qaysi modelda ifodalanadi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) CMYK</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) RGB (Red, Green, Blue)</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) HSV</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) YCbCr</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">37. 24-bitli rangli tasvirda bitta piksel xotiradan qancha joy egallaydi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) 1 bayt</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) 2 bayt</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) 16 bit</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) 3 bayt (har bir rang komponenti: R, G, B uchun 8 bitdan - jami 24 bit)</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">38. BMP formati tasvirlarni qanday saqlaydi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Siqilmagan, har bir piksel rang qiymatini asl holicha saqlaydi (Bitmap)</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Kuchli siqilgan va yo'qotishlar bilan</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Faqatgina vektorli chiziqlar shaklida</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Shifrlangan ma'lumotlar blokida</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">39. JPEG formatida tasvirni siqishda qaysi matematik o‘zgartirish qo‘llaniladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Furye o'zgartirishi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) LSB algoritmi</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) DCT (Discrete Cosine Transform - Diskret Kosinus O'zgartirishi)</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Xesh funksiya</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">40. PNG formatining JPEG dan asosiy farqi nimada?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Ranglar soni kamligida</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) PNG ma'lumotlarni yo'qotishsiz siqadi (Lossless) va shaffoflikni (Alpha channel) qo'llab-quvvatlaydi</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Faqat Linux tizimida ishlashida</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Hajmi har doim kichik bo'lishida</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">41. Rasm LSB usulida bitta pikselning oxirgi biti o‘zgarganda rang qanchaga o‘zgaradi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Rang qiymati maksimal 1 birlikka o'zgaradi (bu inson ko'zi uchun mutlaqo sezilarsizdir)</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Piksel qora rangga aylanadi</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Rasm sifati butkul buziladi</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Piksel shaffof bo'lib qoladi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">42. Rasm steganografiyasida "Konteyner" (Cover-object) nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Yashirin xabarning o'zi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Dastur o'rnatiladigan papka</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Shifrlash kaliti</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Ichiga maxfiy xabar yashiriladigan dastlabki begunoh tasvir (fayl)</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">43. Stego-ob'ekt (Stego-object) nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Anti-virus dasturi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Ichiga maxfiy ma'lumot joylashtirib bo'lingan (yashirilgan) yakuniy tasvir yoki fayl</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Shifrlangan matn oqimi</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Xabar uzatuvchi kanal</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">44. Steganografik kalit (Stego-key) nima uchun kerak?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Kompyuter parolini tiklash uchun</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Tasvir o'lchamini o'zgartirish uchun</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Maxfiy xabarni konteyner piksellariga joylashtirish tartibini (yoki tasodifiy joylashuvini) aniqlash va himoya qilish uchun</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Fayl formatini o'zgartirish uchun</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">45. Kriptografiya va Steganografiyaning asosiy farqi nimada?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Kriptografiya xabarning mazmunini tushunarsiz qilib shifrlaydi, Steganografiya esa xabarning borligi (fakti)ning o'zini yashiradi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Kriptografiya faqat Linuxda ishlaydi</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Steganografiya xabarni shifrlaydi</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Hech qanday farqi yo'q</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">46. "Kerckhoffs tamoyili" steganografiyaga qanday tatbiq etiladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Yashirish dasturi mutlaqo maxfiy bo'lishi kerak</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Faqat ochiq kodli rasmlardan foydalanish shart</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Tizim mutlaqo buzilmas bo'lishi shart</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Tizimning xavfsizligi algoritmning maxfiyligiga emas, balki steganografik kalitning maxfiyligiga asoslanishi kerak</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">47. "Steganografik sig'im" (Capacity) nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Faylning qattiq diskdagi umumiy hajmi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Konteyner sifatini sezilarli darajada buzmagan holda uning ichiga yashirish mumkin bo'lgan maxfiy ma'lumotning maksimal miqdori (bayt yoki bitlarda)</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Dasturning ishlash tezligi</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Tarmoq o'tkazuvchanligi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">48. Robastlik (Robustness) steganografiyada nimani anglatadi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Algoritmning juda tez ishlashini</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Fayl formatining ochiqligini</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Stego-tasvirga tashqi ta'sirlar (siqish, kesish, shovqin berish) bo'lganda ham ichidagi yashirin xabarning saqlanib qolish (chidamlilik) qobiliyati</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Kalitning uzunligini</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">49. Kontentning haqiqiyligini tekshirish va mualliflik huquqini himoya qilish uchun steganografiyaning qaysi turidan foydalaniladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Raqamli suv belgilari (Watermarking) texnologiyasidan</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Tarmoq steganografiyasidan</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) LSB algoritmidan</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Sarlavhalarni shifrlashdan</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">50. Matnli steganografiya nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Matnli fayllarni parollash</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Xabarlarni Telegram orqali yuborish</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Maxfiy xabarlarni oddiy matnli hujjatlar ichiga (bo'shliqlar, ko'rinmas belgilar, shrift o'zgarishlari yordamida) bildirmasdan yashirish</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) PDF formatini o'zgartirish</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">51. Raqamli suv belgilarining (Watermark) oddiy steganografiyadan farqi nimada?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Ularning hajmi har doim katta bo'lishida</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Suv belgisining maqsadi ob'ektning o'zini (muallifini) himoya qilishdir va u har qanday hujum va siqishlarga o'ta chidamli (robast) bo'lishi shart</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Suv belgisi har doim ko'rinib turishi shart</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Hech qanday farqi yo'q</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">52. Steganografik tizimda "Yashirin xabar" (Secret message) nima bo'lishi mumkin?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Faqat matnli ma'lumot</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Faqat parollar</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Faqat IP manzillar</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Ixtiyoriy raqamli ma'lumot: matn, fayl, kalitlar, boshqa rasm yoki audio oqim</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">53. Stegokanal (Stego-channel) nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Stego-ob'ektlar uzatiladigan mantiqiy yoki jismoniy aloqa kanali</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) YouTube dagi xakerlik kanali</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Shifrlangan router porti</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Maxfiy xabarlar ombori</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">54. Quyidagilardan qaysi biri steganografik vosita (software) hisoblanadi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Wireshark</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Nmap</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Steghide</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) John the Ripper</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">55. Inson idrok qilish tizimining (HVS - Human Visual System) cheklovlaridan steganografiyada qanday foydalaniladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Rasm hajmini kichraytirish uchun</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Inson ko'zi ranglarning juda mayda (arzimas) o'zgarishlarini farqlay olmasligidan foydalanib, piksellarning eng kichik bitlariga ma'lumot yashiriladi</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Monitor yorqinligini oshirish uchun</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Tasvirlarni qora-oq rangga o'tkazish uchun</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">56. Steganografik tizimlar o‘zaro ta’sirining etalon modeli nechta darajada ko‘riladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) 3 ta</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) 5 ta</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) 6 ta</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) 7 ta</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">57. Konteyner tashkil etish usuliga ko'ra steganografiya metodolari quyidagilarga bo'linadi:</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Generatsiyalovchi, modifikatsiyalovchi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Statik, dinamik</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Shifrlangan, ochiq</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Simmetrik, asimmetrik</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">58. Steganografik tizimlarning xavfsizlik tahdidlarining nechta asosiy xususiyati mavjud?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) 2 ta</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) 3 ta (Yashirinlik, Robastlik, Sig'im)</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) 4 ta</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) 5 ta</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">59. Ma’lumotlarga kirish usuliga ko'ra steganografiya metodolari quyidagilarga bo'linadi:</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Faqat global</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Tashqi va ichki</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) To'g'ridan-to'g'ri (Direct) va bilvosta (Indirect) kirish usullari</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Himoyalangan va himoyalanmagan</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">60. HTTP so‘rov-paket strukturasi to‘rt qismga bo‘lingan?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) So'rov satri (Request line), Sarlavhalar (Headers), Bo'sh satr (Blank line), Paket tanasi (Body)</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) IP manzil, MAC manzil, Port, Ma'lumot</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Method, URL, Version, Cookie</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) GET, POST, PUT, DELETE</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">61. Kontentning haqiqiyligini tekshirish uchun kontentga belgi qo‘yish nima deb yuritiladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Kriptografik xesh</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Raqamli imzo</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Fayl identifikatori</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Raqamli suv belgisi (Digital Watermarking) qo'yish</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">62. Steganografik tizimda yashiringan xabarni aniqlash yoki ochishga urinish jarayoni nima deb ataladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Kriptotahlil</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Stegoanaliz (Stegoanalysis)</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Reverse Engineering</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Dekodlash</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">63. Qadimgi xitoyliklar steganografiyaning qaysi turini keng ishlatgan?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Qog'oz orqasiga yozish</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Sochni qirqib boshga yozish</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Ipak matoga yashirin yozib, uni mum bilan qoplab (sharcha holatida) yutish yoki yashirish usulini</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Ko'rinmas siyohlarni</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">64. Qadimgi Xitoy imperiyasida xabarlarni ipakda yashirish kimlar orqali amalga oshirilgan?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Maxfiy va ishonchli choparlar (kuryerlar) orqali</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Savdogarlar guruhi orqali</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Harbiylar orqali</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Kabutarlar orqali</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">65. Ko‘rinmas siyohlar (Sympathetic inks) tarixda ilk bor qachon ishlatilgan?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) 20-asrda</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Qadimgi Rim va Gretsiya davrlarida (sut, o'simlik sharbatlari yordamida)</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) O'rta asrlarda</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Birinchi jahon urushida</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">66. Qadimgi yunon tarixchisi Gerodot o‘z asarlarida steganografiyaning qaysi usulini tasvirlagan?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Ko'rinmas qog'ozlar</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Toshlarga o'yib yozish</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Ipak iplarga tugun tugish</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Qulning sochini qirqib, bosh terisiga xabarni yozish va soch o'sgandan keyin jo'natish usulini</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="2">
                <div class="question-text">67. Kriptografiya so‘zining ma’nosi nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Yashirin yozuv</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Qadimiy yozuv</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Maxfiy yozuv (Shifrlash)</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Tasvirlar yordamida yozish</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">68. Steganografiya so‘zining etimologik ma’nosi nima?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) "Yashirin yozuv" yoki "Yashirin qoplama" (Yunoncha steganos - yashirin, graphia - yozuv)</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Raqamli xavfsizlik</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Mukammal shifrlash</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Qadimiy tarix</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="1">
                <div class="question-text">69. Steganografik tizim tushunchasini mukammal tavsiflovchi klassik ilmiy muammo nima deb ataladi?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Turing muammosi</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) "Mahbuslar muammosi" (Simmons tomonidan taklif etilgan Prisoners' Problem)</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Ikki general muammosi</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Ryukzak muammosi</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="3">
                <div class="question-text">70. "Mahbuslar muammosi" (Prisoners' Problem) kim tomonidan taklif etilgan?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Claude Shannon</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Ron Rivest</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Adi Shamir</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Gustavus Simmons (1983-yilda)</div>
                </div>
            </div>

            <div class="question-card" data-correct-index="0">
                <div class="question-text">71. Simmons modelidagi "Kuzatuvchi" (Warden) kim?</div>
                <div class="options-list">
                    <div class="option-item" onclick="checkAnswer(this, 0)">A) Mahbuslar o'rtasidagi aloqa kanalini nazorat qiluvchi va u yerda yashirin xabar borligini aniqlashga urinuvchi stegoanalitik (qo'riqchi)</div>
                    <div class="option-item" onclick="checkAnswer(this, 1)">B) Xabarni shifrlab jo'natuvchi shaxs</div>
                    <div class="option-item" onclick="checkAnswer(this, 2)">C) Shifrlangan kalit generatori</div>
                    <div class="option-item" onclick="checkAnswer(this, 3)">D) Ma'lumotlar bazasi admini</div>
                </div>
            </div>

        </div>

        <div id="result-block" class="terminal-result">
            <h2>[+] SECURITY AUDIT COMPLETE</h2>
            <p>Muvaffaqiyatli topshirilgan testlar soni: <span id="res-correct" style="color:var(--matrix-green)">0</span></p>
            <p>Tizim xatoliklari (Xato javoblar): <span id="res-wrong" style="color:var(--error-red)">0</span></p>
            <div style="margin-top:15px;">Sizning joriy kiber unvoningiz:</div>
            <div id="cyber-rank" class="rank-badge">SYS_ADMIN</div>
            <br>
            <button class="restart-btn" onclick="location.reload()">Tizimni Qayta Yuklash (Restart)</button>
        </div>
    </main>

    <script>
        // --- MATRIX DIGITAL RAIN EFFECTS ---
        const canvas = document.getElementById('matrix-canvas');
        const ctx = canvas.getContext('2d');

        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        const alphabet = "0101010101ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789#@$&";
        const fontSize = 16;
        const columns = canvas.width / fontSize;
        const rainDrops = Array.from({ length: columns }).fill(1);

        function drawMatrix() {
            ctx.fillStyle = 'rgba(13, 13, 13, 0.05)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            ctx.fillStyle = '#00ff41';
            ctx.font = fontSize + 'px monospace';

            for (let i = 0; i < rainDrops.length; i++) {
                const text = alphabet.charAt(Math.floor(Math.random() * alphabet.length));
                ctx.fillText(text, i * fontSize, rainDrops[i] * fontSize);

                if (rainDrops[i] * fontSize > canvas.height && Math.random() > 0.975) {
                    rainDrops[i] = 0;
                }
                rainDrops[i]++;
            }
        }
        setInterval(drawMatrix, 30);

        window.addEventListener('resize', () => {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        });


        // --- SYSTEM ENGINE (QUIZ LOGIC) ---
        const cards = document.querySelectorAll('.question-card');
        let totalQuestions = cards.length;
        let currentIdx = 0;
        let correctCount = 0;
        let wrongCount = 0;
        let firewallHP = 5; // Umumiy 5 ta xato qilish imkoniyati (Firewall HP)
        let timeLeft = 60;
        let timerInterval;

        // Boshlang'ich yuklash
        document.getElementById('total-count').innerText = totalQuestions;
        updateDashboard();
        startTimer();

        function startTimer() {
            clearInterval(timerInterval);
            timeLeft = 60;
            const timerEl = document.getElementById('timer');
            timerEl.classList.remove('warning');
            timerEl.innerText = timeLeft + "s";

            timerInterval = setInterval(() => {
                timeLeft--;
                timerEl.innerText = timeLeft + "s";

                if (timeLeft <= 10) {
                    timerEl.classList.add('warning');
                }

                if (timeLeft <= 0) {
                    clearInterval(timerInterval);
                    autoFailCurrentQuestion();
                }
            }, 1000);
        }

        function checkAnswer(selectedOption, optionIndex) {
            clearInterval(timerInterval);
            
            const currentCard = cards[currentIdx];
            const correctIndex = parseInt(currentCard.getAttribute('data-correct-index'));
            const options = currentCard.querySelectorAll('.option-item');

            // Barcha variantlarni bloklash
            options.forEach(opt => opt.classList.add('disabled'));

            if (optionIndex === correctIndex) {
                selectedOption.classList.add('correct');
                correctCount++;
            } else {
                selectedOption.classList.add('wrong');
                options[correctIndex].classList.add('correct');
                wrongCount++;
                firewallHP--;
            }

            updateDashboard();

            if (firewallHP <= 0) {
                setTimeout(endQuiz, 1500);
                return;
            }

            // Keyingi savolga o'tish kechikishi (1.5 soniya natijani ko'rish uchun)
            setTimeout(() => {
                currentCard.classList.remove('active');
                currentIdx++;
                
                if (currentIdx < totalQuestions) {
                    document.getElementById('current-idx').innerText = currentIdx + 1;
                    cards[currentIdx].classList.add('active');
                    startTimer();
                } else {
                    endQuiz();
                }
            }, 1500);
        }

        function autoFailCurrentQuestion() {
            const currentCard = cards[currentIdx];
            const correctIndex = parseInt(currentCard.getAttribute('data-correct-index'));
            const options = currentCard.querySelectorAll('.option-item');

            options.forEach(opt => opt.classList.add('disabled'));
            options[correctIndex].classList.add('correct');
            
            wrongCount++;
            firewallHP--;
            updateDashboard();

            setTimeout(() => {
                currentCard.classList.remove('active');
                currentIdx++;

                if (firewallHP <= 0 || currentIdx >= totalQuestions) {
                    endQuiz();
                } else {
                    document.getElementById('current-idx').innerText = currentIdx + 1;
                    cards[currentIdx].classList.add('active');
                    startTimer();
                }
            }, 1500);
        }

        function updateDashboard() {
            // HP panelini yangilash (5 talik blok)
            let hpBar = "";
            for(let i=0; i<5; i++) {
                hpBar += (i < firewallHP) ? "█" : "░";
            }
            document.getElementById('firewall-hp').innerText = hpBar;

            // Aniqlik darajasi (Accuracy)
            let totalAnswered = correctCount + wrongCount;
            let accuracy = totalAnswered > 0 ? Math.round((correctCount / totalAnswered) * 100) : 100;
            document.getElementById('accuracy').innerText = accuracy + "%";
        }

        function endQuiz() {
            clearInterval(timerInterval);
            document.getElementById('quiz-wrapper').style.display = 'none';
            
            const resultBlock = document.getElementById('result-block');
            resultBlock.style.display = 'block';
            
            document.getElementById('res-correct').innerText = correctCount;
            document.getElementById('res-wrong').innerText = wrongCount;

            // Kiber Unvon hisoblash
            let scorePercent = (correctCount / totalQuestions) * 100;
            let rank = "";
            
            if (firewallHP <= 0) {
                rank = "SYSTEM COMPROMISED (Tizim buzildi)";
                document.getElementById('cyber-rank').style.color = 'var(--error-red)';
                document.getElementById('cyber-rank').style.borderColor = 'var(--error-red)';
            } else if (scorePercent >= 90) {
                rank = "ROOT ADMIN / CYBER COMMANDER";
            } else if (scorePercent >= 70) {
                rank = "ETHICAL HACKER (Oq qalpoqli xaker)";
            } else if (scorePercent >= 45) {
                rank = "SECOPS ANALYST (Tahlilchi)";
            } else {
                rank = "SCRIPT KIDDIE (Yangi boshlovchi)";
            }
            
            document.getElementById('cyber-rank').innerText = rank;
        }
    </script>
</body>
</html>
