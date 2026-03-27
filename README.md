-- ADICIONE ESTE CÓDIGO AO SEU SCRIPT EXISTENTE
-- Coloque dentro do mesmo LocalScript, após o código anterior

-- ========== SISTEMA DE AUTO COLETA DE MOEDAS ==========

-- Variáveis do sistema de coleta
local autoCollectEnabled = false
local collectConnection = nil
local collectInterval = nil

-- Criar o botão no contentFrame existente
local autoCollectButton = Instance.new("TextButton")
autoCollectButton.Name = "AutoCollectButton"
autoCollectButton.Size = UDim2.new(0.8, 0, 0, 35)
autoCollectButton.Position = UDim2.new(0.1, 0, 0.45, 0)
autoCollectButton.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
autoCollectButton.Text = "🔴 Auto Collect: OFF"
autoCollectButton.TextColor3 = Color3.fromRGB(255, 255, 255)
autoCollectButton.Font = Enum.Font.GothamBold
autoCollectButton.TextSize = 14
autoCollectButton.Parent = contentFrame

-- Arredondar botão
local buttonCorner3 = Instance.new("UICorner")
buttonCorner3.CornerRadius = UDim.new(0, 4)
buttonCorner3.Parent = autoCollectButton

-- Ajustar posição dos outros elementos para não sobrepor
speedValueLabel.Position = UDim2.new(0.1, 0, 0.7, 0)
instructionLabel.Position = UDim2.new(0.1, 0, 0.85, 0)

-- Função para encontrar e coletar moedas
local function collectCoins()
    local character = player.Character
    if not character or not character:FindFirstChild("HumanoidRootPart") then
        return
    end
    
    local rootPart = character.HumanoidRootPart
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    
    -- Procurar por moedas no Workspace
    -- Em Anime Fighters Simulator, moedas geralmente são partes com nomes como "Coin", "Money", "Cash", etc.
    local coins = workspace:GetDescendants()
    
    local collectedCount = 0
    local collectionRadius = 15 -- Raio de coleta
    
    for _, obj in ipairs(coins) do
        -- Verificar se é uma moeda (ajuste os nomes conforme necessário)
        if obj:IsA("BasePart") and obj.Parent and (
            obj.Name:lower():find("coin") or 
            obj.Name:lower():find("money") or 
            obj.Name:lower():find("cash") or
            obj.Name:lower():find("drop") or
            obj.Name:lower():find("orb") or
            (obj:FindFirstChild("TouchInterest") and obj.Parent:FindFirstChild("Value")) -- Para detectar itens coletáveis
        ) then
            -- Calcular distância
            local distance = (rootPart.Position - obj.Position).Magnitude
            
            -- Se estiver dentro do raio, coletar
            if distance <= collectionRadius then
                -- Simular toque ou coletar diretamente
                -- Método 1: Simular toque (fire touch)
                if obj:FindFirstChild("TouchInterest") then
                    local touchEvent = obj.TouchInterest:Fire(rootPart)
                end
                
                -- Método 2: Teleportar até a moeda (mais agressivo, mas funciona em alguns jogos)
                -- rootPart.CFrame = obj.CFrame
                
                -- Método 3: Procurar por um ClickDetector ou proximidade
                local clickDetector = obj:FindFirstChildOfClass("ClickDetector")
                if clickDetector then
                    clickDetector:Click()
                end
                
                -- Método 4: Verificar se tem uma função de coleta no servidor
                local collectFunction = obj:FindFirstChild("Collect")
                if collectFunction and typeof(collectFunction) == "function" then
                    collectFunction:Invoke(player)
                end
                
                collectedCount = collectedCount + 1
            end
        end
    end
    
    if collectedCount > 0 then
        -- Atualizar visualização (opcional)
        autoCollectButton.Text = "🟢 Auto Collect: ON (" .. collectedCount .. ")"
        wait(0.5)
        autoCollectButton.Text = "🟢 Auto Collect: ON"
    end
end

