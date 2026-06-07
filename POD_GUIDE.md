# HireFlow AI — Pod Guide

**Summer Profile Building Drive 2026 | Newton School of Technology**  
**Framework: Build → Understand → Defend**

---

## What Is a Pod?

A pod is your team for this project — 5 students working like a real engineering team, not a group assignment.

| Role | Count | Responsibility |
|------|-------|----------------|
| **Maintainer** | 1 | Owns the repo, reviews all PRs, integrates modules, leads Q&A from the review side |
| **Contributor** | 4 | Owns specific milestones, raises PRs, defends their work at Q&A |
| **Faculty** | — | Acts as Product Manager — reviews direction, quality, edge cases |

---

## Pod Assignments

| Role | Name | GitHub Username | Milestone(s) |
|------|------|-----------------|--------------|
| Maintainer | | | M1 (scaffold), M7 (hiring side), M8 (integration) |
| Contributor 1 | | | M2 — Job Discovery |
| Contributor 2 | | | M3 + M4 (quota/resume) |
| Contributor 3 | | | M4 (resume engine) + M5 (automation) |
| Contributor 4 | | | M6 (prep guide) + M7 (report) |

> Fill in names and GitHub usernames when the pod is formed.

---

## Sprint Timeline (~5 Weeks)

| Week | Milestones | Q&A Days |
|------|-----------|----------|
| Week 1 | M1 + M2 | Day 3 + Day 6 |
| Week 2 | M3 + M4 | Day 3 + Day 6 |
| Week 3 | M5 + M6 | Day 3 + Day 6 |
| Week 4 | M7 | Day 3 + Day 5 |
| Week 5 | M8 — Integration + Demo | Day 3 + Day 5 (final demo) |

---

## Daily Standup (Async — in GitHub Discussions or pod channel)

Every day, each contributor posts:
1. What I did yesterday
2. What I'm doing today
3. Any blockers

Keep it to 3 bullet points. This is not optional.

---

## Milestone Q&A Rules

Every 2–3 days, the pod sits with faculty for 15–20 minutes.

**Format:**
- Contributor walks through what they built (live demo or code walkthrough)
- Faculty asks defense questions from the milestone issue
- Maintainer is asked why they approved the PR — what did they review for?

**If you can't answer a defense question:**  
That's fine — say what you know, say what you don't, and commit to finding out. What's not acceptable: "AI wrote it, I don't know why."

---

## AI Usage Policy

You **can** use AI tools (ChatGPT, Claude, Copilot, etc.) for:
- Generating boilerplate
- Explaining concepts
- Debugging
- Drafting code

You **cannot**:
- Copy-paste AI output without understanding it
- Submit a PR and be unable to explain any line of it
- Let AI make design decisions without you understanding and owning them

The Q&A session is the accountability layer. No rule enforcement needed — the conversations will reveal everything.

---

## What Makes a Good Maintainer?

A good Maintainer does not just approve PRs. They:
- Read every line of the PR diff
- Run the code locally before approving
- Ask "does this handle the edge cases?"
- Reject PRs that don't meet acceptance criteria — even from friends
- Track integration: do all modules connect correctly?
- Know the full architecture well enough to answer "walk me through the whole system"

Being a Maintainer is the hardest role in the pod and the most valuable one for interviews. Own it.

---

## What Makes a Good Contributor?

A good Contributor:
- Picks up issues proactively, doesn't wait to be assigned
- Writes code in small, testable chunks — not one giant PR
- Writes clear PR descriptions
- Reviews their own code before requesting review
- Shows up to Q&A ready to defend every decision

---

## GitHub Profile Building — What You're Building Here

By the end of this project, your GitHub profile will show:
- ✅ Commits with meaningful messages
- ✅ PRs raised and merged (contribution graph activity)
- ✅ Issues opened and closed
- ✅ Code reviews given and received
- ✅ A real README, real project structure, real tech stack

This is exactly what recruiters and open-source program selectors (GSoC, SSoC, etc.) look for. A solo project with 3 commits and no issues does not signal team readiness. This does.

---

## Quick Reference — Git Commands

```bash
# Start a new feature
git checkout main && git pull
git checkout -b feature/m2-lever-scraper

# Save work
git add .
git commit -m "feat(scrapers): add Lever scraper with pagination"

# Push and open PR
git push origin feature/m2-lever-scraper
# Then open PR on GitHub

# Stay up to date with main
git fetch origin
git rebase origin/main
```

---

*Summer Profile Building Drive 2026 | Newton School of Technology*
