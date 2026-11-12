# Universal AI Development Starter Pack

> Satu repository sebagai entry point dan panduan kerja modular untuk AI serta manusia.

Starter Pack ini membantu AI memahami kebutuhan produk, merancang pengalaman pengguna, merencanakan dan membangun aplikasi, menguji hasil, serta menjaga kesinambungan project. Berikan **link repository dan satu instruksi awal**. AI bertanggung jawab membaca dokumentasi yang tersedia secara mandiri sebelum meminta informasi project.

---

## Untuk AI: Prosedur Onboarding Wajib

Ketika USER memberikan link repository Starter Pack ini, lakukan seluruh prosedur berikut dalam satu proses. Jangan meminta USER mengirim prompt tambahan untuk menyuruh Anda membaca file berikutnya.

### 1. Temukan dan inventarisasi dokumen

1. Identifikasi owner, repository, branch, dan root Starter Pack dari link yang diberikan.
2. Periksa daftar file Markdown aktual pada root repository. Jangan mengandalkan inventaris statis atau jumlah file yang diingat.
3. Baca README terlebih dahulu, kemudian seluruh dokumen Markdown yang merupakan bagian Starter Pack.
4. Gunakan akses repository, halaman file, GitHub API, atau raw file URL yang tersedia. Jika satu tampilan terpotong, lanjutkan dari bagian/offset berikutnya atau ambil raw file tersebut. Pemotongan respons bukan bukti bahwa file tidak dapat diakses.
5. Baca setiap file sampai akhir. Pastikan tidak ada bagian yang belum terbaca sebelum menandainya selesai.
6. Catat nama file, fungsi, status pembacaan, dan batas verifikasi secara internal selama proses.
7. Periksa referensi silang, file yang hilang, penomoran, istilah, versi publik, serta instruksi yang bertentangan.

**Jangan meminta ZIP, lampiran, atau unggahan ulang hanya karena satu respons pembacaan memiliki batas panjang.** Lakukan pembacaan bertahap dan gunakan jalur akses repository lain terlebih dahulu.

Jika setelah mencoba jalur akses repository yang wajar suatu file benar-benar tidak dapat diakses, sebutkan hanya file yang gagal beserta upaya yang telah dilakukan. Jangan mengklaim telah membacanya. Minta bantuan USER hanya untuk hambatan akses yang nyata, bukan untuk menggantikan pembacaan bertahap yang seharusnya dapat Anda lakukan sendiri.

### 2. Pahami sistem instruksi

- Perlakukan seluruh dokumen Starter Pack sebagai satu sistem yang saling melengkapi.
- `MASTER_BOOTSTRAP.md` adalah pusat aturan kerja dan routing modul.
- Modul bernomor menjelaskan prosedur spesialis. Baca dan terapkan modul sesuai kebutuhan pekerjaan setelah onboarding.
- `PROJECT_MIGRATION_PROTOCOL.md` hanya aktif ketika USER meminta migrasi, handover, atau perpindahan project/chat/AI/platform.
- Jangan menganggap rekomendasi AI sebagai keputusan atau persetujuan USER.
- Jangan mengarang isi file, status runtime, keputusan, atau bukti.

### 3. Batas tindakan

Pemberian link Starter Pack hanya mengizinkan pembacaan dan pemahaman dokumentasi tersebut.

Itu **bukan** izin untuk:
- membuat atau mengubah project,
- mengubah file repository,
- menginstal atau menghapus dependency, tools, MCP, atau skills,
- menjalankan setup atau konfigurasi,
- melakukan commit, push, deployment, migrasi, atau tindakan destruktif.

Tindakan tersebut memerlukan instruksi dan persetujuan yang sesuai dengan ruang lingkupnya.

### 4. Laporan onboarding

Setelah semua dokumen berhasil dibaca, berikan satu laporan awal yang memuat:
- jumlah dan daftar file Markdown aktual,
- fungsi singkat setiap dokumen,
- hubungan dan urutan kerja antar modul,
- konflik atau referensi yang tidak valid,
- status pembacaan setiap file,
- konfirmasi kesiapan menerapkan workflow.

Jangan mengirim laporan final sebelum pembacaan seluruh file selesai. Setelah laporan diberikan, tunggu tujuan project dari USER. Jangan memulai pekerjaan project hanya karena Starter Pack diberikan.

---

## Untuk Manusia: Cara Menggunakan

### A. Memulai AI baru

1. Pastikan repository dapat diakses oleh AI yang digunakan.
2. Kirim link repository.
3. Sertakan satu instruksi awal, misalnya:

> Ini adalah Universal AI Development Starter Pack: https://github.com/zyxel-lyn/universal-ai-starter-kit

Baca README dan seluruh dokumen Markdown Starter Pack sampai selesai menggunakan akses repository secara mandiri. Lakukan inventarisasi, pahami hubungan dan prioritas dokumen, periksa konflik, lalu berikan satu laporan onboarding.

Jangan meminta ZIP atau unggahan ulang hanya karena respons pembacaan terpotong; lanjutkan membaca secara bertahap sampai seluruh dokumen selesai dibaca.

Jangan mulai mengubah atau membuat project sebelum saya memberikan tujuan pekerjaan.

4. Tunggu laporan onboarding. Setelah itu, berikan tujuan project.

### B. Jika AI benar-benar tidak memiliki akses repository

AI harus menjelaskan keterbatasan akses secara spesifik setelah mencoba jalur yang tersedia. Barulah USER dapat menyediakan dokumen melalui cara yang didukung platform. Keterbatasan platform tidak boleh disamarkan sebagai keberhasilan pembacaan.

### C. Project baru

Setelah onboarding selesai, sampaikan ide atau kebutuhan project. AI akan memilih modul yang relevan dan memulai discovery sesuai MASTER.

### D. Project yang sedang berjalan

Jika ingin memindahkan project ke chat, AI, platform, atau workspace lain, gunakan `PROJECT_MIGRATION_PROTOCOL.md`. Protokol ini terpisah dari onboarding project baru.

---

## Struktur Repository

Inventaris selalu ditentukan dari isi repository aktual. Secara umum, Starter Pack terdiri dari:
- README sebagai entry point onboarding,
- MASTER sebagai pusat aturan dan routing,
- modul spesialis untuk discovery, UI/UX, engineering, QA, project management, setup tools, dan continuity,
- protokol migrasi khusus.

Jika file ditambah, dihapus, atau diganti nama, AI wajib mengikuti struktur aktual dan memperbarui laporannya. Jangan membuat file yang hilang tanpa instruksi USER.

---

## Prinsip Utama

- **Read first, understand second, execute only when instructed.**
- USER menentukan tujuan dan menyetujui keputusan penting.
- AI bertanggung jawab membaca dokumentasi yang dapat diakses secara mandiri.
- AI harus membedakan fakta, asumsi, rekomendasi, dan keputusan USER.
- Tidak ada klaim pemeriksaan atau pengujian tanpa bukti.
- Tidak ada perubahan project tanpa otorisasi yang sesuai.

---

**Universal AI Development Starter Pack**  
*One repository. One entry point. A consistent workflow across AI tools.*
