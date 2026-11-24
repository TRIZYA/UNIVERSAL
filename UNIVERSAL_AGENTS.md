# UNIVERSAL_AGENTS.md — Panduan Dasar untuk Semua Proyek

Dokumen ini berlaku untuk **semua proyek** pemilik. Isinya: profil perangkat, aturan kerja agent, kemampuan (skill) yang diharapkan, dan standar UI/UX.

**Urutan prioritas aturan** (yang lebih atas menang jika bertentangan):

1. Permintaan terbaru pengguna.
2. Dokumen proyek di folder `.agent/` (`agent.md`, `ui.md`, `roadmap.md`, `readme.md`, `database_schema.md`) — detail spesifik proyek.
3. Dokumen universal ini (baseline untuk semua proyek).

Jika proyek belum punya folder `.agent/`, dokumen ini menjadi acuan utama dan agent menjalankan **Bagian 0 (Bootstrap Proyek Baru)** bila pengguna ingin membuat aplikasi baru. Periksa struktur dan file aktual proyek sebelum membuat asumsi.

---

## 0. Langkah Pertama: Bootstrap Proyek Baru

Bagian ini dijalankan **sebelum menulis kode apa pun** ketika pengguna ingin membuat aplikasi baru.

### 0.1 Pemicu

Contoh prompt pengguna:

> `baca /universal saya ingin membuat aplikasi [deskripsi aplikasi]. buatkan .md yang diperlukan serta masukkan fitur-fitur yang diperlukan`

`/universal` berarti file ini (`UNIVERSAL_AGENTS.md`). Jika path-nya tidak jelas, cari di root proyek, `~/.config/agents/`, atau tanyakan satu kali.

Pemicu lain yang setara: "buat proyek baru", "mulai aplikasi ...", "siapkan dokumen proyek".

### 0.2 Hasil yang wajib dibuat

Buat folder **`.agent/`** di root proyek, lalu isi dengan:

```text
<nama-proyek>/
└── .agent/
    ├── agent.md              # aturan kerja agent untuk proyek ini
    ├── ui.md                 # panduan UI/UX dan alur pengguna
    ├── roadmap.md            # fase, checklist, prioritas
    ├── readme.md             # gambaran proyek, fitur, cara menjalankan
    └── database_schema.md    # HANYA jika aplikasi memakai database
```

Aturan:

- Nama file **huruf kecil** persis seperti di atas.
- `database_schema.md` dibuat hanya jika aplikasi menyimpan data persisten. Jika tidak (mis. kalkulator, aplikasi statis), jangan dibuat dan tulis alasannya di `agent.md` bagian Catatan keputusan.
- Opsional: buat `AGENTS.md` satu baris di root yang berisi `Baca .agent/agent.md` agar tool yang hanya mencari `AGENTS.md` tetap menemukannya.
- Jangan membuat kode aplikasi, folder `src/`, atau memasang dependensi pada tahap ini. Hanya dokumen.

### 0.3 Alur langkah demi langkah

1. **Baca** dokumen universal ini sampai selesai (terutama bagian 1 perangkat, 4 UI/UX, 5 teknologi).
2. **Analisis deskripsi aplikasi.** Tentukan: tujuan, pengguna, alur inti, data yang disimpan, integrasi luar, kebutuhan login.
3. **Tanya hanya jika wajib.** Maksimal 3 pertanyaan singkat, dan hanya jika jawabannya mengubah arsitektur (mis. perlu login atau tidak, butuh database atau tidak). Jika tidak wajib, **buat asumsi wajar, tulis di bagian Catatan keputusan**, lalu lanjut.
4. **Tentukan fitur.** Susun daftar fitur sendiri sesuai aplikasi (lihat 0.4); jangan hanya menyalin fitur dari pengguna.
5. **Pilih stack ringan** sesuai bagian 5 dan profil perangkat (bagian 1). Tulis alasan singkat jika menyimpang dari default.
6. **Buat folder `.agent/`** lalu tulis dokumen **dengan urutan ini** agar konsisten antar dokumen:
   1. `readme.md` (sumber kebenaran fitur dan stack)
   2. `database_schema.md` (jika perlu)
   3. `roadmap.md`
   4. `ui.md`
   5. `agent.md` (terakhir, merujuk dokumen lain)
7. **Cek konsistensi** (0.6), lalu **laporkan** ringkasan: fitur yang dipilih, stack, asumsi, dan pertanyaan terbuka.
8. **Berhenti dan minta persetujuan** sebelum mulai implementasi. Setelah disetujui, kerjakan dari item P0 pertama di `roadmap.md`.

### 0.4 Cara menentukan fitur

Kelompokkan fitur dalam kategori berikut (hapus kategori yang tidak relevan):

| Kategori | Isi |
| :--- | :--- |
| **Fitur inti (MVP)** | Alur utama dari awal sampai hasil bernilai; tanpa ini aplikasi tidak berguna |
| **Otomasi** | Hal yang berjalan sendiri (cron, notifikasi, sinkronisasi) |
| **Kemudahan pakai** | Pencarian, filter, urutan cerdas, toast, ekspor, responsif |
| **Keamanan & akun** | Login, peran (RBAC), isolasi data, validasi, rate limit |
| **Fitur AI (opsional)** | Hanya bila bernilai nyata; selalu opsional dan ada fallback |
| **Fitur lanjutan** | Ide di luar MVP; masuk P2 |

