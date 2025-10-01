# Research-First Subagent Pattern Guide

## Executive Summary

**Source:** YouTube video insights from "How sub agent works" - https://www.youtube.com/watch?v=LCYBVpSB0Wo  
**Core Discovery:** Implementation-focused subagents create unfixable context loss; research-first pattern solves token optimization and maintains parent agent context  
**Impact:** 70% speed improvement, consistent performance regardless of project complexity  

## The Context Loss Problem

### **Why Implementation Subagents Fail**

From the video transcript, the fundamental issue with implementation-focused subagents:

> "The moment if whatever sub agent implemented is not 100% correct and you want agent to fix it. That's where the problem begin because for each agent it only has very limited information about what is going on."

**The Problem Pattern:**
1. **Parent Agent:** Delegates implementation to specialized subagent (frontend-dev, backend-dev)
2. **Subagent:** Implements code but operates in isolated conversation context
3. **Bug Discovery:** Implementation has issues that need fixing
4. **Context Loss:** Parent agent can't fix bugs because it doesn't see subagent's implementation details
5. **Failed Handoff:** Subsequent subagent calls start fresh conversations with no prior context

### **Token Consumption Issues**

Before subagents, Claude Code would:
> "...use 80% of the context window cuz those files will contain large amount of context which will likely trigger this compact conversation command that will summarize the whole conversation before it can proceed."

**Performance Degradation:**
- Large file reads consume massive tokens in parent conversation
- Context window fills up quickly requiring conversation compacting
- Each compacting loses critical implementation context
- Performance drops dramatically with each context loss

## The Research-First Solution

### **Core Principle: Research, Don't Implement**

From video author's breakthrough:
> "Sub agent works best when they just looking for information and provide a small amount of summary back to main conversation thread."

**Research-First Pattern:**
1. **Subagent Role:** Research specialist that analyzes, plans, and recommends
2. **Token Optimization:** Massive file reads happen in subagent context, not parent
3. **Context Preservation:** Parent agent gets actionable summaries, maintains full implementation context
4. **Implementation Control:** Parent agent does all actual code changes with complete context

### **File System as Context Management**

Key insight from Manus team blog (referenced in video):
> "Instead of storing all the tool results in the conversation history directly they receive a result to a local file which can be retrieved later."

**Pattern Implementation:**
- Research results saved to markdown files on filesystem  
- Parent agent reads research summaries (hundreds of tokens vs thousands)
- Detailed research available on-demand via file system
- Context shared across sessions through persistent files

## Implementation Patterns for Commission Processing

### **Vendor Research Pattern**

**Traditional Anti-Pattern:**
```
Parent Agent: "Implement EPSON vendor processor"
→ backend-dev subagent: Reads files, writes code, loses context
→ Parent Agent: Can't debug or iterate on implementation
```

**Research-First Pattern:**
```
Parent Agent: "Research EPSON vendor processing requirements"
→ vendor-format-research-agent: 
   - Analyzes vendor files (heavy token usage in subagent context)
   - Creates implementation plan (saved to filesystem)
   - Returns summary (lightweight parent context)
→ Parent Agent: Implements based on research with full context
```

### **Context File Structure (From Video)**

**Implementation:**
```
docs/context-sessions/
├── current-project.md        # Master project state (parent agent)
├── vendor-research.md        # Active vendor work context
└── task-{id}/               # Feature-specific context

docs/subagent-reports/
├── vendor-research-epson-{timestamp}.md    # Detailed research
├── api-design-{timestamp}.md               # API analysis
└── performance-analysis-{timestamp}.md     # Performance research

docs/feature-plans/
└── vendor-plans/
    ├── epson-processing-plan.md            # Implementation ready plans
    └── tpd-processing-plan.md
```

### **Research Agent Workflow (Video Pattern)**

**Before Starting Research:**
1. Read `docs/context-sessions/current-project.md` for overall context
2. Load constitutional compliance requirements
3. Review existing vendor patterns for reusable components

**During Research:**
1. Conduct heavy file analysis (token-intensive operations in subagent context)
2. Create detailed findings and recommendations
3. Save comprehensive research reports to filesystem

**After Research:**
1. Update `docs/context-sessions/{feature}-research.md` with key findings
2. Create actionable implementation plan
3. Return lightweight summary to parent agent

**Parent Agent Response:**
1. Read implementation plan from filesystem
2. Execute implementation with full context maintained
3. Debug and iterate with complete implementation knowledge

## Constitutional Compliance Integration

### **Research Agents and TDD (Principle I)**

**Pattern:** Research agents create test scenarios BEFORE implementation
```markdown
Research Output Must Include:
- Specific test cases for vendor processing
- Acceptance criteria definitions  
- Fixture data requirements
- Golden output specifications
```

