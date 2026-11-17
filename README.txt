MySchool - Paket Hosting Vercel

ISI PAKET
- index.html : aplikasi MySchool yang telah diperbarui
- api/search.js : fungsi serverless Vercel untuk pencarian musik (Deezer + iTunes, tanpa API key)
- vercel.json : konfigurasi hosting statis Vercel

CARA DEPLOY
1. Ekstrak ZIP ini.
2. Buka https://vercel.com/ lalu login.
3. Pilih Add New -> Project. Jika menghubungkan GitHub, unggah isi folder ini ke repository lalu impor repository tersebut.
4. Framework Preset: Other. Build Command: kosongkan / None. Output Directory: ./ (root).
5. Deploy, lalu buka URL HTTPS yang diberikan Vercel.

CATATAN MUSIK
- Lagu YouTube diputar sebagai audio: layar pemutar hanya menampilkan sampul dan tombol (putar/jeda, geser posisi, sebelumnya/berikutnya). Di balik layar tetap dipakai YouTube IFrame Player API resmi, dengan pemutar disembunyikan. Tombol "Tampilkan video" di layar pemutar memunculkan videonya kapan saja. Jika browser meminta sentuhan langsung, pemutar muncul sementara; tekan Putar sekali.
- Cari musik: di menu Musik, kotak "Cari musik" memanggil /api/search (fungsi Vercel). Fungsi ini mengambil METADATA dari API publik Deezer dan iTunes Search (tanpa login/API key), lalu: mencocokkan judul + artis + relevansi; mengenali artis yang dicari dan menampilkan lagu aslinya lebih dulu; memisahkan Original / DJ / Remix / Cover (tab di hasil); mengutamakan lagu Indonesia (tombol "Utamakan lagu Indonesia", bisa dimatikan); dan mengurutkan dengan popularitas asli dari "rank" Deezer. Jika datanya tidak ada, popularitas tidak dikarang. Label "Populer" hanya muncul bila rank Deezer tinggi.
- Penanda "Indonesia" adalah heuristik (genre iTunes, artis yang sama, kata dalam judul), bukan data resmi, jadi bisa meleset pada lagu tertentu.
- Audio tidak diambil dari Deezer/iTunes. Lagu yang diketuk masuk ke "Lagu saya" lalu diputar sebagai audio lewat pemutar YouTube resmi yang disembunyikan (dicocokkan dengan judul, artis, dan durasi asli), tanpa video. Jika /api/search tidak tersedia (misalnya file dibuka tanpa Vercel), pencarian otomatis memakai katalog Audius sebagai cadangan.
- Tampilkan atribusi: data lagu dari Deezer dan iTunes. Periksa syarat layanan keduanya bila aplikasi dipakai secara komersial.
- Pemutaran YouTube tetap bergantung pada izin video untuk diputar di situs lain, koneksi internet, dan ketersediaan layanan YouTube.
- File MP3/M4A dari HP tetap diputar secara lokal di browser. File lokal tersimpan di penyimpanan browser perangkat dan tidak otomatis berpindah ke domain Vercel/perangkat lain.
- Data MySchool yang tersimpan di browser pada alamat content:// tidak otomatis berpindah ke domain HTTPS Vercel. Ekspor/cadangkan data sebelum beralih jika aplikasi menyediakan fitur ekspor.
- Vercel hanya menyediakan hosting HTTPS; tidak menjamin server Piped/proxy publik selalu aktif.

LIRIK TERSINKRON
- Di layar pemutar, geser sampul album ke samping untuk membuka lirik; geser lagi untuk kembali ke sampul. Saat pemutar dibuka, yang tampil selalu sampul.
- Lirik bertimestamp (format LRC) diambil dari LRCLIB (lrclib.net, gratis, tanpa API key) lewat /api/lyrics (fungsi Vercel), dengan cadangan langsung ke LRCLIB. Judul, artis, dan durasi dicocokkan; bila tidak cocok atau tidak ada, tampil "Lirik belum tersedia". Tidak ada lirik yang dikarang.
- Baris aktif ikut posisi lagu (au.currentTime), jadi tetap sinkron saat maju/mundur. Ketuk satu baris untuk melompat ke bagian itu. Geser lirik naik/turun secara manual memunculkan tombol "Sinkron ulang".
- Lagu dari Spotify embed tidak mendukung lirik (tidak ada posisi waktu).

VIDEO MUSIK & SAMPUL PER LAGU
- /api/video (fungsi Vercel) mencari video YouTube di sisi server, lalu aplikasi memilih video musik asli berdasarkan judul + artis + durasi (kanal Topic/Topik, lirik, audio, remix, live dibuang) dan memeriksa video boleh disematkan. Jika video musik asli tidak ada, mode Video menampilkan pesan "Video musik asli tidak ditemukan", bukan gambar.
- Sampul tiap lagu diambil dari video lagu itu sendiri sehingga berbeda per lagu. Seluruh koleksi dicocokkan ulang otomatis di latar belakang sekali setelah update ini.

