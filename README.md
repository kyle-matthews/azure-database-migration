# Azure Database Migration

<div align='center'> 

![Azure](https://img.shields.io/badge/azure-%230072C6.svg?style=for-the-badge&logo=microsoftazure&logoColor=white)![MicrosoftSQLServer](https://img.shields.io/badge/Microsoft%20SQL%20Server-CC2927?style=for-the-badge&logo=microsoft%20sql%20server&logoColor=white)![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)![Windows 11](https://img.shields.io/badge/Windows%2011-%230079d5.svg?style=for-the-badge&logo=Windows%2011&logoColor=white)![macOS](https://img.shields.io/badge/mac%20os-000000?style=for-the-badge&logo=macos&logoColor=F0F0F0)

#### In-project Noise Provided by: 
![Apple Music](https://img.shields.io/badge/Apple_Music-9933CC?style=for-the-badge&logo=apple-music&logoColor=white)

![Geese Migrating](images/migratory-geese.png)
</div>

## Introduction

For this project I am going to architect and implement a cloud-based database for a manufactoring company's global operations. I will begin by establishing a database from a saved backup before migrating it to an Azure cloud-based database. As part of this project I will be focussing on crucial aspects of cloud and database engineering: data backup, automated scheduling and restoration. 
Once the database is established and working to a high standard, I will be simulating a disaster scenario that could encompass data loss. Furthermore I will be exploring the complexities of geo-replication and failover configurations to ensure data availablity under adverse conditions. 
Finally, to enhance database security I will be employing Microsoft Entra ID integration to define access roles, which will add an extra layer of control and protection to the cloud-based database. 
The following sections of this `README` will be a documentation the steps I take in order to achieve the final goal of the project. 


## Contents
1. Virtual Machine Set-up
2. Creation of Production Database
3. Setting up the Azure Database 
4. Data Migration
5. Data Backup and Restore

## Step 1: Setting up the Virtual Machine
In order to create a safe workspace to develop the database I have utilised Azure's virtual machine capabilities, this means that I have a non-physical operating system in which to build and test the database. The virtual machine is scalable to the needs of the company and database and it will be readily avaialable with minimal downtime. Configuring the virtual machine was rather straightforward; using a `standard_b2ms` system named `migration-vm`. Once it was accessable through Microsoft Remote Desktop I began installing the tools that I needed in order to make this project a success. 

![migration-vm](images/migration-vm.png)

## Step 2: Creation of the Production Database
For this stage I opted to download the database to a local machine but I have also saved a copy in the `adventurebackup` storage account as a precautionary measure. Using the `.bak` to restore the database in SMSS was rather straightforward, and at the moment it is being hosted on the `migration-vm` virtual machine. 

![Production Schema](images/recovered_production_database.png)

## Step 3: Setting up the Azure Database
I began by creating a new database server called `adventure-works-azure`, I thought this would be a suitable solution as it would provide Adventure Works with their own dedicated server, should they ever expand or require additional databases. Within this server I created the `adventure-works-cloud` database where the data was going to be migrated to. 
As a security measure I have only allowed connections from my IP address to access the database, all other public IP's are denied access. 

![Firewall rules](/images/firewall-rules.png)

## Step 4: Data Migration
After configuring the database it was time to begin the migration process. The first thing I did was compare the database schemas in `Azure Data Studio` using `SQL Scheme Compare v1.21.0`. 

![SQL Schema Compare](images/schema-compare.png)

(Please excuse the terminal over the top of everything. This Macbook I'm using is over 10 years old and a little bit shonky).

Thankfully, the extension decided the schemas were compatible and as a result the migration process was fairly smooth. Using the `Azure Migration v1.5.1` extension I was able to migrate the database to the cloud.
As part of this process I had to download `Microsoft Integration Runtime` and configure it using an authentication key in order to make the migration happen. 

![Azure Migration](images/Azure%20SQL%20migration.png)

## Step 5: Data Backup and Restore
For the next part of the project I created another Virtual Machine that I will be using to create a safety net for the original database incase of any unforeseen issues. Initially the backup was saved to the local machine but in order to add an extra layer of security a copy of the `.bak` file was uploaded to the `adventurebackup` storage account. 
Finally, for best practice the access keys for `adventurebackup` were saved as the credential `backupbarry` (similar to Wreck-it-Ralph) so that an automated backup schedule could be created. The Database now automatically creates backups at midnight every Sunday, a time when the database should be under a very low load. 

## Step 6: Disaster Recovery Simulation 
During this phase of the project I simulated data corruption within the database by creating a query that set a number values in `Person.EmailAddress` to `NULL`. 
![Table comparison](images/database-corrupt.png)
