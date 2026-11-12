# PROJECT MIGRATION PROTOCOL
## Final Version — AI Lama Menyiapkan, AI Baru Meminta

**Tujuan:** Memindahkan pekerjaan dari chat lama ke chat baru atau AI/platform lain dengan akurat, tanpa mengharuskan USER menghafal nama file, urutan dokumen, atau kebutuhan teknis migrasi.

---

# 1. Prinsip Utama

1. USER cukup memberikan playbook ini kepada AI lama.
2. AI lama bertanggung jawab menyiapkan paket handover berdasarkan konteks percakapan dan kondisi project yang benar-benar dapat diaksesnya.
3. USER memindahkan hasil handover dari AI lama ke AI baru.
4. **AI baru wajib meminta sendiri dokumen starter pack dan file project tambahan yang dibutuhkan.**
5. AI baru tidak boleh menganggap dokumen handover sebagai pengganti seluruh source code atau konfigurasi lingkungan.
6. AI baru harus memverifikasi kondisi project sebelum mengubah file atau melanjutkan implementasi.
7. USER tidak boleh dibebani untuk menebak file apa yang harus dikirim. AI harus menyebutkan nama file, alasan, dan urutan pengirimannya secara jelas.

# 2. Alur Migrasi untuk USER

## Tahap A — Meminta AI Lama Menyiapkan Handover

1. Buka chat lama yang berisi pengerjaan project.
2. Upload file `PROJECT_MIGRATION_PROTOCOL.md`.
3. Kirim instruksi:

   > Ikuti seluruh instruksi pada PROJECT_MIGRATION_PROTOCOL.md. Siapkan paket handover project ini berdasarkan konteks dan kondisi aktual yang tersedia. Jangan mulai pekerjaan baru. Jangan mengarang informasi yang tidak diketahui. Buat dokumen handover yang bisa langsung saya pindahkan ke chat atau AI baru.

4. Tunggu AI lama menyelesaikan paket handover.
5. Simpan seluruh file hasil handover yang dibuat AI lama.

## Tahap B — Memulai Chat atau AI Baru

1. Buka chat baru atau AI/platform baru.
2. Upload seluruh dokumen hasil handover dari AI lama.
3. Kirim instruksi singkat:

   > Baca seluruh dokumen handover yang saya unggah. Ikuti prosedur pada AI_NEW_CHAT_START_HERE.md. Kamu harus meminta sendiri dokumen Starter Pack dan file project tambahan yang diperlukan. Jangan mulai implementasi sebelum proses pemulihan dan verifikasi konteks selesai.

4. **Jangan mengunggah seluruh Starter Pack sekaligus kecuali AI baru memang memintanya.**
5. AI baru harus mengidentifikasi dokumen yang belum tersedia, lalu meminta file-file tersebut kepada USER dalam satu permintaan yang terstruktur.
6. USER cukup mengunggah file yang diminta AI baru.
7. Setelah semua kebutuhan terpenuhi, AI baru melakukan verifikasi dan menyampaikan ringkasan kondisi project serta langkah berikutnya untuk dikonfirmasi USER.

---

# 3. Instruksi Wajib untuk AI Lama

Saat menerima playbook ini, bertindak sebagai **Outgoing Project Agent**.

## 3.1 Kumpulkan konteks

Gunakan hanya informasi yang benar-benar tersedia dari:
- Riwayat percakapan project yang dapat diakses.
- File dan source project yang memang tersedia atau telah diizinkan USER.
- Keputusan dan persetujuan eksplisit USER.
- Hasil implementasi, pengujian, audit, dan error yang benar-benar tercatat.

Jangan mengklaim telah memeriksa file, repository, database, server, atau environment jika akses tersebut tidak tersedia.

## 3.2 Buat paket handover

Buat file-file berikut. Jika suatu bagian belum diketahui, tulis `UNKNOWN`, `NOT PROVIDED`, atau `NOT VERIFIED` dan jelaskan apa yang perlu diperiksa. Jangan mengisi kekosongan dengan asumsi.

### Wajib dibuat

