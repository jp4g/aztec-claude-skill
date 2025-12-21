# Pattern: State Variables

## Mental Model
State variables in Aztec are either **public** (on-chain, readable by anyone) or **private** (stored as encrypted notes, only accessible to owners). This manifest explains all of your options, but does not explain HOW to use them. You need to use the context7 MCP to see how to actually implement them.

## Index
- `PublicMutable<T>` - Public, read/write. Counters, balances, flags.
- `PublicImmutable<T>` - Public, write-once. Config, token metadata, admin.
- `PrivateMutable<T>` - Private, single replaceable note. User settings.
- `PrivateImmutable<T>` - Private, write-once. Signing keys, secrets.
- `PrivateSet<T>` - Private, add/remove notes. Token balances, collections.
- `DelayedPublicMutable<T>` - Public writes, public/ private reads. Has special rules around access
- `Map<K, V>` - Wraps other types. Per-user storage, mappings.

## Storage Types:

### `PublicMutable<T>`
Standard mutable public storage:

### `PublicImmutable<T>`
Set once during initialization, readable forever in bototh private and public contexts. Good for configurations that will never change and need to be read potentially in private context.

### `PrivateMutable<T>`
Single private note that can be replaced. If you read PrivateMutable in private context and don't intend on replacing the note, you must re-emit the note if using constrained emission.

### `PrivateImmutable<T>`
Single private note, set once. Much like PublicImmutable except used when public data leakage is unacceptable.

### `PrivateSet<T>`
Collection of private notes. Consider like a wallet - if you own $30, you may have 5 $1 bills, 1 $5 bill, and 2 $10 bills. Your total balance is 30 by using multiple notes in your set (wallet).

### `Map<K, V>`
Key-value mapping, wraps other state var types:

### `DelayedPublicMutable<T, DELAY>`
Public state that private functions can read with stability guarantees. Changes are scheduled and take effect after DELAY seconds. If you want to use DelayedPublicMutable, check [delayed-public-mutable.md](./delayed-public-mutable.md) for details first.

## Note Emission
Private state changes require emitting notes with `MessageDelivery`:
- `CONSTRAINED_ONCHAIN` - Encryption verified in circuit, logged on-chain
- `UNCONSTRAINED_ONCHAIN` - Encryption not verified, logged on-chain (cheaper)