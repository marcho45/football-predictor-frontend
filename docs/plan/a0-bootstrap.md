# A0 — Bootstrap

Status: **In Progress**

Tujuan: proyek Compose kosong berjalan di emulator, struktur paket siap, repo dan dokumen terpasang.

| Slice | Isi | Status |
|---|---|---|
| A0a | Buat repo GitHub `football-predictor-frontend` (tanpa README/.gitignore otomatis) | [ ] |
| A0b | Android Studio: New Project > Empty Activity (Compose), package `com.portfolio.footballpredictor`, minSdk sesuai Q5 | [ ] |
| A0c | Jalankan di emulator: layar template tampil | [ ] |
| A0d | `git init`, `.gitattributes`, commit awal, push | [ ] |
| A0e | Salin `AGENTS.md` dan folder `docs/` dari paket dokumen | [ ] |
| A0f | Putuskan Q1 (Hilt/Koin), cek versi library ke sumber resmi, catat di `decisions.md` | [ ] |
| A0g | Pasang plugin dan dependency dasar (Hilt, ViewModel Compose, serialization, test) + Sync | [ ] |
| A0h | Buat kerangka paket kosong (`domain`, `data`, `presentation`, `di`) | [ ] |

## Acceptance
1. App jalan di emulator, `gradlew.bat assembleDebug testDebugUnitTest lintDebug` hijau.
2. Dokumen terpasang, `progress.md` terisi, push ke GitHub.
3. Q1 dan Q5 terjawab.

## Catatan
Dependency Retrofit dan TFLite BELUM dipasang di A0; dipasang saat milestone yang memakainya (A2, A4).
