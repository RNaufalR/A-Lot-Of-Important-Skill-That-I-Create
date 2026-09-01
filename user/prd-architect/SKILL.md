---
name: prd-architect
description: Gunakan skill ini saat user minta dibuatkan, disusun, direvisi, atau
  dieksekusi PRD (Product Requirement Document) — termasuk saat user memberi
  brief produk/fitur mentah dan minta diubah jadi requirement terstruktur,
  atau saat user minta implementasi/kode dibangun berdasarkan PRD yang sudah
  ada (baik ditulis di chat maupun diupload sebagai file). Aktifkan juga saat
  user menyebut "PRD", "requirement doc", "spek produk/fitur", "breakdown
  requirement", atau minta fitur besar dipecah jadi task teknis sebelum
  dikerjakan. Jangan lewati skill ini untuk brief mentah yang terlihat
  sederhana — brief yang tidak diformalkan ke PRD adalah sumber utama
  requirement yang terlewat dan implementasi yang salah paham.
---

# PRD Architect

Skill ini punya dua pekerjaan yang harus selalu berjalan berurutan: **menyusun
PRD yang lengkap** dari brief mentah, lalu **mengeksekusinya menjadi
implementasi** tanpa salah paham requirement. Jangan pernah lompat langsung
ke coding dari brief mentah — brief yang belum diformalkan menyembunyikan
ambiguitas yang baru ketahuan setelah setengah jalan implementasi, dan itu
mahal untuk diperbaiki.

**Relasi dengan skill lain:** skill ini mengatur *apa yang harus benar secara
requirement* (kelengkapan PRD, kejelasan scope, kepatuhan implementasi
terhadap requirement). Skill ini tidak menggantikan disiplin eksekusi teknis
lain — lihat bagian Auto-Orchestration di bawah untuk kapan skill lain wajib
dipanggil bersamaan.

## Alur Kerja (jangan lewati tahap)

```
Capture Intent
  → Draft PRD (tandai bagian yang diasumsikan)
  → Clarification Round (1 batch, blocking-only)
  → Finalize PRD
  → Comprehension Pass (reread utuh + checklist)
  → Deteksi & panggil skill relevan
  → Execute
  → Verify (audit checklist, stop-slop, laporkan gap)
```

Setiap tahap boleh cepat kalau brief sudah jelas, tapi jangan dihilangkan
diam-diam. Kalau user hanya minta "buatkan PRD" tanpa minta eksekusi, alur
berhenti setelah Finalize PRD — jangan mengeksekusi tanpa diminta.

---

## Fase 1: PRD Creation

Susun PRD dari brief mentah user. Gunakan template ini sebagai kerangka
wajib, tapi sesuaikan kedalaman tiap bagian dengan skala fitur — fitur kecil
tidak butuh bagian risiko lima paragraf.

```markdown
# PRD: [Nama Fitur/Produk]

## 1. Problem Statement
Masalah apa yang sedang diselesaikan, untuk siapa, kenapa penting sekarang.

## 2. Tujuan & Non-Tujuan
- Tujuan: hasil yang ingin dicapai (measurable kalau bisa)
- Non-Tujuan: batas eksplisit apa yang SENGAJA tidak dikerjakan di scope ini

## 3. Target User / Persona
Siapa pengguna, konteks pemakaian mereka.

## 4. Functional Requirements
Daftar kemampuan sistem, per-item bisa ditelusuri ke user story terkait.

## 5. Non-Functional Requirements
Performa, keamanan, skalabilitas, aksesibilitas, kompatibilitas — apa pun
yang relevan untuk fitur ini (jangan isi generik kalau tidak relevan).

## 6. User Stories & Acceptance Criteria
Format: "Sebagai [peran], saya ingin [aksi], supaya [manfaat]."
Setiap story punya acceptance criteria yang bisa diverifikasi ya/tidak.

## 7. Batasan Teknis
Stack, constraint infrastruktur, dependency eksternal yang mengikat desain.

## 8. Dependency
Sistem/tim/API lain yang harus siap sebelum atau selama implementasi.

## 9. Metrik Sukses
Bagaimana kita tahu fitur ini berhasil setelah dirilis.

## 10. Risiko
Hal yang bisa gagal, dan mitigasinya kalau ada.

## 11. Milestone
Tahapan pengerjaan kalau scope-nya cukup besar untuk dipecah.

## 12. Out-of-Scope
Eksplisit: apa yang berdekatan dengan fitur ini tapi TIDAK termasuk.
```

