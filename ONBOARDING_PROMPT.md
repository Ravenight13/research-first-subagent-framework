# Research-First Subagent System - New Project Onboarding

**⭐ Copy-paste this entire prompt into your Claude Code session to enable research-first subagent development in your project.**

---

## 🎯 What You're Learning

You're learning the **Research-First Subagent Pattern** - a proven approach for using Claude Code subagents that:
- **Improves development speed by 70%** (proven in production)
- **Optimizes token usage** (prevents context window exhaustion)
- **Preserves implementation context** (no context loss problems)
- **Enables iterative debugging** (parent agent maintains full context)

**Pattern Source:** [YouTube: How sub agent works](https://www.youtube.com/watch?v=LCYBVpSB0Wo)

**Production Validation:** Commission Processing system (7,415 lines, 6 agents, 100% success rate)

---

## 📚 Core Pattern: Why Traditional Subagents Fail

### The Context Loss Problem

Traditional implementation-focused subagents create unfixable context loss. From the YouTube video:

> "The moment if whatever sub agent implemented is not 100% correct and you want agent to fix it. That's where the problem begin because for each agent it only has very limited information about what is going on."

**What Goes Wrong:**
1. Parent agent: "Implement feature X" → delegates to subagent
2. Subagent: Implements code in isolated conversation context
3. Bug Discovery: Implementation has issues
4. **Parent Can't Fix**: Parent lacks implementation details (happened in subagent context)
5. **Failed Handoff**: Subsequent subagent calls start fresh conversations (no prior context)

**Result:** Unfixable context loss, degraded performance, increased token usage

### Token Consumption Problem

Before subagents, massive file reads would:
> "...use 80% of the context window cuz those files will contain large amount of context which will likely trigger this compact conversation command that will summarize the whole conversation before it can proceed."

**Performance Degradation:**
- Large file reads consume massive tokens in parent context
- Context window fills → conversation compacting
- Each compacting loses critical implementation details
- Performance drops dramatically with each context loss

---

## ✅ The Research-First Solution

### Core Principle

From the video author's breakthrough:
> "Sub agent works best when they just looking for information and provide a small amount of summary back to main conversation thread."

**Research-First Pattern:**
1. **Subagent Researches** (Read-Heavy)
   - Massive file reading in subagent context (thousands of tokens)
   - Analysis, planning, and recommendation
   - Output: Implementation-ready specification

2. **Parent Implements** (Write-Heavy)
   - Follow research specifications
   - Maintain full implementation context
   - Can iterate, debug, and fix effectively

3. **Validate** (Automated + Optional Subagents)
   - Quality assurance checks
   - Project principles compliance
   - Integration verification

### File System as Context Management

Key insight from Manus team blog (referenced in video):
> "Instead of storing all the tool results in the conversation history directly they receive a result to a local file which can be retrieved later."

**How It Works:**
- Research results saved to markdown files on filesystem
- Parent agent reads research summaries (hundreds of tokens vs thousands)
- Detailed research available on-demand via file system
- Context shared across sessions through persistent files

---

## 🛠️ Setting Up Research-First in Your Project

### Step 1: Create Context Management Structure

Create these directories and files in your project:

```bash
mkdir -p docs/context-sessions
mkdir -p docs/subagent-reports
mkdir -p .claude/agents
```

**Context Files:**
- `docs/context-sessions/current-project.md` - Master project state
- `docs/context-sessions/research-context.md` - Active research focus
- `docs/subagent-reports/` - Research agent outputs

### Step 2: Define Your First Research Agent

Create `.claude/agents/your-research-agent.md`:

```markdown
---
name: your-research-agent
description: Research [DOMAIN] and create implementation plans without implementing code directly.
tools: Write, Read, List
model: inherit
---

# Your Research Agent

## Core Mission
Research-only analysis for [DOMAIN]. Create implementation plans, test scenarios, and technical specifications. **NEVER implement code directly.**

## Research Capabilities
- [Capability 1]: [Description]
- [Capability 2]: [Description]
- [Capability 3]: [Description]

## Research Output Protocol
1. Read relevant context files
2. Analyze [DOMAIN] requirements
3. Create implementation plan
4. Save to docs/subagent-reports/research-[topic]-[date].md
5. Return summary to parent agent

## Anti-Patterns (NEVER)
- ❌ NEVER write implementation code
- ❌ NEVER modify source files directly
- ❌ NEVER create pull requests or commits
- ❌ NEVER bypass research → implementation flow

## Success Criteria
- Clear, actionable implementation plan
- All edge cases documented
- Test scenarios specified
- Parent agent can implement without questions
```

### Step 3: Use the Research-First Workflow

**For New Features:**
```
1. You (Parent Agent): Invoke research agent
   "Research [FEATURE] requirements and create implementation plan"

2. Research Agent:
   - Reads relevant files (thousands of tokens in subagent context)
   - Analyzes requirements
   - Creates detailed plan
   - Saves to docs/subagent-reports/research-[feature]-2025-10-01.md
   - Returns summary (hundreds of tokens to parent)

3. You (Parent Agent):
   - Read research summary
   - Implement following specifications
   - Maintain full implementation context
   - Can iterate and debug effectively
```

**Example Session:**
```
Parent: @your-research-agent Research user authentication requirements

[Research agent analyzes, creates plan, saves to filesystem]

Parent: [Reads summary] Implement authentication following research plan
[Parent implements with full context, can iterate/debug]
```

---

## 📋 Research Agent Templates

### Template 1: Data Format Research Agent

**Use For:** Analyzing vendor files, API responses, database schemas

```markdown
---
name: data-format-research
description: Research data formats and create parsing implementation plans
tools: Write, Read, List
model: inherit
---

# Data Format Research Agent

## Core Mission
Analyze data file formats, identify parsing patterns, document data relationships, and create implementation plans. **NEVER implement parsers directly.**

## Research Capabilities
- File format analysis (Excel, CSV, JSON, XML, PDF)
- Data structure identification
- Business rules extraction
- Edge case documentation
- Parsing strategy recommendations

## Research Output
1. File format specification
2. Sample data analysis
3. Parsing strategy
4. Edge cases and handling
5. Test scenarios
6. Implementation roadmap

## Output Location
`docs/subagent-reports/data-format-[name]-[date].md`
```

### Template 2: API Design Research Agent

**Use For:** Designing REST APIs, microservices, endpoints

```markdown
---
name: api-design-research
description: Research API requirements and create design specifications
tools: Write, Read, List
model: inherit
---

# API Design Research Agent

## Core Mission
Analyze API requirements, review existing patterns, design scalable architecture. **NEVER implement endpoints directly.**

## Research Capabilities
- Requirements analysis
- Existing endpoint pattern review
- RESTful design principles
- Security considerations
- Performance optimization
- Documentation standards

## Research Output
1. API specification (OpenAPI/Swagger)
2. Endpoint definitions
3. Request/response schemas
4. Authentication/authorization strategy
5. Error handling approach
6. Implementation roadmap

## Output Location
`docs/subagent-reports/api-design-[feature]-[date].md`
```

### Template 3: Testing Strategy Research Agent

**Use For:** Comprehensive testing strategy design

```markdown
---
name: testing-strategy-research
description: Research testing requirements and create comprehensive test plans
tools: Write, Read, List
model: inherit
---

# Testing Strategy Research Agent

## Core Mission
Analyze testing needs, identify test scenarios, design comprehensive testing strategy. **NEVER write test code directly.**

## Research Capabilities
- Requirements-based test scenario identification
- Edge case analysis
- Test data requirements
- Testing framework recommendations
- Coverage strategy
- Integration test design

## Research Output
1. Test scenario matrix
2. Test data specifications
3. Coverage requirements
4. Testing framework recommendations
5. Mock/fixture requirements
6. Implementation roadmap

## Output Location
`docs/subagent-reports/testing-strategy-[feature]-[date].md`
```

---

## 🎓 Best Practices

### 1. Context File Management

**Keep Context Files Small (<5,000 tokens each):**
```markdown
# docs/context-sessions/current-project.md

## Project State
[Current phase, recent changes, next steps]

## Active Work
[What you're working on right now]

## Research Results
[Links to recent research reports, not full content]
```

**Separate by Domain:**
- `research-context.md` - Data format research
- `api-context.md` - API development
- `testing-context.md` - Testing work

### 2. Research Agent Invocation

**Always Specify Output Location:**
```
@data-format-research Research vendor file format and save to
docs/subagent-reports/vendor-acme-format-2025-10-01.md
```

**Provide Context:**
```
@api-design-research Research user API requirements.
Context: docs/context-sessions/api-context.md
Related: docs/subagent-reports/user-requirements-2025-09-15.md
```

### 3. Implementation Following Research

**Read Research First:**
```
1. Read docs/subagent-reports/research-[feature]-[date].md
2. Understand implementation plan
3. Implement following specifications
4. Maintain full context throughout
```

**Iterate Effectively:**
- Parent maintains implementation context
- Can debug and fix without re-researching
- Can modify approach while following research plan

### 4. When to Re-Research

**Re-research if:**
- Requirements changed significantly
- Edge cases discovered not covered in research
- Implementation reveals research gaps

**Don't re-research if:**
- Minor implementation bugs
- Code style issues
- Documentation updates

---

## 🔧 Common Patterns

### Pattern 1: New Feature Implementation

```
Phase 1: Research (Subagent)
- @feature-research Research [FEATURE] requirements
- Output: docs/subagent-reports/feature-[name]-[date].md

Phase 2: Implementation (Parent - You)
- Read research report
- Implement following specifications
- Maintain full context

Phase 3: Validation (Optional Subagent/Automated)
- Verify implementation matches research
- Run tests
- Check compliance
```

### Pattern 2: Data Format Analysis

```
Phase 1: Format Research (Subagent)
- @data-format-research Analyze vendor file format
- Heavy file reading in subagent context
- Output: Parsing strategy + edge cases

Phase 2: Parser Implementation (Parent)
- Read format specification
- Implement parser
- Handle edge cases from research

Phase 3: Validation
- Test with sample data from research
- Verify edge case handling
```

### Pattern 3: API Endpoint Design

```
Phase 1: API Research (Subagent)
- @api-design-research Research endpoint requirements
- Review existing patterns
- Output: API specification + design decisions

Phase 2: Endpoint Implementation (Parent)
- Read API specification
- Implement endpoints
- Follow design decisions

Phase 3: Integration Testing
- Test endpoints
- Verify specification compliance
```

---

## ❓ Troubleshooting

### Problem: Research Output Too Generic

**Solution:**
- Provide more specific context to research agent
- Include relevant files/documentation
- Ask research agent to analyze specific examples

### Problem: Token Usage Still High

**Solution:**
- Check if research agent is implementing (should be research-only)
- Verify research outputs are saved to files (not returned in full to parent)
- Review context file sizes (keep <5,000 tokens each)

### Problem: Context Loss Between Sessions

**Solution:**
- Research results should be saved to filesystem
- Parent should read from filesystem, not rely on conversation memory
- Use persistent files for critical information

### Problem: Implementation Doesn't Match Research

**Solution:**
- Re-read research report carefully
- Parent maintains implementation context - can course-correct
- If research was incomplete, re-research with more specific questions

---

## 📖 Next Steps

### 1. Create Your Project-Specific Research Agents

Based on your project needs, create:
- Data format research agent (if handling external data)
- API design research agent (if building APIs)
- Testing strategy research agent (for comprehensive testing)
- Security research agent (for security-critical features)

### 2. Set Up Context Management

Create context files:
- `docs/context-sessions/current-project.md` - Master project state
- Domain-specific context files as needed
- `docs/subagent-reports/` directory for research outputs

### 3. Integrate with Your Workflow

Add to your development process:
- Research phase before implementation
- Context file updates after major changes
- Research report review before coding

### 4. Optional: Add Validation

Set up automated validation:
- Tests verify implementation matches research
- Linting ensures code quality
- CI/CD runs validation on PRs

---

## 🎉 You're Ready!

You now understand the research-first subagent pattern and can apply it to your project.

**Key Takeaways:**
1. ✅ Subagents research (read-heavy), parent implements (write-heavy)
2. ✅ Research results saved to filesystem (persistent, low-token access)
3. ✅ Parent maintains implementation context (can iterate/debug)
4. ✅ 70% speed improvement, optimal token usage

**Resources:**
- Full framework documentation: [research-first-subagent-framework](https://github.com/YOUR-ORG/research-first-subagent-framework)
- Pattern source: [YouTube video](https://www.youtube.com/watch?v=LCYBVpSB0Wo)
- Case study: Commission Processing (see framework docs/examples/)

**Questions?** Review [docs/implementation-guides/troubleshooting.md](./docs/implementation-guides/troubleshooting.md) or open an issue on GitHub.

---

**Happy coding with research-first subagents!** 🚀
