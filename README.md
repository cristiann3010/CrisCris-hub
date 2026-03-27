-- ========== SCRIPT COMPLETO - SPEED + AUTO COLLECT ==========
-- Cole isto em um LocalScript dentro de StarterGui ou StarterPlayerScripts

local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Name = "SpeedGUI"
gui.Parent = player:WaitForChild("PlayerGui")

-- ========== CRIAÇÃO DO FRAME PRINCIPAL ==========
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 220, 0, 200)
mainFrame.Position = UDim2.new(0, 10, 0, 10)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.BackgroundTransparency = 0.1
mainFrame.BorderSizePixel = 2
mainFrame.BorderColor3 = Color3.fromRGB(255, 255, 255)
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = gui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 8)
corner.Parent = mainFrame

-- Título
local title = Instance.new("TextLabel")
title.Name = "Title"
title.Size = UDim2.new(1, 0, 0, 30)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
title.BackgroundTransparency = 0.5
title.Text = "Speed Control + Auto Collect"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 14
title.Parent = mainFrame

-- Botão Minimizar
local minimizeButton = Instance.new("TextButton")
minimizeButton.Name = "MinimizeButton"
minimizeButton.Size = UDim2.new(0, 30, 0, 30)
minimizeButton.Position = UDim2.new(1, -35, 0, 0)
minimizeButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
minimizeButton.Text = "−"
minimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeButton.Font = Enum.Font.GothamBold
minimizeButton.TextSize = 20
minimizeButton.Parent = mainFrame

local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 4)
buttonCorner.Parent = minimizeButton

-- Frame de conteúdo
local contentFrame = Instance.new("Frame")
contentFrame.Name = "ContentFrame"
contentFrame.Size = UDim2.new(1, 0, 1, -30)
contentFrame.Position = UDim2.new(0, 0, 0, 30)
contentFrame.BackgroundTransparency = 1
contentFrame.Parent = mainFrame

-- ========== CONTROLE DE SPEED ==========
local speedLabel = Instance.new("TextLabel")
speedLabel.Size = UDim2.new(0.9, 0, 0, 25)
speedLabel.Position = UDim2.new(0.05, 0, 0.05, 0)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "Walk Speed:"
speedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
speedLabel.Font = Enum.Font.Gotham
speedLabel.TextSize = 14
speedLabel.TextXAlignment = Enum.TextXAlignment.Left
speedLabel.Parent = contentFrame

local speedValue = Instance.new("TextLabel")
speedValue.Size = UDim2.new(0.3, 0, 0, 25)
speedValue.Position = UDim2.new(0.65, 0, 0.05, 0)
speedValue.BackgroundTransparency = 1
speedValue.Text = "16"
speedValue.TextColor3 = Color3.fromRGB(0, 200, 255)
speedValue.Font = Enum.Font.GothamBold
speedValue.TextSize = 14
speedValue.TextXAlignment = Enum.TextXAlignment.Right
speedValue.Parent = contentFrame

-- Slider
local sliderBg = Instance.new("Frame")
sliderBg.Size = UDim2.new(0.9, 0, 0, 25)
sliderBg.Position = UDim2.new(0.05, 0, 0.2, 0)
sliderBg.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
sliderBg.BackgroundTransparency = 0.5
sliderBg.Parent = contentFrame

local sliderBgCorner = Instance.new("UICorner")
sliderBgCorner.CornerRadius = UDim.new(0, 4)
sliderBgCorner.Parent = sliderBg

local sliderFill = Instance.new("Frame")
sliderFill.Size = UDim2.new(0, 0, 1, 0)
sliderFill.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
sliderFill.BackgroundTransparency = 0.3
sliderFill.Parent = sliderBg

local sliderFillCorner = Instance.new("UICorner")
sliderFillCorner.CornerRadius = UDim.new(0, 4)
sliderFillCorner.Parent = sliderFill

local sliderButton = Instance.new("TextButton")
sliderButton.Size = UDim2.new(0, 20, 1, -4)
sliderButton.Position = UDim2.new(0, -10, 0, 2)
sliderButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
sliderButton.Text = ""
sliderButton.Parent = sliderBg

local sliderButtonCorner = Instance.new("UICorner")
sliderButtonCorner.CornerRadius = UDim.new(1, 0)
sliderButtonCorner.Parent = sliderButton

