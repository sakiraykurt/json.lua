# ![json.lua](https://cloud.githubusercontent.com/assets/3920290/9281532/99e5e0cc-42bd-11e5-8fce-eaff2f7fc681.png)
A lightweight JSON library for Lua

## Features
* Implemented in pure Lua: works with 5.1, 5.2, 5.3 and JIT
* Fast: generally outperforms other pure Lua JSON implementations
  ([benchmark scripts](bench/))
* Tiny: around 280sloc, 9kb
* Proper error messages, *eg:* `expected '}' or ',' at line 203 col 30`

## Usage
The [json.lua](json.lua) file should be dropped into an existing project
and required by it:
```lua
json = require "json"
```
The library provides the following functions:

#### json.encode(value)
Returns a string representing `value` encoded in JSON.
```lua
json.encode({
    firstName = "John",
    isAlive = true,
    age = 25,
    phoneNumbers = {
      nil,
      { type = "office", number = "646 555-4567" },
      nil
    },
    children = {},
    spouse = nil
  }) -- Returns {"firstName":"John","isAlive":true,"age":25,"children":null,"phoneNumbers":[null,{"number":"646 555-4567","type":"office"}]}
```

#### json.decode(str)
Returns a value representing the decoded JSON string.
```lua
json.decode('[1,2,3,{"x":10}]') -- Returns { 1, 2, 3, { x = 10 } }
```

#### json.set(empty_table_type)
Changes mapping empty `{}` tables to `[]`, `{}` or `null`. [`Default`](json.lua#L33) is `"null"`.
```lua
json.set("array")
json.encode({}) -- Returns []
json.set("object")
json.encode({}) -- Returns {}
json.set("null")
json.encode({}) -- Returns null
```

## Notes
* There is a Lua table library limitation for finding `nil` values.
  Unknown index of last `nil` values will not be added into array
  *eg:* `json.encode({nil,"x",nil,"y",nil}) --> [null,"x",null,"y"]`
* Trying to encode values which are unrepresentable in JSON will never result
  in type conversion or other magic: sparse arrays, tables with mixed key types
  or invalid numbers (NaN, -inf, inf) will raise an error
* `null` values contained within an array or object are converted to `nil` and
  are therefore lost upon decoding
* *Pretty* encoding is not supported, `json.encode()` only encodes to a compact
  format


## License
This library is free software; you can redistribute it and/or modify it under
the terms of the MIT license. See [LICENSE](LICENSE) for details.
