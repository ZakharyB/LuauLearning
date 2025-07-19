# For Loops

Loops allow you to repeat a block of code multiple times — perfect for automation, iteration, or scanning collections like arrays or game objects.

---

## Numeric `for` Loop
Used when you want to **count numbers in a range**.

### Basic Form:
```lua
for i = 1, 5 do
    print("Count:", i)
end
```
This prints:
```
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
```

---

### With a Step (Increment/Decrement)
```lua
for i = 10, 0, -2 do
    print(i)
end
```
This counts **down from 10 to 0**, subtracting 2 each time.

Output:
```
10
8
6
4
2
0
```

---

## Generic `for` Loop
Used for looping over **collections or tables**, like arrays or dictionaries.

### `ipairs` – Ordered, index-based (1, 2, 3…)
```lua
local fruits = {"apple", "banana", "cherry"}

for index, fruit in ipairs(fruits) do
    print(index, fruit)
end
```

Use `ipairs` when:
- You have an **array-like** table.
- You want to **preserve order**.
- You **stop at the first nil** (e.g., gaps end the loop).

---

### `pairs` – Key-value, unordered
```lua
local playerStats = {
    Health = 100,
    Stamina = 75,
    Mana = 50
}

for key, value in pairs(playerStats) do
    print(key .. ": " .. value)
end
```

🔍 Use `pairs` when:
- Your table is a **dictionary or map**.
- Keys are **non-numeric**, or order doesn't matter.
- You want to iterate **all entries**, even with gaps.

---

## Nested Loops
Great for 2D grids or scanning complex structures.

```lua
for x = 1, 3 do
    for y = 1, 2 do
        print("Position:", x, y)
    end
end
```

Output:
```
Position: 1 1
Position: 1 2
Position: 2 1
Position: 2 2
Position: 3 1
Position: 3 2
```

---

## The `break` Statement

`break` exits a loop **immediately** when a condition is met.

```lua
for i = 1, 10 do
    if i == 5 then
        break
    end
    print(i)
end
```

Output:
```
1
2
3
4
```

---

## Luau-only Methods for Object Iteration

These are commonly used in Roblox Studio to loop over objects in the game.

### `:GetChildren()`
Returns **direct children** of an object.
```lua
for _, child in pairs(workspace:GetChildren()) do
    print(child.Name)
end
```

### `:GetDescendants()`
Returns **all nested children** (recursive).
```lua
for _, obj in pairs(workspace:GetDescendants()) do
    if obj:IsA("Part") then
        obj.Transparency = 0.5
    end
end
```

Use `GetDescendants` when:
- You need to affect **every object inside a hierarchy** (e.g., models inside folders).

---

## Best Practices

- **Don’t modify** the loop counter (`i = i + 1`) inside a `for` loop.
- Use `break` to **exit early** when a condition is met.
- For object scanning (`GetChildren`), prefer `ipairs` if order matters.
- Keep loop bodies **concise** and move logic to functions if too long.

---
