---
name: qa-testing
version: "1.0.0"
language: "id-ID"
purpose: "Strategi pengujian, verifikasi, audit UX, aksesibilitas, visual, integrasi, regresi, dan release acceptance."
source: "UNIVERSAL_AI_WEB_APP_DEVELOPMENT_BOOTSTRAP v1.3.0"
---

# 04 — QA & Testing

Dokumen ini mengatur cara membuktikan bahwa implementasi bekerja sesuai requirement. QA bukan sekadar menjalankan satu command lalu menyebut project selesai. Pilih pemeriksaan berdasarkan risiko dan perubahan aktual, lalu laporkan bukti serta batas verifikasinya.

## 1. QA Principles

1. **Requirement-driven:** setiap test harus terkait requirement, risiko, atau acceptance criteria.
2. **Risk-based:** prioritaskan alur kritis, data, permission, pembayaran, dan operasi yang sulit dipulihkan.
3. **Evidence-based:** klaim hanya boleh dibuat berdasarkan pemeriksaan yang benar-benar dilakukan.
4. **Layered:** gunakan kombinasi unit, integration, API, browser, accessibility, visual, dan operational checks sesuai kebutuhan.
5. **Repeatable:** test harus dapat dijalankan kembali dengan environment dan data yang diketahui.
6. **Targeted:** uji area terdampak dan dependency-nya; perluas cakupan bila blast radius atau bukti mengharuskannya.
7. **Honest reporting:** bedakan test yang tidak tersedia, tidak dijalankan, gagal, dan berhasil.
8. **No destructive surprise:** test tidak boleh merusak data atau environment tanpa persetujuan dan isolasi yang tepat.

## 2. QA Scope Selection

Tentukan cakupan sebelum menjalankan test.

| Kondisi perubahan | Cakupan minimum yang disarankan |
|---|---|
| Copy, label, atau styling lokal | Targeted visual + responsive check |
| Komponen shared | Component states + seluruh consumer penting |
| Form atau business logic | Validation + unit/integration + user flow |
| API contract | API/contract + consumer regression |
| Auth/role/permission | Positive dan negative authorization tests |
| Schema/migration | Migration + data integrity + rollback/restore plan |
| Integrasi eksternal | Contract/sandbox + timeout/retry/error path |
| Routing/navigation | Route coverage + browser smoke test |
| Release candidate | Critical journey + build + security/accessibility checks relevan |
| Repository tidak dikenal atau bukti bertentangan | Baseline audit yang lebih luas |

Gunakan audit scope yang paling kecil tetapi cukup:

- `NONE`: perubahan trivial dan bukti terkini sudah memadai.
- `TARGETED VERIFICATION`: test pada file, komponen, atau flow yang berubah.
- `TARGETED AUDIT`: beberapa module atau shared behavior terdampak.
- `FULL AUDIT`: kondisi sistem tidak dikenal, perubahan sistemik, atau evidence tidak dapat dipercaya.
- `SPECIALIZED AUDIT`: security, data migration, accessibility, performance, atau visual review sesuai risiko.

Catat alasan jika memilih full audit. Jangan menjadikan full audit sebagai ritual untuk setiap perubahan kecil.

## 3. Test Strategy

### 3.1 Unit Tests

Gunakan untuk fungsi dan aturan yang dapat diuji secara terisolasi:

- business rules,
- calculations,
- validation,
- state transitions,
- formatting/parsing,
- permission decision logic,
- utility functions.

Unit test sebaiknya cepat, deterministik, dan tidak bergantung pada jaringan, database produksi, atau waktu nyata. Mock boundary eksternal, bukan seluruh sistem sampai test hanya menguji mock.

### 3.2 Integration Tests

Gunakan untuk memeriksa kerja sama beberapa komponen:

- service dengan database,
- controller/handler dengan validation,
- repository dengan query,
- queue/job,
- file storage,
- auth middleware,
- API client dengan adapter.

