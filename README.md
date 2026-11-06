# Advanced Database Engineering Portfolio

An end-to-end database engineering case study for an Olist-style Brazilian online marketplace. I designed and implemented relational and document data models, analyzed SQL query performance, explored transaction and concurrency behavior, and documented cloud database operations and scaling decisions.

**Author:** `ochecodes` | **Project type:** Database engineering portfolio | **Status:** Implemented exercises and analysis; proposed architecture is identified as design work.

## Project overview

The marketplace brings together customers, sellers, products, orders, payments, delivery records, and reviews. I used the public Olist Brazilian E-Commerce dataset to examine how different database technologies support the same business. MySQL stores the normalized relational model and supports analytical queries and transactional examples. MongoDB provides a document-oriented model for order history and catalogue use cases. The repository includes implementation scripts, execution plans, test logs, screenshots, design notes, and measured results.

## What I built

| Area | Implementation | Evidence |
|---|---|---|
| Relational design | Nine-table MySQL schema, primary and foreign keys, constraints, ER diagram, normalization analysis, CSV loading, and validation | [`P1-database-design`](P1-database-design/) |
| Query optimization | Eight analytical queries, `EXPLAIN ANALYZE`, six workload-driven indexes, plan comparisons, and performance analysis | [`P2-query-optimisation`](P2-query-optimisation/) |
| Transactions and concurrency | Commit and rollback, isolation levels, lost update reproduction, row locking, atomic stock update, and deadlock detection | [`P3-transactions-concurrency`](P3-transactions-concurrency/) |
| Document modelling | MySQL-to-JSON export, MongoDB collections, embedding and referencing decisions, aggregation, indexing, and validation | [`P4-nosql`](P4-nosql/) |
| Cloud database operations | MongoDB Atlas deployment, replica-set observations, replication and write-concern experiments, plus a measured sharding proposal | [`P5-distributed-cloud`](P5-distributed-cloud/) |

## Results and lessons

- I loaded approximately **99,441 orders** and **1,000,163 geolocation records** across nine relational tables.
- A covering MySQL index reduced a postcode lookup from a full scan of one million geolocation rows to 103 matching index entries. The measured median time changed from about 1.88 seconds to 0.17 milliseconds on my test machine.
- I reproduced a lost update in concurrent sessions, then demonstrated `SELECT ... FOR UPDATE` and a conditional atomic update as remedies.
- I modelled a 20,000-order sample in MongoDB. Each order embeds bounded items, payments, reviews, and a customer snapshot. Products and sellers remain referenced as shared entities.
- My Atlas cluster showed one primary and two secondaries. At the measured document size, all 99,441 orders are estimated at about 80 MB, so sharding is not needed for this dataset. I documented a hashed customer key as a possible future choice if the workload grows enough to justify sharding.

Execution times depend on hardware, cache state, dataset state, network distance, and cloud tier. I discuss these limits alongside the measurements. Atlas failover and sharding were not exercised on the free tier, so I present those topics as design analysis rather than deployed features.

## Architecture and tools

- **Relational database:** MySQL Community Server 8.0.46, MySQL Workbench, and the command-line client
- **Document database:** MongoDB Atlas M0, MongoDB 8.0.36, MongoDB Compass, and its shell
- **Dataset:** Brazilian E-Commerce Public Dataset by Olist
- **Implementation artefacts:** SQL and MongoDB scripts, JSON examples, execution plans, session logs, screenshots, and analysis documents

This project focuses on database engineering and cloud database operations. It does not include a web application, automated infrastructure deployment pipeline, or continuous integration build. The Atlas cluster is deployed and was used for operational experiments. The broader production architecture and sharding configuration are proposals.

## Repository layout

```text
P1-database-design/           MySQL schema, ER model, loading, and validation
P2-query-optimisation/        Analytical SQL, indexes, plans, and performance analysis
P3-transactions-concurrency/  Transaction scripts, logs, and concurrency analysis
P4-nosql/                     MongoDB model, export, queries, and sample documents
P5-distributed-cloud/         Atlas setup, replication, consistency, and sharding design
data/                         Dataset download and setup instructions
```

## Reproducing the work

### Prerequisites

- MySQL Community Server 8.0 and MySQL Workbench or the `mysql` command-line client
- MongoDB Compass and access to a MongoDB Atlas cluster for the NoSQL and cloud exercises
- The Olist dataset CSV files, downloaded separately from Kaggle

### Setup

1. Clone the repository:

   ```bash
   git clone https://github.com/ochecodes/advanced-database-portfolio.git
   cd advanced-database-portfolio
   ```

2. Download the Brazilian E-Commerce Public Dataset by Olist from [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce). Follow [`data/README.md`](data/README.md) and extract the CSV files into `data/raw/`. Dataset files are intentionally excluded from Git.

3. Follow [`P1-database-design/README.md`](P1-database-design/README.md) to create and populate the `olist_marketplace` MySQL database. The load scripts use local file paths and may need adjustment for your MySQL installation and secure file import settings.

4. Follow the README in each project folder in sequence. Portfolios 2 and 3 use the MySQL database created in Portfolio 1. Portfolio 4 exports selected records to JSON and imports them into MongoDB. Portfolio 5 describes the Atlas deployment and experiments.

5. Use your own Atlas credentials and network access rules. Connection strings and secrets are not included in this repository. Do not commit credentials, private keys, or exported production data.

## Data and references

The project uses the [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), published on Kaggle under the CC BY-NC-SA 4.0 license. The dataset is not included here. Review its license before redistributing derived data or screenshots containing data.

Technical references include the MySQL 8.0 Reference Manual, MongoDB Manual, MongoDB Atlas documentation, and the analyses in each project folder. Results and screenshots are documented alongside the scripts that produced them.

## License and reuse

The Olist dataset has its own CC BY-NC-SA 4.0 license. That license applies to the dataset, not automatically to the code and documentation in this repository. No separate software license is provided here.