1. **AI_NEW_CHAT_START_HERE.md**
   - Dokumen pertama yang harus dibaca AI baru.
   - Berisi instruksi agar AI baru meminta Starter Pack dan file tambahan sendiri.
   - Menjelaskan urutan pemulihan, verifikasi, dan larangan implementasi prematur.

2. **PROJECT_STATE.md**
   - Nama dan tujuan project.
   - Kondisi terakhir project.
   - Stack, versi teknologi, struktur penting yang diketahui.
   - Fitur selesai, sedang dikerjakan, belum dimulai.
   - Error, bug, blocker, dan risiko yang masih terbuka.
   - Status build, test, deployment, dan environment berdasarkan bukti.
   - File atau modul terakhir yang disentuh.
   - Tugas berikutnya yang disarankan, tanpa menganggapnya sebagai persetujuan USER.

3. **FLOW_BASELINE.md**
   - Requirement dan ruang lingkup yang telah disetujui.
   - Aktor, role, alur pengguna, halaman, fitur, aturan bisnis, data, dan integrasi.
   - Kriteria penerimaan yang diketahui.
   - Pisahkan requirement disetujui dari usulan atau hal yang belum jelas.

4. **DECISIONS.md**
   - Keputusan penting yang telah disetujui USER.
   - Keputusan teknis dan alasan yang diketahui.
   - Hal yang ditolak atau dilarang.
   - Keputusan yang masih terbuka.
   - Jangan mengubah rekomendasi AI menjadi keputusan USER tanpa persetujuan eksplisit.

5. **HANDOVER_MANIFEST.md**
   - Daftar semua file handover.
   - Daftar file project/source yang diperlukan untuk melanjutkan.
   - Lokasi file jika diketahui.
   - Alasan setiap file diperlukan.
   - Status: tersedia, perlu diminta dari USER, tidak relevan, atau tidak diketahui.

### Buat jika relevan

- `WBS.md` atau daftar pekerjaan dan progres.
- `QA_REPORT.md` dan ringkasan hasil testing.
- `ARCHITECTURE.md` atau dokumentasi arsitektur.
- `API_DOCUMENTATION.md`, skema database, atau data model.
- `DEPLOYMENT.md` dan catatan environment.
- `KNOWN_ISSUES.md` atau daftar bug.
- Screenshot, mockup, spesifikasi desain, dan referensi.
- Catatan migrasi atau perubahan besar.

Jangan membuat file kosong hanya untuk memenuhi daftar. Jika tidak relevan, catat alasannya di manifest.

## 3.3 Source project

AI lama harus mengidentifikasi source yang diperlukan untuk pekerjaan berikutnya. Jika memiliki akses dan izin, siapkan salinan atau daftar file yang relevan. Jika tidak memiliki akses, tuliskan path atau nama file yang perlu USER unggah.

Jangan pernah memasukkan password, API key, token, private key, credential, file `.env`, atau secret lain ke dalam dokumen handover. Catat nama variabel atau kebutuhan konfigurasi tanpa nilainya.

## 3.4 Validasi paket

Sebelum menyatakan selesai:
- Pastikan `AI_NEW_CHAT_START_HERE.md` menjadi entrypoint AI baru.
- Pastikan status fakta, asumsi, dan hal yang belum diverifikasi dibedakan.
- Pastikan keputusan USER tidak tercampur dengan saran AI.
- Pastikan manifest menunjukkan file apa yang belum tersedia.
- Pastikan tidak ada secret.
- Berikan daftar file hasil handover dan instruksi singkat kepada USER untuk memindahkannya.

---

# 4. Instruksi Wajib untuk AI Baru

Saat menerima `AI_NEW_CHAT_START_HERE.md` beserta dokumen handover, bertindak sebagai **Incoming Project Agent**.

## 4.1 Baca dan pahami dahulu

Baca semua dokumen handover yang tersedia. Gunakan `HANDOVER_MANIFEST.md` untuk memahami dokumen dan source yang masih belum tersedia.

Jangan langsung:
- Mengubah source code.
- Membuat ulang project.
- Menginstal atau menghapus dependency.
- Menjalankan migrasi database.
- Mengubah konfigurasi environment.
- Menganggap status dari AI lama pasti masih sama dengan kondisi aktual.

