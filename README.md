# enum-as-inner

A deriving proc-macro for generating functions to automatically give access to the inner members of enum.

## Basic unnamed field case

The basic case is meant for single item enums, like:

```rust
#[macro_use]
extern crate enum_as_inner;

#[derive(EnumAsInner)]
enum OneEnum {
    One(u32),
}
```

where the inner item can be retrieved with the `as_*()` function:

```rust
let one = OneEnum::One(1);

assert_eq!(*one.as_one().unwrap(), 1);
```

the result is always a reference for inner items.

## Unit case

This will return copy's of the value of the unit variant, as `isize`:

```rust
#[macro_use]
extern crate enum_as_inner;

#[derive(EnumAsInner)]
enum UnitVariants {
    Zero = 0_isize,
    One,
    Two,
}
```

These are not references:

```rust
let unit = UnitVariants::Two;

assert_eq!(unit.as_two().unwrap(), 2_isize);
```

## Mutliple, unnamed field case

This will return a tuple of the inner types: 

```rust
#[macro_use]
extern crate enum_as_inner;

#[derive(EnumAsInner)]
enum ManyVariants {
    One(u32),
    Two(u32, i32),
    Three(bool, u32, i64),
}
```

And can be accessed like:

```rust
let many = ManyVariants::Three(true, 1, 2);

assert_eq!(many.as_three().unwrap(), (&true, &1_u32, &2_i64));
```

## Multiple, named field case

This will return a tuple of the inner types, like the unnamed option: 

```rust
#[macro_use]
extern crate enum_as_inner;

#[derive(EnumAsInner)]
enum ManyVariants {
    One{ one: u32 },
    Two{ one: u32, two: i32 },
    Three{ one: bool, two: u32, three: i64 },
}
```

And can be accessed like:

```rust
let many = ManyVariants::Three(true, 1, 2);

assert_eq!(many.as_three().unwrap(), (&true, &1_u32, &2_i64));
```