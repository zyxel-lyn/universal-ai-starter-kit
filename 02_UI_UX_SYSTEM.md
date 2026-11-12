---
name: ui-ux-system
version: "1.0.0"
document_type: "specialist-module"
source: "UNIVERSAL_AI_WEB_APP_DEVELOPMENT_BOOTSTRAP_v1.2.md"
source_internal_version: "1.3.0"
language_default: "id-ID"
---

# 02 — UI/UX SYSTEM
## UX Requirements, Design-System Selection, Tokens, Components, Workbench, dan Visual Quality

> Modul spesialis untuk UI/UX. Gunakan bersama `MASTER_BOOTSTRAP.md` dan `01_PRODUCT_DISCOVERY.md`. Jangan mengunci arah UI sebelum flow produk yang relevan cukup dipahami dan dikonfirmasi. Modul ini bukan perintah untuk menumpuk semua design system, library, atau skill yang tercantum.

---

# 1. Mandat UI/UX

Tanggung jawab AI:
1. Memilih fondasi UI yang sesuai dengan platform, pengguna, workflow, kepadatan informasi, dan kemampuan pemeliharaan.
2. Memisahkan identitas visual produk dari library komponen yang digunakan untuk implementasi.
3. Menetapkan UX dan aksesibilitas sebagai baseline.
4. Menetapkan token dan kontrak komponen bersama.
5. Memverifikasi sistem komponen sebelum memperbanyak halaman pada proyek non-trivial.
6. Mencegah UI drift antarhalaman.
7. Menggunakan skill secara selektif sesuai tahap.
8. Menilai hasil dari tampilan dan perilaku aktual, bukan hanya source code atau klaim AI.

Prinsip utama:

> **Satu produk harus memiliki satu pengalaman visual dan interaksi yang koheren. Komponen halaman mengonsumsi sistem bersama, bukan menciptakan aturan visualnya sendiri-sendiri.**

Urutan default untuk UI non-trivial:

```text
Product Flow
→ UX Architecture
→ Visual Direction + Tokens
→ UI Component System
→ Component Workbench / Preview
→ Representative Reference Surface
→ Remaining Pages
→ Runtime / Visual / Accessibility QA
```

Prototype sekali pakai boleh menggunakan proses yang lebih ringan, tetapi bila lebih dari satu layar dibangun, tetap gunakan lapisan token dan komponen bersama yang ringkas.

---

# 2. UI/UX Bootstrap Setelah Discovery

Setelah flow produk yang relevan dikonfirmasi melalui modul Product Discovery, susun UI/UX Project Bootstrap yang ringkas dan siap ditinjau client.

Wajib mencakup:

1. Klasifikasi proyek dan kondisi saat ini.
2. Primary UI design system yang dipilih.
3. Alasan pemilihan berdasarkan pengguna, workflow, kepadatan informasi, platform, dan maintainability.
4. Design system yang tidak dipilih dan alasan utama, hanya bila berguna.
5. Skills yang dipilih untuk tahap sekarang dan urutan penggunaannya.
6. Baseline UX dan aksesibilitas.
7. Arah UI: hierarki visual, kepadatan informasi, model navigasi, intent tipografi, spacing, tingkat motion, responsive behavior, dan karakter interaksi.
8. Strategi reusable component / component slicing.
9. Strategi copywriting.
10. WBS awal dengan dependensi dan acceptance criteria.
11. Risiko kritis atau bukti yang belum tersedia.
12. Langkah eksekusi berikutnya.

Jika arah visual benar-benar ambigu, berikan maksimal tiga arah yang secara material berbeda. Jelaskan trade-off dengan bahasa client, berikan rekomendasi developer, lalu minta client memilih atau menyetujui sebelum arah tersebut dikunci.

Jangan membuat pilihan visual seolah-olah sudah disetujui. Gunakan status `PROPOSED BY AI` sampai client menyetujuinya.

---

# 3. UI Requirement Selector

Daftar berikut adalah kandidat sistem, bukan persyaratan kumulatif. Pilih berdasarkan kebutuhan produk dan bukti, bukan popularitas.

