# 📘 Azure Data Engineer (ADF) Master Guide / Interview Bible

> **Audience:** Beginner → Advanced → Senior  
> **Primary Focus:** Azure Data Factory (ADF)  
> **Secondary Focus:** Azure Storage & Azure Data Engineering Ecosystem  
> **Purpose:** Learning roadmap, hands-on reference, and interview preparation  

---

## 📌 How to Use This Repository

- **Beginners** → Follow chapters sequentially  
- **Working Professionals** → Focus on ADF Core & Advanced sections  
- **Interview Preparation** → Chapters marked ⭐⭐⭐⭐⭐  
- **Senior / Architect Roles** → Architecture, Performance, Security, CI/CD  

---

## 🧭 Learning Layers

| Level | Focus |
|------|------|
| 🟢 Foundation | Core data concepts & Azure ecosystem |
| 🔵 Professional | Azure Data Factory deep dive |
| 🔴 Advanced | Scale, optimization, enterprise architecture |

---

# 🟢 FOUNDATION LAYER (Beginner)

---

## 1️⃣ Data Engineering Fundamentals ⭐⭐⭐⭐⭐

### Topics
- What is Data Engineering
- OLTP vs OLAP
- ETL vs ELT
- Batch vs Streaming
- Structured vs Semi-Structured vs Unstructured Data
- Data Lake vs Data Warehouse vs Lakehouse
- Medallion Architecture (Bronze / Silver / Gold)

---

## 2️⃣ Azure Data Engineering Ecosystem ⭐⭐⭐⭐⭐

### Core Azure Services
- Azure Data Factory (ADF)
- Azure Data Lake Storage Gen2 (ADLS)
- Azure Synapse Analytics
- Azure Databricks
- Azure SQL Database
- Azure Event Hub
- Azure Stream Analytics
- Azure Functions
- Azure Logic Apps

### Interview Focus
- When to use ADF vs Databricks vs Synapse
- Cost vs performance trade-offs

---

## 3️⃣ Azure Storage (Critical for ADF) ⭐⭐⭐⭐⭐

### Storage Types
- Azure Blob Storage
- Azure Data Lake Storage Gen2
- Azure File Storage
- Azure Queue Storage
- Azure Table Storage

### ADLS Gen2 Deep Dive
- Containers & folder structure
- Hierarchical namespace
- RBAC vs ACLs
- Hot / Cool / Archive tiers
- File formats: Parquet, CSV, JSON, Avro
- Partitioning strategies

---

# 🔵 PROFESSIONAL LAYER (ADF CORE)

---

## 4️⃣ Azure Data Factory – Architecture ⭐⭐⭐⭐⭐

### Topics
- Control Plane vs Data Plane
- Metadata-driven orchestration
- Stateless execution model
- Integration Runtime (IR)
  - Azure IR
  - Self-Hosted IR
  - Azure-SSIS IR

### Interview Focus
- Data movement flow in ADF
- When to use Self-Hosted IR

---

## 5️⃣ ADF Core Components ⭐⭐⭐⭐⭐

### Building Blocks
- Linked Services
- Datasets
- Pipelines
- Activities
- Triggers

### Design Principles
- Reusability
- Loose coupling
- Environment isolation

---

## 6️⃣ ADF Activities (High Value) ⭐⭐⭐⭐⭐

### Must-Know Activities
- Copy Activity
- Lookup
- Get Metadata
- ForEach
- If Condition
- Switch
- Execute Pipeline
- Web Activity
- Stored Procedure Activity

### Advanced Topics
- Parallel copy
- Staging
- Retry & timeout
- Fault tolerance

---

## 7️⃣ Mapping Data Flows (ADF Transformations) ⭐⭐⭐⭐

### Topics
- Spark-based execution
- Debug vs Trigger execution
- Schema drift
- Transformations:
  - Source & Sink
  - Join
  - Derived Column
  - Aggregate
  - Conditional Split
  - Surrogate Key

### Interview Focus
- Data Flow vs Databricks

---

## 8️⃣ Expressions, Parameters & Dynamic Pipelines ⭐⭐⭐⭐⭐

