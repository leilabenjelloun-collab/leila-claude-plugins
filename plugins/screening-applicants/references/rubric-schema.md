# Rubric Schema

This skill requires a rubric document to exist before screening starts (see the main `SKILL.md` "Before you start" section — no rubric means no screening, no exceptions in v1). This file defines the shape that document must take, illustrated with a real, verified example.

Rubrics are **role-specific**, not reusable across roles. The example below (AI Product Engineer) is only valid for screening against that exact posting — it says nothing useful about screening a sales, admin, or any other role. Every new role needs its own rubric written from scratch, or adapted deliberately, not inherited by default.

## Required structure

A rubric document has three parts:

1. **Criteria** — a list of weighted, numeric-scored dimensions. Each criterion needs:
   - A short name
   - A point value (the max score for that criterion)
   - A description of what evidence earns full marks, partial marks, and zero
   - Any weighting or ceiling rules specific to that criterion (e.g. a years-of-experience cap that limits the maximum achievable score regardless of other strengths)
2. **Total** — the sum of all criteria's max points. The posted note's top-line score is always `<achieved> / <total>`.
3. **Flags** — a separate list of qualitative, non-scored checks. Each flag has a name, a trigger condition, and is either raised (with a one-sentence reason) or not raised. Flags don't add or subtract from the score; they're separate signal for the recruiter to weigh however they judge fit.

## Worked example: AI Product Engineer (real rubric, recovered from live screening notes)

This is the actual rubric already in use for the "AI Product Engineer" requisition, reconstructed from two real posted notes. It's included here as a genuine example of the schema working, not as a template to copy for other roles.

**Criteria (21 points total):**

| Criterion | Max points | What earns full marks |
|---|---|---|
| Production ownership | 6 | Multi-year, well-documented track record of building and deploying real systems used by real users, across one or more roles. **Ceiling rule:** years of relevant experience caps the achievable score band regardless of narrative strength — e.g. ~4 years of experience caps this criterion at 4/6 even with excellent detail. |
| AI/LLM stack (hands-on) | 6 | Direct, named evidence of building production LLM/agent systems — specific frameworks (LangGraph, MCP, CrewAI, Google ADK, Dify), measurable outcomes (cost reduction, scale figures, adoption numbers). Naming a framework the candidate didn't use doesn't count against them; absence of framework names should be noted factually, not penalized as a gap unless the rubric says otherwise. |
| Evaluation & guardrail discipline | 3 | Explicit description of evaluation pipelines, observability, guardrails, or failure-mode thinking on a real production system — not just "I care about quality," but named practices (e.g. structured evals, trace-level debugging, deterministic checks outside the model). |
| Full-stack breadth | 3 | Demonstrated (not just listed) comfort across backend, frontend, and/or workflow automation, evidenced in the actual work narrative — not from a skills list alone. |
| Domain exposure | 3 | Direct, described experience in a regulated or document-heavy domain (real estate, healthcare, finance, etc.), not just adjacent industry exposure. |

**Flags (qualitative, not scored):**

- **Possible CV parsing problem** — raised when parsed fields look truncated, garbled, or contain content unrelated to the field they're in (see `SKILL.md` step 6 and `references/ambiguity-handling.md` for the general principle this flag implements).
- **Verbatim / full job-ad match** — raised when a candidate's answers or CV content closely mirror the job posting's own language, a signal worth naming rather than silently scoring as a strong match.
- **Skills listed without matching experience** — raised when a candidate's stated skills include items never referenced anywhere in their actual work narrative (e.g. a language or tool that appears only in a skills list, never in a job description).

## What a real evidence note looks like

From an actual posted note (candidate identity removed, this is the exact prose style expected):

> **AI/LLM stack (6/6).** Extensive, outcome-driven track record shipping LLM/agent systems across multiple employers, with explicit measured impact — scaled a healthcare AI assistant from 45K to over 1.2M monthly interactions, reduced infrastructure costs by €70K per year... LangGraph and MCP are both explicitly named and used... No evidence of CrewAI, Google ADK, or Dify anywhere in the CV.

Note what this does: states the score, cites specific, concrete facts from the candidate's real background, and names absence factually ("no evidence of X") without treating absence as an automatic penalty beyond what the criterion's own rules specify. See `references/note-formatting.md` for the exact structural spec this maps to.

## A known limitation, worth restating here

The note-creation tool only accepts plain text — no bold, no rich formatting (confirmed by direct test; markdown syntax like `**text**` is not interpreted and appears as literal characters). Write evidence as clear, well-organized plain prose; don't attempt markdown formatting expecting it to render.
