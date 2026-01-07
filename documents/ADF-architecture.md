# Chapter 4 – Azure Data Factory (ADF) Architecture

**Version:** 1.0  
**Role:** Azure Data Engineer / Cloud Data Platform Engineer  
**Focus:** ADF architecture, control vs data plane, metadata-driven orchestration, Integration Runtimes (Azure, Self-Hosted, SSIS), data movement, and enterprise interview prep  

---

## Table of Contents

1. [Introduction](#introduction)  
2. [Control Plane vs Data Plane](#control-plane-vs-data-plane)  
3. [Metadata-Driven Orchestration](#metadata-driven-orchestration)  
4. [Stateless Execution Model](#stateless-execution-model)  
5. [Integration Runtime (IR)](#integration-runtime-ir)  
   - [Azure Integration Runtime](#azure-integration-runtime)  
   - [Self-Hosted Integration Runtime](#self-hosted-integration-runtime)  
   - [Azure-SSIS Integration Runtime](#azure-ssis-integration-runtime)  
6. [Data Movement Flow in ADF](#data-movement-flow-in-adf)  
7. [When to Use Self-Hosted IR](#when-to-use-self-hosted-ir)  
8. [Scenario-Based Interview Questions with Explanations](#scenario-based-interview-questions-with-explanations)  
9. [Best Practices](#best-practices)  
10. [Key Takeaways](#key-takeaways)  

---

## Introduction

Azure Data Factory (ADF) is a **cloud-native, fully managed ETL/ELT orchestration service** designed for **data movement, transformation, and orchestration**.  

ADF separates its architecture into **control and data planes**, supports **metadata-driven pipelines**, **stateless execution**, and multiple **Integration Runtimes (IR)** to meet hybrid enterprise requirements.  

This chapter explains ADF architecture in-depth, **integration scenarios**, **security considerations**, and **interview-focused Q&A with detailed explanations**.  

---

## Control Plane vs Data Plane

ADF separates **control operations** (management) from **data operations** (movement/transformation):

| Plane           | Responsibilities                                      | Examples / Notes |
|-----------------|------------------------------------------------------|-----------------|
| **Control Plane** | Stores metadata, manages pipelines, triggers, monitoring | ADF portal, REST API, pipeline orchestration |
| **Data Plane**    | Moves and transforms data, executes activities      | Copy activity, Databricks transformation, Data Flow execution |

**Follow-Up Questions:**  

**Q1:** What happens if the control plane is down?  
**Answer:**  
- Pipelines cannot be **triggered, monitored, or edited**.  
- Already running activities in the **data plane continue execution**, because data plane is **decoupled**.  

**Q2:** How does control/data plane separation help enterprise pipelines?  
**Answer:**  
- Enables **scalability**: control plane handles orchestration metadata, data plane handles heavy data movement.  
- Supports **hybrid pipelines** with on-prem and cloud data sources.  

---

## Metadata-Driven Orchestration

ADF supports **metadata-driven pipelines**, where pipelines are **parameterized** and **data source metadata** drives execution dynamically:

- Use **control tables or JSON configs** to store file paths, table names, connection info  
- Pipelines **read metadata** and process multiple datasets dynamically  

**Follow-Up Questions:**  

**Q1:** Why use metadata-driven orchestration?  
**Answer:**  
- Reduces **pipeline duplication**, improves **maintainability**, and **scales to hundreds of sources**.  
- Example: Copy all CSV files from multiple source folders into a Delta Lake automatically using metadata table.  

**Q2:** How does ADF handle dynamic datasets?  
**Answer:**  
- **Parameterized datasets and pipelines** allow **dynamic source/sink selection** at runtime.  
- Integration with **ForEach activity** enables iteration over metadata-driven lists.  

---

## Stateless Execution Model

ADF **activities are stateless**:

- Pipeline orchestration is **lightweight**, ADF does not store intermediate data  
- Data is written directly to **sink storage or transformation engines (Databricks, Synapse)**  
- Pipeline state is tracked via **control plane metadata**  

**Follow-Up Questions:**  

**Q1:** What are the advantages of stateless execution?  
**Answer:**  
- Improves **scalability and reliability**: no single node failure affects ongoing operations  
- Enables **parallel execution** and **dynamic scaling**  

**Q2:** How does ADF handle failures in stateless activities?  
**Answer:**  
- Retry policies per activity  
- Monitoring and alerts in control plane  
- **Checkpointing** can be implemented via metadata tables  

---

## Integration Runtime (IR)

IR is the **compute infrastructure** that executes ADF activities. Three main types:

### Azure Integration Runtime

- Fully managed, cloud-hosted, auto-scalable  
- Handles **data movement across cloud data stores** and **compute for mapping data flows**  
- Supports **Hybrid connections** via managed VNet integration  

**Follow-Up Questions:**  

**Q1:** When should you use Azure IR?  
**Answer:**  
- Moving data between **cloud sources** (Azure Blob, ADLS, SQL Database, Synapse)  
- Performing **cloud transformations** (mapping data flow, Databricks notebook activities)  

---

### Self-Hosted Integration Runtime

- Installed on **on-prem servers or VMs**  
- Handles **data movement between on-prem and cloud**, supports **private network**  
- Can be **clustered for high availability**  

**Follow-Up Questions:**  

**Q1:** When is Self-Hosted IR required?  
**Answer:**  
- Accessing **on-prem databases** behind firewall  
- Moving data from **legacy systems** to cloud  
- Ensures **compliance and security** by keeping traffic within private network  

**Q2:** How do you ensure enterprise security with Self-Hosted IR?  
**Answer:**  
- Use **VPN or ExpressRoute** for private connectivity  
- Assign **managed identities or service principals**  
- Cluster IR for **fault tolerance and high availability**  

---

### Azure-SSIS Integration Runtime

- Runs **SSIS packages** natively in ADF  
- Supports **lift-and-shift of existing ETL workloads**  
- Scalable compute, can auto-pause when idle  

**Follow-Up Questions:**  

**Q1:** When would you choose Azure-SSIS IR over Databricks or mapping data flows?  
**Answer:**  
- Existing **SSIS ETL packages** must be migrated without rewriting  
- Integration with **on-premises SQL Server**  
- Supports **legacy transformations that are not supported natively in ADF**  

---

## Data Movement Flow in ADF

- **Source → Integration Runtime → Sink**  
- Steps:  
  1. **Control plane triggers** pipeline execution  
  2. **IR reads source data** (cloud or on-prem)  
  3. **Transformation executed** (Mapping Data Flow, Databricks, SQL)  
  4. **Sink storage updated** (ADLS, Synapse, SQL DB, Snowflake)  
  5. **Control plane logs execution**  

**Follow-Up Questions:**  

**Q1:** How does ADF move large datasets efficiently?  
**Answer:**  
- Uses **parallel copy**, **chunking**, and **auto-scaling compute**  
- **Self-Hosted IR** supports parallel threads and **high-throughput network optimization**  

**Q2:** Can ADF copy between **two on-prem sources** without cloud?  
**Answer:**  
- Yes, using **Self-Hosted IR** deployed on-prem, connected to both sources  
- Control plane orchestrates activity, data plane handles movement  

---

## When to Use Self-Hosted IR

- **On-prem sources** behind firewall  
- **Private network connectivity** required  
- **Custom compute requirements** (transformations requiring local resources)  
- Hybrid ETL pipelines connecting **legacy and cloud systems**  

**Q1:** Can you use Self-Hosted IR for cloud-only pipelines?  
**Answer:**  
- Not recommended; **Azure IR** is fully managed, auto-scalable, and cost-effective  

---

## Scenario-Based Interview Questions with Explanations

**Q1:** Design a pipeline to move data from **on-prem SQL Server to ADLS Gen2**, then transform via **Databricks**.  
**Answer:**  
- Self-Hosted IR reads on-prem SQL  
- Azure IR or Databricks handles transformations  
- Bronze/Silver/Gold layers in ADLS Gen2  
- Parameterize pipelines using **metadata-driven approach**  

**Q2:** How does ADF handle **concurrent pipeline executions**?  
**Answer:**  
- Control plane schedules execution  
- Data plane (IR) runs **parallel activities**  
- Concurrency settings per pipeline, activity, or IR  

**Q3:** How would you secure pipelines accessing sensitive on-prem data?  
**Answer:**  
- Self-Hosted IR in **private network**  
- Managed Identity / Service Principal for ADF  
- RBAC + ACLs on ADLS sinks  

**Q4:** Explain enterprise pattern for **metadata-driven orchestration**.  
**Answer:**  
- Use **control table** for source/sink and transformations  
- **ForEach activity** loops over metadata  
- Parameterized pipelines handle **dynamic datasets**  
- Enables **scalability, auditability, and maintainability**  

---

## Best Practices

- Use **Azure IR** for cloud-only pipelines; **Self-Hosted IR** for hybrid/on-prem  
- Implement **metadata-driven pipelines** for scalability  
- Parameterize datasets, linked services, and pipeline parameters  
- Monitor pipelines via **ADF monitoring & Azure Monitor**  
- Implement **retry policies, concurrency control, and logging**  
- Secure pipelines using **Managed Identities, RBAC, VNet/private endpoints**  

---

## Key Takeaways

- ADF architecture separates **control plane** (orchestration) from **data plane** (movement/transformation)  
- **Integration Runtimes (Azure, Self-Hosted, SSIS)** handle different compute/data access requirements  
- **Metadata-driven pipelines** enable dynamic, scalable orchestration  
- **Self-Hosted IR** is required for **on-prem, private, or legacy source pipelines**  
- Enterprise pipelines require **security, monitoring, and governance** for reliability and compliance  

