## 🔟 Incremental Loading & CDC – Deep Conceptual Understanding & Enterprise Design

In modern data platforms, **full loads are expensive and often impractical**.  
Incremental loading ensures pipelines process **only new or changed data**, reducing cost, improving performance, and meeting SLAs.  

Understanding **Incremental Load strategies and CDC** is critical for interviews at top product companies and GCCs.

---

## 1. Why Incremental Loading Exists (First Principles)

Full refresh:
- Reads all data every time
- Consumes more compute, storage, and network
- Breaks for large datasets
- Can create downtime or inconsistent views

Incremental loading:
- Reads only **delta data**  
- Supports high-frequency updates  
- Reduces cost and runtime  

> Incremental loading is the foundation for **scalable, production-grade pipelines**.

---

### Interview Question

**Q1: Why not always use full loads?**  

**Deep Explanation:**  
Full loads are simple but inefficient.  
Large tables can take hours or days.  
Incremental loads optimize compute, storage, and network usage while maintaining correctness.

---

## 2. Incremental Loading Techniques

---

### 2.1 Watermark Strategy

**Concept:**  
- Track the **maximum value** of a timestamp or numeric column (watermark)  
- Use it to filter **new or updated rows** in the next run

**Implementation Flow:**  
1. Store last max watermark (e.g., `LastModifiedDate`) in control table or pipeline variable  
2. Query source: `WHERE LastModifiedDate > LastWatermark`  
3. Process and store data  
4. Update watermark

**Enterprise Notes:**  
- Ensure column is indexed for performance  
- Must handle late-arriving data carefully

**Interview Question**

**Q2: How do you handle late-arriving data with watermarks?**  

**Deep Explanation:**  
- Use **overlapping windows** or **lookback periods**  
- Example: query data modified in the last `X` days instead of strictly after last watermark  
- Ensure idempotency so duplicates are not written

---

### 2.2 LastModifiedDate Approach

**Concept:**  
- Similar to watermark but relies on the source system’s `ModifiedDate` column  
- Incremental fetch: `WHERE ModifiedDate > LastRunTime`

**Trade-offs:**  
- Depends on source system providing reliable last modified column  
- Cannot detect deleted rows  
- Late-arriving records may be missed without overlap

**Interview Question**

**Q3: What are pitfalls of using LastModifiedDate for incremental loads?**  

**Deep Explanation:**  
- Missing updates if source clock is inconsistent  
- Late-arriving data  
- Deleted records not captured (requires separate strategy)

---

### 2.3 Change Data Capture (CDC)

**Concept:**  
- CDC tracks **row-level changes** (insert, update, delete) in the source system  
- Most databases (SQL Server, Oracle, Postgres) support CDC natively

**ADF Integration:**  
- Read CDC tables via SQL queries or ADF connectors  
- Merge changes into target table using Upsert logic

**Enterprise Notes:**  
- Supports **exactly-once processing**  
- Works well for OLTP sources

**Interview Question**

**Q4: When should you use CDC over watermarks?**  

**Deep Explanation:**  
- CDC is ideal for **highly transactional systems**  
- Watermarks are simpler but may miss deletes or updates  
- CDC adds complexity (requires CDC-enabled source and proper retention handling)

---

### 2.4 Hash-Based Change Detection

**Concept:**  
- Compute a hash (MD5/SHA) of row attributes  
- Compare with previous hash to detect changes

**Use Cases:**  
- No reliable timestamp column  
- Detect changes in **any column**  
- SCD Type 2 scenarios

**Trade-offs:**  
- CPU intensive on large tables  
- Cannot detect deleted rows unless entire source scanned

**Interview Question**

**Q5: What are advantages and limitations of hash-based incremental loads?**  

**Deep Explanation:**  
Advantages:
- Detects changes without timestamps  
- Works for multiple columns  
Limitations:
- Expensive on large datasets  
- Needs storage for previous hash values  
- Does not natively detect deletes

---

### 2.5 Slowly Changing Dimensions (SCD)

SCDs capture historical changes for dimensional tables.

**Types in ADF:**  

| Type | Description | ADF Implementation Notes |
|------|-------------|-------------------------|
| Type 1 | Overwrite old values | Copy activity + Upsert logic |
| Type 2 | Preserve history | Surrogate keys + Effective start/end date columns |
| Type 3 | Partial history | Maintain limited previous value columns |

**Enterprise Notes:**  
- Type 2 is most common in analytics systems  
- Type 1 for correction-only scenarios  
- ADF Mapping Data Flow provides built-in SCD transformations

**Interview Question**

**Q6: How do you design a Type 2 SCD pipeline in ADF?**  

**Deep Explanation:**  
- Source → Lookup → Conditional Split → Update old row (set `EndDate`) → Insert new row  
- Use surrogate keys to decouple business keys  
- Ensure idempotency for retries and backfills

---

## 3. Enterprise Considerations

1. **Late-arriving data:** Always design pipelines to handle late updates using lookback windows or overlapping watermarks.  
2. **Idempotency:** Avoid duplicate inserts during retries. Use primary keys or merge logic.  
3. **Partitioning:** Process data in partitions to optimize performance.  
4. **Audit & Logging:** Store run metadata, row counts, watermarks, and errors for monitoring.  
5. **Hybrid approaches:** Use a combination of watermarks, CDC, and SCD for complex pipelines.

---

## 4. Interview Anti-Patterns

- Full loads without justification  
- Ignoring deletes in incremental pipelines  
- Hardcoding watermarks  
- Ignoring late-arriving data  
- Not using surrogate keys in SCD Type 2  

---

## 5. Key Takeaways (Interview Mental Models)

- Incremental loading is **mandatory for scalable pipelines**  
- Watermarks are simplest but require reliable timestamps  
- CDC is enterprise-grade for transactional sources  
- Hash-based detection is useful when timestamps are unreliable  
- SCDs ensure historical accuracy in dimensional tables  
- Always plan for **late-arriving data, idempotency, and partitioning**  

Mastering incremental load patterns demonstrates **production-readiness and deep ETL understanding**, which is frequently tested in interviews at GCCs, top product, and service-based companies.

---