Aturan:

- Mulai dari **alur inti satu kalimat**: "pengguna melakukan A, sistem melakukan B, pengguna mendapat C".
- MVP harus kecil: cukup untuk menyelesaikan alur inti dari ujung ke ujung.
- Setiap fitur harus bisa dikaitkan ke alur inti, atau ditaruh di P2.
- Selalu pikirkan **jalur cadangan** untuk bagian yang berisiko gagal (mis. input manual bila otomatis gagal).
- Pertimbangkan batas perangkat (bagian 1): hindari fitur yang memaksa proses berat di laptop pengguna.

### 0.5 Templat setiap dokumen

Setiap dokumen memakai **bahasa Indonesia**, judul `#` satu per file, heading `##` bernomor bila diminta templat, tabel Markdown untuk perbandingan/field, dan checklist `- [ ]` untuk progres. Semua checklist **dimulai dari `[ ]`** (belum ada yang selesai) kecuali dokumen itu sendiri (`[x] readme.md disusun`).

#### A. `readme.md`

Urutan bagian:

1. `# <Nama Aplikasi>` + 1–2 kalimat deskripsi + `> Status: tahap perancangan (MVP belum selesai).`
2. `## Fitur` — sub-judul tebal per kategori (**Fitur inti (MVP)**, **Otomasi**, **Kemudahan pakai**, **Fitur AI (opsional)**, **Fitur lanjutan (opsional)**), isinya bullet.
3. `## Alur penggunaan` — daftar bernomor 4–7 langkah dari pengguna memulai sampai hasil.
4. `## Tech Stack` — tabel `Bagian | Teknologi` (Frontend, Backend, Database, AI, Deploy).
5. `## Arsitektur` — diagram teks (blok kode) + 2–4 bullet alasan keputusan penting.
6. `## Model Data` — ringkasan entitas (tanpa detail; detail ada di `database_schema.md`).
7. `## Struktur Folder (rencana)` — pohon folder di blok kode.
8. `## Menyiapkan <Database/Layanan>` — langkah bernomor (jika ada layanan luar).
9. `## Menjalankan Secara Lokal` — prasyarat, perintah bash berurutan, URL default.
10. `## Environment Variables` — contoh `.env.example` + catatan (jangan commit `.env`).
11. `## Dokumentasi` — tautan ke `agent.md`, `ui.md`, `roadmap.md`, `database_schema.md`.
12. `## Roadmap ringkas` — 6–8 checklist `[ ]`; rujuk `roadmap.md`.
13. `## Catatan` — risiko dan ketentuan penting (ketentuan layanan pihak ketiga, keamanan).
14. `## Lisensi` — `Belum ditentukan.`

#### B. `database_schema.md` (jika perlu)

Urutan bagian:

1. `# Skema Database - <Nama Aplikasi>` + paragraf: jenis database, siapa yang mengakses (backend), dan bahwa klien tidak mengakses langsung.
2. `## 1. Gambaran umum` — pohon/ERD teks + bullet konvensi key/ID.
3. `## 2..N. <Entitas>` — satu bagian per entitas: path/tabel + tabel `Field | Tipe | Keterangan` + catatan (denormalisasi, index, relasi).
4. `## Hubungan antar entitas` — daftar bernomor.
5. `## Konvensi data` — tipe (uang = bilangan bulat), waktu (epoch ms / timestamptz), field opsional, validasi di backend, operasi atomik.
6. `## Rules dan index` — aturan akses (Firebase Rules) atau index/constraint SQL, dalam blok kode.
7. `## Contoh data (fiktif)` — satu contoh realistis dalam blok kode.
8. `## Rekomendasi penambahan` — hal opsional yang ditambah saat dibutuhkan.
9. `## Catatan penting untuk pengembangan lanjutan` — batas paket gratis, retensi data, backup, larangan migrasi tanpa izin, kewajiban memperbarui dokumen ini.

Untuk SQL (PostgreSQL/Prisma): ganti "node" dengan "tabel", tambahkan PK/FK/index/unique, dan catat perintah migrasi.

#### C. `roadmap.md`

Urutan bagian:

1. `# Roadmap & Checklist <Nama Aplikasi>` + paragraf tujuan dan alur inti.
2. `## Status proyek saat ini` — "Proyek berada di Fase 1" + checklist dokumen yang sudah dibuat (`[x]` untuk dokumen yang baru dibuat, `[ ]` untuk keputusan terbuka).
3. `## Alur kerja yang ingin dicapai` — daftar bernomor langkah alur inti + kalimat "Jika salah satu langkah belum jalan, aplikasi belum terasa nyata."
4. `## Fase 1: Persiapan & konfigurasi awal`
5. `## Fase 2: Data & infrastruktur`
6. `## Fase 3: Pengembangan backend` — sub-bagian per area (3.1, 3.2, ...).
7. `## Fase 4: Frontend`
8. `## Fase 5: Fitur AI / lanjutan` (jika relevan)
9. `## Fase 6: Finishing & deploy`
10. `## Prioritas` — `### P0 (wajib untuk MVP)`, `### P1 (penting setelah MVP)`, `### P2 (fitur lanjutan)`.
11. `## Gap kritis yang perlu diselesaikan` — 3–6 risiko/keputusan terbuka bernomor.
12. `## Catatan pembaruan` — aturan: centang item yang selesai, perbarui "Status proyek saat ini", dan sinkronkan `database_schema.md` / `ui.md`.

