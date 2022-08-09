## Setting up the network

```
export STARKNET_NETWORK=alpha-goerli
```

## Chossing the wallet provider

Ethereum has different EOA and contracts, but in StarkNet, account is represented by a deployed contract.

```
export STARKNET_WALLET=starkware.starknet.wallets.open_zeppelin.OpenZeppelinAccount
```

## Creating an account

```
starknet deploy_account --wallet <wallet_provider> --account <account_name>

starknet deploy_account --account stk_test_account --network alpha-goerli --wallet starkware.starknet.wallets.open_zeppelin.OpenZeppelinAccount
```

## Transferring Goerli ETH to the account

- Use the [StarkNet Faucet](https://faucet.goerli.starknet.io/)
- Use the StarkGate - the StarkNet L2 bridge ([L1 contract](https://goerli.etherscan.io/address/0xc3511006C04EF1d78af4C8E0e74Ec18A6E64Ff9e) / (Web App)[https://goerli.starkgate.starknet.io/terms]) to transfer your existing Goerli L1 ETH to and from the L2 account.
