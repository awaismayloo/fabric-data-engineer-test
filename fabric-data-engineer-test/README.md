
# Microsoft Fabric Technical Test – Data Engineer

This repository contains the solution to a Microsoft Fabric-based data engineering technical test. It showcases a full Bronze → Silver → Gold pipeline using Lakehouse and Warehouse in a single workspace, with Dataflow Gen2 powering the Silver → Gold transfer.

---

## 🧱 Workspace Structure

**Workspace**: `mytest`

### 📦 Lakehouse: `test_Lakehouse`

#### 📁 Files (Bronze Layer)
| Path                  | Description                           |
|------------------------|---------------------------------------|
| `/Files/bronze/`       | Contains raw CSV files for ingestion  |
| students.csv           | Student details                       |
| courses.csv            | Course catalog                        |
| assignments.csv        | Assignment metadata                   |
| submissions.csv        | Student assignment submissions        |

#### 🧪 Silver Layer Tables (Lakehouse Tables)
| Table Name             | Description                           |
|------------------------|---------------------------------------|
| `silver_students`      | Cleaned and structured student data   |
| `silver_courses`       | Cleaned courses data                  |
| `silver_assignments`   | Cleaned assignments                   |
| `silver_submissions`   | Cleaned submissions data              |

---

### 🏛 Warehouse: `test_Warehouse` (Gold Layer)

Data is transferred from Lakehouse to Warehouse using **Dataflow Gen2**.

| Table Name                        | Description                                                                 |
|-----------------------------------|-----------------------------------------------------------------------------|
| `gold_assignment_101_defaulters`  | Students who did **not** submit Assignment_101                              |
| `gold_assignment_courses`         | Assignments joined with their course context                                |
| `gold_overdue_assignments`        | Number of **overdue** assignments per student                               |
| `gold_students_submissions`       | Complete submission details joined with student info                        |
| `gold_submissions_ratio`          | **Submission rate per course**, expressed as a percentage                   |

---

## 📓 Notebooks

| Notebook Name              | Purpose                                                                  |
|----------------------------|--------------------------------------------------------------------------|
| `Bronze_Silver_Notebook`   | Reads raw CSVs from `/Files/bronze/` and loads structured Silver tables |
| `Silver_Gold_Notebook`     | Creates Gold-level analytical views from Silver tables                  |

---

## 🔄 Data Movement

**Lakehouse → Warehouse** transfer is handled using:
- ✅ **Dataflow Gen2**
- Reads from Silver tables in `test_Lakehouse`
- Loads structured Gold tables into `test_Warehouse`

---

## 📊 Analytical Outputs

### ✔ Submission Rate Per Course (`gold_submissions_ratio`)
```sql
(total_submissions) / (total_assignments × total_students) × 100
```

### ✔ Other Metrics
- **Defaulters for Assignment_101** → `gold_assignment_101_defaulters`
- **Overdue Assignments** → `gold_overdue_assignments`
- **Submission Analytics** → `gold_students_submissions`
- **Assignments + Course Join** → `gold_assignment_courses`

---

## 🚀 Execution Steps

1. Open Microsoft Fabric Workspace: `mytest`
2. Run notebooks:
   - `Bronze_Silver_Notebook`
   - `Silver_Gold_Notebook`
3. Use **Dataflow Gen2** to transfer Silver → Gold
4. Query final tables in `test_Warehouse`

---

## 📈 Power BI Integration

- Connect Power BI to `test_Warehouse` using the SQL endpoint
- Build dashboards directly on top of Gold tables (especially `gold_students_submissions` and `gold_submissions_ratio`)

---

## 🔐 Optional: Security & Governance

- Column-level access with `GRANT SELECT (column_name)` on Warehouse tables
- Dynamic Data Masking for PII fields (`email_address`, `full_name`)
- Example:
```sql
GRANT SELECT ON OBJECT::dbo.gold_students_submissions (full_name)
TO [analyst@email.com];
```

---

## 📂 Repository Structure

```text
/
├── data/
│   ├── students.csv
│   ├── courses.csv
│   ├── assignments.csv
│   └── submissions.csv
├── notebooks/
│   ├── Bronze_Silver_Notebook.ipynb
│   └── Silver_Gold_Notebook.ipynb
├── dataflow/
│   └── dataflow.png 
├── pipeline/
│   └── pipeline.png 
├── README.md
```

---

## 🙋 Author

Prepared by: **[Muhammad Awais Qasim]**  
Email: `awaisqasim888@gmail.com`  
