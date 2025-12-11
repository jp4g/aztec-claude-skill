# Pattern: Aztec Project Setup

## When to use
When initializing a new aztec project or when modifying an existing contract setup

### Project Creation Workflow
1. Use `aztec-nargo init --name {PROJECT_NAME} --contract` to create a new contract repository.
2. Initialize a typescript project inside pnpm inside this repository with empty index at `ts/index.ts`