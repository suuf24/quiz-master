# Quiz Master 🃏

Quiz platform **single-file** (`index.html`): HTML + CSS + Vanilla JS murni, tanpa framework, tanpa backend/database. Semua berjalan 100% di browser pengguna. Satu-satunya request jaringan adalah tiga font Google (lihat bagian Bahasa desain); tanpa jaringan aplikasi tetap jalan dengan font cadangan sistem.

## ✨ Fitur

| Fitur | Keterangan |
|---|---|
| 📄 Load TXT | Baca file `.txt` dari perangkat (klik, atau drag & drop); tetap ada di dalam modal impor |
| 🌐 input kode | Tombol di Home: membuka modal impor langsung di **tab Code** (default), tempel kode Pastebin saja (`pastebin.com/raw/Tnk390bz` → `Tnk390bz`) atau link raw penuh; tab **File / URL** tetap ada untuk baca file .txt dan link **raw text** (Pastebin Raw, GitHub raw, Gist raw) |
| 🧾 Format satu baris | Satu soal satu baris, bagian dipisah tanda pipa, opsi benar ditandai `*`; pas untuk file hasil generate ratusan soal |
| 📋 Copy template | Tombol **Salin template** tepat di bawah baris input Home: contoh format TXT sekali klik, tanpa buka modal impor |
| 🔗 Shareable URL | `?source=<url>`, `?code=<kode>`, dan `/code=<kode>` otomatis me-load kuis saat halaman dibuka |
| 🔢 Randomisasi | Soal & opsi diacak dengan **Fisher-Yates** (bukan `sort(random)`) |
| ⚙️ Settings | Jumlah soal (default **20**; All/5/10/20/Custom 1-200), urutan soal/opsi, timer per soal, tampilkan kunci |
| ⏱ Timer | Off / 10 / 20 / 30 / 60 detik per soal, auto-advance (dihitung *skipped*) |
| 🌍 i18n | EN / ID, mengikuti bahasa browser, bisa diganti manual, tersimpan di localStorage |
| ↪️ RTL | Teks soal/opsi beraksen Arab/Ibrani otomatis di-render `dir="rtl"` |
| 🌗 Tema | Auto / Terang / Gelap di Settings. `Auto` mengikuti `prefers-color-scheme`; pilihan tersimpan di localStorage |
| ⛶ Fokus kuis | Header disembunyikan dan halaman masuk **fullscreen otomatis** selama layar kuis aktif (keluar saat kuis selesai/ditinggalkan; gagal = diam, aplikasi tetap jalan) |
| 🪪 Credit | Baris kecil **made by suuf24** di dasar layar kuis, tampil di semua lebar; di HP footer penuh tetap disembunyikan agar soal + jawaban tetap muat satu layar |
| ⌨️ Keyboard | `1-9` pilih opsi · `Enter` lanjut · panah kiri/kanan navigasi · `Esc` tutup modal |
| 🔎 Review Jawaban | Modal rinci per soal; yang **salah dijawab naik ke atas**, lalu yang dilewati, lalu yang benar, dan nomor soal tetap nomor aslinya |
| 🔒 Privasi | File & jawaban **tidak pernah** dikirim ke server manapun |
| ♿ A11y | Fokus terlihat, `aria-pressed`/`aria-valuenow`, modal focus-trap, `prefers-reduced-motion` |
| 💾 Resume | Banner "Lanjutkan kuis terakhir?", posisi & jawaban tersimpan otomatis |
| 🧪 Parser aman | Semua teks soal dirender via `textContent`/DOM API, tahan XSS dari file/URL |

## 🎨 Bahasa desain

Gaya visualnya mengikuti spec **neo-brutalist / print-shop** yang diberikan pemilik proyek (`ui.txt` di proyek Word Search Maker), dipakai ulang di sini. Spesifikasi lengkap gaya itu ada di file tersebut; ringkasan penerapannya:

