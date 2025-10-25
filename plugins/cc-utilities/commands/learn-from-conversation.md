---
name: learn-from-conversation
description: "Analyze current conversation to extract user expectations and behavioral patterns, then update CLAUDE.md with actionable insights for improving future interactions."
tools: Read,Edit,Grep
---

**Arguments:**
- FOCUS_AREA = $1 (optional: specific aspect to analyze, e.g., "workflow automation", "code review", "agent design")

**IMPORTANT**: This command analyzes conversations and updates the **user-level memory file** at `~/.claude/CLAUDE.md` (not project-specific CLAUDE.md files). This ensures learnings apply across all projects.

Analyze the current conversation to derive learnings and update `~/.claude/CLAUDE.md` with improved behavioral guidance.

## Objective

Extract actionable insights from user interactions to help Claude better understand and meet user expectations in future conversations. Focus on:
- User's preferred communication style and level of detail
- Patterns in what the user requests vs. what they actually want
- Successful interaction patterns that should be reinforced
- Failed interactions that need correction or refinement
- Implicit expectations not explicitly stated
- Workflow preferences and automation opportunities

## Analysis Process

### Step 1: Conversation Analysis

**Identify Positive Interactions** (what worked well):
- User expressed satisfaction, approval, or confirmation
- User provided positive feedback ("good", "perfect", "exactly", "yes")
- User accepted suggestions without modification
- Workflow completed smoothly without backtracking
- User's implicit needs were correctly anticipated
- Efficient exchanges with minimal clarification needed

**Identify Negative Interactions** (what needs improvement):
- User expressed confusion, frustration, or dissatisfaction
- User had to clarify, correct, or redirect multiple times
- User said "no", "wait", "that's wrong", or similar corrections
- Misalignment between what was delivered vs. what was wanted
- Unnecessary verbosity or insufficient detail
- Missed implicit expectations or context
- Repeated patterns of miscommunication

**Extract Behavioral Patterns**:
- What types of requests does this user make frequently?
- What level of autonomy does the user expect?
- Does the user prefer step-by-step guidance or autonomous execution?
- What communication style resonates (concise, detailed, technical, casual)?
- What implicit assumptions can be made about this user's preferences?
- What workflows or tasks are repetitive and could be optimized?

### Step 2: Review Existing User-Level CLAUDE.md

**IMPORTANT**: Read `~/.claude/CLAUDE.md` (user-level memory file) to check:
- Does existing guidance already address the identified patterns?
- If yes, did it work? If not, why did it fail?
- Are there conflicting instructions that need reconciliation?
- Are there gaps where new guidance is needed?
- Is existing guidance too vague or too prescriptive?

### Step 3: Synthesize Learnings

For each identified pattern, create an actionable learning:

**Format:**
```markdown
## Learning: [Brief Title]

**Context**: [What situation triggered this learning]

**User Expectation**: [What the user actually wanted]

**Current Behavior**: [What Claude did or what `~/.claude/CLAUDE.md` says]

**Recommended Update**: [Specific instruction to add/modify in `~/.claude/CLAUDE.md`]

**Rationale**: [Why this change will improve future interactions]
```

**Quality Criteria for Learnings**:
- Specific and actionable (not vague aspirations)
- Based on repeated patterns, not one-off requests
- Generalizable to future similar situations
- Does not conflict with existing successful patterns
- Clear success criteria (how to know if it's working)

### Step 4: Update User-Level CLAUDE.md

**IMPORTANT**: All updates go to `~/.claude/CLAUDE.md` (user-level memory file).

**For New Learnings** (not covered in existing `~/.claude/CLAUDE.md`):
- Add new section or subsection with clear heading
- Use concrete, directive language ("DO X", "NEVER Y", "ALWAYS Z")
- Include examples showing desired behavior
- Place in relevant section (don't create new top-level sections unless necessary)

**For Refinements** (existing guidance that failed):
- Identify the existing instruction that didn't work
- Analyze why it failed (too vague, wrong priority, conflicting with other rules)
- Rewrite with more specificity, examples, or priority indicators
- Add "IMPORTANT:" or "SO IMPORTANT:" if critical
- Consider adding negative examples ("NOT like this")

**For Conflicts** (new learning contradicts existing):
- Evaluate which pattern is more recent or more frequent
- Consider if both can coexist with conditions (e.g., "When X, do A; when Y, do B")
- If one must override, clearly mark deprecated guidance
- Document the context for the change

### Step 5: Validation

Before finalizing updates, check:
- [ ] Learning is based on pattern, not single instance
- [ ] Instruction is clear and unambiguous
- [ ] No conflicts with other sections in `~/.claude/CLAUDE.md`
- [ ] Specific enough to change behavior
- [ ] Includes examples or success criteria
- [ ] Placed in logical location within `~/.claude/CLAUDE.md`

## Output Format

Present findings before updating:

```markdown
# Conversation Analysis Summary

## Positive Patterns Identified
1. [Pattern] - [Why it worked]
2. [Pattern] - [Why it worked]

## Negative Patterns Identified
1. [Pattern] - [Why it failed]
2. [Pattern] - [Why it failed]

## Proposed Updates to ~/.claude/CLAUDE.md

### New Section: [Title]
**Location**: [Where in `~/.claude/CLAUDE.md`]
**Content**:
```
[Exact text to add]
```

### Refinement: [Existing Section Title]
**Current text**:
```
[What it says now]
```
**Proposed change**:
```
[Updated text]
```
**Reason**: [Why this change addresses the pattern]

## Changes Not Recommended
- [Pattern] - [Why not actionable or not adding to `~/.claude/CLAUDE.md`]
```

**IMPORTANT: Wait for user approval before making changes to `~/.claude/CLAUDE.md`**

## Constraints

- NEVER add learnings based on single interactions (need pattern confirmation)
- NEVER create conflicting rules without resolving the conflict
- NEVER be vague ("try to understand user better" ❌, "When user says X, do Y" ✅)
- ALWAYS provide specific examples in new instructions
- ALWAYS check existing `~/.claude/CLAUDE.md` for related guidance
- ALWAYS update `~/.claude/CLAUDE.md` (user-level), not project-specific CLAUDE.md files
- IMPORTANT: Focus on behavioral patterns, not content preferences
- IMPORTANT: Prioritize systematic improvements over one-off fixes

## Example Scenarios

**Good Learning**:
- "User frequently corrects verbosity. Add: 'Keep responses under 3 sentences unless complexity requires more detail.'"
- "User expects autonomous execution after approval. Add: 'After plan approval, proceed through all steps without requesting permission for each step.'"

**Bad Learning**:
- "User wanted dark mode feature" (too specific, not behavioral)
- "Be more helpful" (too vague, not actionable)
- "User likes Python" (content preference, not behavior pattern)

## Success Metrics

After updates, future conversations should show:
- Fewer corrections and clarifications needed
- Better anticipation of implicit needs
- Improved alignment between delivered and expected output
- Reduced friction in common workflows
- User expresses satisfaction more frequently
