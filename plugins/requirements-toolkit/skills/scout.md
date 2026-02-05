---
name: scout
description: Gather prerequisite information needed for planning with concise, structured output citing sources
argument-hint: '"task description" [optional: agent_count]'
tools: Read,Grep,Glob,Bash,WebSearch,WebFetch,Task
---

**Arguments:**
- TASK_DESCRIPTION = $1
- AGENT_COUNT = $2 (defaults to 1 if not provided)

Scout and gather all prerequisite information needed to plan: TASK_DESCRIPTION

**Agent Configuration:**
- **Task description**: TASK_DESCRIPTION (required)
- **Agent count**: AGENT_COUNT (optional, defaults to 1)

**Usage examples:**
- `/scout "implement dark mode"` - Uses 1 agent (default)
- `/scout "implement dark mode" 4` - Uses 4 parallel agents
- `/scout "refactor auth system" 2` - Uses 2 parallel agents

**Scouting Strategy:**
Determine agent count from AGENT_COUNT parameter (default to 1 if not provided).

When using multiple agents, divide the scouting work across parallel agents using the Task tool with subagent_type="general-purpose":
- Agent 1: File and pattern discovery (Glob, Grep for relevant code)
- Agent 2: Dependency and architecture analysis (Read package files, configs)
- Agent 3: Similar implementation patterns (search for existing similar features)
- Agent 4: Constraints and requirements discovery (docs, tests, conventions)

Each agent should run in parallel and return concise, structured findings.

**Scouting Objectives:**
1. Identify relevant files, functions, and code patterns
2. Understand existing architecture and conventions
3. Locate dependencies and related components
4. Find similar implementations or patterns in the codebase
5. Identify potential constraints or requirements

**Output Requirements:**
- Concise, structured format (use lists, tables, or YAML)
- Cite specific sources for every finding (file paths with line numbers)
- Focus only on information applicable to the task
- No verbose explanations - just facts and locations
- Group findings by category (files, patterns, dependencies, etc.)

**Format Example:**
```yaml
relevant_files:
  - path/to/file.ts:123 - [brief description]

patterns_found:
  - path/to/example.ts:45-67 - [pattern description]

dependencies:
  - package-name: [usage context]

constraints:
  - [constraint with source citation]
```

**Consolidation:**
After all agents complete, synthesize their findings into a single, organized report following the format above. Remove duplicates and organize by category.

DO NOT proceed to planning or implementation. Only gather and report information.
