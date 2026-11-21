---
name: auto-setup-tools
version: "1.0.0"
language: "id-ID"
purpose: "Panduan verifikasi runtime, AUTO-SETUP aset MCP dan skill yang tercantum, penggunaan tools, keamanan, serta kesinambungan workspace."
---

# 06 — AUTO-SETUP, MCP, Tools & Skills

Dokumen ini memisahkan seluruh prosedur setup runtime dan aset pendukung dari bootstrap utama. Tujuannya adalah membuat AI mampu memeriksa kemampuan environment, menyiapkan aset yang memang tercantum, dan menggunakan tools secara aman tanpa mengarang ketersediaan.

> **Prinsip utama:** verifikasi dahulu, gunakan ulang yang sudah ada, jangan membuat duplikasi, jangan mengklaim tool aktif tanpa bukti, dan jangan membiarkan kegagalan satu aset menghentikan pekerjaan lain.

## 1. Ruang Lingkup

Modul ini mencakup:

- identifikasi runtime dan workspace,
- AUTO-SETUP MCP shadcn dan Ant Design,
- AUTO-SETUP Codebase Memory MCP (graph retrieval proyek),
- pemeriksaan Browser MCP pada mesin user,
- instalasi dan routing skill eksternal yang tercantum, termasuk Anti Slop,
- inventaris skill native platform,
- keamanan instalasi dan konfigurasi,
- model persistensi workspace/runtime,
- profil MCP dan custom tools,
- prosedur Windows dan PowerShell,
- continuity antar-chat/workspace.

Modul ini **tidak** menganggap semua kemampuan tersedia di setiap platform. Ketersediaan harus diverifikasi pada runtime yang sedang digunakan.

## 2. Persetujuan Instalasi dan Perubahan Runtime

Dokumen ini tidak memberikan persetujuan otomatis untuk memasang, menghapus, atau mengubah aset pada perangkat maupun akun pengguna.

Sebelum instalasi atau perubahan runtime:
1. Periksa apakah capability sudah tersedia dan dapat digunakan.
2. Jelaskan aset, sumber, versi, lokasi, serta perubahan yang akan dilakukan.
3. Jelaskan efek samping dan kebutuhan restart atau perubahan konfigurasi.
4. Minta persetujuan eksplisit pengguna sebelum melakukan instalasi atau perubahan.
5. Gunakan kembali aset yang sudah tersedia dan hindari duplikasi.
6. Persetujuan untuk satu aset tidak berlaku otomatis untuk aset atau tindakan lain.

Pemeriksaan read-only terhadap runtime dan inventarisasi capability boleh dilakukan tanpa persetujuan instalasi, selama tidak mengubah kondisi sistem.

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

Operasi destruktif berisiko tinggi membutuhkan persetujuan spesifik per kasus. Persetujuan instalasi tidak menggantikan persetujuan spesifik untuk operasi tersebut.

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

1. Identifikasi platform/runtime aktif: aplikasi desktop AI, web chat, CLI, sandbox, remote workspace, atau lainnya.
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

### 4.2 Status Terverifikasi (Mesin Penulis, BUKAN Mesin Anda)

> **Baca ini sebelum membaca tabel.**
>
> Tabel di bawah mencatat hasil pengujian pada **satu mesin tertentu milik penulis dokumen ini**, bukan pada mesin yang sedang Anda gunakan. Status `READY` di tabel ini **tidak berlaku** untuk runtime Anda sampai Anda memverifikasinya sendiri.
>
> Jika Anda melakukan onboarding di platform lain (web chat, sandbox, mesin berbeda), status jujur untuk **seluruh baris** tabel ini pada runtime Anda adalah `NOT VERIFIED`.

**Identitas mesin penulis:**

| Atribut | Nilai |
|---|---|
| Jenis | Satu laptop pribadi, Windows 11, lokal (bukan sandbox/cloud) |
| Runtime AI | CLI agent dengan akses shell dan filesystem lokal |
| Scope klaim | Hanya berlaku untuk mesin itu, pada tanggal yang tercatat |

Hostname, UUID, dan path lengkap mesin sengaja **tidak** dicantumkan. Identitas perangkat tidak membantu AI lain bekerja, dan tidak boleh disebar bersama dokumen.

**Artinya bagi Anda:**

