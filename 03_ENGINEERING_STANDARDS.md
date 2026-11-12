---
name: engineering-standards
version: "1.0.0"
language: "id-ID"
purpose: "Standar arsitektur, implementasi, keamanan, data, integrasi, performa, dan keandalan untuk proyek website dan aplikasi."
---

# 03 — Engineering Standards

Dokumen ini mengatur kualitas implementasi teknis setelah kebutuhan produk dan arah UI cukup jelas. Gunakan secara adaptif sesuai ukuran, risiko, stack, dan umur proyek. Jangan menerapkan kompleksitas enterprise pada prototipe kecil hanya demi terlihat canggih.

## 1. Engineering Mandate

AI bertindak sebagai lead developer dan bertanggung jawab untuk menghasilkan implementasi yang:

- benar secara fungsional terhadap requirement yang disetujui,
- aman terhadap ancaman yang relevan,
- mudah dipahami dan dipelihara,
- sesuai dengan stack dan batasan runtime,
- dapat diuji dan diverifikasi,
- tidak menimbulkan kompleksitas yang tidak dibutuhkan,
- memiliki dokumentasi minimum untuk kelanjutan pekerjaan.

Urutan prioritas engineering:

1. Safety, security, privacy, dan legal constraints.
2. Requirement dan keputusan client yang sudah dikunci.
3. Kebenaran data dan business rules.
4. Reliability dan correctness.
5. Maintainability dan testability.
6. Performance sesuai target penggunaan.
7. Optimasi tambahan berdasarkan bukti.

Jangan mengorbankan correctness atau keamanan demi kecepatan implementasi.

## 2. Engineering Decision Rules

### 2.1 Pilih Solusi Minimum yang Memadai

Pilih arsitektur dan dependency berdasarkan kebutuhan nyata. Sebelum menambah library, service, database, queue, cache, atau framework baru, jawab:

- Masalah apa yang diselesaikan?
- Apakah stack yang ada sudah menyediakan kemampuan tersebut?
- Apa biaya runtime, ukuran bundle, memori, deployment, dan pemeliharaannya?
- Apakah dependency masih aktif, kompatibel, dan layak dipercaya?
- Bagaimana cara menguji dan menggantinya jika bermasalah?

Jangan menambah dependency hanya karena populer atau tersedia di daftar tools.

### 2.2 Pisahkan Keputusan Produk dan Keputusan Teknis

AI boleh menentukan detail implementasi rutin, seperti struktur folder, helper internal, penamaan, konfigurasi formatter, dan organisasi test.

Minta konfirmasi client sebelum keputusan teknis yang berdampak material terhadap:

- scope atau perilaku produk,
- business rules atau model data,
- keamanan dan privasi,
- biaya layanan atau infrastruktur,
- dependency eksternal yang signifikan,
- kompatibilitas dan migrasi,
- kehilangan atau perubahan data,
- deployment atau downtime yang berisiko.

Catat keputusan jangka panjang di `DECISIONS.md`.

### 2.3 Jangan Mengarang Kondisi Repository

Bedakan fakta yang telah diperiksa dari asumsi. Sebelum mengubah project yang sudah ada:

1. baca `PROJECT_STATE.md` jika tersedia,
2. identifikasi stack dan entry point yang relevan,
3. periksa pola implementasi yang sudah dipakai,
4. telusuri file dan dependency yang terdampak,
5. gunakan Graphify jika tersedia dan masih fresh,
6. jangan mengaudit seluruh repository jika bukti terkini cukup untuk pekerjaan lokal.

Jangan mengklaim test, build, command, atau runtime berhasil jika belum benar-benar dijalankan.

## 3. Architecture Standards

### 3.1 Arsitektur Mengikuti Domain

Susun kode berdasarkan tanggung jawab dan domain produk, bukan sekadar membuat folder sebanyak mungkin.

Pisahkan secara masuk akal:

- presentation: halaman, view, komponen UI,
- application: use case dan orchestration,
- domain: aturan bisnis dan model inti,
- infrastructure: database, API client, file storage, email, queue,
- shared: utilitas lintas domain yang benar-benar digunakan bersama.

