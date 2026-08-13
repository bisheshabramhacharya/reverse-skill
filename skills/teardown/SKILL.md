---
name: teardown
description: Competitive open-source teardown methodology (route R11) — our primary mode. Use when analyzing ANY software we did not write (competitor repos, upstream packages, open-source projects) to understand exactly how it works so we can build our own version — as good or better, never sloppy. Read-only: evidence = file:line citations, every Finding ends in a Path (what WE build), reports land in docs/competitive/proposals/.
---

# Competitive teardown (R11) — our primary mode

## When to use
"Reverse engineer this repo", "tear down X so we can rebuild it", "how does
competitor Y do Z", "analyze this open-source package". Read-only analysis of
software we did not write. This is the mode this skill was rebuilt for.

## Principles
1. **Read-only**: we never modify the target repo. Scope = read/analyze only.
2. **Feature-area first**: read the module that maps to the feature area under
   study (their router, renderer, pipeline, captions, config), NOT the whole
   repo. Whole-repo reads waste context and produce shallow output.
3. **Evidence is file:line**: every Finding cites `path/file.ext:line` or a
   commit hash — never "I recall".
4. **Finding → Path**: every Finding ends with a concrete Path: what WE should
   build, which files, which risks, why it fits. Findings without Paths are
   gossip.
5. **Never copy wholesale**: MIT targets may be *adapted with attribution*;
   anything copied must keep its license header. Sloppy copying is the failure
   mode the owner explicitly forbids.

## Workflow (ACTION REQUIRED)
1. **case-init**: `bash skills/scripts/case-init.sh --hint "<repo> <area>"`
   → `work/<case>/scope.md` with:
   - in_scope.assets = [target repo URL or path, feature area under study]
   - network_profile = offline (or authorized_target_only if the analysis
     needs to fetch the repo — fetching the repo itself is in scope)
   - activities = [read_only_analysis, rebuild_execution]
2. **Clone & orient** (E-001): clone to `work/<case>/target/` at a pinned
   commit (`git clone … && git checkout <sha>` — record the sha as evidence).
   Inventory: top-level layout, README claims, entry points
   (`package.json`/`pyproject.toml`/`go.mod`/`Cargo.toml`), test layout.
3. **Map the feature area**: find the files implementing the area under study
   (search by behavior keywords, not file names). Draw a 5-10 node data/control
   flow map of that area. This map is a deliverable.
4. **Deep read**: for each file in the area, note responsibilities, key
   functions, and anything we would do differently. Collect file:line evidence
   as you go (E-* entries, or inline `file:line` citations in the report).
5. **Findings**: consolidate into F-* items — what the target does, how it
   does it, what's good (learn from it), what's weak (do better), what's
   missing (opportunity). Each with evidence citations.
6. **Paths**: for each Finding, write the Path: what WE build (files to create
   in our project), key decisions, risks, effort (s/m/l), and why it beats the
   target for our use case. **The Paths are the spec.**
7. **BUILD IT**: execute the Paths now — scaffold, implement, check, review,
   ship (see "Execution — actually build it" below). Do NOT stop at Paths.
8. **Report**: one markdown file per area:
   `docs/competitive/proposals/<area>-YYYYMMDD.md` — format:

```markdown
# Teardown: <target> — <area>
- target: <repo> @ <commit sha>
- date: <ISO>
- scope: <what area, what excluded>
## Map
<ascii/mermaid control-flow map of the area>
## Findings
F-1 … (evidence: file:line)   → Path P-1: what we build…
F-2 …
## Paths
P-1 … P-2 …
## Verdict
<2-5 lines: what to copy conceptually, what to avoid, go/no-go>
```

8. **Journal**: anonymized lessons → `skills/field-journal/` (patterns worth
   reusing, traps found).

## Deliverables
- Control-flow map of the studied area
- Findings with file:line evidence
- Paths (what we build) — the actual value of the exercise
- Proposal doc per area under `docs/competitive/proposals/`

## Execution — actually build it (owner default, not advice)

The teardown is NOT the deliverable. After Paths are written, **build the thing**:

1. **Scaffold** the project at `/Users/bishesha/projects/<name>` (per business
   memory: all code lives there, kebab-case names). One parent, fewest files.
2. **Implement each Path** in order. Reuse existing helpers first; add only what
   the Paths require. No gold-plating.
3. **Leave one runnable check** per non-trivial module (assert-based self-check
   or one small test — no frameworks).
4. **Run the check** — fix until it passes. This is the round-1 evidence.
5. **Independent review round** (loop skill): a fresh-context reviewer checks
   the build against the Paths; fix its findings; repeat up to 4 rounds.
6. **Ship**: commit + push to a private repo on the owner's GitHub (default),
   unless the owner says otherwise.
7. **Report** what was built, what passed, what's deliberately out of scope.

Proposal-only mode is opt-in: only if the owner explicitly asks for "just the
plan / advice only".

## Boundaries
- Never modify the target; never copy code wholesale without license
  attribution; no decompilation needed for readable source (that's Mode 1).
- If the target is NOT readable source (binary-only, obfuscated), this is
  Mode 1 security RE — switch routes (R1/R3/R0).
