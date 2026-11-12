---
name: project-management
version: "1.0.0"
language: "id-ID"
purpose: "Pengelolaan scope, WBS, prioritas, approval, perubahan requirement, risiko, checkpoint, dan handoff proyek."
---

# 05 — Project Management

Dokumen ini mengatur bagaimana project direncanakan, dibagi, diprioritaskan, disetujui, dilacak, dan diserahkan. Gunakan proses secukupnya: project kecil membutuhkan kontrol sederhana; project besar atau berisiko membutuhkan artefak dan gate yang lebih formal.

## 1. Project Management Principles

1. **Outcome over activity:** kelola deliverable yang dapat diperiksa, bukan daftar aktivitas yang tidak memiliki hasil.
2. **Client-led product decisions:** AI memberi rekomendasi teknis dan product insight; keputusan produk material tetap berada pada client.
3. **Visible state:** status, dependency, risiko, dan keputusan penting harus dapat ditemukan.
4. **Small meaningful phases:** gunakan beberapa fase utama dengan gate yang jelas, bukan puluhan fase buatan.
5. **Incremental delivery:** prioritaskan bagian produk yang memberi nilai dan mengurangi risiko lebih awal.
6. **Evidence-based acceptance:** pekerjaan dinyatakan verified atau accepted berdasarkan bukti yang relevan.
7. **Controlled change:** perubahan requirement dianalisis dampaknya sebelum baseline diperbarui.
8. **Continuity:** checkpoint harus cukup untuk melanjutkan pekerjaan tanpa mengulang discovery dan audit yang sudah valid.

## 2. Roles and Responsibilities

### 2.1 Client / Product Owner

Client bertanggung jawab untuk:

- menjelaskan tujuan, proses operasional, dan kebutuhan pengguna,
- memberikan informasi bisnis yang hanya diketahui pihak client,
- meninjau Approval Package,
- menyetujui keputusan produk, scope, dan perubahan material,
- menyediakan akses atau keputusan eksternal yang memang menjadi kewenangannya,
- menerima atau meminta revisi atas deliverable.

Client tidak diwajibkan memilih framework, database, atau detail teknis internal kecuali memang ingin terlibat.

### 2.2 AI / Lead Developer

AI bertanggung jawab untuk:

- menerjemahkan kebutuhan menjadi requirement dan deliverable,
- mengajukan pertanyaan yang diperlukan pada gate aktif,
- memberi rekomendasi dan menjelaskan trade-off penting,
- memilih detail teknis rutin secara mandiri,
- menyusun WBS, dependency, acceptance criteria, dan risk register,
- mengimplementasikan atau mengarahkan pekerjaan sesuai akses yang tersedia,
- memverifikasi hasil dan melaporkan keterbatasan,
- menjaga dokumentasi dan project state tetap terkini.

AI tidak boleh menganggap rekomendasi sendiri sebagai requirement client yang telah disetujui.

### 2.3 External Contributor

Bila ada designer, developer lain, vendor, atau administrator eksternal, catat kontribusinya hanya jika memengaruhi dependency, akses, biaya, jadwal, atau acceptance. Jangan mengasumsikan pihak eksternal telah menyetujui pekerjaan tanpa bukti.

## 3. Project Mode and Management Approach

Klasifikasikan project sebelum memilih tingkat proses:

| Mode | Fokus pengelolaan |
|---|---|
| `GREENFIELD` | Discovery, baseline produk, arsitektur awal, critical journey |
| `EXISTING_PRODUCT` | Kondisi existing, delta scope, risiko regresi |
| `FEATURE_EXTENSION` | Deliverable fitur dan dependency pada sistem aktif |
| `REDESIGN` | Paritas fungsi, perubahan UX/UI, migrasi bertahap |
| `MIGRATION` | Mapping, compatibility, data migration, rollback |
| `AUDIT_ONLY` | Temuan, evidence, severity, rekomendasi, acceptance audit |
| `PROTOTYPE` | Validasi asumsi dengan scope kecil dan timebox |
| `MAINTENANCE` | Defect, patch, dependency, reliability, operational health |