Saat brief mentah tidak menyebut sesuatu (misalnya tech stack, target
platform), **jangan biarkan bagian itu kosong dan jangan diam-diam
mengarang**. Isi dengan asumsi paling wajar berdasarkan konteks yang ada, dan
tandai jelas:

```
[ASUMSI] Tech stack: mengikuti stack existing project (belum disebutkan user).
```

Bagian yang ditandai `[ASUMSI]` inilah yang jadi kandidat pertanyaan
klarifikasi di fase berikutnya — bukan otomatis ditanyakan, tergantung apakah
dia blocking atau tidak.

---

## Fase 2: Clarification Protocol

Kumpulkan semua ambiguitas dari draft PRD, lalu klasifikasikan:

- **Blocking** — kalau salah asumsi di sini, implementasi bisa error, salah
  arah total, atau perlu dibongkar ulang. Contoh: model data inti, batas
  akses/otorisasi, requirement yang saling kontradiksi, target platform yang
  menentukan arsitektur.
- **Non-blocking** — bisa diasumsikan wajar tanpa risiko besar kalau meleset.
  Contoh: nama variabel, warna tombol, copy microcopy, urutan field di form.

Aturan keras: **jangan tanya soal kosmetik**, dan **jangan tanya satu-satu**.
Semua pertanyaan blocking digabung jadi **satu batch** pertanyaan yang
diajukan ke user sekali jalan. Untuk tiap item non-blocking, langsung tulis
asumsinya di PRD (`[ASUMSI]`) dan lanjut — jangan menunggu konfirmasi.

Kalau setelah didata ternyata tidak ada item blocking sama sekali, lewati
fase ini dan langsung ke Finalize PRD — jangan memaksa membuat pertanyaan
demi formalitas.

---

## Fase 3: Finalize PRD

Masukkan jawaban user (kalau ada clarification round) ke PRD, hilangkan
placeholder yang sudah terjawab, tapi **pertahankan tag `[ASUMSI]`** untuk
item non-blocking yang diasumsikan — supaya siapa pun yang membaca PRD tahu
mana bagian yang solid dan mana yang masih bisa berubah.

Kalau user minta PRD saja (tanpa eksekusi), berhenti di sini dan serahkan
dokumennya. Kalau file PRD perlu diekspor ke Word, ini titik untuk memanggil
skill `docx` (lihat Auto-Orchestration).

---

## Fase 4: Comprehension Pass

Sebelum baris kode pertama ditulis, PRD final **wajib dibaca utuh** — bukan
skimming per-heading. Ini bukan formalitas: requirement yang saling
berhubungan sering ada di bagian berbeda (misal acceptance criteria di
bagian 6 yang mengunci detail di functional requirement bagian 4), dan
skimming melewatkan koneksi itu.

Setelah reread utuh, buat **requirement-to-implementation checklist**: satu
baris per requirement/acceptance criteria yang bisa ditelusuri, dengan status
belum-dikerjakan. Checklist ini yang jadi acuan audit di fase Verify —
requirement yang tidak ada di checklist berarti requirement itu terlewat di
comprehension pass, bukan sekadar belum sempat.

```
[ ] FR-1: ...
[ ] FR-2: ...
[ ] AC (US-1): ...
[ ] NFR-1: ...
```

### PRD panjang

Kalau PRD terlalu panjang untuk dipegang utuh dalam satu pass, jangan
membaca sepotong-sepotong tanpa strategi. Baca per-bagian sesuai urutan
template di atas (problem → requirements → acceptance criteria → batasan),
susun checklist gabungan sambil jalan, lalu di akhir baca ulang khusus
bagian 4–6 (functional requirements, NFR, user stories) sekali lagi untuk
menangkap requirement yang saling silang antar bagian — tiga bagian ini yang
paling sering punya dependensi tersembunyi satu sama lain.

---

## Fase 5: Auto-Orchestration

Sebelum eksekusi, deteksi kebutuhan yang melibatkan skill lain dan panggil
tanpa menunggu user memintanya eksplisit. Jangan reimplementasi logika skill
lain dari nol — panggil skill-nya.