### Topics
- Pipeline parameters
- Dataset parameters
- Linked service parameters
- System variables
- Dynamic expressions
- Passing parameters across pipelines

### Advanced
- Metadata-driven pipelines
- JSON configuration-driven orchestration

---

## 9️⃣ Triggers & Scheduling ⭐⭐⭐⭐

### Trigger Types
- Schedule Trigger
- Tumbling Window Trigger
- Event-based Trigger

### Advanced
- Backfill scenarios
- Late-arriving data
- Trigger dependencies

---

# 🔴 ADVANCED & SENIOR LAYER

---

## 🔟 Incremental Loading & CDC ⭐⭐⭐⭐⭐

### Techniques
- Watermark strategy
- LastModifiedDate approach
- Change Data Capture (CDC)
- Hash-based change detection
- Slowly Changing Dimensions (SCD Type 1 & Type 2)

---

## 1️⃣1️⃣ Source & Sink Patterns ⭐⭐⭐⭐⭐

### Common Sources
- ADLS / Blob Storage
- Azure SQL DB / SQL Server (On-Prem)
- REST APIs
- SAP
- SFTP

### Common Sinks
- Azure Synapse Analytics
- ADLS (Parquet / Delta)
- Azure SQL Database
- Azure Cosmos DB

### Advanced
- API pagination
- Compression strategies
- Partition pruning

---

## 1️⃣2️⃣ Performance Optimization ⭐⭐⭐⭐⭐

### Topics
- Copy Activity DIUs
- Parallelism & concurrency
- Partitioning strategies
- PolyBase & COPY INTO
- Pushdown optimization

---

## 1️⃣3️⃣ Error Handling, Monitoring & Logging ⭐⭐⭐⭐⭐

### Topics
- Try/Catch patterns using pipeline logic
- Failure paths
- Rerun strategies
- Azure Monitor
- Log Analytics
- Custom logging tables

---

## 1️⃣4️⃣ Security & Governance ⭐⭐⭐⭐⭐

### Security
- Managed Identity
- Azure Key Vault
- Role-Based Access Control (RBAC)
- Private Endpoints
- Encryption at rest & in transit

### Governance
- Microsoft Purview
- Data lineage
- Data classification

---

## 1️⃣5️⃣ CI/CD & DevOps for ADF ⭐⭐⭐⭐⭐

### Topics
- Git integration (Azure DevOps / GitHub)
- Collaboration branches
- ARM templates
- Release pipelines
- Environment promotion (Dev → Test → Prod)

---

## 1️⃣6️⃣ Cost Optimization ⭐⭐⭐⭐

### Topics
- Data Flow cluster sizing
- Pipeline concurrency limits
- Storage tier optimization
- Reserved capacity

---

## 1️⃣7️⃣ Integration with Other Azure Services ⭐⭐⭐⭐

### Integrations
- ADF + Databricks
- ADF + Synapse
- ADF + Azure Functions
- ADF + Event Grid
- ADF + Logic Apps

---

## 1️⃣8️⃣ Real-World Architectures ⭐⭐⭐⭐⭐

### Must-Know Scenarios
- On-Prem → Azure migration
- Lakehouse architecture
- Near real-time ingestion
- Metadata-driven ingestion framework
- Multi-source enterprise pipelines

---

## 1️⃣9️⃣ Interview Preparation (Final Chapter) ⭐⭐⭐⭐⭐

### Must Prepare
- One end-to-end ADF project
- One incremental load design
- One production failure & recovery story
- One performance optimization example
- One cost reduction case study

---

## 🎯 Final Advice

If you can confidently explain:
- ADF architecture & data flow
- Incremental loading strategies
- Performance tuning approaches
- Error handling & monitoring
- End-to-end Azure data architecture

👉 You are **interview-ready for Azure Data Engineer roles**.

---

## 📂 Suggested Repository Structure

```text
azure-data-engineer-bible/
│
├── fundamentals/
├── azure-storage/
├── adf-core/
├── adf-dataflows/
├── incremental-loading/
├── performance/
├── security-governance/
├── cicd/
├── real-world-projects/
└── interview-questions/
