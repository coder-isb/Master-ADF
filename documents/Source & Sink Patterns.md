## 1️⃣1️⃣ Source & Sink Patterns – Deep Conceptual Understanding & Enterprise Design

Understanding **sources and sinks** is critical for any data engineer.  
A pipeline is only as good as how efficiently it **reads from sources** and **writes to sinks**.  
This chapter covers **common source types, sink destinations, and enterprise-level performance and reliability strategies**.

---

## 1. Why Source & Sink Design Matters

Poorly designed sources/sinks can lead to:
- Slow pipelines
- High costs
- Skewed partitions
- Failed incremental loads
- SLA violations

Good source/sink design ensures:
- **Optimal throughput**
- **Scalable partitioning**
- **Reliable incremental loading**
- **Cost efficiency**

> Interviewers often test candidates on source-to-sink mapping and trade-offs in large-scale ETL systems.

---

## 2. Popular Sources & Their ADF Dataset Types

| Source Type | Examples | ADF Dataset Type | Notes / Considerations |
|------------|---------|-----------------|----------------------|
| Relational DB | SQL Server, MySQL, Oracle, Postgres | Azure SQL Database, ODBC, JDBC datasets | Supports incremental loading (watermark/CDC), queries, batch fetches |
| Cloud DB | Azure SQL DB, Snowflake, Synapse SQL Pools | Azure SQL Database, Synapse Dataset, Snowflake Dataset | High concurrency, supports partitioned reads |
| File Storage | ADLS Gen2, Blob Storage, SFTP, FTP | Binary, Delimited Text (CSV), Parquet, JSON, Avro datasets | Compression, partitioning, schema evolution, hierarchical namespace |
| NoSQL | Cosmos DB, MongoDB | Cosmos DB dataset, MongoDB dataset | Partition key design critical, throughput configuration |
| SaaS / API | REST, OData, Salesforce, Dynamics | REST dataset, HTTP dataset | Pagination, OAuth or API Key auth, rate limits, retries |
| Streaming / Event | Event Hub, Kafka | Event Hub dataset, Azure Event Hub dataset | Event processing, windowing, partition-based reads |
| Excel / JSON / Parquet | Files in Blob/ADLS | Excel dataset, JSON dataset, Parquet dataset | Schema inference, support for nested structures (JSON/Parquet) |

**Interview Question**

**Q1: How do you choose the correct dataset type in ADF for a given source?**

**Deep Explanation:**  
- Dataset type depends on **source type and format**  
- Binary or structured file datasets for raw files  
- Relational datasets for databases  
- NoSQL or JSON datasets for document-based sources  
- Dataset choice affects **schema mapping, partitioning, and copy performance**  

---

## 3. Common Sinks

| Sink Type | Typical Use Case | Key Considerations |
|----------|----------------|------------------|
| Azure Synapse Analytics | Analytical reporting, star schema | PolyBase vs Copy, distribution style, partitioning, concurrency |
| ADLS (Parquet / Delta) | Lakehouse, Delta pipelines | File format, compression, partition pruning, schema evolution |
| Azure SQL Database | Operational reporting | Upserts, batch vs streaming load, transaction limits |
| Azure Cosmos DB | Low-latency queries, JSON storage | Partition key design, throughput (RU/s), write consistency |

**Interview Question**

**Q2: When would you use ADLS as a sink over Synapse?**  

**Deep Explanation:**  
- ADLS: raw/historic storage, cheap, flexible schema, supports Data Lakehouse pattern  
- Synapse: structured analytics, fast SQL queries, high concurrency  
- Pipeline design often **writes to ADLS first (Bronze/Silver/Gold layers)**, then loads curated data into Synapse  

---

## 4. Advanced Source & Sink Considerations

### 4.1 API Pagination
- Many APIs return limited rows per request  
- Handle via Copy activity pagination or ForEach loops  
- Track “next page” tokens for incremental runs  

### 4.2 Compression Strategies
| Format | Pros | Cons |
|--------|------|-----|
| gzip | High compression ratio | CPU overhead |
| snappy | Fast compression/decompression | Slightly larger files |
| parquet/delta | Columnar compression | Requires compatible readers |

### 4.3 Partition Pruning
- Read/write only relevant partitions  
- Improves **parallelism and performance**  
- Critical for **Bronze/Silver/Gold pipelines**  

### 4.4 Connectivity & Security
- Managed Identity for ADLS/Synapse  
- Service Principal for automation/multi-tenant  
- Key Vault integration for secrets  
- OAuth for SaaS/REST APIs  

**Interview Question**

**Q3: When do you prefer Service Principal over Managed Identity in ADF?**

**Answer:**  
- Managed Identity: works well within same Azure tenant, no key management  
- Service Principal: multi-tenant access, automation outside ADF, fine-grained permission control  

---

## 5. Enterprise Patterns & Best Practices

1. **Bronze/Silver/Gold Layers** – Source → Bronze → Silver → Gold  
2. **Schema Evolution** – Sink format must support evolving schema (Delta, Parquet)  
3. **Retry and Fault Tolerance** – Sources prone to failure require retries with exponential backoff  
4. **Parallelism** – Use partitioned reads/writes to leverage IR parallelism  
5. **Audit & Logging** – Track row counts, file sizes, errors, start/end times  

---

## 6. Common Anti-Patterns (Interview Red Flags)

- Writing unpartitioned large files to ADLS  
- Ignoring API rate limits  
- Hardcoding connection strings or secrets  
- Using SQL-based sinks for raw large-volume data  
- Ignoring idempotency in incremental pipelines  

---

## 7. Key Takeaways (Interview Mental Models)

- Source choice depends on **throughput, latency, and security**  
- Sink choice depends on **analytics vs archival**  
- Partitioning and compression are **critical for performance and cost optimization**  
- Enterprise pipelines **layer raw → curated → analytics**  
- Connectivity and auth mechanisms (Managed Identity, SP, Key Vault, OAuth) are **core to secure designs**  

> Mastering source and sink patterns enables **efficient, secure, and scalable ADF pipelines**, a top differentiator in interviews.

---
