# Adding User Authentication

Suppose that instead of maintaining one global variable `balance`, we would like to have a balance for each user (users will be identified by their STARK public keys).

## Getting the caller address

```
from starkware.starknet.common.syscalls import get_caller_address

# ...

let (caller_address) = get_caller_address()
```

## Commands

### Commands to compile & deploy

```
starknet-compile user_auth.cairo \
    --output user_auth_compiled.json \
    --abi user_auth_abi.json
```

```
starknet deploy --contract user_auth_compiled.json --no_wallet
```

### Commands to interact with contract

```
starknet invoke \
    --address ${CONTRACT_ADDRESS} \
    --abi user_auth_abi.json \
    --function increase_balance \
    --inputs 4321
```

```
starknet call \
    --address ${CONTRACT_ADDRESS} \
    --abi user_auth_abi.json \
    --function get_balance \
    --inputs ${ACCOUNT_ADDRESS}

```

```
starknet tx_status \
    --hash TX_HASH \
    --contracts ${CONTRACT_ADDRESS}:user_auth_compiled.json \
    --error_message
```
