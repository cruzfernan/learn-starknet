# Constructors

A contract may need to initialize its state before it is ready for public use.

## Commands

```
starknet-compile ownable.cairo \
    --output ownable_compiled.json \
    --abi ownable_abi.json
```

```
starknet declare --contract ownable_compiled.json
```

```
starknet deploy --contract ownable_compiled.json --inputs 123 --no_wallet
```