- **Arah**: kertas krem, kartu putih tulang, garis luar hitam dekat 1.5px, *hard offset shadow* tanpa blur, sudut membulat sedang, dan mikro-label monospace uppercase ber-tracking lebar (bahasa stempel toko print).
- **Satu aksen**: vermillion. `#e8432a` hanya untuk isian tanpa teks; untuk teks/isi tombol dipakai langkah text-safe `#d93a1f` (putih di atasnya 4.59:1), plus warna semantik benar/salah/lewati. State terpilih pada kontrol memakai *fill* ink (permukaan terbalik), bukan aksen.
- **Font**: Archivo Black (judul, uppercase), Space Grotesk (soal & body), IBM Plex Mono (angka dan label) lewat satu link Google Fonts. Tanpa jaringan mereka gugur ke stack sistem, aplikasi tetap jalan.
- **Ikon**: [Tabler Icons](https://tabler.io/icons) (MIT), di-*inline* agar tetap satu file, stroke 2.0. Tidak ada emoji di UI.
- **Bentuk**: satu skala radius (kontainer 16px, kontrol 11px, chip 999px, kotak kecil 9px), garis luar 1.5px.
- **Gerak**: durasi pendek (≤450ms), tombol bergeser fisik saat hover/tekan, opsi muncul bertahap, toast masuk dari kanan. Semuanya otomatis nonaktif saat `prefers-reduced-motion: reduce`.
- **State lengkap**: loading memakai skeleton berbentuk sama dengan hasil akhirnya, error tampil inline di modal dengan kode masalah, dan tombol nonaktif punya gaya sendiri.

Aturan rinci (palet, tipografi, bentuk, gerak, di mana aksen boleh muncul) tertulis di [`DESIGN.md`](DESIGN.md).

## 🚀 Cara Menjalankan

Cukup buka `index.html` di browser (double-click), atau jalankan server statis:

```bash
npx http-server -p 8080
# buka http://localhost:8080
```

Tombol **Mulai Kuis Contoh** memuat 20 soal bawaan (Benny the Rabbit) memakai parser TXT yang sama seperti file `.txt`.

Di HP, layar soal dipadatkan supaya pertanyaan dan seluruh pilihan jawaban tampil tanpa scroll; pertanyaan yang sangat panjang menggelinding di dalam kotaknya sendiri. Kotak nomor soal bisa dibuka dan ditutup lewat tombol grid di baris judul kuis (di layar sempit tertutup secara default, pilihanmu disimpan).

## 📄 Format TXT

**Format satu baris (disarankan untuk file besar):**

```txt
Title: Judul Kuis Kamu

Pertanyaan pertama? | *Opsi benar | Opsi kedua | Opsi ketiga | Opsi keempat
Pertanyaan kedua? | Opsi pertama | Opsi kedua | *Opsi benar
```

- Satu soal satu baris, **tanpa nomor**. Urutan baris tidak penting: aplikasi mengacak sendiri, mengambil **20 soal per sesi** (bisa diubah lewat Settings), dan mengacak urutan opsi jawabannya juga.
- Bagian dipisah `|`: teks soal dulu, lalu 2 sampai 8 opsi.
- Opsi benar ditandai satu bintang di depannya, mis. `*Opsi benar`.
- Teks soal/opsi sebaiknya tidak memakai `|` karena karakter itu jadi pemisah.

**Format lama (multi-baris bernomor) tetap didukung:**

```txt
Title: Judul Kuis Kamu

1. Pertanyaan pertama?
A. Opsi pertama
B. Opsi kedua
Answer: A
```

Aturan:

- `Title:` opsional di kedua format (default: *Quiz Master*). Hanya baris judul pertama yang dipakai.
- Format lama: nomor soal `1.` atau `1)` (1-3 digit), opsi huruf `A`-`H` diikuti `.`, `)`, atau `:`, minimal **2 opsi** berurutan dari A, dan kunci `Answer: A` yang wajib menunjuk opsi yang ada.
- Case-insensitive; baris kosong & spasi ekstra diabaikan; di format lama teks soal/opsi boleh melanjut ke baris berikutnya.
- Satu huruf opsi tidak boleh dipakai dua kali (duplikat = peringatan, nilai pertama dipakai).
- Format satu baris dianggap dimulai saat satu baris punya minimal dua pemisah `|`, jadi baris format lama yang berisi satu `|` tetap dibaca seperti biasa.

Error ditampilkan per soal lengkap dengan **nomor soal & nomor baris**, dipisah *error* (merah) vs *warning* (kuning, kuis tetap bisa dimuat):

| Kode | Arti |
|---|---|
| `pNoText` / noText | Teks soal kosong |
| `pFewOptions` / countShort | Opsi kurang dari 2 |
| `pMissingOption` / missingOption | Huruf opsi melompat (mis. ada A, C tanpa B) |
| `pMissingAnswer` / missingAnswer | Tidak ada kunci jawaban: `Answer:` hilang (format lama) atau tidak ada `*` (format satu baris) |
| `pInvalidAnswer` / invalidAnswer | Kunci bukan huruf opsi valid |
| `pAnswerNoOption` / answerNoOption | Kunci menunjuk opsi yang tidak ada |
| `pOptionOutside` / dupOption | Opsi duplikat (warning) |
| `pEmptyOption` / emptyOption | Kolom opsi kosong di format satu baris |
| `pMultiAnswer` / multiAnswer | Lebih dari satu opsi bertanda `*` (yang pertama dipakai) |
| `pTooManyOptions` / tooManyOptions | Opsi lebih dari 8 |
| `pNoQuestions` / noQuestions | Tidak ada soal valid sama sekali |

Contoh file siap pakai: [`sample-quiz.txt`](sample-quiz.txt).

## 🌐 Pastebin Raw & Link Raw Lain

Soal harus berupa **plain text**, bukan halaman HTML. Gunakan link raw:

| Layanan | Link biasa (❌ HTML) | Link raw (✅ plain text) |
|---|---|---|
| Pastebin | `https://pastebin.com/abc123` | `https://pastebin.com/raw/abc123` |
| GitHub | `https://github.com/user/repo/blob/main/quiz.txt` | `https://raw.githubusercontent.com/user/repo/main/quiz.txt` |
| Gist | `https://gist.github.com/user/abc123` | klik tombol **Raw** pada file |

> Aplikasi otomatis mengubah `pastebin.com/abc123` → `pastebin.com/raw/abc123`. Layanan lain wajib memakai link raw langsung.
>
> Tab **Code** (tab default) di modal input kode menerima kode telanjang: tempel `Tnk390bz` saja (atau link raw penuh), aplikasi menyusun `https://pastebin.com/raw/Tnk390bz` sendiri.

Pastebin tidak mengirim header CORS, jadi browser tidak bisa membaca link-nya secara langsung. Aplikasi mencoba jalur cadangan berurutan:

1. **Fetch langsung** dari browser (host yang mengizinkan, mis. GitHub raw / Gist raw).
2. **Fungsi serverless sendiri** di `/api/raw` (otomatis ada di deployment Vercel, lihat [`api/raw.js`](api/raw.js)).
3. **Proxy CORS publik**: `api.allorigins.win` → `corsproxy.io` → `api.codetabs.com`, dicoba satu per satu dan hanya untuk file yang sedang dimuat.

Proxy publik bisa penuh atau berubah aturan kapan saja, jadi cara paling andal untuk Pastebin adalah deploy ke Vercel dengan fungsi `/api/raw`.

Jika semua jalur gagal (CORS/offline), muncul pesan ramah + saran memakai link raw + tombol **Coba Lagi** / **Load TXT**.

### Shareable URL

Setelah load via URL, salin link dari address bar (tombol Share sudah dihapus; tidak ada lagi tombol share di topbar):

```
https://domainmu.vercel.app/?source=https%3A%2F%2Fpastebin.com%2Fraw%2Fabc123
```

Untuk kuis Pastebin ada dua bentuk link pendek; nilainya sama seperti isi kotak **Code** di modal impor (kode telanjang atau link raw penuh):

```
https://domainmu.vercel.app/?code=Tnk390bz   (tanpa konfigurasi, jalan di host statis mana pun)
https://domainmu.vercel.app/code=Tnk390bz    (bentuk path, dilayani rewrite vercel.json)
```

Bentuk `/code=` membaca kodenya dari address bar setelah `vercel.json` menulis-ulang path ke `index.html`; bentuk `?code=` tidak butuh konfigurasi apa pun. Siapa pun yang membuka link itu langsung melihat kuisnya dimuat otomatis.

## ▲ Deploy ke Vercel

Cara termudah:

```bash
npm i -g vercel
cd KTI
vercel --prod
```

Atau:

1. Push folder ini ke GitHub.
2. Buka [vercel.com/new](https://vercel.com/new) → import repo.
3. Framework Preset: **Other** (biarkan kosong) → **Deploy**.

Vercel melayani `index.html` sebagai static file dan otomatis mengubah [`api/raw.js`](api/raw.js) menjadi fungsi serverless di `/api/raw` (Node runtime yang sama, tanpa dependency npm). Tanpa build step dan tanpa environment variable; satu-satunya config adalah `vercel.json` (rewrite bentuk link `/code=<kode>` ke `index.html`, lihat bagian Shareable URL).

## 🧩 Daftar Fitur Baru vs Versi Lama

- ✅ Multi-screen: Home / Settings / Quiz / Result (sebelumnya satu layar)
- ✅ Home punya satu entri `input kode` yang membuka modal langsung di tab Code (default); tab File dan URL tetap tersedia
- ✅ Format satu baris per soal (tanpa nomor, opsi benar bertanda `*`); format lama tetap didukung
- ✅ Tombol Salin template di Home (contoh format TXT langsung ke clipboard)
- ✅ Load link dengan jalur cadangan berlapis: browser → `/api/raw` (serverless) → 3 proxy CORS publik
- ✅ Import TXT (klik + drag & drop) dan URL raw, dengan parser + error reporting per baris
- ✅ Shareable URL `?source=` (kuis ter-load otomatis saat link dibuka)
- ✅ Settings lengkap (jumlah, urutan, timer, kunci), tersimpan di localStorage
- ✅ i18n EN/ID + deteksi bahasa browser + RTL otomatis
- ✅ Fokus kuis: header disembunyikan + fullscreen otomatis selama kuis berjalan
- ✅ Resume kuis terakhir
- ✅ Score berbasis persen + lingkaran progres + statistik benar/salah/lewati
- ✅ Review Jawaban mengurutkan soal yang salah dijawab ke atas
- ✅ A11y: semantic HTML, ARIA, focus trap, reduced-motion
- ✅ Responsive penuh 320px → ultra-wide (tanpa `100vw`/font `vh` seperti versi lama)
- ✅ Keamanan: render via `textContent` saja, tidak ada `innerHTML` untuk data eksternal

## 🌍 Kompatibilitas Browser

| Browser | Versi minimum | Catatan |
|---|---|---|
| Chrome / Edge | 88+ | Semua fitur |
| Firefox | 78+ | Semua fitur |
| Safari (macOS) | 14+ | Semua fitur |
| Safari (iOS) | 14+ | Semua fitur; fullscreen menggunakan prefix `webkit` |
| Samsung Internet | 15+ | Semua fitur |

Fitur opsional digunakan dengan aman bila tersedia: `requestFullscreen` (prefix `webkit` di Safari), `navigator.clipboard`, `File.text()`, `100svh` (fallback otomatis), `backdrop-filter` (kosmetik saja).

## ✅ Checklist Testing

Perilaku sudah diverifikasi otomatis via browser:

- [x] Mulai kuis contoh: 20 soal, acak setiap sesi
- [x] Pilih opsi (klik & tombol `1-4`), pilih ulang = deselect
- [x] Memilih opsi memperbarui tombol di tempat (tanpa rebuild DOM): animasi masuk tidak terputar ulang — tanpa kedip
- [x] Baris credit **made by suuf24** tampil di dasar layar kuis di semua lebar (320/375/414/640), tanpa membuat halaman scroll
- [x] `Enter` lanjut · `←`/`→` navigasi · klik nomor di nav-strip untuk lompat
- [x] Nomor soal bisa disembunyikan (aria-expanded + pilihan disimpan di localStorage)
- [x] Lewati soal (tidak dijawab), dihitung *skipped* di hasil
- [x] Selesai → skor %, pecahan, statistik benar/salah/lewati
- [x] Review jawaban (modal), status benar/salah/lewati + kunci, soal salah dijawab tampil paling atas
- [x] Play Again: undian 20 soal baru dari bank lengkap (atau acak ulang bila sesi dilanjutkan), jawaban direset
- [x] Load TXT valid / tidak valid (error per soal + baris)
- [x] Format satu baris tanpa nomor: baris valid termuat, baris rusak (tanpa `*`, dua `*`, kolom kosong, 9 opsi) dilaporkan di modal masalah
- [x] Salin template dari Home: contoh TXT masuk clipboard, toast muncul di dua bahasa
- [x] Load URL raw sukses / gagal CORS (pesan ramah + retry + Load TXT)
- [x] Normalisasi `pastebin.com/xxx` → `pastebin.com/raw/xxx`
- [x] Load lewat **code**: kode telanjang `Tnk390bz` dan link raw penuh sama-sama menuju `pastebin.com/raw/<kode>`
- [x] Link `?code=Tnk390bz` dan `/code=Tnk390bz` memuat kuis otomatis; kode tidak valid → modal error + tombol Coba Lagi
- [x] i18n EN ⇄ ID, persist setelah reload
- [x] Settings tersimpan; jumlah > tersedia → memakai semua yang ada
- [x] Timer aktif → auto-advance saat habis; chip timer menguning/merah
- [x] Banner "Continue last quiz?" + resume posisi & jawaban
- [x] Header tersembunyi + fullscreen otomatis selama layar kuis, kembali normal di layar hasil/Home
- [x] Tanpa horizontal scroll di 320px; layout 2 kolom opsi ≥768px
- [x] Console bersih (tanpa error) pada alur normal

Yang sebaiknya diuji manual di perangkat asli:

- [ ] Load link Pastebin saat online (rantai fallback `/api/raw` → proxy publik)
- [ ] Drag & drop `.txt` dari file manager
- [ ] Fullscreen otomatis di perangkat asli (butuh gesture; iPhone Safari tidak mengizinkan fullscreen halaman)
- [ ] Mode gelap sistem / `prefers-reduced-motion`
- [ ] Screen reader (NVDA/VoiceOver) navigasi kuis

## 📁 File

| File | Keterangan |
|---|---|
| `index.html` | Seluruh aplikasi (HTML + CSS + JS) |
| `sample-quiz.txt` | Contoh soal untuk uji Load TXT/URL |
| `api/raw.js` | Fungsi serverless Vercel `/api/raw` (jalur cadangan CORS) |
| `package.json` | Penanda ESM untuk fungsi `api/` (tanpa dependency) |
| `vercel.json` | Rewrite Vercel untuk bentuk link `/code=<kode>` |
| `0.000.001.html` | Backup versi lama (jangan diubah) |
