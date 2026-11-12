---
name: auto-setup-tools
version: "1.0.0"
language: "id-ID"
purpose: "Panduan verifikasi runtime, AUTO-SETUP aset MCP dan skill yang tercantum, penggunaan tools, keamanan, serta kesinambungan workspace."
source: "UNIVERSAL_AI_WEB_APP_DEVELOPMENT_BOOTSTRAP v1.3.0"
---

# 06 — AUTO-SETUP, MCP, Tools & Skills

Dokumen ini memisahkan seluruh prosedur setup runtime dan aset pendukung dari bootstrap utama. Tujuannya adalah membuat AI mampu memeriksa kemampuan environment, menyiapkan aset yang memang tercantum, dan menggunakan tools secara aman tanpa mengarang ketersediaan.

> **Prinsip utama:** verifikasi dahulu, gunakan ulang yang sudah ada, jangan membuat duplikasi, jangan mengklaim tool aktif tanpa bukti, dan jangan membiarkan kegagalan satu aset menghentikan pekerjaan lain.

## 1. Ruang Lingkup

Modul ini mencakup:

- identifikasi runtime dan workspace,
- AUTO-SETUP MCP shadcn dan Ant Design,
- pemeriksaan Browser MCP pada mesin user,
- instalasi dan routing skill eksternal yang tercantum,
- inventaris skill native platform,
- keamanan instalasi dan konfigurasi,
- model persistensi workspace/runtime,
- profil MCP dan custom tools,
- prosedur Windows dan PowerShell,
- continuity antar-chat/workspace.

Modul ini **tidak** menganggap semua kemampuan tersedia di setiap platform. Ketersediaan harus diverifikasi pada runtime yang sedang digunakan.

## 2. Persetujuan Standing AUTO-SETUP

Bootstrap sumber mencatat bahwa client telah memberikan persetujuan berdiri pada 9 November 2026 untuk menyiapkan aset yang tercantum di dalamnya, selama setup bersifat idempoten dan mematuhi batas keamanan pada modul ini.

Konsekuensinya:

1. Aset yang tercantum dalam daftar AUTO-SETUP boleh diverifikasi dan dipasang tanpa meminta approval berulang.
2. Aset yang sudah tersedia atau sudah aktif tidak boleh diduplikasi.
3. Instalasi atau verifikasi satu aset yang gagal tidak boleh menghentikan langkah lain.
4. AUTO-SETUP hanya berlaku untuk aset yang tercantum di modul ini.
5. MCP, skill, atau tool baru di luar daftar memerlukan proses intake dan persetujuan client.
6. Standing approval untuk setup bukan izin untuk menghapus file, mengubah project, mengirim secret, commit/push, atau menjalankan operasi destruktif.

Jangan memperluas persetujuan ini ke aset atau operasi yang tidak tercantum.

## 3. Batas Keamanan yang Selalu Berlaku

### 3.1 Secret dan credential

Jangan pernah mencetak, mengirim, atau memasukkan hal berikut ke log, screenshot, laporan, atau handoff pack:

- API key,
- access/refresh token,
- password,
- cookie/session,
- private key,
- raw Authorization header,
- credential database,
- pairing secret,
- token shutdown daemon.

Bila secret perlu digunakan, kirim hanya melalui mekanisme autentikasi dan endpoint tujuan yang memang ditetapkan.

### 3.2 Operasi sensitif

Sebelum operasi berikut dilakukan, jelaskan target dan efeknya:

- menghapus atau menimpa file,
- mematikan proses,
- mengubah konfigurasi runtime/MCP,
- mengganti dependency global,
- commit atau push ke repository client,
- menjalankan migrasi/destructive database operation,
- mengubah akses atau permission.

Operasi destruktif berisiko tinggi membutuhkan persetujuan spesifik per kasus. Standing approval AUTO-SETUP tidak menggantikan persetujuan tersebut.

### 3.3 Instalasi eksternal

- Clone/unduh hanya dari URL yang tercantum atau URL baru yang telah disetujui melalui intake.
- Jangan menjalankan script asing secara membabi buta.
- Periksa script, command, permission, dan efek samping sebelum menjalankannya.
- Hindari menjalankan installer dengan privilege administrator jika tidak diperlukan.
- Jangan mengubah konfigurasi project tanpa memeriksa konteks dan scope file.
- Catat versi/commit jika dependency atau skill akan memengaruhi hasil project.

### 3.4 Klaim berbasis evidence

Gunakan status:

- `READY`: tersedia dan berhasil diverifikasi.
- `PARTIAL`: sebagian capability tersedia.
- `NOT AVAILABLE`: tidak tersedia pada runtime saat ini.
- `FAILED`: proses pemeriksaan/instalasi gagal, sertakan sebab.
- `NOT VERIFIED`: belum cukup bukti.

Nama package, konfigurasi, atau path di dokumen bukan bukti bahwa aset sedang aktif.

## 4. Step 0 — Identifikasi Runtime dan Workspace

Sebelum setup:

1. Identifikasi platform/runtime aktif: ZCode desktop, web chat, CLI, sandbox, remote workspace, atau lainnya.
2. Identifikasi root workspace yang benar-benar dapat diakses.
3. Periksa apakah runtime memiliki terminal/shell, filesystem, Node.js, npm/npx, Git, dan akses internet yang diperlukan.
4. Bila relevan, catat versi:
   ```powershell
   node --version
   npm --version
   npx --version
   git --version
   ```
