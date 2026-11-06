# M6: Azure Function on a timer

**Goal:** the pipeline runs every night in Azure without you touching it.

**You'll learn:** Azure Functions (Python v2 model), timer triggers, app settings, Key Vault, managed identity, Application Insights logs, deploying from GitHub Actions.

## Build

1. **Host the mock API** where Azure can reach it: an Azure App Service on the free F1 plan works. Remember to set its API key as an app setting.
2. **`function_app/function_app.py`:** a timer-triggered function that calls your `pipeline/run.py` logic.
    - Azure timer schedules have six fields, starting with seconds: `0 0 6 * * *` means 6:00:00 every day.
    - Times are UTC, so 6:00 UTC is about 1:00 a.m. in Vermont.
3. **Run it locally first** with `func start`. Local secrets go in `local.settings.json`, which is already in `.gitignore`.
4. **Create the Function App** in `rg-campus-data-hub` on a Consumption plan, Python 3.12.
5. **Secrets:** put the API key and SQL connection string in a Key Vault, and reference them from the Function App's settings with Key Vault references.
6. **Deploy with GitHub Actions** using the official Azure Functions deploy action.

## Stretch

- Give the Function App a managed identity and grant it access to Azure SQL, so no SQL password exists anywhere.
- Deploy with OpenID Connect (federated credentials) instead of a stored publish profile.

## Hints

- If the function can't reach SQL, check the SQL server firewall setting that allows Azure services.
- Application Insights (Logs) shows every run and every exception. Learn to read it here, because that's how you'd troubleshoot at work.

## Done when

- [ ] A run shows up in `core.pipeline_runs` the morning after deploying, with no action from you
- [ ] No secret exists in the repo or in plain text in app settings
- [ ] You can find that run's logs in Application Insights

**Interview line:** "The pipeline runs on a timer-triggered Azure Function, secrets come from Key Vault, and deploys go through GitHub Actions."
