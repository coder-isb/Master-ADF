## 1️⃣4️⃣ Security & Governance – Azure Data Engineering

Security and governance are **non-negotiable requirements** in enterprise-grade pipelines.  
This chapter covers **core authentication mechanisms, access control, encryption, and data governance practices** for ADF and the broader Azure ecosystem.

---

## 1. Security in Azure Data Engineering

### 1.1 Managed Identity
- Managed Identity (MI) is an **Azure AD–managed service identity** used to authenticate services without secrets.  
- Supports **ADF → ADLS, Synapse, Key Vault, Databricks** access.  
- Eliminates **manual credential management**, reducing security risks.  

**Scenario Example:**  
- ADF pipeline reads/writes to ADLS Gen2 → configure **Managed Identity on Linked Service** → assign **RBAC Reader/Writer** role on ADLS container.  

**Interview Question**  

**Q1: When would you use Managed Identity vs Service Principal?**  

**Answer:**  
- **Managed Identity:** Ideal for **intra-Azure authentication**, fully managed, no credentials to rotate.  
- **Service Principal:** Required for **multi-tenant, cross-subscription access**, automation outside Azure, or when fine-grained permissions are needed.  

---

### 1.2 Azure Key Vault
- Centralized **secret management** for passwords, keys, certificates.  
- ADF Linked Services can **reference secrets from Key Vault** instead of hardcoding credentials.  
- Supports **dynamic retrieval at runtime**, enabling **environment isolation** (Dev/QA/Prod).  

**Interview Question**  

**Q2: How do you integrate ADF with Key Vault securely?**  

**Answer:**  
- Create **Key Vault** and store credentials (storage keys, DB passwords)  
- Configure **Linked Service to reference Key Vault** via Managed Identity or SP  
- Ensure **access policies** restrict permissions to only required pipelines  

---

### 1.3 Role-Based Access Control (RBAC)
- **Azure RBAC** defines who can **create, edit, or trigger pipelines**.  
- Assign roles like: **Contributor, Data Factory Operator, Reader**  
- Works with **Resource Group, Data Factory, or storage account scopes**  

**Scenario:**  
- Data engineers: **Contributor**  
- Analytics team: **Reader**  
- Automation scripts: **Managed Identity with Reader/Writer on storage**  

**Interview Question**  

**Q3: How do RBAC and ACLs differ in ADLS?**  

**Answer:**  
- **RBAC:** Azure-wide access to resources (factory, storage account)  
- **ACLs:** Fine-grained, folder/file-level permissions on **ADLS Gen2**  
- Use RBAC for admin-level access and ACLs for **data-level control**  

---

### 1.4 Private Endpoints
- Connect services over **private Azure network**  
- Avoids exposure to public internet, reduces attack surface  
- Can be used for **ADF → ADLS / Synapse / Key Vault**  

**Scenario:**  
- Configure **private endpoint for ADLS**  
- Pipeline traffic remains within Azure backbone, improving **security and compliance**  

---

### 1.5 Encryption
- **At Rest:** All ADLS/Synapse storage encrypted via **Azure Storage Service Encryption (SSE)**  
- **In Transit:** TLS encryption for data movement  
- **Optional BYOK (Bring Your Own Key)** for additional compliance  

**Interview Question**  

**Q4: Explain encryption strategies in ADF pipelines.**  

**Answer:**  
- Data in **source and sink** can be encrypted at rest  
- During transfer, **TLS/HTTPS** ensures encryption in transit  
- Optional customer-managed keys (CMK) in Key Vault provide **organization-controlled encryption**

---

## 2. Governance in Azure Data Engineering

### 2.1 Microsoft Purview
- Provides **data catalog, discovery, lineage, classification**  
- Enables **tracking sensitive data** across pipelines, lakes, and warehouses  

**Scenario Example:**  
- Pipeline loads PII from SQL → ADLS → Synapse  
- Purview tracks lineage and identifies **PII columns**, enabling masking policies  

**Interview Question**  

**Q5: How does Purview help in governance of ADF pipelines?**  

**Answer:**  
- Purview **maps source-to-sink lineage**  
- Enables classification of sensitive data (PII, PCI)  
- Supports compliance audits and **data retention policies**  

---

### 2.2 Data Lineage
- Tracks **data flow from source to sink**  
- Essential for **impact analysis, debugging, and audit**  
- Can be combined with **custom logging** and Purview  

**Scenario:**  
- Customer data moves from SQL → ADLS → Synapse → Power BI  
- Lineage provides **visibility into transformations, timing, and responsible pipelines**

---

### 2.3 Data Classification
- Categorizes data based on **sensitivity and compliance requirements**  
- Examples: PII, PHI, financial data, confidential  
- Ensures **data masking, encryption, and access controls** are applied appropriately  

**Interview Question**  

**Q6: How do you enforce data classification in pipelines?**  

**Answer:**  
- Use Purview or tagging mechanisms  
- Apply **masking policies** in Synapse or Databricks  
- Restrict pipeline and storage access using **RBAC and ACLs**  

---

## 3. Enterprise Best Practices

1. **Use Managed Identity wherever possible** to reduce secrets management  
2. **Store all secrets in Key Vault**, reference dynamically in pipelines  
3. **Layer RBAC and ACLs** for both resource-level and data-level security  
4. **Enable private endpoints** for all sensitive storage and databases  
5. **Track data lineage and classification** with Purview  
6. **Audit pipelines and storage access** for compliance reporting  

---

## 4. Common Anti-Patterns

- Hardcoding credentials in Linked Services or pipelines  
- Using public endpoints for sensitive data movement  
- Ignoring ACLs on ADLS while relying only on RBAC  
- Not maintaining lineage or classification metadata  
- Lack of encryption for critical data at rest or in transit  

---

## 5. Key Takeaways

- **Security and governance** are critical for production-ready, enterprise pipelines  
- Managed Identity + Key Vault + RBAC + ACL + Private Endpoints = **strong security posture**  
- Microsoft Purview provides **lineage, classification, and compliance auditing**  
- Enterprise pipelines must combine **security, governance, and observability** to meet **compliance, audit, and SLA requirements**  

> Mastering this chapter ensures you can **design secure, compliant, and auditable ADF and Azure pipelines**, a frequent and high-value interview topic.
