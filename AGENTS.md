# AI conventions

## About this repository
Public portfolio repository of Athena Chew, a Business Administration student
(B.B.A. in Management & International Business, Shidler College of Business,
University of Hawaii at Manoa, May 2027), built for a business course.
Canonical file: AGENTS.md. CLAUDE.md points here.

## My field
Business administration: management and international business. My work
background is guest operations (cruise port check-in and passenger flow), food
service, and community program volunteering. My coursework so far covers
business statistics, business calculus, business law, information systems, and
global management. I work in Excel, Google Sheets, Word, PowerPoint, and Google
Docs/Slides on a MacBook.

## Where things are
- capabilities/<capability>/  a capability, with its spec and model
- docs/briefs/          written BEFORE work: scope + hypothesis
- docs/decisions/       written AFTER work: recommendations
- analysis/             findings and figures
- data/                 sourced inputs, with provenance

## Naming
- The directory matters most. A file in the wrong folder may not be found
  at all. If you are not certain which folder a file belongs in, ask me
  before you write it — do not choose for me.
- Graded files use the exact filename the stage brief gives — lowercase,
  hyphens, no spaces. Some courses date-stamp (YYYY-MM-DD-lastname-slug.md);
  the stage page says so when they do.
- Slugs name the engagement, never the week, the course, or the assignment
  number.
- Never invent a path or a filename. I will give you the exact one.

## How I work
- Explain concepts fully and walk the worked example with real numbers. Do not
  hand me conclusions.
- Define every business, statistics, or calculus term the first time you use it,
  in plain language.
- When a calculation is involved, show the steps in Excel or Google Sheets on a
  Mac (which cells, which formula) rather than in code.
- Use examples from operations (queues, check-in, service flow, program
  logistics) when they fit.
- Critique my reasoning directly. I would rather be corrected than agreed with.
- When you are uncertain, say so and say what would resolve it.

## What you may and may not draft
- You MAY explain, critique, debug, quiz me, and draft mechanical files.
- You MAY NOT write my briefs, analyses, memos, or reflections.
- Every statistic or figure you give me is a draft until I verify it against a source.

## Documentation
When work changes, update the document that describes it in the same commit.
A capability's README names the engagements that exercised it — keep that current.

## Scope
Do the work I asked for. If you notice something worth doing that I did not ask
for, tell me instead of doing it.

## Commits
Descriptive messages: what changed and why. Never "update" or "stuff".

## Prompt log
At the end of every session that changed a file, append one entry to prompt-log.md: the date, what I asked, what you produced, what was wrong and how it was caught. Never backfill earlier sessions and never edit a past entry.

## Data that must never be pasted into a model or committed
If a document would not be safe in a public repository, it is not safe in a
chat window either. From my work and activities, that means:

Port work (MC&A, cruise terminal):
- Passenger or guest names, booking or reservation numbers, cabin numbers,
  boarding passes, check-in lists, and passenger manifests.
- Passport, ID, visa, or other travel-document details, and any health,
  mobility, or accessibility information about a guest.
- Employer or cruise-line operating information: sailing and terminal
  schedules, staff rosters and shift schedules, security, access, and arrival
  procedures, internal emails, and anything marked confidential.

Restaurant work (England Rose Garden):
- Customer names, reservations, contact details, and payment card data.
- Staff schedules, pay, and tip records, and the restaurant's internal
  recipes, prep procedures, and non-public pricing.

Community programs (backpack drive, homework club, food packing drive):
- Names, grades, schoolwork, or behavior notes of any child in the homework
  club, and school staff contact details.
- Names, addresses, and family circumstances of anyone who received backpacks
  or food; donor names, contact details, and donation amounts; any internal
  records of the non-profits involved.

University:
- Student ID numbers, grades, transcripts, and advising records; classmates'
  names or work; licensed material such as textbook chapters, publisher slides,
  and paid datasets.

Personal:
- Passwords, API keys, tokens, Social Security or government ID numbers, and
  bank or card details.

If an exercise needs data like this, use public, invented, or fully anonymized
aggregate data instead. If I paste something that fits these lists, stop and
tell me rather than continuing or committing it.

## Mistakes to avoid (append to this list)
Record errors here as they happen, so the same one does not repeat.
- (empty — add the first one when it happens)
