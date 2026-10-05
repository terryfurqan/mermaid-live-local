# Session Checkpoint: Mermaid Live Local & Offline Editor (2026-10-04)

## 1. Objective & Starting Context
- **Initial Goal**: Mereplikasi diagram flowchart dari tautan `mermaid.live` (Christmas sample dengan tema `redux-dark-color`, grid, dan pan/zoom) menjadi file HTML mandiri lokal.
- **Starting State**: Memulai dari nol decode parameter `#pako:...` dari URL mermaid.live.

## 2. Completed Work & Artifacts
- **Files Modified / Created**:
  - `mermaid_diagram.html`: Editor split-screen lokal berbasis CDN dengan Mermaid v11, FontAwesome 6, dan pan/zoom.
  - `mermaid_diagram_offline.html`: Versi 100% offline & air-gapped dengan aset lokal, CSP ketat (`connect-src 'none'`), dan `securityLevel: 'antiscript'`.
  - `assets/`: Folder aset offline mandiri (`assets/js/mermaid.min.js` UMD 3.4MB tanpa chunk eksternal, `svg-pan-zoom.min.js`, `pako.min.js`, `fontawesome.min.css`, dan webfonts `.woff2`).
  - `1. Mermaid JS Local/`: Workspace utama & repositori git lokal yang tersinkronisasi ke GitHub (`terryfurqan/mermaid-live-local`).
  - `docs/`: Dokumentasi sesi (`walkthrough.md`, `implementation_plan.md`, dan tangkapan layar verifikasi).
  - `diagram.mmd`, `diagram.svg`, `diagram.png`, `diagram.pdf`: Sampel template diagram alur kerja.
- **Key Actions / Verifications**:
  - Pushed to GitHub: [https://github.com/terryfurqan/mermaid-live-local](https://github.com/terryfurqan/mermaid-live-local).
  - Verifikasi render visual menggunakan headless Chrome screenshot (`docs/screenshot_color_presets.png`).
  - Fitur Load/Save `.mmd` (`Cmd/Ctrl+O`, `Cmd/Ctrl+S`, drag & drop).
  - Fitur Pistol-style **Safety Lock** pada tombol eksternal `mermaid.live` untuk pencegahan kebocoran data konfidensial.
  - Kolom **Warna Node & Warna Garis** compact di sisi kiri: masing-masing **24 kotak warna murni** (grid 4×6 tanpa teks di dalam kotak) + tombol **`🎨 Pilih`** (`<input type="color">`) dan **`✕ Reset`**, dengan auto-deteksi target node/garis dari posisi kursor maupun klik langsung pada SVG.
  - **Pewarnaan & Gaya Garis Tanpa Nomor (`Edge ID`)**: Menggunakan sintaks Mermaid v11 `e_A_B@-->` + `classDef ln<Warna>` + `class e_A_B ln<Warna>` (bukan `linkStyle <index>`) sehingga warna garis tetap menempel meski ada penambahan atau penggeseran urutan node/panah, dilengkapi 3 tombol tipe garis (`-->`, `-.->`, `==>`).
  - **Auto-Crop, Interactive AOI & Ultra-HD Export (SVG & PNG)**: Menyimpan salinan SVG murni (`lastCleanSvg`) dan *tight bounding box* (`lastDiagramBox` via `getBBox()`) sebelum dimodifikasi `svg-pan-zoom`, mendukung kotak Area of Interest (AOI) interaktif, serta merender PNG pada skala vektor minimal **4×** (2×–8×) agar hasil ekspor fokus pada diagram dan tidak pecah.

## 3. Decision Log & Idea Evolution
| Decision / Topic | Initial / Explored Idea | Pivot / Why Discarded | Settled Decision & Rationale |
| :--- | :--- | :--- | :--- |
| **Arsitektur Editor** | Diagram viewer statis | User meminta editor di kiri persis mermaid.live | Split-screen resizable dengan live sync, penomoran baris, dan error banner |
| **Kerahasiaan Data & Offline** | CDN jsDelivr dengan `securityLevel: 'loose'` | Ada risiko data konfidensial bocor dan tag HTML liar | Dibuat versi 100% offline dengan CSP `connect-src 'none'`, `securityLevel: 'antiscript'`, dan tombol mermaid.live double-locked |
| **Pewarnaan Node & Garis Cepat** | Tombol memanjang bertulisan nama warna (12 warna) | Memakan ruang vertikal & user ingin lebih banyak warna | Grid 4×6 (24 kotak warna murni tanpa teks) + pemilih warna bebas (`🎨 Pilih`) & `✕ Reset` untuk Node maupun Garis |
| **Pewarnaan Garis Tanpa Nomor** | `linkStyle <index> stroke:<color>` | Urutan indeks bergeser dan salah sasaran saat menyisipkan node/panah baru | Gunakan **Edge ID** Mermaid v11 (`A e_A_B@--> B` + `class e_A_B ln<Warna>`) sehingga warna terikat pada ID panah, bukan nomor urut |
| **Export PNG & SVG** | Clone SVG langsung dari kanvas (`getBoundingClientRect()`) | Menangkap seluruh kanvas kosong akibat transformasi `svg-pan-zoom` dan gambar pecah karena tertahan `max-width` | Simpan `lastCleanSvg` + `getBBox()` sebelum `svgPanZoom` aktif, dukung bingkai AOI interaktif, dan rasterisasi vektor pada skala Ultra-HD (2×–8×) |

## 4. Current Status
- **Working / Verified**:
  - Seluruh file web, aset offline, sampel diagram, dan catatan telah dipindahkan ke `/Users/terryfurqan/Downloads/PIVPy/1. System/1. Mermaid JS Local`.
  - Editor split-screen offline & online berjalan normal.
  - Load & Save `.mmd` berfungsi normal.
  - Palet compact 24 warna Node & 24 warna Garis + custom color picker berfungsi.
  - Pewarnaan panah berbasis Edge ID (`e_A_B@-->`) dan pengubah tipe garis (`-->`, `-.->`, `==>`) terkonfirmasi tahan terhadap penyisipan node/panah baru.
  - Export SVG & PNG Ultra-HD (termasuk mode Area of Interest / AOI) berfungsi normal.
- **Known Blockers / Caveats**:
  - Tanda petik ganda (`"`) di dalam teks panah `|...|` akan memicu parse error pada grammar Mermaid v11; gunakan `linkStyle` atau teks polos tanpa petik ganda di dalam panah.

## 5. Next Session Scope & Action Plan
1. Pengembangan fitur tambahan pada editor lokal (misalnya: color picker custom `<input type="color">`, preset bentuk node: belah ketupat, lingkaran, silinder).
2. Pembuatan template diagram alur kerja / SOP spesifik untuk proyek pengguna.
3. Penyempurnaan dokumentasi atau integrasi ekspor PDF resolusi tinggi langsung dari kanvas.

## 6. Suggested Skills & Tools for Next Session
- `visualisasi-data`: Jika ingin merancang figur ilmiah atau diagram proses yang terstandar publikasi jurnal.
- `session-docs`: Jika ingin menyusun dokumentasi teknis atau README yang lebih lengkap.