Setiap item checklist harus **spesifik dan bisa diverifikasi** (bukan "buat backend", tapi "Endpoint tambah produk dengan validasi URL").

#### D. `ui.md`

Urutan bagian:

1. `# Panduan UI & Alur Pengguna <Nama Aplikasi>` + paragraf: fokus UI pada alur nyata, bukan sekadar form dan tabel.
2. `## Prinsip desain utama` — 5–7 butir bernomor, spesifik ke aplikasi (termasuk mobile first dan rujukan ke bagian 4 dokumen universal).
3. `## Status implementasi UI saat ini` — awalnya: "Belum ada UI yang diimplementasikan."
4. `## 0..N. <Halaman/Fitur>` — satu bagian per halaman: elemen yang tampil, aksi, empty state, kondisi khusus (loading, gagal, duplikat).
5. `## Alur UI yang disarankan` — alur sukses dan alur masalah, bernomor.
6. `## Standar komponen UI yang disarankan` — pohon `src/` (components, pages, hooks, utils) dalam blok kode.
7. `## Prinsip visual yang harus dijaga` — warna status, format angka/tanggal/mata uang, ukuran gambar, scroll wrapper, area sentuh 40px, konfirmasi aksi destruktif. Tulis **token warna aksen proyek** (nilai hex) bila berbeda dari default universal.
8. `## Backlog UI prioritas berikutnya` — `### Prioritas P0/P1/P2` dengan checklist `[ ]`.
9. `## Kesimpulan` — satu pertanyaan pengguna yang harus dijawab UI dengan cepat.

#### E. `agent.md`

Urutan bagian (salin gaya dan tegasnya; sesuaikan isi dengan proyek):

1. `## 1. Project Overview` — deskripsi, tahap proyek, prinsip kerja **Inspect → Plan → Implement → Run → Preview → Evaluate → Refine → Verify**.
2. `## 2. Project Structure` — pohon folder (termasuk `.agent/`).
3. `## 3. Tech Stack` — daftar + aturan "jangan menambah dependensi tanpa kebutuhan jelas".
4. `## 4. Key Commands` — perintah dari `package.json` yang benar-benar ada; URL default.
5. `## 5. Mandatory Live Preview Workflow` — server tetap hidup, loop edit–preview–refine, daftar yang diperiksa, "never fake visual verification". Tambahkan aturan hemat sumber daya perangkat.
6. `## 6. Avoid Repeated Builds`
7. `## 7. Architecture and Coding Rules` — Frontend / Backend / Database.
8. `## 8. <Aturan Domain Spesifik>` — mis. scraping, pembayaran, upload file, integrasi pihak ketiga. Jika tidak ada, hapus bagian dan nomori ulang.
9. `## 9. <Aturan Database/Auth>` — akses lewat backend, token, isolasi data, rules, atomik, validasi.
10. `## 10. API Integration` — 7 langkah + pembeda jenis error.
11. `## 11. AI Features` (jika dipakai).
12. `## 12. Priority Features` — urutan kerja, rujuk `roadmap.md`.
13. `## 13. Testing Rules`
14. `## 14. Environment Variables and Git Safety`
15. `## 15. Boundaries` — Allowed Without Asking / Ask First / Never.
16. `## 16. Definition of Done`
17. `## 17. Final Report` — 6 butir + prinsip akhir.
18. `## 18. Catatan implementasi dan keputusan` — dokumen acuan, keputusan yang sudah diambil, asumsi dari bootstrap, dan hal yang belum ditentukan.

Di bagian atas `agent.md` tambahkan: `Ikuti aturan dasar di UNIVERSAL_AGENTS.md; dokumen ini hanya berisi aturan khusus proyek.` Bagian yang sudah dicakup dokumen universal boleh diringkas, tetapi **bagian 15 (Boundaries), 16, dan 17 wajib tetap ada** agar model apa pun menjalankannya.

### 0.6 Cek konsistensi sebelum melapor

- [ ] Nama aplikasi, stack, dan daftar fitur **sama** di semua dokumen.
- [ ] Fitur di `readme.md` muncul sebagai item di `roadmap.md` dan, bila ada tampilannya, di `ui.md`.
- [ ] Entitas dan field di `database_schema.md` mencukupi semua fitur yang butuh data; nama field konsisten dengan `roadmap.md`/`agent.md`.
- [ ] Pohon folder di `readme.md` dan `agent.md` identik.
- [ ] Tidak ada item yang ditandai `[x]` selain dokumen yang baru dibuat.
- [ ] Tidak ada secret, kunci, atau data nyata dalam dokumen; contoh data bersifat fiktif.
- [ ] Stack dan fitur tidak melanggar prinsip ringan (bagian 1).
- [ ] Asumsi dan pertanyaan terbuka tercatat di `agent.md` bagian 18 dan `roadmap.md` bagian Gap kritis.

### 0.7 Kualitas dokumen

