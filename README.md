--========================================================--
-- 🔵 [ Max hub (xerifes vs assassina) ] 🔵
--========================================================--

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- ========================================== --
-- 1. SISTEMA DE MARCA D'ÁGUA (NOME EM AZUL)
-- ========================================== --
local function criarWatermark()
    -- Tenta usar o CoreGui (Delta/Executores) para não aparecer na lista do jogo
    local guiParent
    local sucesso = pcall(function()
        guiParent = game:GetService("CoreGui")
    end)
    
    -- Se falhar (ex: Roblox Studio normal), usa o PlayerGui do jogador
    if not sucesso or not guiParent then
        guiParent = LocalPlayer:WaitForChild("PlayerGui")
    end

    -- Limpa a versão anterior se você executar o script duas vezes
    if guiParent:FindFirstChild("MaxHubInterface") then
        guiParent.MaxHubInterface:Destroy()
    end

    -- Cria a tela
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "MaxHubInterface"
    screenGui.ResetOnSpawn = false -- Não some quando o jogador morre
    screenGui.Parent = guiParent

    -- Cria o texto em azul
    local textLabel = Instance.new("TextLabel")
    textLabel.Parent = screenGui
    textLabel.Size = UDim2.new(0, 450, 0, 50)
    textLabel.Position = UDim2.new(0.5, -225, 0, 15) -- Centralizado no topo
    textLabel.BackgroundTransparency = 1
    
    -- O texto que você pediu!
    textLabel.Text = "[ Max hub (xerifes vs assassina) ]"
    
    -- Cor Azul 
    textLabel.TextColor3 = Color3.fromRGB(0, 130, 255) -- Azul bem vibrante
    textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Borda preta
    textLabel.TextStrokeTransparency = 0.2 -- Deixa a borda bem visível
    
    -- Estilo da fonte
    textLabel.Font = Enum.Font.GothamBlack
    textLabel.TextScaled = true
end

-- ========================================== --
-- 2. SISTEMA DE HITBOX (QUADRADOS AZUIS)
-- ========================================== --
-- Tamanho aumentado em 2.5x (6 * 2.5 = 15)
local TAMANHO_HITBOX = Vector3.new(11, 11, 11) 
-- Cor azul pura para máximo destaque
local COR_AZUL_FORTE = Color3.fromRGB(0, 0, 255) 
-- Transparência levemente reduzida para a cor ficar mais forte
local TRANSPARENCIA = 0.4 

local function criarQuadradoAzul(character)
    if not character then return end
    
    local rootPart = character:WaitForChild("HumanoidRootPart", 5)
    local humanoid = character:WaitForChild("Humanoid", 5)
    
    if rootPart and humanoid and humanoid.Health > 0 then
        rootPart.CanCollide = false 
        rootPart.Size = TAMANHO_HITBOX
        rootPart.Transparency = TRANSPARENCIA
        rootPart.Color = COR_AZUL_FORTE -- Usando Color3 puro
        rootPart.Material = Enum.Material.Neon -- O material Neon deixa a cor brilhante e super nítida
    end
end

local function monitorarJogador(player)
    if player == LocalPlayer then return end

    if player.Character then
        criarQuadradoAzul(player.Character)
    end

    player.CharacterAdded:Connect(function(char)
        task.wait(0.5) 
        criarQuadradoAzul(char)
    end)
end

-- ========================================== --
-- 3. INICIALIZAÇÃO
-- ========================================== --

-- Cria o letreiro azul na tela
criarWatermark()

-- Imprime no console do desenvolvedor (F9) em azul também
print('<font color="rgb(0, 130, 255)"><b>[ Max hub (xerifes vs assassina) ] Carregado com sucesso!</b></font>')

-- Aplica a hitbox nos jogadores existentes
for _, player in ipairs(Players:GetPlayers()) do
    monitorarJogador(player)
end

-- Aplica a hitbox em quem entrar no servidor depois
Players.PlayerAdded:Connect(function(player)
    monitorarJogador(player)
end)
