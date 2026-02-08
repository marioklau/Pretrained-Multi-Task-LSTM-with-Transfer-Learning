📘 README — Definisi Range Fitur (Hourly Dataset – Berdasarkan Data Asli)
📌 Konteks Dataset

Dataset bersifat per jam (hourly)

Jumlah data: 3913 baris

Model menggunakan LSTM (SEQ_LEN = 6) → 6 jam terakhir

Fitur input:

FEATURES = ["StepTotal", "Calories", "heart_rate", "stress"]


Range di bawah ini diambil dari statistik dataset asli, bukan perkiraan.

## 🚶 StepTotal — Langkah per Jam

Statistik Dataset

Min: 0

Median: 123

Mean: 384

Q3 (75%): 481

Max: 5890

| Kategori                  | Range (langkah/jam) |
|---------------------------|--------------------|
| Tinggi / Aktif            | > 500              |
| Normal                    | 120 – 500          |
| Rendah                    | 5 – 120            |
| **Sangat Rendah (Buruk)** | **< 5**            |


📉 StepTotal < 5/jam sering terjadi saat:

duduk lama

kelelahan

jam tidur atau pasif ekstrem

## 🔥 Calories — Kalori Terbakar per Jam

Statistik Dataset

Min: 50

Median: 84

Mean: 106

Q3: 123

Max: 612

| Kategori                  | Range (kcal/jam) |
|---------------------------|-----------------|
| Tinggi                    | > 120           |
| Normal                    | 80 – 120        |
| Rendah                    | 64 – 80         |
| **Sangat Rendah (Buruk)** | **< 64**        |


📉 Kalori < 64/jam menunjukkan aktivitas fisik hampir nol.

## ❤️ Heart Rate — Denyut Jantung (bpm)

Statistik Dataset

Min: 43

Median: 69

Mean: 71

Q3: 79

Max: 150

| Kategori                                 | Range (bpm) |
|------------------------------------------|-------------|
| Rendah / Istirahat                       | < 60        |
| Normal                                   | 60 – 79     |
| Tinggi                                   | 80 – 90     |
| **Sangat Tinggi / Tidak Stabil (Buruk)** | **> 90**    |


⚠️ HR > 90 bpm dalam dataset sering berkorelasi dengan:

stress tinggi

recovery rendah

readiness rendah

## 😵 Stress — Level Stress per Jam

Statistik Dataset

Min: -15

Median: 6.47

Mean: 8.88

Q3: 15.34

Max: 79.83

Digunakan dalam rumus:

recovery_score = 100 - stress * 5

| Kategori                  | Range    |
|---------------------------|---------|
| Rendah                    | ≤ 5     |
| Normal                    | 5 – 15  |
| Tinggi                    | 15 – 25 |
| **Sangat Tinggi (Buruk)** | **> 25**|


📉 Stress > 25 akan menurunkan recovery score ke < 0 (di-clip).

😴 Pola Konsisten Buruk (Hourly Time Series)

Karena model berbasis LSTM, kondisi dianggap buruk secara konsisten jika:

Definisi Pola Buruk

Dalam 1 sequence (6 jam):

≥ 4 dari 6 jam memenuhi minimal 2 kondisi berikut:

StepTotal < 5
Calories  < 64
heart_rate > 90
stress > 25


📌 Pola konsisten jauh lebih berpengaruh dibanding 1 jam buruk saja.

🧪 Contoh Data Dummy Buruk (Konsisten, Per Jam)
dummy_bad = pd.DataFrame([
    [2,  55,  95, 30],
    [0,  52,  98, 35],
    [3,  60, 100, 38],
    [1,  58, 102, 40],
    [4,  62, 105, 42],
    [0,  50, 108, 45],
], columns=["StepTotal", "Calories", "heart_rate", "stress"])

🎯 Ekspektasi Output Model

Untuk data dengan karakteristik di atas, model yang sehat diharapkan menghasilkan:

sleep_score rendah

recovery_score sangat rendah

hrv_score rendah

rhr_score rendah

readiness_score < 40

✅ Catatan Penting

Semua range berdasarkan distribusi dataset asli

Dummy data tidak keluar dari domain data

Konsistensi skala wajib dijaga antara training & inference