| Situasi Anda | Status yang boleh Anda klaim |
|---|---|
| Mesin yang sama, konfigurasi tidak berubah | Boleh mengandalkan tabel, tetap catat tanggal akses |
| Mesin berbeda, platform sama | `NOT VERIFIED` sampai Anda memverifikasi sendiri |
| Web chat atau sandbox tanpa akses filesystem | `NOT AVAILABLE` untuk semua aset yang butuh filesystem |
| Versi tool berbeda dari yang tercatat | `NOT VERIFIED` sampai diuji ulang |

Membaca file `scripts/stdio_mcp_client.mjs` dari GitHub **tidak** membuktikan file itu ada di filesystem Anda. Melihat dokumentasi Browser MCP **tidak** membuktikan Chrome dan daemon Anda terpasang.

Tabel ini mencatat status yang **sudah dibuktikan pada runtime nyata**, agar sesi pada mesin yang sama tidak memverifikasi ulang dari nol. Periksa ulang hanya bila mesin, versi, atau konfigurasi berubah.

| Aset | Status | Bukti | Diperiksa |
|---|---|---|---|
| Codebase Memory MCP | `READY` | binary `codebase-memory-mcp` v0.12.1 di PATH; 19 tool MCP aktif (`mcp__codebase_memory__*`); `list_projects` mengembalikan 4 project terindeks | 21 Nov 2026 |
| Anti Slop | `READY` | 6 folder skill di `~/.config/.../skills/` (`antislop`, `-ui`, `-human`, `-code`, `-copywriting`, `-layoutmobile`) | 21 Nov 2026 |
| `scripts/stdio_mcp_client.mjs` | `READY` | handshake ke `@ant-design/cli mcp` berhasil: serverInfo `antd` v6.7.1, 8 tools | 21 Nov 2026 |
| shadcn MCP | `NOT VERIFIED` | belum dijalankan pada mesin ini | — |
| Ant Design MCP | `PARTIAL` | terverifikasi lewat driver stdio, belum didaftarkan sebagai MCP native | 21 Nov 2026 |
| Browser MCP | `READY` | tool `browser` tersedia pada runtime | 21 Nov 2026 |
| Anti Slop mode | `NOT SET` | `~/.config/antislop/settings.json` belum ada; mode belum disimpan | 21 Nov 2026 |

Cara membaca tabel ini:

- `READY` berarti sudah dijalankan dan hasilnya teramati, bukan hanya tercantum di dokumentasi.
- Entri dengan tanggal `—` berarti **belum pernah diuji**. Jangan menaikkan statusnya tanpa bukti baru.
- Status di sini menggambarkan mesin referensi. Runtime lain harus diverifikasi sendiri.
- Bila versi berubah, status kembali ke `NOT VERIFIED` sampai diuji ulang.

Jangan mengubah tabel ini menjadi klaim. Setiap kenaikan status membutuhkan satu baris bukti yang bisa diperiksa ulang.

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

1. Pastikan driver tersedia di `scripts/stdio_mcp_client.mjs`. Bila file itu tidak ada, salin dari `references/STDIO_MCP_DRIVER.md` bagian A.
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

### Step 3 — Codebase Memory MCP (CBM)

**Peran:** MCP server lokal yang membangun knowledge graph persisten dari source code, dokumentasi, konfigurasi, dan infrastructure project. Dipakai untuk menemukan simbol, menelusuri caller/dependency, menilai blast radius perubahan, dan mengambil snippet source secara terarah.

**Hubungan dengan Graphify:** keputusan alat retrieval mana yang dipakai ditentukan oleh `03_ENGINEERING_STANDARDS.md` bagian 12, dan dicatat sebagai satu baris `RETRIEVAL:` di `DECISIONS.md`. Modul ini tidak memutuskan ulang; ia hanya memasang CBM dan mencatat statusnya. Jangan menjalankan Graphify dan CBM sebagai sumber kebenaran ganda.

**Sumber dan lisensi:** https://github.com/DeusData/codebase-memory-mcp (MIT). Runtime native, tanpa Docker dan tanpa API key. Index disimpan lokal pada mesin user.

**Status pada mesin referensi:** `READY`, v0.12.1, 4 project terindeks. Lihat tabel status di bagian 4.2. Bila mesin atau versi berbeda, verifikasi ulang sebelum mengandalkan status itu.

**Windows (PowerShell):**

```powershell
Invoke-WebRequest -Uri https://raw.githubusercontent.com/DeusData/codebase-memory-mcp/main/install.ps1 -OutFile install.ps1
notepad install.ps1            # periksa isi script sebelum eksekusi
Unblock-File .\install.ps1
.\install.ps1                  # tambahkan --skip-config bila hanya ingin binary
```

