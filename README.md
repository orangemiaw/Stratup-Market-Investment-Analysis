# Startup Investment (Crunchbase) Analysis using MongoDB

## 👋 Introduction
Dataset yang dianalisis merupakan data Investment Venture Capital (VC), Angel Investor, maupun Seed Stage Investment terhadap beberapa Startup dari berbagai kategori market yang berbeda. Dataset ini dilansir dari halaman Kaggle, diambil dari situs Crunchbase. Crunchbase sendiri adalah platform untuk menemukan informasi bisnis tentang perusahaan swasta dan publik. Informasi Crunchbase termasuk informasi investasi dan pendanaan, anggota pendiri dan individu dalam posisi kepemimpinan, merger dan akuisisi, berita, dan tren industri.

Informasi yang berada pada dataset dapat digunakan sebagai alat bantu untuk menghadirkan ide-ide startup yang cemerlang, dimana saat ini hal tersebut mungkin terasa sulit bagi calon wirausahawan terutama ketika tampaknya semua orang sudah menyapu setiap ide bagus untuk bisnis. Tetap saja, sangat mungkin untuk menjadi sukses dengan memperbaiki produk yang ada atau memutar ide unik pada ide lama. Manfaat wirausaha dapat membuat upaya meluncurkan startup sangat bermanfaat. Selain kebebasan yang berasal dari menjadi bos Anda sendiri, memulai bisnis membawa lebih banyak kemandirian, kepuasan kerja yang lebih besar, dan potensi penghasilan yang berpotensi terbuka. Hal tersebut yang melatar belakangi kami memilih topik ini.

## 📕 Dataset
Kaggle Link: https://www.kaggle.com/arindam235/startup-investments-crunchbase

Total Data Original: 54.294

Total Data After Cleaning: 49.439

File Size (CSV/MongoDB): 11.95 MB / 43.49MB

## 🌈 Tools Usage
- MongoDB Atlas
- MongoDB Charts
- MongoDB Console

## 🦄 Credentials
### Database Access
    Username: whiterose
    Password: justforfun666ECorp
    Privileges: Read an Write

    Username: dosen
    Password: vPKyrGO0G9AlPAd4
    Privileges: Only Read

### Database Connection
#### Login to Database
    mongo "mongodb+srv://cluster0.01ube.mongodb.net/stratUp" --username whiterose
#### Import Dataset
    mongoimport --host atlas-wg6f26-shard-0/cluster0-shard-00-00.01ube.mongodb.net:27017,cluster0-shard-00-01.01ube.mongodb.net:27017,cluster0-shard-00-02.01ube.mongodb.net:27017 --ssl --username whiterose --password justforfun666ECorp --authenticationDatabase whiterose --db startUp --collection investments --type csv --file /home/ubuntu/StartUp-Investments-clean.csv --headerline

## 💻 Server Spesification
### Cloud Server (MongoDB Atlas)
- Provider: Amazon Web Services (AWS)
- Region: Singapore (ap-southeast-1)
- Cluster Tier: M0 Sandbox (Shared RAM, 512 Storage)
- MongoDB Version: 4.2
- Auto Backup: No
- Cluster Name: Cluster0
### Client (MongoDB Console)
- Provider: Amazon Web Services (AWS) / Amazon Lightsail
- Region: Seoul, South Korea (ap-southeast-2)
- Memory: 32 GB
- Processing: 8 vCPUs
- Storage: 640 GB SSD
- Transfer: 7 TB

## 🔥 Query Performance Analysis
Performa eksekusi query dari database, dengan daftar query pada method API: find(), shord(), dan aggregat (sum, avg, min, dan max)

### Method: find()
-------------

#### Get All Data
```javascript
db.investments.find().explain("executionStats")
```
- Total Returned Data: 49438
- Time: 16ms
- Screenshot:

![IMG](images/find-all.PNG)

#### Get Startups Since 2000
```javascript
db.investments.find(
    { founded_year : { $gte: 2000 } }
).explain("executionStats")
```
- Total Returned Data: 34823
- Time: 29ms
- Screenshot:

![IMG](images/find-stratup-since-2000.PNG)

### Method: aggregat()
-------------

