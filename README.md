# Azure Database Migration

#### Tech Used: 
![Azure](https://img.shields.io/badge/azure-%230072C6.svg?style=for-the-badge&logo=microsoftazure&logoColor=white)![MicrosoftSQLServer](https://img.shields.io/badge/Microsoft%20SQL%20Server-CC2927?style=for-the-badge&logo=microsoft%20sql%20server&logoColor=white)![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)![Windows 11](https://img.shields.io/badge/Windows%2011-%230079d5.svg?style=for-the-badge&logo=Windows%2011&logoColor=white)![macOS](https://img.shields.io/badge/mac%20os-000000?style=for-the-badge&logo=macos&logoColor=F0F0F0)

#### In-project Noise Provided by: 
![Apple Music](https://img.shields.io/badge/Apple_Music-9933CC?style=for-the-badge&logo=apple-music&logoColor=white)

## Introduction

For this project I am going to architect and implement a cloud-based database for a manufactoring company's global operations. I will begin by establishing a database from a saved backup before migrating it to an Azure cloud-based database. As part of this project I will be focussing on crucial aspects of cloud and database engineering: data backup, automated scheduling and restoration. 
Once the database is established and working to a high standard, I will be simulating a disaster scenario that could encompass data loss. Furthermore I will be exploring the complexities of geo-replication and failover configurations to ensure data availablity under adverse conditions. 
Finally, to enhance database security I will be employing Microsoft Entra ID integration to define access roles, which will add an extra layer of control and protection to the cloud-based database. 
The following sections of this `README` will be a documentation the steps I take in order to achieve the final goal of the project. 


## Contents
1. Virtual Machine set-up
2. Creation of production database
3. To be continued... 

## Step 1: Setting up the virtual machine
In order to create a safe workspace to develop the database I have utilised Azure's virtual machine capabilities, this means that I have a non-physical operating system in which to build and test the database. The virtual machine is scalable to the needs of the company and database and it will be readily avaialable with minimal downtime. Configuring the virtual machine was rather straightforward; using a `standard_b2ms` system named `migration-vm`. Once it was accessable through Microsoft Remote Desktop I began installing the tools that I needed in order to make this project a success. 

## Step 2: Creation of the Production Database
For this stage I opted to download the database to a local machine but I have also saved a copy in the `adventurebackup` storage account as a precautionary measure. Using the `.bak` to restore the database in SMSS was rather straightforward, and at the moment it is being hosted on the `migration-vm` virtual machine. 