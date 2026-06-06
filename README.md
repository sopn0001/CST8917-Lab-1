# CST8917 Lab 1 — Text Analyzer Azure Function

Serverless HTTP functions that analyze text and persist results to Azure Cosmos DB.

## Endpoints

| Function | Method | URL (local) |
|----------|--------|-------------|
| TextAnalyzer | GET, POST | `http://localhost:7071/api/TextAnalyzer` |
| GetAnalysisHistory | GET | `http://localhost:7071/api/GetAnalysisHistory` |

**Azure (deployed):**

- `https://cst8917-func-lab1-bjhvb2bffwexchg7.canadacentral-01.azurewebsites.net/api/TextAnalyzer`
- `https://cst8917-func-lab1-bjhvb2bffwexchg7.canadacentral-01.azurewebsites.net/api/GetAnalysisHistory`

## Prerequisites

- Python 3.12+
- [Azure Functions Core Tools](https://learn.microsoft.com/azure/azure-functions/functions-run-local)
- Azurite (VS Code extension or `npm install -g azurite`)

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Create `local.settings.json` (not committed — see `.gitignore`):

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "DATABASE_CONNECTION_STRING": "<your-cosmos-connection-string>",
    "COSMOS_DATABASE_NAME": "TextAnalyzerDb",
    "COSMOS_CONTAINER_NAME": "AnalysisResults"
  }
}
```

> **Note:** Do not use `CONTAINER_NAME` — it is reserved by the Azure Functions host. Use `COSMOS_CONTAINER_NAME` instead.

## Run locally

1. Start Azurite: VS Code → `F1` → **Azurite: Start**
2. Start the function host:

```bash
source .venv/bin/activate
func start
```

## Test

```bash
curl "http://localhost:7071/api/TextAnalyzer?text=Hello%20world"

curl -X POST http://localhost:7071/api/TextAnalyzer \
  -H "Content-Type: application/json" \
  -d '{"text": "Serverless computing is amazing."}'

curl "http://localhost:7071/api/GetAnalysisHistory?limit=5"
```

## Deploy to Azure

```bash
func azure functionapp publish cst8917-func-lab1 --build remote
```

Add the same Cosmos DB environment variables in Azure Portal → Function App → **Environment variables**.

## Project files

- `function_app.py` — TextAnalyzer and GetAnalysisHistory functions
- `requirements.txt` — Python dependencies
- `DATABASE_CHOICE.md` — Database selection rationale
- `DEMO.md` — Demo video link
