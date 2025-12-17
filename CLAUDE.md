# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a Claude Code skill repository containing documentation patterns for Aztec Protocol smart contract development. The markdown files serve as reference documentation for AI assistants working with Aztec projects.

## Repository Structure

- **workspace/**: Project setup and configuration patterns
  - `project-setup.md`: Initializing new Aztec contracts with workspace structure
  - `nargo-imports.md`: Managing Noir package dependencies
  - `compilation-script.md`: TypeScript compilation scripts for contract artifacts
  - `gitmodules-strategy.md`: Handling external contract dependencies via git submodules

- **contract-dev/**: Smart contract development patterns
  - `authwit.md`: Authentication witness pattern for delegated calls
  - `partial_notes.md`: Partial notes for private/public balance transfers

- **unit-testing/**: Testing patterns (in development)
  - `txe-setup.md`: Test execution environment setup
  - `cheatcodes.md`: Testing cheatcodes

## Key Aztec Concepts

### Project Structure
Aztec projects use a workspace layout:
```
project-root/
├── src/
│   └── contract_name/
│       ├── Nargo.toml
│       └── src/main.nr
├── Nargo.toml (workspace)
└── ts/ (TypeScript bindings)
```

### Essential Commands
- `aztec-nargo init --name {NAME} --contract`: Create new contract
- `aztec-nargo compile --force`: Compile contracts
- `aztec codegen target --outdir target -f`: Generate TypeScript bindings

### Dependencies
- External contracts (type=contract) use git submodules in `deps/` with symlinks
- Libraries (type=lib) use direct git references in Nargo.toml
- Never use gitmodule strategy for `aztec-packages` or library-type packages