Gunakan database atau service test yang terisolasi. Pastikan setup dan cleanup aman serta tidak menyentuh data pengguna.

### 3.3 API and Contract Tests

Periksa:

- method, route, status code,
- request validation,
- response schema dan tipe,
- authentication/authorization,
- pagination/filter/sort,
- error format,
- idempotency,
- backward compatibility untuk consumer yang ada.

Untuk API eksternal, gunakan sandbox atau fixture yang terdokumentasi. Jangan mengirim transaksi nyata hanya untuk menguji endpoint.

### 3.4 End-to-End Tests

Gunakan untuk alur pengguna yang paling penting dari awal hingga hasil akhir.

Contoh alur:

- sign up / login / logout,
- create, view, update, delete sesuai izin,
- pencarian dan filter,
- submit form beserta error recovery,
- checkout/pembayaran melalui sandbox,
- approval workflow,
- upload/download,
- admin operation yang kritis.

E2E harus memeriksa outcome yang terlihat pengguna, bukan hanya apakah tombol dapat diklik. Jaga jumlah E2E tetap proporsional karena test browser cenderung lebih lambat dan rapuh.

### 3.5 Manual Exploratory Testing

Lakukan ketika requirement ambigu, interaksi kompleks, atau automation belum tersedia. Catat:

- tujuan eksplorasi,
- role dan environment,
- langkah,
- hasil aktual,
- hasil yang diharapkan,
- bukti,
- defect dan severity.

Manual test bukan alasan untuk mengklaim seluruh kombinasi telah diuji.

## 4. Test Design

### 4.1 Derive Tests from Acceptance Criteria

Untuk setiap acceptance criterion, tentukan:

- happy path,
- invalid input,
- boundary values,
- empty state,
- unauthorized/forbidden state,
- dependency failure,
- duplicate/retry behavior,
- concurrency/race condition bila relevan,
- recovery behavior.

Jangan membuat test berdasarkan asumsi yang belum disetujui. Requirement yang belum jelas harus ditandai dan diklarifikasi sebelum dianggap sebagai expected behavior.

### 4.2 Equivalence and Boundary Coverage

Kelompokkan input yang berperilaku sama, lalu pilih representative cases. Uji batas penting, misalnya:

- minimum dan maksimum panjang,
- angka nol, negatif, dan nilai maksimum,
- tanggal awal/akhir,
- file kosong, terlalu besar, atau format salah,
- daftar kosong dan daftar sangat panjang,
- karakter Unicode dan whitespace,
- nilai duplikat.

### 4.3 State Transition Coverage

Untuk workflow/status, definisikan transisi yang sah dan tidak sah. Uji bahwa:

- transisi valid berhasil,
- transisi ilegal ditolak,
- actor yang salah tidak dapat memicu transisi,
- perubahan status tercatat konsisten,
- retry tidak menggandakan side effect.

## 5. Functional QA

Periksa setiap requirement yang termasuk scope:

- halaman dan route dapat diakses sesuai role,
- primary action menghasilkan outcome yang benar,
- navigasi menuju tujuan yang benar,
- form memiliki validasi yang benar,
- data tersimpan dan dibaca secara konsisten,
- filter/sort/search menghasilkan data yang tepat,
- loading, empty, success, dan error state muncul pada kondisi yang tepat,
- refresh/back/forward tidak merusak flow,
- tidak ada tombol atau kontrol penting yang inert,
- tidak ada data contoh yang menyamar sebagai data nyata.

Gunakan data uji yang diketahui. Jangan menyimpulkan kebenaran data hanya dari tampilan satu record.

## 6. Authentication and Authorization QA

Uji authorization pada server, bukan hanya visibilitas UI.

Minimal untuk fitur sensitif:

