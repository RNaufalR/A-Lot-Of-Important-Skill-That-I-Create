# context-efficiency

Skill orkestrasi untuk efisiensi context window, token, dan kontinuitas
lintas-sesi. Menggabungkan empat prinsip perilaku yang terdokumentasi
publik dari empat proyek open-source — **Caveman** (kompresi output),
**Ponytail** (minimalisme coding), **Context Mode** (routing data besar
keluar dari context utama), dan **agentmemory** (memori selektif
lintas-sesi) — sebagai satu sistem koheren, bukan empat prompt yang
ditempel.

Ini adalah **sintesis orisinal** berdasarkan perilaku yang didokumentasikan
publik dari proyek-proyek tersebut, bukan salinan dari `SKILL.md` atau kode
sumber mereka.

## Isi paket

```
context-efficiency/
├── SKILL.md                        # Inti: orkestrasi + 4 modul (ringkas, <500 baris)
├── README.md                       # Berkas ini
├── references/
│   ├── response-compression.md     # Detail Module 1 + contoh before/after
│   ├── coding-minimalism.md        # Detail Module 2 + strategi repo besar
│   ├── context-routing.md          # Detail Module 3 + pola pemrosesan data besar
│   ├── memory-policy.md            # Detail Module 4 + aturan anti-klaim-palsu
│   └── validation.md               # Checklist validasi skill ini sendiri
└── tests/
    ├── coding.md                   # Test matrix Module 2
    ├── daily.md                    # Test matrix Module 1
    ├── memory.md                   # Test matrix Module 4
    └── context.md                  # Test matrix Module 3 + exactness
```

`SKILL.md` sengaja dijaga ringkas — detail panjang dipindah ke
`references/` agar hanya dimuat saat benar-benar dibutuhkan (progressive
disclosure), konsisten dengan prinsip Context Mode/agentmemory sendiri:
jangan masukkan semua hal ke context sekaligus.

## Instalasi

### Claude.ai (Skills)

Unggah folder ini (atau file `.skill` hasil packaging) melalui halaman
pengaturan Skills di Claude.ai / Claude Cowork. Skill akan otomatis
dipertimbangkan Claude berdasarkan `description` di frontmatter — tidak
perlu dipanggil manual, meski perintah mode (`/caveman`, `/ponytail`, dst.)
tetap bisa diketik langsung.

### Claude Code

Salin folder `context-efficiency/` ke direktori skills proyek/
pengguna Claude Code Anda (mis. `.claude/skills/` atau lokasi skills
global sesuai konfigurasi Anda). Claude Code akan memuat `SKILL.md` sesuai
mekanisme skill loading bawaannya.

## Referensi perintah

| Perintah | Modul | Efek |
|---|---|---|
| `/caveman-off` `/caveman-lite` `/caveman` `/caveman-ultra` | 1 — Kompresi | Atur level keringkasan respons. |
| `/ponytail-off` `/ponytail-lite` `/ponytail` `/ponytail-ultra` | 2 — Minimalisme coding | Atur ketatnya ladder minimalisme untuk task coding. |
| `/efficiency-auto` `/efficiency-deep` `/efficiency-ultra` | Preset gabungan | Kombinasi cepat lintas keempat modul (lihat tabel preset di `SKILL.md`). |

Level bersifat persisten sampai diubah lagi, dan tidak diumumkan di setiap
respons (tidak ada "saya sekarang dalam mode caveman" di tiap balasan).

## Catatan kompatibilitas — Claude.ai Skills tanpa hooks/MCP

Empat modul tidak setara dari sisi "seberapa nyata" ia bisa dijalankan pada
host yang hanya mendukung skill-prompt biasa (tanpa hook/MCP/server
runtime):

- **Module 1 (Caveman)** — murni gaya perilaku, berjalan penuh di host
  apa pun.
- **Module 2 (Ponytail)** — berjalan penuh sebagai kebiasaan coding, tetapi
  akses nyata ke repository tetap bergantung pada tool yang disediakan
  host (mis. bash/file tools).
- **Module 3 (Context Mode)** — hanya bisa ditiru sebagai *routing policy*
  memakai tool yang memang tersedia (bash/script/search); ini **bukan** MCP
  server sungguhan dan tidak boleh mengaku sebagai satu.
- **Module 4 (agentmemory)** — hanya bisa menjadi penyimpanan/konvensi
  nyata **jika** host menyediakan mekanisme persistensi yang bisa diakses
  (memory tool, file proyek, dsb.). Tanpa itu, modul ini eksplisit jatuh ke
  kontinuitas sesi-lokal dan mengatakan demikian — tidak berpura-pura ada
  memori permanen.

Skill ini tidak boleh mengklaim melakukan sesuatu yang sebenarnya tidak
bisa dilakukan oleh host tempat ia berjalan.

## Batasan yang diketahui

- Tidak memperbesar context window native model — hanya memperbaiki
  pemanfaatan context yang tersedia.
- Efektivitas Module 3 & 4 bergantung penuh pada tool nyata yang tersedia
  di environment; pada host minimal, keduanya terdegradasi menjadi
  kebijakan/kebiasaan, bukan mekanisme runtime.
- Kompresi (Module 1) dan minimalisme (Module 2) bisa terasa terlalu ringkas
  bagi pengguna yang mengharapkan penjelasan panjang secara default —
  gunakan `/caveman-off` atau `/efficiency-deep` bila itu terjadi.
- Test matrix di `tests/` bersifat manual/deskriptif (prompt + kriteria
  lulus), bukan hasil eksekusi otomatis — pada environment tanpa subagent
  paralel (mis. sesi chat biasa), jalankan satu per satu sesuai kebutuhan.

## Pemeliharaan & versioning

- Perubahan pada `SKILL.md` inti harus tetap menjaga panjang di bawah
  ~500 baris; jika mendekati batas, pindahkan detail baru ke `references/`
  alih-alih memperpanjang inti.
- Saat menambah modul/perintah baru, perbarui tabel perintah di README ini
  **dan** bagian terkait di `SKILL.md` secara bersamaan agar tidak
  berbeda.
- Simpan riwayat perubahan penting (apa yang berubah dan kenapa) di bagian
  ini agar revisi berikutnya tidak mengulang diskusi yang sama:

  ```
  ## Changelog
  - 2026-08-12: Rilis awal — sintesis 4 modul (Caveman, Ponytail,
    Context Mode, agentmemory) sebagai satu orchestration layer.
  ```

- Jika deskripsi frontmatter `SKILL.md` mulai kurang akurat mewakili
  perilaku skill (mis. modul baru ditambah tapi trigger belum disebut),
  perbarui deskripsi tersebut — itu mekanisme utama yang menentukan kapan
  Claude mempertimbangkan skill ini.

## Sumber

Sintesis ini disusun berdasarkan dokumentasi publik proyek-proyek berikut
(prinsip & alur kerja, bukan teks/kode sumber):

- `JuliusBrussee/caveman`
- `DietrichGebert/ponytail`
- `mksglu/context-mode`
- `rohitg00/agentmemory`
