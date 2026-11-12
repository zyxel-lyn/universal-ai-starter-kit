---
name: project-continuity
version: "1.0.0"
language: "id-ID"
purpose: "Menjaga kesinambungan konteks, keputusan, requirement, evidence, dan pekerjaan lintas sesi maupun workspace."
---

# 07 — Project Continuity & Handoff

Dokumen ini mengatur bagaimana AI melanjutkan project tanpa membangun ulang konteks dari nol, kehilangan keputusan yang telah disetujui, atau menganggap file yang disebut dalam riwayat otomatis tersedia.

> **Prinsip utama:** project state harus berasal dari artefak dan evidence yang dapat diakses. Riwayat chat membantu, tetapi bukan pengganti checkpoint yang dapat diverifikasi.

## 1. Tujuan dan Ruang Lingkup

Modul ini mengatur:

- checkpoint project setelah pekerjaan bermakna,
- struktur artefak konteks,
- pemulihan state pada sesi baru,
- pencatatan keputusan dan approval,
- sinkronisasi requirement, desain, UI system, WBS, dan QA,
- migrasi antar-chat, workspace, account, atau platform,
- handoff ke AI/developer berikutnya,
- pencegahan audit ulang dan pengulangan pekerjaan yang tidak perlu.

Modul ini tidak menganggap semua project harus memiliki seluruh dokumen. Buat artefak sesuai kompleksitas dan kebutuhan project.

## 2. Sumber Kebenaran dan Urutan Prioritas

Gunakan urutan berikut ketika informasi berbeda atau bertentangan:

1. Batas keselamatan, keamanan, dan hukum.
2. Instruksi eksplisit client yang berlaku saat ini.
3. Baseline terbaru yang telah disetujui client.
4. Requirement dan evidence project yang telah diverifikasi.
5. Keputusan arsitektur/desain yang telah diterima.
6. State/checkpoint project terbaru yang masih valid.
7. Riwayat percakapan yang relevan.
8. Asumsi atau default AI.

Jangan membiarkan ringkasan AI, skill eksternal, atau dokumen lama diam-diam mengalahkan keputusan client terbaru.

### 2.1 Bedakan Status Informasi

Gunakan label konsisten:

| Status | Makna |
|---|---|
| CONFIRMED | Client menyatakan atau mengonfirmasi secara eksplisit |
| PROPOSED | Rekomendasi developer/AI yang belum disetujui |
| NEEDS CONFIRMATION | Informasi penting masih ambigu |
| APPROVED / LOCKED | Baseline atau keputusan telah disetujui |
| CHANGED | Informasi berubah dari baseline sebelumnya |
| VERIFIED | Diperiksa dengan evidence yang relevan |
| STALE | Pernah valid, tetapi berpotensi tidak lagi mencerminkan kondisi aktual |
| UNKNOWN | Belum diketahui dan tidak boleh ditebak |

Jangan mengubah PROPOSED menjadi CONFIRMED hanya karena client belum membantahnya.

## 3. Project Context Artifacts

Pilih artefak berdasarkan kebutuhan. Untuk project web/app non-trivial, struktur inti yang direkomendasikan:

| Artefak | Isi dan fungsi |
|---|---|
| `PRODUCT.md` | Tujuan produk, pengguna, nilai, batas scope |
| `FLOW_BASELINE.md` | User journey, halaman, role, data, business rules, integrasi |
| `DESIGN.md` | Arah visual, design rationale, typography, color, responsive |
| `UI_SYSTEM.md` | Tokens, component contracts, states, motion, registry |
| `DECISIONS.md` | Keputusan penting, status, alasan, konsekuensi |
| `PROJECT_STATE.md` | Snapshot kondisi project dan next actions |
| `WBS.md` | Work breakdown, dependency, milestone, status |
| `QA_REPORT.md` | Test plan, hasil, defect, evidence, acceptance |
| `RELEASE_NOTES.md` | Perubahan rilis, known limitations, rollback/handoff |
| `GRAPH_REPORT.md` | Ringkasan graph struktural jika Graphify digunakan |

Dokumen tambahan hanya dibuat jika memberikan nilai nyata, misalnya architecture decision record, API contract, data dictionary, threat model, migration map, atau operational runbook.

### 3.1 Suggested Repository Context Layout

