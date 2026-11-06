# M2: Extractor

**Goal:** Python code that reliably pulls every record (or only what changed) from the API, no matter how badly the API behaves.

**You'll learn:** consuming REST APIs, timeouts, retries with exponential backoff, honoring `Retry-After`, incremental watermarks, unit testing with mocks.

## Build (in `pipeline/extract.py`)

- A function like `fetch_all(resource, updated_since=None)` that follows `next_page` until it's null and yields records.
- An `httpx.Client` with a base URL, the API key header, and a timeout.
- Retry rules:
    - 429: wait the number of seconds in `Retry-After`, then retry.
    - 500 to 599 or a timeout: wait 1, 2, 4, 8 seconds, then give up after 5 tries and raise a clear error.
    - 401: don't retry. Raise immediately, because retrying won't fix a bad key.
- Log each page fetched (resource, page, record count) with Python's `logging` module.
- For now, store the watermark (the time of the last successful pull per resource) in `state.json`. In M4 it moves into SQL.

## Hints

- Write the retry logic as its own small function so you can test it alone.
- Pass the `sleep` function in as a parameter. Tests can then pass a fake sleep that records calls instead of waiting.
- `httpx.MockTransport` lets tests return any response you want without running the API.
- Record the watermark as the time the pull *started*, not when it finished, so records changed during the pull aren't missed next time.

## Tests (`tests/test_extract.py`)

- Follows pages until `next_page` is null and returns every record
- A 429 followed by a 200 retries once and waits the `Retry-After` value
- Five 500s in a row raises your error
- A 401 raises immediately with no retry

## Done when

- [ ] A full pull of students returns 400 records
- [ ] With `FLAKY_RATE=0.2` a full pull of all four resources still completes
- [ ] After `POST /admin/touch?resource=students&n=20`, an incremental pull returns about 20 students
- [ ] All tests pass and the PR is open

**Interview line:** "My extractor retries with backoff, respects the vendor's rate limits, never retries an auth failure, and only pulls what changed since the last good run."
