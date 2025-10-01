# Three-Tier Agent Architecture (Framework v2.0)

**Status**: Approved Architecture
**Version**: 2.0.0
**Date**: 2025-10-01
**Innovation**: Quality-gated parallel development with research reuse

---

## 🎯 Core Architectural Principle

```
Tier 1: RESEARCHERS → Create comprehensive plans (research-only)
           ↓ (writes to context-sessions/)
Tier 2: PLANNING-REVIEW → Validate plans (quality gate, ONE iteration)
           ↓ (approves for implementation)
Tier 3: DO-ERS → Parallel implementation (multiple agents simultaneously)
```

**The Innovation**: Quality gate (planning-review) between research and implementation prevents flawed plans from reaching parallel execution, where errors scale across multiple agents.

---

## 🏗️ The Three Tiers

### **Tier 1: Project-Specific Researchers**

**Location**: `.claude/agents/` (project root)

**Purpose**: Deep research and comprehensive planning

**Characteristics**:
- ✅ **Research-only**: NEVER implement code directly
- ✅ **Heavy reading**: 50,000+ token context loads in isolated subagent context
- ✅ **Comprehensive planning**: Create detailed, actionable implementation plans
- ✅ **Context-aware**: Read project state before researching (don't duplicate existing work)
- ✅ **Write to context**: Store plans in `docs/context-sessions/` for do-ers to consume
- ✅ **Standards-based**: Reference `docs/standards/` (token efficiency)

**Agents**:
- `vendor-format-research` - Analyze vendor files, create parsing strategy
- `api-design-research` - Study API patterns, design endpoints
- `testing-strategy-research` - Evaluate coverage, plan test suite
- `performance-research` - Analyze bottlenecks, optimization strategy
- `security-research` - Assess vulnerabilities, remediation plan
- `frontend-architecture-research` - Study UI requirements, component plans

**Output Format** (stored in context file ~3,000 tokens):
```markdown
# {Scope} Research

## Analysis
[Detailed findings from heavy research]

## Implementation Plan
1. Task 1: [Specific, actionable task for do-ers]
   - File: backend/src/...
   - Requirements: [Clear specifications]
   - Testing: [Test approach]

2. Task 2: [Next task]
   [...]

## Testing Strategy
[Comprehensive test approach]

## Constitutional Compliance
[All 11 principles addressed]

## Implementation Tasks (for parallel delegation)
- [ ] python-wizard: Implement parser
- [ ] fastapi-pro: Build endpoint
- [ ] tdd-orchestrator: Create test suite
```

**Token Efficiency**:
- Heavy reading: 50,000 tokens (in subagent context, isolated)
- Output to orchestration chat: 500 tokens (summary only)
- Output to context file: 3,000 tokens (full plan for do-ers)
- **Savings**: Orchestration chat only loads 500 tokens, not 50,000

---

### **Tier 2: Planning-Review Agent** (Quality Gate)

**Location**: `.claude/agents/planning-review.md` (project root)

**Purpose**: Validate research plans before parallel implementation

**The Problem This Solves**:
```
Without Planning-Review:
Flawed plan → 3 do-ers implement in parallel →
Discover flaw late → 3× work wasted → Redo all 3 agents

With Planning-Review:
Flawed plan → Planning-review catches flaw →
Researcher fixes (15 min) → 3 do-ers implement validated plan →
Success on first try

**Savings**: Hours of wasted parallel work prevented by 5-minute validation
```

**Characteristics**:
- ✅ **Validation-focused**: Reviews plans for completeness, constitutional compliance, feasibility
- ✅ **Quality gate**: Catches gaps BEFORE parallel implementation starts
- ✅ **ONE-iteration feedback**: Comprehensive refinements in single pass (prevents analysis paralysis)
- ✅ **Constitutional enforcement**: Validates all 11 principles, especially Principle IV (reconciliation)
- ✅ **Standards compliance**: Checks against `docs/standards/` requirements
- ✅ **Approval authority**: Must approve before do-ers can proceed

**Validation Criteria**:
1. **Completeness**: All required sections present (file format, transformation, testing, etc.)
2. **Constitutional Compliance**: All 11 principles addressed
3. **Implementation Feasibility**: Tasks actionable and clear for do-ers
4. **Standards Compliance**: Follows type-safety, context management, domain standards

**ONE-Iteration Rule**:
```
Planning-Review → Refinements (comprehensive feedback) →
Researcher → Updated plan (addresses all refinements) →
Planning-Review → MUST APPROVE (no second iteration) →
Do-ers → Implementation
```

**Why This Works**:
- Forces planning-review to be thorough first time
- Prevents infinite refinement loops
- Maintains development velocity
- 80/20 rule: First iteration catches 80% of issues, second pass catches remaining

**Escape Hatch**: If plan fundamentally flawed after 1 iteration, escalate to orchestration chat (restart research vs proceed with risks).

**Output Format**:
```markdown
## Status: ✅ APPROVED
or
## Status: ⚠️ APPROVED WITH REFINEMENTS

### Required Refinements
1. CRITICAL: [Must fix - blocks implementation]
2. IMPORTANT: [Should fix - prevents issues]
3. RECOMMENDED: [Nice to have - improves quality]

Estimated time to address: {X} minutes
```

**Token Efficiency**:
- Reads plan: 3,000 tokens (from context file)
- Reads standards: 1,500 tokens (loaded once per session)
- Does NOT read full research context (50,000 tokens)
- Returns feedback: 800 tokens
- **Savings**: 45,000+ tokens vs re-researching

---

### **Tier 3: General-Purpose Do-ers**

**Location**: `~/.claude/agents/` (user-level)

**Purpose**: Parallel implementation based on validated plans

**Characteristics**:
- ✅ **Context-aware**: Read validated plans from `docs/context-sessions/`
- ✅ **Implementation authority**: Write code, run tests, commit changes
- ✅ **Standards-based**: Reference `docs/standards/` (token efficiency)
- ✅ **Parallel execution**: Multiple agents work simultaneously
- ✅ **General-purpose**: Not tied to specific domain, reusable across projects
- ✅ **Update context**: Document progress in context files

**Agents**:
- `python-wizard` - Implement Python modules following validated plan
- `fastapi-pro` - Build API endpoints per design specification
- `sql-pro` - Create database queries/schemas
- `tdd-orchestrator` - Write comprehensive test suites
- `backend-architect` - Refactor architecture
- `code-reviewer` - Review code quality
- `documentation-automator` - Generate documentation
- [All other general-purpose agents]

**Execution Protocol**:
```markdown
1. Read validated plan from docs/context-sessions/{domain}.md
2. Read applicable standards from docs/standards/
3. Implement specific task delegated by orchestration chat
4. Follow TDD (tests first)
5. Update context file with progress
6. Return completed work
```

**Parallel Delegation Example**:
```bash
# Orchestration chat invokes 3 agents SIMULTANEOUSLY

# Agent 1: python-wizard
Task python-wizard "Implement ACME parser per vendor-research.md task #1"

# Agent 2: fastapi-pro (PARALLEL)
Task fastapi-pro "Build endpoint per api-development.md task #2"

# Agent 3: tdd-orchestrator (PARALLEL)
Task tdd-orchestrator "Create test suite per testing-phase.md task #3"
```

**All 3 agents**:
- Read SAME validated plan (planning-review approved)
- Implement different pieces simultaneously
- Update context with their progress
- Return completed work

**Time**: Serial would be 20 + 25 + 30 = 75 minutes → Parallel is max(20, 25, 30) = **30 minutes** (2.5× speedup)

**Token Efficiency**:
- Each agent reads plan: 3,000 tokens
- Each agent reads standards: 1,500 tokens (loaded once)
- Does NOT need full research context (50,000 tokens)
- **Savings per agent**: 45,000+ tokens

---

## 📊 Complete Workflow Example

### **Scenario: Implement New ACME Vendor**

**Phase 1: Research** (Serial - Must Be Comprehensive)

**Time**: 15 minutes

```
Orchestration Chat → vendor-format-research

Agent Actions:
1. Reads .project_status.md (check current state)
2. Reads docs/context-sessions/vendor-research.md (check existing research)
3. Loads ACME vendor file (heavy reading in subagent context - 50,000 tokens)
4. Analyzes file structure, data patterns, business rules
5. Creates comprehensive implementation plan
6. Writes plan to docs/context-sessions/vendor-research.md (3,200 tokens)
7. Returns to orchestration chat: "✅ Research complete. Plan stored."

Orchestration Chat receives: 500 token summary (not 50,000 tokens)
```

**Phase 2: Planning Review** (Quality Gate - ONE Iteration Max)

**Time**: 5 + 10 = 15 minutes

```
Orchestration Chat → planning-review

Agent Actions (First Review):
1. Reads plan from vendor-research.md (3,200 tokens)
2. Reads .specify/memory/constitution.md (11 principles)
3. Reads docs/standards/commission-processing-requirements.md
4. Validates against 4 criteria:
   - Completeness: ⚠️ Missing reconciliation validation
   - Constitutional: ⚠️ Principle IV incomplete
   - Feasibility: ✅ Tasks actionable
   - Standards: ✅ Follows standards
5. Returns: "⚠️ APPROVED WITH REFINEMENTS: 3 items (1 CRITICAL, 1 IMPORTANT, 1 RECOMMENDED)"

Orchestration Chat → vendor-format-research (address feedback)

Researcher Updates (10 minutes):
1. Adds reconciliation validation strategy
2. Clarifies error handling for malformed files
3. Adds performance testing for large files
4. Updates vendor-research.md (3,450 tokens)
5. Returns: "✅ Plan updated. Ready for validation."

Orchestration Chat → planning-review (final validation)

Agent Actions (Second Review):
1. Reads updated plan (3,450 tokens)
2. Verifies all 3 refinements addressed
3. Returns: "✅ APPROVED - All refinements complete. Ready for parallel implementation."
```

**Phase 3: Parallel Implementation** (3 Agents Simultaneously)

**Time**: 25 minutes (parallel)

```
Orchestration Chat invokes 3 agents SIMULTANEOUSLY:

├─ python-wizard: "Implement parser per vendor-research.md task #1"
│  Actions:
│  1. Reads vendor-research.md (3,450 tokens - validated plan)
│  2. Reads docs/standards/type-safety-requirements.md
│  3. Implements parser in backend/src/extractors/acme/parser.py
│  4. Writes unit tests (TDD)
│  5. Updates vendor-research.md: "Task #1 ✅ Complete"
│  6. Returns: "✅ Parser implemented with 8 unit tests passing"
│  Time: 20 minutes
│
├─ fastapi-pro: "Build endpoint per api-development.md task #2"
│  Actions:
│  1. Reads api-development.md (validated plan)
│  2. Reads docs/standards/type-safety-requirements.md
│  3. Creates POST /api/vendors/acme/process endpoint
│  4. Integrates with parser
│  5. Updates api-development.md: "Task #2 ✅ Complete"
│  6. Returns: "✅ Endpoint created with request/response validation"
│  Time: 15 minutes
│
└─ tdd-orchestrator: "Create test suite per testing-phase.md task #3"
   Actions:
   1. Reads testing-phase.md (validated plan)
   2. Writes unit tests for parser
   3. Writes integration tests for endpoint
   4. Adds golden file fixtures
   5. Updates testing-phase.md: "Task #3 ✅ Complete"
   6. Returns: "✅ Test suite created: 15 tests, all passing"
   Time: 25 minutes

All 3 agents work SIMULTANEOUSLY:
Total elapsed time: max(20, 15, 25) = 25 minutes
(vs serial: 20 + 15 + 25 = 60 minutes)
Speedup: 2.4× faster
```

**Phase 4: Integration & Validation** (Orchestration Chat)

**Time**: 5 minutes

```
Orchestration Chat:
1. Reviews all 3 implementations (code + test results)
2. Runs full test suite: pytest backend/tests/acme/
   Result: 15/15 tests passing ✅
3. Validates integration: All 3 pieces work together
4. Commits work: git add . && git commit -m "feat(vendor-acme): implement parser, endpoint, tests"
5. Updates .project_status.md: "ACME vendor: ✅ Complete"
```

**Total Time**: 15 (research) + 5 (review) + 10 (refinement) + 25 (parallel impl) + 5 (integration) = **60 minutes**

**vs Traditional Serial**: 15 (research) + 60 (implementation serial) + 5 (integration) = **80 minutes**

**Speedup**: 25% faster

**vs Without Planning-Review**: Risk of flawed plan → 3 agents implement → discover flaw → redo 3× work = **120+ minutes**

**Planning-Review ROI**: 15 minutes spent on quality gate saves 60+ minutes of potential rework.

---

## 🎯 Why This Architecture Is Optimal

### **1. Token Efficiency** (Massive Savings)

**Traditional Approach** (everything in orchestration chat):
```
Orchestration chat loads:
- Vendor file analysis: 50,000 tokens
- Implementation: 30,000 tokens
- Testing: 15,000 tokens
Total: 95,000 tokens in orchestration context
```

**Three-Tier Approach**:
```
Tier 1 (Researcher):
- Heavy reading: 50,000 tokens (in subagent context, isolated)
- Returns summary: 500 tokens to orchestration chat
- Writes plan: 3,000 tokens to context file

Tier 2 (Planning-Review):
- Reads plan: 3,000 tokens (not 50,000)
- Reads standards: 1,500 tokens
- Returns feedback: 800 tokens

Tier 3 (Do-ers - 3 agents in parallel):
- Each reads plan: 3,000 tokens
- Each reads standards: 1,500 tokens (loaded once per session)
- Total: 3 × 4,500 = 13,500 tokens

Orchestration chat total: 500 + 800 + (3 × summary) = ~3,000 tokens

Savings: 95,000 → 3,000 = 97% reduction in orchestration context
```

**The Key**: Heavy research isolated in Tier 1 subagent context, never loaded into orchestration chat.

### **2. Quality Gates** (Risk Mitigation)

**Without Tier 2 (Planning-Review)**:
```
Researcher → Plan with gaps →
3 Do-ers implement in parallel →
Discover gaps during implementation →
All 3 agents blocked or produce flawed code →
Redo all 3 implementations

Cost: 3 × (implementation time + debugging) = 3-6 hours
```

**With Tier 2 (Planning-Review)**:
```
Researcher → Plan with gaps →
Planning-Review catches gaps (5 min) →
Researcher fixes gaps (10 min) →
3 Do-ers implement validated plan →
Success on first try

Cost: 15 minutes vs 3-6 hours
ROI: 12-24× time savings
```

**The Critical Insight**: Errors caught at planning stage have linear cost (researcher fixes). Errors caught at implementation stage have multiplicative cost (all parallel agents affected).

### **3. Parallel Scaling** (Speed Multiplication)

**Serial Implementation** (traditional):
```
Task 1: 20 minutes →
Task 2: 15 minutes →
Task 3: 25 minutes →
Total: 60 minutes
```

**Parallel Implementation** (Tier 3):
```
Task 1: 20 minutes ┐
Task 2: 15 minutes ├─ All simultaneous
Task 3: 25 minutes ┘
Total: max(20, 15, 25) = 25 minutes

Speedup: 60 / 25 = 2.4× faster
```

**Scaling with More Agents**:
- 2 agents: 1.5× faster
- 3 agents: 2.4× faster
- 4 agents: 3× faster
- 5 agents: 3.5× faster

**Planning-Review validates ONCE**, then N do-ers benefit from validated plan.

### **4. Research Reuse** (Efficiency Compounding)

**Traditional** (research every time):
```
Developer 1 implements vendor: 80 minutes (includes research)
Developer 2 implements same vendor later: 80 minutes (re-researches)
Total: 160 minutes
```

**Three-Tier with Stored Research**:
```
Developer 1:
- Researcher creates plan: 15 minutes → Stored in vendor-research.md
- Planning-review validates: 5 minutes
- Do-ers implement: 25 minutes (parallel)
- Total: 45 minutes

Developer 2 (later):
- Reads existing vendor-research.md: 2 minutes
- Do-ers implement (plan already validated): 25 minutes
- Total: 27 minutes

Combined: 45 + 27 = 72 minutes
vs Traditional: 160 minutes
Savings: 55% reduction
```

**The Compounding Effect**: Research stored in context files is reused by future developers without re-researching.

### **5. ONE-Iteration Constraint** (Momentum Preservation)

**The Trap**: Infinite planning refinement loops
```
Plan v1 → Feedback → Plan v2 → Feedback → Plan v3 → Feedback → ...
Never reaches implementation (analysis paralysis)
```

**The Solution**: ONE refinement iteration maximum
```
Plan v1 → Comprehensive Feedback (planning-review thorough) →
Plan v2 → MUST APPROVE (no second iteration) →
Implementation proceeds

If plan truly broken after v2: Escalate to orchestration chat (rare case)
```

**Why This Works**:
- Forces planning-review to be comprehensive first time (can't rely on multiple iterations)
- Prevents endless refinement cycles
- 80/20 rule: First iteration catches 80% of issues
- Maintains development velocity
- Rare escape hatch for fundamentally flawed plans

---

## 🔧 Agent Specifications Summary

### **Tier 1: Researchers** (`.claude/agents/`)

**Template**: See `docs/agent-templates/vendor-format-research-agent-template.md`

**Key Requirements**:
- ✅ Research-only (NEVER implement code)
- ✅ Heavy reading in subagent context
- ✅ Write comprehensive plans to `docs/context-sessions/`
- ✅ Return lightweight summaries to orchestration chat
- ✅ Read project state before researching (context-aware)
- ✅ Reference standards documents (token efficiency)

**Agents to Create**:
- vendor-format-research ✅ (exists)
- api-design-research ✅ (exists)
- testing-strategy-research ✅ (exists)
- performance-research (create)
- security-research (create)
- frontend-architecture-research (create)

### **Tier 2: Planning-Review** (`.claude/agents/planning-review.md`)

**Specification**: ✅ Created in `.claude/agents/planning-review.md`

**Key Requirements**:
- ✅ Validate plans for completeness, constitutional compliance, feasibility, standards
- ✅ Comprehensive feedback in ONE iteration (prevent analysis paralysis)
- ✅ MUST approve after one refinement (no infinite loops)
- ✅ Enforce all 11 constitutional principles (especially Principle IV)
- ✅ Clear, actionable refinements with time estimates

### **Tier 3: Do-ers** (`~/.claude/agents/`)

**Template**: Add context-awareness protocol to existing agents

**Key Requirements**:
- ✅ Read validated plans from `docs/context-sessions/`
- ✅ Reference standards from `docs/standards/` (token efficiency)
- ✅ Implement code following plans
- ✅ Update context files with progress
- ✅ Work in parallel with other do-ers

**Agents to Upgrade** (23 existing agents):
- python-wizard (add context-awareness)
- fastapi-pro (add context-awareness)
- tdd-orchestrator (add context-awareness)
- [All other agents in ~/.claude/agents/]

---

## 📊 Expected Outcomes

### **Token Efficiency**

**Per Workflow**:
- Traditional: 95,000 tokens in orchestration chat
- Three-Tier: 3,000 tokens in orchestration chat
- **Savings**: 97% reduction

**Research Reuse**:
- First implementation: 45 minutes (includes research)
- Second implementation: 27 minutes (reuses research)
- **Compounding**: 40% time reduction on repeated work

### **Quality Impact**

**Planning-Review Quality Gate**:
- Catches 90%+ of plan gaps before implementation
- Prevents wasted parallel work (3× error scaling)
- ROI: 5-minute validation saves hours of rework

**Constitutional Compliance**:
- All 11 principles validated before implementation
- Principle IV (Data Integrity) specifically enforced
- Reduces code review rejections by 80%+

### **Speed Improvements**

**Parallel Execution**:
- 3 agents: 2.4× speedup vs serial
- 4 agents: 3× speedup vs serial
- 5 agents: 3.5× speedup vs serial

**Workflow Optimization**:
- Traditional: 80 minutes (serial, with rework)
- Three-Tier: 60 minutes (parallel, validated plan)
- **Improvement**: 25% faster

**With Planning-Review Preventing Rework**:
- Without quality gate: 120+ minutes (includes rework from flawed plan)
- With quality gate: 60 minutes
- **Improvement**: 50% faster

### **Maintainability**

**Standards Documents Integration**:
- Token savings: 67% per agent (200-300 tokens reduced to 100 tokens)
- Update efficiency: 8× faster (update 1 standard vs 8 agents)
- Consistency: 100% (all agents reference same standards)

**Context Management**:
- Research stored and reused: No repeated analysis
- Plans validated before implementation: Fewer surprises
- Progress tracked in context files: Easy to resume work

---

## 🚀 Implementation Roadmap

### **Phase 1: Standards Documents** (Week 1)

**Create 5 core standards** to eliminate token redundancy:
1. type-safety-requirements.md (300 tokens, saves 1,600)
2. research-first-pattern.md (250 tokens, saves 900)
3. constitutional-compliance.md (400 tokens, saves 2,400)
4. context-management.md (200 tokens, saves 1,500)
5. commission-processing-requirements.md (350 tokens, saves 1,250)

**Deliverable**: `docs/standards/*.md` in framework repository

### **Phase 2: Planning-Review Agent** (Week 1)

**Status**: ✅ COMPLETE

**Deliverable**: `.claude/agents/planning-review.md` created

### **Phase 3: Upgrade Do-ers** (Week 2)

**Add context-awareness to 23 existing agents**:
- Add protocol: "Read plan from docs/context-sessions/"
- Add protocol: "Reference standards from docs/standards/"
- Add protocol: "Update context with progress"

**Deliverable**: Updated agents in `~/.claude/agents/`

### **Phase 4: Framework Documentation** (Week 2)

**Update framework repository**:
- Add THREE_TIER_ARCHITECTURE.md ✅ (this document)
- Update README.md with three-tier explanation
- Update ONBOARDING_PROMPT.md with planning-review workflow
- Create templates for each tier

**Deliverable**: Framework v2.0 documentation complete

### **Phase 5: Testing & Validation** (Week 3)

**Validate workflow**:
- Test complete workflow (research → review → parallel implementation)
- Measure token efficiency (target: 97% reduction)
- Measure speed improvements (target: 2-3× with parallel execution)
- Measure quality impact (target: 90%+ gaps caught by planning-review)

**Deliverable**: Validated three-tier architecture with metrics

---

## ✅ Success Criteria

**Architecture is successful when**:

### **Token Efficiency**
- [ ] Orchestration chat token usage reduced by >90%
- [ ] Research isolated in Tier 1 subagent contexts
- [ ] Standards documents reduce agent size by >60%

### **Quality Gates**
- [ ] Planning-review catches >90% of plan gaps
- [ ] ONE-iteration rule prevents analysis paralysis
- [ ] Constitutional compliance validated before implementation

### **Parallel Execution**
- [ ] Multiple do-ers work simultaneously on validated plans
- [ ] Speed improvement: 2-3× with 3-5 agents
- [ ] No implementation conflicts (clear task delegation)

### **Research Reuse**
- [ ] Plans stored in context files
- [ ] Future implementations reuse existing research
- [ ] 40%+ time reduction on repeated work

### **Workflow Integration**
- [ ] Natural flow: Research → Review → Implement
- [ ] Quality gate prevents wasted parallel work
- [ ] Orchestration chat coordinates all tiers

---

**Framework**: Research-First Subagent Framework v2.0
**Architecture**: Three-Tier (Researchers → Planning-Review → Do-ers)
**Innovation**: Quality-gated parallel development with research reuse
**Status**: Architecture defined, planning-review agent created, ready for implementation
**Next**: Phase 1 (Standards Documents) and Phase 3 (Upgrade Do-ers)
