# Universal AI Development Starter Pack

> One repository. One entry point. A consistent development workflow across AI tools.

Universal AI Development Starter Pack adalah kumpulan instruksi modular untuk membantu AI menjalankan proses pengembangan aplikasi secara terstruktur, mulai dari memahami kebutuhan, merancang UI/UX, menyusun engineering plan, mengimplementasikan fitur, melakukan pengujian, hingga menjaga kesinambungan project.

**Repository resmi:** https://github.com/zyxel-lyn/universal-ai-starter-kit

Berikan link repository dan satu instruksi onboarding kepada AI. AI harus membaca dokumentasi secara mandiri sebelum meminta brief project. Starter Pack ini tidak memberi izin otomatis untuk mengubah project atau sistem pengguna.

---

## 1. Onboarding Wajib untuk AI

Saat menerima link repository ini, lakukan seluruh langkah berikut sebelum memulai pekerjaan project.

### 1.1 Inventarisasi repository

1. Identifikasi owner, nama repository, branch, dan struktur repository dari link yang diberikan.
2. Periksa file dan folder Markdown yang benar-benar tersedia. Jangan mengandalkan daftar file yang diingat atau inventaris statis.
3. Baca README terlebih dahulu.
4. Setelah README, **wajib** baca seluruh 10 modul panduan di root. Daftar path persisnya ada di bagian 3.1. Ini cukup untuk memahami sistem kerja Starter Pack.
5. `references/` dan `scripts/` adalah pendukung, bukan modul panduan. Saat onboarding cukup catat keberadaannya. Jangan menuntaskan keduanya lebih dulu, dan jangan menganggapnya hilang bila tidak dibaca.
6. Catat nama file, fungsi, dan status pembacaannya agar tidak ada dokumen yang terlewat.

### 1.2 Baca seluruh isi sampai selesai

- Gunakan akses repository yang tersedia: GitHub, GitHub API, halaman file, atau raw file URL.
- Jika respons terpotong atau memiliki batas panjang, lanjutkan dari bagian/nomor baris berikutnya. Alternatifnya, ambil isi file melalui raw file atau API.
- Pastikan setiap dokumen dibaca dari awal sampai akhir sebelum menandainya selesai.
- Jangan menganggap respons yang terpotong sebagai kegagalan akses.
- **Jangan meminta ZIP, lampiran, atau unggahan ulang hanya karena batas panjang respons.** Lanjutkan pembacaan secara bertahap.

Jika sebuah file benar-benar tidak dapat diakses setelah jalur yang wajar dicoba, laporkan nama file, kendala spesifik, dan metode akses yang sudah dicoba. Jangan mengaku telah membaca file yang gagal diakses.

### 1.3 Pahami hubungan dan prioritas dokumen

Perlakukan dokumen Starter Pack sebagai satu sistem instruksi yang saling melengkapi:

- `MASTER_BOOTSTRAP.md` adalah pusat aturan kerja, prinsip pengambilan keputusan, dan routing ke modul.
- Modul bernomor menjelaskan prosedur spesialis sesuai tahap atau kebutuhan pekerjaan.
- `PROJECT_MIGRATION_PROTOCOL.md` digunakan ketika USER meminta migrasi atau handover project, chat, AI, platform, maupun workspace.
- README adalah entry point onboarding dan tidak menggantikan aturan rinci di MASTER atau modul terkait.

Periksa referensi silang, nama file, istilah, urutan proses, dan kemungkinan instruksi yang bertentangan. Jika ada konflik, jelaskan konflik tersebut dan gunakan prioritas yang ditetapkan MASTER. Jangan menyelesaikan konflik dengan asumsi yang tidak didukung dokumen.

### 1.4 Batas izin

Pemberian link repository ini hanya merupakan izin untuk membaca dan memahami dokumentasi Starter Pack.

Link tersebut **bukan izin** untuk:

- membuat, mengubah, atau menghapus project maupun file pengguna;
- mengubah repository atau konfigurasi lokal;
- memasang, memperbarui, atau menghapus dependency, tools, MCP, maupun skills;
- menjalankan setup yang mengubah sistem;
- melakukan commit, push, deployment, migrasi, atau tindakan destruktif.

Minta instruksi dan persetujuan yang sesuai sebelum melakukan tindakan di luar pembacaan dokumentasi. Jangan menganggap rekomendasi AI sebagai keputusan atau persetujuan USER.

### 1.5 Laporan onboarding

Setelah seluruh dokumen berhasil dibaca, berikan satu laporan onboarding yang mencakup:

1. Inventaris aktual seluruh dokumen Markdown dan jumlahnya.
2. Fungsi singkat setiap dokumen.
3. Hubungan, prioritas, dan urutan penggunaan modul.
4. Hasil pemeriksaan konflik, referensi silang, dan file yang hilang.
5. Status pembacaan setiap file: selesai atau gagal, disertai alasan yang spesifik.
6. Kesiapan menerapkan workflow dan batasan yang masih ada.

