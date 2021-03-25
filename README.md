# Typed Tuples proposal

This is follow-on proposal devired from [issue#218](https://github.com/tc39/proposal-record-tuple/issues/218) in [Records & Tuples proposal repository](https://github.com/tc39/proposal-record-tuple).

Proposal author is [Evgeniy Istomin](https://github.com/MadProbe) (@MadProbe).

## Stage

This proposal is stage 0 in the [process document](https://tc39.github.io/process-document/).

# Intro

This proposal adds new immutable primitive data structures - typed tuples.

They are similar to existing typed arrays and tuples by concept:

1. There are 11 types of typed tuples.
2. Same as tuples, typed tuples are immutable and primitive.
3. Have same syntax as tuples have.
4. Prototypes of object representations of typed tuples share a prototype - %TypedTuple%
5. %TypedTuple% has same methods as in Typle.prototype and some recent proposals like [relative indexing method](https://github.com/tc39/proposal-relative-indexing-method) and [array find from last](https://github.com/tc39/proposal-array-find-from-last)

# Syntax

Syntax of typed tuples extends syntax of tuples by adding tokens '<', %TypedTupleTypeNameOrTypeAlias% and '>' between tokens '#' and '['

### Examples

```js
var uint32_tuple = #<uint32_t>[0, 1337, 777, 322];
uint32_tuple[0]; // 0
uint32_tuple[3]; // 322
var float64_tuple = #<double>[666, ...uint32_tuple, 42]; // tuples of other types can be "rested" to tuple of another type
function* gen() {
    yield* [0, 1, 2, 3, 4];
}
console.log(#<uint8_t>[...gen()]); // Logs #<uint8_t>[0, 1, 2, 3, 4], any iterator can be used when "resting" into typed tuple
uint32_tuple[0] = 1; // TypeError: Cannot assign to read only property '0' of object '[object Uint32Tuple]'
```

### Syntax Errors
Like in tuples, holes are prevented in syntax.
```js
#<int>[,]; // SyntaxError, holes are disallowed by syntax
```

# Equality

```js
assert(#<int>[0, 1] === #<int>[0, 1]);
assert(#<int>[0, 1] !== #<uint>[0, 1]); // Typed tuples of different types are not equal even if they have same contents
assert(Object(#<int>[0, 1]) !== Object(#<int>[0, 1])); // Object wrappers are different even if they wrap same typed tuple
```

# Typed tuples table

| Constructor Name  | typeof tuple          | Syntax                          | Aliases                                            |
| ----------------- | --------------------- | ------------------------------- | -------------------------------------------------- |
| Int8Tuple         | "int8-tuple"          | `#<int8_t>[...values]`          | char, int8, signed_char                            |
| Uint8Tuple        | "uint8-tuple"         | `#<uint8_t>[...values]`         | byte, uint8, unsigned_char                         |
| Uint8ClampedTuple | "uint8-tuple-clamped" | `#<uint8_clamped_t>[...values]` | byte_clamped, uint8_clamped, unsigned_char_clamped |
| Int16Tuple        | "int16-tuple"         | `#<int16_t>[...values]`         | short, int16, char16, wchar_t, char16_t            |
| Uint16Tuple       | "uint16-tuple"        | `#<uint16_t>[...values]`        | ushort, uint16, uchar16, uchar16_t                 |
| Int32Tuple        | "int32-tuple"         | `#<int32_t>[...values]`         | int, int32, char32, char32_t, codepoint_t          |
| Uint32Tuple       | "uint32-tuple"        | `#<uint32_t>[...values]`        | uint, uint32, uchar32, uchar32_t                   |
| Float32Tuple      | "float32-tuple"       | `#<float32_t>[...values]`       | float, float32                                     |
| Float64Tuple      | "float64-tuple"       | `#<float64_t>[...values]`       | double, float64                                    |
| BigInt64Tuple     | "bigint64-tuple"      | `#<bigint64_t>[...values]`      | int64, bigint64, int64_t                           |
| BigUint64Tuple    | "biguint64-tuple"     | `#<biguint64_t>[...values]`     | uint64, biguint64, uint64_t                        |

<!-- ## Before creating a proposal

Please ensure the following:

  1. You have read the [process document](https://tc39.github.io/process-document/)
  1. You have reviewed the [existing proposals](https://github.com/tc39/proposals/)
  1. You are aware that your proposal requires being a member of TC39, or locating a TC39 delegate to "champion" your proposal

## Maintain your proposal repo

  1. Make your changes to `spec.emu` (ecmarkup uses HTML syntax, but is not HTML, so I strongly suggest not naming it ".html")
  1. Any commit that makes meaningful changes to the spec, should run `npm run build` and commit the resulting output.
  1. Whenever you update `ecmarkup`, run `npm run build` and commit any changes that come from that dependency.

  [explainer]: https://github.com/tc39/how-we-work/blob/HEAD/explainer.md -->
