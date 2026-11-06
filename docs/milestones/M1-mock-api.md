# M1: Mock school API

**Goal:** a FastAPI app that pretends to be a school's SaaS system (think student information or advancement software), including the annoying behavior real vendor APIs have.

**You'll learn:** REST resource design, JSON, pagination, API keys, HTTP status codes, rate limiting.

## Build (in `api/`)

- **Fake data:** generate it once at startup with Faker, seeded (`Faker.seed(42)`) so it's identical every run.
    - 400 students (id, first_name, last_name, class_year, advisor_email, updated_at)
    - 30 courses (id, code, title, department, term, updated_at)
    - 2,000 enrollments (id, student_id, course_id, grade, updated_at)
    - 1,500 donations (id, donor_name, student_id or null, amount, gift_date, updated_at)
- **Endpoints:** `GET /students`, `/courses`, `/enrollments`, `/donations`.
- **Pagination:** `?page=1&page_size=100` returns `{"data": [...], "page": 1, "next_page": 2, "next_url": "/students?page=2&page_size=100"}`. On the last page both `next_page` and `next_url` are `null`.
- **Incremental filter:** `?updated_since=2026-11-01T00:00:00Z` returns only records updated after that time.
- **Auth:** require an `X-API-Key` header matching `SCHOOL_API_KEY` from `.env`. Missing or wrong key returns 401.
- **Rate limit:** more than 10 requests in 10 seconds from the same key returns 429 with a `Retry-After` header (in seconds).
- **Flakiness:** with `FLAKY_RATE=0.05`, about 5% of requests return 500 at random.
- **Admin helper:** `POST /admin/touch?resource=students&n=20` bumps `updated_at` to now on 20 random records. You'll use it to test incremental loads.

## Hints

- FastAPI's `Depends` is the clean way to check the API key on every route.
- For the rate limit, keep a `collections.deque` of request times per key and drop entries older than 10 seconds.
- FastAPI builds interactive docs for you at `/docs`. Use them to poke at your API.
- Run it with `uvicorn api.main:app --reload`.

## Tests (`tests/test_api.py`, using FastAPI's `TestClient`)

- No key returns 401
- Page 4 of students with page_size 100 has exactly 100 records and `next_page` is null
- `updated_since` filters correctly after a `touch`
- The 11th request in a burst returns 429 with `Retry-After`

## Done when

- [ ] `/docs` loads and every endpoint works there
- [ ] All four tests pass
- [ ] PR opened from `m1-mock-api` with a description of what you built and how to run it

**Interview line:** "I built a fake vendor API that misbehaves on purpose: paging, rate limits, random 500s. That forced my pipeline to handle what real SaaS APIs do."