Bila muncul execution policy error:

```powershell
Set-ExecutionPolicy -Scope Process Bypass
# atau
powershell -ExecutionPolicy Bypass -File .\install.ps1
```

**macOS / Linux:**

```bash
curl -fsSL https://raw.githubusercontent.com/DeusData/codebase-memory-mcp/main/install.sh | bash
```

Unduh-lalu-jalankan lebih diutamakan daripada `curl | bash` bila koneksi atau isi script belum diperiksa.

**Verifikasi:**

1. Periksa binary tersedia dan dapat dipanggil dari shell aktif:
   ```powershell
   codebase-memory-mcp --version
   ```
2. Inventarisasi konfigurasi MCP yang ditulis installer (pada Hermes: `$HERMES_HOME/config.yaml`). Installer menulis konfigurasi untuk client yang terdeteksi; jangan membuat entri duplikat.
3. Restart runtime/agent sebelum mengklaim tool aktif.
4. Index satu project uji secara terarah, lalu periksa status index dan coverage-nya.
5. Tandai `READY` hanya bila koneksi MCP benar-benar menjawab dan index project uji terbentuk.

**Catatan penting:**

- Belum ada index berarti belum ada graph. `index_status` tanpa project terindeks bukan bukti CBM siap dipakai.
- Index bersifat lokal dan persisten, tetapi tidak otomatis segar. Tandai `STALE` setelah perubahan struktural besar dan index ulang sebelum menyimpulkan blast radius.
- Coverage index bisa parsial (file dilewati, bahasa belum terdukung, PDF tanpa text layer). Periksa coverage sebelum menyatakan sesuatu "tidak ada di codebase".
- Hasil graph bukan bukti bahwa UI, flow, authorization, atau business rule benar. Tetap verifikasi dengan source, test, dan runtime.
- Index hanya pada project yang izin aksesnya sudah diberikan.

### Step 4 — Browser MCP

**Peran:** menguji website di Chrome asli pada mesin user melalui ekstensi dan daemon lokal.

Browser MCP hanya dapat digunakan bila runtime memiliki jalur nyata ke mesin user atau berjalan langsung di mesin tersebut.

- Runtime lokal dengan shell/file access ke mesin user: periksa kit Browser MCP yang relevan.
- Runtime sandbox/cloud tanpa akses ke mesin user: tandai `NOT AVAILABLE`.
- Jangan mengklaim dapat mengontrol Chrome user dari cloud bila tidak ada jalur koneksi yang terverifikasi.
- Bila tidak tersedia, gunakan browser/runtime testing alternatif yang tersedia.

Profil teknis dan prosedur deploy ada di `references/BROWSER_MCP_PROFILE.md`.

### Step 5 — Skill Eksternal yang Memerlukan Persetujuan

Daftar kandidat skill eksternal **tidak diulang di sini**. Sumber tunggalnya adalah `02_UI_UX_SYSTEM.md` bagian 5.1. Modul ini hanya mengatur cara memasang, memverifikasi, dan mencatat statusnya.

Pencantuman dalam daftar kandidat bukan persetujuan untuk mengunduh, memasang, atau mengaktifkannya. Setiap instalasi memerlukan persetujuan eksplisit pengguna setelah aset, sumber, versi, lokasi, dan dampaknya dijelaskan.

Beberapa kandidat adalah paket berisi banyak skill (Vercel Agent Skills, Anti Slop). Saat melaporkan status, hitung paket yang terpasang, bukan jumlah file `SKILL.md`.

### Prosedur khusus Anti Slop

Anti Slop bukan satu skill tunggal, melainkan satu paket berisi enam skill yang saling melengkapi. Paket ini adalah **filter**, bukan sumber arah desain: ia menolak pola UI/teks/kode generik, tetapi tidak menentukan warna, font, atau layout. Arah visual tetap berasal dari `DESIGN.md` atau brief client.

Daftar skill dan perannya:

