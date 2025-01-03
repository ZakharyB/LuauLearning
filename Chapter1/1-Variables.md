# Luau Variables And Types

## Variables Basics
In Luau, variables are containers that store data. They are declared using the `local` keyword:
```lua
local myVariable = 10
```
Variables can have global or local scope, but it's recommended to use local variables for better performance and memory management
--- ---
## Variable Declaration Best Practices

- Always use descriptive names that reflect the variable's purpose
- Prefer local variables over global ones
- Initialize variables when declaring them
- Use constants (uppercase) for fixed values.
--- --- 
## Data Types
Luau supports several built-in data types:
### NIL
Represents the absence of a value or undefined data:
```lua
local emptyVariable = nil
print(typeof(emptyVariable)) -- Outputs: "nil"
```
---
### Boolean
Represents logical values with two states:
```lua
local isPlayerAlive = true
local isGameOver = false
local canJump = not isGameOver -- Boolean operations (we will see the operators more in details later)
```
---
### Number
Represents both integer and floating-point numbers:
```lua
local playerHealth = 100      -- Integer
local gravity = 9.81          -- Float
local negativeValue = -42     -- Negative numbers
local scientific = 1.23e4     -- Scientific notation
```
---
### String
Text data with powerful manipulation features:
```lua
local playerName = "Alex"
local message = 'Hello!'      -- Single or double quotes
local multiline = [[
    This is a
    multiline string
]] -- Multiline strings are pretty useless in my opinion they do the same thing in the end.
local concatenated = "Hello " .. playerName -- String concatenation
```
---
### Table
Versatile data structure that can be used as arrays, dictionaries, or both:
```lua
local fruits = {"apple", "banana", "orange"}
local person = {name = "Alice", age = 25}
```
---
### Function
Stores a function as a value:
```lua
-- (we will see functions in more details later)
local greet = function(name)
    print("Hello, " .. name)
end
```
--- ---
## Type Annotations
Luau supports optional type annotations for variables:
```lua
local playerName: string = "RobloxPlayer123"
local playerScore: number = 100
```
--- ---
## Variable Scope

### Local Scope
Variables declared with local have limited scope:
```lua
local function exampleFunction()
    local localVar = 5
    print(localVar) -- Accessible
end

print(localVar) -- Not accessible, will print nil
```

```lua
local function gameLoop()
    local score = 0  -- Only exists within gameLoop
    
    local function updateScore()
        score = score + 1  -- Can access score from parent scope
    end
end
```
---
### Global Scope
Variables without the local keyword are global:
```lua
globalVar = 10 -- Global variable

local function anotherFunction()
    print(globalVar) -- Accessible
end
```
--- ---
## Constants
Constants are variables that cannot be reassigned(They must be in capitals letters):
```lua
local MAX_PLAYERS = 10
local DEFAULT_HEALTH = 100
local GRAVITY_CONSTANT = Vector3.new(0, -196.2, 0)
```
--- ---
## Variable Shadowing
Shadowing occurs when a local variable in an inner scope has the same name as a variable in an outer scope:
```lua
local outerVar = 10
local function shadowingExample()
    local outerVar = 20
    print(outerVar) -- Prints 20
```
--- ---
## Variable Naming nventions
### Camel Case
Start with
a lowercase letter and capitalize subsequent words:
```lua
local myVariable = 10
```
---