```text
project/
├── PRODUCT.md
├── FLOW_BASELINE.md
├── DESIGN.md
├── UI_SYSTEM.md
├── DECISIONS.md
├── PROJECT_STATE.md
├── WBS.md
├── QA_REPORT.md
├── RELEASE_NOTES.md
├── graphify-out/
│   ├── GRAPH_REPORT.md
│   ├── graph.json
│   └── graph.html
├── docs/
│   ├── architecture/
│   ├── ux/
│   ├── qa/
│   └── handoff/
└── src/
```

Jangan membuat dokumen kosong hanya untuk memenuhi struktur contoh. Gunakan nama/path yang sudah dipakai project bila konsisten; jangan menciptakan duplikasi artefak dengan fungsi sama.

## 4. PROJECT_STATE.md — Snapshot Utama

`PROJECT_STATE.md` adalah titik masuk utama untuk memulihkan kondisi project. Jaga agar ringkas, aktual, dan dapat diverifikasi.

Template:

```markdown
# PROJECT_STATE

## Project Identity
- Name:
- Goal:
- Repository/workspace:
- Current mode: GREENFIELD | EXISTING_PRODUCT | REDESIGN | MIGRATION | AUDIT_ONLY | FEATURE_EXTENSION | PROTOTYPE

## Current Phase
- Phase:
- WBS milestone:
- Current scope:
- Last completed increment:

## Client Collaboration State
- Latest Approval Package:
- Approval status: WAITING | APPROVED | REVISION REQUESTED
- Last client instruction:
- Pending client confirmations:

## Requirement State
- CONFIRMED:
- PROPOSED:
- NEEDS CONFIRMATION:
- LOCKED:
- CHANGED:

## Stack and Environment
- Frontend:
- Backend:
- Database:
- Runtime:
- Deployment:
- Relevant versions:

## Product and UI Baseline
- FLOW_BASELINE path/version:
- DESIGN path/version:
- UI_SYSTEM path/version/status:
- Representative reference surface:
- Known component gaps:

## Architecture Snapshot
- Routes:
- Shared layouts:
- Core modules:
- State/data flow:
- Auth/roles:
- External integrations:

## Active Skills and Tools
- Name:
- Why needed:
- Availability verified:
- Version/commit:

## Decisions
- Latest accepted decision:
- Superseded decisions:
- Pending decisions:

## Last Verified Evidence
- Build:
- Tests:
- Runtime:
- Responsive:
- Accessibility:
- Security:
- Deployment:
- Commit/source checkpoint:

## Open Risks and Defects
- RISK-001:
- BUG-001:

## Current Work
- IN PROGRESS:
- BLOCKED:
- WAITING FOR CLIENT:

## Next Actions
1.
2.
3.

## Freshness
- State updated:
- Source commit:
- Evidence timestamp:
- Graph status: FRESH | STALE | NOT AVAILABLE
- Known stale areas:
```

Aturan:

- Update hanya bagian yang berubah; jangan menulis ulang seluruh state tanpa kebutuhan.
- Tanggal/waktu harus mencerminkan waktu pemeriksaan aktual.
- Jika evidence tidak tersedia, tulis NOT VERIFIED atau UNKNOWN.
- Jangan mencantumkan secret.
- Simpan path artefak agar AI berikutnya tahu dokumen mana yang perlu dibaca.

## 5. FLOW_BASELINE.md — Product Truth

Flow baseline menyimpan kontrak produk yang telah disepakati.

Minimum isi:

- product purpose dan target user,
- roles dan permission,
- primary user journeys,
- daftar halaman/screen dan tujuan,
- navigasi,
- input/output data,
- business rules,
- state loading/empty/error/success,
- integrations,
- responsive requirements,
- confirmed requirements,
- proposed items dan open questions,
- approval/version history.

Setiap requirement penting sebaiknya memiliki ID stabil:

```text
REQ-001 — Pengguna dapat membuat permohonan.
Status: CONFIRMED
Source: Client approval package v0.2
Acceptance: ...
Dependencies: ...
```

Perubahan flow atau business rule harus tercermin di baseline dan impact analysis. Jangan hanya mengubah source code lalu membiarkan dokumentasi produk menyatakan perilaku lama.

## 6. DECISIONS.md — Decision Log

Catat keputusan yang memengaruhi scope, UX, arsitektur, data, keamanan, dependency, atau delivery.

Template:

```markdown
## DEC-XXX — <Decision title>

- Date:
- Status: PROPOSED | ACCEPTED | SUPERSEDED
- Context:
- Decision:
- Alternatives considered:
- Rationale:
- Consequences:
- Evidence:
- Approved by:
- Supersedes:
```

Aturan decision log:

