---
name: rilis
description: >
  Disiplin penomoran versi dan dokumentasi rilis: tentukan nomor versi tanpa
  bingung (SemVer MAJOR.MINOR.PATCH plus pohon keputusan), CHANGELOG yang
  selalu sinkron (Keep a Changelog, bagian Unreleased), dan README yang tidak
  basi. Dipakai setiap ada perubahan atau update pada aplikasi, library, atau
  skill: sebelum commit fitur/fix, saat menyiapkan rilis, atau saat versi di
  beberapa tempat tidak sama.
  Trigger chat: "versi berapa", "bump versi", "rilis", "changelog",
  "update versi", "naik versi", "versioning".
license: MIT
version: 1.0.0
---

# rilis

> Satu aturan versi, satu sumber kebenaran, satu langkah rilis.
> Bingung menentukan nomor versi biasanya bukan soal angkanya, tapi soal
> tidak adanya aturan yang diputuskan sekali dan dipakai selamanya.

## 1. Aturan versi: SemVer (putuskan sekali, pakai selamanya)

Format: `MAJOR.MINOR.PATCH`, contoh `2.4.1`. Tentukan dengan pohon keputusan
ini, jawab berurutan dari atas:

1. **Apakah perubahan ini merusak cara lama?** API berubah tidak kompatibel,
   format data berubah, perilaku yang diandalkan user hilang, user wajib
   menyesuaikan sesuatu → naikkan **MAJOR**, reset MINOR dan PATCH ke 0.
   Contoh: `2.4.1` → `3.0.0`
2. **Tidak merusak. Apakah ada kemampuan baru?** Fitur baru, endpoint baru,
   opsi baru, semuanya kompatibel ke belakang → naikkan **MINOR**, reset PATCH
   ke 0. Contoh: `2.4.1` → `2.5.0`
3. **Tidak ada kemampuan baru, cuma perbaikan?** Bug fix, perbaikan performa,
   refactor internal, dokumentasi, typo → naikkan **PATCH**.
   Contoh: `2.4.1` → `2.4.2`

Selesai. Tidak ada pengecualian di luar pohon ini. Ragu antara dua jawaban?
Ambil yang lebih besar, itu lebih jujur ke user.

### Versi 0.x (belum stabil)

Selama MAJOR masih 0, API belum janji stabil:

- Merusak atau ada fitur baru → naikkan MINOR (`0.4.2` → `0.5.0`)
- Perbaikan → naikkan PATCH (`0.4.2` → `0.4.3`)
- Naik ke `1.0.0` adalah janji publik pertama: "API ini stabil, aturan MAJOR
  berlaku penuh mulai sekarang."

### Suffix pra-rilis (bila perlu)

`1.5.0-alpha.1` < `1.5.0-beta.1` < `1.5.0-rc.1` < `1.5.0`. Pakai hanya bila
memang ada tahap uji publik. Jangan untuk rilis internal biasa.

## 2. Satu sumber kebenaran

- Nomor versi tinggal di **SATU tempat**: `package.json`, `pyproject.toml`,
  file `VERSION`, frontmatter `version` di `SKILL.md` (untuk skill), atau git
  tag. Pilih satu per project dan tulis pilihannya di README project atau ADR.
- Tempat lain (badge README, dokumentasi, halaman unduhan) **mengacu** ke
  sumber itu. Jangan menyimpan angka sendiri yang bisa basi.
- Cek sinkron sebelum rilis: versi di sumber = versi teratas di CHANGELOG =
  badge README = tag yang akan dibuat. Beda satu saja, rilis ditunda.

## 3. CHANGELOG: tulis saat berubah, bukan saat rilis

Format: Keep a Changelog.

- Paling atas selalu ada bagian `## [Unreleased]`.
- **Setiap perubahan dicatat di Unreleased pada commit yang sama**, di
  kategori yang benar:
  - `Added`: kemampuan baru
  - `Changed`: perilaku berubah
  - `Deprecated`: akan dihapus, masih ada
  - `Removed`: dihapus
  - `Fixed`: bug diperbaiki
  - `Security`: perbaikan keamanan
- Entri ditulis untuk manusia: apa yang berubah dan dampaknya ke user.
  Bukan salinan pesan commit, bukan dump `git log`.
- Saat rilis: ubah judul `## [Unreleased]` menjadi `## [X.Y.Z] - YYYY-MM-DD`,
  lalu buat `## [Unreleased]` kosong baru di atasnya. Isi entri tidak diubah.

## 4. Langkah rilis (atomik, satu napas)

1. Tentukan nomor baru dari isi Unreleased pakai pohon keputusan bagian 1.
   Unreleased berisi Removed atau Changed yang merusak → MAJOR. Berisi Added
   → minimal MINOR. Hanya berisi Fixed → PATCH.
2. Perbarui nomor di sumber kebenaran.
3. Pindahkan Unreleased menjadi [versi] - tanggal hari ini. Buat Unreleased
   baru yang kosong.
4. Perbarui badge atau penyebutan versi di README.
5. Commit satu kali: `vX.Y.Z: <ringkasan rilis>`. Buat git tag `vX.Y.Z` bila
   project memakai tag.

Tidak ada rilis setengah: bump tanpa changelog, tag tanpa bump, atau entri
rilis tanpa tanggal, semuanya pelanggaran.

## 5. Kapan tanya user, kapan putuskan sendiri

- Pohon keputusan itu mekanis: MINOR dan PATCH putuskan sendiri.
- **MAJOR selalu konfirmasi ke user.** Breaking change adalah keputusan
  produk, bukan keputusan teknis.
- Klasifikasi ambigu (fitur atau fix? merusak atau tidak?) dan jawabannya
  mengubah nomor → tanya singkat dengan opsi. Jangan mengarang klasifikasi.

## Anti-pattern

- Nomor versi naik tanpa entri changelog (rilis diam-diam).
- Changelog ditulis saat rilis dari ingatan, setengah perubahan hilang.
- PATCH untuk fitur baru, atau MAJOR untuk sekadar fix.
- Versi di README, package, dan tag beda beda.
- Penomoran tanggal atau acak ("versi final2") tanpa aturan tertulis.
- Satu rilis melompat dua tingkat (`1.2.0` langsung `1.4.0`).
- Tanggal rilis di changelog tidak sama dengan tanggal tag.

## Do & Don't

| ✅ DO | ❌ DON'T |
|---|---|
| Ikuti pohon keputusan SemVer | Menebak nomor dari perasaan |
| Catat perubahan di Unreleased pada commit yang sama | Menulis changelog dari ingatan saat rilis |
| Satu sumber kebenaran versi | Menyimpan angka versi di banyak tempat |
| Rilis atomik: bump + changelog + README + tag | Bump versi tanpa changelog |
| Konfirmasi ke user untuk MAJOR | Diam-diam menaikkan MAJOR |
| Entri changelog soal dampak ke user | Copy-paste pesan commit ke changelog |

## Kapan skill ini dipakai

- Otomatis oleh agent: setiap selesai fitur/fix yang akan dirilis, setiap
  menyiapkan rilis, setiap melihat versi tidak sinkron antar tempat.
- Dipicu dari chat: "versi berapa", "bump versi", "rilis", "changelog",
  "naik versi".

## Relasi opsional (bukan dependensi)

Skill ini mandiri. Ia melengkapi `seperlunya` (disiplin agent umum) dan
`ponytail` (kode ramping), bukan syarat.

## Kompatibilitas

Markdown murni, SKILL.md format standar. Tanpa hooks, plugin, MCP, atau script.
Install: copy folder `rilis/` ke direktori skill masing-masing harness.
