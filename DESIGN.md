# DESIGN.md - Quiz Master

> Arah desain proyek ini. File ini yang jadi acuan berikutnya.
> **2026-09-30**: arah visual diganti sesuai spec dari pemilik proyek:
> `D:\Mainan\Project\Word Search Maker\ui.txt` ("INSANE WORDS SEARCH MAKER" UI SPEC).
> Spec itu sumber aturan gaya di bawah. Keputusan yang tidak di spec (makna state,
> keputusan kontras, aksesibilitas) tetap milik proyek ini dan ditulis lengkap di sini.
> Sebelum tanggal ini arahnya "answer sheet" biru; catatan audit 001 sampai 007 tetap
> berlaku untuk hal yang tidak berubah.

## Identity

- **Product**: Quiz Master, pemuat kuis pilihan ganda satu file: satu halaman yang memuat kuis dari mana saja, bukan latihan satu topik. Kuis bawaan bertitel "Quiz Master" (20 soal Benny the Rabbit); kuis lain masuk lewat file .txt atau URL raw.
- **Audience**: pelajar Indonesia dan pembelajar kasual, sering dibuka dari HP.
- **Bahasa**: EN / ID, default mengikuti bahasa browser.
- **Bahasa visual**: "neo-brutalist print-shop": kertas krem, kartu putih tulang, garis luar hitam dekat 1.5px, bayangan keras tanpa blur, sudut membulat sedang, satu aksen merah-oranye, dan mikro-label monospace uppercase ber-tracking lebar.

## Logo

- **Mark**: monogram kotak berhuruf **B** (Benny), radius 11px, isi aksen, garis 1.5px ink, bayangan keras kecil, huruf Archivo Black. Favicon memakai monogram yang sama dengan isi aksen.
- **Status**: dikonfirmasi pemilik proyek pada 2026-09-24 sebagai jawaban atas temuan 3 di `anti-slop/audit-001-2026-09-24.md`; bentuknya dipertahankan saat gaya diganti 2026-09-30.
- **Wordmark**: teks "Quiz Master" di samping mark, IBM Plex Mono 12.5px, weight 700, uppercase, tracking .13em (aturan brand dari spec).

## Personality

Percaya diri dan sedikit berisik, seperti cetakan toko print: tebal, datar, kontras tinggi. Bukan korporat, bukan juga kekanak-kanakan. Terasa dikerjakan tangan, bukan di-render dari template.

## Palette

Satu aksen, plus warna semantik. Neutrals tidak dihitung sebagai bagian palet inti.

- **Aksen murni**: vermillion `#e8432a` (`--brand-bright`), hanya untuk mark, blok highlight, dan isian tanpa teks di atasnya.
- **Aksen teks**: `--brand` = `#d93a1f`, langkah text-safe dari hue yang sama. Putih di atasnya **4.59:1** (lolos AA). `--brand-dark` = `#b73318` untuk tepi tombol aksen, putih di atasnya 6.0:1. `--brand-soft` = `#ffe4dc` untuk tint latar terang.
- **Neutrals**: canvas `#f4efe4` (kertas), surface `#fffdf6` (tulang, bukan putih murni), surface-2 `#faf6ec`, ink `#1b1610` (dekat hitam, bukan `#000`), ink-soft `#6b6154`, line `#ddd4c2`, line-strong `#b9ae99`.
- **Semantik** (status, bukan aksen): benar `#0b7a4f`, salah `#b3261e`, lewat `#8a5a00`, masing-masing dengan tint lembut.
- **Dark** (hangat, bukan biru): canvas `#15110c`, surface `#1e1913`, surface-2 `#241f18`, ink `#f4efe4`, ink-soft `#a99e8b`, line `#3a332a`, line-strong `#5f574a`, brand-soft `#3a211a`. Aksen tidak berubah agar putih-di-aksen tetap 4.59:1.
- **Bayangan**: `--sh` (kontrol keras) `rgba(27,22,16,.85)` terang / `rgba(0,0,0,.75-.8)` gelap; `--pop` (kartu) 6px 7px 0 alpha .14-.6; `--pop-sm` (tombol) 2.5px 3px 0; `--pop-lg` (modal) 8px 9px 0.
- **Larangan**: tanpa gradient dekoratif, tanpa glow. Gradient hanya untuk fungsi: `conic-gradient` di cincin skor, `linear-gradient` di shimmer loading.

### Di mana aksen boleh muncul

Ditulis supaya aksen tidak bocor ke mana-mana (temuan 14 di `anti-slop/audit-004-2026-09-24.md`). Aksen dipakai **hanya** untuk:

1. Mark brand.
2. Aksi utama (satu tombol primer per layar).
3. Huruf opsi pada jawaban yang dipilih.
4. State "sudah dijawab" dan sel nomor yang sedang aktif pada sel navigasi soal.
5. Isi progress bar.
6. Cincin fokus keyboard.

