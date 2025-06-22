# Laporan Proyek Machine Learning - Nabilla Nurulita Dewi

## Project Domain

Konsumsi energi pada sektor bangunan menyumbang lebih dari 36% dari total konsumsi energi global dan emisi karbon​​ [1]. Hal ini menjadikan efisiensi energi dalam bangunan sebagai salah satu prioritas utama dalam upaya konservasi energi dan keberlanjutan lingkungan. Perkembangan teknologi smart building serta ketersediaan data konsumsi energi secara real-time melalui smart meter membuka peluang besar untuk memanfaatkan pendekatan data-driven dalam memprediksi konsumsi energi secara akurat [2]​. Pendekatan berbasis data (data-driven) memungkinkan pengembangan model prediksi konsumsi energi tanpa memerlukan parameter teknis bangunan secara lengkap, seperti yang dibutuhkan oleh model fisik (white-box) [3]. Dalam berbagai studi, metode seperti Random Forest, XGBoost, dan Artificial Neural Network (ANN) sering digunakan karena kemampuannya menangkap pola non-linear dalam data [4]​. Oleh karena itu, proyek ini mengevaluasi kecocokan berbagai algoritma terhadap karakteristik data.

**References:**

[1] [A review of data-driven building energy consumption prediction studies](https://doi.org/10.1016/j.rser.2017.04.095)  
[2] [Energy consumption prediction by using machine learning for smart building: Case study in Malaysia](https://doi.org/10.1016/j.dibe.2020.100037)  
[3] [Data-Driven Tools for Building Energy Consumption Prediction-A Review](https://doi.org/10.3390/en16062574)  
[4] [Building energy consumption prediction for residential buildings using deep learning and other machine learning techniques](https://doi.org/10.1016/j.jobe.2021.103406)  

---

## Business Understanding

### Problem Statements

- Konsumsi energi pada bangunan sulit diprediksi secara akurat karena dipengaruhi oleh banyak faktor, seperti cuaca, waktu, dan kebiasaan penggunaan.
- Tidak adanya sistem prediktif menyebabkan operator bangunan kesulitan dalam mengelola beban puncak, yang berdampak pada efisiensi dan biaya operasional.
- Metode konvensional (seperti regresi linier atau time-series klasik) memiliki keterbatasan dalam menangkap pola non-linier dan musiman dari konsumsi energi.

### Goals

- Membangun dan membandingkan performa beberapa algoritma untuk memilih model terbaik berdasarkan akurasi prediksi dan efisiensi komputasi.
- Menyediakan sistem pendukung keputusan bagi pengelola gedung dalam mengoptimalkan konsumsi energi.

### Solution Statements

- Membangun model prediksi konsumsi energi menggunakan algoritma Ridge Regression, Lasso Regression, Random Forest, dan XGBoost.
- Melakukan hyperparameter tuning untuk meningkatkan performa masing-masing model.
- Menggunakan metrik RMSE, MAE, dan R² sebagai dasar evaluasi performa model.
- Mengevaluasi dampak hasil prediksi terhadap pengambilan keputusan operasional pada sistem bangunan cerdas.

---

## Data Understanding

Dataset diambil dari Kaggle dengan judul: [Energy Consumption Prediction](https://www.kaggle.com/datasets/mrsimple07/energy-consumption-prediction/data).

### Informasi Dataset:

- **Jumlah data**: 15.187 baris × 10 kolom
- **Kondisi data**:
  - Tidak terdapat missing values
  - Terdapat beberapa outlier yang ditangani dengan visualisasi distribusi
  - Tidak ada duplikasi data
- **Sumber data**: Kaggle (tautan di atas)
- **Target variabel**: `EnergyConsumption`

### Fitur Dataset:

| Fitur              | Deskripsi                                          |
|--------------------|----------------------------------------------------|
| Temperature        | Suhu sekitar dalam derajat Celcius                 |
| Humidity           | Kelembapan lingkungan (%)                          |
| SquareFootage      | Luas area bangunan (sqft)                          |
| Occupancy          | Jumlah penghuni                                    |
| HVACUsage          | Status penggunaan HVAC ('On' / 'Off')             |
| LightingUsage      | Status pencahayaan ('On' / 'Off')                 |
| RenewableEnergy    | Persentase energi dari sumber terbarukan (%)       |
| DayOfWeek          | Hari dalam seminggu                                |
| Holiday            | Apakah hari tersebut hari libur ('Yes' / 'No')     |
| EnergyConsumption  | Jumlah konsumsi energi (target prediksi)           |

---

## Data Preparation

- **Feature Selection**: Semua fitur (kecuali target) digunakan untuk pelatihan model.
- **Encoding**: Fitur kategorikal (HVACUsage, LightingUsage, Holiday, DayOfWeek) diencoding menggunakan OneHotEncoder.
- **Scaling**: Fitur numerik (Temperature, Humidity, dll) discaling menggunakan StandardScaler.
- **Train-Test Split**: 80% data digunakan untuk training dan 20% untuk testing.

---

## Modeling

Model yang digunakan:

- **Ridge Regression** – regresi linear dengan regularisasi L2
- **Lasso Regression** – regresi linear dengan regularisasi L1 + seleksi fitur
- **Random Forest Regressor** – ensemble model berbasis decision tree
- **XGBoost Regressor** – boosting model dengan kontrol regularisasi detail

### Hyperparameter Tuning

Dilakukan tuning menggunakan `RandomizedSearchCV` dengan 5-fold cross-validation menggunakan skor RMSE sebagai metrik optimasi. Hasil parameter terbaik:

| Model             | Best Parameters |
|------------------|-----------------|
| Ridge Regression | alpha = 10 |
| Lasso Regression | alpha = 0.01 |
| Random Forest    | n_estimators = 200, max_depth = 8, min_samples_split = 10 |
| XGBoost          | n_estimators = 100, max_depth = 3, learning_rate = 0.05, subsample = 1.0 |

---

## Evaluation

### Metrik Evaluasi

- **MAE (Mean Absolute Error)**: Rata-rata selisih absolut antara nilai prediksi dan aktual
- **RMSE (Root Mean Squared Error)**: Menghitung akar dari rata-rata kuadrat selisih, sensitif terhadap outlier
- **R-squared (R²)**: Proporsi variasi target yang dijelaskan oleh model

---

### Baseline Results (Tanpa Tuning)

| Model             | MAE   | RMSE  | R-squared |
|------------------|-------|-------|-----------|
| Ridge Regression | 4.121 | 26.55 | 0.595     |
| Random Forest    | 4.407 | 29.95 | 0.543     |
| XGBoost          | 4.599 | 33.43 | 0.490     |
| Lasso Regression | 4.587 | 33.84 | 0.483     |

---

### After Tuning

| Model             | MAE   | RMSE  | R-squared |
|------------------|-------|-------|-----------|
| Lasso Regression | 4.117 | 26.44 | 0.596     |
| Ridge Regression | 4.123 | 26.55 | 0.595     |
| Random Forest    | 4.329 | 29.41 | 0.551     |
| XGBoost          | 4.351 | 29.87 | 0.544     |

---

### Perbandingan Baseline vs Tuning (RMSE)

| Model           | RMSE (Baseline) | RMSE (Tuning) | ΔRMSE     |
|----------------|------------------|----------------|-----------|
| Lasso          | 33.84            | 26.44          | ▼7.40     |
| Ridge          | 26.55            | 26.55          | ±0.00     |
| Random Forest  | 29.95            | 29.41          | ▼0.54     |
| XGBoost        | 33.43            | 29.87          | ▼3.56     |

---

### Analisis Hasil

- Model **Lasso Regression** menunjukkan peningkatan paling signifikan setelah tuning, dengan penurunan RMSE lebih dari 7 poin.
- **Ridge Regression** menunjukkan performa stabil dan mendekati Lasso, namun tidak mengunggulinya.
- **Random Forest dan XGBoost**, meskipun dikenal menangani non-linearitas, tidak lebih unggul pada dataset ini—menunjukkan bahwa pola data cenderung linear.
- **Model terbaik secara keseluruhan** adalah **Lasso Regression** setelah tuning: akurasi tertinggi (R²: 0.596) dan RMSE terendah (26.44).

---

## Business Impact

Dengan model prediksi berbasis Lasso Regression:

✅ **Operator gedung dapat memprediksi konsumsi energi lebih akurat**, sehingga dapat:
- Mencegah overuse energi pada jam sibuk,
- Menyesuaikan pengaturan HVAC dan pencahayaan berdasarkan kebutuhan aktual,
- Merencanakan pemakaian energi dengan lebih efisien untuk mengurangi biaya operasional.

✅ **Implementasi model ini dalam sistem smart building** berpotensi:
- Meningkatkan efisiensi energi hingga puluhan persen,
- Mengurangi emisi karbon melalui pengendalian konsumsi,
- Memberikan *insight* data-driven untuk pengambilan keputusan jangka panjang.

---

## Kesimpulan

Proyek ini membuktikan bahwa pendekatan data-driven menggunakan machine learning mampu membangun sistem prediktif yang bermanfaat bagi sektor energi bangunan. Lasso Regression terbukti menjadi pilihan terbaik pada dataset ini. Ke depan, model ini dapat dikembangkan dengan menambahkan fitur waktu, data historis jangka panjang, atau integrasi ke dashboard operasional.