5. Buktikan path dapat dibaca sebelum menggunakannya.
6. Jangan membuat path berdasarkan dugaan.
7. Gunakan struktur folder yang sudah ada. Bila belum ada dan dibutuhkan, buat hanya direktori minimum yang relevan.

Konvensi workspace yang dapat digunakan bila sesuai:

| Folder | Fungsi |
|---|---|
| `upload/` | Input dari client |
| `scripts/` | Helper scripts |
| `download/` | Deliverables |
| `skills/` | Skill eksternal |
| `docs/` | Dokumentasi project |

Jangan membuat seluruh folder hanya demi mengikuti contoh. Buat sesuai kebutuhan.

### 4.1 Runtime Capability Snapshot

Catat hasil secara ringkas:

```text
RUNTIME:
- Platform:
- Workspace root:
- Filesystem access: READY / PARTIAL / NOT AVAILABLE
- Shell access: READY / NOT AVAILABLE
- Node:
- npm/npx:
- Git:
- Internet access:
- Native MCP:
- Native Skills:
- Browser access:
- Date checked:
```

## 5. AUTO-SETUP Workflow

Lakukan langkah berikut secara berurutan. Langkah yang tidak relevan atau tidak dapat dijangkau harus diberi status yang jujur, bukan dipaksakan.

### Step 1 — shadcn MCP

**Peran:** registry/reference server untuk komponen shadcn/ui.

Command verifikasi melalui driver stdio:

```powershell
node scripts/stdio_mcp_client.mjs --server "npx -y shadcn@latest mcp" --list --timeout 120000
```

Baseline tools yang tercatat pada sumber:

- `get_project_registries`
- `list_items_in_registries`
- `search_items_in_registries`
- `view_items_in_registries`
- `get_item_examples_from_registries`
- `get_add_command_for_items`
- `get_audit_checklist`

Ekspektasi baseline: serverInfo `shadcn`, sekitar 7 tools.

Verifikasi:

1. Pastikan driver tersedia; bila tidak, gunakan Appendix A pada modul ini untuk membuatnya.
2. Jalankan command dengan timeout yang cukup untuk unduhan pertama.
3. Catat serverInfo dan jumlah tools aktual.
4. Tandai `READY` hanya jika handshake dan daftar tools berhasil.
5. Sebagian tool registry memerlukan `components.json` pada project target. Kegagalan karena file tersebut belum ada tidak otomatis berarti MCP server gagal.
6. Bila runtime native telah mendaftarkan shadcn MCP, inventarisasi konfigurasi aktif dan jangan membuat entri duplikat.

### Step 2 — Ant Design MCP

**Peran:** referensi API, dokumentasi, demo, token, semantic, dan changelog Ant Design.

Command:

```powershell
node scripts/stdio_mcp_client.mjs --server "npx -y @ant-design/cli mcp" --list --timeout 130000
```

Baseline tools yang tercatat pada sumber:

- `antd_list`
- `antd_info`
- `antd_doc`
- `antd_demo`
- `antd_token`
- `antd_design_md`
- `antd_semantic`
- `antd_changelog`

Ekspektasi baseline: serverInfo `antd`, sekitar 8 tools.

Verifikasi handshake dan jumlah tool aktual. Jangan menganggap versi/tool count baseline sebagai jaminan versi terkini.

### Step 3 — Browser MCP

**Peran:** menguji website di Chrome asli pada mesin user melalui ekstensi dan daemon lokal.

Browser MCP hanya dapat digunakan bila runtime memiliki jalur nyata ke mesin user atau berjalan langsung di mesin tersebut.

- Runtime lokal dengan shell/file access ke mesin user: periksa kit Browser MCP yang relevan.
- Runtime sandbox/cloud tanpa akses ke mesin user: tandai `NOT AVAILABLE`.
- Jangan mengklaim dapat mengontrol Chrome user dari cloud bila tidak ada jalur koneksi yang terverifikasi.
- Bila tidak tersedia, gunakan browser/runtime testing alternatif yang tersedia.

Profil teknis dan prosedur deploy terdapat pada Bagian 12.

### Step 4 — Skill Eksternal yang Disetujui

Daftar berikut adalah skill yang tercantum dalam bootstrap sumber dan memiliki standing approval untuk instalasi idempoten:

| # | Skill | Repository |
|---|---|---|
| 1 | UI UX Pro Max | https://github.com/nextlevelbuilder/ui-ux-pro-max-skill |
| 2 | Taste Skill | https://github.com/leonxlnx/taste-skill |
| 3 | Impeccable | https://github.com/pbakaus/impeccable |
| 4 | Emil Kowalski Skills | https://github.com/emilkowalski/skills |
| 5 | UX/UI Agent Skills | https://github.com/plugin87/ux-ui-agent-skills |
| 6 | UI Craft | https://github.com/educlopez/ui-craft |
| 7 | Apple Design Skill | https://github.com/dickwu/apple-design-skill |
| 8 | Vercel Agent Skills | https://github.com/vercel-labs/agent-skills |

Vercel Agent Skills memuat sub-skill yang relevan:

- Web Design Guidelines
- React Best Practices
- Composition Patterns

