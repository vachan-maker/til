# Working with Coding Agents (Codex)

> [!NOTE]
> Notes summarized from the OpenAI *Getting Started with Codex* crash course.

> [!WARNING]
> This content was generated and structured using AI. Please review and verify commands against your project's environment.

---

## Agent Collaboration Lifecycle

Working effectively with coding agents follows a disciplined five-phase loop:

1. **Prompt**: Provide a clear goal, relevant context pointers, constraints, and completion criteria.
2. **Inspect**: Explore how the repository works and establish a safe baseline check before editing.
3. **Act**: Review the plan, implement a bounded change, and steer the agent when drift occurs.
4. **Validate**: Run focused tests, execute quality checks, and manually verify the experience.
5. **Report**: Review the diff and generate an evidence-based developer handoff.

---

## 1. Exploring Unfamiliar Codebases

When opening a new or unfamiliar repository, explore before modifying code.

### 6 Essential Exploration Questions
1. How the application is structured.
2. Which files manage state and controls.
3. How to run the application (development commands).
4. Which tests and validation commands are available.
5. Which files need to change for the target feature.
6. What assumptions, risks, or missing information exist.

### Example: Codebase Exploration Prompt
```text
Inspect this repository without making any changes.

Explain:
1. How the application is structured.
2. Which files manage the game’s state and controls.
3. How to run the application.
4. Which tests or validation commands are available.
5. Which files may need to change to add pause and resume controls.
6. Any assumptions, risks, or missing information.
```

### Establishing a Safe Baseline
Before requesting code changes, run existing validation commands to establish an operational baseline:
- **Passing baseline**: Confirms the environment is healthy before new code is introduced.
- **Failing baseline**: Helps separate pre-existing bugs from regressions introduced by the agent.
- **Visual applications**: Baseline feedback should include unit tests for state logic, the dev server command, and a brief manual interface check.
- **Libraries**: Baseline feedback should include targeted tests, linter, type checks, and build commands.

---

## 2. Context Management & Scoping

- **Scope to one feature**: Keep the agent focused on a single bounded problem per session.
- **Start fresh threads**: When pivoting to another task or feature, start a new chat/thread to avoid context pollution and token bloat.
- **Supply only necessary context**: Include only the files, errors, and constraints relevant to the immediate task.

---

## 3. Reusable Guidance with `AGENTS.md`

An `AGENTS.md` file captures recurring project instructions and expectations so you do not have to repeat them in every prompt. It keeps human-facing `README.md` files clean of agent-specific trivia.

### Key Contents to Include
- Important directories and application components.
- Exact commands for running the app, linting, building, and running tests.
- Coding conventions and architectural patterns.
- Protected files and security requirements (files never to edit).
- Verification steps required before completing a task.

### Example: Repository `AGENTS.md`
```markdown
# Project guidance
- Run the application with `pnpm dev`.
- Run automated tests with `pnpm test`.
- Follow existing patterns for game state, interface components, and user interactions.
- Keep changes focused on the requested feature and avoid unrelated modifications.
- Preserve existing scoring, reset behavior, and application styling.
- Add or update tests when changing application behavior.
- Run the relevant checks and report any issues before marking a task complete.
```

### Layered Guidance Hierarchy
Guidance can be layered across scopes:
1. **Global Guidance (`~/.codex/AGENTS.md`)**: Applies across all projects on your machine.
2. **Repository Guidance (`<root>/AGENTS.md`)**: Applies to the entire codebase.
3. **Folder Guidance (`<folder>/AGENTS.md`)**: Applies when the agent works inside a specific subdirectory (e.g. `frontend/AGENTS.md`).

> **Precedence Rule**: Guidance closer to the current working directory overrides broader guidance. An `AGENTS.override.md` file can completely replace the standard `AGENTS.md` at that level when an explicit override is required.

---

## 4. Making Workflows Executable

An agent can only self-correct if the repository provides executable feedback.

### 5 Steps to an Executable Development Path
1. **Document setup**: Clear instructions to install dependencies and run the app.
2. **Provide focused tests**: Fast, targeted tests for core state and logic.
3. **Expose quality tooling**: Make linting, formatting, type-checking, and build commands readily available.
4. **Detail environment fixtures**: Document environment variables, test services, and seed data without committing secrets.
5. **Define manual verification paths**: Clear instructions for verifying UI and behaviors not covered by automated tests.

