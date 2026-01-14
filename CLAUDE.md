# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a template repository for exploring different Git workflow use cases to identify optimal solutions for a small-medium SAAS company. The company maintains a procurement platform (Adam) with interconnected applications and has a QA:Dev ratio of 1:10, requiring fast feature shipping and bug fixing.

## Repository Structure

```
template-repo/
├── input/                    # Problem statements and requirements
│   └── problem-statement.prompt.md
├── output/                   # Research outputs and findings
├── .opencode/               # OpenAgents framework (AI agent system)
│   ├── agent/              # AI agents for development workflow
│   ├── command/            # Custom slash commands
│   ├── context/            # Coding patterns and standards
│   ├── skill/              # Specialized skills
│   └── tool/               # Optional tools (Gemini AI)
├── AGENTS.md               # Development guidelines for AI agents
└── README.md               # Project overview
```

## Key Context Files

### Current Git Workflow (from input/problem-statement.prompt.md)

The company uses a dual-branch deployment model:
- **`master` branch**: Represents staging environment (auto-deploys on merge)
- **`production` branch**: Represents production environment (auto-deploys on merge)
- **Feature branches**: `ST-XXX/FEAT/<name>` - For feature development
- **Bugfix branches**: `ST-XXX/FIX/<name>` - For bug fixes
- **Deployment branches**:
  - `ST-XXX/DEPLOYMENT-<date>` - For full deployment flow
  - `ST-XXX/CHERRY-PICK/<date>` - For cherry-pick deployment flow

### Two Deployment Flows

1. **Full Deployment Flow**:
   - Branch from `master` → Develop → PR to `master` → QA on staging
   - Create deployment branch from `production` → Merge `master` into it
   - PR to `production` → Auto-deploy

2. **Cherry-Pick Deployment Flow**:
   - Branch from `master` → Develop → PR to `master` → QA on staging
   - Create cherry-pick branch from `production` → Cherry-pick from `master`
   - PR to `production` → Auto-deploy

### Pain Points Being Addressed

**Developer challenges:**
- Fast feature shipping required (customer retention)
- Quick bug/hotfix turnaround critical (credibility)
- Balancing code review with active development

**QA challenges:**
- Blocked by code review process
- Late involvement in development lifecycle

## OpenAgents Framework

This repository uses the OpenAgents framework for AI-assisted development:

### Main Agents
- **openagent**: Universal coordinator for general tasks (default)
- **opencoder**: Complex coding and architecture work
- **system-builder**: Custom AI system generation

### Common Commands
```bash
# Start universal agent
opencode --agent openagent

# Start coding specialist
opencode --agent opencoder

# Smart git commits
/commit

# Context management
/context

# Test workflows
/test

# Code optimization
/optimize

# Cleanup operations
/clean

# Validate repository
/validate-repo
```

## Development Patterns (from AGENTS.md)

### Technology Stack
- **Language**: TypeScript
- **Runtime**: Node.js / Bun
- **Package Manager**: pnpm (preferred)
- **Build**: TypeScript Compiler (tsc)
- **Testing**: Jest or Vitest
- **Linting**: ESLint

### Code Style Principles

**Core Philosophy**: Modular, Functional, Maintainable

**Naming Conventions**:
- Files: `lowercase-with-dashes.ts`
- Functions: `verbPhrase` (e.g., `getUser`)
- Predicates: `is`/`has`/`can` prefix
- Variables: Descriptive, `const` by default
- Constants: `UPPER_SNAKE_CASE`
- Types: PascalCase

**Code Quality Standards**:
- 2-space indentation
- Max 100 lines per file (ideally < 50)
- Max 50 lines per function
- Max 3 nesting levels (use early returns)
- Semicolons required
- Single quotes for strings
- Strict TypeScript (no `any`)

**Functional Programming**:
- Prefer pure functions
- Immutability by default
- Declarative over imperative
- Explicit dependency injection
- Result pattern for error handling

### Security Requirements
- Never hardcode credentials or secrets
- Use environment variables for sensitive data
- Sanitize all user input
- Use parameterized queries
- Never log passwords, tokens, or API keys

## Working on Git Workflow Research

When conducting research for this project:

1. **Input Location**: Read problem statements from `input/problem-statement.prompt.md`
2. **Output Location**: Write findings to `output/` directory (e.g., `output/git-workflow-rnd.md`)
3. **Output Format**: Markdown with tabular pros/cons comparisons
4. **Research Focus**:
   - Popular Git workflows used by successful SAAS enterprises
   - Specific challenges they faced
   - Why their chosen workflow worked for their case
   - Comparison with current workflow pain points

## Common Tasks

### Researching Git Workflows
```bash
# Agents will help research and document findings
opencode --agent openagent
> "Research popular Git workflows for SAAS companies and document findings in output/git-workflow-rnd.md"
```

### Creating Documentation
```bash
# Use documentation subagent for structured docs
opencode --agent openagent
> "Create a comparison document for Git workflows"
```

### Code Review
Agents follow these principles:
- TypeScript type safety validation
- Security vulnerability checking
- Code style compliance
- Functional programming patterns
- No over-engineering

## Git Workflow

When working in this repository:
- Use conventional commit messages (handled by `/commit` command)
- Feature branches follow: `ST-XXX/FEAT/<name>`
- Bug fix branches follow: `ST-XXX/FIX/<name>`
- Main branch is `main` (standard for template repos, differs from company's `master`)

## Notes for AI Agents

- This is a research repository, not production code
- Focus on documentation quality and comprehensive analysis
- Output should be clear, structured, and actionable
- When creating comparison tables, include specific examples
- Consider the company's constraints: 1:10 QA:Dev ratio, fast shipping needs
- Prioritize practical solutions over theoretical best practices