| Skill | Cakupan | Dipakai untuk |
|---|---|---|
| `antislop` | Filter inti: 40 rules, tier, Delivery Gate, liveliness | Selalu dimuat |
| `antislop-ui` | Layout, color, component, decoration, motion, struktur, performance | Pekerjaan UI dan visual |
| `antislop-copywriting` | Headline, CTA, tone, fake stats, anti-AI writing | Copy dan teks |
| `antislop-human` | Contrast, keyboard, focus, states, affordance | Accessibility |
| `antislop-layoutmobile` | Reflow, breakpoint, grid, overflow, tap target | Responsive |
| `antislop-code` | Komentar kode bergaya AI: mana yang dibuang, mana yang disimpan | Kebersihan komentar |

Sumber dan lisensi: https://github.com/miqdadbadjuber/anti-slop (MIT). Nama paket npm adalah `antislop-ai`; `npx antislop` adalah package lain dan bukan proyek ini.

Pilihan instalasi, pilih **satu**:

1. **Installer** (paling lengkap):
   ```bash
   npx antislop-ai
   ```
   Installer menanyakan skill yang dipilih, scope (project atau global), dan agent yang dipakai, lalu menulis pointer agar aturan termuat setiap sesi.

2. **Skills directory** (hanya menyalin folder skill):
   ```bash
   npx skills add miqdadbadjuber/anti-slop
   ```

3. **Plugin** untuk agent yang mendukung plugin, misalnya Claude Code, Codex, Cursor, Cline, Kimi Code, Antigravity.

4. **Single file** bila runtime tidak mendukung skill sama sekali:
   ```bash
   curl -o antislop.md https://raw.githubusercontent.com/miqdadbadjuber/anti-slop/main/antislop.md
   ```

Verifikasi:

1. Periksa folder skill benar-benar terbentuk dan `SKILL.md` dapat dibaca.
2. Periksa pointer aturan termuat pada giliran berikutnya, bukan hanya pada sesi instalasi.
3. Skill yang baru dipasang belum terdaftar pada sesi yang sedang berjalan; restart/reload runtime sebelum mengklaim aktif.
4. Catat versi/npm release dan tanggal pasang.

Mode kerja Anti Slop ditanyakan sekali di awal sesi: `during` (diterapkan sambil menulis) atau `after` (audit setelah selesai).

**Default Starter Pack: `during`.** Alasannya praktis: `during` mencegah slop muncul dan menghindari pekerjaan ulang, sementara `after` selalu berarti satu putaran audit tambahan. Simpan sebagai preferensi agar tidak ditanyakan berulang di setiap sesi:

```bash
npx antislop-ai --mode during
```

Bila preferensi belum tersimpan, tanyakan sekali, gunakan `during` sebagai usulan, lalu tawarkan menyimpannya. Jangan mengubah preferensi tersimpan pengguna tanpa instruksi eksplisit.

Batasan yang wajib dijaga:

- Anti Slop tidak boleh dipakai sebagai alasan menolak permintaan client. Ia menyaring mutu, bukan menggantikan keputusan produk.
- Tanpa `DESIGN.md`, hasilnya wajib disebut sebagai draft tanpa arah, bukan deliverable final.
- Konflik antara brief client dan aturan Anti Slop harus disebutkan beserta nama aturannya, lalu diputuskan client, bukan diputuskan sendiri.

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

### Step 6 — Inventaris Skill Native Platform

Periksa daftar skill yang tersedia pada runtime aktif.

Daftar skill bersifat spesifik terhadap platform dan dapat berubah. Jangan memakai daftar historis dari platform lain sebagai bukti ketersediaan. Periksa nama, status enabled, lokasi, dan instruksi aktual melalui mekanisme yang didukung runtime saat ini.

Ikuti mekanisme pengelolaan skill native, plugin, atau ekstensi yang benar-benar tersedia pada platform aktif. Jangan mengasumsikan mekanisme satu platform berlaku pada platform lain.
Jangan:

- mengklaim skill aktif karena namanya tercantum,
- menganggap skill platform web otomatis ada di desktop,
- menginstal ulang skill native tanpa alasan,
- memanggil skill yang disabled atau tidak dapat dibaca.

### Step 7 — Status, Pencatatan, dan Sambungan Project

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
- CBM MCP         : READY / PARTIAL / NOT AVAILABLE
- INDEX COVERAGE  : <project terindeks, status freshness, area terlewat>
- BROWSER MCP     : READY / PARTIAL / NOT AVAILABLE
- EXTERNAL SKILLS : <N> ready · <M> already available · <K> failed
- ANTI SLOP       : READY / PARTIAL / NOT AVAILABLE (<mode: during/after>)
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
| Anti Slop (core) | Filter pola AI generik: rules, tier, Delivery Gate, liveliness |
| Anti Slop UI | Layout, komponen, dekorasi, motion, struktur, performance |
| Anti Slop Copywriting | Headline, CTA, tone, fake stats, kebersihan markdown |
| Anti Slop Human | Contrast, keyboard, focus, states, affordance interaksi |
| Anti Slop Layout Mobile | Reflow lintas lebar layar, breakpoint, grid, overflow, tap target |
| Anti Slop Code | Kebersihan komentar kode bergaya AI |