## 3.1 Kandidat UI Systems

### Ant Design

Pertimbangkan ketika produk:
- enterprise/admin-heavy,
- padat data,
- intensif tabel, form, dan workflow,
- bersifat operasional atau B2B,
- membutuhkan komponen React siap pakai yang matang.

Jangan membiarkan hasil akhir terlihat seperti Ant Design bawaan. Tetapkan token, tipografi, spacing, density, dan komposisi yang khas produk.

### Material Design 3

Pertimbangkan ketika:
- konsistensi lintas perangkat penting,
- pola interaksi Material terasa familiar bagi pengguna,
- pengaruh Android/mobile web relevan,
- produk membutuhkan bahasa desain komprehensif, bukan hanya paket komponen.

### MUI

Pertimbangkan untuk proyek React yang membutuhkan:
- komponen berbasis Material yang matang,
- dukungan ekosistem luas,
- theming,
- form dan data UI kompleks,
- implementasi cepat dengan pola yang dapat diprediksi.

Jangan memilih Material Design 3 dan MUI seolah keduanya dua design system yang tidak berkaitan. MUI dapat menjadi opsi implementasi untuk arah Material.

### shadcn/ui + Radix

Pertimbangkan ketika:
- produk membutuhkan identitas SaaS/product yang khas,
- kepemilikan kode dan composability penting,
- Tailwind/CSS-variable tokenization cocok dengan stack,
- accessibility primitives dibutuhkan,
- tim ingin mengendalikan implementasi komponen, bukan mengonsumsi library monolitik.

Ini merupakan kandidat kuat untuk custom modern product UI, tetapi bukan pilihan otomatis untuk semua proyek.

### Microsoft Fluent 2

Pertimbangkan ketika:
- produk dekat dengan konteks Microsoft 365, Windows, atau enterprise productivity,
- workflow produktivitas dominan,
- pola interaksi desktop-like diinginkan,
- familiaritas ekosistem Microsoft mengurangi beban belajar pengguna.

### IBM Carbon

Pertimbangkan ketika:
- enterprise operations, analytics, infrastructure, engineering, industrial, atau workflow data-heavy dominan,
- informasi terstruktur dan governance design system lebih penting daripada styling consumer yang ekspresif.

### Shopify Polaris

Pertimbangkan ketika:
- produk adalah merchant/ecommerce administration,
- workflow menyerupai store operations, catalog, orders, fulfillment, merchant settings, atau commerce back office.

Jangan memilih Polaris hanya karena produk menjual sesuatu. Gunakan ketika pola operasional merchant memang relevan.

### Atlassian Design System

Pertimbangkan ketika:
- produk berorientasi kolaborasi, work management, atau produktivitas,
- issue/task/project workflow yang padat menjadi inti,
- navigasi dan interaksi menyerupai team tooling kompleks.

## 3.2 Aturan Pemilihan

Pisahkan tiga keputusan:

1. **Product visual language** — tipografi milik produk, token, density, composition, motion, dan karakter interaksi.
2. **Primary component foundation / implementation library** — misalnya shadcn/Radix, Ant Design, MUI, Fluent, Carbon, atau fondasi lain.
3. **Secondary references** — prinsip dari produk atau design system lain yang digunakan sebagai inspirasi, bukan library komponen tambahan.

Kecuali proyek sedang dalam masa migrasi sementara:

> **Pilih SATU primary component foundation / implementation strategy. DESIGN.md dan UI_SYSTEM.md milik produk tetap menjadi sumber kebenaran untuk pengalaman akhir produk.**

Prinsip dari sistem lain boleh dipinjam, tetapi jangan mencampur beberapa library komponen secara sembarangan.

Pencampuran hanya diperbolehkan bila:
1. migrasi bertahap memiliki rencana deprecation yang eksplisit,
2. ada constraint kompatibilitas legacy,
3. ada embedded surface yang terisolasi,
4. komponen yang dibutuhkan tidak dapat dibuat atau diperoleh secara wajar dari sistem utama,
5. pengguna secara eksplisit memintanya dan menerima biaya pemeliharaan.

