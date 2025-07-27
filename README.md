local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local CurrentSpeed = 16
local CurrentJump = 50
local CurrentGravity = 196.2

-- Função para reaplicar configurações após morrer
Player.CharacterAdded:Connect(function(char)
    task.wait(1)
    char:WaitForChild("Humanoid").WalkSpeed = CurrentSpeed
    char:WaitForChild("Humanoid").JumpPower = CurrentJump
    workspace.Gravity = CurrentGravity
end)

local Window = Rayfield:CreateWindow({
    Name = "⚫ Dark Hub",
    LoadingTitle = "Dark Hub",
    LoadingSubtitle = "by cbleviatanqr and GPT",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "DarkHubConfig",
        FileName = "DarkHubData"
    },
    Discord = {
        Enabled = false
    },
    KeySystem = false
})

----------------------------------------------------------------
-- 1. 🛠️ Hacks
----------------------------------------------------------------
local Hacks = Window:CreateTab("🛠️ Hacks", "rbxassetid://7733960981")

Hacks:CreateButton({
    Name = "💻 FPS Unlocker",
    Callback = function()
        for _, v in pairs(game:GetDescendants()) do
            if v:IsA("Texture") or v:IsA("Decal") then
                v:Destroy()
            end
        end
    end
})

Hacks:CreateToggle({
    Name = "👊 Super Punch [Função Forte Não Recomendável!]",
    CurrentValue = false,
    Callback = function(state)
        if state then
            local function punch()
                local chr = Player.Character
                local hum = chr and chr:FindFirstChildOfClass("Humanoid")
                if hum then
                    for _, plr in pairs(game.Players:GetPlayers()) do
                        if plr ~= Player and plr.Character then
                            plr.Character:BreakJoints()
                        end
                    end
                end
            end
            punch()
        end
    end
})

Hacks:CreateToggle({
    Name = "🎯 Fling (Girar Personagem - Velocidade 25 a 50)",
    CurrentValue = false,
    Callback = function(state)
        if state then
            local hrp = Player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                local bv = Instance.new("BodyAngularVelocity")
                bv.AngularVelocity = Vector3.new(0, 50, 0)
                bv.MaxTorque = Vector3.new(0, math.huge, 0)
                bv.P = 1250
                bv.Name = "FlingForce"
                bv.Parent = hrp
            end
        else
            local hrp = Player.Character:FindFirstChild("HumanoidRootPart")
            if hrp and hrp:FindFirstChild("FlingForce") then
                hrp.FlingForce:Destroy()
            end
        end
    end
})

