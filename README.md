<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
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
        <h1>Pengetahuan Umum Dasar</h1>
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


  { "en": "Apa Itu Konversi Daya?", "id": "Mengubah Karakteristik Daya Listrik Dari Satu Bentuk." },
  { "en": "Apa Tujuan Utama Konverter Daya?", "id": "Mencocokkan Sumber Daya Dengan Kebutuhan Beban." },
  { "en": "Apa Itu Elektronika Daya?", "id": "Aplikasi Elektronika Solid-State Untuk Kontrol Daya." },
  { "en": "Sebutkan Empat Jenis Konversi Daya Dasar?", "id": "AC ke DC, DC ke AC, DC ke DC, AC ke AC." },
  { "en": "Apa Nama Lain Untuk Konverter AC ke DC?", "id": "Penyearah Atau Rectifier." },
  { "en": "Apa Fungsi Dari Penyearah?", "id": "Mengubah Tegangan Bolak-balik Menjadi Searah." },
  { "en": "Apa Nama Lain Untuk Konverter DC ke AC?", "id": "Pembalik Atau Inverter." },
  { "en": "Apa Fungsi Dari Inverter?", "id": "Mengubah Tegangan Searah Menjadi Bolak-balik." },
  { "en": "Apa Nama Lain Untuk Konverter DC ke DC?", "id": "Chopper Atau Regulator Switching." },
  { "en": "Apa Fungsi Dari Konverter DC-DC?", "id": "Mengubah Level Tegangan DC." },
  { "en": "Apa Nama Lain Untuk Konverter AC ke AC?", "id": "Siklokonverter Atau Kontroler Tegangan AC." },
  { "en": "Apa Itu Saklar Elektronik Daya?", "id": "Komponen Kunci Dalam Konverter Daya." },
  { "en": "Sebutkan Contoh Saklar Elektronik Daya?", "id": "Dioda, Transistor, Thyristor." },
  { "en": "Apa Itu Dioda Daya?", "id": "Saklar Tak Terkendali Yang Mengalirkan Arus Satu Arah." },
  { "en": "Apa Itu Thyristor?", "id": "Saklar Semiterkendali Dengan Tiga Terminal." },
  { "en": "Apa Kepanjangan Dari SCR (Silicon-Controlled Rectifier)?", "id": "Penyearah Terkendali Silikon." },
  { "en": "Apa Itu Transistor?", "id": "Saklar Terkendali Penuh." },
  { "en": "Apa Kepanjangan Dari BJT (Bipolar Junction Transistor)?", "id": "Transistor Sambungan Bipolar." },
  { "en": "Apa Kepanjangan Dari MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Transistor Efek Medan Semikonduktor Oksida Logam." },
  { "en": "Apa Kepanjangan Dari IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Transistor Bipolar Gerbang Terisolasi." },
  { "en": "Apa Keuntungan MOSFET Daya?", "id": "Kecepatan Switching Tinggi, Tahanan Rendah." },
  { "en": "Apa Keuntungan IGBT?", "id": "Kemampuan Tegangan Dan Arus Tinggi." },
  { "en": "Apa Itu Penyearah Setengah Gelombang?", "id": "Hanya Melewatkan Setengah Siklus Dari Gelombang AC." },
  { "en": "Apa Itu Penyearah Gelombang Penuh?", "id": "Memanfaatkan Kedua Setengah Siklus Gelombang AC." },
  { "en": "Sebutkan Dua Tipe Penyearah Gelombang Penuh?", "id": "Jembatan (Bridge) Dan Center-Tapped." },
  { "en": "Berapa Dioda Yang Dibutuhkan Penyearah Jembatan?", "id": "Empat Dioda." },
  { "en": "Apa Itu Riak (Ripple) Tegangan?", "id": "Variasi Periodik Kecil Pada Tegangan DC." },
  { "en": "Bagaimana Cara Mengurangi Riak?", "id": "Menggunakan Kapasitor Filter." },
  { "en": "Apa Itu Faktor Riak?", "id": "Ukuran Kuantitatif Efektivitas Filter." },
  { "en": "Apa Itu Penyearah Terkendali?", "id": "Penyearah Yang Tegangan Outputnya Dapat Diatur." },
  { "en": "Komponen Apa Yang Digunakan Dalam Penyearah Terkendali?", "id": "SCR Atau Thyristor." },
  { "en": "Apa Itu Sudut Penyulutan (Firing Angle)?", "id": "Sudut Untuk Memicu SCR." },
  { "en": "Apa Itu Mode Konduksi Kontinu (CCM)?", "id": "Arus Induktor Tidak Pernah Mencapai Nol." },
  { "en": "Apa Itu Mode Konduksi Discontinue (DCM)?", "id": "Arus Induktor Mencapai Nol Selama Periode." },
  { "en": "Apa Fungsi Induktor Dalam Konverter Daya?", "id": "Menyimpan Energi Dalam Bentuk Medan Magnet." },
  { "en": "Apa Fungsi Kapasitor Dalam Konverter Daya?", "id": "Menyimpan Energi Dalam Bentuk Medan Listrik." },
  { "en": "Apa Itu Konverter Buck?", "id": "Konverter DC-DC Penurun Tegangan." },
  { "en": "Apa Itu Konverter Boost?", "id": "Konverter DC-DC Penaik Tegangan." },
  { "en": "Apa Itu Konverter Buck-Boost?", "id": "Dapat Menaikkan Atau Menurunkan Tegangan." },
  { "en": "Apa Polaritas Output Konverter Buck-Boost?", "id": "Terbalik Dari Polaritas Input." },
  { "en": "Apa Itu Siklus Kerja (Duty Cycle)?", "id": "Rasio Waktu On Terhadap Periode Total." },
  { "en": "Bagaimana Siklus Kerja Mengontrol Output Konverter Buck?", "id": "Tegangan Output Sebanding Dengan Siklus Kerja." },
  { "en": "Bagaimana Siklus Kerja Mengontrol Output Konverter Boost?", "id": "Tegangan Output Berbanding Terbalik Dengan (1-D)." },
  { "en": "Apa Itu Konverter Cuk?", "id": "Jenis Konverter Buck-Boost Dengan Kapasitor." },
  { "en": "Apa Itu Konverter SEPIC (Single-Ended Primary-Inductor Converter)?", "id": "Konverter DC-DC Dengan Output Tidak Terbalik." },
  { "en": "Apa Itu Konverter Terisolasi?", "id": "Input Dan Output Terisolasi Secara Elektrik." },
  { "en": "Apa Komponen Yang Menyediakan Isolasi?", "id": "Transformator Frekuensi Tinggi." },
  { "en": "Apa Itu Konverter Flyback?", "id": "Konverter Terisolasi Berbasis Topologi Buck-Boost." },
  { "en": "Apa Itu Konverter Forward?", "id": "Konverter Terisolasi Berbasis Topologi Buck." },
  { "en": "Apa Itu Konverter Jembatan Penuh (Full-Bridge)?", "id": "Menggunakan Empat Saklar Untuk Mengerakkan Trafo." },
  { "en": "Apa Itu Konverter Setengah Jembatan (Half-Bridge)?", "id": "Menggunakan Dua Saklar Dan Dua Kapasitor." },
  { "en": "Apa Itu Pulse Width Modulation (PWM)?", "id": "Teknik Modulasi Untuk Mengontrol Daya." },
  { "en": "Bagaimana PWM Bekerja?", "id": "Mengubah Lebar Pulsa Sinyal." },
  { "en": "Apa Itu Inverter Sumber Tegangan (VSI)?", "id": "Inverter Dengan Sumber DC Berupa Tegangan." },
  { "en": "Apa Itu Inverter Sumber Arus (CSI)?", "id": "Inverter Dengan Sumber DC Berupa Arus." },
  { "en": "Apa Itu Inverter Gelombang Kotak (Square Wave)?", "id": "Menghasilkan Output AC Berbentuk Gelombang Kotak." },
  { "en": "Apa Itu Inverter Gelombang Sinus Modifikasi?", "id": "Outputnya Lebih Baik Dari Gelombang Kotak." },
  { "en": "Apa Itu Inverter Gelombang Sinus Murni?", "id": "Menghasilkan Output AC Hampir Sempurna." },
  { "en": "Apa Itu Inverter Satu Fasa?", "id": "Menghasilkan Output AC Satu Fasa." },
  { "en": "Apa Itu Inverter Tiga Fasa?", "id": "Menghasilkan Output AC Tiga Fasa." },
  { "en": "Apa Itu Inverter Jembatan (Bridge Inverter)?", "id": "Topologi Umum Untuk Inverter Satu Fasa." },
  { "en": "Apa Itu Inverter Multilevel?", "id": "Menghasilkan Output AC Dengan Beberapa Tingkat Tegangan." },
  { "en": "Apa Keuntungan Inverter Multilevel?", "id": "Kualitas Gelombang Lebih Baik, THD Rendah." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran Distorsi Harmonik Suatu Sinyal." },
  { "en": "Apa Itu Kontroler Tegangan AC?", "id": "Mengontrol Tegangan RMS AC Tanpa Mengubah Frekuensi." },
  { "en": "Apa Itu Siklokonverter?", "id": "Mengubah Frekuensi AC Secara Langsung." },
  { "en": "Apa Itu Konverter Matriks?", "id": "Konverter AC-AC Tanpa Penyimpanan Energi DC." },
  { "en": "Apa Itu Switching Losses?", "id": "Kehilangan Daya Selama Transisi Saklar." },
  { "en": "Apa Itu Conduction Losses?", "id": "Kehilangan Daya Saat Saklar Dalam Keadaan On." },
  { "en": "Apa Itu Efisiensi Konverter?", "id": "Rasio Daya Output Terhadap Daya Input." },
  { "en": "Apa Itu Heatsink?", "id": "Perangkat Untuk Mendinginkan Komponen Elektronik." },
  { "en": "Mengapa Heatsink Penting?", "id": "Mencegah Komponen Dari Panas Berlebih." },
  { "en": "Apa Itu Rangkaian Snubber?", "id": "Rangkaian Untuk Melindungi Saklar Dari Lonjakan." },
  { "en": "Apa Itu Soft Switching?", "id": "Teknik Mengurangi Switching Losses." },
  { "en": "Apa Itu Zero Voltage Switching (ZVS)?", "id": "Menghidupkan Saklar Saat Tegangan Nol." },
  { "en": "Apa Itu Zero Current Switching (ZCS)?", "id": "Mematikan Saklar Saat Arus Nol." },
  { "en": "Apa Itu Konverter Resonan?", "id": "Menggunakan Rangkaian Resonan Untuk Soft Switching." },
  { "en": "Apa Itu Power Factor (PF)?", "id": "Rasio Daya Nyata Terhadap Daya Tampak." },
  { "en": "Apa Penyebab Power Factor Rendah?", "id": "Beban Induktif Atau Kapasitif." },
  { "en": "Apa Itu Power Factor Correction (PFC)?", "id": "Teknik Untuk Meningkatkan Power Factor." },
  { "en": "Apa Itu PFC Pasif?", "id": "Menggunakan Komponen Pasif Seperti Kapasitor." },
  { "en": "Apa Itu PFC Aktif?", "id": "Menggunakan Konverter Daya Untuk Membentuk Arus." },
  { "en": "Apa Itu Uninterruptible Power Supply (UPS)?", "id": "Menyediakan Daya Darurat Saat Listrik Padam." },
  { "en": "Apa Tipe Utama UPS?", "id": "Offline (Standby), Line-Interactive, Dan Online." },
  { "en": "Apa Itu Mode Switching?", "id": "Operasi Saklar Sebagai On Atau Off." },
  { "en": "Apa Itu Mode Linier?", "id": "Operasi Transistor Di Wilayah Aktif." },
  { "en": "Mana Yang Lebih Efisien: Mode Switching Atau Linier?", "id": "Mode Switching Jauh Lebih Efisien." },
  { "en": "Apa Itu Linear Regulator?", "id": "Regulator Yang Bekerja Dalam Mode Linier." },
  { "en": "Apa Kerugian Linear Regulator?", "id": "Efisiensi Rendah, Menghasilkan Banyak Panas." },
  { "en": "Apa Itu Switch-Mode Power Supply (SMPS)?", "id": "Catu Daya Berbasis Konverter Switching." },
  { "en": "Apa Keuntungan SMPS?", "id": "Efisiensi Tinggi, Ukuran Kecil, Ringan." },
  { "en": "Apa Itu Electromagnetic Interference (EMI)?", "id": "Gangguan Elektromagnetik Yang Dihasilkan Konverter." },
  { "en": "Bagaimana Cara Mengurangi EMI?", "id": "Menggunakan Filter, Layout PCB Yang Baik." },
  { "en": "Apa Itu Loop Umpan Balik (Feedback Loop)?", "id": "Digunakan Untuk Meregulasi Tegangan Output." },
  { "en": "Apa Itu Stabilitas Loop?", "id": "Kemampuan Sistem Untuk Tetap Stabil." },
  { "en": "Apa Itu Diagram Bode?", "id": "Grafik Untuk Menganalisis Stabilitas Loop." },
  { "en": "Apa Itu Phase Margin?", "id": "Ukuran Kestabilan Relatif Suatu Sistem." },
  { "en": "Apa Itu Gain Margin?", "id": "Ukuran Kestabilan Lainnya." },
  { "en": "Apa Itu Konverter Bidirectional?", "id": "Konverter Yang Aliran Dayanya Bisa Dua Arah." },
  { "en": "Apa Itu Commutation?", "id": "Proses Mematikan Saklar Elektronik." },
  { "en": "Apa Itu Line Commutation?", "id": "Commutation Yang Terjadi Secara Alami Oleh Tegangan AC." },
  { "en": "Apa Itu Forced Commutation?", "id": "Menggunakan Rangkaian Eksternal Untuk Mematikan Saklar." },
  { "en": "Apa Itu Beban Resistif?", "id": "Beban Yang Hanya Terdiri Dari Resistor." },
  { "en": "Apa Itu Beban Induktif?", "id": "Beban Yang Memiliki Sifat Induktansi." },
  { "en": "Apa Itu Beban Kapasitif?", "id": "Beban Yang Memiliki Sifat Kapasitansi." },
  { "en": "Apa Itu Freewheeling Diode?", "id": "Dioda Untuk Memberikan Jalur Arus Beban Induktif." },
  { "en": "Apa Fungsi Dari Freewheeling Diode?", "id": "Mencegah Lonjakan Tegangan Berbahaya." },
  { "en": "Apa Itu Rectifier Tiga Fasa?", "id": "Penyearah Untuk Sumber Tegangan Tiga Fasa." },
  { "en": "Apa Keuntungan Penyearah Tiga Fasa?", "id": "Riak Output Lebih Rendah Dari Satu Fasa." },
  { "en": "Apa Itu Penyearah Enam Pulsa?", "id": "Penyearah Gelombang Penuh Tiga Fasa." },
  { "en": "Apa Itu Penyearah Dua Belas Pulsa?", "id": "Menggabungkan Dua Penyearah Enam Pulsa." },
  { "en": "Apa Keuntungan Penyearah Dua Belas Pulsa?", "id": "Harmonik Arus Input Sangat Rendah." },
  { "en": "Apa Itu Harmonik?", "id": "Komponen Frekuensi Kelipatan Dari Frekuensi Fundamental." },
  { "en": "Mengapa Harmonik Menjadi Masalah?", "id": "Menyebabkan Pemanasan, Distorsi, Dan Interferensi." },
  { "en": "Apa Itu Filter Harmonik?", "id": "Rangkaian Untuk Mengurangi Atau Menghilangkan Harmonik." },
  { "en": "Apa Itu Filter Pasif?", "id": "Filter Harmonik Yang Menggunakan Komponen Pasif." },
  { "en": "Apa Itu Filter Aktif?", "id": "Filter Harmonik Yang Menggunakan Konverter Daya." },
  { "en": "Apa Itu Topologi Konverter?", "id": "Susunan Komponen Dalam Sebuah Konverter Daya." },
  { "en": "Apa Itu Konverter Push-Pull?", "id": "Konverter Terisolasi Dengan Trafo Center-tapped." },
  { "en": "Apa Itu Drive Gerbang (Gate Drive)?", "id": "Rangkaian Untuk Mengontrol Saklar Transistor." },
  { "en": "Mengapa Gate Drive Penting?", "id": "Memastikan Switching Yang Cepat Dan Andal." },
  { "en": "Apa Itu Isolasi Gerbang?", "id": "Mengisolasi Sinyal Kontrol Dari Sirkuit Daya." },
  { "en": "Apa Itu Bootstrap Power Supply?", "id": "Teknik Untuk Menyediakan Daya Ke Gate Drive Sisi Atas." },
  { "en": "Apa Itu Overcurrent Protection?", "id": "Fitur Keamanan Untuk Melindungi Dari Arus Berlebih." },
  { "en": "Apa Itu Overvoltage Protection?", "id": "Fitur Keamanan Untuk Melindungi Dari Tegangan Berlebih." },
  { "en": "Apa Itu Thermal Shutdown?", "id": "Mematikan Perangkat Jika Suhu Terlalu Tinggi." },
  { "en": "Apa Itu Diac (Diode for Alternating Current)?", "id": "Dioda Pemicu Dua Arah." },
  { "en": "Apa Itu Triac (Triode for Alternating Current)?", "id": "Thyristor Dua Arah." },
  { "en": "Di Mana Triac Biasa Digunakan?", "id": "Dalam Rangkaian Dimmer Lampu Dan Kontrol Motor." },
  { "en": "Apa Itu Gate Turn-Off Thyristor (GTO)?", "id": "Thyristor Yang Dapat Dimatikan Melalui Gerbang." },
  { "en": "Apa Itu Insulated-Gate Commutated Thyristor (IGCT)?", "id": "Thyristor Dengan Unit Gate Drive Terintegrasi." },
  { "en": "Apa Itu Semikonduktor Wide-Bandgap?", "id": "Material Semikonduktor Dengan Celah Pita Besar." },
  { "en": "Sebutkan Contoh Semikonduktor Wide-Bandgap?", "id": "Silikon Karbida (SiC) Dan Galium Nitrida (GaN)." },
  { "en": "Apa Keuntungan Perangkat Berbasis SiC Dan GaN?", "id": "Efisiensi Lebih Tinggi, Frekuensi Lebih Tinggi." },
  { "en": "Apa Itu Transformator?", "id": "Perangkat Pasif Untuk Mentransfer Energi Listrik." },
  { "en": "Apa Prinsip Kerja Transformator?", "id": "Induksi Elektromagnetik Antara Dua Kumparan." },
  { "en": "Apa Itu Transformator Step-Up?", "id": "Menaikkan Tegangan AC." },
  { "en": "Apa Itu Transformator Step-Down?", "id": "Menurunkan Tegangan AC." },
  { "en": "Apa Itu Turns Ratio?", "id": "Rasio Jumlah Lilitan Antara Kumparan." },
  { "en": "Apa Itu Inti Transformator (Transformer Core)?", "id": "Material Magnetik Untuk Memusatkan Fluks." },
  { "en": "Apa Itu Inverter Jembatan H (H-Bridge)?", "id": "Nama Lain Untuk Inverter Jembatan Penuh." },
  { "en": "Apa Itu Sinusoidal Pulse Width Modulation (SPWM)?", "id": "Teknik PWM Untuk Menghasilkan Gelombang Sinus." },
  { "en": "Bagaimana SPWM Bekerja?", "id": "Membandingkan Gelombang Sinus Dengan Gelombang Segitiga." },
  { "en": "Apa Itu Rasio Modulasi Amplitudo?", "id": "Rasio Amplitudo Sinus Referensi Terhadap Segitiga." },
  { "en": "Apa Itu Rasio Modulasi Frekuensi?", "id": "Rasio Frekuensi Segitiga Terhadap Sinus Referensi." },
  { "en": "Apa Itu Space Vector Modulation (SVM)?", "id": "Algoritma PWM Canggih Untuk Inverter Tiga Fasa." },
  { "en": "Apa Keuntungan Space Vector Modulation?", "id": "Pemanfaatan Tegangan Bus DC Lebih Baik." },
  { "en": "Apa Itu Renewable Energy?", "id": "Energi Yang Berasal Dari Sumber Daya Terbarukan." },
  { "en": "Bagaimana Konversi Daya Berperan Dalam Energi Terbarukan?", "id": "Menghubungkan Sumber Terbarukan Ke Jaringan." },
  { "en": "Apa Itu Panel Surya Fotovoltaik (PV)?", "id": "Mengubah Sinar Matahari Langsung Menjadi Listrik DC." },
  { "en": "Apa Itu Inverter Surya?", "id": "Mengubah Output DC Panel Surya Menjadi AC." },
  { "en": "Apa Itu Maximum Power Point Tracking (MPPT)?", "id": "Algoritma Untuk Mendapatkan Daya Maksimal Dari PV." },
  { "en": "Apa Itu Turbin Angin?", "id": "Mengubah Energi Kinetik Angin Menjadi Listrik." },
  { "en": "Jenis Konverter Apa Yang Digunakan Turbin Angin?", "id": "Penyearah Dan Inverter (Back-to-Back Converter)." },
  { "en": "Apa Itu Electric Vehicle (EV)?", "id": "Kendaraan Yang Digerakkan Oleh Motor Listrik." },
  { "en": "Apa Itu On-Board Charger (OBC)?", "id": "Pengisi Daya Baterai Di Dalam Kendaraan Listrik." },
  { "en": "Apa Fungsi On-Board Charger?", "id": "Mengubah AC Dari Jaringan Menjadi DC." },
  { "en": "Apa Itu DC Fast Charger?", "id": "Pengisi Daya Eksternal Yang Menyediakan DC Daya Tinggi." },
  { "en": "Apa Itu Motor Drive?", "id": "Sistem Elektronik Untuk Mengontrol Kecepatan Motor." },
  { "en": "Apa Itu Variable Frequency Drive (VFD)?", "id": "Mengontrol Kecepatan Motor AC Dengan Mengubah Frekuensi." },
  { "en": "Apa Itu Induktor?", "id": "Komponen Pasif Yang Menyimpan Energi Magnetik." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Pasif Yang Menyimpan Energi Listrik." },
  { "en": "Apa Itu Inti Ferit (Ferrite Core)?", "id": "Material Keramik Untuk Inti Induktor Frekuensi Tinggi." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus AC Cenderung Mengalir Di Permukaan Konduktor." },
  { "en": "Apa Itu Efek Kedekatan (Proximity Effect)?", "id": "Arus Terdistribusi Tidak Merata Akibat Medan Magnet." },
  { "en": "Apa Itu Kabel Litz?", "id": "Kabel Terpilin Untuk Mengurangi Efek Kulit." },
  { "en": "Apa Itu Disipasi Daya?", "id": "Konversi Energi Listrik Menjadi Panas." },
  { "en": "Apa Itu Resistansi Termal?", "id": "Ukuran Hambatan Terhadap Aliran Panas." },
  { "en": "Apa Itu Printed Circuit Board (PCB)?", "id": "Papan Untuk Menghubungkan Komponen Elektronik." },
  { "en": "Apa Itu Layout PCB?", "id": "Tata Letak Fisik Komponen Dan Jalur." },
  { "en": "Mengapa Layout PCB Penting Dalam Konversi Daya?", "id": "Mempengaruhi Kinerja, EMI, Dan Manajemen Termal." },
  { "en": "Apa Itu Ground Plane?", "id": "Lapisan Tembaga Besar Sebagai Jalur Kembali Arus." },
  { "en": "Apa Itu Induktansi Parasitik?", "id": "Induktansi Yang Tidak Diinginkan Dalam Rangkaian." },
  { "en": "Apa Itu Kapasitansi Parasitik?", "id": "Kapasitansi Yang Tidak Diinginkan Dalam Rangkaian." },
  { "en": "Apa Itu Kontrol Mode Tegangan?", "id": "Mengontrol Output Berdasarkan Umpan Balik Tegangan." },
  { "en": "Apa Itu Kontrol Mode Arus?", "id": "Mengontrol Saklar Berdasarkan Umpan Balik Arus." },
  { "en": "Apa Keuntungan Kontrol Mode Arus?", "id": "Respons Lebih Cepat, Perlindungan Arus." },
  { "en": "Apa Itu Kompensator?", "id": "Rangkaian Dalam Loop Umpan Balik Untuk Stabilitas." },
  { "en": "Apa Itu Kontrol Proporsional-Integral-Derivatif (PID)?", "id": "Mekanisme Kontrol Loop Umpan Balik." },
  { "en": "Apa Itu Respon Transien?", "id": "Respons Sistem Terhadap Perubahan Input." },
  { "en": "Apa Itu Steady-State?", "id": "Kondisi Sistem Setelah Respon Transien Mereda." },
  { "en": "Apa Itu Switched-Capacitor Converter?", "id": "Konverter DC-DC Menggunakan Kapasitor Dan Saklar." },
  { "en": "Apa Nama Lain Switched-Capacitor Converter?", "id": "Charge Pump." },
  { "en": "Apa Keuntungan Charge Pump?", "id": "Tidak Memerlukan Induktor Magnetik." },
  { "en": "Apa Itu Low-Dropout (LDO) Regulator?", "id": "Linear Regulator Dengan Tegangan Dropout Rendah." },
  { "en": "Apa Itu Tegangan Dropout?", "id": "Perbedaan Tegangan Minimum Antara Input Dan Output." },
  { "en": "Apa Itu High-Voltage Direct Current (HVDC)?", "id": "Transmisi Daya Listrik Menggunakan DC Tegangan Tinggi." },
  { "en": "Apa Keuntungan Transmisi HVDC?", "id": "Kerugian Lebih Rendah Untuk Jarak Jauh." },
  { "en": "Apa Itu Konverter Sumber Tegangan (VSC)?", "id": "Tipe Konverter Yang Digunakan Dalam HVDC Modern." },
  { "en": "Apa Itu Solid-State Transformer (SST)?", "id": "Transformator Daya Berbasis Elektronika Daya." },
  { "en": "Apa Itu Motor DC?", "id": "Motor Listrik Yang Ditenagai Oleh Arus Searah." },
  { "en": "Apa Itu Motor AC?", "id": "Motor Listrik Yang Ditenagai Oleh Arus Bolak-balik." },
  { "en": "Apa Itu Motor Induksi?", "id": "Jenis Motor AC Yang Paling Umum." },
  { "en": "Apa Itu Motor Sinkron?", "id": "Motor AC Dimana Putaran Rotor Sinkron." },
  { "en": "Apa Itu Brushless DC (BLDC) Motor?", "id": "Motor DC Tanpa Sikat Mekanis." },
  { "en": "Apa Itu Resistor?", "id": "Komponen Pasif Yang Menghambat Aliran Arus." },
  { "en": "Apa Itu Hukum Ohm?", "id": "Menjelaskan Hubungan Antara Tegangan, Arus, Resistansi." },
  { "en": "Apa Itu Daya Listrik?", "id": "Laju Transfer Energi Listrik Per Satuan Waktu." },
  { "en": "Apa Satuan Daya Listrik?", "id": "Watt (W)." },
  { "en": "Apa Itu Daya Nyata (Real Power)?", "id": "Daya Aktual Yang Digunakan Oleh Beban." },
  { "en": "Apa Itu Daya Reaktif (Reactive Power)?", "id": "Daya Yang Disimpan Dan Dikembalikan Oleh Beban." },
  { "en": "Apa Satuan Daya Reaktif?", "id": "Volt-Ampere Reactive (VAR)." },
  { "en": "Apa Itu Daya Tampak (Apparent Power)?", "id": "Kombinasi Vektor Dari Daya Nyata Dan Reaktif." },
  { "en": "Apa Satuan Daya Tampak?", "id": "Volt-Ampere (VA)." },
  { "en": "Apa Itu Segitiga Daya?", "id": "Representasi Grafis Hubungan Antara Tiga Daya." },
  { "en": "Apa Itu Thyristor Terpadu Gerbang-Terkendali (IGCT)?", "id": "Nama Lain Untuk Insulated-Gate Commutated Thyristor." },
  { "en": "Apa Itu Mode Saturasi?", "id": "Wilayah Operasi Transistor Dimana Sepenuhnya On." },
  { "en": "Apa Itu Mode Cut-off?", "id": "Wilayah Operasi Transistor Dimana Sepenuhnya Off." },
  { "en": "Apa Itu Safe Operating Area (SOA)?", "id": "Kondisi Tegangan Dan Arus Aman Untuk Perangkat." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Dengan Penurunan Tegangan Maju Rendah." },
  { "en": "Mengapa Dioda Schottky Digunakan Dalam SMPS?", "id": "Mengurangi Kerugian Dan Meningkatkan Efisiensi." },
  { "en": "Apa Itu Waktu Pemulihan Terbalik (Reverse Recovery Time)?", "id": "Waktu Dioda Untuk Berhenti Menghantar Terbalik." },
  { "en": "Apakah Dioda Schottky Memiliki Pemulihan Terbalik?", "id": "Hampir Tidak Ada, Membuatnya Sangat Cepat." },
  { "en": "Apa Itu Snubber Disipatif?", "id": "Snubber Yang Membuang Energi Dalam Bentuk Panas." },
  { "en": "Apa Itu Snubber Non-Disipatif (Lossless)?", "id": "Snubber Yang Mentransfer Energi Kembali Ke Sumber." },
  { "en": "Apa Itu Konverter Zeta?", "id": "Konverter DC-DC Non-inverting Buck-Boost." },
  { "en": "Apa Itu Konverter Interleaved?", "id": "Menggabungkan Beberapa Konverter Secara Paralel." },
  { "en": "Apa Keuntungan Konverter Interleaved?", "id": "Mengurangi Riak Arus Input Dan Output." },
  { "en": "Apa Itu Model Rata-rata Keadaan-Ruang?", "id": "Teknik Untuk Menganalisis Dinamika Konverter." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Respons Sistem Terhadap Input Sinusoidal." },
  { "en": "Apa Itu Penyearah Jembatan Tunggal-Fasa Terkendali Penuh?", "id": "Menggunakan Empat SCR." },
  { "en": "Apa Itu Penyearah Jembatan Tunggal-Fasa Setengah-Terkendali?", "id": "Menggunakan Dua SCR Dan Dua Dioda." },
  { "en": "Apa Itu Operasi Empat Kuadran?", "id": "Konverter Dapat Beroperasi Di Empat Kuadran." },
  { "en": "Apa Itu Chopper Dua Kuadran?", "id": "Dapat Mengontrol Tegangan Dan Arus Positif." },
  { "en": "Apa Itu Chopper Empat Kuadran?", "id": "Dapat Mengontrol Tegangan Dan Arus Positif/Negatif." },
  { "en": "Apa Itu Pengereman Regeneratif (Regenerative Braking)?", "id": "Mengembalikan Energi Pengereman Kembali Ke Sumber." },
  { "en": "Bagaimana Konverter Memungkinkan Pengereman Regeneratif?", "id": "Dengan Aliran Daya Dua Arah." },
  { "en": "Apa Itu Sistem Tenaga Listrik?", "id": "Jaringan Komponen Listrik Untuk Memasok Daya." },
  { "en": "Apa Itu Pembangkitan (Generation)?", "id": "Proses Menghasilkan Energi Listrik." },
  { "en": "Apa Itu Transmisi?", "id": "Proses Menyalurkan Daya Jarak Jauh." },
  { "en": "Apa Itu Distribusi?", "id": "Proses Menyalurkan Daya Ke Pengguna Akhir." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Untuk Mengubah Level Tegangan." },
  { "en": "Apa Itu Jaringan Listrik (Grid)?", "id": "Jaringan Transmisi Dan Distribusi Yang Terhubung." },
  { "en": "Apa Itu Smart Grid?", "id": "Jaringan Listrik Cerdas Dengan Komunikasi Digital." },
  { "en": "Apa Itu Flexible AC Transmission System (FACTS)?", "id": "Sistem Berbasis Elektronika Daya Untuk Jaringan AC." },
  { "en": "Sebutkan Contoh Perangkat FACTS?", "id": "STATCOM, SVC, Dan UPFC." },
  { "en": "Apa Itu Static VAR Compensator (SVC)?", "id": "Mengatur Tegangan Dengan Menyerap Daya Reaktif." },
  { "en": "Apa Itu Static Synchronous Compensator (STATCOM)?", "id": "SVC Berbasis Konverter Sumber Tegangan." },
  { "en": "Apa Itu Unified Power Flow Controller (UPFC)?", "id": "Perangkat FACTS Paling Serbaguna." },
  { "en": "Apa Itu Microgrid?", "id": "Jaringan Listrik Lokal Terkendali." },
  { "en": "Apa Itu Mode Terhubung Jaringan (Grid-Tied)?", "id": "Microgrid Beroperasi Terhubung Ke Jaringan Utama." },
  { "en": "Apa Itu Mode Pulau (Islanding)?", "id": "Microgrid Beroperasi Secara Independen." },
  { "en": "Apa Itu Sistem Penyimpanan Energi Baterai (BESS)?", "id": "Menyimpan Energi Listrik Dalam Baterai." },
  { "en": "Apa Itu Baterai Lithium-Ion?", "id": "Jenis Baterai Isi Ulang Yang Populer." },
  { "en": "Apa Itu State of Charge (SoC)?", "id": "Tingkat Pengisian Baterai." },
  { "en": "Apa Itu Depth of Discharge (DoD)?", "id": "Persentase Baterai Yang Telah Dikosongkan." },
  { "en": "Apa Itu Manajemen Termal?", "id": "Mengontrol Suhu Komponen Elektronik." },
  { "en": "Apa Itu Pendinginan Udara Paksa (Forced Air Cooling)?", "id": "Menggunakan Kipas Untuk Meniupkan Udara." },
  { "en": "Apa Itu Pendinginan Cair (Liquid Cooling)?", "id": "Menggunakan Cairan Untuk Mentransfer Panas." },
  { "en": "Apa Itu Heat Pipe?", "id": "Perangkat Transfer Panas Pasif." },
  { "en": "Apa Itu Simbiosis Termal?", "id": "Memanfaatkan Panas Buangan Satu Sistem." },
  { "en": "Apa Itu Keandalan (Reliability)?", "id": "Kemungkinan Sistem Berfungsi Sesuai Harapan." },
  { "en": "Apa Itu Mean Time Between Failures (MTBF)?", "id": "Waktu Rata-rata Antara Kegagalan." },
  { "en": "Apa Itu Analisis Kegagalan?", "id": "Proses Menyelidiki Penyebab Kegagalan Komponen." },
  { "en": "Apa Itu Derating?", "id": "Mengoperasikan Komponen Di Bawah Peringkat Maksimum." },
  { "en": "Apa Itu Magnetics?", "id": "Studi Tentang Medan Magnet Dan Komponen Magnetik." },
  { "en": "Apa Itu Fluks Magnetik?", "id": "Ukuran Total Medan Magnet." },
  { "en": "Apa Itu Permeabilitas?", "id": "Ukuran Kemampuan Bahan Mendukung Medan Magnet." },
  { "en": "Apa Itu Histeresis?", "id": "Ketergantungan Keadaan Sistem Pada Sejarahnya." },
  { "en": "Apa Itu Kerugian Histeresis (Hysteresis Loss)?", "id": "Energi Yang Hilang Sebagai Panas Dalam Inti." },
  { "en": "Apa Itu Arus Eddy (Eddy Current)?", "id": "Arus Berputar Yang Diinduksi Dalam Konduktor." },
  { "en": "Apa Itu Kerugian Arus Eddy?", "id": "Energi Yang Hilang Akibat Arus Eddy." },
  { "en": "Bagaimana Cara Mengurangi Kerugian Arus Eddy?", "id": "Menggunakan Inti Berlaminasi Atau Bubuk." },
  { "en": "Apa Itu Induktansi?", "id": "Sifat Komponen Menentang Perubahan Arus." },
  { "en": "Apa Itu Induktansi Kebocoran (Leakage Inductance)?", "id": "Fluks Magnetik Yang Tidak Terhubung." },
  { "en": "Apa Itu Induktansi Magnetisasi (Magnetizing Inductance)?", "id": "Fluks Magnetik Yang Terhubung Dalam Trafo." },
  { "en": "Apa Itu Saturasi Inti?", "id": "Kondisi Inti Tidak Dapat Menahan Fluks Lagi." },
  { "en": "Apa Itu Gapped Inductor?", "id": "Induktor Dengan Celah Udara Di Intinya." },
  { "en": "Mengapa Celah Udara Digunakan?", "id": "Mencegah Saturasi Inti Pada Arus DC." },
  { "en": "Apa Itu Kapasitor Elektrolitik?", "id": "Jenis Kapasitor Terpolarisasi Dengan Kapasitansi Tinggi." },
  { "en": "Apa Itu Kapasitor Film?", "id": "Kapasitor Yang Menggunakan Film Plastik." },
  { "en": "Apa Itu Kapasitor Keramik?", "id": "Kapasitor Yang Menggunakan Bahan Keramik." },
  { "en": "Apa Itu Equivalent Series Resistance (ESR)?", "id": "Resistansi Internal Efektif Kapasitor." },
  { "en": "Apa Itu Equivalent Series Inductance (ESL)?", "id": "Induktansi Internal Efektif Kapasitor." },
  { "en": "Apa Itu Desain Digital?", "id": "Merancang Sirkuit Elektronik Menggunakan Logika Digital." },
  { "en": "Apa Itu Mikrokontroler?", "id": "Komputer Kecil Dalam Satu Sirkuit Terpadu." },
  { "en": "Apa Itu Digital Signal Processor (DSP)?", "id": "Mikroprosesor Khusus Untuk Pemrosesan Sinyal." },
  { "en": "Bagaimana Kontrol Digital Diterapkan?", "id": "Menggunakan Mikrokontroler Atau DSP." },
  { "en": "Apa Itu Analog-to-Digital Converter (ADC)?", "id": "Mengubah Sinyal Analog Menjadi Digital." },
  { "en": "Apa Itu Digital-to-Analog Converter (DAC)?", "id": "Mengubah Sinyal Digital Menjadi Analog." },
  { "en": "Apa Itu Kuantisasi?", "id": "Proses Pemetaan Nilai Kontinu Ke Nilai Diskret." },
  { "en": "Apa Itu Resolusi ADC?", "id": "Jumlah Bit Yang Digunakan Untuk Representasi." },
  { "en": "Apa Itu Laju Sampling (Sampling Rate)?", "id": "Frekuensi Pengambilan Sampel Sinyal Analog." },
  { "en": "Apa Itu Teorema Sampling Nyquist?", "id": "Laju Sampling Harus Dua Kali Frekuensi." },
  { "en": "Apa Itu Aliasing?", "id": "Distorsi Sinyal Akibat Laju Sampling Rendah." },
  { "en": "Apa Itu Field-Programmable Gate Array (FPGA)?", "id": "Sirkuit Terpadu Yang Dapat Diprogram." },
  { "en": "Apa Itu Very High Speed Integrated Circuit Hardware Description Language (VHDL)?", "id": "Bahasa Deskripsi Perangkat Keras." },
  { "en": "Apa Itu Verilog?", "id": "Bahasa Deskripsi Perangkat Keras Lainnya." },
  { "en": "Apa Itu Simulasi?", "id": "Meniru Perilaku Sistem Menggunakan Model." },
  { "en": "Apa Itu Hardware-in-the-Loop (HIL) Simulation?", "id": "Simulasi Yang Melibatkan Komponen Perangkat Keras." },
  { "en": "Apa Itu Rapid Prototyping?", "id": "Pembuatan Cepat Model Atau Prototipe." },
  { "en": "Apa Itu Kondisi Gangguan?", "id": "Kondisi Operasi Tidak Normal Atau Kesalahan." },
  { "en": "Apa Itu Arus Hubung Singkat?", "id": "Arus Sangat Tinggi Akibat Hubungan Rendah." },
  { "en": "Apa Itu Sirkuit Terbuka?", "id": "Kondisi Dimana Jalur Arus Terputus." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda Yang Dirancang Beroperasi Dalam Breakdown Terbalik." },
  { "en": "Apa Fungsi Dioda Zener?", "id": "Sebagai Referensi Atau Regulator Tegangan." },
  { "en": "Apa Itu Regulator Shunt?", "id": "Regulator Yang Ditempatkan Paralel Dengan Beban." },
  { "en": "Apa Itu Regulator Seri?", "id": "Regulator Yang Ditempatkan Seri Dengan Beban." },
  { "en": "Apa Itu Respon Langkah (Step Response)?", "id": "Respons Sistem Terhadap Input Perubahan Mendadak." },
  { "en": "Apa Itu Overshoot?", "id": "Output Melebihi Nilai Akhir Sebelum Stabil." },
  { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Output Naik Dari 10% Ke 90%." },
  { "en": "Apa Itu Waktu Turun (Fall Time)?", "id": "Waktu Output Turun Dari 90% Ke 10%." },
  { "en": "Apa Itu Waktu Tunda (Delay Time)?", "id": "Waktu Output Mencapai 50% Untuk Pertama Kali." },
  { "en": "Apa Itu Waktu Stabil (Settling Time)?", "id": "Waktu Output Masuk Dalam Batas Kesalahan." },
  { "en": "Apa Itu Konverter Kuasi-Resonan?", "id": "Konverter Switching Yang Menggunakan Teknik Resonan." },
  { "en": "Apa Itu Topologi Jembatan Geser Fasa?", "id": "Topologi Jembatan Penuh Yang Menggunakan Pergeseran Fasa." },
  { "en": "Apa Itu Power over Ethernet (PoE)?", "id": "Menyalurkan Daya Listrik Melalui Kabel Ethernet." },
  { "en": "Apa Itu Konverter resonan LLC?", "id": "Jenis Konverter Resonan Yang Populer." },
  { "en": "Apa Kepanjangan LLC?", "id": "Induktor, Induktor, Kapasitor (L-L-C)." },
  { "en": "Apa Itu Wireless Power Transfer (WPT)?", "id": "Transfer Energi Listrik Tanpa Kabel." },
  { "en": "Apa Itu Inductive Coupling?", "id": "Metode WPT Menggunakan Medan Magnet." },
  { "en": "Di Mana Inductive Coupling Digunakan?", "id": "Pengisian Nirkabel Ponsel Dan Sikat Gigi." },
  { "en": "Apa Itu Magnetic Resonance Coupling?", "id": "WPT Untuk Jarak Yang Lebih Jauh." },
  { "en": "Apa Itu Standar Qi?", "id": "Standar Terbuka Untuk Pengisian Daya Nirkabel." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor DC Brushless Yang Bergerak Bertahap." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor Dengan Umpan Balik Posisi." },
  { "en": "Apa Itu Encoder?", "id": "Sensor Untuk Mengukur Posisi Atau Kecepatan." },
  { "en": "Apa Itu Resolver?", "id": "Sensor Posisi Sudut Elektromekanis." },
  { "en": "Apa Itu Inverter Tiga Tingkat?", "id": "Jenis Inverter Multilevel." },
  { "en": "Apa Itu Neutral-Point Clamped (NPC) Inverter?", "id": "Topologi Umum Inverter Tiga Tingkat." },
  { "en": "Apa Itu Flying Capacitor Inverter?", "id": "Jenis Inverter Multilevel Lainnya." },
  { "en": "Apa Itu Cascaded H-Bridge (CHB) Inverter?", "id": "Inverter Multilevel Berbasis Sel Jembatan-H." },
  { "en": "Apa Itu Topologi Modular?", "id": "Sistem Yang Dibangun Dari Modul Berulang." },
  { "en": "Apa Itu Common-Mode Voltage?", "id": "Tegangan Rata-rata Antara Konduktor Dan Ground." },
  { "en": "Apa Itu Common-Mode Current?", "id": "Arus Yang Mengalir Dalam Arah Sama." },
  { "en": "Mengapa Arus Common-Mode Menjadi Masalah?", "id": "Menyebabkan Interferensi Elektromagnetik (EMI) Yang signifikan." },
  { "en": "Apa Itu Common-Mode Choke?", "id": "Induktor Untuk Menekan Derau Common-Mode." },
  { "en": "Apa Itu Differential-Mode Voltage?", "id": "Tegangan Antara Dua Konduktor." },
  { "en": "Apa Itu Differential-Mode Current?", "id": "Arus Yang Mengalir Dalam Arah Berlawanan." },
  { "en": "Apa Itu Sistem Tenaga Fasa-Tunggal?", "id": "Menggunakan Dua Konduktor (Fasa Dan Netral)." },
  { "en": "Apa Itu Sistem Tenaga Tiga-Fasa?", "id": "Menggunakan Tiga Konduktor Dengan Tegangan Berbeda." },
  { "en": "Apa Keuntungan Sistem Tiga-Fasa?", "id": "Transfer Daya Lebih Efisien Dan Konstan." },
  { "en": "Apa Itu Koneksi Wye (Y) atau Bintang?", "id": "Jenis Koneksi Tiga-Fasa." },
  { "en": "Apa Itu Koneksi Delta (Δ)?", "id": "Jenis Koneksi Tiga-Fasa Lainnya." },
  { "en": "Apa Itu Tegangan Fasa?", "id": "Tegangan Antara Satu Fasa Dan Netral." },
  { "en": "Apa Itu Tegangan Antar-Fasa (Line Voltage)?", "id": "Tegangan Antara Dua Fasa." },
  { "en": "Apa Itu Urutan Fasa?", "id": "Urutan Tegangan Fasa Mencapai Puncaknya." },
  { "en": "Apa Itu Daya Pulsating?", "id": "Daya Sesaat Dalam Sistem Fasa-Tunggal." },
  { "en": "Apa Itu Daya Konstan?", "id": "Daya Sesaat Dalam Sistem Tiga-Fasa Seimbang." },
  { "en": "Apa Itu Beban Seimbang?", "id": "Beban Dimana Impedansi Setiap Fasa Sama." },
  { "en": "Apa Itu Beban Tidak Seimbang?", "id": "Beban Dimana Impedansi Fasa Berbeda." },
  { "en": "Apa Itu Komponen Simetris?", "id": "Metode Analisis Sistem Tiga-Fasa Tidak Seimbang." },
  { "en": "Apa Itu Urutan Positif?", "id": "Komponen Dengan Urutan Fasa Normal." },
  { "en": "Apa Itu Urutan Negatif?", "id": "Komponen Dengan Urutan Fasa Terbalik." },
  { "en": "Apa Itu Urutan Nol?", "id": "Komponen Dimana Ketiga Fasa Sefasa." },
  { "en": "Apa Itu Transformator Scott-T?", "id": "Menghubungkan Sistem Tiga-Fasa Ke Dua-Fasa." },
  { "en": "Apa Itu Autotransformator?", "id": "Transformator Dengan Hanya Satu Kumparan." },
  { "en": "Apa Kerugian Autotransformator?", "id": "Tidak Menyediakan Isolasi Elektrik." },
  { "en": "Apa Itu Inrush Current?", "id": "Arus Puncak Saat Perangkat Pertama Dihidupkan." },
  { "en": "Apa Itu Soft Starter?", "id": "Perangkat Untuk Membatasi Inrush Current Motor." },
  { "en": "Apa Itu Crowbar Circuit?", "id": "Rangkaian Perlindungan Overvoltage Yang Membuat Hubung Singkat." },
  { "en": "Apa Itu Varistor?", "id": "Resistor Yang Nilainya Bergantung Pada Tegangan." },
  { "en": "Apa Itu Metal-Oxide Varistor (MOV)?", "id": "Jenis Varistor Umum Untuk Proteksi Lonjakan." },
  { "en": "Apa Itu Gas Discharge Tube (GDT)?", "id": "Perangkat Proteksi Lonjakan Lainnya." },
  { "en": "Apa Itu Surge Protective Device (SPD)?", "id": "Perangkat Untuk Melindungi Peralatan Listrik." },
  { "en": "Apa Itu Lightning Arrester?", "id": "Perangkat Untuk Melindungi Dari Sambaran Petir." },
  { "en": "Apa Itu Pembumian (Grounding)?", "id": "Menghubungkan Sirkuit Ke Bumi." },
  { "en": "Mengapa Pembumian Penting?", "id": "Untuk Keselamatan Dan Perlindungan Peralatan." },
  { "en": "Apa Itu Ground Loop?", "id": "Kondisi Arus Tidak Diinginkan Antara Titik Ground." },
  { "en": "Apa Itu Isolasi Galvanik?", "id": "Mengisolasi Bagian Fungsional Rangkaian Listrik." },
  { "en": "Bagaimana Isolasi Galvanik Dicapai?", "id": "Menggunakan Transformator, Optocoupler, Atau Kapasitor." },
  { "en": "Apa Itu Optocoupler?", "id": "Mentransfer Sinyal Listrik Menggunakan Cahaya." },
  { "en": "Apa Itu Inverter Terhubung Jaringan (Grid-Tied Inverter)?", "id": "Inverter Yang Sinkron Dengan Jaringan Listrik." },
  { "en": "Apa Itu Anti-Islanding Protection?", "id": "Fitur Keamanan Untuk Mematikan Inverter." },
  { "en": "Apa Itu Islanding?", "id": "Kondisi Inverter Terus Memasok Daya." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sistem Kontrol Untuk Sinkronisasi Fasa." },
  { "en": "Apa Itu Grid Forming Inverter?", "id": "Inverter Yang Dapat Menciptakan Jaringan Sendiri." },
  { "en": "Apa Itu Grid Following Inverter?", "id": "Inverter Yang Mengikuti Tegangan Jaringan." },
  { "en": "Apa Itu Virtual Synchronous Machine (VSM)?", "id": "Strategi Kontrol Inverter Meniru Mesin Sinkron." },
  { "en": "Apa Itu Inersia?", "id": "Kecenderungan Benda Menolak Perubahan Gerak." },
  { "en": "Apa Itu Inersia Virtual?", "id": "Inersia Yang Disimulasikan Oleh Konverter Daya." },
  { "en": "Apa Itu Droop Control?", "id": "Metode Kontrol Untuk Berbagi Daya." },
  { "en": "Apa Itu Kualitas Daya (Power Quality)?", "id": "Karakteristik Tegangan, Arus, Atau Frekuensi." },
  { "en": "Apa Itu Voltage Sag?", "id": "Penurunan Tegangan RMS Jangka Pendek." },
  { "en": "Apa Itu Voltage Swell?", "id": "Kenaikan Tegangan RMS Jangka Pendekek." },
  { "en": "Apa Itu Interupsi (Interruption)?", "id": "Kehilangan Total Tegangan Suplai." },
  { "en": "Apa Itu Transien?", "id": "Peristiwa Tegangan Atau Arus Sub-siklus." },
  { "en": "Apa Itu Dynamic Voltage Restorer (DVR)?", "id": "Perangkat Untuk Mengkompensasi Voltage Sag." },
  { "en": "Apa Itu Active Power Filter (APF)?", "id": "Nama Lain Untuk Filter Harmonik Aktif." },
  { "en": "Apa Itu Shunt Active Filter?", "id": "Menyuntikkan Arus Kompensasi Secara Paralel." },
  { "en": "Apa Itu Series Active Filter?", "id": "Menyuntikkan Tegangan Kompensasi Secara Seri." },
  { "en": "Apa Itu Hybrid Filter?", "id": "Menggabungkan Filter Aktif Dan Pasif." },
  { "en": "Apa Itu Teori Daya Reaktif Sesaat (PQ Theory)?", "id": "Metode Kontrol Untuk Filter Aktif." },
  { "en": "Apa Itu Synchronous Reference Frame (SRF) Theory?", "id": "Metode Kontrol Lainnya Berbasis Transformasi." },
  { "en": "Apa Itu Transformasi Clarke?", "id": "Mengubah Variabel Tiga-Fasa Menjadi Dua-Fasa." },
  { "en": "Apa Itu Transformasi Park?", "id": "Mengubah Variabel Dua-Fasa Stasioner Menjadi Berputar." },
  { "en": "Apa Itu Kerangka Referensi DQ0?", "id": "Kerangka Referensi Berputar Untuk Analisis Tiga-Fasa." },
  { "en": "Apa Itu Modulasi Lebar Pulsa Acak?", "id": "Teknik PWM Untuk Menyebarkan Spektrum Harmonik." },
  { "en": "Apa Itu Dithering?", "id": "Menambahkan Derau Untuk Mengurangi Distorsi." },
  { "en": "Apa Itu Konverter Modular Multilevel (MMC)?", "id": "Topologi Konverter Untuk Aplikasi Tegangan Tinggi." },
  { "en": "Apa Itu Submodul (Submodule)?", "id": "Blok Bangunan Dasar Dari MMC." },
  { "en": "Apa Itu Kapasitor Submodul?", "id": "Komponen Penyimpanan Energi Dalam Submodul." },
  { "en": "Apa Itu Balancing Kapasitor?", "id": "Menyeimbangkan Tegangan Pada Kapasitor Seri." },
  { "en": "Mengapa Balancing Kapasitor MMC Penting?", "id": "Menjamin Operasi Submodul Yang Benar." },
  { "en": "Apa Itu Arus Sirkulasi?", "id": "Arus Internal Dalam Konverter Modular." },
  { "en": "Apa Penyebab Arus Sirkulasi Di MMC?", "id": "Ketidakseimbangan Tegangan Kapasitor." },
  { "en": "Apa Itu Konverter Hibrid?", "id": "Menggabungkan Topologi Konverter Yang Berbeda." },
  { "en": "Apa Itu Kontrol Prediktif?", "id": "Menggunakan Model Sistem Untuk Memprediksi Perilaku." },
  { "en": "Apa Itu Model Predictive Control (MPC)?", "id": "Strategi Kontrol Canggih Berbasis Model." },
  { "en": "Apa Keuntungan Kontrol Prediktif?", "id": "Respons Dinamis Cepat Dan Penanganan Kendala." },
  { "en": "Apa Itu Logika Fuzzy?", "id": "Bentuk Logika Multi-nilai." },
  { "en": "Bagaimana Logika Fuzzy Digunakan Dalam Kontrol?", "id": "Untuk Menangani Ketidakpastian Dan Non-linieritas." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?", "id": "Model Komputasi Yang Terinspirasi Otak." },
  { "en": "Bagaimana Jaringan Saraf Digunakan Dalam Kontrol?", "id": "Untuk Identifikasi Sistem Dan Kontrol Adaptif." },
  { "en": "Apa Itu Algoritma Genetik?", "id": "Algoritma Optimisasi Yang Terinspirasi Evolusi." },
  { "en": "Apa Itu Dead Time?", "id": "Waktu Tunda Untuk Mencegah Hubung Singkat." },
  { "en": "Mengapa Dead Time Dibutuhkan?", "id": "Mencegah Saklar Atas Dan Bawah On Bersamaan." },
  { "en": "Apa Efek Negatif Dari Dead Time?", "id": "Menyebabkan Distorsi Pada Tegangan Output." },
  { "en": "Apa Itu Kompensasi Dead Time?", "id": "Teknik Untuk Mengurangi Efek Distorsi." },
  { "en": "Apa Itu Dioda Intrinsik (Body Diode)?", "id": "Dioda Parasitik Di Dalam MOSFET." },
  { "en": "Apa Masalah Dengan Body Diode MOSFET?", "id": "Waktu Pemulihan Terbalik Yang Lambat." },
  { "en": "Apa Itu Superkapasitor?", "id": "Kapasitor Dengan Kapasitansi Sangat Tinggi." },
  { "en": "Apa Nama Lain Superkapasitor?", "id": "Ultracapacitor Atau Kapasitor Lapisan Ganda." },
  { "en": "Apa Aplikasi Superkapasitor?", "id": "Penyimpanan Energi, Daya Puncak." },
  { "en": "Apa Beda Baterai Dan Superkapasitor?", "id": "Superkapasitor Punya Kepadatan Daya Lebih Tinggi." },
  { "en": "Apa Itu Kepadatan Daya?", "id": "Laju Energi Dapat Disimpan Atau Dilepaskan." },
  { "en": "Apa Itu Kepadatan Energi?", "id": "Jumlah Energi Yang Disimpan Per Satuan." },
  { "en": "Mana Yang Kepadatan Energinya Lebih Tinggi?", "id": "Baterai Memiliki Kepadatan Energi Lebih Tinggi." },
  { "en": "Apa Itu Sel Bahan Bakar (Fuel Cell)?", "id": "Mengubah Energi Kimia Menjadi Listrik." },
  { "en": "Jenis Konverter Apa Yang Dibutuhkan Sel Bahan Bakar?", "id": "Konverter Boost DC-DC." },
  { "en": "Apa Itu Elektrolisis?", "id": "Proses Memisahkan Air Menjadi Hidrogen." },
  { "en": "Apa Itu Sistem Tenaga Hibrid?", "id": "Menggabungkan Beberapa Sumber Energi Berbeda." },
  { "en": "Apa Itu Manajemen Daya?", "id": "Strategi Untuk Mengontrol Aliran Daya." },
  { "en": "Apa Itu Gallium Nitride (GaN)?", "id": "Semikonduktor Wide-Bandgap." },
  { "en": "Apa Itu High Electron Mobility Transistor (HEMT)?", "id": "Struktur Transistor Umum Untuk Perangkat GaN." },
  { "en": "Apa Itu Silicon Carbide (SiC)?", "id": "Semikonduktor Wide-Bandgap Lainnya." },
  { "en": "Apa Keunggulan Perangkat SiC?", "id": "Operasi Suhu Tinggi, Tegangan Breakdown Tinggi." },
  { "en": "Apa Itu Konverter Tipe-Z?", "id": "Konverter Impedansi Sumber." },
  { "en": "Apa Keuntungan Konverter Tipe-Z?", "id": "Dapat Menaikkan Tegangan Dengan Satu Tahap." },
  { "en": "Apa Itu Inverter Tipe-Z?", "id": "Inverter Yang Menggunakan Jaringan Impedansi." },
  { "en": "Apa Itu Shoot-Through State?", "id": "Kondisi Saklar Atas Bawah On Bersamaan." },
  { "en": "Apakah Shoot-Through Diizinkan Di Inverter Tipe-Z?", "id": "Ya, Digunakan Untuk Menaikkan Tegangan." },
  { "en": "Apa Itu Saklar Empat Kuadran?", "id": "Saklar Yang Dapat Memblokir Tegangan." },
  { "en": "Apa Itu Common Emitter?", "id": "Konfigurasi Transistor BJT." },
  { "en": "Apa Itu Common Source?", "id": "Konfigurasi Transistor MOSFET." },
  { "en": "Apa Itu Miller Capacitance?", "id": "Efek Kapasitansi Umpan Balik Dalam Transistor." },
  { "en": "Apa Efek Dari Miller Capacitance?", "id": "Membatasi Kecepatan Switching." },
  { "en": "Apa Itu Miller Clamp?", "id": "Rangkaian Untuk Mengurangi Efek Miller." },
  { "en": "Apa Itu Avalanche Breakdown?", "id": "Mekanisme Breakdown Dalam Semikonduktor." },
  { "en": "Apa Itu Zener Breakdown?", "id": "Mekanisme Breakdown Lainnya Pada Tegangan Rendah." },
  { "en": "Apa Itu Karakteristik V-I?", "id": "Grafik Hubungan Antara Tegangan Dan Arus." },
  { "en": "Apa Itu Titik Operasi (Quiescent Point)?", "id": "Titik Operasi DC Suatu Perangkat." },
  { "en": "Apa Itu Garis Beban (Load Line)?", "id": "Garis Pada Karakteristik Yang Mewakili Beban." },
  { "en": "Apa Itu Kelas Penguat (Amplifier Class)?", "id": "Klasifikasi Penguat Berdasarkan Karakteristik." },
  { "en": "Apa Itu Penguat Kelas A?", "id": "Transistor Selalu Aktif, Efisiensi Rendah." },
  { "en": "Apa Itu Penguat Kelas B?", "id": "Transistor Aktif Setengah Siklus." },
  { "en": "Apa Itu Penguat Kelas AB?", "id": "Kompromi Antara Kelas A Dan B." },
  { "en": "Apa Itu Penguat Kelas C?", "id": "Transistor Aktif Kurang Dari Setengah Siklus." },
  { "en": "Apa Itu Penguat Kelas D?", "id": "Penguat Switching Yang Sangat Efisien." },
  { "en": "Apakah Konverter Daya Termasuk Penguat Kelas D?", "id": "Ya, Beroperasi Dengan Prinsip Switching." },
  { "en": "Apa Itu Passivation?", "id": "Melapisi Permukaan Semikonduktor." },
  { "en": "Apa Itu Epitaxy?", "id": "Proses Menumbuhkan Lapisan Kristal Tipis." },
  { "en": "Apa Itu Wafer?", "id": "Potongan Tipis Bahan Semikonduktor." },
  { "en": "Apa Itu Die (Semiconductor)?", "id": "Potongan Kecil Wafer Yang Berisi Sirkuit." },
  { "en": "Apa Itu Wire Bonding?", "id": "Menghubungkan Die Ke Kaki Paket." },
  { "en": "Apa Itu Packaging?", "id": "Enkapsulasi Perangkat Semikonduktor." },
  { "en": "Apa Itu Modul Daya?", "id": "Paket Yang Berisi Beberapa Perangkat Daya." },
  { "en": "Apa Itu Intelligent Power Module (IPM)?", "id": "Modul Daya Dengan Rangkaian Kontrol Terintegrasi." },
  { "en": "Apa Itu Thermal Interface Material (TIM)?", "id": "Bahan Untuk Meningkatkan Transfer Panas." },
  { "en": "Di Mana TIM Digunakan?", "id": "Antara Perangkat Daya Dan Heatsink." },
  { "en": "Apa Itu Resistansi Kontak Termal?", "id": "Resistansi Termal Pada Antarmuka." },
  { "en": "Apa Itu Siklus Termal (Thermal Cycling)?", "id": "Fluktuasi Suhu Berulang." },
  { "en": "Apa Efek Dari Siklus Termal?", "id": "Dapat Menyebabkan Kegagalan Mekanis." },
  { "en": "Apa Itu Koefisien Ekspansi Termal?", "id": "Ukuran Perubahan Ukuran Bahan Dengan Suhu." },
  { "en": "Apa Itu Finite Element Analysis (FEA)?", "id": "Metode Numerik Untuk Analisis Teknik." },
  { "en": "Bagaimana FEA Digunakan Dalam Elektronika Daya?", "id": "Untuk Analisis Termal Dan Mekanis." },
  { "en": "Apa Itu Computational Fluid Dynamics (CFD)?", "id": "Metode Untuk Menganalisis Aliran Fluida." },
  { "en": "Bagaimana CFD Digunakan?", "id": "Untuk Mendesain Sistem Pendingin Udara Atau Cair." },
  { "en": "Apa Itu Topologi Resonan Seri?", "id": "Rangkaian Resonan Dimana L Dan C Seri." },
  { "en": "Apa Itu Topologi Resonan Paralel?", "id": "Rangkaian Resonan Dimana L Dan C Paralel." },
  { "en": "Apa Itu Frekuensi Resonansi?", "id": "Frekuensi Dimana Impedansi Rangkaian Minimal." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor)?", "id": "Ukuran Kualitas Rangkaian Resonan." },
  { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Operasi." },
  { "en": "Apa Itu Topologi Tiga Tingkat NPC?", "id": "Neutral-Point Clamped." },
  { "en": "Apa Itu Diode Penjepit (Clamping Diode)?", "id": "Dioda Yang Digunakan Dalam Topologi NPC." },
  { "en": "Apa Itu Kontrol V/f?", "id": "Metode Kontrol Motor Induksi Sederhana." },
  { "en": "Apa Itu Kontrol Vektor (Field-Oriented Control)?", "id": "Metode Kontrol Motor AC Kinerja Tinggi." },
  { "en": "Apa Itu Direct Torque Control (DTC)?", "id": "Metode Kontrol Motor AC Lainnya." },
  { "en": "Apa Itu Komutasi Elektronik?", "id": "Digunakan Dalam Motor BLDC." },
  { "en": "Apa Itu Sensor Efek Hall?", "id": "Sensor Untuk Mendeteksi Posisi Rotor." },
  { "en": "Apa Itu Sensorless Control?", "id": "Kontrol Motor Tanpa Menggunakan Sensor Posisi." },
  { "en": "Bagaimana Kontrol Sensorless Bekerja?", "id": "Mengestimasi Posisi Dari Tegangan Atau Arus." },
  { "en": "Apa Itu Back EMF (Electromotive Force)?", "id": "Tegangan Yang Diinduksi Di Kumparan Motor." },
  { "en": "Apa Itu Slip?", "id": "Perbedaan Antara Kecepatan Sinkron Dan Rotor." },
  { "en": "Apa Itu Kecepatan Sinkron?", "id": "Kecepatan Putaran Medan Magnet Stator." },
  { "en": "Apa Itu Torsi?", "id": "Gaya Putar Yang Dihasilkan Motor." },
  { "en": "Apa Itu Konverter AC-DC Empat-Kuadran?", "id": "Dapat Berfungsi Sebagai Penyearah Atau Inverter." },
  { "en": "Apa Itu Vienna Rectifier?", "id": "Penyearah PFC Tiga-Fasa Tiga-Tingkat." },
  { "en": "Apa Itu Standar IEC?", "id": "Standar Dari International Electrotechnical Commission." },
  { "en": "Apa Itu Standar IEEE?", "id": "Standar Dari Institute of Electrical and Electronics Engineers." },
  { "en": "Apa Itu Standar UL?", "id": "Standar Keamanan Dari Underwriters Laboratories." },
  { "en": "Apa Itu Tanda CE?", "id": "Tanda Sertifikasi Untuk Produk Di Eropa." },
  { "en": "Apa Itu RoHS (Restriction of Hazardous Substances)?", "id": "Direktif Eropa Yang Membatasi Bahan Berbahaya." },
  { "en": "Apa Itu WEEE (Waste Electrical and Electronic Equipment)?", "id": "Direktif Eropa Untuk Limbah Elektronik." },
  { "en": "Apa Itu EMI Filter?", "id": "Filter Untuk Menekan Interferensi Elektromagnetik." },
  { "en": "Apa Itu Kapasitor X?", "id": "Kapasitor Keamanan Yang Dipasang Antar-Jalur." },
  { "en": "Apa Itu Kapasitor Y?", "id": "Kapasitor Keamanan Yang Dipasang Antara Jalur-Ground." },
  { "en": "Apa Itu Creepage?", "id": "Jarak Terpendek Antara Konduktor Di Permukaan." },
  { "en": "Apa Itu Clearance?", "id": "Jarak Terpendek Antara Konduktor Melalui Udara." },
  { "en": "Mengapa Creepage Dan Clearance Penting?", "id": "Untuk Keamanan Dan Pencegahan Busur Api." },
  { "en": "Apa Itu Isolasi Dasar?", "id": "Isolasi Tunggal Untuk Perlindungan Dasar." },
  { "en": "Apa Itu Isolasi Ganda?", "id": "Dua Lapisan Isolasi Independen." },
  { "en": "Apa Itu Isolasi Diperkuat?", "id": "Isolasi Tunggal Yang Setara Dengan Ganda." },
  { "en": "Apa Itu Potential Transformer (PT)?", "id": "Transformator Untuk Mengukur Tegangan Tinggi." },
  { "en": "Apa Itu Current Transformer (CT)?", "id": "Transformator Untuk Mengukur Arus Tinggi." },
  { "en": "Apa Itu Pengukuran Empat Titik (Kelvin)?", "id": "Metode Untuk Mengukur Resistansi Rendah." },
  { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu Berdasarkan Efek Seebeck." },
  { "en": "Apa Itu Resistance Temperature Detector (RTD)?", "id": "Sensor Suhu Yang Resistansinya Berubah." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Sensitif Termal." },
  { "en": "Apa Itu NTC (Negative Temperature Coefficient) Thermistor?", "id": "Resistansi Turun Saat Suhu Naik." },
  { "en": "Apa Itu PTC (Positive Temperature Coefficient) Thermistor?", "id": "Resistansi Naik Saat Suhu Naik." },
  { "en": "Apa Itu Osiloskop?", "id": "Instrumen Untuk Melihat Bentuk Gelombang Tegangan." },
  { "en": "Apa Itu Probe Diferensial?", "id": "Probe Osiloskop Untuk Mengukur Tegangan Diferensial." },
  { "en": "Apa Itu Penganalisis Spektrum (Spectrum Analyzer)?", "id": "Instrumen Untuk Melihat Sinyal Dalam Domain Frekuensi." },
  { "en": "Apa Itu Penganalisis Jaringan Vektor (VNA)?", "id": "Mengukur Parameter Jaringan Perangkat Frekuensi Radio." },
  { "en": "Apa Itu Beban Elektronik (Electronic Load)?", "id": "Instrumen Untuk Menguji Catu Daya." },
  { "en": "Apa Itu Sumber Daya Terprogram?", "id": "Catu Daya Yang Dapat Dikontrol Secara Digital." },
  { "en": "Apa Itu Standar Kualitas Daya IEEE 519?", "id": "Standar Untuk Batas Distorsi Harmonik." },
  { "en": "Apa Itu Power System Stabilizer (PSS)?", "id": "Sistem Kontrol Untuk Meredam Osilasi Daya." },
  { "en": "Apa Itu Osilasi Daya?", "id": "Fluktuasi Daya Aktif Antara Generator." },
  { "en": "Apa Itu Black Start?", "id": "Memulihkan Pembangkit Listrik Tanpa Daya Eksternal." },
  { "en": "Apa Itu Islanding?", "id": "Kondisi Dimana Generator Terdistribusi Terisolasi." },
  { "en": "Apa Itu Point of Common Coupling (PCC)?", "id": "Titik Dimana Jaringan Pengguna Terhubung." },
  { "en": "Apa Itu Sag Correction?", "id": "Kompensasi Untuk Penurunan Tegangan." },
  { "en": "Apa Itu Inverter Siap Jaringan (Grid-Ready Inverter)?", "id": "Inverter Dengan Fitur Pendukung Jaringan." },
  { "en": "Apa Itu Volt-VAR Control?", "id": "Fitur Inverter Untuk Mengatur Tegangan." },
  { "en": "Apa Itu Frequency-Watt Control?", "id": "Fitur Inverter Untuk Menstabilkan Frekuensi." },
  { "en": "Apa Itu Ride-Through Capability?", "id": "Kemampuan Tetap Terhubung Selama Gangguan." },
  { "en": "Apa Itu Low Voltage Ride-Through (LVRT)?", "id": "Kemampuan Melewati Gangguan Tegangan Rendah." },
  { "en": "Apa Itu High Voltage Ride-Through (HVRT)?", "id": "Kemampuan Melewati Gangguan Tegangan Tinggi." },
  { "en": "Apa Itu Konverter Sumber Impedansi?", "id": "Nama Lain Untuk Konverter Tipe-Z." },
  { "en": "Apa Itu Damping?", "id": "Proses Meredam Atau Mengurangi Osilasi." },
  { "en": "Apa Itu Rangkaian Damping?", "id": "Rangkaian Untuk Mengurangi Osilasi Listrik." },
  { "en": "Apa Itu Faktor Daya Perpindahan?", "id": "Faktor Daya Akibat Pergeseran Fasa." },
  { "en": "Apa Itu Faktor Daya Distorsi?", "id": "Faktor Daya Akibat Distorsi Harmonik." },
  { "en": "Apa Itu Total Demand Distortion (TDD)?", "id": "Ukuran Distorsi Arus Harmonik." },
  { "en": "Apa Itu Interharmonik?", "id": "Harmonik Dengan Frekuensi Bukan Kelipatan Fundamental." },
  { "en": "Apa Itu Notching?", "id": "Distorsi Periodik Pada Tegangan AC." },
  { "en": "Apa Penyebab Notching?", "id": "Komutasi Pada Konverter Daya." },
  { "en": "Apa Itu Flicker?", "id": "Fluktuasi Kecerahan Lampu Yang Terlihat." },
  { "en": "Apa Penyebab Flicker?", "id": "Fluktuasi Cepat Pada Tegangan Suplai." },
  { "en": "Apa Itu Kontrol Histeresis?", "id": "Metode Kontrol Dengan Batas Atas Dan Bawah." },
  { "en": "Apa Itu Kontrol Bang-Bang?", "id": "Kontroler Yang Hanya Memiliki Dua Keadaan." },
  { "en": "Apa Itu State Machine?", "id": "Model Matematis Dengan Keadaan Dan Transisi." },
  { "en": "Apa Itu Look-Up Table (LUT)?", "id": "Tabel Yang Menyimpan Nilai Yang Telah Dihitung." },
  { "en": "Apa Itu CORDIC (Coordinate Rotation Digital Computer)?", "id": "Algoritma Untuk Menghitung Fungsi Trigonometri." },
  { "en": "Apa Itu Floating Point?", "id": "Representasi Angka Dengan Titik Desimal." },
  { "en": "Apa Itu Fixed Point?", "id": "Representasi Angka Dengan Posisi Titik Tetap." },
  { "en": "Mana Yang Lebih Cepat: Fixed Atau Floating?", "id": "Operasi Fixed Point Biasanya Lebih Cepat." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Timer Untuk Mereset Sistem Jika Terjadi Kegagalan." },
  { "en": "Apa Itu Galvanometer?", "id": "Aktuator Elektromekanis Yang Menghasilkan Defleksi." },
  { "en": "Apa Itu Pemrosesan Sinyal?", "id": "Analisis, Modifikasi, Atau Sintesis Sinyal." },
  { "en": "Apa Itu Filter Low-Pass?", "id": "Melewatkan Sinyal Frekuensi Rendah." },
  { "en": "Apa Itu Filter High-Pass?", "id": "Melewatkan Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Filter Band-Pass?", "id": "Melewatkan Sinyal Dalam Rentang Frekuensi." },
  { "en": "Apa Itu Filter Band-Stop?", "id": "Menolak Sinyal Dalam Rentang Frekuensi." },
  { "en": "Apa Itu Respon Impuls?", "id": "Output Filter Terhadap Input Impuls." },
  { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Untuk Menggabungkan Dua Sinyal." },
  { "en": "Apa Itu Transformasi Fourier?", "id": "Mengubah Sinyal Ke Domain Frekuensi." },
  { "en": "Apa Itu Fast Fourier Transform (FFT)?", "id": "Algoritma Cepat Untuk Menghitung DFT." },
  { "en": "Apa Itu Discrete Fourier Transform (DFT)?", "id": "Transformasi Fourier Untuk Sinyal Diskrit." },
  { "en": "Apa Itu Superposisi?", "id": "Prinsip Dimana Efek Total Adalah Jumlah Efek." },
  { "en": "Apa Itu Sirkuit Linier?", "id": "Sirkuit Yang Memenuhi Prinsip Superposisi." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Menyederhanakan Sirkuit Linier Menjadi Sumber Tegangan." },
  { "en": "Apa Itu Teorema Norton?", "id": "Menyederhanakan Sirkuit Linier Menjadi Sumber Arus." },
  { "en": "Apa Itu Transfer Daya Maksimum?", "id": "Terjadi Saat Resistansi Beban Sama Dengan Sumber." },
  { "en": "Apa Itu Impedansi?", "id": "Ukuran Total Hambatan Terhadap Arus AC." },
  { "en": "Apa Itu Reaktansi?", "id": "Bagian Imajiner Dari Impedansi." },
  { "en": "Apa Itu Reaktansi Induktif?", "id": "Hambatan Terhadap Arus AC Oleh Induktor." },
  { "en": "Apa Itu Reaktansi Kapasitif?", "id": "Hambatan Terhadap Arus AC Oleh Kapasitor." },
  { "en": "Apa Itu Admitansi?", "id": "Kebalikan Dari Impedansi." },
  { "en": "Apa Itu Konduktansi?", "id": "Bagian Nyata Dari Admitansi." },
  { "en": "Apa Itu Suseptansi?", "id": "Bagian Imajiner Dari Admitansi." },
  { "en": "Apa Itu Bilangan Kompleks?", "id": "Bilangan Dengan Bagian Nyata Dan Imajiner." },
  { "en": "Apa Itu Fasor?", "id": "Representasi Bilangan Kompleks Dari Sinyal Sinusoidal." },
  { "en": "Apa Itu Nilai RMS (Root Mean Square)?", "id": "Nilai Efektif Dari Tegangan Atau Arus AC." },
  { "en": "Apa Itu Crest Factor?", "id": "Rasio Nilai Puncak Terhadap Nilai RMS." },
  { "en": "Apa Itu Arus Bolak-balik (AC)?", "id": "Arus Listrik Yang Arahnya Berubah Periodik." },
  { "en": "Apa Itu Arus Searah (DC)?", "id": "Arus Listrik Yang Mengalir Dalam Satu Arah." },
  { "en": "Apa Itu Frekuensi?", "id": "Jumlah Siklus Per Detik." },
  { "en": "Apa Itu Periode?", "id": "Waktu Untuk Menyelesaikan Satu Siklus." },
  { "en": "Apa Itu Amplitudo?", "id": "Nilai Puncak Maksimum Dari Gelombang." },
  { "en": "Apa Itu Fasa?", "id": "Posisi Relatif Titik Dalam Waktu." },
  { "en": "Apa Itu Pergeseran Fasa?", "id": "Perbedaan Fasa Antara Dua Gelombang." },
  { "en": "Apa Itu Sirkuit RLC?", "id": "Sirkuit Yang Mengandung Resistor, Induktor, Kapasitor." },
  { "en": "Apa Itu Resonansi?", "id": "Kondisi Osilasi Amplitudo Besar." },
  { "en": "Apa Itu Faktor Disipasi?", "id": "Ukuran Kehilangan Energi Dalam Sistem." },
  { "en": "Apa Itu Tangen Rugi (Loss Tangent)?", "id": "Ukuran Lain Dari Faktor Disipasi." },
  { "en": "Apa Itu Dielektrik?", "id": "Bahan Isolator Listrik." },
  { "en": "Apa Itu Konstanta Dielektrik?", "id": "Ukuran Kemampuan Bahan Menyimpan Energi." },
  { "en": "Apa Itu Kekuatan Dielektrik?", "id": "Medan Listrik Maksimum Yang Dapat Ditahan." },
  { "en": "Apa Itu Busur Api (Arc Flash)?", "id": "Ledakan Listrik Berbahaya." },
  { "en": "Apa Itu Energi Insiden?", "id": "Ukuran Energi Termal Dari Busur Api." },
  { "en": "Apa Itu Personal Protective Equipment (PPE)?", "id": "Peralatan Pelindung Diri." },
  { "en": "Apa Itu Standar NFPA 70E?", "id": "Standar Untuk Keselamatan Listrik Di Tempat Kerja." },
  { "en": "Apa Itu Lockout-Tagout (LOTO)?", "id": "Prosedur Keselamatan Untuk Menonaktifkan Mesin." },
  { "en": "Apa Itu Efisiensi Kuantum?", "id": "Rasio Elektron Yang Dikumpulkan Terhadap Foton." },
  { "en": "Apa Itu Sistem Tenaga Listrik Otonom?", "id": "Sistem Yang Dapat Beroperasi Secara Mandiri." },
  { "en": "Apa Itu Topologi Cincin (Ring)?", "id": "Topologi Jaringan Listrik." },
  { "en": "Apa Itu Topologi Radial?", "id": "Topologi Jaringan Listrik Paling Sederhana." },
  { "en": "Apa Itu Keandalan Sistem Tenaga?", "id": "Kemampuan Menyediakan Listrik Tanpa Interupsi." },
  { "en": "Apa Itu Indeks Keandalan SAIDI?", "id": "System Average Interruption Duration Index." },
  { "en": "Apa Itu Indeks Keandalan SAIFI?", "id": "System Average Interruption Frequency Index." },
  { "en": "Apa Itu Konverter AC-AC Tak Langsung?", "id": "Memiliki Tautan DC (DC Link) Di Tengah." },
  { "en": "Apa Itu Tautan DC (DC Link)?", "id": "Tahap DC Antara Penyearah Dan Inverter." },
  { "en": "Apa Fungsi Kapasitor Tautan DC?", "id": "Menstabilkan Tegangan DC Dan Menyimpan Energi." },
  { "en": "Apa Itu Konverter Back-to-Back?", "id": "Dua Konverter Yang Terhubung Melalui Tautan DC." },
  { "en": "Apa Itu Kontrol Sisi Jaringan (Grid-Side Control)?", "id": "Mengontrol Aliran Daya Ke Jaringan." },
  { "en": "Apa Itu Kontrol Sisi Mesin (Machine-Side Control)?", "id": "Mengontrol Operasi Mesin Listrik." },
  { "en": "Apa Itu Estimasi Keadaan (State Estimation)?", "id": "Memperkirakan Keadaan Internal Sistem." },
  { "en": "Apa Itu Observer?", "id": "Model Matematis Untuk Mengestimasi Keadaan." },
  { "en": "Apa Itu Sliding Mode Control (SMC)?", "id": "Teknik Kontrol Non-linier Yang Kokoh." },
  { "en": "Apa Itu Permukaan Geser (Sliding Surface)?", "id": "Permukaan Dalam Ruang Keadaan." },
  { "en": "Apa Itu Chattering?", "id": "Osilasi Frekuensi Tinggi Dalam Kontrol SMC." },
  { "en": "Apa Itu Kontrol Adaptif?", "id": "Kontroler Yang Menyesuaikan Parameternya." },
  { "en": "Apa Itu Kontrol Kokoh (Robust Control)?", "id": "Kontroler Yang Tidak Sensitif Terhadap Ketidakpastian." },
  { "en": "Apa Itu Optimal Control?", "id": "Menemukan Sinyal Kontrol Optimal." },
  { "en": "Apa Itu Kriteria Kinerja?", "id": "Fungsi Yang Harus Diminimalkan Atau Dimaksimalkan." },
  { "en": "Apa Itu Konverter Dual-Active Bridge (DAB)?", "id": "Konverter DC-DC Terisolasi Dua Arah." },
  { "en": "Apa Keuntungan Konverter DAB?", "id": "Kepadatan Daya Tinggi Dan Soft-Switching." },
  { "en": "Apa Itu Pergeseran Fasa?", "id": "Parameter Kontrol Utama Dalam Konverter DAB." },
  { "en": "Apa Itu Model Sinyal Kecil?", "id": "Model Linier Sistem Non-linier." },
  { "en": "Apa Itu Analisis Rata-rata?", "id": "Menyederhanakan Analisis Konverter Switching." },
  { "en": "Apa Itu Mode Simetris?", "id": "Operasi Konverter Dengan Pola Switching Simetris." },
  { "en": "Apa Itu Mode Asimetris?", "id": "Operasi Dengan Pola Switching Tidak Simetris." },
  { "en": "Apa Itu Konverter Fasa-Tunggal ke Tiga-Fasa?", "id": "Menghasilkan Output Tiga-Fasa Dari Input Fasa-Tunggal." },
  { "en": "Apa Itu Topologi Scott-T?", "id": "Menggunakan Dua Transformator Untuk Konversi Fasa." },
  { "en": "Apa Itu Sistem Embedded?", "id": "Sistem Komputer Dengan Fungsi Khusus." },
  { "en": "Apa Itu Real-Time Operating System (RTOS)?", "id": "Sistem Operasi Untuk Aplikasi Real-time." },
  { "en": "Apa Itu Tugas (Task) Dalam RTOS?", "id": "Unit Eksekusi Dalam Program." },
  { "en": "Apa Itu Penjadwal (Scheduler)?", "id": "Bagian RTOS Yang Mengatur Eksekusi Tugas." },
  { "en": "Apa Itu Prioritas Tugas?", "id": "Menentukan Urutan Eksekusi Tugas." },
  { "en": "Apa Itu Interupsi?", "id": "Sinyal Yang Menghentikan Prosesor Sementara." },
  { "en": "Apa Itu Interrupt Service Routine (ISR)?", "id": "Fungsi Yang Dijalankan Saat Interupsi." },
  { "en": "Apa Itu Jitter?", "id": "Variasi Waktu Dari Sinyal Periodik." },
  { "en": "Apa Itu Laten?", "id": "Waktu Tunda Antara Stimulus Dan Respons." },
  { "en": "Apa Itu CAN Bus?", "id": "Protokol Komunikasi Standar Dalam Otomotif." },
  { "en": "Apa Itu MODBUS?", "id": "Protokol Komunikasi Untuk Otomasi Industri." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Kontrol Untuk Proses Industri." },
  { "en": "Apa Itu Programmable Logic Controller (PLC)?", "id": "Komputer Digital Untuk Otomasi Industri." },
  { "en": "Apa Itu Ladder Logic?", "id": "Bahasa Pemrograman Grafis Untuk PLC." },
  { "en": "Apa Itu Human-Machine Interface (HMI)?", "id": "Antarmuka Pengguna Untuk Sistem Kontrol." },
  { "en": "Apa Itu Kontrol Umpan Maju (Feedforward)?", "id": "Mengantisipasi Gangguan Sebelum Mempengaruhi Output." },
  { "en": "Apa Itu Decoupling Control?", "id": "Mengontrol Sistem MIMO Seolah-olah SISO." },
  { "en": "Apa Itu Sistem SISO (Single-Input Single-Output)?", "id": "Sistem Dengan Satu Input Dan Satu Output." },
  { "en": "Apa Itu Sistem MIMO (Multiple-Input Multiple-Output)?", "id": "Sistem Dengan Banyak Input Dan Output." },
  { "en": "Apa Itu Sensor?", "id": "Mengubah Besaran Fisik Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Aktuator?", "id": "Mengubah Sinyal Listrik Menjadi Gerakan Fisik." },
  { "en": "Apa Itu Transduser?", "id": "Mengubah Satu Bentuk Energi Ke Bentuk Lain." },
  { "en": "Apa Itu Pengkondisian Sinyal?", "id": "Memproses Sinyal Agar Sesuai Untuk Pengukuran." },
  { "en": "Apa Itu Amplifier?", "id": "Menguatkan Amplitudo Sinyal." },
  { "en": "Apa Itu Filter?", "id": "Menghilangkan Komponen Frekuensi Yang Tidak Diinginkan." },
  { "en": "Apa Itu Noise?", "id": "Sinyal Acak Yang Tidak Diinginkan." },
  { "en": "Apa Itu Shielding?", "id": "Melindungi Rangkaian Dari Interferensi Elektromagnetik." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Kabel Dengan Konduktor Pusat Dan Pelindung." },
  { "en": "Apa Itu Kabel Twisted Pair?", "id": "Dua Kabel Terpilin Untuk Mengurangi Interferensi." },
  { "en": "Apa Itu Arus Bocor (Leakage Current)?", "id": "Arus Yang Mengalir Melalui Jalur Isolasi." },
  { "en": "Apa Itu Pengukuran Hi-Pot?", "id": "Menguji Kekuatan Dielektrik Isolasi." },
  { "en": "Apa Itu Resistansi Isolasi?", "id": "Resistansi Sangat Tinggi Dari Bahan Isolator." },
  { "en": "Apa Itu Megohmmeter?", "id": "Instrumen Untuk Mengukur Resistansi Isolasi." },
  { "en": "Apa Itu Pemanasan Induksi?", "id": "Memanaskan Benda Konduktif Dengan Medan Magnet." },
  { "en": "Jenis Konverter Apa Yang Digunakan Pemanasan Induksi?", "id": "Inverter Resonan Berfrekuensi Tinggi." },
  { "en": "Apa Itu Catu Daya Las?", "id": "Menyediakan Arus Tinggi Untuk Proses Pengelasan." },
  { "en": "Apa Itu Mode Arus Konstan (CC)?", "id": "Catu Daya Mengatur Arus Output." },
  { "en": "Apa Itu Mode Tegangan Konstan (CV)?", "id": "Catu Daya Mengatur Tegangan Output." },
  { "en": "Apa Itu Power Supply Laboratorium?", "id": "Catu Daya Serbaguna Untuk Eksperimen." },
  { "en": "Apa Itu Pengganda Tegangan?", "id": "Sirkuit Untuk Menghasilkan Tegangan DC Tinggi." },
  { "en": "Apa Itu Cockcroft-Walton Generator?", "id": "Jenis Sirkuit Pengganda Tegangan." },
  { "en": "Apa Itu Pembangkit Marx?", "id": "Menghasilkan Pulsa Tegangan Sangat Tinggi." },
  { "en": "Apa Itu Plasma?", "id": "Keadaan Materi Keempat, Gas Terionisasi." },
  { "en": "Apa Itu Catu Daya Plasma?", "id": "Menyediakan Daya Untuk Menghasilkan Plasma." },
  { "en": "Apa Itu Magnetron?", "id": "Tabung Vakum Untuk Menghasilkan Gelombang Mikro." },
  { "en": "Di Mana Magnetron Digunakan?", "id": "Dalam Oven Microwave." },
  { "en": "Apa Itu Tabung Sinar-X?", "id": "Sumber Sinar-X Untuk Pencitraan Medis." },
  { "en": "Apa Itu Catu Daya Sinar-X?", "id": "Menyediakan Tegangan Sangat Tinggi Untuk Tabung." },
  { "en": "Apa Itu Efisiensi Dinding-Steker (Wall-Plug Efficiency)?", "id": "Efisiensi Total Dari Stopkontak Ke Output." },
  { "en": "Apa Itu Konsumsi Daya Siaga (Standby)?", "id": "Daya Yang Dikonsumsi Saat Perangkat Mati." },
  { "en": "Apa Itu Energy Star?", "id": "Program Efisiensi Energi Internasional." },
  { "en": "Apa Itu 80 Plus?", "id": "Program Sertifikasi Efisiensi Untuk Catu Daya." },
  { "en": "Apa Itu Konduksi Termal?", "id": "Transfer Panas Melalui Kontak Langsung." },
  { "en": "Apa Itu Konveksi Termal?", "id": "Transfer Panas Melalui Gerakan Fluida." },
  { "en": "Apa Itu Radiasi Termal?", "id": "Transfer Panas Melalui Gelombang Elektromagnetik." },
  { "en": "Apa Itu Emisivitas?", "id": "Ukuran Kemampuan Benda Memancarkan Radiasi." },
  { "en": "Apa Itu Benda Hitam?", "id": "Benda Ideal Yang Menyerap Semua Radiasi." },
  { "en": "Apa Itu Termografi?", "id": "Teknik Pencitraan Menggunakan Radiasi Inframerah." },
  { "en": "Apa Itu Analisis Sirkuit?", "id": "Proses Menentukan Tegangan Dan Arus." },
  { "en": "Apa Itu Hukum Arus Kirchhoff (KCL)?", "id": "Jumlah Arus Masuk Sama Dengan Arus Keluar." },
  { "en": "Apa Itu Hukum Tegangan Kirchhoff (KVL)?", "id": "Jumlah Tegangan Dalam Loop Tertutup Adalah Nol." },
  { "en": "Apa Itu Simpul (Node)?", "id": "Titik Pertemuan Dua Atau Lebih Komponen." },
  { "en": "Apa Itu Cabang (Branch)?", "id": "Jalur Antara Dua Simpul." },
  { "en": "Apa Itu Loop?", "id": "Jalur Tertutup Dalam Sebuah Sirkuit." },
  { "en": "Apa Itu Analisis Simpul (Nodal Analysis)?", "id": "Metode Analisis Sirkuit Berbasis KCL." },
  { "en": "Apa Itu Analisis Mesh?", "id": "Metode Analisis Sirkuit Berbasis KVL." },
  { "en": "Apa Itu Sumber Tegangan?", "id": "Menyediakan Tegangan Konstan Terlepas Dari Arus." },
  { "en": "Apa Itu Sumber Arus?", "id": "Menyediakan Arus Konstan Terlepas Dari Tegangan." },
  { "en": "Apa Itu Sumber Dependen?", "id": "Sumber Yang Nilainya Bergantung Pada Variabel Lain." },
  { "en": "Apa Itu Sumber Independen?", "id": "Sumber Yang Nilainya Tidak Bergantung Pada Apapun." },
  { "en": "Apa Itu Konduktansi?", "id": "Kemudahan Arus Listrik Mengalir." },
  { "en": "Apa Satuan Konduktansi?", "id": "Siemens (S)." },
  { "en": "Apa Itu Linearitas?", "id": "Sifat Sistem Dimana Output Sebanding Input." },
  { "en": "Apa Itu Rangkaian Seri?", "id": "Komponen Terhubung Ujung Ke Ujung." },
  { "en": "Apa Itu Rangkaian Paralel?", "id": "Komponen Terhubung Pada Pasangan Simpul Sama." },
  { "en": "Apa Itu Pembagi Tegangan?", "id": "Sirkuit Resistor Seri Untuk Menurunkan Tegangan." },
  { "en": "Apa Itu Pembagi Arus?", "id": "Sirkuit Resistor Paralel Untuk Membagi Arus." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel Tiga Terminal." },
  { "en": "Apa Itu Rheostat?", "id": "Resistor Variabel Dua Terminal." },
  { "en": "Apa Itu Termoelektrik?", "id": "Bidang Yang Mempelajari Konversi Panas-Listrik." },
  { "en": "Apa Itu Generator Termoelektrik (TEG)?", "id": "Mengubah Panas Langsung Menjadi Listrik." },
  { "en": "Apa Itu Pendingin Termoelektrik (TEC)?", "id": "Memompa Panas Menggunakan Listrik." },
  { "en": "Apa Itu Faktor Merit Termoelektrik (ZT)?", "id": "Ukuran Kinerja Bahan Termoelektrik." },
  { "en": "Apa Itu Piezoelektrik?", "id": "Kemampuan Bahan Menghasilkan Tegangan Saat Ditekan." },
  { "en": "Apa Itu Efek Piezoelektrik Terbalik?", "id": "Bahan Berubah Bentuk Saat Diberi Tegangan." },
  { "en": "Apa Aplikasi Efek Piezoelektrik?", "id": "Sensor, Aktuator, Dan Penghasil Frekuensi." },
  { "en": "Apa Itu Resonator Kristal?", "id": "Komponen Elektronik Berbasis Resonansi Piezoelektrik." },
  { "en": "Apa Itu Piroelektrik?", "id": "Kemampuan Bahan Menghasilkan Tegangan Akibat Suhu." },
  { "en": "Apa Itu Ferroelektrik?", "id": "Bahan Dengan Polarisasi Listrik Spontan." },
  { "en": "Apa Itu Kurva Histeresis Ferroelektrik?", "id": "Grafik Polarisasi Terhadap Medan Listrik." },
  { "en": "Apa Itu Dielektrik?", "id": "Isolator Listrik Yang Dapat dipolarisasi." },
  { "en": "Apa Itu Medan Listrik?", "id": "Gaya Per Satuan Muatan." },
  { "en": "Apa Itu Medan Magnet?", "id": "Medan Yang Dihasilkan Oleh Muatan Bergerak." },
  { "en": "Apa Itu Gaya Lorentz?", "id": "Gaya Pada Muatan Bergerak Dalam Medan." },
  { "en": "Apa Itu Hukum Ampere?", "id": "Menghubungkan Arus Dengan Medan Magnet." },
  { "en": "Apa Itu Hukum Faraday Tentang Induksi?", "id": "Medan Magnet Berubah Menginduksi Tegangan." },
  { "en": "Apa Itu Elektromagnet?", "id": "Magnet Yang Dibuat Dengan Aliran Arus." },
  { "en": "Apa Itu Solenoida?", "id": "Kumparan Kawat Berbentuk Heliks." },
  { "en": "Apa Itu Toroida?", "id": "Solenoida Yang Dibengkokkan Menjadi Bentuk Donat." },
  { "en": "Apa Itu Reluktansi?", "id": "Hambatan Terhadap Pembentukan Fluks Magnetik." },
  { "en": "Apa Itu Sirkuit Magnetik?", "id": "Jalur Tertutup Untuk Fluks Magnetik." },
  { "en": "Apa Itu Gaya Gerak Magnet (MMF)?", "id": "Penyebab Fluks Dalam Sirkuit Magnetik." },
  { "en": "Apa Itu Permeabilitas Vakum?", "id": "Konstanta Fisik Fundamental." },
  { "en": "Apa Itu Permeabilitas Relatif?", "id": "Rasio Permeabilitas Bahan Terhadap Vakum." },
  { "en": "Apa Itu Bahan Feromagnetik?", "id": "Bahan Yang Sangat Tertarik Medan Magnet." },
  { "en": "Apa Itu Bahan Paramagnetik?", "id": "Bahan Yang Sedikit Tertarik Medan Magnet." },
  { "en": "Apa Itu Bahan Diamagnetik?", "id": "Bahan Yang Sedikit Ditolak Medan Magnet." },
  { "en": "Apa Itu Titik Curie?", "id": "Suhu Dimana Bahan Feromagnetik Kehilangan Magnet." },
  { "en": "Apa Itu Magnet Permanen?", "id": "Bahan Yang Menghasilkan Medan Magnet Sendiri." },
  { "en": "Apa Itu Bus DC?", "id": "Konduktor Yang Mendistribusikan Daya DC." },
  { "en": "Apa Itu Bus AC?", "id": "Konduktor Yang Mendistribusikan Daya AC." },
  { "en": "Apa Itu Sinkronisasi Jaringan?", "id": "Menyesuaikan Frekuensi, Fasa, Tegangan." },
  { "en": "Apa Itu Sinkronoskop?", "id": "Alat Untuk Mengindikasikan Sinkronisasi." },
  { "en": "Apa Itu Pengisian Baterai?", "id": "Proses Mengisi Ulang Energi Baterai." },
  { "en": "Apa Itu Mode Arus Konstan (CC)?", "id": "Mengisi Dengan Arus Yang Tetap." },
  { "en": "Apa Itu Mode Tegangan Konstan (CV)?", "id": "Mengisi Dengan Tegangan Yang Tetap." },
  { "en": "Apa Itu Pengisian Tetes (Trickle Charging)?", "id": "Mengisi Baterai Penuh Dengan Arus Rendah." },
  { "en": "Apa Itu Sistem Manajemen Baterai (BMS)?", "id": "Sistem Elektronik Yang Mengelola Baterai." },
  { "en": "Apa Fungsi Utama BMS?", "id": "Melindungi Baterai, Memantau, Menyeimbangkan Sel." },
  { "en": "Apa Itu Penyeimbangan Sel (Cell Balancing)?", "id": "Menyamakan Keadaan Pengisian Sel-sel Seri." },
  { "en": "Apa Itu Penyeimbangan Pasif?", "id": "Membuang Energi Dari Sel Bertegangan Tinggi." },
  { "en": "Apa Itu Penyeimbangan Aktif?", "id": "Memindahkan Energi Antar Sel." },
  { "en": "Apa Itu Pengujian Non-Destruktif?", "id": "Metode Evaluasi Tanpa Merusak Komponen." },
  { "en": "Apa Itu Pengujian Destruktif?", "id": "Pengujian Yang Menyebabkan Kerusakan Pada Sampel." },
  { "en": "Apa Itu Kegagalan Lelah (Fatigue)?", "id": "Pelemahan Material Akibat Beban Berulang." },
  { "en": "Apa Itu Kerapuhan (Brittleness)?", "id": "Sifat Material Yang Mudah Patah." },
  { "en": "Apa Itu Keuletan (Ductility)?", "id": "Kemampuan Material Mengalami Deformasi." },
  { "en": "Apa Itu Konduktivitas Termal?", "id": "Kemampuan Bahan Menghantarkan Panas." },
  { "en": "Apa Itu Konduktivitas Listrik?", "id": "Ukuran Kemampuan Bahan Menghantarkan Arus." },
  { "en": "Apa Itu Konduktor?", "id": "Bahan Yang Mudah Menghantarkan Arus." },
  { "en": "Apa Itu Isolator?", "id": "Bahan Yang Sulit Menghantarkan Arus." },
  { "en": "Apa Itu Dioda Varaktor?", "id": "Dioda Yang Kapasitansinya Bervariasi Dengan Tegangan." },
  { "en": "Apa Itu Dioda Terobosan (Tunnel Diode)?", "id": "Dioda Dengan Efek Terobosan Kuantum." },
  { "en": "Apa Itu Dioda PIN?", "id": "Dioda Dengan Lapisan Intrinsik Lebar." },
  { "en": "Apa Itu Dioda Gunn?", "id": "Digunakan Sebagai Osilator Frekuensi Gelombang Mikro." },
  { "en": "Apa Itu Dioda IMPATT?", "id": "Sumber Gelombang Mikro Berdaya Tinggi." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Yang Nilainya Sangat Tergantung Suhu." },
  { "en": "Apa Itu Fotokonduktor?", "id": "Resistansi Berkurang Saat Terkena Cahaya." },
  { "en": "Apa Itu Fotodioda?", "id": "Dioda Yang Mengubah Cahaya Menjadi Arus." },
  { "en": "Apa Itu Dioda Pemancar Cahaya (LED)?", "id": "Mengubah Arus Listrik Menjadi Cahaya." },
  { "en": "Apa Itu Dioda Laser?", "id": "LED Yang Menghasilkan Cahaya Laser." },
  { "en": "Apa Itu Optocoupler?", "id": "Mengisolasi Sirkuit Menggunakan LED Dan Fotodetektor." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relai Elektronik Tanpa Bagian Bergerak." },
  { "en": "Apa Keuntungan SSR Dibanding Relai Mekanis?", "id": "Lebih Cepat, Tahan Lama, Tidak Bising." },
  { "en": "Apa Itu Relai?", "id": "Saklar Yang Dioperasikan Secara Elektrik." },
  { "en": "Apa Itu Selenoida?", "id": "Jenis Elektromagnet Untuk Menghasilkan Gerakan." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Tegangan DC Yang Sangat Tinggi." },
  { "en": "Apa Itu Konfigurasi Inverting?", "id": "Output Terbalik Fasa Dari Input." },
  { "en": "Apa Itu Konfigurasi Non-Inverting?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Op-Amp Dengan Penguatan Satu." },
  { "en": "Apa Itu Komparator?", "id": "Op-Amp Untuk Membandingkan Dua Tegangan." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator Dengan Histeresis." },
  { "en": "Apa Itu Penguat Diferensial?", "id": "Menguatkan Perbedaan Antara Dua Sinyal." },
  { "en": "Apa Itu Common-Mode Rejection Ratio (CMRR)?", "id": "Kemampuan Menolak Sinyal Common-mode." },
  { "en": "Apa Itu Penguat Instrumentasi?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Apa Itu Filter Aktif?", "id": "Filter Yang Menggunakan Komponen Aktif." },
  { "en": "Apa Itu Topologi Sallen-Key?", "id": "Topologi Populer Untuk Filter Aktif." },
  { "en": "Apa Itu Osilator?", "id": "Sirkuit Yang Menghasilkan Sinyal Periodik." },
  { "en": "Apa Itu Kriteria Barkhausen?", "id": "Syarat Untuk Terjadinya Osilasi." },
  { "en": "Apa Itu Osilator Jembatan Wien?", "id": "Osilator Yang Menghasilkan Gelombang Sinus." },
  { "en": "Apa Itu Osilator Relaksasi?", "id": "Osilator Yang Menghasilkan Gelombang Non-sinusoidal." },
  { "en": "Apa Itu Multivibrator Astabil?", "id": "Osilator Tanpa Keadaan Stabil." },
  { "en": "Apa Itu Multivibrator Monostabil?", "id": "Memiliki Satu Keadaan Stabil." },
  { "en": "Apa Itu Multivibrator Bistabil?", "id": "Memiliki Dua Keadaan Stabil." },
  { "en": "Apa Itu 555 Timer IC?", "id": "Sirkuit Terpadu Populer Untuk Timer." },
  { "en": "Apa Itu Sistem Tenaga Hibrid AC/DC?", "id": "Jaringan Yang Mendistribusikan Daya AC Dan DC." },
  { "en": "Apa Keuntungan Jaringan DC?", "id": "Efisiensi Lebih Tinggi, Integrasi Sumber Terbarukan." },
  { "en": "Apa Itu Konverter AC-AC Tanpa Tautan DC?", "id": "Dikenal Sebagai Konverter Matriks." },
  { "en": "Apa Itu Modulasi Vektor Ruang (SVM)?", "id": "Teknik PWM Untuk Inverter Tiga Fasa." },
  { "en": "Apa Itu Sel Bahan Bakar Oksida Padat (SOFC)?", "id": "Jenis Sel Bahan Bakar Suhu Tinggi." },
  { "en": "Apa Itu Konverter Resonan Multilevel?", "id": "Menggabungkan Topologi Resonan Dan Multilevel." },
  { "en": "Apa Itu Soft-Switching?", "id": "Mengurangi Kerugian Switching Dengan ZVS/ZCS." },
  { "en": "Apa Itu Desain Magnetik Terintegrasi?", "id": "Menggabungkan Beberapa Komponen Magnetik." },
  { "en": "Apa Itu Planar Magnetics?", "id": "Komponen Magnetik Dengan Kumparan Datar." },
  { "en": "Apa Keuntungan Planar Magnetics?", "id": "Profil Rendah, Repetabilitas, Manajemen Termal." },
  { "en": "Apa Itu Efek Ferranti?", "id": "Tegangan Ujung Penerima Lebih Tinggi." },
  { "en": "Kapan Efek Ferranti Terjadi?", "id": "Pada Jalur Transmisi Panjang Tanpa Beban." },
  { "en": "Apa Itu Kompensasi Seri?", "id": "Memasang Kapasitor Seri Di Jalur Transmisi." },
  { "en": "Apa Itu Kompensasi Shunt?", "id": "Memasang Reaktansi Paralel Di Jalur Transmisi." },
  { "en": "Apa Itu Induktor Shunt?", "id": "Menyerap Daya Reaktif." },
  { "en": "Apa Itu Kapasitor Shunt?", "id": "Menyediakan Daya Reaktif." },
  { "en": "Apa Itu Tap Changer?", "id": "Mengubah Rasio Lilitan Transformator." },
  { "en": "Apa Itu On-Load Tap Changer (OLTC)?", "id": "Tap Changer Yang Bekerja Saat Trafo Berbeban." },
  { "en": "Apa Itu Sistem Kendali Terdistribusi (DCS)?", "id": "Sistem Kontrol Untuk Pabrik Atau Proses." },
  { "en": "Apa Itu Redundansi?", "id": "Duplikasi Komponen Kritis Untuk Keandalan." },
  { "en": "Apa Itu Redundansi N+1?", "id": "Satu Unit Cadangan Untuk N Unit." },
  { "en": "Apa Itu Hot Swapping?", "id": "Mengganti Komponen Tanpa Mematikan Sistem." },
  { "en": "Apa Itu Power Line Communication (PLC)?", "id": "Mengirim Data Melalui Kabel Listrik." },
  { "en": "Apa Itu Narrowband PLC?", "id": "PLC Dengan Laju Data Rendah." },
  { "en": "Apa Itu Broadband PLC?", "id": "PLC Dengan Laju Data Tinggi." },
  { "en": "Apa Itu Analisis Fourier?", "id": "Memecah Sinyal Menjadi Komponen Frekuensi." },
  { "en": "Apa Itu Deret Fourier?", "id": "Representasi Fungsi Periodik." },
  { "en": "Apa Itu Domain Waktu?", "id": "Analisis Sinyal Terhadap Waktu." },
  { "en": "Apa Itu Domain Frekuensi?", "id": "Analisis Sinyal Terhadap Frekuensi." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu Dalam Waktu Dan Amplitudo." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit Dalam Waktu Dan Amplitudo." },
  { "en": "Apa Itu Alokasi Daya Optimal?", "id": "Mendistribusikan Daya Secara Efisien." },
  { "en": "Apa Itu Economic Dispatch?", "id": "Mengalokasikan Pembangkitan Untuk Meminimalkan Biaya." },
  { "en": "Apa Itu Unit Commitment?", "id": "Menentukan Pembangkit Mana Yang Harus Dijalankan." },
  { "en": "Apa Itu Aliran Daya (Power Flow)?", "id": "Analisis Aliran Listrik Dalam Jaringan." },
  { "en": "Apa Itu Bus Slack?", "id": "Simpul Referensi Dalam Analisis Aliran Daya." },
  { "en": "Apa Itu Bus Beban (PQ Bus)?", "id": "Simpul Dimana Daya Aktif Dan Reaktif Diketahui." },
  { "en": "Apa Itu Bus Generator (PV Bus)?", "id": "Simpul Dimana Daya Aktif Dan Tegangan Diketahui." },
  { "en": "Apa Itu Metode Newton-Raphson?", "id": "Metode Iteratif Untuk Menyelesaikan Aliran Daya." },
  { "en": "Apa Itu Metode Gauss-Seidel?", "id": "Metode Iteratif Lain Untuk Aliran Daya." },
  { "en": "Apa Itu Stabilitas Sistem Tenaga?", "id": "Kemampuan Sistem Untuk Tetap Sinkron." },
  { "en": "Apa Itu Stabilitas Sudut Rotor?", "id": "Kemampuan Generator Tetap Sinkron." },
  { "en": "Apa Itu Stabilitas Tegangan?", "id": "Kemampuan Sistem Mempertahankan Tegangan Stabil." },
  { "en": "Apa Itu Stabilitas Frekuensi?", "id": "Kemampuan Sistem Mempertahankan Frekuensi Stabil." },
  { "en": "Apa Itu Voltage Collapse?", "id": "Penurunan Tegangan Progresif Yang Tak Terkendali." },
  { "en": "Apa Itu Analisis Kontingensi?", "id": "Mempelajari Efek Kegagalan Komponen." },
  { "en": "Apa Itu Cadangan Berputar (Spinning Reserve)?", "id": "Kapasitas Pembangkitan Cadangan Yang Sinkron." },
  { "en": "Apa Itu Automatic Generation Control (AGC)?", "id": "Mengatur Pembangkitan Untuk Menjaga Frekuensi." },
  { "en": "Apa Itu Load Shedding?", "id": "Memutuskan Beban Secara Sengaja." },
  { "en": "Apa Itu Brownout?", "id": "Penurunan Tegangan Yang Disengaja." },
  { "en": "Apa Itu Blackout?", "id": "Kehilangan Daya Total Di Area Luas." },
  { "en": "Apa Itu Pemulihan Sistem (Restoration)?", "id": "Proses Memulihkan Jaringan Setelah Blackout." },
  { "en": "Apa Itu Wide Area Measurement System (WAMS)?", "id": "Menggunakan GPS Untuk Pengukuran Sinkron." },
  { "en": "Apa Itu Phasor Measurement Unit (PMU)?", "id": "Perangkat Yang Mengukur Fasor Tegangan Dan Arus." },
  { "en": "Apa Itu Sinkrofasor?", "id": "Fasor Yang Disinkronkan Waktu." },
  { "en": "Apa Itu Wireless Sensor Network (WSN)?", "id": "Jaringan Sensor Nirkabel Untuk Pemantauan." },
  { "en": "Apa Itu Energy Harvesting?", "id": "Mengumpulkan Energi Dari Lingkungan Sekitar." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Kendaraan Listrik Menyediakan Daya Ke Jaringan." },
  { "en": "Apa Itu Vehicle-to-Home (V2H)?", "id": "Kendaraan Listrik Memberi Daya Pada Rumah." },
  { "en": "Apa Itu Demand Response?", "id": "Perubahan Penggunaan Listrik Oleh Konsumen." },
  { "en": "Apa Itu Time-of-Use (TOU) Pricing?", "id": "Tarif Listrik Bervariasi Berdasarkan Waktu." },
  { "en": "Apa Itu Real-Time Pricing (RTP)?", "id": "Harga Listrik Berubah Sangat Cepat." },
  { "en": "Apa Itu Advanced Metering Infrastructure (AMI)?", "id": "Infrastruktur Untuk Meteran Cerdas." },
  { "en": "Apa Itu Meteran Cerdas (Smart Meter)?", "id": "Meteran Listrik Dengan Komunikasi Dua Arah." },
  { "en": "Apa Itu Home Area Network (HAN)?", "id": "Jaringan Di Dalam Rumah." },
  { "en": "Apa Itu Building Automation System (BAS)?", "id": "Sistem Kontrol Otomatis Untuk Bangunan." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Untuk Pemantauan Dan Kontrol Jarak Jauh." },
  { "en": "Apa Itu Distributed Control System (DCS)?", "id": "Sistem Kontrol Dengan Kontroler Terdistribusi." },
  { "en": "Apa Itu RTU (Remote Terminal Unit)?", "id": "Perangkat Elektronik Untuk Antarmuka Dengan SCADA." },
  { "en": "Apa Itu IED (Intelligent Electronic Device)?", "id": "Perangkat Cerdas Untuk Otomasi Gardu Induk." },
  { "en": "Apa Itu Protokol DNP3?", "id": "Protokol Komunikasi Untuk Otomasi Utilitas." },
  { "en": "Apa Itu Protokol IEC 61850?", "id": "Standar Komunikasi Untuk Gardu Induk." },
  { "en": "Apa Itu Cybersecurity?", "id": "Perlindungan Sistem Komputer Dari Ancaman." },
  { "en": "Apa Itu Firewall?", "id": "Sistem Keamanan Jaringan." },
  { "en": "Apa Itu Intrusion Detection System (IDS)?", "id": "Sistem Yang Mendeteksi Aktivitas Mencurigakan." },
  { "en": "Apa Itu Intrusion Prevention System (IPS)?", "id": "Sistem Yang Mendeteksi Dan Mencegah Intrusi." },
  { "en": "Apa Itu Keamanan Fisik?", "id": "Perlindungan Personel Dan Aset Fisik." },
  { "en": "Apa Itu FMEA (Failure Mode and Effects Analysis)?", "id": "Metode Untuk Menganalisis Potensi Kegagalan." },
  { "en": "Apa Itu FTA (Fault Tree Analysis)?", "id": "Analisis Kegagalan Sistem Top-down." },
  { "en": "Apa Itu Six Sigma?", "id": "Metodologi Untuk Peningkatan Kualitas." },
  { "en": "Apa Itu Lean Manufacturing?", "id": "Metodologi Untuk Mengurangi Pemborosan." },
  { "en": "Apa Itu Kaizen?", "id": "Filosofi Perbaikan Berkelanjutan." },
  { "en": "Apa Itu Poka-Yoke?", "id": "Mekanisme Pencegahan Kesalahan." },
  { "en": "Apa Itu Konverter Flyback?", "id": "Konverter DC-DC Terisolasi Berbasis Buck-Boost." },
  { "en": "Apa Keuntungan Konverter Flyback?", "id": "Sederhana, Murah, Dan Multiple Output." },
  { "en": "Apa Itu Operasi Mode Discontinue (DCM)?", "id": "Arus Induktor Mencapai Nol Setiap Siklus." },
  { "en": "Apa Itu Operasi Mode Kontinu (CCM)?", "id": "Arus Induktor Tidak Pernah Nol." },
  { "en": "Apa Itu Rangkaian Penjepit (Clamp Circuit)?", "id": "Membatasi Tegangan Pada Saklar Flyback." },
  { "en": "Apa Itu Konverter Forward?", "id": "Konverter DC-DC Terisolasi Berbasis Buck." },
  { "en": "Apa Perbedaan Utama Flyback Dan Forward?", "id": "Flyback Menyimpan Energi, Forward Mentransfer Langsung." },
  { "en": "Apa Itu Kumparan Reset?", "id": "Digunakan Untuk Mereset Inti Trafo Konverter Forward." },
  { "en": "Apa Itu Konverter Jembatan Aktif-Clamp?", "id": "Topologi Efisiensi Tinggi Dengan Soft-Switching." },
  { "en": "Apa Itu Konverter Resonan Seri?", "id": "Menggunakan Rangkaian Resonan Seri Untuk Soft-Switching." },
  { "en": "Apa Itu Konverter Resonan Paralel?", "id": "Rangkaian Resonan Terpasang Paralel Dengan Beban." },
  { "en": "Apa Itu Konverter LLC?", "id": "Konverter Resonan Dengan Tiga Elemen Reaktif." },
  { "en": "Apa Keuntungan Konverter LLC?", "id": "Efisiensi Sangat Tinggi Pada Beban Penuh." },
  { "en": "Apa Itu Penyearah Sinkron?", "id": "Menggunakan MOSFET Sebagai Pengganti Dioda." },
  { "en": "Mengapa Penyearah Sinkron Digunakan?", "id": "Mengurangi Kerugian Konduksi Dan Meningkatkan Efisiensi." },
  { "en": "Apa Itu Kontrol Sisi Primer (Primary-Side Control)?", "id": "Regulasi Output Tanpa Umpan Balik Optocoupler." },
  { "en": "Apa Keuntungan Kontrol Sisi Primer?", "id": "Mengurangi Biaya Dan Meningkatkan Keandalan." },
  { "en": "Apa Itu Penggerak Gerbang (Gate Driver)?", "id": "Rangkaian Untuk Mengaktifkan Dan Menonaktifkan Transistor." },
  { "en": "Apa Itu Level Shifter?", "id": "Menggeser Level Tegangan Sinyal Kontrol." },
  { "en": "Apa Itu Penggerak Sisi-Atas (High-Side Driver)?", "id": "Menggerakkan Saklar Yang Terhubung Ke Rel Positif." },
  { "en": "Apa Itu Penggerak Sisi-Bawah (Low-Side Driver)?", "id": "Menggerakkan Saklar Yang Terhubung Ke Ground." },
  { "en": "Apa Itu Under-Voltage Lockout (UVLO)?", "id": "Mematikan Sirkuit Jika Tegangan Suplai Rendah." },
  { "en": "Apa Itu Kapasitor Bypass?", "id": "Menyaring Derau Frekuensi Tinggi Pada Jalur Daya." },
  { "en": "Apa Itu Kapasitor Decoupling?", "id": "Menyediakan Arus Lokal Untuk Sirkuit Digital." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Pasif Untuk Menekan Derau Frekuensi Tinggi." },
  { "en": "Apa Itu Tata Letak Bintang (Star Grounding)?", "id": "Teknik Pembumian Untuk Mengurangi Derau." },
  { "en": "Apa Itu Jalur Kembali Arus?", "id": "Jalur Yang Diambil Arus Kembali Ke Sumber." },
  { "en": "Mengapa Area Loop Penting?", "id": "Loop Arus Besar Menghasilkan EMI Tinggi." },
  { "en": "Apa Itu Radiasi Medan Dekat?", "id": "Medan Elektromagnetik Dekat Dengan Sumber." },
  { "en": "Apa Itu Radiasi Medan Jauh?", "id": "Medan Elektromagnetik Jauh Dari Sumber." },
  { "en": "Apa Itu Antena?", "id": "Struktur Yang Meradiasikan Gelombang Elektromagnetik." },
  { "en": "Apa Itu Pelindung Faraday (Faraday Shield)?", "id": "Kandang Logam Untuk Memblokir Medan Elektromagnetik." },
  { "en": "Apa Itu Gasket EMI?", "id": "Bahan Konduktif Untuk Menyegel Celah." },
  { "en": "Apa Itu Konduksi Common-Mode?", "id": "Derau Yang Mengalir Searah Pada Jalur." },
  { "en": "Apa Itu Konduksi Differential-Mode?", "id": "Derau Yang Mengalir Berlawanan Arah." },
  { "en": "Apa Itu Line Impedance Stabilization Network (LISN)?", "id": "Digunakan Untuk Pengujian Emisi Terkonduksi." },
  { "en": "Apa Itu Ruang Anechoic?", "id": "Ruangan Yang Dirancang Menyerap Gelombang Elektromagnetik." },
  { "en": "Apa Itu Sel TEM?", "id": "Transverse Electromagnetic Cell Untuk Pengujian EMI." },
  { "en": "Apa Itu Electrostatic Discharge (ESD)?", "id": "Pelepasan Listrik Statis Yang Tiba-tiba." },
  { "en": "Bagaimana Cara Mencegah Kerusakan ESD?", "id": "Menggunakan Gelang Antistatis Dan Alas." },
  { "en": "Apa Itu Model Tubuh Manusia (HBM)?", "id": "Model Standar Untuk Pengujian Ketahanan ESD." },
  { "en": "Apa Itu Latch-up?", "id": "Kondisi Hubung Singkat Dalam Sirkuit CMOS." },
  { "en": "Apa Itu Dioda Perlindungan ESD?", "id": "Dioda Untuk Melindungi Sirkuit Dari ESD." },
  { "en": "Apa Itu Power Bank?", "id": "Pengisi Daya Portabel Untuk Perangkat Elektronik." },
  { "en": "Apa Itu Pengisian Cepat (Fast Charging)?", "id": "Teknologi Untuk Mengisi Baterai Lebih Cepat." },
  { "en": "Apa Itu USB Power Delivery (USB-PD)?", "id": "Standar Pengisian Daya Tegangan Tinggi." },
  { "en": "Apa Itu Qualcomm Quick Charge?", "id": "Protokol Pengisian Cepat Populer." },
  { "en": "Apa Itu Lighting?", "id": "Teknologi Pencahayaan." },
  { "en": "Apa Itu Ballast Elektronik?", "id": "Sirkuit Untuk Menyalakan Lampu Fluoresen." },
  { "en": "Apa Itu Driver LED?", "id": "Catu Daya Khusus Untuk Dioda Pemancar Cahaya." },
  { "en": "Apa Itu Peredupan (Dimming)?", "id": "Mengatur Kecerahan Sumber Cahaya." },
  { "en": "Apa Itu Peredupan PWM?", "id": "Menggunakan Sinyal PWM Untuk Mengatur Kecerahan LED." },
  { "en": "Apa Itu Peredupan Analog?", "id": "Mengatur Arus DC LED Untuk Mengubah Kecerahan." },
  { "en": "Apa Itu Triac Dimmer?", "id": "Peredup Umum Untuk Lampu Pijar." },
  { "en": "Bisakah Triac Dimmer Digunakan Untuk LED?", "id": "Memerlukan Driver LED Yang Kompatibel." },
  { "en": "Apa Itu Cold Cathode Fluorescent Lamp (CCFL)?", "id": "Digunakan Sebagai Lampu Latar LCD Lama." },
  { "en": "Apa Itu Inverter Lampu Latar?", "id": "Menyediakan Tegangan AC Tinggi Untuk CCFL." },
  { "en": "Apa Itu Welding?", "id": "Proses Penyambungan Material." },
  { "en": "Apa Itu Inverter Las?", "id": "Catu Daya Las Berbasis Inverter." },
  { "en": "Apa Keuntungan Inverter Las?", "id": "Ringan, Efisien, Dan Kontrol Lebih Baik." },
  { "en": "Apa Itu Sistem Audio Mobil?", "id": "Sistem Reproduksi Suara Di Kendaraan." },
  { "en": "Apa Itu Penguat Kelas D?", "id": "Jenis Penguat Audio Yang Sangat Efisien." },
  { "en": "Apa Itu Konverter Daya Audio?", "id": "Menyediakan Daya Untuk Penguat Audio." },
  { "en": "Apa Itu Tegangan Rel (Rail Voltage)?", "id": "Tegangan Suplai Positif Dan Negatif." },
  { "en": "Apa Itu Data Center?", "id": "Fasilitas Untuk Menampung Sistem Komputer." },
  { "en": "Apa Itu Power Distribution Unit (PDU)?", "id": "Mendistribusikan Daya Listrik Di Dalam Rak." },
  { "en": "Apa Itu Catu Daya Redundan?", "id": "Memiliki Lebih Dari Satu Catu Daya." },
  { "en": "Apa Itu Hot-Swap?", "id": "Memungkinkan Penggantian Modul Tanpa Mematikan." },
  { "en": "Apa Itu Telekomunikasi?", "id": "Transmisi Informasi Jarak Jauh." },
  { "en": "Apa Itu Catu Daya Telekomunikasi?", "id": "Menyediakan Daya DC Andal (-48V)." },
  { "en": "Mengapa Tegangan -48V Digunakan?", "id": "Mengurangi Korosi Galvanik Pada Kabel." },
  { "en": "Apa Itu Sistem Dirgantara (Avionics)?", "id": "Sistem Elektronik Di Pesawat Terbang." },
  { "en": "Apa Tantangan Daya Dirgantara?", "id": "Keandalan Tinggi, Ringan, Tahan Getaran." },
  { "en": "Apa Itu Peralatan Medis?", "id": "Perangkat Yang Digunakan Dalam Layanan Kesehatan." },
  { "en": "Apa Persyaratan Catu Daya Medis?", "id": "Keamanan Sangat Tinggi Dan Arus Bocor Rendah." },
  { "en": "Apa Itu Standar IEC 60601?", "id": "Standar Keamanan Internasional Untuk Peralatan Medis." },
  { "en": "Apa Itu Means of Protection (MOP)?", "id": "Ukuran Perlindungan Terhadap Sengatan Listrik." },
  { "en": "Apa Itu MOPP (Means of Patient Protection)?", "id": "Perlindungan Untuk Pasien." },
  { "en": "Apa Itu MOOP (Means of Operator Protection)?", "id": "Perlindungan Untuk Operator." },
  { "en": "Apa Itu Defibrilator?", "id": "Perangkat Medis Untuk Mengatasi Aritmia Jantung." },
  { "en": "Bagaimana Defibrilator Bekerja?", "id": "Memberikan Kejutan Listrik Terkontrol." },
  { "en": "Apa Itu Electrosurgical Unit (ESU)?", "id": "Menggunakan Arus Frekuensi Tinggi Untuk Memotong." },
  { "en": "Apa Itu Sistem Tenaga Hibrida?", "id": "Menggabungkan Beberapa Sumber Daya." },
  { "en": "Apa Itu Peralatan Uji?", "id": "Instrumen Untuk Mengukur Dan Menguji." },
  { "en": "Apa Itu Kalibrasi?", "id": "Membandingkan Pengukuran Instrumen Dengan Standar." },
  { "en": "Apa Itu Akurasi?", "id": "Seberapa Dekat Pengukuran Dengan Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi?", "id": "Seberapa Dekat Pengukuran Satu Sama Lain." },
  { "en": "Apa Itu Resolusi?", "id": "Perubahan Terkecil Yang Dapat Dideteksi." },
  { "en": "Apa Itu Linearitas?", "id": "Seberapa Proporsional Output Terhadap Input." },
  { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Operasi Suatu Instrumen." },
  { "en": "Apa Itu Saklar Analog?", "id": "Saklar Elektronik Untuk Melewatkan Sinyal Analog." },
  { "en": "Apa Itu Multiplexer (Mux)?", "id": "Memilih Satu Dari Banyak Sinyal Input." },
  { "en": "Apa Itu Demultiplexer (Demux)?", "id": "Mengirim Sinyal Input Ke Satu Output." },
  { "en": "Apa Itu Sample-and-Hold Circuit?", "id": "Mengambil Sampel Sinyal Dan Menahannya." },
  { "en": "Apa Itu Zero-Crossing Detector?", "id": "Sirkuit Yang Mendeteksi Saat Sinyal Melewati Nol." },
  { "en": "Apa Itu Peak Detector?", "id": "Sirkuit Untuk Menemukan Nilai Puncak Sinyal." },
  { "en": "Apa Itu Integrator?", "id": "Sirkuit Yang Outputnya Integral Dari Input." },
  { "en": "Apa Itu Differentiator?", "id": "Sirkuit Yang Outputnya Turunan Dari Input." },
  { "en": "Apa Itu Logarithmic Amplifier?", "id": "Penguat Dengan Output Sebanding Logaritma Input." },
  { "en": "Apa Itu Antilog Amplifier?", "id": "Outputnya Adalah Anti-Logaritma Dari Input." },
  { "en": "Apa Itu Konverter V-ke-F?", "id": "Mengubah Tegangan Menjadi Sinyal Frekuensi." },
  { "en": "Apa Itu Konverter F-ke-V?", "id": "Mengubah Frekuensi Menjadi Sinyal Tegangan." },
  { "en": "Apa Itu True RMS-to-DC Converter?", "id": "Menghasilkan Tegangan DC Setara Nilai RMS." },
  { "en": "Apa Itu Konverter Switched-Capacitor?", "id": "Menggunakan Kapasitor Untuk Mentransfer Muatan." },
  { "en": "Apa Itu Charge Pump?", "id": "Nama Lain Untuk Konverter Switched-Capacitor." },
  { "en": "Apa Keuntungan Konverter Switched-Capacitor?", "id": "Tidak Membutuhkan Induktor Magnetik." },
  { "en": "Apa Itu Bandgap Reference?", "id": "Sirkuit Referensi Tegangan Yang Stabil." },
  { "en": "Apa Itu Current Mirror?", "id": "Sirkuit Untuk Menyalin Arus." },
  { "en": "Apa Itu Wilson Current Mirror?", "id": "Penyempurnaan Dari Cermin Arus Dasar." },
  { "en": "Apa Itu Widlar Current Source?", "id": "Sumber Arus Yang Dapat Menghasilkan Arus Rendah." },
  { "en": "Apa Itu Beban Aktif?", "id": "Menggunakan Transistor Sebagai Beban Resistif." },
  { "en": "Apa Itu Cascode Amplifier?", "id": "Konfigurasi Untuk Meningkatkan Bandwidth." },
  { "en": "Apa Itu Darlington Pair?", "id": "Dua Transistor Bipolar Yang Terhubung." },
  { "en": "Apa Keuntungan Pasangan Darlington?", "id": "Penguatan Arus Yang Sangat Tinggi." },
  { "en": "Apa Itu Pasangan Sziklai?", "id": "Pasangan Komplementer Mirip Darlington." },
  { "en": "Apa Itu Class G Amplifier?", "id": "Menggunakan Beberapa Tegangan Rel." },
  { "en": "Apa Itu Class H Amplifier?", "id": "Memodulasi Tegangan Rel Secara Aktif." },
  { "en": "Apa Itu Doherty Amplifier?", "id": "Penguat Efisiensi Tinggi Untuk Sinyal RF." },
  { "en": "Apa Itu Chireix Combiner?", "id": "Metode Untuk Menggabungkan Daya." },
  { "en": "Apa Itu Amplifier Terdistribusi?", "id": "Memperoleh Bandwidth Sangat Lebar." },
  { "en": "Apa Itu Noise Figure (NF)?", "id": "Ukuran Degradasi SNR Oleh Komponen." },
  { "en": "Apa Itu Noise Temperature?", "id": "Ukuran Alternatif Untuk Derau." },
  { "en": "Apa Itu Intercept Point (IP3)?", "id": "Ukuran Linearitas Penguat." },
  { "en": "Apa Itu 1-dB Compression Point (P1dB)?", "id": "Titik Dimana Penguatan Turun 1 dB." },
  { "en": "Apa Itu Spurious-Free Dynamic Range (SFDR)?", "id": "Rentang Dimana Sinyal Tidak Terdistorsi." },
  { "en": "Apa Itu Mixer?", "id": "Sirkuit Untuk Mengubah Frekuensi Sinyal." },
  { "en": "Apa Itu Konversi Naik (Up-conversion)?", "id": "Mengubah Ke Frekuensi Yang Lebih Tinggi." },
  { "en": "Apa Itu Konversi Turun (Down-conversion)?", "id": "Mengubah Ke Frekuensi Yang Lebih Rendah." },
  { "en": "Apa Itu Local Oscillator (LO)?", "id": "Sumber Frekuensi Referensi Dalam Mixer." },
  { "en": "Apa Itu Frekuensi Gambar (Image Frequency)?", "id": "Frekuensi Yang Tidak Diinginkan." },
  { "en": "Apa Itu Filter Penolak Gambar?", "id": "Filter Untuk Menekan Frekuensi Gambar." },
  { "en": "Apa Itu Penerima Superheterodyne?", "id": "Arsitektur Penerima Radio Paling Umum." },
  { "en": "Apa Itu Frekuensi Menengah (IF)?", "id": "Frekuensi Tetap Hasil Dari Proses Mixing." },
  { "en": "Apa Itu Penerima Homodyne (Direct-Conversion)?", "id": "Mengubah Sinyal RF Langsung Ke Baseband." },
  { "en": "Apa Itu DC Offset?", "id": "Masalah Umum Dalam Penerima Direct-Conversion." },
  { "en": "Apa Itu Detector?", "id": "Sirkuit Untuk Mengekstrak Informasi Dari Sinyal." },
  { "en": "Apa Itu Demodulasi?", "id": "Proses Mengekstrak Sinyal Informasi Asli." },
  { "en": "Apa Itu Modulasi Amplitudo (AM)?", "id": "Informasi Dikodekan Dalam Amplitudo." },
  { "en": "Apa Itu Modulasi Frekuensi (FM)?", "id": "Informasi Dikodekan Dalam Frekuensi." },
  { "en": "Apa Itu Modulasi Fasa (PM)?", "id": "Informasi Dikodekan Dalam Fasa." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sistem Kontrol Umpan Balik Untuk Frekuensi." },
  { "en": "Apa Tiga Komponen Dasar PLL?", "id": "Phase Detector, Loop Filter, VCO." },
  { "en": "Apa Itu Phase Detector (PD)?", "id": "Membandingkan Fasa Dua Sinyal." },
  { "en": "Apa Itu Loop Filter?", "id": "Filter Low-pass Dalam PLL." },
  { "en": "Apa Itu Voltage-Controlled Oscillator (VCO)?", "id": "Osilator Yang Frekuensinya Dikontrol Tegangan." },
  { "en": "Apa Itu Lock Range?", "id": "Rentang Frekuensi Dimana PLL Dapat Tetap Terkunci." },
  { "en": "Apa Itu Capture Range?", "id": "Rentang Frekuensi Dimana PLL Dapat Mengunci." },
  { "en": "Apa Itu Frequency Synthesizer?", "id": "Sirkuit Untuk Menghasilkan Berbagai Frekuensi." },
  { "en": "Apa Itu Prescaler?", "id": "Pembagi Frekuensi Dalam Synthesizer." },
  { "en": "Apa Itu Phase Noise?", "id": "Fluktuasi Acak Dalam Fasa Osilator." },
  { "en": "Apa Itu Jitter?", "id": "Deviasi Waktu Dari Sinyal Periodik." },
  { "en": "Apa Itu Silicon-Controlled Switch (SCS)?", "id": "Thyristor Dengan Gerbang Anoda Dan Katoda." },
  { "en": "Apa Itu Unijunction Transistor (UJT)?", "id": "Transistor Tiga Terminal Sebagai Saklar." },
  { "en": "Apa Itu Programmable UJT (PUT)?", "id": "Versi UJT Yang Dapat Diprogram." },
  { "en": "Apa Itu Step-Recovery Diode?", "id": "Dioda Untuk Menghasilkan Pulsa Sangat Cepat." },
  { "en": "Apa Itu Pelindung Termal?", "id": "Perangkat Untuk Melindungi Dari Panas Berlebih." },
  { "en": "Apa Itu Sekering (Fuse)?", "id": "Perangkat Pelindung Arus Berlebih Satu Kali." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Otomatis Untuk Perlindungan Arus." },
  { "en": "Apa Itu Ground Fault Circuit Interrupter (GFCI)?", "id": "Melindungi Dari Sengatan Listrik." },
  { "en": "Apa Itu Arc Fault Circuit Interrupter (AFCI)?", "id": "Melindungi Dari Busur Api Berbahaya." },
  { "en": "Apa Itu Magnetoresistansi?", "id": "Perubahan Resistansi Bahan Akibat Medan Magnet." },
  { "en": "Apa Itu Giant Magnetoresistance (GMR)?", "id": "Efek Magnetoresistansi Kuantum Yang Besar." },
  { "en": "Di Mana GMR Digunakan?", "id": "Dalam Kepala Pembaca Hard Disk Drive." },
  { "en": "Apa Itu Tunnel Magnetoresistance (TMR)?", "id": "Efek Mekanika Kuantum Lainnya." },
  { "en": "Apa Itu Spintronics?", "id": "Studi Tentang Spin Intrinsik Elektron." },
  { "en": "Apa Itu MRAM (Magnetoresistive RAM)?", "id": "Memori Non-volatile Berbasis Efek Magnetoresistif." },
  { "en": "Apa Itu Memristor?", "id": "Komponen Elektronik Pasif Keempat." },
  { "en": "Apa Sifat Unik Memristor?", "id": "Resistansinya Bergantung Pada Sejarah Arus." },
  { "en": "Apa Itu Sistem Tenaga Listrik Pesawat?", "id": "Menghasilkan Dan Mendistribusikan Daya Di Pesawat." },
  { "en": "Apa Itu More Electric Aircraft (MEA)?", "id": "Konsep Mengganti Sistem Hidrolik Dengan Listrik." },
  { "en": "Apa Itu All-Electric Aircraft (AEA)?", "id": "Pesawat Yang Hampir Sepenuhnya Bertenaga Listrik." },
  { "en": "Apa Itu Sistem Tenaga Listrik Kapal?", "id": "Sistem Daya Untuk Aplikasi Kelautan." },
  { "en": "Apa Itu Propulsion Listrik Terpadu?", "id": "Sistem Propulsi Kapal Modern." },
  { "en": "Apa Itu Elektronika Daya Skala Grid?", "id": "Aplikasi Elektronika Daya Pada Skala Jaringan." },
  { "en": "Apa Itu STATCOM Modular Multilevel?", "id": "STATCOM Yang Menggunakan Topologi MMC." },
  { "en": "Apa Itu Kereta Listrik?", "id": "Kereta Yang Digerakkan Oleh Listrik." },
  { "en": "Apa Itu Catu Daya Tepi Jalan?", "id": "Menyediakan Daya Untuk Sistem Kereta." },
  { "en": "Apa Itu Pantoograph?", "id": "Kontak Geser Untuk Mengambil Daya." },
  { "en": "Apa Itu Jalur Ketiga (Third Rail)?", "id": "Metode Penyediaan Daya Listrik Kereta." },
  { "en": "Apa Itu Pengereman Rheostatik?", "id": "Mengubah Energi Kinetik Menjadi Panas." },
  { "en": "Apa Itu Konverter Traksi?", "id": "Konverter Daya Untuk Menggerakkan Motor Kereta." },
  { "en": "Apa Itu Welding Listrik?", "id": "Proses Pengelasan Menggunakan Busur Listrik." },
  { "en": "Apa Itu SMAW (Shielded Metal Arc Welding)?", "id": "Proses Las Busur Logam Terlindung." },
  { "en": "Apa Itu GMAW (Gas Metal Arc Welding)?", "id": "Juga Dikenal Sebagai Las MIG." },
  { "en": "Apa Itu GTAW (Gas Tungsten Arc Welding)?", "id": "Juga Dikenal Sebagai Las TIG." },
  { "en": "Apa Itu Catu Daya Plasma?", "id": "Menghasilkan Plasma Untuk Pemotongan Atau Pengelasan." },
  { "en": "Apa Itu Pemanasan Dielektrik?", "id": "Memanaskan Bahan Isolator Dengan Medan Listrik." },
  { "en": "Apa Itu Electroplating?", "id": "Proses Pelapisan Logam Menggunakan Listrik." },
  { "en": "Apa Itu Anodizing?", "id": "Proses Pasivasi Elektrolitik." },
  { "en": "Apa Itu Electrostatic Precipitator (ESP)?", "id": "Perangkat Filtrasi Partikel Menggunakan Listrik." },
  { "en": "Apa Itu Ozonizer?", "id": "Perangkat Untuk Menghasilkan Gas Ozon." },
  { "en": "Apa Itu Pelepasan Korona?", "id": "Pelepasan Listrik Akibat Ionisasi Fluida." },
  { "en": "Apa Itu Peralatan Ilmiah?", "id": "Instrumen Untuk Penelitian Ilmiah." },
  { "en": "Apa Itu Catu Daya Magnet?", "id": "Menyediakan Arus Sangat Stabil Untuk Elektromagnet." },
  { "en": "Apa Itu Akselerator Partikel?", "id": "Mesin Yang Mempercepat Partikel." },
  { "en": "Apa Itu Catu Daya Klystron?", "id": "Menyediakan Daya Untuk Tabung Klystron." },
  { "en": "Apa Itu Klystron?", "id": "Tabung Vakum Penguat Gelombang Mikro." },
  { "en": "Apa Itu Konverter Matriks?", "id": "Konverter AC-AC Langsung Tanpa Tautan DC." },
  { "en": "Apa Itu Saklar Dua Arah?", "id": "Saklar Yang Dapat Memblokir Tegangan." },
  { "en": "Berapa Saklar Yang Dibutuhkan Konverter Matriks 3x3?", "id": "Sembilan Saklar Dua Arah." },
  { "en": "Apa Itu Masalah Komutasi Konverter Matriks?", "id": "Mencegah Hubung Singkat Dan Sirkuit Terbuka." },
  { "en": "Apa Itu Komutasi Empat Langkah?", "id": "Strategi Komutasi Berbasis Arus." },
  { "en": "Apa Itu Konverter Matriks Jarang (Sparse)?", "id": "Konverter Matriks Dengan Jumlah Saklar Lebih Sedikit." },
  { "en": "Apa Itu Konverter Matriks Tak Langsung?", "id": "Memiliki Tautan DC Fiktif." },
  { "en": "Apa Itu Kegagalan Semikonduktor?", "id": "Perangkat Berhenti Berfungsi Sesuai Harapan." },
  { "en": "Apa Itu Elektromigrasi?", "id": "Transportasi Material Akibat Gerakan Ion." },
  { "en": "Apa Itu Thermal Runaway?", "id": "Siklus Umpan Balik Positif Panas." },
  { "en": "Apa Itu Second Breakdown?", "id": "Mode Kegagalan Destruktif Pada BJT." },
  { "en": "Apa Itu Latch-up Parasitik?", "id": "Kondisi Hubung Singkat Pada Struktur CMOS." },
  { "en": "Apa Itu Avalanche Energy Rating (EAS)?", "id": "Energi Maksimum Yang Dapat Diserap MOSFET." },
  { "en": "Apa Itu Short-Circuit Withstand Time?", "id": "Durasi Maksimum Menahan Arus Hubung Singkat." },
  { "en": "Apa Itu Hot Carrier Injection?", "id": "Mekanisme Degradasi Dalam Perangkat MOSFET." },
  { "en": "Apa Itu Time-Dependent Dielectric Breakdown (TDDB)?", "id": "Mekanisme Kegagalan Isolator Gerbang." },
  { "en": "Apa Itu Paket Perangkat (Device Package)?", "id": "Enkapsulasi Untuk Melindungi Die Semikonduktor." },
  { "en": "Apa Itu TO-220?", "id": "Jenis Paket Perangkat Tembus Lubang." },
  { "en": "Apa Itu TO-247?", "id": "Paket Tembus Lubang Yang Lebih Besar." },
  { "en": "Apa Itu Surface Mount Device (SMD)?", "id": "Komponen Yang Dipasang Di Permukaan PCB." },
  { "en": "Apa Itu D2PAK (TO-263)?", "id": "Paket Pemasangan Permukaan Umum." },
  { "en": "Apa Itu Direct Bonded Copper (DBC)?", "id": "Substrat Yang Digunakan Dalam Modul Daya." },
  { "en": "Apa Itu Active Metal Brazing (AMB)?", "id": "Substrat Alternatif Untuk Modul Daya." },
  { "en": "Apa Itu Press-Pack Device?", "id": "Perangkat Daya Tegangan Tinggi." },
  { "en": "Apa Itu Current Sharing?", "id": "Membagi Arus Secara Merata Antar Perangkat." },
  { "en": "Mengapa Pembagian Arus Penting?", "id": "Mencegah Satu Perangkat Mengalami Beban Berlebih." },
  { "en": "Apa Itu Metode Droop?", "id": "Metode Pasif Untuk Pembagian Beban." },
  { "en": "Apa Itu Active Current Sharing?", "id": "Menggunakan Sirkuit Kontrol Untuk Membagi Arus." },
  { "en": "Apa Itu Paralleling Converters?", "id": "Menghubungkan Konverter Secara Paralel." },
  { "en": "Apa Itu Desain Filter Input?", "id": "Merancang Filter Untuk Mencegah Osilasi." },
  { "en": "Apa Itu Stabilitas Konverter Bertingkat (Cascaded)?", "id": "Menjamin Stabilitas Sistem Secara Keseluruhan." },
  { "en": "Apa Itu Kriteria Middlebrook?", "id": "Kriteria Desain Untuk Stabilitas Filter Input." },
  { "en": "Apa Itu Impedansi Output Sumber?", "id": "Impedansi Yang Terlihat Dari Sisi Input." },
  { "en": "Apa Itu Impedansi Input Beban?", "id": "Impedansi Yang Terlihat Dari Sisi Output." },
  { "en": "Apa Itu Konverter Jembatan Tiga Fasa?", "id": "Konverter Dengan Enam Saklar." },
  { "en": "Apa Itu Konverter B6?", "id": "Nama Lain Untuk Konverter Jembatan Tiga Fasa." },
  { "en": "Apa Itu Mode Operasi 180 Derajat?", "id": "Setiap Saklar Menghantar Selama 180 Derajat." },
  { "en": "Apa Itu Mode Operasi 120 Derajat?", "id": "Setiap Saklar Menghantar Selama 120 Derajat." },
  { "en": "Apa Kerugian Mode 120 Derajat?", "id": "Pemanfaatan Tegangan DC Lebih Rendah." },
  { "en": "Apa Itu Third Harmonic Injection PWM?", "id": "Teknik Untuk Meningkatkan Pemanfaatan Tegangan Bus." },
  { "en": "Apa Itu Discontinuous PWM (DPWM)?", "id": "Teknik PWM Untuk Mengurangi Kerugian Switching." },
  { "en": "Bagaimana DPWM Mengurangi Kerugian?", "id": "Satu Fasa Dijepit Ke Rel DC." },
  { "en": "Apa Itu Konverter Resonan Z-Source?", "id": "Menggabungkan Konverter Resonan Dan Tipe-Z." },
  { "en": "Apa Itu Konverter Quasi-Z-Source?", "id": "Varian Dari Konverter Tipe-Z." },
  { "en": "Apa Itu Digital Control Loop?", "id": "Loop Umpan Balik Yang Diimplementasikan Secara Digital." },
  { "en": "Apa Itu Keterlambatan Komputasi?", "id": "Waktu Yang Dibutuhkan Prosesor Untuk Menghitung." },
  { "en": "Apa Efek Keterlambatan Komputasi?", "id": "Mengurangi Margin Fasa Dan Stabilitas." },
  { "en": "Apa Itu Aliasing Frekuensi?", "id": "Frekuensi Tinggi Tampak Sebagai Frekuensi Rendah." },
  { "en": "Apa Itu Filter Anti-Aliasing?", "id": "Filter Low-pass Sebelum Proses Sampling." },
  { "en": "Apa Itu Zero-Order Hold (ZOH)?", "id": "Efek DAC Yang Menahan Nilai." },
  { "en": "Apa Efek Zero-Order Hold?", "id": "Memperkenalkan Keterlambatan Fasa Tambahan." },
  { "en": "Apa Itu Kompensator Digital?", "id": "Implementasi Digital Dari Jaringan Kompensasi." },
  { "en": "Apa Itu Transformasi Bilinear?", "id": "Metode Untuk Mendesain Kompensator Digital." },
  { "en": "Apa Itu Digital PID Controller?", "id": "Implementasi Digital Dari Kontroler PID." },
  { "en": "Apa Itu Perangkat Lunak SPICE?", "id": "Simulation Program with Integrated Circuit Emphasis." },
  { "en": "Untuk Apa SPICE Digunakan?", "id": "Untuk Simulasi Sirkuit Elektronik Analog." },
  { "en": "Apa Itu Model Perangkat?", "id": "Representasi Matematis Dari Komponen Elektronik." },
  { "en": "Apa Itu Analisis Titik Operasi DC?", "id": "Menghitung Kondisi DC Sirkuit." },
  { "en": "Apa Itu Analisis AC (Sapuan Frekuensi)?", "id": "Menganalisis Respons Frekuensi Sirkuit." },
  { "en": "Apa Itu Analisis Transien?", "id": "Menganalisis Respons Sirkuit Terhadap Waktu." },
  { "en": "Apa Itu Masalah Konvergensi?", "id": "Simulator Gagal Menemukan Solusi." },
  { "en": "Apa Itu Inti Amorf?", "id": "Material Inti Magnetik Non-kristal." },
  { "en": "Apa Keuntungan Inti Amorf?", "id": "Kerugian Inti Sangat Rendah." },
  { "en": "Apa Itu Inti Nanokristalin?", "id": "Material Dengan Butiran Berukuran Nanometer." },
  { "en": "Apa Itu Inti Bubuk Besi?", "id": "Inti Yang Terbuat Dari Partikel Besi." },
  { "en": "Apa Itu Gapping Terdistribusi?", "id": "Celah Udara Didistribusikan Di Seluruh Inti." },
  { "en": "Apa Itu Kumparan Foil?", "id": "Kumparan Yang Dibuat Dari Lembaran Tembaga." },
  { "en": "Apa Itu Busbar?", "id": "Konduktor Logam Untuk Distribusi Daya." },
  { "en": "Apa Itu Busbar Berlaminasi?", "id": "Busbar Dengan Lapisan Isolasi." },
  { "en": "Apa Keuntungan Busbar Berlaminasi?", "id": "Induktansi Parasitik Sangat Rendah." },
  { "en": "Apa Itu Mercury-Arc Rectifier?", "id": "Jenis Penyearah Tegangan Tinggi Lama." },
  { "en": "Apa Itu Thyratron?", "id": "Tabung Berisi Gas Sebagai Saklar." },
  { "en": "Apa Itu Magnetic Amplifier (Mag Amp)?", "id": "Penguat Menggunakan Saturasi Inti Magnetik." },
  { "en": "Apa Itu Welding Frekuensi Tinggi?", "id": "Menggunakan Arus Frekuensi Tinggi Untuk Mengelas." },
  { "en": "Apa Itu Catu Daya Elektroforesis?", "id": "Menyediakan Tegangan Tinggi Untuk Memisahkan Molekul." },
  { "en": "Apa Itu Standar CISPR?", "id": "Standar Internasional Untuk Interferensi Radio." },
  { "en": "Apa Itu Standar FCC Bagian 15?", "id": "Aturan FCC Untuk Emisi Frekuensi Radio." },
  { "en": "Apa Itu Standar IEC 62368-1?", "id": "Standar Keamanan Untuk Peralatan Audio/Video." },
  { "en": "Apa Itu Standar AEC-Q101?", "id": "Standar Kualifikasi Stres Untuk Komponen Otomotif." },
  { "en": "Apa Itu Functional Safety?", "id": "Keamanan Yang Bergantung Pada Operasi Benar." },
  { "en": "Apa Itu Standar ISO 26262?", "id": "Standar Keamanan Fungsional Untuk Sistem Otomotif." },
  { "en": "Apa Itu Automotive Safety Integrity Level (ASIL)?", "id": "Tingkat Risiko Dalam Standar ISO 26262." },
  { "en": "Apa Itu Safety Integrity Level (SIL)?", "id": "Tingkat Kinerja Keamanan." },
  { "en": "Apa Itu Analisis Pohon Kejadian (ETA)?", "id": "Metode Analisis Risiko Maju." },
  { "en": "Apa Itu HAZOP (Hazard and Operability Study)?", "id": "Studi Sistematis Untuk Mengidentifikasi Masalah." },
  { "en": "Apa Itu Redundansi Dingin (Cold Standby)?", "id": "Unit Cadangan Mati Hingga Dibutuhkan." },
  { "en": "Apa Itu Redundansi Panas (Hot Standby)?", "id": "Unit Cadangan Berjalan Bersamaan." },
  { "en": "Apa Itu Fault Tolerance?", "id": "Kemampuan Sistem Beroperasi Meski Ada Kesalahan." },
  { "en": "Apa Itu Graceful Degradation?", "id": "Penurunan Kinerja Bertahap Saat Terjadi Kegagalan." },
  { "en": "Apa Itu Fail-Safe?", "id": "Desain Yang Gagal Dalam Cara Aman." },
  { "en": "Apa Itu Fail-Operational?", "id": "Sistem Terus Beroperasi Setelah Kegagalan." },
  { "en": "Apa Itu Konverter AC-DC Tiga Tingkat NPC?", "id": "Topologi Penyearah Aktif." },
  { "en": "Apa Itu Penyearah Jembatan Hibrid?", "id": "Menggabungkan Saklar Terkendali Dan Tak Terkendali." },
  { "en": "Apa Itu Kontrol Deadbeat?", "id": "Mencapai Respons Selesai Dalam Jumlah Langkah." },
  { "en": "Apa Itu Observer Luenberger?", "id": "Jenis Observer Keadaan." },
  { "en": "Apa Itu Filter Kalman?", "id": "Estimator Optimal Untuk Sistem Linier." },
  { "en": "Apa Itu Extended Kalman Filter (EKF)?", "id": "Versi Filter Kalman Untuk Sistem Non-linier." },
  { "en": "Apa Itu Unscented Kalman Filter (UKF)?", "id": "Filter Non-linier Lainnya." },
  { "en": "Apa Itu Particle Filter?", "id": "Metode Monte Carlo Untuk Estimasi." },
  { "en": "Apa Itu Sensorless Field-Oriented Control?", "id": "Kontrol Vektor Tanpa Sensor Kecepatan." },
  { "en": "Apa Itu Model Referensi Adaptif?", "id": "Sistem Kontrol Yang Menggunakan Model Referensi." },
  { "en": "Apa Itu Self-Tuning Regulator?", "id": "Regulator Yang Secara Otomatis Menyesuaikan Parameternya." },
  { "en": "Apa Itu Jaringan Listrik Kapal?", "id": "Sistem Daya Terisolasi Di Atas Kapal." },
  { "en": "Apa Tantangan Jaringan Listrik Kapal?", "id": "Ukuran Kompak, Beban Berubah, Stabilitas." },
  { "en": "Apa Itu Sistem Propulsi Listrik?", "id": "Menggunakan Motor Listrik Untuk Mendorong Kapal." },
  { "en": "Apa Itu Propulsion Pod?", "id": "Unit Propulsi Yang Dapat Diputar." },
  { "en": "Apa Itu Variable Speed Drive (VSD)?", "id": "Nama Lain Untuk Variable Frequency Drive." },
  { "en": "Apa Itu Static Frequency Converter (SFC)?", "id": "Konverter AC-AC Untuk Menghubungkan Jaringan Frekuensi." },
  { "en": "Apa Itu Shore Power?", "id": "Menyediakan Daya Listrik Untuk Kapal Di Pelabuhan." },
  { "en": "Apa Itu Cold Ironing?", "id": "Istilah Lain Untuk Shore Power." },
  { "en": "Apa Itu Penggerak Regeneratif?", "id": "Drive Motor Yang Dapat Melakukan Pengereman Regeneratif." },
  { "en": "Apa Itu Common DC Bus?", "id": "Bus DC Umum Untuk Beberapa Inverter." },
  { "en": "Apa Keuntungan Common DC Bus?", "id": "Berbagi Energi Pengereman, Efisiensi." },
  { "en": "Apa Itu Topologi Konverter Vienna?", "id": "Penyearah Tiga Fasa Dengan Power Factor Correction." },
  { "en": "Apa Itu Konverter Swiss Rectifier?", "id": "Penyearah Tiga Fasa Bidirectional." },
  { "en": "Apa Itu Konverter Bridgeless PFC?", "id": "Topologi PFC Tanpa Penyearah Jembatan." },
  { "en": "Apa Keuntungan Konverter Bridgeless?", "id": "Efisiensi Lebih Tinggi." },
  { "en": "Apa Itu Totem-Pole PFC?", "id": "Topologi PFC Bridgeless Yang Populer." },
  { "en": "Saklar Apa Yang Digunakan Totem-Pole PFC?", "id": "Menggunakan Saklar Cepat Seperti GaN." },
  { "en": "Apa Itu Digital Twin?", "id": "Model Virtual Dari Sistem Fisik." },
  { "en": "Apa Kegunaan Digital Twin?", "id": "Pemantauan, Analisis, Dan Simulasi." },
  { "en": "Apa Itu Artificial Intelligence (AI)?", "id": "Kecerdasan Yang Ditunjukkan Oleh Mesin." },
  { "en": "Apa Itu Machine Learning (ML)?", "id": "Sub-bidang AI Yang Memungkinkan Sistem Belajar." },
  { "en": "Apa Itu Deep Learning?", "id": "Sub-bidang ML Dengan Jaringan Saraf Dalam." },
  { "en": "Apa Itu Big Data?", "id": "Kumpulan Data Yang Sangat Besar." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Fisik Yang Terhubung." },
  { "en": "Apa Itu Cloud Computing?", "id": "Menyediakan Layanan Komputasi Melalui Internet." },
  { "en": "Apa Itu Edge Computing?", "id": "Memproses Data Dekat Sumbernya." },
  { "en": "Apa Itu Fog Computing?", "id": "Lapisan Antara Edge Dan Cloud." },
  { "en": "Apa Itu Solid State Circuit Breaker?", "id": "Pemutus Sirkuit Berbasis Semikonduktor." },
  { "en": "Apa Keuntungan Pemutus Sirkuit Solid State?", "id": "Sangat Cepat, Tidak Ada Aus Mekanis." },
  { "en": "Apa Itu Fault Current Limiter (FCL)?", "id": "Perangkat Untuk Membatasi Arus Gangguan." },
  { "en": "Apa Itu Superconducting FCL?", "id": "Pembatas Arus Berbasis Bahan Superkonduktor." },
  { "en": "Apa Itu Superkonduktivitas?", "id": "Fenomena Resistansi Listrik Nol." },
  { "en": "Apa Itu Suhu Kritis?", "id": "Suhu Dimana Superkonduktivitas Terjadi." },
  { "en": "Apa Itu Superconducting Magnetic Energy Storage (SMES)?", "id": "Menyimpan Energi Dalam Medan Magnet." },
  { "en": "Apa Itu Flywheel Energy Storage (FES)?", "id": "Menyimpan Energi Dalam Bentuk Energi Kinetik." },
  { "en": "Apa Itu Compressed Air Energy Storage (CAES)?", "id": "Menyimpan Energi Dengan Mengompresi Udara." },
  { "en": "Apa Itu Pumped Hydroelectric Storage?", "id": "Menyimpan Energi Dengan Memompa Air." },
  { "en": "Apa Itu State Observer?", "id": "Nama Lain Untuk Estimator Keadaan." },
  { "en": "Apa Itu Kalman Gain?", "id": "Parameter Kunci Dalam Filter Kalman." },
  { "en": "Apa Itu Noise Kovarians?", "id": "Ukuran Statistik Dari Derau." },
  { "en": "Apa Itu Inovasi (Residual)?", "id": "Perbedaan Antara Pengukuran Dan Prediksi." },
  { "en": "Apa Itu White Noise?", "id": "Sinyal Acak Dengan Spektrum Daya Datar." },
  { "en": "Apa Itu Gaussian Noise?", "id": "Derau Dengan Distribusi Probabilitas Gaussian." },
  { "en": "Apa Itu Proses Wiener?", "id": "Model Matematis Gerak Brown." },
  { "en": "Apa Itu Kalkulus Stokastik?", "id": "Cabang Matematika Untuk Proses Stokastik." },
  { "en": "Apa Itu Persamaan Diferensial Stokastik?", "id": "Persamaan Diferensial Dengan Suku Acak." },
  { "en": "Apa Itu Integrasi Monte Carlo?", "id": "Metode Numerik Menggunakan Angka Acak." },
  { "en": "Apa Itu Metode Euler-Maruyama?", "id": "Metode Untuk Menyelesaikan Persamaan Diferensial Stokastik." },
  { "en": "Apa Itu Optimisasi?", "id": "Menemukan Solusi Terbaik Dari Alternatif." },
  { "en": "Apa Itu Fungsi Objektif?", "id": "Fungsi Yang Akan Dimaksimalkan Atau Diminimalkan." },
  { "en": "Apa Itu Kendala (Constraint)?", "id": "Kondisi Yang Harus Dipenuhi Solusi." },
  { "en": "Apa Itu Pemrograman Linier?", "id": "Masalah Optimisasi Dengan Fungsi Linier." },
  { "en": "Apa Itu Pemrograman Non-linier?", "id": "Masalah Optimisasi Dengan Fungsi Non-linier." },
  { "en": "Apa Itu Pemrograman Kuadratik?", "id": "Fungsi Objektif Kuadratik Dan Kendala Linier." },
  { "en": "Apa Itu Optimisasi Konveks?", "id": "Masalah Optimisasi Dengan Himpunan Konveks." },
  { "en": "Apa Itu Pemrograman Dinamis?", "id": "Metode Untuk Memecahkan Masalah Kompleks." },
  { "en": "Apa Itu Prinsip Optimalitas Bellman?", "id": "Prinsip Dasar Pemrograman Dinamis." },
  { "en": "Apa Itu Gradien?", "id": "Vektor Turunan Parsial Pertama." },
  { "en": "Apa Itu Matriks Hessian?", "id": "Matriks Persegi Dari Turunan Parsial Kedua." },
  { "en": "Apa Itu Metode Gradien Turun?", "id": "Algoritma Iteratif Untuk Menemukan Minimum." },
  { "en": "Apa Itu Laju Belajar (Learning Rate)?", "id": "Parameter Yang Mengontrol Ukuran Langkah." },
  { "en": "Apa Itu Metode Newton?", "id": "Metode Pencarian Akar Lainnya." },
  { "en": "Apa Itu Pengali Lagrange?", "id": "Strategi Untuk Menemukan Maksima/Minima." },
  { "en": "Apa Itu Kondisi Karush-Kuhn-Tucker (KKT)?", "id": "Generalisasi Dari Metode Pengali Lagrange." },
  { "en": "Apa Itu Algoritma Simplex?", "id": "Algoritma Populer Untuk Pemrograman Linier." },
  { "en": "Apa Itu Interior Point Method?", "id": "Kelas Algoritma Lain Untuk Optimisasi." },
  { "en": "Apa Itu Simulated Annealing?", "id": "Metode Optimisasi Probabilistik Metaheuristik." },
  { "en": "Apa Itu Optimisasi Kawanan Partikel (PSO)?", "id": "Metaheuristik Terinspirasi Dari Perilaku Kawanan." },
  { "en": "Apa Itu Pencarian Tabu?", "id": "Metaheuristik Yang Menggunakan Memori." },
  { "en": "Apa Itu Sistem Tenaga Listrik DC?", "id": "Jaringan Yang Hanya Menggunakan Daya DC." },
  { "en": "Apa Itu Jaringan DC Rumah?", "id": "Sistem Kelistrikan DC Untuk Rumah." },
  { "en": "Apa Itu Konverter Daya Solid-State?", "id": "Nama Lain Untuk Konverter Elektronika Daya." },
  { "en": "Apa Itu Topologi Jembatan Tak Terkendali?", "id": "Penyearah Jembatan Yang Menggunakan Dioda." },
  { "en": "Apa Itu Freewheeling Path?", "id": "Jalur Alternatif Untuk Arus Induktor." },
  { "en": "Apa Itu Ripple Arus?", "id": "Variasi Arus Pada Induktor." },
  { "en": "Apa Itu Ripple Tegangan?", "id": "Variasi Tegangan Pada Kapasitor." },
  { "en": "Apa Itu Small-Signal Model?", "id": "Model Linierisasi Dari Konverter." },
  { "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Output Terhadap Input." },
  { "en": "Apa Itu Pole?", "id": "Akar Dari Polinomial Penyebut." },
  { "en": "Apa Itu Zero?", "id": "Akar Dari Polinomial Pembilang." },
  { "en": "Apa Itu Lokus Akar (Root Locus)?", "id": "Grafik Posisi Pole Loop Tertutup." },
  { "en": "Apa Itu Respon Frekuensi Loop Terbuka?", "id": "Digunakan Untuk Menganalisis Stabilitas." },
  { "en": "Apa Itu Crossover Frequency?", "id": "Frekuensi Dimana Penguatan Loop 0 dB." },
  { "en": "Apa Itu Efek Miller?", "id": "Peningkatan Kapasitansi Efektif." },
  { "en": "Apa Itu Desain Berbasis Keandalan?", "id": "Mempertimbangkan Keandalan Selama Proses Desain." },
  { "en": "Apa Itu Fisika Kegagalan?", "id": "Mempelajari Proses Fisik Penyebab Kegagalan." },
  { "en": "Apa Itu Uji Umur Dipercepat?", "id": "Pengujian Di Bawah Kondisi Stres." },
  { "en": "Apa Itu Model Arrhenius?", "id": "Model Untuk Ketergantungan Suhu." },
  { "en": "Apa Itu Model Coffin-Manson?", "id": "Model Untuk Kegagalan Akibat Kelelahan Termal." },
  { "en": "Apa Itu Pengukuran Impedansi?", "id": "Mengukur Impedansi Kompleks Suatu Komponen." },
  { "en": "Apa Itu Penganalisis Impedansi?", "id": "Instrumen Untuk Mengukur Impedansi." },
  { "en": "Apa Itu Spektroskopi Impedansi Elektrokimia?", "id": "Teknik Untuk Menganalisis Baterai." },
  { "en": "Apa Itu Kesehatan Baterai (SoH)?", "id": "Ukuran Kondisi Baterai Saat Ini." },
  { "en": "Apa Itu Sisa Umur Berguna (RUL)?", "id": "Estimasi Sisa Waktu Operasi." },
  { "en": "Apa Itu Prognostik?", "id": "Bidang Yang Berkaitan Dengan Prediksi Kegagalan." },
  { "en": "Apa Itu Fusion Energi?", "id": "Proses Penggabungan Inti Atom." },
  { "en": "Apa Itu Tokamak?", "id": "Perangkat Fusi Magnetik Berbentuk Torus." },
  { "en": "Apa Itu Catu Daya Magnet Superkonduktor?", "id": "Menyediakan Arus Sangat Besar Untuk Magnet Fusi." },
  { "en": "Apa Itu Sistem Pemanasan Plasma?", "id": "Memanaskan Plasma Hingga Suhu Fusi." },
  { "en": "Apa Itu Pemanasan Ohmik?", "id": "Memanaskan Plasma Menggunakan Arus." },
  { "en": "Apa Itu Injeksi Berkas Netral?", "id": "Menembakkan Partikel Berenergi Tinggi Ke Plasma." },
  { "en": "Apa Itu Pemanasan Gelombang Radio?", "id": "Menggunakan Gelombang Elektromagnetik Untuk Memanaskan Plasma." },
  { "en": "Apa Itu Kapasitor Bank?", "id": "Menyimpan Energi Dalam Jumlah Besar." },
  { "en": "Apa Itu Crowbar?", "id": "Sistem Proteksi Cepat Untuk Catu Daya." },
  { "en": "Apa Itu Snubber?", "id": "Rangkaian Untuk Menekan Lonjakan Tegangan." },
  { "en": "Apa Itu Konverter Multilevel Modular (MMC)?", "id": "Topologi Kunci Untuk HVDC Dan FACTS." },
  { "en": "Apa Itu Jaringan Distribusi DC?", "id": "Menggunakan DC Untuk Distribusi Daya Lokal." },
  { "en": "Apa Itu Solid State Transformer (SST)?", "id": "Transformator Cerdas Berbasis Elektronika Daya." },
  { "en": "Apa Fungsi SST?", "id": "Mengontrol Aliran Daya, Tegangan, Dan Frekuensi." },
  { "en": "Apa Itu Port Energi?", "id": "Antarmuka Cerdas Antara Beban Dan Jaringan." },
  { "en": "Apa Itu Penggerak Motor Matriks?", "id": "Konverter Matriks Yang Digunakan Sebagai Penggerak Motor." },
  { "en": "Apa Itu Regenerasi?", "id": "Mengembalikan Energi Pengereman Ke Sumber." },
  { "en": "Apa Itu Energy Router?", "id": "Perangkat Cerdas Untuk Mengelola Aliran Energi." },
  { "en": "Apa Itu Sistem Tenaga Listrik DC Hibrida?", "id": "Menggabungkan Bus DC Dan AC." },
  { "en": "Apa Itu Pemutus Sirkuit DC?", "id": "Tantangan Utama Dalam Jaringan DC." },
  { "en": "Mengapa Memutus Arus DC Sulit?", "id": "Karena Tidak Ada Titik Nol Alami." },
  { "en": "Apa Itu Pemutus Sirkuit Hibrida?", "id": "Menggabungkan Komponen Mekanis Dan Solid-State." },
  { "en": "Apa Itu Resonant DC-DC Converter?", "id": "Mencapai Soft Switching Menggunakan Resonansi." },
  { "en": "Apa Itu Gallium Oxide (Ga2O3)?", "id": "Semikonduktor Wide-Bandgap Yang Muncul." },
  { "en": "Apa Itu Inti Udara (Air Core)?", "id": "Induktor Atau Trafo Tanpa Inti Magnetik." },
  { "en": "Kapan Inti Udara Digunakan?", "id": "Pada Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Efek Ferranti?", "id": "Kenaikan Tegangan Di Ujung Jalur Transmisi." },
  { "en": "Apa Itu Sistem Per Unit?", "id": "Normalisasi Kuantitas Listrik." },
  { "en": "Mengapa Sistem Per Unit Digunakan?", "id": "Menyederhanakan Analisis Sistem Tenaga." },
  { "en": "Apa Itu Diagram Satu Garis?", "id": "Representasi Sederhana Dari Sistem Tiga Fasa." },
  { "en": "Apa Itu Impedansi Urutan Nol?", "id": "Impedansi Terhadap Arus Urutan Nol." },
  { "en": "Apa Itu Impedansi Urutan Positif?", "id": "Impedansi Terhadap Arus Urutan Positif." },
  { "en": "Apa Itu Impedansi Urutan Negatif?", "id": "Impedansi Terhadap Arus Urutan Negatif." },
  { "en": "Apa Itu Gangguan Simetris?", "id": "Gangguan Yang Mempengaruhi Ketiga Fasa Secara Sama." },
  { "en": "Apa Itu Gangguan Asimetris?", "id": "Gangguan Yang Mempengaruhi Fasa Secara Berbeda." },
  { "en": "Apa Itu Gangguan Fasa-ke-Tanah?", "id": "Jenis Gangguan Asimetris Yang Paling Umum." },
  { "en": "Apa Itu Gangguan Fasa-ke-Fasa?", "id": "Gangguan Antara Dua Fasa." },
  { "en": "Apa Itu Relai Proteksi?", "id": "Perangkat Yang Mendeteksi Gangguan." },
  { "en": "Apa Itu Relai Jarak?", "id": "Melindungi Jalur Transmisi Berdasarkan Impedansi." },
  { "en": "Apa Itu Relai Diferensial?", "id": "Melindungi Trafo Atau Generator." },
  { "en": "Apa Itu Relai Arus Lebih?", "id": "Bekerja Saat Arus Melebihi Batas." },
  { "en": "Apa Itu Koordinasi Proteksi?", "id": "Memastikan Perangkat Proteksi Bekerja Berurutan." },
  { "en": "Apa Itu Recloser?", "id": "Pemutus Sirkuit Yang Dapat Menutup Kembali." },
  { "en": "Apa Itu Sectionalizer?", "id": "Perangkat Isolasi Gangguan." },
  { "en": "Apa Itu Sistem SCADA?", "id": "Supervisory Control and Data Acquisition." },
  { "en": "Apa Itu Sistem Manajemen Energi (EMS)?", "id": "Aplikasi Komputer Untuk Memantau Jaringan." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Struktur Hubungan Dalam Jaringan." },
  { "en": "Apa Itu Busbar?", "id": "Konduktor Umum Dalam Gardu Induk." },
  { "en": "Apa Itu Disconnecting Switch?", "id": "Saklar Isolasi Untuk Keamanan." },
  { "en": "Apa Itu Insulator?", "id": "Bahan Untuk Mengisolasi Konduktor." },
  { "en": "Apa Itu Bushing?", "id": "Insulator Yang Memungkinkan Konduktor Melewati." },
  { "en": "Apa Itu Surge Arrester?", "id": "Melindungi Peralatan Dari Lonjakan Tegangan." },
  { "en": "Apa Itu Saluran Transmisi?", "id": "Menyalurkan Daya Listrik Jarak Jauh." },
  { "en": "Apa Itu Menara Transmisi?", "id": "Struktur Penyangga Saluran Udara." },
  { "en": "Apa Itu Kabel Bawah Tanah?", "id": "Saluran Transmisi Yang Ditanam." },
  { "en": "Apa Itu Korona?", "id": "Pelepasan Listrik Di Sekitar Konduktor." },
  { "en": "Apa Efek Dari Korona?", "id": "Kerugian Daya Dan Interferensi Radio." },
  { "en": "Apa Itu Konduktor Berkas (Bundled Conductor)?", "id": "Menggunakan Beberapa Konduktor Per Fasa." },
  { "en": "Mengapa Konduktor Berkas Digunakan?", "id": "Untuk Mengurangi Efek Korona." },
  { "en": "Apa Itu Transposisi Jalur?", "id": "Menukar Posisi Fasa Secara Periodik." },
  { "en": "Mengapa Transposisi Dilakukan?", "id": "Untuk Menyeimbangkan Induktansi Jalur." },
  { "en": "Apa Itu Sag?", "id": "Lengkungan Ke Bawah Konduktor Udara." },
  { "en": "Apa Itu Tension?", "id": "Gaya Tarik Pada Konduktor." },
  { "en": "Apa Itu Kawat Tanah (Ground Wire)?", "id": "Kawat Di Atas Saluran Untuk Melindungi." },
  { "en": "Apa Fungsi Kawat Tanah?", "id": "Melindungi Dari Sambaran Petir Langsung." },
  { "en": "Apa Itu Motor Sinkron Magnet Permanen?", "id": "Motor Efisiensi Tinggi Dengan Magnet Permanen." },
  { "en": "Apa Itu Motor Reluktansi Tersaklar?", "id": "Motor Tanpa Magnet Atau Kumparan Rotor." },
  { "en": "Apa Itu Konverter Daya Untuk Dirgantara?", "id": "Harus Ringan, Andal, Dan Tahan Getaran." },
  { "en": "Apa Itu Frekuensi 400 Hz?", "id": "Frekuensi Umum Dalam Sistem Tenaga Pesawat." },
  { "en": "Mengapa 400 Hz Digunakan?", "id": "Memungkinkan Transformator Yang Lebih Kecil." },
  { "en": "Apa Itu RAM Air Turbine (RAT)?", "id": "Turbin Kecil Untuk Daya Darurat Pesawat." },
  { "en": "Apa Itu APU (Auxiliary Power Unit)?", "id": "Unit Daya Tambahan Di Pesawat." },
  { "en": "Apa Itu Sistem Tenaga Satelit?", "id": "Menyediakan Daya Untuk Operasi Satelit." },
  { "en": "Sumber Daya Utama Satelit Adalah Apa?", "id": "Panel Surya Dan Baterai." },
  { "en": "Apa Itu Maximum Power Point Tracking (MPPT)?", "id": "Mengekstrak Daya Maksimal Dari Panel Surya." },
  { "en": "Apa Itu Regulator Shunt Berurutan?", "id": "Regulator Daya Umum Untuk Satelit." },
  { "en": "Apa Itu Radiasi Pengion?", "id": "Radiasi Yang Dapat Merusak Elektronik." },
  { "en": "Apa Itu Radiation Hardening?", "id": "Membuat Komponen Elektronik Tahan Radiasi." },
  { "en": "Apa Itu Efek Dioda?", "id": "Konduksi Satu Arah." },
  { "en": "Apa Itu Efek Transistor?", "id": "Penguatan Sinyal Atau Switching." },
  { "en": "Apa Itu Efek Thyristor?", "id": "Switching Regeneratif." },
  { "en": "Apa Itu Daerah Aktif?", "id": "Wilayah Operasi Untuk Penguatan." },
  { "en": "Apa Itu Arus Saturasi Terbalik?", "id": "Arus Bocor Kecil Pada Dioda." },
  { "en": "Apa Itu Tegangan Breakdown?", "id": "Tegangan Dimana Dioda Mulai Menghantar." },
  { "en": "Apa Itu Penguat Umpan Balik?", "id": "Penguat Yang Menggunakan Umpan Balik." },
  { "en": "Apa Itu Umpan Balik Negatif?", "id": "Menstabilkan Penguatan Dan Mengurangi Distorsi." },
  { "en": "Apa Itu Umpan Balik Positif?", "id": "Dapat Menyebabkan Osilasi." },
  { "en": "Apa Itu Stabilitas?", "id": "Kemampuan Sistem Kembali Ke Keseimbangan." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Bagaimana Sistem Merespons Frekuensi Berbeda." },
  { "en": "Apa Itu Bode Plot?", "id": "Grafik Penguatan Dan Fasa." },
  { "en": "Apa Itu Nyquist Plot?", "id": "Grafik Lain Untuk Analisis Stabilitas." },
  { "en": "Apa Itu Kriteria Stabilitas Nyquist?", "id": "Menentukan Stabilitas Loop Tertutup." },
  { "en": "Apa Itu Gain Margin?", "id": "Ukuran Kestabilan Relatif." },
  { "en": "Apa Itu Phase Margin?", "id": "Ukuran Kestabilan Relatif Lainnya." },
  { "en": "Apa Itu Catu Daya Terdistribusi?", "id": "Arsitektur Daya Dengan Banyak Konverter Lokal." },
  { "en": "Apa Itu Bus Converter?", "id": "Konverter Terisolasi Untuk Daya Menengah." },
  { "en": "Apa Itu Point of Load (PoL) Converter?", "id": "Konverter Non-terisolasi Dekat Beban." },
  { "en": "Apa Itu Digital Power?", "id": "Manajemen Daya Menggunakan Kontrol Digital." },
  { "en": "Apa Itu PMBus?", "id": "Protokol Komunikasi Digital Untuk Daya." },
  { "en": "Apa Itu System Management Bus (SMBus)?", "id": "Bus Dua Kawat Untuk Komunikasi." },
  { "en": "Apa Itu Power Management Bus (PMBus)?", "id": "Protokol Digital Berbasis SMBus Untuk Daya." },
  { "en": "Apa Itu Advanced Configuration and Power Interface (ACPI)?", "id": "Standar Terbuka Untuk Manajemen Daya." },
  { "en": "Apa Itu Keadaan Tidur (Sleep State)?", "id": "Keadaan Daya Rendah Dalam ACPI." },
  { "en": "Apa Itu Hibernasi?", "id": "Menyimpan Keadaan Sistem Ke Disk." },
  { "en": "Apa Itu Standby?", "id": "Menjaga Keadaan Sistem Dalam RAM." },
  { "en": "Apa Itu Dynamic Voltage Scaling?", "id": "Mengubah Tegangan CPU Sesuai Beban." },
  { "en": "Apa Itu Dynamic Frequency Scaling?", "id": "Mengubah Frekuensi CPU Sesuai Beban." },
  { "en": "Apa Itu Power Gating?", "id": "Mematikan Daya Ke Blok Sirkuit." },
  { "en": "Apa Itu Clock Gating?", "id": "Menonaktifkan Sinyal Clock Ke Blok." },
  { "en": "Apa Itu Konverter Piezoelektrik?", "id": "Transformator Berbasis Bahan Piezoelektrik." },
  { "en": "Apa Itu Harvesting Energi Getaran?", "id": "Mengubah Getaran Mekanis Menjadi Listrik." },
  { "en": "Apa Itu Harvesting Energi Termal?", "id": "Mengubah Gradien Suhu Menjadi Listrik." },
  { "en": "Apa Itu Harvesting Energi RF?", "id": "Mengumpulkan Energi Dari Gelombang Radio." },
  { "en": "Apa Itu Rectenna?", "id": "Antena Penyearah Untuk Harvesting Energi RF." },
  { "en": "Apa Itu Baterai Film Tipis?", "id": "Baterai Padat Yang Sangat Tipis." },
  { "en": "Apa Itu Baterai Nuklir?", "id": "Menghasilkan Listrik Dari Peluruhan Radioaktif." },
  { "en": "Apa Itu Betavoltaics?", "id": "Sel Surya Yang Ditenagai Sumber Beta." },
  { "en": "Apa Itu Siklotron?", "id": "Jenis Akselerator Partikel." },
  { "en": "Apa Itu Kapasitor Keramik Multilayer (MLCC)?", "id": "Bentuk Umum Kapasitor Keramik." },
  { "en": "Apa Itu Efek Piezoelektrik MLCC?", "id": "Dapat Menghasilkan Derau Akustik." },
  { "en": "Apa Itu Singing Capacitors?", "id": "Derau Terdengar Dari Kapasitor." },
  { "en": "Apa Itu Pengukuran Empat-Kawat?", "id": "Nama Lain Pengukuran Kelvin." },
  { "en": "Apa Itu Shunt Arus?", "id": "Resistor Presisi Rendah Untuk Mengukur Arus." },
  { "en": "Apa Itu Penguat Sensor Arus?", "id": "Menguatkan Penurunan Tegangan Di Atas Shunt." },
  { "en": "Apa Itu Sensor Arus Efek Hall?", "id": "Mengukur Arus Menggunakan Medan Magnet." },
  { "en": "Apa Keuntungan Sensor Efek Hall?", "id": "Menyediakan Isolasi Galvanik." },
  { "en": "Apa Itu Sensor Arus Fluxgate?", "id": "Sensor Arus Yang Sangat Sensitif." },
  { "en": "Apa Itu Koil Rogowski?", "id": "Sensor Arus Fleksibel." },
  { "en": "Apa Itu Sistem Fase-Tunggal Tiga-Kawat?", "id": "Menyediakan Dua Level Tegangan." },
  { "en": "Apa Itu Ground Terisolasi?", "id": "Ground Terpisah Untuk Peralatan Sensitif." },
  { "en": "Apa Itu Konverter AC-DC Power Factor Correction (PFC)?", "id": "Membentuk Arus Input Agar Sinusoidal." },
  { "en": "Apa Itu Mode Kritis (CRM)?", "id": "Operasi PFC Di Batas CCM/DCM." },
  { "en": "Apa Itu Kontrol Frekuensi Variabel?", "id": "Frekuensi Switching Berubah Dengan Beban." },
  { "en": "Apa Itu Topologi PFC Tanpa Jembatan?", "id": "Meningkatkan Efisiensi Dengan Mengurangi Penurunan Dioda." },
  { "en": "Apa Itu Kontrol Digital?", "id": "Menggunakan Prosesor Digital Untuk Loop Kontrol." },
  { "en": "Apa Itu Kontroler Proporsional (P)?", "id": "Output Sebanding Dengan Kesalahan." },
  { "en": "Apa Itu Kontroler Integral (I)?", "id": "Mengurangi Kesalahan Steady-State." },
  { "en": "Apa Itu Kontroler Derivatif (D)?", "id": "Meningkatkan Respon Transien." },
  { "en": "Apa Itu Tuning PID?", "id": "Menentukan Parameter Optimal Untuk Kontroler PID." },
  { "en": "Apa Itu Metode Ziegler-Nichols?", "id": "Metode Heuristik Untuk Tuning PID." },
  { "en": "Apa Itu Konverter Modular?", "id": "Konverter Yang Dibangun Dari Modul Identik." },
  { "en": "Apa Itu Skalabilitas?", "id": "Kemampuan Sistem Untuk Tumbuh." },
  { "en": "Apa Itu Ketersediaan (Availability)?", "id": "Persentase Waktu Sistem Beroperasi." },
  { "en": "Apa Itu Pemeliharaan Prediktif?", "id": "Memprediksi Kegagalan Sebelum Terjadi." },
  { "en": "Apa Itu Fusi Sensor?", "id": "Menggabungkan Data Dari Beberapa Sensor." },
  { "en": "Apa Itu Redundansi Analitik?", "id": "Menggunakan Model Untuk Memverifikasi Data Sensor." },
  { "en": "Apa Itu Sistem Otomotif 48V?", "id": "Standar Tegangan Baru Untuk Kendaraan Hibrida." },
  { "en": "Mengapa Sistem 48V Digunakan?", "id": "Mendukung Beban Daya Lebih Tinggi." },
  { "en": "Apa Itu Hibrida Ringan (Mild Hybrid)?", "id": "Kendaraan Dengan Bantuan Listrik Terbatas." },
  { "en": "Apa Itu Konverter DC-DC 48V-ke-12V?", "id": "Menyediakan Daya Untuk Sistem 12V." },
  { "en": "Apa Itu Baterai Starter?", "id": "Baterai Untuk Menyalakan Mesin." },
  { "en": "Apa Itu Baterai Siklus Dalam (Deep Cycle)?", "id": "Baterai Yang Dirancang Untuk Dikosongkan." },
  { "en": "Apa Itu Pelepasan Diri (Self-Discharge)?", "id": "Baterai Kehilangan Muatan Seiring Waktu." },
  { "en": "Apa Itu Efek Memori?", "id": "Fenomena Pada Baterai NiCd Lama." },
  { "en": "Apa Itu Baterai Asam Timbal (Lead-Acid)?", "id": "Teknologi Baterai Yang Sudah Matang." },
  { "en": "Apa Itu Absorbed Glass Mat (AGM)?", "id": "Jenis Baterai Asam Timbal." },
  { "en": "Apa Itu Baterai Gel?", "id": "Baterai Asam Timbal Dengan Elektrolit Gel." },
  { "en": "Apa Itu Baterai Nickel-Cadmium (NiCd)?", "id": "Baterai Isi Ulang Yang Lebih Tua." },
  { "en": "Apa Itu Baterai Nickel-Metal Hydride (NiMH)?", "id": "Penerus Baterai NiCd." },
  { "en": "Apa Itu Solid-State Battery?", "id": "Baterai Yang Menggunakan Elektrolit Padat." },
  { "en": "Apa Keuntungan Baterai Solid-State?", "id": "Keamanan Lebih Tinggi Dan Kepadatan Energi." },
  { "en": "Apa Itu Baterai Aliran (Flow Battery)?", "id": "Menyimpan Energi Dalam Cairan Elektrolit." },
  { "en": "Apa Aplikasi Baterai Aliran?", "id": "Penyimpanan Energi Skala Jaringan." },
  { "en": "Apa Itu Baterai Logam-Udara?", "id": "Menggunakan Oksigen Dari Udara." },
  { "en": "Apa Itu Anoda?", "id": "Elektroda Tempat Terjadinya Oksidasi." },
  { "en": "Apa Itu Katoda?", "id": "Elektroda Tempat Terjadinya Reduksi." },
  { "en": "Apa Itu Elektrolit?", "id": "Media Konduktif Ion Antara Elektroda." },
  { "en": "Apa Itu Separator?", "id": "Memisahkan Anoda Dan Katoda." },
  { "en": "Apa Itu Laju C (C-Rate)?", "id": "Ukuran Laju Pengisian Atau Pengosongan." },
  { "en": "Apa Itu Thermal Runaway Baterai?", "id": "Kegagalan Eksotermik Yang Tidak Terkendali." },
  { "en": "Apa Itu Sirkuit Perlindungan Baterai?", "id": "Melindungi Baterai Dari Kondisi Berbahaya." },
  { "en": "Apa Itu Pengukur Bahan Bakar (Fuel Gauge)?", "id": "Sirkuit Untuk Mengestimasi State of Charge." },
  { "en": "Apa Itu Penghitungan Coulomb?", "id": "Metode Untuk Melacak Muatan." },
  { "en": "Apa Itu Karakterisasi Baterai?", "id": "Proses Mengukur Perilaku Baterai." },
  { "en": "Apa Itu Model Baterai?", "id": "Representasi Matematis Dari Perilaku Baterai." },
  { "en": "Apa Itu Model Thevenin Baterai?", "id": "Model Sirkuit Sederhana." },
  { "en": "Apa Itu Motor Sinkron Enggan?", "id": "Motor Tanpa Magnet Atau Kumparan." },
  { "en": "Apa Itu Direct Current (DC) Microgrid?", "id": "Jaringan Distribusi Lokal Berbasis DC." },
  { "en": "Apa Itu Konverter Boost Interleaved?", "id": "Mengurangi Riak Arus Input." },
  { "en": "Apa Itu Penyearah Jembatan Tiga Fasa?", "id": "Menggunakan Enam Dioda." },
  { "en": "Apa Itu Penyearah Terkendali Tiga Fasa?", "id": "Menggunakan Enam SCR." },
  { "en": "Apa Itu Topologi HERIC?", "id": "Topologi Inverter Surya Efisiensi Tinggi." },
  { "en": "Apa Itu Konverter Topologi NPC?", "id": "Neutral-Point Clamped." },
  { "en": "Apa Itu Konverter Flying Capacitor?", "id": "Konverter Multilevel Menggunakan Kapasitor." },
  { "en": "Apa Itu Konverter DAB Tiga Fasa?", "id": "Konverter Dual-Active Bridge Untuk Daya Tinggi." },
  { "en": "Apa Itu Konverter AC-DC Matriks?", "id": "Konverter AC-AC Langsung." },
  { "en": "Apa Itu Kontrol Tanpa Sensor?", "id": "Mengontrol Motor Tanpa Sensor Posisi." },
  { "en": "Apa Itu Injeksi Sinyal?", "id": "Teknik Untuk Estimasi Posisi Rotor." },
  { "en": "Apa Itu Model Saliency?", "id": "Memanfaatkan Asimetri Magnetik Motor." },
  { "en": "Apa Itu Observer Fluks?", "id": "Mengestimasi Fluks Magnetik Motor." },
  { "en": "Apa Itu Model Referensi Adaptif (MRAS)?", "id": "Teknik Estimasi Kecepatan." },
  { "en": "Apa Itu Lithium Titanate (LTO) Battery?", "id": "Jenis Baterai Lithium-Ion Dengan Anoda Titanat." },
  { "en": "Apa Keuntungan Baterai LTO?", "id": "Siklus Hidup Sangat Panjang, Pengisian Cepat." },
  { "en": "Apa Itu Baterai Sodium-Ion?", "id": "Baterai Isi Ulang Berbasis Ion Natrium." },
  { "en": "Apa Itu Baterai Dual-Ion?", "id": "Anion Dan Kation Berpartisipasi Dalam Reaksi." },
  { "en": "Apa Itu Elektrolit Padat?", "id": "Elektrolit Berwujud Padat, Bukan Cair." },
  { "en": "Apa Itu Dendrit Dalam Baterai?", "id": "Formasi Mirip Jarum Yang Menyebabkan Hubung Singkat." },
  { "en": "Bagaimana Elektrolit Padat Mencegah Dendrit?", "id": "Menghalangi Pertumbuhan Dendrit Secara Fisik." },
  { "en": "Apa Itu Konverter Daya Kepadatan Tinggi?", "id": "Konverter Dengan Ukuran Sangat Kecil." },
  { "en": "Bagaimana Cara Mencapai Kepadatan Daya Tinggi?", "id": "Frekuensi Switching Tinggi, Komponen Canggih." },
  { "en": "Apa Itu Efek Termoelektrik?", "id": "Konversi Langsung Panas Menjadi Listrik." },
  { "en": "Apa Itu Efek Seebeck?", "id": "Menghasilkan Tegangan Akibat Perbedaan Suhu." },
  { "en": "Apa Itu Efek Peltier?", "id": "Menghasilkan Pemanasan Atau Pendinginan." },
  { "en": "Apa Itu Efek Thomson?", "id": "Pemanasan Atau Pendinginan Konduktor." },
  { "en": "Apa Itu Generator Termoelektrik?", "id": "Perangkat Yang Menggunakan Efek Seebeck." },
  { "en": "Apa Itu Pendingin Termoelektrik?", "id": "Perangkat Yang Menggunakan Efek Peltier." },
  { "en": "Apa Itu Faktor Merit (ZT)?", "id": "Ukuran Kinerja Bahan Termoelektrik." },
  { "en": "Apa Itu Topologi Jembatan Geser-Fasa?", "id": "Topologi Jembatan Penuh Dengan Soft-Switching." },
  { "en": "Apa Itu Konverter DAB Tiga-Port?", "id": "Menghubungkan Tiga Port DC." },
  { "en": "Apa Itu Konverter Multi-Port?", "id": "Menghubungkan Lebih Dari Dua Sumber Atau Beban." },
  { "en": "Apa Itu Saklar Cerdas (Smart Switch)?", "id": "Saklar Dengan Kemampuan Komunikasi Dan Kontrol." },
  { "en": "Apa Itu Sistem Tenaga DC Pulsed?", "id": "Menghasilkan Pulsa Daya Tinggi." },
  { "en": "Apa Itu Pembangkit Marx?", "id": "Sirkuit Untuk Menghasilkan Pulsa Tegangan Tinggi." },
  { "en": "Apa Itu Pulse Forming Network (PFN)?", "id": "Menghasilkan Pulsa Listrik Dengan Bentuk Tertentu." },
  { "en": "Apa Itu Kapasitor Tautan Film?", "id": "Menggunakan Film Polimer Sebagai Dielektrik." },
  { "en": "Apa Keuntungan Kapasitor Film?", "id": "ESR Rendah, Keandalan Tinggi." },
  { "en": "Apa Itu Winding Tembaga Foil?", "id": "Menggunakan Lembaran Tembaga Untuk Kumparan." },
  { "en": "Apa Itu Manajemen Baterai Nirkabel?", "id": "Sistem Manajemen Baterai Tanpa Kabel." },
  { "en": "Apa Itu Kontroler Resonan Digital?", "id": "Kontroler Digital Khusus Untuk Konverter Resonan." },
  { "en": "Apa Itu Konverter AC-DC Tanpa Jembatan?", "id": "Meningkatkan Efisiensi PFC." },
  { "en": "Apa Itu Desain Berbasis Model?", "id": "Menggunakan Model Sistem Sepanjang Proses Desain." },
  { "en": "Apa Itu Simulasi HIL?", "id": "Hardware-in-the-Loop." },
  { "en": "Apa Itu Rapid Control Prototyping (RCP)?", "id": "Menguji Algoritma Kontrol Dengan Cepat." },
  { "en": "Apa Itu Kode Otomatis?", "id": "Menghasilkan Kode Program Secara Otomatis." },
  { "en": "Apa Itu Verifikasi Perangkat Lunak?", "id": "Memastikan Perangkat Lunak Bekerja Benar." },
  { "en": "Apa Itu Validasi Perangkat Lunak?", "id": "Memastikan Perangkat Lunak Memenuhi Kebutuhan." },
  { "en": "Apa Itu Standar MISRA C?", "id": "Panduan Pemrograman C Untuk Sistem Kritis." },
  { "en": "Apa Itu Analisis Kode Statis?", "id": "Menganalisis Kode Sumber Tanpa Menjalankannya." },
  { "en": "Apa Itu Tinjauan Kode (Code Review)?", "id": "Pemeriksaan Sistematis Kode Sumber." },
  { "en": "Apa Itu Continuous Integration (CI)?", "id": "Mengintegrasikan Perubahan Kode Secara Teratur." },
  { "en": "Apa Itu Continuous Deployment (CD)?", "id": "Menyebarkan Perubahan Ke Produksi Secara Otomatis." },
  { "en": "Apa Itu DevOps?", "id": "Kombinasi Pengembangan Dan Operasi." },
  { "en": "Apa Itu Perangkat Lunak Open Source?", "id": "Perangkat Lunak Dengan Kode Sumber Terbuka." },
  { "en": "Apa Itu Lisensi Permisif?", "id": "Lisensi Open Source Dengan Sedikit Batasan." },
  { "en": "Apa Itu Lisensi Copyleft?", "id": "Mempertahankan Kebebasan Perangkat Lunak." },
  { "en": "Apa Itu GitHub?", "id": "Platform Hosting Untuk Pengembangan Perangkat Lunak." },
  { "en": "Apa Itu Git?", "id": "Sistem Kontrol Versi Terdistribusi." },
  { "en": "Apa Itu Cabang (Branch)?", "id": "Versi Paralel Dari Repositori." },
  { "en": "Apa Itu Penggabungan (Merge)?", "id": "Menggabungkan Perubahan Dari Cabang Berbeda." },
  { "en": "Apa Itu Pull Request?", "id": "Permintaan Untuk Menggabungkan Perubahan." },
  { "en": "Apa Itu Power Electronics Building Block (PEBB)?", "id": "Konsep Konverter Modular." },
  { "en": "Apa Itu Passive Component Integration?", "id": "Mengintegrasikan Induktor Dan Kapasitor." },
  { "en": "Apa Itu Integrated Magnetics?", "id": "Menggabungkan Trafo Dan Induktor." },
  { "en": "Apa Itu Desain Termal?", "id": "Manajemen Panas Dalam Sistem Elektronik." },
  { "en": "Apa Itu Junction Temperature?", "id": "Suhu Operasi Die Semikonduktor." },
  { "en": "Apa Itu Case Temperature?", "id": "Suhu Pada Bagian Luar Paket." },
  { "en": "Apa Itu Ambient Temperature?", "id": "Suhu Udara Di Sekitar." },
  { "en": "Apa Itu Resistansi Termal Junction-to-Case?", "id": "Resistansi Panas Antara Die Dan Paket." },
  { "en": "Apa Itu Resistansi Termal Case-to-Sink?", "id": "Resistansi Panas Antara Paket Dan Heatsink." },
  { "en": "Apa Itu Resistansi Termal Sink-to-Ambient?", "id": "Resistansi Panas Antara Heatsink Dan Udara." },
  { "en": "Apa Itu Model Termal?", "id": "Representasi Matematis Dari Sistem Termal." },
  { "en": "Apa Itu Analogi Termal-Listrik?", "id": "Memodelkan Panas Seperti Sirkuit Listrik." },
  { "en": "Apa Itu Kapasitansi Termal?", "id": "Kemampuan Menyimpan Energi Panas." },
  { "en": "Apa Itu Impedansi Termal Transien?", "id": "Respon Termal Terhadap Pulsa Daya." },
  { "en": "Apa Itu Konverter AC Chopper?", "id": "Nama Lain Untuk Kontroler Tegangan AC." },
  { "en": "Apa Itu On-Off Control?", "id": "Metode Kontrol Sederhana Untuk Kontroler AC." },
  { "en": "Apa Itu Phase-Angle Control?", "id": "Mengontrol Sudut Penyulutan Thyristor." },
  { "en": "Apa Itu Integral Cycle Control?", "id": "Menghubungkan Beban Untuk Beberapa Siklus." },
  { "en": "Apa Itu Pemanas Resistif?", "id": "Mengubah Listrik Menjadi Panas." },
  { "en": "Apa Itu Static VAR Compensator (SVC)?", "id": "Mengatur Tegangan Jaringan." },
  { "en": "Apa Itu Thyristor-Controlled Reactor (TCR)?", "id": "Induktor Variabel Menggunakan Thyristor." },
  { "en": "Apa Itu Thyristor-Switched Capacitor (TSC)?", "id": "Kapasitor Yang Disaklar Menggunakan Thyristor." },
  { "en": "Apa Itu Inverter Sumber Arus?", "id": "Inverter Yang Diberi Makan Sumber Arus." },
  { "en": "Apa Itu Self-Commutated Inverter?", "id": "Inverter Yang Saklarnya Dapat Dimatikan." },
  { "en": "Apa Itu Line-Commutated Inverter?", "id": "Inverter Yang Mengandalkan Jaringan AC." },
  { "en": "Apa Itu Inverter HVDC?", "id": "Mengubah DC Tegangan Tinggi Menjadi AC." },
  { "en": "Apa Itu Mode Inverter?", "id": "Operasi Dimana Daya Mengalir Dari DC Ke AC." },
  { "en": "Apa Itu Mode Penyearah?", "id": "Operasi Dimana Daya Mengalir Dari AC Ke DC." },
  { "en": "Apa Itu Sudut Pemadaman (Extinction Angle)?", "id": "Parameter Kontrol Dalam Inverter." },
  { "en": "Apa Itu Kegagalan Komutasi?", "id": "Kegagalan Thyristor Untuk Mati." },
  { "en": "Apa Itu Saklar Elektromekanis?", "id": "Saklar Yang Menggunakan Kontak Bergerak." },
  { "en": "Apa Itu Relai?", "id": "Saklar Elektromekanis." },
  { "en": "Apa Itu Kontaktor?", "id": "Relai Berdaya Tinggi." },
  { "en": "Apa Itu Solenoid?", "id": "Aktuator Yang Menghasilkan Gerakan Linier." },
  { "en": "Apa Itu Sirkuit Terpadu (IC)?", "id": "Kumpulan Komponen Elektronik Dalam Satu Chip." },
  { "en": "Apa Itu Skala Integrasi?", "id": "Jumlah Transistor Per Chip." },
  { "en": "Apa Itu SSI (Small-Scale Integration)?", "id": "Beberapa Gerbang Logika." },
  { "en": "Apa Itu MSI (Medium-Scale Integration)?", "id": "Hingga Ratusan Gerbang." },
  { "en": "Apa Itu LSI (Large-Scale Integration)?", "id": "Hingga Ribuan Gerbang." },
  { "en": "Apa Itu VLSI (Very Large-Scale Integration)?", "id": "Jutaan Atau Milyaran Transistor." },
  { "en": "Apa Itu Hukum Moore?", "id": "Prediksi Jumlah Transistor Berlipat Ganda." },
  { "en": "Apa Itu System on a Chip (SoC)?", "id": "Sistem Komputer Lengkap Dalam Satu Chip." },
  { "en": "Apa Itu Application-Specific Integrated Circuit (ASIC)?", "id": "IC Yang Dirancang Untuk Tugas Khusus." },
  { "en": "Apa Itu Sensor Gambar CMOS?", "id": "Sensor Gambar Yang Dibuat Dengan Proses CMOS." },
  { "en": "Apa Itu Active Pixel Sensor (APS)?", "id": "Jenis Sensor Gambar CMOS." },
  { "en": "Apa Itu Charge-Coupled Device (CCD)?", "id": "Jenis Sensor Gambar Lainnya." },
  { "en": "Apa Itu Efisiensi Konversi?", "id": "Rasio Daya Output Terhadap Daya Input." },
  { "en": "Apa Itu Kurva Efisiensi?", "id": "Grafik Efisiensi Terhadap Beban." },
  { "en": "Apa Itu Beban Ringan?", "id": "Operasi Pada Persentase Rendah." },
  { "en": "Apa Itu Beban Penuh?", "id": "Operasi Pada Daya Maksimum." },
  { "en": "Apa Itu Mode Burst?", "id": "Mode Operasi Efisiensi Tinggi." },
  { "en": "Apa Itu Pulse Skipping?", "id": "Melewatkan Siklus Switching Pada Beban Ringan." }



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
            }, 7000);
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
