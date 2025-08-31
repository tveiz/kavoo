local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "teste"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game:GetService("CoreGui")

local function UICorner(parent, radius)
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, radius)
    c.Parent = parent
end

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 600, 0, 400)
MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 50)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.ClipsDescendants = true
UICorner(MainFrame, 8)

local Shadow = Instance.new("Frame")
Shadow.Name = "Shadow"
Shadow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Shadow.Size = MainFrame.Size + UDim2.new(0, 10, 0, 10)
Shadow.Position = MainFrame.Position - UDim2.new(0, 5, 0, 5)
Shadow.BackgroundTransparency = 0.75
Shadow.BorderSizePixel = 0
Shadow.ZIndex = 0
Shadow.AnchorPoint = Vector2.new(0.5, 0.5)
Shadow.Parent = ScreenGui
UICorner(Shadow, 8)

MainFrame:GetPropertyChangedSignal("Position"):Connect(function()
    Shadow.Position = MainFrame.Position - UDim2.new(0,5,0,5)
end)
MainFrame:GetPropertyChangedSignal("Size"):Connect(function()
    Shadow.Size = MainFrame.Size + UDim2.new(0,10,0,10)
end)

local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 40)
TitleBar.BackgroundColor3 = Color3.fromRGB(45, 45, 80)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame
UICorner(TitleBar, 8)

local Title = Instance.new("TextLabel")
Title.Name = "Title"
Title.Text = "teste"
Title.Font = Enum.Font.GothamBold
Title.TextSize = 22
Title.TextColor3 = Color3.fromRGB(230, 230, 255)
Title.BackgroundTransparency = 1
Title.Size = UDim2.new(1, -100, 1, 0)
Title.Position = UDim2.new(0, 20, 0, 0)
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TitleBar

local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Size = UDim2.new(0, 35, 0, 30)
MinimizeButton.Position = UDim2.new(1, -80, 0, 5)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(60, 60, 100)
MinimizeButton.Text = "-"
MinimizeButton.TextColor3 = Color3.fromRGB(200, 200, 255)
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.TextSize = 25
MinimizeButton.AutoButtonColor = false
MinimizeButton.Parent = TitleBar
UICorner(MinimizeButton, 6)

MinimizeButton.MouseEnter:Connect(function()
    MinimizeButton.BackgroundColor3 = Color3.fromRGB(100, 100, 150)
end)
MinimizeButton.MouseLeave:Connect(function()
    MinimizeButton.BackgroundColor3 = Color3.fromRGB(60, 60, 100)
end)

local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 35, 0, 30)
CloseButton.Position = UDim2.new(1, -40, 0, 5)
CloseButton.BackgroundColor3 = Color3.fromRGB(200, 40, 60)
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 25
CloseButton.AutoButtonColor = false
CloseButton.Parent = TitleBar
UICorner(CloseButton, 6)

CloseButton.MouseEnter:Connect(function()
    CloseButton.BackgroundColor3 = Color3.fromRGB(255, 70, 90)
end)
CloseButton.MouseLeave:Connect(function()
    CloseButton.BackgroundColor3 = Color3.fromRGB(200, 40, 60)
end)

local Sidebar = Instance.new("Frame")
Sidebar.Name = "Sidebar"
Sidebar.BackgroundColor3 = Color3.fromRGB(38, 38, 68)
Sidebar.Size = UDim2.new(0, 140, 1, -40)
Sidebar.Position = UDim2.new(0, 0, 0, 40)
Sidebar.Parent = MainFrame
UICorner(Sidebar, 8)

local TabListLayout = Instance.new("UIListLayout")
TabListLayout.Padding = UDim.new(0, 10)
TabListLayout.SortOrder = Enum.SortOrder.LayoutOrder
TabListLayout.Parent = Sidebar

local ContentFrame = Instance.new("Frame")
ContentFrame.Name = "ContentFrame"
ContentFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 50)
ContentFrame.Size = UDim2.new(1, -140, 1, -40)
ContentFrame.Position = UDim2.new(0, 140, 0, 40)
ContentFrame.Parent = MainFrame
UICorner(ContentFrame, 8)

local tabs = {}
local contents = {}

local function createTab(name)
    local button = Instance.new("TextButton")
    button.Name = name.."Tab"
    button.Size = UDim2.new(1, -10, 0, 40)
    button.BackgroundColor3 = Color3.fromRGB(45, 45, 80)
    button.Text = name
    button.Font = Enum.Font.GothamSemibold
    button.TextSize = 18
    button.TextColor3 = Color3.fromRGB(190, 190, 255)
    button.AutoButtonColor = false
    button.Parent = Sidebar
    UICorner(button, 6)

    button.MouseEnter:Connect(function()
        if not button:GetAttribute("Selected") then
            button.BackgroundColor3 = Color3.fromRGB(70, 70, 120)
            button.TextColor3 = Color3.fromRGB(255, 255, 255)
        end
    end)
    button.MouseLeave:Connect(function()
        if not button:GetAttribute("Selected") then
            button.BackgroundColor3 = Color3.fromRGB(45, 45, 80)
            button.TextColor3 = Color3.fromRGB(190, 190, 255)
        end
    end)

    local content = Instance.new("Frame")
    content.Name = name.."Content"
    content.Size = UDim2.new(1, -20, 1, -20)
    content.Position = UDim2.new(0, 10, 0, 10)
    content.BackgroundTransparency = 1
    content.Visible = false
    content.Parent = ContentFrame

    local contentLayout = Instance.new("UIListLayout")
    contentLayout.SortOrder = Enum.SortOrder.LayoutOrder
    contentLayout.Padding = UDim.new(0, 8)
    contentLayout.Parent = content

    tabs[name] = button
    contents[name] = content

    button.MouseButton1Click:Connect(function()
        selectTab(name)
    end)

    return button, content
end

local currentTab = nil
function selectTab(name)
    if currentTab == name then return end
    for tabName, btn in pairs(tabs) do
        if tabName == name then
            btn.BackgroundColor3 = Color3.fromRGB(70, 70, 120)
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            btn:SetAttribute("Selected", true)
            contents[tabName].Visible = true
        else
            btn.BackgroundColor3 = Color3.fromRGB(45, 45, 80)
            btn.TextColor3 = Color3.fromRGB(190, 190, 255)
            btn:SetAttribute("Selected", false)
            contents[tabName].Visible = false
        end
    end
    currentTab = name
