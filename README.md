# Daily Weather Automation using n8n
## Overview

This project implements a daily weather automation workflow using n8n, OpenWeatherMap, Supabase, and Email notifications.

The workflow runs automatically every morning, fetches the latest weather data for a configurable city, applies alert rules (rain, heat, frost), stores the results in Supabase, and sends a formatted daily weather email to a recipient.

This demonstrates API integration, data transformation, alert logic, persistence, and workflow automation using n8n.

## Workflow Overview

The workflow follows the sequence below:

1. Cron Trigger – Runs the workflow daily at a fixed time

2. Set Node (Config) – Configures the target city

3. OpenWeatherMap API – Fetches current weather data

4. Code Node – Applies alert rules and builds a summary

5. Supabase Node – Stores the weather data

6. Email Node – Sends a daily weather email

```
Cron → Set (City Config) → Weather API → Code (Logic) → Supabase → Email
```

## Node-by-Node Explanation

### 1️⃣ Cron Node – Workflow Trigger
- Triggers the workflow daily at 7:30 AM
- Ensures the automation runs consistently without manual execution

Configuration:
  - Mode: Every day
  - Time: 7:30 AM (local time)

### 2️⃣ Set Node – City Configuration
- Used as a configuration node to define the city for which the weather report is generated
- Makes the workflow easily reusable and maintainable

Example
```
{
  "city": "San Francisco"
}

```
Changing the city in this node updates the entire workflow.

### 3️⃣ OpenWeatherMap API Node
- Fetches real-time weather data from OpenWeatherMap
- Uses metric units (°C)

Data retrieved
- City name
- Temperature
- Feels-like temperature
- Weather condition
- Humidity
- Wind speed

Configuration
- API Key: OpenWeatherMap API key
- Units: Metric
- City: Dynamically passed from Set Node

### 4️⃣ Code Node – Weather Logic & Alerts
- Applies logic and alert rules
- Generates a human-readable weather summary

Alert Rules
- Precipitation Alert: rain, snow, drizzle, mist, storm, thunder
- Heat Alert: feels-like temperature > 32°C
- Frost Alert: feels-like temperature < 0°C
- Otherwise: No alert

Outputs
- Formatted weather summary
- Alert message
- Execution date
- Structured data for storage and email

### 5️⃣ Supabase Node – Data Storage
- Stores daily weather results in a Supabase Postgres table named weather_logs

Stored fields
- run_at (timestamp)
- city
- temperature
- temperature_unit
- condition
- humidity
- wind_speed
- alert_type
- summary
- raw_response (JSON)

Configuration
- Supabase project URL
- Service role API key
- Table schema created beforehand

### 6️⃣ Email Node – Daily Notification
- Sends a daily weather email to the configured recipient

Email content
- Subject: Daily Weather for <City> – <Date>
- Body: Weather summary and alert message

Supports plain text or HTML emails.

Things to Configure
---

#### Node 1 – Cron Trigger
- Set the time when the workflow should run daily

#### Node 2 – Set Node
- Configure the city name

#### Node 3 – OpenWeatherMap API
- Add OpenWeatherMap API key
- Select metric or imperial units

#### Node 4 – Code Node
- Customize alert rules or summary formatting if needed

#### Node 5 – Supabase
- Create weather_logs table
- Configure Supabase project URL
- Use Service Role API key for inserts

#### Node 6 – Email Node
- Configure email credentials (SMTP / Gmail)
- Set sender and recipient email addresses

### How to Import & Run
1. Import the provided .json workflow file into n8n
2. Configure all required credentials
3. Update the city in the Set node
4. Run the workflow manually once to test
5. Enable the workflow to allow daily execution

### Conclusion

This workflow demonstrates an end-to-end automation solution using n8n, combining scheduling, API integration, data processing, persistence, and notification delivery in a clean and maintainable way.
