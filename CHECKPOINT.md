# Session Checkpoint: Mermaid Live Local & Offline Editor (2026-10-04)

## 1. Objective & Starting Context
- **Initial Goal**: Mereplikasi diagram flowchart dari tautan `mermaid.live` (Christmas sample dengan tema `redux-dark-color`, grid, dan pan/zoom) menjadi file HTML mandiri lokal.
- **Starting State**: Memulai dari nol decode parameter `#pako:...` dari URL mermaid.live.

## 2. Completed Work & Artifacts
- **Files Modified / Created**:
  - `mermaid_diagram.html`: Editor split-screen lokal berbasis CDN dengan Mermaid v11, FontAwesome 6, dan pan/zoom.
  - `mermaid_diagram_offline.html`: Versi 100% offline & air-gapped dengan aset lokal, CSP ketat (`connect-src 'none'`), dan `securityLevel: 'antiscript'`.
  - `assets/`: Folder aset offline mandiri (`assets/js/mermaid.min.js` UMD 3.4MB tanpa chunk eksternal, `svg-pan-zoom.min.js`, `pako.min.js`, `fontawesome.min.css`, dan webfonts `.woff2`).
  - `mermaid-live-local/`: Repositori git lokal yang tersinkronisasi ke GitHub.
- **Key Actions / Verifications**:
  - Pushed to GitHub: [https://github.com/terryfurqan/mermaid-live-local](https://github.com/terryfurqan/mermaid-live-local).
  - Verifikasi render visual menggunakan headless Chrome screenshot (`screenshot_color_presets.png`).
  - Fitur Load/Save `.mmd` (`Cmd/Ctrl+O`, `Cmd/Ctrl+S`, drag & drop).
  - Fitur Pistol-style **Safety Lock** pada tombol eksternal `mermaid.live` untuk pencegahan kebocoran data konfidensial.
  - Kolom **Warna Node** di sisi kiri dengan 12 preset warna (stroke putih, mnemonic `M`, `O`, `K`, `H`, `T`, `C`, `B`, `I`, `U`, `P`, `CK`, `A`) dengan auto-deteksi target node dari posisi kursor maupun klik langsung pada SVG.
  - Validasi sintaks pewarnaan teks panah (`linkStyle <N> stroke:<color>,color:<color>`) dan komentar (`%%`).

## 3. Decision Log & Idea Evolution
| Decision / Topic | Initial / Explored Idea | Pivot / Why Discarded | Settled Decision & Rationale |
| :--- | :--- | :--- | :--- |
| **Arsitektur Editor** | Diagram viewer statis | User meminta editor di kiri persis mermaid.live | Split-screen resizable dengan live sync, penomoran baris, dan error banner |
| **Kerahasiaan Data & Offline** | CDN jsDelivr dengan `securityLevel: 'loose'` | Ada risiko data konfidensial bocor dan tag HTML liar | Dibuat versi 100% offline dengan CSP `connect-src 'none'`, `securityLevel: 'antiscript'`, dan tombol mermaid.live double-locked |
| **Pewarnaan Node Cepat** | Mengetik manual `style` / `classDef` | Terlalu repot dan harus menebak kode warna | Kolom preset warna 1-klik di sisi kiri dengan visual box ber-stroke putih + auto target node tracking |
| **Pewarnaan Teks Panah** | Tag HTML `<font color="...">` | Gagal di markdown previewer yang mematikan HTML (`htmlLabels: false`) dan error jika ada petik ganda | Gunakan `linkStyle <index> stroke:<color>,color:<color>` yang kompatibel di semua renderer |

## 4. Current Status
- **Working / Verified**:
  - Editor split-screen offline & online berjalan normal.
  - Load & Save `.mmd` berfungsi normal.
  - Pewarnaan node 1-klik dan mnemonic berfungsi.
  - Pewarnaan panah via `linkStyle` dan komentar via `%%` terkonfirmasi.
  - Seluruh file dan histori commit tersinkron ke GitHub `terryfurqan/mermaid-live-local`.
- **Known Blockers / Caveats**:
  - Tanda petik ganda (`"`) di dalam teks panah `|...|` akan memicu parse error pada grammar Mermaid v11; gunakan `linkStyle` atau teks polos tanpa petik ganda di dalam panah.

## 5. Next Session Scope & Action Plan
1. Pengembangan fitur tambahan pada editor lokal (misalnya: color picker custom `<input type="color">`, preset bentuk node: belah ketupat, lingkaran, silinder).
2. Pembuatan template diagram alur kerja / SOP spesifik untuk proyek pengguna.
3. Penyempurnaan dokumentasi atau integrasi ekspor PDF resolusi tinggi langsung dari kanvas.

## 6. Suggested Skills & Tools for Next Session
- `visualisasi-data`: Jika ingin merancang figur ilmiah atau diagram proses yang terstandar publikasi jurnal.
- `session-docs`: Jika ingin menyusun dokumentasi teknis atau README yang lebih lengkap.
