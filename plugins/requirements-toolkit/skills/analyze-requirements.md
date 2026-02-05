---
name: analyze-requirements
description: Systematically analyze task requirements to identify ambiguities, gaps, and unclear acceptance criteria. Engage user interactively to resolve issues, then offer to update the source document.
argument-hint: '"task description"'
---

**Arguments:**
- TASK_DESCRIPTION = $1

**Input formats:**
- Inline text: `/analyze-requirements "Build a user authentication system"`
- File path: `/analyze-requirements ./docs/requirements.md`
- Ticket reference: `/analyze-requirements LINEAR-123`

## Analysis Process

### Step 1: Input Acquisition

**For inline text:** Use as-is for analysis

**For file paths:**
- Read the file using the Read tool
- Analyze full content

**For ticket references:**
- If Linear ticket (format: PROJECT-123), use Bash tool to fetch via Linear API
- Extract title, description, and acceptance criteria
- Note: User must have LINEAR_API_KEY configured

### Step 2: Systematic Analysis

Apply these evaluation criteria using chain-of-thought reasoning:

#### Critical Gaps (Must resolve before implementation)
- **Missing core functionality**: What does the system/feature actually DO?
- **Undefined scope boundaries**: What's explicitly OUT of scope?
- **Missing constraints**: Performance, security, compliance, scalability requirements?
- **Undefined terms**: Are there domain-specific terms without definitions?
- **Missing dependencies**: Required systems, services, or data?
- **Environment specifications**: Where does this run? What platforms?

#### Ambiguities (Multiple valid interpretations)
- **Vague quantifiers**: "fast", "many", "some", "improved" without metrics
- **Unclear actors**: WHO performs each action?
- **Conditional logic**: WHEN/IF statements without clear conditions
- **Implied behavior**: Assumptions about "obvious" functionality
- **Contradictions**: Conflicting statements within requirements
- **Generic verbs**: "handle", "manage", "process" without specifics

#### Missing Acceptance Criteria
- **No success metrics**: How do we know it works?
- **Untestable statements**: Can this be objectively verified?
- **Missing edge cases**: What happens when things go wrong?
- **No input/output examples**: Concrete examples of expected behavior?
- **Undefined quality standards**: Performance thresholds, error rates, UX standards?

#### Technical Feasibility Concerns
- **Technology mismatches**: Requirements that conflict with stated tech stack?
- **Unrealistic constraints**: "Real-time", "100% uptime", "instant" without context?
- **Integration gaps**: How does this connect to existing systems?
- **Data flow unclear**: Where does data come from and go to?

### Step 3: Present Findings

Structure your analysis clearly:

```markdown
## Requirements Analysis: [Title/Reference]

### ✅ Clear Aspects
[List what IS well-defined - build confidence]

---

### 🚨 Critical Gaps (MUST RESOLVE)
[Number each item for reference]

**CG-1: [Gap title]**
- **Issue**: [What's missing and why it's critical]
- **Impact**: [What can't be implemented without this]
- **Question**: [Specific question to resolve]

[Repeat for each critical gap]

---

### ⚠️ Ambiguities (Clarification Needed)

**AM-1: [Ambiguity title]**
- **Statement**: "[Quote the ambiguous text]"
- **Issue**: [What's unclear or has multiple interpretations]
- **Interpretations**:
  - Option A: [First interpretation]
  - Option B: [Second interpretation]
- **Question**: [Targeted clarification question]

[Repeat for each ambiguity]

---

### 📋 Missing Acceptance Criteria

**AC-1: [Area needing criteria]**
- **Functionality**: [What behavior needs acceptance criteria]
- **Current state**: [What's specified now, if anything]
- **Needed**: [Specific testable criteria required]
- **Question**: [How to verify success]

[Repeat for each missing criteria]

---

### 🔧 Technical Feasibility Notes

[Any concerns about technical implementation]
[Questions about architecture, performance, or integration]

---

## Summary

- **Critical Gaps**: [X items] - BLOCKING
- **Ambiguities**: [Y items] - HIGH PRIORITY
- **Missing Criteria**: [Z items] - MEDIUM PRIORITY
- **Feasibility Concerns**: [N items] - REVIEW NEEDED

**Confidence Level**: [Low/Medium/High] - [Explanation]

**Recommendation**: [Proceed/Block/Needs Major Revision]
```