Jangan menyatakan onboarding selesai jika masih ada dokumen yang belum dibaca. Setelah laporan onboarding diberikan, langsung mulai Product Discovery secara aktif:

1. Periksa pesan dan konteks yang masih berlaku agar informasi yang sudah diberikan tidak ditanyakan ulang.
2. Jika tujuan pekerjaan belum diketahui, tanyakan satu hal pembuka: project atau pekerjaan apa yang ingin dilakukan, dan hasil utama apa yang ingin dicapai?
3. Setelah pengguna menjawab, rangkum pemahaman singkat lalu ajukan pertanyaan lanjutan yang paling penting berdasarkan jawaban tersebut. Secara default, ajukan satu pertanyaan fokus per giliran. Jangan berhenti setelah pertanyaan pembuka atau menutup percakapan dengan pernyataan pasif bahwa AI menunggu instruksi.
4. Jika pekerjaan melibatkan project existing atau membutuhkan akses ke file/repository, pastikan lokasinya. Jika belum diketahui, tanyakan apakah project berada di **Local** (komputer pengguna) atau **GitHub**. Setelah itu, minta path folder lokal atau URL repository yang diperlukan; tanyakan branch hanya bila belum jelas atau relevan.
5. Jika project baru akan dibuat, tanyakan apakah targetnya Local atau GitHub, lalu pastikan folder/repository tujuan sebelum membuat file, folder, atau repository. Untuk GitHub, izin membuat repository dan melakukan push harus dipastikan secara terpisah.
6. Jawaban tentang lokasi bukan izin untuk mengakses atau mengubah project. Minta izin terpisah sebelum melakukan pemeriksaan read-only. Izin membaca tidak otomatis memberi izin menulis, menghapus, memasang tools, commit, atau push.

Jangan membuat atau mengubah project sebelum tujuan dan ruang lingkup awal dipahami serta izin yang sesuai tersedia.

---

## 2. Cara Menggunakan untuk Pengguna

### Memulai AI baru

1. Pastikan AI yang digunakan dapat mengakses repository GitHub.
2. Kirim link repository resmi berikut:

   https://github.com/zyxel-lyn/universal-ai-starter-kit

3. Kirim satu instruksi onboarding berikut:

   > Ini adalah Universal AI Development Starter Pack: https://github.com/zyxel-lyn/universal-ai-starter-kit
   >
   > Baca README dan seluruh dokumen Markdown Starter Pack sampai selesai menggunakan akses repository secara mandiri. Lakukan inventarisasi, pahami hubungan dan prioritas dokumen, periksa konflik, lalu berikan satu laporan onboarding.
   >
   > Jangan meminta ZIP atau unggahan ulang hanya karena respons pembacaan terpotong; lanjutkan membaca secara bertahap sampai seluruh dokumen selesai dibaca.
   >
   > Setelah selesai membaca, mulai Product Discovery secara aktif. Jika tujuan belum saya jelaskan, tanyakan project/pekerjaan apa yang ingin dilakukan dan hasil utamanya. Setelah jawaban saya, lanjutkan dengan pertanyaan penting berikutnya, secara default satu pertanyaan fokus per giliran. Jika project existing yang perlu diperiksa atau project baru yang akan dibuat, pastikan apakah targetnya Local atau GitHub; minta path folder atau URL repository setelahnya. Lokasi bukan izin: minta izin read-only secara terpisah sebelum memeriksa project. Jangan berhenti dengan pernyataan menunggu instruksi dan jangan mengubah project tanpa izin yang sesuai.

4. Setelah onboarding, AI harus menjalankan discovery secara bertahap dan aktif: gali tujuan, lanjutkan pertanyaan berdasarkan jawaban, serta pastikan lokasi Local/GitHub bila project perlu diakses atau dibuat.
5. Lokasi project dan izin pemeriksaan harus dikonfirmasi secara terpisah; jangan membaca atau mengubah project sebelum izin yang sesuai diberikan.

### Jika AI benar-benar tidak dapat mengakses repository

AI harus menjelaskan kendala akses secara spesifik dan menyebutkan file yang gagal diakses setelah mencoba metode yang tersedia. USER baru perlu menyediakan dokumen secara manual jika memang ada hambatan akses nyata yang tidak dapat diatasi AI.

### Memulai project

Setelah onboarding, sampaikan ide, kebutuhan, atau masalah yang ingin diselesaikan. AI harus mengikuti `MASTER_BOOTSTRAP.md` untuk menentukan proses dan modul yang relevan.

### Melanjutkan atau memindahkan project

Untuk project yang sudah berjalan dan perlu dipindahkan ke chat, AI, platform, atau workspace lain, gunakan `PROJECT_MIGRATION_PROTOCOL.md`. Protokol migrasi tidak menggantikan onboarding Starter Pack.

---

## 3. Struktur dan Peran Dokumen

Struktur repository aktual adalah sumber kebenaran. AI wajib menginventarisasi ulang isi repository saat onboarding dan tidak boleh mengarang file yang tidak tersedia.

### 3.1 Daftar Lengkap File (13 file)

