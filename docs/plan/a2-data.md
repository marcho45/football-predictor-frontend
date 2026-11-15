# A2 — Data layer (Retrofit ke backend)

Status: **Pending** — butuh A1 Approved dan backend M1 berjalan.

| Slice | Isi | Status |
|---|---|---|
| A2a | Tambah dependency Retrofit/OkHttp/serialization (minta izin dulu) | [ ] |
| A2b | `TeamDto`, `TeamMapper` + unit test mapper | [ ] |
| A2c | `TeamApi` (`GET api/teams`), konfigurasi Retrofit, BASE_URL via BuildConfig | [ ] |
| A2d | `TeamRepositoryImpl` (Result, tangani HTTP error dan IOException) + test MockWebServer | [ ] |
| A2e | Izin INTERNET + `network_security_config` khusus debug | [ ] |
| A2f | Uji nyata: emulator menarik 20 tim dari backend (menuntaskan **M1f** di repo backend) | [ ] |

Acceptance: log/UI sementara menampilkan 20 tim dari backend; test hijau; cleartext HANYA di debug.
