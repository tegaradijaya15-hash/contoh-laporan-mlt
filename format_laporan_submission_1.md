# Laporan Proyek Machine Learning - Ahmad Tegar Adi Jaya - 231113023

## Project Overview
Di era konsumsi musik digital saat ini, platform streaming menghadapi tantangan besar dalam menyajikan konten yang relevan di tengah jutaan katalog lagu yang tersedia. Salah satu pendekatan yang paling efektif untuk memecahkan masalah ini adalah melalui sistem rekomendasi. Namun, sering kali sistem rekomendasi hanya mengandalkan popularitas atau riwayat klik pengguna, yang mengakibatkan lagu-lagu dari wilayah tertentu atau dengan karakteristik unik menjadi terabaikan

**Rubrik/Kriteria Tambahan (Opsional)**:
- Jelaskan mengapa dan bagaimana masalah tersebut harus diselesaikan
Masalah ini harus diselesaikan untuk mengatasi fenomena cold start pada lagu baru atau musik dari wilayah terpencil yang sulit mendapatkan rekomendasi karena belum memiliki riwayat putar, serta untuk memecah batasan popularitas (filter bubble) agar penemuan musik didasarkan pada karakteristik audio yang objektif. Dengan memanfaatkan fitur teknis seperti irama dan harmoni, sistem dapat menyajikan personalisasi yang lebih akurat sekaligus mendemokratisasi akses terhadap musik dari berbagai belahan dunia berdasarkan kemiripan "jiwa" audionya, bukan sekadar tren pasar atau lokasi geografis semata
- Menyertakan hasil riset terkait atau referensi. Referensi yang diberikan harus berasal dari sumber yang kredibel dan author yang jelas.
Berikut hasil riset, referensi & authornya: Riset ini membahas penggunaan algoritma regresi dan klasifikasi untuk memetakan fitur audio ke koordinat bumi, yang menjadi dasar teknis mengapa fitur-fitur dalam dataset ini sangat efektif untuk sistem rekomendasi berbasis konten (Content-Based Filtering).
[Zhou, F., Q, C., & King, R. D. (2014). Predicting the Geographical Origin of Music. Dalam Proceedings of the 2014 IEEE International Conference on Data Mining (ICDM).]
- Sumber yang digunakan [Scholar](https://scholar.google.com/)

## Business Understanding
Proses klarifikasi masalah dimulai dengan mengidentifikasi bahwa dataset ini memerlukan pendekatan Content-Based Filtering karena ketiadaan data pengguna, dilanjutkan dengan menormalisasi 116 fitur audio untuk memastikan keadilan bobot data. Masalah diselesaikan dengan menghitung jarak kedekatan antar fitur menggunakan metrik seperti Cosine Similarity, di mana hasil akhirnya diklarifikasi kembali melalui data koordinat geografis untuk melihat apakah terdapat pola konsistensi antara karakter suara dan asal wilayah musik tersebut.

### Problem Statements
**Menjelaskan pernyataan masalah:
- Bagaimana membangun sistem rekomendasi yang tetap akurat dan relevan pada dataset yang tidak memiliki riwayat interaksi pengguna (rating atau klik), sehingga lagu hanya bisa disarankan berdasarkan kesamaan karakteristik fisik audio dan asal geografisnya?
- Bagaimana mengolah dan menyederhanakan 116 fitur audio yang kompleks (termasuk fitur standar dan kromatik) agar mesin dapat secara efektif mengukur tingkat kemiripan antar lagu untuk menghasilkan rekomendasi yang koheren secara musikal?

### Goals

Menjelaskan tujuan proyek yang menjawab pernyataan masalah:
- Membangun Model Rekomendasi Berbasis Konten (Content-Based Filtering):
Tujuan utamanya adalah menciptakan sistem yang dapat memberikan saran musik secara akurat dengan hanya mengandalkan ekstraksi fitur audio dan data geografis. Hal ini bertujuan untuk memecahkan masalah cold start, sehingga lagu-lagu baru atau lagu dari wilayah yang kurang populer tetap dapat direkomendasikan kepada pengguna berdasarkan kesamaan karakteristik audionya tanpa memerlukan data riwayat pengguna.
- Mengoptimalkan Pengolahan Data Audio Multi-Dimensi:
Tujuannya adalah menerapkan teknik feature engineering dan perhitungan metrik kemiripan (seperti Cosine Similarity atau Euclidean Distance) untuk mengolah 116 fitur audio yang kompleks. Dengan demikian, sistem dapat menentukan derajat kedekatan antar lagu secara efisien, memastikan hasil rekomendasi memiliki koherensi musikal yang tinggi, dan mampu memetakan hubungan antara karakter suara dengan asal geografis musik tersebut.

    ### Solution statements
- Implementasi Algoritma K-Nearest Neighbors (KNN):
Solusi utama adalah menggunakan algoritma KNN untuk menghitung kemiripan antar lagu. KNN bekerja dengan mencari sejumlah 'k' tetangga terdekat dari sebuah lagu target berdasarkan jarak fitur audionya.
Penerapan: Menggunakan metrik Euclidean Distance atau Manhattan Distance pada fitur audio untuk mengidentifikasi lagu-lagu yang memiliki profil teknis serupa.
- Pengukuran Kedekatan dengan Cosine Similarity;
Untuk mengatasi variasi skala pada fitur audio dan fokus pada orientasi atau pola musik (seperti pola ritme atau harmoni), digunakan Cosine Similarity.
Penerapan: Menghitung sudut antar vektor fitur lagu. Semakin mendekati angka 1 (sudut 0 derajat), maka semakin mirip karakteristik konten dari kedua lagu tersebut, terlepas dari perbedaan volume atau intensitas data mentahnya.
- Normalisasi Data Menggunakan StandardScaler:
Karena fitur audio standar dan fitur kromatik memiliki rentang nilai yang berbeda-beda, diperlukan proses standarisasi.
Penerapan: Menerapkan StandardScaler untuk mengubah data sehingga memiliki rata-rata (mean) 0 dan standar deviasi 1. Hal ini memastikan tidak ada satu fitur (seperti frekuensi tinggi) yang mendominasi perhitungan kemiripan hanya karena skala angkanya lebih besar dari fitur lain.

## Data Understanding
dataset ini adalah Geographical Original of Music yang bersumber dari UCI Machine Learning Repository. Dataset ini sangat spesifik karena menghubungkan karakteristik fisik suara dengan lokasi geografis asal musik tersebut. dengan jumlah data 1059 trek musik
berikut sumber datasets yang di gunakan; (https://archive.ics.uci.edu/dataset/315/geographical+original+of+music)

Variabel-variabel pada Restaurant UCI dataset adalah sebagai berikut:
- song_id : Sebagai Identitas Unik (Primary Key) untuk setiap lagu dalam dataset.
- region : Menunjukkan Wilayah atau Area Geografis tempat musik tersebut berasal dalam bentuk kategori.
- longitude : Memberikan posisi geografis yang Sangat Presisi menggunakan sistem koordinat global(Longitude (Bujur): Menentukan posisi Timur-Barat)
- latitude : Memberikan posisi geografis yang Sangat Presisi menggunakan sistem koordinat global(Latitude (Lintang): Menentukan posisi Utara-Selatan)
- lat_long : Merupakan Variabel Kombinasi (Tuple/String) yang menggabungkan latitude dan longitude menjadi satu kolom

## Data Preparation
**Rubrik/Kriteria Tambahan (Opsional)**: 
** Menjelaskan proses data preparation yang dilakukan

- Memisahkan Fitur (Features) dan Target (Labels)
Dataset ini mencampurkan fitur audio dengan koordinat geografis dalam satu baris. Langkah pertama adalah memisahkannya.
Fitur (X): Kolom 1 sampai 68 (atau 116 jika menggunakan fitur kromatik). Ini adalah data yang akan dipelajari mesin.
Target (y): Dua kolom terakhir (Latitude dan Longitude). Dalam sistem rekomendasi berbasis konten, target ini sering digunakan sebagai label untuk memvalidasi kemiripan lokasi.
- Penanganan Skala Data (Feature Scaling)
Data audio dalam dataset ini memiliki rentang nilai yang sangat beragam (beberapa bernilai negatif kecil, yang lain bernilai positif besar). Algoritma seperti KNN atau Cosine Similarity sangat sensitif terhadap perbedaan skala ini.
- Reduksi Dimensi (Dimensionality Reduction)
Dataset ini memiliki dimensi yang cukup tinggi (116 fitur). Terlalu banyak fitur dapat menyebabkan masalah Curse of Dimensionality, di mana jarak antar data menjadi kurang bermakna
-Pengecekan Missing Values dan Duplikasi
Memastikan tidak ada baris yang kosong atau data yang terduplikasi. Jika ada lagu yang identik fitur audionya tetapi memiliki ID berbeda, salah satunya harus dihapus agar tidak terjadi bias pada hasil rekomendasi.
- Pembagian Data (Data Splitting)
Membagi data menjadi Training Set (untuk membangun indeks kemiripan) dan Test Set (sebagai simulasi "lagu yang baru didengar pengguna" untuk dicarikan rekomendasinya).
- Pembuatan Matriks Kemiripan (Similarity Matrix)
Mengonversi data yang sudah distandarisasi menjadi matriks Cosine Similarity. Matriks ini akan menjadi "otak" dari sistem rekomendasi yang menyimpan skor kemiripan antara setiap lagu di dalam dataset.
- Pemberian ID Unik:
Membuat identitas (song_id) untuk setiap lagu karena dataset asli hanya berupa angka tanpa label.
- Pemisahan Data:
Memisahkan fitur audio (kolom 1-116) sebagai input mesin dan koordinat geografis (Latitude/Longitude) sebagai label informasi asal musik.
- Normalisasi (Scaling):
Menyamakan skala nilai semua fitur audio menggunakan Standardization agar fitur dengan angka besar tidak mendominasi perhitungan kemiripan.
- Penghitungan Kemiripan:
Mengonversi data fitur menjadi Matriks Cosine Similarity yang berfungsi sebagai "peta" untuk menemukan lagu dengan karakteristik suara paling identik.
- Pemetaan Data (Data Mapping):
adalah proses menghubungkan hasil perhitungan teknis kembali ke informasi yang bisa dipahami manusia. Dalam dataset ini, pemetaan dilakukan untuk mengaitkan fitur audio dengan identitas lagu dan lokasi geografisnya.

** Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

- Menghindari Dominasi Fitur (Masalah Skala):
Fitur audio memiliki satuan yang berbeda-beda. Tanpa Scaling (Normalisasi), fitur dengan rentang angka besar (misal: frekuensi dalam ribuan) akan dianggap lebih penting oleh algoritma dibanding fitur dengan rentang kecil (misal: nada dalam skala 0-1), padahal keduanya sama pentingnya.
- Mengatasi Ketiadaan Label (Memberi Identitas):
Dataset asli hanya berisi angka fitur audio dan koordinat. Kita perlu Pemetaan (Mapping) dan pembuatan Song ID agar sistem bisa memberi tahu pengguna lagu mana yang direkomendasikan, bukan sekadar memberikan daftar angka koordinat yang membingungkan.
- Mengonversi Karakteristik Suara menjadi Angka yang Terukur:
Melalui pembentukan Similarity Matrix, kita mengubah karakteristik abstrak seperti "nuansa musik" menjadi skor kemiripan matematis yang pasti. Ini memungkinkan komputer untuk menghitung seberapa mirip lagu dari Afrika dengan lagu dari Amerika Selatan secara objektif.
- Meningkatkan Efisiensi Komputasi:
Memproses 116 fitur secara langsung untuk setiap pencarian bisa sangat lambat. Tahapan seperti Reduksi Dimensi atau pembersihan data duplikat membantu sistem bekerja lebih cepat dalam memberikan rekomendasi secara real-time.
- Menjamin Konsistensi Data:
pembersihan data memastikan tidak ada nilai yang hilang (null) yang bisa menyebabkan error saat perhitungan algoritma sedang berjalan.

## Modeling
**Rubrik/Kriteria Tambahan (Opsional)**: 
**Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Content-Based Filtering dengan Cosine Similarity
Solusi ini berfokus pada orientasi fitur. Algoritma ini menghitung sudut antara dua vektor lagu untuk menentukan seberapa mirip pola suara mereka.
Cara Kerja: Model membandingkan input lagu dengan seluruh dataset dan mencari nilai skor mendekati 1.0 (sangat mirip).
- K-Nearest Neighbors (KNN)
Solusi ini berfokus pada jarak spasial. Algoritma ini mencari sejumlah $k$ tetangga terdekat dalam ruang multidimensi.
Cara Kerja: Setiap lagu dianggap sebagai titik koordinat. Model menghitung jarak lurus (seperti penggaris) antara satu titik lagu ke titik lainnya. Lagu dengan jarak fisik terpendek dalam ruang fitur dianggap paling serupa.

** Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.
- Content-Based Filtering dengan Cosine Similarity
Kelebihan:
Sangat efektif untuk data dengan banyak fitur (116 fitur) karena tidak terpengaruh oleh perbedaan "volume" atau besaran angka absolut, melainkan fokus pada pola frekuensi dan ritme.
kekurangan:
Mengabaikan Magnitudo: Karena hanya mengukur sudut, algoritma ini tidak mempedulikan perbedaan intensitas atau volume suara yang mungkin penting bagi beberapa pendengar.
Ketidakmampuan Menangani Outlier: Jika terdapat data yang rusak atau memiliki nilai ekstrem, arah vektor bisa melenceng jauh dan merusak hasil rekomendasi.
- K-Nearest Neighbors (KNN)
Kelebihan:
Sangat bagus untuk mendeteksi kesamaan lokasi geografis secara langsung jika fitur audio memiliki korelasi linear yang kuat dengan koordinat asal musik.
Kekurangan:
Sangat Sensitif terhadap Skala:Jika Anda lupa melakukan StandardScaler secara sempurna, fitur dengan angka besar (seperti frekuensi Hz) akan mendominasi hasil perhitungan sepenuhnya, mengabaikan fitur nada (chromatic) yang angkanya lebih kecil.
Beban Komputasi Tinggi: KNN harus menghitung jarak ke seluruh 1.059 data satu per satu setiap kali ada request baru, yang bisa menjadi lambat jika dataset bertambah besar di masa depan.

## Evaluation
1. Analisis Metrik Cosine Similarity
Metrik utama yang Anda gunakan adalah skor antara 0 hingga 1.
Hasil:
sistem berhasil mengidentifikasi lagu-lagu dengan skor kemiripan yang tinggi (mendekati 1.0) untuk wilayah referensi.
Interpretasi:
Sebagai contoh, saat Anda memasukkan Region 29.0, 77.0, sistem menemukan SONG-870 (Region 12.0, 105.0) sebagai rekomendasi teratas. Hal ini menunjukkan bahwa meskipun secara geografis berbeda, secara metrik fitur audio, keduanya memiliki pola frekuensi yang sangat identik.
2. Evaluasi Top-N Recommendation (N=10)
Sistem mengevaluasi keberhasilan berdasarkan "tetangga terdekat" dalam ruang fitur.
Presisi Konten: Dari 1.059 baris data, model mampu menyaring 10 lagu yang paling relevan. Output pada notebook Anda menunjukkan urutan yang konsisten (dari yang paling mirip ke yang kurang mirip).
Keberagaman Geografis: Metrik ini menunjukkan hasil yang menarik. Model tidak hanya merekomendasikan musik dari lokasi yang sama, tetapi juga dari wilayah lain yang memiliki kemiripan budaya musik. Ini adalah indikator bahwa fitur audio berhasil menangkap karakteristik instrumen atau ritme lintas batas negara.
3. Visualisasi Sebagai Alat Evaluasi
Dalam notebook, Anda menyiapkan alat visualisasi (Matplotlib) untuk memahami sebaran data.
Evaluasi Distribusi: Dengan melihat scatter plot koordinat, kita bisa mengevaluasi apakah rekomendasi yang diberikan masuk akal secara spasial atau justru memberikan variasi baru bagi pendengar.

**Rubrik/Kriteria Tambahan (Opsional)**: 
**Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.
- Representasi Vektor: Setiap lagu dianggap sebagai satu titik panah (vektor) yang ditarik dari titik pusat (0,0). Karena Anda memiliki 116 fitur audio, maka panah ini berada dalam ruang 116 dimensi.
- Mengukur Sudut, Bukan Jarak: Berbeda dengan jarak Euclidean (garis lurus), Cosine Similarity hanya peduli pada arah atau pola fitur tersebut.Jika dua lagu memiliki pola frekuensi dan nada yang sama tetapi volume (magnitudo) berbeda, sudutnya akan tetap kecil ($0^\circ$), sehingga skornya tinggi.
- Rentang Skor:
- Skor 1.0 (Kosinus 0°): Berarti kedua lagu identik atau memiliki pola yang searah sempurna.
- Skor 0 (Kosinus 90°): Berarti kedua lagu tidak memiliki kesamaan sama sekali (ortogonal).
- Skor -1 (Kosinus 180°): Berarti kedua lagu berlawanan arah (jarang terjadi pada fitur audio positif).

**---Ini adalah bagian akhir laporan---**

_Catatan:_

<img width="584" height="455" alt="kc" src="https://github.com/user-attachments/assets/736a2102-7fbc-4be1-ac7b-651ccd69902d" />

Berikut ini penjelasan singkat pada gambar di atas:

Gambar di atas membuktikan bahwa Sistem Rekomendasi Berbasis Konten ini bekerja secara efektif. Masalah "Geographical Origin of Music" diselesaikan dengan cara menunjukkan bahwa musik adalah bahasa universal; pola audio dari satu wilayah dapat ditemukan kemiripannya di belahan dunia lain menggunakan perhitungan matriks Cosine Similarity.
