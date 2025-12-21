# Pattern: Contract With Custom Keys

## When to use
When you are using the secret escrow pattern and need to test it in the TXE


### Imports
You need `txe_oracles` to make this work:

```rust
use aztec::test::helpers::txe_oracles;
```

### Contract With Custom Keys
Instead of a normal contract deployment function as shown in [the setup](./setup.md) section, you will do the following (keeping in mind this is an example and initializer arguments will vary)
```rust
pub unconstrained fn deploy_escrow_contract(
    env: &mut TestEnvironment,
    owner: AztecAddress,
) -> (AztecAddress, Field) {
    // setup keys for escrow account
    let escrow_secret = unsafe { random() };
    let _ = txe_oracles::add_account(escrow_secret);
    // get deployment iface
    let initializer_call_interface = Escrow::interface().constructor(owner);
    let mut contract_instance = env.deploy("Escrow");
    // set the keys to the secret you added as an account
    contract_instance.secret = escrow_secret;
    // deploy
    let contract_address = contract_instance
        .with_private_initializer(owner, initializer_call_interface);
    // return
    (contract_address, escrow_secret)
}
```

Depending on your usecase, you may need to return and store the secret key for the contract, although because the TXE is unified across all accounts, all callers will know the decryption keys (meaning you don't need to re-add to test, and cannot test key leaking in the TXE).