# Advanced Database Engineering Portfolio

An end-to-end database case study for an Olist-style Brazilian online marketplace. The project demonstrates relational data modelling, SQL performance analysis, transaction and concurrency control, MongoDB document design, and cloud database operations using a consistent dataset and workload.

**Portfolio owner and publisher:** `ochecodes`  
**Client and original project author:** Osimen Odigie  
**Project type:** Client portfolio case study  
**Status:** Implemented database exercises and analysis, with selected cloud architecture proposals clearly identified as proposals.

## Project overview

The marketplace brings together customers, sellers, products, orders, payments, delivery records, and reviews. I use the public Olist Brazilian E-Commerce dataset to explore how different database technologies support the same business. The relational system stores the normalized source model and supports reporting and transactional examples. A MongoDB model supports document-oriented order history and catalogue use cases. The project includes implementation scripts, execution evidence, design notes, and measured results.

This repository was originally developed by Osimen Odigie for an individual academic portfolio. I am publishing and presenting it as a client project with his attribution. The original student details and course submission report have been removed from the current project files. The original source repository may retain that earlier material in its history; this portfolio repository should be created as a fresh repository so its public history does not expose the client's student report. The technical implementation and client attribution are preserved in the project materials where relevant.

## What the project demonstrates

| Area | Work demonstrated | Evidence |
|---|---|---|
| Relational design | Nine-table MySQL schema, keys, constraints, ER diagram, normalization decisions, CSV loading, and data validation | [`P1-database-design`](P1-database-design/) |
| Query optimization | Eight analytical queries, `EXPLAIN ANALYZE`, six workload-driven indexes, plan comparisons, and timing caveats | [`P2-query-optimisation`](P2-query-optimisation/) |
| Transactions and concurrency | Commit and rollback, isolation levels, lost update reproduction, row locking, atomic stock update, and deadlock detection | [`P3-transactions-concurrency`](P3-transactions-concurrency/) |
| Document modelling | MySQL-to-JSON export, MongoDB collections, embedding and referencing decisions, aggregation, indexing, and validation | [`P4-nosql`](P4-nosql/) |
| Cloud and distribution | MongoDB Atlas replica-set observations, replication and write-concern experiments, deployment notes, and a measured sharding proposal | [`P5-distributed-cloud`](P5-distributed-cloud/) |

## Selected results

- The relational database contains approximately **99,441 orders** and **1,000,163 geolocation records** across nine source tables.
- A covering MySQL index reduced a postcode lookup from a full scan of one million geolocation rows to 103 matching index entries. The measured median time changed from about 1.88 seconds to 0.17 milliseconds on the test machine.
- The concurrency exercises reproduced a lost update, then demonstrated `SELECT ... FOR UPDATE` and a conditional atomic update as remedies.
- MongoDB stores a 20,000-order sample as documents with bounded order items, payments, reviews, and customer snapshots embedded. Products and sellers remain referenced as shared entities.
- The Atlas cluster showed one primary and two secondaries. The full order dataset is estimated at about 80 MB at the measured document size, so sharding is not needed for this dataset. A hashed customer key is documented as a possible future design if workload growth justifies sharding.

Execution times depend on hardware, cache state, dataset state, network distance, and cloud tier. The performance notes discuss those limits rather than treating one machine's timings as universal. Atlas failover and sharding were not exercised on the free tier; they are described as design analysis, not deployed features.

## Architecture and tools

- **Relational database:** MySQL Community Server 8.0.46 with MySQL Workbench and the command-line client
- **Document database:** MongoDB Atlas M0, MongoDB 8.0.36, MongoDB Compass, and its shell
- **Dataset:** Brazilian E-Commerce Public Dataset by Olist
- **Implementation artefacts:** SQL schema and query scripts, MongoDB JavaScript queries, JSON examples, execution plans, session logs, screenshots, and analysis documents

The project focuses on database engineering and operational experiments. The repository does not contain a web application, an automated infrastructure deployment pipeline, or a continuously integrated build. Cloud work covers an Atlas deployment and operational observations, with the broader production architecture kept explicitly as a proposal.

## Repository layout

```text
P1-database-design/          MySQL schema, ER model, loading, validation
P2-query-optimisation/       Analytical SQL, indexes, plans, performance analysis
P3-transactions-concurrency/ Transaction scripts, logs, and concurrency analysis
P4-nosql/                    MongoDB model, export, queries, and sample documents
P5-distributed-cloud/        Atlas setup, replication, consistency, and sharding design
data/                        Dataset download and setup instructions
```

## Reproducing the database exercises

### Prerequisites

- MySQL Community Server 8.0 and MySQL Workbench or the `mysql` command-line client
- MongoDB Compass and access to a MongoDB Atlas cluster for the cloud and NoSQL exercises
- The Olist dataset CSV files, downloaded separately from Kaggle

### Setup

1. Clone the repository:

   ```bash
   git clone https://github.com/ochecodes/advanced-database-portfolio.git
   cd advanced-database-engineering-portfolio
   ```

2. Download the Brazilian E-Commerce Public Dataset by Olist from [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce). Follow [`data/README.md`](data/README.md) and extract the CSV files into `data/raw/`. Dataset files are intentionally excluded from Git.

3. Follow [`P1-database-design/README.md`](P1-database-design/README.md) to create and populate the `olist_marketplace` MySQL database. The load scripts use local file paths and may need adjustment for the MySQL installation and secure file import settings on your machine.

4. Follow the README in each project folder in sequence. Portfolios 2 and 3 use the MySQL database created in Portfolio 1. Portfolio 4 exports selected records to JSON and imports them into MongoDB. Portfolio 5 describes the Atlas replica-set setup and the experiments that were actually run.

5. Use your own Atlas credentials and network access rules. Connection strings and secrets are not included in this repository. Do not commit credentials, private keys, or exported production data.

## Data and references

The project uses the [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), published on Kaggle under the CC BY-NC-SA 4.0 license. The dataset is not included here. Review the dataset license before redistributing derived data or screenshots containing data.

Technical references include the MySQL 8.0 Reference Manual, MongoDB Manual, MongoDB Atlas documentation, and the project-specific analyses in each folder. Results and screenshots are documented alongside the scripts that produced them.

## Credits and project presentation

This is a client project completed by **Osimen Odigie**. I am showcasing the database design and implementation as part of my portfolio under **ochecodes**, with the client's contribution attributed clearly. The repository retains the implementation and evidence, while its public presentation is written for a professional portfolio rather than as a course submission.

## License and use

The Olist dataset has its own CC BY-NC-SA 4.0 license. That license applies to the dataset, not automatically to the code and documentation in this repository. Contact the repository owner before reusing client-specific implementation materials.
