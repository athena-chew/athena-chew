# Prompt log

## 2026-09-30

- **Asked:** Set up the athena-chew portfolio repository: folder skeleton, a tailored AGENTS.md based on the course baseline, CLAUDE.md pointer, prompt-log.md, RESUME.md from my resume, a placeholder README.md, and .gitignore, all shown for review before any commit.
- **Produced:** README.md, RESUME.md, AGENTS.md, CLAUDE.md, prompt-log.md, .gitignore, and one-line README.md files in capabilities/, docs/briefs/, docs/decisions/, data/, and analysis/figures/.
- **What was wrong and how it was caught:** (1) The first two requests arrived without the resume text; the upload folder was checked, found empty, and nothing was drafted until the resume was pasted. (2) The "never paste into a model" categories and the "How I work" section were inferred from job titles and coursework, not from my own statements; this was flagged at the time and left for me to correct before committing. (3) The request said graduate course but the resume lists a B.B.A. (May 2027); the files say "a business course". (4) The resume includes my phone number, email, and permanent address, which would be public in this repository; this was flagged during review and left as my decision.