1. Satu keputusan penting memiliki ID stabil.
2. Catat alasan, bukan hanya hasil akhir.
3. Tandai keputusan lama sebagai SUPERSEDED; jangan menghapus sejarahnya.
4. Keputusan yang belum disetujui tetap PROPOSED.
5. Jangan membuka kembali keputusan ACCEPTED tanpa bukti baru, perubahan requirement, risiko baru, atau instruksi client.
6. Jangan menyimpulkan persetujuan dari diam atau tidak adanya komentar.

## 7. WBS.md — Progress dan Dependency

WBS harus menunjukkan pekerjaan yang benar-benar dilakukan, bukan daftar aspirasi yang tampak produktif.

Setiap task sebaiknya mencakup:

- ID,
- deliverable,
- priority,
- status,
- dependency,
- owner,
- acceptance criteria,
- evidence,
- blocker.

Gunakan daftar status WBS kanonis pada `05_PROJECT_MANAGEMENT.md` bagian 6.5. Jangan membuat daftar status WBS alternatif di PROJECT_STATE, WBS, atau dokumen continuity.

Saat menampilkan progres, gunakan status kanonis persis sebagaimana didefinisikan modul 05. `DONE` bukan status WBS: gunakan `ACCEPTED` jika client telah menerima deliverable, atau `VERIFIED` jika kriteria verifikasi telah terpenuhi tetapi penerimaan client belum dicatat.

Aturan:

- `IN REVIEW` bukan `VERIFIED` atau `ACCEPTED`.
- `WAITING FOR CLIENT` hanya digunakan ketika ada input/review/approval client yang benar-benar ditunggu; tulis hal yang ditunggu.
- `BLOCKED` harus memiliki alasan dan dependency yang jelas.
- `DEFERRED` dan `CANCELLED` harus memiliki alasan/catatan.
- Scope baru masuk melalui change/impact assessment.
- Pecah task sampai cukup kecil untuk diverifikasi.
- Hindari estimasi palsu; tandai estimasi sebagai perkiraan dan sebutkan asumsi.

## 8. Approval dan Baseline Lock

Approval hanya berlaku untuk isi Approval Package yang disebutkan.

Approval Package minimum:

```markdown
## Approval Package — <name/version>

### Sudah disepakati
- ...

### Rekomendasi yang akan dikunci
- ...

### Perubahan dari baseline sebelumnya
- Tidak ada / ...

### Pertanyaan terbuka
- Tidak ada / ...

### Dampak dan batas scope
- ...

### Setelah disetujui
- Next step: ...

**Status: WAITING FOR CLIENT APPROVAL**
```

Ketika client menyetujui:

1. catat nama/version package,
2. tandai APPROVED/LOCKED,
3. update FLOW_BASELINE atau DECISIONS jika relevan,
4. update PROJECT_STATE dan WBS,
5. lanjutkan hanya scope yang disetujui.

Instruksi seperti `Approve` atau `Oke Lanjutkan` hanya memajukan workflow dari checkpoint eksplisit terbaru. Instruksi `Sebelum lanjut...` menghentikan langkah berikutnya sampai perubahan dan dampaknya diintegrasikan.

## 9. Checkpoint Setelah Pekerjaan Bermakna

Buat checkpoint ketika terjadi salah satu kondisi:

- milestone atau WBS task selesai,
- requirement atau user flow berubah,
- client menyetujui baseline,
- keputusan arsitektur/data/security dibuat,
- shared component atau UI system berubah,
- migrasi atau perubahan database dilakukan,
- build/test/release menghasilkan evidence penting,
- pekerjaan akan berpindah ke chat, agent, atau developer lain,
- sesi berpotensi berakhir sebelum task selesai.

Checkpoint tidak harus berupa dokumen baru. Update artefak yang tepat dan PROJECT_STATE.

### 9.1 Checkpoint Record

```markdown
## Checkpoint — <date/time>

- Project:
- Current phase:
- Source commit/version:
- Work completed:
- Files/modules changed:
- Decisions made:
- Client approvals:
- Verification performed:
- Verification result:
- Known limitations:
- Open blockers:
- Next exact action:
- Required client input:
- State freshness:
```

Next exact action harus berupa langkah yang dapat dijalankan, bukan kalimat umum seperti "lanjutkan development".

## 10. Completion / Handoff Protocol

Pada akhir setiap increment yang bermakna:

1. Jelaskan perubahan yang dilakukan.
2. Jelaskan pemeriksaan yang berhasil dilakukan.
3. Jelaskan bagian yang belum diverifikasi.
4. Nyatakan apakah approval client dibutuhkan sebelum tahap berikutnya.
5. Update WBS.
6. Update PROJECT_STATE.
7. Update FLOW_BASELINE bila requirement/flow berubah.
8. Update DECISIONS bila keputusan durable berubah.
9. Update DESIGN bila arah visual atau design truth berubah.
10. Update UI_SYSTEM bila token, variant, state, atau component contract berubah.
11. Update QA_REPORT bila test plan/hasil/defect berubah.
12. Refresh Graphify bila perubahan struktural memang membutuhkannya.
13. Catat limitation dan risiko.
14. Siapkan Approval Package berikutnya atau next action prioritas.

Jangan menyatakan seluruh project selesai hanya karena satu increment telah selesai.

### 10.1 Handoff Summary

Gunakan format ringkas:

```text
PROJECT HANDOFF
- Project:
- Current phase:
- Latest approved baseline:
- Completed:
- In progress:
- Blocked:
- Last verified commit:
- Tests/build:
- Known limitations:
- Pending client decisions:
- Next exact action:
- Files to read first:
```

## 11. Pemulihan State pada Chat atau Sesi Baru

Saat memulai sesi baru:

1. Identifikasi platform, runtime, dan workspace aktif.
2. Pastikan path project benar-benar dapat diakses.
3. Temukan PROJECT_STATE yang relevan.
4. Baca PROJECT_STATE terlebih dahulu.
5. Baca baseline/decision/WBS/QA/UI artifacts sesuai tugas berikutnya.
6. Periksa commit atau checkpoint sumber.
7. Verifikasi apakah evidence masih fresh.
8. Periksa perubahan sejak checkpoint hanya pada area yang relevan.
9. Identifikasi konflik antara dokumen, source, dan instruksi terbaru.
10. Lanjutkan dari next exact action yang valid.

Jangan:

- meminta client mengulang informasi yang sudah terkonfirmasi dan dapat diakses,
- menganggap file yang disebut dalam chat otomatis tersedia,
- mengulang audit seluruh project tanpa alasan,
- mengklaim pekerjaan lama masih valid tanpa memeriksa freshness,
- melanjutkan task yang sebenarnya masih menunggu approval.

### 11.1 Audit Necessity Classifier

| Kondisi | Tindakan |
|---|---|
| State, source, dan evidence fresh; perubahan lokal | Targeted inspection dan verification |
| Perubahan shared component | Trace consumers dan audit affected pages |
| Perubahan auth/role/security boundary | Security + functional audit terkait |
| Perubahan schema/migration | Data integrity + migration verification |
| Graph fresh, runtime evidence stale | Gunakan graph untuk navigasi; rerun runtime/visual tests |
| Graph stale setelah perubahan struktural besar | Refresh graph sebelum traversal |
| Legacy project tidak dikenal | Initial baseline audit biasanya diperlukan |
| Release candidate dengan evidence tidak lengkap | Audit release yang relevan |
| Konflik dokumen dan source | Periksa source/evidence dan rekonsiliasi state |
| Bukti tidak cukup untuk permintaan saat ini | Perluas inspeksi secara terarah |

Graphify membantu mengetahui area yang perlu diperiksa. Graphify bukan bukti bahwa aplikasi berjalan benar.

## 12. Chat Migration Pack

Untuk memindahkan project ke percakapan ChatGPT Web baru, lampirkan artefak yang tersedia dan diperlukan, dalam urutan berikut:

```text
01 MASTER_BOOTSTRAP.md
02 PROJECT_STATE.md
03 FLOW_BASELINE.md
04 DESIGN.md
05 UI_SYSTEM.md                 # jika shared component system non-trivial
06 DECISIONS.md
07 WBS.md
08 QA_REPORT.md                 # jika QA sedang berlangsung/relevan
09 graphify-out/GRAPH_REPORT.md # bila Graphify digunakan
10 component workbench + screenshots/visual evidence yang relevan
11 source files yang diperlukan untuk next task saja
12 graphify-out/graph.json      # opsional untuk traversal lebih dalam
```

Urutan ini adalah prioritas baca, bukan kewajiban mengirim seluruh file. Jika file tidak ada atau tidak relevan, jangan membuat salinan kosong hanya untuk memenuhi daftar.

### 12.1 Instruksi untuk AI Penerima