**Perubahan 2026-10-04**: sel navigasi nomor yang **current** memakai cincin aksen *inset* (`box-shadow` 2.5px) alih-alih outline keluar, jadi memilih nomor tidak pernah mengubah ukuran sel; pada sel yang sudah dijawab cincin itu memakai `--on-brand` agar tetap terbaca di atas fill aksen. Drawer nomor (ponsel) juga dipadatkan (`align-content:start`) supaya jarak barisnya 6px seperti gap horizontal, bukan 55px hasil stretch.

**Perubahan 2026-09-30**: state terpilih pada *kontrol* (pill pengatur, tab bahasa, tab modal, kartu opsi jawaban) tidak lagi memakai aksen. Ia memakai **fill ink** dengan teks canvas, alias permukaan terbalik, persis bahasa radio-card dan tab di spec. Alasannya: aksen jadi lebih langka dan lebih mudah dibaca sebagai "aksi", sementara pilihan tetap terlihat menonjol. Di luar enam hal di atas aksen tidak dipakai. Panel sekunder memakai `--surface-2` dengan garis putus-putus, bukan tepi berwarna. Warna status (benar, salah, lewat) bukan aksen dan tidak pernah dipakai sebagai warna brand.

## Typography

Tiga wajah, satu `<link>` Google Fonts di `<head>` (spec bagian 2).

- **Display**: `'Archivo Black'`, weight 400 saja (tidak di-bold lagi), selalu uppercase, tracking -0.01em sampai -0.015em, line-height .98-1.12. Dipakai untuk judul layar: `home-title`, `screen-title`, `quiz-title`, judul modal. Tidak dipakai untuk teks soal.
- **Body**: `'Space Grotesk'` 16px / 1.5. Teks soal, opsi jawaban, subteks, hint.
- **Mono**: `'IBM Plex Mono'` untuk angka (counter, timer, skor, nomor sel, huruf opsi) **dan** untuk mikro-label: uppercase dengan tracking .1em sampai .16em (label grup pengatur, chip, tombol kecil, brand wordmark, kbd hint).
- **Judul soal**: tetap Space Grotesk bold, bukan display. Alasannya dua: soal adalah konten, bukan judul; dan Archivo Black uppercase pada soal panjang menurunkan keterbacaan sekaligus merusak anggaran layar sempit.
- **Konsekuensi jujur**: tanpa jaringan (atau dibuka dari `file://` tanpa internet) ketiga wajah itu gugur ke fallback stack sistem (`Arial Black` / `Segoe UI` / monospace). Aplikasi tetap jalan penuh, hanya wajahnya yang berubah. Ini pengganti keputusan lama "tanpa webfont sama sekali" (temuan 8 di `audit-001`), yang sebelumnya membuat identitas tipografi jadi milik OS.

## Shape

Satu skala radius, mengikuti spec: **kontainer 16px** (`--r-lg`), **kontrol 11px** (`--r-md`), **kotak kecil 9px** (`--r-sm`), **chip/tag 999px**, lingkaran hanya untuk cincin skor, status disc toast, dan tombol sosial. Garis luar interaktif dan kartu: **1.5px solid ink** (sebelumnya 2px). Garis putus-putus panel sekunder: 1.5px `--line-strong`.

## Chrome

