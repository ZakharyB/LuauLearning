# If Statements

## Basic If Statement
The simplest form using boolean conditions:
```lua
local playerHealth = 100

if playerHealth > 0 then
    print("Player is alive!")
end
```
--- ---
## If-Else Statement
Making decisions between two options:
```lua
local playerScore = 85

if playerScore >= 70 then
    print("You passed!")
else
    print("Try again!")
end
```
--- ---
## If-Elseif-Else Statement
Handling multiple conditions:
```lua
local rank = "Gold"

if rank == "Diamond" then
    print("Top tier player!")
elseif rank == "Gold" then
    print("Advanced player!")
elseif rank == "Silver" then
    print("Intermediate player!")
else
    print("Beginner player!")
end
```
--- ---
## Nested If Statements
Combining conditions:
```lua
local isAlive = true
local hasWeapon = true
local hasAmmo = false

if isAlive then
    if hasWeapon then
        if hasAmmo then
            print("Ready to fight!")
        else
            print("Need ammo!")
        end
    else
        print("Need weapon!")
    end
end
```
--- ---
## Ternary Operator
A shorthand for if-else statements:
```lua
local isGameOver = true
local gameOverMessage = isGameOver and "Game Over" or "Game is still running"
```
--- ---
## Using Logical Operators
Combining multiple conditions:

### And Operator
```lua
local health = 100
local shield = 50

if health > 0 and shield > 0 then
    print("Full protection active!")
end
```
---
### Or Operator
```lua
local hasKey = false
local isAdmin = true

if hasKey or isAdmin then
    print("Access granted!")
end
```
---
### Not Operator
```lua
local isGameOver = false

if not isGameOver then
    print("Game is still running!")
end
```
--- ---
## Examples In More Complex Scenario
### Player Damage System
```lua
local playerHealth = 75
local hasShield = true
local damageAmount = 20

if hasShield then
    if playerHealth > damageAmount then
        playerHealth = playerHealth - (damageAmount / 2)
        print("Damage reduced by shield!")
    else
        print("Player defeated despite shield!")
    end
else
    if playerHealth > damageAmount then
        playerHealth = playerHealth - damageAmount
        print("Full damage taken!")
    else
        print("Player defeated!")
    end
end
```
---
### Scoring System
```lua
local score = 95

if score >= 90 then
    print("S Rank!")
elseif score >= 80 then
    print("A Rank!")
elseif score >= 70 then
    print("B Rank!")
elseif score >= 60 then
    print("C Rank!")
else
    print("Try Again!")
end
```
--- ---
## Guard Clauses

```lua
-- (We Will See Functions Later but Guard Clause Need A Function To Work(well it depends but for now lets say it does))
local function processPlayerData(data)
    if not data then
        print("No data provided")
        return
    end
    
    if not data.Name then
        print("No player name")
        return
    end
    
    -- Process the data
    print("Processing data for: " .. data.Name)
end
```
## Best Practices
- Keep conditions simple and readable
- Consider the order of elseif statements
- Avoid deeply nested if statements

--- ---
