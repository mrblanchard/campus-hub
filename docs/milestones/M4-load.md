# M4: Load and log

**Goal:** one command runs the whole pipeline: extract, load staging, merge, record what happened.

**You'll learn:** connecting Python to SQL Server (pyodbc), fast bulk inserts, transactions, idempotency, run logging, failure recovery.

## Build

- Add `pyodbc` to `requirements.txt`. Read the connection string from `.env`.
- **`pipeline/load.py`:** insert extracted records into the `stg` table with `cursor.fast_executemany = True`, then call the merge procedure.
- **`core.pipeline_runs` table:** run_id, resource, started_at, finished_at, rows_extracted, rows_inserted, rows_updated, status (`running`, `succeeded`, `failed`), error_message.
- **`core.watermarks` table:** resource, last_success_at. Move the watermark here from `state.json`.
- **`pipeline/run.py`:** for each resource: log the run as running, extract since the watermark, load, merge, then mark succeeded and advance the watermark. On any error: mark failed, save the message, and do not move the watermark.
- Load order matters: students and courses before enrollments, because of the foreign keys.

## Hints

- Wrap the load and merge for one resource in a transaction so a failure doesn't leave half the data behind.
- Never log the connection string or API key.

## Done when

- [ ] First run loads everything
- [ ] Second run, with no changes, extracts about 0 rows
- [ ] After `touch` on 20 students, a run updates exactly those 20
- [ ] Stop the API mid-run: the run is marked failed, the watermark doesn't move, and the next run completes cleanly
- [ ] `core.pipeline_runs` shows every run

**Interview line:** "The watermark only advances after a successful run, and every run is logged with row counts and errors, so failures are visible and recovery is just running it again."
