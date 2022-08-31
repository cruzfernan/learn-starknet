# Deploying a contract by another contract

A contract may use the `deploy` system call in order to deploy another contract.
Here the contract class of the deployed contract must be declared before tx invoking the system call.

```
func deploy{syscall_ptr : felt*}(
    class_hash : felt,
    contract_address_salt : felt,
    constructor_calldata_size : felt,
    constructor_calldata : felt*,
    deploy_from_zero : felt,
) -> (contract_address : felt):
end
```

- `class_hash`: The class hash of the contract to deploy
- `contract_address_salt`: An arbitrary value used to determine the address of the new contract. Using the same salt, the same class_hash and the same constructor arguments from the same contract will result in the same address, which means that the second deploy will fail.
- `constructor_calldata_size`: The size of the constructor’s arguments (calldata). Note that this may differ from the number of arguments in the cases where not all the arguments are felts.
- `constructor_calldata`: A pointer to an array containing the arguments for the constructor.
- `deploy_from_zero`: A flag determining whether the deployer’s address will affect the contract address or not. When equal to TRUE, the contract address computation will use 0 instead of the deployer address. When equal to FALSE, the actual deployer’s address will be used.

## Commands

```
starknet-compile ownable_contract_deployer.cairo \
    --output ownable_contract_deployer_compiled.json \
    --abi ownable_contract_deployer_abi.json
```

```
starknet deploy --contract ownable_contract_deployer_compiled.json \
    --inputs  ${OWNABLE_CLASS_HASH} --no_wallet

```

```
starknet invoke \
    --address ${CONTRACT_ADDRESS} \
    --abi ownable_contract_deployer_abi.json \
    --function deploy_ownable_contract \
    --inputs OWNER_ADDRESS
```

```
starknet call \
    --address OWNABLE_CONTRACT_ADDRESS \
    --abi ownable_abi.json \
    --function get_owner

```
