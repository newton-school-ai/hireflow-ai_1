# HireFlow AI — GitHub Issues (M1 to M8)

This file contains the full issue descriptions for all 8 milestones.  
Copy each issue into GitHub Issues when setting up the project board.  
Each issue maps to the **HireFlow Sprint Tracker** board → Backlog column.

> HireFlow supports both **job applications** (full-time) and **internship applications**. All modules should handle both types — they differ mainly in `experience_required`, `stipend_salary` (vs salary), and application form structure.

---

## M1 — Project Scaffold & User Onboarding

**Labels:** `m1`, `milestone`, `infra`  
**Assignees:** Maintainer + Contributor 1  

### What needs to be built

Set up the complete project foundation: database schema, data models, and the user onboarding flow. A user should be able to upload their resume (or fill a profile form), have their skills and experience extracted by an LLM, set a weekly application quota, and have their profile stored in the database.

This milestone also covers the initial repo structure, environment setup, and deployment scaffold.

### Acceptance Criteria

- [ ] PostgreSQL schema created with all 5 tables: `users`, `jobs`, `applications`, `prep_guides`, `weekly_reports` (+ `shortlists` for optional module)
- [ ] SQLAlchemy models defined for all tables in `src/models/`
- [ ] FastAPI endpoint: `POST /profile` — accepts PDF resume upload or JSON profile, extracts skills/experience via LLM, saves to DB
- [ ] FastAPI endpoint: `GET /profile/{user_id}` — returns stored profile
- [ ] `.env.example` filled with all required keys, `.env` working locally
- [ ] `requirements.txt` complete and tested in a fresh `venv`
- [ ] `docker-compose up` starts API + PostgreSQL successfully
- [ ] Alembic migration for initial schema
- [ ] Basic README setup instructions work for a fresh clone

### Files Expected

- `src/models/user.py` — User model
- `src/models/job.py` — Job model
- `src/models/application.py` — Application model
- `src/models/prep_guide.py` — PrepGuide model
- `src/models/report.py` — WeeklyReport model
- `src/api/routes/profile.py` — Profile endpoints
- `src/config/settings.py` — Pydantic settings from .env
- `src/config/database.py` — SQLAlchemy engine + session
- `migrations/` — Alembic migration files

### Defense Questions

1. What is a virtual environment (`venv`) and why is it non-negotiable in this project? What breaks in CI/CD if you don't use one?
2. Why do we store API keys in `.env` and never commit them? What happens if you accidentally push a key to a public repo?
3. Walk me through the profile intake flow — what happens from the moment a user uploads their PDF to when the data is stored in the DB?
4. Why did you use Alembic for migrations instead of running raw SQL? What's the advantage in a team setting?
5. The user profile has a `weekly_quota` field. How does this value flow into later modules (match scorer, quota selector)?

---

## M2 — Job Discovery Engine

**Labels:** `m2`, `milestone`, `scraper`  
**Assignees:** Contributor 1  

### What needs to be built

Build the job discovery engine that scrapes career pages and job portals to build a pool of 100+ relevant job and internship openings per cycle. Must handle static pages, JavaScript-rendered SPAs, and API-backed portals. Includes a spam/quality filter.

### Acceptance Criteria

- [ ] Scraper for **Lever** career pages (e.g., `jobs.lever.co/company`) — extracts all required fields
- [ ] Scraper for **Greenhouse** career pages (e.g., `boards.greenhouse.io/company`) — extracts all required fields
- [ ] Generic scraper for custom career pages using Playwright (handles JS-rendered content)
- [ ] BeautifulSoup scraper for static pages
- [ ] All scrapers extract: `company_name`, `role_title`, `jd_text`, `skills_required`, `experience_required`, `location`, `stipend_salary`, `application_url`, `posting_date`, `selection_process`, `source`
- [ ] Spam/quality filter: flags vague JDs, missing company info, unrealistic claims — saves `is_spam=True` with `spam_confidence` score
- [ ] All discovered jobs saved to the `jobs` table
- [ ] Scraper handles both job (full-time) and internship listings
- [ ] Rate limiting implemented — no more than X requests per second per domain
- [ ] At least 5 test cases in `tests/test_scrapers.py`

### Files Expected

- `src/scrapers/lever_scraper.py`
- `src/scrapers/greenhouse_scraper.py`
- `src/scrapers/generic_scraper.py`
- `src/scrapers/static_scraper.py`
- `src/agents/spam_filter.py`
- `tests/test_scrapers.py`

### Defense Questions

1. Why Playwright over Selenium for JS-rendered pages? When would Selenium still be the better choice?
2. How do you handle a career page that loads jobs via infinite scroll or "Load More" button?
3. Your spam filter flags a real early-stage startup that has a short, simple JD. How do you reduce false positives?
4. What is your rate limiting strategy? How do you avoid getting IP-banned while scraping?
5. A career page updates its HTML structure overnight — your CSS selectors break. How do you make your scraper resilient to this?
6. How do you differentiate between internship listings and full-time job listings in your scraper output?

