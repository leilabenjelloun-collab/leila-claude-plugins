# Screening Applicants

Screens job candidates in Recruitee against a structured, role-specific rubric and posts the evaluation as a note on their candidate profile — all through an anonymization layer that keeps candidate PII from ever reaching Claude directly.

## What it does

Given an offer and pipeline stage, this skill lists undecided candidates, checks each for a prior AI screening note to avoid duplicates, scores each one against a rubric you provide, and posts the result back to Recruitee — in batches of ~5, with a human checkpoint between each batch.

## Prerequisites

- **The Recruitee MCP connector** must be connected and enabled — this plugin has no MCP server of its own; it relies on an existing, separately-maintained Make-based Recruitee toolset (`get_candidate`, `list_offer_candidates`, `list_offer_stages`, `list_notes_for_candidate`, `create_note_for_candidate`).
- **A rubric document for the role being screened.** This skill will not draft one for you in v1 — see `references/rubric-schema.md` for the expected format. No rubric means no screening.

## Known v1 limitations

- Screening runs entirely on Recruitee's own parsed CV fields — the raw CV PDF is never exposed to Claude, by design (this is the anonymization boundary, not a bug).
- Posted notes are plain text only. Bold/rich formatting isn't supported by the underlying note-creation tool (confirmed by direct testing) — see `references/note-formatting.md`.
- This skill does not resolve note-to-offer scoping ambiguity if a candidate has applied to multiple offers — a known, accepted limitation of the underlying Recruitee API.

## Status

v0.1.0 — first iteration, awaiting real-world feedback from HR before promotion to the PropTech Partners company marketplace.
