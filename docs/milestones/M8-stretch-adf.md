# M8 (stretch): The same pipeline in Azure Data Factory

**Goal:** build one resource's pipeline in Azure Data Factory, so you can compare low-code orchestration with your Python code.

**You'll learn:** ADF pipelines, linked services, datasets, the Copy activity, REST pagination rules, the Stored Procedure activity, triggers, run monitoring. These map directly to Data Factory in Microsoft Fabric.

## Build

- Create a Data Factory in `rg-campus-data-hub`.
- Linked services: a REST service pointing at your mock API (with the `X-API-Key` header) and your Azure SQL database.
- A pipeline for students:
    1. Copy activity: REST source to `stg.students`, with a pagination rule that follows `next_url` from the response body
    2. Stored Procedure activity: run `core.usp_merge_students`
- Trigger it manually a few times, then try a schedule trigger. Watch the runs in the Monitor tab.

## Cost note

ADF bills per activity run and data movement. Run it by hand a few times, then delete the factory.

## Done when

- [ ] Students load through ADF and merge correctly
- [ ] You can explain when you'd choose ADF or Fabric pipelines over a Python Function, and the reverse

**Interview line:** "I built the same load both ways. ADF is faster to set up and easier for others to monitor; Python gives me full control for messy APIs and tests."
