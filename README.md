# Orbital Royalty Child

A factory contract used by the Alkane Pandas collection contract for creating individual Orbital Royalty NFTs.

## Building

```bash
cargo build --target wasm32-unknown-unknown --release
```

The compiled WASM binary will be available in `target/wasm32-unknown-unknown/release/alkane_pandas_child.wasm`. 

## Deployment

```bash
oyl alkane new-contract -c ./target/alkanes/wasm32-unknown-unknown/release/alkane_pandas_child.wasm -data 3,888 -p oylnet
```

## Tracing

```bash
oyl provider alkanes --method trace -params '{"txid":"88a68a2fcef7139232d858b49ff39f5e50da79a308616ff84a80adf344ea4341", "vout":3}' -p oylnet
``` 