end

local function createToggle(parent, text, default, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 30)
    frame.BackgroundTransparency = 1
    frame.Parent = parent

    local label = Instance.new("TextLabel")
    label.Text = text
    label.Font = Enum.Font.GothamSemibold
    label.TextSize = 17
    label.TextColor3 = Color3.fromRGB(220, 220, 255)
    label.BackgroundTransparency = 1
    label.Size = UDim2.new(1, -50, 1, 0)
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = frame

    local toggleBtn = Instance.new("Frame")
    toggleBtn.Size = UDim2.new(0, 38, 0, 22)
    toggleBtn.Position = UDim2.new(1, -42, 0, 4)
    toggleBtn.BackgroundColor3 = default and Color3.fromRGB(0, 200, 130) or Color3.fromRGB(40, 40, 50)
    toggleBtn.Parent = frame
    UICorner(toggleBtn, 12)

    local circle = Instance.new("Frame")
    circle.Size = UDim2.new(0, 18, 0, 18)
    circle.Position = default and UDim2.new(1, -18, 0, 2) or UDim2.new(0, 2, 0, 2)
    circle.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
    circle.Parent = toggleBtn
    UICorner(circle, 9)

    local toggled = default
    local function updateToggle()
        if toggled then
            toggleBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 130)
            circle:TweenPosition(UDim2.new(1, -18, 0, 2), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.15, true)
        else
            toggleBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
            circle:TweenPosition(UDim2.new(0, 2, 0, 2), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.15, true)
        end
    end

    toggleBtn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            toggled = not toggled
            updateToggle()
            if callback then callback(toggled) end
        end
    end)

    updateToggle()

    return toggleBtn, function() return toggled end
end

local function createSlider(parent, text, min, max, default, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 40)
    frame.BackgroundTransparency = 1
    frame.Parent = parent

    local label = Instance.new("TextLabel")
    label.Text = text
    label.Font = Enum.Font.GothamSemibold
    label.TextSize = 17
    label.TextColor3 = Color3.fromRGB(220, 220, 255)
    label.BackgroundTransparency = 1
    label.Size = UDim2.new(1, -60, 0.5, 0)
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = frame

    local valueLabel = Instance.new("TextLabel")
    valueLabel.Text = tostring(default)
    valueLabel.Font = Enum.Font.GothamSemibold
    valueLabel.TextSize = 17
    valueLabel.TextColor3 = Color3.fromRGB(180, 180, 255)
    valueLabel.BackgroundTransparency = 1
    valueLabel.Size = UDim2.new(0, 50, 0.5, 0)
    valueLabel.Position = UDim2.new(1, -50, 0, 0)
    valueLabel.Parent = frame

    local sliderBar = Instance.new("Frame")
    sliderBar.BackgroundColor3 = Color3.fromRGB(60, 60, 90)
    sliderBar.Size = UDim2.new(1, -10, 0, 10)
    sliderBar.Position = UDim2.new(0, 5, 1, -20)
    sliderBar.Parent = frame
    UICorner(sliderBar, 6)

    local sliderFill = Instance.new("Frame")
    sliderFill.BackgroundColor3 = Color3.fromRGB(0, 170, 220)
    sliderFill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
    sliderFill.Parent = sliderBar
    UICorner(sliderFill, 6)

    local dragging = false

    sliderBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            local mouse = game.Players.LocalPlayer:GetMouse()
            local relativeX = math.clamp(mouse.X - sliderBar.AbsolutePosition.X, 0, sliderBar.AbsoluteSize.X)
            local percent = relativeX / sliderBar.AbsoluteSize.X
            local value = math.floor(min + (max - min) * percent)
            sliderFill.Size = UDim2.new(percent, 0, 1, 0)
            valueLabel.Text = tostring(value)
            if callback then callback(value) end
        end
    end)

    sliderBar.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)

    sliderBar.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local mouse = game.Players.LocalPlayer:GetMouse()
            local relativeX = math.clamp(mouse.X - sliderBar.AbsolutePosition.X, 0, sliderBar.AbsoluteSize.X)
            local percent = relativeX / sliderBar.AbsoluteSize.X
            local value = math.floor(min + (max - min) * percent)
            sliderFill.Size = UDim2.new(percent, 0, 1, 0)
            valueLabel.Text = tostring(value)
            if callback then callback(value) end
        end
    end)

    return frame
end

local function createButton(parent, text, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(50, 50, 80)
    btn.TextColor3 = Color3.fromRGB(220, 220, 255)
    btn.Text = text
    btn.Font = Enum.Font.GothamSemibold
    btn.TextSize = 18
    btn.AutoButtonColor = false
    btn.Parent = parent
    UICorner(btn, 8)

    btn.MouseEnter:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(70, 70, 120)
    end)
    btn.MouseLeave:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(50, 50, 80)
    end)

    btn.MouseButton1Click:Connect(function()
        if callback then callback() end
    end)

    return btn
end

-- Variáveis de estado dos sistemas
local state = {
    aimbot = false,
    triggerbot = false,
    silentaim = false,
    espPlayers = false,
    espItems = false,
    chams = false,
    fly = false,
    noclip = false,
    fov = 70,
    speed = 16,
}

local tabCombat, contentCombat = createTab("Combat")
local tabVisuals, contentVisuals = createTab("Visuals")
local tabUtility, contentUtility = createTab("Utility")
local tabSettings, contentSettings = createTab("Settings")

selectTab("Combat")


-- Variável para controlar o toggle
local aimbotEnabled = false
local FOV = 150 -- Ajuste o FOV que você quiser (menor é mais preciso, maior é mais amplo)

-- Reutilizando variáveis do seu script
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Camera = workspace.CurrentCamera
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local Target = nil