- unauthenticated request ditolak,
- authenticated user dengan role benar diizinkan,
- role yang salah ditolak,
- user tidak dapat mengakses resource milik user lain,
- perubahan ID/resource identifier tidak melewati ownership check,
- direct API request tetap dilindungi,
- session expiry dan logout bekerja,
- privilege escalation tidak terjadi melalui payload yang dimanipulasi.

Buat matriks role-permission bila sistem memiliki beberapa role.

| Actor | Resource/action | Expected |
|---|---|---|
| Guest | Public resource | Allow sesuai kebijakan |
| Guest | Protected resource | Deny |
| User A | Own resource | Allow sesuai permission |
| User A | User B resource | Deny |
| Admin | Administrative action | Allow sesuai role dan scope |
| Unauthorized role | Restricted action | Deny |

Sesuaikan contoh ini dengan model akses produk. Jangan menganggap setiap aplikasi harus memiliki role Admin.

## 7. Data and Migration QA

Untuk perubahan data atau schema:

- verifikasi schema sebelum dan sesudah,
- jalankan migration pada database test/staging,
- uji data existing dan data baru,
- periksa nullability, unique, foreign key, dan default,
- validasi backfill/transformation,
- periksa record count dan invariant penting,
- uji aplikasi lama/consumer jika migration bertahap,
- dokumentasikan rollback atau forward-fix.

Jangan menjalankan migration destruktif pada data penting sebagai bagian dari test tanpa backup dan persetujuan eksplisit. Rollback migration tidak otomatis memulihkan data yang sudah hilang.

## 8. UI and Visual QA

### 8.1 Visual Consistency

Bandingkan implementasi dengan UI System dan Representative Reference Surface yang telah disetujui.

Periksa:

- typography dan hierarchy,
- color/token usage,
- spacing dan alignment,
- component variants,
- border, radius, elevation,
- icon sizing,
- content density,
- navigation and page shell,
- state consistency,
- layout at target viewports.

Perbaiki sumber inkonsistensi pada shared component atau token, bukan dengan patch lokal di setiap halaman.

### 8.2 Component State Coverage

Untuk komponen interaktif yang relevan, periksa:

- default,
- hover,
- pressed/active,
- focus-visible,
- disabled,
- loading,
- selected/current,
- success,
- error,
- empty,
- expanded/collapsed.

Tidak semua komponen membutuhkan seluruh state tersebut. Gunakan hanya state yang bermakna bagi komponen.

### 8.3 Responsive QA

Uji viewport yang mewakili target penggunaan, setidaknya:

- mobile kecil,
- mobile umum,
- tablet bila relevan,
- desktop umum,
- desktop lebar bila relevan.

Periksa:

- horizontal overflow,
- clipping dan overlap,
- text wrapping,
- navigation collapse,
- table/list behavior,
- touch target,
- modal/dialog sizing,
- sticky/fixed elements,
- keyboard viewport dan form behavior.

Jangan menyatakan responsive hanya karena CSS menggunakan media query.

### 8.4 Screenshot and Visual Regression

Gunakan screenshot ketika perubahan UI perlu bukti visual atau perbandingan antar versi. Pastikan:

- viewport sama,
- route dan state sama,
- data fixture konsisten,
- font dan asset selesai dimuat,
- animasi/loading state dikendalikan,
- sensitive data disamarkan.

Pixel-diff dapat menghasilkan noise karena font rendering, browser, dan antialiasing. Gunakan review manusia untuk perubahan visual yang bermakna; jangan menerima atau menolak berdasarkan angka diff saja.

## 9. Accessibility QA

Gunakan WCAG 2.2 AA sebagai baseline sesuai ruang lingkup produk. Audit bukan hanya warna dan automated scan.

Periksa:

### 9.1 Keyboard

- seluruh fungsi penting dapat digunakan dengan keyboard,
- urutan tab logis,
- focus indicator terlihat,
- tidak ada keyboard trap,
- modal mengelola focus dengan benar,
- Escape dan shortcut tidak mengganggu input normal,
- skip link tersedia bila diperlukan.

