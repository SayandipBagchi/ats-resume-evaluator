# ATS Resume Evaluator

A Claude skill that scores a resume for applicant tracking system (ATS) readiness, explains every point it takes off, and rewrites the resume as an ATS-safe file, applying only the recommendations you accept.

It reads the file the way a parser does. Then it scores 31 checks with fixed thresholds, so the same resume gets the same score, and quotes your own text for every finding. It never invents a metric, title, date or skill.

## What you get

- **ATS Score (0 to 100)** across five dimensions:

  | Dimension | Points |
  | --- | --- |
  | Parsability and ATS essentials | 25 |
  | Keywords and relevance | 30 |
  | Impact and content | 25 |
  | Brevity, style and language | 10 |
  | Sections and completeness | 10 |

- **JD Match Rate** when you supply a job description. It is weighted toward hard skills, then job title, then competencies.
- **Parse preview:** the exact text an ATS extracts from your file, with lost or scrambled parts called out. This catches two-column layouts, metric tiles, contact details in headers, hidden characters and link-only LinkedIn labels.
- **Numbered recommendations** with estimated point lift and effort.
- **Rewrite on acceptance:** reply `all` or a list like `R1, R3`. You get a single-column .docx (or PDF), re-scored to show before and after, with a change log.

## Score bands

| Score | Band |
| --- | --- |
| 0–59 | At risk |
| 60–79 | Needs work |
| 80–89 | Strong |
| 90–100 | Industry grade |

## Three ways to run it

| Mode | You give | Keywords scored against |
| --- | --- | --- |
| A | Resume + job description | Units extracted from the JD |
| B | Resume + role + seniority | A fixed baseline for the role (APM/PM, Senior PM to VP, Full Stack, Backend, Data Analyst, or one built for any other role) |
| C | Resume only | Role and seniority inferred and stated for you to correct |

When the skill starts, it asks once for any of: JD, target role, seniority, target market, and output format. Every item is optional.

## Install

**Claude (web, desktop, mobile):** zip the `ats-resume-evaluator` folder and upload it under Settings → Capabilities → Skills.

**Claude Code:** copy the folder into `~/.claude/skills/` (personal) or `.claude/skills/` in a project.

```
ats-resume-evaluator/
└── SKILL.md
```

The skill is a single file. Its text extractor is embedded in SKILL.md and written out at run time.

**Runtime needs:** Python 3, `pdftotext` (poppler) for PDFs and `python-docx` for Word files. `pdfplumber` is optional and used to read link targets.

## Use

Upload a resume and ask, for example:

- "What's the ATS score of my resume?"
- "Check this resume against this JD" (and paste the JD)
- "Score my resume for a senior full stack engineer role"
- "Make my resume ATS friendly"

## How it was built and tested

- **Benchmarked** against how seven resume checkers score and report: Jobscan, Resume Worded, Enhancv, Rezi, Teal, SharpenResume and 1MillionResume. From them it adopts:
  - separate readiness and match scores
  - parse rate as a first-class signal
  - hard skills weighted above soft skills
  - concrete thresholds
  - honest framing: no single "ATS score" exists across employers
- **Consistency:** the same resume was scored by independent runs. The first draft rubric spread 15 points across runs. After the rules were tightened (fixed role baselines, numeric Pass/Partial bands, one penalty per defect), three runs landed within 1 point of each other.
- **Rewrite flow:** a test resume went from 78 (Needs work) to 90 (Industry grade) on three accepted recommendations, with no facts added or lost.
- **Real-world dry run:** four resumes across product and engineering roles. It surfaced parsing problems that are invisible on the page: detached dates, scrambled skill grids, footers injected mid-section, zero-width characters and link-only LinkedIn labels.

## Guardrails

- It never invents or inflates metrics, titles, employers, dates, degrees or certifications. Unknown numbers stay as visible `[placeholders]`.
- It never adds a skill you don't have because a JD asks for it. That is reported as a genuine gap, with a truthful route to close it.
- It doesn't use hidden-text or keyword-stuffing tricks.
- It doesn't infer or advise on protected characteristics or career breaks.
- It makes no promises of ranking, percentile or interviews. The score is a readiness guide; employers' ATSs parse and rank differently.

## Related

It pairs with any skill for deeper bullet rewrites, LinkedIn or README work, or removing AI-sounding prose. The skill routes those requests out rather than handling them itself.

## License

MIT. See [LICENSE](LICENSE).