Prosedur setiap repository:

1. Periksa apakah skill sudah ada di lokasi yang benar.
2. Jika sudah tersedia dan valid, catat `ALREADY AVAILABLE`; jangan clone ulang.
3. Jika belum ada, clone shallow sesuai source:
   ```bash
   git clone --depth 1 <repository-url> skills/<skill-name>
   ```
4. Catat commit:
   ```bash
   git -C skills/<skill-name> rev-parse --short HEAD
   ```
5. Temukan entrypoint `SKILL.md` di lokasi repository.
6. Catat path entrypoint dan commit.
7. Bila runtime menggunakan native skill directory, salin hanya jika benar-benar dibutuhkan dan sesuai aturan scope platform.
8. Jangan menganggap clone berhasil berarti skill otomatis terdaftar/aktif.
9. Verifikasi skill dapat dibaca sebelum menyatakan siap digunakan.

### Step 5 — Inventaris Skill Native Platform

Periksa daftar skill yang tersedia pada runtime aktif.

Contoh skill platform yang pernah terverifikasi pada z.ai web di source baseline:

- `impeccable`
- `components`
- `ui-intergrity`
- `fullstack-dev`
- `docx`
- `pdf`
- `xlsx`
- `charts`
- `pptx`
- `github`

Daftar tersebut adalah catatan historis, bukan jaminan bahwa skill masih tersedia. Periksa nama, enabled state, dan instruksi aktual.

Pada ZCode desktop, skill dapat dikelola melalui plugin atau sistem skill native. Sesuaikan dengan mekanisme platform yang benar-benar aktif.

Jangan:

- mengklaim skill aktif karena namanya tercantum,
- menganggap skill platform web otomatis ada di desktop,
- menginstal ulang skill native tanpa alasan,
- memanggil skill yang disabled atau tidak dapat dibaca.

### Step 6 — Status, Pencatatan, dan Sambungan Project

Setelah setup:

1. Buat laporan status AUTO-SETUP.
2. Simpan/verifikasi helper script di workspace bila memang tersedia dan dibutuhkan.
3. Catat status aset, versi, commit, tanggal, dan evidence.
4. Periksa `PROJECT_STATE.md` pada project aktif jika dapat diakses dan memang relevan.
5. Sambungkan pengerjaan ke checkpoint terakhir.
6. Jangan melakukan audit ulang seluruh repository hanya karena chat baru dimulai.
7. Lanjutkan ke discovery/product workflow dari bootstrap utama.

Jangan membuat atau mengubah file project yang tidak relevan hanya untuk mencatat keberhasilan setup.

## 6. Format Laporan AUTO-SETUP

Gunakan laporan ringkas:

```text
AUTO-SETUP: SELESAI PENUH / SELESAI SEBAGIAN
- WORKSPACE       : <root> (platform <name>)
- SHADCN MCP      : READY / FAILED / NOT AVAILABLE
- ANTD MCP        : READY / FAILED / NOT AVAILABLE
- BROWSER MCP     : READY / PARTIAL / NOT AVAILABLE
- EXTERNAL SKILLS : <N> ready · <M> already available · <K> failed
- PLATFORM SKILLS : <verified relevant list>
- PROJECT STATE   : FRESH / STALE / NOT FOUND / NOT ACCESSIBLE
- EVIDENCE        : <commands, versions, tool counts, commits>
- BLOCKERS        : <none or reason>
- NEXT STEP       : <next project action>
```

Gunakan versi dan jumlah tool aktual dari pemeriksaan. Angka contoh tidak boleh disalin sebagai hasil aktual.

## 7. Failure Handling

- Kegagalan satu langkah tidak menghentikan langkah berikutnya.
- Maksimal dua percobaan untuk satu langkah setup, kecuali ada alasan teknis yang jelas untuk diagnosis tambahan.
- Unduhan pertama dari npm/npx dapat memerlukan timeout lebih lama.
- Catat error utama, bukan seluruh log yang mungkin berisi secret.
- Jika dependency tidak tersedia, gunakan alternatif runtime yang tersedia.
- Jika tidak ada akses shell/filesystem, jangan mengarang bahwa instalasi telah dilakukan.
- Jika capability tidak tersedia, beri status `NOT AVAILABLE` dan lanjutkan pekerjaan yang tidak bergantung padanya.

## 8. Persistence and Runtime Scope

### 8.1 Tiga Lapisan yang Berbeda

Jangan menyamakan:

1. **Chat folder/task group:** pengelompokan percakapan.
2. **Project workspace:** filesystem dan file tree project.
3. **Runtime configuration:** MCP, skill, plugin, process, dan service.

Folder chat yang sama tidak membuktikan bahwa file, konfigurasi MCP, skill, plugin, atau daemon mesin user ikut tersedia pada chat lain.

### 8.2 Persistence Matrix

| Asset | Persistence | Tindakan |
|---|---|---|
| Workspace files | Bergantung environment | Verifikasi file pada sesi aktif |
| Native/global/project skills | Bergantung platform dan scope | Inventarisasi enabled state dan entrypoint |
| MCP configuration | Bergantung user/workspace scope | Periksa config dan server status |
| Conversation history | Terpisah dari workspace | Pulihkan melalui project state dan evidence |
| Sandbox process | Tidak dijamin | Periksa proses aktual |
| External runtime dependencies | Bergantung environment | Verifikasi sebelum install |
| Browser MCP daemon | Berjalan pada mesin user | Verifikasi daemon dan pairing aktual |