-- Função para achar o alvo mais próximo do mouse dentro do FOV
local function GetClosestTarget()
    local closestPlayer = nil
    local shortestDistance = FOV

    local mousePos = UserInputService:GetMouseLocation()

    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") and player.Character:FindFirstChild("Humanoid") then
            local humanoid = player.Character.Humanoid
            if humanoid.Health > 0 then
                local Head = player.Character.Head
                local screenPos, onScreen = Camera:WorldToViewportPoint(Head.Position)

                if onScreen then
                    local dist = (Vector2.new(screenPos.X, screenPos.Y) - mousePos).Magnitude
                    if dist < shortestDistance then
                        shortestDistance = dist
                        closestPlayer = player
                    end
                end
            end
        end
    end

    return closestPlayer
end

-- Conectando o toggle na tab Combat
local toggleAimbot, getAimbotState = createToggle(contentCombat, "Aimbot FOV", false, function(state)
    aimbotEnabled = state
    if not state then
        Target = nil
    end
end)

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Variável de controle
local autoQuitEnabled = false
local connection

-- Função para monitorar a vida
local function monitorarVida(char)
    local humanoid = char:WaitForChild("Humanoid")

    -- Garante que não tenha conexão duplicada
    if connection then
        connection:Disconnect()
    end

    connection = humanoid.HealthChanged:Connect(function(vida)
        if autoQuitEnabled and vida <= 2 then
            LocalPlayer:Kick("Você foi kikado automaticamente por chegar em 2 de vida!")
        end
    end)
end

-- Garantir que funcione após respawn
LocalPlayer.CharacterAdded:Connect(monitorarVida)
if LocalPlayer.Character then
    monitorarVida(LocalPlayer.Character)
end

-- Criando o toggle (no mesmo estilo do seu exemplo)
local toggleAutoQuit, getAutoQuitState = createToggle(contentCombat, "Auto Quit (HP ≤ 2)", false, function(state)
    autoQuitEnabled = state
end)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Backpack = LocalPlayer:WaitForChild("Backpack")
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

local hiddenGui
local container
local toolButtons = {}

local function safe(func)
    local ok, result = pcall(func)
    if not ok then
        -- ignorar erros silenciosamente
    end
end

local function atualizarInventario()
    local tools = {}

    for _, obj in ipairs(Backpack:GetChildren()) do
        if obj:IsA("Tool") then
            table.insert(tools, obj)
        end
    end

    for _, obj in ipairs(Character:GetChildren()) do
        if obj:IsA("Tool") then
            table.insert(tools, obj)
        end
    end

    for _, tool in ipairs(tools) do
        if not toolButtons[tool] then
            safe(function()
                local btn = Instance.new("ImageButton")
                btn.Size = UDim2.new(0, 55, 0, 55)
                btn.BackgroundTransparency = 0.3
                btn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
                btn.BorderSizePixel = 0
                btn.Image = tool.TextureId ~= "" and tool.TextureId or "rbxasset://Textures/Tool.png"
                btn.Name = "Slot_" .. math.random(10000,99999)
                btn.Parent = container

                local label = Instance.new("TextLabel")
                label.Size = UDim2.new(1, 0, 0.3, 0)
                label.Position = UDim2.new(0, 0, 0.7, 0)
                label.BackgroundTransparency = 1
                label.Text = tool.Name
                label.TextColor3 = Color3.new(1, 1, 1)
                label.TextScaled = true
                label.Font = Enum.Font.Gotham
                label.Parent = btn

                btn.MouseButton1Click:Connect(function()
                    safe(function()
                        if tool.Parent == Backpack then
                            tool.Parent = Character
                        elseif tool.Parent == Character then
                            tool.Parent = Backpack
                        end
                    end)
                end)

                tool.AncestryChanged:Connect(function()
                    safe(function()
                        if not (tool:IsDescendantOf(Backpack) or tool:IsDescendantOf(Character)) then
                            if toolButtons[tool] then
                                toolButtons[tool]:Destroy()
                                toolButtons[tool] = nil
                            end
                        end
                    end)
                end)

                toolButtons[tool] = btn
            end)
        end
    end
end

local function criarUI()
    -- Cria UI protegida
    hiddenGui = Instance.new("ScreenGui")
    hiddenGui.Name = "UI_"..tostring(math.random(100000,999999))
    hiddenGui.DisplayOrder = math.random(1000,9999)
    hiddenGui.ResetOnSpawn = false
    hiddenGui.IgnoreGuiInset = true
    hiddenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    hiddenGui.Parent = (LocalPlayer:FindFirstChildOfClass("PlayerGui") or LocalPlayer:WaitForChild("PlayerGui"))

    -- Container centralizado e leve
    container = Instance.new("Frame")
    container.Size = UDim2.new(0, 280, 0, 60)
    container.Position = UDim2.new(0.5, -140, 0.88, 0)
    container.BackgroundTransparency = 1
    container.Name = "Container_"..math.random(100000,999999)
    container.Parent = hiddenGui

    local layout = Instance.new("UIGridLayout")
    layout.CellSize = UDim2.new(0, 55, 0, 55)
    layout.CellPadding = UDim2.new(0, 3, 0, 3)
    layout.FillDirectionMaxCells = 6
    layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    layout.Parent = container

    safe(function()
        Backpack.ChildAdded:Connect(atualizarInventario)
        Backpack.ChildRemoved:Connect(atualizarInventario)
        Character.ChildAdded:Connect(atualizarInventario)
        Character.ChildRemoved:Connect(atualizarInventario)
    end)

    safe(atualizarInventario)
end

local function destruirUI()
    if hiddenGui then
        hiddenGui:Destroy()
        hiddenGui = nil
        container = nil
        toolButtons = {}
    end
end

-- Toggle na aba Combat
createToggle(contentCombat, "UI Inventário Ferramentas", false, function(state)
    if state then
        criarUI()
    else
        destruirUI()
    end
end)


-- Variável para saber se botão direito está pressionado
local RightMouseDown = false

UserInputService.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton2 then
        RightMouseDown = true
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton2 then
        RightMouseDown = false
        Target = nil
    end
end)