### 9.2 Semantics and Assistive Technology

- heading hierarchy masuk akal,
- landmark dan label tersedia,
- button/link digunakan sesuai fungsi,
- form field memiliki accessible name,
- error dikaitkan dengan field terkait,
- status update diumumkan bila diperlukan,
- icon-only control memiliki accessible label,
- tabel memiliki header yang benar.

### 9.3 Visual Accessibility

- contrast teks dan komponen memadai,
- informasi tidak hanya disampaikan melalui warna,
- zoom dan text resizing tidak merusak layout,
- focus tidak tertutup sticky element,
- motion dapat dikurangi,
- target interaksi sesuai kebutuhan WCAG dan konteks perangkat.

### 9.4 Accessibility Tools

Automated scanner membantu menemukan sebagian masalah, tetapi tidak membuktikan kepatuhan penuh. Gabungkan automated scan dengan keyboard testing, pemeriksaan semantik, dan assistive technology checks yang relevan.

Catat temuan dengan criterion/area, lokasi, dampak pengguna, severity, dan evidence.

## 10. Usability QA

Gunakan Nielsen's 10 Usability Heuristics sebagai panduan review:

1. Visibility of system status.
2. Match between system and the real world.
3. User control and freedom.
4. Consistency and standards.
5. Error prevention.
6. Recognition rather than recall.
7. Flexibility and efficiency of use.
8. Aesthetic and minimalist design.
9. Help users recognize, diagnose, and recover from errors.
10. Help and documentation.

Periksa apakah pengguna memahami:

- sedang berada di mana,
- apa yang dapat dilakukan,
- apa yang sedang diproses,
- apakah tindakan berhasil,
- bagaimana membatalkan atau memperbaiki kesalahan,
- apa konsekuensi tindakan destruktif.

Jangan menganggap tampilan modern sebagai bukti usability.

## 11. Browser and Device QA

Pilih browser/device berdasarkan target audience dan dukungan produk. Periksa:

- route loading dan deep link,
- refresh pada nested route,
- browser back/forward,
- cookie/session behavior,
- file upload/download,
- console errors,
- failed network requests,
- font/image loading,
- touch and pointer interactions,
- browser-specific layout differences.

Jika Browser MCP tersedia dan terhubung pada mesin yang tepat, gunakan untuk menjalankan browser test dan mengambil screenshot. Jika tidak tersedia, tandai `NOT AVAILABLE` dan gunakan browser/runtime testing lain yang benar-benar dapat diakses.

Jangan mengklaim Browser MCP atau browser lokal telah digunakan jika runtime AI tidak memiliki akses ke mesin tersebut.

### 11.1 Browser Test Sequence

Pola smoke test yang disarankan:

1. Buka base URL yang benar.
2. Pastikan halaman utama selesai dimuat.
3. Ambil snapshot atau inspeksi struktur.
4. Periksa title dan URL.
5. Jalankan interaksi utama.
6. Verifikasi outcome.
7. Periksa console errors.
8. Periksa request gagal yang relevan.
9. Ulangi untuk route kritis.
10. Ambil screenshot pada state yang perlu bukti.

Gunakan assertion berdasarkan konten/behavior source yang aktual, bukan string yang diingat atau ditebak.

## 12. Performance QA

Pilih metrik berdasarkan target produk dan perangkat pengguna.

Periksa bila relevan:

- initial load dan route transition,
- Core Web Vitals,
- JavaScript bundle size,
- image/font payload,
- API latency dan error rate,
- query count/latency,
- memory/CPU usage,
- long tasks,
- layout shift,
- throughput dan concurrency.

Catat environment, perangkat, network condition, dataset, dan metode pengukuran. Jangan membandingkan angka dari environment yang berbeda tanpa menjelaskan perbedaannya.

Load testing hanya dilakukan pada target yang diizinkan. Jangan membebani layanan pihak ketiga atau production tanpa persetujuan dan batas beban yang jelas.

