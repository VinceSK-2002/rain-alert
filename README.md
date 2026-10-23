# Rain alert for Vienna

Every morning at 06:00 (Vienna time) a GitHub Actions workflow runs `rain-alert.R`, which

1. downloads today's hourly forecast for Vienna from the [Open-Meteo API](https://open-meteo.com/) (GET request, no API key),
2. finds the hours between 07:00 and 22:00 with a rain probability of at least 50 % or at least 0.3 mm of rain, and merges them into time windows,
3. if there is rain, sends an email with the rain times, a chart and the reminder to take an umbrella (R package `blastula`, Gmail),
4. saves the forecast to `data/rain-forecasts.csv` and a line to `data/rain-alert-log.txt`, and commits both back to this repo.

**Success check:** the script stops with an error if the API does not answer with status 200 or the forecast is empty. The error is written to the log, a "Rain check failed" email is sent, and the run shows up red in the Actions tab.

**Scheduling:** `.github/workflows/rain-alert.yml`. GitHub cron runs in UTC, so the job starts at 04:00 and 05:00 UTC and only continues when it is 06:xx in Vienna (works in summer and winter time).

**Secrets** (Settings > Secrets and variables > Actions): `SMTP_USER`, `SMTP_PASSWORD` (Gmail app password), `RAIN_RECIPIENTS` (comma-separated).
