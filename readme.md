# n8n Breach Monitor

n8n Breach Monitor is a self-hosted automation workflow that checks a list of emails against the Have I Been Pwned API daily at 3AM. It logs any new breaches to a PostgreSQL database and sends real-time alerts to a Discord channel. 

I chose the HIBP API because it's one of the most trusted and reliable sources for breach data. It offers accurate, up-to-date information.

## Setup Instructions

### 1. Import the Workflow into n8n

- Open your n8n instance
- Click on the menu (top right) → **Import Workflow**
- Upload the provided `.json` file (this repo includes it)

### 2. Database Schema (PostgreSQL)

Create a table named `email_breaches`:

```sql
CREATE TABLE email_breaches (
  id SERIAL PRIMARY KEY,
  email TEXT NOT NULL,
  breach_name TEXT NOT NULL,
  breach_date TIMESTAMP,
  date_found TIMESTAMP,
  domain TEXT,
  pwn_count TEXT,
  description TEXT,
  unique_id TEXT UNIQUE,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### 3. Get Your HIBP API Key

- Register at: https://haveibeenpwned.com/API/Key
- Add the API key in your HTTP Request node's headers under:  
  `hibp-api-key`

### 4. Setup a Discord Webhook

- Go to Discord → Server Settings → Integrations → Webhooks
- Create a new webhook and copy the URL
- Paste that URL into your Discord node in n8n

More info: https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks

### 5. Workflow Scheduling

- Workflow is set to run daily at 3:00 AM
- You can modify this in the `Schedule Trigger` node
