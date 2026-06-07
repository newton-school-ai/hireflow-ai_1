# HireFlow AI 🚀

> **Career autopilot for students.** HireFlow discovers jobs, tailors your resume for each one, applies on your behalf, and gives you a prep guide — all in one weekly cycle.

**Summer Profile Building Drive 2026 | Newton School of Technology**  
**Framework: Build → Understand → Defend**

---

## What Does HireFlow Do?

You tell HireFlow:
- Your resume / skills
- How many jobs you want to apply to per week (e.g., 10)
- What roles you're targeting ("AI Engineer Intern", "ML Developer")

HireFlow then:
1. **Discovers** 100+ relevant job openings from career pages and portals
2. **Scores** every job against your profile and picks the best matches
3. **Tailors** your resume specifically for each job (a different resume per company)
4. **Applies** automatically using form-filling automation
5. **Sends you** a weekly report with every resume used + a prep guide for each company

It also works in reverse — companies can use HireFlow to shortlist the best applicants from a pile of 100+ resumes.

---

## Who Is This For?

This project is built by a **pod of 5 students** as part of the Summer Profile Building Drive 2026. Every student owns a module, raises PRs, defends their decisions, and builds a real GitHub contribution history.

---

## Tech Stack

| What | Tool |
|------|------|
| Agent Orchestration | LangGraph |
| LLM | Claude / OpenAI API |
| Web Scraping | Playwright + BeautifulSoup |
| Vector Matching | FAISS + Sentence Transformers |
| Backend API | FastAPI |
| Frontend Dashboard | React + Tailwind CSS |
| Database | PostgreSQL |
| Resume Generation | LaTeX (XeLaTeX) |
| Email Delivery | SendGrid / Resend |
| Web Search | Tavily / SerpAPI |
| Deployment | Docker + Railway/Render |

---

## Project Structure

```
hireflow-ai/
├── src/
│   ├── agents/          # LangGraph agents (supervisor, application, prep guide, etc.)
│   ├── scrapers/        # Job discovery scrapers (Lever, Greenhouse, generic)
│   ├── pipelines/       # Match scoring, embedding, resume generation
│   ├── automation/      # Form filler, CAPTCHA handler
│   ├── api/             # FastAPI routes
│   ├── models/          # Data models (User, Job, Application, PrepGuide)
│   ├── templates/       # LaTeX resume templates
│   ├── utils/           # Shared helpers
│   └── config/          # Configuration and settings
├── tests/               # Unit and integration tests
├── notebooks/           # Exploration and experimentation
├── data/                # Sample data, fixtures
├── docs/                # Architecture diagrams, module docs
├── frontend/            # React dashboard
├── .env.example         # Copy this to .env and fill in your keys
├── requirements.txt     # Python dependencies
├── docker-compose.yml   # Run everything with one command
└── README.md
```

---

## Getting Started (Local Setup)

### Prerequisites
- Python 3.10+
- Node.js 18+ (for frontend)
- Docker (optional but recommended)
- PostgreSQL

### Step 1: Clone the repo
```bash
git clone https://github.com/newton-school-ai/hireflow-ai.git
cd hireflow-ai
```

### Step 2: Set up Python environment
```bash
python -m venv venv
source venv/bin/activate        # Mac/Linux
venv\Scripts\activate           # Windows

pip install -r requirements.txt
```

### Step 3: Configure environment variables
```bash
cp .env.example .env
# Open .env and fill in your API keys (OpenAI, Tavily, etc.)
```

### Step 4: Run with Docker (easiest)
```bash
docker-compose up
```

Or run manually:
```bash
uvicorn src.api.main:app --reload
```

---

## Pod Structure

| Role | Who | Responsibility |
|------|-----|----------------|
| **Maintainer** | 1 senior student | Repo architecture, PR reviews, LangGraph orchestration, integration |
| **Contributor 1** | Student | M2 — Job Discovery Engine (scrapers + spam filter) |
| **Contributor 2** | Student | M3 + M4 — Match Scorer + Resume Tailoring |
| **Contributor 3** | Student | M5 + M4 — Application Automation Agent |
| **Contributor 4** | Student | M6 + M7 — Prep Guide Generator + Weekly Report |

---

## Milestones (8 Sprints × ~3 days each)

| # | Milestone | Key Output |
|---|-----------|------------|
| M1 | Project Scaffold & User Onboarding | DB schema, profile model, profile intake API |
| M2 | Job Discovery Engine | Scrapers for 3 portal types + spam classifier |
| M3 | Job-Profile Match Scorer | Embedding pipeline + FAISS + multi-factor scorer |
| M4 | Weekly Quota Selector + Resume Engine | Top-N selector + LaTeX resume generator |
| M5 | Application Automation Agent | Playwright form filler + error recovery |
| M6 | Preparation Guide Generator | Interview rounds prediction + resources + mock questions |
| M7 | Weekly Report + Hiring Side Module | Email report + hiring shortlist agent |
| M8 | Dashboard, Integration & Demo | React dashboard + end-to-end testing + demo video |

---

## How to Contribute

Read **[CONTRIBUTING.md](CONTRIBUTING.md)** for the full workflow. Short version:

1. Find an open issue assigned to your milestone
2. Comment `"I'm working on this"` to claim it
3. Create a branch: `git checkout -b feature/your-feature-name`
4. Write your code, commit with clear messages
5. Open a PR → Maintainer reviews → Merge only when all criteria are met

> **AI Tools are allowed.** But you must be able to explain every decision in your code. If you used LangChain, know why. If you chose FAISS over ChromaDB, know why.

---

## Milestone Q&A (Accountability Sessions)

Every 2–3 days, students sit with faculty for a short Q&A on what was built in that sprint:
- **Contributors:** Why this solution? What other approaches exist?
- **Maintainer:** Why did you approve this PR? What did you check?
- **Faculty probes:** Edge cases, error handling, design decisions

This is not a gotcha — it's how you own your work.

---

## Links

- 📋 [Project Board — HireFlow Sprint Tracker](#) *(add GitHub Projects link)*
- 📁 [Issues](#) *(add GitHub Issues link)*
- 📖 [Blueprint / Full Spec](hireflow-ai-blueprint%20(1).md)

---

*HireFlow AI | Summer Profile Building Drive 2026 | Newton School of Technology*  
*Framework: Build → Understand → Defend*
