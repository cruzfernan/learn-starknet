# Signature verification

Cairo allows one to verify ECDSA signatures over the STARK-friendly elliptic curve (for technical details see STARK Curve).

Note that in most cases the best way to handle authentication is by using account contracts, rather than verifying the signature directly in the contract

`verify_ecdsa_signature()` behaves like an assert – in case the signature is invalid, the function will revert the entire transaction.

Note that we don’t handle replay attacks here – once the user signs a transaction someone may call it multiple times. One way to prevent replay attacks is to add a nonce component to the signed message.

## Commands

### Compile and deploy¶
```
starknet-compile signature_verification.cairo \
    --output signature_compiled.json \
    --abi signature_abi.json

starknet deploy --contract signature_compiled.json --no_wallet
```