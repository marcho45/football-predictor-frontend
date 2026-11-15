# Setup Lokal

## Prasyarat
- Android Studio versi stabil terbaru, JDK bawaan Android Studio
- Emulator (AVD) atau HP fisik
- Backend `football-predictor-api` berjalan (lihat repo backend)

## Menjalankan
1. Jalankan backend: `.\kotlin.bat run` (PowerShell) di repo backend.
2. Jalankan app dari Android Studio (tombol Run).

## Menghubungkan ke backend
| Perangkat | Base URL |
|---|---|
| Emulator | `http://10.0.2.2:8080/` (bukan `localhost`) |
| HP fisik | `http://<IP-LAN-komputer>:8080/`, satu Wi-Fi; backend harus `host: 0.0.0.0` |

HTTP (bukan HTTPS) hanya diizinkan untuk **debug** lewat `network_security_config` di source set `debug`. Jangan aktifkan cleartext untuk release.

## Gate sebelum commit
```
gradlew.bat assembleDebug testDebugUnitTest lintDebug
```
