# PROJECT 6

## DATA STREAMING : CONFLUENT
<div align="center"> 
  <img src="https://github.com/lolipop20/project6/blob/master/pics/Confluent-logo-3-1.jpeg">
</div>

Confluent adalah sebuah perusahaan yang fokus pada platform streaming data yang handal bernama ***Confluent Platform***

- Distribusi *Apache Kafka* tingkat enterprise, sebuah *platform* standar industri untuk *streaming data real-time.*
- Menyediakan *platform* yang handal, skalabel, dan aman untuk menangani aliran data secara *real-time.*
- Memungkinkan bisnis untuk membangun dan menerapkan aplikasi *data real-time* dengan lebih mudah dibandingkan dengan menggunakan *Apache Kafka* secara langsung.
- *Confluen*t menawarkan solusi komprehensif untuk membangun dan mengelola *pipeline* pemrosesan *data real-time*

***

## 1. Buat environment
### Pertama kita membuat environment dengan ***Click + Add Environment. Specify an Environment Name dan Click Create.*** 

<div>
  <img src="https://github.com/lolipop20/project6/blob/master/pics/create-environment.png">
</div>

***

## 2. Pilih Cluster
### 2.1 Pilih GCP Jakarta Region for Stream Governance Essentials, click Continue.

<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/pilih-cluster2.png">
</div>

### 2.2 Setelah membuat ***environment*** kemudian memilih ***cluster*** sesuai kebutuhan ***user***
<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/pilih-cluster.png">
</div>

### 2.3 Pilih pembayaran, jikalau ingin mencoba/***trial*** bisa menggunakan ***code promo***: *CONFLUENTDEV1*

<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/pilih-cluster3.png">
</div>

### 2.4 Click Launch Cluster.

<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/pilih-cluster5.png">
</div>

***

## 3. Buat sebuah ksqlDB Application
1. Di menu navigation, pilih ksqlDB dan *click Create Application Myself*.
2. Pilih *Global Access dan Continue*.
3. Beri nama *ksqlDB applicatio*n sesuai kebutuhan dan *Click Launch Application*

<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/create-ksqldb.png">
</div>

 ***

## 4. Buat TOPIC
1. Di dalam navigation menu, pilih Topics dan click Create Topic.
2. masukan dalam kolom nama dengan nama ***users_topic***, number of partitions yaitu 1, dan kemudian click Create with defaults.

<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/create_user_topic.png">
</div>

3. ulangin langkah ke 2 namun dengan nama masukan ***stocks_topic***

<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/create_stock_topic.png">
</div>

***

## 5. Buat *API KEY*
1. Click ***API KEY*** di navigation menu
2. Kemudian ***Create KEY***

<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/create-apikey.png" width="70%">
</div>

3. Pilih Akses Global lalu ***Click Next***.
4. Salin atau simpan ***API KEY*** dan ***SECRET*** di *folder* masing-masing.
5. Setelah membuat dan menyimpan *API KEY*, kita bisa melihat *API KEY* ini di *Confluent Cloud UI* di bagian *API KEYS*.

***

## 6. Buat Datagen Connectors untuk ***Users_topic*** dan ***Stocks_topic***
1. Pertama, kita akan membuat konektor yang akan mengirim data ke *users_topic*. Dari *Confluent Cloud UI*, klik tab Connectors pada menu navigasi. Click *icon* Datagen Source.

<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/connector_sample_data.png">
</div>

2. Masukkan detail konfigurasi berikut ini. Kolom lainnya dapat dikosongkan.

<div align="center">

| setting                            | value                                    |
|------------------------------------|------------------------------------------|
| name                               | DatagenSourceConnector_users             |
| api key                            | Sesuaikan dengan *API KEY* yang dibuat   |
| api secret                         | Sesuaikan dengan *API KEY* yang dibuat   |
| topic                              | users_topic                              |
| output message format              | AVRO                                     |
| quickstart                         | Users                                    |
| max interval between messages (ms) | 1000                                     |
| tasks                              | 1                                        |

</div>

<br>

<div>
  <img src="https://github.com/lolipop20/project6/blob/master/pics/sample_data_user_topic2.png">
</div>

4. Sebelum meluncurkan *connector*, pastikan data masukan sama, jika semuanya sudah sama, pilih *Launch*.

<div>
  <img src="https://github.com/lolipop20/project6/blob/master/pics/sample_data_user_topic4.png">
</div>

5. Ulangi langkah 1 sampai 4, namun kali ini untuk masukan *stocks_topic*

<div align="center">

