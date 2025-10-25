---
name: build
description: Execute implementation based on the established plan, with built-in code review loop
argument-hint: '"task description"'
tools: Read,Write,Edit,Bash,Grep,Glob,TodoWrite,SlashCommand
---

**Arguments:**
- TASK_DESCRIPTION = $1

Execute the implementation for: TASK_DESCRIPTION

**Usage example:**
- `/build "implement dark mode toggle"`

---

## Build Process

The build phase follows an implement-review-fix loop to ensure quality code:

```
Step 1: Implement
    ↓
Step 2: Code Review
    ↓
    ├─ PASS → Build Complete
    └─ ISSUES FOUND → Add to todo → Loop to Step 1
```

---

## Step 1: Implementation

**Objectives:**
1. Follow the established plan systematically
2. Implement changes incrementally
3. Test after each significant change
4. Update todo list as you progress
5. Handle errors and adjust as needed

**Implementation Guidelines:**
- Mark tasks as in_progress before starting
- Mark tasks as completed immediately after finishing
- Write clean, maintainable code following project conventions
- Add appropriate error handling
- Include comments for complex logic
- Run tests/build to validate changes

**Quality Standards:**
- Code should match existing style and patterns
- All functionality should be tested
- No broken builds or failing tests
- Proper error handling and edge cases

**When implementation is complete**, proceed immediately to Step 2: Code Review.

---

## Step 2: Code Review (AUTONOMOUS)

**Objective**: Validate code quality before considering build complete.

**Execute**: `/review-code`

This runs the code-reviewer agent on changed files to identify:
- Code quality issues
- Logic errors
- Security vulnerabilities
- Style inconsistencies
- Best practice violations

**Review outcomes**:

### Outcome A: PASS (No significant issues)
- Build phase complete
- Code meets quality standards
- Ready to proceed to next workflow phase

### Outcome B: ISSUES FOUND
Code review identifies problems that need fixing.

**Action - Automatic Fix Loop**:

1. **Extract issues from review**:
   - Categorize by severity (critical, important, minor)
   - Focus on critical and important issues
   - Note specific file locations and problems

2. **Append issues to todo list** using TodoWrite:
   ```
   For each critical/important issue:
   - Add todo: Fix [specific issue] in [file:line]
   - Mark as pending
   - Include context from review
   ```

3. **Loop back to Step 1 (Implementation)**:
   - Address each todo item
   - Fix code issues identified in review
   - Mark todos as completed as fixes are applied
   - After all fixes complete, return to Step 2

4. **Re-review**:
   - Execute `/review-code` again
   - Check if issues are resolved
   - Repeat loop if new issues found

**Loop termination**:
- Maximum 3 code-review loops
- If issues persist after 3 loops, report to user and continue
- Minor issues may be acceptable to proceed

**Issue severity handling**:
- **Critical issues** (security, logic errors): MUST fix, blocks completion
- **Important issues** (code quality, maintainability): Should fix in loop
- **Minor issues** (style preferences, suggestions): May defer

---

## Build Completion Criteria

Build is only complete when:
1. ✅ All planned functionality implemented
2. ✅ Tests/build passing
3. ✅ Code review passes (no critical/important issues)
4. ✅ All todos marked completed

If any criterion fails, continue working until all are satisfied.

---

## Example Execution

```
User: /build "implement dark mode toggle"

[Step 1: Implementation]
Implementing dark mode toggle...
- Created DarkModeToggle component
- Added state management
- Updated theme provider
- Added CSS variables
All todos completed.

[Step 2: Code Review]
Running code review on changed files...
Review found 2 important issues:
- Missing error handling in theme provider
- CSS variable naming inconsistency

Adding issues to todo list and fixing...

[Step 1: Implementation - Loop 1]
Fixing: Add error handling in theme provider...
Fixing: Standardize CSS variable names...
All fixes completed.

[Step 2: Code Review - Loop 1]
Re-running code review...
Review PASS: No significant issues found.

Build complete! Ready for next phase.
```

---

Begin with Step 1: Implementation, then automatically proceed through review loop.