#### Get Total Startup in Each Market
```javascript
db.investments.aggregate( 
    {$group : { _id : { "market" : "$market", "name" : "$name" }, startup : { $sum : 1 } } }, 
    {$group : { _id : "$_id.market", startup : { $sum : 1 } } } 
)
```
- Total Returned Data: NaN
- Time: NaN
- Screenshot:

![IMG](images/aggregat-startup-market.PNG)

#### Get Current Startup Status
```javascript
db.investments.aggregate( 
    {$group : { _id : { "status" : "$status", "name" : "$name" }, startup : { $sum : 1 } } }, 
    {$group : { _id : "$_id.status", startup : { $sum : 1 } } } 
)
```
- Total Returned Data: NaN
- Time: NaN
- Screenshot:

![IMG](images/aggregat-startup-status.PNG)

## 👨‍🔬 Data Analysis
### Data Cleaning
Sebelum melakukan sebuah analisis perlu untuk melakukan pengecekan pada dataset apakah dataset tersebut sudah baik untuk digunakan. Setelah dilihat terdapat beberapa row data yang valuenya NaN atau kosong semua. Sehingga disini diperlukan pembersihan data (Data Cleaning) dengan cara menghapus data - data tersebut yang tidak terpakai. Untuk melakukan data cleaning kami menggunkan simple code PHP untuk melakukan pengecekan setiap row datanya memiliki field ```name``` atau tidak, jika didalam row tidak terdapat field ```name``` data row tersebut akan dihapus. Untuk melalukan data cleaning gunakan perintah:

```bash
php cleaner
```

Output yang didapatkan adalah kita memiliki jumlah data sebanyak **49.439 dari 54.294** data sebelumnya.

### Quick Analysis
***Beberapahal yang dapat diketahui sacara langsung:***
1. Jumlah startup sangatlah condong ke kiri dimana pada data ini condong ke 3 negara yaitu US, GBR dan CAN. 3 negara tersebut menyumbang paling banyak data startup pada dataset ini.
2. Dataset memiliki data startup yang berdiri dari tahun **1902 hingga 2014**.
3. Sebagian banyak stratup yang ada pada dataset ini masih berjalan sampai saat ini.
4. Analisis pada dataset ini dapat memberikan gambaran mengenai pengembangan startup yang dapat dilakukan maupun sebagai gambaran terhadap investor untuk sebagai bahan pertimbangan memberikan funding.

#### Startups Founding Year

![IMG](images/charts/Startups-Founding-Year.png)

Dari chart di atas dapat diketahui bahwa pertumbuhan startup tiap tahunya semakin pesat ditunjukan dengan block chart yang semakin naik. Dimana pada rentang tahun 2000 - 2014 pertumbuhan stratup sangatlah pesat.

#### Startups Since 2000

![IMG](images/charts/Stratups-Since-2000.png)

Pertumbuhan startup - startup baru dari tahun 2000 secara lebih jelas dapat dilihat pada chart diatas. Dari chart tersebut pertumbuhan startup terbanyak terjadi pada tahun **2012** dimana terdapat **5211** startup baru berdiri, kemudian dari tahun 2000 sampai 2015 tren pertumbuhan startup naik. Namun, pada tahun 2013 sampai 2014 pertumbuhan startup menunjukan penurunan. Hal ini dapat dipengaruhi oleh beberapa sebab.

#### Distribution of Startup Across Countries

![IMG](images/charts/Distribution-of-Startup-Across-Countries.png)

Pendistribusian startup berdasarkan negara dikuasai oleh Amerika Serikat dengan memiliki startup 28793 startup yang telah berdiri. Kemudian diikuti United Kingdom dan Canada dibelakangnya.

#### Top States for Startup in USA

![IMG](images/charts/Top-States-for-Startup-in-USA.png)

California memiliki jumlah startup maksimum dibandingkan dengan semua negara bagian lain di Amerika Serikat (USA).

#### Status of Startups

![IMG](images/charts/Status-of-Startup.png)

Chart diatas menunjukan bahwa sebagain besar startup masih terus berjalan (operating). Dari halaman Kaggle sendiri dataset terakhirkali diperbarui pada tanggal **18-02-2020**.

## Disclaimer
***Note: modifications, changes, or alterations to this sourcecode is acceptable, however,any public releases utilizing this code must be approved by writen this application ( - Imam Kusniadi - ).***
