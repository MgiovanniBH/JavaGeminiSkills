---
name: full-code-review
description: Comprehensive master review orchestrating security, performance, SOLID, clean code, java patterns, and logging. Provides a lean, action-oriented report focusing only on necessary changes.
---

# Full Code Review Skill

This master skill orchestrates a multi-dimensional technical audit of Java projects. It focuses on identifying technical debt, vulnerabilities, and architectural smells, providing a concise report of actions needed.

## When to Use
- Before major releases
- During architectural reviews
- To identify technical debt in legacy code
- When the user asks for a "complete review" or "full audit"

## Execution Order & Rationale

1.  **`security-audit`**: Identify critical vulnerabilities (injection, insecure config) as priority zero.
2.  **`performance-smell-detection`**: Detect structural bottlenecks (N+1, memory leaks) early.
3.  **`solid-principles`**: Validate the underlying architecture and boundary definitions.
4.  **`java-code-review`**: Check for language-specific anti-patterns, null safety, and modern Java idioms.
5.  **`clean-code`**: Refine readability, naming, and function design.
6.  **`logging-patterns`**: Ensure full observability and traceability (MDC, JSON, kv).

## Output Strategy (LEAN & OPTIMIZED)

**CRITICAL RULE**: Do NOT list what is correct. Report ONLY what needs to be changed or improved.

### Report Template

```markdown
# 🛡️ Full Code Review Report: [Project/Module Name]

> **Summary**: [1 sentence overview of the current state and biggest risk]

## 🚨 Critical (Must fix immediately)
- **[Area]**: [Brief description of the issue]
  - **File**: `path/to/file.java:L123`
  - **Action**: [Clear instruction on how to fix]

## ⚠️ High Priority (Technical Debt / Performance)
- **[Area]**: [Description]
  - **Location**: `file.java`
  - **Action**: [Refactoring suggestion]

## 💡 Improvements (Clean Code / Logging)
- **[Area]**: [Naming, pattern refinement, or log structure]
  - **Action**: [Brief suggestion]

---
**Verdict**: [Pass / Pass with reservations / Needs major refactoring]
```

### Artifacts Generation
After generating the report, the agent MUST:
1.  **Save the Report**: Write the full report to `build/reports/full-code-review-report.md`.
2.  **Generate Tasks**: Create a `build/reports/review-tasks.md` file using the `task` artifact type. This file must contain a concrete implementation plan with checkboxes for each item found in the report, categorized by priority.
3.  **Request Approval**: Present the `review-tasks.md` artifact to the user and explicitly wait for their review and approval. **DO NOT start executing any task until the user gives explicit consent.**

## Workflow for the Agent

1.  **Analyze**: Perform a quick scan of the targeted codebase.
2.  **Orchestrate**: Mentally run through the checklists of all 6 sub-skills in the specified order.
3.  **Filter**: Discard all findings that represent "good practices already followed".
4.  **Synthesize**: Group findings into the categories above.
5.  **Report**: Deliver the lean report in the chat.
6.  **Persist**: Create the markdown report and task list artifacts in the `build/reports` directory.
7.  **Review & Approval**: Present the Task List artifact on screen and request the user's review. Stop and wait for the user to approve the plan or suggest changes before proceeding with any implementation.
8.  **Validate before Commit**: Before each commit, ALWAYS run `./gradlew clean compileJava test` to ensure stability. No commit should be made if the build or tests fail.

## Token Optimization
- Do not quote large blocks of code unless necessary for context.
- Use line number references `[file.java:12-15]`.
- Use bullet points for high density of information.

