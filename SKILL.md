---
name: aztec
description: Patterns for Aztec Protocol smart contract development in Noir. Use when creating Aztec projects, managing Nargo dependencies, writing compilation scripts, handling contract imports via git submodules, or implementing patterns like auth witness and partial notes.
---

# Aztec Protocol Development Skill

This skill provides patterns and guidance for Aztec Protocol smart contract development.

## Available Patterns

### Workspace Setup
- [Project Setup](workspace/project-setup.md) - Initialize new Aztec contract workspaces
- [Nargo Imports](workspace/nargo-imports.md) - Manage Noir package dependencies
- [Compilation Script](workspace/compilation-script.md) - Generate TypeScript contract artifacts
- [Git Submodules](workspace/gitmodules-strategy.md) - Handle external contract dependencies

### Contract Development
- [Auth Witness](contract-dev/authwit.md) - Delegated authorization pattern
- [Partial Notes](contract-dev/partial_notes.md) - Private/public balance transfers

### Unit Testing
- [TXE Setup](unit-testing/txe-setup.md) - Test execution environment
- [Cheatcodes](unit-testing/cheatcodes.md) - Testing utilities

## Quick Reference

### New Project
```bash
aztec-nargo init --name {NAME} --contract
```

### Compile Contracts
```bash
aztec-nargo compile --force
aztec codegen target --outdir target -f
```

### Key Dependencies
```toml
aztec = { git = "https://github.com/AztecProtocol/aztec-packages/", tag = "{version}", directory = "noir-projects/aztec-nr/aztec" }
```