Jika mencampur sistem, dokumentasikan:
- alasan,
- komponen yang dicampur,
- strategi isolasi CSS/token,
- implikasi aksesibilitas,
- biaya bundle/runtime,
- rencana penghapusan atau konvergensi.

## 3.3 UI Selection Decision Matrix

Nilai secara internal pada skala 1–5. Jangan menampilkan presisi palsu jika bukti belum cukup.

Evaluasi:
- platform fit,
- workflow fit,
- information-density fit,
- accessibility maturity,
- component coverage,
- customization freedom,
- brand distinctiveness,
- engineering fit,
- team maintainability,
- bundle/performance implications,
- ecosystem longevity,
- migration cost.

Pemilihan sistem harus dijustifikasi dengan bukti proyek, bukan tren popularitas.

---

# 4. UX Requirements — ALWAYS ACTIVE

## 4.1 Nielsen's 10 Usability Heuristics

Periksa setiap flow yang relevan berdasarkan:

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

Jangan menjadikannya checklist kosong. Hubungkan temuan dengan layar, komponen, state, dan tugas pengguna yang konkret.

## 4.2 WCAG 2.2

Target default: **WCAG 2.2 AA**.

Minimal verifikasi:
- semantic structure,
- keyboard navigation,
- visible focus,
- logical focus order,
- contrast,
- text resize/reflow,
- labels and instructions,
- accessible names,
- form errors and recovery,
- status announcements bila berlaku,
- komunikasi yang tidak bergantung pada warna saja,
- target size/touch ergonomics,
- reduced motion behavior,
- responsive reflow,
- modal/dialog focus management,
- accessible tables/data visualization bila digunakan.

Gunakan AAA secara selektif bila proyek atau kontennya memang memerlukan. Jangan mengklaim full AAA tanpa bukti.

## 4.3 Universal UX Quality Floor

Evaluasi juga:
- first-use comprehension,
- navigation clarity,
- progressive disclosure,
- cognitive load,
- empty states,
- loading states,
- skeleton/progress behavior,
- validation states,
- errors,
- offline/degraded states bila relevan,
- permissions/authorization states,
- destructive-action confirmation,
- undo/recovery opportunities,
- mobile ergonomics,
- responsive adaptation,
- internationalization readiness,
- performance perception,
- accessibility of motion,
- discoverability of advanced features.

---

# 5. Skill Router

Skill adalah sumber keahlian spesialis. URL atau nama skill dalam dokumen bukan bukti bahwa skill telah dipasang atau digunakan.

## 5.1 Candidate External Skills

Daftar kandidat yang tercantum dalam sumber:
- UI UX Pro Max: https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
- Taste Skill: https://github.com/leonxlnx/taste-skill
- Impeccable: https://github.com/pbakaus/impeccable
- Emil Kowalski Skills: https://github.com/emilkowalski/skills
- UX/UI Agent Skills: https://github.com/plugin87/ux-ui-agent-skills
- UI Craft: https://github.com/educlopez/ui-craft
- Apple Design Skill: https://github.com/dickwu/apple-design-skill
- Vercel Agent Skills / Web Design Guidelines: https://github.com/vercel-labs/agent-skills/tree/main/skills/web-design-guidelines
- Vercel React Best Practices: https://github.com/vercel-labs/agent-skills/tree/main/skills/react-best-practices
- Vercel Composition Patterns: https://github.com/vercel-labs/agent-skills/tree/main/skills/composition-patterns
- Vercel open Agent Skills CLI/ecosystem: https://github.com/vercel-labs/skills

**Critical Skill Rule:** sebelum menyatakan skill digunakan:
- pastikan skill terpasang/tersedia, atau
- baca instruksi aktualnya bila tools mengizinkan, atau
- nyatakan secara eksplisit bahwa yang digunakan hanya prinsip/referensi publiknya.

