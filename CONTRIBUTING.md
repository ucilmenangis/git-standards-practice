# Contributing Guide: PMM AutoCare

Dokumen ini adalah kesepakatan tim tentang cara memakai Git di proyek PMM AutoCare, sistem informasi servis mobil bengkel PMM. Semua anggota, termasuk anggota baru, wajib membacanya sebelum membuat commit pertama.

## Tentang Proyek

| Bagian | Teknologi | Repository |
|--------|-----------|------------|
| Panel web admin dan pemilik, REST API | Laravel 13, PHP 8.3, Tailwind CSS, MySQL | `pmmauto-web` |
| Aplikasi pelanggan | Flutter (Dart), target Android | `pmmauto_mobile` |
| Pengujian | PHPUnit (feature test), `flutter test` | di masing-masing repository |

Branch `main` selalu berisi kode yang siap dipakai demo. Semua perubahan masuk lewat Pull Request.

## Branch Convention

Format: `<type>/<deskripsi-singkat>`

| Type | Kegunaan | Contoh di proyek ini |
|------|----------|----------------------|
| `feature/` | Fitur baru | `feature/45-booking-slot-servis` |
| `fix/` | Perbaikan bug | `fix/total-tagihan-nota` |
| `hotfix/` | Perbaikan darurat di server produksi | `hotfix/login-api-500` |
| `refactor/` | Merapikan kode tanpa mengubah perilaku | `refactor/pesanan-service` |
| `docs/` | Dokumentasi | `docs/api-reference` |
| `test/` | Menambah atau memperbaiki test | `test/pembayaran-flow` |
| `chore/` | Pemeliharaan, dependency, konfigurasi | `chore/update-flutter-deps` |

**Aturan:**
* Huruf kecil semua.
* Pisahkan kata dengan tanda hubung (`-`), bukan spasi atau underscore.
* Sertakan nomor issue di depan bila ada, contoh `feature/45-booking-slot-servis`.
* Maksimal 50 karakter.
* Selalu dibuat dari `main` terbaru.

| Benar | Salah | Alasan salah |
|-------|-------|--------------|
| `feature/12-riwayat-servis` | `Feature/RiwayatServis` | Huruf besar, tanpa pemisah |
| `fix/kuota-slot-penuh` | `fix_kuota slot` | Underscore dan spasi |
| `docs/cara-install` | `irfan-coba` | Tidak ada type, nama orang bukan deskripsi |

## Commit Convention

Mengikuti [Conventional Commits](https://www.conventionalcommits.org).

Format: `<type>(<scope>): <subject>`

**Type:** `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

**Scope** memakai nama modul PMM AutoCare: `auth`, `booking`, `pesanan`, `pembayaran`, `sparepart`, `mekanik`, `laporan`, `api`, `mobile`, `readme`.

**Aturan:**
* Subject huruf kecil, kalimat perintah (imperative mood), tanpa titik di akhir.
* Header maksimal 72 karakter, usahakan di bawah 50.
* Satu commit untuk satu perubahan (commit atomik).
* Body dipisah satu baris kosong dari header, jelaskan *kenapa*, bukan *apa*.
* Tautkan issue di footer: `Closes #123`.

**Contoh benar:**
```
feat(booking): add daily quota check for service slot
fix(pembayaran): reject payment when order is cancelled
docs(readme): update local setup steps
test(auth): add feature test for expired token
refactor(pesanan): extract status transition to service
```

**Contoh salah:**

| Commit message | Masalah |
|----------------|---------|
| `update` | Tidak ada type dan tidak jelas apa yang berubah |
| `Fix Bug Login.` | Huruf besar, tanpa scope, diakhiri titik |
| `wip` | Commit sementara, tidak boleh masuk riwayat |
| `feat: tambah booking, fix nota, ubah readme` | Tiga perubahan dalam satu commit |

Format ini dicek otomatis oleh commitlint dan Husky (hook `commit-msg`). Commit yang tidak sesuai akan ditolak sebelum tercatat.

## Pull Request

* Judul PR memakai format commit message, contoh `feat(booking): add daily quota check for service slot`.
* Ukuran PR di bawah 400 baris perubahan. Fitur besar dipecah jadi beberapa PR.
* Isi deskripsi PR dengan template berikut:

```markdown
## Deskripsi
Apa yang berubah dan kenapa.

## Perubahan
* [x] file atau modul yang diubah

## Tipe Perubahan
* [ ] Bug fix
* [ ] Fitur baru
* [ ] Breaking change

## Checklist
* [ ] `php artisan test` atau `flutter test` lulus
* [ ] Self review
* [ ] Tidak ada konflik dengan main

## Referensi
Closes #
```

* Wajib minimal 1 approval dari anggota lain.
* Wajib lolos test sebelum merge.
* Merge memakai **Squash and merge** agar riwayat `main` tetap satu commit per fitur.
* Hapus branch setelah merge.

## Code Review

Reviewer memberi label di awal komentar:

| Label | Arti |
|-------|------|
| `blocking:` | Harus diperbaiki sebelum merge |
| `nit:` | Saran kecil, boleh diabaikan |
| `question:` | Butuh penjelasan dari penulis |
| `praise:` | Apresiasi bagian yang bagus |

Contoh: `blocking: validasi nomor HP belum menolak huruf, tolong tambahkan test-nya juga.`

Review fokus pada kode, bukan orangnya. Penulis PR menjawab setiap komentar `blocking:` sebelum minta review ulang.

## Workflow

1. `git checkout main && git pull origin main`
2. `git checkout -b feature/45-booking-slot-servis`
3. Kerjakan perubahan, commit kecil dan atomik.
4. Jalankan test: `php artisan test` (web) atau `flutter test` (mobile).
5. `git push -u origin feature/45-booking-slot-servis`
6. Buat Pull Request di GitHub dengan template di atas.
7. Tunggu review dan perbaiki komentar `blocking:`.
8. Squash and merge.
9. Hapus branch di GitHub dan di lokal: `git branch -D feature/45-booking-slot-servis`.

## Yang Dilarang

* Push langsung ke `main`.
* Commit message tidak jelas seperti `wip`, `test`, `asdf`, `update`.
* Commit file `.env`, `vendor/`, `node_modules/`, `build/`, file kredensial, atau API key.
* Rebase atau force push branch yang sudah dipakai anggota lain.
* Merge PR sendiri tanpa review.