Jangan menerapkan seluruh lifecycle greenfield pada feature extension kecil. Sesuaikan deliverable dengan mode dan risiko.

## 4. Project Lifecycle and Stage Gates

Gunakan enam fase utama sebagai baseline. Fase dapat digabung untuk project kecil atau dipecah bila kompleksitas membutuhkannya.

### Phase 1 — Discovery and Product Baseline

Deliverables yang relevan:

- tujuan produk dan masalah yang diselesaikan,
- aktor dan role,
- primary journey dan exception flow,
- screen/route map,
- core data dan business rules,
- integration requirements,
- project mode,
- scope in/out,
- constraints dan risks,
- initial acceptance criteria,
- Flow Baseline,
- Approval Package.

**Gate 1:** tujuan dan alur inti dapat dipahami; ambiguity material telah ditangani; baseline relevan mendapat approval client.

### Phase 2 — UX, Design System, and Delivery Plan

Deliverables yang relevan:

- information architecture,
- UX direction,
- primary UI strategy,
- tokens dan visual direction,
- UI Component System,
- component workbench/preview untuk project non-trivial,
- representative reference surface,
- WBS dan dependency,
- milestone/acceptance gates,
- technical approach dan risk review.

**Gate 2:** arah produk/UI dan rencana deliverable cukup jelas untuk memulai implementasi yang disetujui.

### Phase 3 — Core Implementation

Deliverables yang relevan:

- app shell,
- core routes/screens,
- primary user journey,
- data interactions dan business logic,
- auth/role enforcement,
- integration contracts,
- loading/error/empty/recovery states,
- implementation evidence.

**Gate 3:** primary journey berjalan end-to-end pada environment target yang sesuai, dengan issue penting tercatat.

### Phase 4 — Expansion and Integration

Deliverables yang relevan:

- secondary features,
- edge cases,
- operational/admin surfaces,
- integrations,
- localization bila diperlukan,
- telemetry/analytics bila disetujui,
- feature-level verification.

**Gate 4:** scope yang disetujui telah diimplementasikan atau pengecualiannya disepakati.

### Phase 5 — Quality and Acceptance

Deliverables yang relevan:

- functional/regression QA,
- responsive and browser QA,
- accessibility review,
- security/data review sesuai risiko,
- performance review sesuai target,
- visual/UX review,
- defect list dan residual risks,
- QA report.

**Gate 5:** critical defects telah ditangani; sisa defect dan limitation ditampilkan untuk keputusan acceptance.

### Phase 6 — Release and Handoff

Deliverables yang relevan:

- release/build evidence,
- deployment configuration,
- migration and rollback/forward-fix plan,
- operational documentation,
- post-release smoke test,
- final project state,
- known limitations,
- handoff summary.

**Gate 6:** release evidence tersedia dan pihak penerima dapat memahami cara menjalankan serta melanjutkan project.

### Gate Rules

- Jangan menyatakan fase selesai hanya karena aktivitas fase telah dilakukan.
- Gate membutuhkan hasil dan evidence yang telah ditentukan.
- Gate yang tidak relevan boleh ditandai `NOT APPLICABLE` dengan alasan.
- Approval sebuah gate hanya berlaku pada scope yang tertulis di Approval Package.
- Jika evidence tidak cukup, pertahankan status `IN PROGRESS`, `BLOCKED`, atau `IMPLEMENTED — NOT VERIFIED`.

## 5. Scope Management

### 5.1 Define Scope

Tulis batas scope secara eksplisit.

```markdown
## Scope Baseline

### In Scope
- ...

### Out of Scope
- ...

### Assumptions
- ...

### Constraints
- ...

### Dependencies
- ...

### Acceptance Outcome
- ...
```

Scope harus menggambarkan outcome dan batas pekerjaan, bukan hanya daftar halaman.

### 5.2 Scope Boundary

Sebuah item termasuk scope bila:

- client menyatakannya atau telah menyetujuinya,
- dibutuhkan untuk acceptance criteria yang disetujui,
- merupakan dependency teknis yang tidak dapat dihindari untuk deliverable tersebut dan tidak mengubah keputusan material.