### 10.3 Targeted Skill Policy

Pilih skill sesuai tahap; jangan memuat seluruh repository ke setiap giliran.

Panduan maksimum:

- discovery/planning: 1–2 skill,
- active UI design/build: 1–3 skill,
- audit/polish: 1–3 skill,
- final review: Apple Design Skill bila tersedia dan relevan,
- arah dan mutu visual: Anti Slop core selalu aktif sebagai filter, ditambah maksimal satu skill Anti Slop yang paling relevan (`antislop-ui`, `antislop-copywriting`, `antislop-human`, `antislop-layoutmobile`, atau `antislop-code`).

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

## 11. MCP Profiles: shadcn, Ant Design, and Codebase Memory

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

### 11.3 Codebase Memory MCP

Instalasi:

```powershell
# Windows
Invoke-WebRequest -Uri https://raw.githubusercontent.com/DeusData/codebase-memory-mcp/main/install.ps1 -OutFile install.ps1
Unblock-File .\install.ps1
.\install.ps1
```

```bash
# macOS / Linux
curl -fsSL https://raw.githubusercontent.com/DeusData/codebase-memory-mcp/main/install.sh | bash
```

Karakteristik profil:

- Runtime native dalam C, tanpa Docker dan tanpa language runtime tambahan.
- Index dan graph disimpan lokal pada mesin user.
- Tidak memerlukan API key atau service hosted.
- Koneksi ke agent melalui MCP; installer menulis konfigurasi untuk client yang terdeteksi, termasuk `$HERMES_HOME/config.yaml` pada Hermes.
- Tersedia juga akses CLI langsung dan varian paket npm/PyPI/Go.

Baseline tools yang tercatat di sumber (17 tools). Nama dan jumlah dapat berubah antar rilis; verifikasi pada runtime aktif:

- `index_repository`
- `search_graph`
- `query_graph`
- `trace_path`
- `get_code_snippet`
- `get_file_outline`
- `get_graph_schema`
- `compare_graphs`
- `get_architecture`
- `search_code`
- `list_projects`
- `delete_project`
- `index_status`
- `check_index_coverage`
- `detect_changes`
- `manage_adr`
- `ingest_traces`

Alur kerja yang disarankan:

1. index project (lokal, izin harus sudah ada),
2. periksa `index_status` sebelum mengandalkan hasil,
3. cari simbol atau area terdampak dengan query terarah,
4. ambil snippet atau outline, lalu buka source asli untuk verifikasi,
5. periksa `check_index_coverage` sebelum menyatakan sesuatu tidak ada,
6. index ulang bila perubahan struktural membuat graph stale.

`delete_project` adalah operasi destruktif terhadap index. Perlakukan seperti operasi sensitif pada bagian 3.2: sebutkan target dan minta persetujuan spesifik. Index dapat dibangun ulang, tetapi data proyek di dalamnya tidak boleh hilang tanpa persetujuan.

### 11.4 stdio Lifecycle

Server shadcn dan Ant Design dicatat sebagai stdio JSON-RPC. Pada profil yang diuji, keduanya bersifat stateless per sesi. Codebase Memory berbeda: binary-nya persisten dan index disimpan di disk, sehingga sesi baru dapat melanjutkan graph yang sudah ada.

1. jalankan driver ketika dibutuhkan,
2. lakukan handshake dan list/call,
3. gunakan hasil pada sesi tersebut,
4. proses berakhir setelah driver menghentikan child process,
5. hidupkan kembali saat dibutuhkan.

Jangan menganggap server berjalan permanen setelah pemeriksaan selesai.

### 11.5 Native MCP vs Generic Driver

Jika runtime menyediakan MCP native:

- inventarisasi server dan tools yang sudah terdaftar,
- gunakan koneksi native tersebut,
- hindari menjalankan driver duplikat tanpa alasan.

Gunakan driver stdio bila runtime tidak menyediakan koneksi native tetapi terminal dapat menjalankan server stdio.