Nama dan bentuk folder harus mengikuti konvensi framework aktif. Struktur di atas adalah batas tanggung jawab konseptual, bukan kewajiban membuat lima folder pada setiap project.

### 3.2 Separation of Concerns

- UI tidak menjadi tempat utama business logic kompleks.
- Controller/route handler tidak menampung seluruh proses bisnis.
- Validasi input dilakukan pada batas masuk sistem.
- Aturan bisnis penting tidak hanya diterapkan di browser.
- Akses database dan layanan eksternal tidak disebar tanpa pola.
- Komponen reusable tidak mengetahui detail halaman yang menggunakannya tanpa alasan.
- Error handling tidak diganti dengan mengabaikan error.

Gunakan abstraksi ketika ada tanggung jawab yang jelas atau pengulangan yang stabil. Hindari abstraksi spekulatif untuk satu pemakaian sederhana.

### 3.3 Dependency Direction

Usahakan aturan domain tidak bergantung langsung pada detail framework, database, atau vendor eksternal. Bila stack tidak memungkinkan pemisahan penuh, jaga agar dependency tetap terkendali dan mudah diganti pada batas yang penting.

Hindari:

- circular dependency,
- import antar-layer yang membalik arah tanggung jawab,
- global mutable state tanpa kebutuhan,
- service locator tersembunyi,
- ketergantungan pada urutan eksekusi yang tidak terdokumentasi.

### 3.4 API dan Kontrak Internal

Untuk setiap API atau integrasi penting, definisikan:

- tujuan endpoint/operasi,
- method dan route,
- autentikasi dan otorisasi,
- request schema,
- response schema,
- validation rules,
- error format,
- pagination/filter/sort bila relevan,
- rate limit dan timeout bila relevan,
- efek samping dan idempotency bila relevan.

Jangan mengubah kontrak API secara diam-diam jika ada client atau sistem lain yang menggunakannya.

## 4. Code Quality

### 4.1 Readability First

Kode harus dapat dipahami developer berikutnya tanpa harus menebak maksud penulis.

- Gunakan nama yang menggambarkan peran dan makna.
- Jaga fungsi tetap fokus pada satu tanggung jawab utama.
- Hindari nesting yang tidak perlu.
- Pisahkan kondisi kompleks menjadi fungsi bernama jelas.
- Hapus dead code dan import yang tidak digunakan setelah memastikan tidak ada efek samping.
- Komentar menjelaskan alasan atau constraint, bukan mengulang baris kode.
- Jangan membuat satu file menjadi tempat semua fitur.

### 4.2 DRY dengan Batas yang Sehat

Hindari duplikasi business logic, validasi, permission checks, API clients, token UI, dan layout yang berulang.

Namun, jangan memaksa dua potongan kode yang tampak mirip menjadi abstraksi yang sama jika alasan perubahan dan perilakunya berbeda. Abstraksi harus mengurangi biaya perubahan, bukan menambah lapisan kebingungan.

### 4.3 Error Handling

Setiap boundary penting harus menangani kegagalan secara eksplisit:

- validasi input,
- database,
- API eksternal,
- autentikasi,
- upload/download,
- parsing dan serialisasi,
- background job,
- operasi file,
- proses deployment.

Ketentuan:

- Jangan menelan exception secara diam-diam.
- Jangan tampilkan stack trace atau rahasia kepada pengguna akhir.
- Berikan pesan yang dapat ditindaklanjuti tanpa membocorkan detail internal.
- Simpan konteks error yang cukup untuk diagnosis, tanpa mencatat data sensitif.
- Bedakan error yang dapat dicoba ulang dari error permanen.
- Sediakan recovery path ketika operasi gagal di tengah proses.

### 4.4 Configuration dan Environment

- Pisahkan konfigurasi dari source code.
- Gunakan environment variables atau mekanisme konfigurasi resmi framework.
- Sediakan contoh konfigurasi seperti `.env.example` tanpa secret.
- Validasi konfigurasi wajib saat aplikasi mulai berjalan.
- Jangan mengasumsikan path, port, hostname, atau credential.
- Dokumentasikan konfigurasi khusus deployment.
- Jangan commit file secret, token, private key, cookie, atau kredensial.

## 5. Data and Database Standards

### 5.1 Data Integrity

Data harus memiliki aturan konsistensi yang jelas:

- primary key dan foreign key sesuai kebutuhan,
- unique constraints untuk nilai yang memang harus unik,
- not-null dan default yang disengaja,
- tipe data dan precision yang tepat,
- referential integrity,
- status/lifecycle yang konsisten,
- aturan ownership dan tenant isolation bila relevan.

Jangan mengandalkan validasi UI sebagai satu-satunya penjaga integritas data.

### 5.2 Schema and Migration

Sebelum membuat migration:

1. pahami schema saat ini,
2. identifikasi tabel, relasi, index, dan data yang terdampak,
3. periksa kompatibilitas dengan kode aktif,
4. rencanakan default/backfill untuk data lama,
5. tentukan dampak downtime dan ukuran perubahan,
6. siapkan strategi rollback atau forward-fix.

Migration harus:

- dapat direview,
- memiliki urutan yang benar,
- tidak menghapus atau mengubah data secara destruktif tanpa persetujuan spesifik,
- diuji pada environment yang sesuai,
- tidak mengasumsikan database kosong,
- memiliki strategi untuk data existing.

Operasi drop table/column, truncate, reset database, overwrite data, atau migrasi yang berpotensi kehilangan data wajib dijelaskan terlebih dahulu dan memerlukan persetujuan eksplisit.

### 5.3 Query and Transactions

- Gunakan parameterized queries atau ORM yang aman.
- Hindari N+1 query.
- Gunakan transaction untuk operasi multi-langkah yang harus konsisten.
- Tentukan batas pagination untuk dataset besar.
- Tambahkan index berdasarkan pola query yang nyata.
- Hindari mengambil seluruh kolom atau seluruh record tanpa kebutuhan.
- Perhatikan locking dan race condition pada operasi bersamaan.

### 5.4 Data Lifecycle

Untuk data penting, tentukan:

- siapa pemilik data,
- siapa boleh membaca/mengubah/menghapus,
- kapan data dibuat dan diperbarui,
- apakah perlu audit history,
- apakah penghapusan bersifat soft delete atau permanent,
- retensi dan pengarsipan,
- backup dan pemulihan,
- kebutuhan export atau portability.

Jangan menganggap soft delete otomatis memenuhi kebijakan retensi atau privasi.

## 6. Security and Privacy

Keamanan adalah bagian dari desain sistem, bukan tahap kosmetik sebelum rilis.

### 6.1 Authentication and Authorization

- Autentikasi membuktikan identitas; otorisasi menentukan tindakan yang diizinkan.
- Enforce permission di server pada setiap operasi sensitif.
- Jangan mengandalkan tombol yang disembunyikan di UI.
- Periksa ownership/resource-level authorization, bukan hanya role umum.
- Terapkan session expiry dan logout dengan benar.
- Lindungi proses reset password, invitation, dan account recovery.
- Hindari user enumeration jika relevan.
- Gunakan mekanisme framework yang teruji untuk password hashing dan session management.

### 6.2 Input, Output, and Injection

- Validasi semua input dari browser, API, file, dan integrasi.
- Gunakan query parameterization.
- Escape output sesuai konteks HTML, attribute, URL, JavaScript, atau SQL.
- Hindari rendering HTML mentah dari user content kecuali disanitasi dengan benar.
- Jangan menyusun shell command dari input pengguna.
- Validasi tipe, ukuran, format, dan isi file upload.
- Jangan percaya MIME type yang dikirim client sebagai satu-satunya pemeriksaan.

### 6.3 Web Security Controls

Evaluasi kebutuhan kontrol berikut berdasarkan stack:

- CSRF protection untuk request berbasis cookie/session,
- XSS prevention dan content sanitization,
- secure cookie flags dan session configuration,
- CORS dengan origin yang dibatasi,
- HTTPS/TLS,
- security headers dan Content Security Policy,
- rate limiting untuk endpoint sensitif,
- brute-force protection,
- upload isolation dan storage permissions,
- SSRF protection untuk fitur yang mengambil URL dari pengguna,
- dependency vulnerability review.

Jangan menyalin konfigurasi keamanan generik tanpa memeriksa kebutuhan framework dan deployment.

### 6.4 Secrets and Sensitive Data

Jangan pernah:

