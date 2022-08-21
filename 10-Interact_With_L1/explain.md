# Messages from L2 to L1

1. The StarkNet(L2) contract calls the library function `send_message_to_l1()`

- The destination L1 contract `to`
- The data to send `payload`

The StarktNet OS adds the `from` address, which is the L2 address

2. Once state update containing the L2 tx is accepted on-chain, message is stored on the L1 StarkNet Core contract, waiting to be consumed.

3. The L1 contract specified by the `to` address invokes the `consumeMessageFromL2()` of the StarkNet core contract.

- Since any L2 contract can send message to any L1 contract, it's recommended that the L1 contract check the `from` address

# Messages from L1 to L2

1. The L1 contract calls the `send_message()` function of the StarkNet core contract.
   In this case, the message includes an additional field - the `selector` which determines what function to call in the corresponding L2 contract.

2. The StarkNet Sequencer automatically consumes the message and invokes the requested L2 function of the contract designated by the `to` address.

- `Important Note` : The StarkNet Alpha system is still under development, and therefore from time to time the state of the system will reset and all contracts will be removed. This means that you shouldn’t move valuable assets to the StarkNet system, unless you have a way to withdraw them given the removal of the L2 contract.
