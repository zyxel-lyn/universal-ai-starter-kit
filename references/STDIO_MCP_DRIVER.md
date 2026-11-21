# Generic stdio MCP Driver & Browser Test Plan

Referensi untuk memverifikasi server MCP melalui terminal, dan contoh menyusun test plan browser. File ini adalah bagian rujukan dari `06_AUTO_SETUP_TOOLS.md`.

**Kapan membaca file ini:**

- ketika runtime tidak punya MCP native tetapi terminal bisa menjalankan server stdio,
- ketika perlu memeriksa jumlah tool yang benar-benar diekspos sebuah server MCP,
- ketika menyusun test plan browser berbasis route.

**Penting:** driver yang dibahas di sini sudah ada sebagai file nyata di `scripts/stdio_mcp_client.mjs`. Blok kode di bawah adalah salinan untuk audit isi, bukan instruksi menulis ulang file tersebut.

Salinan di bawah sudah disinkronkan dengan file asli, termasuk dua perbaikan Windows: resolusi `npx`/`npm` lewat shell, dan penolakan request seketika saat spawn gagal.

---

## A. Generic stdio MCP Driver

Gunakan driver ini hanya bila runtime tidak memiliki MCP native yang sesuai dan dapat menjalankan server stdio. Pastikan package/server tepercaya dan command-nya telah ditinjau.

**File ini sudah tersedia di repository** pada `scripts/stdio_mcp_client.mjs`. Jangan membuat ulang. Blok di bawah adalah salinan rujukan untuk audit isi, bukan instruksi untuk menulis file baru ke workspace project.

Kedua command pada Step 1 dan Step 2 memakai driver ini dan **sudah diuji jalan** pada mesin Windows: handshake ke `@ant-design/cli mcp` mengembalikan serverInfo `antd` versi 6.7.1 dengan 8 tools.

Catatan kompatibilitas yang sudah diperbaiki di file tersebut:

- Pada Windows, `npx`, `npm`, dan `node` adalah shim `.cmd`. `spawn()` tidak melewati `PATHEXT`, sehingga nama tanpa ekstensi gagal dengan `ENOENT`. Driver sekarang meresolve command bare melalui shell hanya pada Windows.
- Jika proses gagal di-spawn, driver langsung menolak request yang menunggu alih-alih menggantung sampai timeout habis. Kegagalan dilaporkan dalam hitungan milidetik.

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

// On Windows, npx/npm/node are .cmd shims. spawn() does not go through PATHEXT,
// so a bare "npx" fails with ENOENT. Resolve through the shell in that case only.
const isWindows = process.platform === "win32";
const looksLikeBareCommand = !/[\\/]/.test(executable) && !/\.(cmd|exe|bat)$/i.test(executable);
const useShell = isWindows && looksLikeBareCommand;

const child = spawn(executable, commandArgs, {
  stdio: ["pipe", "pipe", "inherit"],
  windowsHide: true,
  shell: useShell,
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
  for (const item of pending.values()) {
    clearTimeout(item.timer);
    item.reject(new Error(`spawn failed: ${error.message}`));
  }
  pending.clear();
  process.exitCode = 2;
});

main();
```

Catatan:

- Driver ini adalah helper generik, bukan pengganti penuh runtime MCP. Lokasi resminya `scripts/stdio_mcp_client.mjs` di repository Starter Pack.
- Pemecahan command menjadi executable/args memiliki batas untuk quoting kompleks. Untuk command kompleks gunakan konfigurasi argumen eksplisit dan terstruktur, bukan shell parsing yang lebih permisif.
- Periksa apakah protocol version masih didukung server aktual. Perbarui berdasarkan dokumentasi server, bukan menebak.
- Gunakan hanya tool yang memang diperlukan. Jangan jalankan tool mutasi hanya untuk mengetes koneksi.

## B. Browser MCP Test Plan Example

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

---

## Kembali ke modul

- `06_AUTO_SETUP_TOOLS.md` bagian Step 1 dan Step 2 — penggunaan driver untuk shadcn dan Ant Design
- `06_AUTO_SETUP_TOOLS.md` bagian 12 — Browser MCP yang diuji oleh test plan
- `scripts/stdio_mcp_client.mjs` — file driver yang sebenarnya
