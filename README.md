local CollectionService = game:GetService("CollectionService")
local Players = game:GetService("Players")

-- Define the coin model name
local COIN_MODEL_NAME = "Coin"

-- Define the event function to handle player collecting coins
local function onTouched(coin, other)
    if other:IsA("Player") then
        -- Award points to the player
        local player = other
        -- You can add your scoring system here
        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + 1

        -- Destroy the coin
        coin:Destroy()
    end
end

-- Function to handle coin spawning
local function spawnCoins()
    -- Spawn coins in the game world
    for i = 1, 10 do -- Adjust the number of coins as needed
        local coin = Instance.new("Part")
        coin.Name = "Coin"
        coin.Size = Vector3.new(2, 2, 2) -- Adjust the size of the coin as needed
        coin.Shape = Enum.PartType.Ball
        coin.BrickColor = BrickColor.new("Bright yellow")
        coin.Position = Vector3.new(math.random(-50, 50), 5, math.random(-50, 50)) -- Adjust spawn area as needed
        coin.Anchored = true
        coin.CanCollide = false
        coin.Parent = game.Workspace

        -- Tag the coin with a collection tag
        CollectionService:AddTag(coin, COIN_MODEL_NAME)

        -- Connect the onTouched event to the coin
        coin.Touched:Connect(function(other)
            onTouched(coin, other)
        end)
    end
end

-- Function to handle player joining
local function onPlayerAdded(player)
    -- Create leaderstats folder for player
    local leaderstats = Instance.new("Folder")
    leaderstats.Name = "leaderstats"
    leaderstats.Parent = player

    -- Create Coins value for player
    local coins = Instance.new("IntValue")
    coins.Name = "Coins"
    coins.Value = 0
    coins.Parent = leaderstats

    -- Spawn coins for the player
    spawnCoins()
end

-- Connect player added event
Players.PlayerAdded:Connect(onPlayerAdded)
