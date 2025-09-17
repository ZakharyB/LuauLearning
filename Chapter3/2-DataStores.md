# DataStores in Roblox

DataStores are persistent storage systems in Roblox that allow you to save data between game sessions.

## Getting Started

First, enable DataStores in Game Settings:
1. Go to Game Settings
2. Under Security, enable "Allow Studio Access to API Services"
3. Under Security, enable "Enable Studio Access to DataStore Service"

## Basic Usage

### Getting a DataStore
```lua
local DataStoreService = game:GetService("DataStoreService")
local myDataStore = DataStoreService:GetDataStore("MyDataStore")
```

### Saving Data (Set)
```lua
local success, error = pcall(function()
    myDataStore:SetAsync("key", {
        coins = 100,
        level = 5,
        items = {"sword", "shield"}
    })
end)

if not success then
    warn("Failed to save data:", error)
end
```

### Loading Data (Get)
```lua
local success, data = pcall(function()
    return myDataStore:GetAsync("key")
end)

if success then
    print("Loaded data:", data)
else
    warn("Failed to load data:", data)
end
```

### Updating Data (Update)
```lua
local success, data = pcall(function()
    return myDataStore:UpdateAsync("key", function(oldData)
        if not oldData then
            return {
                coins = 100,
                level = 1
            }
        end
        
        -- Modify the existing data
        oldData.coins = oldData.coins + 50
        oldData.level = oldData.level + 1
        
        return oldData
    end)
end)
```

## Best Practices

### 1. Error Handling
```lua
local function saveData(key, data)
    local success, result = pcall(function()
        return myDataStore:SetAsync(key, data)
    end)
    
    if not success then
        warn("Failed to save data for key:", key)
        warn("Error:", result)
        return false
    end
    
    return true
end
```

### 2. Rate Limiting
```lua
local function getData(key)
    -- Wait for any throttling
    local timeToWait = myDataStore:GetRequestBudgetForRequestType(Enum.DataStoreRequestType.GetAsync)
    if timeToWait > 0 then
        task.wait(timeToWait)
    end
    
    return myDataStore:GetAsync(key)
end
```

### 3. Default Values
```lua
local defaultData = {
    coins = 0,
    level = 1,
    inventory = {},
    settings = {
        musicVolume = 0.5,
        sfxVolume = 0.7
    }
}

local function getPlayerData(key)
    local success, data = pcall(function()
        return myDataStore:GetAsync(key)
    end)
    
    if not success or not data then
        return table.clone(defaultData)
    end
    
    return data
end
```

## Data Structure Examples

### Player Data Structure
```lua
local playerData = {
    stats = {
        level = 1,
        xp = 0,
        health = 100,
        maxHealth = 100
    },
    inventory = {
        items = {},
        capacity = 20
    },
    achievements = {
        unlockedIds = {},
        points = 0
    },
    settings = {
        musicVolume = 1,
        sfxVolume = 1
    }
}
```

### Saving on Player Leave
```lua
game.Players.PlayerRemoving:Connect(function(player)
    local success, error = pcall(function()
        myDataStore:SetAsync(player.UserId, playerData)
    end)
    
    if not success then
        warn("Failed to save data for player:", player.Name)
        warn("Error:", error)
    end
end)
```

## Common Pitfalls

1. Exceeding Size Limits
```lua
-- Bad: Might exceed 4MB limit
local hugeData = {}
for i = 1, 1000000 do
    hugeData[i] = "lots of data"
end

-- Good: Check data size
local function isDataTooLarge(data)
    local success, jsonData = pcall(function()
        return game:GetService("HttpService"):JSONEncode(data)
    end)
    return success and #jsonData > 4 * 1024 * 1024 -- 4MB
end
```

2. Not Handling Session Locks
```lua
-- Bad: Might conflict with other servers
myDataStore:SetAsync(key, data)

-- Good: Use UpdateAsync for modifications
myDataStore:UpdateAsync(key, function(oldData)
    if not oldData then return data end
    -- Modify oldData safely here
    return oldData
end)
```

3. Not Using Backup Systems
```lua
local function saveWithBackup(key, data)
    -- Try main save
    local success = pcall(function()
        myDataStore:SetAsync(key, data)
    end)
    
    if not success then
        -- Try backup save
        local backupStore = DataStoreService:GetDataStore("Backup_" .. key)
        pcall(function()
            backupStore:SetAsync(key, data)
        end)
    end
end
```