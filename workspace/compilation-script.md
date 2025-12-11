# Pattern: Aztec Compilation Script

## When to use
* When making or modifying a compilation script
* When a new contract gitmodule is added

## Workflow
1. Create `ts/src/artifacts/{contract}/` dirs for each workspace member + `index.ts` exporting contracts:
```js
export { TokenContract, TokenContractArtifact } from "./token/Token";
export { BridgeContract, BridgeContractArtifact } from "./bridge/Bridge";
```

2. Create `scripts/utils.ts` with `execCommand` and `replaceInFile` helpers (see below)

3. Create `scripts/compile-contracts.ts` that:
   * `aztec-nargo compile --force`
   * `aztec-postprocess-contract`
   * `aztec codegen target --outdir target -f`
   * Copies `.json` + `.ts` per contract to artifacts dir
   * Fixes bytecode import paths in `.ts` bindings
   * Adds console logs indicating success at each step

4. Add to package.json: `"compile": "npx tsx scripts/compile-contracts.ts"`

## utils.ts
```js
import { spawn } from "child_process";
import { readFile, writeFile } from "fs/promises";

export async function execCommand(command: string, args: string[] = [], cwd?: string, env?: Record<string, string>): Promise<void> {
  return new Promise((resolve, reject) => {
    const proc = spawn(command, args, { cwd, stdio: "inherit", env: { ...process.env, ...env } });
    proc.on("close", (code) => code !== 0 ? reject(new Error(`${command} failed (${code})`)) : resolve());
    proc.on("error", (e) => reject(e));
  });
}

export async function replaceInFile(filePath: string, search: string, replace: string): Promise<void> {
  const content = await readFile(filePath, "utf-8");
  await writeFile(filePath, content.replace(new RegExp(search.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'), 'g'), replace));
}
```

## Example Script
```js
#!/usr/bin/env node
import { dirname, join } from "path";
import { copyFile } from "fs/promises";
import { fileURLToPath } from "url";
import { execCommand, replaceInFile } from "./utils.js";

const __dirname = dirname(fileURLToPath(import.meta.url));
const packageDir = join(__dirname, "..");
const artifactsDir = join(packageDir, "ts/src/artifacts");

async function main() {
  await execCommand("aztec-nargo", ["compile", "--force"]);
  await execCommand("aztec-postprocess-contract", [], packageDir);

  const aztecPath = process.env.HOME + "/.aztec/bin:" + process.env.PATH;
  await execCommand("aztec", ["codegen", "target", "--outdir", "target", "-f"], packageDir, { PATH: aztecPath });

  // Per contract: copy artifacts + fix imports (repeat for each)
  await copyFile("./target/Bridge.ts", join(artifactsDir, "bridge/Bridge.ts"));
  await copyFile("./target/bridge-Bridge.json", join(artifactsDir, "bridge/Bridge.json"));
  await replaceInFile(join(artifactsDir, "bridge/Bridge.ts"), "./bridge-Bridge.json", "./Bridge.json");
}

main().catch((e) => { console.error(e); process.exit(1); });
```

## Troubleshooting
* Wrong filepath in copy: check `./target` for actual filenames
* Wrong bytecode import: verify replaceInFile search string matches
* Contract build errors: out of scope
