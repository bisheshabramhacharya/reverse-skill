---
name: docs-generator
description: Report and documentation generation (route R9). Use to produce security/RE/CTF/teardown reports from a case directory: findings summary, evidence references, diagrams, and handoff-ready markdown. Runs at the end of every case (completion checklist).
---

# Docs / report generation (R9)

## When to use
End of every case (checklist item 1): turn `work/<case>/` into a report.
Also standalone: "write the report / 写报告 / 生成技术文档" for a finished case.

## Workflow (ACTION REQUIRED)
1. Read the case: `scope.md` (meta), `timeline.md`, `workitems.md`,
   `evidence/` (E-*), findings in notes/report drafts.
2. Validate the chain first: `python3 skills/case-review/scripts/review_case.py
   work/<case> --strict` — fix errors before writing the report.
3. Produce `work/<case>/report/FINAL.md`:
   - Executive summary (2-5 lines, non-technical)
   - Scope + authorization basis (one line each)
   - Findings table: F-ID, severity, category, evidence refs, status
   - Per finding: context → evidence → analysis → Path
   - Appendix: repro commands, artifact hashes
4. Anonymize: no real-user PII, no out-of-scope hostnames.
5. Teardown cases: proposal format instead — see `skills/teardown/SKILL.md`
   (report lands in `docs/competitive/proposals/`).
6. Optional diagram: Mermaid flowchart of the chain (evidence→finding→path).

## Boundaries
- Reports state confidence levels; never claim validated without evidence.
