# Abyssal Sweep — Way of Working

This is the planning store for the Abyssal Sweep project. It holds all specs, changes, and initiatives that span the project's repos.

## Repo structure

```
abyssal-sweep-store/   ← you are here — planning, specs, initiatives (clone by everyone)
abyssal-sweep/         ← game service repo
abyssal-sweep-skills/  ← shared AI skills submodule (managed via submodule, do not clone directly)
```

Each service repo includes `abyssal-sweep-skills` as a git submodule at `.claude/skills/`, so AI skills are available automatically after cloning.

---

## Roles

**Product / design** — work primarily in this store repo. Create and refine changes, specs, and initiatives. No need to clone service repos.

**Developers** — work in a service repo (e.g. `abyssal-sweep`) for implementation. Clone this store repo separately and register it so the OpenSpec CLI can find it from any service repo.

---

## First-time setup

### 1. Prerequisites

- [Claude Code](https://claude.ai/code) installed
- [OpenSpec CLI](https://github.com/fission-ai/openspec) installed: `npm install -g openspec`
- SSH access to GitHub configured

### 2. Clone repos

Clone the repos you need into a common parent folder (e.g. `~/work/abyssal-sweep/`):

```bash
# Everyone
git clone --recurse-submodules git@github.com:silverkea/abyssal-sweep-store.git

# Developers only
git clone --recurse-submodules git@github.com:silverkea/abyssal-sweep.git
```

`--recurse-submodules` ensures the shared skills are pulled in automatically.

### 3. Register the store

Run this once from anywhere on your machine so the OpenSpec CLI can resolve the store by ID from any service repo:

```bash
openspec store register --path ~/work/abyssal-sweep/abyssal-sweep-store --id abyssal-sweep-store
```

Adjust the path to wherever you cloned the store repo.

### 4. Create your workset

A workset is a personal, local grouping of repos that opens together in your editor. It is never committed — each person creates their own.

**Product / design:**
```bash
openspec workset create abyssal-sweep \
  --member store=~/work/abyssal-sweep/abyssal-sweep-store
```

**Developers:**
```bash
openspec workset create abyssal-sweep \
  --member store=~/work/abyssal-sweep/abyssal-sweep-store \
  --member abyssal-sweep=~/work/abyssal-sweep/abyssal-sweep
```

Add more `--member` flags for additional services as needed.

### 5. Open your workset

```bash
openspec workset open abyssal-sweep --tool code
```

This opens all member repos together as a multi-folder VS Code workspace, useful for cross-repo navigation, search, and the Claude Code VS Code extension.

> Note: `--tool claude` (Claude Code CLI with full multi-repo context) is temporarily unavailable while OpenSpec reworks CLI-agent integration. Until it's restored, run `claude` from whichever repo you're working in — the OpenSpec CLI resolves the store by registered ID regardless of which directory you launch from.

---

## Day-to-day workflow

### Planning a new change (product / design)

1. Open your workset: `openspec workset open abyssal-sweep`
2. Use `/openspec-propose` to create a new change with all planning artifacts in one step, or `/openspec-new-change` to step through them one at a time
3. Raise a PR on this store repo for spec review
4. Once merged, notify the relevant service team

### Implementing a change (developers)

1. Open your workset: `openspec workset open abyssal-sweep`
2. Use `/openspec-apply-change` in the service repo to implement tasks from the change
3. Use `/openspec-verify-change` to validate implementation against specs before archiving
4. Use `/openspec-archive-change` when done — this syncs specs to the main spec files and moves the change to archive
5. Raise a PR on the service repo

### Cross-service changes (initiatives)

When a change spans multiple services, product creates an initiative in this store repo:

```bash
openspec initiative create <id> \
  --store abyssal-sweep-store \
  --title "<title>" \
  --summary "<summary>"
```

Each service team then creates a linked local change in their service repo:

```bash
openspec new change <name> \
  --initiative <initiative-id> \
  --store abyssal-sweep-store
```

---

## Keeping skills up to date

Skills live in the `abyssal-sweep-skills` repo. When skills are updated:

```bash
# In each repo that uses skills as a submodule
git submodule update --remote .claude/skills
git add .claude/skills
git commit -m "chore: update skills submodule"
git push
```

---

## Key commands

| Command | What it does |
|---|---|
| `/openspec-explore` | Think through a problem before committing to a change |
| `/openspec-propose` | Create a change with all artifacts in one step |
| `/openspec-new-change` | Create a change, step through artifacts one at a time |
| `/openspec-continue-change` | Progress to the next artifact in an existing change |
| `/openspec-ff-change` | Fast-forward: generate all artifacts at once for an existing change |
| `/openspec-update-change` | Revise planning artifacts mid-flight |
| `/openspec-apply-change` | Implement tasks from a change (service repos) |
| `/openspec-verify-change` | Validate implementation against specs before archiving |
| `/openspec-archive-change` | Finalise and archive a completed change |
| `/openspec-bulk-archive-change` | Archive multiple changes at once |
| `/openspec-sync-specs` | Sync delta specs to main specs without archiving |