RunService.RenderStepped:Connect(function()
    if aimbotEnabled and RightMouseDown then
        if not Target or not Target.Character or not Target.Character:FindFirstChild("Head") or Target.Character.Humanoid.Health <= 0 then
            Target = GetClosestTarget()
        else
            local headPos, onScreen = Camera:WorldToViewportPoint(Target.Character.Head.Position)
            local mousePos = UserInputService:GetMouseLocation()
            local dist = (Vector2.new(headPos.X, headPos.Y) - mousePos).Magnitude

            if dist > FOV then
                Target = GetClosestTarget()
            end
        end

        if Target and Target.Character and Target.Character:FindFirstChild("Head") then
            local headPos = Target.Character.Head.Position
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, headPos)
        end
    end
end)

local RunService = game:GetService("RunService")
local camera = workspace.CurrentCamera

-- Criar o círculo do FOV usando Drawing API
local fovCircle = Drawing.new("Circle")
fovCircle.Visible = false -- começa invisível
fovCircle.Color = Color3.fromRGB(255, 0, 0)
fovCircle.Thickness = 2
fovCircle.NumSides = 64
fovCircle.Radius = 100
fovCircle.Filled = false

-- Toggle para mostrar/esconder o círculo de FOV
local toggleFOVCircle, getFOVCircleState = createToggle(contentCombat, "Mostrar Círculo FOV", false, function(state)
    fovCircle.Visible = state
end)

-- Atualiza a posição do círculo a cada frame, só se estiver visível
RunService.RenderStepped:Connect(function()
    if fovCircle.Visible then
        local screenSize = camera.ViewportSize
        fovCircle.Position = Vector2.new(screenSize.X / 2, screenSize.Y / 2)
    end
end)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local HitboxPart = "Head"
local HitboxSize = Vector3.new(7, 7, 7)
local Transparency = 0.5
local Color = Color3.fromRGB(255, 0, 0)

local hitboxConnection
local hitboxAtivo = false

createToggle(contentCombat, "Hitbox Expander", false, function(state)
    hitboxAtivo = state
    if state then
        hitboxConnection = RunService.RenderStepped:Connect(function()
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= Players.LocalPlayer and player.Character then
                    local part = player.Character:FindFirstChild(HitboxPart)
                    if part and part:IsA("BasePart") then
                        part.Size = HitboxSize
                        part.Transparency = Transparency
                        part.Color = Color
                        part.Material = Enum.Material.ForceField
                        part.CanCollide = false
                    end
                end
            end
        end)
    else
        if hitboxConnection then
            hitboxConnection:Disconnect()
            hitboxConnection = nil
        end
        -- Resetar os hitboxes
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= Players.LocalPlayer and player.Character then
                local part = player.Character:FindFirstChild(HitboxPart)
                if part and part:IsA("BasePart") then
                    part.Size = Vector3.new(2, 1, 1)
                    part.Transparency = 0
                    part.Color = Color3.fromRGB(255, 255, 255)
                    part.Material = Enum.Material.Plastic
                    part.CanCollide = true
                end
            end
        end
    end
end)


-- Serviços
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer

-- ⬇️ BOTÃO NA ABA UTILITY (substitua 'contentUtility' se necessário)
local teleportGuiCreated = false
local screenGui -- armazenará o GUI para poder ativar/desativar

createButton(contentUtility, "Teleportador", function()
    if not teleportGuiCreated then
        -- Criar ScreenGui
        screenGui = Instance.new("ScreenGui")
        screenGui.Name = "TeleportUI"
        screenGui.ResetOnSpawn = false
        screenGui.Parent = game:GetService("CoreGui")
        screenGui.Enabled = true

        -- Frame principal
        local Frame = Instance.new("Frame")
        Frame.Size = UDim2.new(0, 300, 0, 150)
        Frame.Position = UDim2.new(0.5, -150, 0.5, -75)
        Frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
        Frame.BorderSizePixel = 0
        Frame.AnchorPoint = Vector2.new(0.5, 0.5)
        Frame.Parent = screenGui
        Frame.Visible = true

        -- UICorner
        local corner = Instance.new("UICorner", Frame)
        corner.CornerRadius = UDim.new(0, 10)

        -- Título
        local title = Instance.new("TextLabel")
        title.Size = UDim2.new(1, 0, 0, 30)
        title.BackgroundTransparency = 1
        title.Text = "Teleportador"
        title.TextColor3 = Color3.new(1, 1, 1)
        title.Font = Enum.Font.GothamBold
        title.TextSize = 20
        title.Parent = Frame

        -- Botão fechar
        local closeButton = Instance.new("TextButton")
        closeButton.Size = UDim2.new(0, 30, 0, 30)
        closeButton.Position = UDim2.new(1, -35, 0, 0)
        closeButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
        closeButton.Text = "X"
        closeButton.TextColor3 = Color3.new(1, 1, 1)
        closeButton.Font = Enum.Font.GothamBold
        closeButton.TextSize = 20
        closeButton.Parent = Frame

        closeButton.MouseButton1Click:Connect(function()
            screenGui.Enabled = false
        end)

        -- TextBox
        local TextBox = Instance.new("TextBox")
        TextBox.Size = UDim2.new(0.9, 0, 0, 40)
        TextBox.Position = UDim2.new(0.05, 0, 0, 50)
        TextBox.PlaceholderText = "Digite o nome ou display name"
        TextBox.ClearTextOnFocus = false
        TextBox.TextColor3 = Color3.new(1, 1, 1)
        TextBox.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        TextBox.Font = Enum.Font.Gotham
        TextBox.TextSize = 18
        TextBox.Parent = Frame

        local corner2 = Instance.new("UICorner", TextBox)
        corner2.CornerRadius = UDim.new(0, 8)

        -- Botão Teleportar
        local teleportButton = Instance.new("TextButton")
        teleportButton.Size = UDim2.new(0.5, 0, 0, 40)
        teleportButton.Position = UDim2.new(0.25, 0, 0, 100)
        teleportButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
        teleportButton.Text = "Teleportar"
        teleportButton.TextColor3 = Color3.new(1, 1, 1)
        teleportButton.Font = Enum.Font.GothamBold
        teleportButton.TextSize = 20
        teleportButton.Parent = Frame

        local corner3 = Instance.new("UICorner", teleportButton)
        corner3.CornerRadius = UDim.new(0, 8)

        -- Função de teleport
        local function teleportToPlayer(nameOrDisplay)
            local targetPlayer = nil
            local nameLower = nameOrDisplay:lower()

            for _, plr in pairs(Players:GetPlayers()) do
                if plr.Name:lower() == nameLower or (plr.DisplayName and plr.DisplayName:lower() == nameLower) then
                    targetPlayer = plr
                    break
                end
            end

            if not targetPlayer then
                warn("[TeleportUI] Jogador não encontrado: " .. nameOrDisplay)
                return false, "Jogador não encontrado"
            end

            if not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                warn("[TeleportUI] Personagem inválido do jogador: " .. targetPlayer.Name)
                return false, "Personagem inválido"
            end

            local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                local targetPos = targetPlayer.Character.HumanoidRootPart.Position

                local success, err = pcall(function()
                    hrp.CFrame = CFrame.new(targetPos + Vector3.new(0, 3, 0))
                end)

                if success then
                    return true, "Teleportado com sucesso para " .. targetPlayer.Name
                else
                    warn("[TeleportUI] Erro ao teleportar: " .. tostring(err))
                    return false, "Erro ao teleportar"
                end
            else
                return false, "Seu personagem não está pronto"
            end
        end

        teleportButton.MouseButton1Click:Connect(function()
            local text = TextBox.Text
            if text == "" or not text then return end

            local success, msg = teleportToPlayer(text)
            if success then
                print("[TeleportUI] " .. msg)
            else
                warn("[TeleportUI] " .. msg)
            end
        end)

        -- Drag
        local dragging, dragInput, dragStart, startPos

        local function update(input)
            local delta = input.Position - dragStart
            Frame.Position = UDim2.new(
                startPos.X.Scale,
                startPos.X.Offset + delta.X,
                startPos.Y.Scale,
                startPos.Y.Offset + delta.Y
            )
        end

        Frame.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = true
                dragStart = input.Position
                startPos = Frame.Position

                input.Changed:Connect(function()
                    if input.UserInputState == Enum.UserInputState.End then
                        dragging = false
                    end
                end)
            end
        end)

        Frame.InputChanged:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseMovement then
                dragInput = input
            end
        end)

        UserInputService.InputChanged:Connect(function(input)
            if input == dragInput and dragging then
                update(input)
            end
        end)

        teleportGuiCreated = true
    else
        screenGui.Enabled = not screenGui.Enabled
    end