### 8.3 Persistency Rules

- Jangan berasumsi file dari chat lama otomatis ter-mount.
- Jangan menganggap nama file yang disebut di riwayat sebagai bukti akses.
- Sebelum transfer, uji akses file yang diperlukan.
- Bila file sudah ada dan valid, jangan membuat duplikat.
- Bila environment berbeda, perlakukan sebagai migration/handoff, bukan kelanjutan otomatis.
- Jangan mengirim data rahasia melalui handoff pack.

## 9. ZCode-Specific Notes

Bagian ini hanya berlaku bila runtime memang ZCode.

### 9.1 Workspace dan Files

- Workspace ZCode memiliki file tree.
- Cari file project yang sudah tersedia sebelum mengunggah salinan.
- Gunakan referensi file yang didukung runtime, misalnya `@`, bila tersedia.
- File pada percakapan tanpa project dan file dalam project workspace adalah konteks berbeda.

### 9.2 Skills

- Periksa Settings → Skills.
- Verifikasi nama, source/scope, dan enabled state.
- User-level skill dapat berada pada `~/.zcode/skills/<skill-name>/SKILL.md`.
- Project-level skill hanya berlaku untuk workspace terkait.
- Gunakan mekanisme pemanggilan yang didukung runtime, misalnya `$skill-name`.
- Setelah membuat atau mengubah skill, refresh lalu verifikasi.

### 9.3 MCP

Konfigurasi yang tercatat:

- User-level: `~/.zcode/cli/config.json`
- Workspace-level: `<workspace>/.zcode/config.json`
- Compatibility path: `.agents/mcp.json`, bila didukung versi runtime.

Catatan:

- User scope dapat lintas workspace.
- Workspace scope hanya berlaku pada project terkait.
- Dalam perilaku precedence yang tercatat di source, konfigurasi `.zcode` pada scope sama mengalahkan `.agents`; keduanya tidak otomatis digabung.
- Jangan membuat dua konfigurasi dengan asumsi keduanya akan merge.
- Periksa konfigurasi project yang tidak dikenal karena MCP dapat menjalankan command, membaca/menulis file, atau mengakses jaringan.
- Verifikasi konfigurasi dengan dokumentasi/runtime versi aktif sebelum mengubahnya.

### 9.4 Plugin dan Remote Workspace

Plugin dapat menyediakan skill, command, subagent, hooks, atau MCP. Menonaktifkan plugin dapat menonaktifkan komponen yang disediakannya.

Remote workspace melalui SSH/WSL tidak otomatis memiliki konfigurasi lokal. Sync user-level skills/MCP/plugin hanya jika memang diperlukan dan didukung.

## 10. Skill Routing and Usage

### 10.1 Skill Availability Rule

Skill hanya boleh disebut digunakan bila:

- skill benar-benar tersedia dan instruksinya dibaca, atau
- tool/runtime mengonfirmasi skill dijalankan.

Jika hanya menggunakan prinsip publik, katakan bahwa yang digunakan adalah referensi/prinsipnya, bukan skill executable.

### 10.2 Skill Responsibility Map

| Skill | Kegunaan utama |
|---|---|
| UI UX Pro Max | Broad UI/UX intelligence, design-system planning |
| Taste Skill | Anti-generic UI, hierarchy, typography, spacing, motion |
| Impeccable | Design audit, critique, polish, UX copy, accessibility-aware refinement |
| Emil Kowalski Skills | Motion, interaction craft, mobile-native behavior |
| UX/UI Agent Skills | Tokens, components, accessibility, objective quality gates |
| UI Craft | Production-quality surfaces and implementation review |
| Apple Design Skill | Post-build craft/HIG review when applicable |
| Vercel Web Design Guidelines | Web UI implementation and accessibility review |
| Vercel React Best Practices | React/Next.js performance and implementation |
| Vercel Composition Patterns | React component API and composition architecture |

### 10.3 Targeted Skill Policy

Pilih skill sesuai tahap; jangan memuat seluruh repository ke setiap giliran.

Panduan maksimum:

- discovery/planning: 1–2 skill,
- active UI design/build: 1–3 skill,
- audit/polish: 1–3 skill,
- final review: Apple Design Skill bila tersedia dan relevan.

Gunakan sub-skill yang paling kecil dan tepat. Contoh UX/UI Agent Skills:

- `design-tokens`
- `design-component`
- `design-code`
- `a11y-audit`
- `apply-aesthetic`

### 10.4 External Skill Intake for New Skills

Skill baru di luar daftar AUTO-SETUP adalah kandidat, bukan dependency tepercaya otomatis.

Evaluasi:

1. Stage fit: apakah menutup capability gap yang nyata?
2. Source quality: utamakan repository author/vendor asli.
3. Maintenance: aktivitas, dokumentasi, issue, dan release.
4. Scope clarity: tanggung jawab harus jelas.
5. Determinism: checklist/script/validation lebih baik daripada instruksi samar bila keduanya tersedia.
6. Compatibility: cocok dengan framework/runtime/agent project.
7. Security: periksa script dan shell permissions.
8. License: sesuai kebutuhan project.
9. Context cost: tidak tumpang tindih dan terlalu berat.
10. Version reproducibility: catat commit/version jika berdampak pada hasil.

