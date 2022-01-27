## Syntax
Since Lua++ is an upgrade of lua, the majority of it's syntax comes from [Lua 5.1](https://www.lua.org/manual/5.1/). Please refer to it's manual for detailed documentation.

* [Compound assignments](https://github.com/luapp-org/Documentation/blob/main/README.md#compound-assignments)
* [Postfix operators](https://github.com/luapp-org/Documentation/blob/main/README.md#postfix-operators)
* [Continue statement](https://github.com/luapp-org/Documentation/blob/main/README.md#continue-statement)
* [Constant variables](https://github.com/luapp-org/Documentation/blob/main/README.md#continue-statement)
* [Type annotations](https://github.com/luapp-org/Documentation/blob/main/README.md#type-annotations)
* [Classes](https://github.com/luapp-org/Documentation/blob/main/README.md#classes)
* [Constructors](https://github.com/luapp-org/Documentation/blob/main/README.md#constructors)
* [Events](https://github.com/luapp-org/Documentation/blob/main/README.md#events)
* [Macros](https://github.com/luapp-org/Documentation/blob/main/README.md#macros)

<br/>

### Compound assignments
Unlike Lua, Lua++ allows compound assignments with the following operators: ```+=```, ```-=```, ```*=```, ```/=```, ```%=```, ```^=```, and  ```..=```. Just like normal assignmens these are statements, not expressions. Here is an example with some of these operators in use:

```lua
local a = 1
local str = "Random numbers:"

while a > 10 do
  local b = math.random(5) 
  a += b
  str ..= " " + b
 end
 
 print(str)
```

<br/>

### Postfix operators
> Postfix operators have been temporarily disabled due to them interfering with standard lua comments. I have
> yet to find a good workaround

Lua++ has two postfix operators, the post-increment and post-decrement operators. These operators can be used as expressions or statements. Here is an example of the post-increment operator in use:

```lua
local x = 10
local a = x++   
```
The value of ```a``` will be 10 because the value of ```x``` is assigned to ```a``` and then ```x``` is incremented.

And an example of the post-decrement operator in use:

```lua
local x = 10
local a = x--   
```
The value of ```a``` will be 10 because the value of ```x``` is assigned to ```a``` and then ```x``` is decremented.

<br/>

### Continue statement
Similar to the ```break``` statement, Lua++ supports the ```continue``` statement. It can only be used within a loop and must be the last statement in the block:

```lua
local a = 0

while a < 5 do
  if a == 4 then
    a--
    continue
   end
   a++
end
```
If ```continue``` is used in a ```repeat ... until``` loop, it may not skip the local variable used in the loop condition. Code like this is invalid and will throw and error at compile time:

```lua
repeat
  continue
  local a = 1
until a > 0
```

<br/>

### Constant variables
The ```const``` keyword specifies that a variable's value is constant and tells the compiler to prevent the programmer from modifying it. Constant variables can only be referenced and not modified. Example:

```lua
const a = 12

print(a)
```
Constant functions can only consist of a ```return``` statement which would be guided by an arrow (```->```):
```lua
const function add(x, y): integer ->
  return x + y
  
print(add(1, 2))
```

<br/>

### Type annotations
Types can be declared for local variables, constant variables, function arguments, and function return types by a ```:``` seperator. 

Lua++ only supports three primitive types, ```boolean```, ```string```, and ```integer``` for simplicity. Here is a simple example of type annotation:

```lua
function foo(a: integer): boolean
  local b: integer = a + 2
  return b == 3
end
```
To specify multiple return types for a function, seperate each type name with a ```,```:

```lua
function foo(a: integer): integer, string
  return a * 2, tostring(a * 2)
end
```
To define a function as a type, by wrapping the function argument types with ```()``` and return type with ```:```. Example:

```lua
local func: (integer, string): boolean

func = function(x: integer, y: string): boolean
  return x == tointeger(y)
end
```

<br/>

### Classes
Unlike Lua, Lua++ supports real object oriented programming with classes. A class in Lua++ is structured like so:

```ts
class [tag [template-spec] : [base-list]] {
  member-list
}
```
**Parameters**
| Keyword         | Description                                                   |
|-----------------|---------------------------------------------------------------|
| *tag*           | The type name given to the class.                             |
| *template-spec* | Optional template specifications.                             |
| *base-list*     | Optional list of classes this class will derive members from. |
| *member-list*   | List of members.                                              |

Here is an example of class usage in Lua++ to calculate the area of a triangle:
```ts
class Triangle {
  b: integer,
  h: integer,
  
  constructor(b: integer, h: integer)
    self.b, self.h = b, h
  end,
  
  const function area(): integer ->
    return (self.b * self.h) / 2.0
}

print(Triangle(3, 4).area())
```

Templating in Lua++ is quite simple compared to other languages. It works like so: ```tag<T>```. For multiple types, you would make a list of T's: ```tag<T1, T2, ...>```. A simple example of templating is attached below:
```ts
class Pair<T1, T2> {
  first: T1,
  second: T2,
  
  implicit constructor(f: T1, s: T2)
    self.first, self.second = f, s
  end
}

local pair: Pair<integer, string> = { 3, "Three" }
```

<br/>

### Constructors
Similar to C++, Lua++ has two types of constructors, ```implicit``` and explicit constructors. An ```implicit``` constructor will called whenever a variable with it's type is assigned to a value:
```ts
class Number {
  num: integer,
  
  implicit constructor(num: integer)
    self.num = num
  end
}

local number: Number = 1
```

A custructor is explicit at default, meaning you need to explicitly call the custructor when you are trying to create a new instance of a class.
```ts
class Number {
  num: integer,
  
  constructor(num: integer)
    self.num = num
  end
}

local number: Number = Number(100)
```

<br/>

### Events
The ```event``` keyword is used to declare an event in a publisher class.

The following example shows how to declare and raise an ```event```:
```ts
class Publisher {
  event SampleEvent: (sender, text: string),
  
  function RaiseSampleEvent()
    SampleEvent.Invoke(self, "text")
  end
}
```

An ```event``` functions similarly to a function. If you wanted to call a function every time the ```SampleEvent``` is invoked you would need to use the ```+=``` operator to assign a function to the event (multiple may be assigned). Here is an example:
```lua
function OnInvoked(sender, text: string)
  print("SampleEvent has been invoked!")
end

local publisher: Publisher = Publisher()

publisher.SampleEvent += OnInvoked
```

Anonymous functions can be used aswell: 
```lua
local publisher: Publisher = Publisher()

publisher.SampleEvent += function(sender, text: string)
  print("SampleEvent has been invoked!")
end
```

<br/>

### Macros
Currently, there is only one macro planned for Lua++, the ```--!strict``` macro which tells the compiler to enable strict type checking within the program. This means that lua compatibility will be extreamly limited, since the compiler will begin throwing errors if you don't specify the type of a variable. Enabling this feature forces cleaner and efficient code. An example is shown below:
```lua
local a: integer = 12 -- no compiler error

local b = 12 -- compiler error
```

<br/>

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

<br/>