Jika skill tidak tersedia, lanjutkan dengan kemampuan native tanpa memblokir proyek.

## 5.2 Skill Responsibilities

Gunakan routing berikut sebagai panduan, bukan pembatas eksklusif.

### ui-ux-pro-max-skill
Cocok untuk:
- broad UI/UX design intelligence,
- design-system generation,
- style/pattern exploration,
- framework-aware UI planning.

### taste-skill
Cocok untuk:
- mengurangi tampilan generik seperti hasil AI,
- kualitas layout,
- typography rhythm,
- spacing,
- visual hierarchy,
- motion direction,
- product-specific taste.

### impeccable
Cocok untuk:
- design audit,
- critique dan polish,
- anti-pattern detection,
- accessibility-aware refinement,
- responsive behavior,
- theming,
- UX copy,
- error/edge states,
- production-grade final refinement.

### emilkowalski/skills
Cocok untuk:
- animation/motion quality,
- UI library selection,
- mobile-native web behavior,
- interaction details dan engineering craft.

### ux-ui-agent-skills
Cocok untuk:
- token-driven design-system rigor,
- accessibility checks,
- component engineering,
- objective quality gates,
- framework/design-system mapping.

### ui-craft
Cocok untuk:
- production-quality surface composition,
- designer-grade implementation review,
- forms, dashboards, layout, polish,
- acceptance-bar driven UI delivery.

### apple-design-skill
Gunakan setelah UI yang koheren sudah ada, untuk:
- post-build design review,
- interaction clarity,
- hierarchy,
- platform craft,
- mobile/desktop product ergonomics,
- Apple HIG review ketika Apple platforms relevan secara langsung.

Untuk web non-Apple, perlakukan Apple HIG sebagai secondary craft lens, bukan mandat membuat produk terlihat seperti iOS/macOS.

### vercel-labs/web-design-guidelines
Cocok untuk:
- focused review of web UI implementation,
- accessibility/interaction quality checks,
- forms, controls, content, responsiveness, dan interface best practices,
- code-level UI review sebelum acceptance.

Utamanya skill review/verifikasi, bukan satu-satunya otoritas desain produk.

### vercel-react-best-practices
Gunakan hanya untuk pekerjaan React / Next.js-family yang relevan:
- data-fetching dan waterfall review,
- bundle-size dan rendering-performance checks,
- server/client boundary quality,
- rerender dan JavaScript-performance review,
- implementation/refactor guidance.

Jangan muat untuk proyek non-React.

### vercel-composition-patterns
Gunakan hanya untuk arsitektur komponen React-family:
- reusable component APIs,
- compound-component patterns,
- menghindari boolean-prop proliferation,
- state/provider boundaries,
- scalable component composition.

Berguna saat membangun UI Component System, tetapi tidak menentukan visual direction.

### ux-ui-agent-skills — targeted sub-skill routing
Jika repository tersedia, jangan perlakukan sebagai satu skill raksasa. Pilih kemampuan spesifik:
- `design-tokens` / token workflow — token source of truth,
- `design-component` — component anatomy/variants/states,
- `design-code` — token-driven production implementation,
- `a11y-audit` — accessibility-specific verification,
- `apply-aesthetic` — controlled visual direction.

Gunakan sub-skill sekecil mungkin untuk gate saat ini.

## 5.3 Skill Selection Policy

Pilih skill berdasarkan tugas saat ini. Rekomendasi maksimum per tahap:
- discovery/planning: 1–2 skills,
- active UI design/build: 1–3 skills,
- audit/polish: 1–3 skills,
- final review: sertakan apple-design-skill jika tersedia dan relevan dengan review contract.

Jangan memuat enam skill dengan instruksi tumpang tindih pada setiap giliran.

Jika ada konflik, urutan penyelesaian:
1. explicit user brief,
2. established project design system,
3. current product evidence,
4. accessibility/usability requirements,
5. primary UI system conventions,
6. selected specialist skill,
7. general aesthetic preference.

## 5.4 External Skill Intake Protocol

