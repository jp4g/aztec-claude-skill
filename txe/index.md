# TXE Unit Testing

TXE (Test Execution Environment) is Aztec's native test framework for Noir contracts. It simulates the Aztec network, allowing you to unit test private/public functions of a smart contract quickly and without all the overhead.

Remember to adhere to standard unit testing methodologies
 * Test expected cases and edge cases
 * When creating or changing state, check that expected values match including all contracts
 * Split up unit testing into files logically - i.e. authorization, groups of logic/ paths, etc

Be aware of cyclic dependencies - if you're building mulitple contracts in a single workspace, you might need to choose a main single contract downstream all others to unit test all logic.

## Subskills
Traverse according to needed functionality:
* [Setup](./setup.md) - Initializing TXE test structure and utilities (start here)
* [Writing Tests](./writing-tests.md) - Read when writing or modifying tests (test attributes, calling functions)
* [Test Running](./test-running.md) - Compiling and running tests
* [Authwit](./authwit.md) - Testing functions that require authwit
* [Deploy Contract with Keys](./deploy-contract-with-keys.md) - Deploying contracts with chosen keys