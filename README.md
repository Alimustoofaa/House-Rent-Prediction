



# Proyek Pertama House Rent Prediction

#### Disusun oleh : Ali Mustofa

Ini adalah proyek pertama predictive analytics untuk memenuhi submission dicoding. Proyek ini membangun model machine learning yang dapat memprediksi harga sewa rumah dan apartement.

## Domain Proyek

### Latar Belakang

Tempat tinggal adalah kebutuhan primer bagi manusia untuk berlindung dan hidup menetap. Tempat tinggal selalu berada dalam wilayah tertentu atau dapat pula berupa rumah, apartement yang berada dalam wilayah atau tertentu. Tempat tinggal ini memiliki nilai tergantung dari karakteristik yang memiliki seperti lokasi, luas, jumlah kamar mandi, jumlah kamar tidur, dan fasilitas lainnya.

<br>

<div><img src="https://www.pinhome.id/blog/wp-content/uploads/2021/01/25321-rumah-idaman-1.jpg" width="1000"/></div>

[Referensi gambar](https://www.pinhome.id/)

<br>

Harga dari setiap rumah diukur dari nilai yang dimiliki oleh rumah tersebut. Namun, harga ini tidak selalu pasti dan sulit untuk melakukan prediksi akurat secara manual. Faktor ketidakpastian perlu dikurangi oleh perusahaan sewa dengan membangun sistem prediksi yang dapat menentukan berapa harga sewa yang pantas untuk karakteristik rumah tertentu.

Dalam mencapai hal tersebut, maka dilakukan penelitian untuk memprediksi harga sewa rumah menggunakan model machine learning. Diharapkan model ini mampu memprediksi harga sewa yang sesuai dengan harga pasar. Prediksi ini nantinya dijadikan acuan bagi perusahaan dalam menyewa rumah dengan harga yang dapat mendatangkan profit bagi perusahaan.

Referensi : [Analisis Prediksi Harga Rumah Sesuai Spesifikasi Menggunakan Multiple Linear Regression](https://ejournal.upnvj.ac.id/index.php/informatik/article/download/3635/1498)

## Business Understanding

Proyek ini dibangun untuk perusahaan dengan karakteristik bisnis sebagai berikut :

+ Perusahaan memiliki atau membeli rumah dan apartemen kemudian menyewanya ke konsumen.
+ Perusahaan membuka jasa konsultasi harga sewa rumah dan apartemen ke konsumen.

### Problem Statement

1. Fitur apa yang paling berpengaruh terhadap harga sewa rumah atau apartemen?
2. Bagaimana cara memproses data agar dapat dilatih dengan baik oleh model?
3. Berapa harga sewa rumah di pasaran berdasarkan karakteristik tertentu?

### Goals

1. Mengetahui fitur yang paling berpengaruh pada harga sewa rumah atau apartemen.
2. Melakukan persiapan data untuk dapat dilatih oleh model.
3. Membuat model machine learning yang dapat memprediksi harga sewa rumah seakurat mungkin berdasarkan karakteristik tertentu.

### Solution Statement

1. Menganalisis data dengan melakukan univariate analysis dan multivariate analysis. Memahami data juga dapat dilakukan dengan visualisasi. Memahami data dapat membantu untuk mengetahui kolerasi antar fitur dan mendeteksi outlier.
2. Menyiapkan data agar bisa digunakan dalam membangun model.
3. Melakukan hyperparameter tuning menggunakan grid search dan membangun model regresi yang dapat memprediksi bilangan kontinu. ALgoritma yang dipakai dalam proyek ini adalah K-Nearest Neighbour, Random Forest, dan AdaBoost.

## Data Understanding & Removing Outlier

Dataset yang digunakan dalam proyek ini merupakan data harga sewa rumah dengan berbagai karakteristik di India. Dataset ini dapat diunduh di [Kaggle : House Rent Prediction Dataset](https://www.kaggle.com/datasets/rkb0023/houserentpredictiondataset).

Berikut informasi pada dataset :

+ Dataset memiliki format CSV (Comma-Seperated Values).
+ Dataset memiliki 265190 sample dengan 22 columns.
+ Dataset memiliki 10 column bertipe int64 , 8 column bertipe object, dan 3 column bertype float64
+ Dataset memiliki missing value terdapat di 42 row di column sqfeet, 7526 rows di column beds dan 782 row di column price.

### Variable - variable pada dataset
+ id: Id posting.
+ url: URL tentang deskripsi lengkap rumah/apartemen.
+ region: Daerah dimana rumah/apartemen berada.
+ region_url: URL dari daerah dimana rumah/apartemen berada..
+ price: Harga sewa dari rumah/apartemen.
+ type: Jenis bangunan rumah/apartemen.
+ sqfeet: Luas dari rumah/apartemen dalam square feet (sqft).
+ beds: Jumlah kamar tidur.
+ baths: Jumlah kamar mandi.
+ cats_allowed: Fasilitas memperbolehkan/tidak memelihara kucing.
+ dogs_allowed: Fasilitas memperbolehkan/tidak memelihara anjing.
+ smoking_allowed: Fasilitas memperbolehkan/tidak merokok.
+ wheelchair_access: Fasilitar akses kursi roda.
+ electric_vehicle_charge: Fasilitas charge untuk mobil listrik.
+ comes_furnished: Fasilitas perabotan rumah/apartemen, baik Furnished atau Semi-Furnished atau Unfurnished.
+ laundry_options: Fasilitas pencucian pakaian yang ada di rumah/apartemen.
+ parking_options: Jenis tempat parkir yang disediakan di rumah/apartemen.
+ image_url : URL foto rumah/apartemen.
+ description: Deskripsi tentang rumah/apartemen.
+ lat: Koordinat latitude.
+ long: Koordinat longitude.
+ state: Negara dimana rumah/apartemen tersebut.

Dari 22 column terdapat beberapa column yang tidak mempengaruhi harga sewa rumah/apartemen tersebut, sehingga akan dihapus. Hal ini dikarenakan kedua fitur tersebut tidak diperlukan dalam membangun model prediksi harga sewa. berikut adalah column yang akan dihapus:
+ id
+ url
+ region_url
+ image_url
+ description
+ lat
+ long

### Univariate Analysis

Univariate Analysis adalah menganalisis setiap fitur secara terpisah.

#### Analisis jumlah nilai unique pada setiap fitur category

Column kategoti laundry_options, parking_options memiliki sebaran sample yang cukup merata.
<div><img src="https://user-images.githubusercontent.com/56061857/202908616-4405a0f7-41b6-488f-a6e5-e77ecd3d9304.png" width="250"/></div><br />
<div><img src="https://user-images.githubusercontent.com/56061857/202908631-65a290ef-35b1-4bf6-99e1-39b7de5efbc6.png" width="250"/></div><br />

Berikut adalah fitur dengan sample yang tidak merata :

+ type
  <div><img src="https://user-images.githubusercontent.com/56061857/202908717-abc93755-d962-4205-a4fc-bdb2822a19a9.png" width="250"/></div><br />
  Hanya terdapat 1 sampke assisted living dan 4 sample land. Untuk menghindari high dimensional data, maka kedua sample ini akan dihapus.

+ region
  <div><img src="https://user-images.githubusercontent.com/56061857/202909123-75c85f84-7d46-4335-81b5-430ff1dcce9c.png" width="250"/></div><br />
  Hanya terdapat 1 sample zanesville / cambridge sample tersebut akan dihapus. Untuk menghindari high dimensional data, maka sample ini akan dihapus.
  
+ state
  <div><img src="https://user-images.githubusercontent.com/56061857/202909272-6b398793-6542-4af8-a1b6-1c3d483d82fa.png" width="250"/></div><br />
  Column state ada 2 kategori ok memiliki sample 49 dan or memiliki sample 44, untuk menghindari high dimensional data, maka sample ini akan dihapus
  
#### Analisis sebaran pada setiap fitur numerik
<div><img src="https://user-images.githubusercontent.com/56061857/202910078-50a5fe0b-7c69-4262-82ee-6161337f394f.png" width="600"/></div><br />
Berikut analisis dari grafik di atas :

+ Rentang harga sewa cukup tinggi, yaitu dari 825.000000 hingga 2768307249.000000. Namun, rata-rata harga rumah hanya 12656.828313. Distribusi harga yang kurang bagus seperti ini dapat berimplikasi pada model.
+ Rata-rata rumah/apartement memiliki luas di bawah 1111 sqft
+ Sebagian besar rumah/apartement memiliki 1 sampai 8 beds dan 1 sampai 9 kamar mandi.
+ Sebagian besar rumah/apartement memiliki fasilitas memelihara kucing dan anjing.
+ Sebagian besar rumah/apartement memperbolehkan merokok.
+ Sebagian besar rumah/apartement tidak memiliki akses untuk kursi roda.
+ Sebagian besar rumah/apartement tidak memiliki charging untuk mobil listrik.
+ Sebagian besar rumah/apartement tidak dilengkapi dengan perlengkapan rumah.

### Multivariate Analysis

Multivariate Analysis menunjukkan hubungan antara dua atau lebih fitur dalam data.

#### Analisis fitur numerik

+ Column beds and baths
	Kedua column ini dianalisis karena tidak memungkinkan 1 rumah memiliki 1100 kamar tidur dan 75 kamar mandri. Data tersebut diatas maksimal akan dihapus. Hal ini menyebabkan jumlah sampe berkurangnya jumlah sample sebesar 3. Dan dianalisa terdapat data dibawah minimal yaitu 0, maka sample data kamar tidur dan kamar mandi akan dihapus. Hal ini menyebabkan berkurangnya jumlah sample sebesar 9573
+ Fitur sqfeet dan price (Menghapus price per sqfeet Outlier)
Untuk itu penghapusan outlier price outlier dengan mean dan one standard deviation yang telah dikelompokkan berdasarkan type. Hal ini menyebabkan berkurangnya jumlah sample sebesar 960. begitu jga degan sqfeet yang menyebabkan jumlahnya sample berkurang 631.

+ Melihat kolerasi antara semua fitur numerik
  <div><img src="https://user-images.githubusercontent.com/56061857/202914854-fc2b207e-c320-4309-a072-e7e02e019949.png" width="650"/></div>
  Columnt sqfeet, beds, baths ,cats_allowed, dogs_allowedberkorelasi tidak signifikan dengan fitur target (price). Hal ini mungkin   disebabkan oleh kurangnya data dalam penelitian ini. Column beds, baths berkolerasi signifikan dengan fitur sqfeet. Hal ini sudah sesuai harapan dari penghapusan outlier yang sudah dilakukan sebelumnya.

#### Analisis fitur kategorik

Analisis ini dilakukan untuk melihat kolerasi antara fitur kategorik dengan fitur target (Rent).

+ Column type
  <div><img src="https://user-images.githubusercontent.com/56061857/202915653-03ee9ec8-eb8e-4b54-95a4-50390da07294.png" width="500"/></div>
  Column type cukup memiliki pengaruh terhadap rata-rata harga sewa, terutama jika type nya flat, in-law dan apartement
+ Cloumn laundry_option
   <div><img src="https://user-images.githubusercontent.com/56061857/202915906-9d07367c-2715-458e-9e73-a644b56f202d.png" width="500"/></div>
   Column laundry_option cukup memiliki pengaruh terhadap rata-rata harga sewa, terutama jika laundry_option nya hookups
+ Column parking_options
   <div><img src="https://user-images.githubusercontent.com/56061857/202916317-515a3fab-41c5-49bc-90d2-19000e4213fd.png" width="500"/></div>
   Column parking_options cukup memiliki pengaruh terhadap rata-rata harga sewa, terutama jika parking_options nya valet parking, carport, street parkingm dan off street parking
+ Column state
  <div><img src="https://user-images.githubusercontent.com/56061857/202916431-3ae77eed-3f94-4e34-bfdb-d814cd478a2d.png" width="500"/></div>
  Fitur City memiliki pengaruh cukup besar terhadap rata-rata harga sewa, terutama jika rumah berada di state nc

## Data preparation

+ One Hot Encoding

  One hot encoding adalah teknik mengubah data kategorik menjadi data numerik dimana setiap kategori menjadi kolom baru dengan nilai 0 atau 1. Fitur yang akan diubah menjadi numerik pada proyek ini adalah type, state, laundry_options, parking_options
  
+ Train Test Split

  Train test split aja proses membagi data menjadi data latih dan data uji. Data latih akan digunakan untuk membangun model, sedangkan data uji akan digunakan untuk menguji performa model. Pada proyek ini dataset sebesar 253979 dibagi menjadi 241280 untuk data latih dan 12699 untuk data uji.
  
+ Normalization

  Algoritma machine learning akan memiliki performa lebih baik dan bekerja lebih cepat jika dimodelkan dengan data seragam yang memiliki skala relatif sama. Salah satu teknik normalisasi yang digunakan pada proyek ini adalah Standarisasi dengan sklearn.preprocessing.StandardScaler.

## Modeling

+ Algoritma
  Penelitian ini melakukan pemodelan dengan 3 algoritma, yaitu K-Nearest Neighbour, Random Forest, dan
  + K-Nearest Neighbour
    K-Nearest Neighbour bekerja dengan membandingkan jarak satu sampel ke sampel pelatihan lain dengan memilih sejumlah k tetangga terdekat. Proyek ini menggunakan [sklearn.neighbors.KNeighborsRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsRegressor.html) dengan memasukkan X_train dan y_train dalam membangun model. Parameter yang digunakan pada proyek ini adalah :
    + `n_neighbors` = Jumlah k tetangga tedekat.

  + Random Forest
    Algoritma random forest adalah teknik dalam machine learning dengan metode ensemble. Teknik ini beroperasi dengan membangun banyak decision tree pada waktu pelatihan. Proyek ini menggunakan [sklearn.ensemble.RandomForestRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html) dengan memasukkan X_train dan y_train dalam membangun model. Parameter yang digunakan pada proyek ini adalah :
    + `n_estimators` = Jumlah maksimum estimator di mana boosting dihentikan.
    + `max_depth` = Kedalaman maksimum setiap tree.
    + `random_state` = Mengontrol seed acak yang diberikan pada setiap base_estimator pada setiap iterasi boosting.

  + Adaboost
    AdaBoost juga disebut Adaptive Boosting adalah teknik dalam machine learning dengan metode ensemble.  Algoritma yang paling umum digunakan dengan AdaBoost adalah pohon keputusan (decision trees) satu tingkat yang berarti memiliki pohon Keputusan dengan hanya 1 split. Pohon-pohon ini juga disebut Decision Stumps. Algoritma ini bertujuan untuk meningkatkan performa atau akurasi prediksi dengan cara menggabungkan beberapa model sederhana dan dianggap lemah (weak learners) secara berurutan sehingga membentuk suatu model yang kuat (strong ensemble learner). Proyek ini menggunakan [sklearn.ensemble.AdaBoostRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostRegressor.html) dengan memasukkan X_train dan y_train dalam membangun model. Parameter yang digunakan pada proyek ini adalah :
    + `n_estimators` = Jumlah maksimum estimator di mana boosting dihentikan.
    + `learning_rate` = Learning rate memperkuat kontribusi setiap regressor.
    + `random_state` = Mengontrol seed acak yang diberikan pada setiap base_estimator pada setiap iterasi boosting.

## Evaluation

Pada Proyek ini menggunakan model machine learning bertipe regresi yang berarti Jika prediksi mendekati nilai sebenarnya, performanya baik. Sedangkan jika tidak, performanya buruk. Secara teknis, selisih antara nilai sebenarnya dan nilai prediksi disebut eror. Maka, semua metrik mengukur seberapa kecil nilai error tersebut.

Metrik yang akan kita gunakan pada prediksi ini adalah MSE atau Mean Squared Error yang menghitung jumlah selisih kuadrat rata-rata nilai sebenarnya dengan nilai prediksi. MSE didefinisikan dalam persamaan berikut.
<div><img src="https://user-images.githubusercontent.com/56061857/203086179-7189dfbf-bd13-493f-a517-6be0915cb4dc.png" width="450"/></div>
Keterangan:

N = jumlah dataset

yi = nilai sebenarnya

y_pred = nilai prediksi

Berikut Hasil perhitungan MSE

|			| train           | test             |
|----------|------------------|------------------|
| KNN  	| 30046.629366  | 134069.662903  |
| RF   | 18464.87181  | 135898.532381   |
| Boosting | 91101.065848  | 134836.83253 |

Untuk memudahkan, mari kita plot matrik tersebut dengan bar chart
<div><img src="https://user-images.githubusercontent.com/56061857/203088136-45f20cef-174e-460c-beba-213755fcd766.png" width="400"/></div>

Dari hasil evaluasi dapat dilihat bahwa model dengan algoritma Random Forest memiliki akurasi lebih tinggi tinggi dan tingkat error lebih kecil dibandingkan algoritma lainnya dalam proyek ini.
