-- Variáveis
local ESPEnabled = false

-- Função para criar um BillboardGui (exibir nome e rosto)
local function CreateBillboardGui(player)
    if not player.Character then return end
    local char = player.Character

    -- Verificar se o ESP já foi criado para este jogador
    if char:FindFirstChild("PlayerESP") then return end

    -- Criar BillboardGui
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Name = "PlayerESP"
    billboardGui.Parent = char:WaitForChild("Head")
    billboardGui.AlwaysOnTop = true
    billboardGui.Size = UDim2.new(4, 0, 2, 0) -- Ajuste o tamanho
    billboardGui.StudsOffset = Vector3.new(0, 3, 0)

    -- Nome do Jogador
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Parent = billboardGui
    nameLabel.Size = UDim2.new(1, 0, 0.5, 0)
    nameLabel.Position = UDim2.new(0, 0, 0, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.Text = player.Name
    nameLabel.TextColor3 = Color3.new(1, 1, 1) -- Cor branca
    nameLabel.TextStrokeTransparency = 0 -- Adicionar contorno no texto
    nameLabel.TextScaled = true
    nameLabel.Font = Enum.Font.GothamBold

    -- Imagem do Rosto (Thumbnail do jogador)
    local faceImage = Instance.new("ImageLabel")
    faceImage.Parent = billboardGui
    faceImage.Size = UDim2.new(1, 0, 0.5, 0)
    faceImage.Position = UDim2.new(0, 0, 0.5, 0)
    faceImage.BackgroundTransparency = 1
    faceImage.Image = "https://www.roblox.com/headshot-thumbnail/image?userId=" .. player.UserId .. "&width=420&height=420&format=png"
end

-- Função para ativar/desativar ESP
local function ToggleESP(state)
    ESPEnabled = state
    if ESPEnabled then
        for _, player in ipairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer then
                CreateBillboardGui(player)
            end
        end
        game.Players.PlayerAdded:Connect(function(newPlayer)
            if ESPEnabled then
                CreateBillboardGui(newPlayer)
            end
        end)
    else
        for _, player in ipairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and player.Character then
                local char = player.Character
                if char:FindFirstChild("Head") and char.Head:FindFirstChild("PlayerESP") then
                    char.Head.PlayerESP:Destroy()
                end
            end
        end
    end
end

-- Configuração das teclas
local UserInputService = game:GetService("UserInputService")

UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Insert then
        ToggleESP(true)
    elseif input.KeyCode == Enum.KeyCode.Delete then
        ToggleESP(false)
    end
end)