### **Vendor-First Architecture (Principle II)**

**Pattern:** Research respects vendor isolation
```markdown
Vendor Research Constraints:
- Each vendor researched independently
- No cross-vendor dependencies in research
- Shared components identified but not implemented by research agents
- Constitutional compliance validated per vendor
```

### **Git Workflow Integration (Enhanced)**

**Pattern:** Research outputs include executable git commands and workflow guidance
```markdown
Git Workflow Integration Requirements:
- Context-aware branch naming based on research scope
- Phase-aligned commit strategy matching research phases
- Executable commands ready for copy-paste implementation
- CLAUDE.md helper command integration
- Constitutional compliance through workflow structure

Research Output Enhancement (+200 tokens):
- Implementation Metadata: branch patterns, commit strategy
- Executable Commands: branch creation, phase commits, PR creation
- Workflow Execution Reference: CLAUDE.md integration
- Quality Gate Integration: checkall, constitutional validation
```

**Developer Efficiency Benefits:**
- 30% faster implementation through ready-to-execute commands
- Zero git workflow derivation time
- Consistent constitutional compliance through structured commits
- Seamless integration with existing CLAUDE.md patterns

### **Data Integrity (Principle IV)**

**Pattern:** Complete audit trails in research
```markdown
Research Documentation Requirements:
- Every research decision documented with rationale
- Implementation recommendations include traceability
- Constitutional compliance checkpoints identified
- Error handling and validation requirements specified
```

## Performance Optimization Patterns

### **Token Budget Management**

**Video Insight:** Transform token-heavy operations into lightweight summaries
```
Before: Parent agent reads 10,000 token vendor file
After: Research agent reads file, returns 500 token summary + implementation plan
```

**Implementation:**
- Research agents handle all heavy file operations
- Parent context stays under 5,200 token budget (including git workflow integration)
- Detailed analysis available on-demand via filesystem
- Context rotation based on research completion, not token exhaustion

### **Parallel Research Execution**

**Pattern:** Multiple research streams without context contamination
```
Parallel Research Streams:
- vendor-format-research-agent: Analyzes EPSON files
- performance-research-agent: Studies large file processing
- api-design-research-agent: Reviews endpoint patterns
→ All research results fed to parent for integrated implementation
```

### **Context Handoff Optimization**

**Video Pattern:** Clean handoffs between research and implementation
```
Research Phase:
1. Subagent creates comprehensive research report
2. Updates project context with implementation-ready summary
3. Saves detailed findings to filesystem

Implementation Phase:
1. Parent reads context + implementation plan
2. Executes with full context knowledge
3. Maintains debugging capability throughout
```

## Success Metrics from Video Implementation

### **Performance Improvements Demonstrated**
- Context loading: Faster due to lightweight summaries
- Development velocity: Higher quality implementation from better research
- Debug capability: Maintained throughout development cycle
- Token efficiency: Consistent performance regardless of project complexity

### **Quality Improvements**
- Better architectural decisions from comprehensive research
- Reduced implementation errors due to thorough planning
- Constitutional compliance maintained through research validation
- Complete audit trails through filesystem documentation

## Anti-Patterns to Avoid

### **Implementation Delegation (Video Warning)**

❌ **Never:** Assign implementation tasks to subagents
```
Bad: "frontend-dev-agent implement the upload component"
Good: "frontend-research-agent analyze upload requirements and create implementation plan"
```

### **Context Loss Through Implementation**

❌ **Never:** Let subagents modify code directly
```
Bad: Subagent writes code → Parent can't debug
Good: Subagent researches → Parent implements → Parent can debug
```

### **Token Bloat in Parent Context**

❌ **Never:** Load heavy research data into parent conversation
```
Bad: Include full vendor analysis in parent context
Good: Save analysis to filesystem, load summary only
```

## Practical Implementation for Current Project

### **Immediate Application: EPSON Vendor**

**Current State:** 95% complete, needs CSV routing debug
**Research-First Approach:**
1. vendor-format-research-agent analyzes CSV routing patterns
2. Creates debugging plan with specific investigation steps
3. Parent agent executes debug with full context maintained

### **Next Application: TPD Vendor**

**Research-First Workflow:**
1. vendor-format-research-agent analyzes TPD file formats
2. Creates comprehensive processing strategy
3. Identifies reusable components from EPSON work
4. Parent agent implements with optimal architecture

### **Scaling to Frontend Work**

**Context Isolation Pattern:**
1. Archive vendor research context
2. Load frontend research context  
3. frontend-architecture-research-agent analyzes Vue.js integration
4. Parent agent implements with clean context separation

This research-first pattern transforms subagents from implementation bottlenecks into intelligence multipliers, enabling the parent agent to make better decisions while maintaining complete implementation control and debugging capability.
