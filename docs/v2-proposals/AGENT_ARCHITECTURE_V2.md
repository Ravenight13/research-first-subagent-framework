# Agent Architecture v2.0: Researchers vs Do-ers

**Status**: Proposal
**Version**: 2.0.0
**Date**: 2025-10-01

---

## 🎯 Core Architectural Principle

The Research-First Framework defines **two distinct agent types** based on execution context and implementation authority:

### **1. Project-Specific Researchers** (`.claude/agents/`)

**Location**: Project root `.claude/agents/` directory

**Purpose**: Context-optimized analysis agents that maintain the research-first pattern

**Characteristics**:
- ✅ **Research-only**: NEVER implement code directly
- ✅ **Heavy reading in subagent context**: Load large files, analyze codebases, research patterns
- ✅ **Lightweight output**: Return focused summaries, implementation plans, specifications
- ✅ **Context-aware**: Read project state files (`.project_status.md`, `docs/context-sessions/`)
- ✅ **Token-efficient**: Keep heavy analysis isolated, parent maintains implementation context
- ✅ **Project-specific**: Tailored to project domain and constitutional requirements

**Execution Pattern**:
```
Parent invokes researcher → Researcher reads heavy context →
Researcher analyzes/documents → Returns lightweight summary →
Parent implements based on research
```

**Examples**:
- `vendor-format-research` - Analyzes vendor file formats, returns parsing strategy
- `api-design-research` - Studies API endpoints, returns design specifications
- `testing-strategy-research` - Evaluates test coverage, returns comprehensive test plan
- `performance-research` - Analyzes bottlenecks, returns optimization recommendations

**Token Savings**: 70% improvement by isolating heavy reading in subagent context

---

### **2. User-Level Do-ers** (`~/.claude/agents/`)

**Location**: User home directory `~/.claude/agents/`

**Purpose**: General-purpose execution agents that can both analyze and implement

**Characteristics**:
- ✅ **Full implementation authority**: Can research AND write code
- ✅ **Full context capability**: Can read entire codebases if needed
- ✅ **Orchestration assistance**: Work directly with parent agent on implementation
- ✅ **Flexibility**: Can research when needed, but primarily execute
- ✅ **General-purpose**: Not tied to specific project domain

**Execution Pattern**:
```
Parent invokes do-er → Do-er analyzes context →
Do-er implements solution → Returns with code changes →
Parent integrates and validates
```

**Examples**:
- `python-wizard` - Can research Python patterns AND implement the code
- `backend-architect` - Can analyze architecture AND refactor systems
- `sql-pro` - Can study database schema AND write optimized queries
- `fastapi-pro` - Can research FastAPI patterns AND build endpoints

**Use Case**: When parent needs delegation of both analysis and implementation

---

## 🔄 Pattern Comparison

| Aspect | Researchers (Project) | Do-ers (User) |
|--------|----------------------|---------------|
| **Location** | `.claude/agents/` | `~/.claude/agents/` |
| **Scope** | Project-specific | General-purpose |
| **Implementation** | ❌ Research only | ✅ Research + Implement |
| **Context Awareness** | ✅ Required | ⚠️ Optional |
| **Token Optimization** | ✅ Primary goal | ⚠️ Secondary |
| **Output** | Lightweight summary | Code + Documentation |
| **Best For** | Heavy analysis | Delegated implementation |

---

## 🏗️ When to Use Each Type

### **Use Researchers When**:
1. Task requires heavy file reading (large codebases, vendor files, APIs)
2. Analysis needed before implementation (research-first pattern)
3. Token optimization is critical (keep heavy context isolated)
4. Project-specific domain knowledge required
5. Constitutional compliance validation needed

### **Use Do-ers When**:
1. Parent needs delegation of complete feature implementation
2. Task requires both analysis and code changes
3. General-purpose capability needed across projects
4. Parent wants to maintain high-level orchestration only
5. Task is self-contained and doesn't require heavy context loading

---

## 📊 Token Efficiency Analysis

### **Researcher Pattern** (Research-First):
```
Parent context: 5,000 tokens (implementation focus)
Researcher context: 50,000 tokens (analysis focus)
Researcher output: 2,000 tokens (summary only)
Parent receives: 2,000 tokens to implement

Total parent tokens: 7,000 tokens
Token efficiency: 70% savings vs parent loading everything
```

### **Do-er Pattern** (Delegation):
```
Parent context: 5,000 tokens (orchestration focus)
Do-er context: 30,000 tokens (analysis + implementation)
Do-er output: 8,000 tokens (code + documentation)
Parent receives: 8,000 tokens to integrate

Total parent tokens: 13,000 tokens
Token efficiency: Optimal for delegated implementation
```

---

## 🎯 Implementation Strategy

### **For Project-Specific Researchers**:

1. **Installation**: Copy from framework `agents/domain-specific/` to project `.claude/agents/`
2. **Customization**: Update with project-specific context file paths
3. **Context Files**: Ensure project has required context files:
   - `.project_status.md`
   - `docs/context-sessions/`
   - `.specify/memory/constitution.md`
