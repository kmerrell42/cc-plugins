---
name: build-from-requirements
description: Complete requirements-driven development workflow - analyze requirements, scout-plan-build implementation, then validate against original requirements
argument-hint: '"requirements source"'
tools: SlashCommand,TodoWrite
---

**Arguments:**
- REQUIREMENTS_SOURCE = $1 (file path, ticket reference, or inline description)
- SCOUT_AGENT_COUNT = $2 (optional, defaults to 4)

**Input formats:**
- File path: `/build-from-requirements ./docs/requirements.md`
- Ticket reference: `/build-from-requirements LINEAR-123`
- Inline text: `/build-from-requirements "Build user auth with JWT"`
- With agent count: `/build-from-requirements LINEAR-123 6`

**Usage examples:**
- `/build-from-requirements ./requirements.md` - Uses 4 agents (default)
- `/build-from-requirements LINEAR-123 2` - Linear ticket with 2 scout agents
- `/build-from-requirements "Add dark mode toggle"` - Inline requirements

This command orchestrates a complete requirements-driven development workflow with automatic validation and gap-closing loops.

---

## Workflow Overview

```
Phase 0: Analyze Requirements (Interactive - User Approval Required)
    ↓
Phase 1-3: Scout-Plan-Build (Autonomous)
    ↓
Phase 4: Evaluate Against Requirements (Autonomous)
    ↓
    ├─ PASS → Workflow Complete
    └─ FAIL → Append gaps to todo → Loop back to Phase 3 (Build)
```

---

## Phase 0: Analyze Requirements (INTERACTIVE)

**Objective**: Clarify all ambiguities and gaps in requirements before implementation.

**Execute**: `/analyze-requirements "REQUIREMENTS_SOURCE"`

**Process**:
1. Analyze requirements for gaps, ambiguities, missing acceptance criteria
2. Engage user interactively to resolve each issue
3. Track resolutions and build clarified requirements
4. Offer to update source document if applicable

**Output**: Clarified requirements with all ambiguities resolved

**CHECKPOINT: Wait for user approval before proceeding**

User must explicitly approve to continue (commands like "proceed", "continue", "do it", etc.)

**Track requirements context**:
- Store the original REQUIREMENTS_SOURCE reference
- If requirements were updated, note the updated source location
- Keep full clarified requirements text in memory for subsequent phases

---

## Phase 1-3: Scout-Plan-Build (AUTONOMOUS)

**Objective**: Gather information, create plan, implement solution.

**Determine agent count**: Use SCOUT_AGENT_COUNT if provided, otherwise default to 4.

**Execute**: `/scout-plan-build "REQUIREMENTS_SOURCE" SCOUT_AGENT_COUNT`

**Context to maintain**:
- Pass clarified requirements to all phases
- Ensure plan addresses all acceptance criteria
- Build implements complete plan

**Proceed automatically** through all three phases (Scout → Plan → Build) without user approval.

Once build completes, immediately proceed to Phase 4.

---

## Phase 4: Evaluate Against Requirements (AUTONOMOUS)

**Objective**: Validate implementation satisfies all requirements.

**Execute**: `/evaluate-against-requirements REQUIREMENTS_SOURCE`

**Use the requirements source from Phase 0**:
- If file path was provided: use that path
- If ticket reference: use that ticket ID
- If inline text: use the full clarified requirements text from Phase 0
- If source was updated in Phase 0: use the updated location

**Evaluation determines next action**:

### Scenario A: PASS ✅
All requirements satisfied, zero critical gaps.

**Action**:
- Present completion summary
- Workflow complete
- Ready for code review/PR

### Scenario B: FAIL ❌ or PARTIAL ⚠️
One or more requirements not satisfied.

**Action - Automatic Gap Resolution Loop**:

1. **Extract gaps from evaluation**:
   - List all NOT SATISFIED requirements
   - Identify specific missing functionality
   - Note concrete actions needed

2. **Append gaps to todo list** using TodoWrite:
   ```
   For each unsatisfied requirement:
   - Add todo: [Specific action needed from evaluation]
   - Mark as pending
   ```

