# Home
Lua++ is a revolutionary, fast, safe, type-based programming language based on Lua 5.1. We loved Lua syntax but hated the fact that it was slow and did not have real [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming). Lua++ inherits all of the great features of Lua and patches all of its inconsistencies.

Lua++ is also syntactically backward-compatible with Lua 5.1, meaning you can write any Lua code you want and it will compile. You may get a couple of warnings from the linter since the compiler is designed to produce efficient and fast programs. But really, who needs to write Lua code when you can write beautiful Lua++ code like this: 
```ts
class Triangle {
  base: integer,
  height: integer,
  
  constructor(base: integer, height: integer)
    self.base, self.height = base, height
  end,
  
  const function area(): integer ->
    return (self.base * self.height) / 2.0
}

print(Triangle(3, 4).area())
```

If you are interested in this project and would like to take a deeper look at it, feel free to navigate through our web pages (located on the left of your screen). You will be presented with the syntax of Lua++ and much more. If you are a real curious critter and want to look at the source code of our project, you can click _View the Project on GitHub_. You will be sent to the main repository of the Lua++ project.

<br/>

## Performance 
Lua++ drastically outperforms any version of Lua. With various compiler optimizations and a custom bytecode format, it's almost impossible to go wrong with Lua++. We have implemented a strict compile-time type checking system that provides the user with a safe development environment. Not to mention, Lua++ also has a multitude of linter passes that analyze your codebase and notify you of any inconsistencies.

<br/>

## Future 
The future of Lua++ is still unknown. We still have a lot of work left to do on this project until we can make an official release. As of January 24, 2022 we have started rewriting our codebase from C++ to C for portability and convenience. If you want to recieve updates on our project feel free to follow our twitter account [@LuaPlusPlus](https://twitter.com/LuaPlusPlus). Anyways, thank you for showing interest in Lua++ and we wish you the best! Happy coding!
