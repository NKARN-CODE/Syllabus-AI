# Syllabus-AI
# Syllabus AI

A self-hosted academic command center for tracking a school syllabus —
chapters, exams, resources, revision, and progress — with a built-in
AI agent that can act on your data through natural language, not just
answer questions about it.

No cloud account. No subscription. Your data lives in a local SQLite
file on your own machine, reachable from your phone over your home
Wi-Fi.

![Python](https://img.shields.io/badge/python-3.10+-blue)
![FastAPI](https://img.shields.io/badge/backend-FastAPI-009688)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

## What it does

- **Tracks every chapter** across every subject — status, priority,
  difficulty, effort, notes — with instant inline updates, no modals.
- **Exam countdowns** computed from real dates, with per-exam
  readiness based on how many linked chapters are actually done.
- **Resource checklists** (textbook, tuition, question banks, DPPs…)
  auto-attached per subject by fixed importance — nothing to
  configure.
- **Retrieval / spaced revision** worksheets with due dates and
  scores.
- **Analytics** — completion, revision rate, weakest subjects, daily
  activity heatmap, weekly trend, subject breakdown, goals. Every
  number is computed from what you've actually logged; nothing is
  decorative or hardcoded.
- **XP, ranks, and achievements** as a side effect of normal use —
  moving a chapter forward earns XP automatically.
- **An AI agent, not just a chatbot.** A small floating orb lives on
  every page. Click it, tell it what you want in plain language —
  "mark Arithmetic Progressions as done," "add a Physics exam next
  Friday" — and it calls real backend tools to make the change, the
  same way a human clicking through the UI would. Supports Anthropic
  (Claude), OpenAI, or NVIDIA NIM as the model provider.

## Screens

| Dashboard | Analytics | Chapters |
|---|---|---|
| Landing hero: greeting, nearest exam, one real stat at a time | Every chart and number that used to clutter the home screen, now all in one place | Fast 4-state checklist per chapter, searchable and filterable |

---

## Quick start

```bash
git clone https://github.com/NKARN-CODE/Syllabus-AI.git
cd Syllabus-AI/Syllabus-dashboard-Final-complete

pip install -r requirements.txt

cd backend
python import_notion.py ../data/notion_export.json   # one-time: build syllabus.db from your data
python main.py
```

Open **http://localhost:8000**.

**Windows one-click:** double-click `Run_Dashboard.bat` instead — it
sets up the environment, imports your data, and opens a native window
automatically on first run.

**Phone access:** run `python desktop.py` instead of `main.py`. It
opens a native desktop window *and* keeps the server listening on
your LAN, so your phone can open `http://<your-pc-lan-ip>:8000` in a
browser at the same time — same database, same data, instantly
"synced" because there's only ever one copy.

---

## Using your own syllabus data

This ships with an `import_notion.py` script that expects a Notion
export (`data/notion_export.json`) as the seed data — that's how the
original tracker was built. You don't need Notion going forward:
after the one-time import, this app owns the data completely.

To use your own subjects/chapters instead of the sample data, either
adapt your own export to the same JSON shape `import_notion.py`
expects, or add chapters directly from the app's **Add** page once
it's running.

---

## Enabling the AI agent

The agent needs a model provider API key. From the app's **Settings**
page, choose a provider and paste in a key:

- **Anthropic** — a Claude API key
- **OpenAI** — an OpenAI API key
- **NVIDIA NIM** — a NIM API key

Without a key configured, the rest of the app works fully — the AI
orb will just report that it isn't configured yet when clicked.

---

## Tech stack

- **Backend:** FastAPI + SQLite (via `pydantic`, `uvicorn`)
- **Frontend:** plain HTML/CSS/JS — no framework, no build step
- **Desktop wrapper:** `pywebview` for a native window
- **AI:** `anthropic` / `openai` SDKs, tool-calling agent in
  `backend/ai_agent.py`

## Project structure

```
Syllabus-dashboard-Final-complete/
├── Run_Dashboard.bat        one-click launcher (Windows)
├── requirements.txt
├── backend/
│   ├── main.py               FastAPI app, all API routes
│   ├── db.py                 SQLite schema + scoring constants
│   ├── scoring.py             ROI/urgency scoring logic
│   ├── gamification.py         XP, ranks, achievements
│   ├── ai_agent.py            tool-calling AI agent
│   ├── ai_settings.py          provider/key management
│   ├── analytics.py, exams.py, goals.py, resources.py, worksheets.py
│   ├── import_notion.py        one-time data import
│   └── desktop.py             pywebview wrapper (native window + LAN)
├── frontend/
│   ├── index.html
│   ├── style.css
│   └── app.js
└── data/
    └── notion_export.json     sample/seed syllabus data
```

---

## License

MIT — do what you want with it.
