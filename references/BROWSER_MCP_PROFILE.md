# Browser MCP Profile

Referensi teknis untuk Browser MCP. File ini adalah bagian rujukan dari `06_AUTO_SETUP_TOOLS.md`, dipisahkan agar modul setup tetap ringkas saat dibaca.

**Kapan membaca file ini:** hanya ketika tugas memang membutuhkan pengujian di Chrome asli pada mesin pengguna. Untuk pekerjaan biasa, cukup ringkasan di `06_AUTO_SETUP_TOOLS.md` bagian Step 4.

**Ringkasan satu paragraf:** Browser MCP mengendalikan Chrome milik pengguna melalui extension dan daemon lokal. Alat ini hanya berguna bila runtime AI berjalan di mesin yang sama atau punya jalur nyata ke mesin itu. Pada runtime sandbox atau cloud tanpa akses ke mesin pengguna, statusnya `NOT AVAILABLE` dan jangan diklaim bisa dipakai.

---

## Profil Teknis

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

---

## Kembali ke modul

- `06_AUTO_SETUP_TOOLS.md` bagian Step 4 — ringkasan dan kapan dipakai
- `06_AUTO_SETUP_TOOLS.md` bagian 8 — persistensi daemon antar sesi
