# Universal AI Development Starter Pack

> Satu repository sebagai pintu masuk untuk AI dan panduan penggunaan untuk manusia.

Starter Pack ini berisi seperangkat instruksi dan modul kerja agar AI dapat membantu merencanakan, merancang, membangun, menguji, dan memelihara project aplikasi secara terstruktur. Repository ini dirancang agar mudah dibagikan: **cukup berikan link repository kepada AI yang mendukung akses GitHub atau pembacaan URL.**

---

## Untuk AI: Mulai dari Sini

Jika USER memberikan link repository ini, ikuti prosedur berikut secara berurutan.

### 1. Baca seluruh dokumentasi

1. Baca README ini terlebih dahulu.
2. Identifikasi seluruh file Markdown (`.md`) yang merupakan bagian dari Starter Pack di repository ini.
3. Baca isi setiap dokumen Starter Pack sampai selesai. Jangan hanya membaca nama file, ringkasan, atau beberapa baris pertama.
4. Buat inventaris nama file dan jelaskan fungsi setiap dokumen secara singkat.
5. Periksa apakah ada file yang hilang, nomor modul yang lompat, referensi nama file yang sudah berubah, atau instruksi yang saling bertentangan.
6. Jangan mengarang isi dokumen yang tidak dapat diakses. Jika akses GitHub atau isi file terbatas, sebutkan file yang belum terbaca dan minta USER menyediakan akses atau file tersebut.

### 2. Pahami sistem sebelum menerapkannya

- Perlakukan seluruh dokumen Starter Pack sebagai satu sistem instruksi yang saling melengkapi.
- `MASTER_BOOTSTRAP.md` adalah entrypoint aturan kerja utama jika dokumen tersebut tersedia.
- Modul lain menjelaskan prosedur khusus sesuai domainnya.
- Jangan menganggap README menggantikan isi dokumen modul.
- Jangan langsung mengubah source code, membuat project, menginstal dependency, atau menjalankan perintah hanya karena repository Starter Pack diberikan.
- Membaca dan memahami Starter Pack **bukan** izin untuk mengubah repository project milik USER.

### 3. Laporkan hasil pembacaan

Setelah seluruh dokumen berhasil dibaca, sampaikan:
- Daftar dokumen yang berhasil dibaca.
- Fungsi singkat masing-masing dokumen.
- Cara dokumen-dokumen tersebut bekerja sebagai satu workflow.
- File yang hilang, tidak dapat diakses, atau memiliki konflik.
- Konfirmasi bahwa instruksi telah dipahami dan siap digunakan.

Jangan mengklaim seluruh dokumen telah dibaca jika ada file yang belum berhasil diakses.

### 4. Tunggu tujuan USER

Setelah memahami Starter Pack, tanyakan atau tunggu instruksi project yang ingin dikerjakan. Terapkan modul yang relevan berdasarkan jenis pekerjaan dan tahap project. Jangan memaksakan seluruh prosedur ke pekerjaan yang tidak relevan.

### 5. Aturan khusus protokol migrasi

`PROJECT_MIGRATION_PROTOCOL.md` adalah **protokol khusus**, bukan instruksi kerja default.

- Aktifkan hanya jika USER meminta migrasi project, perpindahan chat/AI/platform, handover, atau pemulihan konteks project.
- Jangan menjalankan proses handover hanya karena file ini ada di repository.
- Jika migrasi diminta, baca dan ikuti seluruh protokol tersebut.
- Jika migrasi tidak diminta, abaikan prosedur migrasi dan jalankan workflow Starter Pack yang relevan.

### 6. Batas akses dan keamanan

- Repository publik dapat dibaca tanpa autentikasi, tetapi repository privat memerlukan akses yang sah.
- Jangan meminta USER membagikan password, token, API key, private key, atau credential.
- Jangan menyalin secret ke dokumen, issue, commit, atau laporan.
- Jangan melakukan commit, push, perubahan file, atau tindakan destruktif tanpa instruksi dan otorisasi USER.
- Jika AI tidak memiliki konektor GitHub atau akses URL, minta USER mengunggah dokumen yang diperlukan. Jangan berpura-pura telah membaca repository.

---

## Untuk Manusia: Cara Menggunakan Starter Pack

