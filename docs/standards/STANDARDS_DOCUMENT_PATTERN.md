# Standards Document Pattern

**Purpose**: Eliminate 200-300 token instruction redundancy across agents
**Token Efficiency**: 67% reduction (300 tokens → 100 tokens per agent)
**Pattern**: Reference centralized standards instead of repeating instructions

---

## 🎯 Problem Statement

**Current Anti-Pattern** (Token-Heavy):

Every commission processing agent repeats 200-300 tokens of identical instructions:

```markdown
## Type-Safety Requirements (300 tokens in EVERY agent)

**Decimal Usage:**
- All monetary values must use Python's Decimal type
- All quantity values must use int
- Never use float for financial calculations
- Always preserve precision to 2 decimal places
- Use quantize() for rounding operations

**Type Annotations:**
- All functions must have complete type hints
- Use Optional[] for nullable values
- Use Union[] for multiple types
- Pydantic models for all data structures

**Validation:**
- All inputs validated before processing
- All outputs validated before return
- Use Pydantic for automatic validation
- Raise ValueError for invalid data

[... continues for 250+ tokens ...]
```

**Result**:
- 5 commission agents × 300 tokens = 1,500 tokens wasted
- Every subagent invocation loads redundant instructions
- Changes require updating all agents
- Token budget consumed by repetition

---

## ✅ Standards Document Solution

**New Pattern** (Token-Light):

### **1. Create Centralized Standards Document**

**Location**: `docs/standards/type-safety-requirements.md`

```markdown
# Type-Safety Constitutional Requirements

**Authority**: Constitutional Principle - Type Safety
**Status**: Mandatory for all implementations
**Last Updated**: 2025-10-01

## Decimal Usage

All monetary values must use Python's Decimal type:
- ✅ Commission amounts, totals, unit prices
- ✅ Percentage calculations (convert to Decimal)
- ❌ Never use float for financial data
- ✅ Always preserve precision to 2 decimal places
- ✅ Use quantize() for rounding: `value.quantize(Decimal('0.01'))`

[... full 300-token specification ...]
```

### **2. Reference in Agents** (100 tokens)

```markdown
## Type-Safety Requirements

**REFERENCE**: `/docs/standards/type-safety-requirements.md`

**Critical Constitutional Compliance:**
All implementations must follow type-safety requirements:
- Decimal for all monetary values
- int for all quantity values
- Complete type annotations
- Pydantic validation

**Violation Consequences**: Implementation will be rejected in code review.
```

**Token Savings**: 300 tokens → 100 tokens = 67% reduction per agent

---

## 📊 Token Efficiency Analysis

### **Current State** (23 Agents in ~/.claude/agents/):

**Commission Processing Agents** (5 agents):
- commission-data-engineer.md: 300 tokens type-safety
- commission-fastapi-pro.md: 300 tokens type-safety
- commission-tdd-orchestrator.md: 300 tokens type-safety
- commission-backend-security-coder.md: 300 tokens type-safety
- commission-python-wizard.md: 300 tokens type-safety

**Total Redundancy**: 5 × 300 = 1,500 tokens

**Per Workflow Waste**:
- Load 3 agents → 900 tokens of redundancy
- 10 subagent invocations → 9,000 tokens wasted per session

### **After Standards Documents**:

**One-time Load**: `/docs/standards/type-safety-requirements.md` (300 tokens)

**Agent References**: 5 × 100 = 500 tokens

**Total Tokens**: 800 tokens (vs 1,500 tokens)

**Savings**: 700 tokens = 47% reduction

**Per Workflow**:
- Parent loads standards once: 300 tokens
- 10 subagent invocations with references: 1,000 tokens
- **Total**: 1,300 tokens (vs 3,000 tokens)
- **Savings**: 1,700 tokens = 57% reduction per workflow

---

## 🎯 Standards Document Categories

### **1. Type-Safety Requirements** (`type-safety-requirements.md`)

**Applies To**: All Python agents (python-wizard, data-engineer, backend-architect, etc.)

**Content**:
- Decimal usage for monetary values
- Type annotation requirements
- Pydantic validation patterns
- Error handling standards

**Token Budget**: ~300 tokens

**Agents Affected**: 8 agents

### **2. Research-First Pattern** (`research-first-pattern.md`)

**Applies To**: All research agents (vendor-format-research, api-design-research, etc.)

**Content**:
- Research-only mandate (never implement)
- Context loading protocol
- Research output format
- File verification requirements

**Token Budget**: ~250 tokens

**Agents Affected**: 6 agents

### **3. Constitutional Compliance** (`constitutional-compliance.md`)

**Applies To**: All agents (especially TDD orchestrator, code-reviewer)

**Content**:
- All 11 constitutional principles
- Principle validation checklist
- Compliance enforcement protocol
- Violation consequences

**Token Budget**: ~400 tokens

**Agents Affected**: 12 agents

### **4. Context Management** (`context-management.md`)

**Applies To**: All research agents and context-aware agents