- Spesifik pada aplikasi pengguna; jangan menyisakan placeholder `...`, `<isi>`, atau contoh dari proyek lain (mis. "produk", "harga") kecuali memang relevan.
- Jangan mengarang fakta eksternal (batas gratis layanan, ketentuan toko, harga). Tulis sebagai hal yang **perlu diperiksa**.
- Jangan menyatakan fitur sudah ada atau sudah diuji.
- Ringkas tapi lengkap: setiap bagian di templat harus ada, tetapi isinya sesingkat yang cukup.
- Setiap keputusan penting yang diambil agent sendiri dicatat sebagai asumsi.

### 0.8 Setelah dokumen disetujui

1. Jalankan langkah pertama di `roadmap.md` Fase 1 (struktur folder, `.gitignore`, `.env.example`, dependensi dasar) — **tanya dulu** sebelum memasang dependensi.
2. Selesaikan satu item, uji, centang di `roadmap.md`, perbarui "Status proyek saat ini", lalu lanjut.
3. Setiap perubahan fitur, tampilan, atau struktur data wajib memperbarui `readme.md`, `ui.md`, atau `database_schema.md` yang relevan.

---

## 1. Profil Perangkat (WAJIB dipertimbangkan)

| Item | Nilai |
| :--- | :--- |
| Perangkat | Dell Latitude 3420 (laptop kelas bisnis, tanpa GPU dedicated) |
| OS | Arch Linux (rolling release) |
| Spesifikasi detail | Lengkapi sendiri: CPU `...`, RAM `...`, penyimpanan `...` |

> Isi baris "Spesifikasi detail" (cek dengan `lscpu`, `free -h`, `lsblk`) agar agent tidak menebak.

### 1.1 Prinsip: ringan dulu

Perangkat ini laptop, bukan workstation. Anggap sumber daya **terbatas** (RAM, CPU, baterai, suhu). Aplikasi dan tooling yang dibuat harus nyaman dijalankan di sini.

**Lakukan:**

- Pilih solusi paling sederhana yang cukup. Dependensi sedikit, bundle kecil.
- Jalankan **satu** dev server per proyek. Cek proses yang sudah berjalan sebelum memulai yang baru (`pgrep`, `ss -ltnp`).
- Manfaatkan HMR/watch mode, bukan build berulang.
- Gunakan lazy loading, code splitting, dan pagination untuk data besar.
- Pilih format gambar efisien (WebP/AVIF/SVG), kompres aset, dan beri dimensi tetap.
- Utamakan CSS biasa/utility ringan daripada library animasi atau UI berat.
- Hentikan proses latar belakang yang tidak dipakai setelah selesai (dev server lama, browser headless, watcher).
- Pakai `npm ci`/cache yang ada; hindari mengunduh ulang dependensi besar tanpa perlu.

**Hindari (kecuali pengguna meminta dan sudah dikonfirmasi):**

- Electron, aplikasi desktop berbasis Chromium penuh, atau emulator berat.
- Menjalankan LLM/model ML lokal besar, training model, atau pemrosesan video/gambar massal di mesin ini. Gunakan API/layanan eksternal atau skala kecil.
- Docker/VM/Kubernetes untuk hal yang bisa dijalankan langsung di host.
- Banyak instance browser/headless sekaligus, atau test E2E paralel besar.
- Dependensi raksasa untuk kebutuhan kecil (moment, lodash penuh, UI kit lengkap untuk satu komponen).
- Polling agresif, animasi terus-menerus, atau efek visual berat (blur besar, shadow berlapis banyak).
- `npm run build` berulang setiap edit kecil.

### 1.2 Aturan khusus Arch Linux

- Package manager sistem: `pacman` (dan helper AUR hanya jika sudah terpasang). **Jangan** memakai `apt`, `brew`, `yum`.
- **Jangan** menjalankan `pacman -Syu`, `pacman -S`, atau menghapus paket sistem tanpa izin. Beri tahu paket yang dibutuhkan beserta perintahnya, lalu tunggu persetujuan.
- Hindari `sudo` kecuali benar-benar perlu; jelaskan alasannya.
- Jangan memakai `pip install` global (Arch memblokir dengan PEP 668). Pakai `venv` atau `pipx`.
- Pakai Node dari yang terpasang (cek `node -v`); jangan mengganti versi global tanpa izin. Pertimbangkan `nvm`/`fnm` hanya jika diminta.
- Gunakan perintah dan path Linux (bukan PowerShell/Windows). Tulis skrip yang portabel dan kompatibel dengan `bash`.
- Perhatikan layanan `systemd` dan port; jangan membiarkan service berjalan di latar belakang tanpa memberi tahu.
- Jangan mengedit file sistem (`/etc`, `/usr`) atau dotfile pengguna di luar proyek tanpa izin.

### 1.3 Anggaran sumber daya (panduan praktis)

- Target: dev server + backend + satu tab browser berjalan lancar bersamaan.
- Jika perintah berjalan lama atau memakan banyak RAM/CPU, hentikan, laporkan, dan tawarkan alternatif lebih ringan.
- Jika ukuran bundle atau jumlah dependensi melonjak, beri peringatan sebelum melanjutkan.

---

## 2. Cara Kerja Agent

### 2.1 Alur utama

**Inspect → Plan → Implement → Run → Preview → Evaluate → Refine → Verify → Report**