end)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

-- Variáveis de controle
local connection
local Boxes = {}
local ESPFolder

-- Função para criar linha
local function CreateLine()
    local line = Drawing.new("Line")
    line.Thickness = 1
    line.Color = Color3.new(1, 1, 1)
    line.Transparency = 1
    return line
end

-- Função para iniciar ESP
local function startESP()
    -- Limpar anterior
    if ESPFolder then
        ESPFolder:Destroy()
    end

    ESPFolder = Instance.new("Folder", game:GetService("CoreGui"))
    ESPFolder.Name = "ESP3DBox"
    Boxes = {}

    connection = RunService.RenderStepped:Connect(function()
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local hrp = player.Character.HumanoidRootPart
                local size = Vector3.new(4, 7, 4)
                local corners = {
                    Vector3.new(-size.X, -size.Y, -size.Z),
                    Vector3.new(size.X, -size.Y, -size.Z),
                    Vector3.new(size.X, -size.Y, size.Z),
                    Vector3.new(-size.X, -size.Y, size.Z),
                    Vector3.new(-size.X, size.Y, -size.Z),
                    Vector3.new(size.X, size.Y, -size.Z),
                    Vector3.new(size.X, size.Y, size.Z),
                    Vector3.new(-size.X, size.Y, size.Z)
                }

                if not Boxes[player] then
                    Boxes[player] = {}
                    for i = 1, 12 do
                        table.insert(Boxes[player], CreateLine())
                    end
                end

                local screenPoints = {}
                local onScreen = true
                for i, offset in ipairs(corners) do
                    local worldPoint = hrp.Position + (hrp.CFrame:VectorToWorldSpace(offset / 2))
                    local screenPos, visible = Camera:WorldToViewportPoint(worldPoint)
                    if not visible then
                        onScreen = false
                        break
                    end
                    screenPoints[i] = screenPos
                end

                if onScreen then
                    local lines = Boxes[player]
                    local edges = {
                        {1,2}, {2,3}, {3,4}, {4,1}, -- base
                        {5,6}, {6,7}, {7,8}, {8,5}, -- topo
                        {1,5}, {2,6}, {3,7}, {4,8}  -- laterais
                    }

                    for i, edge in ipairs(edges) do
                        lines[i].From = Vector2.new(screenPoints[edge[1]].X, screenPoints[edge[1]].Y)
                        lines[i].To = Vector2.new(screenPoints[edge[2]].X, screenPoints[edge[2]].Y)
                        lines[i].Visible = true
                    end
                else
                    for _, line in ipairs(Boxes[player]) do
                        line.Visible = false
                    end
                end
            elseif Boxes[player] then
                for _, line in ipairs(Boxes[player]) do
                    line.Visible = false
                end
            end
        end
    end)

    -- Remove linhas ao player sair
    Players.PlayerRemoving:Connect(function(plr)
        if Boxes[plr] then
            for _, line in ipairs(Boxes[plr]) do
                line:Remove()
            end
            Boxes[plr] = nil
        end
    end)
end

-- Função para parar ESP
local function stopESP()
    if connection then
        connection:Disconnect()
        connection = nil
    end

    if Boxes then
        for _, lines in pairs(Boxes) do
            for _, line in ipairs(lines) do
                line:Remove()
            end
        end
        Boxes = {}
    end

    if ESPFolder then
        ESPFolder:Destroy()
        ESPFolder = nil
    end
end

-- ✅ TOGGLE NA ABA VISUALS
createToggle(contentVisuals, "3D Box ESP", false, function(state)
    if state then
        startESP()
    else
        stopESP()
    end
end)

-- Função para abrir baú
local function abrirBau()
    if antiLagAtivo then return end
    local args = {"trasnferebau", "", "", 1}
    game:GetService("ReplicatedStorage").Modules.InvRemotes.InvRequest:InvokeServer(unpack(args))
end

-- Botão na aba Utility
createButton(contentUtility, "Abrir Baú", function()
    abrirBau()
end)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