Daftar ini disediakan agar AI yang **tidak dapat melihat struktur direktori** (misalnya web chat yang hanya bisa membuka URL satu per satu) tetap tahu persis file apa yang ada dan di path mana. Gunakan daftar ini sebagai rujukan; tetap verifikasi keberadaannya bila punya akses repository.

**10 modul panduan** (wajib dibaca saat onboarding) — semuanya di root repository:

| No | Path persis | Peran |
|---|---|---|
| 1 | `README.md` | Entry point dan prosedur onboarding |
| 2 | `MASTER_BOOTSTRAP.md` | Aturan utama, prinsip kerja, dan routing modul |
| 3 | `01_PRODUCT_DISCOVERY.md` | Memahami masalah, kebutuhan, dan ruang lingkup produk |
| 4 | `02_UI_UX_SYSTEM.md` | Proses desain UI/UX dan konsistensi sistem visual |
| 5 | `03_ENGINEERING_STANDARDS.md` | Standar teknis dan implementasi engineering |
| 6 | `04_QA_TESTING.md` | Strategi verifikasi, pengujian, dan pelaporan kualitas |
| 7 | `05_PROJECT_MANAGEMENT.md` | Perencanaan, pelacakan, dan pengelolaan pekerjaan |
| 8 | `06_AUTO_SETUP_TOOLS.md` | Evaluasi dan penyiapan tools dengan persetujuan pengguna |
| 9 | `07_PROJECT_CONTINUITY.md` | Menjaga konteks, keputusan, dan kesinambungan project |
| 10 | `PROJECT_MIGRATION_PROTOCOL.md` | Handover dan migrasi project antar lingkungan kerja |

**2 file referensi** (bacaan pendukung, bukan modul panduan):

| No | Path persis | Isi |
|---|---|---|
| 11 | `references/STDIO_MCP_DRIVER.md` | Salinan driver stdio dan contoh browser test plan |
| 12 | `references/BROWSER_MCP_PROFILE.md` | Profil teknis Browser MCP |

**1 file skrip** (alat yang dieksekusi, bukan bacaan):

| No | Path persis | Peran |
|---|---|---|
| 13 | `scripts/stdio_mcp_client.mjs` | Driver MCP stdio generik untuk memverifikasi server shadcn dan Ant Design |

**Tidak ada file lain.** Jika AI tidak menemukan salah satu dari 13 path di atas, laporkan sebagai `NOT AVAILABLE` beserta metode akses yang dicoba. Jangan menyimpulkan seluruh repository kosong hanya karena satu cara akses gagal — coba jalur alternatif (halaman file GitHub, raw URL, atau GitHub API) sebelum menyimpulkan.

**Catatan untuk akses web chat:** halaman direktori `github.com/<owner>/<repo>/tree/main` sering gagal diambil oleh fasilitas pembacaan web. Kegagalan itu **bukan** bukti file tidak ada. Gunakan URL raw langsung:

```text
https://raw.githubusercontent.com/zyxel-lyn/universal-ai-starter-kit/main/<PATH>
```

Contoh: `.../main/references/BROWSER_MCP_PROFILE.md`

### 3.2 Folder pendukung

Kedua folder berikut bukan artefak project; jangan menyalinnya ke project baru.

`scripts/` berisi helper yang benar-benar dieksekusi.

**Perintah ini hanya membaca (read-only).** `stdio_mcp_client.mjs` melakukan handshake MCP lalu meminta daftar tool; ia tidak memasang, mengubah, atau menghapus apa pun. Keberadaan file ini **bukan** izin untuk instalasi, perubahan konfigurasi, atau modifikasi sistem. Persetujuan untuk itu tetap diatur `MASTER_BOOTSTRAP.md` bagian 8 dan `06_AUTO_SETUP_TOOLS.md` bagian 2.

`references/` berisi bacaan pendukung yang dipisahkan dari modul utama agar modul tetap ringkas saat dibaca di awal sesi.

Perbedaan keduanya: `scripts/` adalah alat yang dijalankan, `references/` adalah bacaan. Materi yang sudah dipindahkan ke `references/` tidak perlu dibaca rutin.

Daftar path lengkap, peran, dan cara mengaksesnya sudah ada di bagian 3.1 di atas.

---

## 4. Prinsip Kerja

- **Read first, understand second, execute only when instructed.**
- USER menentukan tujuan dan menyetujui keputusan penting.
- AI bertanggung jawab membaca dokumentasi yang dapat diakses secara mandiri.
- AI harus membedakan fakta, asumsi, rekomendasi, dan keputusan USER.
- Jangan mengarang isi dokumen, status runtime, keputusan, atau hasil pengujian.
- Jangan mengklaim pemeriksaan atau pengujian tanpa bukti.
- Jangan melakukan perubahan di luar ruang lingkup izin.
- Jika informasi penting belum tersedia, tanyakan hal yang paling diperlukan sebelum mengambil keputusan.

---

**Universal AI Development Starter Pack**  
*One repository. One entry point. A consistent workflow across AI tools.*
