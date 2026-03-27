-- Criar a ScreenGui
local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Name = "SpeedGUI"
gui.Parent = player:WaitForChild("PlayerGui")

-- Criar o Frame principal
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 200, 0, 150)
mainFrame.Position = UDim2.new(0, 10, 0, 10)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.BackgroundTransparency = 0.1
mainFrame.BorderSizePixel = 2
mainFrame.BorderColor3 = Color3.fromRGB(255, 255, 255)
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = gui

-- Adicionar borda arredondada
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 8)
corner.Parent = mainFrame

-- Título do Frame
local title = Instance.new("TextLabel")
title.Name = "Title"
title.Size = UDim2.new(1, 0, 0, 30)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
title.BackgroundTransparency = 0.5
title.Text = "Speed Control"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 18
title.Parent = mainFrame

-- Botão Minimizar/Maximizar
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

-- Corner do botão minimizar
local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 4)
buttonCorner.Parent = minimizeButton

-- Frame de conteúdo (que será minimizado)
local contentFrame = Instance.new("Frame")
contentFrame.Name = "ContentFrame"
contentFrame.Size = UDim2.new(1, 0, 1, -30)
contentFrame.Position = UDim2.new(0, 0, 0, 30)
contentFrame.BackgroundTransparency = 1
contentFrame.Parent = mainFrame

-- Slider para velocidade
local speedSlider = Instance.new("Frame")
speedSlider.Name = "SpeedSlider"
speedSlider.Size = UDim2.new(0.8, 0, 0, 40)
speedSlider.Position = UDim2.new(0.1, 0, 0.2, 0)
speedSlider.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
speedSlider.BackgroundTransparency = 0.5
speedSlider.Parent = contentFrame

local sliderCorner = Instance.new("UICorner")
sliderCorner.CornerRadius = UDim.new(0, 4)
sliderCorner.Parent = speedSlider

-- Barra de progresso do slider
local sliderProgress = Instance.new("Frame")
sliderProgress.Name = "SliderProgress"
sliderProgress.Size = UDim2.new(0.5, 0, 1, 0)
sliderProgress.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
sliderProgress.BackgroundTransparency = 0.3
sliderProgress.Parent = speedSlider

local progressCorner = Instance.new("UICorner")
progressCorner.CornerRadius = UDim.new(0, 4)
progressCorner.Parent = sliderProgress

-- Botão do slider (para arrastar)
local sliderButton = Instance.new("TextButton")
sliderButton.Name = "SliderButton"
sliderButton.Size = UDim2.new(0, 20, 1, -4)
sliderButton.Position = UDim2.new(0.5, -10, 0, 2)
sliderButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
sliderButton.Text = ""
sliderButton.Parent = speedSlider

local buttonCorner2 = Instance.new("UICorner")
buttonCorner2.CornerRadius = UDim.new(1, 0)
buttonCorner2.Parent = sliderButton

-- Label para mostrar o valor da velocidade
local speedValueLabel = Instance.new("TextLabel")
speedValueLabel.Name = "SpeedValueLabel"
speedValueLabel.Size = UDim2.new(0.8, 0, 0, 30)
speedValueLabel.Position = UDim2.new(0.1, 0, 0.6, 0)
speedValueLabel.BackgroundTransparency = 1
speedValueLabel.Text = "Speed: 16"
speedValueLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
speedValueLabel.Font = Enum.Font.Gotham
speedValueLabel.TextSize = 16
speedValueLabel.Parent = contentFrame

-- Label de instrução
local instructionLabel = Instance.new("TextLabel")
instructionLabel.Size = UDim2.new(0.8, 0, 0, 20)
instructionLabel.Position = UDim2.new(0.1, 0, 0.8, 0)
instructionLabel.BackgroundTransparency = 1
instructionLabel.Text = "Drag to adjust speed"
instructionLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
instructionLabel.Font = Enum.Font.Gotham
instructionLabel.TextSize = 12
instructionLabel.Parent = contentFrame

-- Variáveis
local isMinimized = false
local originalSize = mainFrame.Size
local originalContentHeight = contentFrame.Size.Y.Scale
local currentSpeed = 16 -- Velocidade padrão do Roblox

-- Função para salvar a velocidade
local function saveSpeed(speed)
    -- Salvar em diferentes lugares para garantir persistência
    if player:FindFirstChild("SpeedValue") then
        player.SpeedValue.Value = speed
    else
        local speedValue = Instance.new("NumberValue")
        speedValue.Name = "SpeedValue"
        speedValue.Value = speed
        speedValue.Parent = player
    end
    
    -- Também salvar no DataStore (opcional, para persistência entre sessões)
    -- Isso requer configuração de DataStore no servidor
end

-- Função para carregar a velocidade salva
local function loadSavedSpeed()
    local savedSpeed = 16
    if player:FindFirstChild("SpeedValue") then
        savedSpeed = player.SpeedValue.Value
    end
    return savedSpeed
end

-- Função para aplicar velocidade ao personagem
local function applySpeed(speed)
    local character = player.Character
    if character then
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = speed
        end
    end
end

-- Função para atualizar o slider visualmente
local function updateSliderVisual(value)
    local minSpeed = 16
    local maxSpeed = 100
    local normalizedValue = (value - minSpeed) / (maxSpeed - minSpeed)
    sliderProgress.Size = UDim2.new(normalizedValue, 0, 1, 0)
    sliderButton.Position = UDim2.new(normalizedValue, -10, 0, 2)
    speedValueLabel.Text = "Speed: " .. math.floor(value)
end

-- Função para atualizar velocidade
local function updateSpeed(speed)
    speed = math.clamp(speed, 16, 100)
    currentSpeed = speed
    saveSpeed(speed)
    applySpeed(speed)
    updateSliderVisual(speed)
end

-- Carregar velocidade salva
local savedSpeed = loadSavedSpeed()
updateSpeed(savedSpeed)

-- Conectar com o reset do personagem
player.CharacterAdded:Connect(function(character)
    wait(0.5) -- Aguardar o humanoid carregar
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = currentSpeed
    end
    
    -- Conectar para quando o humanoid for adicionado
    character.ChildAdded:Connect(function(child)
        if child:IsA("Humanoid") then
            child.WalkSpeed = currentSpeed
        end
    end)
end)

-- Lógica do slider arrastável
local dragging = false
local sliderButton = sliderButton

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
        local sliderFrame = speedSlider
        local relativeX = mouse.X - sliderFrame.AbsolutePosition.X
        local width = sliderFrame.AbsoluteSize.X
        local percent = math.clamp(relativeX / width, 0, 1)
        
        local minSpeed = 16
        local maxSpeed = 100
        local newSpeed = minSpeed + (maxSpeed - minSpeed) * percent
        
        updateSpeed(newSpeed)
    end
end)

-- Botão minimizar/maximizar
local function toggleMinimize()
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
end

minimizeButton.MouseButton1Click:Connect(toggleMinimize)

-- Tornar o frame arrastável pelo título
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

print("Speed GUI loaded! Speed will persist after reset.")