```text
Anda melanjutkan project yang sudah berjalan, bukan memulai project baru.

1. Baca PROJECT_STATE dan tentukan checkpoint terbaru.
2. Baca FLOW_BASELINE, DESIGN, UI_SYSTEM bila relevan, DECISIONS,
   WBS, QA report, dan evidence yang terkait dengan next task.
3. Gunakan keputusan client yang telah disetujui sebagai baseline.
4. Jangan meminta client mengulang informasi yang telah dikonfirmasi.
5. Jangan melakukan full-project audit ulang kecuali evidence stale,
   kontradiktif, berisiko, atau tidak cukup untuk task saat ini.
6. Verifikasi akses dan freshness file sebelum mengandalkannya.
7. Jangan menganggap tool, skill, dependency, atau service tersedia
   tanpa pemeriksaan runtime.
8. Lanjutkan dari next exact action.
9. Jangan mengubah scope atau keputusan locked tanpa approval.
10. Laporkan apa yang diverifikasi dan apa yang belum.
```

## 13. Migrasi Antar-Workspace atau Platform

Perpindahan environment adalah migrasi, bukan sekadar chat baru.

Sebelum transfer:

1. Tentukan artefak apa yang sudah tersedia pada tujuan.
2. Identifikasi file/configuration yang benar-benar hilang.
3. Pilih hanya artefak yang relevan untuk pekerjaan berikutnya.
4. Periksa ukuran, format, encoding, permission, dan integritas file.
5. Buat manifest transfer.
6. Jangan sertakan cache, build output, dependency folder, atau duplikat kecuali dibutuhkan.
7. Jangan sertakan secret.
8. Setelah transfer, verifikasi file tujuan dan hash bila byte integrity penting.
9. Perbarui PROJECT_STATE agar mencerminkan workspace tujuan.

### 13.1 Transfer Manifest

```text
MIGRATION MANIFEST
- Source environment:
- Destination environment:
- Migration date:
- Project checkpoint:
- Files transferred:
- Files intentionally excluded:
- Secrets excluded: YES
- Integrity verification:
- Destination access verified:
- Runtime differences:
- Missing capabilities:
- Follow-up actions:
```

### 13.2 Batasan Migrasi

- File yang sama dapat memiliki path berbeda.
- Native skills/MCP/plugins tidak otomatis ikut bersama source code.
- Process/service yang sedang berjalan tidak ikut dipindahkan sebagai file.
- Environment variable dan secret harus dikonfigurasi ulang melalui mekanisme aman.
- Browser MCP ke Chrome mesin user membutuhkan jalur koneksi baru yang valid.
- Runtime cloud/sandbox mungkin tidak memiliki akses ke filesystem lokal user.
- Build yang berhasil di satu mesin tidak menjamin build identik di mesin lain.

## 14. Evidence, Freshness, dan Klaim

Setiap klaim status harus memiliki batas yang jelas.

Bedakan:

- **DESIGNED:** rancangan dibuat.
- **IMPLEMENTED:** perubahan telah diterapkan.
- **BUILT:** build command berhasil.
- **TESTED:** test tertentu dijalankan.
- **VERIFIED:** acceptance criterion diperiksa dengan evidence yang sesuai.
- **ACCEPTED:** client/stakeholder menerima hasil.

Contoh:

```text
Build: PASS (command + timestamp)
Unit tests: PASS (N tests)
E2E: NOT RUN
Responsive: VERIFIED (viewport A, B)
Accessibility: PARTIAL (automated scan only)
Deployment: NOT VERIFIED
Client acceptance: WAITING
```

Jangan menulis "semua sudah aman" jika yang dilakukan hanya build. Jangan mengubah screenshot lama menjadi bukti runtime saat ini.

### 14.1 Evidence Freshness

Tandai evidence STALE bila:

- source berubah setelah evidence diambil,
- dependency/framework berubah,
- environment atau konfigurasi berubah,
- test plan tidak lagi sesuai,
- route/component yang diuji berubah,
- waktu atau data eksternal membuat hasil tidak relevan.

Evidence lama dapat menjadi referensi historis, tetapi tidak boleh disajikan sebagai verifikasi terbaru.

## 15. Graphify and Structural Context

Gunakan Graphify jika membantu menavigasi hubungan antar-file, simbol, route, component, atau dependency.

- Catat commit/source checkpoint yang menjadi dasar graph.
- Tandai graph STALE setelah perubahan struktural signifikan.
- Refresh graph bila manfaatnya melebihi biaya.
- Gunakan graph untuk menemukan blast radius dan area inspeksi.
- Verifikasi hasil dengan source, runtime, tests, dan acceptance evidence.