- **Header tanpa garis.** Topbar tetap `position:sticky` dengan `background:var(--canvas-veil)` + `blur(6px)`, tapi **tidak** punya `border-bottom`. Pemisahnya adalah veil-nya sendiri plus jarak, bukan garis. Untuk itu `padding-bottom:var(--gap-shell)` dan `margin-bottom:calc(-1 * var(--gap-shell))` dipertahankan supaya pita celah antar blok tetap ikut tertutup veil dan tinggi tata letak tidak berubah. (Sebelum 2026-09-30 header memakai garis bawah 1.5px ink; catatan itu ada di `anti-slop/audit-008-2026-09-30.md`.)
- **Kartu Review Jawaban** adalah satu-satunya modal yang panjangnya selalu melewati layar, jadi bar gulirnya disembunyikan (`scrollbar-width:none` plus `::-webkit-scrollbar`). Gulirnya tetap ada (roda, papan tombol, sentuh); yang menggantikan bar itu kepala kartu yang `position:sticky` dengan latar `--surface`, sehingga judul dan tombol tutup selalu di layar pada posisi gulir mana pun. Modal lain tidak disentuh dan tetap memakai bar bawaan.
- **Urutan isi Review Jawaban**: salah dulu, lalu dilewati, lalu benar, masing-masing tetap urutan asli kuis, dan nomor tiap soal tetap nomor aslinya. Satu baris keterangan `review.order` di bawah judul menjelaskan itu, karena daftar yang nomornya melompat akan terbaca seperti bug.
- **Kuis menguasai layar.** Saat layar kuis aktif header disembunyikan (`body.quiz-active .topbar{display:none}`) di **semua** lebar, dan halaman diminta masuk fullscreen otomatis lewat `setQuizFullscreen(true)`; keduanya dilepas begitu layar kuis ditinggalkan (hasil maupun Home). Alasannya fokus: yang tersisa hanya soal, opsi, dan navigasi. Fullscreen bersifat best-effort (gesture browser, prefix `webkit`, iPhone Safari tidak mengizinkan), kegagalannya ditelan agar kuis tetap jalan. Tombol Share / Presentation / Fullscreen di topbar dihapus pada hari yang sama.
- **Tumpukan aksi Home**: Mulai Kuis Contoh (primer, satu baris penuh), lalu satu tombol `input kode` (`btn-secondary`, penuh, ikon `</>`) yang membuka modal impor langsung di **tab Code** (default: kode Pastebin, langsung fokus ke kolomnya); tab **File / URL** tetap di sebelahnya untuk baca file dan tempel link raw. Layar tetap punya satu aksen saja. Di bawahnya baris dua kolom berisi Salin template dan Settings, keduanya `btn-ghost`, jadi baris itu dibaca sebagai utilitas, bukan aksi kedua. (Disusun ulang 2026-09-30 dari satu tombol "Load Kuis"; digabung jadi satu entri ber-tab 2026-10-04; label jadi `input kode` dengan tab Code sebagai default pada hari yang sama setelah permintaanmu.)

## Motion

- Durasi pendek: tombol dan state 150ms (`--t`), transisi layar 450ms, modal 300ms, toast 350ms. Easing utama `cubic-bezier(.2,.75,.25,1)`; modal dan toast memakai spring ringan `cubic-bezier(.2,.8/.9,.3,1.2)`.
- Tombol bergeser fisik: hover `translate(-1px,-1px)` dengan bayangan membesar ke 4.5px 5.5px 0, aktif `translate(1.5px,1.5px)` dengan bayangan menyusut ke 1px 1px 0. Bayangan dan transform bergerak bersama supaya terasa ditekan (spec bagian 16).
- Yang ada dan alasannya: transisi antar layar (perubahan state), opsi masuk bertahap sesuai urutan baca (hierarki), tombol terasa ditekan (umpan balik), skor menghitung naik (cerita hasil), timer berdenyut di bawah 5 detik (status hidup), toast masuk dari kanan (kejadian singkat).
- **Denyut timer, alasan tertulis** (temuan 15 di `anti-slop/audit-004-2026-09-24.md`): satu-satunya animasi berulang, dan sengaja dibiarkan berulang. Hanya muncul saat sisa waktu 5 detik atau kurang, jadi jendelanya terikat waktu dan berakhir sendiri.
- Semuanya berhenti saat `prefers-reduced-motion: reduce`.

## Icons

Tabler Icons (MIT), disalin inline: tetap satu file, tanpa request jaringan untuk ikonnya. Stroke **2.0** untuk semua ikon, mengikuti spec (`.ic{stroke-width:2}`), naik dari 1.75. Angka ini menutup opsi 1 di temuan 5 `audit-001`. Ikon dalam tombol memakai `1.1em` dan `flex:none` supaya tidak tergencet oleh label panjang.

## Dials

- Dibangun dengan dial dari skill design-taste-frontend: `VARIANCE 6 / MOTION 6 / DENSITY 5`.
- Padanan dial antislop, **diturunkan** dari nilai di atas: `ENERGY 2 / RHYTHM 2 / MOTION 2`.
- RHYTHM 2 = komposisi tiap layar konsisten dengan pengecualian yang disengaja (layar hasil punya stamp miring, layar kuis punya sheet soal sendiri).

## Motif identitas

Bayangan keras offset + garis 1.5px + mikro-label mono uppercase + satu aksen vermillion.

Catatan jujur: gaya ini dipinjam dari spec proyek lain (`ui.txt`), jadi belum bisa disebut identitas yang benar-benar milik produk ini. Motifnya juga tidak mengikat ke "Benny the Rabbit", padahal itu nama brand-nya. Lihat temuan 7 di `anti-slop/audit-001-2026-09-24.md`.

## Theme

Light + dark + auto, semua wajib lolos kontras. Tidak ada perpindahan tema tambahan: presentation mode dihapus 2026-10-05 bersama tombol Share/Presentation/Fullscreen (tidak dipakai dan mengganggu fokus). Angka kontras terakhir diukur, bukan dikira-kira: putih-di-aksen 4.59:1, teks utama di surface 15-17:1, label ink-soft 5.96-6.61:1, di kedua tema.
