## Storage variable with multiple values

We can use multi values as pair in storage variables.

```
# A mapping from user to a pair (min, max).
@storage_var
func range(user : felt) -> (res: (felt, felt)):
end
```

You can read and write this value as follows:

```
@external
func extend_range{
    syscall_ptr : felt*,
    pedersen_ptr : HashBuiltin*,
    range_check_ptr,
}(user : felt):
    let (min_max) = range.read(user)
    range.write(user, (min_max[0] - 1, min_max[1] + 1))
    return()
end
```

Note that in this case the range.read() returns one item that is a pair. Thus, let (min, max) = range.read(user) will not work.

## Storage variable with struct arguments

Storage variable can be struct or tuple without pointers.

```
struct User:
    member first_name : felt
    member last_name : felt
end

@storage_var
func user_voted(user : User) -> (res : felt):
end

@external
func vote{
    syscall_ptr : felt*,
    pedersen_ptr : HashBuiltin*,
    range_check_ptr,
}(user : User):
    user_voted.write(user, 1)
    return()
end
```

## Array arguments in calldata¶

An external function may get an array of field elements as an argument. In order to define an array named `a`, pass two consecutive arguments: `a_len` of type `felt` and `a` of type `felt\*` (the first argument must be named `a_len` if the second argument is named a). For example:

```
@external
func compare_arrays(
    a_len : felt,
    a : felt*,
    b_len : felt,
    b : felt*
):
    assert a_len = b_len
    if a_len == 0:
        return ()
    end

    assert a[0] = b[0]
    return compare_arrays(
        a_len = a_len - 1 ,
        a = &a[1],
        b_len = b_len - 1,
        b = &b[1]
    )
end
```

```
@external
func compare_arrays(
    a_len : felt,
    a : felt*,
    b_len : felt,
    b : felt*
):
    assert a_len = b_len
    if a_len == 0:
        return ()
    end

    assert a[0] = b[0]
    return compare_arrays(
        a_len = a_len - 1 ,
        a = &a[1],
        b_len = b_len - 1,
        b = &b[1]
    )
end
```

## Passing tuples and structs in calldata

Calldata arguments and return values may be of any type that does not contain pointers. E.g., structs with felt members, tuples of felts and tuples of tuples of felts. For example:

```
struct Point:
    member x : felt
    member y : felt
end

@view
func sum_points(points : (Point, Point)) -> (res : Point):
    return (
        res=Point(
        x=points[0].x + points[1].x,
        y=points[0].y + points[1].y),
    )
end
```

```
starknet call \
    --address CONTRACT_ADDRESS \
    --abi contract_abi.json \
    --function sum_points \
    --inputs 1 2 10 20
```

## Passing arrays of structs¶

In a similar way, passing arrays of structs is supported, as long as the structs do not contain pointers:

```
@external
func sum_points_arr(a_len : felt, a : Point*) -> (res : Point):
    if a_len == 0:
        return (Point(0,0))
    end
    let (res) = sum_points_arr(
        a_len=a_len-1,
        a=&a[1]
    )
    return (res=Point(x = res.x + a[0].x, y = res.y + a[0].y))
end
```

```
starknet call \
    --address CONTRACT_ADDRESS \
    --abi contract_abi.json \
    --function sum_points_arr \
    --inputs 3 1 2 10 20 100 200
```

## Retrieving the transaction information

You can retrieve the transaction information (which includes, for example, the signature and the transaction fee), by using the `get_tx_info()` library function:

```
func get_tx_max_fee{syscall_ptr : felt*}() -> (max_fee : felt):
    let (tx_info) = get_tx_info()
    return (max_fee=tx_info.max_fee)
end
```

## Block number and timestamp

```
from starkware.starknet.common.syscalls import (
    get_block_number,
    get_block_timestamp,
)

# ...

let (block_number) = get_block_number()
let (block_timestamp) = get_block_timestamp()
```