---

## M3 — Job-Profile Match Scorer

**Labels:** `m3`, `milestone`, `agent`  
**Assignees:** Contributor 2  

### What needs to be built

Build the embedding-based matching engine that scores every discovered job against the user's profile. Output is a ranked list where rank 1 = best fit, with per-job skill gap analysis.

**Scoring weights:**
- Skill Match: 40%
- Role Fit: 20%
- Experience Fit: 15%
- Location Match: 10%
- Stipend/Salary Fit: 10%
- Company Signal: 5%

### Acceptance Criteria

- [ ] Embedding pipeline: generates vector embeddings for both user profile (skills + experience + projects) and each JD
- [ ] FAISS index built from job embeddings, supports similarity search
- [ ] Multi-factor scorer implemented with exact weights above
- [ ] Output per job: `match_score` (0–1 float), `skill_matches`, `skill_gaps`, `rank`
- [ ] Skill gap extraction: clearly identifies skills in JD that are missing from the user's profile
- [ ] Handles both internship (stipend) and full-time job (salary) scoring — `stipend_salary` field
- [ ] Match results saved to `applications` table (planned status)
- [ ] At least 3 unit tests in `tests/test_matcher.py`
- [ ] Scoring function is deterministic (same input → same output)

### Files Expected

- `src/pipelines/embedding_pipeline.py`
- `src/pipelines/match_scorer.py`
- `tests/test_matcher.py`

### Defense Questions

