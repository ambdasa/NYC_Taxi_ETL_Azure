# NYC_Taxi_ETL_Azure

# 🚖 NYC Taxi ETL Pipeline with Azure Data Factory

This project implements a full ETL workflow for the NYC Taxi Zone dataset using **Azure Blob Storage** and **Azure Data Factory** (ADF). The pipeline ingests a CSV file, performs transformation using Data Flows, and saves the output in a clean format for downstream use.

## 🧱 Architecture

Local or Web CSV
↓
Azure Blob Storage (raw-data container)
↓
Azure Data Factory Pipeline
↓
Data Flow: Rename columns
↓
Azure Blob Storage (transformed-data container)

## 🔧 Azure Components Used

| Component              | Purpose                                            |
|------------------------|----------------------------------------------------|
| **Storage Account**    | Stores CSVs and serves as a data lake              |
| **Blob Containers**    | `raw-data` (input) and `transformed-data` (output) |
| **Azure Data Factory** | Pipeline and Data Flow creation                    |
| **Integration Runtime**| Executes the transformation logic                  |

---

## 🧰 Setup Instructions

### 1. Create Resources in Azure

- Go to [Azure Portal](https://portal.azure.com)
- Create:
  - Storage Account (e.g., `ambinyctaxistorage`)
  - Blob containers:
    - `raw-data`
    - `transformed-data`
  - Azure Data Factory (e.g., `ambi-nycTaxiADF`)

---

### 2. Upload Raw Data to Blob Storage

Option 1: Use the notebook

```python
# nyc_upload_web_to_blob.ipynb
# Uploads 'taxi_zone_lookup.csv' to 'raw-data' container
````

Option 2: Use Azure CLI

```bash
az storage blob upload \
  --account-name ambinyctaxistorage \
  --container-name raw-data \
  --name taxi_zone_lookup.csv \
  --file "A:\DE\NYC-Taxi-ETL-Azure\taxi_zone_lookup.csv"
```

---

### 3. Build ADF Pipeline

In Azure Data Factory Studio:

* **Dataset (Source)**:

  * Type: Delimited Text
  * Linked to `raw-data/taxi_zone_lookup.csv`
* **Data Flow**:

  * Add source (TaxiSource)
  * Add *Derived Column* transformation:

    * Rename `Zone` → `Zone_name`, `LocationID` → `location_id`, etc.
  * Add sink to `transformed-data` container
* **Dataset (Sink)**:

  * Name: `ds_taxi_transformed`
  * File: `taxi_zone_lookup_transformed.csv`

---

## ▶️ Run the Pipeline

1. Go to the **pipeline canvas**
2. Enable **Data Flow Debug**
3. Click **Debug** or **Add Trigger** → **Trigger Now**
4. Monitor status under the **Monitor** tab

---

## ✅ Outputs

After a successful run, check your storage account:

* 📁 `raw-data/taxi_zone_lookup.csv` — Original file
* 📁 `transformed-data/taxi_zone_lookup_transformed.csv` — Cleaned and renamed columns

---

## 🔍 Monitor and Logs

* Go to **Monitor** in ADF Studio
* Check activity status, duration, and logs
* Export run details as CSV (optional)

Happy Coding!

---

```

Let me know if you want a version with screenshots, badges, or GitHub Actions integration too!
```