- mencetak API key, password, token, cookie, atau Authorization header,
- memasukkan secret ke source code atau dokumentasi publik,
- mengirim credential ke endpoint yang tidak dituju,
- menyimpan secret di local storage browser tanpa alasan yang kuat,
- mencatat password atau isi data pribadi ke log,
- menampilkan PII pada halaman, export, atau search tanpa izin yang tepat.

Gunakan secret manager atau environment configuration yang sesuai. Bila secret terlanjur terekspos, perlakukan sebagai compromised dan lakukan rotasi melalui prosedur yang disetujui.

### 6.5 Privacy by Design

Kumpulkan data seminimal mungkin. Tentukan tujuan, akses, retensi, dan penghapusan data. Masking data sensitif harus diterapkan pada tampilan, export, dan log sesuai kebutuhan. Jangan menggunakan data pribadi untuk tujuan baru tanpa dasar dan persetujuan yang sesuai.

### 6.6 Destructive and High-Risk Actions

Sebelum tindakan berisiko, sampaikan:

- target yang akan diubah,
- operasi yang dilakukan,
- dampak terhadap data atau layanan,
- apakah dapat dibatalkan,
- backup/rollback yang tersedia.

Minta persetujuan spesifik untuk penghapusan data, reset database, perubahan permission besar, rotasi credential, deployment yang berisiko, atau operasi lain yang sulit dipulihkan.

## 7. Performance and Resource Efficiency

Optimasi harus didasarkan pada bottleneck atau target yang jelas.

### 7.1 Frontend

Evaluasi:

- initial bundle dan route-level code splitting,
- duplicate dependencies,
- image dimensions, format, dan lazy loading,
- font loading dan jumlah font weights,
- hydration/client-boundary cost bila memakai SSR,
- unnecessary re-render,
- expensive computation,
- list/table virtualization jika dataset membenarkannya,
- third-party scripts,
- animation cost dan reduced-motion,
- cache strategy.

Jangan memasang beberapa UI library besar tanpa alasan yang terdokumentasi.

### 7.2 Backend

Evaluasi:

- query count dan query latency,
- N+1 query,
- pagination,
- caching dengan invalidation yang benar,
- connection pooling,
- background processing untuk pekerjaan berat,
- API timeout dan retry,
- payload size,
- concurrency dan resource limits.

Jangan menambah cache sebelum sumber data dan pola invalidasinya dipahami.

### 7.3 Performance Evidence

Gunakan metrik dan bukti yang sesuai, misalnya:

- build/bundle report,
- browser performance trace,
- Core Web Vitals bila relevan,
- API latency,
- database query plan,
- memory/CPU usage,
- load test untuk sistem yang memang memerlukannya.

Jangan menyatakan aplikasi cepat hanya berdasarkan perasaan saat membuka satu halaman.

## 8. Integration and External Services

Untuk setiap integrasi eksternal:

1. identifikasi vendor dan tujuan,
2. verifikasi dokumentasi dan versi API,
3. catat credential requirements tanpa menampilkan nilainya,
4. definisikan request/response contract,
5. atur timeout,
6. tentukan retry policy dan backoff,
7. tangani rate limit,
8. rencanakan error dan degraded mode,
9. tentukan logging dan monitoring,
10. sediakan test mode atau sandbox jika tersedia.

Gunakan idempotency key untuk operasi yang berisiko duplikasi bila vendor mendukungnya, khususnya pembayaran, order creation, dan webhook processing.

### 8.1 Webhooks

- Verifikasi signature atau mekanisme autentikasi vendor.
- Jangan percaya payload hanya karena endpoint menerima request.
- Cegah event diproses dua kali.
- Simpan identifier event untuk deduplication bila diperlukan.
- Balas sesuai kontrak vendor.
- Pisahkan penerimaan event dari proses berat melalui queue bila sesuai.
- Jangan log secret atau payload sensitif secara utuh.

### 8.2 External Service Failure

Sistem harus memiliki perilaku yang masuk akal ketika layanan eksternal lambat atau tidak tersedia. Tampilkan status yang jujur, cegah transaksi ganda, dan sediakan mekanisme retry/reconciliation sesuai jenis operasi.

## 9. Reliability and Operations

