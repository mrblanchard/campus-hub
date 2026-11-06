# M5: CI and code review

**Goal:** every push is linted and tested automatically, and nothing reaches `main` without a pull request and a passing build.

**You'll learn:** GitHub Actions, branch protection, writing a good PR description, giving and responding to code review.

## Build

- **`.github/workflows/ci.yml`:** run on `push` and `pull_request`. Steps: check out the code, set up Python 3.12, install `requirements.txt`, run `ruff check .`, run `pytest`.
- **Branch protection on `main`** (GitHub repo settings): require a pull request before merging and require the CI check to pass.
- **From now on, every milestone is a PR** with a description: what changed, why, how to test it, anything risky.
- **Practice review:** ask Claude to review the PR like a teammate. Reply to every comment, either with a fix or a reason you're keeping it.

## Hints

- Tests that call the real API or database don't belong in CI yet. Mark them (for example with a pytest marker) and skip them in the workflow.
- Read a failed Actions log from the top of the failing step. The first error is usually the real one.

## Done when

- [ ] A PR with a deliberately failing test shows a red check and can't be merged
- [ ] Fixing the test turns it green, and the PR merges
- [ ] At least one PR has review comments with your replies

**Interview line:** "Every change went through a pull request with automated lint and tests, and main was protected, so nothing merged without a green build and a review."
