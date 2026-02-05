---
name: evaluate-against-requirements
description: Evaluate completed work against original requirements to verify all acceptance criteria are met and work stayed in scope
argument-hint: '[requirements-source]'
---

**Arguments:**
- REQUIREMENTS_SOURCE = $1 (optional: file path, ticket reference, or inline description)

**Input formats:**
- No args (detect automatically): `/evaluate-against-requirements`
- File path: `/evaluate-against-requirements ./docs/requirements.md`
- Ticket reference: `/evaluate-against-requirements LINEAR-123`
- Inline text: `/evaluate-against-requirements "User authentication with JWT"`

## Evaluation Process

### Step 1: Locate Requirements

**Auto-detection (if no args provided):**
1. Check for `.local.plan/requirements.md` or similar in workspace
2. Check for Linear ticket reference in recent git commits
3. Check for requirements in project docs
4. If none found, ask user to provide requirements source

**File path provided:**
- Read requirements from specified file
- Extract all acceptance criteria and functional requirements

**Ticket reference provided:**
- Fetch from Linear API using Bash tool
- Extract description, acceptance criteria, sub-issues
- Note: Requires LINEAR_API_KEY configured

**Inline text:**
- Use provided text as requirements baseline
- Note: Less detailed evaluation possible

### Step 2: Identify Changes to Evaluate

**Default: Uncommitted changes**
```bash
git diff --name-only
git diff
```

**Alternative: Specific commit**
```bash
git diff --name-only <commit>^..<commit>
git diff <commit>^..<commit>
```

**Alternative: Commit range**
```bash
git diff --name-only <commit1>..<commit2>
git diff <commit1>..<commit2>
```

**Alternative: Current branch vs base**
```bash
git diff --name-only main...HEAD
git diff main...HEAD
```

**Ask user which scope to evaluate:**
- Uncommitted changes (default)
- Last commit
- Specific commit SHA
- Commit range
- Branch changes

### Step 3: Extract Comprehensive Acceptance Criteria

**IMPORTANT: Use Task tool to delegate this extraction to a sub-task.**

Create a new task with the following objective:

```
Task: Extract all acceptance criteria from requirements source

Input: [Full requirements content from Step 1]

Instructions:
Parse the requirements and produce an exhaustive, unambiguous list of acceptance criteria. Consider:

**Extract everything explicitly stated (regardless of format):**
- Acceptance criteria in any form (bullets, narrative, embedded in description)
- Functional requirements described in text or lists
- Technical constraints mentioned anywhere in requirements
- Success metrics or quality standards specified
- Edge cases described in any section
- Error conditions required by the specification
- Scope boundaries (included/excluded features stated anywhere)

**Key principle: If it's written in the requirements, extract it.**

**DO NOT add requirements based on:**
- Your judgment of what "should" be included
- Industry best practices not mentioned
- Assumed edge cases not described
- Inferred quality standards
- Implementation approaches (unless specified as constraint)

Output format (simple numbered list):
1. [Clear, testable acceptance criterion]
2. [Clear, testable acceptance criterion]
3. [Clear, testable acceptance criterion]
...

OUT OF SCOPE:
- [Explicitly excluded item]
- [Explicitly excluded item]

Each criterion must be:
- Found in the requirements text (bullets, paragraphs, anywhere)
- Testable and observable
- Clear paraphrase of what was written

Examples of valid extraction:
- "Users can log in with email" → Extract: "User login with email functionality"
- "The system should handle up to 1000 concurrent users" → Extract: "Support 1000 concurrent users"
- "Don't implement password reset in v1" → Extract to OUT OF SCOPE: "Password reset"

Do NOT add:
- Your opinion on what's missing
- Best practices not mentioned
- Assumed requirements
- Code quality standards unless specified
```

**Wait for task completion.** Use the output list as the definitive acceptance criteria for evaluation in Step 4.

**If no clear acceptance criteria can be extracted:**
- Report this to user
- Explain why requirements are insufficient for evaluation
- Recommend running `/analyze-requirements` first

### Step 4: Analyze Implementation Against Each Expectation

For each requirement (REQ-X) and acceptance criterion (AC-Xa):

**Evidence-based evaluation:**
1. Read changed files identified in git diff
2. Search for implementation of specific requirement
3. Verify behavior matches expected outcome
4. Check edge cases are handled
5. Confirm no scope creep beyond requirement

**Pass criteria:**
- Functionality exists in changed files
- Behavior matches specification exactly
- All stated acceptance criteria satisfied
- Implementation confined to requirement scope

**Fail criteria:**
- Functionality missing entirely
- Partial implementation (some AC met, others not)
- Behavior differs from specification
- Scope creep (added unrelated features)
- Related AC not addressed