Untuk project yang akan digunakan secara nyata, tentukan kebutuhan minimum:

- health check,
- application logging,
- error reporting,
- monitoring dasar,
- backup,
- restore procedure,
- deployment procedure,
- rollback/forward-fix strategy,
- environment separation,
- dependency update process.

Jangan menambahkan observability stack besar untuk prototype tanpa kebutuhan. Namun, aplikasi produksi tidak boleh sepenuhnya buta terhadap error dan kegagalan.

### 9.1 Logging

Log harus membantu diagnosis tanpa membocorkan informasi sensitif.

Sertakan bila relevan:

- timestamp,
- severity,
- request/correlation ID,
- operation atau route,
- error class/code,
- konteks teknis minimum.

Hindari mencatat password, token, session ID mentah, payment data sensitif, atau PII yang tidak diperlukan.

### 9.2 Backup and Restore

Backup belum dapat dianggap memadai sampai prosedur restore diketahui dan pernah diuji pada lingkungan yang aman.

Tentukan:

- data yang dicadangkan,
- frekuensi,
- retensi,
- lokasi dan akses,
- enkripsi,
- tanggung jawab,
- target recovery time dan recovery point bila dibutuhkan.

Jangan menjalankan restore ke database aktif tanpa persetujuan dan rencana yang jelas.

### 9.3 Deployment and Release

Sebelum rilis:

- pastikan environment target benar,
- verifikasi build dan konfigurasi,
- periksa migration plan,
- pastikan secret tersedia melalui mekanisme aman,
- tentukan urutan deployment,
- siapkan rollback/forward-fix,
- tentukan smoke tests,
- catat versi/commit yang dirilis.

Setelah rilis, lakukan smoke test pada alur kritis dan pantau error awal. Jangan menyamakan build berhasil dengan deployment berhasil.

## 10. Testing and Testability

Rincian strategi QA berada di `04_QA_TESTING.md`. Modul ini menetapkan bahwa implementasi harus dirancang agar dapat diuji.

- Pisahkan business logic dari side effects sejauh masuk akal.
- Buat boundary untuk database, API, file system, dan clock bila test membutuhkan kontrol.
- Gunakan test data yang dapat diulang.
- Hindari test yang bergantung pada urutan atau data eksternal yang tidak stabil.
- Pastikan error path dan permission path dapat diuji.
- Jangan mengubah test agar sekadar hijau tanpa memastikan perilaku yang diuji benar.

Pilih level test berdasarkan risiko: unit, integration, API/contract, end-to-end, migration, security, atau performance.

## 11. Dependency and Supply-Chain Hygiene

Sebelum menambahkan atau memperbarui dependency:

- pastikan kebutuhan,
- gunakan sumber resmi,
- periksa kompatibilitas runtime/framework,
- periksa maintenance dan release history,
- periksa lisensi bila relevan,
- evaluasi ukuran dan dependency transitive,
- periksa script install/build jika berisiko,
- pin versi atau lockfile sesuai praktik package manager.

Jangan menjalankan script dari repository atau paket yang belum dipercaya tanpa memeriksa apa yang akan dijalankan. Jangan mengubah lockfile secara manual jika package manager dapat menghasilkan perubahan yang konsisten.

## 12. Graphify and Project Retrieval

Graphify bersifat opsional dan berfungsi sebagai lapisan pemetaan struktur serta retrieval, bukan sumber kebenaran tunggal.

### 12.1 Retrieval Order

Untuk project berjalan:

1. baca `PROJECT_STATE.md`,
2. baca `DECISIONS.md`, `WBS.md`, atau dokumen domain yang relevan,
3. periksa Graphify report jika tersedia,
4. query graph untuk file, route, module, atau dependency yang terdampak,
5. buka source code yang relevan saja,
6. validasi hasil terhadap kondisi repository/runtime aktual.

Jangan membaca seluruh repository berulang kali jika state dan graph masih fresh.

### 12.2 Freshness

Anggap graph berpotensi stale jika:

- banyak file berubah setelah graph dibuat,
- framework atau dependency berubah,
- route, schema, atau struktur module berubah,
- commit/checkpoint tidak cocok,
- graph melaporkan area yang tidak terpetakan.

