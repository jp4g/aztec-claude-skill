# Pattern: Aztec Gitmodule Dependencies

## When to use
Importing a contract from GitHub that the Aztec contracts use. [CRITICAL] NEVER USE FOR aztec-packages or any Nargo.toml that is a library.

## Workflow

### 1. Create the deps folder
Create a `deps/` folder at the project root:
```bash
mkdir -p deps
```

### 2. Add the gitmodule
Add the repository as a git submodule inside the `deps/` folder:
```bash
git submodule add <repo-url> deps/<repo-name>
```

### 3. Checkout the correct version
Navigate to the submodule and checkout the version matching your Aztec Version:
```bash
cd deps/<repo-name>
git checkout <tag-or-commit>
cd ../..
```
If the tag is not found, ask the user for the version they want before continuing

### 4. Configure Nargo.toml
Add the dependency path to your `Nargo.toml`:
```toml
[dependencies]
some_contract = { path = "deps/<repo-name>/path/to/contract" }
```

### 5. Initialize submodules (for cloning)
When cloning a project with submodules:
```bash
git clone --recurse-submodules <your-repo>
```

Or if already cloned:
```bash
git submodule update --init --recursive
```

### 6. Symlinking

## Directory Structure
```
project-root/
├── deps/
│   └── <repo-name>/
│       └── contracts/
├── contracts/
│   └── your_contract/
│       ├── Nargo.toml
│       └── src/
├── Nargo.toml
└── .gitmodules
```
