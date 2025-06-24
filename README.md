# azure-billing-optimization
Managing Billing Records in Azure Serverless Architecture
Move billing records older than 3 months from Azure Cosmos DB to Azure Blob Storage (Hot/Cool tier), and implement a data access layer that automatically fetches from Blob Storage when Cosmos DB doesn’t contain the requested record.

1. Data Tiering with Azure Functions
Use a Timer-triggered Azure Function (daily/weekly) to move records older than 3 months to Blob Storage.

Store archived records as individual JSON files in a hierarchical folder structure
 - container/billing/yyyy/mm/dd/recordId.json
   
2. Read-Through Fallback Logic
Update the API logic:
-First try to read the billing record from Cosmos DB.
-If NotFound and record is older than 3 months, fallback to Blob Storage

3. Blob Storage Tiering
- Use Hot tier for recent archived records (up to 6 months old).
- Transition to Cool/Archive tier using Lifecycle Management Rules.

  Deployment Notes
- Use Bicep/ARM templates or Terraform to set up storage lifecycle rules.
- Archive Function can be scheduled with Durable Functions if large batch processing is needed.