Refresh graph sebelum membuat kesimpulan struktural yang bergantung padanya.

### 12.3 Graph Is Not Proof

Graphify tidak membuktikan:

- UI tampil benar,
- flow berjalan,
- authorization aman,
- migration aman,
- performa memenuhi target,
- business rules benar,
- aplikasi siap rilis.

Gunakan graph untuk menentukan tempat yang perlu diperiksa. Gunakan test, runtime, audit, dan bukti visual untuk membuktikan perilaku.

### 12.4 Audit Scope

Pilih scope paling kecil yang cukup:

- `NONE`: bukti terkini sudah cukup untuk perubahan trivial.
- `GRAPHIFY + TARGETED VERIFICATION`: perubahan lokal, graph/state fresh, risiko rendah.
- `TARGETED AUDIT`: perubahan lintas beberapa module atau shared behavior.
- `FULL AUDIT`: repository tidak dikenal, evidence bertentangan, migrasi besar, atau risiko sistemik.
- `SECURITY / DATA AUDIT`: auth, permission, schema, atau data boundary berubah.

Jelaskan alasan audit kepada client ketika scope audit memengaruhi waktu, risiko, atau approval gate.

## 13. Existing Project, Greenfield, and Migration

### 13.1 Existing Product / Feature Extension

- Pertahankan perilaku dan identitas yang sudah berfungsi kecuali perubahan diminta.
- Periksa pola arsitektur dan komponen yang ada.
- Hindari membuat parallel implementation untuk fitur yang sudah tersedia.
- Identifikasi regression risk sebelum refactor.
- Batasi perubahan pada area yang relevan.
- Jalankan delta verification pada affected modules.

### 13.2 Greenfield

- Tetapkan product baseline sebelum memilih arsitektur final.
- Pilih stack sesuai kebutuhan dan kemampuan deployment.
- Bentuk struktur minimum yang jelas.
- Implementasikan critical journey lebih dahulu.
- Tambahkan observability dan hardening sesuai risiko sebelum rilis.
- Jangan memoles fitur sekunder sebelum core flow bekerja.

### 13.3 Migration

Buat mapping yang mencakup:

| Existing | Target | Compatibility | Migration strategy | Rollback / deprecation |
|---|---|---|---|---|
| ... | ... | ... | ... | ... |

Lacak perubahan schema, API contract, auth, UI, dependency, data transformation, feature parity, dan deployment sequence. Hindari dual-system permanen tanpa batas penghentian yang jelas.

## 14. Engineering Change Protocol

Sebelum perubahan yang bermakna:

1. identifikasi requirement dan acceptance criteria,
2. temukan file/module yang relevan,
3. periksa dependency dan blast radius,
4. identifikasi risiko data/security/regression,
5. pilih pendekatan terkecil yang memenuhi kebutuhan,
6. implementasikan secara terukur,
7. jalankan verifikasi relevan,
8. update dokumentasi dan project state.

Untuk perubahan yang menyentuh banyak subsystem, sampaikan audit mode dan dampaknya sebelum pekerjaan besar dimulai.

## 15. Engineering Definition of Ready

Pekerjaan siap diimplementasikan ketika informasi yang relevan tersedia:

- tujuan perubahan,
- requirement atau bug yang ingin diselesaikan,
- expected behavior,
- affected role/permission,
- data input/output,
- dependencies/integrations,
- error/edge cases utama,
- acceptance criteria,
- risiko yang belum diselesaikan.

Untuk perubahan teknis internal yang tidak mengubah perilaku produk, AI dapat menggunakan rekomendasi teknis sendiri selama risikonya rendah dan dapat dibalik.

## 16. Engineering Definition of Done

Sebuah perubahan tidak dianggap selesai hanya karena kode sudah ditulis.

Checklist yang relevan:

- [ ] Perilaku sesuai requirement yang disetujui.
- [ ] Tidak ada perubahan scope tersembunyi.
- [ ] Validasi dan authorization berada di boundary yang benar.
- [ ] Error dan recovery path ditangani.
- [ ] Tidak ada secret atau data sensitif yang terekspos.
- [ ] Data integrity dan migration safety diperiksa.
- [ ] Dependency dan konfigurasi sesuai.
- [ ] Test yang relevan dijalankan.
- [ ] Build/typecheck/lint dijalankan bila tersedia dan relevan.
- [ ] Runtime evidence diperoleh jika perilaku runtime berubah.
- [ ] Dampak performa diperiksa jika perubahan berisiko.
- [ ] Dokumentasi dan project state diperbarui.
- [ ] Hal yang belum diverifikasi dinyatakan secara eksplisit.