Item baru tidak otomatis masuk scope hanya karena ditemukan selama implementasi. Catat sebagai candidate change dan analisis dampaknya.

### 5.3 Scope Creep

Tanda scope creep:

- fitur baru dimasukkan tanpa perubahan baseline,
- acceptance criteria bertambah tanpa review,
- halaman sekunder ditambahkan karena dianggap "sekalian",
- redesign meluas ke area yang tidak diminta,
- integrasi eksternal baru diasumsikan wajib,
- pekerjaan operasional muncul tanpa perencanaan.

Respons:

1. catat permintaan atau temuan baru,
2. jelaskan alasan dan nilai yang diharapkan,
3. analisis dampak scope, data, UI, dependency, risiko, dan jadwal,
4. rekomendasikan masuk sekarang, dijadwalkan berikutnya, atau ditolak/defer,
5. minta approval bila baseline berubah,
6. perbarui WBS dan project state setelah keputusan.

Jangan diam-diam menambah pekerjaan di luar baseline.

## 6. Work Breakdown Structure (WBS)

### 6.1 Purpose

WBS memecah scope menjadi deliverable yang dapat dikerjakan dan diverifikasi. WBS bukan daftar semua klik atau langkah coding.

Setiap level harus membantu menjawab:

- apa hasilnya,
- siapa yang mengerjakan,
- apa dependency-nya,
- bagaimana hasil diterima,
- apa risiko utamanya.

### 6.2 Decomposition Rules

- Mulai dari deliverable atau fase.
- Pecah hanya sampai unit yang dapat diperkirakan, ditugaskan, dan diverifikasi.
- Hindari task yang terlalu besar sehingga statusnya tidak informatif.
- Hindari task mikro yang biaya pengelolaannya lebih besar daripada manfaatnya.
- Pisahkan milestone/gate dari task implementasi.
- Identifikasi dependency yang benar-benar memblokir pekerjaan.
- Gabungkan task serupa bila acceptance dan ownership-nya sama.

Untuk project sederhana, WBS satu level dapat cukup. Untuk project besar, gunakan ID bertingkat seperti `1.0`, `1.1`, `1.1.1`.

### 6.3 WBS Item Schema

| Field | Isi |
|---|---|
| WBS ID | ID stabil |
| Phase | Fase utama |
| Deliverable / Task | Hasil spesifik |
| Priority | Critical / Important / Optional |
| Owner | Client / AI / Developer / Designer / External |
| Dependency | Predecessor atau dependency |
| Status | Status terstandardisasi |
| Acceptance Criteria | Kondisi yang dapat diamati |
| Evidence | Test, file, screenshot, URL, runtime result |
| Risk | Risiko utama |
| Estimate | Estimasi beserta unit dan asumsi, jika diperlukan |
| Notes | Keputusan atau batasan |

### 6.4 Priority

Gunakan:

- `CRITICAL`: diperlukan agar primary journey, security, data integrity, atau acceptance inti terpenuhi.
- `IMPORTANT`: memberi nilai besar atau menutup risiko penting, tetapi bukan blocker utama.
- `OPTIONAL`: enhancement yang dapat ditunda tanpa melanggar baseline.

Jangan memberi semua item prioritas Critical. Prioritas harus membantu memilih urutan.

### 6.5 WBS Status

Gunakan status yang konsisten:

- `NOT STARTED`
- `IN PROGRESS`
- `IMPLEMENTED — NOT VERIFIED`
- `VERIFIED`
- `ACCEPTED`
- `BLOCKED`
- `DEFERRED`
- `CANCELLED`
- `REWORK REQUIRED`

Perbedaan penting:

- `IMPLEMENTED — NOT VERIFIED`: perubahan sudah dibuat, bukti belum cukup.
- `VERIFIED`: pemeriksaan teknis yang relevan berhasil.
- `ACCEPTED`: client menerima hasil sesuai scope.
- `BLOCKED`: ada dependency atau kendala yang mencegah progress.
- `DEFERRED`: item sengaja dipindahkan ke tahap berikutnya.
- `CANCELLED`: item tidak lagi menjadi bagian pekerjaan.

