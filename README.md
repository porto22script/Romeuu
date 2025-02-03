if not game:IsLoaded() then
    game.Loaded:Wait()
end

local player = game.Players.LocalPlayer
repeat wait() until player.Character
local character = player.Character
local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")

-- Executa o Infinite Yield
loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source", true))()

-- Configurações
local flying = false
local ringActive = false
local ringRadius = 10
local numObjects = 10

-- Criar GUI estilosa
local screenGui = Instance.new("ScreenGui", game.CoreGui)
screenGui.Name = "RomeuScripts"
screenGui.ResetOnSpawn = false

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 250, 0, 350)
frame.Position = UDim2.new(0, 50, 0, 150)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BorderSizePixel = 3
frame.BorderColor3 = Color3.fromRGB(255, 255, 255)

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 50)
title.Text = "ROMEUSCRIPTS"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
title.TextScaled = true
title.Font = Enum.Font.SourceSansBold

local function createButton(name, position, callback)
    local button = Instance.new("TextButton", frame)
    button.Size = UDim2.new(1, -20, 0, 40)
    button.Position = UDim2.new(0, 10, 0, position)
    button.Text = name
    button.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 18
    button.BorderSizePixel = 2
    button.BorderColor3 = Color3.fromRGB(255, 255, 255)
    button.MouseButton1Click:Connect(callback)
end

-- Ativar/Desativar voo
local function toggleFlight()
    flying = not flying
    while flying do
        humanoidRootPart.Velocity = Vector3.new(0, 50, 0)
        wait(0.1)
    end
end

-- Criar ou remover o anel (ativa/desativa NoClip junto)
local function toggleRing()
    ringActive = not ringActive

    if ringActive then
        -- Ativar NoClip
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end

        -- Criar o anel
        for i = 1, numObjects do
            local angle = (i / numObjects) * math.pi * 2
            local x = math.cos(angle) * ringRadius
            local z = math.sin(angle) * ringRadius

            local obj = Instance.new("Part")
            obj.Size = Vector3.new(2, 2, 2)
            obj.Position = humanoidRootPart.Position + Vector3.new(x, 0, z)
            obj.Anchored = true
            obj.Material = Enum.Material.Neon
            obj.Color = Color3.fromRGB(255, 255, 255)
            obj.Name = "RingPart"
            obj.Parent = game.Workspace
        end
    else
        -- Desativar NoClip
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end

        -- Remover o anel
        for _, part in pairs(game.Workspace:GetChildren()) do
            if part.Name == "RingPart" then
                part:Destroy()
            end
        end
    end
end

-- Criar os botões estilizados na UI
createButton("Ativar Voo", 60, toggleFlight)
createButton("Criar Anel (Com NoClip)", 110, toggleRing)
createButton("Aumentar Raio", 160, function() ringRadius = ringRadius + 5 end)
createButton("Diminuir Raio", 210, function() ringRadius = ringRadius - 5 end)