PERBAIKAN MUSIK (fixed_12)
- Pencarian: ambang kecocokan dinaikkan; hasil yang hanya "mirip" dibuang. Jika tidak ada yang cocok tampil "Lagu tidak ditemukan" (bukan lagu lain). Cadangan Audius hanya menampilkan lagu yang semua kata kuncinya cocok.
- Video: video hanya diterima bila judul, artis (judul/kanal), dan durasi cocok; judul video tidak boleh memuat kata lain di luar judul lagu. Pemilihan cadangan yang hanya memakai durasi dihapus. Kanal Topic resmi artis hanya dipakai sebagai audio (mode Video menampilkan pesan, bukan gambar).
- Sampul: sampul playlist tidak lagi dipakai sebagai pengganti sampul lagu (penyebab sampul sama semua). Tanpa artwork asli dipakai placeholder yang sama. Sampul yang terlanjur dipakai bersama banyak lagu impor dicocokkan ulang sekali otomatis.
- Lirik: judul harus persis sama, artis harus cocok, durasi selisih maksimal 4 detik, dan harus berformat bertimestamp; selain itu tampil "Lirik belum tersedia".

PERBAIKAN MUSIK (fixed_13)
- Tombol "Sinkron ulang": kini tombol bulat kecil (32 px) tanpa teks di pojok kanan bawah area lirik, tidak menutupi lirik.
- Lagu gagal diputar: kode galat YouTube 100/101/150 (video dihapus / sematan dilarang) dibedakan. Aplikasi mencari sumber alternatif yang cocok (judul + penyanyi + durasi); bila tidak ada, tombol Putar dinonaktifkan (redup), muncul tulisan "Sumber audio tidak tersedia" dan tombol "Cari sumber lagi". Status "Memuat..." tidak bisa menggantung lebih dari 45 detik.
- Sumber alternatif: bila video musik resmi tidak bisa dipakai, video berlabel lirik/audio dengan judul, penyanyi, dan durasi yang cocok boleh dipakai HANYA sebagai audio (mode Video tetap menampilkan "Video musik asli tidak ditemukan"). Remix/DJ/cover/live/karaoke tetap ditolak.
- Sampul: urutan (1) sampul album asli yang cocok (judul persis, artis utama cocok, versi sama, durasi mirip), (2) thumbnail video YouTube yang sudah lolos pencocokan lagu itu, (3) gambar pengganti. Sampul yang sama dipakai artis berbeda dicocokkan ulang sekali otomatis.
- Pencarian: hasil yang dipilih disimpan dengan kunci judul+artis+versi (juga untuk hasil iTunes tanpa ID Deezer), sehingga ketukan Play selalu memutar lagu yang sama dengan hasil yang dipilih; album ikut disimpan.
- Lirik, urutan sumber: (1) LRCLIB bertimestamp, (2) LRCLIB lirik biasa, (3) lyrics.ovh lirik biasa (lewat /api/lyrics?e=ovh). Lirik biasa ditampilkan apa adanya dengan keterangan "tanpa sinkronisasi waktu"; tidak ada sinkronisasi palsu. Judul harus persis sama, artis harus cocok, durasi dicek; lagu versi DJ/remix/cover/live tidak memakai lirik biasa dari versi lain.
- Better Lyrics (sumber lirik TERAKHIR, bertimestamp): lewat /api/lyrics?e=bl ke lyrics-api.boidu.dev (GET /getLyrics?s=&a=&d=), respons TTML diurai di aplikasi (lyTtml). Terjemahan/transliterasi/vokal latar dibuang. API ini tidak mengirim judul/artis, jadi hasilnya diterima hanya bila durasi lirik masuk akal terhadap durasi lagu (>=35% dan <= durasi+3 dtk) dan minimal 4 baris; versi DJ/remix/cover tidak memakainya. Label "Sumber: Better Lyrics" tampil kecil di pojok kiri bawah lirik.
  PERHATIAN: skema respons Better Lyrics belum bisa diuji dari sandbox (parser menerima TTML mentah atau JSON yang memuat string TTML). Layanan ini dibuat untuk ekstensi Better Lyrics; pembuatnya menyatakan sebagian endpoint sengaja dibatasi agar tidak disalahgunakan, dan sumber datanya diduga berformat Apple Music TTML, sehingga lisensi lirik tidak jelas. API ber-lisensi GPL v3 (atribusi: Better Lyrics / Boidushya, github.com/better-lyrics/api). Gunakan atas tanggung jawab sendiri, terutama bila aplikasi dipakai komersial. Untuk mematikan: hapus blok "cadangan TERAKHIR" di lyFind() (index.html).
