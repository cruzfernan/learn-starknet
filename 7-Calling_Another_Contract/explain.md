# Calling another contract

In order to call this contract from another contract, define an interface by copying the declarations of the external functions:

```
@contract_interface
namespace IBalanceContract:
    func increase_balance(amount : felt):
    end

    func get_balance() -> (res : felt):
    end
end
```

Body of the functions and the implicit arguments should be removed from the definitions. This requires passing one argument before the original argumetns - the address of the called contract.

```
@external
func call_increase_balance{syscall_ptr : felt*, range_check_ptr}(
    contract_address : felt, amount : felt
):
    IBalanceContract.increase_balance(
        contract_address=contract_address, amount=amount
    )
    return ()
end

@view
func call_get_balance{syscall_ptr : felt*, range_check_ptr}(
    contract_address : felt
) -> (res : felt):
    let (res) = IBalanceContract.get_balance(
        contract_address=contract_address
    )
    return (res=res)
end
```

## Interact with deployed contract

```
starknet invoke \
    --address PROXY_CONTRACT \
    --abi proxy_contract_abi.json \
    --function call_increase_balance \
    --inputs BALANCE_CONTRACT 10000
```