1. What embedding model did you use? Why? How would you evaluate whether it's doing a good job at matching JDs to profiles?
2. Why FAISS over ChromaDB for this use case? When would ChromaDB be a better choice?
3. The skill match weight is 40%. Why not 60%? How did you decide the relative weights?
4. Two jobs have the same match score (0.87). How does the system break the tie?
5. What chunk size did you use when embedding JDs? What happens if the JD is only 3 lines long?
6. A user applies for internships only. How does the experience fit component handle the scoring (they're a fresher — `experience_years = 0`)?

---

## M4 — Weekly Quota Selector + Resume Tailoring Engine

**Labels:** `m4`, `milestone`, `agent`  
**Assignees:** Contributor 2 (quota selector) + Contributor 3 (resume engine)  

### What needs to be built

Two connected components: (1) the quota selector that picks the top N jobs from the ranked list based on the user's weekly limit, and (2) the resume tailoring engine that generates a unique, ATS-optimized resume for each selected job.

### Acceptance Criteria

**Quota Selector:**
- [ ] Takes ranked job list + user's `weekly_quota` → returns top N jobs
- [ ] Filters out: already-applied companies, expired listings, user blacklist
- [ ] If `auto_apply = false`: returns the list to the user for confirmation before proceeding
- [ ] Weekly application plan generated (structured list of top N with match scores, key gaps)

**Resume Tailoring Engine:**
- [ ] For each selected job: extracts required skills from JD → maps to user's matching skills and projects
- [ ] Generates tailored resume sections: Summary (role-specific), Skills (JD-priority ordered), Projects (top 2–3 most relevant), Experience, Education
- [ ] Resume generated as PDF (LaTeX or ReportLab)
- [ ] Each resume saved at: `data/resumes/{user_id}/{job_id}_resume.pdf`
- [ ] Application record updated in DB: `resume_path`, `resume_version`, `status = 'planned'`
- [ ] Works for both internship and full-time applications (adjust summary tone accordingly)
- [ ] At least 2 test cases

### Files Expected

- `src/pipelines/quota_selector.py`
- `src/pipelines/resume_generator.py`
- `src/templates/resume_latex/base_template.tex`
- `tests/test_resume.py`

### Defense Questions

1. How does the resume tailoring engine decide which 2–3 projects to include when the user has 6 projects?
2. Why LaTeX for resume generation? What are the trade-offs vs HTML/CSS → PDF or ReportLab?
3. What happens when a user has zero projects that match the JD requirements?
4. The user's weekly quota is 10 but only 7 jobs pass the quality threshold. What does the system do?
5. How do you ensure the generated resume is ATS-parseable and not just visually pretty?
6. How does the resume tone/language differ between an internship application and a full-time application?

---

## M5 — Application Automation Agent

**Labels:** `m5`, `milestone`, `agent`  
**Assignees:** Contributor 3  

### What needs to be built

Build the Playwright-based automation agent that fills and submits job/internship application forms. Must handle multi-step forms, file uploads, free-text questions, rate limiting, CAPTCHA detection, and error recovery.

### Acceptance Criteria

- [ ] Agent navigates to `application_url` for each job in the weekly plan
- [ ] Fills standard form fields: name, email, phone, education, experience, skills
- [ ] Uploads tailored resume PDF to file upload fields
- [ ] Handles free-text fields (e.g., "Why do you want to work here?") using LLM generation with JD context
- [ ] Multi-step / multi-page form support — resumes from last completed page on failure
- [ ] CAPTCHA detection: flags the application as `status = 'needs_action'` (does NOT attempt to solve)
- [ ] Rate limiting: configurable delay between submissions
- [ ] Application status logged: `'applied'`, `'failed'`, `'needs_action'`
- [ ] Failure reason recorded in DB for failed applications
- [ ] Works for both job portals (LinkedIn, etc.) and internship portals (Internshala, Wellfound)
- [ ] At least 3 test cases with mock pages

### Files Expected

- `src/agents/application_agent.py`
- `src/automation/form_filler.py`
- `src/automation/captcha_handler.py`
- `tests/test_automation.py`

### Defense Questions

1. How do you handle a multi-page application where page 3 of 4 fails mid-submission?
2. The application portal has a CAPTCHA. What does your agent do, and why is solving it not an option?
3. How do you avoid being detected as a bot? What anti-detection measures did you implement in Playwright?
4. What is your retry strategy? After how many attempts do you mark an application as failed?
5. A free-text field asks "What's your biggest weakness?" — how does your agent generate a response that doesn't sound generic?
6. How does form-filling differ between a LinkedIn job application and an Internshala internship application?

---

## M6 — Preparation Guide Generator

**Labels:** `m6`, `milestone`, `agent`  
**Assignees:** Contributor 4  

### What needs to be built

For every job or internship applied to, generate a structured interview preparation guide. This includes: predicted interview rounds, topics to prepare (strong/moderate/gap), curated learning resources, mock interview questions, and company intelligence.

### Acceptance Criteria

- [ ] Interview round prediction: analyzes JD + company career page + company stage to predict number of rounds and focus areas
- [ ] Topic analysis: categorizes user's skills into Strong / Moderate / Gap relative to JD requirements
- [ ] Resource finder: for each gap or key topic, finds 2–3 relevant links (docs, tutorials, videos) using Tavily/SerpAPI
- [ ] Mock question generator: generates 5–10 JD-specific interview questions (technical + behavioral + design)
- [ ] Company intel scraper: collects company stage, tech stack, recent news, Glassdoor/AmbitionBox interview patterns (if available)
- [ ] Handles gracefully when company has zero reviews — provides fallback guidance
- [ ] Prep guide saved to `prep_guides` table and as PDF/HTML in `data/reports/`
- [ ] Prep guide is distinct per job — not a generic template repeated 10 times
- [ ] Works for both internship interviews (lighter, fewer rounds) and full-time interviews (more structured)

### Files Expected

- `src/agents/prep_guide_agent.py`
- `src/agents/company_intel_agent.py`
- `tests/test_prep_guide.py`

### Defense Questions

1. How do you predict the number of interview rounds when the JD doesn't explicitly mention a selection process?
2. Your resource finder returns a link that's broken or paywalled. How do you handle this?
3. How do you make sure the mock questions are specific to the JD and company, not just generic AI interview questions?
4. The company has zero Glassdoor reviews and no public interview history. What does the company intel module output?
5. Internship interviews are typically shorter and less structured than full-time interviews. How does your round prediction account for this?
6. You're generating prep guides for 10 different jobs in one weekly cycle. How do you keep them distinct and high-quality without just templating the same content?

---

## M7 — Weekly Report + Hiring Side Module

**Labels:** `m7`, `milestone`, `agent`  
**Assignees:** Contributor 4 + Maintainer  

### What needs to be built

Two components: (1) the weekly report generator that compiles all 10 applications + resumes + prep guides into a single user-facing email report, and (2) the hiring side agent that takes a company's job/internship description + applicant pool and shortlists the top candidates.

### Acceptance Criteria

**Weekly Report:**
- [ ] Aggregates all applications from the weekly cycle: status, match score, resume used, prep guide
- [ ] Cross-application insights: most in-demand skills this week, user's strongest/weakest match categories, skill gap frequency analysis
- [ ] Weekly study plan: prioritized list of skills to learn based on gap frequency across all JDs
- [ ] Report sent via email (SendGrid/Resend) with resume PDFs attached or linked
- [ ] Report saved to `weekly_reports` table and as HTML/PDF in `data/reports/`
- [ ] Clearly distinguishes between internship applications and full-time applications in the summary

**Hiring Side Agent (Optional Module):**
- [ ] Accepts: JD (text) + applicant data (CSV or JSON)
- [ ] Parses each applicant: skills, experience, projects, education
- [ ] Scores each applicant against JD using the same scoring engine as M3
- [ ] Produces shortlist of top N candidates (company specifies N)
- [ ] Generates shortlist report: per-candidate summary with match score and key strengths
- [ ] Sends email to hiring team with shortlist attached
- [ ] Saved to `shortlists` table

### Files Expected

- `src/agents/report_generator.py`
- `src/agents/hiring_shortlist_agent.py`
- `tests/test_report.py`

### Defense Questions

1. The weekly report has 10 prep guides. How do you keep the email readable — do you embed everything or link out?
2. How do you prioritize which skill gap to study first when 10 JDs each have different gaps?
3. For the hiring side: your scoring engine ranks applicants by skill match. How do you prevent it from inadvertently penalizing non-traditional backgrounds (self-taught, bootcamp, non-CS degree)?
4. One application failed (CAPTCHA). How does the report handle it — what does the user see and what action are they prompted to take?
5. How do you handle the case where the user applied to both internships and full-time jobs in the same week — does the report treat them differently?

---

## M8 — Dashboard, Integration & Demo

**Labels:** `m8`, `milestone`, `frontend`  
**Assignees:** All pod members  

### What needs to be built

Build the React frontend dashboard and complete the end-to-end integration of all modules. Every module (M1–M7) must connect and run as a single pipeline. Includes edge case handling, a demo video, and full project documentation.

### Acceptance Criteria

**Frontend Dashboard:**
- [ ] Weekly plan view: shows top N selected jobs with match scores, skill gaps, confirm/edit before applying
- [ ] Application tracker: table of all applications with status (applied / failed / needs action), match score, resume link
- [ ] Prep guide viewer: per-application prep guide display (topics, resources, mock questions, company intel)
- [ ] Resume viewer: list of all tailored resumes with download links
- [ ] Profile page: user can update their master profile and weekly quota
- [ ] Mobile-responsive layout

**Integration:**
- [ ] End-to-end flow works: profile upload → scrape → score → select → tailor resume → apply → prep guide → report → dashboard
- [ ] All failure states handled: CAPTCHA blocked, scraper error, LLM timeout
- [ ] Handles internship applications and full-time job applications in the same weekly cycle

**Demo & Docs:**
- [ ] Demo video (3–5 min): walkthroughs the full pipeline from profile upload to weekly report
- [ ] `docs/architecture.md`: system architecture diagram + module descriptions
- [ ] All README setup instructions tested on a fresh machine
- [ ] At least 10 total tests passing across all modules

### Files Expected

- `frontend/src/` — React components (Dashboard, ApplicationTracker, PrepGuideViewer, etc.)
- `docs/architecture.md`
- `docs/demo.md` (link to demo video)

### Defense Questions (All Pod Members)

**Maintainer:**
1. Walk me through the complete data flow: "user sets quota = 10" → "weekly report delivered." Name every function, every DB write.
2. How do agents coordinate in LangGraph? What is the state schema? What happens when the scraper fails mid-pipeline?
3. How would you add support for a new job portal (e.g., AngelList) without touching core architecture?

**All Contributors:**
4. If you had to do your milestone over, what would you build differently?
5. What was the hardest edge case you encountered and how did you handle it?
6. What is one thing about your module that could fail silently in production, and how would you detect it?

---

## GitHub Labels to Create

Run these in your repo settings or use `gh label create`:

| Label | Color | Description |
|-------|-------|-------------|
| `m1` | `#0075ca` | Milestone 1 — Scaffold & Onboarding |
| `m2` | `#0075ca` | Milestone 2 — Job Discovery |
| `m3` | `#0075ca` | Milestone 3 — Match Scorer |
| `m4` | `#0075ca` | Milestone 4 — Quota Selector + Resume |
| `m5` | `#0075ca` | Milestone 5 — Application Agent |
| `m6` | `#0075ca` | Milestone 6 — Prep Guide Generator |
| `m7` | `#0075ca` | Milestone 7 — Weekly Report + Hiring |
| `m8` | `#0075ca` | Milestone 8 — Dashboard + Demo |
| `agent` | `#e4e669` | LangGraph / agent code |
| `scraper` | `#d93f0b` | Playwright / BeautifulSoup scrapers |
| `frontend` | `#bfd4f2` | React dashboard |
| `infra` | `#c2e0c6` | Docker, DB, CI/CD |
| `milestone` | `#0e8a16` | Milestone-level tracking issue |
| `good first issue` | `#7057ff` | Good starting point |
| `needs-review` | `#fbca04` | PR ready for Maintainer review |

---

## Project Board: HireFlow Sprint Tracker

**Columns:**

| Column | Purpose |
|--------|---------|
| **Backlog** | All 8 milestone issues start here |
| **In Progress** | Issue is actively being worked on |
| **In Review** | PR raised, waiting for Maintainer review |
| **Done** | PR merged, issue closed |

**Setup:** GitHub → Projects → New Project → Board view → Add all 8 issues to Backlog.
