## Automated News - Workflow 2

## What is the Automated News Aggregator?
The Automated News Aggregator is the backend scraping engine of Persona. While Workflow 1 handles user registration and chatbot preferences, this second workflow runs silently in the background every 3 hours. Its job is to fetch, clean, and store fresh news articles matching the exact interests of Persona's active user base.

---

## How It Works (Step-by-Step)

### 1. Get data from database
Every 3 hours, a cron timer wakes up the workflow. It immediately queries the central Supabase database to pull the raw profile data of all registered users.

### 2. The Data Normalizer (JavaScript)
Since users can type their interests freely in the chatbot (e.g., `"AI, tennis, football and cinema"`), the workflow passes this raw data through a JavaScript normalization node. 
* It splits mixed strings into individual topics using commas `,`, `" and "`, or `" et "`.
* It cleans up the text by removing trailing spaces, forcing lowercase, and stripping accents (e.g., `"Cinéma"` becomes `"cinema"`).
* It passes everything through a JavaScript `Set` to automatically destroy duplicate requests.

### 3. Target-Querying Google News
The workflow loops through each unique topic one by one and queries the official Google News RSS engine dynamically using the following structure:
`https://news.google.com/rss/search?q={{ $json.topic }}`

### 4. Smart Budgeting (The Context Limit)
To prevent database bloating and protect the AI's memory window for future newsletter generation, the incoming stream is capped. The workflow filters the RSS output to retain **only the top 5 most recent/relevant articles** per topic.

### 5. Storage in database
Finally, the selected articles are pushed into the `news` table using an insert.

---

## Technical Node Specifications

* **Trigger Type:** `Schedule Trigger` (Interval: 3 Hours, Trigger at Minute: 0)
* **Database Target:** Supabase (`news` table)
* **Loop Batch Size:** `1` (Processes topics in isolation to maintain stream stability)
* **Filtering Mechanism:** `Item Lists Node` configured to a maximum of 5 items.

---

## Database Schema (`news`)

This workflow continuously feeds a decoupled table designed to serve the downstream AI generation engine:

| Column | Type | Constraint | Role |
| :--- | :--- | :--- | :--- |
| `id` | int8 / uuid | Primary Key | Auto-generated row ID |
| `title` | text | Not Null | Headline of the news article |
| `link` | text | Not Null | The web link |
| `content` | text | - | Raw snippet text parsed from the RSS feed |
| `topic` | text | Not Null | Cleaned keyword bound to this specific article |
| `created_at` | timestamp | Default: `now()` | Used for the 24-hour automatic retention policy |

---

## System Optimization & Best Practices

- **Data Freshness:** Running every 3 hours ensures that whether a user schedules their Persona newsletter for 08:00 or 18:00, they always receive fresh, up-to-date summaries.
- **Storage Management:** A 24-hour retention policy ensures old, expired news is automatically purged, keeping the database footprint lightweight and efficient.