Skill yang ditemukan di web adalah kandidat, bukan dependency yang otomatis dipercaya. Sebelum menambahkan skill baru di luar daftar yang telah disetujui, evaluasi:

1. Stage fit — apakah menyelesaikan gap yang belum tercakup?
2. Source quality — utamakan repository author/vendor asli daripada fork.
3. Maintenance signal — aktivitas terbaru, dokumentasi, kondisi issue.
4. Scope clarity — tanggung jawab skill jelas dan terbatas.
5. Determinism — jika setara, utamakan checklist/script/validation dibanding prompt gaya yang samar.
6. Compatibility — dukungan framework/runtime/agent cocok.
7. Security — periksa script dan shell permissions sebelum menjalankannya.
8. License — dapat diterima untuk proyek.
9. Context cost — hindari skill pack besar yang tumpang tindih.
10. Version reproducibility — catat versi/commit bila skill memengaruhi perilaku proyek secara material.

Tambahkan skill baru hanya jika setidaknya satu kondisi terpenuhi:
- menutup capability gap,
- memberi verifikasi deterministik yang jauh lebih baik,
- mengurangi waktu implementasi/audit tanpa menurunkan kualitas,
- memberi keahlian framework yang benar-benar digunakan proyek.

Jangan menambah skill hanya karena populer atau menarik secara estetika.

### Preferred Routing by Stage

| Stage | Primary need | Preferred skill class |
|---|---|---|
| Discovery / product framing | Product truth, flows, ambiguity | native reasoning / product-context skill |
| Visual direction | Taste, hierarchy, composition | taste-skill atau impeccable sebagai primary craft lens |
| Tokens / components | System rigor, states, accessibility | ux-ui-agent-skills targeted sub-skills |
| React component architecture | Scalable composition | vercel-composition-patterns |
| Motion | Purposeful interaction motion | Emil Kowalski animate / motion review |
| React / Next implementation | Performance and implementation quality | vercel-react-best-practices |
| UI review | Interface rules and accessibility | web-design-guidelines |
| Final polish | Visual defects / anti-patterns | impeccable atau equivalent terpilih |

Jangan menumpuk semua baris sekaligus.

---

# 6. Design-System Contract

Untuk proyek yang diperkirakan bertahan lebih lama daripada prototype sekali pakai, tetapkan lapisan design system.

## 6.1 Required Token Categories

Minimal definisikan atau petakan:
- color primitives,
- semantic colors,
- typography families,
- type scale,
- font weights,
- line heights,
- spacing scale,
- radii,
- borders,
- shadows/elevation,
- z-index layers,
- container widths,
- responsive breakpoints,
- motion durations,
- easing,
- opacity,
- focus style,
- state tokens,
- data-visualization tokens bila relevan.

Utamakan semantic tokens seperti:
- `surface.default`
- `surface.subtle`
- `text.primary`
- `text.secondary`
- `border.default`
- `action.primary`
- `status.success`
- `status.warning`
- `status.danger`

Hindari hard-coded colors yang tersebar di komponen.

## 6.2 Design Truth Files

Jika akses repository tersedia, pelihara sesuai kebutuhan:
- `PRODUCT.md` — product truth dan pengguna.
- `DESIGN.md` — visual language, token philosophy, composition rules.
- `UI_SYSTEM.md` — component registry, variants, states, interaction contracts, dan preview/workbench status untuk UI non-trivial. Proyek kecil boleh menaruhnya sebagai bagian khusus di `DESIGN.md`.
- `DECISIONS.md` — keputusan arsitektur/desain beserta alasan.
- `PROJECT_STATE.md` — kondisi ringkas saat ini dan tindakan berikutnya.
- `WBS.md` — work breakdown dan gates.

Jangan menduplikasi penjelasan panjang yang sama di banyak file. Setiap dokumen memiliki tugas berbeda.

## 6.3 UI Component System Contract — HARD GATE

Design system belum siap diimplementasikan hanya karena token warna, tipografi, dan spacing sudah ada.

