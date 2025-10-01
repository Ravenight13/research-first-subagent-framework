# Research-First Subagent Framework

**Version:** 1.0.0
**Pattern Source:** [YouTube: How sub agent works](https://www.youtube.com/watch?v=LCYBVpSB0Wo)
**Proven Results:** 70% speed improvement, optimal token usage, context preservation

## What Is This?

A framework for using Claude Code subagents in **research-first mode**, solving the context loss and token consumption problems of implementation-focused subagents.

### The Problem (Traditional Subagents)

Traditional implementation-focused subagents create unfixable context loss:

> "The moment if whatever sub agent implemented is not 100% correct and you want agent to fix it. That's where the problem begin because for each agent it only has very limited information about what is going on."

**What Goes Wrong:**
1. Parent agent delegates implementation to subagent
2. Subagent implements code in isolated context
3. Bug discovered in implementation
4. **Parent can't fix** - lacks implementation details from subagent context
5. **Subsequent subagent calls start fresh** - no prior context available

### The Solution (Research-First Pattern)

> "Sub agent works best when they just looking for information and provide a small amount of summary back to main conversation thread."

**How It Works:**
1. **Subagent researches** - Heavy file reading in subagent context (thousands of tokens)
2. **Subagent summarizes** - Returns actionable summary to parent (hundreds of tokens)
3. **Parent implements** - Maintains full implementation context, can iterate/debug
4. **File system persists** - Research results saved to filesystem, available across sessions

**Key Benefits:**
- ✅ 70% speed improvement (proven in production)
- ✅ Optimal token usage (massive reads in subagent context)
- ✅ Context preservation (parent maintains implementation context)
- ✅ Iterative debugging (parent can fix implementation issues)
- ✅ Session persistence (research results survive conversation compacting)

## Quick Start

### 1. Read the Onboarding Prompt
Copy-paste [ONBOARDING_PROMPT.md](./ONBOARDING_PROMPT.md) into your Claude Code session to learn the complete pattern.

### 2. Understand the Core Pattern
Read [docs/core-concepts/research-first-pattern.md](./docs/core-concepts/research-first-pattern.md) for the foundational pattern explanation.

### 3. Create Your First Research Agent
Follow [docs/implementation-guides/quick-start.md](./docs/implementation-guides/quick-start.md) to create a project-specific research agent.

### 4. Set Up Context Management
Follow [docs/implementation-guides/context-management-setup.md](./docs/implementation-guides/context-management-setup.md) for token optimization.

## Framework Structure

```
research-first-framework/
├── README.md                          # This file
├── ONBOARDING_PROMPT.md              # Complete copy-paste prompt for new projects
├── docs/
│   ├── core-concepts/                 # Understand the pattern
│   │   ├── research-first-pattern.md
│   │   ├── context-management.md
│   │   └── token-optimization.md
│   ├── agent-templates/               # Ready-to-customize agents
│   │   ├── data-format-research-agent.md
│   │   ├── api-design-research-agent.md
│   │   └── testing-strategy-research-agent.md
│   ├── implementation-guides/         # Step-by-step setup
│   │   ├── quick-start.md
│   │   ├── creating-research-agents.md
│   │   ├── context-management-setup.md
│   │   └── troubleshooting.md
│   └── examples/                      # Real-world case studies
│       └── commission-processing-case-study.md
└── .claude/agent-templates/           # Agent definition templates
```

## Core Concepts

### Research-First Pattern
- **Research**: Subagents analyze, plan, and summarize (read-heavy)
- **Implement**: Parent agent codes following research (maintains context)
- **Validate**: Automated or optional subagent verification

### Context Management
- **File System as Context**: Research results saved to markdown files
- **Token Optimization**: Massive reads in subagent context, summaries in parent
- **Session Persistence**: Results survive conversation compacting

### Agent Architecture
- **Research Specialists**: Domain-specific analysis agents
- **Research-Only Mandate**: Agents NEVER implement code directly
- **Actionable Outputs**: Summaries drive parent agent implementation

## Case Studies

### Commission Processing System
- **Scale**: 7,415 lines of reference material, 6 production agents
- **Results**: 37/37 tests passing, 100% success rate
- **Impact**: 70% speed improvement, optimal token usage
- **Details**: [docs/examples/commission-processing-case-study.md](./docs/examples/commission-processing-case-study.md)

## When to Use Research-First

### ✅ Use For
- **New feature implementation** (>50 lines of code)
- **Data format analysis** (vendor files, APIs, databases)
- **Architecture design** (API endpoints, system design)
- **Security-critical changes** (authentication, validation)
- **Complex refactoring** (multi-file changes)

### ⚠️ Optional For
- **Simple bug fixes** (<10 lines)
- **Documentation updates**
- **Configuration changes**

### ❌ Never For
- **Git operations** (commit, push, branch)
- **Environment setup** (dependency installation)
- **Trivial changes** (<10 lines, single file)

## Contributing

We welcome contributions! See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines on:
- Proposing new agent templates
- Improving documentation
- Sharing case studies
- Reporting issues

## License

MIT License - See [LICENSE](./LICENSE) file for details.

## Credits

- **Pattern Discovery**: Based on insights from [YouTube: How sub agent works](https://www.youtube.com/watch?v=LCYBVpSB0Wo)
- **Production Validation**: Commission Processing Vendor Extractors project
- **Framework Development**: Extracted and generalized for community use

---

**Get Started**: Read [ONBOARDING_PROMPT.md](./ONBOARDING_PROMPT.md) to integrate this pattern into your project in <30 minutes.
