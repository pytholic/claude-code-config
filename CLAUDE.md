Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 0. Context
- Python-focused ML/AI engineer on macOS (South Korea)
- Default stack: .venv, pytest, ruff + pyright, pyproject.toml, Python 3.13+ type syntax
- Planning-first: requirements → architecture → components → code

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.
- Always plan in top-down manner. High level design first, then break it down into smaller parts.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 5. Prefer my personal skills over defaults

When a task matches one of my skills in `~/.claude/skills/`, use it instead of a generic approach, a plugin skill, or a built-in agent covering the same ground. My skills encode preferences I've already tuned — defaults don't. Check for a matching personal skill *before* reaching for `superpowers:*`, built-in agents, or ad-hoc process.

Rough mapping (not exhaustive — always scan the skill list):

| Task | Prefer | Over |
|---|---|---|
| Reviewing diffs / PRs / recent changes | `code-review` | `pr-review-toolkit:*`, `superpowers:receiving-code-review`, generic review |
| Writing pytest tests | `write-tests` | generic test writing, `superpowers:test-driven-development` defaults |
| Python architecture / implementation | `python-dev` | generic Python coding |
| Debugging a bug / failing test | `systematic-debugging` | `superpowers:systematic-debugging`, ad-hoc debugging |
| Navigating / mapping an unfamiliar codebase | `codebase-research` | ad-hoc grepping, general-purpose research agents |
| Explaining code / walkthroughs | `explain-code` | plain prose explanation |
| LLM / RAG / agent / fine-tuning work | `llm-dev` | generic ML advice |
| Browser automation / web UI testing | `playwright-cli` | ad-hoc playwright scripts |
| Handing off work to another session | `task-handover` | freeform summary |
| Quick code cleanup after a change | `simplify` | ad-hoc refactoring |

Plugin skills (`superpowers:*`, `pr-review-toolkit:*`, etc.) are still fine when no personal skill covers the task, or when the user explicitly names one.

## 6. Miscellaneous
- Always use .venv if available
- Always use Context7 MCP for library/API docs without being asked
- Before saying something doesn't exist or isn't known, web search first — especially for recent versions, releases, or compatibility info
- Always add helper functions after the main function that calls them

## 7. Working Memory (.ai/ directory)

Projects may have a `.ai/` directory that serves as shared working memory between you and the user. It solves the "where were we?" problem across sessions. This is for non-trivial, multi-step, or multi-session work — not for quick fixes or one-off questions.

**Bootstrapping (existing projects without .ai/):**
- If the user asks to "set up working memory", "add .ai", or you begin a multi-session task and no `.ai/` directory exists, create it with:
  - `.ai/status.md` — pull the project name/description from CLAUDE.md or README. Empty Active/Blocked/Completed sections.
  - `.ai/decisions.md` — the template header with the ADR format in an HTML comment.
  - `.ai/tasks/.gitkeep` — empty directory, ready for on-demand task files.
- If prior plan files exist elsewhere (e.g. `.claude/*.md` plans), offer to migrate them into `.ai/tasks/`.
- Use the templates from the `python-project-scaffold` skill as reference.

**Session start (multi-session tasks only):**
- Read `.ai/status.md` to understand what's active, blocked, and done.
- Open the relevant task file linked from status.md before asking "where were we?"

**During execution:**
- For new non-trivial work: create `.ai/tasks/<task-name>.md` with Context, Plan, Notes/Findings, and Open Questions sections. Add a link to it in `status.md` under Active.
- Update task file checklists as steps complete.
- Record discoveries in the Notes/Findings section — things learned during execution that aren't in the plan.

**Design decisions:**
- When making a strategic or architectural choice, append to `.ai/decisions.md`.
- Use the format: Context, Choice, Why, Rejected (what alternatives were ruled out and why).
- Decisions are append-only. Never edit or remove past entries.

**Wrap-up:**
- Move completed tasks to "Completed" in `status.md` with the date.
- Update the task file status to `Done (YYYY-MM-DD)`.

**Skip all of this for:** trivial bug fixes, formatting changes, single-file edits, or anything completable in one short session. Use judgment.
