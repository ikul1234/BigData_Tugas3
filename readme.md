# Tugas3_BigData_05111740000169

- [Business Understanding](#business-understanding)
- [Data Understanding](#data-understanding)
- [Data Preparation](#data-preparation)
    - [File Reader File](#file-reader-file)
    - [File Reader versi SPARK](#file-reader-versi-spark)
- [Modeling](#modeling)
- [Evaluation](#evaluation)
- [Deployment](#deployment)
    - [konfigurasi node top 20](#konfigurasi-node-top-20)
  - [hasil deploy](#hasil-deploy)
- [susunan KNIME](#susunan-knime)
- [3.Perbandingan menggunakan timer info](#3perbandingan-menggunakan-timer-info)

## Business Understanding
Kemungkinan-kemungkinan yg dapat dilakukan yaitu:
1. Dari data tersebut, dapat dilakukan proses untuk melihat rekomendasi film terbaik yang dinilai oleh sebelumnya
2. Dari data tersebut, dapat dilihat berupa kumpulan data dengan isi kumpulan film yang disertai dengan ratings
3. Dari data tersebut, dapat dilakukan proses evaluasi serta filtering agar menghasilkan 10 film terbaik yang sudah dinilai sebelumnya

## Data Understanding

- Dataset menggambarkan penilaian dari movielens sebagai pelayanan movie recommendation, dataset ini berisi 20000263<br>
  peringkat, data ini dibuat oleh 138493 pengguna dari tanggal 09 januari 1995 sampai 31 maret 2015. dataset ini berisi<br>
  pengguna yang dipilih secara acak untuk dimasukkan. semua pengguna memiliki nilai setidaknya 20 film.
  
- dataset  masing-masing pengguna di representasi oleh id

- dataset berisi csv sebagai berikut :
  - tag.csv that berisi tags movie yang dilakukan oleh users:
  - userId
  - movieId
  - tag
  - timestamp

- rating.csv rating yang dilakukan oleh users:
  - userId
  - movieId
  - rating
  - timestamp
  
- movie.csv mengandung informasi dari movie:
  - movieId
  - title
  - genres
  
- link.csv pengidentifikasi yang disangkutpautkan ke source lain:
  - movieId
  - imdbId
  - tmbdId
  
- genome_scores.csv mengandung movie-tag relevance data:
  - movieId
  - tagId
  - relevance
  
- genome_tags.csv mengandung tag descriptions:
  - tagId
  - tag
  
- Source dataset : https://grouplens.org/datasets/movielens/

## Data Preparation

- data disiapkan dengan dua cara, yang satu menyiapkan data yang didownload dari link di atas, <br/>
  karena berbentuk csv dan menyiapkan data   yang menggunakan spark. dua persiapan tersebut dilakukan<br/> 
  dengan menyiapkan node di KNIME.

### File Reader File

- yang pertama membaca data dari file yang sudah di download dari file terlampir. yang dibaca hanya <br/>
  movies.csv<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/file_reader.PNG "file reader")<br>

- dengan melakukan konfigurasi seperti ini, dengan memastikan movieID terbaca<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/file_reader_conf.PNG "file reader conf")<br>

- dari ini kita melakukan konfigurasi di dalam node add fields untuk menambahkan userid dan timestamp.<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/add_fields.PNG "add field")<br>

- di dalam add field terdapat workflow yang dapat menyeting userid dan timestamp<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/node_add_fields.PNG "add field")<br>

- melakukan konfigurasi di dalam node constant value untuk menyeting timestamp dan node selanjutnya untuk menyeting 
  userid<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/node_timestamp.PNG "add field")
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/timestamp.PNG "add field")<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/node_userid.PNG "add field")
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/userid.PNG "add field")<br>

- melakukan penambahan node row splitter untuk memecah data untuk keperluan mentraining data serta memilih 20 film<br/>
  yang dipilih secara acak.<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/row_splitter.PNG "add field")<br>

- melakukan konfigurasi di dalam row splitter seperti ini<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/row_splitter_conf.PNG "add field")<br>

- open vie node tersebut untuk melihat hasilnya<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/node_rating.PNG "add field")<br>

- hasil rating didapati seperti ini, dengan keterangan tertera pada gambar, hasil ini sesuai dengan userid yang <br/>
  disetting<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/hasil_ratings.PNG "add field")<br>

