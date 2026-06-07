# Contributing to HireFlow AI

Welcome to the HireFlow AI project! This is a collaborative, open-source style project built by a pod of 5 students as part of the **Summer Profile Building Drive 2026** at Newton School of Technology.

This document explains how to work on the project — from picking up an issue to getting your PR merged.

---

## The Golden Rule

> **AI tools are allowed. But every decision in your code must be yours to defend.**

If you used a chunk size of 512 in your RAG pipeline, you should know why. If you chose FAISS over ChromaDB, you should know the trade-off. Faculty Q&A sessions happen every 2–3 days — you'll be asked.

---

## Pod Roles

### Maintainer (1 per pod)
- Sets up and owns the repository architecture
- Reviews all PRs before merging — never auto-merges
- Ensures modules connect correctly (integration responsibility)
- Owns `src/agents/supervisor.py` and `src/config/`
- Co-owns M1 setup and M7/M8 integration
- Leads the milestone Q&A from the review side

### Contributors (4 per pod)
- Each contributor owns specific milestones (see issue assignments)
- Pick up issues, build the solution, raise a PR
- Must write a clear PR description explaining what was built and why
- Cannot merge their own PR — the Maintainer merges after review
- Attend Q&A sessions ready to explain their code

---

## Workflow (Fork → Issue → Branch → PR → Review → Merge)

### Step 1: Find your issue
- Go to the [Issues tab](../../issues)
- Find an open issue in your assigned milestone (M1–M8)
- Read the issue description, acceptance criteria, and defense questions carefully

### Step 2: Claim it
- Comment `"I'm working on this"` on the issue
- The Maintainer will assign it to you

### Step 3: Create a branch
Always branch off `main`. Use this naming convention:

```
feature/m2-lever-scraper
fix/m3-embedding-null-handling
docs/m1-db-schema
```

```bash
git checkout main
git pull origin main
git checkout -b feature/m2-lever-scraper
```

### Step 4: Build
- Work in the relevant `src/` subfolder
- Keep your changes focused on what the issue asks
- Write at least basic tests in `tests/` for your code
- Use meaningful commit messages:

```bash
# Good
git commit -m "feat(scrapers): add Lever career page scraper with pagination"
git commit -m "fix(matcher): handle null stipend field in scoring"

# Bad
git commit -m "done"
git commit -m "fix stuff"
```

### Step 5: Open a Pull Request
When your code is ready:

```bash
git push origin feature/m2-lever-scraper
```

Open a PR on GitHub. Use the PR template — fill every section. At minimum:
- What does this PR do?
- Which issue does it close? (`Closes #12`)
- How did you test it?
- Any design decisions worth calling out?

### Step 6: Review and Merge
- Maintainer reviews the PR (may request changes)
- Address all review comments
- Maintainer merges when all acceptance criteria are met
- **Contributors do not merge their own PRs.**

---

## Branch Protection Rules

The `main` branch is protected:
- No direct pushes — always open a PR
- PR requires at least 1 approval (from Maintainer)
- All checks must pass before merge

---

## Coding Standards

- **Python formatting:** Use `black` — run `black src/` before committing
- **Linting:** Use `ruff` — run `ruff check src/`
- **Environment variables:** Never hardcode API keys. Always use `.env` (see `.env.example`)
- **Virtual environment:** Always develop inside `venv`. Never install global packages for the project.
- **Dependencies:** If you add a new package, add it to `requirements.txt`

```bash
# After installing a new package
pip freeze | grep package-name >> requirements.txt
```

---

## File & Secret Management

| File | Status | Rule |
|------|--------|------|
| `.env` | ❌ Never commit | Contains API keys — already in `.gitignore` |
| `.env.example` | ✅ Commit | Template showing what keys are needed |
| `requirements.txt` | ✅ Commit | Update when you add dependencies |
| `data/resumes/` | ❌ Never commit | Generated PDFs — already in `.gitignore` |

If you accidentally commit a secret, tell the Maintainer immediately. The commit history must be cleaned and keys rotated.

---

## Milestone Q&A Sessions

Every 2–3 days, the pod sits with faculty for a short structured Q&A.

**What to expect:**
- You'll be asked to walk through what you built
- You'll be asked *why* you made specific technical decisions
- The Maintainer will be asked why they approved specific PRs
- Faculty will probe edge cases and error handling

**This is not optional.** Every contributor must be able to defend their milestone work. If you can't explain it, you don't own it.

---

## Issue Labels

| Label | Meaning |
|-------|---------|
| `m1` – `m8` | Which milestone the issue belongs to |
| `agent` | Related to LangGraph / agent code |
| `scraper` | Related to Playwright / BeautifulSoup scrapers |
| `frontend` | Related to React dashboard |
| `infra` | Related to Docker, DB, CI/CD |
| `milestone` | Milestone-level tracking issue |
| `good first issue` | Good starting point for new contributors |
| `needs-review` | PR is ready for Maintainer review |

---

## Questions?

Open a [Discussion](../../discussions) or ask in the pod channel. Don't stay stuck silently.

---

*Summer Profile Building Drive 2026 | Newton School of Technology*