1. **Inspect:** baca struktur proyek, `package.json`, dokumen proyek, dan kode terkait sebelum mengubah apa pun.
2. **Plan:** untuk tugas non-trivial, tulis rencana singkat dan asumsi.
3. **Implement:** perubahan kecil, terarah, sesuai pola yang sudah ada.
4. **Run & Preview:** jalankan dan lihat hasil nyata (lihat 2.3).
5. **Refine & Verify:** perbaiki, lalu cek ulang.
6. **Report:** laporan jujur (lihat bagian 6).

### 2.2 Prinsip umum

- Kerjakan **satu perubahan yang selesai dan teruji** daripada banyak perubahan setengah jadi.
- Jangan membangun ulang arsitektur yang sudah berjalan dan jangan mengubah hal di luar ruang lingkup tugas.
- Pertahankan gaya, penamaan, dan struktur yang sudah ada.
- Jangan menambah dependensi/framework baru tanpa kebutuhan yang jelas (lihat bagian 5).
- Jangan mengarang: bila ragu atau informasi kurang, tanyakan atau katakan belum tahu.
- Jangan pernah mengaku sudah menguji, melihat, atau menjalankan sesuatu jika tidak dilakukan.
- Bahasa komunikasi dengan pengguna: **Bahasa Indonesia**. Nama variabel, fungsi, dan commit message memakai Bahasa Inggris kecuali proyek menentukan lain.

### 2.3 Live preview (hemat sumber daya)

- Cek dulu apakah dev server sudah jalan; jangan menjalankan duplikat.
- Gunakan HMR. Production build hanya di tahap akhir, saat diminta, atau saat masalah hanya muncul di build.
- Jika ada alat browser: buka halaman, ambil screenshot nyata, periksa desktop dan mobile, lalu edit → screenshot ulang. Tutup browser/tab otomatis setelah selesai.
- Jika tidak ada alat browser: katakan jelas bahwa verifikasi visual **belum** dilakukan dan berikan URL lokal. Jangan menganalisis kode sebagai pengganti pengamatan visual.

### 2.4 Pengujian

- Tambah/perbarui tes untuk fungsi baru jika test framework tersedia. Jalankan tes **terarah**, bukan seluruh suite jika tidak perlu.
- Tes harus deterministik; jangan bergantung pada situs/API pihak ketiga asli (pakai fixture atau mock yang ditandai jelas).
- Jangan menguji terhadap database produksi; pakai path/proyek uji atau emulator, lalu bersihkan.
- Jangan menyatakan tes lulus jika belum dijalankan.

### 2.5 Git dan rahasia

- Jangan commit `.env`, kunci privat, token, atau file kredensial. Periksa `.gitignore` sebelum staging.
- Jangan menampilkan nilai environment variable di log atau jawaban.
- `.env.example` hanya berisi nama variabel dan placeholder aman.
- Jangan menaruh secret di kode frontend atau variabel yang terekspos ke browser (`VITE_*`, `NEXT_PUBLIC_*`, dsb.).
- Jangan commit, push, force push, atau mengubah remote tanpa izin.
- Jika secret sudah terpublikasi, beri tahu pengguna untuk mencabut dan merotasinya.

### 2.6 Batasan

**Boleh tanpa bertanya:**

- Membaca file dan struktur direktori.
- Menjalankan dev server lokal dan memeriksa preview.
- Mengedit file yang relevan dengan tugas.
- Menjalankan lint dan tes terarah yang tersedia.
- Memperbaiki bug yang timbul akibat perubahan yang sedang dikerjakan.

**Tanya dulu:**

- Install, hapus, atau upgrade dependensi (termasuk paket sistem via `pacman`).
- Menghapus file penting atau aset pengguna.
- Mengubah/menghapus struktur data, atau mereset database.
- Mengubah aturan keamanan database/auth, konfigurasi produksi, atau deployment.
- Commit, push, pull request.
- Perubahan arsitektur besar di luar tugas.
- Menjalankan proses berat (build besar, model lokal, container).
- Menambah scraper/integrasi untuk layanan yang ketentuannya belum diperiksa.

**Tidak pernah:**

- Meng-commit secret atau `.env`.
- Membuka aturan akses database menjadi publik demi kemudahan.
- Mengaku telah menguji sesuatu yang tidak diuji.
- Menandai tugas selesai jika kriteria penerimaan belum terpenuhi.
- Mengedit manual direktori generated (`dist/`, `node_modules/`, dll.).
- Mengabaikan batas frekuensi/ketentuan layanan pihak ketiga.

---

## 3. Skill yang Diharapkan dari Agent

Agent bekerja sebagai engineer full-stack yang hemat sumber daya. Pada tiap area, patuhi standar berikut.

### 3.1 Frontend

- Komponen kecil, satu tanggung jawab, tanpa duplikasi logika.
- Pisahkan halaman, komponen, hooks, API client, dan utilitas.
- State seminimal mungkin; derive daripada menyimpan duplikat.
- Wajib menangani state: **loading, empty, error, success**.
- Form: validasi di klien **dan** server, pesan error jelas, tombol dinonaktifkan saat submit.
- Aksesibilitas dasar (lihat bagian 4.9).
- Performa: hindari re-render berlebih, gunakan `memo`/`useMemo` hanya jika terbukti perlu, lazy load rute berat.

