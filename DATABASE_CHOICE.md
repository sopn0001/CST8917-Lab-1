# Database Choice: Text Analyzer Lab

## Your Choice

**Azure Cosmos DB for NoSQL** (serverless capacity mode)

- **Account:** `serverlesslab1cosmosdb`
- **Database:** `TextAnalyzerDb`
- **Container:** `AnalysisResults`

---

## Justification

Azure Cosmos DB is the best fit for this lab because the Text Analyzer produces structured **JSON documents** (analysis results, metadata, and original text) that map naturally to Cosmos DB's document model without requiring a predefined schema. It integrates cleanly with Azure Functions through the official **`azure-cosmos`** Python SDK, making it straightforward to insert and query records from serverless code. The **serverless** billing model aligns with the course's serverless architecture — you pay per request rather than for idle capacity. For a student project storing analysis history with simple queries (e.g., retrieve recent results by timestamp), Cosmos DB provides the right balance of simplicity, performance, and scalability.

---

## Alternatives Considered

| Database | Why It Was Rejected |
|----------|---------------------|
| **Azure Table Storage** | Lower cost, but designed for flat key-value entities. Nested JSON (analysis objects with multiple fields) would need to be serialized or flattened, adding unnecessary complexity. |
| **Azure SQL Database** | Strong for relational data with strict schemas and complex joins, but overkill for storing self-contained JSON documents. Requires table design and migrations that are not needed for this use case. |
| **Azure Blob Storage** | Can store JSON files cheaply, but is not a true database. Listing and querying historical analyses would require reading blobs one-by-one rather than using a simple query API. |

---

## Cost Considerations

Azure Cosmos DB offers pricing options suitable for development and learning:

- **Serverless mode:** Billed per request (Request Units consumed) with no minimum throughput when idle. Ideal for low-traffic lab workloads where the function is called occasionally.
- **Free tier:** New accounts can use the free tier (limited RU/s and storage per month), which is sufficient for this lab's text analysis storage needs.
- **No fixed server cost:** Unlike provisioned throughput, serverless does not charge for reserved capacity when the function is not running — consistent with pay-per-use serverless principles.

For this lab, expected costs are minimal: each Text Analyzer call performs one write (insert document), and each GetAnalysisHistory call performs one read query. At student usage levels, the workload stays within or near the free tier.