local deadESP = {}
local deadESPEnabled = false

local function clearDeadESP()
    for _, v in pairs(deadESP) do
        v:Remove()
    end
    deadESP = {}
end

local function createDeadESP(screenPos)
    local text = Drawing.new("Text")
    text.Text = "MORTO"
    text.Position = screenPos
    text.Size = 14
    text.Center = true
    text.Outline = true
    text.OutlineColor = Color3.new(0, 0, 0)
    text.Color = Color3.fromRGB(255, 0, 0)
    text.Transparency = 1
    text.Visible = true
    table.insert(deadESP, text)
end

local function UpdateESP()
    clearDeadESP()
    if not deadESPEnabled then return end

    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")

            if humanoid and hrp and humanoid.Health <= 0 then
                local screenPos, onScreen = Camera:WorldToViewportPoint(hrp.Position + Vector3.new(0, 3, 0))
                if onScreen then
                    createDeadESP(Vector2.new(screenPos.X, screenPos.Y))
                end
            end
        end
    end
end

RunService.RenderStepped:Connect(UpdateESP)

-- Supondo que 'contentVisuals' é o container da aba Visuals
createToggle(contentVisuals, "ESP Mortos", false, function(state)
    deadESPEnabled = state
    if not state then
        clearDeadESP()
    end
end)

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

local espEnabled = false

local function createHighlight(player)
	if player ~= LocalPlayer and player.Character and not player.Character:FindFirstChild("ESP_Highlight") then
		local highlight = Instance.new("Highlight")
		highlight.Name = "ESP_Highlight"
		highlight.FillColor = Color3.fromRGB(255, 0, 0)
		highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
		highlight.FillTransparency = 0.5
		highlight.OutlineTransparency = 0
		highlight.Adornee = player.Character
		highlight.Parent = player.Character
	end
end

local function removeHighlight(player)
	if player.Character then
		local highlight = player.Character:FindFirstChild("ESP_Highlight")
		if highlight then
			highlight:Destroy()
		end
	end
end

local function updateESP()
	if not espEnabled then return end
	for _, player in pairs(Players:GetPlayers()) do
		if player ~= LocalPlayer then
			createHighlight(player)
		end
	end
end

RunService.RenderStepped:Connect(function()
	if espEnabled then
		updateESP()
	else
		-- Remove todos highlights caso esteja desligado
		for _, player in pairs(Players:GetPlayers()) do
			removeHighlight(player)
		end
	end
end)

-- Supondo que contentVisuals seja o container da aba Visuals da sua UI
createToggle(contentVisuals, "ESP Highlight", false, function(state)
	espEnabled = state
	if not state then
		-- Limpa highlights se desligar
		for _, player in pairs(Players:GetPlayers()) do
			removeHighlight(player)
		end
	end
end)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

local distances = {}
local espDistEnabled = false

local function clearDistances()
	for _, text in pairs(distances) do
		text:Remove()
	end
	distances = {}
end

local function drawDistance(pos, distance)
	local text = Drawing.new("Text")
	text.Position = pos
	text.Text = tostring(math.floor(distance)) .. "m"
	text.Size = 14
	text.Center = true
	text.Outline = true
	text.OutlineColor = Color3.new(0, 0, 0)
	text.Color = Color3.fromRGB(255, 255, 255)
	text.Visible = true
	table.insert(distances, text)
end

local function updateDistanceESP()
	clearDistances()
	if not espDistEnabled then return end

	for _, player in ipairs(Players:GetPlayers()) do
		if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
			local hrp = player.Character.HumanoidRootPart
			local screenPos, onScreen = Camera:WorldToViewportPoint(hrp.Position)

			if onScreen and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
				local dist = (LocalPlayer.Character.HumanoidRootPart.Position - hrp.Position).Magnitude
				drawDistance(Vector2.new(screenPos.X, screenPos.Y - 20), dist)
			end
		end
	end
end

RunService.RenderStepped:Connect(updateDistanceESP)

-- Integração com seu menu
createToggle(contentVisuals, "ESP Distância", false, function(state)
	espDistEnabled = state
	if not state then
		clearDistances()
	end
end)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local localPlayer = Players.LocalPlayer

local lines = {}
local espLineEnabled = false

local function clearLines()
    for _, line in pairs(lines) do
        line:Remove()
    end
    lines = {}
end

local function createLine(fromPos, toPos, color)
    local line = Drawing.new("Line")
    line.From = fromPos
    line.To = toPos
    line.Color = color or Color3.fromRGB(255, 0, 0)
    line.Thickness = 1.5
    line.Transparency = 1
    line.Visible = true
    table.insert(lines, line)
end

local function updateLines()
    clearLines()
    if not espLineEnabled then return end

    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= localPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = player.Character.HumanoidRootPart
            local pos, onScreen = Camera:WorldToViewportPoint(hrp.Position)

            if onScreen then
                local screenCenter = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                createLine(screenCenter, Vector2.new(pos.X, pos.Y), Color3.fromRGB(0, 255, 0))
            end
        end
    end
end

RunService.RenderStepped:Connect(updateLines)

-- Integração com seu menu
createToggle(contentVisuals, "ESP Line", false, function(state)
    espLineEnabled = state
    if not state then
        clearLines()
    end
end)

-- Variáveis do sistema
local revistarCoroutine
local revistarToggle = false
local detectados = {}

-- Função para iniciar o revistar automático
local function iniciarRevistar()
    if revistarCoroutine then return end
    revistarToggle = true
    revistarCoroutine = coroutine.create(function()
        while revistarToggle do
            local char = LocalPlayer.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                local pos = char.HumanoidRootPart.Position
                for _, otherPlayer in pairs(Players:GetPlayers()) do
                    if (otherPlayer ~= LocalPlayer and isDead(otherPlayer) 
                        and otherPlayer.Character and otherPlayer.Character:FindFirstChild("HumanoidRootPart")) then
                        local otherPos = otherPlayer.Character.HumanoidRootPart.Position
                        if (distanceBetween(pos, otherPos) <= DETECTION_RADIUS) then
                            if not detectados[otherPlayer] then
                                detectados[otherPlayer] = true
                                if dsada then
                                    pcall(function()
                                        dsada:FireServer("/revistar morto")
                                    end)
                                end
                            end
                        else
                            detectados[otherPlayer] = nil
                        end
                    end
                end
            end
            wait(CheckInterval)
        end
    end)
    coroutine.resume(revistarCoroutine)
    notificar("Revistar Automático", "Ativado!", 2)
