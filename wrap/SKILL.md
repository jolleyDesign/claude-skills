---
name: wrap
description: Wrap up the session before /clear — record decisions and gaps where the project keeps them, update memory, commit the work with a message in the repo's style, and check nothing would be lost. Run only when the user types /wrap.
disable-model-invocation: true
---

# Wrap

The user is about to clear this session. Everything that matters must live in files or git
afterwards, because the conversation won't. Typing `/wrap` is the explicit request to commit.

## 1. Read the project's rules

Read the project instruction files (`CLAUDE.md`, `AGENTS.md` and what they import) for:
where decisions, gaps and progress are recorded; the commit message style; what "finishing" a
unit of work requires; test or CI rules. Those rules win over the defaults below.

## 2. Take stock

- `git status`, `git diff --stat`, `git diff --cached --stat`, `git stash list`, recent `git log`.
- Go back over this session: decisions the user made, preferences or corrections they gave,
  gaps or bugs found and not fixed, work started and not finished, what was verified (tests,
  CI, device or manual runs) and what wasn't.
- Files written outside the repo that matter (scratchpad, `/tmp`): move anything worth keeping
  into the repo or say it will be lost.
- Background work still running (shells, agents, monitors): wait for it, stop it, or report it.

## 3. Save decisions and state

Write each item where the project keeps it, not only in the commit message:

- **Decisions** → the doc the project names (plan, ADR, open-decisions list), dated in the
  project's format. Decisions still open → the project's open-decisions place, with a
  recommendation.
- **Gaps found** → the backlog or issue list. Fixed ones → remove.
- **Progress** → the roadmap/plan: mark finished steps; for unfinished work, leave enough that a
  fresh session can pick it up (what's done, what's next, any traps found).
- **How the user wants me to work** (corrections, confirmed approaches, environment facts) →
  memory, following the memory rules: update an existing memory rather than duplicate, and don't
  save what the repo already records.

If the project has no place for in-progress notes, put a short `project` memory with the
next step instead of inventing a new file in the repo.

## 4. Commit

- Check what will be committed: no secrets, credentials, `.env` files, large binaries or build
  output, stray debug files, or restricted data (follow any organisation data rules). If a change
  looks unrelated to the session or accidental, ask before including it.
- Stage the session's work, including deletions and new files, by name rather than `git add -A`
  when the tree has anything questionable.
- Message: match the repo's convention from `git log` and its instruction files. Say honestly
  what's in progress, what was and wasn't tested or run. Follow the user's rules on attribution
  lines (some users forbid them — check memory).
- Run the project's commit hooks; if a hook fails, fix the cause and make a new commit — never
  `--no-verify`, never amend an earlier commit unless asked.
- Don't push unless asked.
- Don't start a long test or CI run just to wrap. If the project requires one before committing
  finished work and it hasn't run, ask whether to run it or commit as in progress.

## 5. Verify and report

- `git status` is clean, or every remaining file is deliberately left out — list those.
- Report briefly: the commit (hash and subject), what was saved where, anything not saved or not
  verified, and the next step a new session should start from (and where it's written down).
