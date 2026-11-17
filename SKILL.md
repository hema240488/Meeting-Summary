---
name: meeting-summary
description: Turn a meeting transcript (Teams .vtt, .txt, .docx, or pasted text) into meeting minutes (MOM). Use when the user invokes /meeting-summary, asks for minutes, MOM, meeting notes, action items, decisions, or a summary of a meeting transcript. Every item is traced back to its place in the transcript.
---

# /meeting-summary

Produce meeting minutes (MOM) from a transcript. Every item must say what it is and point to where it came from in the transcript.

## Inputs

- The transcript: a Teams `.vtt` file (WEBVTT with `<v Speaker>` cue tags), a plain-text transcript, or pasted text.
- Optional: the user's name or team, if they want their own items highlighted.

If the transcript is missing or unreadable, say so and ask for it. Do not invent content.

## Steps

1. Read the whole transcript before writing anything. For `.vtt` files, keep the cue timestamps (`HH:MM:SS.mmm`) and speaker names, since they are the citations.
2. Read the header block (`Meeting:`, `Date:`, `Duration:`, `Participants:`). Use the header date to resolve relative deadlines such as "by Friday", "by Monday", "next week", or "Tuesday". State the resolved date and keep the original wording in brackets, for example "Friday, 20 Nov 2026 (said: 'by Friday')". If no date can be resolved, write "Not stated".
3. Extract each section below. Attach a source to every item in the form `[HH:MM:SS, Speaker]`, or a range `[HH:MM:SS–HH:MM:SS]` when a point spans several cues.
4. Check for internal inconsistencies, such as a header date that does not match dates mentioned in the discussion, a participant who speaks but is not listed, or a deadline that conflicts with another. List these under "Flags for review".
5. If the transcript contains personal identifiers that the organization forbids displaying (SSN, Aadhaar, PAN, passport or driver's licence numbers, credit card numbers, patient records), redact them as `[REDACTED]` and note that a redaction was made. Do not repeat them anywhere in the output.

## Sections to produce

**1. Attendees.** List every participant named in the header or who speaks in the transcript. Give each person's role only if it is stated in the meeting. Mark anyone who is listed but never speaks, or who speaks but is not listed, under "Flags for review".

**2. Agenda.** List the topics in the order they were discussed. Give each topic a short label and the timestamp where it starts.

**3. Decisions.** Record only what was actually agreed or settled. For each one, give the decision, who proposed or confirmed it, and the source. Separate decisions from proposals that were not yet agreed; put those under "Proposed, not decided" with the source.

**4. Action items.** Use a table with these columns: Action, Owner, Deadline, Source. Owner and deadline must come from the transcript. If either is missing, write "Not assigned" or "Not stated". Do not guess an owner from context. If someone says they will "look into" something without a deadline, record it with "Not stated" and flag it.

**5. Risks.** Record concerns, dependencies, or blockers raised in the meeting. Mark each as "Raised by [Speaker, timestamp]". Do not add risks the transcript does not mention. If a risk is clearly implied but not named, you may add it under "Possible risks (inferred)" and say what it is inferred from.

**6. Open questions.** List questions that were raised but not answered, or that were deferred. Include who was asked to answer, if the transcript says so, and the source.

**7. Follow-ups.** List the next meeting date and time, any scheduled reviews, and any items that were parked for a later meeting. Include the source.

**8. Flags for review.** List inconsistencies, missing owners or deadlines, unclear items, and redactions. If there are none, say "None".

## Output format

Use this order: a title line with the meeting name, date, and duration; then the sections above, numbered as shown. Use tables for action items and attendees. Use short paragraphs or bullets elsewhere. Keep each item to one or two sentences. Do not add a summary paragraph at the top unless the user asks for one.

Produce the minutes as a Markdown file named `meeting-minutes-<YYYY-MM-DD>.md` in `/mnt/user-data/outputs/`, unless the user asks for a Word or PDF file. Also give the full minutes in the reply, or a short version if the minutes are long, with the file for the rest.

## Rules

- Use only what the transcript says. Mark any inference explicitly.
- Quote short phrases when the exact wording matters, such as a commitment or a number.
- Do not turn "we should consider" into a decision, or "I might" into an action item. Put them under proposals or open questions.
- Do not include personal data beyond names and the content the meeting itself discussed.
- If the transcript is too short or too unclear to produce a section, write "Not enough information in the transcript" for that section and say why.
