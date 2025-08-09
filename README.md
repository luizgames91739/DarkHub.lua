local Rayfield = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Rayfield/main/source"))()

local Window = Rayfield:CreateWindow({
    Name = "Dark Spawner - Grow A Garden",
    LoadingTitle = "Dark Spawner",
    LoadingSubtitle = "Grow A Garden",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = nil,
        FileName = "dark_spawner_config"
    },
    Discord = {Enabled = false},
    KeySystem = false
})

local function findRemote(keyword)
    local RS = game:GetService("ReplicatedStorage")
    local possibles = {}
    local function search(container)
        for _, obj in ipairs(container:GetDescendants()) do
            if (obj:IsA("RemoteEvent") or obj:IsA("RemoteFunction")) and string.find(string.lower(obj.Name), string.lower(keyword)) then
                table.insert(possibles, obj)
            end
        end
    end
    search(RS)
    search(workspace)
    return possibles[1] -- retorna o primeiro achado
end

local function fireRemote(remote, args)
    if remote then
        if remote:IsA("RemoteEvent") then
            remote:FireServer(unpack(args))
        elseif remote:IsA("RemoteFunction") then
            pcall(function() remote:InvokeServer(unpack(args)) end)
        end
        return true
    end
    return false
end

-- Pet Spawner
local PetTab = Window:CreateTab("Pet Spawner", 4483362458)
local petName, petWeight, petAge = "Raccoon", 1, 1

PetTab:CreateInput({Name = "Pet Name", PlaceholderText = "Raccoon", Callback = function(text) petName = text ~= "" and text or "Raccoon" end})
PetTab:CreateSlider({Name = "Pet Weight", Range = {1, 100}, Increment = 1, CurrentValue = 1, Callback = function(v) petWeight = v end})
PetTab:CreateSlider({Name = "Pet Age", Range = {1, 100}, Increment = 1, CurrentValue = 1, Callback = function(v) petAge = v end})
PetTab:CreateButton({
    Name = "Spawn Pet",
    Callback = function()
        local payload = {Name = petName, Weight = petWeight, Age = petAge}
        local remote = findRemote("Pet")
        if fireRemote(remote, {payload}) then
            Rayfield:Notify({Title="Pet Spawner", Content="Pet spawn enviado.", Duration=3, Type="Success"})
        else
            Rayfield:Notify({Title="Pet Spawner", Content="Remote de Pet não encontrado.", Duration=4, Type="Warning"})
            print("Payload Pet:", payload)
        end
    end
})

-- Seed Spawner
local SeedTab = Window:CreateTab("Seed Spawner", 6023426922)
local seedName, seedAmount = "Sunflower", 1

SeedTab:CreateInput({Name = "Seed Name", PlaceholderText = "Sunflower", Callback = function(txt) seedName = txt ~= "" and txt or "Sunflower" end})
SeedTab:CreateSlider({Name = "Amount", Range = {1, 50}, Increment = 1, CurrentValue = 1, Callback = function(v) seedAmount = v end})
SeedTab:CreateButton({
    Name = "Spawn Seeds",
    Callback = function()
        local payload = {seedName, seedAmount}
        local remote = findRemote("Seed")
        if fireRemote(remote, payload) then
            Rayfield:Notify({Title="Seed Spawner", Content="Seeds spawn enviadas.", Duration=3, Type="Success"})
        else
            Rayfield:Notify({Title="Seed Spawner", Content="Remote de Seed não encontrado.", Duration=4, Type="Warning"})
            print("Payload Seed:", payload)
        end
    end
})

-- Egg Spawner
local EggTab = Window:CreateTab("Egg Spawner", 6023426922)
local eggType, eggQuantity = "Common", 1

EggTab:CreateDropdown({Name = "Egg Type", Options = {"Common", "Rare", "Epic", "Legendary"}, CurrentOption = "Common", Multiple = false, Callback = function(opt) eggType = opt end})
EggTab:CreateSlider({Name = "Quantity", Range = {1, 20}, Increment = 1, CurrentValue = 1, Callback = function(v) eggQuantity = v end})
EggTab:CreateButton({
    Name = "Spawn Eggs",
    Callback = function()
        local payload = {eggType, eggQuantity}
        local remote = findRemote("Egg")
        if fireRemote(remote, payload) then
            Rayfield:Notify({Title="Egg Spawner", Content="Eggs spawn enviados.", Duration=3, Type="Success"})
        else
            Rayfield:Notify({Title="Egg Spawner", Content="Remote de Egg não encontrado.", Duration=4, Type="Warning"})
            print("Payload Egg:", payload)
        end
    end
})
