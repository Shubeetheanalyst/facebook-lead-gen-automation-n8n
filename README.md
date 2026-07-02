# 🎯 Hybrid Facebook Lead Generation & Outreach System

> An autonomous n8n workflow that discovers, qualifies, and drafts personalized outreach for high-intent Facebook leads — twice a day, with zero manual searching.

Built by **Muhammad Sohaib** — (https://shubeetheanalyst.github.io/)
🔗 Portfolio: [shubeetheanalyst.github.io](https://shubeetheanalyst.github.io/)
⭐ 5-Star Rated on Fiverr: [fiverr.com/s/rEWdoK0](https://www.fiverr.com/s/rEWdoK0)


## 📽️ Demo
```
<img width="1606" height="663" alt="FB_Lead_Gen" src="https://github.com/user-attachments/assets/7fc55251-34ed-4c61-8995-8ecb11533e2d" />

```

## 🧠 What This Solves

Freelancers and agencies lose hours every week manually scrolling Facebook groups, searching for people who need automation, AI, or workflow help. This system removes that entirely — it finds the leads, qualifies them, and drafts the outreach, so the only human step left is hitting "send."

## ⚙️ How It Works

```
Cron (every 12h)
   │
   ▼
Apify Facebook Search Scraper  ──►  6 targeted keywords
   │
   ▼
Filter: age < 7 days → spam removal → duplicate check
   │
   ▼
AI (Groq / Llama 3.3 70B) — Extract intent, industry, pain point, lead score (1-10)
   │
   ▼
IF score ≥ 6
   │                              │
   ▼                              ▼
AI generates reply         Log as Rejected
(comment + DM versions)
   │
   ▼
Append to Google Sheets (Status: Pending)
   │
   ▼
Human reviews → manually posts/sends
```

## 🔑 Key Features

- **Fully automated discovery** — scans Facebook every 12 hours across 6 high-intent keywords (`n8n`, `AI automation`, `automation expert needed`, `looking for automation developer`, `workflow automation`, `Need n8n`)
- **Multi-layer filtering** — age filter (< 7 days), spam/junk regex filtering, duplicate detection against existing Sheet records
- **AI-powered qualification** — every post gets scored 1–10 on lead quality, with a confidence score and extracted pain point/industry
- **AI-drafted outreach** — qualified leads (score ≥ 6) get both a public comment reply and a private DM draft, written in a professional, non-salesy tone
- **Human-in-the-loop by design** — nothing posts automatically. Every reply sits in a "Pending" queue for manual review before it goes anywhere near Facebook
- **Error handling** — a dedicated error workflow logs any node failure (API timeout, quota limits, etc.) to a separate Sheet tab, so failures are visible, not silent
- **Rate-limit safe** — batched AI requests with enforced delays to stay within free-tier API limits

## 🛠️ Tech Stack

| Component | Tool | Why |
|---|---|---|
| Orchestration | [n8n](https://n8n.io) | Visual, self-hostable workflow automation |
| Data source | [Apify](https://apify.com) — Facebook Search Scraper | Runs on Apify's infrastructure, zero risk to personal FB account |
| AI qualification & drafting | [Groq](https://groq.com) — Llama 3.3 70B | Fast inference, generous free-tier rate limits |
| Data store | Google Sheets | Lightweight CRM, zero setup cost, easy manual review |

## 📊 Google Sheets Schema

| Column | Description |
|---|---|
| Timestamp | When the post was processed |
| Username | Post author (if available) |
| Post text | Full post content |
| Post link | Direct URL to the post |
| Source | Group/Page name |
| Keywords matched | Which search term surfaced this post |
| Lead score | AI-assigned quality score (1–10) |
| Confidence | AI's confidence in its own scoring (1–10) |
| Industry | AI-inferred industry |
| Pain point | AI-extracted core problem |
| AI comment reply | Drafted public reply |
| AI DM reply | Drafted private message |
| Status | Pending / Approved / Rejected / Sent |

## 🚀 Setup

1. Import `Hybrid Facebook Lead Generation & Outreach System - GIT.json` into your n8n instance
2. Create Google Sheets credential in n8n, connect to a sheet with the schema above (tabs: `Leads`, `Errors`)
3. Get a free [Apify](https://apify.com) API token, find a current Facebook Search actor in the Apify Store, update the actor ID in the `Apify - FB Search Scraper` node
4. Get a free [Groq](https://console.groq.com/keys) API key, add it to both Groq HTTP nodes
5. **Enable batching** on both Groq nodes (Options → Batching → Items per Batch: 1, Batch Interval: 3000ms) to stay within free-tier rate limits
6. Activate the workflow — it runs automatically every 12 hours

## ⚠️ Known Constraints

- Facebook has no official public search API — this relies on a third-party scraper (Apify), which can break when Facebook updates anti-scraping measures. Check the Apify actor monthly.
- Free-tier API limits (Apify credits, Groq RPM) cap total throughput — tune `maxPosts` per keyword to match your usage needs.
- This workflow **intentionally does not auto-post**. Every reply requires manual review and posting, both to protect the connected Facebook account from spam-detection risk and to keep a human judgment layer in the loop.

## 📄 License

MIT — feel free to fork, adapt, and build on this for your own lead-gen needs.

---

**Built with n8n, Apify, and Groq — by [Muhammad Sohaib](https://shubeetheanalyst.github.io/)**
