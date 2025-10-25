# Claude Plugins Personal

Personal Claude Code plugin marketplace featuring curated command collections and specialized agents for software development workflows.

## Plugin Collections

### 🎯 requirements-toolkit
**Comprehensive requirements analysis and planning workflow**

**Commands:**
- `/analyze-requirements` - Systematically analyze task requirements to identify ambiguities, gaps, and unclear acceptance criteria
- `/scout` - Gather prerequisite information needed for planning with concise, structured output citing sources
- `/plan` - Create detailed implementation plan based on task requirements and scouting information
- `/scout-plan-build` - Complete development workflow - scout information, create plan, then build implementation

**Agents:**
- `requirement-analyzer` - Senior engineer requirement analysis specialist for technical recommendations
- `adhoc-requirements-analyzer` - Requirements specification expert for adhoc development tasks

**Use Cases:**
- Analyzing user stories and feature requests before implementation
- Gathering context about existing codebase before making changes
- Creating structured implementation plans
- End-to-end requirement-to-implementation workflows

---

### 🚀 build-execution-suite
**Complete build lifecycle management from initialization to validation**

**Commands:**
- `/build` - Execute implementation based on the established plan, with built-in code review loop
- `/build-from-requirements` - Complete requirements-driven development workflow - analyze, scout-plan-build, validate
- `/evaluate-against-requirements` - Evaluate completed work against original requirements to verify acceptance criteria

**Agents:**
- `work-initializer` - Development environment setup specialist (git branches, dependencies, project structure)
- `work-completion-evaluator` - Implementation evaluation specialist for pass/fail validation
- `context-discovery` - Project context discovery specialist that populates workflow cache

**Use Cases:**
- Setting up development environment before starting work
- Building features with automated code review
- Validating completed work against original requirements
- Requirements-driven development with full traceability

---

### ✨ code-quality-toolkit
**Comprehensive code review, security analysis, and performance optimization**

**Commands:**
- `/review-code` - Review code quality using the code-reviewer agent with specified scope

**Agents:**
- `code-reviewer` - Expert code review specialist for quality, performance, and maintainability
- `security-reviewer` - Security-focused code review for vulnerabilities and defensive patterns
- `code-optimizer` - Senior performance engineering consultant for bottleneck identification

**Use Cases:**
- Pre-commit code quality checks
- Security vulnerability scanning
- Performance optimization analysis
- Code maintainability reviews

---

### 📊 project-management-tools
**Project activity reporting and intelligent ticket consultation**

**Agents:**
- `activity-reporter` - Management reporting specialist for git and ticket history analysis
- `ticket-consultant` - Senior project analyst for intelligent ticket recommendations

**Use Cases:**
- Sprint review reports and development activity summaries
- Deciding what to work on next
- Prioritizing backlog items
- Historical work analysis

---

### 🛠️ cc-utilities
**Claude Code meta-utilities for self-improvement and customization**

**Commands:**
- `/learn-from-conversation` - Analyze current conversation to extract user expectations and update CLAUDE.md

**Agents:**
- `subagent-designer` - PROACTIVELY create or update custom subagents for specialized task automation

**Use Cases:**
- Teaching Claude Code your preferences and patterns
- Creating custom agents for repetitive workflows
- Updating and improving existing agents
- Personalizing Claude Code behavior

---

## Installation

### Prerequisites
- Claude Code installed and configured
- Git access to this repository

### Add Marketplace

```bash
/plugins add git https://github.com/kmerrell42/cc-plugins.git
```

### Install Individual Plugins

```bash
# Install all plugins
/plugins install requirements-toolkit@claude-plugins-personal
/plugins install build-execution-suite@claude-plugins-personal
/plugins install code-quality-toolkit@claude-plugins-personal
/plugins install project-management-tools@claude-plugins-personal
/plugins install cc-utilities@claude-plugins-personal

# Or install selectively based on your needs
/plugins install requirements-toolkit@claude-plugins-personal
```

### Update Plugins

```bash
# Update marketplace
/plugins update claude-plugins-personal

# Reinstall individual plugins to get latest version
/plugins uninstall requirements-toolkit@claude-plugins-personal
/plugins install requirements-toolkit@claude-plugins-personal
```

## Usage Examples

### Requirements Analysis Flow
```bash
# Analyze requirements for clarity
/analyze-requirements "Build user authentication system"

# Scout existing codebase patterns
/scout "authentication patterns" 2

# Create implementation plan
/plan "Implement JWT-based authentication"

# Or do it all at once
/scout-plan-build "Add OAuth2 support to auth system" 3
```

### Build & Validation Flow
```bash
# Build from requirements with full validation
/build-from-requirements ./docs/feature-spec.md

# Or build with existing plan
/build "Implement the authentication feature"

# Validate completed work
/evaluate-against-requirements ./docs/feature-spec.md
```

### Code Quality Flow
```bash
# Review recent changes
/review-code

# Use agents directly for specific analysis
# (via Claude Code's agent system)
```

## Plugin Manifest

| Plugin | Commands | Agents | Total Items |
|--------|----------|--------|-------------|
| requirements-toolkit | 4 | 2 | 6 |
| build-execution-suite | 3 | 3 | 6 |
| code-quality-toolkit | 1 | 3 | 4 |
| project-management-tools | 0 | 2 | 2 |
| cc-utilities | 1 | 1 | 2 |
| **TOTAL** | **9** | **11** | **20** |

## Development

### Repository Structure
```
claude-plugins-personal/
├── README.md
├── plugins/
│   ├── requirements-toolkit/
│   │   ├── .claude-plugin/plugin.json
│   │   ├── commands/
│   │   └── agents/
│   ├── build-execution-suite/
│   ├── code-quality-toolkit/
│   ├── project-management-tools/
│   └── cc-utilities/
└── .gitignore
```

### Version Management
- All plugins start at v1.0.0
- Update version in `.claude-plugin/plugin.json` when making changes
- Users must reinstall plugins to get updates (no auto-update yet)

### Contributing
This is a personal plugin repository. Feel free to fork and customize for your own use.

## License

MIT License - Use freely across your machines and projects.

## Author

Kelly Merrell (keldog@gmail.com)

---

**Built with Claude Code** | [Documentation](https://docs.claude.com/en/docs/claude-code/)
