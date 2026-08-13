# Evidence → Finding → Path (the chain)

Every conclusion must be traceable to evidence. Markdown field contract —
readable by humans, validated by `skills/case-review/scripts/review_case.py`.

## 1. Evidence (immutable observation)

```markdown
### E-{nnn}
- title:
- observed_at:
- source_type: command | screenshot | file | log | memory | network | manual
- source_ref: {path or command id}
- content_hash: {sha256 of artifact, or n/a}
- artifact_path: {relative path under case root, or n/a}
- repro_command: |
    {exact command}
- raw_excerpt: |
    {anonymized excerpt}
- linked_workitem: WI-{nnn} | n/a
- supersedes: E-{nnn} | none
```

**Teardown mode**: evidence = `file:line` citations (or commit hash) in the
target repo, plus a short quote. `repro_command` = the read/grep command used.

## 2. Finding (conclusion)

```markdown
### F-{nnn}
- title:
- severity: critical | high | medium | low | info | n/a_re
- category: vuln | misconfig | design | reverse_algo | bypass | teardown | other
- status: candidate | validated | false_positive | accepted_risk
- evidence: [E-{nnn}, ...]        # MUST have ≥1
- confidence: high | medium | low
- notes:
```

## 3. Path (what we do next)

```markdown
### P-{nnn}
- finding: F-{nnn}
- action: what WE build/fix/avoid (teardown) or how to exploit/verify (security)
- files: [paths in OUR repo when teardown]
- risks: []
- effort: s | m | l
```

## Chain rules

- Finding without evidence = opinion, not finding. Fix or mark candidate.
- `review_case.py` is the read-only validator:

```bash
python3 skills/case-review/scripts/review_case.py work/<case> --verify-hashes --strict
```

- Hash fixity: when an evidence artifact is a file, record sha256 so tampering
  is detectable.
- Anonymize: no real-user PII, no real target hostnames in reports/journals.
