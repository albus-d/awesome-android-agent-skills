---
allowed-tools: Bash(git diff*), Bash(git fetch*), Bash(git log*), Read, Glob, Grep, Agent
description: Review diff between current branch and an origin branch for security issues, bugs, wrong logic, and Android/Compose skill violations
---

## Your task

You are reviewing code changes between the current branch and an origin branch. The user will provide:
- An origin branch to diff against (e.g., `origin/develop`, `origin/main`)

## Workflow

### Step 1: Fetch Latest & Get the Diff

First, fetch the latest remote state:

```bash
git fetch origin
```

Then list changed files:

```bash
git diff <ORIGIN_BRANCH>...HEAD --name-only
```

Then get the full diff:

```bash
git diff <ORIGIN_BRANCH>...HEAD
```

Read all changed files from the diff to understand the full scope of changes. For each changed file, also read the full file in the codebase (not just the diff) to understand context.

### Step 2: Code Review — Security Issues

Analyze all changed code for security problems:

- Hardcoded secrets, API keys, tokens
- SQL injection, command injection
- Insecure data storage (SharedPreferences for sensitive data without encryption)
- Missing input validation at system boundaries
- Insecure network communication (HTTP instead of HTTPS)
- Improper permission handling
- WebView vulnerabilities (JavaScript enabled without safeguards)
- Intent injection / exported components without protection
- Logging sensitive data in production
- Insecure deserialization

### Step 3: Code Review — Bugs & Wrong Logic

Analyze all changed code for:

**Bugs:**
- Null pointer risks (unchecked nullable types)
- Race conditions in coroutines/threading
- Memory leaks (context leaks, uncleared references)
- Off-by-one errors
- Incorrect exception handling (swallowing CancellationException, etc.)
- Resource leaks (unclosed streams, cursors)
- Incorrect lifecycle handling

**Wrong Logic:**
- Business logic errors
- Incorrect state transitions
- Wrong conditional branches
- Missing edge cases
- Incorrect data transformations
- Incorrect API usage or mismatched request/response handling

### Step 4: Check Against Personal Android/Compose Skills

Review all changed code against these personal skill rules. For each skill, invoke the skill using the Skill tool to load the full rule set, then check the changed code against those rules. Reference the skill name when reporting violations:

**compose-ui**: State hoisting, modifier conventions, performance (remember, derivedStateOf), theming, previews
**compose-convention-lint**: CompositionLocal misuse, missing remember, wrong parameter ordering, composable constant anti-patterns, mixed CompositionLocal styles
**compose-performance-audit**: Recomposition storms, unstable keys, heavy work in composition, unstable lambdas, missing remember, large images, layout thrash
**compose-navigation**: Type-safe routes, argument passing, navigation patterns
**android-architecture**: Clean Architecture layers, Hilt DI, dependency direction
**android-viewmodel**: StateFlow for UI state, SharedFlow for events, proper encapsulation, scope usage
**android-coroutines**: Dispatcher injection, main-safety, lifecycle-aware collection, GlobalScope prohibition, CancellationException handling, cooperative cancellation
**kotlin-concurrency-expert**: Structured concurrency, lifecycle safety, state encapsulation, exception handling
**android-data-layer**: Repository pattern, Room/Retrofit usage, offline-first sync
**android-retrofit**: URL manipulation, Hilt config, error handling in repositories
**coil-compose**: AsyncImage usage, performance in lists, content descriptions
**android-testing**: Test coverage for new code
**android-accessibility**: Content descriptions, touch target size, color contrast, focus/semantics
**android-gradle-logic**: Convention plugins, version catalog usage

Only check skills that are relevant to the changed files. For example, skip compose-ui if no Composable files were changed.

### Step 5: Output Report

Structure your report as follows:

```
# Branch Code Review: <current_branch> vs <origin_branch>

## Summary
<Brief description of what the changes do>

## Changed Files
<List of changed files>

## Security Issues
| Severity | File:Line | Issue | Recommendation |
|----------|-----------|-------|----------------|
| ...      | ...       | ...   | ...            |

## Bugs & Logic Errors
| Severity | File:Line | Issue | Recommendation |
|----------|-----------|-------|----------------|
| ...      | ...       | ...   | ...            |

## Skill Violations
| Skill | Rule | File:Line | Issue | Fix |
|-------|------|-----------|-------|-----|
| ...   | ...  | ...       | ...   | ... |

## Overall Verdict
<CLEAN / HAS ISSUES>
<Summary of critical items that must be addressed>
```

Use severity levels: CRITICAL, HIGH, MEDIUM, LOW, INFO

If a section has no findings, write "No issues found." instead of an empty table.

## Important Notes
- Be thorough but avoid false positives — only report issues you are confident about
- When checking skills, only flag clear violations, not stylistic preferences
- Always provide actionable recommendations, not just problem descriptions
- Focus on the diff — do not review unchanged code unless it provides necessary context
- If a finding is borderline, use INFO severity rather than over-flagging
