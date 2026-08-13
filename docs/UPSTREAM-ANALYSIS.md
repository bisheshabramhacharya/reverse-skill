# Upstream analysis: zhaoxuya520/reverse-skill

*Teardown performed 2026-08-12 by reverse-skill (our own teardown mode).
Target: [zhaoxuya520/reverse-skill](https://github.com/zhaoxuya520/reverse-skill),
commit `b5ea7fb`, v1.0.1, MIT (CTF dir GPLv3).*

## 1. What it is

A client-neutral "cybersecurity skills router" for AI coding agents. It does not
write code for you; it tells the agent WHICH playbook to follow and WHICH
installed tools to use when the task is APK reverse engineering, binary
analysis, frontend JS encryption, packet capture, CTF, or pentest.

The pitch: AI agents guess commands ("should I use jadx or Frida or IDA?"); this
package replaces guessing with a deterministic route + repeatable workflow.

## 2. Architecture (how it works)

```
task text
  → RULES.md (trigger keywords, bilingual)
  → skills/config/routing.json — single source of truth: 41 rules R0–R40
    (schema: routes{id → {label, skill, keywords[{must,mustAll,exclude}]}}, priority[], fallbackId)
  → skills/scripts/master-route.sh — bash router, embeds a python scorer:
    regex keyword hits → score per route → primary = highest score, ties by priority order
    (evidence: skills/scripts/master-route.sh:1-166, scoring block near line 60)
  → skills/scripts/case-init.sh — creates work/<case>/ with scope.md (auth gate),
    timeline.md, workitems.md (evidence: case-init.sh:1-343)
  → module SKILL.md (the playbook) → tools from tool-index.md (auto-generated)
  → ops/evidence-finding-path.md: Evidence (E-nnn) → Finding (F-nnn) → Path
  → skills/case-review/scripts/review_case.py — stdlib-only python validator
  → docs-generator report + field-journal write-back (experience memory)
```

Key design decisions observed:

- **Routing config is the only route table**; markdown and scripts are generated
  or derived from it (`routing.json` header comment; verify-routing-coherence.ps1).
- **Tool index is machine-specific and gitignored**; refresh scripts scan the
  host and write paths+versions, so "is tool X installed" is never guessed.
- **Authorization is a hard gate**: scope.md must show `auth.status=granted` +
  `network_profile` before any target ACT (ops/scope-contract.md).
- **Windows is the primary platform** (PowerShell scripts, 16 .ps1 vs 10 .sh).
- **Aggressive autonomy engineering**: RULES.md contains an "excuse rebuttal
  table" ordering agents to act immediately, not wait for confirmation, and not
  output safety disclaimers. This is the risky part — safe only inside the scope
  gate above.

## 3. Inventory (what was actually in there)

| Area | Count | Notes |
|------|-------|-------|
| skills/ modules (each a SKILL.md) | 41 | apk-reverse, js-reverse, ida-reverse, radare2, ghidra-reverse, dotnet-reverse, malware-analysis, pentest-tools, pwn-chain, edr-bypass-re, windows-ad, cloud-k8s, firmware-pentest, wifi-wireless, digital-forensics, threat-hunting, api-security, llm-security, protocol-reverse, go-rust-reverse, macos-reverse, … |
| CTF-Sandbox-Orchestrator | 42 | competition-* sub-skills (web/pwn/crypto/forensics/AD/cloud/mobile…) |
| routing rules | 41 (R0–R40) | routing.json, 13.8 KB |
| regression cases | 163 | tests/routing-benchmark.json |
| scripts | 26 total | 16 .ps1 (Windows), 10 .sh (Linux/macOS) |
| MCP bundles | 3 | burp-mcp-full (Java), anything-analyzer, jshookmcp |
| ops contracts | 8 | scope-contract, evidence-finding-path, role-map, timeline-workitem, IDENTITY, sandbox-profile, skill-supply-chain |
| field-journal | ~17 entries | experience write-backs from real jobs |

## 4. What we kept / dropped / rebuilt

### Reused (MIT, cross-platform, tested, valuable) — with attribution

| File | Why |
|------|-----|
| skills/scripts/case-init.sh | Excellent: presets (offline-sample/ctf-public/own-system), scope.md template, timeline+workitems bootstrap |
| skills/scripts/master-route.sh | Fully config-driven, verifies PRIMARY skill exists |
| skills/scripts/refresh-tool-index.sh | macOS-aware, detection-only |
| skills/scripts/case-guard.sh | scope gate check |
| skills/scripts/test-routing.sh | benchmark harness |
| skills/case-review/scripts/review_case.py | stdlib-only evidence validator |

### Dropped (Windows-only / not runnable on this Mac / out of scope)

- ida-reverse (IDA Pro licensed/Windows; we use Ghidra + radare2 → binary-re)
- burp-mcp-full (Java Burp extension), dotnet-reverse (dnSpy Windows-only),
  windows-ad, edr-bypass-re, threat-hunting, ot-ics, radio-sdr, wifi-wireless,
  hardware-security, database-security, email-security, identity-federation,
  thick-client, browser-extension-reverse, cloud-k8s, api-security (folded into
  pentest-tools), llm-security, supply-chain-security, attack-chain
- kali/ (Kali-specific), CTF-Sandbox-Orchestrator (GPLv3 — deliberately not
  included; replaced by a fresh lightweight ctf/ module)
- All 16 .ps1 scripts; README_zh/RULES_zh (bilingual kept in routing keywords only)
- The "excuse rebuttal / anti-laziness" machinery and Windows-centric bootstrap
  (bootstrap-reverse.ps1/sh, 808 lines) — our tool install strategy is simpler:
  Homebrew + module-level hints

### Rebuilt fresh (our content)

- RULES.md, SKILL.md, README.md — leaner contract, two modes (security RE +
  **teardown**), macOS-native
- skills/config/routing.json — 12 rules (R0–R11), our modules only
- 12 module playbooks — concise, macOS commands, authorized-only boundaries
- skills/ops/ — 5 lean contracts adapted from upstream's 8
- docs/UPSTREAM-ANALYSIS.md (this file) + tests/routing-benchmark.json (~30 cases)
- **skills/teardown/** — NEW: competitive open-source teardown playbook (our
  actual primary use case; upstream had no equivalent)

## 5. Verdict

The upstream is a well-engineered routing core buried under 13 MB of
Windows/Kali-specific material we cannot run. Our rebuild keeps the core
machinery (scripts + evidence chain) and replaces the payload with macOS-native
playbooks + the teardown mode. Smaller (≈2 MB), fully documented, honest about
what came from where.

## 6. Evidence

- Upstream clone (at teardown time): `~/.pi/agent/skills/reverse-skill/` (commit
  b5ea7fb); upstream is public at github.com/zhaoxuya520/reverse-skill
- Inventory counts: `find skills -maxdepth 2 -name SKILL.md | wc -l` → 41;
  CTF dir → 42 sub-dirs; routing.json 13,830 bytes; benchmark 163 cases
- Scripts: 16 .ps1 / 10 .sh (repo-wide find)
