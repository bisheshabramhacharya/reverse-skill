# Timeline & work items

## timeline.md (append-only)

```markdown
## {ISO-8601} | {role} | {phase}
- action: {what was done}
- command_or_ref: {command or file}
- result_summary: {1 line}
- artifacts: [paths]
- evidence_ids: [E-{nnn}]
- next: {what happens next}
```

## workitems.md

| ID | title | role | targets | surface | status | evidence | notes |

Status flow: `in_progress` → `done` (needs evidence refs) | `blocked` | `cancelled`.

## Coverage checklist (before calling a case complete)

- [ ] Recon/analysis complete for in_scope assets
- [ ] Findings validated with Evidence (E-* or file:line)
- [ ] Path documented per finding
- [ ] Timeline continuous across phases
- [ ] Report generated (docs-generator or teardown proposal)
- [ ] field-journal write-back (anonymized)
