# Laporan Proyek Machine Learning - Nabilla Nurulita Dewi

## Project Domain

Konsumsi energi pada sektor bangunan menyumbang lebih dari 36% dari total konsumsi energi global dan emisi karbon​​ [1]. Hal ini menjadikan efisiensi energi dalam bangunan sebagai salah satu prioritas utama dalam upaya konservasi energi dan keberlanjutan lingkungan. Perkembangan teknologi smart building serta ketersediaan data konsumsi energi secara real-time melalui smart meter membuka peluang besar untuk memanfaatkan pendekatan data-driven dalam memprediksi konsumsi energi secara akurat [2]​. Pendekatan berbasis data (data-driven) memungkinkan pengembangan model prediksi konsumsi energi tanpa memerlukan parameter teknis bangunan secara lengkap, seperti yang dibutuhkan oleh model fisik (white-box) [3]. Dalam berbagai studi, metode seperti Random Forest, XGBoost, dan Artificial Neural Network (ANN) sering digunakan karena kemampuannya menangkap pola non-linear dalam data [4]​. Oleh karena itu, proyek ini mengevaluasi kecocokan berbagai algoritma terhadap karakteristik data.

**References:**

[1] [A review of data-driven building energy consumption prediction studies](https://doi.org/10.1016/j.rser.2017.04.095) 

[2] [Energy consumption prediction by using machine learning for smart building: Case study in Malaysia](https://doi.org/10.1016/j.dibe.2020.100037) 

[3] [Data-Driven Tools for Building Energy Consumption Prediction-A Review](https://doi.org/10.3390/en16062574) 

[4] [Building energy consumption prediction for residential buildings using deep learning and other machine learning techniques](https://doi.org/10.1016/j.jobe.2021.103406)


## Business Understanding

### Problem Statements

- Konsumsi energi pada bangunan sulit diprediksi secara akurat karena dipengaruhi oleh banyak faktor, seperti cuaca, waktu, dan kebiasaan penggunaan.
- Tidak adanya sistem prediktif menyebabkan operator bangunan kesulitan dalam mengelola beban puncak, yang berdampak pada efisiensi dan biaya operasional.
- Metode konvensional (seperti regresi linier atau time-series klasik) memiliki keterbatasan dalam menangkap pola non-linier dan musiman dari konsumsi energi.

### Goals

- Membandingkan performa beberapa algoritma untuk memilih model terbaik berdasarkan akurasi dan efisiensi.

### Solution Statements
- Membangun model prediksi menggunakan beberapa algoritma, seperti Lasso dan Ridge Regression, Random Forest, XGBoost untuk membandingkan performa.
- Melakukan hyperparameter tuning pada model untuk mendapatkan performa optimal.
- Menggunakan metrik evaluasi, yaitu RMSE, MAE, dan R-squared, untuk mengukur kinerja prediksi.

## Data Understanding
Dataset yang digunakan berasal dari platform Kaggle, berjudul “[Energy Consumption Prediction](https://www.kaggle.com/datasets/mrsimple07/energy-consumption-prediction/data)”, yang menyajikan data time-series tentang konsumsi energi bangunan beserta variabel lingkungan eksternal.

### The variables in the dataset
- **`Temperature`**: mewakili suhu sekitar dalam derajat Celcius.  
- **`Humidity`**: mencerminkan tingkat kelembapan sebagai persentase.  
- **`SquareFootage`**: merepresentasikan ukuran lingkungan dalam rekaman persegi.  
- **`Occupancy`**: menunjukkan jumlah penghuni.  
- **`HVACUsage`**: menunjukkan status operasional sistem HVAC atau Heating, Ventilation, Air Conditioning (‘On’ atau ‘Off’).  
- **`LightingUsage`**: menunjukkan status operasional sistem pencahayaan (‘On’ atau ‘Off’).  
- **`RenewableEnergy`**: mewakili kontribusi sumber energi terbarukan dalam bentuk persentase.  
- **`DayOfWeek`**: menunjukkan hari dalam seminggu.  
- **`Holiday`**: menunjukkan apakah hari tersebut merupakan hari libur (‘Yes’ atau ‘No’).  
- **`EnergyConsumption`**: mewakili konsumsi energi.

Pada tahap ini, dilakukan beberapa tahapan yang diperlukan untuk memahami data, yaitu teknik visualisasi data atau exploratory data analysis, seperti pemeriksaan struktur dan tipe data, pengecekan missing values, analisis statistik deskriptif, visualisasi distribusi fitur, deteksi outlier, dan korelasi antar fitur.

## Data Preparation
- **Feature Selection**: Fitur yang relevan dan digunakan antara lain, Temperature, Humidity, SquareFootage, Occupancy, HVACUsage, LightingUsage, RenewableEnergy, DayOfWeek, dan Holiday. Pemisahan antara fitur (X) dan target (y) dilakukan untuk mempersiapkan data ke tahap pelatihan model. Pemilihan fitur penting untuk mengurangi kompleksitas model dan meningkatkan performa prediksi. 
- **Encoding dan Scaling**: Fitur numerik seperti Temperature dan SquareFootage di-scaling menggunakan StandardScaler untuk memastikan semua fitur memiliki skala yang seragam. Fitur kategorikal seperti HVACUsage, LightingUsage, DayOfWeek, dan Holiday di-encode menggunakan OneHotEncoder. Scaling dibutuhkan karena beberapa model sensitif terhadap skala data, sedangkan encoding diperlukan untuk mengubah fitur kategorikal menjadi format numerik yang dapat diproses model.
- **Train-Test Split**: Dataset dibagi menjadi data latih dan data uji dengan rasio 80:20. Pembagian ini dilakukan untuk mengevaluasi performa model secara objektif pada data yang belum pernah dilihat sebelumnya.

## Modeling
Pada tahap ini, dilakukan pemodelan menggunakan empat algoritma regresi berbeda, baik model konvensional maupun model berbasis ensemble, untuk memprediksi konsumsi energi bangunan. Proses diawali dengan implementasi model baseline menggunakan parameter default, dilanjutkan dengan proses hyperparameter tuning untuk meningkatkan performa masing-masing model.

### Models
- **Ridge Regression**
    - Model regresi linear dengan regularisasi L2 untuk mengurangi overfitting.
    - Cocok untuk data yang memiliki korelasi antar fitur.
- **Lasso Regression**
    - Model regresi linear dengan regularisasi L1, yang juga melakukan seleksi fitur otomatis.
    - Sering efektif pada data dengan fitur yang kurang informatif.
- **Random Forest Regressor**
    - Model ensemble berbasis pohon, mampu menangkap hubungan non-linear antar fitur.
    - Kuat terhadap outlier dan data noise, namun cenderung overfit jika tidak dituning.
- **XGBoost Regressor**
    - Model boosting yang sangat populer untuk prediksi tabular, dengan kontrol regulasi yang lebih detail.
    - Perlu tuning agar performanya maksimal.

### Hyperparameter Tuning

Model dioptimasi menggunakan RandomizedSearchCV dengan 5-fold cross-validation dan metrik evaluasi neg_root_mean_squared_error. Parameter terbaik yang diperoleh:

| Model             | Best Parameters |
|------------------|-----------------|
| **Ridge Regression** | `alpha = 10` |
| **Lasso Regression** | `alpha = 0.01` |
| **Random Forest**    | `n_estimators = 200`, `max_depth = 8`, `min_samples_split = 10` |
| **XGBoost**          | `n_estimators = 100`, `max_depth = 3`, `learning_rate = 0.05`, `subsample = 1.0` |

## Evaluation

### Metrics
Pada tahap ini, digunakan tiga metrik evaluasi utama yang sesuai untuk kasus regresi, yaitu MAE, RMSE, dan R-squared.

- **MAE (Mean Absolute Error)**: MAE mengukur rata-rata selisih absolut antara nilai prediksi dan nilai aktual.
- **RMSE (Root Mean Squared Error)**: RMSE menghitung akar dari rata-rata kuadrat error, lebih sensitif terhadap outlier.
- **R-squared**: R-squared menunjukkan proporsi variasi pada data target yang bisa dijelaskan oleh model.

### Results

| Model             | MAE   | RMSE  | R-squared |
|------------------|-------|-------|--------|
| **Lasso Regression** | 4.117 | 26.44 | 0.596  |
| **Ridge Regression** | 4.123 | 26.55 | 0.595  |
| **Random Forest**    | 4.329 | 29.41 | 0.551  |
| **XGBoost**          | 4.351 | 29.87 | 0.544  |

- Lasso Regression menghasilkan performa terbaik berdasarkan semua metrik: MAE terendah, RMSE terkecil, dan R-squared tertinggi.
- Hal ini menunjukkan bahwa model sederhana berbasis regresi linear dengan regularisasi L1 mampu menangkap pola pada data dengan lebih baik dibanding model kompleks seperti Random Forest dan XGBoost.
- Model lain seperti Random Forest dan XGBoost memiliki keunggulan dalam menangani hubungan non-linear, namun pada dataset ini performanya justru lebih rendah, kemungkinan karena data memiliki pola yang cenderung linear.
