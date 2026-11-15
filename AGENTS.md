# AGENTS.md — Aturan Kerja AI (football-predictor-frontend)

> Dibaca otomatis oleh Antigravity (dan agent lain). Untuk Cline/Roo, salin ke `.clinerules`.

## 0. Wajib baca sebelum menulis kode (urutan)
1. `docs/00-project-overview.md` — gambaran seluruh proyek (ML + backend + Android).
2. `docs/build-spec.md` — sumber kebenaran Android (scope, stack, kontrak, "Jangan dilakukan").
3. `docs/plan/` — buka milestone berstatus **In Progress**. Hanya itu yang boleh dikerjakan.
4. `docs/project-structure.md` — sebelum membuat/memindah file.
5. `docs/progress.md` — lihat pekerjaan terakhir.
6. `docs/open-questions.md` — jangan memutuskan sendiri hal yang masih terbuka.

## 1. Alur kerja per tugas
1. Kerjakan **satu slice** dari milestone aktif.
2. Jalankan gate: `gradlew.bat assembleDebug testDebugUnitTest lintDebug` (Windows) / `./gradlew ...` (macOS/Linux). Harus hijau.
3. Catat di `docs/progress.md` **SEBELUM** melapor.
4. Centang slice di file milestone.
5. Lapor ringkas (file berubah, hasil gate), lalu **BERHENTI dan tunggu "lanjut"**.
6. Jangan lompat ke slice/milestone berikutnya tanpa persetujuan.

## 2. Aturan arsitektur (Clean Architecture + MVVM)
- Arah dependency: `presentation -> domain <- data`.
- **Domain** = Kotlin murni. DILARANG import `android.*`, Compose, Retrofit, TFLite.
- **Composable** hanya menampilkan state dan mengirim event. DILARANG: panggilan jaringan, kalkulasi prediksi, akses TFLite di Composable.
- **ViewModel** menyimpan state (`StateFlow`), memanggil use case. Tidak mengimpor Retrofit/TFLite langsung.
- DTO (data layer) tidak boleh bocor ke UI; selalu dipetakan ke model domain lewat mapper.
- Prediksi hanya lewat interface `MatchPredictor`.

## 3. Aturan kode
- Kotlin, Jetpack Compose + Material 3, Coroutines + Flow.
- Tidak ada string UI hardcode panjang di Composable; pakai `strings.xml`.
- Tidak ada API key/secret di kode. Base URL lewat `BuildConfig`/konfigurasi, bukan tersebar.
- Komentar singkat berbahasa Indonesia pada fungsi publik; nama kode berbahasa Inggris.
- Ubah seminimal mungkin; jangan refactor file di luar tugas.

## 4. Jangan dilakukan
- Jangan mengerjakan milestone **Pending**.
- Jangan menambah/mengubah dependency (`libs.versions.toml`, `app/build.gradle.kts`) atau plugin tanpa izin. Bila perlu, **hentikan dan tanyakan**, sertakan alasan dan versi stabil.
- Jangan mengubah kontrak API (`docs/build-spec.md` §5). Sumber kebenarannya ada di repo backend.
- Jangan mengubah `gradlew`, `gradlew.bat`, folder `gradle/wrapper`, atau konfigurasi signing.
- Jangan membuat file sementara yang tertinggal (skrip, dump). Hapus sebelum melapor.
- Jangan commit secret atau `local.properties`.
- Jangan menebak: spesifikasi ambigu -> **tanya pengguna**.

## 5. Konvensi commit
`feat(a2): ...` · `fix(a2): ...` · `test(a1): ...` · `docs: ...` · `chore: ...`