end

-- Função para parar o revistar automático
local function pararRevistar()
    revistarToggle = false
    detectados = {}
    if revistarCoroutine then
        coroutine.close(revistarCoroutine)
        revistarCoroutine = nil
    end
    notificar("Revistar Automático", "Desativado!", 2)
end

-- Toggle do revistar automático
local function toggleRevistar(state)
    if state then
        iniciarRevistar()
    else
        pararRevistar()
    end
end

-- Integração com o menu (aba Combat)
createToggle(contentCombat, "Revistar Automático", false, toggleRevistar)

-- Variáveis do sistema
local ultimoMortoPorVoce = nil
local teleportou = false
local teleportAutoAtivo = false -- Começa desativado, controlado pelo toggle

-- Função para teleportar para o último morto
local function teleportarParaUltimoMorto()
    if not teleportAutoAtivo then return end
    if not ultimoMortoPorVoce then return end
    if teleportou then return end

    local char = LocalPlayer.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return end
    if not ultimoMortoPorVoce.Character or not ultimoMortoPorVoce.Character:FindFirstChild("HumanoidRootPart") then
        ultimoMortoPorVoce = nil
        return
    end

    char.HumanoidRootPart.CFrame = ultimoMortoPorVoce.Character.HumanoidRootPart.CFrame + Vector3.new(0,3,0)
    teleportou = true
    notificar("Teleporte p/ Kill","Teleportado para o último morto!",2)
end

-- Monitorar morte dos jogadores
local function monitorarMorteJogador(p)
    if p == LocalPlayer then return end
    local function conectarMorte(char)
        local humanoid = char:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.Died:Connect(function()
                local creatorTag = humanoid:FindFirstChild("creator") or humanoid:FindFirstChild("Creator")
                if creatorTag and (creatorTag.Value == LocalPlayer) then
                    ultimoMortoPorVoce = p
                    teleportou = false
                    teleportarParaUltimoMorto()
                end
            end)
        end
    end
    p.CharacterAdded:Connect(conectarMorte)
    if p.Character then conectarMorte(p.Character) end
end

Players.PlayerAdded:Connect(monitorarMorteJogador)
for _, p in pairs(Players:GetPlayers()) do monitorarMorteJogador(p) end

-- Toggle do teleport automático (aba Combat)
local function toggleTeleport(state)
    teleportAutoAtivo = state
    if state then
        notificar("Teleporte p/ Kill","Ativado!",2)
    else
        notificar("Teleporte p/ Kill","Desativado!",2)
    end
end

createToggle(contentCombat, "Teleporte p/ Último Kill", false, toggleTeleport)

-- Variáveis do sistema
local espTags = {}
local espEnabled = false
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Função para pegar itens equipados
local function getEquippedItems(player)
    local equipped = {}
    local backpack = player:FindFirstChild("Backpack")
    if backpack then
        for _, item in pairs(backpack:GetChildren()) do
            if #equipped >= 3 then break end
            if item:IsA("Tool") then
                table.insert(equipped, item.Name)
            end
        end
    end
    if player.Character then
        for _, item in pairs(player.Character:GetChildren()) do
            if #equipped >= 3 then break end
            if item:IsA("Tool") then
                local alreadyHave = false
                for _, n in pairs(equipped) do
                    if n == item.Name then
                        alreadyHave = true
                        break
                    end
                end
                if not alreadyHave then
                    table.insert(equipped, item.Name)
                end
            end
        end
    end
    while #equipped < 3 do
        table.insert(equipped, "")
    end
    if #equipped == 0 then
        equipped = {"Sem inventário","",""}
    end
    return equipped
end

-- Função para ativar/desativar ESP
local function toggleESP(state)
    espEnabled = state
    if espEnabled then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("Head") then
                if not espTags[p.UserId] then
                    local head = p.Character.Head
                    local billboard = Instance.new("BillboardGui", head)
                    billboard.Name = "ESP_Tag"
                    billboard.Size = UDim2.new(0,180,0,80)
                    billboard.StudsOffset = Vector3.new(0,2,0)
                    billboard.AlwaysOnTop = true

                    local label = Instance.new("TextLabel", billboard)
                    label.Size = UDim2.new(1,0,0,24)
                    label.Position = UDim2.new(0,0,0,0)
                    label.BackgroundTransparency = 1
                    label.TextColor3 = Color3.fromRGB(0,170,255)
                    label.Font = Enum.Font.GothamBold
                    label.TextStrokeTransparency = 0
                    label.TextSize = 16

                    local itemsLabels = {}
                    for i = 1, 3 do
                        local itemLabel = Instance.new("TextLabel", billboard)
                        itemLabel.Size = UDim2.new(1,0,0,18)
                        itemLabel.Position = UDim2.new(0,0,0,24 + ((i-1) * 18))
                        itemLabel.BackgroundColor3 = Color3.new(0,0,0)
                        itemLabel.BackgroundTransparency = 0.7
                        itemLabel.TextColor3 = Color3.new(1,1,1)
                        itemLabel.Font = Enum.Font.GothamBold
                        itemLabel.TextStrokeTransparency = 0
                        itemLabel.TextSize = 14
                        itemLabel.Text = ""
                        table.insert(itemsLabels, itemLabel)
                    end

                    espTags[p.UserId] = {billboard=billboard, label=label, itemsLabels=itemsLabels}

                    RunService:BindToRenderStep("ESP_Update_"..p.UserId, 500, function()
                        if p.Character and p.Character:FindFirstChild("Head") and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                            local pos = p.Character.Head.Position
                            local dist = (pos - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                            local equipped = getEquippedItems(p)
                            label.Text = p.Name .. " | " .. math.floor(dist) .. "m"
                            for i = 1, 3 do
                                itemsLabels[i].Text = (equipped[i] ~= "" and equipped[i]) or ""
                                itemsLabels[i].Visible = equipped[i] ~= ""
                            end
                        else
                            RunService:UnbindFromRenderStep("ESP_Update_"..p.UserId)
                            if espTags[p.UserId] and espTags[p.UserId].billboard then
                                espTags[p.UserId].billboard:Destroy()
                            end
                            espTags[p.UserId] = nil
                        end
                    end)
                end
            end
        end
    else
        for userId, info in pairs(espTags) do
            RunService:UnbindFromRenderStep("ESP_Update_"..userId)
            if info.billboard then
                info.billboard:Destroy()
            end
            espTags[userId] = nil
        end
    end
end

-- Integração com o menu (aba Visuals)
createToggle(contentVisuals, "ESP + Itens", false, toggleESP)

--// Serviços
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer
local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")

-- Variáveis do Fly
local flying = false
local flySpeed = 50
local moveVec = Vector3.zero

-- Função para ativar/desativar Fly
local function toggleFly(state)
    flying = state
    if not flying and hrp then
        hrp.Velocity = Vector3.zero
    end
    notificar("Fly", flying and "Ativado!" or "Desativado!", 2)
end

-- Função para aumentar/diminuir velocidade
local function setFlySpeed(amount)
    flySpeed = math.max(10, flySpeed + amount)
end

-- Input para movimento
UIS.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.W then moveVec = Vector3.new(0,0,-1) end
    if input.KeyCode == Enum.KeyCode.S then moveVec = Vector3.new(0,0,1) end
    if input.KeyCode == Enum.KeyCode.A then moveVec = Vector3.new(-1,0,0) end
    if input.KeyCode == Enum.KeyCode.D then moveVec = Vector3.new(1,0,0) end
    if input.KeyCode == Enum.KeyCode.Space then moveVec = Vector3.new(0,1,0) end
    if input.KeyCode == Enum.KeyCode.LeftShift then moveVec = Vector3.new(0,-1,0) end
end)

