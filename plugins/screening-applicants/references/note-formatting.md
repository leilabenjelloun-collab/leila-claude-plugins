# Note Formatting

The note is for a recruiter to act on, not an audit trail. No literal CV quotes, no meta-references to "the rubric" or "criterion 2 requires," no narrating the scoring mechanics. State findings plainly — what the candidate did, where, for how long, what's missing.

Keep it terse. When a flag isn't raised, say so in one line and move on — don't explain the checking process. Spend words only where a flag *is* raised or a rating needs its specific missing evidence named.

## A known tool limitation — read this before writing anything

The `create_note_for_candidate` tool accepts **plain text only**. This was directly tested: sending markdown syntax like `**bold**` produces literal asterisk characters in the posted note, not bold formatting. Do not attempt bold, italics, or any markdown — write clean plain prose instead, and use blank lines (a double line break) for visual separation between sections, which *does* render correctly as paragraph spacing.

If a future version of this tool exposes rich-text or structured input, this spec should be revisited — bold section headers would improve scanability. For now, structure and clarity of prose carry that job instead.

## Structure

1. **Score line, first.** `Score: <achieved> / <total>` on its own line, followed by a blank line.
2. **One paragraph per criterion**, each starting with the criterion name and its score in parentheses, followed by a period, then plain evidence sentences in the same paragraph — no line break between the criterion header and its evidence. A blank line separates each criterion's paragraph from the next.
3. **One extra blank line** before the flags block, so it's visually separated from the scored criteria.
4. **Flags, one per line**, each formatted `Flag - <name>: Not raised.` or `Flag - <name>: Raised. <one-sentence reason>.` A blank line between each flag line for readability, matching the spacing used between criteria.

## Worked example (real, verified format — this exact structure was posted and confirmed readable)

```
Score: 19 / 21

Production ownership (6/6). Emmanuel has six years of continuous software and data engineering experience across three roles, so no years ceiling applies. He built automated ETL workflows at Domicil Real Estate Group, designed enterprise-scale data platforms at Accenture with measured performance and cost improvements, and is now Senior Data and AI Engineer at Wefra Life, where he designed the end-to-end architecture of a production AI platform and led a 200-plus model data-platform migration.

Hands-on AI/LLM stack experience (6/6). At Wefra Life he designed and built a production LLM orchestration platform using FastAPI, LangGraph, and OpenAI APIs that lets business users query enterprise healthcare data in natural language, with structured outputs, function calling, conversation memory, and fallback strategies. LangGraph is directly evidenced; MCP, CrewAI, Google ADK, and Dify are not mentioned anywhere on the CV.

Evaluation and guardrail discipline (3/3). He describes automated evaluation pipelines, prompt evaluation, and resilient fallback strategies, and explicitly controls what the LLM can access, validates inputs, and keeps deterministic business logic outside the model, all for a live production system.

Full-stack breadth (1/3). The experience narrative across all three roles is backend and data-engineering focused, centered on APIs, ETL pipelines, and data platforms. Frontend and mobile technologies appear in his skills list but are not described anywhere in the actual work history, so only one relevant surface is demonstrated.

Domain exposure (3/3). He worked directly in real estate at Domicil Real Estate Group, building financial reporting and acquisition-analytics pipelines, and is now building AI systems over enterprise healthcare data at Wefra Life.


Flag - Possible CV parsing problem: Not raised.

Flag - Verbatim / full job-ad match: Not raised.

Flag - Skills listed without matching experience: Raised. TypeScript, Docker, and mobile app frameworks (React Native, Expo) are listed among the candidate's skills but none of them are referenced anywhere in the experience narrative.
```

## The `[AI Screening v1]` marker

Don't add this yourself — it's automatically prepended by the note-creation step (see `SKILL.md`). Writing it manually would duplicate it.

## After posting

Re-fetch the note via `list_notes_for_candidate` rather than trusting whatever the post call itself returns — verifying by re-read is cheap insurance against a silent formatting failure. If you can't confirm the note landed as intended, say so in your batch report instead of assuming success and moving on.