Jangan menganggap graph report sebagai dokumentasi lengkap atau pengganti test.

## 16. Context Efficiency

Tujuan continuity adalah mengurangi pengulangan, bukan mengirim seluruh repository dan seluruh riwayat ke setiap sesi.

Praktik:

1. Baca state dan ringkasan terlebih dahulu.
2. Ambil hanya dokumen yang berhubungan dengan task.
3. Baca source pada area terdampak.
4. Gunakan graph atau search untuk menemukan dependency bila tersedia.
5. Hindari memuat semua skill sekaligus.
6. Simpan keputusan durable pada DECISIONS, bukan hanya chat.
7. Simpan next exact action di PROJECT_STATE.
8. Ringkas pekerjaan selesai; pertahankan detail yang diperlukan untuk reproduksi.
9. Pisahkan fakta, rekomendasi, asumsi, dan pertanyaan terbuka.
10. Jangan menghapus konteks penting demi ringkasan yang terlalu pendek.

## 17. Risk and Recovery

Jika checkpoint hilang, rusak, atau bertentangan:

1. Jangan langsung menimpa file.
2. Identifikasi sumber dan tanggal setiap artefak.
3. Bandingkan dengan source commit, test evidence, dan approval terbaru.
4. Rekonstruksi hanya bagian yang dapat dibuktikan.
5. Tandai bagian yang belum pasti sebagai UNKNOWN/NEEDS CONFIRMATION.
6. Minta client memutuskan hanya bila konflik menyangkut keputusan produk yang tidak dapat diselesaikan dari evidence.
7. Simpan recovery note sebelum melanjutkan perubahan besar.

Jika pekerjaan terputus di tengah operasi:

- periksa apakah operasi telah selesai sebelum mengulangnya,
- periksa working tree/status,
- verifikasi file/record target,
- jangan mengulang migrasi, deployment, atau side-effecting command secara membabi buta.

## 18. Operating Checklist

### Sebelum melanjutkan pekerjaan

```text
[ ] Platform/runtime/workspace aktif teridentifikasi.
[ ] PROJECT_STATE dapat dibaca dan freshness diperiksa.
[ ] Latest approved baseline ditemukan.
[ ] Requirement dan open decisions dipahami.
[ ] Next action sesuai WBS dan approval terbaru.
[ ] Source/evidence relevan tersedia.
[ ] Tool/skill yang diperlukan benar-benar tersedia.
[ ] Scope dan risiko task dipahami.
```

### Sebelum mengakhiri increment

```text
[ ] Perubahan dan file terdampak dicatat.
[ ] Build/test/runtime evidence diperbarui.
[ ] Unverified items dan known limitations dicatat.
[ ] WBS diperbarui.
[ ] PROJECT_STATE diperbarui.
[ ] FLOW_BASELINE/DECISIONS/DESIGN/UI_SYSTEM diperbarui bila berubah.
[ ] QA report diperbarui bila relevan.
[ ] Approval kebutuhan tahap berikutnya dinyatakan.
[ ] Next exact action ditulis.
```

### Sebelum migrasi

```text
[ ] Tujuan dan workspace destination diverifikasi.
[ ] Artefak yang sudah tersedia tidak dikirim ulang.
[ ] Hanya file relevan yang dipilih.
[ ] Secret dan credential dikecualikan.
[ ] Manifest dibuat.
[ ] Integritas file diverifikasi bila diperlukan.
[ ] Runtime differences dan capability gaps dicatat.
[ ] PROJECT_STATE destination diperbarui.
```

## 19. Final Continuity Rules

1. PROJECT_STATE adalah titik masuk, bukan pengganti detail baseline.
2. Client-approved baseline mengalahkan proposal AI dan catatan lama.
3. PROPOSED tidak sama dengan CONFIRMED.
4. Status akhir WBS mengikuti `ACCEPTED` atau `VERIFIED` sesuai bukti dan penerimaan; jangan gunakan `DONE` sebagai status task.
5. Evidence stale harus ditandai dan diperbarui bila relevan.
6. Chat folder tidak membuktikan shared filesystem.
7. Chat baru pada workspace sama tidak otomatis berarti migrasi.
8. Workspace/platform berbeda diperlakukan sebagai migrasi.
9. Handoff hanya membawa artefak yang dibutuhkan.
10. Secret tidak boleh masuk ke handoff pack.
11. Jangan mengulang full audit tanpa alasan yang terklasifikasi.
12. Setiap increment bermakna harus meninggalkan checkpoint dan next exact action.