### A. Menggunakan Starter Pack pada AI baru

1. Pastikan AI yang digunakan dapat membuka link GitHub. Repository publik paling mudah dibaca.
2. Salin link repository ini.
3. Tempel link tersebut ke chat AI.
4. Minta AI membaca README dan seluruh dokumen Markdown Starter Pack.
5. AI seharusnya membuat inventaris dokumen, menjelaskan workflow, dan melaporkan jika ada file yang tidak dapat dibaca.
6. Setelah AI memahami Starter Pack, berikan tujuan atau project yang ingin dikerjakan.

**Contoh prompt untuk AI:**

> Ini adalah repository Universal AI Development Starter Pack: [TEMPEL LINK REPOSITORY].
>
> Baca README terlebih dahulu, lalu baca seluruh file Markdown Starter Pack di repository sampai selesai. Pahami hubungan dan prioritas antar dokumen. Jangan mulai membuat atau mengubah project dulu. Setelah selesai, laporkan daftar dokumen yang dibaca, fungsi masing-masing, konflik atau file yang hilang, dan konfirmasikan bahwa kamu siap menerapkan workflow ini.

### B. Menggunakan Starter Pack pada AI yang tidak bisa membaca GitHub

Jika AI tidak dapat mengakses repository:
1. Minta AI menjelaskan bahwa akses repository tidak tersedia.
2. Unduh repository sebagai ZIP dari GitHub, atau unduh dokumen yang diperlukan.
3. Unggah ZIP/dokumen ke AI jika platform mendukungnya.
4. Minta AI membaca seluruh dokumen Markdown, bukan langsung mengerjakan project.

### C. Menggunakan untuk project baru

1. Berikan link repository Starter Pack.
2. Tunggu AI membaca dan memahami seluruh dokumen.
3. Sampaikan ide atau kebutuhan project.
4. Ikuti tahap discovery dan persetujuan yang diminta AI.
5. AI menerapkan modul yang relevan sesuai tahap project.

### D. Memindahkan project lama ke chat atau AI baru

1. Pada AI lama, minta AI menyiapkan dokumen handover dengan mengikuti `PROJECT_MIGRATION_PROTOCOL.md`.
2. Simpan hasil handover project.
3. Berikan hasil handover dan link repository Starter Pack kepada AI baru.
4. AI baru membaca dokumen handover dan Starter Pack.
5. AI baru wajib meminta sendiri file atau informasi project yang belum tersedia.
6. Setelah konteks dan source diverifikasi, lanjutkan pekerjaan sesuai persetujuan USER.

Protokol migrasi hanya digunakan untuk perpindahan project yang memang sudah berjalan. Untuk project baru, tidak perlu membuat dokumen handover.

---

## Struktur dan Inventaris Repository

AI wajib memeriksa struktur repository yang aktual dan menginventarisasi semua dokumen Markdown. Jangan mengandalkan daftar statis yang mungkin tertinggal ketika modul ditambah, dihapus, atau diganti nama.

| Jenis | Fungsi |
|---|---|
| README | Pintu masuk untuk AI dan panduan penggunaan untuk manusia |
| Master Bootstrap | Aturan utama dan entrypoint workflow AI |
| Modul bernomor | Panduan khusus untuk discovery, UI/UX, engineering, QA, setup, dan continuity |
| Project Migration Protocol | Prosedur khusus untuk handover dan migrasi project |

Jika penomoran modul tidak berurutan atau ada dokumen yang belum tersedia, AI harus melaporkannya secara transparan. Jangan membuat atau mengisi modul yang hilang tanpa instruksi USER.

---

## Prinsip Penggunaan

- **Read first, understand second, execute only when instructed.**
- USER menentukan tujuan dan menyetujui keputusan penting.
- AI bertanggung jawab memahami dokumentasi, mengidentifikasi kebutuhan, dan meminta informasi/file yang belum tersedia.
- AI harus membedakan fakta, asumsi, rekomendasi, dan keputusan USER.
- Tidak ada klaim pemeriksaan atau pengujian tanpa bukti.
- Tidak ada perubahan project tanpa otorisasi yang sesuai.

---

**Universal AI Development Starter Pack**  
*One repository. One entry point. A consistent workflow across AI tools.*
