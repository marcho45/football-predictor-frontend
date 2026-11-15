# Arsitektur Android

## Layer
```
presentation  ->  domain  <-  data
(Compose, VM)    (model,      (Retrofit, DTO, mapper,
                  interface,   repository impl, predictor impl)
                  use case)
```
Aturan emas: lapisan luar boleh memanggil dalam, tidak sebaliknya. Domain tidak tahu Android.

## Alur satu prediksi
1. User memilih 2 tim, tekan **Predict**.
2. Composable mengirim event ke `PredictionViewModel`.
3. ViewModel set `Loading`, memanggil `GetMatchPredictionUseCase(home, away)`.
4. Use case memvalidasi (home != away) lalu memanggil `MatchPredictor.predict`.
5. Implementasi predictor: Poisson (A1-A3) lalu TFLite (A4).
6. Hasil `MatchPrediction` kembali -> ViewModel set `Success` -> Compose menampilkan.

## Alur data tim
`ViewModel -> GetTeamsUseCase -> TeamRepository (impl) -> TeamApi (Retrofit) -> backend Ktor`
DTO dipetakan ke `Team` lewat mapper di data layer.

## Mengapa predictor di balik interface
Mengganti Poisson -> TFLite (atau API server) hanya mengganti satu binding DI. UI, ViewModel, dan use case tidak berubah. Ini juga membuat ViewModel mudah diuji dengan predictor palsu.

## Thread
Jaringan dan inferensi berjalan di `Dispatchers.IO` / `Default` (di data layer), bukan di main thread.
