# Tables in Luau

Tables are the primary data structure in Luau. They can be used as arrays, dictionaries, objects, and more.

## Table Creation

```lua
-- Empty table
local myTable = {}

-- Table with initial values (array-like)
local fruits = {"Apple", "Banana", "Orange"}

-- Table with key-value pairs (dictionary-like)
local playerData = {
    name = "Player1",
    score = 100,
    isAlive = true
}
```

## Accessing Table Elements

```lua
-- Array-like access (indices start at 1 in Luau)
local firstFruit = fruits[1] -- "Apple"

-- Dictionary access
local score = playerData.score -- 100
-- Alternative syntax
local score2 = playerData["score"] -- 100
```

## Table Operations

### Adding Elements
```lua
-- To an array-like table
fruits[4] = "Mango"
table.insert(fruits, "Grape") -- Adds to the end

-- To a dictionary-like table
playerData.level = 5
playerData["coins"] = 1000
```

### Removing Elements
```lua
-- From an array-like table
table.remove(fruits, 2) -- Removes "Banana"
fruits[1] = nil -- Removes "Apple" but leaves a hole

-- From a dictionary-like table
playerData.coins = nil
```

### Common Table Functions

1. `table.insert(t, [pos,] value)`
```lua
local numbers = {1, 2, 4}
table.insert(numbers, 3, 3) -- Insert 3 at position 3
-- numbers is now {1, 2, 3, 4}
```

2. `table.remove(t, [pos])`
```lua
local numbers = {1, 2, 3, 4}
local removed = table.remove(numbers, 2) -- removes and returns 2
-- numbers is now {1, 3, 4}
```

3. `table.concat(t, [sep])`
```lua
local words = {"Hello", "World"}
local sentence = table.concat(words, " ") -- "Hello World"
```

4. `#` operator (length)
```lua
local fruits = {"Apple", "Banana", "Orange"}
local count = #fruits -- 3
```

## Table Iteration

### ipairs (for array-like tables)
```lua
local numbers = {10, 20, 30, 40}
for index, value in ipairs(numbers) do
    print(index, value)
end
```

### pairs (for all table elements)
```lua
local player = {
    name = "Player1",
    score = 100,
    level = 5
}

for key, value in pairs(player) do
    print(key, value)
end
```

## Nested Tables

```lua
local gameData = {
    players = {
        {
            name = "Player1",
            inventory = {
                "sword",
                "shield",
                potions = {
                    health = 5,
                    mana = 3
                }
            }
        },
        {
            name = "Player2",
            inventory = {
                "bow",
                "arrows",
                potions = {
                    health = 2,
                    mana = 4
                }
            }
        }
    },
    gameSettings = {
        difficulty = "hard",
        friendlyFire = false
    }
}

-- Accessing nested data
print(gameData.players[1].inventory.potions.health) -- 5
```

## Best Practices

1. Initialize tables with their expected structure:
```lua
local player = {
    stats = {
        health = 100,
        mana = 50
    },
    inventory = {},
    quests = {}
}
```

2. Use consistent access style:
```lua
-- Choose one style and stick to it
player.name = "Player1"  -- Dot notation
-- or
player["name"] = "Player1"  -- Bracket notation
```

3. Check if table exists before accessing nested elements:
```lua
if player and player.stats and player.stats.health then
    -- Safe to access player.stats.health
end
```

4. Use table.clear() when you want to empty a table but keep the reference:
```lua
local t = {1, 2, 3}
table.clear(t) -- t is now {}
```

## Common Pitfalls

1. Forgetting that indices start at 1:
```lua
local array = {"first"}
array[0] = "zero" -- Valid but breaks array functionality
```

2. Using # operator on tables with holes:
```lua
local array = {1, 2, nil, 4}
print(#array) -- Might not return 4
```

3. Modifying tables while iterating:
```lua
-- Incorrect:
for i, v in ipairs(t) do
    table.remove(t, i) -- This can skip elements
end

-- Correct:
for i = #t, 1, -1 do
    if shouldRemove(t[i]) then
        table.remove(t, i)
    end
end
```