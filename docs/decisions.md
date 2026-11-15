# Decision Log

| ID | Tanggal | Keputusan | Alasan |
|---|---|---|---|
| D1 | 2026-10-30 | Repo Android terpisah dari backend | Build system, IDE, dan konteks AI berbeda |
| D2 | 2026-10-30 | Clean Architecture + MVVM | Standar industri; logika terpisah dari UI |
| D3 | 2026-10-30 | DI memakai Hilt **[Usulan]** | Standar industri Android; ganti ke Koin bila terlalu berat (putuskan di A0) |
| D4 | 2026-10-30 | Predictor sementara = Poisson, bukan angka acak | Hasil masuk akal, bisa diuji, jadi baseline untuk TFLite |
| D5 | 2026-10-30 | UI dibangun sebelum model TFLite siap | UI dan ML bisa dikerjakan paralel |
| D6 | 2026-10-30 | Kontrak API bersumber dari repo backend | Satu sumber kebenaran |

Tambahkan nomor versi library yang dipilih di A0 pada tabel di bawah.

## Versi library (isi saat A0)
| Library | Versi | Dicek tanggal |
|---|---|---|
| (isi) | | |
