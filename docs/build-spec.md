# Build Spec — Football Predictor Android

Sumber kebenaran sisi Android. Konflik dengan dokumen lain: dokumen ini menang.

## 1. Tujuan
Aplikasi Android: user memilih tim tuan rumah dan tamu, lalu melihat peluang **Menang / Seri / Kalah (%)** dan **skor prediksi**. Data tim berasal dari backend Ktor (repo `football-predictor-api`). Prediksi dihitung **di perangkat** (awalnya model Poisson, lalu TFLite).

## 2. Scope
**In scope:** satu layar prediksi, pilih 2 tim, hasil prediksi, loading/error, cache data tim, test otomatis.
**Out of scope:** login, favorit, live score, notifikasi, multi-liga, rilis Play Store.

## 3. Stack
| Area | Pilihan |
|---|---|
| UI | Jetpack Compose + Material 3 |
| Arsitektur | MVVM + Clean Architecture |
| Async | Coroutines + Flow (`StateFlow`) |
| Network | Retrofit + OkHttp + kotlinx.serialization |
| ML | TensorFlow Lite (mulai A4) |
| DI | Hilt (lihat `decisions.md` D3; bisa diganti sebelum A0 selesai) |
| Build | Gradle Kotlin DSL + version catalog (`gradle/libs.versions.toml`) |
| Test | JUnit, MockK, Turbine, MockWebServer, Compose UI test |

Nomor versi **dicek ke sumber resmi saat A0** dan dicatat di `decisions.md`. Jangan memakai versi dari ingatan.

## 4. Peta milestone
| ID | Nama | Status |
|---|---|---|
| A0 | Bootstrap proyek | **In Progress** |
| A1 | Domain layer + predictor Poisson + unit test | Pending |
| A2 | Data layer: Retrofit ke backend | Pending |
| A3 | UI layar prediksi + ViewModel | Pending |
| A4 | Integrasi TFLite (butuh file `.tflite` dari ML) | Pending |
| A5 | Polish: error, cache, test UI | Pending |
| A6 | README, screenshot, release APK | Pending |

Satu milestone aktif pada satu waktu.

## 5. Kontrak API (salinan; sumber asli di repo backend)
`GET {BASE_URL}/api/teams` -> `200 application/json`
```json
[ { "id": "ARS", "name": "Arsenal", "attackStrength": 1.40, "defenseStrength": 0.65 } ]
```
| Field | Tipe | Arti |
|---|---|---|
| id | String | Kode 3 huruf unik |
| name | String | Nama tim |
| attackStrength | Float | Gol dicetak ÷ rata-rata liga (1.0 = rata-rata, tinggi = tajam) |
| defenseStrength | Float | Gol kebobolan ÷ rata-rata liga (1.0 = rata-rata, **rendah = kuat**) |

Error: `404` / `500` dengan body `{ "error": "..." }`.
Base URL: emulator `http://10.0.2.2:8080/`; HP fisik memakai IP LAN (backend perlu `host: 0.0.0.0`).

## 6. Model domain
```kotlin
data class Team(val id: String, val name: String, val attackStrength: Float, val defenseStrength: Float)

data class MatchPrediction(
    val homeTeam: Team, val awayTeam: Team,
    val homeWinProbability: Float, val drawProbability: Float, val awayWinProbability: Float,
    val predictedHomeGoals: Int, val predictedAwayGoals: Int
)
```
Aturan: tiga probabilitas ada di [0,1] dan berjumlah ≈ 1 (toleransi 0.001). Gol >= 0.

## 7. Interface inti (domain)
```kotlin
interface TeamRepository { suspend fun getTeams(): Result<List<Team>> }
interface MatchPredictor { suspend fun predict(home: Team, away: Team): MatchPrediction }
```
Use case: `GetTeamsUseCase`, `GetMatchPredictionUseCase(home, away)` (validasi home != away).

## 8. Predictor Poisson (A1, dipakai UI sampai TFLite siap)
Bukan angka acak: model statistik sederhana, murni Kotlin.
- `λ_home = AVG_HOME_GOALS × home.attack × away.defense`
- `λ_away = AVG_AWAY_GOALS × away.attack × home.defense`
- Konstanta awal **[Usulan]**: `AVG_HOME_GOALS = 1.5`, `AVG_AWAY_GOALS = 1.2`.
- Peluang skor `P(h,a) = Poisson(h; λ_home) × Poisson(a; λ_away)` untuk h,a = 0..10; jumlahkan: h>a menang kandang, h=a seri, h<a menang tamu; normalisasi agar total = 1.
- Skor prediksi = skor dengan peluang tertinggi (atau `round(λ)`; putuskan di A1 dan catat).
- Kelak berfungsi sebagai **baseline pembanding** untuk model TFLite.

## 9. Kontrak model TFLite (A4)
Input `float32[1,4]`: `[homeAttack, homeDefense, awayAttack, awayDefense]`.
Output **[Usulan]** `float32[1,5]`: `[pHomeWin, pDraw, pAwayWin, expHomeGoals, expAwayGoals]`.
File: `app/src/main/assets/football_predictor.tflite`. Detail: `00-project-overview.md` §8.

## 10. State UI
```kotlin
data class PredictionUiState(
    val teams: List<Team> = emptyList(),
    val teamsLoading: Boolean = true,
    val teamsError: String? = null,
    val selectedHome: Team? = null,
    val selectedAway: Team? = null,
    val result: PredictionResult = PredictionResult.Idle
)
sealed interface PredictionResult {
    data object Idle : PredictionResult
    data object Loading : PredictionResult
    data class Success(val prediction: MatchPrediction) : PredictionResult
    data class Error(val message: String) : PredictionResult
}
```
Aturan UI: tombol Predict nonaktif jika tim belum lengkap atau home == away. Disclaimer "hanya demo, bukan alat taruhan" selalu tampil bersama hasil.

## 11. Aturan kualitas
- Gate: `assembleDebug`, `testDebugUnitTest`, `lintDebug` hijau.
- Domain & mapper & ViewModel punya unit test.
- Tidak ada `android.*` di domain; tidak ada logika bisnis di Composable.
- Tidak ada secret di repo.

## 12. Glosarium
| Indonesia | Kode |
|---|---|
| Tim | `Team` |
| Tuan rumah / Tamu | `home` / `away` |
| Peluang menang / seri / kalah | `homeWinProbability` / `drawProbability` / `awayWinProbability` |
| Skor prediksi | `predictedHomeGoals` / `predictedAwayGoals` |