Tambahkan skill baru hanya bila:

- menutup kekurangan capability,
- meningkatkan verifikasi secara material,
- mengurangi waktu tanpa menurunkan mutu,
- memberi expertise framework yang benar-benar digunakan project.

Skill baru di luar daftar harus melalui approval client.

## 11. MCP Profiles: shadcn and Ant Design

### 11.1 shadcn MCP

Command baseline:

```bash
npx -y shadcn@latest mcp
```

Peran: mencari registry, melihat komponen/contoh, mengambil perintah add, dan checklist audit komponen.

Tool registry tertentu memerlukan `components.json` pada project target. Jalankan init hanya bila project memang memilih shadcn dan file tersebut belum tersedia.

### 11.2 Ant Design MCP

Command baseline:

```bash
npx -y @ant-design/cli mcp
```

Peran: dokumentasi/API/demo/token/semantic/changelog Ant Design.

### 11.3 stdio Lifecycle

Kedua server dicatat sebagai stdio JSON-RPC. Pada profil yang diuji, proses bersifat stateless per sesi:

1. jalankan driver ketika dibutuhkan,
2. lakukan handshake dan list/call,
3. gunakan hasil pada sesi tersebut,
4. proses berakhir setelah driver menghentikan child process,
5. hidupkan kembali saat dibutuhkan.

Jangan menganggap server berjalan permanen setelah pemeriksaan selesai.

### 11.4 Native MCP vs Generic Driver

Jika runtime menyediakan MCP native:

- inventarisasi server dan tools yang sudah terdaftar,
- gunakan koneksi native tersebut,
- hindari menjalankan driver duplikat tanpa alasan.

Gunakan driver stdio bila runtime tidak menyediakan koneksi native tetapi terminal dapat menjalankan server stdio.

## 12. Browser MCP Profile

### 12.1 Arsitektur

Profil Browser MCP yang tercatat di source:

- Browser: Chrome asli pada mesin user.
- Extension: browser extension.
- Server: npm package `@browsermcp/mcp`.
- Entry point: `dist/index.js`.
- Daemon: proses terpisah pada `ws://127.0.0.1:9010..9012`.
- Home daemon: `~/.browsermcp`.
- Windows home: `C:\Users\<user>\.browsermcp`.
- Metadata/log: `daemon.json`, `daemon.log`.

Port dan struktur ini adalah profil baseline yang pernah diuji, bukan jaminan semua versi menggunakan detail yang sama.

### 12.2 Tool and Command Model

Tool utama: `browser`, input berupa `{ commands }`.

Command yang tercatat:

- `goto <url>`
- `snapshot -i`
- `click --text <text>`
- `wait --text <text>`
- `fill --text <text> --value <value>`
- `press --text <text> --key <key>`
- `eval --code <js>`
- `get title`
- `get url`
- `get text`
- `console --errors`
- `requests --filter <filter>`
- `screenshot --full --path <path>`
- `browser list`
- `help`

Gunakan command sesuai versi aktual. Jangan menganggap daftar command di atas selalu lengkap.

### 12.3 Pairing

Pola pairing pada profil:

1. jalankan probe/daemon,
2. buka popup extension Chrome,
3. masukkan kode pairing 6 digit,
4. user klik **Allow**,
5. periksa koneksi melalui command sederhana.

Kode pairing berotasi antar sesi daemon. Jangan menyimpan atau menampilkan pairing secret dalam report/checkpoint.

Verifikasi awal yang ringan dapat menggunakan `browser list`. Bila extension belum terpasang/terhubung, tool seharusnya mengembalikan petunjuk pairing terbaru.

### 12.4 Status and Shutdown

Endpoint profil yang tercatat:

- status: `GET http://127.0.0.1:<port>/browsermcp/status`
- shutdown: `POST /shutdown?token=<token>`

Token shutdown berada di metadata daemon. Jangan memasukkannya ke chat, log, atau laporan. Hentikan daemon hanya sesuai kebutuhan dan setelah memastikan proses target yang benar.

### 12.5 Runtime Availability

- Runtime lokal yang sama dengan Chrome/mesin user: dapat menggunakan stdio server bila package, extension, pairing, dan akses tersedia.
- Runtime sandbox/cloud tanpa akses shell/file ke mesin user: `NOT AVAILABLE`.
- Browser automation alternatif milik runtime dapat digunakan bila Browser MCP tidak tersedia.

### 12.6 Kit Deployment Pattern

Struktur konseptual kit:

**Agent side**
- `push_bmcp_kit.py`: orchestrator deploy/run/status.
- `eval-uji.py`: evaluator assertions + console noise filter.
- `plan-rute-uji.json`: sequence test plan.
- `pull_out.py`: retrieve result without rerunning plan.

**User machine**
- `bmcp.mjs`: driver probe/plan.
- `package.json`: dependency declarations.
- `node_modules/@browsermcp/mcp`: installed dependency.

Prosedur dasar bila kit memang dibutuhkan:

1. pastikan target folder kit benar,
2. kirim file driver dan package metadata,
3. install dependency di folder kit,
4. jalankan probe,
5. minta user melakukan pairing melalui extension,
6. susun test plan dari routes dan expected behavior aktual,
7. jalankan plan,
8. tarik hasil,
9. evaluasi assertion berdasarkan source project dan acceptance criteria,
10. laporkan hasil beserta evidence.