Sebelum banyak halaman dibuat, tetapkan minimum shared component system yang dibutuhkan oleh core journey yang telah disetujui.

### 6.3.1 Foundation Layers

Tetapkan atau petakan:
- typography roles,
- icon sizing/stroke rules,
- spacing dan layout primitives,
- surfaces / borders / elevation,
- focus ring,
- control heights dan density,
- interaction state tokens,
- motion durations/easing,
- responsive breakpoints,
- reduced-motion behavior.

### 6.3.2 Minimum Component Inventory

Pilih komponen yang relevan dengan produk, tetapi evaluasi secara eksplisit kategori berikut.

**Actions**
- Button hierarchy: primary, secondary, tertiary/ghost, destructive, icon button.
- Link/action text.
- Split/dropdown action bila relevan.

**Inputs**
- Input / Textarea.
- Search input.
- Select / Combobox.
- Checkbox / Radio / Switch bila relevan.
- Segmented control / tabs bila relevan.
- File upload, date/time, numeric controls bila relevan.

**Navigation**
- Navigation item.
- Sidebar / rail item.
- Top navigation item bila relevan.
- Tabs.
- Breadcrumbs bila relevan.
- Pagination bila relevan.

**Surfaces**
- Card.
- Panel.
- Section/container surface.
- Dialog / Drawer / Sheet.
- Popover / Menu / Tooltip bila relevan.

**Structure**
- Page header.
- Section header.
- Toolbar.
- Filter bar / command surface.
- Table/list shell bila relevan.

**Feedback & status**
- Badge / status indicator.
- Alert / banner.
- Toast/notification.
- Progress / skeleton/loading.
- Empty state.
- Error state.
- Success/confirmation state.

**Motion**

Tetapkan perilaku bersama untuk:
- hover/pressed feedback,
- focus transitions,
- open/close,
- expand/collapse,
- enter/exit hanya bila dibenarkan,
- loading/progress,
- spatial transitions bila relevan.

Motion harus mengomunikasikan state, hierarchy, continuity, atau causality. Decorative motion bukan persyaratan default.

### 6.3.3 Interactive State Contract

Setiap komponen interaktif harus secara eksplisit menangani state yang berlaku:

```text
DEFAULT
HOVER
PRESSED / ACTIVE
FOCUS-VISIBLE
DISABLED
LOADING
ERROR
SELECTED / CHECKED / CURRENT
```

Tidak semua komponen membutuhkan semua state, tetapi state yang tidak ada harus merupakan keputusan sadar, bukan kelalaian.

### 6.3.4 Component Contract Schema

Untuk setiap reusable component yang bermakna, definisikan:

| Field | Requirement |
|---|---|
| Name / role | Identitas dan fungsi komponen yang stabil |
| Anatomy | Struktur internal yang diperlukan |
| Variants | Variant eksplisit; hindari boolean-prop explosion |
| Sizes / density | Ukuran dan kepadatan yang didukung |
| States | Applicable interaction/data states |
| Token mapping | Tidak ada arbitrary per-page styling |
| Keyboard model | Perilaku keyboard untuk komponen interaktif |
| Accessibility | Semantic element, label/ARIA, focus behavior |
| Responsive behavior | Narrow-width dan touch behavior |
| Motion | Intent, duration/easing, reduced-motion fallback |
| Content constraints | Panjang label, truncation/wrapping, aturan icon |
| Status | PROPOSED / APPROVED / DEPRECATED |
| Evidence | Workbench screenshot/test/runtime result bila berguna |

### 6.3.5 Page Consumption Rule

Setelah komponen berstatus `APPROVED`, halaman harus menggunakan public variants-nya, bukan mengimplementasikan ulang stylenya secara lokal.

Kode halaman boleh menyusun komponen dengan cara berbeda, tetapi tidak boleh sembarangan membuat:
- warna button baru,
- radius baru,
- padding card/panel baru,
- focus treatment berbeda,
- hover/pressed state alternatif,
- badge/status semantics berbeda,
- timing animasi one-off,
- active navigation styling duplikat.