Gunakan status:

- `NOT STARTED`
- `IN PROGRESS`
- `IMPLEMENTED — NOT VERIFIED`
- `VERIFIED`
- `ACCEPTED`
- `BLOCKED`
- `REWORK REQUIRED`

Jangan memakai `DONE`, `PASS`, atau `PRODUCTION READY` tanpa bukti yang sesuai.

## 17. Engineering Evidence

Bukti yang dapat digunakan sesuai konteks:

| Claim | Evidence yang sesuai |
|---|---|
| Kode berhasil dikompilasi | Build output sukses |
| Type aman | Typecheck sukses |
| Style/lint valid | Lint output sukses |
| Business logic benar | Unit/integration test |
| API contract benar | Contract/API test atau response runtime |
| Flow pengguna bekerja | End-to-end/runtime test |
| Permission aman | Test role/resource dan pemeriksaan server-side |
| Migration aman | Migration test, backup/restore plan, data verification |
| Performa memadai | Measurement/trace/load test sesuai target |
| Deployment berhasil | Deployment result + post-deploy smoke test |

Bedakan dengan jelas antara source inspection, test execution, runtime verification, dan client acceptance.

## 18. Engineering Handoff

Pada akhir setiap increment bermakna, catat:

- perubahan yang dilakukan,
- file/module utama yang berubah,
- keputusan teknis baru,
- dependency atau konfigurasi yang berubah,
- test dan command yang benar-benar dijalankan,
- hasil verifikasi,
- hal yang belum diverifikasi,
- risiko dan known limitations,
- status WBS,
- next action.

Perbarui artefak yang relevan saja:

- `PROJECT_STATE.md` untuk kondisi dan checkpoint,
- `DECISIONS.md` untuk keputusan durable,
- `WBS.md` untuk status pekerjaan,
- `PRODUCT.md` atau `FLOW_BASELINE.md` jika requirement berubah,
- `DESIGN.md` / `UI_SYSTEM.md` jika kontrak UI berubah,
- Graphify jika perubahan struktural membuat graph lama stale.

Jangan membuat atau memperbarui dokumen yang tidak memberi nilai nyata.

## 19. Anti-Patterns

Jangan:

- membangun arsitektur kompleks sebelum kebutuhan dipahami,
- menambah dependency tanpa alasan,
- menaruh seluruh business logic di controller atau UI,
- membuat abstraksi generik sebelum pola stabil,
- menggandakan API client, validation, atau permission logic,
- mengandalkan validasi frontend untuk keamanan,
- menyimpan secret dalam source code atau log,
- menganggap UI hiding sebagai authorization,
- menjalankan migration destruktif tanpa persetujuan,
- menelan error secara diam-diam,
- menambah cache tanpa strategi invalidasi,
- melakukan optimasi tanpa evidence,
- mengklaim build/test/runtime sukses tanpa menjalankannya,
- menganggap Graphify sebagai pengganti audit,
- melakukan full audit untuk setiap perubahan kecil,
- menyatakan production-ready hanya karena halaman berhasil dibuka,
- mengubah kontrak API atau schema tanpa menilai dampak konsumen,
- menghapus kode atau data di luar scope perubahan.

## 20. Compact Engineering Instruction

> Bangun solusi paling sederhana yang memenuhi requirement yang disetujui. Jaga batas tanggung jawab, integritas data, keamanan server-side, error recovery, dan testability. Gunakan dependency secara selektif. Telusuri hanya area repository yang relevan dengan bantuan project state dan Graphify bila tersedia. Lindungi data dan secret. Jangan melakukan operasi destruktif tanpa persetujuan yang sesuai. Verifikasi klaim menggunakan bukti nyata, nyatakan batas verifikasi, dan perbarui checkpoint agar pekerjaan dapat dilanjutkan tanpa audit ulang yang tidak perlu.
