# Persona — AI-Powered Personalized Newsletter Assistant

> **Epitech Tek2 — AI Automation & No-Code/Low-Code Workflows (n8n, Supabase, LLM)**  
> An automated newsletter platform combining a conversational onboarding chatbot, scheduled RSS news aggregation, personalized AI content generation, and GDPR-compliant user management.

---

## Overview

**Persona** is split into modular automation workflows (exportable as n8n JSON blueprints) backed by **Supabase**:

1. **Chatbot & Account Management ([`Persona.md`](Persona.md))**
   - Conversational registration and login for both **regular users** and **advertisers** (`register_user.json`, `login_user.json`, `register_advertisers.json`, `login_advertisers.json`, `update_password.json`, `check_availibility.json`).
   - Users configure their **topics of interest**, **preferred send time**, and **newsletter style** (*concise & neutral*, *detailed & formal*, *casual & fun*, *bullet points*, or *short summaries*).
   - Mandatory **GDPR consent** before any account creation.

2. **Automated News Aggregator ([`News.md`](News.md))**
   - Runs every 3 hours via a scheduled trigger (`News.json`).
   - Pulls user topics from Supabase, normalizes & deduplicates them in JavaScript, queries **Google News RSS**, caps results to the top 5 most recent articles per topic to protect LLM context budgets, and stores them in Supabase.

3. **Personalized Newsletter Dispatch (`send_email.json`)**
   - Generates and delivers tailored newsletters matching each user's schedule and style preferences.

---

## Documentation & Compliance

- [User Manual & Chatbot Workflow (`Persona.md`)](Persona.md)
- [Automated News Aggregator Architecture (`News.md`)](News.md)
- [GDPR / RGPD Compliance (`RGPD.md`)](RGPD.md)
- [Security Audit (`Security_Audit.md`)](Security_Audit.md)

---

## Tech Stack

- **Workflow Orchestration**: n8n (JSON workflow definitions)
- **Database**: Supabase (PostgreSQL)
- **Data Ingestion**: Google News RSS + JavaScript normalization
- **AI**: LLM-driven chatbot & newsletter summarization

---

## Credits

Built as an Epitech Tek2 AI project by **Maëva Arroniz** ([@Maeva2630](https://github.com/Maeva2630)) and **Gustave Binet**.
