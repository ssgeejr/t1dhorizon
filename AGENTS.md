# AGENTS.md

## Project

Build a **Type 1 Diabetes Patient Education Handout Generator** — a small,
self-contained tool that turns a topic + reading level into a printable
patient handout.

Primary user: a nurse practitioner in an outpatient clinic who explains the
same five things to patients every week and wants a consistent, plain-language
handout to send home.

Secondary purpose: this artifact doubles as a **training example** for hospital
staff on safe AI use. It must demonstrate, by construction, that a genuinely
useful clinical-adjacent tool can be built with **zero protected health
information**.

---

## Hard constraints — non-negotiable

These are not style preferences. Violating any of them makes the artifact
unusable for its purpose.

1. **No PHI. Ever.** No patient names, dates of birth, MRNs, account numbers,
   addresses, phone numbers, dates of service, or any of the 18 HIPAA
   identifiers — not in code, not in comments, not in test fixtures, not in
   sample output, not in placeholder strings.
2. **No patient-specific output.** The tool produces *general education
   material about a condition*. It never produces advice about, or a summary
   of, an individual patient. If a feature would require patient data to work,
   do not build it.
3. **No synthetic records either.** Do not generate realistic-looking fake
   patient records as examples or fixtures. Record-shaped data teaches the
   wrong reflex to the audience this is meant to train. Use topic strings, not
   charts.
4. **No clinical decision support.** No dosing calculators, no insulin
   titration math, no "should this patient…" logic. Education only.
5. **No network calls to third-party services** at runtime. The tool must run
   offline once built. No API keys, no telemetry, no analytics, no CDN fonts.
6. **No data persistence.** Nothing the user types is written to disk, logged,
   or cached. Session-only, in memory.

If a requested feature conflicts with any of the above, stop and say so rather
than finding a workaround.

---

## Clinical grounding

Content must be consistent with **public, citable clinical guidance**:

- ADA *Standards of Care in Diabetes* (current edition)
- ADA / AADE patient education materials
- CDC diabetes education resources

Rules:

- Every handout ends with a plain "Where this comes from" line naming the
  guideline source in general terms.
- Do **not** fabricate specific numeric targets, thresholds, or ranges. If a
  number is needed and not verifiable from the sources above, write a
  placeholder like `[your care team will give you your target range]` instead
  of inventing one.
- Every handout ends with: "This handout is general information. Follow the
  plan your care team gave you."

---

## Starting topic set

Build these five first. They are the highest-frequency explanations:

1. Carbohydrate counting — the basics
2. Sick-day rules
3. Recognizing and treating low blood sugar
4. Continuous glucose monitor (CGM) basics
5. Managing blood sugar around exercise

Topics are a configurable list, not hardcoded logic. Adding a sixth should be
a one-line change.

---

## Output requirements

Each generated handout:

- **One page.** If it doesn't fit on one printed page, it's too long.
- **Reading level selectable:** 5th grade (default) / 8th grade / clinician-facing.
  Default to 5th grade — it is the right default for patient handouts.
- **Structure:** title → one-sentence "what this is about" → 3–6 short sections
  with bold headers → a "call your care team if…" box → the source line and
  the general-information line.
- **Plain language.** Short sentences. Second person ("you"). Define any term a
  patient wouldn't know, in-line, the first time it appears.
- **Print-clean.** Black on white, no background colors, no decorative
  graphics, generous margins, body text no smaller than 11pt equivalent.
- **Copy and print** are the only two export actions. No download-to-cloud, no
  share links.

---

## Technical shape

- Single self-contained HTML file. Inline CSS and JS. No build step, no
  package manager, no framework.
- Runs by double-clicking the file. Must work with no internet connection.
- Responsive enough to be usable on a tablet, but **print layout is the
  priority** — this gets handed to a patient on paper.
- System font stack. No web fonts.
- Content for the five starting topics is authored as structured data inside
  the file, so handouts are assembled deterministically rather than generated
  fresh each time. Same input, same output, every time — that matters for a
  clinical setting.

---

## Interface

Keep it to one screen:

- Topic selector (the five topics)
- Reading-level selector
- "Build handout" button
- Preview pane showing the finished handout exactly as it will print
- Copy / Print

No accounts, no settings page, no onboarding, no tour.

---

## Tone and voice

- Warm, direct, non-alarming. Patients reading these may be newly diagnosed.
- Never scolding. No "you must" or "failure to." Use "it helps to" and "try to."
- Do not imply blame for high or low readings.
- Avoid the word "diabetic" as a noun. Say "people with diabetes."

---

## What this project is not

Do not add, propose, or scaffold:

- Any connector, integration, or plugin to email, cloud storage, calendars, or
  an EHR
- Any browser automation or agentic browsing
- Any login, user account, or identity feature
- Any logging, analytics, or usage tracking
- Any data upload of any kind

The absence of all of the above is the point of the demonstration.

---

## Working style

- Ship a working single file early, then refine. Prefer a rough end-to-end
  version over a polished fragment.
- Make surgical edits. Do not rewrite working sections to restructure them.
- When a requirement here is ambiguous, choose the more conservative reading
  and say which reading you took.
- Flag anything that looks like it drifts toward patient-specific output, even
  if it seems harmless.
