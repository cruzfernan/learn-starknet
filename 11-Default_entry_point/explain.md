## Use case

There are cases where the contract entry points are not known in advance. The most prominent example is a delegate proxy that forwards calls to an implementation contract class

The `__default__` entry point is executed if the requested selector does not match any of the entry point selectors in the contract.

- The `@raw_input` decorator instructs the compiler to pass the calldata as-is to the entry point, instead of parsing it into the requested arguments
- The `@raw_output` decorator instructs the compiler not to process the function’s return value.