| setting                            | value                                    |
|------------------------------------|------------------------------------------|
| name                               | DatagenSourceConnector_stocks            |
| api key                            | Sesuaikan dengan *API KEY* yang dibuat   |
| api secret                         | Sesuaikan dengan *API KEY* yang dibuat   |
| topic                              | stocks_topic                             |
| output message format              | AVRO                                     |
| quickstart                         | Stocks trade                             |
| max interval between messages (ms) | 1000                                     |
| tasks                              | 1                                        |

</div>

6. jika semua sudah sama, *click launch* maka hasilnya seperti ini

<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/connector-data-result.png">
</div>

***

## 7. Buat sebuah *Stream* dan *Table*
1. Navigasikan ke tab ksqlDB dan klik nama aplikasi yang sudah dibuat
2. Pertama, buatlah sebuah Stream dengan mendaftarkan *stocks_topic* sebagai sebuah *stream* bernama *stocks_stream.*

```sql
CREATE STREAM stocks_stream (
    side varchar, 
    quantity int, 
    symbol varchar, 
    price int, 
    account varchar, 
    userid varchar
) 
WITH (kafka_topic='stocks_topic', value_format='AVRO');
```
<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/query-1.png">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/stock_stream.png">
</div>

3.Klik ***Query Stream*** yang akan membawa ke Editor. konfigurasi properti ***auto.offset.reset=earliest*** sebelum mengklik ***Run query***.

```sql
SELECT * FROM STOCKS_STREAM EMIT CHANGES;
```
<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/stock_stream_run-query.png">
</div>

4. *Click Stop* untuk menghentikan *query*

***

## 8.Windowing Operations and Fraud Detection
1. membuat tabel bernama accounts_to_monitor dengan akun-akun yang akan dimonitor berdasarkan aktivitas mereka selama jangka waktu tertentu.

```sql
CREATE TABLE accounts_to_monitor_luthfinadli WITH (KAFKA_TOPIC='accounts_to_monitor_luthfinadli') AS
    SELECT ACCOUNT,
           AS_VALUE(ACCOUNT) AS ACCOUNT_NAME,
           COUNT(*) AS quantity,
           TIMESTAMPTOSTRING(WINDOWSTART, 'yyyy-MM-dd HH:mm:ss Z') AS WINDOW_START,
           TIMESTAMPTOSTRING(WINDOWEND, 'yyyy-MM-dd HH:mm:ss Z') AS WINDOW_END
    FROM STOCKS_ENRICHED
    WINDOW TUMBLING (SIZE 5 MINUTES)
    GROUP BY ACCOUNT
    HAVING COUNT(*) > 10;
```
2. Setelah membuat tabel accounts_to_monitor_luthfinadli, gunakan tab Editor atau tab Tables untuk meminta data dari tabel tersebut.

```sql
SELECT * FROM ACCOUNTS_TO_MONITOR_luthfinadli EMIT CHANGES;
```

- dan hasil ini adalah hasil dari *query* diatas
<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/account_to_monitor_result.png">
</div>

***

## 9.Connect BigQuery sink to Confluent Cloud
1. pertama kita buat *connector* baru untuk menyambungkan kita ke ***BigQuery***

<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/connector_big_query.png">
</div>

2. kemudian masukan konfigurasi dibawah kedalam tabel yang kosong

<div align="center">
  
| Setting                | Value                              |
|------------------------|------------------------------------|
| `Topics`               | accounts_to_monitor                |
| `Name`                 | BigQueryStorageSinkConnector_accounts_to_monitor    |
| `Input message format` | Avro                               |
| `Kafka API Key`        | Sesuaikan dengan *API KEY* yang dibuat                        |
| `Kafka API Secret`     | Sesuaikan dengan *API KEY* yang dibuat                        |
| `GCP credentials file` | Upload_your_GCP_Credentials_file   |
| `Project ID`           | your_GCP_project_ID                |
| `Dataset`              | your_GCP_dataset_name              |
| `Sanitize topics`      | true                               |
| `Sanitize field names` | true                               |
| `Auto create tables`   | PARTITION by INGESTION TIME        |
| `Partitioning type`    | DAY                                |
| `Input Kafka record key format`    | STRING                                |
| `Tasks`                | 1                                  |

</div>

3. kemudian kita pilih *topic accounts_to_monitor* yang telah kita buat, lalu *next*
4. pilih *Authentication method* ke *Google cloud service account*, masukan *file credentials, id project* , dan *nama dataset*

<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/bigquery-3.png">
</div>

5. *Click Next*
6. Lalu masukan nama *connector name* jika sudah sesuai *click Launch*

<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/bigquery-4.png">
</div>

7. kemudian kita *check* di *BigQuery*
8. jika berhasil maka data masukan diatas akan masuk kedalam *BigQuey* seperti ini
 
<div align="center">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/result-to-wh2.jpg">
  <img src="https://github.com/lolipop20/project6/blob/master/pics/result-to-wh.jpg" width="80%">
</div>

***