Jika komponen yang ada tidak memenuhi kebutuhan baru yang valid, perluas component system secara sengaja.

## 6.4 Component Workbench / Preview Surface

Untuk proyek non-trivial, buat satu tempat yang dapat diperiksa untuk merender UI system aktif sebelum memperbanyak halaman.

Pilihan yang diutamakan:
1. Storybook/design-system environment yang sudah ada,
2. route lokal ringan seperti `/dev/ui`, `/styleguide`, atau yang setara,
3. component test fixtures/screenshot harness,
4. preview system lain yang native pada repository.

Jangan menambahkan Storybook hanya karena kebiasaan jika preview project-native yang lebih ringan sudah cukup.

Workbench menampilkan sesuai kebutuhan:
- variants,
- sizes,
- light/dark themes,
- long/short content,
- validation/errors,
- loading/disabled states,
- hover/pressed/focus-visible,
- keyboard behavior,
- mobile/narrow width,
- reduced motion.

### Component System Gate

Status boleh menjadi `UI SYSTEM VERIFIED` hanya jika:
- token source koheren,
- minimum component inventory untuk core journey tersedia,
- applicable states sudah diimplementasikan,
- keyboard/focus behavior dapat diterima,
- representative mobile + desktop rendering sudah diverifikasi,
- tidak ada kontradiksi visual antar-komponen yang jelas.

Hanya setelah gate ini, proyek boleh memperbanyak product pages.

## 6.5 Representative Reference Surface

Setelah component system melewati gate-nya, bangun **satu representative product screen** yang menggunakan bagian penting dari sistem.

Pilih layar yang:
- sentral terhadap core journey,
- komposisinya cukup kompleks,
- mewakili information density,
- berpotensi memperlihatkan kelemahan component system.

Gunakan untuk memvalidasi:
- page composition,
- visual hierarchy,
- density,
- responsive transformation,
- interaction relationships,
- domain-specific suitability.

Jika reference surface menunjukkan masalah sistem, perbaiki shared system terlebih dahulu. Jangan hanya menambal halaman referensi.

Setelah disetujui, jadikan reference surface sebagai **golden composition baseline**, bukan template pixel-perfect untuk semua halaman.

---

# 7. Reusable Component Strategy — Component Slicing

Interpretasikan "Slashing" sebagai component slicing, shared-layout extraction, dan reuse yang disiplin untuk UI yang muncul berulang.

Contoh pola:
- sidebar,
- navbar/topbar,
- app shell,
- breadcrumbs,
- page header,
- command/search surfaces,
- cards,
- table patterns,
- filter bars,
- form fields,
- dialogs/drawers,
- notifications/toasts,
- pagination,
- empty/error/loading states,
- role/permission guards,
- recurring data display patterns.

## 7.1 Extraction Rule

Ekstrak shared component bila satu atau lebih kondisi terpenuhi:
- muncul di dua atau lebih lokasi yang bermakna,
- behavior harus konsisten di seluruh produk,
- accessibility logic sebaiknya dipusatkan,
- styling sudah ditokenisasi dan berlaku lintas produk,
- mewakili domain pattern yang stabil,
- duplikasi menimbulkan risiko maintenance atau QA.

Jangan melakukan over-componentization pada markup one-off yang sepele.

## 7.2 Component Architecture

Utamakan lapisan:
1. **Primitives** — Button, Input, Dialog, Tooltip.
2. **Composed UI** — SearchField, FilterBar, DataTableToolbar.
3. **Domain components** — OrderStatus, AssetCard, ProjectHealth.
4. **Layouts** — AppShell, DashboardLayout, AuthLayout.
5. **Features** — kemampuan produk yang mandiri.

Hindari shared component raksasa yang dikendalikan puluhan boolean props. Utamakan explicit variants dan composition.

## 7.3 UI Drift Prevention

Periksa secara berkala apakah halaman yang berulang mulai menyimpang dari sistem yang disetujui.

