🧠 Prediksi Readiness Kesehatan Berbasis Time Series Wearable

Transfer Learning LSTM: Fitbit → Smartwatch

📌 Deskripsi Proyek

Proyek ini mengembangkan model deep learning berbasis time series untuk memprediksi indikator kesiapan kesehatan (health readiness) menggunakan data sensor dari perangkat wearable (smartwatch).

Model dilatih menggunakan pendekatan transfer learning, dengan:

Pretraining pada dataset Fitbit

Fine-tuning pada dataset smartwatch yang bersifat uncleaned

Pendekatan ini bertujuan untuk memanfaatkan pola fisiologis umum dari data Fitbit dan menyesuaikannya dengan karakteristik data smartwatch.

🎯 Tujuan Penelitian

Membangun model time series untuk data wearable

Memprediksi indikator kesiapan tubuh berbasis sensor

Menerapkan transfer learning lintas perangkat wearable

Mengatasi keterbatasan data smartwatch yang noisy dan tidak lengkap

⏱ Karakteristik Data (Time Series)

Dataset diperlakukan sebagai time series multivariat, di mana:

Data disusun berdasarkan urutan waktu

Model menerima input dalam bentuk sliding window

Bentuk input model:

(batch_size, timesteps, features)


Contoh:

(1, 6, 4)


Artinya:

6 timestep berurutan

4 fitur sensor pada setiap timestep

📥 Fitur Input

Fitur yang digunakan pada setiap timestep:

Fitur	Deskripsi
heart_rate	Denyut jantung
blood_oxygen_level	Saturasi oksigen (SpO₂)
step_count	Jumlah langkah
stress_level	Tingkat stres

Semua fitur telah melalui proses:

Pembersihan data

Encoding

Normalisasi / scaling

🎯 Target Prediksi

Model memprediksi lima indikator kesiapan kesehatan secara bersamaan:

Output	Deskripsi
sleep_score	Kualitas tidur
hrv_score	Perkiraan variabilitas denyut jantung (proxy)
rhr_score	Skor denyut jantung istirahat
recovery_score	Kondisi pemulihan tubuh
readiness_score	Skor kesiapan tubuh keseluruhan
⚠️ Catatan HRV

HRV tidak dihitung langsung menggunakan RR-interval, melainkan diperkirakan (proxy) menggunakan:

Variabilitas denyut jantung

Tingkat stres

Pola aktivitas fisik

🏗 Arsitektur Model

Model menggunakan LSTM (Long Short-Term Memory) sebagai encoder utama untuk menangkap dependensi temporal pada data time series.

Input Time Series
        ↓
   LSTM Encoder
        ↓
 Shared Representation
        ↓
 ┌─────────┬─────────┬─────────┬─────────┬─────────┐
 | Sleep   | HRV     | RHR     | Recovery| Readiness|
 └─────────┴─────────┴─────────┴─────────┴─────────┘

🔁 Strategi Transfer Learning

Pretraining

Dataset: Fitbit

Tujuan: mempelajari representasi fisiologis umum manusia

Encoder disimpan sebagai model pretrained

Fine-Tuning

Dataset: Smartwatch

Encoder pretrained digunakan kembali

Model disesuaikan dengan distribusi data perangkat target

Pendekatan ini dikenal sebagai cross-device physiological representation learning.

🧪 Training dan Evaluasi

Fungsi loss: Mean Squared Error (MSE)

Metrik evaluasi:

MAE (Mean Absolute Error)

RMSE (Root Mean Squared Error)

Evaluasi dilakukan pada data uji yang dipisahkan secara temporal (tanpa shuffle).

🚀 Contoh Penggunaan (Inference)
sample = X_test[0].reshape(1, 6, 4)
predictions = model.predict(sample)

readiness_score = predictions[-1][0][0]
print("Prediksi Readiness:", readiness_score)

📱 Penerapan di Aplikasi

Model ini dapat digunakan pada aplikasi berbasis wearable untuk:

Rekomendasi aktivitas harian

Monitoring kelelahan

Deteksi risiko overtraining

Sistem pendukung keputusan kesehatan personal

⚠️ Keterbatasan Penelitian

HRV bersifat estimasi (proxy)

Data smartwatch memiliki noise

Belum menggunakan multi-step forecasting

Belum di-deploy ke perangkat edge

🔮 Pengembangan Selanjutnya

Menggunakan RR-interval untuk HRV

Prediksi multi-step (future forecasting)

Deployment ke aplikasi mobile

Edge AI untuk wearable device

🧑‍💻 Penulis

Mario Klau
Bidang: Machine Learning, Deeo Learning, & Wearable Health Analytics

📄 Lisensi

Proyek ini dikembangkan untuk keperluan akademik dan penelitian.

✅ Status Proyek

✔ Model final telah dilatih
✔ Arsitektur valid secara ilmiah
✔ Siap untuk GitHub
