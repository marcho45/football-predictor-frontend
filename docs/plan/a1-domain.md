# A1 — Domain layer + Predictor Poisson

Status: **Pending** — jangan dikerjakan sebelum A0 Approved.

| Slice | Isi | Status |
|---|---|---|
| A1a | `Team`, `MatchPrediction` (build-spec §6) | [ ] |
| A1b | Interface `TeamRepository`, `MatchPredictor` (§7) | [ ] |
| A1c | `PoissonMatchPredictor` (§8), putuskan Q2 | [ ] |
| A1d | `GetTeamsUseCase`, `GetMatchPredictionUseCase` (tolak home == away) | [ ] |
| A1e | Unit test: jumlah probabilitas ≈ 1; tim lebih kuat -> peluang menang lebih besar; home == away ditolak; gol >= 0 | [ ] |

Aturan: domain tanpa `android.*`. Test berjalan sebagai unit test JVM.

Acceptance: `testDebugUnitTest` hijau, domain tidak mengimpor Android/Retrofit/TFLite.