### Step 4: Interactive Resolution

**DO NOT** proceed until user responds to your analysis.

For each item the user clarifies:
- ✅ Mark as RESOLVED
- Document the resolution
- Update your understanding

**Track resolution status:**
```markdown
### Resolution Tracking

**Resolved:**
- CG-1: [Brief resolution]
- AM-3: [Brief resolution]

**Still Pending:**
- CG-2: [Still needs answer]
- AM-1: [Still needs answer]
- AC-4: [Still needs answer]
```

**If user provides partial answers:**
- Acknowledge what was resolved
- Re-ask remaining questions more specifically
- Provide examples to help user understand what you need

**If user is uncertain:**
- Offer reasonable defaults based on common patterns
- Present trade-offs between options
- Suggest starting simple and iterating

### Step 5: Offer to Update Source

**ONLY after all critical gaps and ambiguities are resolved:**

```markdown
---

## ✅ Requirements Now Clear

All critical items resolved. Updated understanding:

[Provide concise, unambiguous restatement of requirements including:]
- Clear scope (what's in and out)
- Specific functionality with examples
- Measurable acceptance criteria
- Resolved ambiguities with chosen interpretation
- Technical constraints and dependencies

---

**Would you like me to update the source?**

[If file-based]: I can update `[filename]` with the clarified requirements.

[If ticket-based]: I can update [TICKET-ID] with the clarified details. I'll:
- Update description with resolved ambiguities
- Add/refine acceptance criteria
- Add technical notes section if needed

[If inline]: I can create a new requirements document with the full clarified specification.

Respond with:
- "update" to proceed with update
- "show me first" to see the proposed changes
- "no thanks" to skip
```

### Step 6: Execute Update (If Approved)

**For files:**
- Use Edit tool to update the file
- Add "Last Analyzed: [date]" timestamp
- Preserve original structure while clarifying content
- Show summary of changes made

**For tickets:**
- Use Bash tool with Linear API to update
- Show API response confirming update
- Provide link to updated ticket

**For new documents:**
- Create structured requirements document
- Use template with clear sections
- Save to user-specified location

## Constraints

- **NEVER assume or fill in gaps yourself** - always ask the user
- **NEVER proceed with "I'll assume X"** - get explicit confirmation
- **ALWAYS distinguish** between must-have vs nice-to-have clarifications
- **ALWAYS provide examples** when asking for clarification (helps user understand what you need)
- **NEVER mark requirements as "clear" if acceptance criteria can't be objectively tested**
- **IMPORTANT**: Focus on PREVENTING implementation confusion, not creating busywork

## Edge Cases

**User says "just use your best judgment":**
- Push back gently: "I want to avoid building the wrong thing. For [specific item], would you prefer A or B?"
- Only accept "best judgment" for truly minor details
- Document what you assumed in final summary

**Requirements are actually clear:**
- Say so! Don't invent problems.
- Provide brief validation: "These requirements are clear and testable. Here's my understanding: [summary]"
- Still offer to create formal spec if none exists

**Very large/complex requirements:**
- Break analysis into logical sections
- Tackle highest-priority sections first
- Use "CG-1, CG-2" style numbering for easy reference

**Technical jargon heavy:**
- Ask for clarification of domain terms
- Build glossary section in output
- Verify your understanding: "By X, do you mean Y?"

## Response Tone

- **Direct and structured** - use the template format
- **Specific questions** - not "can you clarify this section?" but "what happens when user has no email?"
- **Collaborative** - "let's clarify" not "you didn't specify"
- **Pragmatic** - distinguish blocking issues from nice-to-haves
- **Example-driven** - "For example, if X happens, should the system Y or Z?"
- **User-experience** - Giving multiple choice options is preferred over asking open ended questions

## Success Indicators

You've succeeded when:
1. Every critical gap has a specific answer
2. Ambiguous terms have single, clear interpretations
3. Acceptance criteria can be turned into actual tests
4. Implementation team could build from the clarified requirements
5. User feels confident the right thing will be built

You've failed if:
- Requirements still have "TBD" or "to be discussed"
- Acceptance criteria use vague terms like "good", "fast", "user-friendly"
- Multiple interpretations still possible
- You made assumptions instead of asking