3. **Loop back to Phase 3 (Build)**:
   - Execute `/build "REQUIREMENTS_SOURCE"` with updated todo list
   - Implementation should address all pending todos
   - After build completes, return to Phase 4 (this step)

4. **Re-evaluate**:
   - Execute `/evaluate-against-requirements REQUIREMENTS_SOURCE` again
   - Check if gaps are now satisfied
   - Repeat loop if new gaps remain

**Loop termination**:
- Maximum 3 gap-resolution loops
- If still failing after 3 loops, present status to user and stop
- User can manually continue or modify requirements

---

## Context Propagation Rules

**Requirements must flow through all phases:**

1. **Phase 0 → Phase 1-3**:
   - Pass clarified requirements to scout-plan-build
   - Scout agents search with requirements in mind
   - Plan must address all acceptance criteria
   - Build implements against requirements

2. **Phase 0 → Phase 4**:
   - Evaluation uses same source as analysis
   - Use clarified version if requirements were updated
   - Full context of what was clarified available for evaluation

3. **Phase 4 (loop) → Phase 3**:
   - Gaps from evaluation become specific todos
   - Requirements context maintained
   - Build focuses on remaining gaps

**Internal tracking variables**:
- `original_source`: Initial REQUIREMENTS_SOURCE argument
- `clarified_requirements`: Full text after Phase 0 resolution
- `updated_source_location`: If file/ticket was updated in Phase 0
- `loop_count`: Track gap-resolution iterations (max 3)

---

## Workflow Rules

**Autonomous execution**:
- Only stop for user approval after Phase 0
- All other phases proceed automatically
- Gap-resolution loops happen without user intervention

**Failure handling**:
- Phase 0 issues: User must resolve interactively
- Phase 1-3 errors: Standard error handling (stop on critical failures)
- Phase 4 failures: Automatic loop to Phase 3 with gap todos

**Quality gates**:
- Phase 0: All ambiguities resolved
- Phase 3: Build completes successfully
- Phase 4: All requirements satisfied (PASS status)

**Todo list management**:
- Phase 3 (initial): Standard build todos
- Phase 4 (gaps found): Append missing requirement todos
- Phase 3 (loop): Address gap todos from evaluation
- Clean completed todos as work progresses

---

## Success Criteria

Workflow succeeds when:
1. ✅ Requirements fully analyzed and clarified (Phase 0)
2. ✅ Implementation complete (Phase 3)
3. ✅ All acceptance criteria satisfied (Phase 4 PASS)
4. ✅ Zero scope violations
5. ✅ Ready for code review

Workflow needs attention when:
- ⚠️ Exceeds 3 gap-resolution loops
- ⚠️ Requirements cannot be clarified in Phase 0
- ⚠️ Build phase encounters critical errors
- ⚠️ Evaluation finds scope violations

---

## Example Execution

```
User: /build-from-requirements LINEAR-456

[Phase 0: Analyze Requirements]
Claude: Analyzing LINEAR-456...
Found 3 ambiguities, 2 missing acceptance criteria.
[Interactive clarification with user]
All issues resolved. Updated ticket with clarifications.

Ready to proceed with implementation. Continue?

User: yes

[Phase 1: Scout]
Launching 4 agents to gather information...
Scout complete. Found relevant patterns in auth/ and middleware/.

[Phase 2: Plan]
Creating implementation plan...
Plan created: 8 steps to implement JWT authentication.

[Phase 3: Build]
Implementing step 1/8...
[... builds through all steps ...]
Build complete.

[Phase 4: Evaluate]
Evaluating against LINEAR-456 requirements...
Status: FAIL (4/6 requirements satisfied)

NOT SATISFIED:
- REQ-3: Token refresh endpoint missing
- REQ-5: Rate limiting not implemented

Adding gaps to todo list and resuming build...

[Phase 3: Build - Loop 1]
Implementing todo: Add token refresh endpoint...
Implementing todo: Add rate limiting...
Build complete.

[Phase 4: Evaluate - Loop 1]
Re-evaluating against LINEAR-456 requirements...
Status: PASS ✅ (6/6 requirements satisfied)

Workflow complete! All requirements satisfied.
Ready for code review.
```

---

Begin with Phase 0: Requirements Analysis.
