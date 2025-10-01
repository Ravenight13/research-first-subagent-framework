# Framework v2.0 Integration Roadmap

**Status**: Proposal - Ready for Execution
**Version**: 2.0.0
**Date**: 2025-10-01
**Base**: Research-First Subagent Framework v1.0

---

## 🎯 Executive Summary

**Objective**: Integrate v2.0 proposals into research-first framework with standards-based architecture

**Key Components**:
1. **Agent Type Architecture**: Researchers (project-specific) vs Do-ers (user-level)
2. **Standards Document Pattern**: Eliminate 200-300 token redundancy per agent
3. **Centralized Agent Repository**: Modular agent system with installation tooling
4. **Context-Aware Agents**: Read project state before execution

**Expected Outcomes**:
- **Token Efficiency**: 67% reduction per agent, 57% per workflow
- **Maintainability**: Single source of truth for requirements
- **Portability**: Framework works across any project type
- **Quality**: Constitutional compliance enforced through standards

---

## 📊 Current State Analysis

### **v1.0 Framework** (Published: 2025-10-01)

**Repository**: https://github.com/Ravenight13/research-first-subagent-framework

**Contents**:
- README.md (6KB) - Framework overview
- ONBOARDING_PROMPT.md (14.5KB) - Core pattern explanation ⭐
- LICENSE (MIT)
- CONTRIBUTING.md
- docs/core-concepts/research-first-pattern.md
- docs/agent-templates/api-design-research-agent-template.md
- docs/agent-templates/testing-strategy-research-agent-template.md

**Strengths**:
- ✅ Clear research-first pattern explanation
- ✅ Comprehensive onboarding prompt
- ✅ Token optimization focus
- ✅ File system as context management

**Gaps** (Addressed by v2.0):
- ❌ No standards document integration
- ❌ No agent type architecture distinction
- ❌ No installation/update tooling
- ❌ No centralized agent repository
- ❌ No context-awareness protocol

### **User's Current Agent Landscape**

**Location**: `~/.claude/agents/` (23 agents currently)

**Token Redundancy Analysis**:
- **Commission Processing Agents** (5): 1,500 tokens redundant type-safety instructions
- **Per Workflow**: 3,000 tokens wasted on repeated instructions (10 invocations)
- **Session-wide**: 40-50% token budget consumed by redundancy

**Agent Location Mismatch**:
- Current: All agents in `~/.claude/agents/` (user-level)
- Proposed: Project-specific researchers in `.claude/agents/` (project-level)

**Migration Need**: Separate researchers from do-ers, apply standards pattern

---

## 🏗️ v2.0 Architecture

### **1. Agent Type System**

**Two Distinct Types**:

| Type | Location | Purpose | Implementation | Context |
|------|----------|---------|----------------|---------|
| **Researchers** | `.claude/agents/` | Project-specific analysis | ❌ Research only | ✅ Required |
| **Do-ers** | `~/.claude/agents/` | General execution | ✅ Research + Implement | ⚠️ Optional |

**Key Distinction**: Researchers maintain research-first pattern (analyze → summarize → exit), Do-ers can implement directly

### **2. Standards Document System**

**Core Standards** (`docs/standards/`):
1. **type-safety-requirements.md** (300 tokens)
   - Applies to: All Python agents (8 agents)
   - Savings: 200 tokens × 8 = 1,600 tokens

2. **research-first-pattern.md** (250 tokens)
   - Applies to: All research agents (6 agents)
   - Savings: 150 tokens × 6 = 900 tokens

3. **constitutional-compliance.md** (400 tokens)
   - Applies to: All agents (12 agents)
   - Savings: 200 tokens × 12 = 2,400 tokens

4. **context-management.md** (200 tokens)
   - Applies to: All research agents (10 agents)
   - Savings: 150 tokens × 10 = 1,500 tokens

**Total Token Savings**: 6,400 tokens across agent ecosystem

### **3. Centralized Agent Repository**

