# M3: Azure SQL and T-SQL

**Goal:** a cloud database with staging and core tables, merge procedures, and report views, all in rerunnable T-SQL scripts.

**You'll learn:** Azure SQL Database, T-SQL table design, schemas, `MERGE`, stored procedures, views, window functions, firewall and sign-in basics.

## Build

1. **Create the database** in `rg-campus-data-hub` using the Azure SQL Database free offer (look for "Apply offer" when you create it). Choose Microsoft Entra authentication if you can. Add your current IP address to the server firewall.
2. **`sql/001_schema.sql`:** create schemas `stg` and `core`. Make the four tables in both:
    - `stg` tables are plain landing zones: no keys, everything the API sends, plus `loaded_at`.
    - `core` tables have primary keys, foreign keys (enrollments to students and courses), proper types (`DECIMAL(10,2)` for money, `DATETIME2` for times), and `created_at` and `updated_at`.
3. **`sql/002_merge_procs.sql`:** one procedure per table, like `core.usp_merge_students`, that `MERGE`s from `stg` into `core`, then clears the staging table. Use `OUTPUT $action` to return how many rows were inserted and updated.
4. **`sql/003_reports.sql`:** views for reporting:
    - `core.v_course_enrollment`: students per course and term
    - `core.v_donations_monthly`: total per month, plus a running total with `SUM(...) OVER (ORDER BY month)`
    - `core.v_top_donors_by_class`: rank donors within each class year with `ROW_NUMBER() OVER (PARTITION BY ...)`

## Hints

- Connect with the VS Code mssql extension.
- Use `CREATE OR ALTER` for procedures and views, and `IF NOT EXISTS` checks for tables, so every script can run twice safely.
- Load a handful of rows into `stg` by hand first and test each procedure before wiring Python to it.
- Only update a core row when something actually changed, or every run will look like it updated everything.

## Done when

- [ ] All three scripts run twice in a row with no errors
- [ ] Running a merge procedure twice on the same staging data reports 0 changes the second time
- [ ] Each view returns sensible numbers

**Interview line:** "I land raw data in staging, then a MERGE procedure upserts it into the core tables, so loads are idempotent and the reporting views always read clean data."
