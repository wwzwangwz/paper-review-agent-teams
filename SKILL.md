---
name: paper-review-agent-teams
description: Venue-aware multi-agent academic paper review workflow for LaTeX/PDF manuscripts, supplements, rebuttal drafts, and writing-only revisions. Use when asked to review, pre-review, score, critique, or improve an academic paper with conference/journal standards; must ask for target venue first with CVPR as default and must use a subagent review cluster.
---

# Paper Review Agent Teams

## Core Rule

Before reviewing, ask for the target conference or journal if it is not already specified. State that the default is CVPR. If the user declines to choose or asks to proceed, use CVPR.

Use an actual subagent cluster for every full review. If subagent spawning is unavailable, state that the review cannot satisfy this skill's full workflow and ask to run in an environment with subagents.

## Workflow

1. Identify the manuscript artifacts: main PDF, LaTeX source, supplement, figures, tables, appendix, and any author constraints.
2. Ask the venue/journal question:
   `Target venue/journal? I will default to CVPR if you do not specify one.`
3. Load `references/venue-scorecards.md` when the venue is CVPR, AAAI, NeurIPS, ICLR, ICCV/ECCV, ACL/EMNLP, or when scoring criteria are needed.
4. Spawn at least three independent subagents:
   - Writing/Narrative Team: abstract, introduction, contributions, related work positioning, conclusion, author voice, reviewer readability.
   - Method/Novelty Team: problem framing, technical novelty, mechanism clarity, assumptions, limitations, relation to prior work.
   - Experiment/Protocol Team: tables, figures, claims versus evidence, baseline comparability, ablations, reproducibility, supplement consistency.
   Add a fourth Area-Chair Synthesizer subagent for high-stakes acceptance predictions or conflicting reviews.
5. Give each subagent only the venue, artifacts, and review brief. Ask for file/line or page/table evidence. Do not tell subagents the expected conclusion.
6. Synthesize the reviews neutrally. Do not average scores blindly; explain disagreements.
7. If the user requests editing, modify only writing, LaTeX layout, or presentation unless explicitly authorized to change data, numbers, figures, or experimental results.

## Review Standards

Use venue-specific standards, but always assess:

- Clear problem statement and motivation.
- Novelty beyond combining known components.
- Technical soundness and whether assumptions are stated.
- Experimental support for each claim.
- Fairness and clarity of comparisons without defensive wording.
- Whether ablations isolate the proposed components.
- Consistency between text, tables, figures, and supplement.
- Reproducibility details without exposing anonymous identity.
- Author voice: avoid third-party phrasing such as "the framework", "this supplement", "these results show" when "SARCoP", "we", or "our results" is more natural.

## Output Format

For review-only requests, produce:

- Target venue and default assumptions.
- One-paragraph overall verdict.
- Scores in the target venue's style when possible.
- Strengths.
- Major concerns with evidence.
- Minor concerns with evidence.
- Writing/narrative issues.
- Table/figure/supplement consistency issues.
- Actionable revision plan ranked by expected impact.
- Borderline/acceptance risk assessment.

For revision requests, first summarize planned edits, then implement, compile if LaTeX is involved, and report changed files and validation.

## Guardrails

- Do not invent experiments, numbers, citations, or claims.
- Do not weaken necessary protocol statements merely to sound confident.
- Do remove credibility-damaging phrasing such as "not locally rerun" unless the user explicitly wants it.
- Treat cited baselines as published comparisons by default; do not imply unfairness unless the manuscript explicitly supports it.
- Preserve anonymity in submission drafts: do not add acknowledgments, grant numbers, author names, affiliations, or identifiable repository links.
- When reviewing translated screenshots, map the issue back to the English LaTeX source before editing.
