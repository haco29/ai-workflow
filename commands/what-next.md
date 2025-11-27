# What Next? Command

This command helps decide the **next concrete step** in the 7-step Cursor workflow when the user is unsure what to do.

It uses the `rules/what-next.mdc` decision flow plus existing rules like `react-development.mdc`, `quality-check.mdc`, and `api-development.mdc`.

## When to use

Use `@what-next.md` whenever you feel stuck or uncertain:

- After finishing a coding chunk and asking "what now?"
- When you have many open tasks and need a single next move
- When you join a feature mid-way and need orientation
- Any time you type "what next?" / "next step?" / "I'm lost"

## What this command does

1. **Understands current context**
   - Reads recent discussion and open files
   - Looks for any `progress-*.md` / progress tracker snippets
   - Detects whether you just planned, coded, tested, or ran checks

2. **Locates you in the 7-step workflow**
   - Step 01 – Context & Planning  
   - Step 02 – Generate Progress Doc  
   - Step 03 – Implement Features  
   - Step 04 – Reflect on Changes  
   - Step 05 – Add Tests  
   - Step 06 – Quality Assurance  
   - Step 07 – Commit & Continue

3. **Applies the decision checklist**
   - Do we have a clear feature/goal and context?  
   - Do we have a `progress-*.md` progress doc?  
   - Are there pending steps without implementation?  
   - Did we just finish a chunk of implementation?  
   - Do we lack tests for new behavior?  
   - Have we run pre-commit quality checks?  
   - Are all checks green but changes not yet committed / PR opened?

4. **Returns a compact next-step plan**
   - Identified **current step** in the workflow  
   - Recommended **next command** to run  
   - A **3–5 item checklist** of tiny, testable tasks

## Expected AI behavior

When `@what-next.md` is invoked, the AI should:

1. **Ask at most 3 short clarification questions** (if needed)
   - Example questions:
     - "Do you already have a `progress-*.md` file for this work?"
     - "Did you just finish implementing a specific STEP-XXX?"
     - "Have you added tests for this new behavior yet?"

2. **Determine the next step using `rules/what-next.mdc`**
   - Prefer earlier unfinished steps over later ones  
   - Choose one **primary** next step; mention a secondary only if critical

3. **Produce a minimal, action-focused answer**

Response structure:

- **Where you are**: current workflow step + 1-line context  
- **Next command**: one Cursor command (optionally a shell command)  
- **Checklist (3–5 bullets)**: small actions, commit-ready, no long prose

## Example outputs

### Example 1 – No progress doc yet

> **Where you are**: Step 01 – Context & Planning (feature discussed but not tracked)  
> **Next command**: Run `@progress-md.md` to create `progress-trackable-insights.md`.  
> **Checklist**:  
> - Name the feature and confirm objective.  
> - Capture current context and constraints.  
> - Generate pending steps with STEP-IDs.  
> - Mark which step you want to start with.  

### Example 2 – Mid implementation, missing reflection

> **Where you are**: Step 03 → 04 – Implement Features → Reflect on Changes.  
> **Next command**: Run `@reflect-changes.md` for the files updated in STEP-002.  
> **Checklist**:  
> - Summarize what STEP-002 implemented.  
> - Let AI check React/TS patterns against `react-development.mdc`.  
> - Extract any reusable UI to shared components if suggested.  
> - Update `progress-*.md` with STEP-002 status.  

### Example 3 – Ready to test and precommit

> **Where you are**: Step 05 – Add Tests (implementation done, tests light).  
> **Next command**: Run `@add-tests.md` for the new TrackableOverview flows.  
> **Checklist**:  
> - Add user-centric tests for main happy paths.  
> - Add edge-case tests (empty, loading, error).  
> - Ensure queries use accessibility-first selectors.  
> - Then run `@precommit.md` for full quality checks.  

Keep answers **concise, concrete, and biased toward action**. The goal is to give the user one clear next move, not a full plan rewrite.


