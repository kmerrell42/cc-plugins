---
name: plan
description: Create detailed implementation plan based on task requirements and scouting information
argument-hint: '"task description"'
tools: TaskCreate,TaskUpdate,TaskList,TaskGet,Read,Grep,Glob
---

**Arguments:**
- TASK_DESCRIPTION = $1

Create a detailed implementation plan for: TASK_DESCRIPTION

**Usage example:**
- `/plan "implement dark mode toggle"`

**Planning Objectives:**
1. Break down the task into specific, actionable steps
2. Identify files that need to be created or modified
3. Determine the order of implementation
4. Note any potential risks or challenges
5. Define success criteria

**Output Requirements:**
- **MUST use TaskCreate tool** to create all plan items as tasks
- **DO NOT create plan files** (no .md files, no plan documents) - the task list IS the plan
- Each task should be specific and testable
- Include file paths and function names in task descriptions where applicable
- Note complexity (simple/moderate/complex) in task descriptions
- Use TaskUpdate with `addBlockedBy` to establish task dependencies

**Plan Structure (create tasks in this order):**
1. Preparation steps (setup, research verification)
2. Core implementation tasks
3. Integration and testing tasks
4. Validation and cleanup tasks

**Task Creation Guidelines:**
- Use clear, imperative subjects: "Add authentication middleware to API routes"
- Include `activeForm` for progress display: "Adding authentication middleware"
- Put detailed context in the `description` field
- Set up `blockedBy` relationships for sequential dependencies

DO NOT proceed to implementation. Only create the plan as tasks.