### 6.6 Acceptance Criteria

Acceptance criteria harus observabel dan dapat diuji.

Lemah:
- "Dashboard bagus."
- "Form bekerja."
- "Website cepat."

Lebih baik:
- "User dengan role petugas dapat melihat daftar permohonan miliknya."
- "Field wajib menolak submit ketika kosong dan menampilkan pesan yang terkait dengan field."
- "Setelah submit berhasil, record tersimpan dan muncul pada detail permohonan."
- "Halaman utama tidak mengalami horizontal overflow pada viewport target yang disepakati."

Hindari angka threshold palsu. Jika target performa, kapasitas, atau waktu belum disepakati, usulkan target beserta alasannya untuk dikonfirmasi.

### 6.7 WBS Dependency

Bedakan:

- **Finish-to-start:** task B dimulai setelah task A selesai.
- **Parallelizable:** task dapat berjalan bersamaan tanpa konflik resource atau keputusan.
- **External dependency:** menunggu akses, data, keputusan, vendor, atau pihak lain.
- **Approval dependency:** menunggu client approve sebelum tahap berikutnya.

Jangan membuat dependency hanya untuk menghasilkan diagram yang terlihat rumit. Catat blocker aktual.

### 6.8 WBS Baseline Template

```markdown
# WBS

## Project
- Name:
- Mode:
- Baseline version:
- Last updated:

| ID | Phase | Deliverable / Task | Priority | Owner | Dependency | Status | Acceptance Criteria | Evidence | Risk |
|---|---|---|---|---|---|---|---|---|---|
| 1.0 | Discovery | Product baseline | CRITICAL | AI + Client | - | NOT STARTED | Baseline approved | Approval Package | Ambiguity |
| 2.0 | UX/UI | UI system | CRITICAL | AI | 1.0 | NOT STARTED | UI system verified | Workbench + reference surface | UI drift |
| 3.0 | Build | Core journey | CRITICAL | AI | 2.0 | NOT STARTED | End-to-end flow works | Runtime/test | Integration |
| 4.0 | QA | Quality review | CRITICAL | AI | 3.0 | NOT STARTED | Critical defects resolved | QA report | Regression |
| 5.0 | Release | Handoff | IMPORTANT | AI + Client | 4.0 | NOT STARTED | Release evidence and handoff | Release record | Deployment |
```

Ganti contoh dengan deliverable project aktual. Jangan menyalin task contoh sebagai requirement.

## 7. Prioritization

### 7.1 Priority Order

Secara umum prioritaskan:

1. security, data integrity, dan risiko kehilangan data,
2. blocker yang menghalangi primary journey,
3. requirement wajib untuk acceptance,
4. dependency yang membuka pekerjaan lain,
5. high-value user outcome,
6. reliability dan operability yang diperlukan untuk target release,
7. secondary features,
8. polish dan enhancement.

Urutan dapat berubah berdasarkan risiko dan kebutuhan produk yang disetujui.

### 7.2 Value, Risk, and Effort

Gunakan penilaian kualitatif bila angka belum memiliki dasar:

- user/business value: high / medium / low,
- risk reduction: high / medium / low,
- effort: small / medium / large,
- urgency/dependency: blocking / soon / later.

Jangan membuat skor numerik yang seolah objektif tanpa metode dan data. Untuk keputusan yang sulit, tunjukkan alasan dan trade-off secara eksplisit.

### 7.3 Critical Path

Identifikasi dependency yang menentukan kapan milestone dapat tercapai. Bila ada blocker:

- namai dependency,
- owner yang dapat menyelesaikannya,
- dampak jika terlambat,
- opsi mitigasi,
- keputusan yang dibutuhkan dari client.

Jangan memberi estimasi tanggal pasti jika kapasitas, akses, atau dependency belum diketahui.

## 8. Estimation and Planning

### 8.1 Estimate Responsibly

Estimasi harus memuat:

- unit (jam, hari kerja, atau range),
- asumsi kapasitas dan availability,
- dependency,
- uncertainty,
- apakah termasuk QA/review/deployment.

Gunakan range ketika requirement belum stabil. Pisahkan effort implementasi dari waktu menunggu approval/vendor. Jangan menyajikan perkiraan sebagai janji.

### 8.2 Estimation Confidence

Contoh label:

- `LOW CONFIDENCE`: discovery atau dependency utama belum jelas.
- `MEDIUM CONFIDENCE`: scope utama jelas, beberapa risiko tersisa.
- `HIGH CONFIDENCE`: pekerjaan serupa dipahami dan dependency telah diverifikasi.

Ubah estimasi bila ada evidence baru dan jelaskan penyebab perubahan. Jangan menyembunyikan peningkatan scope di dalam estimasi lama.

### 8.3 Timebox Prototypes

Untuk prototype atau investigasi, tentukan:

- pertanyaan yang ingin dijawab,
- output yang diharapkan,
- batas waktu/effort,
- kondisi berhenti,
- keputusan setelah hasil diperoleh.

Prototype bukan persetujuan otomatis untuk menjadikan solusi sementara sebagai production architecture.

## 9. Approval and Client Collaboration

### 9.1 Approval Package

Pada setiap gate penting, konsolidasikan keputusan dalam satu paket.

```markdown
## Approval Package — <name>

### Baseline yang dikonfirmasi
- ...

### Rekomendasi developer
- ...

### Perubahan sejak approval sebelumnya
- Tidak ada / ...

### Pertanyaan terbuka
- Tidak ada / ...

### Scope yang akan dikerjakan setelah approval
- ...

### Acceptance criteria
- ...

### Risiko / trade-off
- ...

**Status: WAITING FOR CLIENT APPROVAL**
```

Jangan meminta approval untuk item yang masih memiliki ambiguity material. Selesaikan pertanyaan yang memengaruhi gate terlebih dahulu.

### 9.2 Command Semantics

Terapkan perintah client secara konsisten:

**`Approve`**

- menyetujui Approval Package terbaru yang jelas,
- mengubah baseline yang tercakup menjadi `LOCKED`,
- mencatat keputusan durable,
- mengizinkan langkah berikutnya yang tertulis dalam paket.

Bukan izin tak terbatas untuk mengubah atau menyelesaikan seluruh project.

**`Oke Lanjutkan` / `Lanjutkan`**

- melanjutkan langkah berikutnya dari baseline approved/locked,
- dapat dianggap menyetujui Approval Package terakhir jika paket tersebut jelas dan sedang menunggu approval,
- tidak menyelesaikan ambiguity lain yang tidak terkait.

**`Sebelum lanjut...`**

- hentikan rencana yang akan dijalankan,
- integrasikan informasi atau koreksi baru,
- analisis perbedaan terhadap baseline,
- jelaskan dampak ke flow, data, UI, WBS, dan implementasi,
- minta konfirmasi baru hanya bila diperlukan,
- keluarkan Approval Package revisi jika keputusan terkunci berubah.

**Pertanyaan di tengah pekerjaan**

- jawab pertanyaan tanpa kehilangan project state,
- pertanyaan saja tidak mengubah scope atau memberikan approval.

### 9.3 Approval Record

Untuk keputusan penting, simpan:

- package name/version,
- tanggal,
- keputusan,
- scope yang disetujui,
- perubahan yang tercakup,
- siapa yang menyetujui,
- batas approval,
- dependency/next gate.

Jangan merekam dugaan sebagai approval.

## 10. Change Management

### 10.1 Change Request

Perubahan setelah baseline dikunci harus dicatat sebagai Change Request (CR) bila berdampak pada scope, flow, data, security, UI contract, dependency, biaya, atau jadwal.

Template:

```markdown
# CR-XXX — <Change title>

- Requested by:
- Date:
- Current baseline:
- Status: PROPOSED | IMPACT REVIEW | WAITING APPROVAL | APPROVED | REJECTED | DEFERRED

## Requested Change
- ...

## Reason / Expected Value
- ...

## Impact Analysis
- Product flow:
- Screens / UI:
- Data / schema:
- Roles / permissions:
- API / integrations:
- Architecture / dependencies:
- Security / privacy:
- QA / regression:
- Scope / WBS:
- Schedule / cost assumptions:

## Options
- Implement now:
- Defer:
- Alternative:

## Developer Recommendation
- ...

## Decision
- ...

## Approval Evidence
- ...
```

### 10.2 Change Impact Rules

Setiap CR harus dianalisis berdasarkan area yang relevan. Jangan mengisi semua area dengan asumsi; tulis `NO KNOWN IMPACT` hanya setelah pemeriksaan yang cukup atau `NOT ASSESSED` jika belum diperiksa.

### 10.3 Re-baseline

Setelah change disetujui:

1. update Flow Baseline / requirement,
2. update scope dan WBS,
3. update acceptance criteria,
4. update risks dan dependencies,
5. update DECISIONS bila keputusan durable berubah,
6. update timeline/estimate assumptions,
7. catat baseline version baru.

Simpan riwayat perubahan. Jangan menghapus baseline lama seolah perubahan tidak pernah terjadi.

## 11. Risk Management

### 11.1 Risk Register

Catat risiko yang dapat mengubah outcome, biaya, jadwal, keamanan, data, atau acceptance.

| Field | Deskripsi |
|---|---|
| Risk ID | ID stabil |
| Risk | Peristiwa/ketidakpastian |
| Cause | Penyebab |
| Impact | Dampak jika terjadi |
| Likelihood | Low / Medium / High |
| Severity | Low / Medium / High / Critical |
| Owner | Pihak yang mengelola |
| Mitigation | Pencegahan/pengurangan |
| Contingency | Respons jika terjadi |
| Trigger | Indikator risiko mulai terjadi |
| Status | OPEN / MITIGATING / OCCURRED / CLOSED |

### 11.2 Risk Categories

Pertimbangkan:

- product/requirement ambiguity,
- delivery and capacity,
- third-party dependency,
- security/privacy,
- data loss/integrity,
- architecture/dependency,
- performance/scalability,
- accessibility/usability,
- integration,
- deployment/operations,
- vendor or platform availability,
- client decision/access delays.

### 11.3 Risk Response

Pilih respons secara sadar:

- **Avoid:** ubah pendekatan untuk menghilangkan risiko.
- **Mitigate:** kurangi kemungkinan atau dampak.
- **Transfer:** alihkan sebagian risiko melalui vendor/kontrak/layanan, tanpa menganggap tanggung jawab sepenuhnya hilang.
- **Accept:** terima risiko yang diketahui dengan owner dan contingency yang jelas.

Risiko Critical/High harus disampaikan kepada client bila memengaruhi approval, data, keamanan, release, atau keputusan produk.

## 12. Issue and Dependency Management

Issue adalah masalah yang sudah terjadi; risk adalah kemungkinan masalah di masa depan.

Untuk issue/blocker catat:

- ID,
- deskripsi,
- dampak,
- waktu diketahui,
- owner,
- dependency,
- pilihan mitigasi,
- keputusan yang dibutuhkan,
- status dan next action.

Jangan membiarkan blocker hanya muncul sebagai catatan percakapan yang mudah hilang. Masukkan ke `PROJECT_STATE.md` atau issue tracker yang memang digunakan project.

## 13. Communication and Reporting

### 13.1 Communication Rules

- Gunakan bahasa yang dimengerti client.
- Tampilkan keputusan yang perlu dibuat, bukan seluruh detail internal.
- Sampaikan risiko sedini mungkin.
- Pisahkan fakta, asumsi, rekomendasi, dan keputusan.
- Jangan membuat laporan progress yang menyatakan aktivitas tanpa outcome.
- Jangan membanjiri client dengan istilah teknis atau log mentah tanpa konteks.
- Jangan mengulang pertanyaan yang telah dijawab dan masih valid.

### 13.2 Status Report

Gunakan format pendek:

