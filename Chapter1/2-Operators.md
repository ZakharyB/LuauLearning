# Luau Operators

## Arithmetic Operators
| Operator | Description | Example | Result |
|----------|-------------|---------|---------|
| + | Addition | `5 + 3` | 8 |
| - | Subtraction | `10 - 4` | 6 |
| * | Multiplication | `4 * 3` | 12 |
| / | Division | `15 / 3` | 5 |
| % | Modulus | `10 % 3` | 1 |
| ^ | Exponentiation | `2 ^ 3` | 8 |
| - | Negation | `-5` | -5 |
## Comparison Operators
| Operator | Description | Example | Result |
|----------|-------------|---------|---------|
| == | Equal to | `5 == 5` | true |
| ~= | Not equal to | `5 ~= 3` | true |
| > | Greater than | `7 > 3` | true |
| < | Less than | `2 < 6` | true |
| >= | Greater than or equal | `5 >= 5` | true |
| <= | Less than or equal | `4 <= 4` | true |

## Logical Operators
| Operator | Description | Example | Result |
|----------|-------------|---------|---------|
| and | Logical AND | `true and false` | false |
| or | Logical OR | `true or false` | true |
| not | Logical NOT | `not true` | false |

## String Operators
| Operator | Description | Example | Result |
|----------|-------------|---------|---------|
| .. | Concatenation | `"Hello" .. " World"` | "Hello World" |
| # | Length | `#"Hello"` | 5 |

## Assignment Operators
| Operator | Description | Example | Equivalent |
|----------|-------------|---------|------------|
| = | Simple assignment | `x = 5` | `x = 5` |
| += | Add and assign | `x += 3` | `x = x + 3` |
| -= | Subtract and assign | `x -= 2` | `x = x - 2` |
| *= | Multiply and assign | `x *= 4` | `x = x * 4` |
| /= | Divide and assign | `x /= 2` | `x = x / 2` |
| %= | Modulus and assign | `x %= 3` | `x = x % 3` |
| ..= | Concatenate and assign | `str ..= "add"` | `str = str .. "add"` |

## Examples in Roblox Studio Context

### Arithmetic Examples
```lua
local damage = baseDamage + bonusDamage
local health = maxHealth - damageReceived
local totalCoins = coinsPerLevel * currentLevel
local averageDPS = totalDamage / timeElapsed
local remainingAmmo = currentAmmo % magazineSize
```
--- ---
### Comparison in Game Logic
```lua
if playerHealth <= 0 then
    -- Player defeated
end

if score >= highScore then
    -- New record!
end
```
--- ---
### Logical Operators in Conditions
```lua
if isAlive and hasWeapon then
    -- Can attack
end

if isInvulnerable or hasShield then
    -- Can't take damage
end
```
--- ---
### String Operations
```lua
local playerTag = playerName .. "#" .. playerID
local displayName = "[" .. teamName .. "] " .. playerName
```
--- ---
## Operator Precedence
1. ^
2. not, - (unary)
3. *, /, %
4. +, -
5. ..
6. <, >, <=, >=, ~=, ==
7. and
8. or
--- ---
## Best Practices
- Use parentheses to make operator precedence clear
- Break complex operations into smaller steps
- Be careful with floating-point arithmetic
--- ---
## Common Pitfalls
- Division by zero
- Integer overflow
- Floating-point precision errors
- String concatenation in tight loops
- Comparison of different types
--- ---