-- ========== BOTÃO AUTO COLLECT ==========
local autoCollectButton = Instance.new("TextButton")
autoCollectButton.Name = "AutoCollectButton"
autoCollectButton.Size = UDim2.new(0.9, 0, 0, 40)
autoCollectButton.Position = UDim2.new(0.05, 0, 0.45, 0)
autoCollectButton.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
autoCollectButton.Text = "🔴 AUTO COLLECT: OFF"
autoCollectButton.TextColor3 = Color3.fromRGB(255, 255, 255)
autoCollectButton.Font = Enum.Font.GothamBold
autoCollectButton.TextSize = 14
autoCollectButton.Parent = contentFrame

local autoCollectCorner = Instance.new("UICorner")
autoCollectCorner.CornerRadius = UDim.new(0, 4)
autoCollectCorner.Parent = autoCollectButton

-- Instrução
local instructionLabel = Instance.new("TextLabel")
instructionLabel.Size = UDim2.new(0.9, 0, 0, 20)
instructionLabel.Position = UDim2.new(0.05, 0, 0.8, 0)
instructionLabel.BackgroundTransparency = 1
instructionLabel.Text = "Drag to adjust speed | Click button to auto-collect"
instructionLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
instructionLabel.Font = Enum.Font.Gotham
instructionLabel.TextSize = 10
instructionLabel.Parent = contentFrame

-- ========== VARIÁVEIS GLOBAIS ==========
local currentSpeed = 16
local minSpeed = 16
local maxSpeed = 100
local autoCollectEnabled = false
local collectConnection = nil
local collectInterval = nil

-- ========== FUNÇÕES DE SPEED ==========
local function saveSpeed(speed)
    if player:FindFirstChild("SpeedValue") then
        player.SpeedValue.Value = speed
    else
        local speedValueSave = Instance.new("NumberValue")
        speedValueSave.Name = "SpeedValue"
        speedValueSave.Value = speed
        speedValueSave.Parent = player
    end
end

local function loadSavedSpeed()
    if player:FindFirstChild("SpeedValue") then
        return player.SpeedValue.Value
    end
    return 16
end

local function applySpeed(speed)
    local character = player.Character
    if character then
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = speed
        end
    end
end

local function updateSpeed(speed)
    speed = math.clamp(speed, minSpeed, maxSpeed)
    currentSpeed = speed
    saveSpeed(speed)
    applySpeed(speed)
    
    -- Atualizar UI
    speedValue.Text = tostring(math.floor(speed))
    local percent = (speed - minSpeed) / (maxSpeed - minSpeed)
    sliderFill.Size = UDim2.new(percent, 0, 1, 0)
    sliderButton.Position = UDim2.new(percent, -10, 0, 2)
end

-- ========== FUNÇÕES DE AUTO COLLECT ==========
local function collectCoins()
    local character = player.Character
    if not character then return end
    
    local rootPart = character:FindFirstChild("HumanoidRootPart")
    if not rootPart then return end
    
    -- Procurar por itens coletáveis
    local allObjects = workspace:GetDescendants()
    local collected = 0
    
    for _, obj in ipairs(allObjects) do
        if obj:IsA("BasePart") and obj.Parent then
            local isCollectable = false
            
            -- Verificar nomes comuns de moedas/drops
            local objName = obj.Name:lower()
            if objName:find("coin") or 
               objName:find("money") or 
               objName:find("cash") or 
               objName:find("drop") or
               objName:find("orb") or
               objName:find("gem") or
               objName:find("soul") or
               objName:find("essence") then
                isCollectable = true
            end
            
            -- Verificar por efeitos visuais comuns em drops
            if obj:FindFirstChild("PointLight") or 
               obj:FindFirstChild("Sparkles") or
               obj:FindFirstChild("SelectionBox") then
                isCollectable = true
            end
            
            -- Verificar por valores
            if obj:FindFirstChild("Value") or 
               obj:FindFirstChild("MoneyValue") or
               obj:FindFirstChild("CoinValue") then
                isCollectable = true
            end
            
            if isCollectable then
                local distance = (rootPart.Position - obj.Position).Magnitude
                if distance <= 20 then
                    -- Tentar coletar de diferentes formas
                    pcall(function()
                        if obj:FindFirstChild("TouchInterest") then
                            obj.TouchInterest:Fire(rootPart)
                        end
                        
                        local clickDetector = obj:FindFirstChildOfClass("ClickDetector")
                        if clickDetector then
                            clickDetector:Click()
                        end
                    end)
                    collected = collected + 1
                end
            end
        end
    end
    
    -- Atualizar visual do botão se coletar algo
    if collected > 0 then
        autoCollectButton.Text = "🟢 COLLECTING... (" .. collected .. ")"
        task.wait(0.3)
        if autoCollectEnabled then
            autoCollectButton.Text = "🟢 AUTO COLLECT: ON"
        end
    end
