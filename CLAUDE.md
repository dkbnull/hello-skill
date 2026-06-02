# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

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

## 5. Respond in Chinese

- **All responses must be in Chinese**, including explanations, suggestions, error analysis, etc.
- Code comments should be in Chinese (unless the user explicitly requests another language).
- Variable names, function names, and other code identifiers follow existing project conventions and are not affected by this rule.
- String content in code is determined by business needs and is not affected by this rule.

## 6. Auto-Load Relevant SKILLs

Before responding to user requests, you must **automatically identify and load** the most relevant SKILL based on the task context. The identification logic is as follows:

### Identification Principles

1. **Priority Matching**: Match the corresponding SKILL based on the technical domain, framework, and language of the user's request.
2. **Multi-SKILL Combination**: A task may involve multiple SKILLs; load them in combination as needed.
3. **Generic SKILL Fallback**: When an exact match is not possible, load a generic SKILL (e.g., code-generation, code-review, debug, etc.).

### Matching Map

| Task Type | Possible SKILLs |
|-----------|-----------------|
| Writing new features / generating code | code-generation, requirement-analysis |
| Code review / quality check | code-review, defensive-programming |
| Bug fix / troubleshooting | debug, error-handling |
| Performance optimization | performance, refactoring |
| Code refactoring | refactoring, design-patterns |
| API design | api-design, role-switch (Architect) |
| Technical documentation | tech-doc |
| Requirement analysis | requirement-analysis, structured-thinking |
| Frontend development | Corresponding framework SKILL, corresponding language SKILL |
| Backend development | Corresponding framework SKILL, corresponding language SKILL |
| Database operations | Corresponding database SKILL |
| DevOps / Deployment | Corresponding tool SKILL |
| Mobile / Desktop | Corresponding platform SKILL |
| Specific programming language | Corresponding language SKILL |

### Execution Flow

1. **Analyze Request**: Understand the technical domain and intent of the user's current task.
2. **Match SKILL**: Find the most relevant SKILL based on the mapping table above.
3. **Load SKILL**: Invoke the Skill tool to load the corresponding SKILL and obtain professional guidance.
4. **Follow SKILL Specifications**: Complete the task under the guidance of the SKILL.
5. **Role Declaration**: If role-switch is involved, declare the role at the beginning of the response in the format `【Current Role: XXX】`.

### Examples

- User says "Help me write a React login page" → Load `react` SKILL + `code-generation` SKILL
- User says "This Java code has a bug" → Load `debug` SKILL + `java` SKILL
- User says "Help me design a RESTful API" → Load `api-design` SKILL
- User says "Optimize this SQL query" → Load `performance` SKILL + `mysql` (or other database) SKILL
- User says "Write a user management API with Spring Boot" → Load `springboot` SKILL + `java` SKILL + `code-generation` SKILL
- User says "This Spring Boot project's API response is slow" → Load `springboot` SKILL + `performance` SKILL
- User says "Help me refactor this Java project" → Load `java` SKILL + `refactoring` SKILL + `design-patterns` SKILL

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.