UIS.InputEnded:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.W or input.KeyCode == Enum.KeyCode.S or
       input.KeyCode == Enum.KeyCode.A or input.KeyCode == Enum.KeyCode.D or
       input.KeyCode == Enum.KeyCode.Space or input.KeyCode == Enum.KeyCode.LeftShift then
        moveVec = Vector3.zero
    end
end)

-- Loop do Fly
RS.RenderStepped:Connect(function(dt)
    if flying and hrp then
        local camCF = workspace.CurrentCamera.CFrame
        local dir = camCF:VectorToWorldSpace(moveVec)
        hrp.Velocity = dir * flySpeed
    end
end)

-- Integração com menu (aba Combat)
createToggle(contentCombat, "Fly", false, toggleFly)

-- Exemplos de botões de velocidade (podem ser integrados no menu de config)
-- createButton(contentCombat, "+ Velocidade Fly", function() setFlySpeed(10) end)
-- createButton(contentCombat, "- Velocidade Fly", function() setFlySpeed(-10) end)


selectTab("Settings")

local info = Instance.new("TextLabel")
info.Text = "Clique em 'Escolher tecla' e pressione a tecla desejada para minimizar/restaurar o menu"
info.Font = Enum.Font.Gotham
info.TextSize = 15
info.TextColor3 = Color3.fromRGB(180, 180, 200)
info.BackgroundTransparency = 1
info.Size = UDim2.new(1, -20, 0, 50)
info.Position = UDim2.new(0, 10, 0, 10)
info.TextWrapped = true
info.Parent = contentSettings

local chooseKeyButton = createButton(contentSettings, "Escolher tecla", nil)
chooseKeyButton.Size = UDim2.new(0, 130, 0, 30)
chooseKeyButton.Position = UDim2.new(0, 10, 0, 70)

local selectedKey = Enum.KeyCode.K
chooseKeyButton.Text = "Tecla atual: "..selectedKey.Name

local choosingKey = false

chooseKeyButton.MouseButton1Click:Connect(function()
    if choosingKey then return end
    choosingKey = true
    chooseKeyButton.Text = "Pressione uma tecla..."
end)

UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if choosingKey and input.UserInputType == Enum.UserInputType.Keyboard then
        selectedKey = input.KeyCode
        chooseKeyButton.Text = "Tecla atual: "..selectedKey.Name
        choosingKey = false
        return
    end
    if input.KeyCode == selectedKey then
        if ScreenGui.Enabled then
            -- Animar fechamento
            local tweenClose = TweenService:Create(MainFrame, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {BackgroundTransparency = 1, Size = UDim2.new(0, 0, 0, 0)})
            tweenClose:Play()
            tweenClose.Completed:Wait()
            ScreenGui.Enabled = false
            -- Reset propriedades para abrir depois
            MainFrame.Size = UDim2.new(0, 600, 0, 400)
            MainFrame.BackgroundTransparency = 0
        else
            ScreenGui.Enabled = true
            MainFrame.BackgroundTransparency = 1
            MainFrame.Size = UDim2.new(0, 0, 0, 0)
            local tweenOpen = TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {BackgroundTransparency = 0, Size = UDim2.new(0, 600, 0, 400)})
            tweenOpen:Play()
        end
    end
end)

MinimizeButton.MouseButton1Click:Connect(function()
    if ScreenGui.Enabled then
        local tweenClose = TweenService:Create(MainFrame, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {BackgroundTransparency = 1, Size = UDim2.new(0, 0, 0, 0)})
        tweenClose:Play()
        tweenClose.Completed:Wait()
        ScreenGui.Enabled = false
        MainFrame.Size = UDim2.new(0, 600, 0, 400)
        MainFrame.BackgroundTransparency = 0
    else
        ScreenGui.Enabled = true
        MainFrame.BackgroundTransparency = 1
        MainFrame.Size = UDim2.new(0, 0, 0, 0)
        local tweenOpen = TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {BackgroundTransparency = 0, Size = UDim2.new(0, 600, 0, 400)})
        tweenOpen:Play()
    end
end)

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Dragging (igual antes)
local dragging = false
local dragInput, dragStart, startPos

MainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

MainFrame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        Shadow.Position = MainFrame.Position - UDim2.new(0,5,0,5)
    end
end)

print("Kavo UI Clone atualizado com minimizar oculto, tecla dinâmica e animação!")
