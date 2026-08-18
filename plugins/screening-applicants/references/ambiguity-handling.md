# Ambiguity Handling

This file collects the judgment calls this skill makes when the data isn't clean or the rubric doesn't cleanly apply. The general principle: **widen the assessment and say so, rather than silently resolving ambiguity in whichever direction is easiest to score.**

## Parsed data looks wrong

Recruitee's own CV parsing is not always reliable — fields can contain truncated text, garbled fragments, or content that clearly belongs somewhere else (an unrelated sentence landing in an `education.major` field, for example). When you see this:

- **Don't silently score around it.** Treating garbled data as if it simply means "no experience" or "no education" understates the candidate based on a parsing failure, not their actual background.
- **Raise the parsing-issue flag** the rubric defines (see `references/rubric-schema.md`), naming specifically what looked wrong, so the recruiter knows to check the original application rather than trust the note at face value.

## Unexplained employment gaps

A gap in the parsed work history could mean many things — a genuine gap, a parsing artifact that dropped a role, or a job that didn't fit the field's expected format. Don't default to treating a gap as evidence of *absence* of experience. Note the gap factually as part of the relevant criterion's evidence, and let the rubric's own scoring rules (not an assumption) determine whether it affects the score.

## A rubric criterion asks about something the parsed data doesn't mention

If a rubric criterion effectively requires a lookup outside the CV itself — for example, judging whether a named employer operates in a relevant domain — do that lookup rather than defaulting to "no evidence" because the parsed fields are silent on it. Silence in the candidate's own data isn't the same question as "does this fact exist," and conflating the two understates candidates whose CVs just didn't happen to spell out something true.

## A candidate doesn't fit the rubric's assumptions at all

Not every candidate fits neatly into a rubric written with a typical candidate in mind — a career-changer, someone with an unconventional background, or a profile the rubric's author didn't anticipate. When this happens:

- Don't force a score that technically follows the rubric's mechanics but misrepresents the person.
- Surface the mismatch explicitly in the batch report (see `SKILL.md`, the batching step) rather than quietly picking whichever interpretation is easiest.

## The underlying rule

Every case above is the same shape: when a fact is missing, unclear, or doesn't fit expectations, that's a signal to *say so* — as a flag, a batch-report note, or an explicit statement in the evidence text — not a gap to be filled in with the most convenient assumption. The rubric exists to make scoring consistent across the clear cases; ambiguity is exactly what the human checkpoint between batches exists to catch.
