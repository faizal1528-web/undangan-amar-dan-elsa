<!DOCTYPE html>
<html lang="id" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Undangan Pernikahan - Amar & Elsa</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Elegan Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400..900;1,400..900&family=Plus+Jakarta+Sans:wght@300;400;500;600;700&family=Sacramento&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            overflow-x: hidden;
        }
        .font-serif-wedding {
            font-family: 'Playfair Display', serif;
        }
        .font-handwritten {
            font-family: 'Sacramento', cursive;
        }
        
        /* Custom Animations */
        @keyframes pulse-slow {
            0%, 100% { transform: scale(1); opacity: 0.9; }
            50% { transform: scale(1.05); opacity: 1; }
        }
        .animate-pulse-slow {
            animation: pulse-slow 8s infinite ease-in-out;
        }

        @keyframes rotate-vinyl {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }
        .animate-spin-slow {
            animation: rotate-vinyl 10s infinite linear;
        }

        /* Scroll Reveal base classes */
        .reveal {
            opacity: 0;
            transform: translateY(40px);
            transition: all 1.2s cubic-bezier(0.215, 0.610, 0.355, 1);
        }
        .reveal.active {
            opacity: 1;
            transform: translateY(0);
        }

        /* Glassmorphism styling */
        .glass-card {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.5);
        }

        /* Envelope Cover Overlay transition */
        #envelope {
            transition: transform 1.5s cubic-bezier(0.77, 0, 0.175, 1), opacity 1.2s ease;
        }
        #envelope.opened {
            transform: translateY(-100%);
            opacity: 0;
            pointer-events: none;
        }

        /* Canvas for floating petals */
        #petals-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 40;
        }
    </style>
