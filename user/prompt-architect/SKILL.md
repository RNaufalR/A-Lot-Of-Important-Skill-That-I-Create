---
name: prompt-architect
description: Merancang, memperbaiki, atau mengoptimalkan prompt secara terstruktur dan presisi. Gunakan otomatis saat user minta dibuatkan prompt, minta prompt diperbaiki/dikembangkan/dioptimalkan, memberi ide/brief mentah untuk diubah jadi prompt, atau butuh prompt untuk AI text, image, video, coding, research, agent, maupun automation tools.
---

# Prompt Architect

Ubah intent user menjadi instruksi yang jelas, spesifik, terstruktur, dan actionable — bukan sekadar memperpanjang teks. Prinsip utama: **maximum useful specificity, minimum unnecessary verbosity.**

## Workflow

1. **Identify Intent** — tujuan utama, output yang diinginkan, target/audiens output, model/platform tujuan (jika diketahui).
2. **Extract Requirements** — pisahkan explicit vs implicit requirements: style, format, tone, constraint, technical requirement, referensi, acceptance criteria.
3. **Detect Missing Info** — cari info penting yang hilang. Jangan tanya hal tidak material. Kalau bisa diisi dengan asumsi wajar, gunakan asumsi dan tandai bila perlu — jangan membanjiri user dengan pertanyaan untuk brief minim.
4. **Design Architecture** — pilih struktur sesuai kebutuhan tugas, dari komponen berikut (pakai yang relevan saja, jangan paksakan semua):
   Role/Context, Objective, Input, Requirements, Constraints, Process/Method, Output Specification, Quality Criteria, Edge Cases, Examples, Verification/Self-check.
5. **Optimize** — cek: ambiguity, instruksi konflik, verbosity tak perlu, constraint hilang, istilah kabur, redundansi, requirement tidak realistis, potensi salah tafsir, kompatibilitas dengan model/tool tujuan.
6. **Finalize** — beri prompt final siap copy-paste.

## Adaptive Detail
- **Simple** → prompt singkat, langsung.
- **Moderate** → terstruktur: requirements + output specification.
- **Complex** → lengkap: context, objective, constraints, workflow, edge cases, quality criteria, verification.

Jangan buat prompt panjang kalau tugasnya sederhana.

## Model-Specific Optimization
- Model/platform disebutkan → sesuaikan dengan karakteristik & kemampuan model itu, jangan mengarang fitur yang tidak pasti ada.
- Model tidak diketahui → buat model-agnostic; kalau perbedaan model sangat berpengaruh, sebutkan asumsinya.

**Domain checklist (pakai sesuai kebutuhan, bukan checklist wajib semua):**
- *Image/video*: subject, composition, environment, camera, lighting, materials, visual style, mood, color, perspective, motion, temporal consistency, negative constraints.
- *Coding*: context, objective, tech stack, architecture, requirements, constraints, compatibility, error handling, edge cases, testing, acceptance criteria.
- *Research*: objective, scope, source requirements, methodology, evidence standards, uncertainty handling, citation requirements, output structure.
- *Writing*: audience, purpose, tone, structure, style, length, content requirements, prohibited elements, quality criteria.

## Self-Review (sebelum kirim)
1. Prompt benar-benar mencapai tujuan user?
2. Ada instruksi ambigu atau saling bertentangan?
3. Ada requirement penting yang terlewat?
4. Semua detail relevan (tidak ada yang cuma menambah panjang)?
5. Output format sudah jelas?
6. Prompt bisa langsung dipakai (copy-paste ready)?

Perbaiki dulu kalau ada masalah, baru kirim.

## Output Behavior
1. Prompt final dalam format siap copy-paste (gunakan code block).
2. Penjelasan singkat pendekatan/asumsi penting — hanya jika memang perlu, jangan berteori panjang soal prompt engineering.
3. Jika user cuma minta "buatkan prompt", utamakan prompt final di atas penjelasan.