**Defer criteria (not pass/fail):**
- Requirement is ambiguous (document this)
- Cannot verify from static code analysis alone
- Requires runtime testing user must perform

### Step 5: Output Evaluation Results

Use this exact template format:

```markdown
# Work Evaluation: [Requirements Source]

## Evaluation Scope
**Changes analyzed:** [Uncommitted/Commit SHA/Branch range]
**Files modified:** [count] files
**Requirements source:** [File/Ticket/Inline]

---

## Requirements Evaluation

### ✅ SATISFIED ([X]/[Total])

**REQ-1: [Requirement title]**
- Implementation: [File:line reference where implemented]
- Evidence: [Brief description of what was done]

**REQ-2: [Requirement title]**
- Implementation: [File:line references]
- Evidence: [Brief description]

---

### ❌ NOT SATISFIED ([Y]/[Total])

**REQ-N: [Requirement title]**
- **Missing**: [Specific functionality not implemented]
- **Expected**: [What acceptance criteria stated]
- **Found**: [What actually exists, if anything]
- **Action needed**: [Concrete step to satisfy this requirement]

**REQ-M: [Requirement title]**
- **Partial implementation**: [What was done]
- **Missing acceptance criteria**: AC-Ma, AC-Mb
- **Action needed**: [Specific additions required]

---

### ⚠️ SCOPE VIOLATIONS ([Z] items)

**SV-1: [Feature/code added outside requirements]**
- **Location**: [File:line]
- **Issue**: [What was added that wasn't requested]
- **Recommendation**: Remove or document as intentional enhancement

---

### 🔍 NEEDS VERIFICATION ([N] items)

**NV-1: [Requirement that needs runtime testing]**
- **Cannot verify statically**: [Why code inspection insufficient]
- **Test procedure**: [Steps user must perform to verify]
- **Success criteria**: [What indicates this passes]

---

## Summary

**Overall Status:** [PASS ✅ | FAIL ❌ | PARTIAL ⚠️]

**Completion Score:** [X satisfied] / [Total requirements] ([percentage]%)

**Verdict:**
- Ready for PR: [YES/NO]
- Reason: [One line explanation]

**Actions Required:** (only if FAIL/PARTIAL)
1. [Specific action for REQ-N]
2. [Specific action for REQ-M]

**Recommendations:**
- [Any suggestions for improvement, even if requirements met]
```

### Output Rules

**For SATISFIED requirements:**
- List file:line where implementation exists
- Keep evidence brief (1 line)
- No need to explain HOW it was done, just confirm it WAS done

**For NOT SATISFIED requirements:**
- Be extremely specific about what's missing
- Quote the acceptance criterion that failed
- Provide concrete action to satisfy (not vague "implement X")

**For SCOPE VIOLATIONS:**
- Only report if code adds features NOT in requirements
- Distinguish between necessary refactoring vs scope creep
- Don't report internal implementation details as violations

**For NEEDS VERIFICATION:**
- Only list items that cannot be verified from code inspection
- Provide exact test steps user should perform
- Define clear pass/fail criteria

**Overall Status determination:**
- PASS ✅: All requirements satisfied, zero scope violations
- FAIL ❌: One or more requirements not satisfied
- PARTIAL ⚠️: Most requirements met but some gaps or scope issues

## Constraints

- **NEVER pass a requirement unless ALL its acceptance criteria are met**
- **NEVER fail a requirement for code quality issues** (that's code review's job)
- **NEVER fail for missing tests** (unless tests are explicit requirement)
- **NEVER assume intent** - only evaluate against stated requirements
- **ALWAYS distinguish** between "not implemented" vs "implemented differently than specified"
- **FOCUS ON**: Did the work deliver what was requested, nothing more, nothing less?

## Edge Cases

**Requirements are vague/incomplete:**
- List as NEEDS VERIFICATION
- Note the ambiguity
- Suggest clarifying requirement before proceeding

**Implementation exceeds requirements (scope creep):**
- List as SCOPE VIOLATION
- Note: May be intentional improvement
- Let user decide if it should stay

**Cannot access changed files:**
- Request user specify correct git reference
- Explain what range you attempted to analyze

**Requirements contradict each other:**
- Flag contradiction in output
- Do not attempt to resolve
- Ask user for clarification

**Zero changes detected:**
- Report no changes found
- Ask user to specify correct scope
- Verify git status

## Success Criteria

You've succeeded when:
1. Every requirement has explicit PASS/FAIL/VERIFY status
2. All failures include specific actions to resolve
3. User can immediately know if work is PR-ready
4. No ambiguity remains about completion status

You've failed if:
- Output says "mostly satisfied" without specifics
- Failures don't include concrete next actions
- Mixing code quality feedback with requirement satisfaction
- User must re-read requirements to understand what's missing
