## Syntax
Since Lua++ is an upgrade of lua, the majority of it's syntax comes from [Lua 5.1](https://www.lua.org/manual/5.1/). Please refer to it's manual for detailed documentation.

* [Compound assignments](http://www.luaplusplus.org/syntax.html#compound-assignments)
* [Postfix operators](http://www.luaplusplus.org/syntax.html#postfix-operators)
* [Continue statement](http://www.luaplusplus.org/syntax.html#continue-statement)
* [Constant variables](http://www.luaplusplus.org/syntax.html#continue-statement)
* [Type annotations](http://www.luaplusplus.org/syntax.html#type-annotations)
* [Classes](http://www.luaplusplus.org/syntax.html#classes)
* [Constructors](http://www.luaplusplus.org/syntax.html#constructors)
* [Events](http://www.luaplusplus.org/syntax.html#events)
* [Macros](http://www.luaplusplus.org/syntax.html#macros)

<br/>

### Compound assignments
Unlike Lua, Lua++ allows compound assignments with the following operators: ```+=```, ```-=```, ```*=```, ```/=```, ```%=```, ```^=```, and  ```..=```. Just like normal assignments, these are statements, not expressions. Here is an example with some of these operators in use:

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
> Postfix operators have been temporarily disabled due to their interference with standard lua comments. We have
> yet to find a good workaround

Lua++ has two postfix operators: the post-increment and post-decrement operators. These operators can be used as expressions or statements. Here is an example of the post-increment operator in use:

```lua
local x = 10
local a = x++   
```
The value of ```a``` will be 10 because the value of ```x``` is assigned to ```a``` before it is incremented.

And an example of the post-decrement operator in use:

```lua
local x = 10
local a = x--   
```
The value of ```a``` will be 10 because the value of ```x``` is assigned to ```a``` before it is decremented.

<br/>

### Continue statement
Similar to the ```break``` statement, Lua++ supports the ```continue``` statement. It can only be used within a loop and must be the last statement in a block:

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
If ```continue``` is used in a ```repeat ... until``` loop, it may not skip the local variable used in the loop condition. Code like this is invalid and will throw an error at compile time:

```lua
repeat
  continue
  local a = 1
until a > 0
```

<br/>

### Constant variables
The ```const``` keyword specifies that a variable's value is constant and tells the compiler to prevent the programmer from modifying it. Constant variables can only be referenced, not modified. Example:

```lua
const a = 12

print(a)
```
Constant functions can only consist of a ```return``` statement which would be guided by an arrow (```->```):
```lua
const function add(x, y): number ->
  return x + y
  
print(add(1, 2))
```

<br/>

### Type annotations
Types can be declared for local variables, constant variables, function arguments, and function return types using the ```:``` seperator. 

Lua++ only supports three primitive data types: ```boolean```, ```string```, and ```number``` for simplicity. Here is a simple example of type annotation:

```lua
function foo(a: number): boolean
  local b: number = a + 2
  return b == 3
end
```
To specify multiple return types for a function, seperate each type name with a ```,``` :

```lua
function foo(a: number): number, string
  return a * 2, tostring(a * 2)
end
```
To define a function as a type, wrap the function argument type(s) with ```()``` and return type(s) with ```:```. Example:

```lua
local func: (number, string): boolean

func = function(x: number, y: string): boolean
  return x == number(y)
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

| Keyword         | Description                                                   |
|:----------------|:--------------------------------------------------------------|
| *tag*           | The type name given to the class.                             |
| *template-spec* | Optional template specifications.                             |
| *base-list*     | Optional list of classes this class will derive members from. |
| *member-list*   | List of members.                                              |

Here is an example of class usage in Lua++ to calculate the area of a triangle:
```ts
class Triangle {
  b: number,
  h: number,
  
  constructor(b: number, h: number)
    self.b, self.h = b, h
  end,
  
  const function area(): number ->
    return (self.b * self.h) / 2.0
}

print(Triangle(3, 4).area())
```

Templating in Lua++ is quite simple compared to other languages. It works like so: ```tag<T>```. For multiple types, you would make a list of T's: ```tag<T1, T2, ...>```. A simple example of templating is shown below:
```ts
class Pair<T1, T2> {
  first: T1,
  second: T2,
  
  implicit constructor(f: T1, s: T2)
    self.first, self.second = f, s
  end
}

local pair: Pair<number, string> = { 3, "Three" }
```

<br/>

### Constructors
Similar to C++, Lua++ has two types of constructors, ```implicit``` and explicit constructors. An ```implicit``` constructor will be called whenever a variable with it's type is assigned to a value:
```ts
class Number {
  num: number,
  
  implicit constructor(num: number)
    self.num = num
  end
}

local number: Number = 1
```

A custructor is explicit by default, meaning you need to explicitly call a custructor when you are trying to create a new instance of a class.
```ts
class Number {
  num: number,
  
  constructor(num: number)
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

An ```event``` functions similarly to a function. If you wanted to call a function every time the ```SampleEvent``` is invoked, you would need to use the ```+=``` operator to assign the function to that event (multiple may be assigned). Here is an example:
```lua
function OnInvoked(sender, text: string)
  print("SampleEvent has been invoked!")
end

local publisher: Publisher = Publisher()

publisher.SampleEvent += OnInvoked
```

Anonymous functions can be used as well: 
```lua
local publisher: Publisher = Publisher()

publisher.SampleEvent += function(sender, text: string)
  print("SampleEvent has been invoked!")
end
```

<br/>

### Macros
Currently, there is only one macro planned for Lua++, the ```--!strict``` macro. This tells the compiler to enable strict type checking within the program. This means that lua compatibility will be extremely limited, since the compiler will begin throwing errors if you don't specify the type of a variable. Enabling this feature forces cleaner and more efficient code. An example is shown below:
```lua
local a: number = 12 -- no compiler error

local b = 12 -- compiler error
```