</head>
<body class="bg-[#fcfbf7] text-stone-800">

    <!-- CANVAS UNTUK ANIMASI KELOPAK BUNGA GUGUR -->
    <canvas id="petals-canvas"></canvas>

    <!-- FLOATING MUSIC PLAYER BUTTON (Hanya aktif setelah undangan dibuka) -->
    <div id="music-control" class="fixed bottom-6 right-6 z-50 hidden">
        <button onclick="toggleMusic()" class="bg-[#124e41] text-[#eadeca] p-4 rounded-full shadow-2xl flex items-center justify-center transition-all duration-300 hover:scale-110 active:scale-95 focus:outline-none">
            <i id="music-icon" class="fa-solid fa-compact-disc text-2xl animate-spin-slow"></i>
        </button>
        <!-- Background Audio (Piano Romantis Instrumental) -->
        <audio id="bg-audio" loop>
            <source src="Bruno Mars - Risk It All Lyrics.mp3" type="audio/mp3">
        </audio>
    </div>

    <!-- ENVELOPE / OPENING COVER COVER OVERLAY -->
    <div id="envelope" class="fixed inset-0 z-50 flex items-center justify-center bg-[#0d2a23] px-6 text-center">
        <!-- Dekorasi Ornamen Latar -->
        <div class="absolute inset-0 opacity-10 bg-cover bg-center" style="background-image: url('DSC09449.jpg');"></div>
        <div class="absolute inset-0 bg-gradient-to-b from-[#0d2a23]/80 to-[#0d2a23]"></div>

        <div class="relative z-10 max-w-lg mx-auto space-y-6 text-[#eadeca] flex flex-col items-center">
            <p class="uppercase tracking-[0.3em] text-xs text-amber-200/80">The Wedding Celebration of</p>
            <h1 class="font-handwritten text-7xl md:text-8xl text-amber-100 my-2">Amar & Elsa</h1>
            <p class="text-sm md:text-base italic font-serif-wedding text-stone-300">Kepada Yth. Bapak/Ibu/Saudara/i</p>
            
            <div class="my-4 px-6 py-4 rounded-lg bg-[#143d33] border border-amber-200/20 w-80 shadow-inner">
                <span id="guest-name" class="font-serif-wedding font-semibold text-lg text-amber-100">Tamu Undangan</span>
            </div>

            <p class="text-xs text-stone-400 max-w-xs leading-relaxed">Tanpa mengurangi rasa hormat, kami mengundang Anda untuk hadir di hari bahagia kami.</p>
            
            <button onclick="openUndangan()" class="mt-4 bg-gradient-to-r from-amber-200 to-amber-100 text-[#0d2a23] font-bold px-8 py-3.5 rounded-full shadow-lg hover:shadow-amber-200/20 transition-all duration-300 transform hover:-translate-y-1 active:translate-y-0 uppercase tracking-widest text-xs flex items-center gap-2">
                <i class="fa-solid fa-envelope-open-text text-sm"></i> Buka Undangan
            </button>
        </div>
    </div>


    <!-- MAIN CONTENT (Sembunyi sebelum dibuka agar transisi mulus) -->
    <div id="main-content" class="opacity-0 transition-opacity duration-1000">

        <!-- HERO SECTION -->
        <section class="relative min-h-screen flex flex-col justify-center items-center text-center px-6 overflow-hidden">
            <!-- Background Image dengan Animasi Pembesaran Lambat -->
            <div class="absolute inset-0 scale-105 animate-pulse-slow bg-cover bg-center -z-10" 
                 style="background-image: linear-gradient(rgba(13, 42, 35, 0.8), rgba(13, 42, 35, 0.65)), url('DSC09449.jpg');">
            </div>

            <div class="space-y-6 text-[#eadeca] max-w-3xl">
                <div class="inline-block px-4 py-1.5 border border-amber-200/30 rounded-full text-xs uppercase tracking-[0.2em] bg-white/5 backdrop-blur-sm text-amber-200">
                    Menuju Hari Bahagia
                </div>
                <h1 class="font-handwritten text-8xl md:text-9xl text-amber-100">Amar & Elsa</h1>
                <p class="font-serif-wedding text-lg md:text-xl tracking-wider text-stone-200 max-w-xl mx-auto leading-relaxed">
                    Kami mengundang Anda untuk merayakan babak baru dalam perjalanan cinta kami.
                </p>

                <!-- COUNTDOWN TIMER -->
                <div id="countdown" class="grid grid-cols-4 gap-2 md:gap-4 max-w-md mx-auto pt-8">
                    <div class="bg-white/10 backdrop-blur-md p-3 md:p-4 rounded-xl border border-white/10 flex flex-col">
                        <span id="days" class="font-serif-wedding text-2xl md:text-3xl font-bold text-amber-200">00</span>
                        <span class="text-[10px] uppercase tracking-wider text-stone-300">Hari</span>
                    </div>
                    <div class="bg-white/10 backdrop-blur-md p-3 md:p-4 rounded-xl border border-white/10 flex flex-col">
                        <span id="hours" class="font-serif-wedding text-2xl md:text-3xl font-bold text-amber-200">00</span>
                        <span class="text-[10px] uppercase tracking-wider text-stone-300">Jam</span>
                    </div>
                    <div class="bg-white/10 backdrop-blur-md p-3 md:p-4 rounded-xl border border-white/10 flex flex-col">
                        <span id="minutes" class="font-serif-wedding text-2xl md:text-3xl font-bold text-amber-200">00</span>
                        <span class="text-[10px] uppercase tracking-wider text-stone-300">Menit</span>
                    </div>
                    <div class="bg-white/10 backdrop-blur-md p-3 md:p-4 rounded-xl border border-white/10 flex flex-col">
                        <span id="seconds" class="font-serif-wedding text-2xl md:text-3xl font-bold text-amber-200">00</span>
                        <span class="text-[10px] uppercase tracking-wider text-stone-300">Detik</span>
                    </div>
                </div>

                <div class="pt-8">
                    <a href="#mempelai" class="text-xs uppercase tracking-widest text-amber-100/70 hover:text-amber-100 transition-colors flex flex-col items-center gap-2">
                        Scroll Kebawah
                        <i class="fa-solid fa-chevron-down animate-bounce text-sm"></i>
                    </a>
                </div>
            </div>
        </section>


        <!-- AYAT SUCI / MUKADIMAH SECTION -->
        <section class="max-w-4xl mx-auto px-6 py-24 text-center reveal">
            <div class="inline-block text-[#124e41] text-3xl mb-4">
                <i class="fa-solid fa-leaf"></i>
            </div>
            <p class="font-serif-wedding italic text-lg text-stone-600 leading-relaxed max-w-2xl mx-auto">
                "Dan di antara tanda-tanda kekuasaan-Nya ialah Dia menciptakan untukmu isteri-isteri dari jenismu sendiri, supaya kamu cenderung dan merasa tenteram kepadanya, dan dijadikan-Nya diantaramu rasa kasih dan sayang. Sesungguhnya pada yang demikian itu benar-benar terdapat tanda-tanda bagi kaum yang berfikir."
            </p>
            <p class="font-semibold text-[#124e41] mt-6 tracking-widest text-sm uppercase">Ar-Rum: 21</p>
        </section>


        <!-- MEMPELAI SECTION -->
        <section id="mempelai" class="bg-gradient-to-b from-[#fcfbf7] to-[#f4f2ea] py-24 px-6">
            <div class="max-w-5xl mx-auto text-center space-y-16">
                
                <div class="space-y-3 reveal">
                    <h2 class="font-serif-wedding text-4xl md:text-5xl text-[#124e41]">Profil Mempelai</h2>
                    <p class="text-xs uppercase tracking-widest text-amber-700/80">Dengan memohon ridho Tuhan, perkenalkan kami:</p>
                </div>

                <div class="grid md:grid-cols-2 gap-12 items-stretch max-w-4xl mx-auto">
                    <!-- Groom -->
                    <div class="glass-card rounded-2xl p-8 shadow-xl border border-stone-200 flex flex-col justify-between items-center text-center reveal">
                        <div class="w-32 h-32 rounded-full overflow-hidden bg-stone-300 mb-6 border-4 border-amber-100 shadow-lg">
                            <!-- Placeholder atau foto mempelai -->
                            <img class="w-full h-full object-cover" src="pria.png" alt="Mempelai Pria">
                        </div>
                        <div class="space-y-3 flex-grow">
                            <h3 class="font-serif-wedding text-2xl font-bold text-stone-800">Muammar Hanafi</h3>
                            <p class="text-amber-800 font-semibold text-xs uppercase tracking-wider">Mempelai Pria</p>
                            <p class="text-sm text-stone-500 leading-relaxed">
                                Putra Pertama dari Bapak Danuri <br> & Ibu Ningsih
                            </p>
                        </div>
                        <div class="flex gap-3 mt-6">
                            <a href="https://www.instagram.com/hanafimuammar06?utm_source=ig_web_button_share_sheet&igsh=ZDNlZDc0MzIxNw==" class="text-stone-400 hover:text-amber-700"><i class="fa-brands fa-instagram text-xl"></i></a>
                        </div>
                    </div>

                    <!-- Bride -->
                    <div class="glass-card rounded-2xl p-8 shadow-xl border border-stone-200 flex flex-col justify-between items-center text-center reveal">
                        <div class="w-32 h-32 rounded-full overflow-hidden bg-stone-300 mb-6 border-4 border-amber-100 shadow-lg">
                            <img class="w-full h-full object-cover" src="wanita.png" alt="Mempelai Wanita">
                        </div>
                        <div class="space-y-3 flex-grow">
                            <h3 class="font-serif-wedding text-2xl font-bold text-stone-800">Elsa julyana</h3>
                            <p class="text-amber-800 font-semibold text-xs uppercase tracking-wider">Mempelai Wanita</p>
                            <p class="text-sm text-stone-500 leading-relaxed">
                                Putri ke dua dari Bapak Suherman <br> & Ibu Entin Santinah
                            </p>
                        </div>
                        <div class="flex gap-3 mt-6">
                            <a href="https://www.instagram.com/ecajlyna_?utm_source=ig_web_button_share_sheet&igsh=ZDNlZDc0MzIxNw==" class="text-stone-400 hover:text-amber-700"><i class="fa-brands fa-instagram text-xl"></i></a>
                        </div>
                    </div>
                </div>

            </div>
        </section>


        <!-- ACARA SECTION -->
        <section id="acara" class="py-24 px-6 bg-[#0d2a23] text-[#eadeca]">
            <div class="max-w-5xl mx-auto space-y-16 text-center">
                
                <div class="space-y-3 reveal">
                    <h2 class="font-serif-wedding text-4xl md:text-5xl text-amber-100">Waktu & Tempat Acara</h2>
                    <p class="text-xs uppercase tracking-widest text-amber-200/80">Rangkaian acara pernikahan kami</p>
                </div>

                <div class="grid md:grid-cols-2 gap-8 max-w-4xl mx-auto">
                    <!-- Akad Nikah -->
                    <div class="bg-white/5 backdrop-blur-md p-8 rounded-2xl border border-white/10 shadow-2xl space-y-6 text-center reveal">
                        <div class="text-amber-200 text-lg font-semibold uppercase tracking-widest flex items-center justify-center gap-2">
                            <i class="fa-solid fa-ring"></i> Akad Nikah
                        </div>
                        <div class="space-y-1">
                            <p class="font-serif-wedding text-xl font-semibold">Senin, 29 Juni 2026</p>
                            <p class="text-sm text-stone-300">Pukul 08.00 - 10.00 WIB</p>
                        </div>
                        <hr class="border-white/10 w-1/3 mx-auto">
                        <div class="space-y-1">
                            <p class="font-serif-wedding text-lg font-bold text-amber-100">Kp. Tugu Kaum</p>
                            <p class="text-xs text-stone-400 leading-relaxed">Bogor RT 02/RW 05 Desa Cibitung Tengah Kecamatan Tenjolaya Kabupaten Bogor</p>
                        </div>
                    </div>

                    <!-- Resepsi -->
                    <div class="bg-white/5 backdrop-blur-md p-8 rounded-2xl border border-white/10 shadow-2xl space-y-6 text-center reveal">
                        <div class="text-amber-200 text-lg font-semibold uppercase tracking-widest flex items-center justify-center gap-2">
                            <i class="fa-solid fa-champagne-glasses"></i> Resepsi Pernikahan
                        </div>
                        <div class="space-y-1">
                            <p class="font-serif-wedding text-xl font-semibold">Senin, 29 Juni 2026</p>
                            <p class="text-sm text-stone-300">Pukul 11.00 WIB S/d Selesai </p>
                        </div>
                        <hr class="border-white/10 w-1/3 mx-auto">
                        <div class="space-y-1">
                            <p class="font-serif-wedding text-lg font-bold text-amber-100">Kp. Tugu Kaum</p>
                            <p class="text-xs text-stone-400 leading-relaxed">Bogor RT 02/RW 05 Desa Cibitung Tengah Kecamatan Tenjolaya Kabupaten Bogor</p>
                        </div>
                    </div>
                </div>

                <!-- Google Maps Button -->
                <div class="reveal pt-6">
                    <a href="https://maps.app.goo.gl/1yGhWpyoJQQuyjcb7" target="_blank" class="inline-flex items-center gap-2 bg-gradient-to-r from-amber-200 to-amber-100 hover:from-amber-300 hover:to-amber-200 text-[#0d2a23] text-xs font-bold uppercase tracking-widest px-6 py-3.5 rounded-full shadow-lg transition-transform hover:-translate-y-0.5 active:translate-y-0">
                        <i class="fa-solid fa-map-location-dot"></i> Buka Google Maps Lokasi
                    </a>
                </div>

            </div>
        </section>


        <!-- GALERI FOTO EXQUISITE -->
        <section class="py-24 px-6 bg-[#fcfbf7] reveal">
            <div class="max-w-4xl mx-auto text-center space-y-12">
                <div class="space-y-3">
                    <h2 class="font-serif-wedding text-4xl md:text-5xl text-[#124e41]">Momen Bahagia Kami</h2>
                    <p class="text-xs uppercase tracking-widest text-amber-700/80">Galeri Prewedding</p>
                </div>

                <!-- Highlight Frame featuring the supplied image: 17559407200351290105.jpeg -->
                <div class="relative group overflow-hidden rounded-2xl shadow-2xl border-8 border-white bg-stone-100 max-w-2xl mx-auto transition-transform duration-500 hover:scale-[1.01]">
                    <img src="DSC09433.jpg" alt="The Beautiful Gazebo Archway Scene" class="w-full h-auto object-cover transition-transform duration-700 group-hover:scale-105">
                    <div class="absolute inset-0 bg-gradient-to-t from-black/50 via-transparent to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex items-end p-6">
                        <p class="text-white font-serif-wedding text-lg italic">Menanti hari pernikahan yang indah bersama Anda...</p>
                    </div>
                </div>
            </div>
        </section>


        <!-- KADO DIGITAL / WEDDING GIFT -->
        <section class="py-24 px-6 bg-gradient-to-b from-[#fcfbf7] to-[#f4f2ea]">
            <div class="max-w-3xl mx-auto text-center space-y-12">
                
                <div class="space-y-3 reveal">
                    <h2 class="font-serif-wedding text-4xl md:text-5xl text-[#124e41]">Kado Digital</h2>
                    <p class="text-xs uppercase tracking-widest text-amber-700/80">Doa restu Anda adalah berkah paling berharga, namun jika ingin berbagi kado:</p>
                </div>

                <div class="grid sm:grid-cols-2 gap-6 max-w-2xl mx-auto reveal">
                    <!-- Bank Account Card 1 -->
                    <div class="bg-white p-6 rounded-2xl shadow-md border border-stone-200 text-center space-y-4">
                        <div class="flex justify-center items-center h-12">
                            <span class="text-2xl font-bold tracking-widest text-[#124e41]">DANA</span>
                        </div>
                        <div class="space-y-1">
                            <p class="text-xs text-stone-400 font-semibold uppercase tracking-widest">No. Dana</p>
                            <p id="norek1" class="font-mono text-lg font-bold text-stone-800">089517283688</p>
                        </div>
                        <p class="text-sm font-semibold text-stone-600">a.n. Muammar Hanafi</p>
                        <button onclick="copyToClipboard('123-00-112233-4', 'btn-copy-1')" id="btn-copy-1" class="text-xs text-amber-700 font-bold hover:text-amber-800 transition-colors uppercase tracking-wider flex items-center justify-center gap-1 mx-auto border border-amber-700/20 px-3 py-1.5 rounded-full">
                            <i class="fa-solid fa-copy"></i> Salin Rekening
                        </button>
                    </div>

                    <!-- Bank Account Card 2 -->
                    <div class="bg-white p-6 rounded-2xl shadow-md border border-stone-200 text-center space-y-4">
                        <div class="flex justify-center items-center h-12">
                            <span class="text-2xl font-bold tracking-widest text-[#124e41]">BANK BCA</span>
                        </div>
                        <div class="space-y-1">
                            <p class="text-xs text-stone-400 font-semibold uppercase tracking-widest">No. Rekening</p>
                            <p id="norek2" class="font-mono text-lg font-bold text-stone-800">7080630365</p>
                        </div>
                        <p class="text-sm font-semibold text-stone-600">a.n. Elsa Julyana</p>
                        <button onclick="copyToClipboard('890-1234-567', 'btn-copy-2')" id="btn-copy-2" class="text-xs text-amber-700 font-bold hover:text-amber-800 transition-colors uppercase tracking-wider flex items-center justify-center gap-1 mx-auto border border-amber-700/20 px-3 py-1.5 rounded-full">
                            <i class="fa-solid fa-copy"></i> Salin Rekening
                        </button>
                    </div>
                </div>

            </div>
        </section>


        <!-- RSVP & WISH WALL (BUKU TAMU INTERAKTIF) -->
        <section id="rsvp" class="py-24 px-6 bg-gradient-to-b from-[#f4f2ea] to-[#eadeca]">
            <div class="max-w-5xl mx-auto grid lg:grid-cols-5 gap-12 items-start">
                
                <!-- RSVP Form Block -->
                <div class="lg:col-span-2 glass-card p-8 rounded-3xl shadow-xl border border-white reveal space-y-6">
                    <div class="text-center space-y-2">
                        <h2 class="font-serif-wedding text-3xl text-[#124e41]">Kehadiran & Doa</h2>
                        <p class="text-xs text-stone-500 uppercase tracking-wider">Mohon konfirmasi kehadiran Anda</p>
                    </div>

                    <form id="rsvp-form" class="space-y-4 text-stone-700">
                        <div>
                            <label class="block text-xs font-semibold uppercase tracking-widest text-stone-500 mb-1">Nama Lengkap</label>
                            <input type="text" id="form-name" required placeholder="Masukkan nama Anda" class="w-full px-4 py-3 border border-stone-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-[#124e41] bg-white/70">
                        </div>

                        <div>
                            <label class="block text-xs font-semibold uppercase tracking-widest text-stone-500 mb-1">Status Kehadiran</label>
                            <select id="form-presence" required class="w-full px-4 py-3 border border-stone-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-[#124e41] bg-white/70">
                                <option value="Hadir">Saya Akan Hadir</option>
                                <option value="Tidak Hadir">Maaf, Tidak Bisa Hadir</option>
                                <option value="Ragu-Ragu">Masih Ragu-Ragu</option>
                            </select>
                        </div>

                        <div>
                            <label class="block text-xs font-semibold uppercase tracking-widest text-stone-500 mb-1">Pesan / Ucapan Selamat</label>
                            <textarea id="form-message" required rows="4" placeholder="Tulis ucapan dan doa hangat Anda..." class="w-full px-4 py-3 border border-stone-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-[#124e41] bg-white/70"></textarea>
                        </div>

                        <button type="submit" class="w-full bg-[#124e41] hover:bg-[#0d2a23] text-white font-bold py-3.5 rounded-xl transition-all shadow-lg text-xs uppercase tracking-widest flex items-center justify-center gap-2">
                            <i class="fa-solid fa-paper-plane"></i> Kirim Ucapan
                        </button>
                    </form>
                </div>

                <!-- Wish Wall / Dinding Harapan (Real-time update via script) -->
                <div class="lg:col-span-3 space-y-6 reveal">
                    <div class="space-y-2">
                        <h2 class="font-serif-wedding text-3xl text-[#124e41] flex items-center gap-2">
                            <i class="fa-solid fa-comments"></i> Dinding Harapan
                        </h2>
                        <p class="text-xs text-stone-600 uppercase tracking-wider">Doa & Ucapan hangat dari para sahabat & keluarga</p>
                    </div>

                    <!-- Scrollable Wall Container -->
                    <div id="wishes-container" class="max-h-[420px] overflow-y-auto space-y-4 pr-2">
                        <!-- Wishes will populate dynamically here -->
                        <div class="bg-white p-5 rounded-2xl shadow-sm border border-stone-100 flex items-start gap-4">
                            <div class="w-10 h-10 rounded-full bg-amber-100 text-[#124e41] flex items-center justify-center font-bold font-serif-wedding text-lg shrink-0">
                                S
                            </div>
                            <div class="space-y-1">
                                <div class="flex items-center gap-2">
                                    <span class="font-bold text-stone-800">Sarah Wijaya</span>
                                    <span class="text-[10px] bg-green-100 text-green-700 px-2 py-0.5 rounded-full font-semibold uppercase">Hadir</span>
                                </div>
                                <p class="text-sm text-stone-600 leading-relaxed">Selamat menempuh hidup baru Amar & Elsa! Semoga menjadi keluarga yang sakinah, mawaddah, warahmah.</p>
                                <span class="text-[10px] text-stone-400">2 jam yang lalu</span>
                            </div>
                        </div>

                        <div class="bg-white p-5 rounded-2xl shadow-sm border border-stone-100 flex items-start gap-4">
                            <div class="w-10 h-10 rounded-full bg-amber-100 text-[#124e41] flex items-center justify-center font-bold font-serif-wedding text-lg shrink-0">
                                B
                            </div>
                            <div class="space-y-1">
                                <div class="flex items-center gap-2">
                                    <span class="font-bold text-stone-800">Budi Santoso</span>
                                    <span class="text-[10px] bg-green-100 text-green-700 px-2 py-0.5 rounded-full font-semibold uppercase">Hadir</span>
                                </div>
                                <p class="text-sm text-stone-600 leading-relaxed">Selamat bahagia ya kawan! Maaf belum bisa hadir langsung, doa terbaik dari jauh untuk kebahagiaan kalian berdua.</p>
                                <span class="text-[10px] text-stone-400">4 jam yang lalu</span>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </section>


        <!-- OUTRO SECTION -->
        <section class="py-24 px-6 bg-[#0d2a23] text-center text-[#eadeca] relative overflow-hidden">
            <div class="absolute inset-0 scale-105 opacity-10 bg-cover bg-center" style="background-image: url('17559407200351290105.jpeg');"></div>
            <div class="relative z-10 max-w-xl mx-auto space-y-6">
                <p class="uppercase tracking-widest text-xs text-amber-200">Merupakan Suatu Kehormatan</p>
                <h2 class="font-handwritten text-7xl text-amber-100">Terima Kasih</h2>
                <p class="text-sm text-stone-300 max-w-sm mx-auto leading-relaxed">
                    Atas kehadiran serta doa restu yang Anda berikan kepada kami berdua di hari pernikahan ini.
                </p>
                <div class="pt-6">
                    <p class="font-handwritten text-4xl text-amber-100">Amar & Elsa</p>
                </div>
            </div>
        </section>


        <!-- FOOTER -->
        <footer class="bg-[#081d18] text-[#eadeca]/40 py-8 text-center text-xs tracking-widest uppercase">
            <p>© 2026 Amar & Elsa Wedding. All Rights Reserved.</p>
        </footer>

    </div>

    <!-- JAVASCRIPT & LOGIKA ANIMASI -->
    <script>
        // 1. Ambil Parameter Nama Tamu dari URL (misal: index.html?to=Nama+Tamu)
        const urlParams = new URLSearchParams(window.location.search);
        const guestName = urlParams.get('to');
        if (guestName) {
            document.getElementById('guest-name').innerText = guestName;
        }

        // 2. Kontrol Membuka Undangan
        function openUndangan() {
            // Sembunyikan Cover
            document.getElementById('envelope').classList.add('opened');
            // Tampilkan konten utama dengan animasi fade-in
            const mainContent = document.getElementById('main-content');
            mainContent.classList.remove('opacity-0');
            mainContent.classList.add('opacity-100');
            // Munculkan tombol musik melayang
            document.getElementById('music-control').classList.remove('hidden');
            // Mainkan musik romantis
            playMusic();
            // Jalankan deteksi gulir (reveal) setelah terbuka
            scrollReveal();
        }

        // 3. Kontrol Audio Romantis
        const audio = document.getElementById('bg-audio');
        const musicIcon = document.getElementById('music-icon');
        let isPlaying = false;

        function playMusic() {
            audio.play().then(() => {
                isPlaying = true;
                musicIcon.classList.add('animate-spin-slow');
            }).catch(e => console.log("Menunggu interaksi pengguna untuk audio."));
        }

        function toggleMusic() {
            if (isPlaying) {
                audio.pause();
                musicIcon.classList.remove('animate-spin-slow');
                isPlaying = false;
            } else {
                audio.play();
                musicIcon.classList.add('animate-spin-slow');
                isPlaying = true;
            }
        }

        // 4. Hitung Mundur Pernikahan (29 Juni 2026)
        const weddingDate = new Date('Jun 29, 2026 08:00:00').getTime();

        const x = setInterval(function() {
            const now = new Date().getTime();
            const distance = weddingDate - now;

            const d = Math.floor(distance / (1000 * 60 * 60 * 24));
            const h = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const m = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
            const s = Math.floor((distance % (1000 * 60)) / 1000);

            document.getElementById('days').innerText = String(d).padStart(2, '0');
            document.getElementById('hours').innerText = String(h).padStart(2, '0');
            document.getElementById('minutes').innerText = String(m).padStart(2, '0');
            document.getElementById('seconds').innerText = String(s).padStart(2, '0');

            if (distance < 0) {
                clearInterval(x);
                document.getElementById('countdown').innerHTML = "<div class='col-span-4 text-center text-amber-200 font-bold'>HARI BAHAGIA TELAH TIBA!</div>";
            }
        }, 1000);

        // 5. Animasi Gulir (Scroll Reveal / AOS buatan sendiri)
        function scrollReveal() {
            const reveals = document.querySelectorAll('.reveal');
            for (let i = 0; i < reveals.length; i++) {
                const windowHeight = window.innerHeight;
                const elementTop = reveals[i].getBoundingClientRect().top;
                const elementVisible = 100; // Trigger offset

                if (elementTop < windowHeight - elementVisible) {
                    reveals[i].classList.add('active');
                }
            }
        }
        window.addEventListener('scroll', scrollReveal);

        // 6. Buku Tamu / RSVP Interaktif (Update Dinding Harapan secara dinamis)
        const rsvpForm = document.getElementById('rsvp-form');
        const wishesContainer = document.getElementById('wishes-container');

        rsvpForm.addEventListener('submit', function(e) {
            e.preventDefault();

            const name = document.getElementById('form-name').value;
            const presence = document.getElementById('form-presence').value;
            const message = document.getElementById('form-message').value;

            // Buat inisial ikon pengirim
            const initial = name.charAt(0).toUpperCase();

            // Desain baru untuk ucapan yang disubmit
            const newWishHTML = `
                <div class="bg-white p-5 rounded-2xl shadow-sm border border-stone-100 flex items-start gap-4 transition-all duration-500 scale-95 opacity-0 animate-fade-in" style="animation: revealIn 0.5s forwards;">
                    <div class="w-10 h-10 rounded-full bg-amber-100 text-[#124e41] flex items-center justify-center font-bold font-serif-wedding text-lg shrink-0">
                        ${initial}
                    </div>
                    <div class="space-y-1">
                        <div class="flex items-center gap-2">
                            <span class="font-bold text-stone-800">${name}</span>
                            <span class="text-[10px] bg-green-100 text-green-700 px-2 py-0.5 rounded-full font-semibold uppercase">${presence}</span>
                        </div>
                        <p class="text-sm text-stone-600 leading-relaxed">${message}</p>
                        <span class="text-[10px] text-stone-400">Baru Saja</span>
                    </div>
                </div>
            `;

            // Suntik ke tumpukan paling atas container ucapan
            wishesContainer.insertAdjacentHTML('afterbegin', newWishHTML);

            // Reset formulir & berikan umpan balik sukses
            rsvpForm.reset();
            alert("Terima kasih! Ucapan selamat Anda telah dikirim dan ditampilkan di Dinding Harapan.");
        });

        // 7. Salin No Rekening / Copy to Clipboard
        function copyToClipboard(text, btnId) {
            navigator.clipboard.writeText(text).then(() => {
                const btn = document.getElementById(btnId);
                const originalText = btn.innerHTML;
                btn.innerHTML = `<i class="fa-solid fa-check"></i> Tersalin!`;
                btn.classList.add('text-green-600', 'border-green-600');
                
                setTimeout(() => {
                    btn.innerHTML = originalText;
                    btn.classList.remove('text-green-600', 'border-green-600');
                }, 2000);
            }).catch(err => {
                console.error("Gagal menyalin text: ", err);
            });
        }

        // 8. Efek Kelopak Bunga Berjatuhan menggunakan HTML5 Canvas
        const canvas = document.getElementById('petals-canvas');
        const ctx = canvas.getContext('2d');

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        const petalArray = [];
        const petalCount = 40; // Jumlah kelopak bunga di layar secara bersamaan

        class Petal {
            constructor() {
                this.x = Math.random() * canvas.width;
                this.y = Math.random() * canvas.height - canvas.height;
                this.size = Math.random() * 8 + 6;
                this.speedX = Math.random() * 1.5 - 0.5;
                this.speedY = Math.random() * 1 + 1;
                this.angle = Math.random() * 360;
                this.spin = Math.random() * 1 - 0.5;
                // Skema warna sakura kemerahan / krem transparan
                this.color = `rgba(254, 219, 219, ${Math.random() * 0.4 + 0.3})`;
            }

            update() {
                this.x += this.speedX;
                this.y += this.speedY;
                this.angle += this.spin;

                // Set ulang posisi jika kelopak telah keluar batas layar bawah
                if (this.y > canvas.height) {
                    this.y = -10;
                    this.x = Math.random() * canvas.width;
                }
            }

            draw() {
                ctx.save();
                ctx.translate(this.x, this.y);
                ctx.rotate((this.angle * Math.PI) / 180);
                ctx.fillStyle = this.color;
                
                // Menggambar kelopak kecil menggunakan kurva bezier
                ctx.beginPath();
                ctx.ellipse(0, 0, this.size, this.size / 1.5, 0, 0, 2 * Math.PI);
                ctx.fill();
                ctx.restore();
            }
        }

        function initPetals() {
            for (let i = 0; i < petalCount; i++) {
                petalArray.push(new Petal());
            }
        }

        function animatePetals() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            for (let i = 0; i < petalArray.length; i++) {
                petalArray[i].update();
                petalArray[i].draw();
            }
            requestAnimationFrame(animatePetals);
        }

        initPetals();
        animatePetals();
    </script>

    <!-- CSS khusus untuk animasi rsvp yang baru dikirim -->
    <style>
        @keyframes revealIn {
            0% {
                opacity: 0;
                transform: scale(0.9);
            }
            100% {
                opacity: 1;
                transform: scale(1);
            }
        }
    </style>
</body>
</html>
