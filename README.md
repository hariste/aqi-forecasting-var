# aqi-forecasting-var
Final project dari dibimbing untuk path Data Scentist.

Dataset yang saya gunakan adalah dataset tentang Air Quality Index (AQI) di kota Jakarta dari tahun 2010-2021 (https://www.kaggle.com/datasets/senadu34/air-quality-index-in-jakarta-2010-2021/data). Dataset ini terdiri dari 5 file csv yang menggambarkan 5 titik dimana data ini diambil. Antara lain:

ispu_dki1: titik pengukuran ada di Bundaran HI
ispu_dki2: titik pengukuran ada di Kelapa Gading
ispu_dki3: titik pengukuran ada di Jagakarsa
ispu_dki4: titik pengukuran ada di Lubang Buaya
ispu_dki5: titik pengukuran ada di Kebon Jeruk
Kategorisasi berdasarkan nilai pada kolom max:

0 - 50 = BAIK
51 - 100 = SEDANG
100 - 150 = TIDAK SEHAT
Adapun kolom yang tersedia, penjelasannya adalah sebagai berikut:

tanggal: tanggal pengukuran AQI
stasiun: titik tempat pengukuran
pm10: particulate matter dengan ukuran antara 2.5 hingga 10 mikron (fitur)
pm25: particulate matter dengan ukuran lebih kecil atau sama dengan 2.5 mikron (fitur)
so2: kandungan sulfide (fitur)
co: kandungan karbon monoksida (fitur)
o3: kandungan ozone (fitur)
no2: kandungan nitrogen dioksida
max: nilai maksimal dari semua parameter atau fitur (Target untuk menentukan kategori)
critical: parameter/fitur yang memiliki nilai maksimal diantara lainnya
category: kategori target yg disimpulkan dari klasifikasi berdasarkan nilai fitur. (target)
Dataset menggambarkan nilai harian, dan nilai dari fitur merupakan pengukuran dari 1m kubik udara. Semakin besar nilai dari fitur berarti semakin rendah kualitas dari udara yang dihirup. Nilai dari fitur merupakan hasil rata-rata dari pengukuran yang dilakukan pada tanggal tersebut.

Yang saya lakukan di final project ini adalah melakukan prediksi menggunakan regresi tentang kategori dari hasil pengukuran AQI. Langkah-langkah yang dilakukan adalah sebagai berikut:

Data Preprocessing:
EDA
Mencoba menggunakan Ridge dan Lasso Regression pada dataset yang sudah dibersihkan. Hyperparameter tuning untuk memilih parameter tuning yang terbaik
Melakukan evaluasi terhadap model (Ridge dan Lasso Regression)
Jika masih ada waktu, melakukan analisa korelasi untuk menghilangkan fitur yang memiliki korelasi tinggi satu sama lain
Melakukan RIdge dan Lasso Regression dan Hyperparameter tuning lagi
Evaluasi Model mana yang lebih baik antara no 3 atau 6
Berdasarkan feedback dari mentor, ternyata data saya termasuk time series sehingga tidak disarankan menggunakan regresi linier. Mentor menyarankan mengubah metode menjadi ARIMA atau SARIMA.

Setelah saya melakukan studi literatur dan membaca beberapa buku dan jurnal, ternyata metode ARIMA atau SARIMA tidak bisa digunakan. Hal ini dikarenakan ada beberapa fitur lain yang juga ada dan berpengaruh terhadap hasil akhir nilai AQI. Kesimpulan yang saya dapatkan adalah dataset saya jika dilakukan peramalan, maka termasuk peramalan time series yang bersifat multivariate. ARIMA atau SARIMA masuk metode peramalan time series yang bersifat univariate. Sehingga saya harus mencari lagi metode yang cocok digunakan dalam peramalan time series multivariate.

Saya menemukan beberapa metode, antara lain:

VAR (Vector Auto Regressive)
Random Forest Regressor
XGBoost Regressor
Dikarenakan keterbatasan waktu untuk belajar semua metode diatas maka saya memilih menggunakan metode pertama yakni VAR (Vector Auto Regression)

Revisi Pengerjaan Berdasarkan feedback terakhir dari mentor, disarankan untuk menyederhanakan dataset dengan menggunakan forecasting univariate terhadap salah satu aspek dalam pengukuran AQI. Disini, saya memilih fitur pm10 untuk diprediksi lebih lanjut dan menghilangkan semua fitur lainnya.

Langkah-langkah yang saya lakukan berikutnya adalah sebagai berikut:

Data Preprocessing: Melakukan pengecekan terhadap nilai kosong dan melakukan drop terhadap kolom yang tidak digunakan dalam forecasting (station, critical, category), merubah kolom tanggal menjadi datetime. Target kolom adalah max.
Melakukan pengecekan untuk stationarity pada setiap fitur yang akan digunakan, yakni: pm10, pm25, so2, co, o3, no2
Melakukan split untuk data train dan test
Melakukan grid search untuk mencari nilai order P yang terbaik
Predict test data
Invert transformasi
Plot terhadap hasil prediksi
Evaluasi Model menggunakan RMSE, semakin kecil maka semakin bagus.
