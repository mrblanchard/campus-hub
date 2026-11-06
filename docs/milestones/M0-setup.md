# M0: Setup

**Goal:** a GitHub repo, a working Python environment, and an Azure account that can't surprise you with a bill.

**You'll learn:** the Git habits a team expects, virtual environments, Azure resource groups and budgets.

## Build

1. Install on Windows: Python 3.12 (python.org), Git, VS Code (with the Python and SQL Server "mssql" extensions), Azure CLI, Azure Functions Core Tools v4, and Microsoft ODBC Driver 18 for SQL Server.
2. In this folder: `git init`, make a first commit, create a GitHub repo named `campus-data-hub` (private is fine), and push.
3. Create a virtual environment: `python -m venv .venv`, then activate it (`.venv\Scripts\activate` in PowerShell).
4. Create `requirements.txt` with: fastapi, uvicorn, faker, httpx, python-dotenv, pytest, ruff. Install it.
5. Copy `.env.example` to `.env`.
6. Sign up for a free Azure account yourself. Then, before anything else:
    - Cost Management: create a budget of $5 with an email alert.
    - Create a resource group `rg-campus-data-hub` in East US.
7. Create a branch for the next milestone: `git checkout -b m1-mock-api`.

## Hints

- If PowerShell blocks the activate script, look up `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`.
- `git status` before every commit. Read what you're about to commit.

## Done when

- [ ] First commit is on GitHub
- [ ] `python -m pytest` runs (zero tests is fine)
- [ ] `.env` does not show up in `git status`
- [ ] The $5 budget alert exists

**Interview line:** "Before I created anything in Azure, I set a budget alert and kept everything in one resource group, so it was easy to track and tear down."