## 4.2 Minta Starter Pack kepada USER

**AI BARU WAJIB meminta dokumen Starter Pack yang belum tersedia. Jangan menunggu USER mengetahui nama file yang harus diunggah.**

Dalam satu pesan, periksa dan minta dokumen berikut jika belum diunggah:

- `MASTER_BOOTSTRAP.md` — instruksi utama dan aturan kerja AI.
- `07_PROJECT_CONTINUITY.md` — prosedur menjaga dan memulihkan konteks project.
- Modul lain dari Starter Pack yang relevan, berdasarkan kebutuhan project dan pekerjaan berikutnya.

Jangan meminta file yang sudah tersedia. Jangan meminta semua modul tanpa alasan. Namun, jika AI belum dapat menentukan modul mana yang relevan, minta daftar isi atau seluruh modul Starter Pack agar tidak salah memilih.

Contoh permintaan:

> Saya sudah membaca paket handover. Agar dapat melanjutkan project sesuai workflow yang berlaku, saya perlu dokumen Starter Pack berikut:
> 1. `MASTER_BOOTSTRAP.md`
> 2. `07_PROJECT_CONTINUITY.md`
> 3. [Modul lain yang relevan, jika diperlukan]
>
> Silakan unggah file-file tersebut. Setelah itu saya akan memeriksa kebutuhan source project dan meminta file tambahan yang diperlukan dalam satu daftar.

## 4.3 Minta source project yang diperlukan

Setelah menerima Starter Pack, periksa manifest dan kebutuhan pekerjaan berikutnya. Minta source atau artefak yang benar-benar diperlukan, misalnya:
- File atau folder source code terkait.
- File konfigurasi non-rahasia atau contoh konfigurasi.
- Skema database atau migration.
- Dokumentasi API.
- Asset, desain, dan referensi UI.
- Test, log, atau bukti error.
- File lain yang diperlukan untuk memverifikasi kondisi project.

**Permintaan file harus spesifik.** Sebutkan nama/path, tujuan pemeriksaan, dan apakah file wajib atau opsional. Gabungkan permintaan yang sudah dapat diketahui ke dalam satu pesan agar USER tidak bolak-balik mengunggah file.

Jangan meminta seluruh repository jika subset file cukup. Jika pemeriksaan menyeluruh memang diperlukan, jelaskan alasannya.

## 4.4 Verifikasi sebelum melanjutkan

Setelah dokumen dan file tersedia:
1. Cocokkan PROJECT_STATE dengan source aktual.
2. Periksa apakah struktur, dependency, dan versi masih sesuai.
3. Identifikasi perbedaan antara catatan handover dan kondisi terbaru.
4. Verifikasi keputusan dan requirement dari FLOW_BASELINE serta DECISIONS.
5. Tandai informasi yang kedaluwarsa atau belum dapat diverifikasi.
6. Sampaikan ringkasan kondisi project, blocker, risiko, dan rencana kerja.

AI baru harus meminta konfirmasi USER sebelum melakukan tindakan yang mengubah scope, keputusan produk, data, atau kondisi project yang berisiko.

## 4.5 Gerbang mulai bekerja

AI baru hanya boleh mulai implementasi setelah:
- Starter Pack yang diperlukan telah diterima dan dibaca.
- Dokumen handover telah dipahami.
- Source yang diperlukan telah tersedia atau keterbatasannya disetujui USER.
- Kondisi aktual telah diverifikasi sejauh akses memungkinkan.
- Ketidakjelasan yang berpengaruh terhadap pekerjaan telah diselesaikan.
- Tujuan pekerjaan berikutnya telah dikonfirmasi.

Jika ada prasyarat yang belum terpenuhi, jelaskan dan minta hanya informasi/file yang masih kurang.

---

# 5. Format Permintaan File oleh AI Baru

Gunakan format sederhana berikut. Jangan membuat USER mengisi formulir teknis yang tidak perlu.

**Dokumen yang dibutuhkan**
- `nama_file.md` — alasan singkat.