## 13. Security QA

Lakukan pemeriksaan sesuai threat surface:

- authn/authz dan resource ownership,
- input validation dan injection,
- XSS dan unsafe HTML,
- CSRF untuk session-based request,
- CORS dan security headers,
- rate limiting dan brute-force protection,
- file upload validation,
- secret exposure,
- dependency vulnerabilities,
- webhook signature and replay handling,
- SSRF pada fitur URL fetch,
- sensitive data pada response/log/export.

Gunakan tools dan teknik yang diizinkan. Jangan melakukan penetration test, destructive payload, atau scanning agresif terhadap sistem yang bukan milik user atau tanpa otorisasi eksplisit.

Automated dependency scan bukan pengganti review konfigurasi dan application logic.

## 14. Integration and Failure-Path QA

Uji integrasi dalam kondisi normal dan gagal:

- timeout,
- connection refused,
- unauthorized response,
- rate limit,
- malformed response,
- duplicate webhook/event,
- partial failure,
- retry,
- vendor downtime.

Pastikan sistem tidak membuat transaksi ganda atau menampilkan status sukses palsu. Verifikasi reconciliation path untuk operasi yang dapat memiliki hasil ambigu, misalnya request timeout setelah vendor sebenarnya memproses transaksi.

## 15. Regression QA

Setiap perubahan harus mempertimbangkan apa yang mungkin rusak.

Langkah:

1. identifikasi changed files dan dependencies,
2. cari consumer dari shared module,
3. tentukan critical flows yang bersinggungan,
4. jalankan test yang relevan,
5. periksa area adjacent yang berisiko,
6. catat regression yang belum dapat diuji.

Gunakan Graphify untuk menemukan hubungan file/module bila tersedia dan fresh. Verifikasi temuan graph terhadap source aktual. Graphify bukan bukti bahwa behavior aman.

## 16. Defect Management

Setiap defect yang bermakna harus memiliki:

- ID,
- judul ringkas,
- environment/build,
- precondition,
- langkah reproduksi,
- expected result,
- actual result,
- evidence,
- severity,
- priority,
- status,
- owner atau next action.

### 16.1 Severity

| Severity | Definisi |
|---|---|
| Critical | Data loss, security breach, core service unavailable, atau critical journey tidak dapat digunakan tanpa workaround |
| High | Fitur penting gagal, permission salah, atau banyak pengguna terdampak |
| Medium | Fitur non-kritis terganggu atau tersedia workaround |
| Low | Cosmetic, minor copy/layout, atau dampak kecil |

Severity menggambarkan dampak; priority menentukan urutan perbaikan. Jangan menyamakan keduanya.

### 16.2 Defect Status

Gunakan status yang sederhana dan konsisten:

- `OPEN`
- `TRIAGED`
- `IN PROGRESS`
- `FIXED — PENDING RETEST`
- `VERIFIED`
- `REOPENED`
- `DEFERRED`
- `WONT FIX`

Defect tidak menjadi VERIFIED hanya karena kode diperbarui. Retest harus membuktikan hasil yang diharapkan.

## 17. Test Environment and Data Safety

Pisahkan environment development, test, staging, dan production sesuai kebutuhan.

- Pastikan base URL dan database target sebelum test.
- Gunakan fixture atau seed data yang diketahui.
- Jangan gunakan credential production dalam test.
- Jangan mengirim email, SMS, pembayaran, atau webhook nyata kecuali memang direncanakan dan disetujui.
- Bersihkan test data hanya pada namespace/environment yang aman.
- Hindari parallel test yang saling mengubah data bersama.
- Masking atau synthetic data digunakan untuk data sensitif.
- Jangan menyalin production database ke environment yang tidak memiliki perlindungan setara.

Sebelum command yang berpotensi menghapus data, reset database, atau mengubah environment, jelaskan dampaknya dan minta persetujuan yang sesuai.