4. **Standards References**: Link to project standards in `docs/standards/`

**Example researcher agent header**:
```yaml
---
name: vendor-format-research
description: Research vendor file formats, create implementation plans. NEVER implement code directly.
tools: Write, Read, List, web_search, Context7:resolve-library-id, Context7:get-library-docs
model: inherit
---

# Vendor Format Research Agent

## Core Mission
Research-only vendor file analysis following constitutional principles.
Create implementation plans using online research capabilities.
**NEVER implement code directly.**

## Required Context Loading
Before starting research, ALWAYS read:
- `.project_status.md` - Current project state
- `docs/context-sessions/vendor-research.md` - Active vendor context
- `.specify/memory/constitution.md` - Constitutional compliance
```

### **For User-Level Do-ers**:

1. **Installation**: Copy from framework `agents/core/` or `agents/backend/` to `~/.claude/agents/`
2. **Standards Integration**: Reference framework standards documents
3. **No Project Coupling**: Keep general-purpose, no hardcoded project paths
4. **Flexibility**: Can research OR implement based on parent's request

**Example do-er agent header**:
```yaml
---
name: python-wizard
description: Master Python 3.12+ development with TDD methodology. Can research patterns AND implement code.
tools: Read, Write, Edit, Bash, Glob, Grep
model: inherit
---

# Python Wizard Agent

## Core Mission
Expert Python development with modern patterns and TDD methodology.
Can research Python patterns when needed, but primarily implements code.

## Capabilities
- ✅ Research Python best practices (when requested)
- ✅ Implement Python modules following TDD
- ✅ Refactor existing code with modern patterns
- ✅ Write comprehensive test suites
```

---

## 🔧 Migration Path for Existing Agents

### **Step 1: Identify Agent Type**

For each agent in `~/.claude/agents/`, determine:

**Is this agent currently**:
- [ ] Project-specific (references project paths, domain knowledge)?
- [ ] General-purpose (works across any Python/FastAPI project)?

**Does this agent**:
- [ ] Only research and return summaries?
- [ ] Implement code based on specifications?
- [ ] Both research AND implement?

### **Step 2: Choose Target Type**

**Researcher Criteria** (move to `.claude/agents/`):
- Project-specific domain knowledge
- Research-first pattern enforcement
- Constitutional compliance validation
- Heavy context loading requirements

**Do-er Criteria** (keep in `~/.claude/agents/`):
- General-purpose capabilities
- Implementation authority needed
- Works across multiple projects
- Self-contained execution

### **Step 3: Upgrade Agent Pattern**

**For Researchers**:
1. Add context-awareness protocol
2. Remove implementation tools (keep Read, Write for reports only)
3. Add research output format specifications
4. Reference project standards documents
5. Add constitutional compliance validation

**For Do-ers**:
1. Keep full tool access (Read, Write, Edit, Bash)
2. Add standards document references (not inline repetition)
3. Make project-agnostic (no hardcoded paths)
4. Add research capability (optional, based on parent request)

---

## 📚 Standards Document Integration

**Problem**: 200-300 token instruction blocks repeated across agents

**Solution**: Reference centralized standards documents

### **Before** (Token-Heavy):
```markdown
## Type-Safety Requirements (300 tokens)

**Decimal Usage:**
- All monetary values use Decimal
- All quantity values use int
- Never use float for financial data
- Always preserve precision
[... 250+ more tokens ...]
```

### **After** (Token-Light):
```markdown
## Type-Safety Requirements

**REFERENCE**: See `/docs/standards/type-safety-requirements.md`

**Critical**: All implementations must follow type-safety constitutional requirements.
```

**Token Savings**: 67% reduction (300 tokens → 100 tokens per agent)

---

## 🎯 Success Criteria

### **Researchers** ✅:
- [ ] Located in project `.claude/agents/` directory
- [ ] NEVER implement code (research-only mandate)
- [ ] Read project context files before executing
- [ ] Return lightweight summaries (<2,000 tokens)
- [ ] Reference project standards documents
- [ ] Validate constitutional compliance

### **Do-ers** ✅:
- [ ] Located in `~/.claude/agents/` directory
- [ ] Can research AND implement
- [ ] General-purpose across projects
- [ ] Reference framework standards documents
- [ ] No hardcoded project paths
- [ ] Maintain full tool access

---

## 🚀 Next Steps

1. **Phase 1**: Create standards documents to eliminate token redundancy
2. **Phase 2**: Upgrade 23 existing agents with research-first patterns
3. **Phase 3**: Add context-awareness to project-specific researchers
4. **Phase 4**: Create installation tooling (install-agents.sh, update-agents.sh)
5. **Phase 5**: Test token efficiency improvements (target: 67% savings)

---

**Framework**: Research-First Subagent Framework v2.0
**Repository**: https://github.com/Ravenight13/research-first-subagent-framework
**License**: MIT
