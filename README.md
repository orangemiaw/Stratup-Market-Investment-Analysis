# Startup Investment (Crunchbase) Analysis using MongoDB

## Introduction
Dataset yang dianalisis merupakan data Investment Venture Capital (VC) terhadap beberapa StartUp dari berbagai market yang berbeda. Dataset ini dilansir dari halaman Kaggle, diambil dari situs Crunchbase. Crunchbase sendiri adalah platform untuk menemukan informasi bisnis tentang perusahaan swasta dan publik. Informasi Crunchbase termasuk informasi investasi dan pendanaan, anggota pendiri dan individu dalam posisi kepemimpinan, merger dan akuisisi, berita, dan tren industri.

Informasi yang berada pada dataset dapat digunakan sebagai alat bantu untuk market analysis mengenai StratUp yang trending dan sebagainya, hal ini bermanfaat untuk membantu Investor memilih sektor StartUp mana yang akan di investasikan dan juga bagi pelaku StartUp dapat membantu pemilihan model bisnis yang akan digunakan.

Kami memilih pembaahsan ini karena bagi kami sangatlah menarik untuk dibahas. :)

## Dataset
Kaggle Link: https://www.kaggle.com/arindam235/startup-investments-crunchbase

Total Data Original: 54.294

Total Data After Cleaning: 49.439

File Size (CSV/MongoDB): 11.95 MB / 43.49MB

## Tools Usage
- MongoDB Atlas
- MongoDB Charts
- MongoDB Console

## Credentials
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

## Server Spesification
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

## Query Performance Analysis
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

## Data Analysis
### Data Cleaning
Sebelum melakukan sebuah analisis perlu untuk melakukan pengecekan pada dataset apakah dataset tersebut sudah baik untuk digunakan. Setelah dilihat terdapat beberapa row data yang valuenya NaN atau kosong semua. Sehingga disini diperlukan pembersihan data (Data Cleaning) dengan cara menghapus data - data tersebut yang tidak terpakai. Untuk melakukan data cleaning kami menggunkan simple code PHP untuk melakukan pengecekan setiap row datanya memiliki field ```name``` atau tidak, jika didalam row tidak terdapat field ```name``` data row tersebut akan dihapus. Untuk melalukan data cleaning gunakan perintah:

```bash
php cleaner
```

Output yang didapatkan adalah kita memiliki jumlah data sebanyak *49.439* dari *54.294* data sebelumnya.

### Quick Analysis
***Beberapahal yang dapat diketahui sacara langsung:***
1. Jumlah startup sangatlah condong ke kiri dimana pada data ini condong ke 3 negara yaitu US, GBR dan CAN. 3 negara tersebut menyumbang paling banyak data startup pada dataset ini.
2. Dataset memiliki data startup yang berdiri dari tahun **1902 hingga 2014**.
3. Sebagian banyak stratup yang ada pada dataset ini masih berjalan sampai saat ini.
4. Analisis pada dataset ini dapat memberikan gambaran mengenai pengembangan startup yang dapat dilakukan maupun sebagai gambaran etrhadap investor untuk sebagai bahan pertimbangan memberikan funding.

#### Startups Founding Year

![IMG](images/charts/Startups-Founding-Year.png)

#### Status of Startups

![IMG](images/charts/Status-of-Startup.png)

## Disclaimer
***Note: modifications, changes, or alterations to this sourcecode is acceptable, however,any public releases utilizing this code must be approved by writen this application ( - Imam Kusniadi - ).***