## 18. Test Execution Protocol

Sebelum menjalankan test:

1. baca instruksi project dan package scripts yang relevan,
2. identifikasi runtime dan dependency yang tersedia,
3. pastikan environment dan target data benar,
4. pilih command paling spesifik,
5. periksa apakah command memiliki side effect,
6. tentukan expected output atau assertion.

Setelah menjalankan test:

1. simpan hasil penting,
2. bedakan failure produk dari failure environment/tooling,
3. jangan mengubah test untuk menyembunyikan defect,
4. perbaiki akar masalah sesuai scope,
5. jalankan ulang test yang gagal,
6. jalankan regression tests yang relevan,
7. catat command dan hasil aktual.

Jika test tidak dapat dijalankan karena dependency, akses, browser, atau environment, tandai `BLOCKED` atau `NOT RUN` beserta alasannya.

## 19. Release Readiness Gate

Sebelum release, review:

### Critical
- [ ] Primary user journey berjalan.
- [ ] Tidak ada defect Critical atau High yang belum diterima secara eksplisit.
- [ ] Authentication dan authorization sesuai.
- [ ] Data integrity dan migration plan diperiksa.
- [ ] Tidak ada secret yang terekspos.
- [ ] Build/release artifact berhasil dibuat.
- [ ] Konfigurasi environment dan deployment benar.
- [ ] Backup/restore dan rollback/forward-fix plan sesuai risiko.
- [ ] Smoke test pasca-deployment direncanakan.

### Important
- [ ] Responsive checks pada target viewport selesai.
- [ ] Accessibility blockers ditangani.
- [ ] Error, empty, loading, dan recovery states diperiksa.
- [ ] Integrasi eksternal memiliki timeout/failure handling.
- [ ] Logging/error reporting cukup untuk diagnosis.
- [ ] Known limitations didokumentasikan.
- [ ] Dokumentasi operasional dan handoff tersedia.

### Product-dependent
- [ ] Performance/load test memenuhi target yang disepakati.
- [ ] Cross-browser/device matrix selesai.
- [ ] Localization dan timezone diperiksa.
- [ ] Offline behavior/PWA checks selesai.
- [ ] Analytics/telemetry diverifikasi sesuai persetujuan.

Release readiness adalah keputusan berdasarkan risiko dan bukti, bukan jumlah checklist yang dicentang.

## 20. Acceptance and Sign-off

Bedakan tiga hal:

- **Verified:** pemeriksaan teknis yang relevan telah menghasilkan bukti.
- **Accepted:** client menerima hasil sesuai Approval Package dan batas scope.
- **Released:** versi telah dideploy ke target environment dan post-release checks dilakukan.

AI tidak boleh menganggap client acceptance terjadi hanya karena test otomatis berhasil. Perubahan scope atau known limitation yang berdampak material harus disampaikan sebelum acceptance.

## 21. QA Report Template

Gunakan format ringkas berikut setelah increment atau audit:

```markdown
# QA Report

## Scope
- Change / feature:
- Environment:
- Build / commit:
- Audit mode:
- Reason:

## Test Summary
| Test | Status | Evidence / Notes |
|---|---|---|
| Unit | PASS / FAIL / NOT RUN / BLOCKED | ... |
| Integration | ... | ... |
| API / Contract | ... | ... |
| E2E / Browser | ... | ... |
| Responsive | ... | ... |
| Accessibility | ... | ... |
| Security | ... | ... |
| Performance | ... | ... |
| Build / Typecheck / Lint | ... | ... |

## Defects
- DEF-001 — Severity — status — summary

## Verified
- ...

## Not Verified / Blocked
- ...

## Residual Risks
- ...

## Acceptance
- Technical status: IMPLEMENTED / VERIFIED / BLOCKED
- Client acceptance: WAITING / ACCEPTED / REVISION REQUESTED
- Release status: NOT RELEASED / RELEASED

## Next Action
- ...
```