```markdown
## Project Status — <date/checkpoint>

**Overall:** ON TRACK / AT RISK / BLOCKED

### Completed / Verified
- ...

### In Progress
- ...

### Next
- ...

### Decisions Needed
- ...

### Risks / Blockers
- ...

### Evidence
- ...

### Baseline / Approval
- ...
```

Label `ON TRACK` hanya boleh digunakan bila milestone, dependency, dan risiko mendukungnya. Jika jadwal belum ditentukan, gunakan status progress tanpa mengarang kondisi jadwal.

### 13.3 Decision Request

Saat membutuhkan keputusan client:

1. jelaskan bagian yang perlu diputuskan,
2. rangkum fakta yang sudah diketahui,
3. berikan rekomendasi,
4. jelaskan dampak utama,
5. ajukan pertanyaan langsung yang paling penting.

Jangan meminta client memilih detail implementasi yang tidak berdampak pada outcome.

## 14. Project Artifacts and Source of Truth

Gunakan artefak sesuai fungsinya:

| Artifact | Fungsi |
|---|---|
| `PRODUCT.md` | Tujuan, pengguna, product truth |
| `FLOW_BASELINE.md` | Journey, role, screen map, data/business rules |
| `DESIGN.md` | Visual language dan design decisions |
| `UI_SYSTEM.md` | Component contract, variants, states, workbench |
| `DECISIONS.md` | Keputusan durable beserta rationale |
| `PROJECT_STATE.md` | Checkpoint kondisi terkini dan next actions |
| `WBS.md` | Deliverables, dependency, status, acceptance |
| `RISK_REGISTER.md` | Risiko dan mitigasi bila project membutuhkannya |
| `QA_REPORT.md` | Hasil QA dan evidence |
| `CHANGELOG.md` | Perubahan versi/release bila relevan |

Tidak semua project membutuhkan semua file. Untuk project kecil, beberapa artefak dapat digabung jika tetap jelas. Hindari duplikasi panjang dan konflik antar dokumen.

### Source-of-Truth Rules

- Product requirement berada pada baseline produk yang disetujui.
- Keputusan durable berada di decision log.
- Status pekerjaan berada di WBS dan project state.
- Hasil verifikasi berada di QA/evidence record.
- Kode dan runtime aktual tetap menjadi bukti keadaan implementasi.
- Jika dokumen bertentangan dengan kode atau approval terbaru, tandai konflik dan selesaikan sebelum mengambil keputusan berisiko.

## 15. Project Checkpoint and Continuity

Checkpoint harus memungkinkan AI/developer berikutnya melanjutkan tanpa melakukan discovery ulang.

### 15.1 Checkpoint Contents

Simpan secara ringkas:

- project goal dan mode,
- current phase/gate,
- latest approved baseline,
- current WBS status,
- in-progress items,
- last verified evidence,
- locked decisions,
- open decisions,
- risks/blockers,
- relevant architecture,
- environment notes yang aman,
- next actions.

Jangan simpan credential, token, cookie, atau rahasia dalam checkpoint.

### 15.2 Checkpoint Frequency

Buat atau update checkpoint:

- setelah approval penting,
- setelah milestone,
- setelah perubahan arsitektur/schema,
- setelah selesai increment bermakna,
- sebelum context/session berakhir,
- setelah muncul blocker atau risk baru.

Tidak perlu memperbarui dokumen pada setiap perubahan mikro yang tidak mengubah state project.

### 15.3 Handoff to New Chat/Developer

Urutan pemulihan:

1. baca `PROJECT_STATE.md`,
2. baca baseline/approval terakhir,
3. baca WBS dan open items,
4. baca DECISIONS yang relevan,
5. gunakan Graphify/evidence jika tersedia,
6. verifikasi freshness terhadap repository/runtime,
7. lanjutkan dari next action.

Jangan meminta client mengulang informasi yang masih jelas, terkini, dan memiliki sumber. Jika terdapat konflik, ajukan hanya klarifikasi yang diperlukan.

## 16. Meeting / Review / Approval Gates

Review dilakukan saat ada keputusan atau hasil yang perlu diterima, bukan sekadar mengikuti kalender.

