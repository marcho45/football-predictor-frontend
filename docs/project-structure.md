# Struktur Proyek Android

```
football-predictor-frontend/
├── AGENTS.md
├── README.md
├── gradle/libs.versions.toml
├── docs/            (dokumen ini + plan/)
├── ml/              (nanti: skrip Python, dataset, model; dikerjakan terpisah)
└── app/
    ├── build.gradle.kts
    └── src/
        ├── main/
        │   ├── AndroidManifest.xml
        │   ├── assets/football_predictor.tflite     (A4)
        │   └── java/com/portfolio/footballpredictor/
        │       ├── FootballPredictorApp.kt          (Application, Hilt)
        │       ├── MainActivity.kt
        │       ├── di/
        │       ├── domain/
        │       │   ├── model/       Team.kt, MatchPrediction.kt
        │       │   ├── repository/  TeamRepository.kt
        │       │   ├── predictor/   MatchPredictor.kt
        │       │   └── usecase/     GetTeamsUseCase.kt, GetMatchPredictionUseCase.kt
        │       ├── data/
        │       │   ├── remote/      TeamApi.kt, dto/TeamDto.kt
        │       │   ├── mapper/      TeamMapper.kt
        │       │   ├── predictor/   PoissonMatchPredictor.kt, TflitePredictor.kt (A4)
        │       │   └── repository/  TeamRepositoryImpl.kt
        │       └── presentation/
        │           ├── theme/
        │           ├── components/  TeamDropdown.kt, ProbabilityBar.kt, ResultCard.kt
        │           └── prediction/  PredictionScreen.kt, PredictionViewModel.kt, PredictionUiState.kt
        ├── debug/res/xml/network_security_config.xml   (cleartext khusus debug)
        ├── test/        (unit test: domain, mapper, ViewModel)
        └── androidTest/ (UI test)
```
Package: `com.portfolio.footballpredictor` (sama dengan backend).
Jika template Android Studio menaruh file di lokasi lain, ikuti template lalu perbarui dokumen ini.