Hacks:CreateToggle({
    Name = "🧱 Noclip",
    CurrentValue = false,
    Callback = function(state)
        local RunService = game:GetService("RunService")
        if state then
            noclipConnection = RunService.Stepped:Connect(function()
                for _, part in pairs(Player.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end)
        else
            if noclipConnection then noclipConnection:Disconnect() end
        end
    end
})

----------------------------------------------------------------
-- 2. 🎨 Visuais
----------------------------------------------------------------
local Visuais = Window:CreateTab("🎨 Visuais", "rbxassetid://7734053494")

Visuais:CreateButton({
    Name = "🌫️ Remover Fog",
    Callback = function()
        game.Lighting.FogEnd = 100000
        game.Lighting.FogStart = 100000
    end
})

Visuais:CreateToggle({
    Name = "🌕 Fullbright",
    CurrentValue = false,
    Callback = function(state)
        game.Lighting.Brightness = state and 10 or 2
        game.Lighting.Ambient = state and Color3.new(1,1,1) or Color3.new(0.5,0.5,0.5)
    end
})

Visuais:CreateToggle({
    Name = "✨ Shaders [Se seu celular aguentar]",
    CurrentValue = false,
    Callback = function(state)
        if state then
            local blur = Instance.new("BlurEffect", game.Lighting)
            blur.Size = 2
        else
            for _,v in pairs(game.Lighting:GetChildren()) do
                if v:IsA("BlurEffect") then
                    v:Destroy()
                end
            end
        end
    end
})

Visuais:CreateButton({
    Name = "☀️ Alterar para Dia",
    Callback = function() game.Lighting.TimeOfDay = "14:00:00" end
})

Visuais:CreateButton({
    Name = "🌙 Alterar para Noite",
    Callback = function() game.Lighting.TimeOfDay = "00:00:00" end
})

Visuais:CreateToggle({
    Name = "🌟 Personagem Brilhante",
    CurrentValue = false,
    Callback = function(state)
        for _, part in pairs(Player.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.Material = state and Enum.Material.Neon or Enum.Material.Plastic
            end
        end
    end
})

----------------------------------------------------------------
-- 3. ⚡ Velocidade
----------------------------------------------------------------
local Velocidade = Window:CreateTab("⚡ Velocidade", "rbxassetid://7734001754")

Velocidade:CreateSlider({
    Name = "Velocidade",
    Range = {16, 200},
    Increment = 1,
    CurrentValue = 16,
    Callback = function(value)
        CurrentSpeed = value
        Player.Character.Humanoid.WalkSpeed = value
    end
})

Velocidade:CreateButton({
    Name = "🔄 Recarregar",
    Callback = function()
        Player.Character.Humanoid.WalkSpeed = CurrentSpeed
    end
})

----------------------------------------------------------------
-- 4. 🦘 Pulo
----------------------------------------------------------------
local Pulo = Window:CreateTab("🦘 Pulo", "rbxassetid://7734004726")

Pulo:CreateSlider({
    Name = "Pulo",
    Range = {50, 250},
    Increment = 5,
    CurrentValue = 50,
    Callback = function(value)
        CurrentJump = value
        Player.Character.Humanoid.JumpPower = value
    end
})

Pulo:CreateButton({
    Name = "🔄 Recarregar",
    Callback = function()
        Player.Character.Humanoid.JumpPower = CurrentJump
    end
})

----------------------------------------------------------------
-- 5. 🌌 Gravidade
----------------------------------------------------------------
local Gravidade = Window:CreateTab("🌌 Gravidade", "rbxassetid://7734011320")

Gravidade:CreateSlider({
    Name = "Gravidade",
    Range = {0, 500},
    Increment = 10,
    CurrentValue = 196.2,
    Callback = function(value)
        CurrentGravity = value
        workspace.Gravity = value
    end
})

Gravidade:CreateButton({
    Name = "🔄 Recarregar",
    Callback = function()
        workspace.Gravity = CurrentGravity
    end
})

----------------------------------------------------------------
-- 6. 📍 Teleporte
----------------------------------------------------------------
local Teleporte = Window:CreateTab("📍 Teleporte", "rbxassetid://7734017282")

local function updateTeleports()
    for _, plr in pairs(game.Players:GetPlayers()) do
        if plr ~= Player then
            Teleporte:CreateButton({
                Name = "📌 " .. plr.Name,
                Callback = function()
                    local root = plr.Character and plr.Character:FindFirstChild("HumanoidRootPart")
                    if root then
                        Player.Character:MoveTo(root.Position)
                    end
                end
            })
        end
    end
end

updateTeleports()

Teleporte:CreateButton({
    Name = "🔄 Recarregar",
    Callback = updateTeleports
})

----------------------------------------------------------------
-- 7. 👤 Jogador
----------------------------------------------------------------
local Jogador = Window:CreateTab("👤 Jogador", "rbxassetid://7734020922")

Jogador:CreateParagraph({
    Title = "Nome de usuário",
    Content = Player.Name
})

Jogador:CreateParagraph({
    Title = "Link do Perfil",
    Content = "https://www.roblox.com/users/"..Player.UserId.."/profile"
})

Jogador:CreateParagraph({
    Title = "Total de Jogadores no Servidor",
    Content = tostring(#game.Players:GetPlayers())
})

Jogador:CreateButton({
    Name = "🔄 Recarregar",
    Callback = function()
        Rayfield:Notify({
            Title = "Recarregado!",
            Content = "Informações atualizadas.",
            Duration = 3
        })
    end
})

----------------------------------------------------------------
-- 8. ⭐ Créditos
----------------------------------------------------------------
local Creditos = Window:CreateTab("⭐ Créditos", "rbxassetid://7734035323")

Creditos:CreateParagraph({
    Title = "Feito por:",
    Content = "cbleviatanqr e GPT"
})