Contoh checkpoint review:

- product baseline approval,
- UX/UI system approval,
- representative surface review,
- core journey demonstration,
- integration readiness,
- QA/release readiness,
- final handoff acceptance.

Setiap review mencantumkan:

- tujuan,
- scope yang ditinjau,
- bukti/deliverable,
- keputusan yang dibutuhkan,
- hasil review,
- action items dan owner.

## 17. Release and Handoff Management

Sebelum handoff, siapkan sesuai scope:

- ringkasan tujuan dan deliverable,
- fitur yang diimplementasikan,
- versi/commit atau release identifier,
- cara menjalankan project,
- konfigurasi environment tanpa secret,
- migration/deployment notes,
- QA evidence,
- known limitations,
- open defects,
- backup/rollback notes,
- next maintenance actions.

Pisahkan:

- **technical handoff:** informasi agar developer/operator dapat menjalankan dan memelihara sistem,
- **client handoff:** apa yang telah diterima, cara menggunakan, batasan, dan keputusan yang masih terbuka.

Jangan menyebut handoff complete bila akses, dokumen, atau artifact yang disepakati belum tersedia.

## 18. Project Management Templates

### 18.1 Project Brief

```markdown
# PROJECT_BRIEF

## Product
- Name:
- Goal:
- Problem:
- Users:
- Primary outcome:

## Project Mode
- GREENFIELD / EXISTING_PRODUCT / FEATURE_EXTENSION / REDESIGN / MIGRATION / AUDIT_ONLY / PROTOTYPE / MAINTENANCE

## Scope
- In scope:
- Out of scope:

## Constraints
- Technical:
- Operational:
- Budget:
- Schedule:
- Compliance:

## Dependencies
- ...

## Success Criteria
- ...

## Approval State
- ...
```

### 18.2 Milestone Review

```markdown
# MILESTONE REVIEW

- Milestone:
- Date:
- Baseline:
- Owner:

## Expected Deliverables
- ...

## Evidence
- ...

## Acceptance Criteria
- ...

## Outcome
- VERIFIED / ACCEPTED / REWORK REQUIRED / BLOCKED

## Open Items
- ...

## Next Gate
- ...
```

### 18.3 Action Item

```markdown
# ACTION-XXX

- Description:
- Owner:
- Priority:
- Due date / trigger:
- Dependency:
- Status:
- Completion evidence:
```

Jangan mengarang due date jika tidak diberikan atau disepakati.

## 19. Anti-Patterns

Jangan:

- membuat WBS berdasarkan jumlah halaman saja,
- membuat puluhan task mikro tanpa nilai pelacakan,
- memberi status complete tanpa acceptance criteria dan evidence,
- menganggap semua kebutuhan yang muncul otomatis masuk scope,
- mengubah baseline tanpa change impact analysis,
- menyembunyikan risiko atau keterlambatan,
- memberi estimasi pasti dengan dependency yang belum diketahui,
- meminta client memilih library untuk keputusan internal,
- meminta approval atas paket dengan ambiguity material yang belum selesai,
- menganggap `Lanjutkan` sebagai izin tak terbatas,
- mengulang discovery/audit yang bukti validnya sudah tersedia,
- menyimpan seluruh detail project ke satu file raksasa,
- menduplikasi keputusan di banyak dokumen tanpa satu sumber utama,
- menyimpan secret di project state,
- menyamakan implemented, verified, accepted, dan released.

## 20. Compact Project Management Instruction

> Kelola project berdasarkan deliverable, dependency, risiko, dan acceptance criteria. Pertahankan baseline yang telah disetujui; setiap perubahan material harus melalui impact analysis dan approval. Gunakan WBS yang cukup rinci untuk mengarahkan pekerjaan tetapi tidak membebani. Tampilkan progress berdasarkan hasil dan evidence, bukan sekadar aktivitas. Simpan keputusan, blocker, dan checkpoint agar pekerjaan dapat diteruskan tanpa mengulang konteks. Hormati batas approval client dan jangan menyembunyikan risiko.
