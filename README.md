# bq-data-ingestion
Ingest data from local (dockerized) PostgreSQL to BigQuery using Dataflow and Python transformation

## Instruction
The flow sequence is as follows:

1. Load data from a CSV file in the "weekly assignment" folder into a local PostgreSQL database.
2. Then perform data transformations, filtering, and enrichment, and provide reasons why these actions are necessary.
3. Once the data is clean or as desired, load it into Cloud SQL on Google Cloud Platform with the same table name.
4. After the data lands in Cloud SQL, offload it to BigQuery as is or in any desired format.

Here, you are free to create scenarios as you wish, whether to schedule the pipeline or not. The dataset used is the banksim artificial dataset available in the folder.

## Step
#### Installing PostgreSQL on the Docker
*please read the pdf*

#### Create User, Database, and Grant Access
To create database:
```
sudo -u postgres psql
```

Create user:
```
CREATE USER banksim WITH ENCRYPTED PASSWORD 'banksim';
```

Create database:
```
CREATE DATABASE banksim;
```

Grant access to database by user:
```
GRANT ALL PRIVILEGES ON DATABASE banksim TO banksim;
```

We now have a database we can access named `banksim` and the user `banksim`.
Use `\q` to leave from PostgreSQL.

#### Configure PostgreSQL for Remote Access
By default, PostgreSQL will restrict access for remote user for security purpose. To access it from remote access, we need to configure several things.
```
vim /var/lib/postgresql/data/pg_hba.conf
```

Add this line to the last line of the file.
```
host all all 0.0.0.0/0 md5
```

Edit the `postgresql.conf`.
```
vim /var/lib/postgresql/data/postgresql.conf
```

Find the section labeled `CONNECTIONS AND AUTHENTICATION` and add this line under.
```
listen_addresses = '*'
#listen_addresses = 'localhost'
```

Restart the PostgreSQL service.
```
brew services restart postgresql@14
```