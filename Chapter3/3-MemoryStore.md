# MemoryStore in Roblox

MemoryStore is a fast, temporary storage system perfect for cross-server communication and temporary data storage.

## Basic Concepts

MemoryStore provides three main data structures:
1. Queue
2. SortedMap
3. Map

## Getting Started

```lua
local MemoryStoreService = game:GetService("MemoryStoreService")
```

## Maps

### Creating and Using Maps
```lua
local map = MemoryStoreService:GetMap("MapName")

-- Setting a value (with optional expiration time)
local success, err = pcall(function()
    map:SetAsync("key", "value", 3600) -- Expires in 1 hour
end)

-- Getting a value
local success, value = pcall(function()
    return map:GetAsync("key")
end)

-- Removing a value
local success, err = pcall(function()
    map:RemoveAsync("key")
end)

-- Updating a value
local success, newValue = pcall(function()
    return map:UpdateAsync("key", function(oldValue)
        if oldValue then
            return oldValue + 1
        end
        return 1
    end)
end)
```

## Sorted Maps

### Creating and Using Sorted Maps
```lua
local sortedMap = MemoryStoreService:GetSortedMap("HighScores")

-- Setting a value with score
local success, err = pcall(function()
    sortedMap:SetAsync("player1", 100, 3600) -- Score 100, expires in 1 hour
end)

-- Getting range of values
local success, data = pcall(function()
    return sortedMap:GetRangeAsync(Enum.SortDirection.Descending, 10) -- Top 10
end)

-- Removing expired entries
local success, err = pcall(function()
    sortedMap:RemoveAsync("player1")
end)
```

## Queues

### Creating and Using Queues
```lua
local queue = MemoryStoreService:GetQueue("TaskQueue")

-- Adding items to queue
local success, err = pcall(function()
    queue:AddAsync("task1", 300) -- Expires in 5 minutes
end)

-- Reading from queue
local success, items = pcall(function()
    return queue:ReadAsync(5, false, 30) -- Read 5 items, don't remove them, 30s timeout
end)

-- Removing items from queue
local success, items = pcall(function()
    return queue:RemoveAsync(5) -- Remove and return 5 items
end)
```

## Common Use Cases

### Cross-Server Communication
```lua
-- Server A: Sending message
local map = MemoryStoreService:GetMap("ServerMessages")
map:SetAsync("broadcast", {
    message = "Event starting!",
    timestamp = os.time()
}, 60) -- Message expires in 60 seconds

-- Server B: Receiving message
local map = MemoryStoreService:GetMap("ServerMessages")
local success, message = pcall(function()
    return map:GetAsync("broadcast")
end)
```

### Server Load Balancing
```lua
local sortedMap = MemoryStoreService:GetSortedMap("ServerLoads")

-- Update server load
local function updateServerLoad()
    local players = #game.Players:GetPlayers()
    
    pcall(function()
        sortedMap:SetAsync(game.JobId, players, 30)
    end)
end

-- Find least loaded server
local function findLeastLoadedServer()
    local success, servers = pcall(function()
        return sortedMap:GetRangeAsync(Enum.SortDirection.Ascending, 1)
    end)
    
    if success and #servers > 0 then
        return servers[1].key
    end
    return nil
end
```

### Task Queue System
```lua
local taskQueue = MemoryStoreService:GetQueue("Tasks")

-- Producer: Add tasks
local function addTask(taskData)
    pcall(function()
        taskQueue:AddAsync({
            type = taskData.type,
            data = taskData.data,
            created = os.time()
        }, 3600) -- Tasks expire in 1 hour
    end)
end

-- Consumer: Process tasks
local function processNextTask()
    local success, tasks = pcall(function()
        return taskQueue:RemoveAsync(1) -- Get and remove 1 task
    end)
    
    if success and #tasks > 0 then
        local task = tasks[1]
        -- Process the task
        print("Processing task:", task.type)
    end
end
```

## Best Practices

### 1. Error Handling
```lua
local function safeMemoryOperation(operation)
    local success, result = pcall(operation)
    if not success then
        warn("MemoryStore operation failed:", result)
        return nil
    end
    return result
end

-- Usage
local value = safeMemoryOperation(function()
    return map:GetAsync("key")
end)
```

### 2. Rate Limiting
```lua
local function waitForBudget(requestType)
    local timeToWait = MemoryStoreService:GetRequestBudgetForRequestType(requestType)
    if timeToWait > 0 then
        task.wait(timeToWait)
    end
end

-- Usage
waitForBudget(Enum.MemoryStoreBudgetRequestType.GetAsync)
local value = map:GetAsync("key")
```

### 3. Expiration Times
```lua
local function setWithAutoExpiry(map, key, value)
    local DEFAULT_EXPIRY = 3600 -- 1 hour
    local MAX_EXPIRY = 86400 -- 24 hours
    
    pcall(function()
        map:SetAsync(key, value, math.min(DEFAULT_EXPIRY, MAX_EXPIRY))
    end)
end
```

## Limitations and Considerations

1. Data Persistence
```lua
-- Remember: MemoryStore data is temporary
-- Always have a backup plan for critical data
local function saveData(key, value)
    -- Save to MemoryStore for quick access
    pcall(function()
        memoryMap:SetAsync(key, value, 3600)
    end)
    
    -- Backup to DataStore for persistence
    pcall(function()
        dataStore:SetAsync(key, value)
    end)
end
```

2. Size Limits
```lua
local function checkDataSize(data)
    -- MemoryStore has a 32KB limit per value
    local success, jsonData = pcall(function()
        return game:GetService("HttpService"):JSONEncode(data)
    end)
    
    return success and #jsonData <= 32 * 1024 -- 32KB
end
```

3. Request Budgeting
```lua
local function tryOperation(operation, maxAttempts)
    for attempt = 1, maxAttempts do
        local success, result = pcall(operation)
        if success then
            return result
        end
        task.wait(1) -- Wait before retry
    end
    return nil
end
```