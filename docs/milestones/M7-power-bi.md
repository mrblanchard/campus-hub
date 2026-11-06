# M7: Power BI report

**Goal:** a report a dean or development office would actually use, with each viewer seeing only what they should.

**You'll learn:** Power BI Desktop, connecting to Azure SQL, Power Query, relationships, DAX measures, row-level security.

## Build

- Install Power BI Desktop (free, Windows). Connect to your Azure SQL database and load the core tables and views.
- In Model view, create relationships: enrollments to students and courses, donations to students.
- **DAX measures:** Total Donations, Donations Year to Date (`TOTALYTD`), Active Students, Average Class Size.
- **Pages:**
    1. Enrollment: students per course and term, filterable by department
    2. Advancement: donations by month with a running total, top donors by class year
    3. Pipeline health: last run per resource, status, row counts, from `core.pipeline_runs`
- **Row-level security:** create a role "Advisor" that filters students where `advisor_email = USERPRINCIPALNAME()`. Test it with Modeling > View as.

## Hints

- Do cleanup in Power Query (or better, in your SQL views) and calculations in DAX.
- Publishing to the Power BI service usually needs a work or school account. Desktop alone is enough to learn this and take screenshots.

## Done when

- [ ] Three pages work and look clean
- [ ] Row-level security tested with View as
- [ ] Refresh shows new data after a pipeline run

**Interview line:** "I built the reporting layer in Power BI on SQL views, with row-level security so an advisor only sees their own students."
