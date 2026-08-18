---
name: candidate-screen
description: Screen job candidates in Recruitee against a structured scoring rubric and post evaluation notes back to their candidate profile, via the anonymized Recruitee MCP toolset. Use this whenever the user asks to screen, evaluate, score, or triage candidates for a Recruitee offer or pipeline stage, review a batch of applicants, or catch up an offer's candidate list against a rubric — even if they just say "go through the candidates for X," "screen the new applicants," or reference a criteria/rubric file. Make sure to trigger this whenever Recruitee candidates and any kind of scoring or note-posting are both in play, not just when the word "screen" is used explicitly.
---

# Candidate Screen

This skill scores real candidates against a rubric and posts the result as a note on their live Recruitee profile. Nothing here is a dry run — a posted note is visible to the recruiters who make the actual call, so treat every step as if a person is reading over your shoulder, because functionally one will be.

**A known, deliberate limitation:** the Recruitee MCP tools this skill uses never expose a candidate's raw CV file. Screening runs entirely on Recruitee's own parsed `fields` (skills, education, experience) and `open_question_answers` — never the original PDF. This is intentional, not a bug: it's how candidate PII is kept out of this conversation. It also means parsing errors in Recruitee's own extraction are a real, expected failure mode — see the parsing-issue flag in Step 5.

## Before you start — find the rubric

This skill requires a rubric document to already exist for the role being screened. Look for it, in this order: a file the user attached this turn, a project doc matching a name like `*criteria*.json` or `*rubric*.json`, or a path the user names directly.

**If no rubric is found, stop here.** Don't fall back on generic HR judgment about what a "good candidate" looks like — that plausible-sounding-but-ungrounded judgment is exactly the failure mode a rubric exists to prevent, and it's also not this skill's job to construct one on the fly. Tell the user plainly that no rubric was found for this role and screening can't proceed without one, pointing them to `${CLAUDE_PLUGIN_ROOT}/references/rubric-schema.md` for the expected format. Do not attempt to draft a rubric interactively in v1 — that's a deliberate scope boundary, not an oversight.

Read `${CLAUDE_PLUGIN_ROOT}/references/rubric-schema.md` now if you haven't seen a rubric like this before — it explains the schema this skill expects: weighted numeric criteria with descriptive levels, a mandatory evidence note per criterion, and a separate list of qualitative flags. It also includes a full worked example.

## The loop

1. **Identify the offer and stage.** An offer usually has several pipeline stages; screening the wrong one burns real posts on real people for no reason. If the user names a stage but not its ID, call the `list_offer_stages` tool to resolve the name to a `stageId` — stage IDs aren't visible anywhere in Recruitee's own UI, so this lookup is required, not optional. Confirm the offer and stage with the user if either is ambiguous from context.
2. **List candidates in that stage** via `list_offer_candidates`, passing both `offerId` and `stageId`. This returns only `{id, offer_id}` per candidate — no name, no content — by design.
3. **For each candidate, before doing any scoring work, check `list_notes_for_candidate` and read `already_screened`.** If `true`, skip this candidate — do not re-screen or re-post. Report the skip in your batch summary rather than silently omitting it, so the user knows why the count looks lower than expected.
4. **Process the remaining candidates in batches of about 5, not the whole stage in one unbroken run.** After each batch, stop and report back: candidate IDs, scores, any flags raised, any parsing concerns, and any skipped-as-already-screened candidates — then wait for the user to say "continue" before pulling the next batch. This checkpoint isn't bureaucracy for its own sake. There's no undo on a posted note, and a batch of five is small enough for a human to actually read and catch something before ten more go out the same way.
5. **For each candidate, fetch full detail via `get_candidate`**, then score every rubric criterion independently against the evidence bar it defines, using `fields` and `open_question_answers` only. Apply whatever weighting or ceiling adjustments the rubric specifies, sum to the total, and evaluate every flag in the rubric against its own stated trigger conditions — don't invent additional flags or skip ones that seem inapplicable at a glance.
6. **When something's ambiguous, widen the assessment rather than guessing.** If parsed data looks truncated, garbled, or has an unexplained employment gap, that's a signal for a parsing-issue flag (or whatever the rubric's equivalent is called) — raise it rather than quietly scoring the gap as an absence of experience. See `${CLAUDE_PLUGIN_ROOT}/references/ambiguity-handling.md` for the full set of these judgment calls. If a rubric criterion asks you to look something up externally (e.g. what a named employer actually does), do that lookup rather than defaulting to "no evidence" because the parsed data is silent.
7. **Compose the note** per `${CLAUDE_PLUGIN_ROOT}/references/note-formatting.md` and post it via `create_note_for_candidate`. The note-posting tool automatically prefixes every note with the `[AI Screening v1]` marker used by the already-screened check in Step 3 — you don't need to add it yourself, and shouldn't duplicate it.

## After posting

Re-fetch the note (via `list_notes_for_candidate`) rather than trusting whatever the post call itself returns — verifying by re-read is cheap insurance against a silent formatting failure. If you can't confirm the note landed as intended, say so in your batch report instead of assuming success and moving on.

## When the rubric doesn't fit, or something looks off

Not every candidate fits neatly into a rubric written for one role. If a candidate's background doesn't match what the rubric anticipates, the parsed data resists interpretation, or you're genuinely unsure whether a flag applies, surface that in your batch report rather than forcing a score to fit. The rubric exists for consistency across the obvious cases — the edge cases are exactly what the human checkpoint between batches is for.