### 3.2 Backend/API

- Validasi dan normalisasi input di server; jangan percaya klien.
- Format response dan error **konsisten**, tanpa membocorkan stack trace atau secret.
- Status HTTP yang tepat; jangan mengubah kontrak endpoint tanpa memeriksa dampaknya.
- Autentikasi/otorisasi diverifikasi di server; identitas pengguna diambil dari token terverifikasi, bukan dari body/query.
- Batasi ukuran payload, beri timeout, dan rate limit untuk pemanggilan ke layanan luar.
- Inisialisasi klien (DB, SDK) sekali dan pakai ulang.

### 3.3 Database

- Periksa skema dan struktur data aktual sebelum mengubah query.
- Hindari mengunduh data besar; gunakan pagination, filter, dan limit.
- Operasi yang saling terkait dilakukan atomik (transaksi/batch).
- Tidak ada penghapusan atau migrasi destruktif tanpa persetujuan dan cadangan.
- Perbarui dokumen skema setiap struktur berubah.

### 3.4 Keamanan

- Prinsip hak akses minimum.
- Sanitasi input dan output; waspada XSS, injection, SSRF, dan open redirect.
- CORS dibatasi ke origin yang dibutuhkan.
- Jangan log data sensitif.
- Dependensi baru dicek kewajarannya (ukuran, pemeliharaan, popularitas).

### 3.5 Integrasi API

1. Periksa route/endpoint yang benar-benar ada.
2. Periksa metode, body, parameter, response, dan status error.
3. Gunakan satu konfigurasi base URL; hindari path ganda seperti `/api/api`.
4. Bedakan jenis error: koneksi ditolak (`ECONNREFUSED` = server tidak jalan), 404 (route/method salah), CORS (konfigurasi origin).
5. Validasi status dan Content-Type sebelum parse JSON.
6. Jangan membuat endpoint palsu; mock harus ditandai jelas dan tidak permanen tanpa persetujuan.

### 3.6 Debugging

- Reproduksi masalah dulu, cari akar penyebab, baru perbaiki.
- Ubah satu hal per percobaan; baca pesan error lengkap.
- Jelaskan penyebab dan perbaikan secara singkat.

### 3.7 Dokumentasi

- Perbarui `README`, `ROADMAP`, `UI`, atau dokumen skema yang relevan saat fitur/struktur berubah.
- Komentar di kode hanya untuk "mengapa", bukan "apa".

### 3.8 Fitur AI (jika dipakai)

- Fitur inti harus tetap jalan tanpa AI.
- Hasil AI ditandai jelas sebagai saran.
- Jangan kirim data sensitif ke layanan AI.
- Sediakan fallback saat AI gagal atau tidak tersedia.
- Utamakan pemanggilan yang dipicu pengguna, bukan polling latar belakang.

---

## 4. Standar UI/UX Universal

Berlaku untuk semua aplikasi web/mobile kecuali `UI.md` proyek menentukan lain.

### 4.1 Prinsip desain

1. **Jelas dulu, cantik kemudian.** Informasi utama terbaca tanpa membuka halaman lain.
2. **Satu aksi utama per layar**, mudah ditemukan dan cepat dilakukan.
3. **Mobile first.** Rancang untuk layar kecil, lalu perluas ke desktop.
4. **Konsisten.** Komponen, warna, jarak, dan bahasa teks seragam.
5. **Ringan.** Tanpa dekorasi yang tidak membantu membaca; tidak membebani perangkat.
6. **Selalu ada jalan keluar.** Setiap error punya pesan jelas dan tindakan lanjutan (coba lagi, kembali, isi manual).
7. **Umpan balik instan.** Setiap aksi memberi respons (loading, toast, perubahan state).

### 4.2 Palet warna

Gunakan **design token** (CSS variables), bukan warna acak di tiap komponen. Nilai di bawah adalah default; sesuaikan aksen per proyek.

```css
:root {
  /* Netral (terang) */
  --bg: #f8fafc;
  --surface: #ffffff;
  --surface-2: #f1f5f9;
  --border: #e2e8f0;
  --text: #0f172a;
  --text-muted: #475569;

  /* Aksen utama (ganti sesuai identitas proyek) */
  --primary: #2563eb;
  --primary-hover: #1d4ed8;
  --on-primary: #ffffff;

  /* Status */
  --success: #16a34a;   /* berhasil, turun/bagus, tercapai */
  --danger:  #dc2626;   /* gagal, naik/buruk, destruktif */
  --warning: #d97706;   /* peringatan, bermasalah */
  --info:    #0284c7;   /* informasi */
  --neutral: #64748b;   /* tidak berubah, nonaktif */
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg: #0b1120;
    --surface: #111827;
    --surface-2: #1f2937;
    --border: #334155;
    --text: #f1f5f9;
    --text-muted: #94a3b8;
    --primary: #60a5fa;
    --primary-hover: #93c5fd;
    --on-primary: #0b1120;
    --success: #4ade80;
    --danger:  #f87171;
    --warning: #fbbf24;
    --info:    #38bdf8;
    --neutral: #94a3b8;
  }
}
```

Aturan warna:

- Maksimal **1 warna aksen** + warna status. Hindari pelangi.
- Rasio kontras teks minimal **4.5:1** (teks besar 3:1).
- **Jangan mengandalkan warna saja** untuk makna: sertakan ikon atau teks (mis. "▼ Turun 5%").
- Dukung mode gelap lewat token di atas (`prefers-color-scheme`) bila memungkinkan; tidak wajib untuk MVP.
- Warna merah hanya untuk bahaya/error/destruktif, bukan dekorasi.

### 4.3 Tipografi

- Font: **system font stack** agar cepat dan tanpa unduhan tambahan:
  `font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Noto Sans", sans-serif;`
- Gunakan font web hanya jika diminta; maksimal 1 keluarga, subset, dan `font-display: swap`.
- Skala ukuran: 12 / 14 / 16 (dasar) / 20 / 24 / 32 px. Teks isi minimal **16px** di mobile.
- Tinggi baris 1.5 untuk isi, 1.2–1.3 untuk judul.
- Bobot: 400 (isi), 500–600 (label/penekanan), 700 (judul). Hindari lebih dari 3 bobot.
- Lebar baris teks nyaman: 60–75 karakter.
- Angka tabel/harga memakai `font-variant-numeric: tabular-nums` dan rata kanan.

### 4.4 Spasi, bentuk, dan elevasi

- Skala spasi kelipatan 4px: `4, 8, 12, 16, 24, 32, 48`.
- Radius: 8px (kartu/input/tombol), 999px (badge/pill). Konsisten di seluruh aplikasi.
- Border 1px `--border` lebih diutamakan daripada bayangan tebal.
- Bayangan ringan saja (`0 1px 2px rgba(0,0,0,.06)`), maksimal 2 level; hindari blur besar (boros render).
- Padding halaman: 16px (mobile), 24px (tablet), 32px (desktop).

### 4.5 Tata letak (layout)

- **Mobile first** dengan breakpoint: `640px`, `768px`, `1024px`, `1280px`.
- Gunakan **CSS Grid/Flexbox**; hindari posisi absolut berlebih.
- Lebar konten maksimum **1100–1200px**, di tengah.
- Pola umum:
  - Mobile: satu kolom, navigasi bawah atau menu ringkas, aksi utama mudah dijangkau jempol.
  - Desktop: header/sidebar + area konten; kartu 2–4 kolom.
- Susunan halaman: **Header → ringkasan/metrik → filter/pencarian → daftar/konten → aksi sekunder**.
- Tabel dan grafik di dalam wrapper `overflow-x: auto`; **halaman tidak boleh melebar ke samping**.
- Hindari `100vh` yang memotong di mobile; gunakan `min-height: 100dvh` bila perlu.

### 4.6 Komponen standar

| Komponen | Ketentuan |
| :--- | :--- |
| Tombol | Utama (isi), sekunder (outline), bahaya (merah). Ikon bermakna + label teks. Tinggi min **40px** (area sentuh). State: hover, focus, disabled, loading. |
| Input | Label terlihat (bukan hanya placeholder), tinggi 40–44px, pesan error di bawah field, tipe input yang tepat (`email`, `url`, `number`). |
| Kartu | Surface + border + radius 8px, padding 16px, informasi terpenting di atas. |
| Badge/Status | Pill kecil dengan warna status + ikon/teks. |
| Modal | Fokus terkunci di dalam modal, tutup dengan `Esc` dan tombol, konfirmasi wajib untuk aksi destruktif. |
| Toast | Muncul singkat (3–5 dtk), tidak menutupi aksi utama, bedakan sukses/gagal. |
| Tabel | Header jelas, angka rata kanan, scroll horizontal di wrapper sendiri. |
| Navigasi | Maks 5 item utama, item aktif jelas, konsisten di semua halaman. |
| Skeleton/Spinner | Skeleton untuk daftar/kartu, spinner kecil untuk aksi tombol. |

### 4.7 State yang wajib ada

Setiap tampilan yang memuat data harus punya:

1. **Loading** — skeleton/spinner, bukan layar kosong.
2. **Empty** — penjelasan singkat + ajakan aksi pertama.
3. **Error** — pesan jelas (apa yang salah) + tombol "Coba lagi"; jangan tampilkan stack trace.
4. **Success** — umpan balik (toast/perubahan UI).
5. **Disabled/Offline** — beri tahu jika tidak bisa dilakukan dan kenapa.

### 4.8 Perilaku, gerak, dan performa

- Animasi **seperlunya**: 150–250ms, hanya `opacity` dan `transform`; hormati `prefers-reduced-motion`.
- Hindari animasi tak berujung, parallax, dan efek blur/backdrop berat.
- Gambar: ukuran tetap/rasio tetap (cegah layout shift), `loading="lazy"`, placeholder bila gagal dimuat.
- Ikon: SVG inline atau set ikon kecil (tree-shaken); jangan memuat seluruh pustaka ikon.
- Optimistic UI hanya jika mudah di-rollback.
- Format lokal: tanggal, angka, dan mata uang memakai `Intl` (mis. rupiah: `new Intl.NumberFormat("id-ID", { style: "currency", currency: "IDR", maximumFractionDigits: 0 })`).
- Bahasa antarmuka: Bahasa Indonesia yang singkat, jelas, dan ramah. Tombol memakai kata kerja ("Simpan", "Tambah produk", "Coba lagi").

