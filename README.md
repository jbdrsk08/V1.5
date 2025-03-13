# V1.5

local textureId = "rbxassetid://118786811857860"
local soundId = "rbxassetid://100792696468630"

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "CombinedGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 400)
frame.Position = UDim2.new(0.5, -150, 0.5, -200)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BackgroundTransparency = 0.9
frame.Active = true
frame.Draggable = true
frame.Parent = screenGui

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(0, 280, 0, 50)
titleLabel.Position = UDim2.new(0.5, -140, 0.0, 0)
titleLabel.Text = "Legsk Hub v1.5"
titleLabel.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.Font = Enum.Font.SourceSansBold
titleLabel.TextSize = 28
titleLabel.TextStrokeTransparency = 0.8
titleLabel.BackgroundTransparency = 0.7
titleLabel.Parent = frame

local imageLabel = Instance.new("ImageLabel")
imageLabel.Size = UDim2.new(0, 100, 0, 100)
imageLabel.Position = UDim2.new(0.5, -50, 0.2, 0)
imageLabel.Image = textureId
imageLabel.BackgroundTransparency = 1
imageLabel.Parent = frame

local function createButton(text, positionY)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 280, 0, 40)
    button.Position = UDim2.new(0.5, -140, positionY, 0)
    button.Text = text
    button.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.BackgroundTransparency = 0.7
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 20
    button.TextStrokeTransparency = 0.8
    button.Parent = frame
    return button
end

local particlesButton = createButton("Particles", 0.35)
local decalsButton = createButton("Decal", 0.45)
local messageButton = createButton("Send Message", 0.55)
local soundButton = createButton("Play Sound", 0.65)
local messageButton2 = createButton("Send Message 2", 0.75)

local function createFullScreenMessage()
    local player = game.Players.LocalPlayer
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "FullScreenMessage"
    screenGui.Parent = player:WaitForChild("PlayerGui")

    local messageLabel = Instance.new("TextLabel")
    messageLabel.Size = UDim2.new(1, 0, 1, 0)
    messageLabel.Position = UDim2.new(0, 0, 0, 0)
    messageLabel.Text = "LEGSK HACKED THIS GAME L O L"
    messageLabel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    messageLabel.BackgroundTransparency = 0.5
    messageLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
    messageLabel.TextStrokeTransparency = 0.5
    messageLabel.TextSize = 40
    messageLabel.Font = Enum.Font.SourceSansBold
    messageLabel.TextWrapped = true
    messageLabel.Parent = screenGui

    wait(5)
    screenGui:Destroy()
end

local function sendTopBarMessage()
    local notification = {
        Title = "LEGSK HACKED THIS SH##",
        Text = "L O L",
        Icon = textureId,
        Duration = 5,
    }

    game:GetService("CoreGui"):SetCore("SendNotification", notification)
end

messageButton.MouseButton1Click:Connect(createFullScreenMessage)
messageButton2.MouseButton1Click:Connect(sendTopBarMessage)

local function playSound()
    local sound = Instance.new("Sound")
    sound.SoundId = soundId
    sound.Pitch = 0.15
    sound.Volume = math.huge
    sound.Parent = game.Players.LocalPlayer.Character or game.Workspace
    sound:Play()
end

soundButton.MouseButton1Click:Connect(playSound)

local particleId = "rbxassetid://118786811857860"

local function addParticleEmitter(object)
    if object:IsA("BasePart") then
        local particleEmitter = Instance.new("ParticleEmitter")
        particleEmitter.Texture = particleId
        particleEmitter.Lifetime = NumberRange.new(1, 2)
        particleEmitter.Rate = 50
        particleEmitter.VelocitySpread = 180
        particleEmitter.Parent = object
    end

    for _, child in ipairs(object:GetChildren()) do
        addParticleEmitter(child)
    end
end

local function activateParticles()
    for _, object in ipairs(game.Workspace:GetChildren()) do
        addParticleEmitter(object)
    end

    local function addParticleEmitterToPlayer(player)
        local character = player.Character
        if character then
            for _, part in ipairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    local particleEmitter = Instance.new("ParticleEmitter")
                    particleEmitter.Texture = particleId
                    particleEmitter.Lifetime = NumberRange.new(1, 2)
                    particleEmitter.Rate = 50
                    particleEmitter.VelocitySpread = 180
                    particleEmitter.Parent = part
                end
            end
        end
    end

    game.Players.PlayerAdded:Connect(function(player)
        player.CharacterAdded:Connect(function(character)
            addParticleEmitterToPlayer(player)
        end)
    end)

    for _, player in ipairs(game.Players:GetPlayers()) do
        addParticleEmitterToPlayer(player)
    end
end

particlesButton.MouseButton1Click:Connect(activateParticles)

local function replaceTextures(object)
    if object:IsA("BasePart") then
        for _, child in ipairs(object:GetChildren()) do
            if child:IsA("Decal") or child:IsA("Texture") then
                child.Texture = textureId
            end
        end
        if not object:FindFirstChildOfClass("Texture") then
            local texture = Instance.new("Texture")
            texture.Texture = textureId
            texture.Parent = object
        end
        if not object:FindFirstChildOfClass("Decal") then
            local decal = Instance.new("Decal")
            decal.Texture = textureId
            decal.Face = Enum.NormalId.Top
            decal.Parent = object
        end
    end

    for _, child in ipairs(object:GetChildren()) do
        replaceTextures(child)
    end
end

local function activateDecals()
    for _, object in ipairs(game.Workspace:GetChildren()) do
        replaceTextures(object)
    end

    local skybox = Instance.new("Sky")
    skybox.SkyboxBk = textureId
    skybox.SkyboxDn = textureId
    skybox.SkyboxFt = textureId
    skybox.SkyboxLf = textureId
    skybox.SkyboxRt = textureId
    skybox.SkyboxUp = textureId
    skybox.Parent = game.Lighting

    for _, terrain in ipairs(workspace:GetDescendants()) do
        if terrain:IsA("Terrain") then
            terrain.Material = Enum.Material.SmoothPlastic
            terrain.Decal = textureId
        end
        if terrain:IsA("BasePart") then
            for _, child in ipairs(terrain:GetChildren()) do
                if child:IsA("Decal") or child:IsA("Texture") then
                    child.Texture = textureId
                end
            end
            if not terrain:FindFirstChildOfClass("Texture") then
                local texture = Instance.new("Texture")
                texture.Texture = textureId
                texture.Parent = terrain
            end
            if not terrain:FindFirstChildOfClass("Decal") then
                local decal = Instance.new("Decal")
                decal.Texture = textureId
                decal.Face = Enum.NormalId.Top
                decal.Parent = terrain
            end
        end
    end
end

decalsButton.MouseButton1Click:Connect(activateDecals)