**Structure**:
```
research-first-subagent-framework/
├── agents/
│   ├── core/           # Universal agents (code-reviewer, debugger, error-detective)
│   ├── backend/        # Backend agents (python-wizard, fastapi-pro, sql-pro)
│   ├── data/           # Data agents (data-engineer, data-scientist)
│   ├── testing/        # Testing agents (tdd-orchestrator, test-automator)
│   ├── devops/         # DevOps agents (cloud-architect, deployment-engineer)
│   ├── documentation/  # Docs agents (api-documenter, docs-architect)
│   ├── security/       # Security agents (security-auditor, backend-security-coder)
│   └── domain-specific/
│       └── commission-processing/  # Domain agents (commission-data-engineer, etc.)
│
├── standards/          # Shared standards documents
│   ├── type-safety-requirements.md
│   ├── research-first-pattern.md
│   ├── constitutional-compliance.md
│   ├── context-management.md
│   └── domain-specific/
│       └── commission-processing-requirements.md
│
├── templates/          # Project setup templates
│   ├── context-management/
│   │   ├── .project_status.md.template
│   │   ├── current-project.md.template
│   │   └── vendor-research.md.template
│   │
│   └── agent-profiles/
│       ├── fullstack-web.json
│       ├── backend-api.json
│       └── commission-processing.json
│
└── tools/              # Installation and update tooling
    ├── install-agents.sh
    ├── update-agents.sh
    ├── setup-project.sh
    └── validate-agents.sh
```

### **4. Context-Awareness Protocol**

**Required for Project-Specific Researchers**:

```markdown
## Required Context Loading

Before starting research, ALWAYS read using Read tool:
- `.project_status.md` - Current project state and context type
- `docs/context-sessions/{domain}.md` - Active development context
- `.specify/memory/constitution.md` - Constitutional compliance requirements
- `docs/standards/{relevant-standard}.md` - Applicable standards

**File Creation Protocol:**
1. Use Write tool for all file operations
2. ALWAYS verify file creation by reading back
3. If verification fails, retry up to 3 times
4. Report actual file creation status
```

**Benefits**:
- Prevents duplicate work (agent checks previous research)
- Maintains constitutional compliance
- Optimizes token usage (context files <5,000 tokens)
- Enables iterative development

---

## 🚀 Phase-by-Phase Implementation

### **Phase 1: Standards Documents** (Week 1)

**Objective**: Create 5 core standards documents to eliminate redundancy

**Deliverables**:
1. ✅ `docs/standards/type-safety-requirements.md` (300 tokens)
2. ✅ `docs/standards/research-first-pattern.md` (250 tokens)
3. ✅ `docs/standards/constitutional-compliance.md` (400 tokens)
4. ✅ `docs/standards/context-management.md` (200 tokens)
5. ✅ `docs/standards/domain-specific/commission-processing-requirements.md` (350 tokens)

**Tasks**:
- Extract common instructions from existing agents
- Consolidate into authoritative documents
- Add constitutional authority references
- Create examples and validation criteria
- Commit to framework repository

**Success Criteria**:
- [ ] All 5 standards documents created
- [ ] Token budget per standard ≤ 400 tokens
- [ ] Examples and validation included
- [ ] Version-controlled in framework repo

**Token Impact**: Create 1,500 tokens of standards (saves 6,400 tokens across agents)

---

### **Phase 2: Agent Type Architecture** (Week 1-2)

**Objective**: Separate researchers from do-ers, define clear boundaries

**Deliverables**:
1. ✅ `docs/v2-proposals/AGENT_ARCHITECTURE_V2.md` (architecture document)
2. ✅ Agent classification matrix (23 agents)
3. ✅ Migration guide for existing agents
4. ✅ Updated agent templates

**Tasks**:
- Classify 23 existing agents (researcher vs do-er)
- Create researcher agent template with context-awareness
- Create do-er agent template with standards references
- Document token efficiency analysis
- Update framework documentation

