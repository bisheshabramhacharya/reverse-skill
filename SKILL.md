---
name: reverse-skill
description: Cybersecurity reverse-engineering skill router (APK / JS / binary / malware / pentest / CTF playbooks) AND competitive open-source teardown methodology. Two modes: (1) security RE — routes tasks to the right playbook, uses only installed tools (tool-index), case-init scope gate before any target action, Evidence→Finding→Path chain, report + journal. (2) teardown — for analyzing software we did not write (competitor repos, upstream packages): case-init (scope) → methodology → evidence with file:line citations → Finding → Path (what WE should build) → proposal doc. Use when the owner asks to reverse / tear down / research any software, or for authorized security analysis.
---

# reverse-skill

Our own rebuild of the upstream `zhaoxuya520/reverse-skill` routing package (MIT),
adapted for this macOS machine and for how we actually work.

## Two modes

### Mode 1 — Security reverse engineering / pentest / CTF (authorized only)

```
task → RULES.md → MASTER-ROUTING.md (or bash skills/scripts/master-route.sh --hint "...")
     → case-init.sh → work/<case>/scope.md (auth gate: MUST NOT ACT until auth.status=granted)
     → PRIMARY module SKILL.md → tools from tool-index.md (never guess paths)
     → timeline + Evidence→Finding→Path → report + anonymized field-journal
```

- Works on: APK (jadx/apktool/Frida), frontend JS, binaries (radare2/Ghidra),
  malware triage (YARA), authorized pentest tooling, pwn, firmware, CTF.
- Windows-only upstream modules (IDA Pro, Burp MCP, dnSpy, EDR bypass, AD…) are
  intentionally NOT ported — see `docs/UPSTREAM-ANALYSIS.md`.

### Mode 2 — Competitive teardown → REBUILD (our primary mode, owner default)

The same workflow shape, applied to competitor / upstream code — and it does
NOT stop at advice: it ends with a real build.

- **case-init**: declare the target repo + the feature area under study
- **scope**: read-only analysis of the target; our rebuild lives in
  `/Users/bishesha/projects/<name>`
- **methodology**: read the module that maps to the feature area (their router,
  renderer, pipeline), not the whole repo
- **evidence**: every Finding cites `file:line` or a commit hash
- **Finding → Path**: each finding ends with a concrete Path — what WE should
  build, which files, which risks, why it fits our needs
- **BUILD**: execute the Paths — scaffold, implement, run the check, one
  independent review round (see `loop` skill), fix, commit, push to a private
  repo. Proposal-only is opt-in, never the default.

Security modules (Frida/jadx/Burp/CTF) are irrelevant for mode 2 — do not
suggest them for readable open-source code analysis.

## First-run setup (one time, per machine)

```bash
bash skills/scripts/refresh-tool-index.sh   # scans installed tools → tool-index.md (gitignored)
bash skills/scripts/test-routing.sh         # runs the routing benchmark (must pass)
```

## Non-negotiable

1. No target action (scan / hook / exploit / modify) without `work/<case>/scope.md`
   with `auth.status=granted` + `network_profile` — see `skills/ops/scope-contract.md`.
2. Tool paths come ONLY from `tool-index.md`. Missing tool → tell the owner, install,
   re-run refresh-tool-index. Never guess.
3. Findings cite evidence (`file:line` for teardown; command/artifact for security).
4. Finish with report + anonymized `field-journal` write-back.
