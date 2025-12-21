# Pattern: Events and Logs

## Types of Logs

### Public Logs (from public functions)
Unencrypted, visible to everyone. Like Ethereum events.

```rust
context.emit_public_log(value);
context.emit_public_log([1, 2, 3]);
context.emit_public_log("Hello");
```

### Private Events (from private functions)
Encrypted for a specific recipient. Only they can decrypt.

```rust
use aztec::{
    event::event_emission::emit_event_in_private,
    messages::message_delivery::MessageDelivery,
};

emit_event_in_private(
    MyEvent { from, to, amount },
    &mut context,
    recipient,  // who can decrypt
    MessageDelivery.UNCONSTRAINED_ONCHAIN,
);
```

## Defining Events

```rust
use aztec::macros::events::event;

#[event]
struct Transfer {
    from: AztecAddress,
    to: AztecAddress,
    amount: u128,
}
```

## MessageDelivery Options

Same options as note emission:
- **`CONSTRAINED_ONCHAIN`** - In proof, most secure, most expensive
- **`UNCONSTRAINED_ONCHAIN`** - On-chain but not in proof, cheaper
- **`UNCONSTRAINED_OFFCHAIN`** - Not on-chain, cheapest, recipient must be online

## When to Use What

| Scenario | Use |
|----------|-----|
| Public state changes | `emit_public_log` |
| Private transfers | `emit_event_in_private` with `CONSTRAINED_ONCHAIN` |
| Non-critical private notifications | `emit_event_in_private` with `UNCONSTRAINED_ONCHAIN` |

## Reference
`token_contract` - emits Transfer events for private transfers
