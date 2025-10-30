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


  { "en": "Apa Perintah 'uptime' Linux?", "id": "Menampilkan Waktu Hidup (Uptime) Sistem." },
  { "en": "Apa Perintah 'uname -a' Linux?", "id": "Menampilkan Semua Informasi Sistem, Kernel." },
  { "en": "Apa Perintah 'tar -cvf'?", "id": "Membuat Arsip Tar (Create, Verbose, File)." },
  { "en": "Apa Perintah 'tar -xvf'?", "id": "Mengekstrak Arsip Tar (Extract, Verbose, File)." },
  { "en": "Apa Perintah 'gzip' Linux?", "id": "Utilitas Kompresi File (Format .gz)." },
  { "en": "Apa Perintah 'gunzip' Linux?", "id": "Utilitas Dekompresi File (Format .gz)." },
  { "en": "Apa Perintah 'ssh-keygen'?", "id": "Membuat Pasangan Kunci SSH (Publik, Privat)." },
  { "en": "Apa Perintah 'nslookup'?", "id": "Alat Kueri DNS (Mencari IP Domain)." },
  { "en": "Apa Perintah 'dig' (Domain Information Groper)?", "id": "Alat Kueri DNS (Lebih Detail)." },
  { "en": "Apa Itu File '/var/log/syslog'?", "id": "Log Sistem Utama, Kernel, Layanan." },
  { "en": "Apa Perintah Git 'git tag'?", "id": "Menandai Commit Spesifik (Contoh: Rilis v1.0)." },
  { "en": "Apa Perintah Git 'git cherry-pick'?", "id": "Memilih, Menerapkan Commit, Cabang Lain." },
  { "en": "Apa Perintah Git 'git rebase -i'?", "id": "Menggabungkan Commit, Interaktif (Squash, Edit)." },
  { "en": "Apa Perintah Git 'git blame'?", "id": "Menampilkan Penulis, Setiap Baris File." },
  { "en": "Apa Perintah Git 'git reflog'?", "id": "Mencatat Riwayat Perubahan, HEAD (Pemulihan)." },
  { "en": "Apa Itu WebRTC (Web Real-Time Communication)?", "id": "API Browser, Komunikasi Audio, Video Real-Time." },
  { "en": "Apa Itu Service Mesh (Jala Layanan)?", "id": "Lapisan Infrastruktur, Mengelola Komunikasi Layanan (Mikro)." },
  { "en": "Apa Itu Istio?", "id": "Platform Service Mesh, Open-Source Populer." },
  { "en": "Apa Itu gRPC (Google Remote Procedure Call)?", "id": "Kerangka Kerja RPC, Kinerja Tinggi (Google)." },
  { "en": "Apa Itu Protocol Buffers (Protobuf)?", "id": "Format Serialisasi Data, Biner (Efisien)." },
  { "en": "Apa Metode HTTP OPTIONS?", "id": "Meminta Opsi Komunikasi, Diizinkan Server." },
  { "en": "Apa Metode HTTP HEAD?", "id": "Meminta Header Respons, (Sama Seperti GET, Tanpa Body)." },
  { "en": "Apa Metode HTTP CONNECT?", "id": "Membangun Terowongan (Tunnel), Ke Server (Proxy)." },
  { "en": "Apa Metode HTTP TRACE?", "id": "Melakukan Tes Loop-Back, Jalur Permintaan." },
  { "en": "Apa Itu Caching (Singgahan) Sisi Klien?", "id": "Penyimpanan Salinan Data, Di Browser Pengguna." },
  { "en": "Apa Itu Caching (Singgahan) Sisi Server?", "id": "Penyimpanan Salinan Data, Di Server (Aplikasi)." },
  { "en": "Apa Itu Header HTTP 'Cache-Control'?", "id": "Header, Mengatur Kebijakan Caching Browser, Proxy." },
  { "en": "Apa Itu ETag (Entity Tag) HTTP?", "id": "Validator Caching, Versi Sumber Daya (Token)." },
  { "en": "Apa Itu Basis Data Graph (Graf)?", "id": "NoSQL, Fokus Penyimpanan, Kueri Relasi (Node, Edge)." },
  { "en": "Apa Itu Neo4j?", "id": "Platform Basis Data Graph, Populer (Cypher)." },
  { "en": "Apa Itu Fungsi Jendela (Window Function) SQL?", "id": "Melakukan Perhitungan, Kumpulan Baris (Partisi)." },
  { "en": "Apa Itu CTE (Common Table Expression) SQL?", "id": "Tabel Hasil Sementara, (Menggunakan Klausa WITH)." },
  { "en": "Apa Itu Materialized View (Tampilan Terwujud)?", "id": "View, Menyimpan Hasil Kueri, Fisik (Cache)." },
  { "en": "Apa Itu Indeks Full-Text (Teks Penuh)?", "id": "Indeks Khusus, Pencarian Teks, Dokumen." },
  { "en": "Apa Itu Galat (Error) Tipe I (False Positive)?", "id": "Hasil Tes, Salah Mengidentifikasi Positif (Alarm Palsu)." },
  { "en": "Apa Itu Galat (Error) Tipe II (False Negative)?", "id": "Hasil Tes, Salah Mengidentifikasi Negatif (Meleset)." },
  { "en": "Apa Itu Tahap Reconnaissance (Pengintaian) Pen-Testing?", "id": "Tahap Awal, Pengumpulan Informasi Target." },
  { "en": "Apa Itu Tahap Scanning (Pemindaian) Pen-Testing?", "id": "Tahap Identifikasi, Port Terbuka, Kerentanan." },
  { "en": "Apa Itu Tahap Gaining Access (Mendapatkan Akses) Pen-Testing?", "id": "Tahap Eksploitasi, Mendapatkan Akses Sistem." },
  { "en": "Apa Itu Tahap Maintaining Access (Mempertahankan Akses) Pen-Testing?", "id": "Tahap Memasang Backdoor, Mempertahankan Akses." },
  { "en": "Apa Itu Tahap Covering Tracks (Menutupi Jejak) Pen-Testing?", "id": "Tahap Menghapus Log, Bukti Aktivitas Penyerangan." },
  { "en": "Apa Itu IAM (Identity and Access Management)?", "id": "Kerangka Kerja, Mengelola Identitas Digital, Akses." },
  { "en": "Apa Itu Kerberos?", "id": "Protokol Otentikasi Jaringan, Menggunakan Tiket (Ticket)." },
  { "en": "Apa Itu Server Direktori (Directory Service)?", "id": "Layanan Jaringan, Menyimpan Informasi (LDAP, Active Directory)." },
  { "en": "Apa Itu Active Directory (AD)?", "id": "Layanan Direktori, Dikembangkan Oleh Microsoft (Windows)." },
  { "en": "Apa Itu Domain Controller (Pengontrol Domain)?", "id": "Server, Mengelola Keamanan Jaringan Domain (AD)." },
  { "en": "Apa Itu Group Policy (Kebijakan Grup) AD?", "id": "Fitur, Mengelola Konfigurasi Pengguna, Komputer." },
  { "en": "Apa Itu Dokumentasi (Documentation) Perangkat Lunak?", "id": "Teks, Ilustrasi, Mendeskripsikan Perangkat Lunak." },
  { "en": "Apa Itu Tinjauan Kode (Code Review)?", "id": "Pemeriksaan Sistematis, Kode Sumber (Mencari Bug)." },
  { "en": "Apa Itu Analisis Kode Statis (Static Analysis)?", "id": "Analisis Kode, Tanpa Menjalankan Program (Linting)." },
  { "en": "Apa Itu Analisis Kode Dinamis (Dynamic Analysis)?", "id": "Analisis Kode, Saat Program Dieksekusi (Testing)." },
  { "en": "Apa Itu Unit Testing (Pengujian Unit) Framework?", "id": "Alat Bantu, Penulisan, Eksekusi Pengujian Unit." },
  { "en": "Apa Itu JUnit?", "id": "Kerangka Kerja (Framework) Pengujian Unit, Populer (Java)." },
  { "en": "Apa Itu PyTest?", "id": "Kerangka Kerja (Framework) Pengujian, Populer (Python)." },
  { "en": "Apa Itu Mocking (Mengejek) Dalam Pengujian?", "id": "Membuat Objek Palsu (Mock), Simulasi Perilaku." },
  { "en": "Apa Itu Stub (Rintisan) Dalam Pengujian?", "id": "Objek Pengganti, Mengembalikan Jawaban Tetap (Tes)." },
  { "en": "Apa Itu Ensemble Learning (Pembelajaran Ensemble)?", "id": "Metode ML, Menggabungkan Beberapa Model Prediksi." },
  { "en": "Apa Itu Metode Bagging (Bootstrap Aggregating)?", "id": "Ensemble, Melatih Model Paralel, Data Berbeda." },
  { "en": "Apa Itu Metode Boosting (Peningkatan)?", "id": "Ensemble, Melatih Model Sekuensial, Fokus Kesalahan." },
  { "en": "Apa Itu Algoritma AdaBoost (Adaptive Boosting)?", "id": "Algoritma Boosting, Populer, Adaptif (Iteratif)." },
  { "en": "Apa Itu Algoritma XGBoost (Extreme Gradient Boosting)?", "id": "Implementasi Boosting, Gradien, Kinerja Tinggi." },
  { "en": "Apa Itu Algoritma LightGBM (Light Gradient Boosting Machine)?", "id": "Implementasi Boosting, Gradien, Cepat (Microsoft)." },
  { "en": "Apa Itu PSU (Power Supply Unit) 80 Plus?", "id": "Sertifikasi Efisiensi, PSU (Minimal 80% Efisien)." },
  { "en": "Apa Itu Faktor Bentuk (Form Factor) PSU?", "id": "Ukuran Fisik, Spesifikasi PSU (ATX, SFX)." },
  { "en": "Apa Itu PSU Modular (Modular PSU)?", "id": "PSU, Kabel Dapat Dilepas, Pasang (Manajemen Kabel)." },
  { "en": "Apa Itu PSU Non-Modular (Non-Modular PSU)?", "id": "PSU, Semua Kabel Terpasang Permanen." },
  { "en": "Apa Itu PSU Semi-Modular (Semi-Modular PSU)?", "id": "PSU, Sebagian Kabel Permanen, Sebagian Dilepas." },
  { "en": "Apa Itu Slot Ekspansi (Expansion Slot) PCIe?", "id": "Slot Motherboard, Menambah Kartu (GPU, NIC)." },
  { "en": "Apa Itu Faktor Bentuk (Form Factor) SSD 2.5 Inci?", "id": "Ukuran SSD, Koneksi SATA (Seperti HDD Laptop)." },
  { "en": "Apa Itu Faktor Bentuk (Form Factor) SSD M.2?", "id": "Ukuran SSD, Stik Kecil (Langsung Motherboard)." },
  { "en": "Apa Itu VFS (Virtual File System) OS?", "id": "Lapisan Abstraksi, Antarmuka Seragam, Sistem Berkas." },
  { "en": "Apa Itu Kuota Disk (Disk Quota)?", "id": "Batas Penyimpanan, Ditetapkan Pengguna, Grup." },
  { "en": "Apa Itu Fragmentasi (Fragmentation) Disk?", "id": "File Disimpan, Blok Tidak Berdekatan (HDD Lambat)." },
  { "en": "Apa Itu Defragmentasi (Defragmentation) Disk?", "id": "Proses Menata Ulang, Blok File (HDD)." },
  { "en": "Apakah SSD (Solid State Drive) Perlu Defragmentasi?", "id": "Tidak, Defragmentasi Tidak Diperlukan SSD." },
  { "en": "Apa Itu Mode Teks (Text Mode) vs Mode Biner?", "id": "Cara Membaca, Menulis File (Karakter vs Byte)." },
  { "en": "Apa Itu Seek Time (Waktu Pencarian) HDD?", "id": "Waktu Head, Bergerak Ke Track Disk." },
  { "en": "Apa Itu Rotational Latency (Latensi Rotasi) HDD?", "id": "Waktu Piringan, Berputar Ke Sektor Tepat." },
  { "en": "Apa Itu Transfer Time (Waktu Transfer) HDD?", "id": "Waktu Membaca, Menulis Data Dari Sektor." },
  { "en": "Apa Itu DSL (Domain Specific Language)?", "id": "Bahasa Komputer, Khusus, Domain Masalah Tertentu." },
  { "en": "Contoh DSL (Domain Specific Language)?", "id": "SQL (Database), HTML (Web), CSS (Gaya)." },
  { "en": "Apa Itu Bahasa Serba Guna (General Purpose Language)?", "id": "Bahasa, Dapat Digunakan, Berbagai Aplikasi (Python)." },
  { "en": "Apa Itu IDE (Integrated Development Environment) vs Editor?", "id": "IDE Lingkungan Lengkap, Editor Fokus Teks." },
  { "en": "Contoh IDE (Integrated Development Environment)?", "id": "Visual Studio, IntelliJ IDEA, Eclipse." },
  { "en": "Contoh Editor Kode (Code Editor)?", "id": "Visual Studio Code, Sublime Text, Atom." },
  { "en": "Apa Itu WYSIWYG (What You See Is What You Get)?", "id": "Editor, Menampilkan Hasil Akhir, Saat Mengedit." },
  { "en": "Apa Itu Bahasa Markup (Markup Language)?", "id": "Bahasa, Anotasi Teks, Definisi Struktur (HTML)." },
  { "en": "Apa Itu Bahasa Skrip (Scripting Language)?", "id": "Bahasa Interpreter, Otomatisasi Tugas (JavaScript, Python)." },
  { "en": "Apa Itu Bahasa Kompilasi (Compiled Language)?", "id": "Bahasa, Diterjemahkan, Kode Mesin (C++, Java)." },
  { "en": "Apa Itu Transpilasi (Transpilation)?", "id": "Konversi Kode, Antar Bahasa, Tingkat Abstraksi Sama." },
  { "en": "Apa Itu Ahead-of-Time (AOT) Compilation?", "id": "Kompilasi, Sebelum Eksekusi Program (Build Time)." },
  { "en": "Apa Itu Just-in-Time (JIT) Compilation?", "id": "Kompilasi, Saat Eksekusi Program (Runtime)." },
  { "en": "Apa Itu Platform as a Service (PaaS)?", "id": "Model Awan, Menyediakan Platform Pengembangan Aplikasi." },
  { "en": "Apa Itu Infrastructure as a Service (IaaS)?", "id": "Model Awan, Menyediakan Infrastruktur TI Virtual." },
  { "en": "Apa Itu Software as a Service (SaaS)?", "id": "Model Awan, Menyediakan Aplikasi Perangkat Lunak." },
  { "en": "Apa Itu Function as a Service (FaaS)?", "id": "Model Awan, Serverless, Menjalankan Kode (Fungsi)." },
  { "en": "Apa Itu Skalabilitas Elastis (Elastic Scalability)?", "id": "Kemampuan, Otomatis Menambah, Mengurangi Sumber Daya." },
  { "en": "Apa Itu Vendor Lock-In (Keterikatan Vendor)?", "id": "Ketergantungan, Produk, Layanan Vendor Tunggal." },
  { "en": "Apa Itu Interoperabilitas (Interoperability)?", "id": "Kemampuan Sistem, Bekerja Sama, Sistem Lain." },
  { "en": "Apa Itu API (Application Programming Interface) First Design?", "id": "Pendekatan, Mendesain API, Sebelum Implementasi." },
  { "en": "Apa Itu SDK (Software Development Kit) Mobile?", "id": "Kumpulan Alat, Membangun Aplikasi, Platform Seluler." },
  { "en": "Apa Itu Cross-Origin Resource Sharing (CORS)?", "id": "Mekanisme Keamanan, Pembatasan Permintaan Lintas Origin." },
  { "en": "Apa Itu Kebijakan Asal Sama (Same-Origin Policy)?", "id": "Aturan Keamanan Browser, Membatasi Skrip." },
  { "en": "Apa Perbedaan CORS (Cross-Origin Resource Sharing) Dan SOP?", "id": "SOP Membatasi, CORS Memberi Izin." },
  { "en": "Apa Itu Integritas Sub-Sumber Daya (Subresource Integrity)?", "id": "Browser, Verifikasi File (Hash Kriptografi)." },
  { "en": "Apa Itu HTTP (Hypertext Transfer Protocol) Keep-Alive?", "id": "Mempertahankan Koneksi TCP, Banyak Permintaan." },
  { "en": "Apa Itu HSTS (HTTP Strict Transport Security)?", "id": "Memaksa Browser, Komunikasi Hanya HTTPS." },
  { "en": "Apa Itu Inter-Process Communication (IPC)?", "id": "Mekanisme Komunikasi, Antara Proses Berbeda." },
  { "en": "Apa Itu Pipa (Pipe) IPC?", "id": "Saluran Komunikasi, Searah (Antar Proses Terkait)." },
  { "en": "Apa Itu Pipa Bernama (Named Pipe) IPC?", "id": "Pipa (Pipe), Antar Proses Tidak Terkait." },
  { "en": "Apa Itu Memori Berbagi (Shared Memory) IPC?", "id": "Blok Memori, Dibagikan Antara Proses." },
  { "en": "Apa Itu Pengiriman Pesan (Message Passing) IPC?", "id": "Proses Berkomunikasi, Mengirim, Menerima Pesan." },
  { "en": "Apa Itu Mutabilitas (Mutability)?", "id": "Kemampuan Objek, Diubah Setelah Dibuat." },
  { "en": "Apa Itu Imutabilitas (Immutability)?", "id": "Ketidakmampuan Objek, Diubah Setelah Dibuat." },
  { "en": "Apa Itu Pass By Value (Loloskan Nilai)?", "id": "Menyalin Nilai Variabel, Ke Parameter Fungsi." },
  { "en": "Apa Itu Pass By Reference (Loloskan Referensi)?", "id": "Menyalin Alamat Memori, Ke Parameter Fungsi." },
  { "en": "Apa Itu Efek Samping (Side Effect) Fungsi?", "id": "Fungsi, Mengubah Keadaan, Diluar Lingkupnya." },
  { "en": "Apa Itu Fungsi Murni (Pure Function)?", "id": "Fungsi, Tanpa Efek Samping, Output Deterministik." },
  { "en": "Apa Itu Cakupan Kode (Code Coverage)?", "id": "Metrik, Persentase Kode, Dieksekusi Pengujian." },
  { "en": "Apa Itu Cakupan Pernyataan (Statement Coverage)?", "id": "Metrik, Persentase Pernyataan, Dieksekusi Tes." },
  { "en": "Apa Itu Cakupan Cabang (Branch Coverage)?", "id": "Metrik, Persentase Cabang Kondisional, Dieksekusi." },
  { "en": "Apa Itu Cakupan Fungsi (Function Coverage)?", "id": "Metrik, Persentase Fungsi, Yang Dipanggil Tes." },
  { "en": "Apa Perintah 'history' Linux?", "id": "Menampilkan Daftar Perintah, Yang Digunakan." },
  { "en": "Apa Perintah 'jobs' Linux?", "id": "Menampilkan Proses Latar Belakang (Background)." },
  { "en": "Apa Perintah 'fg' (Foreground) Linux?", "id": "Memindahkan Proses, Ke Latar Depan (Foreground)." },
  { "en": "Apa Perintah 'bg' (Background) Linux?", "id": "Melanjutkan Proses, Di Latar Belakang (Background)." },
  { "en": "Apa Perintah 'nohup' (No Hangup) Linux?", "id": "Menjalankan Perintah, Terus Berjalan (Setelah Logout)." },
  { "en": "Apa Perintah 'lsof' (List Open Files)?", "id": "Menampilkan Daftar File, Yang Dibuka Proses." },
  { "en": "Apa Perintah 'netcat' (nc) Linux?", "id": "Utilitas Jaringan, Membaca, Menulis Koneksi Jaringan." },
  { "en": "Apa Perintah 'rsync' (Remote Sync) Linux?", "id": "Utilitas Sinkronisasi File, Jarak Jauh (Remote)." },
  { "en": "Apa Perintah 'journalctl' Linux?", "id": "Melihat Log, Dari Systemd Journal." },
  { "en": "Apa Perintah 'ssh-copy-id'?", "id": "Menyalin Kunci Publik SSH, Ke Server." },
  { "en": "Apa Perintah Git 'git bisect'?", "id": "Mencari Commit, Penyebab Bug (Pencarian Biner)." },
  { "en": "Apa Itu Git Submodules (Submodul Git)?", "id": "Repositori Git, Di Dalam Repositori Git Lain." },
  { "en": "Apa Itu Git Hooks (Kait Git)?", "id": "Skrip Kustom, Dijalankan Otomatis, Event Git." },
  { "en": "Apa Itu Socket (Soket) CPU?", "id": "Konektor Fisik, Motherboard, Tempat CPU." },
  { "en": "Apa Itu Slot DIMM (Dual In-line Memory Module)?", "id": "Slot Motherboard, Memasang Modul RAM." },
  { "en": "Apa Itu Pasta Termal (Thermal Paste)?", "id": "Material Konduktif, Transfer Panas (CPU, Heatsink)." },
  { "en": "Apa Itu Jumper (Pelompat) Motherboard?", "id": "Konektor Kecil, Mengatur Konfigurasi Perangkat Keras." },
  { "en": "Apa Itu Struktur Data Bloom Filter?", "id": "Struktur Data Probabilistik, Cek Keanggotaan Set." },
  { "en": "Apa Itu Struktur Data Skip List?", "id": "Struktur Data Probabilistik, Pencarian Cepat (Log(n))." },
  { "en": "Apa Itu Struktur Data Quadtree?", "id": "Struktur Pohon, Mempartisi Ruang Dua Dimensi." },
  { "en": "Apa Itu Struktur Data Octree?", "id": "Struktur Pohon, Mempartisi Ruang Tiga Dimensi." },
  { "en": "Apa Itu Struktur Data Disjoint Set Union (DSU)?", "id": "Struktur Data, Melacak Set, Elemen Terpisah." },
  { "en": "Apa Itu Algoritma Pengkodean Huffman (Huffman Coding)?", "id": "Algoritma Kompresi Data, Lossless (Prefix Code)." },
  { "en": "Apa Itu Algoritma Kompresi LZW (Lempel-Ziv-Welch)?", "id": "Algoritma Kompresi Data, Lossless (Kamus)." },
  { "en": "Apa Itu Algoritma Rabin-Karp?", "id": "Algoritma Pencarian String, Menggunakan Hashing." },
  { "en": "Apa Itu Algoritma Knuth-Morris-Pratt (KMP)?", "id": "Algoritma Pencarian String, Efisien (Hindari Perbandingan)." },
  { "en": "Apa Itu Enkripsi Simetris (Symmetric Encryption)?", "id": "Satu Kunci Rahasia, Enkripsi, Dekripsi." },
  { "en": "Apa Itu Enkripsi Asimetris (Asymmetric Encryption)?", "id": "Dua Kunci (Publik, Privat), Enkripsi, Dekripsi." },
  { "en": "Apa Itu Pertukaran Kunci (Key Exchange) Diffie-Hellman?", "id": "Metode Kriptografi, Berbagi Kunci Rahasia (Publik)." },
  { "en": "Apa Itu Jabat Tangan (Handshake) SSL/TLS?", "id": "Proses Negosiasi, Koneksi Aman (HTTPS)." },
  { "en": "Apa Itu Kriptografi Homomorfik (Homomorphic Encryption)?", "id": "Enkripsi, Memungkinkan Komputasi, Data Terenkripsi." },
  { "en": "Apa Itu Pembelajaran Terfederasi (Federated Learning)?", "id": "Melatih Model ML, Data Terdesentralisasi (Privasi)." },
  { "en": "Apa Itu Indeks Berkerumun (Clustered Index)?", "id": "Menentukan Urutan Fisik, Data Tabel (Satu)." },
  { "en": "Apa Itu Indeks Tidak Berkerumun (Non-Clustered Index)?", "id": "Struktur Terpisah, Menunjuk Ke Data Tabel (Banyak)." },
  { "en": "Apa Perbedaan Partisi (Partitioning) Basis Data?", "id": "Membagi Tabel Besar, Unit Lebih Kecil (Satu DB)." },
  { "en": "Apa Perbedaan Sharding (Belah) Basis Data?", "id": "Partisi Horizontal, Data, Antar Server Database." },
  { "en": "Apa Itu Cloud Native?", "id": "Pendekatan Membangun, Menjalankan Aplikasi (Skala Awan)." },
  { "en": "Apa Itu Penemuan Layanan (Service Discovery)?", "id": "Otomatis Mendeteksi, Lokasi Layanan (Microservice)." },
  { "en": "Apa Itu Consul?", "id": "Alat Jaringan, Penemuan Layanan, Konfigurasi." },
  { "en": "Apa Itu ETCD?", "id": "Penyimpanan Key-Value, Terdistribusi (Konfigurasi Kubernetes)." },
  { "en": "Apa Itu Format Data YAML (YAML Ain't Markup Language)?", "id": "Format Serialisasi Data, Mudah Dibaca Manusia." },
  { "en": "Apa Itu Format Data TOML (Tom's Obvious, Minimal)?", "id": "Format File Konfigurasi, Minimalis (Jelas)." },
  { "en": "Apa Itu Server X (X Server) Linux?", "id": "Sistem Jendela X, (Dasar GUI Linux)." },
  { "en": "Apa Itu Wayland?", "id": "Protokol Server Tampilan, Pengganti X11 Modern." },
  { "en": "Apa Itu Lingkungan Desktop (Desktop Environment) Linux?", "id": "Kumpulan Komponen, Antarmuka Grafis (GNOME, KDE)." },
  { "en": "Apa Itu Manajer Jendela (Window Manager) Linux?", "id": "Perangkat Lunak, Mengontrol Penempatan, Tampilan Jendela." },
  { "en": "Apa Itu Distro (Distribusi) Linux?", "id": "Sistem Operasi, Berbasis Kernel Linux (Ubuntu)." },
  { "en": "Apa Itu Manajer Paket (Package Manager) Linux?", "id": "Alat, Otomatisasi Instalasi, Pembaruan Perangkat Lunak." },
  { "en": "Apa Itu APT (Advanced Package Tool)?", "id": "Manajer Paket, Berbasis Debian (Ubuntu)." },
  { "en": "Apa Itu YUM (Yellowdog Updater Modified)?", "id": "Manajer Paket, Berbasis RPM (CentOS, RHEL)." },
  { "en": "Apa Itu DNF (Dandified YUM)?", "id": "Manajer Paket, Generasi Berikutnya (Pengganti YUM)." },
  { "en": "Apa Itu Repositori (Repository) Paket?", "id": "Lokasi Penyimpanan, Paket Perangkat Lunak (Server)." },
  { "en": "Apa Itu PPA (Personal Package Archive)?", "id": "Repositori Perangkat Lunak, Khusus Ubuntu (Pihak Ketiga)." },
  { "en": "Apa Itu FHS (Filesystem Hierarchy Standard)?", "id": "Standar Struktur Direktori, Sistem Operasi Unix." },
  { "en": "Apa Direktori '/bin' Linux?", "id": "Berisi Biner Esensial, (Perintah Dasar Pengguna)." },
  { "en": "Apa Direktori '/sbin' Linux?", "id": "Berisi Biner Esensial, (Perintah Administrator Sistem)." },
  { "en": "Apa Direktori '/etc' Linux?", "id": "Berisi File Konfigurasi, Seluruh Sistem." },
  { "en": "Apa Direktori '/dev' Linux?", "id": "Berisi File Perangkat (Devices), (Hardware)." },
  { "en": "Apa Direktori '/proc' Linux?", "id": "Sistem Berkas Virtual, Informasi Kernel, Proses." },
  { "en": "Apa Direktori '/var' Linux?", "id": "Berisi File Data Variabel (Log, Cache)." },
  { "en": "Apa Direktori '/tmp' Linux?", "id": "Berisi File Sementara (Temporary), (Dihapus Reboot)." },
  { "en": "Apa Direktori '/usr' Linux?", "id": "Berisi Program, Pustaka (Shareable, Read-Only)." },
  { "en": "Apa Direktori '/home' Linux?", "id": "Berisi Direktori Pribadi, Pengguna Sistem." },
  { "en": "Apa Direktori '/boot' Linux?", "id": "Berisi File, Dibutuhkan Proses Booting (Kernel)." },
  { "en": "Apa Direktori '/lib' Linux?", "id": "Berisi Pustaka Esensial, Biner (/bin, /sbin)." },
  { "en": "Apa Direktori '/opt' Linux?", "id": "Berisi Paket Perangkat Lunak, Tambahan (Opsional)." },
  { "en": "Apa Direktori '/mnt' Linux?", "id": "Titik Kait (Mount Point) Temporer, Filesystem." },
  { "en": "Apa Direktori '/media' Linux?", "id": "Titik Kait (Mount Point) Media Removable." },
  { "en": "Apa Direktori Root ('/')?", "id": "Direktori Tingkat Tertinggi, Hirarki Filesystem." },
  { "en": "Apa Itu GRUB (Grand Unified Bootloader)?", "id": "Bootloader Populer, Sistem Operasi Linux." },
  { "en": "Apa Itu Init (Inisialisasi) Sistem?", "id": "Proses Pertama, Dimulai Kernel Saat Boot." },
  { "en": "Apa Itu Systemd?", "id": "Manajer Sistem, Layanan Modern Linux (Pengganti Init)." },
  { "en": "Apa Itu Runlevel (Tingkat Proses) Linux?", "id": "Mode Operasi, Sistem Init V (Lama)." },
  { "en": "Apa Itu Target Systemd?", "id": "Pengganti Runlevel, Systemd (Unit Konfigurasi)." },
  { "en": "Apa Itu Daemon (Setan) Linux?", "id": "Program, Berjalan Latar Belakang (Layanan Sistem)." },
  { "en": "Apa Itu SELinux (Security-Enhanced Linux)?", "id": "Modul Keamanan Kernel, Kontrol Akses (MAC)." },
  { "en": "Apa Itu AppArmor?", "id": "Modul Keamanan Kernel, Pembatasan Program (Profil)." },
  { "en": "Apa Itu Iptables?", "id": "Alat Firewall, Baris Perintah Linux (Netfilter)." },
  { "en": "Apa Itu UFW (Uncomplicated Firewall)?", "id": "Antarmuka Firewall, Ramah Pengguna (Manajemen Iptables)." },
  { "en": "Apa Itu Fail2Ban?", "id": "Perangkat Lunak, Pencegahan Intrusi (Memblokir IP)." },
  { "en": "Apa Itu Pemformatan (Formatting) Disk?", "id": "Menyiapkan Media Penyimpanan, Untuk Digunakan." },
  { "en": "Apa Itu Partisi (Partition) Disk?", "id": "Pembagian Logis, Perangkat Penyimpanan Fisik." },
  { "en": "Apa Itu MBR (Master Boot Record)?", "id": "Sektor Boot, Partisi Disk (Metode Lama)." },
  { "en": "Apa Itu GPT (GUID Partition Table)?", "id": "Standar Modern, Tata Letak Partisi Disk." },
  { "en": "Apa Itu Bootloader (Pemuat Boot)?", "id": "Program, Memuat Sistem Operasi, Saat Boot." },
  { "en": "Apa Itu LILO (Linux Loader)?", "id": "Bootloader Linux (Lebih Tua, Digantikan GRUB)." },
  { "en": "Apa Itu Logical Volume Manager (LVM) Linux?", "id": "Manajemen Volume Disk, Fleksibel (Logis)." },
  { "en": "Apa Itu Volume Fisik (Physical Volume) LVM?", "id": "Partisi Disk Fisik, Digunakan Oleh LVM." },
  { "en": "Apa Itu Grup Volume (Volume Group) LVM?", "id": "Kumpulan Volume Fisik, (Satu Unit Penyimpanan)." },
  { "en": "Apa Itu Volume Logis (Logical Volume) LVM?", "id": "Partisi Logis, Dibuat Dari Grup Volume." },
  { "en": "Apa Itu Swap Space (Ruang Swap) Linux?", "id": "Area Disk, Digunakan Sebagai Memori Virtual." },
  { "en": "Apa Itu Swappiness (Kecenderungan Swap) Linux?", "id": "Parameter Kernel, Mengatur Penggunaan Ruang Swap." },
  { "en": "Apa Itu File '/etc/fstab' Linux?", "id": "File Konfigurasi, Definisi Filesystem (Mount Otomatis)." },
  { "en": "Apa Perintah 'mount' Linux?", "id": "Perintah Mengaitkan Filesystem, Ke Direktori (Mount Point)." },
  { "en": "Apa Perintah 'umount' Linux?", "id": "Perintah Melepas Kaitan (Unmount) Filesystem." },
  { "en": "Apa Itu User Interface (UI) Grafis?", "id": "Antarmuka Pengguna, Menggunakan Elemen Grafis (Ikon)." },
  { "en": "Apa Itu User Interface (UI) Teks?", "id": "Antarmuka Pengguna, Berbasis Perintah Teks (CLI)." },
  { "en": "Apa Itu Shell (Cangkang) OS?", "id": "Program Antarmuka, Pengguna Ke Kernel OS." },
  { "en": "Apa Itu Bash (Bourne Again Shell)?", "id": "Shell Baris Perintah, Standar Populer Linux." },
  { "en": "Apa Itu Zsh (Z Shell)?", "id": "Shell Baris Perintah, Fitur Tambahan (Ekstensi Bash)." },
  { "en": "Apa Itu PowerShell?", "id": "Shell Baris Perintah, Windows (Berbasis .NET)." },
  { "en": "Apa Itu Paging (Paging) Permintaan OS?", "id": "Teknik Memori Virtual, Memuat Halaman (Page) Sesuai Kebutuhan." },
  { "en": "Apa Itu Working Set (Set Kerja) Proses?", "id": "Kumpulan Halaman (Page), Aktif Digunakan Proses." },
  { "en": "Apa Itu Algoritma Penggantian Halaman (Page Replacement)?", "id": "Algoritma OS, Memilih Halaman, Dikeluarkan (Swap)." },
  { "en": "Apa Itu Algoritma Penggantian Halaman (Page) FIFO?", "id": "Mengganti Halaman, Yang Paling Lama Masuk." },
  { "en": "Apa Itu Algoritma Penggantian Halaman (Page) LRU?", "id": "Mengganti Halaman, Paling Lama Tidak Digunakan." },
  { "en": "Apa Itu Algoritma Penggantian Halaman (Page) Optimal?", "id": "Mengganti Halaman, Paling Lama Digunakan (Teoritis)." },
  { "en": "Apa Itu Belady's Anomaly (Anomali Belady)?", "id": "Anomali, Menambah Frame, Meningkatkan Page Fault (FIFO)." },
  { "en": "Apa Itu API (Application Programming Interface) DOM?", "id": "API, Mengakses, Memanipulasi Dokumen HTML (Struktur Pohon)." },
  { "en": "Apa Itu Peristiwa (Event) 'click' DOM?", "id": "Peristiwa, Terjadi Saat Pengguna, Mengklik Elemen." },
  { "en": "Apa Itu Peristiwa (Event) 'load' DOM?", "id": "Peristiwa, Terjadi Saat Halaman, Selesai Dimuat." },
  { "en": "Apa Itu Peristiwa (Event) 'submit' DOM?", "id": "Peristiwa, Terjadi Saat Formulir (Form) Dikirim." },
  { "en": "Apa Itu Metode 'getElementById' DOM?", "id": "Metode DOM, Memilih Elemen, Berdasarkan ID." },
  { "en": "Apa Itu Metode 'querySelector' DOM?", "id": "Metode DOM, Memilih Elemen, Selektor CSS Pertama." },
  { "en": "Apa Itu Metode 'querySelectorAll' DOM?", "id": "Metode DOM, Memilih Semua Elemen, Selektor CSS." },
  { "en": "Apa Itu Metode 'addEventListener' DOM?", "id": "Metode DOM, Menambahkan Penangan Peristiwa (Event Handler)." },
  { "en": "Apa Itu Properti 'innerHTML' DOM?", "id": "Properti, Mendapatkan, Mengatur Konten HTML Elemen." },
  { "en": "Apa Itu Properti 'textContent' DOM?", "id": "Properti, Mendapatkan, Mengatur Konten Teks Elemen." },
  { "en": "Apa Itu Properti 'style' DOM?", "id": "Properti, Mengakses, Mengatur Gaya CSS Elemen." },
  { "en": "Apa Itu Server HTTP (Hypertext Transfer Protocol) Apache?", "id": "Server Web, Open-Source, Populer (Lama)." },
  { "en": "Apa Itu Nginx (Engine-X)?", "id": "Server Web, Kinerja Tinggi, Reverse Proxy." },
  { "en": "Apa Itu File '.htaccess' Apache?", "id": "File Konfigurasi, Apache (Per Direktori)." },
  { "en": "Apa Itu Virtual Hosting?", "id": "Menjalankan Banyak Situs Web, Satu Server." },
  { "en": "Apa Itu Komputasi Klien-Server (Client-Server)?", "id": "Model Arsitektur, Klien Meminta, Server Melayani." },
  { "en": "Apa Itu Peer-to-Peer (P2P) Computing?", "id": "Model Jaringan, Setiap Peer, Setara (Klien/Server)." },
  { "en": "Apa Itu BitTorrent?", "id": "Protokol Komunikasi, Peer-to-Peer (Transfer File)." },
  { "en": "Apa Itu Seeder (Penyemai) BitTorrent?", "id": "Peer, Memiliki File Lengkap, Membagikan (Upload)." },
  { "en": "Apa Itu Leecher (Lintah) BitTorrent?", "id": "Peer, Mengunduh File, Belum Lengkap." },
  { "en": "Apa Itu Tracker (Pelacak) BitTorrent?", "id": "Server Khusus, Mengkoordinasi Peer (Swarm)." },
  { "en": "Apa Itu Magnet Link (Tautan Magnet)?", "id": "Standar, Tautan Referensi, Sumber Daya (P2P)." },
  { "en": "Apa Itu FUP (Fair Usage Policy)?", "id": "Kebijakan Penggunaan Wajar (Batasan ISP)." },
  { "en": "Apa Itu IP (Internet Protocol) Statis?", "id": "Alamat IP, Ditetapkan Manual, Tetap (Server)." },
  { "en": "Apa Itu IP (Internet Protocol) Dinamis?", "id": "Alamat IP, Diberikan Otomatis, DHCP (Klien)." },
  { "en": "Apa Itu Kelas (Class) Jaringan IP?", "id": "Sistem Klasifikasi, Alamat IP (Lama, A, B, C)." },
  { "en": "Apa Itu Collision Domain (Domain Tabrakan)?", "id": "Segmen Jaringan, Tabrakan Paket, Terjadi (Hub)." },
  { "en": "Apa Itu Broadcast Domain (Domain Siaran)?", "id": "Segmen Jaringan, Pesan Broadcast, Terkirim (Switch)." },
  { "en": "Bagaimana Switch Mengurangi Collision Domain?", "id": "Setiap Port Switch, Domain Tabrakan Terpisah." },
  { "en": "Bagaimana Router Memisahkan Broadcast Domain?", "id": "Router Tidak Meneruskan, Pesan Broadcast (Default)." },
  { "en": "Apa Itu Routing (Perutean) Statis?", "id": "Administrator, Konfigurasi Rute Jaringan, Manual." },
  { "en": "Apa Itu Routing (Perutean) Dinamis?", "id": "Router, Otomatis Mempelajari Rute, (Protokol Routing)." },
  { "en": "Apa Itu Tabel Routing (Routing Table)?", "id": "Tabel Data, Router, Menyimpan Jalur (Rute)." },
  { "en": "Apa Itu Rute Default (Default Route)?", "id": "Rute Jaringan, Digunakan, Tujuan Tidak Dikenal." },
  { "en": "Apa Itu Metrik (Metric) Routing?", "id": "Nilai, Digunakan Protokol, Menentukan Rute Terbaik." },
  { "en": "Apa Itu Hop (Lompatan) Jaringan?", "id": "Jalur Paket, Melewati Satu Router." },
  { "en": "Apa Itu CSMA/CD (Carrier Sense Multiple Access)?", "id": "Metode Kontrol Akses, Ethernet (Deteksi Tabrakan)." },
  { "en": "Apa Itu CSMA/CA (Carrier Sense Multiple Access)?", "id": "Metode Kontrol Akses, Wi-Fi (Penghindaran Tabrakan)." },
  { "en": "Apa Itu SSID (Service Set Identifier)?", "id": "Nama Unik, Jaringan Nirkabel (WLAN)." },
  { "en": "Apa Itu BSSID (Basic Service Set Identifier)?", "id": "Alamat MAC, Titik Akses (AP) Nirkabel." },
  { "en": "Apa Itu WEP (Wired Equivalent Privacy)?", "id": "Protokol Keamanan Wi-Fi (Lama, Tidak Aman)." },
  { "en": "Apa Itu WPA (Wi-Fi Protected Access)?", "id": "Protokol Keamanan Wi-Fi (Pengganti WEP)." },
  { "en": "Apa Itu WPA2 (Wi-Fi Protected Access 2)?", "id": "Standar Keamanan Wi-Fi, (Menggunakan AES)." },
  { "en": "Apa Itu WPA3 (Wi-Fi Protected Access 3)?", "id": "Standar Keamanan Wi-Fi, Terbaru (Lebih Kuat)." },
  { "en": "Apa Itu TKIP (Temporal Key Integrity Protocol)?", "id": "Protokol Enkripsi, WPA (Pengganti WEP)." },
  { "en": "Apa Itu AES (Advanced Encryption Standard) Kriptografi?", "id": "Standar Enkripsi Simetris, (Digunakan WPA2)." },
  { "en": "Apa Itu WPS (Wi-Fi Protected Setup)?", "id": "Standar Koneksi, Perangkat Nirkabel Mudah (Tombol)." },
  { "en": "Apa Itu Mode Ad-Hoc Nirkabel?", "id": "Jaringan Nirkabel, Peer-to-Peer (Tanpa AP)." },
  { "en": "Apa Itu Mode Infrastruktur Nirkabel?", "id": "Jaringan Nirkabel, Menggunakan Titik Akses (AP)." },
  { "en": "Apa Itu IEEE 802.11a?", "id": "Standar Wi-Fi, (Band 5 GHz, 54 Mbps)." },
  { "en": "Apa Itu IEEE 802.11b?", "id": "Standar Wi-Fi, (Band 2.4 GHz, 11 Mbps)." },
  { "en": "Apa Itu IEEE 802.11g?", "id": "Standar Wi-Fi, (Band 2.4 GHz, 54 Mbps)." },
  { "en": "Apa Itu IEEE 802.11n (Wi-Fi 4)?", "id": "Standar Wi-Fi, (Band 2.4/5 GHz, MIMO)." },
  { "en": "Apa Itu IEEE 802.11ac (Wi-Fi 5)?", "id": "Standar Wi-Fi, (Band 5 GHz, Kinerja Tinggi)." },
  { "en": "Apa Itu IEEE 802.11ax (Wi-Fi 6)?", "id": "Standar Wi-Fi, (Efisiensi Tinggi, OFDMA)." },
  { "en": "Apa Itu MIMO (Multiple-Input Multiple-Output)?", "id": "Teknologi Nirkabel, Banyak Antena (Mempercepat)." },
  { "en": "Apa Itu OFDMA (Orthogonal Frequency-Division Multiple Access)?", "id": "Teknologi Wi-Fi 6, Efisiensi Spektrum." },
  { "en": "Apa Itu Firewall (Tembok Api) UTM?", "id": "Manajemen Ancaman Terpadu (Firewall, IPS, Antivirus)." },
  { "en": "Apa Itu Zero Trust (Nol Kepercayaan) Keamanan?", "id": "Model Keamanan, Verifikasi Ketat (Jangan Percaya)." },
  { "en": "Apa Itu Serangan Evil Twin (Kembar Jahat)?", "id": "Titik Akses (AP) Wi-Fi Palsu, Menipu Pengguna." },
  { "en": "Apa Itu Wardriving (Jalan Perang)?", "id": "Mencari Jaringan Wi-Fi, (Dari Kendaraan Bergerak)." },
  { "en": "Apa Itu Skimming (Penyalinan) ATM?", "id": "Pencurian Data Kartu, Perangkat Palsu ATM." },
  { "en": "Apa Itu Vishing (Voice Phishing)?", "id": "Phishing, Menggunakan Panggilan Suara (Telepon)." },
  { "en": "Apa Itu Smishing (SMS Phishing)?", "id": "Phishing, Menggunakan Pesan Teks (SMS)." },
  { "en": "Apa Itu Pharming (Pengelabuan DNS)?", "id": "Mengalihkan Trafik Situs Web, Situs Palsu." },
  { "en": "Apa Itu Baiting (Umpan) Serangan?", "id": "Rekayasa Sosial, Menawarkan Sesuatu (Flash Drive)." },
  { "en": "Apa Itu Pretexting (Dalih) Serangan?", "id": "Rekayasa Sosial, Membuat Skenario Palsu (Dalih)." },
  { "en": "Apa Itu Quid Pro Quo (Sesuatu Untuk Sesuatu)?", "id": "Rekayasa Sosial, Menawarkan Bantuan (Palsu)." },
  { "en": "Apa Itu Tailgating (Membuntuti) Serangan Fisik?", "id": "Mengikuti Orang, Akses Area Terbatas (Fisik)." },
  { "en": "Apa Itu Dumpster Diving (Mencari Sampah)?", "id": "Mencari Informasi Sensitif, Tempat Sampah." },
  { "en": "Apa Kerugian Model (Model) Prototyping?", "id": "Proses Sulit Diatur, Boros Sumber Daya." },
  { "en": "Apa Model SDLC (Software Development Life Cycle) Incremental?", "id": "Pengembangan Bertahap, Fungsionalitas Meningkat." },
  { "en": "Apa Itu Velocity (Kecepatan) Tim Agile?", "id": "Ukuran Jumlah Pekerjaan, Selesai Per Sprint." },
  { "en": "Apa Standar ISO (International Organization for Standardization) 9001?", "id": "Standar Internasional, Sistem Manajemen Mutu." },
  { "en": "Apa Itu CMMI (Capability Maturity Model Integration)?", "id": "Model Peningkatan, Proses Pengembangan Produk." },
  { "en": "Apa Tingkat Kematangan (Maturity Level) CMMI?", "id": "Lima Tingkat, Mengukur Kematangan Proses." },
  { "en": "Apa Kualitas Internal (Internal Quality) Perangkat Lunak?", "id": "Kualitas Kode, Struktur (Perspektif Pengembang)." },
  { "en": "Apa Kualitas Eksternal (External Quality) Perangkat Lunak?", "id": "Kualitas Fungsionalitas, Kinerja (Perspektif Pengguna)." },
  { "en": "Apa Kualitas Dalam Penggunaan (Quality In Use)?", "id": "Kualitas Efektivitas, Kepuasan Pengguna Nyata." },
  { "en": "Apa Perintah 'diff' (Difference) Linux?", "id": "Membandingkan, Menampilkan Perbedaan Antara Dua File." },
  { "en": "Apa Perintah 'sort' (Urutkan) Linux?", "id": "Mengurutkan Baris, File Teks (Alfabetis, Numerik)." },
  { "en": "Apa Perintah 'uniq' (Unique) Linux?", "id": "Menghapus, Melaporkan Baris Berdampingan Yang Sama." },
  { "en": "Apa Perintah 'wc' (Word Count) Linux?", "id": "Menghitung Jumlah Baris, Kata, Byte File." },
  { "en": "Apa Perintah 'free' (Bebas) Linux?", "id": "Menampilkan Informasi, Penggunaan Memori (RAM, Swap)." },
  { "en": "Apa Perintah 'dmesg' (Display Message) Linux?", "id": "Menampilkan Pesan, Kernel (Informasi Boot, Hardware)." },
  { "en": "Apa Perintah 'useradd' (User Add) Linux?", "id": "Membuat Akun Pengguna (User) Baru Sistem." },
  { "en": "Apa Perintah 'passwd' (Password) Linux?", "id": "Mengubah Kata Sandi (Password) Akun Pengguna." },
  { "en": "Apa Perintah 'groupadd' (Group Add) Linux?", "id": "Membuat Grup (Group) Pengguna Baru." },
  { "en": "Apa Redireksi (Redirection) Input (<)?", "id": "Mengambil Input Perintah, Dari File." },
  { "en": "Apa HERE Document (<<) Shell?", "id": "Redireksi Input, Teks Langsung (Delimiter)." },
  { "en": "Apa Wildcard '*' (Bintang) Shell?", "id": "Mencocokkan Nol, Atau Lebih Karakter Apapun." },
  { "en": "Apa Wildcard '?' (Tanya) Shell?", "id": "Mencocokkan Tepat Satu Karakter Apapun." },
  { "en": "Apa Kurung Kurawal ({}) Ekspansi Shell?", "id": "Menghasilkan Kombinasi String (Contoh: a{b,c}d)." },
  { "en": "Apa Perintah 'ln' (Link) Linux?", "id": "Membuat Tautan (Link), Antara File (Hard, Symbolic)." },
  { "en": "Apa Itu Superblok (Superblock) Filesystem?", "id": "Metadata Krusial, Menyimpan Informasi Filesystem." },
  { "en": "Apa Filesystem Ext3 (Third Extended Filesystem)?", "id": "Evolusi Ext2, Menambahkan Fitur Jurnaling." },
  { "en": "Apa Filesystem XFS (Extent File System)?", "id": "Filesystem 64-bit, Kinerja Tinggi (Jurnaling)." },
  { "en": "Apa Itu Aturan Asosiasi (Association Rule)?", "id": "Metode, Menemukan Hubungan Item (Data Mining)." },
  { "en": "Apa Support (Dukungan) Aturan Asosiasi?", "id": "Frekuensi Kemunculan, Itemset Dalam Transaksi." },
  { "en": "Apa Confidence (Kepercayaan) Aturan Asosiasi?", "id": "Ukuran Probabilitas, (Jika A Maka B)." },
  { "en": "Apa Lift (Peningkatan) Aturan Asosiasi?", "id": "Kekuatan Hubungan, Antara Item (Independen)." },
  { "en": "Apa Algoritma Apriori?", "id": "Algoritma Klasik, Penambangan Aturan Asosiasi." },
  { "en": "Apa Itu Corpus (Korpus) Teks?", "id": "Kumpulan Dokumen Teks, (Data Latih NLP)." },
  { "en": "Apa Itu Stemming (Pangkal Kata) NLP?", "id": "Proses Menghapus Imbuhan, (Menjadi Kata Dasar)." },
  { "en": "Apa Itu Lemmatization (Lematisasi) NLP?", "id": "Proses Konversi Kata, Bentuk Dasar (Kamus)." },
  { "en": "Apa Itu Model Bag-of-Words (BoW)?", "id": "Representasi Teks, Menghitung Frekuensi Kata (Urutan Diabaikan)." },
  { "en": "Apa Itu TF-IDF (Term Frequency-Inverse Document Frequency)?", "id": "Pembobotan Kata, Mengukur Kepentingan Kata Teks." },
  { "en": "Apa Itu Word Embedding (Penyematan Kata)?", "id": "Representasi Vektor Kata, Menangkap Makna Semantik." },
  { "en": "Apa Itu Model Word2Vec?", "id": "Model, Menghasilkan Word Embedding (Vektor Kata)." },
  { "en": "Apa Itu Named Entity Recognition (NER)?", "id": "Ekstraksi Informasi, Entitas Bernama (Orang, Lokasi)." },
  { "en": "Apa Itu Part-of-Speech (POS) Tagging?", "id": "Memberi Label Kata, Berdasarkan Kelas Kata (Verba)." },
  { "en": "Apa Itu Thresholding (Ambang Batas) Gambar?", "id": "Mengubah Gambar Grayscale, Menjadi Biner (Hitam Putih)." },
  { "en": "Apa Itu Deteksi Tepi (Edge Detection)?", "id": "Teknik, Menemukan Batas Objek, Dalam Gambar." },
  { "en": "Apa Itu Konvolusi (Convolution) Gambar?", "id": "Operasi Matematis, Menerapkan Filter (Kernel) Gambar." },
  { "en": "Apa Itu Kernel (Filter) Konvolusi?", "id": "Matriks Kecil, Ekstraksi Fitur (Blur, Sharpen)." },
  { "en": "Apa Itu Pooling (Penyatuan) Layer CNN?", "id": "Lapisan CNN, Mengurangi Dimensi, Peta Fitur." },
  { "en": "Apa Itu Max Pooling (Penyatuan Maksimum)?", "id": "Pooling, Mengambil Nilai Maksimum, Area Tertentu." },
  { "en": "Apa Itu Average Pooling (Penyatuan Rata-Rata)?", "id": "Pooling, Mengambil Nilai Rata-Rata, Area Tertentu." },
  { "en": "Apa Itu Data Augmentation (Augmentasi Data)?", "id": "Teknik, Menambah Variasi Data Latih (Rotasi, Flip)." },
  { "en": "Apa Itu AI (Artificial Intelligence) Generatif?", "id": "Model AI, Mampu Menghasilkan Konten Baru (Teks, Gambar)." },
  { "en": "Apa Itu Model Diskriminatif (Discriminative Model)?", "id": "Model, Mempelajari Batas, Keputusan Antar Kelas." },
  { "en": "Apa Itu Zero-Shot Learning (Pembelajaran Nol-Tembakan)?", "id": "Model, Melakukan Tugas, Tanpa Data Latih Spesifik." },
  { "en": "Apa Itu Few-Shot Learning (Pembelajaran Sedikit-Tembakan)?", "id": "Model, Belajar, Sedikit Contoh Data Latih." },
  { "en": "Apa Itu Paten (Patent) Perangkat Lunak?", "id": "Hak Eksklusif, Diberikan Untuk Inovasi (Algoritma)." },
  { "en": "Apa Itu Merek Dagang (Trademark)?", "id": "Simbol, Nama, Melindungi Identitas Merek (Logo)." },
  { "en": "Apa Itu Rahasia Dagang (Trade Secret)?", "id": "Informasi Rahasia Bisnis, Memberi Keuntungan Kompetitif." },
  { "en": "Apa Itu DMCA (Digital Millennium Copyright Act)?", "id": "Hukum Hak Cipta, Amerika Serikat (Digital)." },
  { "en": "Apa Itu Software Audit (Audit Perangkat Lunak)?", "id": "Tinjauan Resmi, Penggunaan, Lisensi Perangkat Lunak." },
  { "en": "Apa Itu Kepatuhan Lisensi (License Compliance)?", "id": "Memastikan Penggunaan Perangkat Lunak, Sesuai Lisensi." },
  { "en": "Apa Itu Tata Kelola TI (IT Governance)?", "id": "Kerangka Kerja, Memastikan TI Mendukung Tujuan Bisnis." },
  { "en": "Apa Itu COBIT (Control Objectives for Information)?", "id": "Kerangka Kerja (Framework), Tata Kelola, Manajemen TI." },
  { "en": "Apa Fungsi Kerangka Kerja (Framework) COBIT?", "id": "Menyelaraskan TI, Dengan Tujuan Bisnis Organisasi." },
  { "en": "Apa Itu Manajemen Risiko TI (IT Risk Management)?", "id": "Proses Identifikasi, Penilaian, Mitigasi Risiko TI." },
  { "en": "Apa Analisis Risiko (Risk Analysis) Kualitatif?", "id": "Penilaian Risiko, Berbasis Subjektif (Tinggi, Rendah)." },
  { "en": "Apa Analisis Risiko (Risk Analysis) Kuantitatif?", "id": "Penilaian Risiko, Berbasis Angka, Moneter (Biaya)." },
  { "en": "Apa Itu ALE (Annual Loss Expectancy)?", "id": "Total Kerugian Finansial, Diharapkan Per Tahun." },
  { "en": "Apa Itu SLE (Single Loss Expectancy)?", "id": "Kerugian Finansial, Diharapkan Dari Satu Insiden." },
  { "en": "Apa Itu AV (Asset Value)?", "id": "Nilai Moneter, Aset Yang Dilindungi." },
  { "en": "Apa Itu EF (Exposure Factor)?", "id": "Persentase Kerugian Aset, Akibat Ancaman." },
  { "en": "Apa Itu ARO (Annualized Rate of Occurrence)?", "id": "Estimasi Frekuensi, Ancaman Terjadi Per Tahun." },
  { "en": "Apa Itu Mitigasi Risiko (Risk Mitigation)?", "id": "Tindakan, Mengurangi Dampak, Probabilitas Risiko." },
  { "en": "Apa Itu Penerimaan Risiko (Risk Acceptance)?", "id": "Keputusan Menerima, Konsekuensi Risiko (Biaya Kontrol)." },
  { "en": "Apa Itu Penghindaran Risiko (Risk Avoidance)?", "id": "Tindakan, Menghentikan Aktivitas, Penyebab Risiko." },
  { "en": "Apa Itu Transfer Risiko (Risk Transfer)?", "id": "Memindahkan Dampak Risiko, Pihak Ketiga (Asuransi)." },
  { "en": "Apa Kontrol Keamanan (Security Control) Preventif?", "id": "Kontrol, Mencegah Insiden Keamanan Terjadi (Firewall)." },
  { "en": "Apa Kontrol Keamanan (Security Control) Detektif?", "id": "Kontrol, Mendeteksi Insiden, Saat Terjadi (IDS)." },
  { "en": "Apa Kontrol Keamanan (Security Control) Korektif?", "id": "Kontrol, Memperbaiki Kerusakan, Pasca Insiden (Backup)." },
  { "en": "Apa Kontrol Keamanan (Security Control) Administratif?", "id": "Kontrol, Kebijakan, Prosedur Keamanan (Pelatihan)." },
  { "en": "Apa Kontrol Keamanan (Security Control) Teknis?", "id": "Kontrol, Menggunakan Teknologi, Perangkat Lunak (Enkripsi)." },
  { "en": "Apa Kontrol Keamanan (Security Control) Fisik?", "id": "Kontrol, Melindungi Aset Fisik (Kunci, CCTV)." },
  { "en": "Apa Itu Model SDLC (Software Development Life Cycle) Spiral?", "id": "Model Evolusioner, Menggabungkan Iterasi, Risiko." },
  { "en": "Apa Tahap Analisis Risiko (Risk Analysis) Spiral?", "id": "Setiap Iterasi Spiral, Mengevaluasi Risiko Proyek." },
  { "en": "Apa Keuntungan Model (Model) Spiral?", "id": "Manajemen Risiko Kuat, Pengembangan Skala Besar." },
  { "en": "Apa Kerugian Model (Model) Spiral?", "id": "Sangat Kompleks, Membutuhkan Keahlian Risiko." },
  { "en": "Apa Model SDLC (Software Development Life Cycle) V-Model?", "id": "Model Sekuensial, Verifikasi, Validasi (Bentuk V)." },
  { "en": "Apa Hubungan (Relationship) Dalam V-Model?", "id": "Setiap Tahap Pengembangan, Terkait Tahap Pengujian." },
  { "en": "Apa Keuntungan Model (Model) V-Model?", "id": "Penekanan Kuat, Pengujian, Verifikasi Dini." },
  { "en": "Apa Kerugian Model (Model) V-Model?", "id": "Tidak Fleksibel, Terhadap Perubahan Kebutuhan." },
  { "en": "Apa Itu RUP (Rational Unified Process)?", "id": "Kerangka Kerja (Framework), Proses Pengembangan Iteratif." },
  { "en": "Apa Empat Fase (Phase) RUP?", "id": "Insepsi, Elaborasi, Konstruksi, Transisi." },
  { "en": "Apa Itu Fase Insepsi (Inception) RUP?", "id": "Fase Awal, Mendefinisikan Visi, Ruang Lingkup Proyek." },
  { "en": "Apa Itu Fase Elaborasi (Elaboration) RUP?", "id": "Fase Perencanaan, Arsitektur, Mitigasi Risiko." },
  { "en": "Apa Itu Fase Konstruksi (Construction) RUP?", "id": "Fase Pembangunan, Pengembangan Produk, Pengujian." },
  { "en": "Apa Itu Fase Transisi (Transition) RUP?", "id": "Fase Rilis Produk, Ke Pengguna (Deployment)." },
  { "en": "Apa Itu Metode Pengembangan (Development) Crystal?", "id": "Keluarga Metodologi Agile, Fokus Manusia, Interaksi." },
  { "en": "Apa Itu DSDM (Dynamic Systems Development Method)?", "id": "Metodologi Agile, Fokus Proyek Bisnis (Waktu Tetap)." },
  { "en": "Apa Itu Feature Driven Development (FDD)?", "id": "Metodologi Agile, Berpusat Pada Pengembangan Fitur." },
  { "en": "Apa Itu Lean Software Development (LSD)?", "id": "Adaptasi Prinsip Lean, (Menghilangkan Pemborosan)." },
  { "en": "Apa Itu Prinsip Lean (Lean Principle) Eliminasi Pemborosan?", "id": "Menghapus Apapun, Tidak Menambah Nilai Produk." },
  { "en": "Apa Itu Prinsip Lean (Lean Principle) Amplifikasi Pembelajaran?", "id": "Meningkatkan Pembelajaran Tim, Melalui Iterasi." },
  { "en": "Apa Prinsip Lean (Lean Principle) Tunda Keputusan?", "id": "Menunda Keputusan, Hingga Saat Terakhir Bertanggung Jawab." },
  { "en": "Apa Prinsip Lean (Lean Principle) Kirim Cepat?", "id": "Mengirimkan Nilai, Secepat Mungkin Ke Pelanggan." },
  { "en": "Apa Prinsip Lean (Lean Principle) Berdayakan Tim?", "id": "Memberi Kepercayaan, Otonomi Tim Pengembang." },
  { "en": "Apa Prinsip Lean (Lean Principle) Bangun Integritas?", "id": "Integritas Sistem, Kualitas Kode, Sejak Awal." },
  { "en": "Apa Prinsip Lean (Lean Principle) Lihat Keseluruhan?", "id": "Mengoptimalkan Alur Kerja, Secara Keseluruhan (End-to-End)." },
  { "en": "Apa Itu Pengujian Usability (Kebergunaan)?", "id": "Pengujian Kemudahan Penggunaan, Aplikasi Oleh Pengguna." },
  { "en": "Apa Itu Heuristik (Heuristic) Nielsen?", "id": "Sepuluh Prinsip Desain, Antarmuka Pengguna (Usability)." },
  { "en": "Heuristik: Visibilitas Status Sistem?", "id": "Pengguna Tahu, Apa Yang Sedang Terjadi." },
  { "en": "Heuristik: Kecocokan Sistem Dunia Nyata?", "id": "Bahasa, Konsep, Familiar Bagi Pengguna." },
  { "en": "Heuristik: Kontrol Kebebasan Pengguna?", "id": "Mudah Membatalkan Aksi (Undo, Redo, Keluar)." },
  { "en": "Heuristik: Konsistensi Standar?", "id": "Konsisten, Konvensi Platform, Internal Aplikasi." },
  { "en": "Heuristik: Pencegahan Kesalahan (Error Prevention)?", "id": "Desain Mencegah, Kesalahan Pengguna Terjadi." },
  { "en": "Heuristik: Pengenalan Daripada Mengingat?", "id": "Mengurangi Beban Memori, (Opsi Terlihat)." },
  { "en": "Heuristik: Fleksibilitas Efisiensi Penggunaan?", "id": "Pintasan (Shortcut), Mempercepat Pengguna Mahir." },
  { "en": "Heuristik: Desain Estetika Minimalis?", "id": "Antarmuka Hanya, Menampilkan Informasi Relevan." },
  { "en": "Heuristik: Bantu Pengguna Kenali Kesalahan?", "id": "Pesan Kesalahan Jelas, Menawarkan Solusi." },
  { "en": "Heuristik: Bantuan Dokumentasi?", "id": "Bantuan, Dokumentasi, Mudah Dicari, Kontekstual." },
  { "en": "Apa Itu Walkthrough Kognitif?", "id": "Metode Evaluasi Usability, Berbasis Tugas Pengguna." },
  { "en": "Apa Itu Pengujian Gorila (Gorilla Testing)?", "id": "Pengujian Modul, Acak, Intensif (Sangat Kuat)." },
  { "en": "Apa Itu Pengujian Ad-Hoc?", "id": "Pengujian Informal, Tanpa Rencana, Kasus Uji." },
  { "en": "Apa Itu Pengujian Eksploratif (Exploratory Testing)?", "id": "Desain, Eksekusi Pengujian, Bersamaan (Belajar)." },
  { "en": "Apa Itu Pengujian Volume (Volume Testing)?", "id": "Pengujian Sistem, Menggunakan Data Volume Besar." },
  { "en": "Apa Itu Pengujian Konfigurasi (Configuration Testing)?", "id": "Pengujian, Berbagai Konfigurasi Hardware, Software." },
  { "en": "Apa Itu Pengujian Interoperabilitas (Interoperability Testing)?", "id": "Pengujian, Kemampuan Interaksi, Antar Sistem Berbeda." },
  { "en": "Apa Itu Pengujian Kehancuran (Destructive Testing)?", "id": "Pengujian, Mencari Titik Kegagalan Sistem (Batas)." },
  { "en": "Apa Itu Spooling (Simultaneous Peripheral Operations Online)?", "id": "Menyimpan Tugas (Job), Antrean Disk (Printer)." },
  { "en": "Apa Itu Buffering (Penyanggaan)?", "id": "Menyimpan Data Sementara, Memori (RAM), Perangkat." },
  { "en": "Perbedaan Spooling Dan Buffering?", "id": "Spooling (Disk, Antrean Job), Buffering (RAM, Aliran Data)." },
  { "en": "Apa Itu Proses Zombie (Zombie Process)?", "id": "Proses Selesai, Entri Tabel Proses Tersisa." },
  { "en": "Apa Itu Proses Yatim (Orphan Process)?", "id": "Proses, Proses Induk (Parent) Telah Berhenti." },
  { "en": "Apa Itu Sinyal (Signal) Linux/Unix?", "id": "Mekanisme Notifikasi Asinkron, Antar Proses (IPC)." },
  { "en": "Apa Sinyal SIGKILL (Sinyal 9)?", "id": "Sinyal Menghentikan Paksa Proses (Tidak Ditangkap)." },
  { "en": "Apa Sinyal SIGTERM (Sinyal 15)?", "id": "Sinyal Permintaan Terminasi Proses (Bisa Ditangkap)." },
  { "en": "Apa Sinyal SIGHUP (Sinyal 1)?", "id": "Sinyal Hangup (Putus), (Memuat Ulang Konfigurasi)." },
  { "en": "Apa Mode (Permission) Oktal 777?", "id": "Semua, Punya Izin Baca, Tulis, Eksekusi." },
  { "en": "Apa Mode (Permission) Oktal 644?", "id": "Pemilik (Baca, Tulis), Lainnya (Hanya Baca)." },
  { "en": "Apa Mode (Permission) Oktal 755?", "id": "Pemilik (Baca, Tulis, Eksekusi), Lainnya (Baca, Eksekusi)." },
  { "en": "Apa Itu SetUID (Set User ID)?", "id": "Bit Izin, Eksekusi File, Sebagai Pemilik (Owner)." },
  { "en": "Apa Itu SetGID (Set Group ID)?", "id": "Bit Izin, Eksekusi File, Sebagai Grup (Group)." },
  { "en": "Apa Itu Sticky Bit (Bit Lengket)?", "id": "Izin Direktori, Hanya Pemilik, Bisa Hapus File." },
  { "en": "Apa Itu File '/etc/sudoers'?", "id": "File Konfigurasi, Hak Akses Perintah 'sudo'." },
  { "en": "Apa Itu Manajemen Layanan TI (ITSM)?", "id": "Pengelolaan Layanan TI, Memberi Nilai Pelanggan." },
  { "en": "Apa Itu Katalog Layanan (Service Catalog)?", "id": "Daftar Layanan TI, Ditawarkan Departemen TI." },
  { "en": "Apa Itu Manajemen Insiden (Incident Management)?", "id": "Proses Mengatasi Gangguan, Mengembalikan Layanan Cepat." },
  { "en": "Apa Itu Manajemen Masalah (Problem Management)?", "id": "Proses Mencari, Mengatasi Akar Penyebab Insiden." },
  { "en": "Apa Itu Manajemen Perubahan (Change Management)?", "id": "Proses Kontrol, Mengelola Perubahan Infrastruktur TI." },
  { "en": "Apa Itu Manajemen Rilis (Release Management)?", "id": "Proses Perencanaan, Pengujian, Penerapan Rilis Baru." },
  { "en": "Apa Itu CMDB (Configuration Management Database)?", "id": "Database, Menyimpan Informasi Komponen TI (CI)." },
  { "en": "Apa Itu Item Konfigurasi (CI)?", "id": "Aset, Komponen, Perlu Dikelola (Server, Aplikasi)." },
  { "en": "Apa Itu Key Performance Indicator (KPI)?", "id": "Indikator Kinerja Utama, Mengukur Keberhasilan Tujuan." },
  { "en": "Apa Itu Kriptografi Kunci Publik (PKC)?", "id": "Kriptografi, Menggunakan Pasangan Kunci (Asimetris)." },
  { "en": "Apa Fungsi Kunci Publik?", "id": "Untuk Enkripsi Data, Verifikasi Tanda Tangan." },
  { "en": "Apa Fungsi Kunci Privat?", "id": "Untuk Dekripsi Data, Membuat Tanda Tangan." },
  { "en": "Apa Itu Serangan Balas (Replay Attack)?", "id": "Serangan Jaringan, Mencegat, Mengirim Ulang Transmisi Data." },
  { "en": "Apa Itu Nonce (Angka Sekali Pakai)?", "id": "Angka Acak, Digunakan Sekali, Mencegah Replay Attack." },
  { "en": "Apa Itu Perfect Forward Secrecy (PFS)?", "id": "Fitur Keamanan, Kunci Sesi Unik, (Terpisah)." },
  { "en": "Apa Itu Kurva Eliptik (Elliptic Curve Cryptography)?", "id": "Kriptografi Kunci Publik, Berbasis Kurva Eliptik." },
  { "en": "Apa Keuntungan Kriptografi (Cryptography) Kurva Eliptik?", "id": "Ukuran Kunci Lebih Kecil, Keamanan Setara." },
  { "en": "Apa Itu Saluran Samping (Side-Channel Attack)?", "id": "Serangan, Berbasis Implementasi Fisik (Daya, Waktu)." },
  { "en": "Apa Itu Analisis Trafik (Traffic Analysis)?", "id": "Menganalisis Pola Komunikasi (Metadata, Waktu)." },
  { "en": "Apa Itu PGP (Pretty Good Privacy)?", "id": "Program Enkripsi Email, Otentikasi, Privasi." },
  { "en": "Apa Itu OpenPGP?", "id": "Standar Terbuka, PGP (Enkripsi, Tanda Tangan)." },
  { "en": "Apa Itu GnuPG (GPG)?", "id": "Implementasi Open-Source, Gratis, Standar OpenPGP." },
  { "en": "Apa Itu Key Signing Party (Pesta Penandatanganan Kunci)?", "id": "Acara Verifikasi Identitas, Menandatangani Kunci Publik." },
  { "en": "Apa Itu Web of Trust (Jaringan Kepercayaan)?", "id": "Model Kepercayaan PGP, Desentralisasi (Antar Pengguna)." },
  { "en": "Apa Itu Model Awan (Cloud) Komunitas?", "id": "Infrastruktur Awan, Dibagikan Beberapa Organisasi." },
  { "en": "Apa Itu Virtual Private Cloud (VPC)?", "id": "Jaringan Privat Terisolasi, Dalam Awan Publik." },
  { "en": "Apa Itu Availability Zone (AZ)?", "id": "Lokasi Fisik, Pusat Data (Data Center) Unik." },
  { "en": "Apa Itu Region (Wilayah) Awan?", "id": "Area Geografis, Terdiri Banyak Availability Zone." },
  { "en": "Apa Itu Steward (Pengurus) Data?", "id": "Bertanggung Jawab, Kualitas, Definisi Aset Data." },
  { "en": "Apa Itu Garis Keturunan (Lineage) Data?", "id": "Siklus Hidup Data (Asal, Pergerakan, Transformasi)." },
  { "en": "Apa Itu Master Data Management (MDM)?", "id": "Manajemen Data Master (Inti) Bisnis Konsisten." },
  { "en": "Apa Itu Data Master (Master Data)?", "id": "Data Inti Bisnis (Pelanggan, Produk, Karyawan)." },
  { "en": "Apa Itu Data Transaksional (Transactional Data)?", "id": "Data, Mencatat Transaksi Bisnis (Penjualan, Pembelian)." },
  { "en": "Apa Itu Data Tidak Terstruktur (Unstructured Data)?", "id": "Data, Format Tidak Terdefinisi (Teks, Gambar)." },
  { "en": "Apa Itu Data Semi-Terstruktur (Semi-Structured Data)?", "id": "Data, Tidak Terstruktur Penuh (JSON, XML)." },
  { "en": "Apa Itu Data Terstruktur (Structured Data)?", "id": "Data, Format Terdefinisi (Tabel Basis Data)." },
  { "en": "Apa SQL (Structured Query Language) vs NoSQL?", "id": "SQL Relasional (Tabel), NoSQL Non-Relasional (Fleksibel)." },
  { "en": "Apa Itu Skema (Schema) Saat Menulis (Write)?", "id": "Data Divalidasi, Skema, Saat Ditulis (SQL)." },
  { "en": "Apa Itu Skema (Schema) Saat Membaca (Read)?", "id": "Skema Diterapkan, Saat Data Dibaca (NoSQL)." },
  { "en": "Apa Itu Protokol POP3S (POP3 Secure)?", "id": "Versi Aman POP3, Menggunakan Enkripsi SSL/TLS." },
  { "en": "Apa Itu Protokol IMAPS (IMAP Secure)?", "id": "Versi Aman IMAP, Menggunakan Enkripsi SSL/TLS." },
  { "en": "Apa Itu SMTPS (SMTP Secure)?", "id": "Versi Aman SMTP, Menggunakan Enkripsi SSL/TLS." },
  { "en": "Apa Itu Port 995 (POP3S)?", "id": "Port Jaringan Standar, Layanan POP3S." },
  { "en": "Apa Itu Port 993 (IMAPS)?", "id": "Port Jaringan Standar, Layanan IMAPS." },
  { "en": "Apa Itu Port 465 (SMTPS)?", "id": "Port Jaringan Standar, Layanan SMTPS (Lama)." },
  { "en": "Apa Itu STARTTLS?", "id": "Perintah, Meningkatkan Koneksi Teks, (Enkripsi TLS)." },
  { "en": "Apa Itu Port 587 (SMTP Submission)?", "id": "Port Pengiriman SMTP (Klien), Menggunakan STARTTLS." },
  { "en": "Apa Itu Mail Transfer Agent (MTA)?", "id": "Perangkat Lunak, Transfer Email, Antar Server." },
  { "en": "Apa Itu Mail Delivery Agent (MDA)?", "id": "Perangkat Lunak, Pengiriman Email, Kotak Surat Pengguna." },
  { "en": "Apa Itu Mail User Agent (MUA)?", "id": "Aplikasi Klien Email (Outlook, Gmail, Thunderbird)." },
  { "en": "Apa Itu Catatan (Record) DNS SPF?", "id": "Sender Policy Framework, (Mencegah Pemalsuan Email)." },
  { "en": "Apa Itu Catatan (Record) DNS DKIM?", "id": "DomainKeys Identified Mail, (Tanda Tangan Digital Email)." },
  { "en": "Apa Itu Catatan (Record) DNS DMARC?", "id": "Domain-based Message Authentication, (Kebijakan Email)." },
  { "en": "Apa Itu Open Relay (Relai Terbuka) SMTP?", "id": "Server SMTP, Meneruskan Email, Siapapun (Spam)." },
  { "en": "Apa Itu Greylisting (Daftar Kelabu) Email?", "id": "Teknik Anti-Spam, Menolak Sementara Email Asing." },
  { "en": "Apa Itu Blacklist (Daftar Hitam) Email?", "id": "Daftar Alamat IP, Server, Dikenal Pengirim Spam." },
  { "en": "Apa Itu Whitelist (Daftar Putih) Email?", "id": "Daftar Alamat IP, Server, Dipercaya (Bukan Spam)." },
  { "en": "Apa Itu Sistem Push E-Mail?", "id": "Notifikasi Email, Real-Time (Server Ke Klien)." },
  { "en": "Apa Itu Sistem Pull E-Mail?", "id": "Klien Memeriksa Email, Server (Interval Waktu)." },
  { "en": "Apa Itu End-to-End Encryption (E2EE)?", "id": "Enkripsi, Pesan Terlindungi, Pengirim Ke Penerima." },
  { "en": "Apa Perbedaan E2EE (End-to-End Encryption) Dan Transport?", "id": "E2EE Melindungi Konten, Transport Melindungi Jalur." },
  { "en": "Apa Itu Man-in-the-Browser (MitB)?", "id": "Serangan Trojan, Memodifikasi Halaman Web Browser." },
  { "en": "Apa Itu Serangan Typo-squatting (Penyerobotan Tipo)?", "id": "Mendaftarkan Domain, Mirip (Kesalahan Ketik)." },
  { "en": "Apa Itu Serangan Water Hole (Lubang Air)?", "id": "Serangan, Menginfeksi Situs, Dikunjungi Target." },
  { "en": "Apa Itu Serangan Shoulder Surfing (Mengintip Bahu)?", "id": "Melihat Informasi Rahasia, (PIN, Password)." },
  { "en": "Apa Itu Token (Token) Keamanan?", "id": "Perangkat Keras, Token Digital, Otentikasi." },
  { "en": "Apa Itu Token (Token) Perangkat Lunak?", "id": "Aplikasi, Menghasilkan Kode, Otentikasi (OTP)." },
  { "en": "Apa Itu OTP (One Time Password)?", "id": "Kata Sandi, Hanya Berlaku, Satu Kali." },
  { "en": "Apa Itu TOTP (Time-based One Time Password)?", "id": "OTP, Dihasilkan Berdasarkan Waktu (Sinkron)." },
  { "en": "Apa Itu HOTP (HMAC-based One Time Password)?", "id": "OTP, Dihasilkan Berdasarkan Konter (Counter)." },
  { "en": "Apa Itu Defense in Depth (Pertahanan Berlapis)?", "id": "Strategi Keamanan, Menggunakan Banyak Lapisan Kontrol." },
  { "en": "Apa Itu Air Gapping (Celah Udara)?", "id": "Isolasi Fisik, Jaringan Aman, Jaringan Lain." },
  { "en": "Apa Itu CVSS (Common Vulnerability Scoring System)?", "id": "Standar Terbuka, Penilaian Tingkat Keparahan Kerentanan." },
  { "en": "Apa Itu CVE (Common Vulnerabilities and Exposures)?", "id": "Daftar Identifikasi Unik, Kerentanan Keamanan Publik." },
  { "en": "Apa Itu Zero-Knowledge Proof (Bukti Nol-Pengetahuan)?", "id": "Metode Kriptografi, Membuktikan Pernyataan (Tanpa Detail)." },
  { "en": "Apa Itu Penetration Testing (Pengujian Penetrasi)?", "id": "Simulasi Serangan Siber, Evaluasi Keamanan Sistem." },
  { "en": "Apa Itu Pen-Tester (Penguji Penetrasi) White Box?", "id": "Penguji, Pengetahuan Penuh, Infrastruktur, Kode Sumber." },
  { "en": "Apa Itu Pen-Tester (Penguji Penetrasi) Black Box?", "id": "Penguji, Tanpa Pengetahuan Apapun, Sistem Internal." },
  { "en": "Apa Itu Pen-Tester (Penguji Penetrasi) Grey Box?", "id": "Penguji, Pengetahuan Terbatas, Sistem Internal (Login)." },
  { "en": "Apa Itu Red Team (Tim Merah)?", "id": "Tim Keamanan, Mensimulasikan Penyerang (Ofensif)." },
  { "en": "Apa Itu Blue Team (Tim Biru)?", "id": "Tim Keamanan, Mempertahankan Sistem, (Defensif)." },
  { "en": "Apa Itu Purple Team (Tim Ungu)?", "id": "Tim Keamanan, Kolaborasi Tim Merah, Biru." },
  { "en": "Apa Itu SOC (Security Operations Center)?", "id": "Fasilitas Terpusat, Tim Keamanan (Pemantauan, Analisis)." },
  { "en": "Apa Itu Sandbox (Bak Pasir) Analisis Malware?", "id": "Lingkungan Terisolasi, Aman, Menjalankan Malware." },
  { "en": "Apa Itu Analisis Statis (Static Analysis) Malware?", "id": "Menganalisis Malware, Tanpa Menjalankan Kode." },
  { "en": "Apa Itu Analisis Dinamis (Dynamic Analysis) Malware?", "id": "Menganalisis Malware, Saat Dijalankan (Sandbox)." },
  { "en": "Apa Itu Digital Signature (Tanda Tangan Digital) Malware?", "id": "Pola Unik (Hash), Identifikasi Malware Dikenal." },
  { "en": "Apa Itu Malware Polimorfik (Polymorphic Malware)?", "id": "Malware, Mengubah Kode, Menghindari Deteksi Tanda Tangan." },
  { "en": "Apa Itu Malware Metamorfik (Metamorphic Malware)?", "id": "Malware Polimorfik, Menulis Ulang Kode (Sangat Sulit)." },
  { "en": "Apa Itu Backdoor (Pintu Belakang) Perangkat Lunak?", "id": "Metode Tersembunyi, Akses Sistem (Bypass Otentikasi)." },
  { "en": "Apa Itu Rootkit?", "id": "Malware, Dirancang Mendapat Akses Root (Tersembunyi)." },
  { "en": "Apa Itu Rootkit Mode Pengguna (User-Mode)?", "id": "Rootkit, Beroperasi, Ruang Pengguna (User Space)." },
  { "en": "Apa Itu Rootkit Mode Kernel (Kernel-Mode)?", "id": "Rootkit, Beroperasi, Ruang Kernel (Sangat Berbahaya)." },
  { "en": "Apa Itu Bootkit?", "id": "Rootkit, Menginfeksi Master Boot Record (MBR)." },
  { "en": "Apa Itu Logic Bomb (Bom Logika)?", "id": "Kode Jahat, Tertanam, Aktif Pemicu (Tanggal, Kondisi)." },
  { "en": "Apa Itu Fileless (Tanpa File) Malware?", "id": "Malware, Beroperasi, Memori (RAM), Tanpa File Disk." },
  { "en": "Apa Itu Advanced Persistent Threat (APT)?", "id": "Serangan Siber, Jangka Panjang, Tersembunyi (Grup Terorganisir)." },
  { "en": "Apa Itu Cyber Kill Chain (Rantai Pembunuhan Siber)?", "id": "Model Tahapan, Serangan Siber (Lockheed Martin)." },
  { "en": "Apa Itu Indikator Kompromi (Indicator of Compromise)?", "id": "Bukti Forensik, Potensi Intrusi Jaringan (IP Aneh)." },
  { "en": "Apa Itu Serangan DDoS (Distributed Denial of Service) Refleksi?", "id": "Menggunakan Server Pihak Ketiga, Memperkuat Serangan." },
  { "en": "Apa Itu Serangan DDoS (Distributed Denial of Service) Amplifikasi?", "id": "Serangan Refleksi, Respons Jauh Lebih Besar (DNS)." },
  { "en": "Apa Itu Serangan Smurf?", "id": "Serangan Amplifikasi DDoS, Menggunakan ICMP Broadcast." },
  { "en": "Apa Itu Serangan Fraggle?", "id": "Serangan Amplifikasi DDoS, Mirip Smurf (Menggunakan UDP)." },
  { "en": "Apa Itu Ingress Filtering (Penyaringan Masuk)?", "id": "Penyaringan Paket Jaringan, Masuk (Blokir IP Palsu)." },
  { "en": "Apa Itu Egress Filtering (Penyaringan Keluar)?", "id": "Penyaringan Paket Jaringan, Keluar (Cegah Serangan)." },
  { "en": "Apa Itu Deep Packet Inspection (DPI)?", "id": "Firewall, Memeriksa Konten (Payload) Paket Data." },
  { "en": "Apa Itu Network Access Control (NAC)?", "id": "Kebijakan Keamanan, Mengontrol Akses Perangkat, Jaringan." },
  { "en": "Apa Itu Access Control List (ACL)?", "id": "Daftar Aturan Izin, (Firewall, Router, Filesystem)." },
  { "en": "Apa Itu Server AAA (Authentication, Authorization, Accounting)?", "id": "Kerangka Kerja, Kontrol Akses (RADIUS, TACACS+)." },
  { "en": "Apa Itu Otorisasi (Authorization)?", "id": "Menentukan Hak Akses, Sumber Daya (Setelah Otentikasi)." },
  { "en": "Apa Itu Akuntansi (Accounting) AAA?", "id": "Mencatat Penggunaan Sumber Daya, Pengguna (Audit)." },
  { "en": "Apa Itu TACACS+ (Terminal Access Controller Access-Control System)?", "id": "Protokol AAA, Cisco (Pemisahan AAA, TCP)." },
  { "en": "Apa Itu RBAC (Role-Based Access Control)?", "id": "Kontrol Akses, Berbasis Peran (Jabatan) Pengguna." },
  { "en": "Apa Itu MAC (Mandatory Access Control)?", "id": "Kontrol Akses, Berbasis Level Keamanan (Label)." },
  { "en": "Apa Itu DAC (Discretionary Access Control)?", "id": "Kontrol Akses, Pemilik Sumber Daya, Menentukan Izin." },
  { "en": "Apa Itu SAML (Security Assertion Markup Language)?", "id": "Standar XML, Pertukaran Otentikasi, Otorisasi (SSO)." },
  { "en": "Apa Itu Identity Provider (IdP)?", "id": "Entitas, Membuat, Mengelola Identitas (Memverifikasi)." },
  { "en": "Apa Itu Service Provider (SP)?", "id": "Entitas, Menyediakan Layanan (Mempercayai IdP)." },
  { "en": "Apa Itu Directory Traversal (Pelintasan Direktori)?", "id": "Serangan, Mengakses File, Direktori (Diluar Batas)." },
  { "en": "Apa Itu File Inclusion (Penyertaan File) Lokal?", "id": "Serangan, Menyertakan File, Server Lokal (Eksekusi)." },
  { "en": "Apa Itu File Inclusion (Penyertaan File) Remote?", "id": "Serangan, Menyertakan File, Server Jarak Jauh." },
  { "en": "Apa Itu Command Injection (Injeksi Perintah)?", "id": "Serangan, Eksekusi Perintah OS, Melalui Aplikasi." },
  { "en": "Apa Itu Input Validation (Validasi Input)?", "id": "Praktik Keamanan, Memeriksa, Membersihkan Input Pengguna." },
  { "en": "Apa Itu Whitelisting (Daftar Putih) Input?", "id": "Hanya Menerima, Input Yang Diketahui Aman." },
  { "en": "Apa Itu Blacklisting (Daftar Hitam) Input?", "id": "Menolak Input, Yang Diketahui Berbahaya (Kurang Efektif)." },
  { "en": "Apa Itu Output Encoding (Pengkodean Output)?", "id": "Mengubah Karakter Khusus, Data Output (Mencegah XSS)." },
  { "en": "Apa Itu Parameterized Query (Prepared Statement)?", "id": "Kueri SQL, Template (Mencegah SQL Injection)." },
  { "en": "Apa Itu CSRF (Cross-Site Request Forgery) Token?", "id": "Token Unik, Mencegah Serangan CSRF (Validasi)." },
  { "en": "Apa Itu Cookie (Kuki) Atribut 'HttpOnly'?", "id": "Mencegah Kuki, Diakses Oleh JavaScript (XSS)." },
  { "en": "Apa Itu Cookie (Kuki) Atribut 'Secure'?", "id": "Memastikan Kuki, Dikirim Hanya, Lewat HTTPS." },
  { "en": "Apa Itu Cookie (Kuki) Atribut 'SameSite'?", "id": "Mengontrol Kuki, Dikirim Permintaan Lintas Situs (CSRF)." },
  { "en": "Apa Itu Serverless (Tanpa Server) Computing?", "id": "Model Awan, Eksekusi Kode (FaaS), Tanpa Server." },
  { "en": "Apa Itu Cold Start (Mulai Dingin) FaaS?", "id": "Waktu Tunda, Eksekusi Fungsi (Inisialisasi Kontainer)." },
  { "en": "Apa Itu Warm Start (Mulai Hangat) FaaS?", "id": "Eksekusi Fungsi, Cepat (Kontainer Sudah Berjalan)." },
  { "en": "Apa Itu Pola Desain (Design Pattern) Saga?", "id": "Manajemen Transaksi, Sistem Terdistribusi (Layanan Mikro)." },
  { "en": "Apa Itu Orchestration (Orkestrasi) Saga?", "id": "Koordinasi Saga, Terpusat (Satu Koordinator)." },
  { "en": "Apa Itu Choreography (Koreografi) Saga?", "id": "Koordinasi Saga, Terdesentralisasi (Berbasis Event)." },
  { "en": "Apa Itu Transaksi Kompensasi (Compensating Transaction)?", "id": "Transaksi, Membatalkan Efek, Operasi Gagal (Saga)." },
  { "en": "Apa Itu Two-Phase Commit (2PC)?", "id": "Protokol Transaksi Atomik, Terdistribusi (Punya Koordinator)." },
  { "en": "Apa Itu Three-Phase Commit (3PC)?", "id": "Evolusi 2PC, (Mengurangi Masalah Pemblokiran)." },
  { "en": "Apa Itu Paxos?", "id": "Protokol Konsensus, Sistem Terdistribusi (Toleransi Kegagalan)." },
  { "en": "Apa Itu Raft?", "id": "Protokol Konsensus, Alternatif Paxos (Lebih Mudah Dipahami)." },
  { "en": "Apa Itu Gossip Protocol (Protokol Gosip)?", "id": "Protokol Komunikasi, Terdistribusi (Penyebaran Informasi)." },
  { "en": "Apa Itu Sistem Berkas HDFS (Hadoop Distributed File System)?", "id": "Sistem Berkas Terdistribusi, (Dirancang Untuk Hadoop)." },
  { "en": "Apa Itu MapReduce?", "id": "Model Pemrograman, Pemrosesan Data Paralel (Hadoop)." },
  { "en": "Apa Itu Fase Map (Peta) MapReduce?", "id": "Tahap Pemrosesan, Memfilter, Mengurutkan Data (Key-Value)." },
  { "en": "Apa Itu Fase Reduce (Reduksi) MapReduce?", "id": "Tahap Agregasi, Meringkas Data (Hasil Map)." },
  { "en": "Apa Itu Apache Spark?", "id": "Mesin Pemrosesan Data, Terdistribusi (Cepat, In-Memory)." },
  { "en": "Apa Itu RDD (Resilient Distributed Datasets) Spark?", "id": "Struktur Data Inti Spark (Immutable, Terdistribusi)." },
  { "en": "Apa Itu Apache Hive?", "id": "Gudang Data (Warehouse), Kueri SQL (Di Atas Hadoop)." },
  { "en": "Apa Itu Apache Pig?", "id": "Platform Aliran Data, Paralel (Skrip Pig Latin)." },
  { "en": "Apa Itu Apache HBase?", "id": "Database NoSQL, Terdistribusi, Berorientasi Kolom (Hadoop)." },
  { "en": "Apa Itu Apache Cassandra?", "id": "Database NoSQL, Terdistribusi, Ketersediaan Tinggi (Wide-Column)." },
  { "en": "Apa Itu MongoDB?", "id": "Database NoSQL, Berorientasi Dokumen (Format JSON/BSON)." },
  { "en": "Apa Itu BSON (Binary JSON)?", "id": "Format Serialisasi Biner, Representasi JSON." },
  { "en": "Apa Itu CouchDB?", "id": "Database NoSQL, Berorientasi Dokumen (Fokus Replikasi)." },
  { "en": "Apa Itu ArangoDB?", "id": "Database NoSQL, Multi-Model (Dokumen, Graf, Key-Value)." },
  { "en": "Apa Itu Flux (Arsitektur Aplikasi)?", "id": "Pola Arsitektur Aplikasi, Aliran Data Searah (Facebook)." },
  { "en": "Apa Itu Redux?", "id": "Pustaka Manajemen Keadaan (State) Global JavaScript." },
  { "en": "Apa Itu Vuex?", "id": "Pustaka Manajemen Keadaan (State) Global, Vue.js." },
  { "en": "Apa Itu Store (Penyimpanan) Redux?", "id": "Objek, Menyimpan Keadaan (State) Aplikasi Tunggal." },
  { "en": "Apa Itu Action (Aksi) Redux?", "id": "Objek Plain, Mendeskripsikan Perubahan (Payload)." },
  { "en": "Apa Itu Reducer (Pereduksi) Redux?", "id": "Fungsi Murni, Menentukan Perubahan Keadaan (State)." },
  { "en": "Apa Itu Dispatch (Pengiriman) Redux?", "id": "Metode, Mengirimkan Aksi (Action) Ke Store." },
  { "en": "Apa Itu Selector (Pemilih) Redux?", "id": "Fungsi, Mengambil Bagian Keadaan (State) Store." },
  { "en": "Apa Itu Middleware (Perangkat Perantara) Redux?", "id": "Logika Tambahan, Antara Dispatch, Reducer." },
  { "en": "Apa Itu Redux Thunk?", "id": "Middleware Redux, Menulis Aksi (Action) Asinkron." },
  { "en": "Apa Itu Redux Saga?", "id": "Middleware Redux, Menangani Efek Samping (Generator)." },
  { "en": "Apa Itu Desain Responsif (Responsive Design)?", "id": "Desain Web, Adaptasi Berbagai Ukuran Layar." },
  { "en": "Apa Itu Desain Adaptif (Adaptive Design)?", "id": "Desain Web, Tata Letak Tetap, Berbeda." },
  { "en": "Apa Itu Mobile-First Design (Desain Mobile-Dulu)?", "id": "Desain Dimulai, Layar Terkecil (Mobile) Dahulu." },
  { "en": "Apa Itu Graceful Degradation (Degradasi Anggun)?", "id": "Desain, Fitur Berkurang, Browser Lama (Kompleks Dulu)." },
  { "en": "Apa Itu Progressive Enhancement (Peningkatan Progresif)?", "id": "Desain, Fitur Bertambah, Browser Modern (Dasar Dulu)." },
  { "en": "Apa Itu Viewport (Jendela Tampilan) HTML?", "id": "Area Tampilan Halaman Web, Pengguna (Layar)." },
  { "en": "Apa Itu Meta Tag (Tanda) Viewport?", "id": "Tag HTML, Mengontrol Tata Letak, Skala (Mobile)." },
  { "en": "Apa Itu Media Query (Kueri Media) CSS?", "id": "Fitur CSS, Menerapkan Gaya, Berbasis Karakteristik (Lebar)." },
  { "en": "Apa Itu Fluid Grid (Grid Cair)?", "id": "Tata Letak Grid, Menggunakan Persentase (Bukan Piksel)." },
  { "en": "Apa Itu Gambar Fleksibel (Flexible Image)?", "id": "Gambar, Skala Ukuran, Sesuai Kontainer (CSS)." },
  { "en": "Apa Itu Arsitektur ARM (Advanced RISC Machine)?", "id": "Arsitektur Prosesor, RISC, Efisien Daya (Mobile)." },
  { "en": "Apa Itu Arsitektur x86?", "id": "Arsitektur Prosesor, CISC, (Intel, AMD, Desktop)." },
  { "en": "Apa Itu x86-64 (x64)?", "id": "Ekstensi 64-bit, Arsitektur Prosesor x86." },
  { "en": "Apa Itu Komputasi 32-bit?", "id": "Prosesor, Menangani Data, 32 Bit Sekaligus." },
  { "en": "Apa Itu Komputasi 64-bit?", "id": "Prosesor, Menangani Data, 64 Bit Sekaligus." },
  { "en": "Batas Memori (RAM) Sistem 32-bit?", "id": "Hanya Dapat Mengakses, Sekitar 4 Gigabyte RAM." },
  { "en": "Apa Itu Bahasa Assembly (Rakitan)?", "id": "Bahasa Pemrograman, Tingkat Rendah (Mnemonik CPU)." },
  { "en": "Apa Itu Assembler (Perakit)?", "id": "Program, Menerjemahkan Bahasa Assembly, Kode Mesin." },
  { "en": "Apa Itu Disassembler (Pembongkar)?", "id": "Program, Menerjemahkan Kode Mesin, Bahasa Assembly." },
  { "en": "Apa Itu Reverse Engineering (Rekayasa Balik)?", "id": "Proses Menganalisis Produk, Memahami Desain." },
  { "en": "Apa Itu Obfuscation (Obfuskasi) Kode?", "id": "Membuat Kode, Sulit Dibaca, Dipahami (Melindungi)." },
  { "en": "Apa Itu Minification (Minifikasi) Kode?", "id": "Menghapus Karakter Tidak Perlu, Kode (Ukuran File)." },
  { "en": "Apa Itu Uglification (Uglifikasi) Kode?", "id": "Proses Obfuskasi, (Mengubah Nama Variabel, Fungsi)." },
  { "en": "Apa Itu Dead Code (Kode Mati)?", "id": "Kode Program, Dieksekusi, Tidak Pernah Digunakan." },
  { "en": "Apa Itu Dead Code Elimination (Eliminasi Kode Mati)?", "id": "Optimasi Compiler, Menghapus Kode Mati." },
  { "en": "Apa Itu Tree Shaking (Goyangan Pohon)?", "id": "Eliminasi Kode Mati, (JavaScript, Modul ES6)." },
  { "en": "Apa Itu Lazy Loading (Pemuatan Malas)?", "id": "Menunda Pemuatan, Sumber Daya (Hingga Dibutuhkan)." },
  { "en": "Apa Itu Eager Loading (Pemuatan Cepat)?", "id": "Memuat Semua Sumber Daya, Sekaligus (Awal)." },
  { "en": "Apa Itu Preloading (Pramuat)?", "id": "Memuat Sumber Daya, Prioritas Tinggi (Nantinya)." },
  { "en": "Apa Itu Prefetching (Pra-ambil)?", "id": "Memuat Sumber Daya, Prioritas Rendah (Navigasi Berikutnya)." },
  { "en": "Apa Itu Critical Rendering Path (Jalur Render Kritis)?", "id": "Langkah Browser, Konversi HTML, CSS, Tampilan." },
  { "en": "Apa Itu Parsing (Penguraian) HTML?", "id": "Proses Browser, Membaca HTML, Membangun DOM Tree." },
  { "en": "Apa Itu CSSOM (CSS Object Model) Tree?", "id": "Representasi Pohon, Gaya CSS, Halaman Web." },
  { "en": "Apa Itu Render Tree (Pohon Render)?", "id": "Gabungan DOM, CSSOM (Hanya Node Terlihat)." },
  { "en": "Apa Itu Layout (Tata Letak) Browser?", "id": "Browser, Menghitung Posisi, Ukuran Elemen Render." },
  { "en": "Apa Itu Painting (Pengecatan) Browser?", "id": "Browser, Menggambar Piksel Elemen, Ke Layar." },
  { "en": "Apa Itu Reflow (Aliran Ulang) Browser?", "id": "Proses Perhitungan Ulang, Layout (Perubahan Geometri)." },
  { "en": "Apa Itu Repaint (Pengecatan Ulang) Browser?", "id": "Proses Penggambaran Ulang, Piksel (Perubahan Tampilan)." },
  { "en": "Apa Itu Akselerasi Perangkat Keras (Hardware Acceleration) GPU?", "id": "Menggunakan GPU, Mempercepat Tugas (Render Grafis)." },
  { "en": "Apa Itu Skalabilitas (Scalability) Vertikal vs Horizontal?", "id": "Vertikal (Satu Kuat), Horizontal (Banyak Biasa)." },
  { "en": "Apa Itu Sistem Berkas Terdistribusi (Distributed File System)?", "id": "Sistem Berkas, Mengelola File, Banyak Server." },
  { "en": "Apa Itu Ceph?", "id": "Platform Penyimpanan Terdistribusi, Open-Source (Objek, Blok)." },
  { "en": "Apa Itu GlusterFS?", "id": "Sistem Berkas Terdistribusi, Skalabel (Open-Source)." },
  { "en": "Apa Itu Blob (Binary Large Object) Storage?", "id": "Penyimpanan Objek, Data Tidak Terstruktur (Gambar, Video)." },
  { "en": "Apa Itu Penyimpanan Blok (Block Storage)?", "id": "Penyimpanan Data, Blok Ukuran Tetap (Volume Disk)." },
  { "en": "Apa Itu Penyimpanan File (File Storage)?", "id": "Penyimpanan Data, Hirarki File, Folder (NAS)." },
  { "en": "Apa Itu Penyimpanan Objek (Object Storage)?", "id": "Penyimpanan Data, Objek (Data, Metadata, ID Unik)." },
  { "en": "Apa Itu Latensi (Latency) Penyimpanan?", "id": "Waktu Tunda, Permintaan, Respons Data Penyimpanan." },
  { "en": "Apa Itu IOPS (Input/Output Operations Per Second)?", "id": "Ukuran Kinerja Penyimpanan (Operasi Baca/Tulis)." },
  { "en": "Apa Itu Throughput (Laju) Penyimpanan?", "id": "Laju Transfer Data, Berkelanjutan (MB/s, GB/s)." },
  { "en": "Apa Itu Deduplikasi (Deduplication) Data?", "id": "Teknik Penyimpanan, Menghapus Salinan Data Duplikat." },
  { "en": "Apa Itu Kompresi (Compression) Data?", "id": "Proses Mengurangi Ukuran, Data (Hemat Ruang)." },
  { "en": "Apa Itu Thin Provisioning (Penyediaan Tipis)?", "id": "Alokasi Ruang Penyimpanan, Sesuai Kebutuhan (Virtual)." },
  { "en": "Apa Itu Thick Provisioning (Penyediaan Tebal)?", "id": "Alokasi Penuh Ruang Penyimpanan, Sejak Awal." },
  { "en": "Apa Itu CAD (Computer Aided Design)?", "id": "Perangkat Lunak, Desain, Perancangan (Teknik, Arsitektur)." },
  { "en": "Apa Itu CAM (Computer Aided Manufacturing)?", "id": "Perangkat Lunak, Mengontrol Mesin, Manufaktur (Otomatisasi)." },
  { "en": "Apa Itu CAE (Computer Aided Engineering)?", "id": "Perangkat Lunak, Simulasi, Analisis Teknik (Finite Element)." },
  { "en": "Apa Itu GIS (Geographic Information System)?", "id": "Sistem Informasi, Menangkap, Menganalisis Data Geografis." },
  { "en": "Apa Itu GPS (Global Positioning System)?", "id": "Sistem Navigasi Satelit, Penentuan Posisi Global." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Kontrol, Pemantauan Proses Industri (Infrastruktur)." },
  { "en": "Apa Itu IoT (Internet of Things) Industri (IIoT)?", "id": "Penerapan IoT, Sektor Industri, Manufaktur." },
  { "en": "Apa Itu Digital Twin (Kembar Digital)?", "id": "Representasi Virtual, Aset Fisik, Proses (Real-Time)." },
  { "en": "Apa Itu Manufaktur Aditif (Additive Manufacturing)?", "id": "Proses Pembuatan Objek 3D (Cetak 3D)." },
  { "en": "Apa Itu CNC (Computer Numerical Control)?", "id": "Kontrol Komputer Otomatis, Mesin Perkakas (Manufaktur)." },
  { "en": "Apa Itu Sistem Informasi Kesehatan (Health Information System)?", "id": "Sistem, Mengelola Data, Informasi Kesehatan (Rekam Medis)." },
  { "en": "Apa Itu EMR (Electronic Medical Record)?", "id": "Rekam Medis Elektronik, (Internal Satu Faskes)." },
  { "en": "Apa Itu EHR (Electronic Health Record)?", "id": "Rekam Kesehatan Elektronik, (Berbagi Antar Faskes)." },
  { "en": "Apa Itu Standar (Standard) HL7 (Health Level Seven)?", "id": "Standar Internasional, Pertukaran Data Kesehatan Elektronik." },
  { "en": "Apa Itu Standar (Standard) DICOM (Digital Imaging and Communications)?", "id": "Standar Internasional, Gambar Medis (CT Scan, MRI)." },
  { "en": "Apa Itu PACS (Picture Archiving and Communication System)?", "id": "Sistem Penyimpanan, Pengambilan Gambar Medis (DICOM)." },
  { "en": "Apa Itu Telemedicine (Telemedisin)?", "id": "Penyediaan Layanan Kesehatan, Jarak Jauh (Teknologi)." },
  { "en": "Apa Itu HIPAA (Health Insurance Portability and Accountability Act)?", "id": "Hukum AS, Privasi, Keamanan Data Kesehatan." },
  { "en": "Apa Itu Bioinformatika (Bioinformatics)?", "id": "Ilmu Komputasi, Biologi Molekuler (Analisis Genom)." },
  { "en": "Apa Itu Komputasi Kinerja Tinggi (High Performance Computing)?", "id": "Penggunaan Superkomputer, Pemrosesan Data Kompleks." },
  { "en": "Apa Itu FLOPs (Floating Point Operations Per Second)?", "id": "Ukuran Kinerja Komputer (Perhitungan Ilmiah)." },
  { "en": "Apa Itu Klaster (Cluster) Komputer?", "id": "Kumpulan Komputer, Bekerja Bersama (Satu Sistem)." },
  { "en": "Apa Itu Beowulf Cluster?", "id": "Klaster Komputer, Kinerja Tinggi (Komoditas Hardware)." },
  { "en": "Apa Itu MPI (Message Passing Interface)?", "id": "Standar API, Komunikasi Paralel (Antar Proses)." },
  { "en": "Apa Itu OpenMP (Open Multi-Processing)?", "id": "API, Pemrograman Paralel, Memori Berbagi (Shared Memory)." },
  { "en": "Apa Itu CUDA (Compute Unified Device Architecture)?", "id": "Platform Paralel, API, (Pemrograman GPU NVIDIA)." },
  { "en": "Apa Itu OpenCL (Open Computing Language)?", "id": "Standar Terbuka, Pemrograman Paralel, Heterogen (CPU, GPU)." },
  { "en": "Apa Itu GPGPU (General Purpose computing on GPU)?", "id": "Penggunaan GPU, Tugas Komputasi Umum (Non-Grafis)." },
  { "en": "Apa Itu Sistem Berkas Paralel (Parallel File System)?", "id": "Sistem Berkas, Akses Data Bersamaan (Klaster)." },
  { "en": "Apa Itu Lustre (Sistem Berkas)?", "id": "Sistem Berkas Paralel, Open-Source (Skala Besar)." },
  { "en": "Apa Itu GPFS (General Parallel File System)?", "id": "Sistem Berkas Paralel, Kinerja Tinggi (IBM)." },
  { "en": "Apa Itu Jaringan Interkoneksi (Interconnect Network)?", "id": "Jaringan, Menghubungkan Node, Klaster (Cepat)." },
  { "en": "Apa Itu InfiniBand?", "id": "Standar Interkoneksi, Kinerja Tinggi (Latensi Rendah)." },
  { "en": "Apa Itu Ethernet (Ethernet) Kecepatan Tinggi?", "id": "Standar Ethernet, (10GbE, 40GbE, 100GbE)." },
  { "en": "Apa Itu Beban Kerja (Workload) Komputasi?", "id": "Jumlah Pekerjaan, Diminta Dilakukan Sistem Komputer." },
  { "en": "Apa Itu Batch Processing (Pemrosesan Tumpak)?", "id": "Eksekusi Tugas (Job), Non-Interaktif, (Data Besar)." },
  { "en": "Apa Itu Pemrosesan Aliran (Stream Processing)?", "id": "Pemrosesan Data, Real-Time, (Data Mengalir Terus Menerus)." },
  { "en": "Apa Itu Checkpoint (Titik Pemeriksaan) Komputasi?", "id": "Menyimpan Keadaan (State) Aplikasi, (Pemulihan Kegagalan)." },
  { "en": "Apa Itu Pemrosesan Bahasa Alami (Natural Language Processing)?", "id": "Cabang AI, Interaksi Bahasa Manusia." },
  { "en": "Apa Itu Model N-Gram?", "id": "Model Probabilistik, Urutan Kata (N Item)." },
  { "en": "Apa Itu Bigram (N-Gram)?", "id": "Urutan Dua Item (Kata), Berdekatan." },
  { "en": "Apa Itu Trigram (N-Gram)?", "id": "Urutan Tiga Item (Kata), Berdekatan." },
  { "en": "Apa Itu Pohon Urai (Parse Tree)?", "id": "Representasi Pohon, Struktur Sintaksis Kalimat." },
  { "en": "Apa Itu Ambiguitas (Ambiguity) Bahasa?", "id": "Kalimat, Memiliki Banyak Makna (Tafsiran)." },
  { "en": "Apa Itu Machine Translation (Penerjemahan Mesin)?", "id": "Penerjemahan Otomatis, Antar Bahasa Manusia." },
  { "en": "Apa Itu Sistem Rekomendasi (Recommender System)?", "id": "Sistem, Memprediksi Preferensi, Rating Pengguna." },
  { "en": "Apa Itu Penyaringan Kolaboratif (Collaborative Filtering)?", "id": "Metode Rekomendasi, Berbasis Kesamaan Pengguna." },
  { "en": "Apa Itu Penyaringan Berbasis Konten (Content-Based)?", "id": "Metode Rekomendasi, Berbasis Kesamaan Item." },
  { "en": "Apa Itu Masalah Awal Dingin (Cold Start)?", "id": "Masalah Rekomendasi, Pengguna, Item Baru." },
  { "en": "Apa Itu Information Retrieval (IR)?", "id": "Ilmu Pencarian, Informasi, Dokumen, Basis Data." },
  { "en": "Apa Itu Model Ruang Vektor (Vector Space Model)?", "id": "Representasi Dokumen, Teks, Sebagai Vektor." },
  { "en": "Apa Itu Indeks Terbalik (Inverted Index)?", "id": "Struktur Data, Pemetaan Kata, Lokasi Dokumen." },
  { "en": "Apa Itu Query Expansion (Ekspansi Kueri)?", "id": "Menambah Kata Kunci, Kueri Pencarian (Sinonim)." },
  { "en": "Apa Itu Semantic Web (Web Semantik)?", "id": "Ekstensi Web, Data Terstruktur, Makna Jelas." },
  { "en": "Apa Itu RDF (Resource Description Framework)?", "id": "Standar Model Data, Web Semantik (Subjek-Predikat-Objek)." },
  { "en": "Apa Itu SPARQL (SPARQL Protocol and RDF Query Language)?", "id": "Bahasa Kueri, Basis Data RDF (Semantik)." },
  { "en": "Apa Itu Ontologi (Ontology) Komputer?", "id": "Representasi Formal, Pengetahuan, Konsep, Relasi." },
  { "en": "Apa Itu OWL (Web Ontology Language)?", "id": "Bahasa Markup, Mendefinisikan Ontologi Web Semantik." },
  { "en": "Apa Itu Linked Data (Data Tertaut)?", "id": "Metode Publikasi Data, Terstruktur, Terhubung (Web)." },
  { "en": "Apa Itu Domain Name System Security Extensions (DNSSEC)?", "id": "Ekstensi Keamanan DNS, Mencegah Pemalsuan (Validasi)." },
  { "en": "Apa Itu Catatan (Record) DNS RRSIG?", "id": "Tanda Tangan Digital, Catatan DNS (Validasi DNSSEC)." },
  { "en": "Apa Itu Catatan (Record) DNS DS?", "id": "Delegation Signer, (Menghubungkan Zona Induk, Anak)." },
  { "en": "Apa Itu Catatan (Record) DNS NSEC?", "id": "Next Secure, (Membuktikan Catatan Tidak Ada)." },
  { "en": "Apa Itu Serangan DNS Amplification (Amplifikasi DNS)?", "id": "Serangan DDoS Refleksi, Menggunakan Server DNS Terbuka." },
  { "en": "Apa Itu DNS over HTTPS (DoH)?", "id": "Protokol Kueri DNS, Terenkripsi (Lewat HTTPS)." },
  { "en": "Apa Itu DNS over TLS (DoT)?", "id": "Protokol Kueri DNS, Terenkripsi (Lewat TLS)." },
  { "en": "Apa Itu Zona (Zone) DNS?", "id": "Bagian Namespace DNS, Dikelola Server Otoritatif." },
  { "en": "Apa Itu Transfer Zona (Zone Transfer) DNS?", "id": "Proses Menyalin, Data Zona DNS (Server)." },
  { "en": "Apa Itu Server DNS (Domain Name System) Otoritatif?", "id": "Server DNS, Menyimpan Catatan Asli Zona." },
  { "en": "Apa Itu Server DNS (Domain Name System) Rekursif?", "id": "Server DNS, Mencari Jawaban Kueri (ISP)." },
  { "en": "Apa Itu Server DNS (Domain Name System) Root?", "id": "Server Puncak, Hirarki DNS Global (13)." },
  { "en": "Apa Itu Time-To-Live (TTL) DNS?", "id": "Waktu, Catatan DNS, Disimpan Cache Resolver." },
  { "en": "Apa Itu Cache Poisoning (Peracunan Cache) DNS?", "id": "Serangan, Memasukkan Data DNS Palsu, Cache." },
  { "en": "Apa Itu Wildcard (Karakter Pengganti) DNS?", "id": "Catatan DNS, Mencocokkan Subdomain, Tidak Ada." },
  { "en": "Apa Itu Split-Horizon (Cakrawala Terpisah) DNS?", "id": "Menyediakan Jawaban DNS, Berbeda (Internal/Eksternal)." },
  { "en": "Apa Itu Domain-Driven Design (DDD)?", "id": "Pendekatan Desain Perangkat Lunak, (Fokus Domain)." },
  { "en": "Apa Itu Model Domain (Domain Model) DDD?", "id": "Model Konseptual, Objek, Perilaku Domain Bisnis." },
  { "en": "Apa Itu Bounded Context (Konteks Terbatas) DDD?", "id": "Batas Logis, Tempat Model Domain Berlaku." },
  { "en": "Apa Itu Ubiquitous Language (Bahasa Ubiquitous) DDD?", "id": "Bahasa Bersama, Tim Teknis, Bisnis." },
  { "en": "Apa Itu Entitas (Entity) DDD?", "id": "Objek Domain, Identitas Unik, Berkelanjutan." },
  { "en": "Apa Itu Value Object (Objek Nilai) DDD?", "id": "Objek Domain, Tanpa Identitas (Immutable)." },
  { "en": "Apa Itu Agregat (Aggregate) DDD?", "id": "Kumpulan Objek Domain, Diperlakukan Satu Unit." },
  { "en": "Apa Itu Root (Akar) Agregat DDD?", "id": "Entitas Tunggal, Titik Masuk Agregat." },
  { "en": "Apa Itu Repositori (Repository) DDD?", "id": "Pola, Mediasi Antara Domain, Pemetaan Data." },
  { "en": "Apa Itu Layanan (Service) Domain DDD?", "id": "Operasi Domain, Tidak Cocok, Entitas, Value Object." },
  { "en": "Apa Itu Pabrik (Factory) DDD?", "id": "Pola, Menciptakan Objek, Agregat Kompleks." },
  { "en": "Apa Itu Peristiwa (Event) Domain DDD?", "id": "Merekam Kejadian, Penting, Dalam Domain." },
  { "en": "Apa Itu Test-Driven Development (TDD) Merah?", "id": "Fase TDD, Menulis Tes Gagal (Merah)." },
  { "en": "Apa Itu Test-Driven Development (TDD) Hijau?", "id": "Fase TDD, Menulis Kode, Lulus Tes (Hijau)." },
  { "en": "Apa Itu Test-Driven Development (TDD) Refactor?", "id": "Fase TDD, Membersihkan Kode (Tanpa Mengubah Perilaku)." },
  { "en": "Siklus Test-Driven Development (TDD)?", "id": "Merah, Hijau, Refactor (Berulang Terus Menerus)." },
  { "en": "Apa Itu Behavior-Driven Development (BDD)?", "id": "Evolusi TDD, Fokus Perilaku Bisnis (Bahasa Alami)." },
  { "en": "Apa Itu Gherkin (Bahasa BDD)?", "id": "Sintaks Bahasa Alami, (Given, When, Then)." },
  { "en": "Apa Itu Acceptance Test-Driven Development (ATDD)?", "id": "TDD, Ditulis, Perspektif Kriteria Penerimaan Pengguna." },
  { "en": "Apa Itu Selenium?", "id": "Alat Otomatisasi Pengujian, Aplikasi Web (Browser)." },
  { "en": "Apa Itu Appium?", "id": "Alat Otomatisasi Pengujian, Aplikasi Seluler (Mobile)." },
  { "en": "Apa Itu WebDriver (Selenium)?", "id": "API, Kontrol Browser, (Otomatisasi Pengujian Web)." },
  { "en": "Apa Itu Pengujian Beban (Load Testing) JMeter?", "id": "Alat Open-Source, Pengujian Beban, Kinerja." },
  { "en": "Apa Itu Pengujian Stres (Stress Testing) Gatling?", "id": "Alat Pengujian Beban, (Kinerja Tinggi, Scala)." },
  { "en": "Apa Itu Pengujian Mutasi (Mutation Testing) Pitest?", "id": "Alat Pengujian Mutasi, Populer Untuk Java." },
  { "en": "Apa Itu Platform (Platform) Kontainer?", "id": "Lingkungan, Menjalankan, Mengelola Aplikasi Kontainer." },
  { "en": "Apa Itu Pod (Polong) Kubernetes?", "id": "Unit Penerapan (Deployment) Terkecil, Kubernetes (Satu+ Kontainer)." },
  { "en": "Apa Itu Layanan (Service) Kubernetes?", "id": "Abstraksi Jaringan, Mengekspos Kumpulan Pod (Endpoint)." },
  { "en": "Apa Itu Ingress (Jalan Masuk) Kubernetes?", "id": "Objek API, Mengelola Akses Eksternal (HTTP/HTTPS)." },
  { "en": "Apa Itu Deployment (Penerapan) Kubernetes?", "id": "Objek, Mengelola Pembaruan, Replikasi Pod (Deklaratif)." },
  { "en": "Apa Itu ReplicaSet Kubernetes?", "id": "Memastikan Jumlah Pod (Replika) Berjalan Stabil." },
  { "en": "Apa Itu StatefulSet Kubernetes?", "id": "Objek, Mengelola Aplikasi Stateful (Identitas Unik)." },
  { "en": "Apa Itu DaemonSet Kubernetes?", "id": "Memastikan Salinan Pod, Berjalan, Setiap Node." },
  { "en": "Apa Itu ConfigMap Kubernetes?", "id": "Objek API, Menyimpan Data Konfigurasi (Non-Rahasia)." },
  { "en": "Apa Itu Secret (Rahasia) Kubernetes?", "id": "Objek API, Menyimpan Data Sensitif (Password, Token)." },
  { "en": "Apa Itu Volume (Volume) Kubernetes?", "id": "Direktori Penyimpanan, Dapat Diakses Kontainer Pod." },
  { "en": "Apa Itu Persistent Volume (PV) Kubernetes?", "id": "Sumber Daya Penyimpanan, Cluster (Disediakan Admin)." },
  { "en": "Apa Itu Persistent Volume Claim (PVC) Kubernetes?", "id": "Permintaan Penyimpanan, Oleh Pengguna (Menggunakan PV)." },
  { "en": "Apa Itu StorageClass (Kelas Penyimpanan) Kubernetes?", "id": "Menentukan Tipe Penyimpanan, (Penyediaan Dinamis PV)." },
  { "en": "Apa Itu Namespace (Ruang Nama) Kubernetes?", "id": "Partisi Virtual, Cluster Kubernetes (Mengisolasi Sumber Daya)." },
  { "en": "Apa Itu Kubelet?", "id": "Agen Node Kubernetes, (Menjalankan Kontainer Pod)." },
  { "en": "Apa Itu Kube-proxy?", "id": "Proxy Jaringan, Setiap Node (Mengatur Layanan Kubernetes)." },
  { "en": "Apa Itu Kube-apiserver (Server API)?", "id": "Frontend, Control Plane Kubernetes (Mengekspos API)." },
  { "en": "Apa Itu Etcd (Penyimpanan Kunci-Nilai)?", "id": "Penyimpanan Kunci-Nilai, (Database Konfigurasi Kubernetes)." },
  { "en": "Apa Itu Kube-scheduler (Penjadwal)?", "id": "Komponen, Menempatkan Pod, Ke Node (Penjadwalan)." },
  { "en": "Apa Itu Kube-controller-manager (Manajer Kontroler)?", "id": "Komponen, Menjalankan Logika Kontroler (Status Cluster)." },
  { "en": "Apa Itu Helm (Manajer Paket Kubernetes)?", "id": "Manajer Paket, Aplikasi Kubernetes (Charts)." },
  { "en": "Apa Itu Chart (Bagan) Helm?", "id": "Paket Aplikasi, Kubernetes (Kumpulan File YAML)." },
  { "en": "Apa Itu Kustomize?", "id": "Alat Kustomisasi, Konfigurasi YAML Kubernetes (Template-Free)." },
  { "en": "Apa Itu Container Runtime (Runtime Kontainer)?", "id": "Perangkat Lunak, Eksekusi Kontainer (Docker, containerd)." },
  { "en": "Apa Itu Containerd?", "id": "Runtime Kontainer, Standar Industri (Ringan)." },
  { "en": "Apa Itu CRI-O (Container Runtime Interface - OCI)?", "id": "Runtime Kontainer, Ringan, Khusus Kubernetes." },
  { "en": "Apa Itu OCI (Open Container Initiative)?", "id": "Standar Terbuka, Format Runtime, Kontainer." },
  { "en": "Apa Itu CNI (Container Network Interface)?", "id": "Plugin Jaringan, Kontainer Linux (Standar)." },
  { "en": "Apa Itu CSI (Container Storage Interface)?", "id": "Standar API, Penyimpanan Kontainer (Storage)." },
  { "en": "Apa Itu Dockerfile?", "id": "File Teks, Berisi Instruksi, Membangun Image Docker." },
  { "en": "Apa Itu Image (Citra) Docker?", "id": "Template Read-Only, (Dasar Pembuatan Kontainer)." },
  { "en": "Apa Itu Kontainer (Container) Docker?", "id": "Instansi (Instance) Berjalan, Dari Image Docker." },
  { "en": "Apa Itu Docker Hub?", "id": "Registri Image Docker, Publik (Penyimpanan Image)." },
  { "en": "Apa Itu Registri (Registry) Kontainer?", "id": "Penyimpanan, Manajemen Image Kontainer (Publik/Privat)." },
  { "en": "Apa Itu Docker Compose?", "id": "Alat, Mendefinisikan, Menjalankan Aplikasi Multi-Kontainer." },
  { "en": "Apa Itu Volume (Volume) Docker?", "id": "Mekanisme Penyimpanan, Data Persisten Kontainer." },
  { "en": "Apa Itu Bind Mount (Kait Ikat) Docker?", "id": "Mengaitkan Direktori Host, Ke Kontainer." },
  { "en": "Apa Itu Jaringan (Network) Bridge Docker?", "id": "Jaringan Default, Kontainer Terisolasi (Komunikasi via IP)." },
  { "en": "Apa Itu Jaringan (Network) Host Docker?", "id": "Menghapus Isolasi Jaringan, (Kontainer = Host)." },
  { "en": "Apa Itu Jaringan (Network) None Docker?", "id": "Menonaktifkan Semua Jaringan, Untuk Kontainer." },
  { "en": "Apa Itu Jaringan Overlay (Overlay Network)?", "id": "Jaringan Virtual, Dibangun, Atas Jaringan Lain." },
  { "en": "Apa Itu Tunneling (Penerowongan) Protokol?", "id": "Membungkus Satu Protokol, Dalam Protokol Lain." },
  { "en": "Apa Itu Protokol L2TP (Layer 2 Tunneling Protocol)?", "id": "Protokol Tunneling, VPN (Point-to-Point)." },
  { "en": "Apa Itu Protokol PPTP (Point-to-Point Tunneling Protocol)?", "id": "Protokol Tunneling VPN (Lama, Kurang Aman)." },
  { "en": "Apa Itu IPsec (Internet Protocol Security)?", "id": "Rangkaian Protokol, Mengamankan Jaringan IP (VPN)." },
  { "en": "Correction: The user previously requested full names (kepanjangan) in the question, but IPsec is a combination. I will use the established name.", "en": "Apa Itu IPsec (Internet Protocol Security) Mode Transport?", "id": "Mengenkripsi Payload (Data) Paket IP Saja." },
  { "en": "Apa Itu IPsec (Internet Protocol Security) Mode Tunnel?", "id": "Mengenkripsi Seluruh Paket IP (Header, Payload)." },
  { "en": "Apa Itu IKE (Internet Key Exchange)?", "id": "Protokol, Negosiasi Kunci, Asosiasi Keamanan (IPsec)." },
  { "en": "Apa Itu VPN (Virtual Private Network) Site-to-Site?", "id": "Menghubungkan Dua Jaringan LAN, Lewat Internet." },
  { "en": "Apa Itu VPN (Virtual Private Network) Remote Access?", "id": "Menghubungkan Pengguna Jarak Jauh, Jaringan Privat." },
  { "en": "Apa Itu Diagram Jaringan (Network) Fisik?", "id": "Diagram, Menampilkan Tata Letak, Perangkat Fisik." },
  { "en": "Apa Itu Diagram Jaringan (Network) Logis?", "id": "Diagram, Menampilkan Aliran Data, Alamat IP." },
  { "en": "Apa Itu MIB (Management Information Base)?", "id": "Database Hierarkis, Objek, Dikelola (SNMP)." },
  { "en": "Apa Itu OID (Object Identifier) SNMP?", "id": "Alamat Unik, Mengidentifikasi Objek, Dalam MIB." },
  { "en": "Apa Itu Perintah 'snmpget' (SNMP Get)?", "id": "Perintah, Mengambil Nilai OID, Perangkat Jaringan." },
  { "en": "Apa Itu Perintah 'snmpwalk' (SNMP Walk)?", "id": "Perintah, Mengambil Sekelompok OID, (Jelajahi MIB)." },
  { "en": "Apa Itu Syslog (System Log)?", "id": "Standar Protokol, Pengiriman Pesan Log Jaringan." },
  { "en": "Apa Itu Tingkat Keparahan (Severity Level) Syslog?", "id": "Menunjukkan Tingkat Kepentingan, Pesan Log (0-7)." },
  { "en": "Apa Itu Fasilitas (Facility) Syslog?", "id": "Menunjukkan Sumber Pesan Log (Kernel, Mail)." },
  { "en": "Apa Itu API (Application Programming Interface) Polling?", "id": "Klien, Berulang Kali, Meminta Data Server." },
  { "en": "Apa Itu Webhook (Kait Web)?", "id": "Server, Mendorong Data, Klien (Callback HTTP)." },
  { "en": "Apa Itu HTTP (Hypertext Transfer Protocol) Streaming?", "id": "Koneksi HTTP, Terbuka, Mengirim Data Berkelanjutan." },
  { "en": "Apa Itu Server-Sent Events (SSE)?", "id": "Teknologi, Server, Mengirim Pembaruan Otomatis (HTTP)." },
  { "en": "Apa Itu Pustaka (Library) Pemrograman?", "id": "Kumpulan Kode, Fungsi, Siap Pakai (Dipanggil)." },
  { "en": "Apa Itu Kerangka Kerja (Framework) Pemrograman?", "id": "Struktur, Aturan, Membangun Aplikasi (Mengontrol Alur)." },
  { "en": "Perbedaan Pustaka (Library) Dan Kerangka Kerja (Framework)?", "id": "Anda Memanggil Pustaka, Kerangka Kerja Memanggil Anda." },
  { "en": "Apa Itu Inversion of Control (IoC)?", "id": "Prinsip Desain, (Kerangka Kerja Mengontrol Alur)." },
  { "en": "Apa Itu Dependency Injection (DI)?", "id": "Pola Desain, Menyediakan Dependensi, Objek Eksternal." },
  { "en": "Apa Itu Kontainer IoC (Inversion of Control)?", "id": "Kerangka Kerja, Mengelola Objek, Dependensi (DI)." },
  { "en": "Apa Itu Antarmuka (Interface) Pemrograman?", "id": "Kontrak, Mendefinisikan Metode, (Tanpa Implementasi)." },
  { "en": "Apa Itu Kelas (Class) Abstrak?", "id": "Kelas, Tidak Bisa Di-Instansiasi (Bisa Implementasi)." },
  { "en": "Apa Perbedaan Antarmuka (Interface) Dan Abstrak?", "id": "Antarmuka (Kontrak Murni), Abstrak (Template Dasar)." },
  { "en": "Apa Itu Tipe Data (Data Type) Generik?", "id": "Tipe Parameter, Kelas, Metode (Fleksibel)." },
  { "en": "Apa Itu Metaprogramming (Metapemrograman)?", "id": "Program, Menulis, Memanipulasi Program Lain (Kode)." },
  { "en": "Apa Itu Refleksi (Reflection) Pemrograman?", "id": "Kemampuan Program, Memeriksa, Memodifikasi Strukturnya Sendiri." },
  { "en": "Apa Itu Pustaka Standar (Standard Library)?", "id": "Pustaka, Dibundel, Bahasa Pemrograman (Bawaan)." },
  { "en": "Apa Itu SDK (Software Development Kit) Platform?", "id": "Kumpulan Alat, Membangun Aplikasi, Platform Spesifik." },
  { "en": "Apa Itu Kompilasi JIT (Just-In-Time) HotSpot?", "id": "Kompilasi Runtime, (JVM Menganalisis Kode Panas)." },
  { "en": "Apa Itu Ruang (Space) Heap Generasi JVM?", "id": "Heap Dibagi, (Generasi Muda, Tua)." },
  { "en": "Apa Itu Ruang (Space) Heap Eden JVM?", "id": "Bagian Generasi Muda, (Alokasi Objek Baru)." },
  { "en": "Apa Itu Ruang (Space) Heap Survivor JVM?", "id": "Bagian Generasi Muda, (Objek Bertahan GC)." },
  { "en": "Apa Itu Ruang (Space) Heap Tenured (Old)?", "id": "Generasi Tua, (Objek Bertahan Lama)." },
  { "en": "Apa Itu Pengumpul Sampah (Garbage Collector) Minor?", "id": "Membersihkan Generasi Muda (Young Generation) Heap." },
  { "en": "Apa Itu Pengumpul Sampah (Garbage Collector) Major?", "id": "Membersihkan Generasi Tua (Old Generation) Heap." },
  { "en": "Apa Itu Stop-the-World (Hentikan Dunia) GC?", "id": "Jeda Aplikasi, Saat Pengumpulan Sampah (GC)." },
  { "en": "Apa Itu Pengumpul Sampah (Garbage Collector) G1?", "id": "Pengumpul Sampah (GC), Server-Side, Paralel (Java)." },
  { "en": "Apa Itu Stack (Tumpukan) vs Heap Memori?", "id": "Stack (Fungsi, Primitif), Heap (Objek, Dinamis)." },
  { "en": "Apa Itu Stack Frame (Bingkai Tumpukan)?", "id": "Data Terkait, Panggilan Fungsi (Stack)." },
  { "en": "Apa Itu Error (Kesalahan) Stack Overflow?", "id": "Stack Kehabisan Ruang, (Rekursi Tanpa Henti)." },
  { "en": "Apa Itu Error (Kesalahan) OutOfMemoryError Heap?", "id": "Heap Kehabisan Ruang, (Alokasi Objek Gagal)." },
  { "en": "Apa Itu PermGen (Permanent Generation) JVM?", "id": "Area Heap, Menyimpan Metadata Kelas (Lama)." },
  { "en": "Apa Itu Metaspace (Ruang Meta) JVM?", "id": "Pengganti PermGen, (Memori Native, Metadata Kelas)." },
  { "en": "Apa Itu ClassLoader (Pemuat Kelas) Java?", "id": "Komponen JVM, Memuat Kelas Java (Runtime)." },
  { "en": "Apa Itu Kata Kunci 'volatile' Java?", "id": "Memastikan Visibilitas Perubahan, Antar Thread." },
  { "en": "Apa Itu Kata Kunci 'synchronized' Java?", "id": "Mekanisme Penguncian (Lock), (Metode, Blok) Mutex." },
  { "en": "Apa Itu Masalah Atomisitas (Atomicity)?", "id": "Operasi, Harus Terjadi, Utuh (Tidak Terinterupsi)." },
  { "en": "Apa Itu Masalah Visibilitas (Visibility)?", "id": "Perubahan Thread, Tidak Terlihat Thread Lain." },
  { "en": "Apa Itu Masalah Pengurutan (Ordering)?", "id": "Urutan Eksekusi Instruksi, (Diubah Compiler, CPU)." },
  { "en": "Apa Itu Model Memori (Memory Model) Java?", "id": "Spesifikasi, Interaksi Memori, Antar Thread Java." },
  { "en": "Apa Itu Race Condition (Kondisi Balapan)?", "id": "Hasil Bergantung, Urutan Eksekusi Thread (Bug)." },
  { "en": "Apa Itu Deadlock (Kebuntuan) Thread?", "id": "Dua, Lebih Thread, Saling Menunggu (Kunci)." },
  { "en": "Apa Itu Livelock (Kunci Hidup) Thread?", "id": "Thread Aktif, Gagal Membuat Kemajuan (Sibuk)." },
  { "en": "Apa Itu Starvation (Kelaparan) Thread?", "id": "Thread, Tidak Mendapat Waktu CPU (Prioritas Rendah)." },
  { "en": "Apa Itu Kumpulan Thread (Thread Pool)?", "id": "Kumpulan Thread, Siap Pakai (Mengurangi Overhead)." },
  { "en": "Apa Itu ExecutorService Java?", "id": "API, Mengelola Eksekusi, Kumpulan Thread (ThreadPool)." },
  { "en": "Apa Itu Objek Future (Masa Depan) Java?", "id": "Representasi Hasil, Komputasi Asinkron (Menunggu)." },
  { "en": "Apa Itu Objek CompletableFuture Java?", "id": "Evolusi Future, (API Fungsional, Asinkron)." },
  { "en": "Apa Itu Pemrograman Reaktif (Reactive Programming)?", "id": "Paradigma Pemrograman, Aliran Data Asinkron (Event)." },
  { "en": "Apa Itu Backpressure (Tekanan Balik) Reaktif?", "id": "Mekanisme Kontrol Aliran, (Publisher Lambat)." },
  { "en": "Apa Itu Pustaka (Library) RxJava (ReactiveX)?", "id": "Implementasi Reactive Extensions, Untuk Java (Observable)." },
  { "en": "Apa Itu Project Reactor?", "id": "Kerangka Kerja (Framework) Reaktif, (Spring Framework 5)." },
  { "en": "Apa Itu Tipe (Type) Flux Reactor?", "id": "Publisher Reaktif, (0..N Elemen, Aliran Data)." },
  { "en": "Apa Itu Tipe (Type) Mono Reactor?", "id": "Publisher Reaktif, (0..1 Elemen, Hasil Tunggal)." },
  { "en": "Apa Itu RSocket?", "id": "Protokol Komunikasi Biner, (Aplikasi Reaktif, RPC)." },
  { "en": "Apa Itu WebFlux (Spring WebFlux)?", "id": "Kerangka Kerja (Framework) Web Reaktif, (Spring Boot)." },
  { "en": "Apa Itu Netty?", "id": "Kerangka Kerja (Framework) Jaringan, Asinkron, (Server WebFlux)." },
  { "en": "Apa Itu WebSocket (Soket Web)?", "id": "Protokol Komunikasi, Dua Arah, (Koneksi Tunggal)." },
  { "en": "Apa Itu RDBMS (Relational Database Management System) In-Memory?", "id": "RDBMS, Menyimpan Data, Utama Di RAM (Cepat)." },
  { "en": "Apa Itu H2 (Database)?", "id": "Database SQL, Java (In-Memory, Embedded)." },
  { "en": "Apa Itu SQLite?", "id": "Mesin Database SQL, (Embedded, File Tunggal)." },
  { "en": "Apa Itu ORM (Object-Relational Mapping)?", "id": "Teknik Konversi Data, (Model Objek, Database Relasional)." },
  { "en": "Apa Itu Hibernate (ORM)?", "id": "Kerangka Kerja (Framework) ORM, Populer, Java (JPA)." },
  { "en": "Apa Itu JPA (Java Persistence API)?", "id": "Spesifikasi Java, (Standar ORM, Manajemen Data)." },
  { "en": "Apa Itu JDBC (Java Database Connectivity)?", "id": "API Java, Standar, Koneksi Database SQL." },
  { "en": "Apa Itu Kumpulan Koneksi (Connection Pool) Database?", "id": "Kumpulan Koneksi Database, Siap Pakai (Efisiensi)." },
  { "en": "Apa Itu HikariCP?", "id": "Kumpulan Koneksi (Connection Pool) JDBC, Cepat (Default Spring Boot)." },
  { "en": "Apa Itu MyBatis?", "id": "Kerangka Kerja (Framework) Pemetaan SQL, (Kontrol Penuh SQL)." },
  { "en": "Apa Itu DAO (Data Access Object)?", "id": "Pola Desain, Abstraksi Logika Akses Data." },
  { "en": "Apa Itu DTO (Data Transfer Object)?", "id": "Pola Desain, Objek Transfer Data (Antar Lapisan)." },
  { "en": "Apa Perbedaan DAO (Data Access Object) Dan DTO?", "id": "DAO (Akses Data), DTO (Transfer Data)." },
  { "en": "Apa Itu Pemetaan (Mapping) Properti DTO?", "id": "Proses Menyalin Data, (Entitas Ke DTO)." },
  { "en": "Apa Itu MapStruct?", "id": "Generator Kode, Pemetaan Properti, Java (Cepat)." },
  { "en": "Apa Itu Lombok (Project Lombok)?", "id": "Pustaka Java, Mengurangi Kode Boilerplate (Getter, Setter)." },
  { "en": "Apa Itu Anotasi (Annotation) Java?", "id": "Bentuk Metadata, (Ditambahkan Ke Kode Java)." },
  { "en": "Apa Itu Anotasi (Annotation) @Override?", "id": "Menandakan Metode, Mengganti (Override) Metode Induk." },
  { "en": "Apa Itu Anotasi (Annotation) @Deprecated?", "id": "Menandakan Elemen, (Metode, Kelas) Usang." },
  { "en": "Apa Itu Anotasi (Annotation) @SuppressWarnings?", "id": "Menekan Peringatan (Warning), Kompilasi Spesifik." },
  { "en": "Apa Itu Spring (Kerangka Kerja)?", "id": "Kerangka Kerja (Framework) Aplikasi, Ekosistem Java (IoC, DI)." },
  { "en": "Apa Itu Spring (Kerangka Kerja) Boot?", "id": "Memudahkan Pembuatan Aplikasi, Spring (Konfigurasi Otomatis)." },
  { "en": "Apa Itu Konfigurasi Otomatis (Auto-Configuration) Spring Boot?", "id": "Spring Boot, Otomatis, Konfigurasi Aplikasi (Umum)." },
  { "en": "Apa Itu Spring (Kerangka Kerja) MVC?", "id": "Kerangka Kerja (Framework) Web, Spring (Pola MVC)." },
  { "en": "Apa Itu Spring (Kerangka Kerja) Data?", "id": "Proyek Spring, Menyederhanakan Akses Data (JPA, MongoDB)." },
  { "en": "Apa Itu Spring (Kerangka Kerja) Security?", "id": "Kerangka Kerja (Framework), Otentikasi, Otorisasi (Keamanan)." },
  { "en": "Apa Itu Thymeleaf?", "id": "Mesin Template (Template Engine) Java, (Render HTML Server)." },
  { "en": "Apa Itu JSP (JavaServer Pages)?", "id": "Teknologi Render Tampilan, Server (Lama, HTML+Java)." },
  { "en": "Apa Itu Format File BMP (Bitmap)?", "id": "Format Gambar Raster, Tidak Terkompresi." },
  { "en": "Apa Itu Format File TIFF (Tagged Image File)?", "id": "Format Gambar Fleksibel, Kualitas Tinggi." },
  { "en": "Apa Itu Format File WebP?", "id": "Format Gambar Modern Google (Lossy, Lossless)." },
  { "en": "Apa Itu Format Audio AAC (Advanced Audio Coding)?", "id": "Format Audio Kompresi (Penerus MP3)." },
  { "en": "Apa Itu Format Audio OGG (Ogg Vorbis)?", "id": "Format Audio Kompresi, Terbuka, Gratis." },
  { "en": "Apa Itu Format Video WebM?", "id": "Format Video Terbuka, Gratis (Google)." },
  { "en": "Apa Itu Format Video H.264 (MPEG-4 AVC)?", "id": "Standar Kompresi Video, Sangat Umum." },
  { "en": "Apa Itu Format Video H.265 (HEVC)?", "id": "Standar Kompresi Video, (Penerus H.264, Efisien)." },
  { "en": "Apa Itu Container (Wadah) Multimedia?", "id": "File, Menggabungkan Audio, Video, Metadata." },
  { "en": "Apa Perbedaan Codec Dan Container?", "id": "Codec (Kompresi), Container (Wadah File)." },
  { "en": "Apa Itu Shell (Cangkang) Interaktif?", "id": "Shell, Menerima Input, Langsung Dari Pengguna." },
  { "en": "Apa Itu Shell (Cangkang) Non-Interaktif?", "id": "Shell, Menjalankan Perintah, Dari Skrip (File)." },
  { "en": "Apa Itu File Dot (Dot File) Linux?", "id": "File Konfigurasi, Tersembunyi (Nama Diawali Titik)." },
  { "en": "Apa Perintah 'chmod +x' Linux?", "id": "Memberi Izin Eksekusi (Execute) File." },
  { "en": "Apa Perintah 'who' Linux?", "id": "Menampilkan Daftar Pengguna, Yang Sedang Login." },
  { "en": "Apa Perintah 'w' Linux?", "id": "Menampilkan Pengguna Login, Aktivitasnya (Lebih Detail)." },
  { "en": "Apa Perintah 'less' Linux?", "id": "Utilitas Pager, Menampilkan Teks (Maju, Mundur)." },
  { "en": "Apa Perintah 'more' Linux?", "id": "Utilitas Pager, Menampilkan Teks (Hanya Maju)." },
  { "en": "Apa Perbedaan 'less' Dan 'more'?", "id": "Less Lebih Canggih (Bisa Mundur)." },
  { "en": "Apa Perintah 'which' Linux?", "id": "Menampilkan Lokasi Path, Perintah Eksekutabel." },
  { "en": "Apa Perintah 'whereis' Linux?", "id": "Mencari Lokasi Biner, Sumber, Manual Perintah." },
  { "en": "Apa Perintah 'locate' Linux?", "id": "Mencari File, Cepat (Menggunakan Database)." },
  { "en": "Apa Itu Soft Link (Tautan Lunak) Linux?", "id": "Tautan Simbolik, (Pointer Ke Nama File)." },
  { "en": "Apa Perbedaan Hard Link Dan Soft Link?", "id": "Hard Link (Inode Sama), Soft Link (Pointer)." },
  { "en": "Apa Itu Inode (Indeks Node) Filesystem?", "id": "Struktur Data, Menyimpan Metadata File (Bukan Nama)." },
  { "en": "Apakah Hard Link (Tautan Keras) Berbagi Inode?", "id": "Ya, Hard Link, Menunjuk Inode Sama." },
  { "en": "Apakah Soft Link (Tautan Lunak) Berbagi Inode?", "id": "Tidak, Soft Link, Punya Inode Sendiri." },
  { "en": "Apa Itu Sortir (Sort) Tipe Bubble Sort?", "id": "Algoritma Sortir, Perbandingan Sederhana (Gelembung)." },
  { "en": "Apa Kompleksitas Waktu (Time Complexity) Bubble Sort?", "id": "Kasus Terburuk O(n Kuadrat)." },
  { "en": "Apa Itu Sortir (Sort) Tipe Insertion Sort?", "id": "Algoritma Sortir, Membangun Urutan, Satu Persatu." },
  { "en": "Apa Kompleksitas Waktu (Time Complexity) Insertion Sort?", "id": "Kasus Terburuk O(n Kuadrat)." },
  { "en": "Apa Itu Sortir (Sort) Tipe Selection Sort?", "id": "Algoritma Sortir, Memilih Elemen Minimum, Berulang." },
  { "en": "Apa Kompleksitas Waktu (Time Complexity) Selection Sort?", "id": "Kasus Terburuk O(n Kuadrat)." },
  { "en": "Apa Itu Sortir (Sort) Tipe Merge Sort?", "id": "Algoritma Sortir, Divide and Conquer (Pecah, Gabung)." },
  { "en": "Apa Kompleksitas Waktu (Time Complexity) Merge Sort?", "id": "Kasus Terburuk O(n Log n)." },
  { "en": "Apa Itu Sortir (Sort) Tipe Quick Sort?", "id": "Algoritma Sortir, Divide and Conquer (Pivot)." },
  { "en": "Apa Kompleksitas Waktu (Time Complexity) Quick Sort Terburuk?", "id": "Kasus Terburuk O(n Kuadrat)." },
  { "en": "Apa Itu Pivot (Poros) Quick Sort?", "id": "Elemen, Digunakan Mempartisi Array (Data)." },
  { "en": "Apa Itu Partisi (Partitioning) Quick Sort?", "id": "Proses Mengatur Ulang, Elemen (Sekitar Pivot)." },
  { "en": "Apa Itu Pengurutan (Sorting) Di Tempat (In-Place)?", "id": "Algoritma Sortir, Memori Tambahan Konstan." },
  { "en": "Apa Itu Pengurutan (Sorting) Stabil (Stable)?", "id": "Algoritma Sortir, Menjaga Urutan Elemen Sama." },
  { "en": "Apakah Bubble Sort (Sortir Gelembung) Stabil?", "id": "Ya, Bubble Sort Adalah Stabil." },
  { "en": "Apakah Merge Sort (Sortir Gabung) Stabil?", "id": "Ya, Implementasi Umumnya Stabil." },
  { "en": "Apakah Quick Sort (Sortir Cepat) Stabil?", "id": "Tidak, Implementasi Umumnya Tidak Stabil." },
  { "en": "Apa Itu Tahap (Phase) Lexical Analysis Compiler?", "id": "Tahap Awal, Membaca Kode, Menghasilkan Token." },
  { "en": "Apa Itu Token (Leksem)?", "id": "Unit Sintaksis Terkecil (Keyword, Identifier)." },
  { "en": "Apa Itu Tahap (Phase) Syntax Analysis Compiler?", "id": "Memeriksa Tata Bahasa (Grammar), Membangun Parse Tree." },
  { "en": "Apa Itu Parse Tree (Pohon Urai)?", "id": "Representasi Pohon, Struktur Sintaksis Kode." },
  { "en": "Apa Itu Tahap (Phase) Semantic Analysis Compiler?", "id": "Memeriksa Makna Kode, (Pengecekan Tipe Data)." },
  { "en": "Apa Itu Tahap (Phase) Intermediate Code Generation?", "id": "Menghasilkan Kode, Representasi Antara (Abstrak)." },
  { "en": "Apa Itu Tahap (Phase) Code Optimization Compiler?", "id": "Memperbaiki Kode Antara, (Lebih Cepat, Efisien)." },
  { "en": "Apa Itu Tahap (Phase) Code Generation Compiler?", "id": "Menghasilkan Kode Target, (Bahasa Mesin, Assembly)." },
  { "en": "Apa Itu Symbol Table (Tabel Simbol) Compiler?", "id": "Struktur Data, Menyimpan Informasi, Identifier (Variabel)." },
  { "en": "Apa Itu Loader (Pemuat) Sistem Operasi?", "id": "Program OS, Memuat Kode, Memori (Eksekusi)." },
  { "en": "Apa Itu Linker (Penaut) Sistem Operasi?", "id": "Program, Menggabungkan File Objek, Pustaka (Library)." },
  { "en": "Apa Itu Relokasi (Relocation) Kode?", "id": "Proses Penyesuaian, Alamat Memori Kode (Linker)." },
  { "en": "Siapa Charles Babbage (Bapak Komputer)?", "id": "Matematikawan, Perancang Konsep, Komputer Terprogram." },
  { "en": "Apa Mesin (Machine) Analitis Babbage?", "id": "Desain Komputer Mekanis, Serbaguna (Tidak Terbangun)." },
  { "en": "Apa Mesin (Machine) Selisih Babbage?", "id": "Kalkulator Mekanis, Otomatis, (Menghitung Polinomial)." },
  { "en": "Siapa Grace Hopper (Pionir Pemrograman)?", "id": "Ilmuwan Komputer, (Pengembang Compiler Pertama)." },
  { "en": "Apa Kontribusi (Contribution) Grace Hopper?", "id": "Mempopulerkan Bahasa Pemrograman, (COBOL, Debugging)." },
  { "en": "Siapa John Von Neumann (Arsitek Komputer)?", "id": "Matematikawan, (Kontributor Konsep Arsitektur Komputer)." },
  { "en": "Apa Arsitektur (Architecture) Von Neumann?", "id": "Arsitektur Komputer, (Program, Data, Memori Sama)." },
  { "en": "Siapa Gordon Moore (Hukum Moore)?", "id": "Salah Satu Pendiri Intel, (Formulator Hukum Moore)." },
  { "en": "Apa Isi Hukum Moore (Moore's Law)?", "id": "Jumlah Transistor, Chip, Berlipat Ganda (Tiap Dua Tahun)." },
  { "en": "Siapa Douglas Engelbart (Penemu Mouse)?", "id": "Pionir Interaksi Manusia Komputer, (Penemu Mouse)." },
  { "en": "Apa Itu 'The Mother of All Demos'?", "id": "Demonstrasi Engelbart, (Mouse, GUI, Hypertext)." },
  { "en": "Siapa Vint Cerf (Bapak Internet)?", "id": "Salah Satu, Perancang Protokol TCP/IP." },
  { "en": "Siapa Robert Kahn (Bapak Internet)?", "id": "Salah Satu, Perancang Protokol TCP/IP (Bersama Cerf)." },
  { "en": "Kontribusi (Contribution) Vint Cerf, Robert Kahn?", "id": "Mengembangkan Arsitektur Dasar, Protokol Internet (TCP/IP)." },
  { "en": "Apa Itu Access Point (Titik Akses) Nirkabel?", "id": "Perangkat, Menghubungkan Perangkat Nirkabel, Jaringan Kabel." },
  { "en": "Apa Itu Repeater (Pengulang) Jaringan?", "id": "Perangkat, Memperkuat Sinyal Jaringan (Lapisan Fisik)." },
  { "en": "Apa Itu Bridge (Jembatan) Jaringan?", "id": "Perangkat, Menghubungkan Segmen LAN (Lapisan Data Link)." },
  { "en": "Apa Itu Gateway (Gerbang) Jaringan?", "id": "Node, Penghubung Jaringan, Protokol Berbeda (Router)." },
  { "en": "Apa Perbedaan Switch Dan Hub?", "id": "Switch Cerdas (Alamat MAC), Hub Bodoh (Broadcast)." },
  { "en": "Apa Perbedaan Router Dan Switch?", "id": "Router (Lapisan 3, IP), Switch (Lapisan 2, MAC)." },
  { "en": "Apa Itu Virtualisasi (Virtualization) Penuh?", "id": "Hypervisor, Mensimulasikan Perangkat Keras (Hardware) Penuh." },
  { "en": "Apa Itu Paravirtualization (Paravirtualisasi)?", "id": "OS Tamu (Guest), Dimodifikasi, (Berkomunikasi Hypervisor)." },
  { "en": "Apa Itu Virtualisasi (Virtualization) OS-Level (Kontainer)?", "id": "Virtualisasi, Tingkat OS (Berbagi Kernel Host)." },
  { "en": "Apa Itu Hypervisor Tipe 1 (Bare-Metal)?", "id": "Berjalan Langsung, Di Atas Perangkat Keras (Hardware)." },
  { "en": "Apa Itu Hypervisor Tipe 2 (Hosted)?", "id": "Berjalan, Di Dalam Sistem Operasi Host." },
  { "en": "Contoh (Example) Hypervisor Tipe 1?", "id": "VMware ESXi, Hyper-V, KVM." },
  { "en": "Contoh (Example) Hypervisor Tipe 2?", "id": "VirtualBox, VMware Workstation, Parallels." },
  { "en": "Apa Itu Docker Engine?", "id": "Aplikasi Klien-Server, (Membangun, Menjalankan Kontainer Docker)." },
  { "en": "Apa Itu Lapisan (Layer) Image Docker?", "id": "Image, Terdiri, Lapisan Read-Only (Filesystem)." },
  { "en": "Apa Itu Union File System (UFS)?", "id": "Filesystem, Menggabungkan Lapisan (Image, Kontainer) Docker." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Memori Non-Volatile, (Penyimpanan Elektronik, SSD, USB)." },
  { "en": "Apa Itu Memori Flash (Flash) NAND?", "id": "Tipe Flash, Penyimpanan Data (Kepadatan Tinggi, SSD)." },
  { "en": "Apa Itu Memori Flash (Flash) NOR?", "id": "Tipe Flash, Eksekusi Kode (Akses Cepat, BIOS)." },
  { "en": "Perbedaan (Difference) Flash NAND Dan NOR?", "id": "NAND (Penyimpanan Data), NOR (Eksekusi Kode)." },
  { "en": "Apa Itu Wear Leveling (Perataan Keausan) SSD?", "id": "Teknik SSD, Mendistribusikan Penulisan (Umur Panjang)." },
  { "en": "Apa Itu Perintah (Command) TRIM SSD?", "id": "Perintah OS, Memberitahu SSD, Blok Data Kosong." },
  { "en": "Apa Itu SDRAM (Synchronous Dynamic RAM)?", "id": "DRAM, Sinkron, Clock CPU (Lebih Cepat)." },
  { "en": "Apa Itu FPM (Fast Page Mode) RAM?", "id": "Tipe DRAM, Lama (Akses Halaman Cepat)." },
  { "en": "Apa Itu EDO (Extended Data Out) RAM?", "id": "Tipe DRAM, Lama (Peningkatan FPM RAM)." },
  { "en": "Apa Itu Memori (Memory) ECC (Error-Correcting Code)?", "id": "RAM, Mendeteksi, Memperbaiki Kesalahan Data (Server)." },
  { "en": "Apa Itu Parity Bit (Bit Paritas)?", "id": "Bit Tambahan, Mendeteksi Kesalahan Data (Ganjil/Genap)." },
  { "en": "Apa Itu Gerbang (Gate) Logika Universal?", "id": "Gerbang, Dapat Membangun, Gerbang Logika Lain (NAND, NOR)." },
  { "en": "Apa Itu Resistor (Penghambat)?", "id": "Komponen Elektronik, Menghambat Arus Listrik." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Komponen Elektronik, Menyimpan Muatan Listrik." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen, Mengalirkan Arus, Satu Arah." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Semikonduktor, (Penguat, Saklar)." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda, Memancarkan Cahaya, Saat Dialiri Arus." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit)?", "id": "Chip, Kumpulan Komponen Elektronik Terintegrasi." },
  { "en": "Apa Operator (Operator) Modulo (%)?", "id": "Operator, Menghasilkan Sisa, Hasil Bagi." },
  { "en": "Apa Itu Faktorial (Factorial) (N!)?", "id": "Hasil Perkalian, Bilangan Bulat Positif (1-N)." },
  { "en": "Apa Itu Eksponen (Exponent)?", "id": "Angka, Menunjukkan Operasi Pangkat (Perkalian Berulang)." },
  { "en": "Apa Itu Operator Bitwise AND (&)?", "id": "Operasi Bit, Hasil Satu, Jika Kedua Bit Satu." },
  { "en": "Apa Itu Operator Bitwise OR (|)?", "id": "Operasi Bit, Hasil Satu, Jika Salah Satu Bit Satu." },
  { "en": "Apa Itu Operator Bitwise XOR (^)?", "id": "Operasi Bit, Hasil Satu, Jika Bit Berbeda." },
  { "en": "Apa Itu Operator Bitwise NOT (~)?", "id": "Operasi Bit, Membalik Nilai Bit (Inversi)." },
  { "en": "Apa Itu Operator Bitwise Left Shift (<<)?", "id": "Operasi Bit, Menggeser Bit Ke Kiri." },
  { "en": "Apa Itu Operator Bitwise Right Shift (>>)?", "id": "Operasi Bit, Menggeser Bit Ke Kanan." },
  { "en": "Apa Itu Tipe Data (Data Type) Primitif?", "id": "Tipe Data Dasar, Bawaan Bahasa (Integer)." },
  { "en": "Apa Itu Tipe Data (Data Type) Komposit?", "id": "Tipe Data, Dibangun, Tipe Data Lain (Array)." },
  { "en": "Apa Itu Konversi Tipe (Type Conversion) Implisit?", "id": "Konversi Tipe Data, Otomatis Oleh Compiler." },
  { "en": "Apa Itu Konversi Tipe (Type Conversion) Eksplisit?", "id": "Konversi Tipe Data, Manual Oleh Programer." },
  { "en": "Apa Itu Type Casting (Pengecoran Tipe)?", "id": "Bentuk Konversi Tipe Eksplisit (Paksa)." },
  { "en": "Apa Itu Literal (Literal) Pemrograman?", "id": "Notasi, Representasi Nilai Tetap, Kode Sumber." },
  { "en": "Apa Itu Ekspresi (Expression) Pemrograman?", "id": "Kombinasi Nilai, Variabel, Operator (Menghasilkan Nilai)." },
  { "en": "Apa Itu Pernyataan (Statement) Pemrograman?", "id": "Instruksi Perintah, Melakukan Aksi (Tindakan)." },
  { "en": "Apa Itu Identifier (Pengenal)?", "id": "Nama Unik, Diberikan Entitas (Variabel, Fungsi)." },
  { "en": "Apa Itu Fragmen (Fragment) URL (#)?", "id": "Bagian URL, Menunjuk Elemen, Halaman (Anchor)." },
  { "en": "Apa Itu Jangkar (Anchor) Tag HTML?", "id": "Tag <a>, Membuat Hyperlink (Tautan)." },
  { "en": "Apa Itu Slug (Siput) URL?", "id": "Bagian URL, Identifikasi Halaman, (Mudah Dibaca)." },
  { "en": "Apa Itu Transmisi (Transmission) Simplex?", "id": "Komunikasi Data, Satu Arah Saja (Radio)." },
  { "en": "Apa Itu Transmisi (Transmission) Half-Duplex?", "id": "Komunikasi Data, Dua Arah, Bergantian (Walkie-Talkie)." },
  { "en": "Apa Itu Transmisi (Transmission) Full-Duplex?", "id": "Komunikasi Data, Dua Arah, Bersamaan (Telepon)." },
  { "en": "Apa Itu FPU (Floating Point Unit)?", "id": "Bagian CPU, Khusus, Operasi Bilangan Pecahan." },
  { "en": "Apa Itu MMU (Memory Management Unit)?", "id": "Komponen Hardware, Mengelola Alamat Memori Virtual." },
  { "en": "Apa Itu Patch Panel (Panel Tambalan)?", "id": "Panel Koneksi Kabel, Manajemen Jaringan Terstruktur." },
  { "en": "Apa Itu Network Tap (Sadapan Jaringan)?", "id": "Perangkat Keras, Menyalin (Monitor) Trafik Jaringan." },
  { "en": "Apa Itu Keystone Jack?", "id": "Konektor Modular, Dipasang, Dinding, Patch Panel." },
  { "en": "Apa Itu Format (Format) File INI?", "id": "File Konfigurasi Teks, (Struktur Key-Value, Sederhana)." },
  { "en": "Apa Itu Pengkodean (Encoding) Base64?", "id": "Metode Pengkodean, Biner Ke Teks (ASCII)." },
  { "en": "Apa Perintah 'xargs' Linux?", "id": "Membangun, Eksekusi Perintah, (Dari Standard Input)." },
  { "en": "Apa Perintah 'tee' Linux?", "id": "Membaca Input, Menulis Ke Output, File." },
  { "en": "Apa Perintah 'nice' Linux?", "id": "Menjalankan Program, Prioritas Penjadwalan Berbeda." },
  { "en": "Apa Perintah 'renice' Linux?", "id": "Mengubah Prioritas, Proses Yang Sedang Berjalan." },
  { "en": "Apa Perintah 'killall' Linux?", "id": "Mengirim Sinyal, Proses, Berdasarkan Nama Program." },
  { "en": "Apa Perintah 'iostat' (Input/Output Statistics) Linux?", "id": "Memantau Statistik, I/O Perangkat Disk, CPU." },
  { "en": "Apa Perintah 'vmstat' (Virtual Memory Statistics) Linux?", "id": "Melaporkan Statistik, Memori Virtual, Proses, CPU." },
  { "en": "Apa Perintah 'mpstat' (Multiprocessor Statistics) Linux?", "id": "Melaporkan Statistik, Aktivitas, Setiap Prosesor (CPU)." },
  { "en": "Apa Itu Properti (Property) Liveness Konkurensi?", "id": "Sistem, Akhirnya, Membuat Kemajuan (Tidak Macet)." },
  { "en": "Apa Itu Properti (Property) Safety Konkurensi?", "id": "Sistem, Tidak Pernah, Memasuki Keadaan Buruk." },
  { "en": "Apa Itu Checksum (Jumlah Periksa)?", "id": "Nilai (Hash) Kecil, Deteksi Kesalahan Data." },
  { "en": "Apa Itu Algoritma (Algorithm) CRC (Cyclic Redundancy Check)?", "id": "Fungsi Hash, Deteksi Kesalahan (Umum Jaringan)." },
  { "en": "Apa Itu MTU (Maximum Transmission Unit)?", "id": "Ukuran Paket Data, Terbesar, Dapat Dikirim Jaringan." },
  { "en": "Apa Itu Fragmentasi (Fragmentation) IP?", "id": "Proses Memecah, Paket Data IP (Sesuaikan MTU)." },
  { "en": "Apa Itu Path MTU Discovery (PMTUD)?", "id": "Teknik, Menentukan MTU Terkecil, Jalur Jaringan." },
  { "en": "Apa Itu Virtualisasi (Virtualization) Penyimpanan?", "id": "Menggabungkan Penyimpanan Fisik, (Pool Penyimpanan Virtual)." },
  { "en": "Apa Itu API (Application Programming Interface) VMware vSphere?", "id": "Platform Virtualisasi Server, (Manajemen Terpusat)." },
  { "en": "Apa Itu Microsoft Hyper-V?", "id": "Teknologi Hypervisor, Virtualisasi Bawaan Windows." },
  { "en": "Apa Itu KVM (Kernel-based Virtual Machine)?", "id": "Solusi Virtualisasi, Tipe 1 (Bare-Metal) Linux." },
  { "en": "Apa Itu Xen?", "id": "Hypervisor Tipe 1 (Bare-Metal) Open-Source." },
  { "en": "Apa Itu Migrasi (Migration) Langsung VM (Live)?", "id": "Memindahkan Virtual Machine, Antar Host Fisik (Tanpa Downtime)." },
  { "en": "Apa Itu Virtual Machine (VM) Guest Additions?", "id": "Driver, Alat Bantu, (Meningkatkan Kinerja, Integrasi VM)." },
  { "en": "Apa Itu Virtual Appliance (Peranti Virtual)?", "id": "File Virtual Machine, (Pra-Konfigurasi, Aplikasi)." },
  { "en": "Apa Itu Virtual Desktop Infrastructure (VDI)?", "id": "Hosting, Sistem Operasi Desktop, Server Terpusat." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital, Menjadi Sinyal Analog." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog, Menjadi Sinyal Digital." },
  { "en": "Apa Itu Sinyal (Signal) Analog?", "id": "Sinyal Kontinu, (Representasi Gelombang, Suara)." },
  { "en": "Apa Itu Sinyal (Signal) Digital?", "id": "Sinyal Diskrit, (Representasi Nilai Biner, 0/1)." },
  { "en": "Apa Itu Sampling (Pencuplikan) Sinyal?", "id": "Proses Mengukur, Sinyal Analog, Interval (ADC)." },
  { "en": "Apa Itu Kuantisasi (Quantization) Sinyal?", "id": "Proses Pemetaan, Nilai Sampel Analog, Diskrit." },
  { "en": "Apa Itu Teorema (Theorem) Nyquist?", "id": "Frekuensi Sampling, (Minimal Dua Kali, Frekuensi Maksimal)." },
  { "en": "Apa Itu Aliasing (Aliasing) Sinyal?", "id": "Distorsi Sinyal, (Frekuensi Sampling Terlalu Rendah)." },
  { "en": "Apa Itu Modem (Modulator-Demodulator)?", "id": "Mengubah Sinyal Digital, Analog (Dan Sebaliknya)." },
  { "en": "Apa Itu DSL (Digital Subscriber Line)?", "id": "Teknologi Internet, (Menggunakan Jalur Telepon Tembaga)." },
  { "en": "Apa Itu ADSL (Asymmetric Digital Subscriber Line)?", "id": "DSL, Kecepatan Unduh, Unggah Berbeda." },
  { "en": "Apa Itu Kabel (Cable) Modem?", "id": "Modem, Menggunakan Jaringan, Televisi Kabel (Coaxial)." },
  { "en": "Apa Itu FTTH (Fiber to the Home)?", "id": "Internet, Kabel Fiber Optik, Langsung Ke Rumah." },
  { "en": "Apa Itu PON (Passive Optical Network)?", "id": "Jaringan Fiber Optik, (Point-to-Multipoint, Pasif)." },
  { "en": "Apa Itu OLT (Optical Line Terminal)?", "id": "Perangkat Pusat, Jaringan PON (Sisi Provider)." },
  { "en": "Apa Itu ONT (Optical Network Terminal)?", "id": "Perangkat Klien, Jaringan PON (Sisi Pelanggan)." },
  { "en": "Apa Itu Jaringan Seluler (Cellular Network) 1G?", "id": "Generasi Pertama, Teknologi Seluler Analog (Suara)." },
  { "en": "Apa Itu Jaringan Seluler (Cellular Network) 2G?", "id": "Generasi Kedua, Seluler Digital (Suara, SMS)." },
  { "en": "Apa Itu Jaringan Seluler (Cellular Network) 3G?", "id": "Generasi Ketiga, Seluler (Internet Mobile, Data)." },
  { "en": "Apa Itu Jaringan Seluler (Cellular Network) 4G?", "id": "Generasi Keempat, Seluler (Internet Cepat, LTE)." },
  { "en": "Apa Itu Jaringan Seluler (Cellular Network) 5G?", "id": "Generasi Kelima, Seluler (Sangat Cepat, Latensi Rendah)." },
  { "en": "Apa Itu LTE (Long Term Evolution)?", "id": "Standar Komunikasi, Nirkabel Kecepatan Tinggi (4G)." },
  { "en": "Apa Itu GSM (Global System for Mobile)?", "id": "Standar Digital, Jaringan Seluler (Umum, 2G/3G)." },
  { "en": "Apa Itu CDMA (Code Division Multiple Access)?", "id": "Metode Akses Kanal, (Teknologi Seluler 2G/3G)." },
  { "en": "Apa Itu SIM (Subscriber Identity Module) Card?", "id": "Kartu Pintar, Identifikasi Pengguna, Jaringan GSM." },
  { "en": "Apa Itu IMEI (International Mobile Equipment Identity)?", "id": "Nomor Unik, Identifikasi Perangkat Seluler (Hardware)." },
  { "en": "Apa Itu Roaming (Jelajah) Seluler?", "id": "Menggunakan Layanan Seluler, Di Luar Jaringan Asal." },
  { "en": "Apa Itu Bluetooth (Teknologi Nirkabel)?", "id": "Standar Nirkabel, Jarak Dekat (Perangkat Periferal)." },
  { "en": "Apa Itu NFC (Near Field Communication)?", "id": "Teknologi Nirkabel, Jarak Sangat Dekat (Pembayaran)." },
  { "en": "Apa Itu RFID (Radio Frequency Identification)?", "id": "Teknologi Identifikasi, Menggunakan Gelombang Radio (Tag)." },
  { "en": "Apa Itu Tag (Tanda) RFID Pasif?", "id": "Tag RFID, Tanpa Baterai (Daya Dari Pembaca)." },
  { "en": "Apa Itu Tag (Tanda) RFID Aktif?", "id": "Tag RFID, Memiliki Baterai (Memancarkan Sinyal)." },
  { "en": "Apa Itu WiMAX (Worldwide Interoperability for Microwave Access)?", "id": "Standar Komunikasi, Nirkabel Jarak Jauh (IEEE 802.16)." },
  { "en": "Apa Itu Jaringan Mesh (Mesh Network)?", "id": "Topologi Jaringan, Node Saling Terhubung (Reliabel)." },
  { "en": "Apa Itu Zigbee?", "id": "Protokol Jaringan, Nirkabel, Daya Rendah (IoT, Mesh)." },
  { "en": "Apa Itu Z-Wave?", "id": "Protokol Nirkabel, Daya Rendah (Otomatisasi Rumah, Mesh)." },
  { "en": "Apa Itu LoRaWAN (Long Range Wide Area Network)?", "id": "Protokol Jaringan, Jarak Jauh, Daya Rendah (IoT)." },
  { "en": "Apa Itu Firmware (Firmware) Over-the-Air (OTA)?", "id": "Pembaruan Firmware, Nirkabel (Jarak Jauh, Internet)." },
  { "en": "Apa Itu XaaS (Anything as a Service)?", "id": "Model Layanan Awan, Mencakup Semua." },
  { "en": "Apa Itu BPM (Business Process Management)?", "id": "Pendekatan Manajemen, Proses Bisnis Sistematis." },
  { "en": "Apa Itu BPMN (Business Process Model and Notation)?", "id": "Standar Grafis, Pemodelan Proses Bisnis." },
  { "en": "Apa Itu SOA (Service-Oriented Architecture)?", "id": "Arsitektur, Layanan Perangkat Lunak Terdistribusi." },
  { "en": "Apa Itu ESB (Enterprise Service Bus)?", "id": "Pola Middleware, Integrasi Aplikasi (SOA)." },
  { "en": "Apa Perbedaan (Difference) SOA Dan Microservice?", "id": "SOA (Monolitik Terdistribusi), Microservice (Independen)." },
  { "en": "Apa Itu Pola (Pattern) API Gateway?", "id": "Titik Masuk Tunggal, (Semua Permintaan API)." },
  { "en": "Apa Itu Pola (Pattern) Strangler Fig?", "id": "Pola Migrasi, (Menggantikan Sistem Lama Bertahap)." },
  { "en": "Apa Itu Pola (Pattern) Sidecar?", "id": "Komponen Tambahan, (Terkait Kontainer Utama, Pod)." },
  { "en": "Apa Itu Pola (Pattern) Ambassador?", "id": "Proxy Lokal, (Mewakili Aplikasi, Komunikasi Jaringan)." },
  { "en": "Apa Itu Pola (Pattern) Anti-Corruption Layer (ACL)?", "id": "Lapisan, Isolasi, Antara Sistem Berbeda (DDD)." },
  { "en": "Apa Itu Event (Peristiwa) Storming?", "id": "Sesi Kolaboratif, (Menemukan Domain Bisnis, DDD)." },
  { "en": "Apa Itu Aksen (Accent) Warna UI?", "id": "Warna Sekunder, (Menyoroti Elemen Interaktif)." },
  { "en": "Apa Itu Ruang Putih (White Space) UI?", "id": "Ruang Kosong, Antara Elemen Desain (Bernapas)." },
  { "en": "Apa Itu Tipografi (Typography) UI?", "id": "Seni Mengatur, Tampilan Teks (Font, Ukuran)." },
  { "en": "Apa Itu Font (Font) Serif?", "id": "Font, Garis Kecil, Ujung Karakter (Kait)." },
  { "en": "Apa Itu Font (Font) Sans-Serif?", "id": "Font, Tanpa Garis Kecil (Kait), Modern." },
  { "en": "Apa Itu Keterbacaan (Readability) Teks?", "id": "Kemudahan Pembaca, Memahami Teks Tertulis." },
  { "en": "Apa Itu Keterbacaan (Legibility) Teks?", "id": "Kemudahan Membedakan, Karakter Huruf Individual." },
  { "en": "Apa Itu Hukum Fitts (Fitts's Law) UX?", "id": "Waktu Mencapai Target, (Fungsi Jarak, Ukuran)." },
  { "en": "Apa Itu Hukum Hick (Hick's Law) UX?", "id": "Waktu Memutuskan, Bertambah (Jumlah Pilihan)." },
  { "en": "Apa Itu Hukum Jakob (Jakob's Law) UX?", "id": "Pengguna Berharap, Situs Berfungsi (Sama)." },
  { "en": "Apa Itu Hukum Miller (Miller's Law) UX?", "id": "Manusia Mengingat, Sekitar Tujuh Item (Memori)." },
  { "en": "Apa Itu Hukum Tesler (Tesler's Law) UX?", "id": "Konservasi Kompleksitas, (Kompleksitas Tidak Hilang)." },
  { "en": "Apa Itu Efek (Effect) Von Restorff UX?", "id": "Item Berbeda, Lebih Mudah Diingat (Isolasi)." },
  { "en": "Apa Itu Efek (Effect) Zeigarnik UX?", "id": "Tugas Belum Selesai, Lebih Diingat (Interupsi)." },
  { "en": "Apa Itu Desain (Design) Skeuomorphism?", "id": "Desain Meniru, Objek Dunia Nyata (Realistis)." },
  { "en": "Apa Itu Desain (Design) Datar (Flat Design)?", "id": "Desain Minimalis, Dua Dimensi (Sederhana)." },
  { "en": "Apa Itu Desain (Design) Material (Material Design)?", "id": "Bahasa Desain Google, (Berbasis Material, Fisik)." },
  { "en": "Apa Itu Gradien (Gradient) Warna UI?", "id": "Transisi Halus, Antara Dua Warna, Lebih." },
  { "en": "Apa Itu A11Y (Accessibility) Desain Web?", "id": "Aksesibilitas, (Desain Inklusif, Semua Pengguna)." },
  { "en": "Apa Itu Kontras (Contrast) Warna WCAG?", "id": "Rasio Keterbacaan, Teks Terhadap Latar Belakang." },
  { "en": "Apa Itu ARIA (Accessible Rich Internet Applications)?", "id": "Atribut HTML, (Meningkatkan Aksesibilitas Aplikasi Web)." },
  { "en": "Apa Itu Sistem (System) Desain (Design System)?", "id": "Kumpulan Komponen, Standar, Panduan Desain." },
  { "en": "Apa Itu Pustaka (Library) Pola UI?", "id": "Kumpulan Komponen, UI (Siap Pakai, Dapat Digunakan Ulang)." },
  { "en": "Apa Itu Storybook (Alat UI)?", "id": "Alat Pengembangan, Komponen UI (Terisolasi)." },
  { "en": "Apa Itu Figma?", "id": "Alat Desain, Antarmuka (UI, UX) Kolaboratif." },
  { "en": "Apa Itu Sketch (Alat Desain)?", "id": "Editor Grafis Vektor, Desain Digital (MacOS)." },
  { "en": "Apa Itu Adobe XD?", "id": "Alat Desain, UX/UI (Adobe, Vektor)." },
  { "en": "Apa Itu InVision (Alat Prototipe)?", "id": "Platform Prototipe, Desain Produk Digital." },
  { "en": "Apa Itu Diagram Afinitas (Affinity Diagram)?", "id": "Mengelompokkan Ide, (Data Riset UX, Brainstorming)." },
  { "en": "Apa Itu Tinjauan Heuristik (Heuristic Evaluation)?", "id": "Evaluasi Usability, (Menggunakan Prinsip Heuristik)." },
  { "en": "Apa Itu Riset (Research) Etnografi UX?", "id": "Mempelajari Pengguna, Lingkungan Alami Mereka." },
  { "en": "Apa Itu Wawancara (Interview) Kontekstual UX?", "id": "Wawancara Pengguna, Sambil Mengamati Tugas (Konteks)." },
  { "en": "Apa Itu Skala (Scale) Likert?", "id": "Skala Psikometrik, (Mengukur Sikap, Pendapat)." },
  { "en": "Apa Itu Net Promoter Score (NPS)?", "id": "Metrik Loyalitas Pelanggan, (Seberapa Merekomendasikan)." },
  { "en": "Apa Itu Pengujian (Testing) Pohon (Tree Testing)?", "id": "Evaluasi Usability, (Struktur Navigasi, Arsitektur Informasi)." },
  { "en": "Apa Itu Arsitektur Informasi (IA)?", "id": "Organisasi, Struktur, Pelabelan Konten (Situs Web)." },
  { "en": "Apa Itu Taksonomi (Taxonomy) IA?", "id": "Klasifikasi, Hierarki, Informasi (Struktur Kategori)." },
  { "en": "Apa Itu Folksonomi (Folksonomy) IA?", "id": "Klasifikasi Kolaboratif, (Menggunakan Tag, Pengguna)." },
  { "en": "Apa Itu SEO (Search Engine Optimization) Teknis?", "id": "Aspek Teknis, (Crawlability, Indexing, Kecepatan Situs)." },
  { "en": "Apa Itu File 'robots.txt'?", "id": "File Teks, (Instruksi Crawler Mesin Pencari)." },
  { "en": "Apa Itu Peta Situs (Sitemap) XML?", "id": "File XML, (Daftar URL Situs, (Membantu Indeks))." },
  { "en": "Apa Itu Data (Data) Terstruktur (Schema Markup)?", "id": "Kode, (Memberi Konteks, Mesin Pencari)." },
  { "en": "Apa Itu Core Web Vitals?", "id": "Metrik Google, (Pengalaman Pengguna Halaman Web)." },
  { "en": "Apa Itu LCP (Largest Contentful Paint)?", "id": "Metrik Core Web Vital, (Waktu Render Konten Terbesar)." },
  { "en": "Apa Itu FID (First Input Delay)?", "id": "Metrik Core Web Vital, (Waktu Respon Interaksi Pertama)." },
  { "en": "Apa Itu CLS (Cumulative Layout Shift)?", "id": "Metrik Core Web Vital, (Stabilitas Visual Halaman)." },
  { "en": "Apa Itu Hreflang (Atribut Tautan)?", "id": "Atribut HTML, (Menentukan Bahasa, Target Geografis)." },
  { "en": "Apa Itu Tautan (Link) Kanonikal (Canonical)?", "id": "Tag HTML, (Menentukan Versi URL, Pilihan Utama)." },
  { "en": "Apa Itu Tautan (Link) Nofollow?", "id": "Atribut Tautan, (Mencegah Mesin Pencari, Mengikuti Tautan)." },
  { "en": "Apa Itu Otoritas (Authority) Domain?", "id": "Skor Peringkat, (Memprediksi Peringkat Situs Web)." },
  { "en": "Apa Itu Otoritas (Authority) Halaman?", "id": "Skor Peringkat, (Memprediksi Peringkat Halaman Spesifik)." },
  { "en": "Apa Itu SERP (Search Engine Results Page) Snippet?", "id": "Cuplikan Hasil Pencarian, (Judul, Deskripsi, URL)." },
  { "en": "Apa Itu SERP (Search Engine Results Page) Snippet Kaya?", "id": "Cuplikan Hasil, (Informasi Tambahan, Rating, Gambar)." },
  { "en": "Apa Itu Algoritma (Algorithm) Google Panda?", "id": "Algoritma Peringkat, (Menurunkan Peringkat, Konten Berkualitas Rendah)." },
  { "en": "Apa Itu Algoritma (Algorithm) Google Penguin?", "id": "Algoritma Peringkat, (Menurunkan Peringkat, Tautan Spam)." },
  { "en": "Apa Itu Algoritma (Algorithm) Google Hummingbird?", "id": "Algoritma Peringkat, (Fokus Pencarian Semantik, Maksud)." },
  { "en": "Apa Itu Algoritma (Algorithm) Google RankBrain?", "id": "Algoritma Peringkat, (Pembelajaran Mesin, AI, Kueri)." },
  { "en": "Apa Itu Google E-A-T (Expertise, Authoritativeness, Trustworthiness)?", "id": "Pedoman Kualitas Konten, (Keahlian, Otoritas, Kepercayaan)." },
  { "en": "Apa Itu Indeks (Index) Mobile-First Google?", "id": "Google, Menggunakan Versi Seluler, (Indexing, Peringkat)." },
  { "en": "Apa Itu Kata Kunci (Keyword) Ekor Panjang?", "id": "Frasa Kata Kunci, (Lebih Spesifik, Volume Rendah)." },
  { "en": "Apa Itu Isian (Stuffing) Kata Kunci?", "id": "Praktik SEO Buruk, (Mengisi Kata Kunci Berlebihan)." },
  { "en": "Apa Itu Teks (Text) Jangkar (Anchor Text)?", "id": "Teks Klik, Tautan Hiperteks (Hyperlink)." },
  { "en": "Apa Itu Tautan (Link) Internal?", "id": "Tautan Hiperteks, (Antar Halaman, Satu Domain)." },
  { "en": "Apa Itu Tautan (Link) Eksternal?", "id": "Tautan Hiperteks, (Antar Halaman, Domain Berbeda)." },
  { "en": "Apa Itu Tautan (Link) Balik (Backlink)?", "id": "Tautan Masuk, (Situs Lain, Ke Situs Anda)." },
  { "en": "Apa Itu Tautan (Link) Toksik (Toxic)?", "id": "Tautan Balik, (Situs Spam, Berkualitas Rendah)." },
  { "en": "Apa Itu Alat (Tool) Google Search Console?", "id": "Layanan Google, (Memantau Kinerja Situs, Pencarian)." },
  { "en": "Apa Itu Alat (Tool) Google Analytics?", "id": "Layanan Google, (Melacak, Menganalisis Lalu Lintas Situs)." },
  { "en": "Apa Itu Metrik (Metric) Bounce Rate (Rasio Pentalan)?", "id": "Persentase Pengunjung, (Meninggalkan Situs, Satu Halaman)." },
  { "en": "Apa Itu Metrik (Metric) Sesi (Session)?", "id": "Periode Interaksi, Pengguna, Situs Web Anda." },
  { "en": "Apa Itu Metrik (Metric) Pengguna (User)?", "id": "Pengunjung Unik, Situs Web Anda, (Periode Waktu)." },
  { "en": "Apa Itu Metrik (Metric) Tayangan Halaman (Pageview)?", "id": "Total Jumlah Halaman, Yang Dilihat Pengguna." },
  { "en": "Apa Itu UTM (Urchin Tracking Module) Parameter?", "id": "Tag Kustom, (Ditambahkan Ke URL, Melacak Sumber)." },
  { "en": "Apa Itu CRM (Customer Relationship Management) Salesforce?", "id": "Platform CRM, (Manajemen Penjualan, Layanan, Pemasaran)." },
  { "en": "Apa Itu SAP (Systems, Applications, and Products)?", "id": "Perangkat Lunak ERP, (Manajemen Sumber Daya Perusahaan)." },
  { "en": "Apa Itu Oracle (Database)?", "id": "Sistem Manajemen, Basis Data Relasional (RDBMS)." },
  { "en": "Apa Itu Microsoft SQL (Structured Query Language) Server?", "id": "RDBMS, Dikembangkan Oleh Microsoft (Windows)." },
  { "en": "Apa Itu PostgreSQL?", "id": "Sistem RDBMS, Open-Source (Objek-Relasional)." },
  { "en": "Apa Itu MySQL?", "id": "Sistem RDBMS, Open-Source (Populer, Web)." },
  { "en": "Apa Itu MariaDB?", "id": "Cabang (Fork) MySQL, (Dibuat Komunitas, Open-Source)." },
  { "en": "Apa Perbedaan (Difference) MySQL Dan MariaDB?", "id": "MariaDB (Fitur Baru, Lisensi Beda), MySQL (Oracle)." },
  { "en": "Apa Itu Mesin (Engine) Penyimpanan InnoDB?", "id": "Mesin Penyimpanan, MySQL, MariaDB (Default, ACID)." },
  { "en": "Apa Itu Mesin (Engine) Penyimpanan MyISAM?", "id": "Mesin Penyimpanan, MySQL (Lama, Non-Transaksional)." },
  { "en": "Apa Itu Server (Server) Basis Data?", "id": "Sistem Komputer, (Menyediakan Layanan Basis Data)." },
  { "en": "Apa Itu Klien (Client) Basis Data?", "id": "Aplikasi, (Mengakses Layanan, Server Basis Data)." },
  { "en": "Apa Itu Skalabilitas (Scalability) Baca (Read) Database?", "id": "Meningkatkan Kapasitas, (Menangani Banyak Operasi Baca)." },
  { "en": "Apa Itu Replikasi (Replication) Baca (Read)?", "id": "Menyalin Data, (Master Ke Banyak Slave/Replika)." },
  { "en": "Apa Itu Skalabilitas (Scalability) Tulis (Write) Database?", "id": "Meningkatkan Kapasitas, (Menangani Banyak Operasi Tulis)." },
  { "en": "Apa Itu Sharding (Belah) Database?", "id": "Partisi Data, Horizontal, Antar Server Database." },
  { "en": "Apa Itu Kunci (Key) Sharding?", "id": "Kolom, Menentukan Distribusi Data Shard." },
  { "en": "Apa Itu Sistem (System) File FAT32?", "id": "Sistem Berkas (Lama), Batas Ukuran File 4GB." },
  { "en": "Apa Itu Sistem (System) File exFAT?", "id": "Sistem Berkas (Flash Drive), (Mengatasi Batas FAT32)." },
  { "en": "Apa Itu Sistem (System) File APFS (Apple File System)?", "id": "Sistem Berkas Modern, Apple (SSD, Enkripsi)." },
  { "en": "Apa Itu Partisi (Partition) Boot?", "id": "Partisi Disk, (Berisi File Bootloader, Kernel)." },
  { "en": "Apa Itu Partisi (Partition) Swap?", "id": "Partisi Disk, (Digunakan Sebagai Memori Virtual)." },
  { "en": "Apa Itu Partisi (Partition) Root?", "id": "Partisi Utama, (Sistem Operasi, File Sistem)." },
  { "en": "Apa Itu Perintah 'fdisk' Linux?", "id": "Utilitas Baris Perintah, (Manajemen Tabel Partisi MBR)." },
  { "en": "Apa Itu Perintah 'gdisk' Linux?", "id": "Utilitas Baris Perintah, (Manajemen Tabel Partisi GPT)." },
  { "en": "Apa Itu Perintah 'parted' Linux?", "id": "Utilitas Baris Perintah, (Manajemen Partisi Disk)." },
  { "en": "Apa Itu Perintah 'mkfs' (Make Filesystem)?", "id": "Perintah Linux, (Membuat Sistem Berkas Baru)." },
  { "en": "Apa Itu Perintah 'fsck' (File System Check)?", "id": "Perintah Linux, (Memeriksa, Memperbaiki Konsistensi Filesystem)." },
  { "en": "Apa Itu Jurnaling (Journaling) Filesystem?", "id": "Mencatat Perubahan (Log), (Mencegah Korupsi Data)." },
  { "en": "Apa Itu UUID (Universally Unique Identifier) Partisi?", "id": "Identifikator Unik, (Global, Untuk Partisi Disk)." },
  { "en": "Apa Itu GUI (Graphical User Interface) Toolkit?", "id": "Pustaka Komponen Grafis, (Membangun Antarmuka GUI)." },
  { "en": "Contoh (Example) GUI Toolkit?", "id": "GTK (GNOME), Qt (KDE), Swing (Java)." },
  { "en": "Apa Itu Manajer (Manager) Tampilan (Display) Linux?", "id": "Program Grafis, (Menangani Login Pengguna, Sesi)." },
  { "en": "Contoh (Example) Manajer Tampilan?", "id": "GDM (GNOME), SDDM (KDE), LightDM." },
  { "en": "Apa Itu Widget (Widžet) GUI?", "id": "Elemen Kontrol Grafis, (Tombol, Checkbox, Slider)." },
  { "en": "Apa Itu Penangan (Handler) Peristiwa (Event) GUI?", "id": "Fungsi (Callback), (Merespons Aksi Pengguna GUI)." },
  { "en": "Apa Itu Pustaka (Library) SDL (Simple DirectMedia Layer)?", "id": "Pustaka Multimedia, (Akses Audio, Grafis, Input)." },
  { "en": "Apa Itu OpenGL (Open Graphics Library)?", "id": "API Lintas Platform, (Grafis Komputer 2D, 3D)." },
  { "en": "Apa Itu Vulkan (API Grafis)?", "id": "API Grafis, (Modern, Kinerja Tinggi, Overhead Rendah)." },
  { "en": "Apa Itu Microsoft DirectX?", "id": "Kumpulan API Microsoft, (Multimedia, Game, Windows)." },
  { "en": "Apa Itu Shader (Penau) Grafis?", "id": "Program Kecil, (Berjalan Di GPU, Mengontrol Render)." },
  { "en": "Apa Itu Vertex (Simpul) Shader?", "id": "Shader, (Memproses Titik Vertex, (Posisi, Transformasi))." },
  { "en": "Apa Itu Pixel (Fragmen) Shader?", "id": "Shader, (Memproses Piksel Individual, (Warna, Tekstur))." },
  { "en": "Apa Itu Pipeline (Pipa Salur) Grafis?", "id": "Tahapan Render, (Model 3D Menjadi Piksel 2D)." },
  { "en": "Apa Itu Z-Buffering (Penyanggaan Z)?", "id": "Teknik Grafis, (Manajemen Kedalaman Objek, (3D))." },
  { "en": "Apa Itu Tekstur (Texture) Grafis?", "id": "Gambar, (Diterapkan Pada Permukaan, Model 3D)." },
  { "en": "Apa Itu Pemetaan (Mapping) UV?", "id": "Proses, (Memetakan Tekstur 2D, Model 3D)." },
  { "en": "Apa Itu Model (Model) Pencahayaan (Lighting)?", "id": "Model Matematika, (Simulasi Interaksi Cahaya, Permukaan)." },
  { "en": "Apa Itu Pencahayaan (Lighting) Ambient?", "id": "Cahaya Latar, (Merata, (Menerangi Seluruh Adegan))." },
  { "en": "Apa Itu Pencahayaan (Lighting) Diffuse?", "id": "Cahaya, (Arah Tertentu, (Pantulan Baur))." },
  { "en": "Apa Itu Pencahayaan (Lighting) Specular?", "id": "Cahaya, (Pantulan Terfokus, (Titik Kilau))." },
  { "en": "Apa Itu Model (Model) Bayangan (Shading) Flat?", "id": "Satu Warna, (Setiap Poligon, (Tampilan Kotak-Kotak))." },
  { "en": "Apa Itu Model (Model) Bayangan (Shading) Gouraud?", "id": "Interpolasi Warna, (Antar Vertex, (Lebih Halus))." },
  { "en": "Apa Itu Model (Model) Bayangan (Shading) Phong?", "id": "Interpolasi Normal, (Antar Piksel, (Sangat Halus))." },
  { "en": "Apa Itu Ray Tracing (Penelusuran Sinar)?", "id": "Teknik Render, (Simulasi Jalur Cahaya, Realistis)." },
  { "en": "Apa Itu Anti-Aliasing (Anti-Alias)?", "id": "Teknik Grafis, (Menghaluskan Tepi Bergerigi, Jaggies)." },
  { "en": "Apa Itu FPS (Frames Per Second)?", "id": "Jumlah Bingkai (Frame), (Ditampilkan Per Detik)." },
  { "en": "Apa Itu VSync (Sinkronisasi Vertikal)?", "id": "Sinkronisasi FPS Game, (Refresh Rate Monitor)." },
  { "en": "Apa Itu Screen Tearing (Robekan Layar)?", "id": "Artefak Visual, (Monitor Menampilkan, Dua Frame)." },
  { "en": "Apa Itu Refresh Rate (Laju Penyegaran) Monitor?", "id": "Seberapa Sering, (Layar Monitor, (Menggambar Ulang Gambar))." },
  { "en": "Apa Itu Waktu Respons (Response Time) Monitor?", "id": "Waktu Piksel, (Berubah Warna, (Mengurangi Ghosting))." },
  { "en": "Apa Itu Panel (Panel) Monitor TN (Twisted Nematic)?", "id": "Panel LCD, (Waktu Respons Cepat, Sudut Pandang Buruk)." },
  { "en": "Apa Itu Panel (Panel) Monitor IPS (In-Plane Switching)?", "id": "Panel LCD, (Warna Akurat, Sudut Pandang Luas)." },
  { "en": "Apa Itu Panel (Panel) Monitor VA (Vertical Alignment)?", "id": "Panel LCD, (Kontras Tinggi, (Hitam Pekat))." },
  { "en": "Apa Itu Panel (Panel) Monitor OLED (Organic LED)?", "id": "Panel, (Setiap Piksel, Cahaya Sendiri, (Kontras Sempurna))." },
  { "en": "Apa Itu Burn-In (Terbakar) Layar?", "id": "Perubahan Warna, Permanen, (Panel OLED, Statis)." },
  { "en": "Apa Itu HDR (High Dynamic Range)?", "id": "Teknologi Tampilan, (Rentang Kontras, Warna Luas)." },
  { "en": "Apa Itu Resolusi (Resolution) Layar 1080p (Full HD)?", "id": "Resolusi 1920 x 1080 Piksel." },
  { "en": "Apa Itu Resolusi (Resolution) Layar 1440p (Quad HD)?", "id": "Resolusi 2560 x 1440 Piksel." },
  { "en": "Apa Itu Resolusi (Resolution) Layar 4K (Ultra HD)?", "id": "Resolusi 3840 x 2160 Piksel." },
  { "en": "Apa Itu Aspek (Aspect) Rasio Layar?", "id": "Perbandingan Proporsional, (Lebar Terhadap Tinggi Layar)." },
  { "en": "Apa Itu Aspek (Aspect) Rasio 16:9?", "id": "Standar Layar Lebar, (HDTV, Monitor Modern)." },
  { "en": "Apa Itu Aspek (Aspect) Rasio 4:3?", "id": "Standar Layar Lama, (TV Tabung, Monitor Kotak)." },
  { "en": "Apa Itu Aspek (Aspect) Rasio 21:9?", "id": "Standar Layar Ultrawide, (Film, Monitor Lebar)." },
  { "en": "Apa Itu PPI (Pixels Per Inch)?", "id": "Ukuran Kepadatan Piksel, (Ketajaman Tampilan)." },
  { "en": "Apa Itu Ruang (Space) Warna (Color Gamut)?", "id": "Rentang Warna, (Dapat Ditampilkan, Perangkat)." },
  { "en": "Apa Itu Ruang (Space) Warna sRGB?", "id": "Standar Ruang Warna, (Monitor, Web, Umum)." },
  { "en": "Apa Itu Ruang (Space) Warna Adobe RGB?", "id": "Ruang Warna Luas, (Profesional, Fotografi, Cetak)." },
  { "en": "Apa Itu Ruang (Space) Warna DCI-P3?", "id": "Ruang Warna Luas, (Industri Film Digital, HDR)." },
  { "en": "Apa Itu Kalibrasi (Calibration) Warna?", "id": "Proses Penyesuaian, (Monitor, Menampilkan Warna Akurat)." },
  { "en": "Apa Itu Pemrograman (Programming) Berorientasi Aspek (AOP)?", "id": "Paradigma, (Memisahkan Cross-Cutting Concerns, (Logging))." },
  { "en": "Apa Itu Cross-Cutting Concern (Perhatian Lintas Sektor)?", "id": "Fungsionalitas, (Mempengaruhi Banyak Bagian, (Keamanan))." },
  { "en": "Apa Itu Aspek (Aspect) AOP?", "id": "Modul, (Mengenkapsulasi Cross-Cutting Concern, (Logging))." },
  { "en": "Apa Itu Join Point (Titik Gabung) AOP?", "id": "Titik Eksekusi Program, (Aspek Dapat Diterapkan)." },
  { "en": "Apa Itu Advice (Nasihat) AOP?", "id": "Aksi, (Dilakukan Aspek, Pada Join Point)." },
  { "en": "Apa Itu Pointcut (Potongan Titik) AOP?", "id": "Ekspresi, (Memilih Join Point, (Menerapkan Advice))." },
  { "en": "Apa Itu Weaving (Menenun) AOP?", "id": "Proses, (Menggabungkan Aspek, Kode Utama Aplikasi)." },
  { "en": "Apa Itu Spring (Kerangka Kerja) AOP?", "id": "Modul Spring, (Implementasi Pemrograman Berorientasi Aspek)." },
  { "en": "Apa Itu AspectJ?", "id": "Implementasi AOP, (Ekstensi Bahasa Java, Kuat)." },
  { "en": "Apa Itu Pola (Pattern) Desain Interceptor?", "id": "Pola, (Mencegat, Memodifikasi Permintaan, Respons)." },
  { "en": "Apa Itu Pola (Pattern) Desain Filter?", "id": "Pola, (Memproses Permintaan, Respons, Rantai Filter)." },
  { "en": "Apa Itu Servlet (Java Servlet)?", "id": "Teknologi Java, (Menghasilkan Konten Web Dinamis, Server)." },
  { "en": "Apa Itu Kontainer (Container) Servlet (Web)?", "id": "Lingkungan, (Mengelola Siklus Hidup Servlet, (Tomcat))." },
  { "en": "Apa Itu Apache Tomcat?", "id": "Kontainer Servlet, (Open-Source, Implementasi Referensi Java)." },
  { "en": "Apa Itu Jetty (Server Web)?", "id": "Server HTTP, (Kontainer Servlet, Ringan, Embedded)." },
  { "en": "Apa Itu File 'web.xml' (Deployment Descriptor)?", "id": "File Konfigurasi, (Aplikasi Web Java, (Deployment))." },
  { "en": "Apa Itu Berkas (File) WAR (Web Application Archive)?", "id": "Format File, (Paket Aplikasi Web Java, (JAR))." },
  { "en": "Apa Itu Berkas (File) JAR (Java Archive)?", "id": "Format File, (Paket Pustaka, Aplikasi Java, (ZIP))." },
  { "en": "Apa Itu Berkas (File) EAR (Enterprise Archive)?", "id": "Format File, (Paket Aplikasi Java EE, (Enterprise))." },
  { "en": "Apa Itu Java EE (Enterprise Edition)?", "id": "Platform Java, (Pengembangan Aplikasi, Skala Perusahaan)." },
  { "en": "Apa Itu Jakarta EE (Enterprise Edition)?", "id": "Evolusi Java EE, (Pengembangan Komunitas, Eclipse Foundation)." },
  { "en": "Apa Itu EJB (Enterprise JavaBeans)?", "id": "Komponen Sisi Server, (Logika Bisnis Java EE)." },
  { "en": "Apa Itu JMS (Java Message Service)?", "id": "API Java, (Mengirim Pesan, (Asinkron, MOM))." },
  { "en": "Apa Itu MOM (Message-Oriented Middleware)?", "id": "Middleware, (Berbasis Pertukaran Pesan, (Antrean))." },
  { "en": "Apa Itu Point-to-Point (P2P) Messaging?", "id": "Model Pesan, (Satu Pengirim, Satu Penerima, (Antrean))." },
  { "en": "Apa Itu Publish/Subscribe (Pub/Sub) Messaging?", "id": "Model Pesan, (Satu Pengirim, Banyak Penerima, (Topik))." },
  { "en": "Apa Itu Antrean (Queue) Pesan?", "id": "Penyimpanan Pesan, (Model Point-to-Point, (FIFO))." },
  { "en": "Apa Itu Topik (Topic) Pesan?", "id": "Kategori Pesan, (Model Publish/Subscribe, (Broadcast))." },
  { "en": "Apa Itu Antivirus (Perangkat Lunak)?", "id": "Perangkat Lunak, (Mendeteksi, Menghapus Malware, Virus)." },
  { "en": "Apa Itu Deteksi (Detection) Berbasis Tanda Tangan?", "id": "Antivirus, (Mendeteksi Malware, (Database Pola Dikenal))." },
  { "en": "Apa Itu Deteksi (Detection) Berbasis Heuristik?", "id": "Antivirus, (Mendeteksi Malware, (Perilaku Mencurigakan))." },
  { "en": "Apa Itu False Positive (Positif Palsu) Antivirus?", "id": "Antivirus, (Salah Mengidentifikasi, File Aman, (Sebagai Malware))." },
  { "en": "Apa Itu False Negative (Negatif Palsu) Antivirus?", "id": "Antivirus, (Gagal Mendeteksi, Malware Yang Ada)." },
  { "en": "Apa Itu Karantina (Quarantine) Antivirus?", "id": "Isolasi File, (Diduga Terinfeksi, (Mencegah Penyebaran))." },
  { "en": "Apa Itu EDR (Endpoint Detection and Response)?", "id": "Solusi Keamanan, (Memantau, Merespons Ancaman, Endpoint)." },
  { "en": "Apa Itu XDR (Extended Detection and Response)?", "id": "Evolusi EDR, (Mengintegrasi Data, Berbagai Sumber)." },
  { "en": "Apa Itu DLP (Data Loss Prevention)?", "id": "Strategi, (Mencegah Kehilangan, Pencurian Data Sensitif)." },
  { "en": "Apa Itu CASB (Cloud Access Security Broker)?", "id": "Titik Kontrol Keamanan, (Antara Pengguna, Layanan Awan)." },
  { "en": "Apa Itu Skema (Schema) Bintang (Star)?", "id": "Model Data, Warehouse (Fakta, Dimensi)." },
  { "en": "Apa Itu Tabel (Table) Fakta (Fact)?", "id": "Tabel Pusat, (Skema Bintang, (Mengukur Data))." },
  { "en": "Apa Itu Tabel (Table) Dimensi (Dimension)?", "id": "Tabel, (Menjelaskan Data, Tabel Fakta)." },
  { "en": "Apa Itu Skema (Schema) Kepingan Salju?", "id": "Model Data, (Variasi Skema Bintang, (Normalisasi Dimensi))." },
  { "en": "Apa Perbedaan (Difference) Skema Bintang, Kepingan Salju?", "id": "Kepingan Salju, (Dimensi Dinormalisasi, (Lebih Kompleks))." },
  { "en": "Apa Itu Operasi (Operation) Drill-Down (Telusur) OLAP?", "id": "Melihat Data, (Level Lebih Detail, Hierarki)." },
  { "en": "Apa Itu Operasi (Operation) Roll-Up (Gulung) OLAP?", "id": "Melihat Data, (Level Lebih Ringkas, Hierarki)." },
  { "en": "Apa Itu Operasi (Operation) Slice (Iris) OLAP?", "id": "Memilih Satu Dimensi, (Sub-Kubus, Data)." },
  { "en": "Apa Itu Operasi (Operation) Dice (Dadu) OLAP?", "id": "Memilih Sub-Kubus, (Berdasarkan Banyak Dimensi)." },
  { "en": "Apa Itu Operasi (Operation) Pivot (Poros) OLAP?", "id": "Memutar Orientasi, Sumbu Kubus Data." },
  { "en": "Apa Itu Metadata (Data) Bisnis?", "id": "Metadata, (Menjelaskan Konteks Bisnis, Data)." },
  { "en": "Apa Itu Metadata (Data) Teknis?", "id": "Metadata, (Menjelaskan Struktur Teknis, Data)." },
  { "en": "Apa Itu Perlindungan Data (Data Protection)?", "id": "Melindungi Data, (Dari Korupsi, Kehilangan, Serangan)." },
  { "en": "Apa Itu Privasi (Privacy) Data?", "id": "Hak Individu, (Mengontrol Penggunaan, Data Pribadi)." },
  { "en": "Apa Itu Penyembunyian (Masking) Data?", "id": "Menyembunyikan Data Asli, (Data Palsu, Realistis)." },
  { "en": "Apa Itu Tokenisasi (Tokenization) Data?", "id": "Mengganti Data Sensitif, (Token Pengganti, Acak)." },
  { "en": "Apa Perbedaan (Difference) Enkripsi Dan Tokenisasi?", "id": "Enkripsi (Matematis, Reversible), Tokenisasi (Mapping)." },
  { "en": "Apa Itu Arsitektur (Architecture) Berbasis Layanan (Service-Based)?", "id": "Pola Arsitektur, (Aplikasi Dibangun, Kumpulan Layanan)." },
  { "en": "Apa Itu Arsitektur (Architecture) Monolitik (Monolithic)?", "id": "Aplikasi, (Dibangun, Satu Unit Tunggal, Terpadu)." },
  { "en": "Apa Keuntungan (Advantage) Arsitektur Monolitik?", "id": "Sederhana, (Pengembangan Awal, Deployment Mudah)." },
  { "en": "Apa Kerugian (Disadvantage) Arsitektur Monolitik?", "id": "Sulit Diskalakan, (Pemeliharaan Kompleks, Rapuh)." },
  { "en": "Apa Keuntungan (Advantage) Arsitektur Microservice?", "id": "Skalabilitas Independen, (Fleksibel, Teknologi Berbeda)." },
  { "en": "Apa Kerugian (Disadvantage) Arsitektur Microservice?", "id": "Kompleksitas Operasional, (Jaringan, Konsistensi Data)." },
  { "en": "Apa Itu Komunikasi (Communication) Sinkron (Synchronous) Microservice?", "id": "Layanan Menunggu, (Respons Langsung, (HTTP, REST))." },
  { "en": "Apa Itu Komunikasi (Communication) Asinkron (Asynchronous) Microservice?", "id": "Layanan Tidak Menunggu, (Respons, (Message Queue))." },
  { "en": "Apa Itu Koreografi (Choreography) Microservice?", "id": "Layanan Independen, (Bereaksi Terhadap Peristiwa, (Event))." },
  { "en": "Apa Itu Orkestrasi (Orchestration) Microservice?", "id": "Satu Layanan (Orkestrator), (Mengontrol Alur Kerja))." },
  { "en": "Apa Itu Kontrak (Contract) API (Application Programming Interface)?", "id": "Perjanjian Formal, (Cara Berinteraksi, API)." },
  { "en": "Apa Itu Spesifikasi (Specification) OpenAPI (Swagger)?", "id": "Standar Mendefinisikan, (API RESTful, (Format YAML/JSON))." },
  { "en": "Apa Itu API (Application Programming Interface) First Design?", "id": "Mendesain, Mendokumentasikan API, (Sebelum Implementasi)." },
  { "en": "Apa Itu Pemimpin (Leader) Proyek?", "id": "Orang, (Bertanggung Jawab, Memimpin Tim Proyek)." },
  { "en": "Apa Itu Manajer (Manager) Proyek?", "id": "Orang, (Perencanaan, Eksekusi, Penutupan Proyek)." },
  { "en": "Apa Itu Pemangku (Stakeholder) Kepentingan Proyek?", "id": "Individu, Grup, (Terdampak, Hasil Proyek)." },
  { "en": "Apa Itu Piagam (Charter) Proyek?", "id": "Dokumen Formal, (Otorisasi, Tujuan, Ruang Lingkup Proyek)." },
  { "en": "Apa Itu Pernyataan (Statement) Ruang Lingkup Proyek?", "id": "Dokumen, (Mendefinisikan Batasan, Hasil Proyek)." },
  { "en": "Apa Itu WBS (Work Breakdown Structure) Kamus?", "id": "Dokumen, (Deskripsi Detail, Setiap Komponen WBS)." },
  { "en": "Apa Itu Matriks (Matrix) RACI?", "id": "Matriks, (Penugasan Peran, Tanggung Jawab, (Tugas))." },
  { "en": "Apa Arti (Meaning) R Dalam RACI?", "id": "Responsible, (Orang Yang Melakukan Tugas)." },
  { "en": "Apa Arti (Meaning) A Dalam RACI?", "id": "Accountable, (Orang Yang Bertanggung Jawab Penuh)." },
  { "en": "Apa Arti (Meaning) C Dalam RACI?", "id": "Consulted, (Orang Yang Diberi Konsultasi)." },
  { "en": "Apa Arti (Meaning) I Dalam RACI?", "id": "Informed, (Orang Yang Diberi Informasi)." },
  { "en": "Apa Itu Analisis (Analysis) Jalur Kritis (Critical Path)?", "id": "Teknik, (Menentukan Durasi Proyek, (Tugas Kritis))." },
  { "en": "Apa Itu Float (Slack) Total?", "id": "Waktu Tunda, (Tugas, Tanpa Menunda Proyek)." },
  { "en": "Apa Itu Float (Slack) Bebas?", "id": "Waktu Tuda, (Tugas, Tanpa Menunda Tugas Berikutnya)." },
  { "en": "Apa Itu Fast Tracking (Percepatan) Proyek?", "id": "Mempercepat Proyek, (Melakukan Tugas Paralel)." },
  { "en": "Apa Itu Crashing (Pemadatan) Proyek?", "id": "Mempercepat Proyek, (Menambah Sumber Daya, (Biaya))." },
  { "en": "Apa Itu Baseline (Garis Dasar) Proyek?", "id": "Rencana Awal, (Ruang Lingkup, Jadwal, Biaya)." },
  { "en": "Apa Itu Manajemen (Management) Nilai Hasil (EVM)?", "id": "Teknik Mengukur, (Kinerja, Kemajuan Proyek)." },
  { "en": "Apa Itu Pustaka (Library) React Native?", "id": "Kerangka Kerja (Framework), (Membangun Aplikasi Seluler, (JavaScript))." },
  { "en": "Apa Itu Pustaka (Library) Flutter?", "id": "Toolkit UI Google, (Membangun Aplikasi (Cross-Platform))." },
  { "en": "Apa Bahasa (Language) Pemrograman Dart?", "id": "Bahasa Pemrograman, (Dikembangkan Google, (Flutter))." },
  { "en": "Apa Itu Xamarin?", "id": "Kerangka Kerja (Framework), (Microsoft, (Cross-Platform, C#))." },
  { "en": "Apa Perbedaan (Difference) Native vs Cross-Platform?", "id": "Native (Spesifik Platform), Cross-Platform (Satu Kode)." },
  { "en": "Apa Itu Xcode?", "id": "IDE (Integrated Development Environment) Apple, (Pengembangan MacOS, iOS)." },
  { "en": "Apa Itu Android Studio?", "id": "IDE (Integrated Development Environment) Resmi, (Pengembangan Android)." },
  { "en": "Apa Itu Berkas (File) APK (Android Package)?", "id": "Format Paket File, (Instalasi Aplikasi Android)." },
  { "en": "Apa Itu Berkas (File) AAB (Android App Bundle)?", "id": "Format Publikasi, (Google Play, (Ukuran Efisien))." },
  { "en": "Apa Itu TestFlight?", "id": "Platform Apple, (Distribusi, Pengujian Beta iOS)." },
  { "en": "Apa Itu Google Play (Play Store) Console?", "id": "Platform Google, (Manajemen, Publikasi Aplikasi Android)." },
  { "en": "Apa Itu Android (Android) SDK (Software Development Kit)?", "id": "Kumpulan Alat, (Pengembangan Aplikasi Android)." },
  { "en": "Apa Itu Emulator (Emulator) Android?", "id": "Perangkat Lunak, (Simulasi Perangkat Android, Komputer)." },
  { "en": "Apa Itu Simulator (Simulator) iOS?", "id": "Perangkat Lunak, (Simulasi Perangkat iOS, MacOS)." },
  { "en": "Apa Itu Aktivitas (Activity) Android?", "id": "Komponen Aplikasi, (Menyediakan Satu Layar, (UI))." },
  { "en": "Apa Itu Layanan (Service) Android?", "id": "Komponen Aplikasi, (Tugas Latar Belakang, (Tanpa UI))." },
  { "en": "Apa Itu Intent (Maksud) Android?", "id": "Objek Pesan, (Meminta Aksi, Komponen Aplikasi Lain)." },
  { "en": "Apa Itu Manifes (Manifest) Aplikasi Android?", "id": "File Konfigurasi, (Informasi Esensial Aplikasi Android)." },
  { "en": "Apa Itu Gradle (Alat Build)?", "id": "Alat Otomatisasi Build, (Umum, Proyek Android)." },
  { "en": "Apa Itu Cocoa Touch?", "id": "Kerangka Kerja (Framework) UI, (Pengembangan Aplikasi iOS)." },
  { "en": "Apa Itu Swift (Bahasa Pemrograman)?", "id": "Bahasa Pemrograman, (Apple, (Modern, Cepat, Aman))." },
  { "en": "Apa Itu Objective-C (Bahasa Pemrograman)?", "id": "Bahasa Pemrograman, (Lama, Pengembangan Apple, (OOP))." },
  { "en": "Apa Itu MVC (Model-View-Controller) Apple?", "id": "Pola Desain, (Umum, Aplikasi iOS, MacOS)." },
  { "en": "Apa Itu SwiftUI?", "id": "Kerangka Kerja (Framework) UI, (Deklaratif, Modern, Apple)." },
  { "en": "Apa Itu Kernel (Kernel) XNU?", "id": "Kernel Hibrida, (Inti Sistem Operasi MacOS, iOS)." },
  { "en": "Apa Itu APFS (Apple File System)?", "id": "Sistem Berkas, (Default, Perangkat Apple Modern, (SSD))." },
  { "en": "Apa Itu HFS+ (Hierarchical File System Plus)?", "id": "Sistem Berkas, (Lama, Perangkat Apple, (HDD))." },
  { "en": "Apa Itu Firewall (Tembok Api) Aplikasi?", "id": "Firewall, (Kontrol Akses, Berbasis Aplikasi, (Bukan Port))." },
  { "en": "Apa Itu Serangan (Attack) Downgrade (Penurunan Tingkat)?", "id": "Memaksa Sistem, (Menggunakan Versi, Protokol Kurang Aman)." },
  { "en": "Apa Itu Serangan (Attack) Ulang Tahun (Birthday)?", "id": "Serangan Kriptografi, (Mencari Kolisi Hash, (Probabilitas))." },
  { "en": "Apa Itu Analisis (Analysis) Frekuensi?", "id": "Metode Kriptanalisis, (Mempelajari Frekuensi, Karakter Teks Sandi)." },
  { "en": "Apa Itu Sandi (Cipher) Caesar?", "id": "Teknik Enkripsi, Substitusi Sederhana (Geser Alfabet)." },
  { "en": "Apa Itu Sandi (Cipher) Vigenère?", "id": "Teknik Enkripsi, Substitusi Polialfabetik (Kata Kunci)." },
  { "en": "Apa Itu Enigma (Mesin Enigma)?", "id": "Mesin Rotor Kriptografi, (Perang Dunia II, Jerman)." },
  { "en": "Apa Itu One-Time Pad (OTP) Enkripsi?", "id": "Teknik Enkripsi, (Kunci Acak, Panjang Sama, (Sempurna))." },
  { "en": "Apa Itu Data (Data) at Rest (Diam)?", "id": "Data, (Disimpan, Media Penyimpanan Fisik, (Tidak Aktif))." },
  { "en": "Apa Itu Data (Data) in Transit (Transit)?", "id": "Data, (Bergerak, Antar Jaringan, (Aktif))." },
  { "en": "Apa Itu Data (Data) in Use (Digunakan)?", "id": "Data, (Diproses, Memori (RAM), CPU, (Aktif))." },
  { "en": "Apa Itu Enkripsi (Encryption) Disk Penuh (Full Disk)?", "id": "Mengenkripsi Seluruh, Data, Volume Penyimpanan (Sistem)." },
  { "en": "Apa Itu Enkripsi (Encryption) Level File?", "id": "Mengenkripsi File, Folder, Individual (Kontrol Akses)." },
  { "en": "Apa Itu Enkripsi (Encryption) Transparan?", "id": "Enkripsi Otomatis, (Latar Belakang, (Tidak Terlihat Pengguna))." },
  { "en": "Apa Itu Manajemen (Management) Kunci (Key) Kriptografi?", "id": "Siklus Hidup Kunci, (Pembuatan, Penyimpanan, Penghapusan)." },
  { "en": "Apa Itu HSM (Hardware Security Module)?", "id": "Perangkat Keras, (Mengelola, Melindungi Kunci Digital)." },
  { "en": "Apa Itu Key (Kunci) Escrow?", "id": "Penyimpanan Kunci Kriptografi, (Pihak Ketiga Tepercaya)." },
  { "en": "Apa Itu Pemulihan (Recovery) Kunci?", "id": "Proses, (Mendapatkan Kembali, Kunci Kriptografi Hilang)." },
  { "en": "Apa Itu Rotasi (Rotation) Kunci?", "id": "Mengganti Kunci Kriptografi, (Secara Berkala, (Keamanan))." },
  { "en": "Apa Itu Kriptografi (Cryptography) Simetris vs Asimetris?", "id": "Simetris (Satu Kunci), Asimetris (Dua Kunci)." },
  { "en": "Kapan Menggunakan (Use) Enkripsi Simetris?", "id": "Data Volume Besar, (Enkripsi Cepat, (File))." },
  { "en": "Kapan Menggunakan (Use) Enkripsi Asimetris?", "id": "Pertukaran Kunci Aman, (Tanda Tangan Digital, (Lambat))." },
  { "en": "Apa Itu Enkripsi (Encryption) Hibrida?", "id": "Menggabungkan Simetris, (Data), Asimetris, (Kunci Sesi)." },
  { "en": "Bagaimana Cara Kerja (Work) HTTPS?", "id": "Menggunakan Enkripsi Hibrida, (SSL/TLS, (Aman))." },
  { "en": "Apa Itu Sesi (Session) Kunci (Key)?", "id": "Kunci Simetris, (Unik, Sesi Komunikasi Tunggal)." },
  { "en": "Apa Itu Kompilasi (Compilation) JIT (Just-In-Time)?", "id": "Kompilasi Kode, (Saat Program Dijalankan, (Runtime))." },
  { "en": "Apa Itu Kompilasi (Compilation) AOT (Ahead-of-Time)?", "id": "Kompilasi Kode, (Sebelum Program Dijalankan, (Build))." },
  { "en": "Apa Keuntungan (Advantage) Kompilasi JIT?", "id": "Optimasi Runtime, (Kode Platform Spesifik)." },
  { "en": "Apa Kerugian (Disadvantage) Kompilasi JIT?", "id": "Waktu Pemanasan (Warm-up), (Overhead Awal)." },
  { "en": "Apa Keuntungan (Advantage) Kompilasi AOT?", "id": "Waktu Mulai Cepat, (Tanpa Pemanasan)." },
  { "en": "Apa Itu Analisis (Analysis) Leksikal (Lexer)?", "id": "Tahap Compiler, (Memecah Kode, Menjadi Token)." },
  { "en": "Apa Itu Analisis (Analysis) Sintaksis (Parser)?", "id": "Tahap Compiler, (Memeriksa Struktur Tata Bahasa)." },
  { "en": "Apa Itu Pohon (Tree) Sintaks Abstrak (AST)?", "id": "Representasi Struktur Kode, (Hierarkis, Abstrak)." },
  { "en": "Apa Itu Bahasa (Language) Pemrograman Interpretatif?", "id": "Bahasa, (Dieksekusi Langsung, Baris Per Baris)." },
  { "en": "Apa Itu Bahasa (Language) Pemrograman Kompilatif?", "id": "Bahasa, (Diterjemahkan, Kode Mesin, (Sebelumnya))." },
  { "en": "Apa Itu Bytecode (Kode Byte)?", "id": "Kode Menengah, (Portabel, (Dieksekusi VM))." },
  { "en": "Contoh (Example) Bahasa Menggunakan Bytecode?", "id": "Java (JVM), Python (.pyc), C# (CIL)." },
  { "en": "Apa Itu Pemrograman (Programming) Meta (Meta-Programming)?", "id": "Program, (Menulis, Menganalisis, Memodifikasi Program Lain)." },
  { "en": "Apa Itu Refleksi (Reflection) Pemrograman?", "id": "Program, (Memeriksa Strukturnya Sendiri, (Runtime))." },
  { "en": "Apa Itu Model (Model) Aktor (Actor)?", "id": "Model Konkurensi, (Aktor Berkomunikasi, Pesan Asinkron)." },
  { "en": "Apa Itu Erlang (Bahasa Pemrograman)?", "id": "Bahasa Fungsional, (Konkurensi Tinggi, (Model Aktor))." },
  { "en": "Apa Itu Akka (Toolkit Konkurensi)?", "id": "Toolkit, (Membangun Aplikasi Konkuren, (JVM, Aktor))." },
  { "en": "Apa Itu Konkurensi (Concurrency) vs Paralelisme?", "id": "Konkurensi (Menangani Banyak), Paralelisme (Mengerjakan Banyak)." },
  { "en": "Apa Itu Global Interpreter Lock (GIL) Python?", "id": "Mekanisme Mutex, (Satu Thread, Eksekusi Bytecode Python)." },
  { "en": "Apa Dampak (Impact) GIL (Global Interpreter Lock)?", "id": "Membatasi Paralelisme, (Thread CPU-Bound, Python)." },
  { "en": "Apa Itu Multiprocessing (MultiproMrosesan) Python?", "id": "Membuat Proses Baru, (Mengatasi Batasan GIL)." },
  { "en": "Apa Itu AsyncIO (Asinkron) Python?", "id": "Pustaka Konkurensi, (I/O Asinkron, (Event Loop))." },
  { "en": "Apa Kata Kunci (Keyword) 'async' Python?", "id": "Mendefinisikan, Fungsi Coroutine (Asinkron)." },
  { "en": "Apa Kata Kunci (Keyword) 'await' Python?", "id": "Menjeda Eksekusi Coroutine, (Menunggu Hasil Asinkron)." },
  { "en": "Apa Itu Event (Peristiwa) Loop (Putaran)?", "id": "Mekanisme, (Menjalankan Tugas Asinkron, (Tunggal Thread))." },
  { "en": "Apa Itu Pemrograman (Programming) Fungsional?", "id": "Paradigma, (Berbasis Fungsi Matematika, (Immutable))." },
  { "en": "Apa Itu Fungsi (Function) Murni (Pure)?", "id": "Output Deterministik, (Tanpa Efek Samping)." },
  { "en": "Apa Itu Imutabilitas (Immutability)?", "id": "Keadaan (State), Tidak Dapat Diubah, (Setelah Dibuat)." },
  { "en": "Apa Itu Efek (Effect) Samping (Side Effect)?", "id": "Fungsi, (Mengubah Keadaan, Diluar Lingkupnya)." },
  { "en": "Apa Itu Fungsi (Function) Orde Tinggi?", "id": "Fungsi, (Menerima, Mengembalikan Fungsi Lain)." },
  { "en": "Apa Itu Lambda (Ekspresi Lambda)?", "id": "Fungsi Anonim, (Singkat, Tanpa Nama)." },
  { "en": "Apa Itu Penutupan (Closure)?", "id": "Fungsi, (Mengingat Lingkup, Tempat Dibuat)." },
  { "en": "Apa Itu Currying (Kari)?", "id": "Mengubah Fungsi, (Banyak Argumen, (Rantai Fungsi))." },
  { "en": "Apa Itu Rekursi (Recursion) Ekor (Tail)?", "id": "Rekursi, (Panggilan Terakhir, (Optimasi Stack))." },
  { "en": "Apa Itu Memoization (Memo-isasi)?", "id": "Teknik Caching, (Hasil Panggilan Fungsi Mahal)." },
  { "en": "Apa Itu Bahasa (Language) Haskell?", "id": "Bahasa Fungsional Murni, (Tipe Statis, Kuat)." },
  { "en": "Apa Itu Bahasa (Language) Lisp?", "id": "Bahasa Pemrograman Lama, (Fungsional, (S-Expressions))." },
  { "en": "Apa Itu Bahasa (Language) Scala?", "id": "Bahasa Hibrida, (OOP, Fungsional, (JVM))." },
  { "en": "Apa Itu Bahasa (Language) Clojure?", "id": "Bahasa Fungsional Dinamis, (Dialek Lisp, (JVM))." },
  { "en": "Apa Itu Bahasa (Language) F# (F Sharp)?", "id": "Bahasa Fungsional, (Platform .NET, Microsoft)." },
  { "en": "Apa Itu Git (Global Information Tracker) Rebase Interaktif?", "id": "Mengubah Riwayat Commit, (Mengedit, Menggabungkan, Mengurutkan)." },
  { "en": "Apa Itu Git (Global Information Tracker) Squash?", "id": "Menggabungkan Beberapa Commit, (Menjadi Satu Commit)." },
  { "en": "Apa Itu Git (Global Information Tracker) Worktree?", "id": "Mengelola Banyak, (Cabang Kerja, (Satu Repositori))." },
  { "en": "Apa Itu Perintah (Command) 'git fsck'?", "id": "Memeriksa Integritas, Objek Database Git." },
  { "en": "Apa Itu Perintah (Command) 'git gc'?", "id": "Membersihkan File, (Mengoptimalkan Repositori Git, (Garbage Collection))." },
  { "en": "Apa Itu Objek (Object) Blob Git?", "id": "Menyimpan Konten File, (Database Git)." },
  { "en": "Apa Itu Objek (Object) Tree Git?", "id": "Menyimpan Struktur Direktori, (Nama File, Pointer Blob)." },
  { "en": "Apa Itu Objek (Object) Commit Git?", "id": "Menyimpan Snapshot Proyek, (Pointer Tree, Metadata)." },
  { "en": "Apa Itu Objek (Object) Tag Git?", "id": "Menandai Commit Spesifik, (Nama, Pesan, Tanda Tangan)." },
  { "en": "Apa Itu SHA-1 (Secure Hash Algorithm 1) Hash?", "id": "Hash Kriptografis, (Identifikasi Objek Git, (Unik))." },
  { "en": "Apa Itu Direktori (Directory) '.git'?", "id": "Direktori Tersembunyi, (Menyimpan Database, Repositori Git)." },
  { "en": "Apa Itu File (File) 'HEAD' Git?", "id": "Pointer, (Menunjuk Ke Cabang, (Branch) Aktif)." },
  { "en": "Apa Itu File (File) 'config' Git?", "id": "File Konfigurasi, (Pengaturan Repositori, Pengguna)." },
  { "en": "Apa Itu File (File) 'index' Git?", "id": "Area Staging, (Menyimpan Perubahan, (Sebelum Commit))." },
  { "en": "Apa Itu Perintah (Command) 'git log --graph'?", "id": "Menampilkan Riwayat Commit, (Grafik ASCII Visual)." },
  { "en": "Apa Itu Perintah (Command) 'git diff --staged'?", "id": "Menampilkan Perbedaan, (Staging Area, Commit Terakhir)." },
  { "en": "Apa Itu Perintah (Command) 'git remote -v'?", "id": "Menampilkan Daftar Remote, (Beserta URL Lengkap)." },
  { "en": "Apa Itu Alur Kerja (Workflow) GitHub Flow?", "id": "Alur Kerja Git, (Sederhana, (Branch Fitur, Pull Request))." },
  { "en": "Apa Itu Alur Kerja (Workflow) GitLab Flow?", "id": "Alur Kerja Git, (GitHub Flow, (Cabang Lingkungan))." },
  { "en": "Apa Itu Alur Kerja (Workflow) Terpusat (Centralized)?", "id": "Alur Kerja Git, (Tim Kecil, (Satu Cabang 'master'))." },
  { "en": "Apa Itu Cabang (Branch) 'main'?", "id": "Nama Pengganti, (Default, Untuk Cabang 'master')." },
  { "en": "Apa Itu Integrasi (Integration) Berkelanjutan (Continuous)?", "id": "Praktik, (Menggabungkan Kode, (Sering, Otomatis Tes))." },
  { "en": "Apa Itu Pengiriman (Delivery) Berkelanjutan (Continuous)?", "id": "Praktik, (Build Siap Rilis, (Otomatis Tes))." },
  { "en": "Apa Itu Penerapan (Deployment) Berkelanjutan (Continuous)?", "id": "Praktik, (Otomatis Menerapkan, Rilis Ke Produksi)." },
  { "en": "Perbedaan (Difference) CD (Delivery) Dan CD (Deployment)?", "id": "Delivery (Siap Rilis), Deployment (Otomatis Rilis)." },
  { "en": "Apa Itu Rilis (Release) Canary (Kenari)?", "id": "Teknik Deployment, (Rilis Sebagian Kecil Pengguna)." },
  { "en": "Apa Itu Deployment (Deployment) Blue-Green?", "id": "Teknik Deployment, (Dua Lingkungan Identik, (Aktif/Idle))." },
  { "en": "Apa Itu Deployment (Deployment) Rolling (Bergulir)?", "id": "Teknik Deployment, (Memperbarui Instansi, (Satu Persatu))." },
  { "en": "Apa Itu Deployment (Deployment) Recreate?", "id": "Teknik Deployment, (Mematikan Versi Lama, (Lalu Versi Baru))." },
  { "en": "Apa Itu Saklar (Toggle) Fitur?", "id": "Teknik, (Mengaktifkan, Menonaktifkan Fitur, (Tanpa Deployment))." },
  { "en": "Apa Itu Pengujian (Testing) A/B?", "id": "Membandingkan Dua Versi, (Fitur, Halaman, (Menentukan Performa))." },
  { "en": "Apa Itu Pengujian (Testing) Multivariate?", "id": "Mirip A/B, (Menguji Banyak Variasi, Elemen)." },
  { "en": "Apa Itu Sistem (System) Desain?", "id": "Kumpulan Komponen, (Panduan, Standar, (Desain Konsisten))." },
  { "en": "Apa Itu Token (Token) Desain?", "id": "Nilai Desain, (Dapat Digunakan Ulang, (Warna, Spasi))." },
  { "en": "Apa Itu Arsitektur (Architecture) Monorepo?", "id": "Satu Repositori, (Menyimpan Banyak Proyek, Kode)." },
  { "en": "Apa Itu Arsitektur (Architecture) Polyrepo?", "id": "Setiap Proyek, (Repositori Sendiri, (Independen))." },
  { "en": "Keuntungan (Advantage) Monorepo?", "id": "Berbagi Kode Mudah, (Refactor Skala Besar)." },
  { "en": "Kerugian (Disadvantage) Monorepo?", "id": "Build Lambat, (Kompleksitas Alat Bantu, (Skala))." },
  { "en": "Keuntungan (Advantage) Polyrepo?", "id": "Independen, (Build Cepat, (Tim Otonom))." },
  { "en": "Kerugian (Disadvantage) Polyrepo?", "id": "Berbagi Kode Sulit, (Konsistensi Antar Proyek)." },
  { "en": "Apa Itu Lerna (Alat Monorepo)?", "id": "Alat Bantu, (Manajemen Proyek JavaScript, Monorepo)." },
  { "en": "Apa Itu Nx (Nrwl Extensions)?", "id": "Toolkit Build, (Monorepo, (Cerdas, Cepat))." },
  { "en": "Apa Itu PWA (Progressive Web App)?", "id": "Aplikasi Web, (Fitur Native, (Offline, Notifikasi))." },
  { "en": "Apa Itu Manifes (Manifest) Aplikasi Web?", "id": "File JSON, (Informasi PWA, (Nama, Ikon))." },
  { "en": "Apa Itu Service (Layanan) Worker?", "id": "Skrip Latar Belakang, (Browser, (Offline Cache, Push))." },
  { "en": "Apa Itu API (Application Programming Interface) Cache?", "id": "API JavaScript, (Penyimpanan Cache, (Permintaan Jaringan))." },
  { "en": "Apa Itu API (Application Programming Interface) Push?", "id": "API Browser, (Menerima Notifikasi Push, Server)." },
  { "en": "Apa Itu API (Application Programming Interface) Notifikasi?", "id": "API Browser, (Menampilkan Notifikasi, (Sistem Operasi))." },
  { "en": "Apa Itu AMP (Accelerated Mobile Pages)?", "id": "Kerangka Kerja (Framework), (Membuat Halaman Web, (Cepat, Mobile))." },
  { "en": "Apa Keuntungan (Advantage) AMP (Accelerated Mobile Pages)?", "id": "Pemuatan Sangat Cepat, (Cache Google, (Mobile))." },
  { "en": "Apa Kerugian (Disadvantage) AMP (Accelerated Mobile Pages)?", "id": "Keterbatasan Fitur, (Ekosistem Google, (URL))." },
  { "en": "Apa Itu WebAssembly (Wasm)?", "id": "Format Biner, (Portabel, (Eksekusi Kode Cepat, Web))." },
  { "en": "Apa Tujuan (Purpose) WebAssembly?", "id": "Menjalankan Kode, (Performa Tinggi, (C++, Rust), Browser)." },
  { "en": "Apa Itu WASI (WebAssembly System Interface)?", "id": "Antarmuka Sistem, (WebAssembly, (Akses Diluar Browser))." },
  { "en": "Apa Itu Blazor (Microsoft)?", "id": "Kerangka Kerja (Framework), (Membangun Web UI, (C#, WebAssembly))." },
  { "en": "Apa Itu Emscripten?", "id": "Compiler Toolchain, (C/C++, (Kompilasi Ke WebAssembly))." },
  { "en": "Apa Itu WebGL (Web Graphics Library)?", "id": "API JavaScript, (Render Grafis 3D, (Browser, GPU))." },
  { "en": "Apa Itu Three.js?", "id": "Pustaka JavaScript, (Memudahkan Penggunaan WebGL, (3D))." },
  { "en": "Apa Itu Web (Web) Audio API?", "id": "API JavaScript, (Memproses, Sintesis Audio, (Browser))." },
  { "en": "Apa Itu Web (Web) Socket API?", "id": "API JavaScript, (Komunikasi Dua Arah, (Real-time))." },
  { "en": "Apa Itu Web (Web) Storage API?", "id": "API JavaScript, (Penyimpanan Data Sisi Klien, (Local/Session))." },
  { "en": "Apa Itu IndexedDB (Database)?", "id": "API Database, (Sisi Klien, (Struktur, Skala Besar))." },
  { "en": "Apa Itu Web (Web) Worker API?", "id": "API JavaScript, (Menjalankan Skrip, (Thread Latar Belakang))." },
  { "en": "Apa Itu API (Application Programming Interface) WebSocket?", "id": "API Browser, Komunikasi Dua Arah Penuh." },
  { "en": "Apa Itu API (Application Programming Interface) Geolocation?", "id": "API Browser, Mendapatkan Lokasi Geografis Pengguna." },
  { "en": "Apa Itu API (Application Programming Interface) WebRTC?", "id": "API Browser, Komunikasi Audio, Video Real-Time." },
  { "en": "Apa Itu API (Application ProgrammingInterface) Web Audio?", "id": "API Browser, Memproses, Sintesis Audio." },
  { "en": "Apa Itu API (Application Programming Interface) Web Storage?", "id": "API Browser, Penyimpanan Data Sisi Klien." },
  { "en": "Apa Itu Local Storage (Penyimpanan Lokal)?", "id": "Web Storage, Data Persisten (Tanpa Kadaluarsa)." },
  { "en": "Apa Itu Session Storage (Penyimpanan Sesi)?", "id": "Web Storage, Data Per Sesi (Tab Tertutup Hilang)." },
  { "en": "Apa Perbedaan (Difference) Local Dan Session Storage?", "id": "Local (Persisten), Session (Per Sesi Tab)." },
  { "en": "Apa Itu Cookie (Kuki) HTTP?", "id": "Data Kecil, Disimpan Browser (Server Set)." },
  { "en": "Apa Kegunaan (Use) Cookie (Kuki) HTTP?", "id": "Manajemen Sesi, Personalisasi, Pelacakan (Tracking)." },
  { "en": "Apa Itu Cookie (Kuki) Pihak Pertama?", "id": "Cookie, Dibuat Domain, Yang Dikunjungi Pengguna." },
  { "en": "Apa Itu Cookie (Kuki) Pihak Ketiga?", "id": "Cookie, Dibuat Domain Berbeda (Iklan, Pelacakan)." },
  { "en": "Apa Atribut (Attribute) Cookie 'Expires'?", "id": "Mengatur Waktu Kadaluarsa, Cookie (Persisten)." },
  { "en": "Apa Atribut (Attribute) Cookie 'Max-Age'?", "id": "Mengatur Durasi (Detik), Cookie (Modern)." },
  { "en": "Apa Atribut (Attribute) Cookie 'Domain'?", "id": "Menentukan Host, Yang Dapat Menerima Cookie." },
  { "en": "Apa Atribut (Attribute) Cookie 'Path'?", "id": "Menentukan Jalur URL, (Harus Ada, Cookie Dikirim)." },
  { "en": "Apa Itu Cookie (Kuki) Zombie?", "id": "Cookie, Dibuat Ulang Otomatis, (Setelah Dihapus)." },
  { "en": "Apa Itu Supercookie (Kuki Super)?", "id": "Cookie, Disimpan, Lokasi Non-Standar (Sulit Dihapus)." },
  { "en": "Apa Itu Model (Model) OSI (Open Systems Interconnection)?", "id": "Model Konseptual, Tujuh Lapisan, (Fungsi Jaringan)." },
  { "en": "Apa Lapisan (Layer) 1 OSI?", "id": "Lapisan Fisik (Physical), (Transmisi Bit, Kabel)." },
  { "en": "Apa Lapisan (Layer) 2 OSI?", "id": "Lapisan Data Link, (Alamat MAC, Switch)." },
  { "en": "Apa Lapisan (Layer) 3 OSI?", "id": "Lapisan Jaringan (Network), (Alamat IP, Router)." },
  { "en": "Apa Lapisan (Layer) 4 OSI?", "id": "Lapisan Transport (Transport), (Koneksi, TCP, UDP)." },
  { "en": "Apa Lapisan (Layer) 5 OSI?", "id": "Lapisan Sesi (Session), (Manajemen Sesi Komunikasi)." },
  { "en": "Apa Lapisan (Layer) 6 OSI?", "id": "Lapisan Presentasi (Presentation), (Format Data, Enkripsi)." },
  { "en": "Apa Lapisan (Layer) 7 OSI?", "id": "Lapisan Aplikasi (Application), (Protokol HTTP, FTP)." },
  { "en": "Apa Itu PDU (Protocol Data Unit)?", "id": "Unit Data, Spesifik, Setiap Lapisan OSI." },
  { "en": "Apa PDU (Protocol Data Unit) Lapisan Fisik?", "id": "Bit (Satuan Data Bit)." },
  { "en": "Apa PDU (Protocol Data Unit) Lapisan Data Link?", "id": "Frame (Bingkai Data)." },
  { "en": "Apa PDU (Protocol Data Unit) Lapisan Jaringan?", "id": "Paket (Packet Data)." },
  { "en": "Apa PDU (Protocol Data Unit) Lapisan Transport?", "id": "Segmen (TCP), Datagram (UDP)." },
  { "en": "Apa PDU (Protocol Data Unit) Lapisan Aplikasi?", "id": "Data (Informasi Pengguna)." },
  { "en": "Apa Itu Enkapsulasi (Encapsulation) Data Jaringan?", "id": "Proses Menambahkan, Header, (Setiap Lapisan OSI)." },
  { "en": "Apa Itu Dekapsulasi (De-encapsulation) Data Jaringan?", "id": "Proses Melepas, Header, (Setiap Lapisan OSI)." },
  { "en": "Apa Model (Model) TCP/IP (Transmission Control Protocol/Internet Protocol)?", "id": "Model Protokol Jaringan, (Empat Lapisan Praktis)." },
  { "en": "Apa Lapisan (Layer) Network Access TCP/IP?", "id": "Gabungan Fisik, Data Link OSI (Hardware)." },
  { "en": "Apa Lapisan (Layer) Internet TCP/IP?", "id": "Setara Lapisan Jaringan OSI (IP, Routing)." },
  { "en": "Apa Lapisan (Layer) Transport TCP/IP?", "id": "Setara Lapisan Transport OSI (TCP, UDP)." },
  { "en": "Apa Lapisan (Layer) Aplikasi TCP/IP?", "id": "Gabungan Sesi, Presentasi, Aplikasi OSI." },
  { "en": "Apa Perbedaan (Difference) OSI Dan TCP/IP?", "id": "OSI (Teoritis, 7 Lapis), TCP/IP (Praktis, 4 Lapis)." },
  { "en": "Apa Itu Flow Control (Kontrol Aliran) Transport?", "id": "Mekanisme, (Mengatur Laju Pengiriman Data, (TCP))." },
  { "en": "Apa Itu Sliding Window (Jendela Geser) TCP?", "id": "Metode Flow Control, (Mengirim Banyak Segmen, (ACK))." },
  { "en": "Apa Itu Congestion Control (Kontrol Kemacetan) TCP?", "id": "Mekanisme, (Mencegah Jaringan, Terlalu Penuh, (Lambat))." },
  { "en": "Apa Itu Slow Start (Mulai Lambat) TCP?", "id": "Algoritma, (Menaikkan Laju Pengiriman, (Bertahap))." },
  { "en": "Apa Itu Jabat Tangan (Handshake) 3 Arah TCP?", "id": "Proses, (Membangun Koneksi TCP, (SYN, SYN-ACK, ACK))." },
  { "en": "Apa Itu Paket (Packet) SYN?", "id": "Paket, (Memulai, Meminta Koneksi TCP, (Synchronize))." },
  { "en": "Apa Itu Paket (Packet) ACK?", "id": "Paket, (Konfirmasi Penerimaan Data, (Acknowledge))." },
  { "en": "Apa Itu Paket (Packet) FIN?", "id": "Paket, (Mengakhiri Koneksi TCP, (Finish))." },
  { "en": "Apa Itu Paket (Packet) RST?", "id": "Paket, (Reset, (Menghentikan Koneksi Paksa))." },
  { "en": "Apa Itu Port (Port) Well-Known?", "id": "Nomor Port, (0-1023, (Layanan Standar, (HTTP 80)))." },
  { "en": "Apa Itu Port (Port) Registered?", "id": "Nomor Port, (1024-49151, (Aplikasi Terdaftar IANA))." },
  { "en": "Apa Itu Port (Port) Dinamis (Ephemeral)?", "id": "Nomor Port, (49152-65535, (Sisi Klien, (Sementara)))." },
  { "en": "Apa Itu Multiplexing (Multipleksi) Jaringan?", "id": "Menggabungkan Banyak Sinyal, (Satu Saluran Komunikasi)." },
  { "en": "Apa Itu Demultiplexing (Demultipleksi) Jaringan?", "id": "Memisahkan Sinyal Gabungan, (Kembali Ke Asal)." },
  { "en": "Apa Itu FDM (Frequency Division Multiplexing)?", "id": "Multipleksi, (Membagi Saluran, (Frekuensi Berbeda))." },
  { "en": "Apa Itu TDM (Time Division Multiplexing)?", "id": "Multipleksi, (Membagi Saluran, (Slot Waktu Berbeda))." },
  { "en": "Apa Itu Statistical TDM (Time Division Multiplexing)?", "id": "TDM Dinamis, (Alokasi Slot Waktu, (Sesuai Permintaan))." },
  { "en": "Apa Itu Circuit Switching (Pensaklaran Sirkuit)?", "id": "Metode Jaringan, (Jalur Komunikasi Khusus, (Telepon))." },
  { "en": "Apa Itu Packet Switching (Pensaklaran Paket)?", "id": "Metode Jaringan, (Data Dipecah, Paket, (Internet))." },
  { "en": "Perbedaan (Difference) Circuit Dan Packet Switching?", "id": "Circuit (Dedicated, Tetap), Packet (Shared, Dinamis)." },
  { "en": "Apa Itu Jaringan (Network) Connection-Oriented?", "id": "Membutuhkan Koneksi, (Setup Sesi, (TCP, Circuit))." },
  { "en": "Apa Itu Jaringan (Network) Connectionless?", "id": "Tanpa Setup Sesi, (Kirim Langsung, (UDP, Packet))." },
  { "en": "Apakah TCP (Transmission Control Protocol) Connection-Oriented?", "id": "Ya, TCP (Transmission Control Protocol) Membangun Koneksi (Handshake)." },
  { "en": "Apakah UDP (User Datagram Protocol) Connectionless?", "id": "Ya, UDP (User Datagram Protocol) Mengirim Tanpa Koneksi." },
  { "en": "Apa Keuntungan (Advantage) UDP (User Datagram Protocol)?", "id": "Cepat, Overhead Rendah, (Streaming, Game)." },
  { "en": "Apa Kerugian (Disadvantage) UDP (User Datagram Protocol)?", "id": "Tidak Andal, (Tanpa Jaminan Urutan, Pengiriman)." },
  { "en": "Apa Keuntungan (Advantage) TCP (Transmission Control Protocol)?", "id": "Andal, (Jaminan Pengiriman, Urutan Data)." },
  { "en": "Apa Kerugian (Disadvantage) TCP (Transmission Control Protocol)?", "id": "Lebih Lambat, (Overhead Handshake, Kontrol Aliran)." },
  { "en": "Apa Itu Checksum (Jumlah Periksa) UDP?", "id": "Opsional, (Mendeteksi Kesalahan Data, (Integritas))." },
  { "en": "Apa Itu Lapisan (Layer) Fisik (Physical) Ethernet?", "id": "Standar IEEE 802.3, (Kabel, Sinyal Listrik)." },
  { "en": "Apa Itu Kabel (Cable) Kategori 5e (Cat 5e)?", "id": "Kabel UTP, (Mendukung 1 Gbps, (Gigabit Ethernet))." },
  { "en": "Apa Itu Kabel (Cable) Kategori 6 (Cat 6)?", "id": "Kabel UTP, (Mendukung 10 Gbps, (Jarak Pendek))." },
  { "en": "Apa Itu Kabel (Cable) Kategori 6a (Cat 6a)?", "id": "Kabel UTP, (Mendukung 10 Gbps, (Jarak Penuh 100m))." },
  { "en": "Apa Itu Kabel (Cable) Serat Optik (Fiber Optic)?", "id": "Transmisi Data, (Menggunakan Pulsa Cahaya, (Sangat Cepat))." },
  { "en": "Apa Keuntungan (Advantage) Serat Optik?", "id": "Kecepatan Tinggi, (Jarak Jauh, (Tahan Interferensi))." },
  { "en": "Apa Kerugian (Disadvantage) Serat Optik?", "id": "Biaya Mahal, (Instalasi Sulit, (Rapuh))." },
  { "en": "Apa Itu Interferensi (Interference) Elektromagnetik (EMI)?", "id": "Gangguan Sinyal, (Disebabkan Sumber Listrik Eksternal)." },
  { "en": "Apa Itu Crosstalk (Pembicaraan Silang) Kabel?", "id": "Gangguan Sinyal, (Antar Pasangan Kawat Berdekatan)." },
  { "en": "Apa Fungsi (Function) Pilinan Kabel UTP?", "id": "Mengurangi Interferensi Elektromagnetik, Crosstalk." },
  { "en": "Apa Itu Kabel (Cable) Straight-Through (Lurus)?", "id": "Urutan Pin Sama, (Menghubungkan Perangkat Berbeda)." },
  { "en": "Contoh (Example) Penggunaan Kabel Straight-Through?", "id": "Komputer Ke Switch, Router Ke Switch." },
  { "en": "Apa Itu Kabel (Cable) Crossover (Silang)?", "id": "Urutan Pin Silang, (Menghubungkan Perangkat Sejenis)." },
  { "en": "Contoh (Example) Penggunaan Kabel Crossover?", "id": "Komputer Ke Komputer, Switch Ke Switch." },
  { "en": "Apa Itu Auto-MDIX (Automatic Medium-Dependent Interface Crossover)?", "id": "Fitur Port Jaringan, (Otomatis Deteksi Tipe Kabel)." },
  { "en": "Apa Itu Kabel (Cable) Rollover (Konsol)?", "id": "Kabel Khusus, (Mengakses Port Konsol Perangkat Jaringan)." },
  { "en": "Apa Itu Port (Port) Konsol (Console)?", "id": "Port Manajemen, (Akses CLI Langsung, Perangkat Jaringan)." },
  { "en": "Apa Itu Out-of-Band (OOB) Management?", "id": "Manajemen Perangkat, (Menggunakan Jaringan Terpisah, (Konsol))." },
  { "en": "Apa Itu In-Band (IB) Management?", "id": "Manajemen Perangkat, (Menggunakan Jaringan Produksi, (SSH, Telnet))." },
  { "en": "Apa Itu Protokol (Protocol) CDP (Cisco Discovery Protocol)?", "id": "Protokol Cisco, (Menemukan Informasi Perangkat Tetangga)." },
  { "en": "Apa Itu Protokol (Protocol) LLDP (Link Layer Discovery Protocol)?", "id": "Protokol Standar, (Mirip CDP, (Vendor-Neutral))." },
  { "en": "Apa Itu Loopback (Antarmuka Loopback) Router?", "id": "Antarmuka Virtual, (Selalu Aktif, (Testing, Identifikasi))." },
  { "en": "Apa Itu VLAN (Virtual Local Area Network) Tagging?", "id": "Menambahkan Tag (Label), (Frame Ethernet, (Identifikasi VLAN))." },
  { "en": "Apa Itu Protokol (Protocol) IEEE 802.1Q?", "id": "Standar VLAN Tagging, (Trunking, (Antar Switch))." },
  { "en": "Apa Itu Native (Asli) VLAN?", "id": "VLAN, (Tidak Diberi Tag, (Link Trunk 802.1Q))." },
  { "en": "Apa Itu Port (Port) Akses (Access) Switch?", "id": "Port Switch, (Dimiliki, Satu VLAN Tunggal)." },
  { "en": "Apa Itu Port (Port) Trunk (Trunk) Switch?", "id": "Port Switch, (Membawa Trafik, Banyak VLAN)." },
  { "en": "Apa Itu Inter-VLAN (Antar-VLAN) Routing?", "id": "Proses Routing, (Trafik Antara, VLAN Berbeda, (Router))." },
  { "en": "Apa Itu Router-on-a-Stick (ROAS)?", "id": "Metode Inter-VLAN, (Satu Link Trunk, Router)." },
  { "en": "Apa Itu Switch (Saklar) Multilayer (Layer 3)?", "id": "Switch, (Fungsi Routing, (Lapisan 3, (Cepat))." },
  { "en": "Apa Itu SVI (Switch Virtual Interface)?", "id": "Antarmuka VLAN Virtual, (Switch Multilayer, (IP))." },
  { "en": "Apa Itu Logika Proposisional?", "id": "Studi Logika, Proposisi (Benar/Salah)." },
  { "en": "Apa Itu Proposisi (Proposition)?", "id": "Pernyataan, Bernilai Benar, Atau Salah." },
  { "en": "Apa Itu Operator Logika Konjungsi (AND)?", "id": "Operator Logis (Dan, Simbol ∧)." },
  { "en": "Kapan Konjungsi (AND) Bernilai Benar?", "id": "Jika, Semua Proposisi, Bernilai Benar." },
  { "en": "Apa Itu Operator Logika Disjungsi (OR)?", "id": "Operator Logis (Atau, Simbol ∨)." },
  { "en": "Kapan Disjungsi (OR) Bernilai Salah?", "id": "Jika, Semua Proposisi, Bernilai Salah." },
  { "en": "Apa Itu Operator Logika Negasi (NOT)?", "id": "Operator Logis (Bukan, Simbol ¬)." },
  { "en": "Apa Fungsi (Function) Negasi (NOT)?", "id": "Membalik Nilai, Kebenaran (Benar Jadi Salah)." },
  { "en": "Apa Itu Tabel (Table) Kebenaran?", "id": "Tabel, Menunjukkan Hasil, Operasi Logika." },
  { "en": "Apa Itu Tautologi (Tautology)?", "id": "Proposisi Majemuk, Selalu Bernilai Benar." },
  { "en": "Apa Itu Kontradiksi (Contradiction)?", "id": "Proposisi Majemuk, Selalu Bernilai Salah." },
  { "en": "Apa Itu Kontingensi (Contingency)?", "id": "Proposisi, (Bisa Benar, Bisa Salah)." },
  { "en": "Apa Itu Implikasi (Implication) (Jika-Maka)?", "id": "Operator Logis (Kondisional, Simbol →)." },
  { "en": "Kapan Implikasi (Implication) Bernilai Salah?", "id": "Jika, Premis Benar, Konklusi Salah." },
  { "en": "Apa Itu Bi-Implikasi (Jika, Hanya Jika)?", "id": "Operator Logis (Bikondisional, Simbol ↔)." },
  { "en": "Kapan Bi-Implikasi (Bi-Implication) Benar?", "id": "Jika, Kedua Proposisi, Nilai Sama." },
  { "en": "Apa Itu Konvers (Converse) Implikasi?", "id": "Menukar Posisi, Premis, Konklusi (Q → P)." },
  { "en": "Apa Itu Invers (Inverse) Implikasi?", "id": "Menegasi Premis, Konklusi (¬P → ¬Q)." },
  { "en": "Apa Itu Kontraposisi (Contrapositive) Implikasi?", "id": "Menukar, Menegasi, (¬Q → ¬P)." },
  { "en": "Apa Itu Ekuivalensi (Equivalence) Logis?", "id": "Dua Proposisi, (Tabel Kebenaran Sama)." },
  { "en": "Apa Itu Hukum (Law) De Morgan?", "id": "Aturan Ekuivalensi, (Negasi Konjungsi, Disjungsi)." },
  { "en": "Apa Itu Logika (Logic) Predikat?", "id": "Logika, (Menggunakan Variabel, Kuantifikator, Predikat)." },
  { "en": "Apa Itu Kuantifikator (Quantifier) Universal?", "id": "Kuantifikator, (Untuk Semua, Simbol ∀)." },
  { "en": "Apa Itu Kuantifikator (Quantifier) Eksistensial?", "id": "Kuantifikator, (Terdapat, Simbol ∃)." },
  { "en": "Apa Itu Teori (Theory) Himpunan?", "id": "Cabang Matematika, (Studi Himpunan, Kumpulan Objek)." },
  { "en": "Apa Itu Himpunan (Set)?", "id": "Kumpulan Objek, (Berbeda, Terdefinisi Jelas)." },
  { "en": "Apa Itu Elemen (Element) Himpunan?", "id": "Objek, Anggota, Dalam Suatu Himpunan." },
  { "en": "Apa Itu Himpunan (Set) Kosong?", "id": "Himpunan, (Tidak Memiliki Elemen, (Ø))." },
  { "en": "Apa Itu Himpunan (Set) Semesta?", "id": "Himpunan, (Memuat Semua Objek, (Dibicarakan))." },
  { "en": "Apa Itu Kardinalitas (Cardinality) Himpunan?", "id": "Jumlah Elemen Berbeda, Dalam Himpunan." },
  { "en": "Apa Itu Himpunan (Set) Bagian (Subset)?", "id": "Himpunan A, (Subset B, (Semua Elemen A Di B))." },
  { "en": "Apa Itu Himpunan (Set) Kuasa (Power Set)?", "id": "Himpunan, (Semua Himpunan Bagian, (Suatu Himpunan))." },
  { "en": "Apa Itu Operasi (Operation) Gabungan (Union)?", "id": "Menggabungkan Elemen, (Dua Himpunan, (Simbol ∪))." },
  { "en": "Apa Itu Operasi (Operation) Irisan (Intersection)?", "id": "Elemen Yang Sama, (Dua Himpunan, (Simbol ∩))." },
  { "en": "Apa Itu Operasi (Operation) Selisih (Difference)?", "id": "Elemen A, (Yang Tidak Ada, Di B)." },
  { "en": "Apa Itu Operasi (Operation) Komplemen (Complement)?", "id": "Elemen Semesta, (Yang Bukan, Anggota Himpunan)." },
  { "en": "Apa Itu Diagram (Diagram) Venn?", "id": "Diagram, (Visualisasi Hubungan, Antar Himpunan)." },
  { "en": "Apa Itu Produk (Product) Kartesian?", "id": "Operasi Himpunan, (Menghasilkan Pasangan Terurut)." },
  { "en": "Apa Itu Relasi (Relation)?", "id": "Himpunan Bagian, (Produk Kartesian, (Hubungan))." },
  { "en": "Apa Itu Relasi (Relation) Refleksif?", "id": "Setiap Elemen, (Berelasi, Dengan Dirinya Sendiri)." },
  { "en": "Apa Itu Relasi (Relation) Simetris?", "id": "Jika (A Relasi B), Maka (B Relasi A)." },
  { "en": "Apa Itu Relasi (Relation) Transitif?", "id": "Jika (A Relasi B), (B Relasi C), Maka (A Relasi C)." },
  { "en": "Apa Itu Relasi (Relation) Ekuivalensi?", "id": "Relasi, (Refleksif, Simetris, Dan Transitif)." },
  { "en": "Apa Itu Fungsi (Function)?", "id": "Relasi Khusus, (Setiap Input, Satu Output)." },
  { "en": "Apa Itu Domain (Daerah Asal) Fungsi?", "id": "Himpunan, (Semua Nilai Input, Yang Mungkin)." },
  { "en": "Apa Itu Kodomain (Codomain) Fungsi?", "id": "Himpunan, (Semua Nilai Output, Yang Mungkin)." },
  { "en": "Apa Itu Range (Daerah Hasil) Fungsi?", "id": "Himpunan, (Nilai Output Aktual, Fungsi)." },
  { "en": "Apa Itu Fungsi (Function) Injektif (One-to-One)?", "id": "Setiap Input Berbeda, (Output Berbeda)." },
  { "en": "Apa Itu Fungsi (Function) Surjektif (Onto)?", "id": "Setiap Output, (Memiliki Setidaknya, Satu Input)." },
  { "en": "Apa Itu Fungsi (Function) Bijektif?", "id": "Fungsi, (Injektif, Dan Surjektif, (Korespondensi))." },
  { "en": "Apa Itu Fungsi (Function) Invers?", "id": "Fungsi, (Membalik Pemetaan, Fungsi Bijektif)." },
  { "en": "Apa Itu Komposisi (Composition) Fungsi?", "id": "Menerapkan Satu Fungsi, (Hasil Fungsi Lain)." },
  { "en": "Apa Itu Matriks (Matrix)?", "id": "Susunan Bilangan, (Bentuk Baris, Kolom)." },
  { "en": "Apa Itu Ordo (Orde) Matriks?", "id": "Ukuran Matriks, (Jumlah Baris x Kolom)." },
  { "en": "Apa Itu Matriks (Matrix) Persegi?", "id": "Matriks, (Jumlah Baris, Sama Kolom)." },
  { "en": "Apa Itu Matriks (Matrix) Identitas?", "id": "Matriks Persegi, (Diagonal Satu, Lainnya Nol)." },
  { "en": "Apa Itu Transpos (Transpose) Matriks?", "id": "Menukar Posisi, (Baris Menjadi Kolom, (Matriks))." },
  { "en": "Apa Itu Penjumlahan (Addition) Matriks?", "id": "Menjumlahkan Elemen, (Posisi Sama, (Ordo Sama))." },
  { "en": "Apa Itu Perkalian (Multiplication) Skalar Matriks?", "id": "Mengalikan Skalar, (Setiap Elemen Matriks)." },
  { "en": "Apa Itu Perkalian (Multiplication) Matriks?", "id": "Perkalian Baris, (Matriks Pertama, Kolom Kedua)." },
  { "en": "Apa Itu Determinan (Determinant) Matriks?", "id": "Nilai Skalar, (Matriks Persegi, (Linear Algebra))." },
  { "en": "Apa Itu Invers (Inverse) Matriks?", "id": "Matriks, (Jika Dikalikan, Hasil Identitas)." },
  { "en": "Apa Itu Teori (Theory) Graf?", "id": "Studi Matematika, (Graf, (Struktur Jaringan))." },
  { "en": "Apa Itu Graf (Graph)?", "id": "Kumpulan Simpul (Vertex), Sisi (Edge)." },
  { "en": "Apa Itu Simpul (Vertex) Graf?", "id": "Titik, Node, (Entitas Dalam Graf)." },
  { "en": "Apa Itu Sisi (Edge) Graf?", "id": "Garis, (Menghubungkan Dua Simpul, (Graf))." },
  { "en": "Apa Itu Graf (Graph) Berarah (Directed)?", "id": "Graf, (Sisi Memiliki, Arah (Panah))." },
  { "en": "Apa Itu Graf (Graph) Tak Berarah?", "id": "Graf, (Sisi Tidak Memiliki, Arah)." },
  { "en": "Apa Itu Graf (Graph) Berbobot (Weighted)?", "id": "Graf, (Sisi Memiliki, Nilai Bobot)." },
  { "en": "Apa Itu Derajat (Degree) Simpul?", "id": "Jumlah Sisi, (Terhubung, Ke Simpul)." },
  { "en": "Apa Itu Jalur (Path) Graf?", "id": "Urutan Simpul, (Terhubung Oleh Sisi, (Berurutan))." },
  { "en": "Apa Itu Siklus (Cycle) Graf?", "id": "Jalur, (Simpul Awal, Akhir Sama)." },
  { "en": "Apa Itu Graf (Graph) Terhubung?", "id": "Terdapat Jalur, (Antara Setiap, Pasang Simpul)." },
  { "en": "Apa Itu Pohon (Tree) Graf?", "id": "Graf Terhubung, (Tanpa Siklus, (Hierarki))." },
  { "en": "Apa Itu Akar (Root) Pohon?", "id": "Simpul Teratas, (Pohon Berakar, (Hierarki))." },
  { "en": "Apa Itu Daun (Leaf) Pohon?", "id": "Simpul, (Tidak Memiliki Anak, (Derajat Satu))." },
  { "en": "Apa Itu Pohon (Tree) Biner?", "id": "Pohon, (Setiap Simpul, (Maksimal Dua Anak))." },
  { "en": "Apa Itu Induksi (Induction) Matematika?", "id": "Metode Pembuktian, (Pernyataan Bilangan Bulat)." },
  { "en": "Apa Itu Basis (Basis) Induksi?", "id": "Langkah Pertama, (Membuktikan Benar, (Nilai Awal))." },
  { "en": "Apa Itu Hipotesis (Hypothesis) Induksi?", "id": "Asumsi, (Pernyataan Benar, (Untuk Nilai K))." },
  { "en": "Apa Itu Langkah (Step) Induksi?", "id": "Membuktikan, (Jika Benar K, (Benar K+1))." },
  { "en": "Apa Itu Kombinatorika (Combinatorics)?", "id": "Studi Matematika, (Menghitung, Mengatur Objek)." },
  { "en": "Apa Itu Permutasi (Permutation)?", "id": "Susunan Objek, (Urutan Diperhatikan, (P(n,k)))." },
  { "en": "Apa Itu Kombinasi (Combination)?", "id": "Pemilihan Objek, (Urutan Tidak Diperhatikan, (C(n,k)))." },
  { "en": "Apa Itu Prinsip (Principle) Sarang Merpati?", "id": "Prinsip Dasar, (Penempatan Objek, Wadah)." },
  { "en": "Apa Itu Teori (Theory) Probabilitas?", "id": "Studi Matematika, (Peluang, Ketidakpastian, Acak)." },
  { "en": "Apa Itu Ruang (Space) Sampel?", "id": "Himpunan, (Semua Hasil, Yang Mungkin, (Eksperimen))." },
  { "en": "Apa Itu Kejadian (Event)?", "id": "Himpunan Bagian, (Ruang Sampel, (Hasil))." },
  { "en": "Apa Itu Probabilitas (Probability) Kejadian?", "id": "Ukuran Peluang, (Kejadian Terjadi, (0-1))." },
  { "en": "Apa Itu Probabilitas (Probability) Bersyarat?", "id": "Peluang, (Kejadian A, (Mengingat B Terjadi))." },
  { "en": "Apa Itu Teorema (Theorem) Bayes?", "id": "Menghitung Probabilitas Bersyarat, (Informasi Baru)." },
  { "en": "Apa Itu Variabel (Variable) Acak?", "id": "Variabel, (Nilainya Hasil, Fenomena Acak)." },
  { "en": "Apa Itu Ekspektasi (Expectation) (Nilai Harapan)?", "id": "Rata-Rata Tertimbang, (Hasil Variabel Acak)." },
  { "en": "Apa Itu Varian (Variance)?", "id": "Ukuran Sebaran, (Nilai Variabel Acak)." },
  { "en": "Apa Itu Deviasi (Deviation) Standar?", "id": "Akar Kuadrat, Varian, (Ukuran Sebaran)." },
  { "en": "Apa Itu Distribusi (Distribution) Probabilitas?", "id": "Fungsi, (Menjelaskan Probabilitas, Setiap Hasil)." },
  { "en": "Apa Itu Distribusi (Distribution) Normal?", "id": "Distribusi Kontinu, (Bentuk Lonceng, (Umum))." },
  { "en": "Apa Itu Distribusi (Distribution) Binomial?", "id": "Distribusi Diskrit, (Jumlah Sukses, (N Percobaan))." },
  { "en": "Apa Itu Distribusi (Distribution) Poisson?", "id": "Distribusi Diskrit, (Jumlah Kejadian, (Interval Waktu))." },
  { "en": "Apa Itu Regresi (Regression) Linier?", "id": "Model Statistik, (Hubungan Linier, Antar Variabel)." },
  { "en": "Apa Itu Korelasi (Correlation)?", "id": "Ukuran Statistik, (Hubungan Antar, Dua Variabel)." },
  { "en": "Apa Itu Koefisien (Coefficient) Korelasi?", "id": "Nilai, (Mengukur Kekuatan, Arah Korelasi, (-1, 1))." },
  { "en": "Apa Itu Kausalitas (Causality)?", "id": "Hubungan Sebab, Akibat, (Antar Dua Kejadian)." },
  { "en": "Korelasi (Correlation) Tidak Menyiratkan Kausalitas?", "id": "Hubungan Statistik, (Bukan Berarti, Sebab-Akibat)." },
  { "en": "Apa Itu Statistika (Statistics) Deskriptif?", "id": "Metode, (Meringkas, Menggambarkan Data, (Mean))." },
  { "en": "Apa Itu Statistika (Statistics) Inferensial?", "id": "Metode, (Menarik Kesimpulan, Populasi, (Sampel))." },
  { "en": "Apa Itu Populasi (Population) Statistika?", "id": "Keseluruhan Grup, (Objek Penelitian, (Total))." },
  { "en": "Apa Itu Sampel (Sample) Statistika?", "id": "Himpunan Bagian, (Representatif, Dari Populasi)." },
  { "en": "Apa Itu Mean (Rata-Rata)?", "id": "Ukuran Tendensi Sentral, (Jumlah Dibagi Banyaknya)." },
  { "en": "Apa Itu Median (Nilai Tengah)?", "id": "Nilai Tengah, (Data Yang Telah Diurutkan)." },
  { "en": "Apa Itu Modus (Mode)?", "id": "Nilai, (Paling Sering Muncul, Dalam Data)." },
  { "en": "Apa Itu Rentang (Range) Data?", "id": "Selisih, (Nilai Maksimum, Dan Minimum)." },
  { "en": "Apa Itu Kuartil (Quartile)?", "id": "Nilai, (Membagi Data Terurut, (Empat Bagian))." },
  { "en": "Apa Itu Rentang (Range) Antarkuartil (IQR)?", "id": "Selisih, (Kuartil Ketiga (Q3), Kuartil Pertama (Q1))." },
  { "en": "Apa Itu Diagram (Diagram) Batang (Bar)?", "id": "Grafik, (Visualisasi Data Kategorikal, (Batang Persegi))." },
  { "en": "Apa Itu Diagram (Diagram) Lingkaran (Pie)?", "id": "Grafik, (Visualisasi Proporsi Data, (Lingkaran))." },
  { "en": "Apa Itu Histogram (Histogram)?", "id": "Grafik, (Visualisasi Distribusi Frekuensi, (Data Numerik))." },
  { "en": "Apa Itu Diagram (Diagram) Pencar (Scatter Plot)?", "id": "Grafik, (Visualisasi Hubungan, (Antar Dua Variabel))." },
  { "en": "Apa Itu Box Plot (Diagram Kotak)?", "id": "Grafik, (Visualisasi Sebaran Data, (Kuartil, Pencilan))." },
  { "en": "Apa Itu Hipotesis (Hypothesis) Nol (H0)?", "id": "Asumsi Awal, (Tidak Ada Efek, (Status Quo))." },
  { "en": "Apa Itu Hipotesis (Hypothesis) Alternatif (H1)?", "id": "Asumsi, (Ada Efek, (Bertentangan H0))." },
  { "en": "Apa Itu Nilai-P (P-Value)?", "id": "Probabilitas, (Hasil Ekstrem, (Jika H0 Benar))." },
  { "en": "Apa Itu Signifikansi (Significance) Level (Alpha)?", "id": "Ambang Batas, (Menolak Hipotesis Nol (H0))." },
  { "en": "Kapan Menolak (Reject) Hipotesis Nol (H0)?", "id": "Jika, (Nilai-P (P-Value), Kurang Dari Alpha)." },
  { "en": "Apa Itu Aljabar (Algebra) Linier?", "id": "Cabang Matematika, (Studi Vektor, Matriks, Ruang)." },
  { "en": "Apa Itu Vektor (Vector)?", "id": "Objek Matematika, (Besaran, Arah, (Kumpulan Angka))." },
  { "en": "Apa Itu Skalar (Scalar)?", "id": "Besaran, (Hanya Memiliki Nilai, (Angka Tunggal))." },
  { "en": "Apa Itu Ruang (Space) Vektor?", "id": "Kumpulan Vektor, (Dapat Dijumlahkan, Dikalikan Skalar)." },
  { "en": "Apa Itu Kombinasi (Combination) Linier?", "id": "Penjumlahan Vektor, (Dikalikan Skalar, (Hasil Vektor))." },
  { "en": "Apa Itu Basis (Basis) Ruang Vektor?", "id": "Set Vektor, (Independen Linier, (Membangun Ruang))." },
  { "en": "Apa Itu Dimensi (Dimension) Ruang Vektor?", "id": "Jumlah Vektor, (Dalam Basis, (Ukuran Ruang))." },
  { "en": "Apa Itu Nilai (Value) Eigen?", "id": "Skalar Khusus, (Transformasi Linier, (Arah Vektor))." },
  { "en": "Apa Itu Vektor (Vector) Eigen?", "id": "Vektor Non-Nol, (Arah Tidak Berubah, (Transformasi))." },
  { "en": "Apa Itu Dekomposisi (Decomposition) Matriks?", "id": "Memfaktorkan Matriks, (Produk Matriks Sederhana)." },
  { "en": "Apa Itu Kalkulus (Calculus) Diferensial?", "id": "Studi Matematika, (Laju Perubahan, (Turunan))." },
  { "en": "Apa Itu Turunan (Derivative)?", "id": "Mengukur Laju, (Perubahan Sesaat, Fungsi)." },
  { "en": "Apa Itu Gradien (Gradient)?", "id": "Vektor Turunan Parsial, (Arah Kenaikan Tercepat)." },
  { "en": "Apa Itu Kalkulus (Calculus) Integral?", "id": "Studi Matematika, (Akumulasi, (Luas Di Bawah Kurva))." },
  { "en": "Apa Itu Integral (Integral) Tak Tentu?", "id": "Anti-Turunan, (Keluarga Fungsi, (Kebalikan Turunan))." },
  { "en": "Apa Itu Integral (Integral) Tentu?", "id": "Menghitung Akumulasi, (Total, (Antara Dua Batas))." },
  { "en": "Apa Itu Optimasi (Optimization) Matematika?", "id": "Mencari Nilai, (Minimum, Maksimum, (Suatu Fungsi))." },
  { "en": "Apa Itu Titik (Point) Kritis (Critical)?", "id": "Titik, (Dimana Turunan, (Nol, Tidak Terdefinisi))." },
  { "en": "Apa Itu Minimum (Minimum) Lokal?", "id": "Nilai Terendah, (Fungsi, (Area Sekitar Titik))." },
  { "en": "Apa Itu Maksimum (Maximum) Lokal?", "id": "Nilai Tertinggi, (Fungsi, (Area Sekitar Titik))." },
  { "en": "Apa Itu Metode (Method) Numerik?", "id": "Algoritma, (Solusi Aproksimasi, Masalah Matematika)." },
  { "en": "Apa Itu Interpolasi (Interpolation)?", "id": "Estimasi Nilai, (Di Antara, Titik Data Diketahui)." },
  { "en": "Apa Itu Ekstrapolasi (Extrapolation)?", "id": "Estimasi Nilai, (Di Luar, Jangkauan Data Diketahui)." },
  { "en": "Apa Itu Pencocokan (Fitting) Kurva?", "id": "Mencari Kurva, (Paling Cocok, Kumpulan Titik Data)." },
  { "en": "Apa Itu Metode (Method) Kuadrat Terkecil?", "id": "Metode Optimasi, (Meminimalkan Jumlah, Kuadrat Error)." },
  { "en": "Apa Itu Root (Akar) Finding?", "id": "Mencari Nilai Input, (Fungsi Menghasilkan, (Nol))." },
  { "en": "Apa Itu Metode (Method) Biseksi (Bisection)?", "id": "Algoritma Root-Finding, (Memecah Interval, (Iteratif))." },
  { "en": "Apa Itu Metode (Method) Newton-Raphson?", "id": "Algoritma Root-Finding, (Menggunakan Garis Tangen, (Cepat))." },
  { "en": "Apa Itu Integrasi (Integration) Numerik?", "id": "Aproksimasi Nilai, Integral Tentu, (Metode Numerik)." },
  { "en": "Apa Itu Aturan (Rule) Trapesium?", "id": "Metode Integrasi Numerik, (Aproksimasi Trapesium)." },
  { "en": "Apa Itu Aturan (Rule) Simpson?", "id": "Metode Integrasi Numerik, (Aproksimasi Parabola, (Akurat))." },
  { "en": "Apa Itu Simulasi (Simulation) Monte Carlo?", "id": "Teknik Komputasi, (Menggunakan Sampling Acak, (Hasil Numerik))." },
  { "en": "Apa Itu Bilangan (Number) Acak Semu?", "id": "Angka, (Dihasilkan Algoritma, (Terlihat Acak))." },
  { "en": "Apa Itu Generator (Generator) Bilangan Acak?", "id": "Algoritma, (Menghasilkan Urutan, Angka Acak Semu)." },
  { "en": "Apa Itu Seed (Benih) Generator Acak?", "id": "Nilai Awal, (Inisialisasi Generator, (Urutan Sama))." },
  { "en": "Apa Itu Riset (Research) Operasi (Operations)?", "id": "Disiplin, (Metode Analitis, (Pengambilan Keputusan))." },
  { "en": "Apa Itu Pemrograman (Programming) Linier?", "id": "Metode Optimasi, (Model Matematika Linier)." },
  { "en": "Apa Itu Fungsi (Function) Objektif?", "id": "Fungsi, (Yang Ingin, (Dimaksimalkan, Diminimalkan))." },
  { "en": "Apa Itu Kendala (Constraint) Optimasi?", "id": "Batasan, Kondisi, (Harus Dipenuhi, (Solusi))." },
  { "en": "Apa Itu Metode (Method) Simplex?", "id": "Algoritma, (Menyelesaikan Masalah, Pemrograman Linier)." },
  { "en": "Apa Itu Teori (Theory) Antrean (Queueing)?", "id": "Studi Matematika, (Garis Tunggu, (Antrean))." },
  { "en": "Apa Itu Model (Model) Antrean M/M/1?", "id": "Model Antrean, (Satu Server, (Kedatangan Poisson))." },
  { "en": "Apa Itu Analisis (Analysis) Rantai Markov?", "id": "Model Stokastik, (Transisi Status, (Probabilistik))." },
  { "en": "Apa Itu Teori (Theory) Permainan (Game)?", "id": "Studi Matematika, (Interaksi Strategis, (Antar Agen))." },
  { "en": "Apa Itu Ekuilibrium (Equilibrium) Nash?", "id": "Kondisi Teori Permainan, (Strategi Optimal, (Mengingat Lawan))." },
  { "en": "Apa Itu Permainan (Game) Zero-Sum?", "id": "Total Keuntungan, (Satu Pemain, (Sama Dengan Kerugian Lawan))." },
  { "en": "Apa Itu Dilema (Dilemma) Tahanan?", "id": "Contoh Teori Permainan, (Konflik Rasionalitas, (Individu vs Grup))." },
  { "en": "Apa Itu Sistem (System) Pakar (Expert)?", "id": "Program AI, (Meniru Keputusan, (Pakar Manusia))." },
  { "en": "Apa Itu Basis (Base) Pengetahuan (Knowledge)?", "id": "Komponen Sistem Pakar, (Menyimpan Fakta, Aturan)." },
  { "en": "Apa Itu Mesin (Engine) Inferensi?", "id": "Komponen Sistem Pakar, (Menarik Kesimpulan, (Logika))." },
  { "en": "Apa Itu Rantai (Chaining) Maju (Forward)?", "id": "Metode Inferensi, (Mulai Fakta, (Menuju Kesimpulan))." },
  { "en": "Apa Itu Rantai (Chaining) Mundur (Backward)?", "id": "Metode Inferensi, (Mulai Hipotesis, (Mencari Fakta))." },
  { "en": "Apa Itu Jaringan (Network) Semantik?", "id": "Representasi Pengetahuan, (Graf, (Node Konsep, Sisi Relasi))." },
  { "en": "Apa Itu Frame (Bingkai) AI?", "id": "Representasi Pengetahuan, (Struktur Data, (Slot, Atribut))." },
  { "en": "Apa Itu Logika (Logic) Fuzzy?", "id": "Logika, (Menangani Ketidakpastian, (Derajat Kebenaran))." },
  { "en": "Apa Itu Keanggotaan (Membership) Fuzzy?", "id": "Derajat, (Seberapa Jauh, Elemen, (Anggota Himpunan))." },
  { "en": "Apa Itu Algoritma (Algorithm) Genetika?", "id": "Algoritma Pencarian, (Terinspirasi, Evolusi Biologis, (Seleksi))." },
  { "en": "Apa Itu Populasi (Population) Algoritma Genetika?", "id": "Kumpulan Solusi, (Kandidat, (Individu, Kromosom))." },
  { "en": "Apa Itu Kebugaran (Fitness) Algoritma Genetika?", "id": "Ukuran Kualitas, (Seberapa Baik, Solusi, (Individu))." },
  { "en": "Apa Itu Seleksi (Selection) Algoritma Genetika?", "id": "Memilih Individu, (Reproduksi, (Berbasis Kebugaran))." },
  { "en": "Apa Itu Pindah Silang (Crossover) Algoritma Genetika?", "id": "Menggabungkan Gen, (Dua Induk, (Menciptakan Anak))." },
  { "en": "Apa Itu Mutasi (Mutation) Algoritma Genetika?", "id": "Perubahan Acak, (Gen, (Menjaga Keragaman))." },
  { "en": "Apa Itu Jaringan (Network) Saraf Tiruan?", "id": "Model Komputasi, (Terinspirasi, Otak Biologis, (Neuron))." },
  { "en": "Apa Itu Perceptron (Perseptron)?", "id": "Unit Neuron Tunggal, (Model JST, (Paling Sederhana))." },
  { "en": "Apa Itu Jaringan (Network) Saraf Maju (Feedforward)?", "id": "JST, (Aliran Informasi, (Satu Arah, (Input-Output)))." },
  { "en": "Apa Itu Jaringan (Network) Saraf Balik (Recurrent)?", "id": "JST, (Memiliki Koneksi, Siklus, (Memori))." },
  { "en": "Apa Itu Propagasi (Propagation) Balik (Backpropagation)?", "id": "Algoritma, (Melatih JST, (Menghitung Gradien Error))." },
  { "en": "Apa Itu Fungsi (Function) Aktivasi?", "id": "Fungsi, (Menentukan Output, Neuron, (Non-Linier))." },
  { "en": "Apa Itu Fungsi (Function) Aktivasi Sigmoid?", "id": "Fungsi Aktivasi, (Bentuk S, (Output 0-1))." },
  { "en": "Apa Itu Fungsi (Function) Aktivasi Tanh?", "id": "Fungsi Aktivasi, (Bentuk S, (Output -1 Sampai 1))." },
  { "en": "Apa Itu Fungsi (Function) Aktivasi ReLU?", "id": "Fungsi Aktivasi, (Rectified Linear Unit, (Populer))." },
  { "en": "Apa Itu Pembelajaran (Learning) Mendalam (Deep)?", "id": "Sub-Bidang ML, (JST, (Banyak Lapisan, (Hierarki))." },
  { "en": "Apa Itu CNN (Convolutional Neural Network)?", "id": "JST, (Deep Learning, (Efektif, (Visi Komputer)))." },
  { "en": "Apa Itu RNN (Recurrent Neural Network)?", "id": "JST, (Deep Learning, (Data Urutan, (Teks, Waktu)))." },
  { "en": "Apa Itu LSTM (Long Short-Term Memory)?", "id": "Arsitektur RNN, (Mengatasi Masalah, (Gradien Hilang))." },
  { "en": "Apa Itu Pembelajaran (Learning) Transfer?", "id": "Teknik ML, (Menggunakan Model, (Terlatih, (Tugas Baru))." },
  { "en": "Apa Itu Model (Model) Generatif?", "id": "Model ML, (Mempelajari Distribusi Data, (Menghasilkan Baru))." },
  { "en": "Apa Itu GAN (Generative Adversarial Network)?", "id": "Model Generatif, (Dua Jaringan, (Bersaing, (Generator, Diskriminator)))." }



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
