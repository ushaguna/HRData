HR ANALYTICS - AZURE DATA ENGINEERING PRACTICE DATASET
========================================================

All records are synthetic and intended only for learning. Do not treat them
as real employee or payroll information.

RECORD COUNTS
-------------
employees.csv:   182 rows
payroll.csv:     542 rows
benefits.csv:    variable, >100 rows
attendance.csv:  1,802 rows
departments.csv: 121 rows

FILES AND PURPOSE
-----------------
employees.csv   Employee master and slowly changing employee attributes
payroll.csv     Employee payroll transactions for Jan-Mar 2024
benefits.csv    Employee benefit enrollment records
attendance.csv  Daily attendance records for March 2024
departments.csv Department reference data

IMPORTANT JOIN KEYS
-------------------
employees.employee_id = payroll.employee_id
employees.employee_id = benefits.employee_id
employees.employee_id = attendance.employee_id
employees.department_id = departments.department_id

INTENTIONAL DATA-QUALITY SCENARIOS
----------------------------------
The files intentionally include a mixture of:
- Exact duplicate rows
- Duplicate business keys with conflicting attributes
- Missing/null values in keys and non-key columns
- Orphan foreign keys (IDs that do not exist in the parent file)
- Invalid date strings and inconsistent date formats
- Invalid numeric values, negative amounts/hours, and out-of-range hours
- Invalid categorical values and inconsistent casing/whitespace
- Invalid email formats
- Inconsistent currency labels
- Missing primary-key values
- Repeated employee-benefit combinations
- Conflicting duplicate attendance records
- Future-dated records

These are deliberate test cases. Preserve the raw files in Bronze; do not
manually fix the source files. Profile and clean the data in Silver, and
document rejected/quarantined rows.

SUGGESTED PIPELINE EXERCISES
----------------------------
1. Ingest each CSV from a local source into ADLS Gen2 Bronze using ADF.
2. Add ingestion metadata such as source_file, ingestion_timestamp, and batch_id.
3. Profile null counts, duplicate keys, invalid dates and invalid values.
4. Apply schema enforcement and type casting in Databricks/PySpark.
5. Deduplicate using business keys and a deterministic latest-update rule.
6. Separate invalid/orphan records into quarantine tables.
7. Join employee, payroll, benefits, attendance and department data.
8. Implement SCD Type 1 and Type 2 for employee attributes.
9. Create Gold aggregates: monthly payroll by department, headcount by
   department/location, benefits enrollment counts, and attendance rates.
10. Add incremental ingestion and rerun/idempotency tests.

NOTE
----
Counts above are approximate only where marked variable; run a row count on
each file after extraction. Empty fields represent nulls. Some columns contain
intentionally malformed text values, so initially ingest raw data as strings
or use permissive parsing before applying validated types.