Jangan deploy kit hanya karena profile ini tersedia. Gunakan ketika tugas membutuhkan browser asli dan runtime memiliki jalur akses ke mesin user.

### 12.7 Browser Test Plan Pattern

Pola per route:

```text
goto {base}<route>
snapshot -i
get title
console --errors
```

Tambahkan aksi interaksi sesuai acceptance criteria. Jangan menganggap title atau console bersih cukup untuk membuktikan seluruh flow berhasil.

Aserti harus diturunkan dari source, route, requirement, dan acceptance criteria project. Filter noise console hanya bila kategorinya dipahami dan tidak menyembunyikan error nyata.

## 13. Custom Tool Map

Peta tools berikut merupakan catatan dari source. Keberadaan file atau script harus diperiksa pada workspace sebelum dipakai.

| Tool/path | Peran |
|---|---|
| `scripts/stdio_mcp_client.mjs` | Driver MCP stdio generik untuk server seperti shadcn/antd |
| `scripts/bmcp-kit/push_bmcp_kit.py` | Orkestrasi deploy, run, dan status browser kit |
| `scripts/bmcp-kit/bmcp.mjs` | Driver Browser MCP di mesin user |
| `scripts/bmcp-kit/plan-rute-uji.json` | Contoh test plan per route |
| `scripts/bmcp-kit/eval-uji.py` | Evaluator assertion dan console output |
| `scripts/bmcp-kit/pull_out.py` | Mengambil output test dari mesin user |
| `scripts/bmcp-kit/diag_*.py` | Pola diagnosis lokal |
| `check_vhost.py` | Pemeriksaan virtual host |
| `start_laragon_stack.py` | Pola start Apache/MySQL |
| `scripts/build_release_*.py` | Pola release package |
| `gen_*_updater.py` | Pola generator updater |
| `verify_*_remote.py` | Pola verifikasi remote release |

Tool map bukan bukti bahwa file-file tersebut tersedia. Jangan membuat placeholder dengan nama sama lalu mengklaim tool telah siap.

## 14. Windows and Local Runtime Practices

### 14.1 File Transfer Integrity

Untuk membandingkan file di Windows:

```powershell
certutil -hashfile <path> SHA256
```

Bandingkan hash sumber dan tujuan setelah transfer. Hash yang sama membuktikan integritas byte file yang dibandingkan, bukan kebenaran atau keamanan isi file.

### 14.2 Diagnose Connection Refused

`ERR_CONNECTION_REFUSED` biasanya menunjukkan service/port tidak menerima koneksi, bukan otomatis kesalahan kode aplikasi.

Pemeriksaan:

```powershell
netstat -ano | findstr LISTENING
tasklist | findstr /I httpd
```

Pastikan proses/service memang berjalan dan telah bind ke port sebelum melakukan curl atau browser check.

### 14.3 PowerShell Helper Scripts

Untuk logika lebih dari satu baris:

1. tulis helper `.ps1`,
2. periksa kontennya,
3. jalankan melalui:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\helper.ps1
```

Hindari one-liner sangat panjang ketika variabel, quoting, atau pipe membuat output sulit dipercaya.

### 14.4 Non-ASCII and Encoding

Bila transfer teks non-ASCII berisiko mengalami encoding corruption:

- gunakan UTF-8 secara eksplisit,
- verifikasi karakter setelah ditulis,
- gunakan Base64 untuk transfer byte-exact bila transport mengalami kerusakan encoding,
- decode ke tujuan dan bandingkan hash bila file harus identik.

Jangan memakai Base64 sebagai alasan untuk mengabaikan validasi file hasil decode.

### 14.5 Long-Running Commands

Untuk proses instalasi atau build yang lama:

- arahkan output ke log yang aman,
- gunakan sentinel unik untuk menandai proses selesai,
- poll status sampai sentinel muncul,
- periksa exit code,
- jangan mengulang side-effecting command hanya karena tool call pertama belum mengembalikan hasil.

Jangan menyimpan token atau secret di log.

### 14.6 Test Assertions

Aserti harus berasal dari source route, page, API, dan requirement aktual. Jangan membuat expected text dari ingatan. Console PASS hanya setelah error nyata dipisahkan dari noise yang benar-benar dikenali.

## 15. AUTO-SETUP and New-Chat Checklist

Gunakan checklist ini secara adaptif:

```text
[ ] 1. Identifikasi platform, runtime, workspace root.
[ ] 2. Verifikasi shell, filesystem, Node/npm/npx/Git yang diperlukan.
[ ] 3. Verifikasi shadcn MCP atau inventaris native MCP.
[ ] 4. Verifikasi Ant Design MCP atau inventaris native MCP.
[ ] 5. Verifikasi Browser MCP jika tugas dan akses mesin user relevan.
[ ] 6. Periksa delapan repository skill yang disetujui; hindari duplikasi.
[ ] 7. Inventaris skill platform yang benar-benar tersedia/enabled.
[ ] 8. Catat version, commit, status, dan evidence.
[ ] 9. Laporkan AUTO-SETUP.
[ ] 10. Pulihkan PROJECT_STATE project aktif jika tersedia.
[ ] 11. Lanjutkan dari baseline dan checkpoint yang valid.
[ ] 12. Untuk tugas web/app, gunakan discovery, UI-system gate, WBS,
       engineering, dan QA modules yang relevan.
