# RunService in Roblox

RunService is a core service that manages different game loops and execution contexts in Roblox.

## Understanding RunService Events

RunService provides several event connections for different execution contexts:

- `Heartbeat` - Runs every frame after physics
- `RenderStepped` - Runs every frame before rendering (only on client)
- `Stepped` - Runs every physics step
- `PreSimulation` - Runs before physics simulation
- `PostSimulation` - Runs after physics simulation

## Basic Usage

### Getting RunService
```lua
local RunService = game:GetService("RunService")
```

### Checking Context
```lua
-- Check if we're running on the client
if RunService:IsClient() then
    print("This is running on the client!")
end

-- Check if we're running on the server
if RunService:IsServer() then
    print("This is running on the server!")
end

-- Check if we're in Studio
if RunService:IsStudio() then
    print("This is running in Studio!")
end
```

## Event Types and Their Uses

### Heartbeat
```lua
-- Best for general game logic that needs to run every frame
RunService.Heartbeat:Connect(function(deltaTime)
    -- deltaTime is the time elapsed since the last frame
    local movement = speed * deltaTime
    object.Position = object.Position + movement
end)
```

### RenderStepped (Client-Only)
```lua
-- Best for smooth animations and camera updates
RunService.RenderStepped:Connect(function(deltaTime)
    -- Update camera position
    camera.CFrame = camera.CFrame * CFrame.new(0, deltaTime * speed, 0)
end)
```

### Stepped
```lua
-- Runs every physics step
RunService.Stepped:Connect(function(time, deltaTime)
    -- Physics-based updates
    updatePhysics(deltaTime)
end)
```

## Common Use Cases

### Smooth Character Movement
```lua
local function updateCharacterMovement(character, deltaTime)
    if not character then return end
    
    local humanoid = character:FindFirstChild("Humanoid")
    if not humanoid then return end
    
    -- Apply movement based on input
    local moveDirection = Vector3.new(0, 0, 0)
    if input.forward then
        moveDirection = moveDirection + Vector3.new(0, 0, -1)
    end
    -- ... other input checks
    
    humanoid:Move(moveDirection * deltaTime * walkSpeed)
end

RunService.Heartbeat:Connect(function(deltaTime)
    local player = game.Players.LocalPlayer
    if player.Character then
        updateCharacterMovement(player.Character, deltaTime)
    end
end)
```

### Game Loop
```lua
local gameState = {
    isRunning = false,
    score = 0,
    timeElapsed = 0
}

local function startGameLoop()
    gameState.isRunning = true
    
    local connection = RunService.Heartbeat:Connect(function(deltaTime)
        if not gameState.isRunning then
            connection:Disconnect()
            return
        end
        
        gameState.timeElapsed = gameState.timeElapsed + deltaTime
        updateGame(deltaTime)
    end)
end

local function stopGameLoop()
    gameState.isRunning = false
end
```

### Smooth Animations
```lua
local function interpolatePosition(object, target, alpha)
    object.Position = object.Position:Lerp(target, alpha)
end

RunService.RenderStepped:Connect(function(deltaTime)
    -- Smooth movement
    interpolatePosition(object, targetPosition, deltaTime * 5)
end)
```

## Best Practices

### 1. Cleaning Up Connections
```lua
local connections = {}

-- Add connection
local function addConnection(connection)
    table.insert(connections, connection)
end

-- Clean up all connections
local function cleanup()
    for _, connection in ipairs(connections) do
        connection:Disconnect()
    end
    table.clear(connections)
end

-- Usage
addConnection(RunService.Heartbeat:Connect(function(dt)
    -- Your code here
end))
```

### 2. Performance Optimization
```lua
local lastUpdate = 0
local UPDATE_INTERVAL = 0.1 -- Update every 0.1 seconds

RunService.Heartbeat:Connect(function(deltaTime)
    local now = time()
    if now - lastUpdate >= UPDATE_INTERVAL then
        lastUpdate = now
        -- Perform expensive operation here
        updateExpensiveOperation()
    end
end)
```

### 3. Context-Aware Code
```lua
if RunService:IsClient() then
    -- Client-specific initialization
    initializeClientComponents()
else
    -- Server-specific initialization
    initializeServerComponents()
end
```

## Common Pitfalls

### 1. Expensive Operations in RenderStepped
```lua
-- Bad: Heavy computation in RenderStepped
RunService.RenderStepped:Connect(function(dt)
    -- Don't do this! Will cause frame drops
    for i = 1, 1000000 do
        -- Heavy computation
    end
end)

-- Good: Use Heartbeat for heavy computation
RunService.Heartbeat:Connect(function(dt)
    -- Heavy computation here
end)
```

### 2. Not Handling Disconnections
```lua
-- Bad: Connection might never be cleaned up
RunService.Heartbeat:Connect(function(dt)
    -- Code here
end)

-- Good: Store and manage connection
local updateConnection
local function startUpdates()
    if updateConnection then
        updateConnection:Disconnect()
    end
    
    updateConnection = RunService.Heartbeat:Connect(function(dt)
        -- Code here
    end)
end

local function stopUpdates()
    if updateConnection then
        updateConnection:Disconnect()
        updateConnection = nil
    end
end
```

### 3. Wrong Event Choice
```lua
-- Bad: Using RenderStepped for physics
RunService.RenderStepped:Connect(function(dt)
    updatePhysics() -- Don't do physics here!
end)

-- Good: Use Stepped for physics
RunService.Stepped:Connect(function(time, dt)
    updatePhysics() -- This is the right place
end)
```

## Advanced Techniques

### Frame Rate Independent Movement
```lua
local function updatePosition(object, deltaTime)
    local UNITS_PER_SECOND = 10
    local movement = UNITS_PER_SECOND * deltaTime
    object.Position = object.Position + Vector3.new(movement, 0, 0)
end
```

### State Machine Updates
```lua
local gameStates = {
    idle = function(dt)
        -- Idle state logic
    end,
    running = function(dt)
        -- Running state logic
    end,
    paused = function(dt)
        -- Paused state logic
    end
}

local currentState = "idle"

RunService.Heartbeat:Connect(function(dt)
    local stateFunction = gameStates[currentState]
    if stateFunction then
        stateFunction(dt)
    end
end)
```