end

local function startAutoCollect()
    if autoCollectEnabled then return end
    
    autoCollectEnabled = true
    autoCollectButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    autoCollectButton.Text = "🟢 AUTO COLLECT: ON"
    
    -- Coletar a cada 0.5 segundos
    collectInterval = game:GetService("RunService").Heartbeat:Connect(function()
        if autoCollectEnabled and player.Character then
            collectCoins()
        end
    end)
    
    -- Detectar novos drops
    local dropConnection = workspace.DescendantAdded:Connect(function(descendant)
        if autoCollectEnabled and descendant:IsA("BasePart") then
            local objName = descendant.Name:lower()
            if objName:find("coin") or objName:find("money") or objName:find("cash") or objName:find("drop") then
                task.wait(0.1)
                collectCoins()
            end
        end
    end)
    
    -- Armazenar conexões
    if not player:FindFirstChild("AutoCollectData") then
        local dataFolder = Instance.new("Folder")
        dataFolder.Name = "AutoCollectData"
        dataFolder.Parent = player
    end
    player.AutoCollectData:SetAttribute("DropConnection", dropConnection)
end

local function stopAutoCollect()
    if not autoCollectEnabled then return end
    
    autoCollectEnabled = false
    autoCollectButton.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
    autoCollectButton.Text = "🔴 AUTO COLLECT: OFF"
    
    if collectInterval then
        collectInterval:Disconnect()
        collectInterval = nil
    end
    
    if player:FindFirstChild("AutoCollectData") then
        local dropConn = player.AutoCollectData:GetAttribute("DropConnection")
        if dropConn then
            dropConn:Disconnect()
        end
    end
end

local function toggleAutoCollect()
    if autoCollectEnabled then
        stopAutoCollect()
    else
        startAutoCollect()
    end
end

-- ========== EVENTOS ==========
-- Carregar velocidade salva
local savedSpeed = loadSavedSpeed()
updateSpeed(savedSpeed)

-- Reset do personagem
player.CharacterAdded:Connect(function(character)
    task.wait(0.5)
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = currentSpeed
    end
    
    character.ChildAdded:Connect(function(child)
        if child:IsA("Humanoid") then
            child.WalkSpeed = currentSpeed
        end
    end)
end)

-- Slider arrastável
local dragging = false

sliderButton.MouseButton1Down:Connect(function()
    dragging = true
end)

game:GetService("UserInputService").InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

game:GetService("RunService").RenderStepped:Connect(function()
    if dragging then
        local mouse = player:GetMouse()
        local sliderPos = sliderBg.AbsolutePosition.X
        local sliderWidth = sliderBg.AbsoluteSize.X
        local mouseX = mouse.X
        local percent = math.clamp((mouseX - sliderPos) / sliderWidth, 0, 1)
        local newSpeed = minSpeed + (maxSpeed - minSpeed) * percent
        updateSpeed(newSpeed)
    end
end)

-- Botão minimizar
local isMinimized = false
local originalSize = mainFrame.Size

minimizeButton.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    if isMinimized then
        mainFrame.Size = UDim2.new(originalSize.X.Scale, originalSize.X.Offset, 0, 30)
        contentFrame.Visible = false
        minimizeButton.Text = "+"
    else
        mainFrame.Size = originalSize
        contentFrame.Visible = true
        minimizeButton.Text = "−"
    end
end)

-- Arrastar frame pelo título
local draggingFrame = false
local dragStart
local frameStart

title.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        draggingFrame = true
        dragStart = input.Position
        frameStart = mainFrame.Position
    end
end)

title.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        draggingFrame = false
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if draggingFrame and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(frameStart.X.Scale, frameStart.X.Offset + delta.X,
                                        frameStart.Y.Scale, frameStart.Y.Offset + delta.Y)
    end
end)

-- Botão auto collect
autoCollectButton.MouseButton1Click:Connect(toggleAutoCollect)

-- Carregar estado do auto collect se salvo
task.wait(1)
if player:FindFirstChild("AutoCollectState") and player.AutoCollectState.Value then
    startAutoCollect()
end

-- Salvar estado ao sair
local function saveAutoCollectState()
    if player:FindFirstChild("AutoCollectState") then
        player.AutoCollectState.Value = autoCollectEnabled
    else
        local stateValue = Instance.new("BoolValue")
        stateValue.Name = "AutoCollectState"
        stateValue.Value = autoCollectEnabled
        stateValue.Parent = player
    end
end

player.AncestryChanged:Connect(function()
    saveAutoCollectState()
end)

print("✅ Speed + Auto Collect GUI carregado com sucesso!")