**Content**:
- Context file structure
- Token budget limits (5,000 tokens per context)
- Update protocols
- Archival triggers

**Token Budget**: ~200 tokens

**Agents Affected**: 10 agents

### **5. Domain-Specific Standards** (`domain-specific/commission-processing-requirements.md`)

**Applies To**: Commission processing agents only

**Content**:
- Vendor file format expectations
- Commission calculation rules
- Reconciliation validation
- Output format specifications

**Token Budget**: ~350 tokens

**Agents Affected**: 5 agents

---

## 🔧 Implementation Protocol

### **Step 1: Create Standards Document**

1. Extract common instructions from agents
2. Consolidate into single authoritative document
3. Add constitutional authority reference
4. Include version and last-updated date
5. Save to `docs/standards/{category}.md`

### **Step 2: Update Agents**

**Replace this** (300 tokens):
```markdown
## Type-Safety Requirements

**Decimal Usage:**
- All monetary values use Decimal
[... 250+ tokens ...]
```

**With this** (100 tokens):
```markdown
## Type-Safety Requirements

**REFERENCE**: `/docs/standards/type-safety-requirements.md`

**Critical**: All implementations must follow type-safety constitutional requirements.
- Decimal for monetary values
- Complete type annotations
- Pydantic validation
```

### **Step 3: Parent Agent Loading**

Parent agent loads standards ONCE at session start:

```markdown
# Session Context Loading

1. Read `.project_status.md` - Project state
2. Read `docs/standards/type-safety-requirements.md` - Type safety
3. Read `docs/standards/constitutional-compliance.md` - Compliance
4. Read relevant context file: `docs/context-sessions/{domain}.md`

Total: ~1,500 tokens loaded once
```

Subagents reference standards (don't reload):

```markdown
**Type-Safety**: See standards document (already loaded by parent)
```

**Token Efficiency**: 1,500 tokens loaded once vs 300 tokens × 10 invocations = 1,500 savings

---

## 📚 Standards Document Template

```markdown
# {Standard Name}

**Authority**: {Constitutional Principle or Framework Requirement}
**Status**: {Mandatory | Recommended | Optional}
**Applies To**: {Agent types or domains}
**Last Updated**: {YYYY-MM-DD}
**Version**: {Semantic version}

---

## Overview

{Brief description of what this standard covers and why it exists}

## Requirements

### {Category 1}

{Detailed requirements with examples}

**Example**:
\```python
# ✅ Correct implementation
{code example}

# ❌ Incorrect implementation
{anti-pattern}
\```

### {Category 2}

{More requirements}

## Validation

How to verify compliance:
- [ ] {Validation criterion 1}
- [ ] {Validation criterion 2}

## Consequences

**Non-Compliance**: {What happens if requirements not followed}

## References

- Constitutional Principle: {Link}
- Related Standards: {Links}
- External Resources: {Links}
```

---

## 🎯 Migration Checklist

For each agent in `~/.claude/agents/`:

### **Phase 1: Identify Redundancy**
- [ ] Find repeated instruction blocks (>100 tokens)
- [ ] Group by category (type-safety, constitutional, research-first, etc.)
- [ ] Calculate token savings potential

### **Phase 2: Create Standards**
- [ ] Extract instructions to standards document
- [ ] Add constitutional authority
- [ ] Create examples and validation criteria
- [ ] Save to `docs/standards/`

### **Phase 3: Update Agents**
- [ ] Replace inline instructions with references
- [ ] Keep brief summary (100 tokens)
- [ ] Add critical reminders only
- [ ] Test agent functionality

### **Phase 4: Validate Efficiency**
- [ ] Measure token reduction per agent
- [ ] Test subagent invocation workflows
- [ ] Verify compliance maintained
- [ ] Document savings

---

## 📊 Expected Results

### **Token Efficiency Goals**:
- **Per Agent**: 67% reduction (300 → 100 tokens)
- **Per Workflow**: 57% reduction in redundant instructions
- **Session-wide**: 40-50% reduction in total token usage

### **Maintainability Goals**:
- **Single Source of Truth**: Update standards once, affects all agents
- **Consistency**: All agents reference same requirements
- **Discoverability**: Standards documented and version-controlled

### **Quality Goals**:
- **Compliance**: Easier to enforce constitutional requirements
- **Clarity**: Standards more comprehensive than agent snippets
- **Evolution**: Standards can evolve independently of agents

---

## 🚀 Next Steps

1. **Create 5 Core Standards Documents**:
   - `type-safety-requirements.md`
   - `research-first-pattern.md`
   - `constitutional-compliance.md`
   - `context-management.md`
   - `commission-processing-requirements.md`

2. **Upgrade 23 Agents** with standards references

3. **Test Token Efficiency** with real workflows

4. **Document Savings** for validation

5. **Create Installation Tooling** to apply standards

---

**Framework**: Research-First Subagent Framework v2.0
**Repository**: https://github.com/Ravenight13/research-first-subagent-framework
**Pattern**: Standards Document Integration
**Token Efficiency**: 67% improvement per agent
