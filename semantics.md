## Semantics
Lua++ has a powerful and safe semantics (type-checking) engine, that makes sure that the developer doesn't accidentally screw up types somewhere in their program.

* [Primitive types](https://github.com/luapp-org/Documentation/blob/main/README.md#primitive-types)
  * [Integer](https://github.com/luapp-org/Documentation/blob/main/README.md#integer) 
  * [String](https://github.com/luapp-org/Documentation/blob/main/README.md#string)
  * [Boolean](https://github.com/luapp-org/Documentation/blob/main/README.md#boolean)
  * [Table](https://github.com/luapp-org/Documentation/blob/main/README.md#table)
  * [Function](https://github.com/luapp-org/Documentation/blob/main/README.md#function)

<br/>

### Primitive types
Lua++ has a completely different type system then Lua. As of now Lua++ supports ```5``` primitive types: ```integer```, ```string```, ```boolean```, ```function```, and ```table```. Each of these can be annotated in their own little way. Refer to the example below:
```lua
local a: integer = 1
local b: string = "my string"
local c: boolean = true

local myTable: table<integer, string> = { 
  { 1, "value" } 
}

local myFunc: (string, integer) -> boolean = function(a: string, b: integer) -> boolean
  return a == "hey" and b == 1
end
```

<br/>

#### Integer
The ```integer``` data type in Lua++ is equivalent to a 32 bit integer. It has a range from -2,147,483,648 (2^31) to 2,147,483,647 (2^31 - 1). Optimizations in the Lua++ compiler may identify and integer as a smaller datatype, but as a developer, you won't have to worry about that.
```lua
local intMax: integer = 2,147,483,647
local intMin: integer = -2,147,483,648

print("MAX_INT: " .. intMax)
print("MIN_INT: " .. intMin)
```

#### String
The ```string``` data type in Lua++ remains unchanged from Lua 5.1, it's still an array of characters :).
```lua
local myString: integer = "Lua++ is the best language ever."

print("A random fact:", intMin)
```

#### Boolean
The ```boolean``` data type in Lua++ also remains unchanged from Lua 5.1, it's still a one byte value (true or false).
```lua
local myBool: boolean = true

print("Lua++ is the best language:", myBool)
```

#### Table
Tables in Lua++ are a lot different, since they served as a hacky replacement for OOP in Lua. In Lua++ tables function more like dictionaries or hashtables in C#, C, or Java; a key value pair. The following example displays how a ```table``` may be used in a Lua++ program.
```lua
// KEY: Name, VALUE: Id
local users: table<string, integer> = { 
  { "John Doe", 17362 },
  { "Jane Doe", 99999 }
}

print("Name: John Doe ", "Id:", users["John Doe"])

users["Jane Doe"] = 0
print("Name: Jane Doe ", "Id:", users["Jane Doe"])

```

**Output**
```
Name: John Doe  Id: 17362
Name: Jane Doe  Id: 0
```
