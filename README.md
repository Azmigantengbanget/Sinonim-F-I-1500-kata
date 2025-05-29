<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Arduino Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [

  { "en": "Fotometer?", "id": "Pengukur cahaya." },
  { "en": "Foton?", "id": "Partikel cahaya." },
  { "en": "Fotoperiodisme?", "id": "Respon cahaya." },
  { "en": "Fotosel?", "id": "Sel peka cahaya." },
  { "en": "Fotosfer?", "id": "Lapisan matahari." },
  { "en": "Fotosintesis?", "id": "Proses masak tumbuhan." },
  { "en": "Fototaksis?", "id": "Gerak karena cahaya." },
  { "en": "Fototropisme?", "id": "Tumbuh arah cahaya." },
  { "en": "Fovea?", "id": "Bagian retina." },
  { "en": "Fradasi?", "id": "Perataan (arkais)." },
  { "en": "Fragmen?", "id": "Pecahan." },
  { "en": "Fragmentaris?", "id": "Terpecah-pecah." },
  { "en": "Fragmentasi?", "id": "Pemecahan." },
  { "en": "Fraksi?", "id": "Kelompok (parlemen)." },
  { "en": "Fraksinasi?", "id": "Pemisahan fraksi." },
  { "en": "Fraktur?", "id": "Patah tulang." },
  { "en": "Frambos?", "id": "Frambusia." },
  { "en": "Frambusia?", "id": "Penyakit kulit." },
  { "en": "Francium?", "id": "Fransium." },
  { "en": "Frangipani?", "id": "Kamboja." },
  { "en": "Franko?", "id": "Bebas biaya." },
  { "en": "Fransium?", "id": "Unsur kimia." },
  { "en": "Frasa?", "id": "Gabungan kata." },
  { "en": "Frase?", "id": "Frasa." },
  { "en": "Fraseologi?", "id": "Ilmu frasa." },
  { "en": "Frater?", "id": "Biarawan." },
  { "en": "Fraternitas?", "id": "Persaudaraan." },
  { "en": "Freatofit?", "id": "Tumbuhan air tanah." },
  { "en": "Fregat?", "id": "Kapal perang." },
  { "en": "Frekuen?", "id": "Sering." },
  { "en": "Frekuensi?", "id": "Kekerapan." },
  { "en": "Frekuentatif?", "id": "Menyatakan pengulangan." },
  { "en": "Frenologi?", "id": "Ilmu tengkorak (lama)." },
  { "en": "Freon?", "id": "Gas pendingin." },
  { "en": "Fresko?", "id": "Lukisan dinding." },
  { "en": "Friabilitas?", "id": "Kerapuhan." },
  { "en": "Frigid?", "id": "Dingin (seksual)." },
  { "en": "Frigiditas?", "id": "Keadaan frigid." },
  { "en": "Frikatif?", "id": "Bunyi geser." },
  { "en": "Friksi?", "id": "Gesekan." },
  { "en": "Front?", "id": "Barisan depan." },
  { "en": "Frontal?", "id": "Langsung." },
  { "en": "Fruktosa?", "id": "Gula buah." },
  { "en": "Frustrasi?", "id": "Kekecewaan." },
  { "en": "Ftalina?", "id": "Senyawa kimia." },
  { "en": "Fuad?", "id": "Hati (Arab)." },
  { "en": "Fugasitas?", "id": "Kecenderungan menguap." },
  { "en": "Fuksia?", "id": "Warna." },
  { "en": "Fuksina?", "id": "Zat pewarna." },
  { "en": "Fulgurit?", "id": "Batuan sambaran petir." },
  { "en": "Fulminat?", "id": "Garam peledak." },
  { "en": "Fulus?", "id": "Uang (Arab)." },
  { "en": "Fumarol?", "id": "Lubang uap vulkanik." },
  { "en": "Fundamental?", "id": "Mendasar." },
  { "en": "Fundamentalis?", "id": "Penganut paham dasar." },
  { "en": "Fundamentalisme?", "id": "Paham kembali ke dasar." },
  { "en": "Fundasi?", "id": "Fondasi." },
  { "en": "Fungi?", "id": "Jamur." },
  { "en": "Fungibel?", "id": "Dapat ditukar." },
  { "en": "Fungisida?", "id": "Pembasmi jamur." },
  { "en": "Fungistatik?", "id": "Penghambat jamur." },
  { "en": "Fungoid?", "id": "Mirip jamur." },
  { "en": "Fungsi?", "id": "Guna." },
  { "en": "Fungsional?", "id": "Berguna." },
  { "en": "Fungsionalisme?", "id": "Paham fungsi." },
  { "en": "Fungsionaris?", "id": "Pejabat." },
  { "en": "Furqan?", "id": "Pembeda (Al-Quran)." },
  { "en": "Furtif?", "id": "Sembunyi-sembunyi." },
  { "en": "Fusi?", "id": "Penggabungan." },
  { "en": "Fusta?", "id": "Perahu (arkais)." },
  { "en": "Futur?", "id": "Masa depan (arkais)." },
  { "en": "Futuris?", "id": "Penganut futurisme." },
  { "en": "Futurisme?", "id": "Aliran seni." },
  { "en": "Futuristik?", "id": "Mengarah ke masa depan." },
  { "en": "Futurolog?", "id": "Ahli masa depan." },
  { "en": "Futurologi?", "id": "Ilmu masa depan." },
  { "en": "Gabah?", "id": "Padi kering." },
  { "en": "Gabai?", "id": "Besar (arkais)." },
  { "en": "Gabak?", "id": "Mendung." },
  { "en": "Gabardin?", "id": "Kain." },
  { "en": "Gabas?", "id": "Kasar (arkais)." },
  { "en": "Gabi?", "id": "Ubi (arkais)." },
  { "en": "Gableg?", "id": "Pukulan (Jawa)." },
  { "en": "Gabro?", "id": "Batuan." },
  { "en": "Gabuk?", "id": "Tidak berisi (padi)." },
  { "en": "Gabung?", "id": "Satukan." },
  { "en": "Gabus?", "id": "Bahan ringan." },
  { "en": "Gaco?", "id": "Jagoan." },
  { "en": "Gacor?", "id": "Nyaring (burung)." },
  { "en": "Gada?", "id": "Senjata." },
  { "en": "Gadai?", "id": "Jaminan utang." },
  { "en": "Gadamala?", "id": "Limau." },
  { "en": "Gadang?", "id": "Besar (Minang)." },
  { "en": "Gading?", "id": "Taring gajah." },
  { "en": "Gadis?", "id": "Perawan." },
  { "en": "Gadolinit?", "id": "Mineral." },
  { "en": "Gadolinium?", "id": "Unsur kimia." },
  { "en": "Gadon?", "id": "Lauk (Jawa)." },
  { "en": "Gaduh?", "id": "Ribut." },
  { "en": "Gaduk?", "id": "Sombong." },
  { "en": "Gadung?", "id": "Ubi." },
  { "en": "Gae?", "id": "Kerja (Jawa)." },
  { "en": "Gael?", "id": "Senggol." },
  { "en": "Gaet?", "id": "Kait." }
  { "en": "Gafar?", "id": "Pengampun (Arab)." },
  { "en": "Gaflet?", "id": "Tombak (arkais)." },
  { "en": "Gaflah?", "id": "Lalai (Arab)." },
  { "en": "Gafur?", "id": "Pengampun." },
  { "en": "Gaga?", "id": "Padi ladang." },
  { "en": "Gagah?", "id": "Perkasa." },
  { "en": "Gagak?", "id": "Burung." },
  { "en": "Gagal?", "id": "Tidak berhasil." },
  { "en": "Gagang?", "id": "Tangkai." },
  { "en": "Gagas?", "id": "Pikir." },
  { "en": "Gagasan?", "id": "Ide." },
  { "en": "Gagu?", "id": "Bisu." },
  { "en": "Gaguk?", "id": "Kaku (arkais)." },
  { "en": "Gah?", "id": "Sudah (arkais)." },
  { "en": "Gahar?", "id": "Gosok keras." },
  { "en": "Gahara?", "id": "Keturunan raja." },
  { "en": "Gaharu?", "id": "Kayu wangi." },
  { "en": "Gai?", "id": "Kalian (arkais)." },
  { "en": "Gaib?", "id": "Tersembunyi." },
  { "en": "Gail?", "id": "Cerewet (arkais)." },
  { "en": "Gairah?", "id": "Nafsu." },
  { "en": "Gaisi?", "id": "Selir (Jepang)." },
  { "en": "Gait?", "id": "Kait." },
  { "en": "Gajah?", "id": "Hewan besar." },
  { "en": "Gajal?", "id": "Ganjal (arkais)." },
  { "en": "Gaji?", "id": "Upah." },
  { "en": "Gajih?", "id": "Lemak." },
  { "en": "Gajul?", "id": "Ganjal." },
  { "en": "Gakin?", "id": "Miskin (Jawa)." },
  { "en": "Gala?", "id": "Damar." },
  { "en": "Gala-gala?", "id": "Perekat." },
  { "en": "Galaba?", "id": "Kemenangan (arkais)." },
  { "en": "Galaganjur?", "id": "Musik Bali." },
  { "en": "Galagasi?", "id": "Semut." },
  { "en": "Galah?", "id": "Tongkat panjang." },
  { "en": "Galai?", "id": "Dayung (arkais)." },
  { "en": "Galak?", "id": "Buas." },
  { "en": "Galaksi?", "id": "Bimasakti." },
  { "en": "Galan?", "id": "Sopan (arkais)." },
  { "en": "Galang?", "id": "Sangga." },
  { "en": "Galar?", "id": "Dasar lantai." },
  { "en": "Galas?", "id": "Pikul (arkais)." },
  { "en": "Galat?", "id": "Kesalahan." },
  { "en": "Galau?", "id": "Bingung." },
  { "en": "Galeb?", "id": "Menang (arkais)." },
  { "en": "Galeng?", "id": "Pematang." },
  { "en": "Galeri?", "id": "Ruang pamer." },
  { "en": "Gali?", "id": "Korek." },
  { "en": "Galias?", "id": "Kapal." },
  { "en": "Galib?", "id": "Umum." },
  { "en": "Galih?", "id": "Inti kayu." },
  { "en": "Galing?", "id": "Pusing (arkais)." },
  { "en": "Galir?", "id": "Lancar." },
  { "en": "Galium?", "id": "Unsur kimia." },
  { "en": "Galon?", "id": "Satuan isi." },
  { "en": "Galu-galu?", "id": "Penggoda." },
  { "en": "Galuh?", "id": "Permata." },
  { "en": "Galvanis?", "id": "Lapis seng." },
  { "en": "Galvanisasi?", "id": "Proses pelapisan." },
  { "en": "Galvanometer?", "id": "Pengukur arus." },
  { "en": "Gam?", "id": "Lem (Inggris)." },
  { "en": "Gema?", "id": "Gaung." },
  { "en": "Gamak?", "id": "Raba (arkais)." },
  { "en": "Gamal?", "id": "Pohon." },
  { "en": "Gamalisasi?", "id": "Pemusnahan hama." },
  { "en": "Gamam?", "id": "Gugup." },
  { "en": "Gaman?", "id": "Senjata (Jawa)." },
  { "en": "Gamang?", "id": "Takut tinggi." },
  { "en": "Gamat?", "id": "Teripang." },
  { "en": "Gambang?", "id": "Alat musik." },
  { "en": "Gambar?", "id": "Lukisan." },
  { "en": "Gambas?", "id": "Oyong." },
  { "en": "Gambir?", "id": "Tanaman." },
  { "en": "Gamblang?", "id": "Jelas." },
  { "en": "Gambuh?", "id": "Tarian." },
  { "en": "Gambus?", "id": "Alat musik." },
  { "en": "Gamelan?", "id": "Ansambel musik Jawa." },
  { "en": "Gamet?", "id": "Sel kelamin." },
  { "en": "Gametofit?", "id": "Fase tumbuhan." },
  { "en": "Gamik?", "id": "Main (arkais)." },
  { "en": "Gamil?", "id": "Jelek (arkais)." },
  { "en": "Gamis?", "id": "Jubah." },
  { "en": "Gamit?", "id": "Sentuh." },
  { "en": "Gamma?", "id": "Sinar." },
  { "en": "Gampang?", "id": "Mudah." },
  { "en": "Gampar?", "id": "Tampar." },
  { "en": "Gampeng?", "id": "Lereng (arkais)." },
  { "en": "Gamping?", "id": "Batu kapur." },
  { "en": "Gana?", "id": "Kaya (Sanskerta)." },
  { "en": "Ganal?", "id": "Besar (Banjar)." },
  { "en": "Ganas?", "id": "Buas." },
  { "en": "Ganco?", "id": "Pengait." },
  { "en": "Ganda?", "id": "Dobel." },
  { "en": "Gandapura?", "id": "Tanaman." },
  { "en": "Gandar?", "id": "Gagang." },
  { "en": "Gandarua?", "id": "Makhluk halus." },
  { "en": "Gandasuli?", "id": "Tanaman." },
  { "en": "Gandem?", "id": "Kuat (Jawa)." },
  { "en": "Gandeng?", "id": "Berpegangan." },
  { "en": "Gandewa?", "id": "Busur panah." },
  { "en": "Gandi?", "id": "Palu." },
  { "en": "Gandik?", "id": "Hiasan dahi." },
  { "en": "Ganding?", "id": "Bambu." },
  { "en": "Gandrung?", "id": "Terpesona." },
  { "en": "Gandu?", "id": "Buah." },
  { "en": "Gandul?", "id": "Gantung." },
  { "en": "Gandum?", "id": "Serealia." },
  { "en": "Gang?", "id": "Lorong." },
  { "en": "Ganggang?", "id": "Alga." },
  { "en": "Ganggu?", "id": "Usik." },
  { "en": "Gangguan?", "id": "Rintangan." },
  { "en": "Ganglion?", "id": "Simpul saraf." },
  { "en": "Gangsa?", "id": "Gamelan (Bali)." },
  { "en": "Gangsal?", "id": "Ganjil (Jawa)." },
  { "en": "Gangsar?", "id": "Lancar (Jawa)." },
  { "en": "Gangsi?", "id": "Gasing (arkais)." },
  { "en": "Gangsir?", "id": "Jangkrik." },
  { "en": "Gangster?", "id": "Penjahat (Inggris)." },
  { "en": "Ganja?", "id": "Narkotika." },
  { "en": "Ganjal?", "id": "Sangga." },
  { "en": "Ganjar?", "id": "Beri hadiah." },
  { "en": "Ganjaran?", "id": "Imbalan." },
  { "en": "Ganjat?", "id": "Tegang (arkais)." },
  { "en": "Ganjil?", "id": "Aneh." },
  { "en": "Ganjur?", "id": "Tombak." },
  { "en": "Gantar?", "id": "Galah." },
  { "en": "Gantel?", "id": "Terikat (Jawa)." },
  { "en": "Ganteng?", "id": "Tampan." },
  { "en": "Ganti?", "id": "Tukar." },
  { "en": "Gantung?", "id": "Sangkut." },
  { "en": "Ganyah?", "id": "Gosok (arkais)." },
  { "en": "Ganyang?", "id": "Hancurkan." },
  { "en": "Ganyar?", "id": "Keras (arkais)." },
  { "en": "Ganyong?", "id": "Umbi." },
  { "en": "Gap?", "id": "Kesenjangan (Inggris)." },
  { "en": "Gapa?", "id": "Gapai (arkais)." },
  { "en": "Gapah?", "id": "Cekatan." },
  { "en": "Gapai?", "id": "Raih." },
  { "en": "Gapil?", "id": "Nakal (tangan)." },
  { "en": "Gapit?", "id": "Penjepit." },
  { "en": "Gaple?", "id": "Permainan kartu." },
  { "en": "Gaplek?", "id": "Singkong kering." },
  { "en": "Gapyak?", "id": "Ramah (Jawa)." },
  { "en": "Gapyuk?", "id": "Tangkap." },
  { "en": "Gara-gara?", "id": "Sebab." },
  { "en": "Garah?", "id": "Gurau." },
  { "en": "Garai?", "id": "Pilih (arkais)." },
  { "en": "Garam?", "id": "Zat asin." },
  { "en": "Garan?", "id": "Tangkai (Jawa)." },
  { "en": "Garang?", "id": "Galak." },
  { "en": "Garansi?", "id": "Jaminan." },
  { "en": "Garap?", "id": "Kerjakan." },
  { "en": "Garasi?", "id": "Kandang mobil." },
  { "en": "Garda?", "id": "Pengawal." },
  { "en": "Gardan?", "id": "Bagian mobil." },
  { "en": "Gardu?", "id": "Pos jaga." },
  { "en": "Garek?", "id": "Bunyi (Minang)." },
  { "en": "Garen?", "id": "Benang (Belanda)." },
  { "en": "Gareng?", "id": "Tokoh wayang." },
  { "en": "Garib?", "id": "Asing (Arab)." },
  { "en": "Garing?", "id": "Kering." },
  { "en": "Garis?", "id": "Corek." },
  { "en": "Garit?", "id": "Gores." },
  { "en": "Garnis?", "id": "Hiasan." },
  { "en": "Garnisun?", "id": "Pasukan kota." },
  { "en": "Garong?", "id": "Rampok." },
  { "en": "Garpu?", "id": "Alat makan." },
  { "en": "Garu?", "id": "Sisir tanah." },
  { "en": "Garuk?", "id": "Gores." },
  { "en": "Garung?", "id": "Raung." },
  { "en": "Garuda?", "id": "Lambang negara." },
  { "en": "Gas?", "id": "Zat ringan." },
  { "en": "Gasa?", "id": "Kuat (arkais)." },
  { "en": "Gasak?", "id": "Hantam." },
  { "en": "Gasal?", "id": "Ganjil." },
  { "en": "Gasing?", "id": "Mainan putar." },
  { "en": "Gasolin?", "id": "Bensin." },
  { "en": "Gasometer?", "id": "Pengukur gas." },
  { "en": "Gastrin?", "id": "Hormon." },
  { "en": "Gastritis?", "id": "Radang lambung." },
  { "en": "Gastrointestinal?", "id": "Saluran cerna." },
  { "en": "Gastronomi?", "id": "Seni kuliner." },
  { "en": "Gastropoda?", "id": "Kelas moluska." },
  { "en": "Gastrula?", "id": "Tahap embrio." },
  { "en": "Gatal?", "id": "Rasa ingin menggaruk." },
  { "en": "Gatotkaca?", "id": "Tokoh wayang." },
  { "en": "Gatra?", "id": "Baris (tembang)." },
  { "en": "Gatuk?", "id": "Cocok (Jawa)." },
  { "en": "Gau?", "id": "Distrik (Jerman)." },
  { "en": "Gaukang?", "id": "Permata (Makassar)." },
  { "en": "Gaun?", "id": "Pakaian wanita." },
  { "en": "Gaung?", "id": "Gema." },
  { "en": "Gawai?", "id": "Alat." },
  { "en": "Gawal?", "id": "Salah (arkais)." },
  { "en": "Gawang?", "id": "Bingkai gol." },
  { "en": "Gawar?", "id": "Rambu." },
  { "en": "Gawat?", "id": "Darurat." },
  { "en": "Gawir?", "id": "Jurang (Jawa)." },
  { "en": "Gaya?", "id": "Model." },
  { "en": "Gayal?", "id": "Kenyal." },
  { "en": "Gayam?", "id": "Pohon." },
  { "en": "Gayang?", "id": "Goyang (arkais)." },
  { "en": "Gayat?", "id": "Tinggi (arkais)." },
  { "en": "Gayem?", "id": "Kunyah (Jawa)." },
  { "en": "Gayeng?", "id": "Ramai (Jawa)." },
  { "en": "Gayuh?", "id": "Capai (Jawa)." },
  { "en": "Gayuk?", "id": "Sambung." },
  { "en": "Gayung?", "id": "Ciduk." },
  { "en": "Gaz?", "id": "Kain kasa." },
  { "en": "Gazal?", "id": "Puisi Arab." },
  { "en": "Ge?", "id": "Tanah (Yunani)." },
  { "en": "Geber?", "id": "Pukul (drum)." },
  { "en": "Gebang?", "id": "Palma." },
  { "en": "Gebar?", "id": "Selimut." },
  { "en": "Gebeng?", "id": "Buka lebar (arkais)." },
  { "en": "Gebyar?", "id": "Kemeriahan." },
  { "en": "Gebyur?", "id": "Siram." },
  { "en": "Gecar?", "id": "Liar (arkais)." },
  { "en": "Gecek?", "id": "Tekan (arkais)." },
  { "en": "Gecul?", "id": "Lucu (Jawa)." },
  { "en": "Gedabah?", "id": "Perisai." },
  { "en": "Gede?", "id": "Besar (Jawa/Sunda)." },
  { "en": "Gedebak-gedebuk?", "id": "Bunyi berdebar." },
  { "en": "Gedebar?", "id": "Debar." },
  { "en": "Gedebog?", "id": "Batang pisang." },
  { "en": "Gedebuk?", "id": "Bunyi jatuh." },
  { "en": "Gedeh?", "id": "Besar." },
  { "en": "Gedek?", "id": "Dinding anyaman bambu." },
  { "en": "Gedembai?", "id": "Makhluk halus." },
  { "en": "Gedembal?", "id": "Gembel (arkais)." },
  { "en": "Gedempol?", "id": "Besar (arkais)." },
  { "en": "Gading?", "id": "Gading (arkais)." },
  { "en": "Gedok?", "id": "Pukul (Jawa)." },
  { "en": "Gedombak?", "id": "Gendang." },
  { "en": "Gedombrongan?", "id": "Terlalu besar (pakaian)." },
  { "en": "Gedong?", "id": "Gedung." },
  { "en": "Gedombrong?", "id": "Gedombrongan." },
  { "en": "Gegadan?", "id": "Senjata (arkais)." },
  { "en": "Gegai?", "id": "Mudah lepas." },
  { "en": "Gegala?", "id": "Obor." },
  { "en": "Gegan?", "id": "Senjata (arkais)." },
  { "en": "Gegap?", "id": "Riuh." },
  { "en": "Gegar?", "id": "Guncang." },
  { "en": "Gegas?", "id": "Cepat." },
  { "en": "Gegat?", "id": "Serangga." },
  { "en": "Gege?", "id": "Panggang (Jawa)." },
  { "en": "Gegedempol?", "id": "Besar dan kuat." },
  { "en": "Gegep?", "id": "Tang." },
  { "en": "Geger?", "id": "Heboh." },
  { "en": "Gegetun?", "id": "Menyesal (Jawa)." },
  { "en": "Gejah?", "id": "Loncat (arkais)." },
  { "en": "Gejala?", "id": "Tanda." },
  { "en": "Gejolak?", "id": "Pergolakan." },
  { "en": "Gelabur?", "id": "Celup (arkais)." },
  { "en": "Geladak?", "id": "Lantai kapal." },
  { "en": "Geladir?", "id": "Lendir." },
  { "en": "Geladrah?", "id": "Tidak teratur." },
  { "en": "Gelagah?", "id": "Rumput tinggi." },
  { "en": "Gelagapan?", "id": "Gugup." },
  { "en": "Gelagar?", "id": "Balok penyangga." },
  { "en": "Gelagat?", "id": "Firasat." },
  { "en": "Gelah?", "id": "Buka (arkais)." },
  { "en": "Gelak?", "id": "Tertawa." },
  { "en": "Gelakak?", "id": "Tertawa terbahak-bahak." },
  { "en": "Gelalar?", "id": "Tidak teratur." },
  { "en": "Gelam?", "id": "Pohon." },
  { "en": "Gelama?", "id": "Ikan." },
  { "en": "Gelambir?", "id": "Kulit kendur." },
  { "en": "Gelana?", "id": "Sedih (arkais)." },
  { "en": "Gelandang?", "id": "Pemain tengah (sepak bola)." },
  { "en": "Gelang?", "id": "Perhiasan tangan." },
  { "en": "Gelanggang?", "id": "Arena." },
  { "en": "Gelantang?", "id": "Jemur." },
  { "en": "Gelap?", "id": "Gulita." },
  { "en": "Gelar?", "id": "Sebutan." },
  { "en": "Gelas?", "id": "Wadah minum." },
  { "en": "Gelatak?", "id": "Bunyi." },
  { "en": "Gelatik?", "id": "Burung." },
  { "en": "Gelatin?", "id": "Zat perekat." },
  { "en": "Geledah?", "id": "Periksa." },
  { "en": "Geleding?", "id": "Melengkung." },
  { "en": "Geledek?", "id": "Guntur." },
  { "en": "Geledur?", "id": "Keriput." },
  { "en": "Gelegah?", "id": "Gemuruh (arkais)." },
  { "en": "Gelegak?", "id": "Mendidih." },
  { "en": "Gelegar?", "id": "Bunyi keras." },
  { "en": "Gelek?", "id": "Hindar." },
  { "en": "Gelekak?", "id": "Lepas (kulit)." },
  { "en": "Gelekek?", "id": "Tertawa kecil." },
  { "en": "Gelema?", "id": "Lendir (arkais)." },
  { "en": "Gelemat?", "id": "Licin (arkais)." },
  { "en": "Gelembung?", "id": "Buih." },
  { "en": "Gelempang?", "id": "Terbaring." },
  { "en": "Gelendot?", "id": "Bergelayut." },
  { "en": "Geleng?", "id": "Goyang kepala." },
  { "en": "Gelenggang?", "id": "Tanaman." },
  { "en": "Gelenyar?", "id": "Geli." },
  { "en": "Gelepai?", "id": "Terkulai." },
  { "en": "Gelepar?", "id": "Menggelepar." },
  { "en": "Gelepok?", "id": "Jatuh." },
  { "en": "Geler?", "id": "Getar (arkais)." },
  { "en": "Gelesek?", "id": "Gesek." },
  { "en": "Geleser?", "id": "Meluncur." },
  { "en": "Geletah?", "id": "Genit." },
  { "en": "Geletak?", "id": "Terbaring." },
  { "en": "Geletar?", "id": "Getar." },
  { "en": "Geletek?", "id": "Geli." },
  { "en": "Geletuk?", "id": "Bunyi." },
  { "en": "Geli?", "id": "Rasa ingin tertawa." },
  { "en": "Geliang?", "id": "Regang." },
  { "en": "Geliat?", "id": "Gerak menggeliat." },
  { "en": "Gelibir?", "id": "Melengkung ke bawah." },
  { "en": "Gelicik?", "id": "Hindar." },
  { "en": "Geliga?", "id": "Batu sakti." },
  { "en": "Geligi?", "id": "Menggigil." },
  { "en": "Geligit?", "id": "Gigit kecil." },
  { "en": "Gelimang?", "id": "Berkubang." },
  { "en": "Gelimantang?", "id": "Bintang (arkais)." },
  { "en": "Gelimbir?", "id": "Kendor." },
  { "en": "Gelimpang?", "id": "Terbaring." },
  { "en": "Gelincha?", "id": "Lompat (arkais)." },
  { "en": "Gelinding?", "id": "Bergulir." },
  { "en": "Gelinggam?", "id": "Merah tua." },
  { "en": "Gelinjang?", "id": "Melonjak." },
  { "en": "Gelintang?", "id": "Melintang." },
  { "en": "Gelintar?", "id": "Kilat (arkais)." },
  { "en": "Gelintir?", "id": "Butir kecil." },
  { "en": "Gelisah?", "id": "Resah." },
  { "en": "Gelita?", "id": "Gulita." },
  { "en": "Gelo?", "id": "Gila (Sunda)." },
  { "en": "Gelodar?", "id": "Kotor (arkais)." },
  { "en": "Gelogok?", "id": "Minum cepat." },
  { "en": "Gelohok?", "id": "Ternganga." },
  { "en": "Gelojoh?", "id": "Rakus." },
  { "en": "Gelompar?", "id": "Lompat." },
  { "en": "Gelondong?", "id": "Batang kayu." },
  { "en": "Gelonggong?", "id": "Isi air (sapi)." },
  { "en": "Gelongsor?", "id": "Longsor." },
  { "en": "Gelopak?", "id": "Kelopak." },
  { "en": "Gelora?", "id": "Semangat." },
  { "en": "Gelosang?", "id": "Cepat (arkais)." },
  { "en": "Geloso?", "id": "Licin (arkais)." },
  { "en": "Gelotak?", "id": "Bunyi gigi." },
  { "en": "Geluduk?", "id": "Guntur." },
  { "en": "Geluga?", "id": "Zat pewarna." },
  { "en": "Geluh?", "id": "Tanah liat." },
  { "en": "Gelulur?", "id": "Turun." },
  { "en": "Gelumang?", "id": "Berguling." },
  { "en": "Gelumat?", "id": "Lumut (arkais)." },
  { "en": "Geluncur?", "id": "Luncur." },
  { "en": "Gelung?", "id": "Sanggul." },
  { "en": "Gelup?", "id": "Cabut (arkais)." },
  { "en": "Gelupur?", "id": "Menggelepar." },
  { "en": "Gelut?", "id": "Bergulat." },
  { "en": "Gema?", "id": "Gaung." },
  { "en": "Gemal?", "id": "Genggam." },
  { "en": "Gemala?", "id": "Batu sakti." },
  { "en": "Gemap?", "id": "Terkejut (arkais)." },
  { "en": "Gemar?", "id": "Suka." },
  { "en": "Gemas?", "id": "Jengkel." },
  { "en": "Gembak?", "id": "Rambut dahi." },
  { "en": "Gembala?", "id": "Pengurus ternak." },
  { "en": "Gembar-gembor?", "id": "Teriak-teriak." },
  { "en": "Gembel?", "id": "Tuna wisma." },
  { "en": "Gembeng?", "id": "Mudah menangis." },
  { "en": "Gembira?", "id": "Senang." },
  { "en": "Gemblak?", "id": "Anak laki-laki (reog)." },
  { "en": "Gembleng?", "id": "Tempa." },
  { "en": "Gemblok?", "id": "Gendong (Jawa)." },
  { "en": "Gemblong?", "id": "Kue." },
  { "en": "Gembok?", "id": "Kunci." },
  { "en": "Gembol?", "id": "Bawa." },
  { "en": "Gembor?", "id": "Alat siram." },
  { "en": "Gembos?", "id": "Kempis." },
  { "en": "Gembreng?", "id": "Kaleng." },
  { "en": "Gembrot?", "id": "Gemuk." },
  { "en": "Gembul?", "id": "Rakus (Jawa)." },
  { "en": "Gembut?", "id": "Empuk." },
  { "en": "Gemelai?", "id": "Lentur." },
  { "en": "Gemelentam?", "id": "Bunyi." },
  { "en": "Gemelugut?", "id": "Menggigil." },
  { "en": "Gemendul?", "id": "Menggantung." },
  { "en": "Gementar?", "id": "Gemetar." },
  { "en": "Gemerencang?", "id": "Bunyi." },
  { "en": "Gemerencik?", "id": "Bunyi." },
  { "en": "Gemerencung?", "id": "Bunyi." },
  { "en": "Gemeresak?", "id": "Bunyi." },
  { "en": "Gemeresik?", "id": "Bunyi." },
  { "en": "Gemerlap?", "id": "Berkilau." },
  { "en": "Gemertak?", "id": "Bunyi." },
  { "en": "Gemertap?", "id": "Bunyi." },
  { "en": "Gemeruh?", "id": "Gemuruh." },
  { "en": "Gemerusuk?", "id": "Bunyi." },
  { "en": "Gemetar?", "id": "Menggigil." },
  { "en": "Gemi?", "id": "Ikan." },
  { "en": "Gemik?", "id": "Sentuh (arkais)." },
  { "en": "Gemil?", "id": "Penuh (arkais)." },
  { "en": "Gemilang?", "id": "Cemerlang." },
  { "en": "Geming?", "id": "Gerak." },
  { "en": "Geminte?", "id": "Kotapraja (Belanda)." },
  { "en": "Gemirang?", "id": "Sangat gembira." },
  { "en": "Gempa?", "id": "Getaran bumi." },
  { "en": "Gempal?", "id": "Padat berisi." },
  { "en": "Gempar?", "id": "Heboh." },
  { "en": "Gempita?", "id": "Riuh." },
  { "en": "Gempol?", "id": "Kue." },
  { "en": "Gempur?", "id": "Serang." },
  { "en": "Gemuk?", "id": "Subur (badan)." },
  { "en": "Gemul?", "id": "Empuk (arkais)." },
  { "en": "Gemulai?", "id": "Luwes." },
  { "en": "Gemuruh?", "id": "Deru." },
  { "en": "Gen?", "id": "Pembawa sifat." },
  { "en": "Gena?", "id": "Cukup (arkais)." },
  { "en": "Genah?", "id": "Baik (Jawa)." },
  { "en": "Genang?", "id": "Tergenang." },
  { "en": "Genap?", "id": "Lengkap." },
  { "en": "Gencar?", "id": "Terus-menerus." },
  { "en": "Gencat?", "id": "Henti." },
  { "en": "Gencel?", "id": "Tekan (arkais)." },
  { "en": "Gencer?", "id": "Lancar." },
  { "en": "Gendak?", "id": "Kekasih gelap." },
  { "en": "Gendala?", "id": "Halangan." },
  { "en": "Gendang?", "id": "Alat musik." },
  { "en": "Gendar?", "id": "Kerupuk." },
  { "en": "Gendeng?", "id": "Gila (kasar)." },
  { "en": "Gendewa?", "id": "Busur." },
  { "en": "Gending?", "id": "Lagu gamelan." },
  { "en": "Gendis?", "id": "Gula (Jawa)." },
  { "en": "Gendok?", "id": "Bawa (arkais)." },
  { "en": "Gendom?", "id": "Sihir." },
  { "en": "Gendong?", "id": "Dukung." },
  { "en": "Gendu?", "id": "Benjol (arkais)." },
  { "en": "Genduk?", "id": "Panggilan anak perempuan (Jawa)." },
  { "en": "Gendut?", "id": "Gemuk." },
  { "en": "General?", "id": "Umum." },
  { "en": "Generalis?", "id": "Ahli umum." },
  { "en": "Generalisasi?", "id": "Penyimpulan umum." },
  { "en": "Generator?", "id": "Pembangkit." },
  { "en": "Generasi?", "id": "Angkatan." },
  { "en": "Generatif?", "id": "Berkembang biak." },
  { "en": "Generik?", "id": "Umum (obat)." },
  { "en": "Genesis?", "id": "Asal mula." },
  { "en": "Genetik?", "id": "Keturunan." },
  { "en": "Genetika?", "id": "Ilmu keturunan." },
  { "en": "Geng?", "id": "Kelompok." },
  { "en": "Genggam?", "id": "Pegang." },
  { "en": "Gengsi?", "id": "Martabat." },
  { "en": "Genial?", "id": "Ramah." },
  { "en": "Genis?", "id": "Asam (arkais)." },
  { "en": "Genit?", "id": "Centil." },
  { "en": "Genius?", "id": "Jenius." },
  { "en": "Genjah?", "id": "Cepat berbuah." },
  { "en": "Genjang?", "id": "Tidak sejajar." },
  { "en": "Genjer?", "id": "Sayuran." },
  { "en": "Genjik?", "id": "Anak anjing (Jawa)." },
  { "en": "Genjot?", "id": "Kayuh." },
  { "en": "Genre?", "id": "Jenis (sastra/seni)." },
  { "en": "Genta?", "id": "Lonceng besar." },
  { "en": "Gentala?", "id": "Naga (kereta)." },
  { "en": "Gentar?", "id": "Takut." },
  { "en": "Gelas?", "id": "Cangkir." },
  { "en": "Gentat?", "id": "Lekuk (arkais)." },
  { "en": "Gentayangan?", "id": "Berkeliaran (roh)." },
  { "en": "Genteng?", "id": "Atap." },
  { "en": "Genting?", "id": "Kritis." },
  { "en": "Gentur?", "id": "Khidmat." },
  { "en": "Genus?", "id": "Marga (biologi)." },
  { "en": "Geodesi?", "id": "Ilmu ukur bumi." },
  { "en": "Geofisika?", "id": "Fisika bumi." },
  { "en": "Geognosi?", "id": "Ilmu batuan." },
  { "en": "Geografi?", "id": "Ilmu bumi." },
  { "en": "Geohidrologi?", "id": "Ilmu air tanah." },
  { "en": "Geokimia?", "id": "Kimia bumi." },
  { "en": "Geolog?", "id": "Ahli geologi." },
  { "en": "Geologi?", "id": "Ilmu batuan bumi." },
  { "en": "Geologiwan?", "id": "Geolog." },
  { "en": "Geometri?", "id": "Ilmu ukur." },
  { "en": "Geomorfologi?", "id": "Ilmu bentuk lahan." },
  { "en": "Geopolitik?", "id": "Politik geografi." },
  { "en": "Geotermal?", "id": "Panas bumi." },
  { "en": "Geotermi?", "id": "Geotermal." },
  { "en": "Gepai?", "id": "Raih." },
  { "en": "Gepeng?", "id": "Pipih." },
  { "en": "Geplak?", "id": "Kue." },
  { "en": "Gepok?", "id": "Tumpuk." },
  { "en": "Ger?", "id": "Bunyi." },
  { "en": "Gera?", "id": "Ingin (arkais)." },
  { "en": "Gerabah?", "id": "Tembikar." },
  { "en": "Geradi?", "id": "Tiang (arkais)." },
  { "en": "Geragai?", "id": "Kait besi." },
  { "en": "Geragap?", "id": "Gagap." },
  { "en": "Geragas?", "id": "Rakus." },
  { "en": "Gerah?", "id": "Panas (badan)." },
  { "en": "Gerai?", "id": "Kios." },
  { "en": "Geram?", "id": "Marah." },
  { "en": "Geraman?", "id": "Auman." },
  { "en": "Gerang?", "id": "Teriak (arkais)." },
  { "en": "Gerangan?", "id": "Kira-kira." },
  { "en": "Gerantak?", "id": "Serang tiba-tiba." },
  { "en": "Gerap?", "id": "Sambar (arkais)." },
  { "en": "Gerapai?", "id": "Raih." },
  { "en": "Geras?", "id": "Gosok." },
  { "en": "Gerat?", "id": "Gores (arkais)." },
  { "en": "Geratak?", "id": "Geledah." },
  { "en": "Gerawan?", "id": "Ikan." },
  { "en": "Gerawat?", "id": "Kumis." },
  { "en": "Gerayah?", "id": "Raba." },
  { "en": "Gerbak?", "id": "Sergap." },
  { "en": "Gerbang?", "id": "Pintu besar." },
  { "en": "Gerbera?", "id": "Bunga." },
  { "en": "Gerbong?", "id": "Kereta." },
  { "en": "Gercap?", "id": "Cepat (arkais)." },
  { "en": "Gerek?", "id": "Bor." },
  { "en": "Gereja?", "id": "Rumah ibadah Kristen." },
  { "en": "Gerek?", "id": "Korek." },
  { "en": "Geremet?", "id": "Gerak lambat." },
  { "en": "Gerempang?", "id": "Halang (arkais)." },
  { "en": "Gerencang?", "id": "Bunyi." },
  { "en": "Gerendel?", "id": "Kunci pintu." },
  { "en": "Gerengseng?", "id": "Semangat." },
  { "en": "Gerepek?", "id": "Keripik." },
  { "en": "Gergaji?", "id": "Alat potong." },
  { "en": "Gergajul?", "id": "Penjahat." },
  { "en": "Gergasi?", "id": "Raksasa." },
  { "en": "Gerha?", "id": "Rumah (Sanskerta)." },
  { "en": "Gerhana?", "id": "Eklips." },
  { "en": "Geriap?", "id": "Gerak." },
  { "en": "Geriatri?", "id": "Ilmu lansia." },
  { "en": "Geribik?", "id": "Anyaman bambu." },
  { "en": "Gericau?", "id": "Kicau." },
  { "en": "Geridip?", "id": "Kelip." },
  { "en": "Geridit?", "id": "Gerak sedikit." },
  { "en": "Gerigi?", "id": "Gigi gergaji." },
  { "en": "Gerigis?", "id": "Tidak rata." },
  { "en": "Gerih?", "id": "Ikan asin." },
  { "en": "Gerilya?", "id": "Perang kecil." },
  { "en": "Gerim?", "id": "Lukis (arkais)." },
  { "en": "Gerimis?", "id": "Hujan rintik." },
  { "en": "Gerincing?", "id": "Bunyi." },
  { "en": "Gering?", "id": "Sakit." },
  { "en": "Geringsing?", "id": "Kain Bali." },
  { "en": "Gerinjal?", "id": "Ginjal." },
  { "en": "Gerinyau?", "id": "Gatal." },
  { "en": "Gerinyut?", "id": "Kerut." },
  { "en": "Gerip?", "id": "Kikis (arkais)." },
  { "en": "Gerlip?", "id": "Kelip." },
  { "en": "Germo?", "id": "Mucikari." },
  { "en": "Gerobak?", "id": "Pedati." },
  { "en": "Gerogol?", "id": "Rintangan." },
  { "en": "Gerogot?", "id": "Gigit." },
  { "en": "Gerohok?", "id": "Berlubang." },
  { "en": "Gerombol?", "id": "Kumpul." },
  { "en": "Gerombong?", "id": "Kelompok." },
  { "en": "Geronggang?", "id": "Rongga." },
  { "en": "Geronggong?", "id": "Berongga." },
  { "en": "Gerontokrasi?", "id": "Pemerintahan tua." },
  { "en": "Gerontolog?", "id": "Ahli lansia." },
  { "en": "Gerontologi?", "id": "Ilmu lansia." },
  { "en": "Geropes?", "id": "Kikis." },
  { "en": "Geros?", "id": "Bunyi." },
  { "en": "Gerset?", "id": "Korset (arkais)." },
  { "en": "Gersik?", "id": "Pasir." },
  { "en": "Gerta?", "id": "Bentak (arkais)." },
  { "en": "Gertak?", "id": "Ancam." },
  { "en": "Geru?", "id": "Mengaum." },
  { "en": "Gerugut?", "id": "Menggigil." },
  { "en": "Geruh?", "id": "Sial." },
  { "en": "Geruit?", "id": "Semut." },
  { "en": "Gerumit?", "id": "Kerumun." },
  { "en": "Gerun?", "id": "Takut." },
  { "en": "Gerunggung?", "id": "Pohon." },
  { "en": "Gerupis?", "id": "Pahat." },
  { "en": "Gerupuk?", "id": "Rapuh." },
  { "en": "Gerus?", "id": "Lumat." },
  { "en": "Gerut?", "id": "Kikis." },
  { "en": "Gerutu?", "id": "Mengomel." },
  { "en": "Gesa?", "id": "Cepat." },
  { "en": "Gesek?", "id": "Gosok." },
  { "en": "Gesel?", "id": "Gesek." },
  { "en": "Geser?", "id": "Pindah." },
  { "en": "Gesit?", "id": "Tangkas." },
  { "en": "Gestikulasi?", "id": "Gerak isyarat." },
  { "en": "Getah?", "id": "Cairan pohon." },
  { "en": "Getap?", "id": "Mudah patah." },
  { "en": "Getar?", "id": "Gemetar." },
  { "en": "Getas?", "id": "Rapuh." },
  { "en": "Getek?", "id": "Rakit." },
  { "en": "Getik?", "id": "Benci." },
  { "en": "Getir?", "id": "Pahit." },
  { "en": "Getis?", "id": "Rapuh." },
  { "en": "Getok?", "id": "Pukul." },
  { "en": "Getol?", "id": "Rajin." },
  { "en": "Getu?", "id": "Tekan (arkais)." },
  { "en": "Getuk?", "id": "Makanan." },
  { "en": "Getun?", "id": "Menyesal (Jawa)." },
  { "en": "Geulis?", "id": "Cantik (Sunda)." },
  { "en": "Gial?", "id": "Sombong (arkais)." },
  { "en": "Giam?", "id": "Jeram." },
  { "en": "Giardia?", "id": "Protozoa." },
  { "en": "Giat?", "id": "Rajin." },
  { "en": "Giau?", "id": "Pohon." },
  { "en": "Gawar?", "id": "Tanda (arkais)." },
  { "en": "Gibah?", "id": "Menggunjing." },
  { "en": "Gibas?", "id": "Domba." },
  { "en": "Giga?", "id": "Awalan (miliar)." },
  { "en": "Gigahertz?", "id": "Satuan frekuensi." },
  { "en": "Gigi?", "id": "Alat kunyah." },
  { "en": "Gigih?", "id": "Tekun." },
  { "en": "Gigil?", "id": "Gemetar." },
  { "en": "Gigir?", "id": "Punggung gunung." },
  { "en": "Gigis?", "id": "Kikis." },
  { "en": "Gigit?", "id": "Kecap." },
  { "en": "Gila?", "id": "Edan." },
  { "en": "Gilang?", "id": "Cemerlang." },
  { "en": "Gilap?", "id": "Kilap." },
  { "en": "Gilas?", "id": "Lumat." },
  { "en": "Gili?", "id": "Pulau kecil (Lombok)." },
  { "en": "Giling?", "id": "Lumat." },
  { "en": "Gilir?", "id": "Ganti." },
  { "en": "Gim?", "id": "Permainan (Inggris)." },
  { "en": "Gimbal?", "id": "Rambut kusut." },
  { "en": "Gimnastik?", "id": "Senam." },
  { "en": "Gin?", "id": "Minuman keras." },
  { "en": "Gincu?", "id": "Lipstik." },
  { "en": "Ginding?", "id": "Gagah (Sunda)." },
  { "en": "Ginjal?", "id": "Organ tubuh." },
  { "en": "Ginjol?", "id": "Benjol (arkais)." },
  { "en": "Gips?", "id": "Bahan pembalut." },
  { "en": "Gir?", "id": "Roda gigi." },
  { "en": "Giral?", "id": "Surat berharga." },
  { "en": "Girang?", "id": "Gembira." },
  { "en": "Girik?", "id": "Surat pajak." },
  { "en": "Giring?", "id": "Halau." },
  { "en": "Giris?", "id": "Ngeri." },
  { "en": "Giro?", "id": "Simpanan bank." },
  { "en": "Gisal?", "id": "Putar (arkais)." },
  { "en": "Gisik?", "id": "Pesisir." },
  { "en": "Gisil?", "id": "Gosok (arkais)." },
  { "en": "Gista?", "id": "Kehendak (arkais)." },
  { "en": "Gitar?", "id": "Alat musik." },
  { "en": "Gites?", "id": "Pijit." },
  { "en": "Gitik?", "id": "Pukul." },
  { "en": "Giuk?", "id": "Cacing." },
  { "en": "Gium?", "id": "Senyum (arkais)." },
  { "en": "Giwang?", "id": "Anting-anting." },
  { "en": "Gizi?", "id": "Nutrisi." },
  { "en": "Glob?", "id": "Bola dunia (arkais)." },
  { "en": "Global?", "id": "Mendunia." },
  { "en": "Globe?", "id": "Bola dunia." },
  { "en": "Globulin?", "id": "Protein." },
  { "en": "Glos?", "id": "Kata sulit." },
  { "en": "Glosarium?", "id": "Daftar istilah." },
  { "en": "Glositis?", "id": "Radang lidah." },
  { "en": "Glotal?", "id": "Bunyi tekak." },
  { "en": "Glotis?", "id": "Celah tekak." },
  { "en": "Glukagon?", "id": "Hormon." },
  { "en": "Glukosa?", "id": "Gula." },
  { "en": "Glukosida?", "id": "Senyawa." },
  { "en": "Gluten?", "id": "Protein gandum." },
  { "en": "Gneis?", "id": "Batuan." },
  { "en": "Goak?", "id": "Burung gagak (arkais)." },
  { "en": "Gob?", "id": "Lubang (tambang)." },
  { "en": "Gobang?", "id": "Uang logam." },
  { "en": "Gobar?", "id": "Suram." },
  { "en": "Gobek?", "id": "Kunyah sirih." },
  { "en": "Gobet?", "id": "Pisau." },
  { "en": "Goblek?", "id": "Gelas (Jawa)." },
  { "en": "Goblok?", "id": "Bodoh (kasar)." },
  { "en": "Gocoh?", "id": "Tinju." },
  { "en": "Goda?", "id": "Rayu." },
  { "en": "Godam?", "id": "Palu besar." },
  { "en": "Godek?", "id": "Kumis cambang." },
  { "en": "Godok?", "id": "Rebus." },
  { "en": "Godot?", "id": "Tekun (arkais)." },
  { "en": "Goel?", "id": "Goyang." },
  { "en": "Gogok?", "id": "Minum langsung." },
  { "en": "Gogol?", "id": "Tanah milik desa." },
  { "en": "Gohm?", "id": "Satuan hambatan." },
  { "en": "Gojek?", "id": "Bercanda." },
  { "en": "Gokart?", "id": "Mobil balap kecil." },
  { "en": "Gol?", "id": "Masuk (bola)." },
  { "en": "Golek?", "id": "Wayang kayu." },
  { "en": "Golem?", "id": "Makhluk buatan (mitos)." },
  { "en": "Goler?", "id": "Berbaring." },
  { "en": "Golf?", "id": "Olahraga." },
  { "en": "Goli?", "id": "Kelereng (arkais)." },
  { "en": "Golok?", "id": "Parang." },
  { "en": "Golong?", "id": "Kelompokkan." },
  { "en": "Golongan?", "id": "Kelompok." },
  { "en": "Gom?", "id": "Penyakit mulut." },
  { "en": "Gombak?", "id": "Jambul." },
  { "en": "Gombal?", "id": "Kain usang." },
  { "en": "Gombang?", "id": "Tempayan besar." },
  { "en": "Gombeng?", "id": "Bentuk (arkais)." },
  { "en": "Gombrang?", "id": "Terlalu besar (pakaian)." },
  { "en": "Gombroh?", "id": "Gombrang." },
  { "en": "Gonad?", "id": "Kelenjar kelamin." },
  { "en": "Goncang?", "id": "Goyang." },
  { "en": "Goncol?", "id": "Pukul (arkais)." },
  { "en": "Gondang?", "id": "Siput." },
  { "en": "Gondok?", "id": "Bengkak leher." },
  { "en": "Gondol?", "id": "Bawa lari." },
  { "en": "Gondola?", "id": "Perahu Venesia." },
  { "en": "Gondorukem?", "id": "Resin pinus." },
  { "en": "Gong?", "id": "Alat musik." },
  { "en": "Gonggong?", "id": "Salak anjing." },
  { "en": "Gongli?", "id": "Pelacur (arkais)." },
  { "en": "Gongseng?", "id": "Sangrai." },
  { "en": "Goni?", "id": "Karung." },
  { "en": "Goniometer?", "id": "Pengukur sudut." },
  { "en": "Gonjong?", "id": "Atap runcing." },
  { "en": "Gono-gini?", "id": "Harta bersama." },
  { "en": "Gonokokus?", "id": "Bakteri." },
  { "en": "Gonore?", "id": "Penyakit kelamin." },
  { "en": "Gontai?", "id": "Lamban." },
  { "en": "Gonyak?", "id": "Kunyah (arkais)." },
  { "en": "Gopoh?", "id": "Tergesa-gesa." },
  { "en": "Gorat?", "id": "Gores (arkais)." },
  { "en": "Gordijn?", "id": "Tirai (Belanda)." },
  { "en": "Gorden?", "id": "Tirai." },
  { "en": "Gordi?", "id": "Tali (arkais)." },
  { "en": "Gorek?", "id": "Korek api." },
  { "en": "Goreng?", "id": "Masak minyak." },
  { "en": "Gores?", "id": "Luka tipis." },
  { "en": "Gorila?", "id": "Kera besar." },
  { "en": "Gorilya?", "id": "Gerilya (arkais)." },
  { "en": "Gorok?", "id": "Sembelih." },
  { "en": "Gosip?", "id": "Kabar angin." },
  { "en": "Gosok?", "id": "Urut." },
  { "en": "Gosong?", "id": "Hangus." },
  { "en": "Got?", "id": "Selokan." },
  { "en": "Gotek?", "id": "Pegang (arkais)." },
  { "en": "Gotong?", "id": "Angkat bersama." },
  { "en": "Gotri?", "id": "Peluru kecil." },
  { "en": "Gotrok?", "id": "Gerobak (Jawa)." },
  { "en": "Goud?", "id": "Emas (Belanda)." },
  { "en": "Goval?", "id": "Bodoh (arkais)." },
  { "en": "Gowok?", "id": "Buah." },
  { "en": "Grad?", "id": "Satuan sudut." },
  { "en": "Grasi?", "id": "Pengampunan." },
  { "en": "Gradasi?", "id": "Tingkatan warna." },
  { "en": "Grader?", "id": "Alat berat." },
  { "en": "Gradien?", "id": "Kemiringan." },
  { "en": "Gradual?", "id": "Bertahap." },
  { "en": "Graf?", "id": "Bagan." },
  { "en": "Grafem?", "id": "Satuan tulisan." },
  { "en": "Grafika?", "id": "Seni cetak." },
  { "en": "Grafik?", "id": "Lukisan garis." },
  { "en": "Grafit?", "id": "Karbon." },
  { "en": "Grafolog?", "id": "Ahli tulisan tangan." },
  { "en": "Grafologi?", "id": "Ilmu tulisan tangan." },
  { "en": "Graham?", "id": "Tepung." },
  { "en": "Gram?", "id": "Satuan berat." },
  { "en": "Gramatika?", "id": "Tata bahasa." },
  { "en": "Gramatikal?", "id": "Sesuai tata bahasa." },
  { "en": "Grambyang?", "id": "Gamelan (Jawa)." },
  { "en": "Gramofon?", "id": "Perekam piringan." },
  { "en": "Granat?", "id": "Bom tangan." },
  { "en": "Granit?", "id": "Batuan." },
  { "en": "Granula?", "id": "Butiran." },
  { "en": "Granulasi?", "id": "Pembentukan butiran." },
  { "en": "Gras?", "id": "Rumput (Belanda)." },
  { "en": "Gratifikasi?", "id": "Pemberian hadiah." },
  { "en": "Gratis?", "id": "Cuma-cuma." },
  { "en": "Gravel?", "id": "Kerikil." },
  { "en": "Graver?", "id": "Alat ukir." },
  { "en": "Gravimeter?", "id": "Pengukur gravitasi." },
  { "en": "Gravir?", "id": "Ukir." },
  { "en": "Gravitasi?", "id": "Daya tarik bumi." },
  { "en": "Grebek?", "id": "Upacara (Jawa)." },
  { "en": "Greges?", "id": "Menggigil (Jawa)." },
  { "en": "Grendel?", "id": "Kunci." },
  { "en": "Greng?", "id": "Bunyi (mesin)." },
  { "en": "Grenjeng?", "id": "Kertas timah." },
  { "en": "Grip?", "id": "Penyakit (Inggris)." },
  { "en": "Gris?", "id": "Abu-abu (Belanda)." },
  { "en": "Grobak?", "id": "Gerobak." },
  { "en": "Grogi?", "id": "Gugup." },
  { "en": "Grond?", "id": "Tanah (Belanda)." },
  { "en": "Gronggang?", "id": "Rongga." },
  { "en": "Gronggong?", "id": "Berongga." },
  { "en": "Gros?", "id": "Lusinan." },
  { "en": "Grosir?", "id": "Pedagang besar." },
  { "en": "Grotes?", "id": "Aneh." },
  { "en": "Grup?", "id": "Kelompok." },
  { "en": "Gua?", "id": "Lubang besar." },
  { "en": "Guah?", "id": "Saya (arkais)." },
  { "en": "Gual?", "id": "Colek (arkais)." },
  { "en": "Guam?", "id": "Sariawan." },
  { "en": "Guano?", "id": "Pupuk kotoran burung." },
  { "en": "Gubah?", "id": "Susun." },
  { "en": "Gubahan?", "id": "Karangan." },
  { "en": "Gubal?", "id": "Kayu lunak." },
  { "en": "Gubang?", "id": "Perahu." },
  { "en": "Gubar?", "id": "Gores (arkais)." },
  { "en": "Gubel?", "id": "Kerja paksa (Belanda)." },
  { "en": "Gubenur?", "id": "Gubernur." },
  { "en": "Gubernemen?", "id": "Pemerintahan (Belanda)." },
  { "en": "Gubernur?", "id": "Kepala provinsi." },
  { "en": "Gubris?", "id": "Peduli." },
  { "en": "Gubuk?", "id": "Pondok." },
  { "en": "Guci?", "id": "Wadah keramik." },
  { "en": "Guda?", "id": "Kuda (arkais)." },
  { "en": "Gudam?", "id": "Gudang." },
  { "en": "Gudang?", "id": "Tempat simpan." },
  { "en": "Gude?", "id": "Talas (arkais)." },
  { "en": "Gudeg?", "id": "Makanan." },
  { "en": "Gudi?", "id": "Mangkuk (arkais)." },
  { "en": "Gudik?", "id": "Kudis." },
  { "en": "Gue?", "id": "Saya (prokem)." },
  { "en": "Gugah?", "id": "Bangunkan." },
  { "en": "Gugat?", "id": "Tuntut." },
  { "en": "Gugatan?", "id": "Tuntutan." },
  { "en": "Gugon?", "id": "Kepercayaan (Jawa)." },
  { "en": "Guguh?", "id": "Pukul (gong)." },
  { "en": "Guguk?", "id": "Bukit kecil." },
  { "en": "Gugup?", "id": "Panik." },
  { "en": "Gugur?", "id": "Jatuh." },
  { "en": "Gugus?", "id": "Kumpulan." },
  { "en": "Gula?", "id": "Zat manis." },
  { "en": "Gulai?", "id": "Masakan." },
  { "en": "Gulali?", "id": "Permen." },
  { "en": "Gulam?", "id": "Budak (Arab)." },
  { "en": "Gulambai?", "id": "Makhluk halus." },
  { "en": "Gulana?", "id": "Sedih." },
  { "en": "Gulat?", "id": "Olahraga." },
  { "en": "Gulden?", "id": "Mata uang (Belanda)." },
  { "en": "Guli?", "id": "Kelereng." },
  { "en": "Guliga?", "id": "Geliga." },
  { "en": "Guling?", "id": "Berguling." },
  { "en": "Gulir?", "id": "Gelinding." },
  { "en": "Gulita?", "id": "Gelap." },
  { "en": "Gulma?", "id": "Rumput liar." },
  { "en": "Gulud?", "id": "Pematang." },
  { "en": "Gulung?", "id": "Lipat." },
  { "en": "Gum?", "id": "Karet (Inggris)." },
  { "en": "Guma?", "id": "Taring (arkais)." },
  { "en": "Gumal?", "id": "Kusut." },
  { "en": "Gumam?", "id": "Bicara pelan." },
  { "en": "Gumbad?", "id": "Kubah." },
  { "en": "Gumbar?", "id": "Sorak (arkais)." },
  { "en": "Gumboro?", "id": "Penyakit unggas." },
  { "en": "Gumbreg?", "id": "Bangkit (Jawa)." },
  { "en": "Gumbuk?", "id": "Bujuk." },
  { "en": "Gumelaran?", "id": "Hamparan (Jawa)." },
  { "en": "Gumpal?", "id": "Kumpulan padat." },
  { "en": "Gumul?", "id": "Bergulat." },
  { "en": "Gumuk?", "id": "Bukit pasir." },
  { "en": "Guna?", "id": "Manfaat." },
  { "en": "Guna-guna?", "id": "Sihir." },
  { "en": "Gundah?", "id": "Resah." },
  { "en": "Gundal?", "id": "Tanda baca." },
  { "en": "Gundala?", "id": "Petir." },
  { "en": "Gundi?", "id": "Biji saga." },
  { "en": "Gundik?", "id": "Selir." },
  { "en": "Gundul?", "id": "Botak." },
  { "en": "Gung?", "id": "Besar (Jawa)." },
  { "en": "Guni?", "id": "Karung." },
  { "en": "Gunjai?", "id": "Burai." },
  { "en": "Gunjing?", "id": "Gosip." },
  { "en": "Guntak?", "id": "Goncang (arkais)." },
  { "en": "Guntang?", "id": "Pelampung." },
  { "en": "Guntil?", "id": "Gunting (arkais)." },
  { "en": "Gunting?", "id": "Alat potong." },
  { "en": "Guntung?", "id": "Pendek." },
  { "en": "Guntur?", "id": "Gemuruh." },
  { "en": "Gunung?", "id": "Bukit besar." },
  { "en": "Gup?", "id": "Bunyi." },
  { "en": "Gurab?", "id": "Kapal Arab." },
  { "en": "Gurah?", "id": "Pembersihan lendir." },
  { "en": "Guram?", "id": "Suram." },
  { "en": "Gurame?", "id": "Ikan." },
  { "en": "Gurat?", "id": "Goresan." },
  { "en": "Gurau?", "id": "Canda." },
  { "en": "Gurdi?", "id": "Bor." },
  { "en": "Guri?", "id": "Wadah (India)." },
  { "en": "Gurih?", "id": "Lezat." },
  { "en": "Gurindam?", "id": "Puisi lama." },
  { "en": "Gurita?", "id": "Hewan laut." },
  { "en": "Gurnadur?", "id": "Gubernur (arkais)." },
  { "en": "Guru?", "id": "Pengajar." },
  { "en": "Gurub?", "id": "Barat (arkais)." },
  { "en": "Guruh?", "id": "Guntur." },
  { "en": "Gurun?", "id": "Padang pasir." }
  { "en": "Gurur?", "id": "Tipuan (Arab)." },
  { "en": "Gus?", "id": "Panggilan putra kiai." },
  { "en": "Gusar?", "id": "Marah." },
  { "en": "Gusi?", "id": "Daging tempat gigi." },
  { "en": "Gusrek?", "id": "Gosok (Jawa)." },
  { "en": "Gusti?", "id": "Tuan (Jawa)." },
  { "en": "Gutasi?", "id": "Pengeluaran air daun." },
  { "en": "Gutik?", "id": "Sentuh (Jawa)." },
  { "en": "Ha?", "id": "Huruf." },
  { "en": "Hab?", "id": "Biji (Arab)." },
  { "en": "Haba?", "id": "Panas (arkais)." },
  { "en": "Habas?", "id": "Etiopia (arkais)." },
  { "en": "Habibi?", "id": "Kekasihku (Arab)." },
  { "en": "Habitat?", "id": "Tempat hidup." },
  { "en": "Habitual?", "id": "Biasa." },
  { "en": "Habituasi?", "id": "Pembiasaan." },
  { "en": "Habitus?", "id": "Perawakan." },
  { "en": "Habluk?", "id": "Tambang (arkais)." },
  { "en": "Hablun?", "id": "Tali (Arab)." },
  { "en": "Had?", "id": "Batas." },
  { "en": "Hadanah?", "id": "Pemeliharaan anak (Islam)." },
  { "en": "Hadap?", "id": "Arah." },
  { "en": "Hadapan?", "id": "Muka." },
  { "en": "Hadas?", "id": "Najis." },
  { "en": "Hadat?", "id": "Baru (Arab)." },
  { "en": "Hadiah?", "id": "Pemberian." },
  { "en": "Hadir?", "id": "Datang." },
  { "en": "Hadirat?", "id": "Para hadirin." },
  { "en": "Hadirin?", "id": "Peserta." },
  { "en": "Hadis?", "id": "Riwayat Nabi." },
  { "en": "Hadron?", "id": "Partikel." },
  { "en": "Hafal?", "id": "Ingat." },
  { "en": "Hafiz?", "id": "Penghafal Al-Quran." },
  { "en": "Hagiografi?", "id": "Riwayat orang suci." },
  { "en": "Hagiologi?", "id": "Ilmu orang suci." },
  { "en": "Hai?", "id": "Sapaan." },
  { "en": "Haid?", "id": "Datang bulan." },
  { "en": "Haik?", "id": "Pakaian Arab." },
  { "en": "Haiking?", "id": "Mendaki gunung." },
  { "en": "Haiku?", "id": "Puisi Jepang." },
  { "en": "Hail?", "id": "Doa (Arab)." },
  { "en": "Hais?", "id": "Celaka (arkais)." },
  { "en": "Hajat?", "id": "Keinginan." },
  { "en": "Hajatan?", "id": "Kenduri." },
  { "en": "Haji?", "id": "Ibadah Islam." },
  { "en": "Hak?", "id": "Wewenang." },
  { "en": "Hakah?", "id": "Teriakan (Maori)." },
  { "en": "Hakam?", "id": "Wasit (Arab)." },
  { "en": "Hakekat?", "id": "Hakikat." },
  { "en": "Hakiki?", "id": "Sebenarnya." },
  { "en": "Hakim?", "id": "Pengadil." },
  { "en": "Hakimah?", "id": "Hakim wanita." },
  { "en": "Hakul?", "id": "Milik." },
  { "en": "Hal?", "id": "Perkara." },
  { "en": "Hala?", "id": "Arah." },
  { "en": "Hela?", "id": "Tarik." },
  { "en": "Helah?", "id": "Tipu." },
  { "en": "Halal?", "id": "Diizinkan (Islam)." },
  { "en": "Halalan?", "id": "Secara halal." },
  { "en": "Halaman?", "id": "Pekarangan." },
  { "en": "Halang?", "id": "Rintang." },
  { "en": "Halangan?", "id": "Rintangan." },
  { "en": "Halau?", "id": "Usir." },
  { "en": "Halba?", "id": "Rempah." },
  { "en": "Haleluya?", "id": "Pujian Tuhan." },
  { "en": "Halia?", "id": "Jahe." },
  { "en": "Halilintar?", "id": "Petir." },
  { "en": "Halim?", "id": "Lembut (Arab)." },
  { "en": "Halimun?", "id": "Kabut." },
  { "en": "Halipan?", "id": "Lipan." },
  { "en": "Halo?", "id": "Sapaan telepon." },
  { "en": "Halobion?", "id": "Organisme air asin." },
  { "en": "Halofili?", "id": "Suka garam." },
  { "en": "Halofit?", "id": "Tumbuhan garam." },
  { "en": "Halogen?", "id": "Unsur kimia." },
  { "en": "Haloten?", "id": "Obat bius." },
  { "en": "Halter?", "id": "Alat angkat beban." },
  { "en": "Halte?", "id": "Pemberhentian bus." },
  { "en": "Haluan?", "id": "Arah depan." },
  { "en": "Halus?", "id": "Lembut." },
  { "en": "Halusinasi?", "id": "Khayalan." },
  { "en": "Halwa?", "id": "Manisan." },
  { "en": "Ham?", "id": "Daging babi asap." },
  { "en": "Hema?", "id": "Darah (awalan)." },
  { "en": "Hama?", "id": "Pengganggu tanaman." },
  { "en": "Hamal?", "id": "Domba (Arab)." },
  { "en": "Hamartoma?", "id": "Tumor jinak." },
  { "en": "Hamba?", "id": "Abdi." },
  { "en": "Hambali?", "id": "Pengikut mazhab." },
  { "en": "Hambalang?", "id": "Nama tempat." },
  { "en": "Hambal?", "id": "Permadani." },
  { "en": "Hambar?", "id": "Tawar." },
  { "en": "Hambat?", "id": "Halang." },
  { "en": "Hambo?", "id": "Saya (Minang)." },
  { "en": "Hambu?", "id": "Tabur (arkais)." },
  { "en": "Hambur?", "id": "Sebar." },
  { "en": "Hamburger?", "id": "Makanan." },
  { "en": "Hamdalah?", "id": "Ucapan syukur." },
  { "en": "Hamid?", "id": "Terpuji (Arab)." },
  { "en": "Hamil?", "id": "Mengandung." },
  { "en": "Hamin?", "id": "Panas (Yahudi)." },
  { "en": "Hampal?", "id": "Ikan." },
  { "en": "Hampalam?", "id": "Mangga." },
  { "en": "Hampar?", "id": "Bentang." },
  { "en": "Hampas?", "id": "Ampas." },
  { "en": "Hampir?", "id": "Nyaris." },
  { "en": "Hamud?", "id": "Asam (Arab)." },
  { "en": "Hamun?", "id": "Caci." },
  { "en": "Hana?", "id": "Tidak ada (Jawa)." },
  { "en": "Hanacaraka?", "id": "Aksara Jawa." },
  { "en": "Hancing?", "id": "Pesing." },
  { "en": "Hancur?", "id": "Luluh." },
  { "en": "Handa?", "id": "Kawan (arkais)." },
  { "en": "Handai?", "id": "Sahabat." },
  { "en": "Handal?", "id": "Tangguh." },
  { "en": "Handam?", "id": "Hukum (arkais)." },
  { "en": "Handaruan?", "id": "Suara gemuruh." },
  { "en": "Handel?", "id": "Gagang (Belanda)." },
  { "en": "Handikap?", "id": "Rintangan." },
  { "en": "Handuk?", "id": "Pengering badan." },
  { "en": "Hang?", "id": "Gelar Melayu." },
  { "en": "Hangar?", "id": "Gudang pesawat." },
  { "en": "Hangat?", "id": "Panas sedang." },
  { "en": "Hanggar?", "id": "Hangar." },
  { "en": "Hangit?", "id": "Bau terbakar." },
  { "en": "Hangus?", "id": "Terbakar." },
  { "en": "Hanif?", "id": "Lurus (iman)." },
  { "en": "Hanjis?", "id": "Bau anyir." },
  { "en": "Hanra?", "id": "Pertahanan rakyat." },
  { "en": "Hansop?", "id": "Pakaian kerja." },
  { "en": "Hantai?", "id": "Lambat (arkais)." },
  { "en": "Hantam?", "id": "Pukul." },
  { "en": "Hantap?", "id": "Pohon." },
  { "en": "Hantar?", "id": "Kirim." },
  { "en": "Hantu?", "id": "Roh jahat." },
  { "en": "Hanya?", "id": "Cuma." },
  { "en": "Hanyir?", "id": "Amis." },
  { "en": "Hanyut?", "id": "Terbawa arus." },
  { "en": "Hap?", "id": "Tangkap (mulut)." },
  { "en": "Hapalan?", "id": "Hafalan." },
  { "en": "Haplo?", "id": "Tunggal (awalan)." },
  { "en": "Haploid?", "id": "Sel tunggal kromosom." },
  { "en": "Haplologi?", "id": "Pelenyapan suku kata." },
  { "en": "Hapus?", "id": "Hilangkan." },
  { "en": "Hara?", "id": "Zat makanan tumbuhan." },
  { "en": "Harak?", "id": "Gerak (arkais)." },
  { "en": "Harakah?", "id": "Gerakan (Arab)." },
  { "en": "Harakat?", "id": "Tanda baca Arab." },
  { "en": "Haram?", "id": "Terlarang." },
  { "en": "Harang?", "id": "Jarang (arkais)." },
  { "en": "Harap?", "id": "Ingin." },
  { "en": "Harapan?", "id": "Asa." },
  { "en": "Harawan?", "id": "Lemah lembut (arkais)." },
  { "en": "Harbi?", "id": "Perang (Arab)." },
  { "en": "Hardik?", "id": "Bentak." },
  { "en": "Harem?", "id": "Tempat selir." },
  { "en": "Harendong?", "id": "Tanaman." },
  { "en": "Harfiah?", "id": "Menurut huruf." },
  { "en": "Harga?", "id": "Nilai." },
  { "en": "Hari?", "id": "Waktu 24 jam." },
  { "en": "Harian?", "id": "Setiap hari." },
  { "en": "Haribaan?", "id": "Pangkuan." },
  { "en": "Harimau?", "id": "Kucing besar." },
  { "en": "Haring?", "id": "Bau pesing." },
  { "en": "Harini?", "id": "Kijang (Sanskerta)." },
  { "en": "Haris?", "id": "Penjaga (Arab)." },
  { "en": "Harisah?", "id": "Makanan (Arab)." },
  { "en": "Harit?", "id": "Sakit perut." },
  { "en": "Harmoni?", "id": "Keselarasan." },
  { "en": "Harmonis?", "id": "Selaras." },
  { "en": "Harmonika?", "id": "Alat musik." },
  { "en": "Harmonium?", "id": "Alat musik." },
  { "en": "Harnet?", "id": "Jala rambut." },
  { "en": "Harpa?", "id": "Alat musik." },
  { "en": "Hart?", "id": "Keras (Belanda)." },
  { "en": "Harta?", "id": "Kekayaan." },
  { "en": "Hartal?", "id": "Pemogokan (India)." },
  { "en": "Haru?", "id": "Sedih." },
  { "en": "Haruan?", "id": "Ikan gabus." },
  { "en": "Harum?", "id": "Wangi." },
  { "en": "Harungguan?", "id": "Rapat (Batak)." },
  { "en": "Harus?", "id": "Wajib." },
  { "en": "Has?", "id": "Daging pilihan." },
  { "en": "Hasab?", "id": "Hitungan (Arab)." },
  { "en": "Hasad?", "id": "Dengki." },
  { "en": "Hasan?", "id": "Baik (Arab)." },
  { "en": "Hasar?", "id": "Rugi (arkais)." },
  { "en": "Hasib?", "id": "Penghitung (Arab)." },
  { "en": "Hasil?", "id": "Buah." },
  { "en": "Hasrat?", "id": "Keinginan." },
  { "en": "Hasta?", "id": "Ukuran panjang." },
  { "en": "Hasud?", "id": "Hasad." },
  { "en": "Hasut?", "id": "Adu domba." },
  { "en": "Hasyiah?", "id": "Catatan pinggir." },
  { "en": "Hati?", "id": "Kalbu." },
  { "en": "Hatif?", "id": "Suara gaib (Arab)." },
  { "en": "Hatta?", "Maka?": "Maka." },
  { "en": "Haud?", "id": "Kolam (Arab)." },
  { "en": "Haukalah?", "id": "Ucapan la haula." },
  { "en": "Hauler?", "id": "Truk pengangkut." },
  { "en": "Haung?", "id": "Raung." },
  { "en": "Haur?", "id": "Bambu." },
  { "en": "Haus?", "id": "Dahaga." },
  { "en": "Haut?", "id": "Ikan besar (Arab)." },
  { "en": "Hawa?", "id": "Udara." },
  { "en": "Hawadis?", "id": "Peristiwa (Arab)." },
  { "en": "Hawar?", "id": "Penyakit tanaman." },
  { "en": "Hawari?", "id": "Sahabat Nabi Isa." },
  { "en": "Hawiah?", "id": "Neraka." },
  { "en": "Hayam?", "id": "Ayam (Sunda)." },
  { "en": "Hayat?", "id": "Hidup." },
  { "en": "Hayati?", "id": "Berkaitan hidup." },
  { "en": "He?", "id": "Seruan." },
  { "en": "Hebat?", "id": "Luar biasa." },
  { "en": "Heboh?", "id": "Geger." },
  { "en": "Heder?", "id": "Kepala (Belanda)." },
  { "en": "Hédon?", "id": "Penganut hedonisme." },
  { "en": "Hedonis?", "id": "Pemuja kesenangan." },
  { "en": "Hedonisme?", "id": "Paham kesenangan." },
  { "en": "Heksadesimal?", "id": "Basis enambelas." },
  { "en": "Heksagon?", "id": "Segienam." },
  { "en": "Heksagonal?", "id": "Bersudut enam." },
  { "en": "Heksameter?", "id": "Sajak enam matra." },
  { "en": "Heksana?", "id": "Senyawa hidrokarbon." },
  { "en": "Heksapoda?", "id": "Serangga." },
  { "en": "Hekse?", "id": "Penyihir (Belanda)." },
  { "en": "Hektar?", "id": "Satuan luas." },
  { "en": "Hektare?", "id": "Hektar." },
  { "en": "Hektograf?", "id": "Alat salin." },
  { "en": "Hektogram?", "id": "Satuan berat." },
  { "en": "Hektoliter?", "id": "Satuan volume." },
  { "en": "Hektometer?", "id": "Satuan panjang." },
  { "en": "Hela?", "id": "Seret." },
  { "en": "Helai?", "id": "Lembar." },
  { "en": "Heli?", "id": "Helikopter (singkatan)." },
  { "en": "Helian?", "id": "Bunga matahari (arkais)." },
  { "en": "Helikal?", "id": "Berbentuk spiral." },
  { "en": "Helikopter?", "id": "Pesawat." },
  { "en": "Helikultura?", "id": "Budidaya siput." },
  { "en": "Heliofit?", "id": "Tumbuhan matahari." },
  { "en": "Heliograf?", "id": "Perekam matahari." },
  { "en": "Heliogram?", "id": "Berita surya." },
  { "en": "Heliometer?", "id": "Pengukur diameter matahari." },
  { "en": "Heliosentris?", "id": "Berpusat matahari." },
  { "en": "Helioterapi?", "id": "Terapi matahari." },
  { "en": "Heliotrop?", "id": "Bunga." },
  { "en": "Heliotropisme?", "id": "Gerak ke matahari." },
  { "en": "Helium?", "id": "Unsur gas." },
  { "en": "Helm?", "id": "Pelindung kepala." },
  { "en": "Helmintologi?", "id": "Ilmu cacing." },
  { "en": "Helot?", "id": "Budak (Yunani kuno)." },
  { "en": "Hem?", "id": "Keliman." },
  { "en": "Hema?", "id": "Darah." },
  { "en": "Hemat?", "id": "Irit." },
  { "en": "Hematit?", "id": "Mineral besi." },
  { "en": "Hematofobia?", "id": "Takut darah." },
  { "en": "Hematologi?", "id": "Ilmu darah." },
  { "en": "Hematoma?", "id": "Memar." },
  { "en": "Hematuria?", "id": "Kencing darah." },
  { "en": "Hemerokalis?", "id": "Bunga lili." },
  { "en": "Hemilplegia?", "id": "Lumpuh separuh badan." },
  { "en": "Hemiptera?", "id": "Ordo serangga." },
  { "en": "Hemisfer?", "id": "Belahan bumi." },
  { "en": "Hemodialisis?", "id": "Cuci darah." },
  { "en": "Hemofilia?", "id": "Penyakit darah." },
  { "en": "Hemoglobin?", "id": "Protein darah." },
  { "en": "Hemolisis?", "id": "Pecah sel darah." },
  { "en": "Hemoptisis?", "id": "Batuk darah." },
  { "en": "Hemoragi?", "id": "Pendarahan." },
  { "en": "Hemoroid?", "id": "Wasir." },
  { "en": "Hemostatik?", "id": "Penghenti darah." },
  { "en": "Hempap?", "id": "Tindih." },
  { "en": "Hempas?", "id": "Banting." },
  { "en": "Hempt?", "id": "Baju (Belanda)." },
  { "en": "Hena?", "id": "Inai." },
  { "en": "Hendak?", "id": "Akan." },
  { "en": "Hendel?", "id": "Gagang." },
  { "en": "Hendiad?", "id": "Gaya bahasa." },
  { "en": "Heng?", "id": "Bau (arkais)." },
  { "en": "Hening?", "id": "Sunyi." },
  { "en": "Henoteisme?", "id": "Pemujaan satu dewa." },
  { "en": "Hentam?", "id": "Hantam." },
  { "en": "Henti?", "id": "Berhenti." },
  { "en": "Hepatika?", "id": "Lumut hati." },
  { "en": "Hepatitis?", "id": "Radang hati." },
  { "en": "Hepatoma?", "id": "Kanker hati." },
  { "en": "Hepta?", "id": "Tujuh (awalan)." },
  { "en": "Heptagon?", "id": "Segitujuh." },
  { "en": "Heptameter?", "id": "Sajak tujuh matra." },
  { "en": "Heptana?", "id": "Senyawa hidrokarbon." },
  { "en": "Heraldik?", "id": "Ilmu lambang." },
  { "en": "Herba?", "id": "Tumbuhan." },
  { "en": "Herbarium?", "id": "Koleksi tumbuhan kering." },
  { "en": "Herbisida?", "id": "Pembasmi gulma." },
  { "en": "Herbivor?", "id": "Pemakan tumbuhan." },
  { "en": "Hercules?", "id": "Tokoh mitologi." },
  { "en": "Herd?", "id": "Kawanan (Inggris)." },
  { "en": "Hereditas?", "id": "Pewarisan." },
  { "en": "Heresi?", "id": "Ajaran sesat." },
  { "en": "Hermafrodit?", "id": "Berkelamin ganda." },
  { "en": "Hermeneutika?", "id": "Ilmu tafsir." },
  { "en": "Hermetis?", "id": "Kedap." },
  { "en": "Hernia?", "id": "Turun berok." },
  { "en": "Hero?", "id": "Pahlawan." },
  { "en": "Heroik?", "id": "Kepahlawanan." },
  { "en": "Heroin?", "id": "Narkotika." },
  { "en": "Heroisme?", "id": "Sikap pahlawan." },
  { "en": "Heron?", "id": "Burung." },
  { "en": "Herpes?", "id": "Penyakit kulit." },
  { "en": "Herpetolog?", "id": "Ahli reptil." },
  { "en": "Herpetologi?", "id": "Ilmu reptil." },
  { "en": "Hertz?", "id": "Satuan frekuensi." },
  { "en": "Hes?", "id": "Pagar (Belanda)." },
  { "en": "Hesitasi?", "id": "Keraguan." },
  { "en": "Hesperidin?", "id": "Senyawa." },
  { "en": "Hetero?", "id": "Berbeda (awalan)." },
  { "en": "Heterodoks?", "id": "Menyimpang." },
  { "en": "Heterofil?", "id": "Beda daun." },
  { "en": "Heterofit?", "id": "Tumbuhan heterotrof." },
  { "en": "Heterogami?", "id": "Perkawinan beda." },
  { "en": "Heterogen?", "id": "Beraneka." },
  { "en": "Heterogenitas?", "id": "Keanekaragaman." },
  { "en": "Heterograf?", "id": "Beda tulisan." },
  { "en": "Heteroklit?", "id": "Tidak beraturan." },
  { "en": "Heteronim?", "id": "Beda nama." },
  { "en": "Heteronimi?", "id": "Relasi heteronim." },
  { "en": "Heteroseksual?", "id": "Suka lawan jenis." },
  { "en": "Heterosis?", "id": "Keunggulan hibrida." },
  { "en": "Heterotrof?", "id": "Organisme non-fotosintesis." },
  { "en": "Heurestik?", "id": "Metode penemuan." },
  { "en": "Heurisris?", "id": "Heurestik." },
  { "en": "Hewan?", "id": "Binatang." },
  { "en": "Hewani?", "id": "Bersifat hewan." },
  { "en": "Heks?", "id": "Sihir (Belanda)." },
  { "en": "Heksaklorofena?", "id": "Antiseptik." },
  { "en": "Heksaklorida?", "id": "Senyawa." },
  { "en": "Heksoda?", "id": "Tabung elektron." },
  { "en": "Hialin?", "id": "Bening." },
  { "en": "Hialit?", "id": "Mineral." },
  { "en": "Hialoplasma?", "id": "Bagian sel." },
  { "en": "Hibrida?", "id": "Hasil persilangan." },
  { "en": "Hibridisasi?", "id": "Proses persilangan." },
  { "en": "Hidang?", "id": "Sajikan." },
  { "en": "Hidangan?", "id": "Sajian." },
  { "en": "Hidayah?", "id": "Petunjuk." },
  { "en": "Hidayat?", "id": "Hidayah." },
  { "en": "Hidrasi?", "id": "Pengairan." },
  { "en": "Hidraulik?", "id": "Tenaga air." },
  { "en": "Hidraulika?", "id": "Ilmu mekanika fluida." },
  { "en": "Hidrida?", "id": "Senyawa hidrogen." },
  { "en": "Hidrodinamika?", "id": "Ilmu gerak fluida." },
  { "en": "Hidrofit?", "id": "Tumbuhan air." },
  { "en": "Hidrofoil?", "id": "Sayap air." },
  { "en": "Hidrogen?", "id": "Unsur kimia." },
  { "en": "Hidrogenasi?", "id": "Proses penambahan hidrogen." },
  { "en": "Hidrogeologi?", "id": "Ilmu air tanah." },
  { "en": "Hidrograf?", "id": "Pencatat air." },
  { "en": "Hidrografi?", "id": "Pemetaan air." },
  { "en": "Hidrokarbon?", "id": "Senyawa organik." },
  { "en": "Hidroklorida?", "id": "Asam klorida." },
  { "en": "Hidrokortison?", "id": "Hormon." },
  { "en": "Hidroksida?", "id": "Senyawa basa." },
  { "en": "Hidroksil?", "id": "Gugus kimia." },
  { "en": "Hidrolisis?", "id": "Penguraian air." },
  { "en": "Hidrolog?", "id": "Ahli air." },
  { "en": "Hidrologi?", "id": "Ilmu air." },
  { "en": "Hidrometeorologi?", "id": "Ilmu cuaca air." },
  { "en": "Hidrometer?", "id": "Pengukur massa jenis." },
  { "en": "Hidrometri?", "id": "Pengukuran air." },
  { "en": "Hidronan?", "id": "Pesawat air." },
  { "en": "Hidronan?", "id": "Pesawat terbang air." },
  { "en": "Hidronimi?", "id": "Penamaan perairan." },
  { "en": "Hidropati?", "id": "Terapi air." },
  { "en": "Hidroponik?", "id": "Tanam tanpa tanah." },
  { "en": "Hidrops?", "id": "Penumpukan cairan." },
  { "en": "Hidrosfer?", "id": "Lapisan air bumi." },
  { "en": "Hidrostatika?", "id": "Ilmu fluida diam." },
  { "en": "Hidroterapi?", "id": "Terapi air." },
  { "en": "Hidrotermal?", "id": "Panas air." },
  { "en": "Hiena?", "id": "Hewan pemakan bangkai." },
  { "en": "Hierarki?", "id": "Tingkatan." },
  { "en": "Hierarkis?", "id": "Bertingkat." },
  { "en": "Hieroglif?", "id": "Tulisan Mesir kuno." },
  { "en": "Higiene?", "id": "Kebersihan." },
  { "en": "Higienis?", "id": "Bersih." },
  { "en": "Higrograf?", "id": "Pencatat kelembapan." },
  { "en": "Higrometer?", "id": "Pengukur kelembapan." },
  { "en": "Higroskopis?", "id": "Menyerap air." },
  { "en": "Hijab?", "id": "Penutup." },
  { "en": "Hijau?", "id": "Warna daun." },
  { "en": "Hijauan?", "id": "Tumbuhan hijau." },
  { "en": "Hijrah?", "id": "Pindah." },
  { "en": "Hijriah?", "id": "Tahun Islam." },
  { "en": "Hikayat?", "id": "Kisah." },
  { "en": "Hikmah?", "id": "Kebijaksanaan." },
  { "en": "Hilal?", "id": "Bulan sabit muda." },
  { "en": "Hilang?", "id": "Lenyap." },
  { "en": "Hilap?", "id": "Lupa (arkais)." },
  { "en": "Hilir?", "id": "Bagian bawah sungai." },
  { "en": "Himanga?", "id": "Puncak gunung es." },
  { "en": "Himen?", "id": "Selaput dara." },
  { "en": "Himpit?", "id": "Jepit." },
  { "en": "Himpun?", "id": "Kumpul." },
  { "en": "Hina?", "id": "Nista." },
  { "en": "Hinap?", "id": "Menginap (arkais)." },
  { "en": "Hindi?", "id": "Bahasa India." },
  { "en": "Hindu?", "id": "Agama." },
  { "en": "Hinduisme?", "id": "Ajaran Hindu." },
  { "en": "Hingar?", "id": "Bising." },
  { "en": "Hingga?", "id": "Sampai." },
  { "en": "Hinggap?", "id": "Bertengger." },
  { "en": "Hio?", "id": "Dupa Tionghoa." },
  { "en": "Hipantium?", "id": "Dasar bunga." },
  { "en": "Hiper?", "id": "Berlebihan (awalan)." },
  { "en": "Hiperaktif?", "id": "Sangat aktif." },
  { "en": "Hiperbarik?", "id": "Tekanan tinggi." },
  { "en": "Hiperbol?", "id": "Kurva matematika." },
  { "en": "Hiperbola?", "id": "Gaya bahasa melebihkan." },
  { "en": "Hiperbolis?", "id": "Berlebihan." },
  { "en": "Hiperemia?", "id": "Kelebihan darah." },
  { "en": "Hiperestesia?", "id": "Sangat peka." },
  { "en": "Hipergamet?", "id": "Sel kelamin besar." },
  { "en": "Hiperkalsemia?", "id": "Kalsium darah tinggi." },
  { "en": "Hiperkinesis?", "id": "Gerak berlebih." },
  { "en": "Hiperkorek?", "id": "Pembenaran berlebih." },
  { "en": "Hiperlipidemia?", "id": "Lemak darah tinggi." },
  { "en": "Hipermetropia?", "id": "Rabun dekat." },
  { "en": "Hipernim?", "id": "Kata umum." },
  { "en": "Hiperon?", "id": "Partikel." },
  { "en": "Hiperplasia?", "id": "Pertumbuhan jaringan berlebih." },
  { "en": "Hiperpireksia?", "id": "Demam sangat tinggi." },
  { "en": "Hiperseksual?", "id": "Nafsu seks berlebih." },
  { "en": "Hipertensi?", "id": "Tekanan darah tinggi." },
  { "en": "Hipertiroidisme?", "id": "Kelebihan hormon tiroid." },
  { "en": "Hipertonik?", "id": "Berkonsentrasi tinggi." },
  { "en": "Hipertrofi?", "id": "Pembesaran organ." },
  { "en": "Hipnogenik?", "id": "Menyebabkan tidur." },
  { "en": "Hipnosis?", "id": "Keadaan terhipnotis." },
  { "en": "Hipnoterapi?", "id": "Terapi hipnosis." },
  { "en": "Hipnotis?", "id": "Menidurkan." },
  { "en": "Hipnotisme?", "id": "Ilmu hipnosis." },
  { "en": "Hipo?", "id": "Kurang (awalan)." },
  { "en": "Hipoderma?", "id": "Jaringan bawah kulit." },
  { "en": "Hipodermis?", "id": "Lapisan bawah kulit." },
  { "en": "Hipodrom?", "id": "Arena pacuan kuda." },
  { "en": "Hipofisis?", "id": "Kelenjar." },
  { "en": "Hipoglikemia?", "id": "Gula darah rendah." },
  { "en": "Hipokalsemia?", "id": "Kalsium darah rendah." },
  { "en": "Hipokondria?", "id": "Cemas penyakit." },
  { "en": "Hipokotil?", "id": "Bagian kecambah." },
  { "en": "Hipokrisi?", "id": "Kemunafikan." },
  { "en": "Hipokrit?", "id": "Munafik." },
  { "en": "Hipokromia?", "id": "Kurang pigmen." },
  { "en": "Hipolimnion?", "id": "Lapisan bawah danau." },
  { "en": "Hipomastia?", "id": "Payudara kecil." },
  { "en": "Hiponim?", "id": "Kata khusus." },
  { "en": "Hipopituitarisme?", "id": "Kekurangan hormon." },
  { "en": "Hipoplasia?", "id": "Pertumbuhan kurang." },
  { "en": "Hiposentrum?", "id": "Pusat gempa dalam." },
  { "en": "Hipotaksis?", "id": "Hubungan kalimat bertingkat." },
  { "en": "Hipotek?", "id": "Jaminan utang tanah." },
  { "en": "Hipotensi?", "id": "Tekanan darah rendah." },
  { "en": "Hipotenusa?", "id": "Sisi miring segitiga." },
  { "en": "Hipotermia?", "id": "Suhu tubuh rendah." },
  { "en": "Hipotesis?", "id": "Dugaan awal." },
  { "en": "Hipotetik?", "id": "Bersifat dugaan." },
  { "en": "Hipotiroidisme?", "id": "Kekurangan hormon tiroid." },
  { "en": "Hipotonik?", "id": "Berkonsentrasi rendah." },
  { "en": "Hipsometer?", "id": "Pengukur ketinggian." },
  { "en": "Hipsometri?", "id": "Pengukuran ketinggian." },
  { "en": "Hirap?", "id": "Hilang." },
  { "en": "Hirarki?", "id": "Hierarki." },
  { "en": "Hirau?", "id": "Peduli." },
  { "en": "Hirsutisme?", "id": "Bulu berlebih (wanita)." },
  { "en": "Hirudin?", "id": "Zat lintah." },
  { "en": "Hiruk?", "id": "Ribut." },
  { "en": "His?", "id": "Kontraksi rahim." },
  { "en": "Hisab?", "id": "Hitungan." },
  { "en": "Hisap?", "id": "Sedot." },
  { "en": "Histeria?", "id": "Luapan emosi." },
  { "en": "Histeris?", "id": "Tidak terkendali." },
  { "en": "Histidina?", "id": "Asam amino." },
  { "en": "Histiosit?", "id": "Sel jaringan." },
  { "en": "Histokimia?", "id": "Kimia jaringan." },
  { "en": "Histologi?", "id": "Ilmu jaringan." },
  { "en": "Histon?", "id": "Protein." },
  { "en": "Histopatologi?", "id": "Ilmu penyakit jaringan." },
  { "en": "Histori?", "id": "Sejarah." },
  { "en": "Historikus?", "id": "Ahli sejarah." },
  { "en": "Historis?", "id": "Bersejarah." },
  { "en": "Historiografi?", "id": "Penulisan sejarah." },
  { "en": "Hit?", "id": "Populer (Inggris)." },
  { "en": "Hitam?", "id": "Warna gelap." },
  { "en": "Hitung?", "id": "Kalkulasi." },
  { "en": "Hobi?", "id": "Kegemaran." },
  { "en": "Hobnob?", "id": "Bergaul (Inggris)." },
  { "en": "Hodad?", "id": "Papan selancar (slang)." },
  { "en": "Hodah?", "id": "Kursi gajah." },
  { "en": "Hoki?", "id": "Keberuntungan." },
  { "en": "Hol?", "id": "Aula." },
  { "en": "Holisme?", "id": "Paham menyeluruh." },
  { "en": "Holistik?", "id": "Menyeluruh." },
  { "en": "Holmium?", "id": "Unsur kimia." },
  { "en": "Holobentos?", "id": "Organisme dasar laut." },
  { "en": "Holoenzim?", "id": "Enzim aktif." },
  { "en": "Holofit?", "id": "Tumbuhan mandiri." },
  { "en": "Holofrasis?", "id": "Kalimat satu kata." },
  { "en": "Hologamet?", "id": "Sel kelamin serupa." },
  { "en": "Holograf?", "id": "Gambar tiga dimensi." },
  { "en": "Holografi?", "id": "Teknik holograf." },
  { "en": "Holokaus?", "id": "Pembantaian." },
  { "en": "Holokrin?", "id": "Kelenjar." },
  { "en": "Holosen?", "id": "Kala geologi." },
  { "en": "Holotipe?", "id": "Spesimen acuan." },
  { "en": "Holozoik?", "id": "Makan organisme lain." },
  { "en": "Homeostasis?", "id": "Keseimbangan tubuh." },
  { "en": "Homer?", "id": "Penyair Yunani." },
  { "en": "Hominid?", "id": "Keluarga kera." },
  { "en": "Hominoid?", "id": "Mirip manusia." },
  { "en": "Homofon?", "id": "Sama bunyi beda makna." },
  { "en": "Homofora?", "id": "Penunjukan dalam teks." },
  { "en": "Homogami?", "id": "Kawin sesuku." },
  { "en": "Homogen?", "id": "Seragam." },
  { "en": "Homogenitas?", "id": "Keseragaman." },
  { "en": "Homograf?", "id": "Sama tulisan beda makna." },
  { "en": "Homoiotermal?", "id": "Berdarah panas." },
  { "en": "Homolog?", "id": "Sama asal." },
  { "en": "Homologi?", "id": "Persamaan asal." },
  { "en": "Homonim?", "id": "Sama nama beda makna." },
  { "en": "Homonimi?", "id": "Relasi homonim." },
  { "en": "Homorgan?", "id": "Sama organ." },
  { "en": "Homoseksual?", "id": "Suka sesama jenis." },
  { "en": "Homosfer?", "id": "Lapisan atmosfer." },
  { "en": "Homoterm?", "id": "Berdarah panas." },
  { "en": "Hon?", "id": "Rusa (Belanda)." },
  { "en": "Honai?", "id": "Rumah adat Papua." },
  { "en": "Hond?", "id": "Anjing (Belanda)." },
  { "en": "Honorer?", "id": "Tidak tetap (pegawai)." },
  { "en": "Honorarium?", "id": "Upah." },
  { "en": "Hop?", "id": "Tanaman." },
  { "en": "Hopagen?", "id": "Vitamin." },
  { "en": "Hopbiro?", "id": "Kantor pusat (Belanda)." },
  { "en": "Hor?", "id": "Kain kasa (Belanda)." },
  { "en": "Horda?", "id": "Gerombolan (arkais)." },
  { "en": "Hordeolum?", "id": "Bintitan." },
  { "en": "Horen?", "id": "Klakson (Belanda)." },
  { "en": "Horizon?", "id": "Cakrawala." },
  { "en": "Horisontal?", "id": "Mendatar." },
  { "en": "Hormat?", "id": "Takzim." },
  { "en": "Hormon?", "id": "Zat kimia tubuh." },
  { "en": "Horor?", "id": "Kengerian." },
  { "en": "Horoskop?", "id": "Ramalan bintang." },
  { "en": "Hortikultura?", "id": "Ilmu tanaman kebun." },
  { "en": "Hos?", "id": "Selang (Belanda)." },
  { "en": "Hosana?", "id": "Seruan pujian." },
  { "en": "Hospital?", "id": "Rumah sakit." },
  { "en": "Hospitalitas?", "id": "Keramahtamahan." },
  { "en": "Hostes?", "id": "Pramuria." },
  { "en": "Hostia?", "id": "Roti sakramen." },
  { "en": "Hot?", "id": "Panas (Inggris)." },
  { "en": "Hotel?", "id": "Penginapan." },
  { "en": "Howitzer?", "id": "Meriam." },
  { "en": "Hrifnia?", "id": "Mata uang Ukraina." },
  { "en": "Hu?", "id": "Dia (Allah)." },
  { "en": "Hua?", "id": "Bunga (Tionghoa)." },
  { "en": "Hubar?", "id": "Burung." },
  { "en": "Hububan?", "id": "Ubun-ubun." },
  { "en": "Hubung?", "id": "Sambung." },
  { "en": "Hubungan?", "id": "Relasi." },
  { "en": "Huda?", "id": "Petunjuk (Arab)." },
  { "en": "Hudai?", "id": "Sapaan (arkais)." },
  { "en": "Hudud?", "id": "Hukuman Islam." },
  { "en": "Hui?", "id": "Perkumpulan (Tionghoa)." },
  { "en": "Hujah?", "id": "Alasan." },
  { "en": "Hujaj?", "id": "Jemaah haji." },
  { "en": "Hujan?", "id": "Air turun dari langit." },
  { "en": "Hujat?", "id": "Caci." },
  { "en": "Huji?", "id": "Tuduhan (arkais)." },
  { "en": "Hujin?", "id": "Hujan (ejaan lama)." },
  { "en": "Hujung?", "id": "Ujung (ejaan lama)." },
  { "en": "Hukah?", "id": "Pipa rokok Arab." },
  { "en": "Hukum?", "id": "Aturan." },
  { "en": "Hula-hula?", "id": "Tarian Hawaii." },
  { "en": "Hulanda?", "id": "Belanda (arkais)." },
  { "en": "Hulbah?", "id": "Halba." },
  { "en": "Huler?", "id": "Penggilingan." },
  { "en": "Hulu?", "id": "Bagian atas sungai." },
  { "en": "Hulubalang?", "id": "Panglima." },
  { "en": "Hulul?", "id": "Inkarnasi (tasawuf)." },
  { "en": "Hulur?", "id": "Ulur." },
  { "en": "Human?", "id": "Manusia (Inggris)." },
  { "en": "Humani?", "id": "Manusiawi." },
  { "en": "Humaniora?", "id": "Ilmu kemanusiaan." },
  { "en": "Humanis?", "id": "Penganut humanisme." },
  { "en": "Humanisme?", "id": "Paham kemanusiaan." },
  { "en": "Humanitas?", "id": "Kemanusiaan." },
  { "en": "Humaniter?", "id": "Bersifat kemanusiaan." },
  { "en": "Humas?", "id": "Hubungan masyarakat." },
  { "en": "Humbalang?", "id": "Lempar (arkais)." },
  { "en": "Humbang?", "id": "Tanah lapang." },
  { "en": "Humektan?", "id": "Pelembap." },
  { "en": "Humerus?", "id": "Tulang lengan atas." },
  { "en": "Humid?", "id": "Lembap." },
  { "en": "Humidifikasi?", "id": "Pelembapan." },
  { "en": "Humiditas?", "id": "Kelembapan." },
  { "en": "Humidor?", "id": "Kotak tembakau." },
  { "en": "Humor?", "id": "Jenaka." },
  { "en": "Humoris?", "id": "Pelawak." },
  { "en": "Humus?", "id": "Tanah subur." },
  { "en": "Hun?", "id": "Suku bangsa." },
  { "en": "Huncue?", "id": "Tepung." },
  { "en": "Hundam?", "id": "Tusuk (arkais)." },
  { "en": "Huni?", "id": "Tempati." },
  { "en": "Hunian?", "id": "Tempat tinggal." },
  { "en": "Hunjam?", "id": "Tancap." },
  { "en": "Hunkue?", "id": "Huncue." },
  { "en": "Huns?", "id": "Tutup (arkais)." },
  { "en": "Hurah?", "id": "Sorak." },
  { "en": "Hura-hura?", "id": "Pesta pora." },
  { "en": "Huriah?", "id": "Kemerdekaan (Arab)." },
  { "en": "Hurikan?", "id": "Badai." },
  { "en": "Huruf?", "id": "Aksara." },
  { "en": "Hus?", "id": "Diam (seruan)." },
  { "en": "Hutan?", "id": "Rimba." },
  { "en": "Hutang?", "id": "Utang." },
  { "en": "Huyung?", "id": "Goyang." },
  { "en": "Ia?", "id": "Dia." },
  { "en": "Iadat?", "id": "Adat (arkais)." },
  { "en": "Ialah?", "id": "Adalah." },
  { "en": "Ialur?", "id": "Sungai (arkais)." },
  { "en": "Iambus?", "id": "Matra sajak." },
  { "en": "Ian?", "id": "Dia (arkais)." },
  { "en": "Iar?", "id": "Cair (arkais)." },
  { "en": "Iba?", "id": "Kasihan." },
  { "en": "Ibadah?", "id": "Pemujaan." },
  { "en": "Ibadat?", "id": "Ibadah." },
  { "en": "Ibam?", "id": "Lemas (arkais)." },
  { "en": "Iban?", "id": "Suku Dayak." },
  { "en": "Ibas?", "id": "Potong (arkais)." },
  { "en": "Ibau?", "id": "Bau (arkais)." },
  { "en": "Iberah?", "id": "Pelajaran." },
  { "en": "Ibidem?", "id": "Di tempat sama (Latin)." },
  { "en": "Ibil?", "id": "Iblis (arkais)." },
  { "en": "Ibis?", "id": "Burung." },
  { "en": "Iblah?", "id": "Tidak peduli (Arab)." },
  { "en": "Iblis?", "id": "Setan." },
  { "en": "Ibni?", "id": "Anak laki-laki (Arab)." },
  { "en": "Ibnu?", "id": "Ibni." },
  { "en": "Ibo?", "id": "Suku Nigeria." },
  { "en": "Ibrahim?", "id": "Nabi." },
  { "en": "Ibrani?", "id": "Bahasa." },
  { "en": "Ibrat?", "id": "Pelajaran." },
  { "en": "Ibu?", "id": "Bunda." },
  { "en": "Ibuk?", "id": "Ibu (arkais)." },
  { "en": "Ibul?", "id": "Palma." },
  { "en": "Ibun?", "id": "Ibu (arkais)." },
  { "en": "Ibunda?", "id": "Ibu (hormat)." },
  { "en": "Ibus?", "id": "Palma." },
  { "en": "Ici?", "id": "Ini (arkais)." },
  { "en": "Icip?", "id": "Cicip." },
  { "en": "Ide?", "id": "Gagasan." },
  { "en": "Ideal?", "id": "Sempurna." },
  { "en": "Idealis?", "id": "Penganut idealisme." },
  { "en": "Idealisme?", "id": "Paham cita-cita." },
  { "en": "Idem?", "id": "Sama." },
  { "en": "Identifikasi?", "id": "Pengenalan." },
  { "en": "Identik?", "id": "Sama persis." },
  { "en": "Identitas?", "id": "Jati diri." },
  { "en": "Ideofon?", "id": "Kata tiruan bunyi." },
  { "en": "Ideograf?", "id": "Simbol ide." },
  { "en": "Ideografi?", "id": "Tulisan simbol." },
  { "en": "Ideogram?", "id": "Ideograf." },
  { "en": "Ideolog?", "id": "Ahli ideologi." },
  { "en": "Ideologi?", "id": "Paham." },
  { "en": "Ideomotor?", "id": "Gerak tak sadar." },
  { "en": "Ideoplas?", "id": "Sel." },
  { "en": "Idi?", "id": "Zat (arkais)." },
  { "en": "Idil?", "id": "Puisi pedesaan." },
  { "en": "Idiolek?", "id": "Gaya bahasa pribadi." },
  { "en": "Idiom?", "id": "Ungkapan khas." },
  { "en": "Idiomatis?", "id": "Bersifat idiom." },
  { "en": "Idiomorf?", "id": "Bentuk kristal." },
  { "en": "Idiosinkrasi?", "id": "Sifat khas." },
  { "en": "Idiot?", "id": "Dungu." },
  { "en": "Idola?", "id": "Pujaan." },
  { "en": "Idrak?", "id": "Pemahaman (Arab)." },
  { "en": "Idrisi?", "id": "Pohon (arkais)." },
  { "en": "Iduladha?", "id": "Hari raya kurban." },
  { "en": "Idulfitri?", "id": "Hari raya Lebaran." },
  { "en": "If?", "id": "Jika (Inggris)." },
  { "en": "Ifrit?", "id": "Jin jahat." },
  { "en": "Iftitah?", "id": "Doa pembuka salat." },
  { "en": "Iga?", "id": "Tulang rusuk." },
  { "en": "Igah?", "id": "Sakit (arkais)." },
  { "en": "Igal?", "id": "Tari." },
  { "en": "Igama?", "id": "Agama (arkais)." },
  { "en": "Igap?", "id": "Sengal (arkais)." },
  { "en": "Igau?", "id": "Mengigau." },
  { "en": "Igauan?", "id": "Ucapan tak sadar." },
  { "en": "Igla?", "id": "Rudak." },
  { "en": "Iglo?", "id": "Rumah es Eskimo." },
  { "en": "Ih?", "id": "Seruan." },
  { "en": "Ihdad?", "id": "Masa berkabung (Islam)." },
  { "en": "Ihram?", "id": "Pakaian haji." },
  { "en": "Ihsan?", "id": "Kebaikan (Arab)." },
  { "en": "Ihsanat?", "id": "Amal baik." },
  { "en": "Ihtifal?", "id": "Perayaan (Arab)." },
  { "en": "Ihtikar?", "id": "Penimbunan barang." },
  { "en": "Ihtilam?", "id": "Mimpi basah." },
  { "en": "Ihtimal?", "id": "Kemungkinan (Arab)." },
  { "en": "Ihwal?", "id": "Perihal." },
  { "en": "Ija?", "id": "Kain sarung (Aceh)." },
  { "en": "Ijab?", "id": "Ucapan serah terima." },
  { "en": "Ijabah?", "id": "Perkenan doa." },
  { "en": "Ijal?", "id": "Tunda (arkais)." },
  { "en": "Ijazah?", "id": "Surat tanda lulus." },
  { "en": "Ijeman?", "id": "Tiru (Jawa)." },
  { "en": "Ijil?", "id": "Injil (arkais)." },
  { "en": "Ijin?", "id": "Izin (tidak baku)." },
  { "en": "Ijmak?", "id": "Kesepakatan ulama." },
  { "en": "Ijon?", "id": "Beli sebelum panen." },
  { "en": "Ijtihad?", "id": "Upaya sungguh-sungguh." },
  { "en": "Ijuk?", "id": "Serat enau." },
  { "en": "Ikal?", "id": "Keriting." },
  { "en": "Ikamat?", "id": "Seruan salat kedua." },
  { "en": "Ikan?", "id": "Hewan air." },
  { "en": "Ikat?", "id": "Simpul." },
  { "en": "Ikebana?", "id": "Seni merangkai bunga." },
  { "en": "Ikhbar?", "id": "Pemberitahuan (Arab)." },
  { "en": "Ikhlas?", "id": "Tulus." },
  { "en": "Ikhtiar?", "id": "Usaha." },
  { "en": "Ikhtilaf?", "id": "Perbedaan pendapat." },
  { "en": "Ikhtisar?", "id": "Ringkasan." },
  { "en": "Ikhtisariah?", "id": "Ringkas." },
  { "en": "Iklim?", "id": "Cuaca rata-rata." },
  { "en": "Ikon?", "id": "Lambang." },
  { "en": "Ikonis?", "id": "Bersifat ikon." },
  { "en": "Ikonoklasme?", "id": "Penghancuran berhala." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 4000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
