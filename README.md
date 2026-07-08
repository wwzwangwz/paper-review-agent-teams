# Paper Review Agent Teams

Codex skill for venue-aware, multi-agent academic paper review. It is designed for LaTeX/PDF manuscripts, supplementary material, rebuttal drafts, and writing-only revision passes.

## What It Does

This skill reviews a paper with independent agent teams instead of a single monolithic pass. For every full review, it first asks for the target conference or journal. If no venue is specified, it defaults to CVPR.

The required review teams are:

- Writing/Narrative Team: abstract, introduction, contributions, related work, conclusion, author voice, and reviewer readability.
- Method/Novelty Team: problem framing, technical novelty, assumptions, mechanism clarity, limitations, and relation to prior work.
- Experiment/Protocol Team: claims versus evidence, tables, figures, ablations, baselines, reproducibility, and supplement consistency.

For high-stakes acceptance estimates or conflicting reviews, the workflow can add an Area-Chair Synthesizer team.

## Installation

Clone this repository into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/wwzwangwz/paper-review-agent-teams.git ~/.codex/skills/paper-review-agent-teams
```

Restart Codex or reload skills after installation if your environment requires it.

## Usage

Example prompt:

```text
Use paper-review-agent-teams to review my paper. Target venue: AAAI. Review the main PDF, LaTeX source, figures, tables, and supplement, then give a neutral acceptance-risk assessment.
```

If you do not specify a venue, the skill must ask:

```text
Target venue/journal? I will default to CVPR if you do not specify one.
```

You can also provide only a repository or manuscript folder:

```text
Use paper-review-agent-teams to review the paper in this folder. Default to CVPR if I do not specify a venue.
```

## Expected Output

A review-only run should report:

- Target venue and assumptions.
- Overall verdict.
- Venue-style scores when possible.
- Strengths.
- Major concerns with evidence.
- Minor concerns with evidence.
- Writing and narrative issues.
- Table, figure, and supplement consistency issues.
- Ranked revision plan.
- Borderline or acceptance-risk assessment.

For revision requests, the skill should summarize planned edits first, then edit only the authorized writing, layout, or presentation surface.

## Guardrails

- Do not invent experiments, numbers, citations, or claims.
- Do not change data, figure contents, or numeric results unless explicitly authorized.
- Treat cited baselines as published comparisons by default.
- Remove credibility-damaging wording such as "not locally rerun" when it is not necessary.
- Preserve anonymity in anonymous submission drafts. Do not add acknowledgments, funding numbers, author names, affiliations, or identifiable repository links before anonymity restrictions are lifted.
- Map screenshot comments back to the English LaTeX source before editing.

## Repository Layout

```text
paper-review-agent-teams/
|-- SKILL.md
|-- agents/
|   `-- openai.yaml
`-- references/
    `-- venue-scorecards.md
```

## Notes

This is a Codex skill, so it depends on a Codex environment that can load local or GitHub-hosted skills. The full workflow requires subagent spawning. If subagents are unavailable, the skill should state that the full review workflow cannot be satisfied in that environment.