Codebase Memory MCP adalah binary native, bukan server `npx` per sesi. Jalankan melalui konfigurasi MCP yang ditulis installer, bukan melalui driver generik di `references/STDIO_MCP_DRIVER.md` bagian A, kecuali runtime benar-benar hanya mendukung stdio manual.

## 12. Browser MCP Profile

Detail teknis Browser MCP (arsitektur, tool, pairing, status, kit deployment) dipindahkan ke file referensi:

**`references/BROWSER_MCP_PROFILE.md`**

Baca file itu **hanya** ketika tugas membutuhkan pengujian di Chrome asli pada mesin pengguna.

Ringkasan yang cukup untuk sebagian besar pekerjaan:

- Browser MCP mengendalikan Chrome pengguna melalui extension dan daemon lokal pada `ws://127.0.0.1:9010..9012`.
- Butuh jalur nyata ke mesin pengguna. Runtime sandbox atau cloud tanpa akses ke mesin itu berstatus `NOT AVAILABLE`.
- Pairing dilakukan lewat popup extension dengan kode 6 digit. Kode pairing berotasi dan tidak boleh masuk laporan.
- Setelah sesi selesai, daemon tidak dijamin tetap hidup. Verifikasi sebelum mengandalkan.

---


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
[ ] 5. Verifikasi Codebase Memory MCP; pastikan index status dan coverage
       dibaca sebelum mengandalkan hasil graph.
[ ] 6. Verifikasi Browser MCP jika tugas dan akses mesin user relevan.
[ ] 7. Periksa kandidat skill sesuai daftar sumber tunggal di `02_UI_UX_SYSTEM.md` bagian 5.1. Semua kandidat, bukan skill yang sudah disetujui otomatis. Jangan memasang tanpa persetujuan eksplisit.
[ ] 8. Verifikasi Anti Slop terpasang dan mode (during/after) diketahui.
[ ] 9. Inventaris skill platform yang benar-benar tersedia/enabled.
[ ] 10. Catat version, commit, status, dan evidence.
[ ] 11. Laporkan AUTO-SETUP.
[ ] 12. Pulihkan PROJECT_STATE project aktif jika tersedia.
[ ] 13. Lanjutkan dari baseline dan checkpoint yang valid.
[ ] 14. Untuk tugas web/app, gunakan discovery, UI-system gate, WBS,
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

## 17. Referensi Pendukung

Dua bagian rujukan dipisahkan dari modul ini agar tetap ringkas saat dibaca di awal sesi:

| File | Isi | Baca ketika |
|---|---|---|
| `references/STDIO_MCP_DRIVER.md` | Salinan driver MCP stdio generik dan contoh browser test plan | Memverifikasi server MCP lewat terminal, atau menyusun test plan browser |
| `references/BROWSER_MCP_PROFILE.md` | Profil teknis Browser MCP | Tugas membutuhkan Chrome asli pada mesin pengguna |

Driver yang dibahas di referensi tersebut **sudah ada sebagai file nyata**: `scripts/stdio_mcp_client.mjs`. Jangan membuat ulang. Bagian kode di referensi adalah salinan untuk audit isi.

Kedua command verifikasi pada Step 1 dan Step 2 memakai driver itu dan sudah diuji jalan pada Windows. Catatan kompatibilitasnya ada di `references/STDIO_MCP_DRIVER.md` bagian A.

---


## 18. Final Operating Rules

1. AUTO-SETUP bersifat idempoten dan terbatas pada daftar yang disetujui.
2. Runtime aktual selalu lebih tinggi daripada catatan historis.
3. Tool/skill harus diverifikasi sebelum diklaim aktif.
4. Gagal satu capability tidak boleh menghentikan pekerjaan yang tidak bergantung padanya.
5. Browser MCP membutuhkan jalur akses nyata ke mesin user.
6. Index Codebase Memory bersifat lokal namun tidak otomatis segar; periksa status dan coverage sebelum mengandalkan hasil graph.
7. Secret tidak boleh masuk ke log, screenshot, status report, atau handoff.
8. Skill dipilih secara selektif sesuai tahap. Anti Slop menyaring mutu, bukan menggantikan `DESIGN.md`.
9. Skill baru di luar daftar membutuhkan intake dan approval.
10. Jangan mengulang audit atau setup tanpa alasan.
11. Project state dan handoff harus mencatat evidence, bukan klaim.
