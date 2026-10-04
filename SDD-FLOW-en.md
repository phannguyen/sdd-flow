# Spec-Driven Development (SDD) Workflow

**English** · [Tiếng Việt](SDD-FLOW-vi.md)

This project is built **spec first, code second**: every decision lives in the documents under `spec/`, and the code follows those documents.

This document is for the **AI agent** working in the repo and for the **user** (the person who sets scope, approves work, and authorizes merges). Agent: read this entire document before doing anything, then start at [section 0](#0-agent-start-here).

## 0. Agent: start here

**Step 1 — Determine the state of the repo** and follow the matching row:

| State | What to do |
|---|---|
| No `spec/`, repo is nearly empty | Bootstrap a new project ([section 8.1](#81-new-project)) |
| No `spec/`, code already exists | Bootstrap an existing project ([section 8.2](#82-existing-project)) |
| `spec/` exists but constitution files are missing, or docs live elsewhere (`specs/`, `docs/`…) | Tell the user what exists and propose how to align it with this document. Do not rename or move files on your own |
| Constitution exists, `roadmap.md` has a 🔄 phase | Read that phase's spec folder, check you are on the right branch, continue from the first task group still `[ ]` in `plan.md` |
| Constitution exists, no 🔄 phase | Tell the user the next ⬜ phase (and any ⏸ phases), offer to create its spec or resume a paused phase ([section 4](#4-phase-lifecycle)) |

**Step 2 — When the user gives you a specific task**, classify it:

| Task type | Handling |
|---|---|
| Belongs to the current 🔄 phase | Update the spec first, then do it ([section 5](#5-mid-course-changes)) |
| Small change, does not touch the constitution | Fast track ([section 5.2](#52-fast-track-for-small-changes)) |
| Changes conventions, technology, data model… | Update the constitution first ([section 5.1](#51-types-of-change)) |
| New feature not on the roadmap | Propose adding it to `roadmap.md`; create a spec only after the user agrees |

**Step 3 — If the repo has no `CLAUDE.md` or `AGENTS.md`**, offer to create one (a few lines, see [section 2](#2-directory-structure)) so future sessions read this document automatically.

**Never:**

- start coding before the spec is approved;
- create a branch or spec files before the questions are answered;
- commit directly to the target branch (except the very first commit of a new repo, [section 8.1](#81-new-project));
- merge before the user says "merge";
- push, `push --force`, `reset --hard`, delete unmerged branches, or rewrite pushed history without the user's consent;
- do risky work ([rule 15](#1-working-rules)) without the user's confirmation;
- commit secrets to Git.

## 1. Working rules

1. **The constitution beats the spec.** If a spec conflicts with the constitution, update the constitution first and record why.
2. **Ask first, write later.** No branches or files until the questions are answered.
3. **Recommend clearly.** Always state one recommended option with a short reason; never list options as equals and leave the user to guess.
4. **Record every decision**, including the ones the agent made on its own, in the spec or constitution, not only in the chat.
5. **Docs never lag behind code.** When something changes, update the spec first or in the same commit.
6. **One commit per task group;** commit only when the state is consistent (builds, tests pass, config exported…).
7. **Tests are part of "done".** New code ships with tests using the tooling defined in `tech-stack.md`.
8. **Verify with evidence.** Record the output of commands you ran, never "should work".
9. **No new libraries/dependencies** not listed in `tech-stack.md` without the user's approval.
10. **No secrets in Git** (passwords, keys, machine-specific config files).
11. **A spec must be enough to resume in a new session.** If understanding the task requires the chat history, the spec is incomplete: fill the gap.
12. **At most one 🔄 phase at a time.** To start another phase, the current one must be merged, or paused/cancelled per [section 5.1](#51-types-of-change).
13. **Keep Git clean.** Only create or switch branches with a clean working tree (commit pending work to the current branch or ask the user). New branches always start from an up-to-date target branch.
14. **Language:** specs, constitution, CHANGELOG, and commits are written in English unless `tech-stack.md` says otherwise. File, branch, and folder names use ASCII only.
15. **Stop and ask before risky work**, even if the plan lists it: migrations that change or delete data; changing APIs/URLs others depend on; authentication, authorization, payments; any action on a live (production) environment. State what you will do, its impact, and how to roll back; wait for the user's consent.

## 2. Directory structure

```
<project>/
├── SDD-FLOW.md                 # this document
├── CLAUDE.md / AGENTS.md       # short pointer for agents (see below)
├── README.md                   # how to build and run; stakeholder input
├── CHANGELOG.md                # dated change log (section 7)
├── spec/
│   ├── mission.md              # ┐
│   ├── tech-stack.md           # ├ constitution
│   ├── roadmap.md              # ┘
│   └── YYYY-MM-DD-<name>/      # one folder per phase
│       ├── requirements.md
│       ├── plan.md
│       └── validation.md
├── backlog/                    # research notes for open decisions (optional)
│   └── YYYY-MM-DD-<topic>.md
└── .claude/skills/             # optional (section 9)
```

Contents of `CLAUDE.md` / `AGENTS.md` (pointers only, **do not copy constitution content here**):

```markdown
This project follows Spec-Driven Development. Before doing anything:
1. Read `SDD-FLOW.md`.
2. Read `spec/mission.md`, `spec/tech-stack.md`, `spec/roadmap.md`.
3. If the roadmap has a 🔄 phase, read that phase's spec folder.
```

## 3. Constitution: the three foundation documents

The constitution is the project's highest authority. Phase specs must not contradict it.

| File | Answers | Contents |
|---|---|---|
| `mission.md` | What, for whom, why? | Product; target users; stakeholder requirements; differentiators; principles (content, experience, accessibility); what success looks like (measurable: metric, threshold, method); out of scope |
| `tech-stack.md` | Built with what, under which conventions? | Platform, language, versions; data model; URL/API structure; front-end architecture; libraries; **testing** (tools, commands); build/run commands; configuration and environment variables (name, default, meaning); operations; **gaps / tech debt**; **open decisions table** |
| `roadmap.md` | In what order? | Numbered phases (optionally grouped by milestone) with status ✅ done · 🔄 in progress · ⏸ paused · ⬜ not started; each phase has a one-line goal, a `[ ]` task list, an acceptance line, and "decide before"; a "post-launch" section |

**Phase size.** Each phase should be finishable in **one focused working session** and be **a complete slice**: after merging, the project still runs, and the phase can be tested and reviewed on its own. Split by user-visible feature, not by technical layer (no "all the database" phase followed by an "all the UI" phase); layering belongs only in the task groups of `plan.md`. If it is too big, split it (e.g. "Contact page — content" and "Contact page — map"). Several small phases following a familiar pattern may share one spec, but each phase remains its own task group in `plan.md`.

**Release milestones (optional).** If the project needs milestones (MVP, v1.0…), group the phases in `roadmap.md` under milestone headings (`## Milestone 1 — MVP`); record each milestone's scope in `mission.md` as two lists, "In this milestone" and "Deferred". Milestones only group: each phase inside still runs the full lifecycle of section 4, and the first phase should still be a runnable skeleton. When the last phase of a milestone is merged, the agent proposes **milestone acceptance**: the user tries the whole product, not just individual phases; issues found become fix phases within that milestone; the roadmap marks the milestone ✅ once the user confirms.

**Open decisions table** in `tech-stack.md`:

```markdown
| Open decision | Options | Decide before phase | Notes |
|---|---|---|---|
| Hosting | A / B / C | Phase 5 | backlog/2026-10-01-hosting.md |
```

Once the user decides: move the decision into the main body of `tech-stack.md` and remove the row. If `/new-spec` reaches a phase that depends on an open decision, ask the user to settle it first.

**UI design.** If the project has a UI and an approved design (mockup, Figma…), `tech-stack.md` links to it and states that it is the source of truth for the UI: to change the UI, update the design first, then the code.

## 4. Phase lifecycle

```
create spec ─► approve ─► implement plan ─► validate ─► review ─► retrospective ─► changelog ─► "merge" ─► revisit roadmap
(3 question rounds)       (1 commit per group)  │ ❌                                                         (as needed)
                               ▲                │
                               └────────────────┘ fix, then re-run the full validation
```

| Step | Who | Work | Output |
|---|---|---|---|
| 1. Create spec | Agent | Read the constitution and existing specs; pick the first ⬜ phase (or the one the user names); **ask 3 rounds** (section 4.2); only then create a branch from the target branch and the 3 spec files; mark the roadmap 🔄 | Branch `spec/YYYY-MM-DD-<name>`, 3 spec files. **No commit, no code yet** |
| 2. Approve | User | Read the 3 files, especially "Decisions made by the agent"; edit or approve | Agent commits the spec |
| 3. Implement | Agent | Work through `plan.md` group by group; write tests with the code; tick `[x]`; handle changes per section 5. After a group marked 🔍, stop, report, and wait for the user to say "continue" | One commit per group |
| 4. Validate | Agent | If the target branch has new commits: merge the target branch into the spec branch (no rebase) and resolve conflicts. Then run every item in `validation.md`, record ✅/❌ with evidence in the Result column. For items the user checks: list how to check them and wait for the user to report results | Acceptance table filled in |
| 4b. If any ❌ | Agent | Fix, commit, re-run the **entire** validation. If the ❌ comes from a wrong or incomplete spec: fix the spec and tell the user | No ❌ left |
| 5. Review | Agent | Review the full branch diff against the target branch from three angles (below); fix what is worth fixing; record deferred items in `requirements.md` (Deferred section) or the roadmap | Findings handled |
| 6. Retrospective | Agent | Ask yourself: "After this phase, what in the spec/constitution needs clarifying or is now outdated?"; update the constitution; mark the roadmap ✅ | Constitution matches reality |
| 7. Changelog | Agent | Update `CHANGELOG.md` (section 7) | Separate commit |
| 8. Report, await merge | Agent → User | Short report: what was done, validation results, decisions made by the agent, deferred items. Wait for the user to say "merge" | — |
| 9. Merge | Agent | Follow "Merge method" in `validation.md`: local `git merge --no-ff` then delete the spec branch, or push the branch and open a PR. **No squash, no rebase** (keeps the SHAs recorded in CHANGELOG valid) | — |
| 10. Revisit roadmap | User + Agent | As needed: split, merge, or reorder phases; update "decide before" | Ready for the next phase |

**Three review angles (step 5).** If the agent supports subagents, run one subagent per angle in parallel:

1. **Spec conformance:** is everything in `requirements.md` done, is anything extra, does anything contradict the constitution?
2. **Quality:** readable code following the conventions in `tech-stack.md`; security (input handling, authorization, secrets); fragile spots.
3. **Testing and operations:** do the tests actually check what matters; does a from-scratch rebuild work; what still lacks tests?

Very small phases may skip review, but say so in the "Not required" section of `validation.md`.

### 4.1. The three spec files

| File | Purpose | Must contain |
|---|---|---|
| `requirements.md` | **What and why** | Context; scope precise enough to build without guessing (names, types, required, defaults); **patterns to follow** (existing files/features to imitate); out of scope; dated decisions table; **decisions made by the agent** |
| `plan.md` | **In what order** | Numbered task groups in dependency order; each group is one separately reviewable commit; tests ship with the group that creates the code; groups the user must approve before work continues carry 🔍 in their heading; the last group is always wrap-up |
| `validation.md` | **How we know it is done** | Setup commands; automated checks; manual checks; **not required** section; target branch; numbered **merge conditions** |

Any decision in a spec that applies project-wide (e.g. "every page must be responsive", "pin library versions") goes into the constitution right away; the spec just points to it. A spec covering several phases splits `validation.md` into one section per phase.

`requirements.md` template:

```markdown
# Requirements — <phase name>

## Context
<Phase N on the roadmap; what problem it solves; which goal/users in mission.md it serves; which earlier phases it builds on.>

## Scope
| Item | Name | Type | Required | Default | Notes |
|---|---|---|---|---|---|

## Patterns to follow
- <Follow the approach in `path/to/file`.>

## Out of scope
- …

## Deferred
- <Items found during implementation/review, not done in this phase.>

## Decisions
| Date | Decision | Reason | Decided by |
|---|---|---|---|

## Decisions made by the agent
- <What the agent decided without explicit guidance, with reasons. The user reviews and may overrule.>
```

`plan.md` template:

```markdown
# Plan — <phase name>

## Group 1 — <name>
- [ ] 1.1 …
- [ ] 1.2 …
- [ ] 1.3 Tests for the above

## Group 2 — <name> 🔍
- [ ] 2.1 …

## Group N — Wrap-up
- [ ] Run all of validation.md, record results
- [ ] Three-angle review
- [ ] Update the constitution if there are new decisions; roadmap ✅
- [ ] Update CHANGELOG.md
- [ ] Report, await "merge"
```

`validation.md` template:

````markdown
# Validation — <phase name>

Target branch: `main`
Merge method: local `--no-ff` | pull request

## Setup
```
<commands to set up the environment, install dependencies, load data>
```

## Automated checks
| # | Command | Expected | Result |
|---|---|---|---|
| A1 | <build/typecheck command> | exit 0, no errors | |
| A2 | <test command> | all pass; tests exist for: … | |

## Manual checks
| # | Action | Expected | Checked by | Result |
|---|---|---|---|---|
| M1 | … | … | Agent / User | |

## Content check (if there is user-facing content)
- [ ] Follows the content principles in mission.md (voice, terminology)
- [ ] No placeholder content left (lorem ipsum, "TODO", "xxx")

## Not required
- <What is deliberately not checked in this phase.>

## Merge conditions
1. Every item above is ✅.
2. No leftover debug code, temporary logs, temporary TODOs, or stray files.
3. CI (if the project has it) is green.
4. …
````

### 4.2. Three question rounds

Ask **before writing anything**, because the answers may change the branch name, folder name, or scope. Use the agent's multiple-choice question tool (in Claude Code: `AskUserQuestion`); without one, ask as a numbered list with a/b/c options per question.

| Round | For file | Ask about |
|---|---|---|
| 1. Requirements | `requirements.md` | Scope boundaries, constitution choices still open, default values, what to defer |
| 2. Plan | `plan.md` | Approach, what now vs. later, task grouping, which groups need a review stop (🔍), branch name |
| 3. Validation | `validation.md` | How to verify (automated tests, manual steps, both), which items the user checks personally, from-scratch rebuild or not, target branch, merge method (local or PR) |

- 2–4 options per question, **recommended option first**, each option with a one-line trade-off.
- **Never re-ask what the constitution already decides**; write it into the spec and point to where it was decided.
- For a small phase where the constitution already answers almost everything: **merge the 3 rounds into one** (one question per file).
- If the user wants tight oversight (large project, live system): mark every group 🔍 and merge via PR.
- If the user says "you decide": still record every self-made decision under "Decisions made by the agent".

## 5. Mid-course changes

Principle: **docs first (or in the same commit), code second.** Whenever the user adds or removes a requirement, the agent asks itself: "where in the spec or constitution must this be recorded?"

### 5.1. Types of change

| Type | Examples | Handling |
|---|---|---|
| **Within the current phase** | Split a component into its own file; add a minimal page | Update `plan.md` (add/edit a group), sync `requirements.md` and `validation.md`, add a row to the Decisions table, then implement |
| **Project-wide** | Add/switch test tooling; require responsive design; brand colors; naming conventions | In order: (1) update the constitution; (2) review and update **every affected spec and the roadmap**; (3) update the code; (4) add/update tests. If many code changes are needed: add it as a roadmap phase and run the full workflow. Docs only: a `chore/…` branch |
| **Roadmap reshuffle** | Merge phases; split an oversized phase; reorder | On a `chore/…` branch: edit `roadmap.md`, give the reason in the commit, update "decide before", await "merge". Only when no phase is 🔄 |
| **Pause / cancel a phase** | User changes priorities mid-phase | Record the reason and current state in `requirements.md` (Decisions), commit on the spec branch. Pause: mark ⏸ on the roadmap and keep the branch. Cancel: set the roadmap back to ⬜ or drop the phase; delete the branch only with the user's consent |
| **Needs research** | Choose a database, hosting, external service | Research, save to `backlog/YYYY-MM-DD-<topic>.md` (context, options, pros/cons, recommendation), link it from the open decisions table. Move it into the constitution only once the user decides |

### 5.2. Fast track for small changes

A spec folder may be skipped when the change **is** an obvious bug fix, a copy fix, a minor dependency bump, or a UI tweak matching the existing design, **and** it does not touch the constitution.

Steps: branch `fix/YYYY-MM-DD-<name>` → fix → add a test if it is a bug → run the build and the full test suite → update CHANGELOG → report, await "merge".

If you discover mid-way that conventions, libraries, or the data model must change: **stop, tell the user, switch to the full workflow**.

## 6. Naming and symbols

| Item | Convention | Example |
|---|---|---|
| Spec folder | `spec/YYYY-MM-DD-<kebab-name>`, spec creation date | `spec/2026-10-01-login` |
| Spec branch | same as the spec folder | `spec/2026-10-01-login` |
| Fast-track branch | `fix/YYYY-MM-DD-<kebab-name>` | `fix/2026-10-01-footer-link` |
| Docs-only branch | `chore/YYYY-MM-DD-<kebab-name>` (constitution, roadmap, backlog) | `chore/2026-10-01-merge-phases` |
| Research note | `backlog/YYYY-MM-DD-<topic>.md` | `backlog/2026-10-01-hosting.md` |
| Roadmap status | ✅ done · 🔄 in progress · ⏸ paused · ⬜ not started | |
| Validation result | ✅ pass · ❌ fail, with a note | |
| Review stop | 🔍 in a task group heading in `plan.md` | `## Group 2 — Data model 🔍` |
| Spec commit | `Spec: <phase name>` | `Spec: login` |
| Task group commit | Short, imperative, with group number | `Group 2: add login form and tests` |

Names are lowercase ASCII, joined with `-`.

## 7. CHANGELOG

Format:

```markdown
# Changelog

<!-- changelog-last: <full SHA of the last recorded commit> -->

## 2026-10-02

- Add login form with input validation (a1b2c3d)

## 2026-10-01

- Create constitution: mission, tech-stack, roadmap (9f8e7d6)
```

How to update (before every merge):

1. Read the SHA in the `changelog-last` marker. No file yet: take the full history.
2. Get new commits: `git log <SHA>..HEAD --no-merges --date=short --format='%ad %h %s'`.
3. Skip commits that only touch `CHANGELOG.md`. Group by date, newest first; append to an existing date heading if there is one. Rewrite each line so a reader understands it (don't copy cryptic commit messages verbatim), ending with the short SHA.
4. Set the marker to the SHA of `HEAD` (before the changelog commit).
5. Separate commit: `Update CHANGELOG`.

SHAs in the CHANGELOG stay valid only if branches are merged with `--no-ff` (no squash, no rebase). On a merge conflict in `CHANGELOG.md`: keep both sides' entries, re-sort by date, and set the marker to the newer SHA in the target branch's history.

## 8. Bootstrapping

### 8.1. New project

1. Read `README.md` and any available input (stakeholder requirements, feature list, design). If input is missing, ask the user for a short product description first. No Git yet: ask the user, then run `git init` with `main` as the main branch.
2. Ask the user in 3 groups, **before writing to disk**: mission (product, users, success), tech stack (platform, testing, operations), roadmap (priorities, scope of the first release).
3. Create `spec/mission.md`, `spec/tech-stack.md`, `spec/roadmap.md` per [section 3](#3-constitution-the-three-foundation-documents). Keep roadmap phases small; phase 1 should be a runnable skeleton (project scaffold, build command, test command that runs even with no tests).
4. Create `CLAUDE.md` or `AGENTS.md` per [section 2](#2-directory-structure), and a `.gitignore` that excludes secrets (`.env`, keys…). If environment variables are needed, add a `.env.example` with variable names only, no values.
5. Ask the user to approve the constitution. Once approved, commit to `main` (`Create constitution`). This is the only commit made directly on the main branch. Then offer to create the spec for phase 1.

### 8.2. Existing project

The constitution is **reverse-engineered from the current state**:

1. Read the code, configuration, `README.md`, existing TODOs/issues, and recent Git history.
2. Ask the user about mission, target users, tech stack gaps, and upcoming priorities, **before writing to disk**.
3. Create the constitution:
   - `tech-stack.md` describes **what exists** (including tech debt, under Gaps), not what is wanted as if it already existed.
   - `mission.md` from the README and the user's answers.
   - `roadmap.md` from TODOs/issues and the user's wishes, in small phases. Completed work needs no retroactive specs; add a single "Current state" line at the top of the roadmap.
4. If the project has no tests or the tests don't run: propose phase 1 as "set up a minimal test suite" before changing features.
5. Do all of the above on a `chore/YYYY-MM-DD-constitution` branch, including `CLAUDE.md`/`AGENTS.md`. Wait for approval, commit, then await "merge".

## 9. Skills (optional)

If the repo has `.claude/skills/`, prefer the matching skill; otherwise this document is sufficient.

| Skill | Corresponds to | Notes |
|---|---|---|
| `/new-spec` | Section 4 step 1 + section 4.2 | Read conventions from the constitution on every run; don't copy conventions into the skill |
| `/changelog` | Section 7 | |

Set `disable-model-invocation: true` in the frontmatter so skills run only when invoked. When the user types the same long request a second time, offer to turn it into a skill.
