# Luau Functions 

## 1. Function Basics

### 1.1 Simple Function Declaration
A basic function is declared using the `local` keyword and the `function` keyword. Here's an example of a simple function that prints a greeting message:

```lua
local function sayHello()
    print("Hello, player!")
end

-- Calling the function
sayHello()
```
In the above example:

- The function `sayHello` has no parameters.
It simply prints a greeting message when called.
---
### 1.2 Function with Parameters
You can define functions that accept parameters (inputs). These parameters allow you to customize the behavior of the function.
```lua
local function greetPlayer(playerName: string, level: number)
    print("Welcome " .. playerName .. " (Level " .. level .. ")")
end

greetPlayer("Alex", 42)
```
In this example:

- The function `greetPlayer` accepts two parameters: `playerName` (a string) and `level` (a number).
It prints a personalized greeting message.

--- ---
## 2 Return Values
### 2.1 Single Return
Functions can return values to be used elsewhere in your code. Here's an example of a function that returns a single value:
```lua
local function calculateDamage(baseDamage: number, multiplier: number): number
    return baseDamage * multiplier
end

local damage = calculateDamage(10, 1.5) -- Returns 15
```
In this case:
- `calculateDamage` takes two parameters (`baseDamage` and `multiplier`) and returns the product of those two values.
---
### 2.2 Multiple Returns
Lua allows functions to return multiple values. You can capture each returned value by assigning them to different variables.
```lua
local function getPlayerStats(): (number, number, string)
    local health = 100
    local armor = 50
    local status = "Ready"
    return health, armor, status
end

local health, armor, status = getPlayerStats()
```
Here:

- The function `getPlayerStats` returns three values: `health`, `armor`, and `status`.
These values are then captured in variables `health`, `armor`, and `status`.

--- ---
## 3 Function Types
### 3.1 Anonymous Functions
Anonymous functions are functions that are defined without a name. These are often used when functions are passed as arguments or used temporarily.
```lua
local onPlayerDeath = function()
    print("Player defeated!")
end

-- Using in events (we'll see later in details)
game.Players.PlayerAdded:Connect(function(player)
    print(player.Name .. " joined!")
end)
```
In this case:

- The `onPlayerDeath` function is an anonymous function assigned to a variable.
The second example shows the use of an anonymous function as an event handler when a player joins the game.
---
### 3.2 Higher-Order Functions
Higher-order functions are functions that can accept other functions as arguments or return functions.
```lua
local function createMultiplier(factor: number)
    return function(x: number): number
        return x * factor
    end
end

local double = createMultiplier(2)
local result = double(5) -- Returns 10
```
Here:

- `createMultiplier` is a higher-order function that returns a new function (`double`) which multiplies a number by the given `factor`.
--- ---
## 4 Advanced Function Concepts
### 4.1 Default Parameters
>*YOU DO NOT NEED TO UNDERSTAND THIS YET BUT GOOD IF YOU DO*

Default parameters allow you to specify default values for function arguments. If a caller doesn't provide a value for an optional parameter, the default value is used instead. This makes functions more flexible and easier to use.

```lua
local function createPlayer(name: string, health: number?, speed: number?)
    health = health or 100
    speed = speed or 16
    return {
        name = name,
        health = health,
        speed = speed
    }
end
```
If `health` or `speed` are not provided when calling the function, they default to 100 and 16 respectively.

---
### 4.2 Variable Arguments
Variable arguments (varargs) allow a function to accept any number of arguments. This is useful when you don't know in advance how many arguments will be passed to a function.
In Lua, you use ... to denote varargs. The {...} syntax captures all passed arguments into a table.
```lua
local function sumAll(...)
    local args = {...}
    local total = 0
    for _, value in ipairs(args) do
        total = total + value
    end
    return total
end

local result = sumAll(1, 2, 3, 4, 5) -- Returns 15
```
`sumAll` can accept any number of arguments, which are then processed in a loop.

---
### 4.3 Function Closures
A closure is a function that captures and remembers the environment in which it was created. This allows the function to access and manipulate variables from its outer scope, even after that scope has finished executing.
```lua
local function createCounter()
    local count = 0
    return function()
        count = count + 1
        return count
    end
end

local counter = createCounter()
print(counter()) -- 1
print(counter()) -- 2
```
The inner function forms a closure, remembering and modifying the `count` variable from its enclosing scope. Each call to the returned function increments and returns the `count`.
--- ---