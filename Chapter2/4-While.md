# While Loops in Luau

While loops in Luau allow you to repeatedly execute code as long as a condition remains true. This is useful when you don't know exactly how many iterations you need, unlike for loops where you typically know the number of iterations in advance.

## Basic While Loop Syntax

```lua
while condition do
    -- code to execute
end
```

## Example 1: Basic Counter
```lua
local count = 0
while count < 5 do
    print(count)
    count = count + 1
end
```

## Example 2: Input Validation Pattern
```lua
local validInput = false
local userInput = "invalid"

while not validInput do
    if userInput == "valid" then
        validInput = true
        print("Valid input received!")
    else
        print("Please enter valid input")
        break -- In actual game, you would get new input here
    end
end
```

## Example 3: Countdown Timer
```lua
local timeLeft = 10

while timeLeft > 0 do
    print("Time remaining: " .. timeLeft)
    timeLeft = timeLeft - 1
    -- In a real game, you would wait here using task.wait(1)
end
print("Time's up!")
```

## Best Practices

1. Always ensure your while loop has a way to end
2. Be careful with infinite loops
3. Consider using a break statement when needed
4. Use task.wait() when appropriate to prevent freezing

## Break and Continue

You can use `break` to exit a while loop early:
```lua
local i = 0
while true do
    if i >= 5 then
        break
    end
    print(i)
    i = i + 1
end
```

Use `continue` to skip to the next iteration:
```lua
local i = 0
while i < 5 do
    i = i + 1
    if i == 3 then
        continue
    end
    print(i) -- Will print 1, 2, 4, 5
end
```

## Common Pitfalls to Avoid

1. Infinite Loops:
```lua
-- Bad: This will run forever
while true do
    print("This will never stop!")
end

-- Good: Always have a way to break the loop
local counter = 0
while true do
    if counter >= 10 then
        break
    end
    counter = counter + 1
end
```

2. Forgetting to Update the Condition:
```lua
-- Bad: Condition never changes
local x = 0
while x < 5 do
    print("This will run forever!")
end

-- Good: Condition updates
local x = 0
while x < 5 do
    print(x)
    x = x + 1
end
```