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


  { "en": "Sistem oleh rakyat?", "id": "Demokrasi perwakilan." },
  { "en": "Pemerintahan satu orang?", "id": "Otokrasi monarki." },
  { "en": "Gabungan partai politik?", "id": "Koalisi pemerintah." },
  { "en": "Penentang pemerintah?", "id": "Partai oposisi." },
  { "en": "Pemilihan umum?", "id": "Proses elektoral." },
  { "en": "Hak pilih universal?", "id": "Semua warga." },
  { "en": "Kampanye negatif?", "id": "Serangan personal." },
  { "en": "Daerah pemilihan?", "id": "Distrik elektoral." },
  { "en": "Wakil rakyat?", "id": "Anggota parlemen." },
  { "en": "Kekuasaan legislatif?", "id": "Membuat undang-undang." },
  { "en": "Kekuasaan eksekutif?", "id": "Menjalankan pemerintahan." },
  { "en": "Kekuasaan yudikatif?", "id": "Mengadili pelanggaran." },
  { "en": "Pemisahan kekuasaan?", "id": "Trias politica." },
  { "en": "Ideologi politik?", "id": "Sistem gagasan." },
  { "en": "Politik sayap kiri?", "id": "Egalitarianisme sosial." },
  { "en": "Politik sayap kanan?", "id": "Konservatisme tradisional." },
  { "en": "Hukum dasar negara?", "id": "Konstitusi negara." },
  { "en": "Perubahan konstitusi?", "id": "Amandemen undang-undang." },
  { "en": "Hubungan antarnegara?", "id": "Politik luarnegeri." },
  { "en": "Negara tanpa pemerintah?", "id": "Anarki total." },
  { "en": "Pemerintahan oleh elit?", "id": "Oligarki kekuasaan." },
  { "en": "Jabatan presiden?", "id": "Kepala negara." },
  { "en": "Masa jabatan terbatas?", "id": "Limitasi kekuasaan." },
  { "en": "Jajak pendapat?", "id": "Survei opini." },
  { "en": "Tingkat keterpilihan?", "id": "Elektabilitas kandidat." },
  { "en": "Partai politik?", "id": "Organisasi politik." },
  { "en": "Sistem multipartai?", "id": "Banyak partai." },
  { "en": "Sistem dwipartai?", "id": "Dua partai." },
  { "en": "Kecurangan pemilu?", "id": "Manipulasi suara." },
  { "en": "Uang dalam politik?", "id": "Politik uang." },
  { "en": "Melobi pembuat kebijakan?", "id": "Kegiatan lobi." },
  { "en": "Persetujuan tanpa voting?", "id": "Aklamasi bulat." },
  { "en": "Badan legislatif?", "id": "Dewan perwakilan." },
  { "en": "Sidang parlemen?", "id": "Rapat legislatif." },
  { "en": "Hak imunitas?", "id": "Kekebalan hukum." },
  { "en": "Mosi tidak percaya?", "id": "Evaluasi pemerintah." },
  { "en": "Pemberhentian presiden?", "id": "Proses pemakzulan." },
  { "en": "Referendum nasional?", "id": "Pemungutan suara." },
  { "en": "Keputusan mengikat?", "id": "Mandat politik." },
  { "en": "Pengaruh media massa?", "id": "Peran pers." },
  { "en": "Kebebasan berpendapat?", "id": "Hak asasi." },
  { "en": "Masyarakat sipil?", "id": "Organisasi non-pemerintah." },
  { "en": "Aktivisme politik?", "id": "Gerakan sosial." },
  { "en": "Protes massa?", "id": "Demonstrasi publik." },
  { "en": "Pemerintahan militer?", "id": "Junta militer." },
  { "en": "Kudeta militer?", "id": "Perebutan kekuasaan." },
  { "en": "Hukum internasional?", "id": "Aturan global." },
  { "en": "Perjanjian internasional?", "id": "Traktat negara." },
  { "en": "Organisasi antar pemerintah?", "id": "Kerjasama negara." },
  { "en": "Politik identitas?", "id": "Sentimen kelompok." },
  { "en": "Diskriminasi politik?", "id": "Perlakuan tidakadil." },
  { "en": "Nepotisme kekuasaan?", "id": "Pilih kasih." },
  { "en": "Pejabat sementara?", "id": "Pemimpin interim." },
  { "en": "Transparansi pemerintah?", "id": "Keterbukaan informasi." },
  { "en": "Akuntabilitas publik?", "id": "Tanggung jawab." },
  { "en": "Birokrasi pemerintah?", "id": "Aparatur sipil." },
  { "en": "Sentralisasi kekuasaan?", "id": "Pemusatan wewenang." },
  { "en": "Desentralisasi kekuasaan?", "id": "Pendelegasian wewenang." },
  { "en": "Otonomi daerah?", "id": "Kewenangan lokal." },
  { "en": "Federalisme negara?", "id": "Negara bagian." },
  { "en": "Separatisme wilayah?", "id": "Gerakan pemisahan." },
  { "en": "Nasionalisme bangsa?", "id": "Cinta tanahair." },
  { "en": "Radikalisme politik?", "id": "Paham ekstrem." },
  { "en": "Terorisme politik?", "id": "Kekerasan ekstrem." },
  { "en": "Diplomasi antarnegara?", "id": "Negosiasi damai." },
  { "en": "Sanksi ekonomi?", "id": "Hukuman finansial." },
  { "en": "Boikot internasional?", "id": "Aksi protes." },
  { "en": "Propaganda politik?", "id": "Penyebaran informasi." },
  { "en": "Sensor media?", "id": "Pembatasan informasi." },
  { "en": "Golongan putih?", "id": "Tidak memilih." },
  { "en": "Calon petahana?", "id": "Pejabat berkuasa." },
  { "en": "Calon kuda hitam?", "id": "Kandidat kejutan." },
  { "en": "Masa tenang pemilu?", "id": "Larangan kampanye." },
  { "en": "Tempat pemungutan?", "id": "Bilik suara." },
  { "en": "Surat suara?", "id": "Kertas pilihan." },
  { "en": "Saksi pemilu?", "id": "Pengawas independen." },
  { "en": "Hitung cepat?", "id": "Quick count." },
  { "en": "Debat kandidat?", "id": "Adu gagasan." },
  { "en": "Visi misi?", "id": "Program kerja." },
  { "en": "Partisipasi pemilih?", "id": "Tingkat kehadiran." },
  { "en": "Daftar pemilih?", "id": "Data pemilih." },
  { "en": "Politik dinasti?", "id": "Kekuasaan keluarga." },
  { "en": "Mahar politik?", "id": "Biaya pencalonan." },
  { "en": "Parlemen bikameral?", "id": "Dua kamar." },
  { "en": "Parlemen unikameral?", "id": "Satu kamar." },
  { "en": "Kekosongan kekuasaan?", "id": "Vakum kekuasaan." },
  { "en": "Hegemoni politik?", "id": "Dominasi mutlak." },
  { "en": "Politik anggaran?", "id": "Alokasi dana." },
  { "en": "Kebijakan fiskal?", "id": "Pengaturan pajak." },
  { "en": "Kebijakan moneter?", "id": "Pengaturan uang." },
  { "en": "Intervensi negara?", "id": "Campur tangan." },
  { "en": "Demagogi politik?", "id": "Hasutan massa." },
  { "en": "Suara mayoritas?", "id": "Kemenangan mutlak." },
  { "en": "Suara minoritas?", "id": "Kelompok kecil." },
  { "en": "Blok politik?", "id": "Aliansi strategis." },
  { "en": "Gerilya politik?", "id": "Perlawanan tersembunyi." },
  { "en": "Impeachment?", "id": "Proses pemakzulan." },
  { "en": "Rezim otoriter?", "id": "Kekuasaan absolut." },
  { "en": "Pemungutan ulang?", "id": "Pemilu ulang." },
  { "en": "Batas parlemen?", "id": "Ambang batas." },
  { "en": "Konvensi partai?", "id": "Rapat besar." },
  { "en": "Fraksi parlemen?", "id": "Kelompok partai." },
  { "en": "Rancangan undang-undang?", "id": "Usulan legislasi." },
  { "en": "Pengesahan undang-undang?", "id": "Persetujuan hukum." },
  { "en": "Hak veto?", "id": "Hak menolak." },
  { "en": "Kabinet pemerintahan?", "id": "Dewan menteri." },
  { "en": "Reshuffle kabinet?", "id": "Perombakan menteri." },
  { "en": "Perdana menteri?", "id": "Kepala pemerintahan." },
  { "en": "Sistem parlementer?", "id": "Eksekutif parlemen." },
  { "en": "Sistem presidensial?", "id": "Presiden kuat." },
  { "en": "Negara kesatuan?", "id": "Pemerintah pusat." },
  { "en": "Negara serikat?", "id": "Negara bagian." },
  { "en": "Kedaulatan rakyat?", "id": "Kekuasaan tertinggi." },
  { "en": "Warga negara?", "id": "Status hukum." },
  { "en": "Dwi kewarganegaraan?", "id": "Dua paspor." },
  { "en": "Naturalisasi warga?", "id": "Proses pewarganegaraan." },
  { "en": "Ideologi negara?", "id": "Dasar filosofis." },
  { "en": "Pancasila?", "id": "Lima sila." },
  { "en": "Bhinneka Tunggal Ika?", "id": "Berbeda-beda satutujuan." },
  { "en": "Lembaga tinggi negara?", "id": "Institusi utama." },
  { "en": "Mahkamah Konstitusi?", "id": "Pengawal konstitusi." },
  { "en": "Mahkamah Agung?", "id": "Peradilan tertinggi." },
  { "en": "Komisi Yudisial?", "id": "Pengawas hakim." },
  { "en": "Badan Pemeriksa Keuangan?", "id": "Auditor negara." },
  { "en": "Komisi Pemilihan Umum?", "id": "Penyelenggara pemilu." },
  { "en": "Badan Pengawas Pemilu?", "id": "Pengawas pemilu." },
  { "en": "Gubernur?", "id": "Kepala daerahprovinsi." },
  { "en": "Bupati/Walikota?", "id": "Kepala daerahkabupaten/kota." },
  { "en": "Peraturan daerah?", "id": "Hukum lokal." },
  { "en": "Anggaran negara?", "id": "APBN." },
  { "en": "Pendapatan negara?", "id": "Sumber dana." },
  { "en": "Belanja negara?", "id": "Pengeluaran pemerintah." },
  { "en": "Defisit anggaran?", "id": "Kekurangan dana." },
  { "en": "Surplus anggaran?", "id": "Kelebihan dana." },
  { "en": "Utang luar negeri?", "id": "Pinjaman asing." },
  { "en": "Bantuan luar negeri?", "id": "Hibah internasional." },
  { "en": "Hubungan bilateral?", "id": "Dua negara." },
  { "en": "Hubungan multilateral?", "id": "Banyak negara." },
  { "en": "Duta besar?", "id": "Perwakilan negara." },
  { "en": "Konsulat jenderal?", "id": "Perwakilan konsuler." },
  { "en": "Nota diplomatik?", "id": "Surat resmi." },
  { "en": "Azas teritorial?", "id": "Berdasarkan wilayah." },
  { "en": "Ekstradisi pelaku?", "id": "Penyerahan kriminal." },
  { "en": "Suaka politik?", "id": "Perlindungan politik." },
  { "en": "Gerakan non-blok?", "id": "Sikap netral." },
  { "en": "Politik bebas aktif?", "id": "Sikap independen." },
  { "en": "Blokade ekonomi?", "id": "Isolasi finansial." },
  { "en": "Embargo senjata?", "id": "Larangan penjualan." },
  { "en": "Resolusi PBB?", "id": "Keputusan internasional." },
  { "en": "Pasukan perdamaian?", "id": "Misi militer." },
  { "en": "Kejahatan perang?", "id": "Pelanggaran hukum." },
  { "en": "Genosida?", "id": "Pemusnahan etnis." },
  { "en": "Keadilan transisional?", "id": "Pemulihan konflik." },
  { "en": "Rekonsiliasi nasional?", "id": "Perdamaian internal." },
  { "en": "Pluralisme politik?", "id": "Keragaman pandangan." },
  { "en": "Toleransi politik?", "id": "Menghargai perbedaan." },
  { "en": "Budaya politik?", "id": "Sikap warga." },
  { "en": "Partisipasi politik?", "id": "Keterlibatan warga." },
  { "en": "Sosialisasi politik?", "id": "Pembelajaran nilai." },
  { "en": "Komunikasi politik?", "id": "Penyampaian pesan." },
  { "en": "Elit politik?", "id": "Kelompok berkuasa." },
  { "en": "Massa mengambang?", "id": "Pemilih ragu." },
  { "en": "Swing voters?", "id": "Pemilih bimbang." },
  { "en": "Stagnasi politik?", "id": "Kemandekan proses." },
  { "en": "Stabilitas politik?", "id": "Kondisi aman." },
  { "en": "Krisis politik?", "id": "Gejolak besar." },
  { "en": "Suksesi kepemimpinan?", "id": "Peralihan kekuasaan." },
  { "en": "Legitimasi pemerintah?", "id": "Keabsahan kekuasaan." },
  { "en": "Otoritas karismatik?", "id": "Pengaruh pribadi." },
  { "en": "Otoritas tradisional?", "id": "Berdasarkan adat." },
  { "en": "Otoritas legal-rasional?", "id": "Berdasarkan hukum." },
  { "en": "Negara hukum?", "id": "Rule of law." },
  { "en": "Kepastian hukum?", "id": "Jaminan hukum." },
  { "en": "Penegakan hukum?", "id": "Aparat hukum." },
  { "en": "Korupsi politik?", "id": "Penyalahgunaan jabatan." },
  { "en": "Kolusi?", "id": "Persekongkolan ilegal." },
  { "en": "Nepotisme?", "id": "Pilih kasihkerabat." },
  { "en": "Gratifikasi?", "id": "Pemberian hadiah." },
  { "en": "Suap?", "id": "Uang pelicin." },
  { "en": "Pencucian uang?", "id": "Menyembunyikan dana." },
  { "en": "Ombudsman?", "id": "Pengawas pelayanan." },
  { "en": "Petahana?", "id": "Pejabat menjabat." },
  { "en": "Juru bicara?", "id": "Penyampai informasi." },
  { "en": "Konferensi pers?", "id": "Pernyataan resmi." },
  { "en": "Rilis pers?", "id": "Berita tertulis." },
  { "en": "Serangan fajar?", "id": "Bagi-bagi uang." },
  { "en": "Garis keras?", "id": "Kelompok ekstrem." },
  { "en": "Garis lunak?", "id": "Kelompok moderat." },
  { "en": "Status quo?", "id": "Kondisi sekarang." },
  { "en": "Reformasi politik?", "id": "Perubahan sistem." },
  { "en": "Revolusi?", "id": "Perubahan drastis." },
  { "en": "Aneksasi wilayah?", "id": "Pencaplokan paksa." },
  { "en": "Proklamasi kemerdekaan?", "id": "Pernyataan merdeka." },
  { "en": "Dekrit presiden?", "id": "Keputusan darurat." },
  { "en": "Amnesti?", "id": "Pengampunan umum." },
  { "en": "Abolisi?", "id": "Pembatalan tuntutan." },
  { "en": "Juru lobi?", "id": "Pelobi profesional." },
  { "en": "Kelompok kepentingan?", "id": "Grup penekan." },
  { "en": "Anggaran dasar?", "id": "Aturan internal." },
  { "en": "Platform partai?", "id": "Program politik." },
  { "en": "Kaukus politik?", "id": "Pertemuan internal." },
  { "en": "Indoktrinasi politik?", "id": "Penanaman ideologi." },
  { "en": "Mobilisasi massa?", "id": "Pengerahan orang." },
  { "en": "Demobilisasi?", "id": "Penghentian pengerahan." },
  { "en": "Politik transaksional?", "id": "Politik dagangsapi." },
  { "en": "Politik aliran?", "id": "Berdasarkan ideologi." },
  { "en": "Sekularisme?", "id": "Pemisahan agama." },
  { "en": "Negara teokratis?", "id": "Berdasarkan agama." },
  { "en": "Hukum sipil?", "id": "Hukum privat." },
  { "en": "Hukum pidana?", "id": "Hukum publik." },
  { "en": "Sistem peradilan?", "id": "Struktur hukum." },
  { "en": "Preseden hukum?", "id": "Putusan terdahulu." },
  { "en": "Yurisprudensi?", "id": "Ilmu hukum." },
  { "en": "Banding?", "id": "Upaya hukum." },
  { "en": "Kasasi?", "id": "Banding tertinggi." },
  { "en": "Peninjauan kembali?", "id": "Upaya luar biasa." },
  { "en": "Gugatan perdata?", "id": "Tuntutan pribadi." },
  { "en": "Terdakwa?", "id": "Pihak tergugat." },
  { "en": "Jaksa penuntut?", "id": "Penuntut umum." },
  { "en": "Hakim?", "id": "Pemutus perkara." },
  { "en": "Advokat?", "id": "Pengacara hukum." },
  { "en": "Saksi ahli?", "id": "Pemberi keterangan." },
  { "en": "Barang bukti?", "id": "Alat pembuktian." },
  { "en": "Putusan pengadilan?", "id": "Vonis hakim." },
  { "en": "Hukuman mati?", "id": "Vonis fatal." },
  { "en": "Hukuman seumur hidup?", "id": "Penjara permanen." },
  { "en": "Grasi presiden?", "id": "Pengampunan hukuman." },
  { "en": "Rehabilitasi nama?", "id": "Pemulihan nama." },
  { "en": "Masyarakat adat?", "id": "Komunitas lokal." },
  { "en": "Hak ulayat?", "id": "Hak komunal." },
  { "en": "Pemberdayaan masyarakat?", "id": "Penguatan komunitas." },
  { "en": "Pembangunan berkelanjutan?", "id": "Pembangunan ramahlingkungan." },
  { "en": "Ekonomi kerakyatan?", "id": "Sistem pro-rakyat." },
  { "en": "Koperasi?", "id": "Usaha bersama." },
  { "en": "Badan Usaha Milik Negara?", "id": "Perusahaan negara." },
  { "en": "Privatisasi aset?", "id": "Penjualan BUMN." },
  { "en": "Pasar bebas?", "id": "Liberalisme ekonomi." },
  { "en": "Proteksionisme dagang?", "id": "Perlindungan produk." },
  { "en": "Tarif impor?", "id": "Pajak masuk." },
  { "en": "Kuota impor?", "id": "Pembatasan barang." },
  { "en": "Perang dagang?", "id": "Konflik ekonomi." },
  { "en": "Globalisasi ekonomi?", "id": "Integrasi pasar." },
  { "en": "Dana Moneter Internasional?", "id": "IMF." },
  { "en": "Bank Dunia?", "id": "World Bank." },
  { "en": "Organisasi Perdagangan Dunia?", "id": "WTO." },
  { "en": "Blok ekonomi?", "id": "Kawasan dagang." },
  { "en": "Masyarakat Ekonomi ASEAN?", "id": "MEA." },
  { "en": "Uni Eropa?", "id": "Blok Eropa." },
  { "en": "Mata uang tunggal?", "id": "Satu moneter." },
  { "en": "Resesi ekonomi?", "id": "Perlambatan ekonomi." },
  { "en": "Inflasi?", "id": "Kenaikan harga." },
  { "en": "Deflasi?", "id": "Penurunan harga." },
  { "en": "Pertumbuhan ekonomi?", "id": "Peningkatan PDB." },
  { "en": "Produk Domestik Bruto?", "id": "Nilai ekonomi." },
  { "en": "Ketimpangan ekonomi?", "id": "Kesenjangan pendapatan." },
  { "en": "Kemiskinan absolut?", "id": "Kekurangan ekstrim." },
  { "en": "Indeks pembangunan manusia?", "id": "Ukuran kesejahteraan." },
  { "en": "Jaminan sosial?", "id": "Perlindungan warga." },
  { "en": "Asuransi kesehatan?", "id": "Jaminan kesehatan." },
  { "en": "Pendidikan gratis?", "id": "Sekolah publik." },
  { "en": "Wajib belajar?", "id": "Pendidikan dasar." },
  { "en": "Hak cipta?", "id": "Perlindungan karya." },
  { "en": "Paten?", "id": "Hak eksklusif." },
  { "en": "Kebebasan pers?", "id": "Kemerdekaan media." },
  { "en": "Dewan Pers?", "id": "Pengawas media." },
  { "en": "Kode etik jurnalistik?", "id": "Aturan wartawan." },
  { "en": "Berita bohong?", "id": "Hoax." },
  { "en": "Ujaran kebencian?", "id": "Hate speech." },
  { "en": "Persekusi?", "id": "Pemburuan sewenang-wenang." },
  { "en": "Intimidasi politik?", "id": "Ancaman psikologis." },
  { "en": "Kriminalisasi?", "id": "Proses pidana." },
  { "en": "Machiavellianisme?", "id": "Politik licik." },
  { "en": "Populisme?", "id": "Retorika kerakyatan." },
  { "en": "Oportunisme politik?", "id": "Mencari keuntungan." },
  { "en": "Pragmatisme politik?", "id": "Pendekatan praktis." },
  { "en": "Reaksioner?", "id": "Penentang kemajuan." },
  { "en": "Progresif?", "id": "Pendukung kemajuan." },
  { "en": "Sentrisme?", "id": "Posisi tengah." },
  { "en": "Ekstremisme?", "id": "Pandangan radikal." },
  { "en": "Fundamentalisme?", "id": "Ajaran dasar." },
  { "en": "Fasisme?", "id": "Nasionalisme otoriter." },
  { "en": "Komunisme?", "id": "Masyarakat tanpa kelas." },
  { "en": "Sosialisme?", "id": "Kepemilikan sosial." },
  { "en": "Kapitalisme?", "id": "Pasar bebas." },
  { "en": "Liberalisme?", "id": "Kebebasan individu." },
  { "en": "Konservatisme?", "id": "Mempertahankan tradisi." },
  { "en": "Anarkisme?", "id": "Tanpa negara." },
  { "en": "Feminisime?", "id": "Kesetaraan gender." },
  { "en": "Environmentalisme?", "id": "Gerakan lingkungan." },
  { "en": "Pasifisme?", "id": "Anti-perang." },
  { "en": "Absenteisme?", "id": "Ketidakhadiran." },
  { "en": "Ad hoc?", "id": "Bersifat sementara." },
  { "en": "Pemimpin de facto?", "id": "Berkuasa nyata." },
  { "en": "Pemimpin de jure?", "id": "Berkuasa hukum." },
  { "en": "Menteri tanpa portofolio?", "id": "Menteri khusus." },
  { "en": "Duta keliling?", "id": "Utusan khusus." },
  { "en": "Kuasa usaha?", "id": "Pejabat diplomatik." },
  { "en": "Atase pertahanan?", "id": "Perwakilan militer." },
  { "en": "Nota kesepahaman?", "id": "Memorandum of Understanding." },
  { "en": "Perjanjian ekstradisi?", "id": "Penyerahan buronan." },
  { "en": "Zona demiliterisasi?", "id": "Area bebasmiliter." },
  { "en": "Garis demarkasi?", "id": "Garis batas." },
  { "en": "Sengketa perbatasan?", "id": "Konflik wilayah." },
  { "en": "Mahkamah Internasional?", "id": "Pengadilan dunia." },
  { "en": "Arbitrase internasional?", "id": "Penyelesaian sengketa." },
  { "en": "Blokade militer?", "id": "Pengepungan total." },
  { "en": "Invasi militer?", "id": "Serangan bersenjata." },
  { "en": "Okupasi wilayah?", "id": "Pendudukan paksa." },
  { "en": "Pemerintahan boneka?", "id": "Pemerintah terkendali." },
  { "en": "Negara klien?", "id": "Negara tergantung." },
  { "en": "Perang proksi?", "id": "Perang perwakilan." },
  { "en": "Perang saudara?", "id": "Konflik internal." },
  { "en": "Gencatan senjata?", "id": "Penghentian tembakan." },
  { "en": "Perjanjian damai?", "id": "Kesepakatan akhir." },
  { "en": "Keamanan kolektif?", "id": "Pertahanan bersama." },
  { "en": "Aliansi militer?", "id": "Pakta pertahanan." },
  { "en": "Keseimbangan kekuasaan?", "id": "Balance of power." },
  { "en": "Unipolaritas?", "id": "Satu kekuatan." },
  { "en": "Bipolaritas?", "id": "Dua kekuatan." },
  { "en": "Multipolaritas?", "id": "Banyak kekuatan." },
  { "en": "Interdependensi global?", "id": "Saling ketergantungan." },
  { "en": "Kedaulatan negara?", "id": "Kekuasaan tertinggi." },
  { "en": "Intervensi kemanusiaan?", "id": "Campur tangan." },
  { "en": "Tanggung jawab melindungi?", "id": "Responsibility to Protect." },
  { "en": "Diplomasi publik?", "id": "Pendekatan masyarakat." },
  { "en": "Kekuatan lunak?", "id": "Pengaruh budaya." },
  { "en": "Kekuatan keras?", "id": "Pengaruh militer." },
  { "en": "Kekuatan pintar?", "id": "Kombinasi kekuatan." },
  { "en": "Disinformasi?", "id": "Informasi salah." },
  { "en": "Misinformasi?", "id": "Penyebaran ketidaktahuan." },
  { "en": "Geopolitik?", "id": "Politik geografis." },
  { "en": "Teori domino?", "id": "Efek berantai." },
  { "en": "Pembendungan?", "id": "Containment policy." },
  { "en": "Politik isolasi?", "id": "Menutup diri." },
  { "en": "Merkantilisme?", "id": "Ekonomi nasionalis." },
  { "en": "Neokolonialisme?", "id": "Penjajahan baru." },
  { "en": "Gerakan kemerdekaan?", "id": "Perjuangan nasional." },
  { "en": "Dekolonisasi?", "id": "Pelepasan jajahan." },
  { "en": "Identitas nasional?", "id": "Jati diri." },
  { "en": "Integrasi nasional?", "id": "Penyatuan bangsa." },
  { "en": "Disintegrasi bangsa?", "id": "Perpecahan negara." },
  { "en": "Asimilasi budaya?", "id": "Peleburan budaya." },
  { "en": "Akulturasi budaya?", "id": "Percampuran budaya." },
  { "en": "Multikulturalisme?", "id": "Keragaman budaya." },
  { "en": "Xenofobia?", "id": "Anti-asing." },
  { "en": "Rasisme?", "id": "Diskriminasi ras." },
  { "en": "Etnosentrisme?", "id": "Fanatisme suku." },
  { "en": "Chauvinisme?", "id": "Nasionalisme sempit." },
  { "en": "Apatisme politik?", "id": "Sikap acuh." },
  { "en": "Sinisme politik?", "id": "Sikap pesimis." },
  { "en": "Pendidikan kewarganegaraan?", "id": "Pendidikan politik." },
  { "en": "Etika politik?", "id": "Moralitas berpolitik." },
  { "en": "Korupsi sistemik?", "id": "Korupsi terstruktur." },
  { "en": "Kleptokrasi?", "id": "Pemerintahan pencuri." },
  { "en": "Negara gagal?", "id": "Pemerintahan lumpuh." },
  { "en": "Good governance?", "id": "Tata kelola baik." },
  { "en": "Clean government?", "id": "Pemerintahan bersih." },
  { "en": "Birokrasi ramping?", "id": "Struktur efisien." },
  { "en": "Reformasi birokrasi?", "id": "Perbaikan layanan." },
  { "en": "E-government?", "id": "Pemerintahan digital." },
  { "en": "Pelayanan publik?", "id": "Layanan masyarakat." },
  { "en": "Standar pelayanan?", "id": "Ukuran kualitas." },
  { "en": "Pengaduan masyarakat?", "id": "Laporan warga." },
  { "en": "Wistleblower?", "id": "Pengungkap fakta." },
  { "en": "Perlindungan saksi?", "id": "Jaminan keamanan." },
  { "en": "Sistem proporsional?", "id": "Kursi berimbang." },
  { "en": "Sistem distrik?", "id": "Satu wakil." },
  { "en": "Verifikasi partai?", "id": "Pemeriksaan syarat." },
  { "en": "Kampanye terbuka?", "id": "Rapat umum." },
  { "en": "Kampanye tertutup?", "id": "Pertemuan terbatas." },
  { "en": "Dana kampanye?", "id": "Biaya politik." },
  { "en": "Laporan dana?", "id": "Transparansi keuangan." },
  { "en": "Sengketa pemilu?", "id": "Gugatan hasil." },
  { "en": "Politik harapan?", "id": "Pesan positif." },
  { "en": "Politik ketakutan?", "id": "Pesan negatif." },
  { "en": "Kartel politik?", "id": "Kolusi partai." },
  { "en": "Check and balance?", "id": "Saling mengawasi." },
  { "en": "Kultus individu?", "id": "Pemujaan pemimpin." },
  { "en": "Nepotisme kroni?", "id": "Favoritisme teman." },
  { "en": "Legislasi?", "id": "Proses hukum." },
  { "en": "Voting?", "id": "Pemungutan suara." },
  { "en": "Lobi?", "id": "Upaya persuasi." },
  { "en": "Petisi?", "id": "Tuntutan tertulis." },
  { "en": "Amicus curiae?", "id": "Sahabat pengadilan." },
  { "en": "Contempt of court?", "id": "Penghinaan pengadilan." },
  { "en": "Judicial review?", "id": "Uji materiil." },
  { "en": "Dissenting opinion?", "id": "Pendapat berbeda." },
  { "en": "Pro bono?", "id": "Bantuan hukumgratis." },
  { "en": "Status quo ante bellum?", "id": "Keadaan sebelumperang." },
  { "en": "Persona non grata?", "id": "Orang tidakdiinginkan." },
  { "en": "Pakta pertahanan?", "id": "Aliansi militer." },
  { "en": "Realpolitik?", "id": "Politik praktis." },
  { "en": "Jingoism?", "id": "Nasionalisme agresif." },
  { "en": "Egalitarianisme?", "id": "Prinsip kesetaraan." },
  { "en": "Libertarianisme?", "id": "Kebebasan maksimal." },
  { "en": "Komunitarianisme?", "id": "Fokus komunitas." },
  { "en": "Totalitarianisme?", "id": "Kontrol total." },
  { "en": "Perestroika?", "id": "Restrukturisasi ekonomi." },
  { "en": "Glasnost?", "id": "Keterbukaan politik." },
  { "en": "Apartheid?", "id": "Pemisahan ras." },
  { "en": "Balkanisasi?", "id": "Perpecahan wilayah." },
  { "en": "Finlandisasi?", "id": "Netralitas terpaksa." },
  { "en": "Negara penyangga?", "id": "Buffer state." },
  { "en": "Negara maritim?", "id": "Negara kelautan." },
  { "en": "Negara agraris?", "id": "Negara pertanian." },
  { "en": "Masa antara?", "id": "Interregnum." },
  { "en": "Gerakan bawah tanah?", "id": "Perlawanan rahasia." },
  { "en": "Pemerintahan dalam pengasingan?", "id": "Pemerintahan eksil." },
  { "en": "Wilayah sengketa?", "id": "Daerah konflik." },
  { "en": "Enklave?", "id": "Wilayah terkurung." },
  { "en": "Eksklave?", "id": "Wilayah terpisah." },
  { "en": "Daerah otonom?", "id": "Wilayah khusus." },
  { "en": "Ibu kota?", "id": "Pusat pemerintahan." },
  { "en": "Perubahan iklim?", "id": "Isu global." },
  { "en": "Diplomasi lingkungan?", "id": "Negosiasi iklim." },
  { "en": "Pajak karbon?", "id": "Pajak emisi." },
  { "en": "Ekonomi hijau?", "id": "Ekonomi berkelanjutan." },
  { "en": "Kedaulatan pangan?", "id": "Kemandirian pangan." },
  { "en": "Krisis pengungsi?", "id": "Arus migrasi." },
  { "en": "Imigrasi ilegal?", "id": "Pendatang gelap." },
  { "en": "Perdagangan manusia?", "id": "Kejahatan transnasional." },
  { "en": "Keamanan siber?", "id": "Perlindungan digital." },
  { "en": "Perang siber?", "id": "Serangan digital." },
  { "en": "Spionase?", "id": "Mata-mata." },
  { "en": "Kontra-intelijen?", "id": "Lawan spionase." },
  { "en": "Proliferasi nuklir?", "id": "Penyebaran nuklir." },
  { "en": "Perlucutan senjata?", "id": "Pengurangan senjata." },
  { "en": "Perang asimetris?", "id": "Kekuatan tidakseimbang." },
  { "en": "Doktrin militer?", "id": "Prinsip perang." },
  { "en": "Hukum darurat militer?", "id": "Keadaan bahaya." },
  { "en": "Jam malam?", "id": "Pembatasan gerak." },
  { "en": "Sensor mandiri?", "id": "Pembatasan diri." },
  { "en": "Efek bandwagon?", "id": "Ikut-ikutan." },
  { "en": "Efek underdog?", "id": "Dukung yanglemah." },
  { "en": "Gerrymandering?", "id": "Manipulasi dapil." },
  { "en": "Filibuster?", "id": "Menghambat sidang." },
  { "en": "Pork barrel politics?", "id": "Politik akomodatif." },
  { "en": "Logrolling?", "id": "Saling dukung." },
  { "en": "Politik gentong babi?", "id": "Proyek daerah." },
  { "en": "Spoils system?", "id": "Bagi-bagi jabatan." },
  { "en": "Merit system?", "id": "Sistem prestasi." },
  { "en": "Senioritas politik?", "id": "Berdasarkan pengalaman." },
  { "en": "Vote of no confidence?", "id": "Mosi tidakpercaya." },
  { "en": "Recall election?", "id": "Pemilu ulang." },
  { "en": "Konstituen?", "id": "Basis pemilih." },
  { "en": "Komite aksi politik?", "id": "Penggalang dana." },
  { "en": "Kandidat independen?", "id": "Calon non-partai." },
  { "en": "Pemerintahan teknokratis?", "id": "Pemerintahan ahli." },
  { "en": "Plutokrasi?", "id": "Pemerintahan orangkaya." },
  { "en": "Kakistokrasi?", "id": "Pemerintahan terburuk." },
  { "en": "Stratokrasi?", "id": "Pemerintahan militer." },
  { "en": "Meritokrasi?", "id": "Pemerintahan berprestasi." },
  { "en": "Anokrasi?", "id": "Rezim campuran." },
  { "en": "Keterwakilan perempuan?", "id": "Kuota gender." },
  { "en": "Politik afirmatif?", "id": "Tindakan khusus." },
  { "en": "Hukum adat?", "id": "Aturan tradisional." },
  { "en": "Perdamaian abadi?", "id": "Utopia politik." },
  { "en": "Kewajiban sipil?", "id": "Tugas warganegara." },
  { "en": "Pembangkangan sipil?", "id": "Penolakan damai." },
  { "en": "Veto saku?", "id": "Pocket veto." },
  { "en": "Rider legislatif?", "id": "Pasal titipan." },
  { "en": "Sunset provision?", "id": "Klausul kedaluwarsa." },
  { "en": "Amandemen beracun?", "id": "Poison pill." },
  { "en": "Quorum?", "id": "Batas minimum." },
  { "en": "Masa reses?", "id": "Masa istirahat." },
  { "en": "Interpelasi?", "id": "Hak bertanya." },
  { "en": "Hak angket?", "id": "Hak menyelidiki." },
  { "en": "Hak menyatakan pendapat?", "id": "Sikap resmi." },
  { "en": "Pidato kenegaraan?", "id": "Amanat presiden." },
  { "en": "Inaugurasi?", "id": "Pelantikan resmi." },
  { "en": "Serah terima jabatan?", "id": "Peralihan tugas." },
  { "en": "Demisioner?", "id": "Status sementara." },
  { "en": "Kolegial?", "id": "Kepemimpinan bersama." },
  { "en": "Sentralisme demokratis?", "id": "Diskusi internal." },
  { "en": "Negara kesejahteraan?", "id": "Welfare state." },
  { "en": "Neoliberalisme?", "id": "Kapitalisme lanjut." },
  { "en": "Sosial demokrasi?", "id": "Kapitalisme terkendali." },
  { "en": "Politik post-truth?", "id": "Politik pasca-kebenaran." },
  { "en": "Echo chamber?", "id": "Ruang gema." },
  { "en": "Filter bubble?", "id": "Gelembung informasi." },
  { "en": "Astroturfing?", "id": "Dukungan palsu." },
  { "en": "Oposisi loyal?", "id": "Kritik membangun." },
  { "en": "Tatanan dunia baru?", "id": "Struktur global." },
  { "en": "Soft power?", "id": "Daya tarik." },
  { "en": "Politik air?", "id": "Hidropolitik." },
  { "en": "Wilayah yurisdiksi?", "id": "Cakupan hukum." },
  { "en": "Kedaulatan udara?", "id": "Kontrol langit." },
  { "en": "Zona Ekonomi Eksklusif?", "id": "Hak laut." },
  { "en": "Landas kontinen?", "id": "Dasar laut." },
  { "en": "Diplomasi kapal perang?", "id": "Gunboat diplomacy." },
  { "en": "Iredentisme?", "id": "Klaim wilayah." },
  { "en": "Lebensraum?", "id": "Ruang hidup." },
  { "en": "Sphere of influence?", "id": "Lingkup pengaruh." },
  { "en": "Heartland theory?", "id": "Teori geopolitik." },
  { "en": "Rimland theory?", "id": "Teori pinggiran." },
  { "en": "Shatterbelt region?", "id": "Kawasan pecahbelah." },
  { "en": "Perang pre-emptif?", "id": "Serangan duluan." },
  { "en": "Perang preventif?", "id": "Serangan pencegahan." },
  { "en": "Brinkmanship?", "id": "Politik ambangbatas." },
  { "en": "Detente?", "id": "Reda ketegangan." },
  { "en": "Appeasement?", "id": "Politik peredaan." },
  { "en": "Realokasi anggaran?", "id": "Pergeseran dana." },
  { "en": "Pemotongan anggaran?", "id": "Penghematan dana." },
  { "en": "Pagu anggaran?", "id": "Batas tertinggi." },
  { "en": "Politik anggaran partisipatif?", "id": "Keterlibatan publik." },
  { "en": "Dana perimbangan?", "id": "Transfer pusat." },
  { "en": "Dana Otonomi Khusus?", "id": "Dana Otsus." },
  { "en": "Pendapatan Asli Daerah?", "id": "Sumber lokal." },
  { "en": "Resentralisasi?", "id": "Penarikan wewenang." },
  { "en": "Dekonsentrasi?", "id": "Pelimpahan wewenang." },
  { "en": "Tugas pembantuan?", "id": "Medebewind." },
  { "en": "Agenda setting?", "id": "Penentuan prioritas." },
  { "en": "Framing media?", "id": "Pembingkaian berita." },
  { "en": "Priming media?", "id": "Pengutamaan isu." },
  { "en": "Spiral of silence?", "id": "Spiral keheningan." },
  { "en": "Two-step flow communication?", "id": "Aliran duatahap." },
  { "en": "Gatekeeping?", "id": "Penjagaan gerbang." },
  { "en": "Spin doctor?", "id": "Pakar pencitraan." },
  { "en": "Sound bite?", "id": "Kutipan singkat." },
  { "en": "Photo opportunity?", "id": "Sesi foto." },
  { "en": "Canvassing?", "id": "Mendatangi pemilih." },
  { "en": "Exit poll?", "id": "Survei pascapemilu." },
  { "en": "Approval rating?", "id": "Tingkat kepuasan." },
  { "en": "Presidential honeymoon?", "id": "Masa awal." },
  { "en": "Lame duck session?", "id": "Periode transisi." },
  { "en": "Partai kader?", "id": "Partai elit." },
  { "en": "Partai massa?", "id": "Partai rakyat." },
  { "en": "Partai catch-all?", "id": "Partai payung." },
  { "en": "Cleavage politik?", "id": "Patahan sosial." },
  { "en": "Sistem kepartaian?", "id": "Struktur partai." },
  { "en": "Volatilitas elektoral?", "id": "Perubahan dukungan." },
  { "en": "Realigning election?", "id": "Pemilu penataanulang." },
  { "en": "Dealigning election?", "id": "Pemilu pelemahan." },
  { "en": "Mandat elektoral?", "id": "Dukungan pemilih." },
  { "en": "Inkumben?", "id": "Pejabat bertahan." },
  { "en": "Challenger?", "id": "Kandidat penantang." },
  { "en": "Koalisi besar?", "id": "Grand coalition." },
  { "en": "Koalisi pelangi?", "id": "Rainbow coalition." },
  { "en": "Kabinet minoritas?", "id": "Dukungan kurang." },
  { "en": "Kabinet mayoritas?", "id": "Dukungan penuh." },
  { "en": "Pemerintahan persatuan nasional?", "id": "Pemerintahan gabungan." },
  { "en": "Oposisi konstruktif?", "id": "Mengkritik membangun." },
  { "en": "Oposisi destruktif?", "id": "Menjatuhkan pemerintah." },
  { "en": "Parlemen tergantung?", "id": "Hung parliament." },
  { "en": "Gridlock politik?", "id": "Kemacetan legislatif." },
  { "en": "Separation of powers?", "id": "Pemisahan kekuasaan." },
  { "en": "Fusion of powers?", "id": "Penyatuan kekuasaan." },
  { "en": "Sovereign default?", "id": "Gagal bayarutang." },
  { "en": "Bailout?", "id": "Dana talangan." },
  { "en": "Austerity?", "id": "Kebijakan penghematan." },
  { "en": "Stagflasi?", "id": "Stagnasi inflasi." },
  { "en": "Contagion effect?", "id": "Efek penularan." },
  { "en": "Moral hazard?", "id": "Risiko moral." },
  { "en": "Adverse selection?", "id": "Seleksi merugikan." },
  { "en": "Rent-seeking?", "id": "Mencari rente." },
  { "en": "Iron triangle?", "id": "Segitiga besi." },
  { "en": "Issue network?", "id": "Jaringan isu." },
  { "en": "Principal-agent problem?", "id": "Masalah prinsipal-agen." },
  { "en": "Path dependency?", "id": "Ketergantungan alur." },
  { "en": "Critical juncture?", "id": "Momen krusial." },
  { "en": "Rational choice theory?", "id": "Teori pilihanrasional." },
  { "en": "Public choice theory?", "id": "Teori pilihanpublik." },
  { "en": "Game theory?", "id": "Teori permainan." },
  { "en": "Prisoner's dilemma?", "id": "Dilema tahanan." },
  { "en": "Zero-sum game?", "id": "Permainan nirjumlah." },
  { "en": "Positive-sum game?", "id": "Permainan positif." },
  { "en": "Collective action problem?", "id": "Masalah aksi kolektif." },
  { "en": "Free-rider problem?", "id": "Masalah penunggangbebas." },
  { "en": "Tragedy of the commons?", "id": "Tragedi kepemilikanbersama." },
  { "en": "Social contract?", "id": "Kontrak sosial." },
  { "en": "State of nature?", "id": "Keadaan alamiah." },
  { "en": "Natural rights?", "id": "Hak-hak alamiah." },
  { "en": "General will?", "id": "Kehendak umum." },
  { "en": "The common good?", "id": "Kebaikan bersama." },
  { "en": "Civic virtue?", "id": "Kebajikan sipil." },
  { "en": "Rule by law?", "id": "Pemerintahan olehhukum." },
  { "en": "Rule of man?", "id": "Pemerintahan olehmanusia." },
  { "en": "Tyranny of the majority?", "id": "Tirani mayoritas." },
  { "en": "Consent of the governed?", "id": "Persetujuan terperintah." }


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