Sinyal UI drift:
- semantic action yang sama memakai button hierarchy berbeda,
- one-off CSS values berulang,
- anatomy card/panel sedikit berbeda antarhalaman,
- status colors/labels tidak konsisten,
- active navigation behavior berbeda,
- empty/loading/error patterns duplikat,
- motion timing route-specific tanpa alasan,
- page-local responsive fixes yang bertentangan dengan shared component contract.

Jika drift muncul, utamakan memperbaiki shared system atau mengekstrak pattern daripada menormalkan tiap halaman satu per satu.

Untuk proyek React, lakukan composition-pattern review ketika shared components mulai memiliki banyak boolean props atau pengecualian route-specific.

---

# 8. Copywriting dan Konten UI

Copywriting merupakan bagian dari UX, bukan teks pengisi.

Pastikan teks antarmuka:
- jelas dan sesuai bahasa pengguna,
- menyebut tindakan dan hasil secara spesifik,
- tidak menyalahkan pengguna saat terjadi error,
- memberikan instruksi pemulihan bila relevan,
- konsisten dalam istilah,
- mendukung label, helper text, validation, empty state, loading, success, dan confirmation.

Strategi copywriting harus sesuai produk dan pengguna. Jangan membuat klaim, kebijakan, atau informasi bisnis yang belum dikonfirmasi.

---

# 9. Handoff, Evidence, dan Gate

Setelah UI/UX bootstrap disusun:
1. kaitkan arah visual dengan Product Flow yang telah dikonfirmasi,
2. pisahkan keputusan yang diusulkan dan yang sudah disetujui,
3. catat pilihan primary component foundation dan alasan,
4. catat token source dan component strategy,
5. tetapkan component workbench/preview yang sesuai,
6. tetapkan representative reference surface,
7. serahkan kontrak komponen ke engineering,
8. serahkan UX/accessibility criteria ke QA,
9. catat status pada artefak proyek yang relevan.

Jangan menyatakan `UI SYSTEM VERIFIED` hanya karena file token atau komponen sudah dibuat. Gate membutuhkan bukti rendering dan perilaku yang relevan.

Gunakan status yang jelas:
- `PROPOSED`
- `APPROVED`
- `IMPLEMENTED — NOT VERIFIED`
- `UI SYSTEM VERIFIED`
- `DEPRECATED`

Bukti yang dapat mendukung verifikasi:
- workbench/preview,
- screenshot desktop dan mobile,
- keyboard/focus check,
- reduced-motion check,
- test yang relevan,
- hasil pemeriksaan accessibility.

Graphify atau source code saja tidak membuktikan kualitas visual/runtime.

---

# 10. Rujukan ke Modul Lain

- Discovery, user, flow, dan approval → `01_PRODUCT_DISCOVERY.md`
- Arsitektur dan implementasi → `03_ENGINEERING_STANDARDS.md`
- QA, testing, dan verifikasi → `04_QA_TESTING.md`
- WBS dan kontrol pekerjaan → `05_PROJECT_MANAGEMENT.md`
- Skill/MCP/tool setup → `06_AUTO_SETUP_TOOLS.md`
- State, keputusan, dan handoff → `07_PROJECT_CONTINUITY.md`

Baca hanya modul yang relevan dan tersedia. Ikuti mekanisme pencarian/permintaan modul pada MASTER.

---

# 11. Catatan Kesetiaan terhadap Sumber

Modul ini memisahkan materi UI/UX dari dokumen sumber, terutama:
- Bagian 2.10 — UI/UX Bootstrap After Discovery,
- Bagian 2.11 — UI Component System Gate Before Page Proliferation,
- Bagian 3 — UI Requirement Selector,
- Bagian 4 — UX Requirements,
- Bagian 5 — Skill Router,
- Bagian 6 — Design-System Contract,
- Bagian 7 — Reusable Component Strategy,
- Appendix F — UI Component System Minimum Registry Template.

Materi audit/Graphify dan memory efficiency yang berdiri sendiri tetap berada di modul QA/continuity atau MASTER sesuai tanggung jawabnya.
