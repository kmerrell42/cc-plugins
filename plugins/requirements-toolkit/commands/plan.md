---
name: plan
description: Create detailed implementation plan based on task requirements and scouting information
argument-hint: '"task description"'
tools: TodoWrite,Read,Grep,Glob
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
- Use TodoWrite tool to create structured task list
- Each task should be specific and testable
- Include file paths and function names where applicable
- Estimate complexity (simple/moderate/complex) for each task
- Identify dependencies between tasks

**Plan Structure:**
1. Preparation steps (setup, research verification)
2. Core implementation tasks
3. Integration and testing tasks
4. Validation and cleanup tasks

DO NOT proceed to implementation. Only create the plan.
