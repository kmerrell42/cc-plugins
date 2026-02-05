---
name: scout-plan-build
description: Complete development workflow - scout information, create plan, then build implementation
argument-hint: '"task description" [optional: agent_count]'
tools: SlashCommand,TaskCreate,TaskUpdate,TaskList,TaskGet
---

**Arguments:**
- TASK_DESCRIPTION = $1
- SCOUT_AGENT_COUNT = $2 (defaults to 4 if not provided)

Execute complete development workflow for: TASK_DESCRIPTION

**Parameters:**
- **Task description**: TASK_DESCRIPTION (required)
- **Agent count for scouting**: SCOUT_AGENT_COUNT (optional, defaults to 4)

**Usage examples:**
- `/scout-plan-build "implement dark mode"` - Uses 4 agents (default)
- `/scout-plan-build "implement dark mode" 2` - Uses 2 agents for scouting

This command orchestrates a three-phase development process that runs autonomously to completion:

## Phase 1: Scout
Gather all prerequisite information needed for planning.

Determine agent count: Use SCOUT_AGENT_COUNT if provided, otherwise default to 4 agents.

Execute: `/scout "TASK_DESCRIPTION" SCOUT_AGENT_COUNT` (or `/scout "TASK_DESCRIPTION" 4` if SCOUT_AGENT_COUNT not provided)

Once scouting completes, immediately proceed to Phase 2.

---

## Phase 2: Plan
Create detailed implementation plan based on scouted information.

Execute: `/plan "TASK_DESCRIPTION"`

Once planning completes, immediately proceed to Phase 3.

---

## Phase 3: Build
Execute the implementation following the established plan.

Execute: `/build "TASK_DESCRIPTION"`

Once build completes, workflow is finished.

---

**Workflow Rules:**
- Complete each phase fully before moving to the next
- Proceed automatically between phases without user approval
- Use scouting information to inform planning
- Use plan to guide implementation
- Update todo list throughout build phase
- Validate work at each step

Begin with Phase 1: Scouting and continue through all phases to completion.