### File Reader versi SPARK

- memulai dengan memasang node local big data environment untuk disambung ke node spark<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/local_bigdata.PNG "add field")<br>

- pastikan konfigurasi seperti ini<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/conf_local_bigdata.PNG "add field")<br>

- sambungkan local environment big data dengan spark, sehingga dapat membaca file dari directory yang tertera<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/spark.PNG "add field")
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/conf_spark.PNG "add field")<br>

- pasang node spark partitioning untuk melakukan partisi 80-20 pada data, setelah itu datanya digunakan untuk<br/> 
  training model dataset<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/partition.PNG "add field")<br>

- pastikan memilih persen dan memilih draw randomly.<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/conf_partition.PNG "add field")<br>

## Modeling

- proses modeling dimulai ketika menggabungkan data di node spark concatenate, dan node dari ini untuk<br>
  menjalankan algoritma buat modeling dengan memakai data training set, yang nantinya akan di tes dengan test-set.<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/model.PNG "add field")<br>

## Evaluation

- data yang sudah diprediksi berupa seperti ini
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/hasilprediksi.PNG "add field")<br>

- dari data yang sudah di modeling dan dari proses evaluasi ini menghapus juga data NAN.setalah itu terdapat hasil<br>
  untuk menghitung kesalahan antar peringkat film yang awal dan film yang diprediksi.<br/>
 ![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/evaluation.PNG "add field")<br>
 
 - didapati hasil evaluasi dari percobaan seperti berikut<br>
  ![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/hasil_prediksi.PNG "add field")<br>
 

## Deployment

- workflow ini akan mendisplay 10 peringkat prediksi film rekomendasi<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/deploy.PNG " asli csv")<br/>

### konfigurasi node top 20
- tidak lupa menjalankan file reader untuk di join dengan file prediksi<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/deploy_read.PNG " asli csv")<br/>

- jalankan spark predictor dan akan mendapati hasil seperti ini<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/spark_deploy.PNG " asli csv")<br/>

- kemudian jalankan spark to table untu mengubah spark ke dalam table, kemudian masuk ke konfigurasi movies recommended<br>
  dan di dalamnya ada file reader dan joiner, untuk menggabungkan data yang awal dengan data yang telah diprediksi.<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/recom.PNG " asli csv")<br/>

- row filter disini untuk menghapus hasil prediksi yang hasilnya NAN<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/row_predik.PNG " asli csv")<br/>

- dan untuk mengurutkan data hasilnya dipilih ascending untuk nilai kolom prediction<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/asc.PNG " asli csv")<br/>

- row filter dipakai dua kali, untuk row filter terakhir digunakan untuk mengambil best 10 nya, dan outputnya<br>
  disimpan ke direktori ke yang sudah kita pasang menggunakan csv writer.<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/best.PNG " asli csv")<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/excel.PNG " asli csv")<br/>

## hasil deploy
- hasil yang didapati adalah sebagai berikut sesuai dengan arahan untuk memberi best 10 movie<br>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/hasil_done.PNG " asli csv")<br/>

## susunan KNIME
susunan KNIME seperti berikut
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/arsitek.PNG " asli csv")<br/>

## 3.Perbandingan menggunakan timer info

- untuk melakukan perbandingan antara csv to spark dengan reader, kita harus menambahkan node seperti berikut<br> 
  dan mengarahkan kepada data csv yang sama<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/timer.PNG " asli csv")<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/konfi.PNG " asli csv")<br/>

- perbedaan data sebagai berikut<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/spark_time.PNG " asli csv")<br/>
![alt text](https://github.com/farizmpr/Bigdata-2020/blob/master/tugas_3/picture/read_time.PNG " asli csv")<br/>

- dari data diatas sangat jauh perbedaan antara csv to spark dengan file reader, file reader hanya melakukan pengambilan<br/>
  data yang sangat besar dan ketika melakukan eksekusi, komputer hanya melakukan itu  sendiri tanpa bantuan framework<br/>
  computing apapun, tidak seperti spark yang merupakan open source cluster framework, spark itu untuk pemrosesan data<br/>
  yang lebih cepat, karena data yang dipakai juga besar, jadi terdapat perbedaan waktu yang mencolok.<br/>
