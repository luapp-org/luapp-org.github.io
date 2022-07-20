## Semantics
Lua++ has a powerful and safe semantics (type-checking) engine that makes sure a developer doesn't accidentally screw up types somewhere in their program.

* [Primitive types](http://www.luaplusplus.org/semantics.html#primitive-types)
  * [Number](http://www.luaplusplus.org/semantics.html#number) 
  * [String](http://www.luaplusplus.org/semantics.html#string)
  * [Boolean](http://www.luaplusplus.org/semantics.html#boolean)
  * [Table](http://www.luaplusplus.org/semantics.html#table)
  * [Function](http://www.luaplusplus.org/semantics.html#function)

<br/>

### Primitive types
Lua++ has a completely different type system then Lua. As of now, Lua++ supports ```5``` primitive types: ```number```, ```string```, ```boolean```, ```function```, and ```table```. Each of these can be annotated in their own little way. Refer to the example below:
```lua
local a: number = 1
local b: string = "my string"
local c: boolean = true

local myTable: table<number, string> = { 
  { 1, "value" } 
}

local myFunc: (string, number) -> boolean = function(a: string, b: number) -> boolean
  return a == "hey" and b == 1
end
```

<br/>

#### Number
The ```number``` data type in Lua++ is equivalent to a 32 bit float or an integer. It has a range from -2,147,483,648 (2^31) to 2,147,483,647 (2^31 - 1). Optimizations in the Lua++ compiler may identify an integer as a smaller datatype, but as a developer, you won't have to worry about that.
```lua
local intMax: number = 2,147,483,647
local intMin: number = -2,147,483,648

print("MAX_INT: " .. intMax)
print("MIN_INT: " .. intMin)
```

#### String
The ```string``` data type in Lua++ remains unchanged from Lua 5.1, it's still an array of characters :).
```lua
local myString: string = "Lua++ is the best language ever."

print("A random fact: ", myString)
```

#### Boolean
The ```boolean``` data type in Lua++ also remains unchanged from Lua 5.1, it's still a one byte value (true or false).
```lua
local myBool: boolean = true

print("Lua++ is the best language: ", myBool)
```

#### Table
Tables in Lua++ are a lot different, since they served as a hacky replacement for OOP in Lua. In Lua++, tables function more like dictionaries or hashtables found in C#, C, or Java- a key value pair. The following example displays how a ```Table``` might be used in a Lua++ program.
```lua
-- KEY: Name, VALUE: Id
local users: Table<string, integer> = { 
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

#### Array
Arrays are still quite similar to basic arrays in Lua, but in Lua++ they serve as classes. They still have all the original functions but the syntax is a little different.
```lua
local list: Array<number> = { 1, 2, 3 }

-- You can easily iterate through them, there is no need to call ipairs/pairs
for n in list do
 print("List item: " + n)
end

```

**Output**
```
1
2
3
```
