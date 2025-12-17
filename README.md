# Aztec Protocol Skill for Claude Code

A Claude Code skill providing patterns and guidance for Aztec Protocol smart contract development in Noir.

## What's Included

### Workspace Patterns
- **Project Setup**: Initialize Aztec contracts with proper workspace structure
- **Nargo Imports**: Manage Noir package dependencies (local and remote)
- **Compilation Scripts**: TypeScript scripts for generating contract artifacts
- **Git Submodules**: Handle external contract dependencies

### Contract Development Patterns
- **Auth Witness**: Delegated authorization for contract calls
- **Partial Notes**: Private/public balance transfers with refunds

### Unit Testing (In Development)
- **TXE Setup**: Test execution environment configuration
- **Cheatcodes**: Testing utilities

## Installation

### Option 1: Personal Installation

Clone the repository into your Claude Code skills directory:

```bash
git clone https://github.com/jp4g/aztec-claude-skill ~/.claude/skills/aztec
```

### Option 2: Project Installation

Add as a submodule to include with your project (shared with team via git):

```bash
git submodule add https://github.com/jp4g/aztec-claude-skill .claude/skills/aztec
```

Or clone directly:

```bash
git clone https://github.com/jp4g/aztec-claude-skill .claude/skills/aztec
```

### Verify Installation

Restart Claude Code, then ask:
```
What skills are available?
```

Claude should list the Aztec skill among available skills.

## Keeping Updated

This skill is updated as Aztec Protocol evolves. Pull the latest changes periodically:

```bash
cd ~/.claude/skills/aztec  # or .claude/skills/aztec
git pull
```

Restart Claude Code after updating.

## Usage

Once installed, Claude Code will automatically use these patterns when working on Aztec projects. You can also explicitly invoke the skill:

```
/skill aztec
```

Example prompts that benefit from this skill:
- "Create a new Aztec contract project"
- "Add a token contract dependency from aztec-standards"
- "Set up the compilation script for my contracts"
- "Implement auth witness for delegated transfers"

## Requirements

- [Claude Code](https://claude.ai/code) CLI installed
- [Aztec CLI tools](https://docs.aztec.network/) for contract development