**Source/artefak yang dibutuhkan**
- `path/atau/nama_file` — alasan singkat dan bagian yang diperlukan.

**Yang belum dapat diverifikasi**
- Sebutkan keterbatasan akses atau informasi secara spesifik.

Ajukan semua kebutuhan yang sudah diketahui dalam satu pesan. Jika ada kebutuhan baru yang baru muncul setelah file diperiksa, jelaskan mengapa file tambahan tersebut baru diperlukan.

---

# 6. Aturan Keamanan dan Integritas

- Jangan meminta atau mengunggah credential, password, API key, token, private key, atau secret.
- Jangan menampilkan nilai secret dalam laporan atau log handover.
- Jangan menghapus, menimpa, memindahkan, atau mengubah file project tanpa otorisasi yang sesuai.
- Jangan menjalankan perintah destruktif atau migrasi yang berisiko tanpa persetujuan eksplisit.
- Jangan menganggap hasil kerja AI lama pasti benar; verifikasi dengan bukti yang tersedia.
- Jangan mengarang hasil test, status build, isi file, atau kondisi deployment.
- Pisahkan fakta, asumsi, rekomendasi, dan keputusan USER.
- Hormati batasan akses dan izin yang ditentukan USER.

---

# 7. Perbedaan Jenis Migrasi

## Chat baru, project/workspace yang sama

Sebagian file dan tool mungkin masih tersedia, tetapi AI tetap harus membaca handover dan memeriksa konteks yang benar-benar tersedia. Jangan menganggap semua riwayat otomatis terbawa.

## AI atau platform baru

Dokumen handover dan Starter Pack perlu diberikan secara eksplisit. Tool, MCP server, koneksi akun, skill, izin, credential, runtime, dan akses komputer **tidak otomatis berpindah** hanya karena file Markdown diunggah. AI baru harus memeriksa kemampuan yang tersedia di platform barunya.

## Project dipindahkan ke komputer atau workspace lain

Selain dokumen, source project, asset, konfigurasi aman, database yang diperlukan, dan instruksi setup harus disediakan secara terpisah. Jangan menaruh secret di paket migrasi.

---

# 8. Checklist Sederhana untuk USER

- [ ] Upload playbook ini ke AI lama.
- [ ] Minta AI lama menyiapkan paket handover.
- [ ] Simpan seluruh hasil handover.
- [ ] Upload hasil handover ke AI baru.
- [ ] Tunggu AI baru meminta Starter Pack dan source tambahan.
- [ ] Upload file yang diminta AI baru.
- [ ] Baca ringkasan verifikasi dan konfirmasi sebelum implementasi.

**USER tidak bertugas menebak dokumen yang diperlukan. AI baru bertanggung jawab membaca manifest, mengenali kekurangan, dan meminta file yang dibutuhkan.**

---

# 9. Instruksi Singkat Siap Salin

## Untuk AI lama

> Ikuti PROJECT_MIGRATION_PROTOCOL.md. Siapkan seluruh paket handover project berdasarkan konteks dan kondisi aktual yang dapat kamu akses. Buat AI_NEW_CHAT_START_HERE.md sebagai entrypoint AI baru. Jangan mengarang, jangan memasukkan secret, dan jangan mulai pekerjaan baru. Beri tahu saya file apa saja yang harus dipindahkan.

## Untuk AI baru

> Baca seluruh dokumen handover yang saya unggah dan ikuti AI_NEW_CHAT_START_HERE.md. Kamu wajib meminta sendiri MASTER_BOOTSTRAP.md, 07_PROJECT_CONTINUITY.md, modul Starter Pack yang relevan, serta source project yang masih diperlukan. Gabungkan permintaan file yang sudah diketahui dalam satu pesan. Jangan mulai implementasi sebelum konteks dan kondisi project diverifikasi serta prasyarat penting dikonfirmasi.

---

**Akhir dokumen — PROJECT MIGRATION PROTOCOL v2.0**

[executed on device: LAPTOP-SEE8PP9P (22778139-9c01-4bc2-981a-bd27579daaba)]