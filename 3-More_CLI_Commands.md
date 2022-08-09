## get_transaction

```
starknet get_transaction --hash TRANSACTION_HASH
```

## get_transaction_receipt

```
starknet get_transaction_receipt --hash TRANSACTION_HASH
```

## get_transaction_trace

```
starknet get_transaction_trace --hash TRANSACTION_HASH
```

## estimate_fee

```
starknet estimate_fee --address CONTRACT_ADDRESS --abi contract_abi.json --function increase_balance --inputs 1234
```

## get_code

```
starknet get_code --contract_address CONTRACT_ADDRESS
```

## get_full_contract

```
starknet get_full_contract --contract_address CONTRACT_ADDRESS
```

## get_class_hash_at

```
starknet get_class_hash_at --contract_address CONTRACT_ADDRESS
```

## get_block

```
starknet get_block --number BLOCK_NUMBER
```

## get_block_traces

```
starknet get_block_traces --number BLOCK_NUMBER
```

## get_state_update

```
starknet get_state_update --block_number BLOCK_NUMBER
```

## get_storage_at

StarkNet each storage variable is mapped to a storage key.

```
from starkware.starknet.public.abi import get_storage_var_address

balance_key = get_storage_var_address('balance')
print(f'Balance key: {balance_key}')
```

```
starknet get_storage_at --contract_address CONTRACT_ADDRESS --key ${balance_key}
```
