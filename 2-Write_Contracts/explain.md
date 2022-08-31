# Writing StarkNet contracts

## Important knowledges about the contract

- should compile with `starknet-compile`, `not cairo-compile`
- No need to explicilty use the `%builtins` directive in the contracts.
- `@storage_var` declares a variable which will be kept as part of contract storage of state.
  To use `balance` storage, we will use `balance.read()` and `balance.write()` functions
- No `main()` function, instead `external` funtions can be called by other contracts or accounts.
- `pedersen_ptr` allows to compute the Pedersen hash function.
- `range_check_ptr` allows to compare integers.
- `syscall_ptr` allows the code to invoke system calls, unique to StarkNet accounts and not exist in the contracts.
- Using `hints` in StarkNet is not possible, but still used by the standard library functions.

## Compilete the contract

- env
  In order to instruct the CLI to work with the StarkNet testnet you should either pass --network=alpha-goerli on every use, or set the STARKNET_NETWORK environment variable as follows:

```
export STARKNET_NETWORK=alpha-goerli
```

- compile commnad

```
starknet-compile contract.cairo --output contract_compiled.json --abi contract_abi.json
```

## Declare the contract on the StarkNet testnet

```
starknet declare --contract contract_compiled.json
```

## Deploy the contract on the StarkNet testnet

- deploy command

```
starknet deploy --contract contract_compiled.json --no_wallet
```

- set the environment variable
  export CONTRACT_ADDRESS="<address of the previous contract>"

## Interact with the contract¶

- Run below command to invoke the increase_balance()

```
starknet invoke --address ${CONTRACT_ADDRESS} --abi contract_abi.json --function increase_balance --inputs 1234
```

- The following command allows you to query the transaction status based on the transaction hash

```
starknet tx_status --hash TRANSACTION_HASH
```

## Query the balance

```
starknet call --address ${CONTRACT_ADDRESS} --abi contract_abi.json --function get_balance
```