Jangan mengisi PASS tanpa hasil yang benar-benar diamati. Gunakan `NOT APPLICABLE` jika kategori tidak relevan dan jelaskan bila berpotensi disalahartikan.

## 22. Evidence Mapping

| Claim | Minimum supporting evidence |
|---|---|
| Build berhasil | Output build sukses pada environment yang dicatat |
| Typecheck berhasil | Output typecheck sukses |
| Unit logic benar | Test assertion yang relevan berhasil |
| API bekerja | Request/response atau contract test |
| Browser flow bekerja | Runtime/E2E evidence pada route dan state terkait |
| Responsive | Pemeriksaan viewport yang dicatat |
| Accessibility | Automated + manual checks yang relevan |
| Permission benar | Positive dan negative server-side tests |
| Migration aman | Migration/data verification dan recovery plan |
| Performance memenuhi target | Measurement dibanding target yang disepakati |
| Deployment berhasil | Deployment result + post-deploy smoke test |

Evidence harus dapat ditelusuri ke perubahan dan environment yang diuji. Screenshot membuktikan tampilan pada satu kondisi, bukan seluruh behavior aplikasi.

## 23. Definition of QA Done

QA increment selesai ketika:

- [ ] scope dan risiko telah ditentukan,
- [ ] acceptance criteria memiliki pemeriksaan yang relevan,
- [ ] test yang wajib untuk perubahan telah dijalankan,
- [ ] defect penting dicatat dan ditriase,
- [ ] regression scope dipertimbangkan,
- [ ] hasil dan evidence dicatat,
- [ ] test yang tidak dijalankan dinyatakan,
- [ ] residual risk disampaikan,
- [ ] status teknis dan client acceptance dipisahkan,
- [ ] project state dan WBS diperbarui.

## 24. Anti-Patterns

Jangan:

- menganggap build sukses berarti fitur benar,
- menganggap halaman tampil berarti flow selesai,
- mengklaim WCAG compliant hanya dari automated scan,
- menguji hanya happy path,
- menguji permission hanya dengan menyembunyikan tombol,
- memakai data production sembarangan,
- menjalankan test destruktif pada database aktif,
- mengabaikan console/network error yang relevan,
- menutupi test failure dengan mengubah assertion tanpa alasan,
- menerima screenshot dari viewport yang tidak diketahui sebagai bukti responsive,
- menjalankan load test tanpa batas dan izin,
- menganggap Graphify membuktikan behavior,
- menandai defect VERIFIED tanpa retest,
- menyebut aplikasi production-ready tanpa release evidence,
- mencampur status implemented, verified, accepted, dan released.

## 25. Compact QA Instruction

> Tentukan cakupan QA berdasarkan requirement, risiko, dan blast radius. Uji primary journey, boundary, error/recovery path, data, permission, dan regression yang relevan. Gunakan test otomatis serta pemeriksaan manual/visual/accessibility sesuai kebutuhan. Pastikan environment dan data aman sebelum menjalankan command. Catat command, hasil, evidence, defect, dan hal yang belum diverifikasi. Jangan mengklaim keberhasilan tanpa bukti. Pisahkan verified, client accepted, dan released.

---

## Source Mapping

Modul ini memisahkan materi quality dan release dari bootstrap utama, terutama:

- Phase 5 — Quality, Accessibility & Design Review,
- Phase 6 — Release & Handoff,
- UI Component System Gate,
- Page Definition of Ready / Done,
- Critical / Important / Optional quality gates,
- Evidence Rules,
- Graphify Audit Necessity Classifier dan Audit / Graphify Decision Matrix,
- Browser MCP verification profile dan browser test sequence,
- Completion / Handoff Protocol.

Detail test strategy, test data safety, defect lifecycle, security test coverage, dan QA report ditata menjadi prosedur operasional agar hasil verifikasi dapat diulang dan diaudit. Terapkan berdasarkan risiko dan target produk.