| Sinyal di PRD/brief | Skill yang dipanggil |
|---|---|
| PRD perlu diekspor jadi dokumen Word | `docx` |
| Project yang dibangun adalah game (engine apa pun) | `game-studio-assistant` |
| Task besar, berisiko tinggi, atau multi-langkah | `agentic-generalist` |
| Butuh disiplin token/context untuk sesi panjang atau coding | `context-efficiency` |
| Polish teks akhir sebelum diserahkan ke user | `stop-slop` |

Deteksi ini jalan di dua titik: sesaat setelah Finalize PRD (untuk kebutuhan
seperti ekspor dokumen atau penyelarasan domain), dan sesaat sebelum Execute
(untuk kebutuhan disiplin eksekusi). Kalau tidak ada sinyal yang cocok,
lanjut tanpa memanggil skill tambahan — jangan memaksakan orkestrasi yang
tidak relevan.

---

## Fase 6: Execution

Implementasikan mengikuti PRD final, bukan mengikuti ingatan soal brief awal
— kalau ada perbedaan antara draft PRD dan hasil clarification, PRD final
yang menang.

Update status di requirement-to-implementation checklist setiap kali satu
item selesai. Kalau di tengah implementasi ternyata sebuah requirement
ambigu atau kontradiktif dengan requirement lain (lihat Edge Cases di
bawah), berhenti sejenak untuk menyelesaikannya — jangan menebak diam-diam
dan lanjut, karena itu justru sumber error yang skill ini dibuat untuk
dicegah.

---

## Fase 7: Verify

Sebelum menyatakan task selesai:

1. **Audit checklist vs hasil** — telusuri tiap baris checklist, tandai
   selesai/belum, dengan bukti konkret (file, fungsi, atau bagian kode yang
   memenuhinya), bukan asumsi "harusnya sudah kepenuhi".
2. **Jalankan pass `stop-slop`** pada output teks final (dokumentasi, pesan
   ke user, PRD itu sendiri kalau relevan) — jangan menulis ulang aturan
   anti-slop di sini, panggil skill-nya.
3. **Laporkan requirement yang belum terpenuhi secara eksplisit.** Kalau ada
   gap, sebutkan item mana dan kenapa (out-of-scope keputusan, blocker
   teknis, atau memang belum sempat) — jangan disembunyikan atau dibungkus
   supaya terdengar selesai.

Task hanya dianggap selesai kalau checklist sudah diaudit. Kalau user hanya
minta sebagian requirement dikerjakan duluan, itu sah — tapi tetap laporkan
sisa checklist yang belum dikerjakan, jangan diam-diam dianggap keluar
scope.

---

## Edge Cases

**Requirement saling kontradiktif.** Jangan pilih salah satu secara diam-diam.
Kalau ketemu saat Draft PRD atau Comprehension Pass, jadikan item blocking di
Clarification Protocol. Kalau baru ketemu saat Execution (PRD sudah final),
hentikan implementasi bagian itu, laporkan kontradiksinya ke user dengan
kedua sisi requirement yang bentrok, minta keputusan sebelum lanjut.

**PRD berubah di tengah implementasi.** Jangan menimpa PRD lama begitu saja.
Tandai versi baru (`v1`, `v2`, dst. atau tanggal), catat ringkas apa yang
berubah dan requirement mana yang terdampak, lalu update
requirement-to-implementation checklist mengikuti versi baru — item yang
sudah selesai berdasarkan requirement lama yang sekarang berubah perlu
ditinjau ulang, bukan otomatis dianggap masih valid.

**Requirement tanpa info krusial (mis. tech stack tidak disebut).** Ini masuk
pola `[ASUMSI]` di Fase 1 — default ke konteks yang paling wajar (stack
existing project kalau ada, atau pilihan paling umum untuk kasus tersebut),
tandai eksplisit, dan jadikan blocking hanya kalau pilihan itu menentukan
arsitektur secara fundamental (misal: web vs mobile native mengubah semua
functional requirement; nama package tidak).

**PRD terlalu panjang untuk dibaca sekali.** Lihat strategi chunking di Fase
4 — baca berurutan sesuai struktur template, bukan lompat-lompat, dan lakukan
pass kedua khusus untuk bagian yang paling sering punya dependensi silang.