-- Função para iniciar a coleta automática
local function startAutoCollect()
    if autoCollectEnabled then
        return
    end
    
    autoCollectEnabled = true
    autoCollectButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    autoCollectButton.Text = "🟢 Auto Collect: ON"
    
    -- Coletar a cada 0.5 segundos
    collectInterval = game:GetService("RunService").Heartbeat:Connect(function()
        if autoCollectEnabled and player.Character then
            collectCoins()
        end
    end)
    
    -- Também conectar ao movimento do personagem (opcional)
    local character = player.Character
    if character then
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            collectConnection = humanoid:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
                if autoCollectEnabled then
                    collectCoins()
                end
            end)
        end
    end
    
    -- Conectar quando personagem respawnar
    local characterAddedConnection
    characterAddedConnection = player.CharacterAdded:Connect(function(character)
        wait(0.5)
        if autoCollectEnabled then
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                if collectConnection then
                    collectConnection:Disconnect()
                end
                collectConnection = humanoid:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
                    if autoCollectEnabled then
                        collectCoins()
                    end
                end)
            end
        end
    end)
    
    -- Armazenar conexões para depois desconectar
    if not player:FindFirstChild("AutoCollectData") then
        local data = Instance.new("Folder")
        data.Name = "AutoCollectData"
        data.Parent = player
    end
    
    player.AutoCollectData:SetAttribute("CharacterConnection", characterAddedConnection)
end

-- Função para parar a coleta automática
local function stopAutoCollect()
    if not autoCollectEnabled then
        return
    end
    
    autoCollectEnabled = false
    autoCollectButton.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
    autoCollectButton.Text = "🔴 Auto Collect: OFF"
    
    -- Desconectar eventos
    if collectInterval then
        collectInterval:Disconnect()
        collectInterval = nil
    end
    
    if collectConnection then
        collectConnection:Disconnect()
        collectConnection = nil
    end
    
    if player:FindFirstChild("AutoCollectData") then
        local charConnection = player.AutoCollectData:GetAttribute("CharacterConnection")
        if charConnection then
            charConnection:Disconnect()
        end
    end
end

-- Função para alternar o auto collect
local function toggleAutoCollect()
    if autoCollectEnabled then
        stopAutoCollect()
    else
        startAutoCollect()
    end
end

-- Conectar botão
autoCollectButton.MouseButton1Click:Connect(toggleAutoCollect)

-- Função melhorada para detectar drops de NPCs (quando você elimina um)
local function setupDropDetection()
    -- Detectar quando um NPC é eliminado
    local function checkForDrops()
        -- Procurar por novos objetos no workspace
        local newObjects = {}
        
        -- Usar DescendantAdded para detectar novos itens
        workspace.DescendantAdded:Connect(function(descendant)
            if autoCollectEnabled and descendant:IsA("BasePart") then
                -- Verificar se é uma moeda/item coletável
                local isCollectable = false
                
                if descendant.Name:lower():find("coin") or 
                   descendant.Name:lower():find("money") or 
                   descendant.Name:lower():find("cash") or
                   descendant.Name:lower():find("drop") then
                    isCollectable = true
                end
                
                -- Verificar por partes com brilho (comum em drops)
                if descendant:FindFirstChild("PointLight") or descendant:FindFirstChild("Sparkles") then
                    isCollectable = true
                end
                
                -- Verificar se tem um valor associado (moedas geralmente têm)
                if descendant:FindFirstChild("Value") or descendant:FindFirstChild("MoneyValue") then
                    isCollectable = true
                end
                
                if isCollectable then
                    -- Coletar imediatamente
                    local character = player.Character
                    if character and character:FindFirstChild("HumanoidRootPart") then
                        local rootPart = character.HumanoidRootPart
                        local distance = (rootPart.Position - descendant.Position).Magnitude
                        
                        if distance <= 20 then
                            -- Tentar coletar
                            if descendant:FindFirstChild("TouchInterest") then
                                descendant.TouchInterest:Fire(rootPart)
                            end
                        end
                    end
                end
            end
        end)
    end
    
    checkForDrops()
end

-- Iniciar detecção de drops
setupDropDetection()

-- Salvar estado do auto collect (opcional)
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

-- Carregar estado salvo
local function loadAutoCollectState()
    if player:FindFirstChild("AutoCollectState") then
        local savedState = player.AutoCollectState.Value
        if savedState then
            startAutoCollect()
        end
    end
end

-- Carregar estado automaticamente quando o script iniciar
wait(1)
loadAutoCollectState()

-- Salvar estado quando o jogador sair
player.AncestryChanged:Connect(function()
    saveAutoCollectState()
end)

print("Auto Collect System Loaded!")

-- ========== FIM DO SISTEMA DE AUTO COLETA ==========