```

Larangan utama:

- jangan mencetak credential/token,
- jangan mengubah project tanpa batas izin yang sesuai,
- jangan menduplikasi MCP/skills,
- jangan mengklaim aset aktif tanpa evidence,
- jangan menjalankan semua skill pada setiap task,
- jangan melakukan audit ulang tanpa alasan,
- jangan mengirim file atau data project yang tidak terkait.

## 16. Continuity Without Re-Uploading

Bedakan kelanjutan pada workspace persisten yang sama dari perpindahan environment.

### 16.1 Account and Workspace Sama

Bila chat berikutnya memiliki akses ke storage project yang sama:

1. jangan unggah ulang bootstrap bila file masih bisa dibaca,
2. lakukan verifikasi AUTO-SETUP secara idempoten,
3. baca `PROJECT_STATE.md`,
4. baca baseline, decision, WBS, dan evidence yang relevan,
5. pastikan artefak masih aktual,
6. lanjutkan dari next action terakhir yang valid.

### 16.2 Chat Folder Sama, File Access Belum Terbukti

Folder chat yang sama bukan jaminan file dapat diakses.

1. coba akses file yang diperlukan,
2. verifikasi scope workspace dan permission,
3. bila gagal, catat capability yang hilang,
4. jangan berpura-pura launcher/platform dapat memperbaiki persistensi level platform,
5. jangan memindahkan file sebelum mengetahui artefak mana yang benar-benar tidak tersedia.

### 16.3 Workspace, Account, or Platform Berbeda

Perlakukan sebagai migrasi:

- identifikasi artefak yang memang belum tersedia di tujuan,
- siapkan handoff pack hanya untuk file yang relevan,
- gunakan urutan handoff yang didefinisikan pada `07_PROJECT_CONTINUITY.md`,
- jangan menyalin cache, generated bundles, duplikat, atau seluruh repository tanpa kebutuhan,
- jangan menyertakan API key, token, cookie, private key, atau raw authorization header.

### 16.4 Receiving AI Instruction

> Identifikasi platform dan workspace aktif. Baca bootstrap dan checkpoint project terbaru. Gunakan ulang file/configuration yang sudah tersedia. Jangan meminta client mengulang informasi yang sudah dikonfirmasi. Jalankan AUTO-SETUP secara idempoten, tanpa duplikasi. Jangan mengulang full audit kecuali evidence stale, kontradiktif, berisiko, atau tidak cukup. Bila artefak tidak dapat diakses, diagnosis scope, path, permission, dan runtime sebelum mengusulkan transfer.

## 17. Appendix A — Generic stdio MCP Driver

Gunakan driver berikut hanya bila runtime tidak memiliki MCP native yang sesuai dan dapat menjalankan server stdio. Pastikan package/server tepercaya dan command-nya telah ditinjau.

Simpan sebagai `scripts/stdio_mcp_client.mjs` hanya bila memang dibutuhkan dan belum tersedia.

```javascript
#!/usr/bin/env node
/**
 * Generic MCP stdio driver for servers such as shadcn and Ant Design.
 * Requires Node.js 18+.
 *
 * Examples:
 *   node stdio_mcp_client.mjs --server "npx -y shadcn@latest mcp" --list
 *   node stdio_mcp_client.mjs --server "npx -y @ant-design/cli mcp" --list
 *   node stdio_mcp_client.mjs --server "npx -y shadcn@latest mcp" \
 *     --call <tool> --args '{"foo":"bar"}'
 */
import { spawn } from "node:child_process";

const args = process.argv.slice(2);

function getOpt(name, fallback = null) {
  const index = args.indexOf(name);
  return index >= 0 && args[index + 1] ? args[index + 1] : fallback;
}

const serverCommand = getOpt("--server");
const listTools = args.includes("--list");
const toolName = getOpt("--call");
const rawToolArgs = getOpt("--args", "{}");
const timeoutMs = Number.parseInt(getOpt("--timeout", "90000"), 10);

if (!serverCommand || (!listTools && !toolName)) {
  console.error(
    'Usage: --server "<command>" --list OR --server "<command>" --call <tool> --args \'{"json":true}\''
  );
  process.exit(1);
}

if (!Number.isFinite(timeoutMs) || timeoutMs < 1000) {
  console.error("[FATAL] Invalid timeout");
  process.exit(1);
}

