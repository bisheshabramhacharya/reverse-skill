# reverse-skill

Cybersecurity reverse-engineering router **and** competitive open-source teardown
methodology — our own rebuild of [`zhaoxuya520/reverse-skill`](https://github.com/zhaoxuya520/reverse-skill) (MIT),
sized for this macOS machine and for how we actually work.

> **For AI agents:** read `SKILL.md` and `RULES.md` first. Route before you act.

## What it is

When an AI agent encounters an APK, a binary, frontend JS encryption, a CTF
challenge, or a competitor's repo, this package routes it to the right playbook
instead of guessing commands:

```
task → RULES.md → master-route.sh (routing.json = single source of truth)
     → case-init.sh → scope.md (authorization gate)
     → module SKILL.md → tools (tool-index.md) → execute
     → timeline + Evidence→Finding→Path → report + field-journal
```

## Two modes

1. **Security RE / pentest / CTF** (authorized targets only) — APK, JS, binary,
   malware, pentest, pwn, firmware, CTF playbooks.
2. **Competitive open-source teardown** — read-only analysis of software we did
   not write: evidence with `file:line` citations → Finding → Path (what WE
   should build) → proposal docs under `docs/competitive/proposals/`.

See `SKILL.md` for the full mode descriptions.

## Layout

```
SKILL.md                  pi skill entry (two modes)
RULES.md                  routing contract (keywords, gates, checklist)
docs/UPSTREAM-ANALYSIS.md teardown of the upstream package (what we kept/dropped/rebuilt)
skills/
  MASTER-ROUTING.md       priority routing table (R0–R11)
  config/routing.json     routing single source of truth (master-route.sh reads this)
  scripts/                case-init.sh, master-route.sh, refresh-tool-index.sh,
                          case-guard.sh, test-routing.sh (reused from upstream, MIT)
  ops/                    scope contract, evidence chain, roles, timeline
  case-review/            review_case.py — validates case evidence graph (stdlib-only)
  field-journal/          experience write-back (template + index)
  <module>/SKILL.md       12 playbooks (apk, js, binary, malware, pentest, pwn,
                          firmware, ctf, docs, case-review, teardown, general RE)
tests/routing-benchmark.json  routing regression cases (test-routing.sh)
```

## Quickstart

```bash
git clone <our repo> && cd reverse-skill
bash skills/scripts/refresh-tool-index.sh   # one-time: scans local tools → tool-index.md
bash skills/scripts/test-routing.sh         # one-time: routing benchmark must pass

# new task:
bash skills/scripts/master-route.sh --hint "analyze this APK"
bash skills/scripts/case-init.sh --hint "analyze this APK" --preset offline-sample --sample ./app.apk
```

`case-init` presets: `offline-sample` (local files), `ctf-public` (CTF platform),
`own-system` (your own lab/hosts). Any other target requires explicit auth in
`work/<case>/scope.md` — **never act without it**.

## Attribution & license

- MIT licensed (ours), see `LICENSE`.
- Adapted from [zhaoxuya520/reverse-skill](https://github.com/zhaoxuya520/reverse-skill)
  (MIT, © zhaoxuya520): the five `skills/scripts/*.sh` scripts and
  `skills/case-review/scripts/review_case.py` are reused with attribution.
  Everything else (modules, routing rules, docs, ops contracts) is our own work.
- The upstream CTF-Sandbox-Orchestrator (GPLv3) is **not** included; our `ctf/`
  module is a fresh, lightweight playbook.

## Boundaries

Authorized security work only: your own systems, CTF/lab environments, or
written engagement scope. Unauthorized testing of other people's systems is
illegal — the author takes no responsibility for misuse.