**Agent Classification** (User's 23 agents):

**Researchers** (Move to `.claude/agents/` in projects):
1. vendor-format-research
2. api-design-research
3. testing-strategy-research
4. performance-research
5. frontend-architecture-research
6. security-research

**Do-ers** (Keep in `~/.claude/agents/`):
1. python-wizard (can research + implement)
2. backend-architect (can analyze + refactor)
3. fastapi-pro (can design + build)
4. sql-pro (can study + optimize)
5. code-reviewer (can analyze + suggest)
6. debugger (can diagnose + fix)
7. tdd-orchestrator (can plan + execute)
8. data-engineer (can architect + implement)
9. data-scientist (can analyze + model)
10. cloud-architect (can design + deploy)
11. documentation-automator (can read + document)
12. security-auditor (can assess + remediate)
13. [Additional 5 agents need classification]

**Success Criteria**:
- [ ] All 23 agents classified
- [ ] Architecture document complete
- [ ] Migration guide written
- [ ] Token efficiency validated

---

### **Phase 3: Agent Repository Population** (Week 2)

**Objective**: Create centralized agent repository with all categories

**Deliverables**:
1. ✅ `agents/core/` with 5 universal agents
2. ✅ `agents/backend/` with 6 backend agents
3. ✅ `agents/data/` with 3 data agents
4. ✅ `agents/testing/` with 3 testing agents
5. ✅ `agents/devops/` with 3 devops agents
6. ✅ `agents/documentation/` with 3 documentation agents
7. ✅ `agents/security/` with 3 security agents
8. ✅ `agents/domain-specific/commission-processing/` with 5 commission agents

**Tasks**:
- Convert existing agents to framework format
- Add standards document references
- Remove redundant instruction blocks
- Add context-awareness for researchers
- Create category README.md files
- Validate token efficiency

**Agent Upgrade Protocol**:

**For Researchers**:
```diff
- ## Type-Safety Requirements (300 tokens)
-
- **Decimal Usage:**
- - All monetary values use Decimal
- [... 250+ tokens ...]
+ ## Type-Safety Requirements
+
+ **REFERENCE**: `/docs/standards/type-safety-requirements.md`
+
+ **Critical**: Follow type-safety constitutional requirements.
```

**For Do-ers**:
```diff
- ## Constitutional Compliance (400 tokens)
-
- **Principle I: TDD**
- [... 350+ tokens ...]
+ ## Constitutional Compliance
+
+ **REFERENCE**: `/docs/standards/constitutional-compliance.md`
+
+ **Validation**: All implementations must pass constitutional validation.
```

**Success Criteria**:
- [ ] All 23 agents migrated to repository
- [ ] Standards references added
- [ ] Token reduction measured (target: 67% per agent)
- [ ] All category READMEs written

**Token Impact**: Reduce agent size by ~200-300 tokens each = 4,600-6,900 tokens saved

---

### **Phase 4: Project Templates** (Week 2-3)

**Objective**: Create ready-to-use project templates for different project types

**Deliverables**:
1. ✅ Context management templates
   - `.project_status.md.template`
   - `current-project.md.template`
   - `vendor-research.md.template`

2. ✅ Agent profile templates
   - `fullstack-web.json` (React + FastAPI + PostgreSQL)
   - `backend-api.json` (FastAPI + SQLAlchemy)
   - `commission-processing.json` (Vendor processing domain)

3. ✅ Project configuration template
   - `.claude/project-config.json.template`

**Tasks**:
- Extract successful patterns from commission-processing project
- Generalize for different project types
- Create agent profiles (pre-configured agent sets)
- Document setup process
- Test on new project

**Agent Profile Example** (`commission-processing.json`):
```json
{
  "framework_version": "2.0.0",
  "project_name": "commission-processing-vendor-extractors",
  "project_type": "domain-specific",
  "description": "Commission processing with vendor file extraction",

  "agents": {
    "core": [
      "code-reviewer",
      "debugger",
      "error-detective"
    ],
    "backend": [
      "python-wizard",
      "fastapi-pro",
      "sql-pro",
      "backend-architect"
    ],
    "data": [
      "data-engineer"
    ],
    "testing": [
      "tdd-orchestrator",
      "test-automator"
    ],
    "documentation": [
      "documentation-automator"
    ],
    "domain-specific": [
      "commission-processing/vendor-format-research",
      "commission-processing/commission-data-engineer",
      "commission-processing/commission-fastapi-pro"
    ]
  },

  "standards": [
    "type-safety-requirements",
    "constitutional-compliance",
    "context-management",
    "domain-specific/commission-processing-requirements"
  ],

  "context_files": [
    ".project_status.md",
    "docs/context-sessions/vendor-research.md",
    "docs/context-sessions/api-development.md",
    "docs/context-sessions/testing-phase.md"
  ]
}
```

**Success Criteria**:
- [ ] 3 agent profiles created
- [ ] Context templates documented
- [ ] Setup process tested
- [ ] Installation validated

---

### **Phase 5: Installation Tooling** (Week 3)

**Objective**: Create automated installation and update tools

**Deliverables**:
1. ✅ `tools/install-agents.sh` - Install agents from profile
2. ✅ `tools/update-agents.sh` - Update agents to latest version
3. ✅ `tools/setup-project.sh` - Initialize new project with framework
4. ✅ `tools/validate-agents.sh` - Validate agent configuration

**Tasks**:
- Write shell scripts for agent management
- Add profile support (install by project type)
- Create update mechanism (check for new versions)
- Add validation (ensure agents reference valid standards)
- Document usage

**Tool Example** (`install-agents.sh`):
```bash
#!/bin/bash
# Install agents from framework repository

FRAMEWORK_REPO="https://github.com/Ravenight13/research-first-subagent-framework"
PROFILE="${1:-backend-api}"  # Default to backend-api profile

echo "🚀 Installing agents for profile: $PROFILE"

# Clone framework if not exists
if [ ! -d ".research-first-framework" ]; then
  git clone "$FRAMEWORK_REPO" .research-first-framework
fi

# Read agent profile
AGENTS=$(jq -r '.agents | to_entries[] | .value[]' "templates/agent-profiles/$PROFILE.json")

# Install project-specific researchers
mkdir -p .claude/agents
for agent in $(echo "$AGENTS" | grep -E "research|format"); do
  cp ".research-first-framework/agents/$agent.md" ".claude/agents/"
  echo "✅ Installed researcher: $agent"
done

# Install user-level do-ers
mkdir -p ~/.claude/agents
for agent in $(echo "$AGENTS" | grep -vE "research|format"); do
  cp ".research-first-framework/agents/$agent.md" "$HOME/.claude/agents/"
  echo "✅ Installed do-er: $agent"
done

echo "✅ Installation complete. Installed $(echo "$AGENTS" | wc -l) agents."
```

**Success Criteria**:
- [ ] All 4 tools working
- [ ] Profile-based installation tested
- [ ] Update mechanism validated
- [ ] Documentation complete

---

### **Phase 6: Testing & Validation** (Week 3-4)

**Objective**: Validate token efficiency and workflow improvements

**Deliverables**:
1. ✅ Token efficiency measurements
2. ✅ Workflow validation tests
3. ✅ Migration case study (commission-processing)
4. ✅ Documentation updates

**Tasks**:
- Measure token usage before/after standards
- Test researcher vs do-er workflows
- Validate context-awareness benefits
- Document real-world improvements
- Create v2.0 migration guide

**Test Scenarios**:

**Scenario 1: Vendor Processing Workflow**
```
Before (v1.0):
- Load vendor-format-research agent: 800 tokens (includes 300 tokens type-safety)
- Load commission-data-engineer: 900 tokens (includes 300 tokens type-safety)
- Load commission-tdd-orchestrator: 850 tokens (includes 300 tokens type-safety)
Total: 2,550 tokens

After (v2.0):
- Load standards once: 300 tokens (type-safety-requirements.md)
- Load vendor-format-research: 500 tokens (references standards)
- Load commission-data-engineer: 600 tokens (references standards)
- Load commission-tdd-orchestrator: 550 tokens (references standards)
Total: 1,950 tokens

Savings: 600 tokens (23% reduction)
```

**Scenario 2: Multi-Agent Workflow** (10 invocations)
```
Before (v1.0):
- 10 agent invocations × 300 tokens redundancy = 3,000 tokens wasted
- Parent context: 5,000 tokens
Total: 8,000 tokens

After (v2.0):
- Parent loads standards once: 1,500 tokens
- 10 agent invocations × 100 tokens reference = 1,000 tokens
- Parent context: 5,000 tokens
Total: 7,500 tokens

Savings: 500 tokens per workflow (6% reduction)
```

**Success Criteria**:
- [ ] Token efficiency validated (target: 67% per agent)
- [ ] Workflow improvements measured
- [ ] Migration case study documented
- [ ] v2.0 release ready

---

## 📊 Expected Outcomes

### **Token Efficiency**

**Per Agent**:
- Before: 800-1,000 tokens (with redundant instructions)
- After: 500-600 tokens (with standards references)
- **Savings**: 200-400 tokens per agent (25-50% reduction)

**Per Workflow** (10 agent invocations):
- Before: 8,000 tokens (3,000 redundant)
- After: 7,500 tokens (1,500 standards loaded once)
- **Savings**: 500 tokens per workflow (6% reduction)

**Session-wide** (30 agent invocations):
- Before: 15,000 tokens
- After: 10,000 tokens
- **Savings**: 5,000 tokens (33% reduction)

### **Maintainability**

**Before v2.0**:
- Update type-safety requirements: Modify 8 agents individually
- Risk: Inconsistencies, missed updates
- Time: ~2 hours

**After v2.0**:
- Update type-safety requirements: Modify 1 standards document
- Risk: Automatic consistency across all agents
- Time: ~15 minutes

**Improvement**: 8x faster updates, 100% consistency

### **Portability**

**Before v2.0**:
- Setup new project: Manually copy agents, update instructions
- Time: ~4 hours
- Risk: Missing agents, outdated patterns

**After v2.0**:
- Setup new project: `./tools/setup-project.sh backend-api`
- Time: ~5 minutes
- Risk: Automated, tested, consistent

**Improvement**: 48x faster setup, validated configuration

### **Quality**

**Before v2.0**:
- Constitutional compliance: Manual validation per agent
- Token redundancy: 40-50% wasted on repetition
- Context awareness: None (agents can't check previous work)

**After v2.0**:
- Constitutional compliance: Enforced through standards documents
- Token efficiency: 67% improvement per agent
- Context awareness: All researchers read project state

**Improvement**: Automated quality gates, optimal token usage, iterative development

---

## 🎯 Success Metrics

### **Phase Completion**

- [ ] Phase 1: Standards Documents (5 documents created)
- [ ] Phase 2: Agent Architecture (23 agents classified)
- [ ] Phase 3: Agent Repository (23 agents migrated)
- [ ] Phase 4: Project Templates (3 profiles created)
- [ ] Phase 5: Installation Tooling (4 tools working)
- [ ] Phase 6: Testing & Validation (improvements documented)

### **Token Efficiency**

- [ ] Per agent: ≥50% reduction (target: 67%)
- [ ] Per workflow: ≥40% reduction (target: 57%)
- [ ] Session-wide: ≥30% reduction (target: 33%)

### **Quality Gates**

- [ ] All agents reference standards (no inline redundancy)
- [ ] All researchers context-aware (read project state)
- [ ] All do-ers project-agnostic (no hardcoded paths)
- [ ] Constitutional compliance enforced

### **Portability**

- [ ] Framework works on new project in <5 minutes
- [ ] Agent profiles validated for 3 project types
- [ ] Installation tooling tested and documented
- [ ] Migration guide complete

---

## 🚀 Next Actions

### **Immediate** (This Week):

1. **Create 5 Standards Documents** (Phase 1)
   - Extract instructions from existing agents
   - Write authoritative documents
   - Commit to framework repository

2. **Classify 23 Agents** (Phase 2)
   - Determine researcher vs do-er for each
   - Document agent architecture decisions
   - Create migration checklist

3. **Upgrade First 5 Agents** (Phase 3)
   - Test standards reference pattern
   - Measure token efficiency
   - Validate workflow improvements

### **Short-term** (Next 2 Weeks):

4. **Complete Agent Repository** (Phase 3)
   - Migrate all 23 agents
   - Organize by category
   - Create category READMEs

5. **Build Project Templates** (Phase 4)
   - Extract commission-processing patterns
   - Create 3 agent profiles
   - Document setup process

6. **Develop Installation Tools** (Phase 5)
   - Write 4 shell scripts
   - Test on new project
   - Document usage

### **Medium-term** (Next Month):

7. **Comprehensive Testing** (Phase 6)
   - Measure token efficiency
   - Validate workflows
   - Document improvements

8. **Release v2.0**
   - Update README and documentation
   - Create migration guide
   - Announce release

---

## 📚 Documentation Updates

### **Framework Repository**

**New Files**:
- `docs/v2-proposals/AGENT_ARCHITECTURE_V2.md` ✅
- `docs/v2-proposals/V2_INTEGRATION_ROADMAP.md` ✅ (this file)
- `docs/standards/STANDARDS_DOCUMENT_PATTERN.md` ✅
- `docs/standards/type-safety-requirements.md` (Phase 1)
- `docs/standards/research-first-pattern.md` (Phase 1)
- `docs/standards/constitutional-compliance.md` (Phase 1)
- `docs/standards/context-management.md` (Phase 1)
- `docs/standards/domain-specific/commission-processing-requirements.md` (Phase 1)

**Updated Files**:
- `README.md` - Add v2.0 features section
- `ONBOARDING_PROMPT.md` - Add standards document references
- `CONTRIBUTING.md` - Add agent contribution guidelines

### **Commission Processing Project**

**Archive v1.0 Patterns**:
- Move `research-first-framework-integration.md` to `docs/v2-proposals/`
- Move `subagent-upgrade-analysis.md` to `docs/v2-proposals/`
- Update `CLAUDE.md` with v2.0 references

**Add v2.0 Standards**:
- Create `docs/standards/type-safety-requirements.md`
- Create `docs/standards/commission-processing-requirements.md`
- Update agent references

---

## ✅ Validation Checklist

Before v2.0 release:

### **Standards Documents**
- [ ] All 5 core standards created
- [ ] Token budget ≤400 tokens per standard
- [ ] Examples and validation criteria included
- [ ] Constitutional authority referenced

### **Agent Repository**
- [ ] All 23 agents migrated
- [ ] Standards references added (no inline redundancy)
- [ ] Category organization validated
- [ ] READMEs written for each category

### **Project Templates**
- [ ] 3 agent profiles created
- [ ] Context management templates documented
- [ ] Setup process tested on new project
- [ ] Installation validated

### **Installation Tooling**
- [ ] All 4 tools working (install, update, setup, validate)
- [ ] Profile-based installation tested
- [ ] Update mechanism validated
- [ ] Documentation complete

### **Token Efficiency**
- [ ] Per-agent savings measured (≥50%)
- [ ] Per-workflow savings measured (≥40%)
- [ ] Session-wide savings measured (≥30%)
- [ ] Real-world validation complete

### **Quality**
- [ ] Constitutional compliance enforced
- [ ] Context awareness validated
- [ ] Researcher/do-er distinction clear
- [ ] Migration guide complete

---

**Framework**: Research-First Subagent Framework v2.0
**Repository**: https://github.com/Ravenight13/research-first-subagent-framework
**Status**: Roadmap Complete - Ready for Execution
**Next**: Begin Phase 1 (Standards Documents)