---

## 5. Defining Tasks (The 4-Part Task Brief)

Structure task requests into four distinct sections:

1. **Goal**: State the behavior to add, change, or fix, and why it matters.
2. **Context pointers**: Point to relevant files, errors, examples, or screenshots to inspect first.
3. **Constraints**: Define what must remain unchanged, which patterns to follow, and scope boundaries.
4. **Done when**: Specify observable behaviors and the exact checks that must pass.

### Example: 4-Part Task Brief Prompt
```text
Goal:
Add pause and resume controls to the game.

Context pointers:
Before making changes, inspect how the application manages game state and handles existing controls.

Constraints:
Follow the repository’s existing patterns. Do not change scoring, reset behavior, or unrelated styling.

Done when:
The task is complete when the game pauses and resumes correctly, repeated pause-and-resume cycles work, and the relevant tests pass.
```

---

## 6. Planning Mode (`/plan`) & Steering

### When to Use Plan Mode
Use `/plan` before editing when the task:
- Is ambiguous or underspecified.
- Touches multiple components across the codebase.
- Involves unfamiliar code or dependencies.
- Carries meaningful security or regression risks.
- Has more than one valid architectural approach.

### Steering Active Work
If the agent begins drifting into unnecessary refactoring or unrelated styling changes, intervene early rather than restarting:

```text
Keep the existing interface and add the pause and resume controls using the current component patterns. Avoid unrelated design changes and continue with the original plan.
```

---

## 7. The Validation Stack & Iteration

Validate changes using a multi-tiered stack, starting with checks closest to the modification:

### The 3-Tier Validation Stack
1. **Focused Tests**: Verify that unit and state transitions behave as intended (e.g., pausing transitions state correctly, resuming restores progress).
2. **Relevant Quality Checks**: Run type checks, linters, and build commands to confirm no collateral syntax or type breakages.
3. **Manual Verification**: Test interactive user flows, UI synchronization, and confirm nearby behaviors (e.g., reset, score counters) remain intact.

### Investigating Failures as Evidence
When a test fails:
1. Treat the failure as diagnostic evidence, not a prompt to blindly hack tests.
2. Determine whether the cause is in the **implementation**, the **test assertion**, the **environment**, or an **unclear requirement**.
3. Require the agent to identify the root cause before applying fixes.

---

## 8. Review & Evidence-Based Developer Handoff

### Diff Review (`/review`)
Perform a second-pass review of the diff before accepting changes.

```text
Review the pause-and-resume changes against the original task and implementation plan.

Check whether:
- The requested behavior works as intended.
- The changes remain within the agreed scope.
- Existing project patterns were followed.
- Relevant tests cover the new behavior.
- Scoring, reset behavior, and unrelated styling remain unchanged.
- Any security concerns or unresolved risks require attention.

Summarize the most important findings without making additional changes.
```

### Evidence-Based Developer Handoff
Prepare a structured handoff summarizing concrete results:

```text
Prepare an evidence-based developer handoff for this change.
Include:
- a concise summary of the PM request
- what changed and which files were affected
- exact automated commands and results
- browser and manual checks with results
- review findings and how each was handled
- anything unverified
- remaining risks or follow-up work
- the recommended next step

Separate observed evidence from assumptions or confidence statements.
Do not modify files.
```

---

## 9. Four Core Engineering Decisions

Retain engineering judgment by applying these four core principles:

1. **Start with repository evidence**: Inspect existing files, patterns, and run baseline commands before choosing an implementation strategy.
2. **Define success before implementation**: Formulate the 4-part brief (Goal → Context → Constraints → Done criteria) and identify what must be observed to accept the change.
3. **Guide the work within the boundary**: Constrain the agent's working surface, review plans when uncertain, and steer specific drift promptly.
4. **Accept based on evidence**: Evaluate using the 6-point checklist:
   - **Correctness**: Did the feature work as intended?
   - **Scope**: Did changes stay bounded?
   - **Fit**: Were existing patterns maintained?
   - **Coverage**: Are critical paths covered by tests?
   - **Security**: Are there any new vulnerabilities or exposed secrets?
   - **Uncertainty**: Are any unverified behaviors explicitly declared?