const parts = serverCommand.match(/(?:[^\s"]+|"[^"]*")+/g) ?? [];
const executable = parts.shift()?.replace(/^"|"$/g, "");
const commandArgs = parts.map((part) => part.replace(/^"|"$/g, ""));

if (!executable) {
  console.error("[FATAL] Empty server command");
  process.exit(1);
}

const child = spawn(executable, commandArgs, {
  stdio: ["pipe", "pipe", "inherit"],
  windowsHide: true,
});

let buffer = "";
let nextId = 0;
const pending = new Map();

function send(message) {
  child.stdin.write(JSON.stringify(message) + "\n");
}

function request(method, params) {
  return new Promise((resolve, reject) => {
    const id = ++nextId;
    const timer = setTimeout(() => {
      pending.delete(id);
      reject(new Error(`Timeout waiting for ${method} (id=${id})`));
    }, timeoutMs);

    pending.set(id, { resolve, reject, timer });
    send({ jsonrpc: "2.0", id, method, params });
  });
}

child.stdout.on("data", (chunk) => {
  buffer += chunk.toString("utf8");
  let index;

  while ((index = buffer.indexOf("\n")) >= 0) {
    const line = buffer.slice(0, index).trim();
    buffer = buffer.slice(index + 1);
    if (!line) continue;

    let message;
    try {
      message = JSON.parse(line);
    } catch {
      continue;
    }

    if (message.id !== undefined && pending.has(message.id)) {
      const item = pending.get(message.id);
      clearTimeout(item.timer);
      pending.delete(message.id);

      if (message.error) item.reject(new Error(JSON.stringify(message.error)));
      else item.resolve(message.result);
    }
  }
});

child.on("exit", (code) => {
  for (const item of pending.values()) {
    clearTimeout(item.timer);
    item.reject(new Error(`Server exited with code ${code}`));
  }
  pending.clear();
});

async function main() {
  try {
    const init = await request("initialize", {
      protocolVersion: "2025-06-18",
      capabilities: {},
      clientInfo: { name: "stdio-mcp-driver", version: "1.0.0" },
    });

    console.error(`[init] server: ${JSON.stringify(init?.serverInfo)}`);
    send({ jsonrpc: "2.0", method: "notifications/initialized" });

    if (listTools) {
      const result = await request("tools/list", {});
      const tools = result?.tools ?? [];
      console.log(`total tools: ${tools.length}`);

      for (const tool of tools) {
        const description = (tool.description ?? "")
          .replace(/\n/g, " ")
          .slice(0, 120);
        console.log(`- ${tool.name}: ${description}`);
      }
    } else {
      let parsedArgs;
      try {
        parsedArgs = JSON.parse(rawToolArgs);
      } catch {
        throw new Error("--args is not valid JSON");
      }

      const result = await request("tools/call", {
        name: toolName,
        arguments: parsedArgs,
      });
      console.log(JSON.stringify(result, null, 2));
    }

    child.kill();
    process.exit(0);
  } catch (error) {
    console.error(`[FAILED] ${error.message}`);
    child.kill();
    process.exit(2);
  }
}

child.on("error", (error) => {
  console.error(`[SPAWN FAILED] ${error.message}`);
  process.exitCode = 2;
});

main();
```

Catatan:

- Driver ini adalah helper generik, bukan pengganti penuh runtime MCP.
- Pemecahan command menjadi executable/args memiliki batas untuk quoting kompleks. Untuk command kompleks gunakan konfigurasi argumen eksplisit dan terstruktur, bukan shell parsing yang lebih permisif.
- Periksa apakah protocol version masih didukung server aktual. Perbarui berdasarkan dokumentasi server, bukan menebak.
- Gunakan hanya tool yang memang diperlukan. Jangan jalankan tool mutasi hanya untuk mengetes koneksi.

## 18. Appendix B — Browser MCP Test Plan Example

Template berikut bersifat ilustratif. Isi route dan assertions dari source project aktual.

```json
{
  "name": "browser-smoke-test",
  "base": "{base}",
  "steps": [
    {
      "name": "open-home",
      "commands": [
        "goto {base}/",
        "snapshot -i",
        "get title",
        "console --errors"
      ]
    }
  ]
}
```

Satu route yang terbuka bukan bukti seluruh aplikasi lolos QA. Hubungkan setiap step dengan acceptance criteria dan test report.

## 19. Final Operating Rules

1. AUTO-SETUP bersifat idempoten dan terbatas pada daftar yang disetujui.
2. Runtime aktual selalu lebih tinggi daripada catatan historis.
3. Tool/skill harus diverifikasi sebelum diklaim aktif.
4. Gagal satu capability tidak boleh menghentikan pekerjaan yang tidak bergantung padanya.
5. Browser MCP membutuhkan jalur akses nyata ke mesin user.
6. Secret tidak boleh masuk ke log, screenshot, status report, atau handoff.
7. Skill dipilih secara selektif sesuai tahap.
8. Skill baru di luar daftar membutuhkan intake dan approval.
9. Jangan mengulang audit atau setup tanpa alasan.
10. Project state dan handoff harus mencatat evidence, bukan klaim.

---

## Source Mapping

Modul ini memisahkan materi dari bootstrap utama:

- Section A.0–A.6: AUTO-SETUP, standing approval, keamanan, persistence, ZCode scope, dan status reporting.
- Section 5.1–5.4: skill sources, responsibilities, selection policy, external skill intake.
- Appendix I: profil MCP shadcn, Ant Design, dan Browser MCP.
- Appendix J: peta custom tools.
- Appendix K: pola Windows dan mesin lokal.
- Appendix L: generic stdio MCP driver.
- Appendix M: Browser MCP kit deployment dan test flow.
- Appendix N: startup checklist.
- Appendix O: continuity without re-uploading.

**Catatan source:** front matter pada file sumber menyatakan versi internal 1.3.0 dan tanggal 2026-11-10 walaupun nama file yang digunakan masih mengandung `v1.2`. Referensi versi pada modul ini mengikuti metadata internal sumber.