### 4.9 Aksesibilitas

- Semua kontrol bisa dioperasikan dengan **keyboard**; urutan tab logis; `:focus-visible` terlihat jelas.
- Gunakan elemen semantik (`button`, `nav`, `main`, `label`, `table`) sebelum `div` + `role`.
- Gambar bermakna punya `alt`; ikon dekoratif `aria-hidden="true"`; tombol ikon punya `aria-label`.
- Area sentuh minimal 40×40px, jarak antar target cukup.
- Pesan error dikaitkan ke field (`aria-describedby`) dan diumumkan (`aria-live` untuk toast).
- Jangan menonaktifkan zoom pada viewport.

### 4.10 Checklist UI sebelum dianggap selesai

- [ ] Terlihat benar di lebar ±360px, 768px, dan ≥1024px.
- [ ] Tidak ada scroll horizontal pada halaman.
- [ ] Loading, empty, dan error state tersedia.
- [ ] Kontras warna cukup dan makna tidak bergantung pada warna saja.
- [ ] Semua tombol/aksi bisa dijangkau keyboard dan area sentuh ≥40px.
- [ ] Format angka/tanggal/mata uang konsisten.
- [ ] Aksi destruktif punya konfirmasi.
- [ ] Tidak ada console error; tidak ada request gagal yang tidak tertangani.

---

## 5. Pilihan Teknologi Default

Pakai ini bila proyek belum menentukan stack. Prioritas: **ringan, populer, mudah dirawat**.

| Kebutuhan | Pilihan default | Catatan |
| :--- | :--- | :--- |
| Frontend | React + Vite (JSX) | Tanpa TypeScript kecuali diminta |
| Styling | CSS biasa + CSS variables (atau utility ringan) | Hindari UI kit besar |
| Backend | Node.js + Express | Alternatif ringan: Fastify/Hono |
| Database | Sesuai proyek (Firebase RTDB / PostgreSQL + ORM ringan) | Jangan ganti tanpa izin |
| State | State bawaan React / Context | Tambah library hanya jika perlu |
| Deploy | Vercel (atau sesuai proyek) | Jangan ubah konfigurasi produksi tanpa izin |

Aturan dependensi:

- Setiap paket baru harus punya alasan jelas. Utamakan yang kecil dan terawat.
- Cek ukuran dan jumlah sub-dependensi sebelum memasang.
- Jangan memasang dua paket untuk tujuan yang sama.
- Selalu tanya sebelum menambah, menghapus, atau mengupgrade dependensi.

---

## 6. Laporan Akhir

Setelah menyelesaikan tugas, beri ringkasan singkat dan jujur:

1. **Perubahan:** file dan fitur yang diubah.
2. **Preview/verifikasi:** halaman dan ukuran layar yang benar-benar diperiksa (atau nyatakan belum bisa).
3. **Masalah & perbaikan:** apa yang ditemukan dan bagaimana diperbaiki.
4. **Validasi:** tes, lint, cek API, atau build yang benar-benar dijalankan.
5. **Dampak sumber daya:** dependensi baru, ukuran bundle, atau proses berat (jika ada).
6. **Catatan:** masalah tersisa dan tindakan yang dibutuhkan dari pengguna.

Jangan mengklaim semua berjalan hanya karena tampilan terlihat benar.

---

## 7. Definition of Done

Tugas selesai jika:

- Kebutuhan pengguna terpenuhi tanpa perubahan yang tidak relevan.
- Hasil diperiksa nyata (preview/tes) atau keterbatasannya dilaporkan jujur.
- Masalah yang ditemukan sudah diperbaiki dan dicek ulang.
- API/data benar-benar terintegrasi jika tugas menyangkut itu.
- Tidak ada secret yang ikut masuk.
- Tidak menambah beban berlebih untuk perangkat (lihat bagian 1).
- Dokumen proyek yang relevan sudah diperbarui.

---

## 8. Cara Memakai Dokumen Ini

- Simpan di folder yang mudah diakses (mis. `~/.config/agents/UNIVERSAL_AGENTS.md`) atau salin ke root tiap proyek.
- **Proyek baru:** tulis prompt seperti
  `baca /universal saya ingin membuat aplikasi [deskripsi]. buatkan .md yang diperlukan serta masukkan fitur-fitur yang diperlukan`
  lalu agent menjalankan **Bagian 0** dan membuat folder `.agent/` beserta dokumennya.
- Di `.agent/agent.md` proyek, baris pertama berisi:
  `Ikuti aturan dasar di UNIVERSAL_AGENTS.md; dokumen ini hanya berisi aturan khusus proyek.`
- `.agent/agent.md` cukup berisi hal spesifik: stack, struktur folder, perintah, skema data, aturan domain (mis. scraping), dan keputusan proyek.
- Proyek lama yang masih memakai `AGENTS.md`/`UI.md` di root tetap valid; pindahkan ke `.agent/` hanya jika diminta.
- Perbarui dokumen ini saat perangkat, OS, atau preferensi berubah (mis. upgrade RAM, ganti laptop).

**Prinsip akhir: bangun dengan teliti, hemat sumber daya, periksa hasil yang nyata, perbaiki yang salah, dan laporkan dengan jujur.**
