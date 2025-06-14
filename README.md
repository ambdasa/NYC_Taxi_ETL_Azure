# NYC Taxi ETL Project using Azure

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
 
![image](https://github.com/user-attachments/assets/6d8b2c6b-7826-4cd2-a8e7-acca5f2b13f9)

![image](https://github.com/user-attachments/assets/dba9ab51-66bd-4519-b57d-081013990560)

![image](https://github.com/user-attachments/assets/603da961-6447-4068-a714-63318f425c0c)

  

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

![image](https://github.com/user-attachments/assets/f4100395-103a-41bd-9e8a-755a1a07da47)

![image](https://github.com/user-attachments/assets/bfaf18b7-7599-4010-8515-71210df335aa)


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
