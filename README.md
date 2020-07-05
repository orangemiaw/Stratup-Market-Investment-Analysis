# StratUp Investment (Crunchbase) Analysis using MongoDB

## Introduction
Dataset yang dianalisis merupakan data Investment Venture Capital (VC) terhadap beberapa StartUp dari berbagai market yang berbeda. Dataset ini dilansir dari halaman Kaggle, diambil dari situs Crunchbase. Crunchbase sendiri adalah platform untuk menemukan informasi bisnis tentang perusahaan swasta dan publik. Informasi Crunchbase termasuk informasi investasi dan pendanaan, anggota pendiri dan individu dalam posisi kepemimpinan, merger dan akuisisi, berita, dan tren industri.

Informasi yang berada pada dataset dapat digunakan sebagai alat bantu untuk market analysis mengenai StratUp yang trending dan sebagainya, hal ini bermanfaat untuk membantu Investor memilih sektor StartUp mana yang akan di investasikan dan juga bagi pelaku StartUp dapat membantu pemilihan model bisnis yang akan digunakan.

Kami memilih pembaahsan ini karena bagi kami sangatlah menarik untuk dibahas. :)

## Dataset
Link: https://www.kaggle.com/arindam235/startup-investments-crunchbase

Total Data: 54.294

Total Data (Clean): 49.439

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
    mongo "mongodb+srv://cluster0.01ube.mongodb.net/stratUp" --username admin
#### Import Dataset
    mongoimport --host atlas-wg6f26-shard-0/cluster0-shard-00-00.01ube.mongodb.net:27017,cluster0-shard-00-01.01ube.mongodb.net:27017,cluster0-shard-00-02.01ube.mongodb.net:27017 --ssl --username admin --password Devil123 --authenticationDatabase admin --db startUp --collection investments --type csv --file /home/ubuntu/StartUp-Investments-clean.csv --headerline

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

![IMG_1](images/find-all.PNG)

#### Get Startups Since 2000
```javascript
db.investments.find(
    { founded_year : { $gte: 2000 } }
).explain("executionStats")
```
- Total Returned Data: 34823
- Time: 29ms
- Screenshot:

![IMG_1](images/find-stratup-since-2000.PNG)

### Method: aggregat()
-------------

#### Get Total Startup in Each Market
```javascript
db.investments.aggregate( 
    {$group : { _id : { "market" : "$market", "name" : "$name" }, startup : { $sum : 1 } } }, 
    {$group : { _id : "$_id.market", startup : { $sum : 1 } } } 
).toArray()
```

## Disclaimer
***Note: modifications, changes, or alterations to this sourcecode is acceptable, however,any public releases utilizing this code must be approved by writen this application ( - Imam Kusniadi - ).***
