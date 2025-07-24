--[[
    JMODS HUB SCRIPT
    O MELHOR HUB DO MUNDO
    Autor: d3lta-hack
    Descrição: Hub inicial + UNIVERSAL HUB rolável, móvel, minimizável e com FLY básico para PC e Mobile.
--]]

local Players = game:GetService("Players")
local StarterGui = game:GetService("StarterGui")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- Função para tornar o frame móvel/arrastável
local function makeDraggable(frame)
    local dragging, dragInput, dragStart, startPos

    frame.Active = true
    frame.Selectable = true

    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)
    UIS.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(
                startPos.X.Scale, startPos.X.Offset + delta.X,
                startPos.Y.Scale, startPos.Y.Offset + delta.Y
            )
        end
    end)
end

-- FLY globals
local flyActive = false
local flyConnection = nil
local flySpeed = 50
local flyButton = nil
local mobileFlyBtn = nil

local function startFly()
    local player = Players.LocalPlayer
    local char = player.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return end
    local humanoid = char:FindFirstChildWhichIsA("Humanoid")
    if not humanoid then return end

    flyActive = true
    humanoid.PlatformStand = true

    local camera = workspace.CurrentCamera
    local moveVec = Vector3.new()
    local mobileDirection = {forward = false, back = false, left = false, right = false, up = false, down = false}

    -- Mobile Buttons
    if UIS.TouchEnabled and not mobileFlyBtn then
        local function createMobileBtn(name, pos, onDown, onUp)
            local btn = Instance.new("TextButton")
            btn.Size = UDim2.new(0, 60, 0, 60)
            btn.Position = pos
            btn.Text = name
            btn.BackgroundTransparency = 0.3
            btn.BackgroundColor3 = Color3.fromRGB(80,80,200)
            btn.TextColor3 = Color3.new(1,1,1)
            btn.Font = Enum.Font.GothamBold
            btn.TextSize = 22
            btn.Parent = game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChildOfClass("ScreenGui") or game:GetService("Players").LocalPlayer.PlayerGui

            btn.MouseButton1Down:Connect(function() onDown() end)
            btn.MouseButton1Up:Connect(function() onUp() end)
            return btn
        end

        -- Directional Buttons
        local bUp = createMobileBtn("↑", UDim2.new(0,20,1,-200), function() mobileDirection.forward = true end, function() mobileDirection.forward = false end)
        local bDown = createMobileBtn("↓", UDim2.new(0,20,1,-80), function() mobileDirection.back = true end, function() mobileDirection.back = false end)
        local bLeft = createMobileBtn("←", UDim2.new(0,-40,1,-140), function() mobileDirection.left = true end, function() mobileDirection.left = false end)
        local bRight = createMobileBtn("→", UDim2.new(0,80,1,-140), function() mobileDirection.right = true end, function() mobileDirection.right = false end)
        local bTop = createMobileBtn("⇧", UDim2.new(0,150,1,-200), function() mobileDirection.up = true end, function() mobileDirection.up = false end)
        local bBot = createMobileBtn("⇩", UDim2.new(0,150,1,-80), function() mobileDirection.down = true end, function() mobileDirection.down = false end)
        mobileFlyBtn = {bUp, bDown, bLeft, bRight, bTop, bBot}
    end

    flyConnection = RunService.RenderStepped:Connect(function()
        if not flyActive then return end

        moveVec = Vector3.new()
        -- PC Input
        if not UIS.TouchEnabled then
            if UIS:IsKeyDown(Enum.KeyCode.W) then moveVec = moveVec + (camera.CFrame.LookVector * flySpeed) end
            if UIS:IsKeyDown(Enum.KeyCode.S) then moveVec = moveVec - (camera.CFrame.LookVector * flySpeed) end
            if UIS:IsKeyDown(Enum.KeyCode.D) then moveVec = moveVec + (camera.CFrame.RightVector * flySpeed) end
            if UIS:IsKeyDown(Enum.KeyCode.A) then moveVec = moveVec - (camera.CFrame.RightVector * flySpeed) end
            if UIS:IsKeyDown(Enum.KeyCode.Space) then moveVec = moveVec + Vector3.new(0, flySpeed, 0) end
            if UIS:IsKeyDown(Enum.KeyCode.LeftControl) or UIS:IsKeyDown(Enum.KeyCode.LeftShift) then moveVec = moveVec - Vector3.new(0, flySpeed, 0) end
        else
            -- Mobile Input
            if mobileDirection.forward then moveVec = moveVec + (camera.CFrame.LookVector * flySpeed) end
            if mobileDirection.back then moveVec = moveVec - (camera.CFrame.LookVector * flySpeed) end
            if mobileDirection.right then moveVec = moveVec + (camera.CFrame.RightVector * flySpeed) end
            if mobileDirection.left then moveVec = moveVec - (camera.CFrame.RightVector * flySpeed) end
            if mobileDirection.up then moveVec = moveVec + Vector3.new(0, flySpeed, 0) end
            if mobileDirection.down then moveVec = moveVec - Vector3.new(0, flySpeed, 0) end
        end

        char.HumanoidRootPart.Velocity = moveVec
    end)
    StarterGui:SetCore("SendNotification", {
        Title = "Universal Hub";
        Text = "Fly ativado! (Shift+Q para parar no PC)";
        Duration = 3;
    })
end

local function stopFly()
    flyActive = false
    local player = Players.LocalPlayer
    local char = player.Character
    if char and char:FindFirstChildWhichIsA("Humanoid") then
        char:FindFirstChildWhichIsA("Humanoid").PlatformStand = false
        char.HumanoidRootPart.Velocity = Vector3.new(0,0,0)
    end
    if flyConnection then
        flyConnection:Disconnect()
        flyConnection = nil
    end
    -- Remove mobile buttons
    if mobileFlyBtn then
        for _,btn in pairs(mobileFlyBtn) do
            pcall(function() btn:Destroy() end)
        end
        mobileFlyBtn = nil
    end
    StarterGui:SetCore("SendNotification", {
        Title = "Universal Hub";
        Text = "Fly desativado!";
        Duration = 2;
    })
end

-- Bind para PC (Shift+Q para parar o fly)
UIS.InputBegan:Connect(function(input, processed)
    if (input.KeyCode == Enum.KeyCode.Q and UIS:IsKeyDown(Enum.KeyCode.LeftShift)) and flyActive then
        stopFly()
    end
end)

local function toggleFly()
    if not flyActive then
        startFly()
    else
        stopFly()
    end
end

local function createUniversalHub()
    if Players.LocalPlayer.PlayerGui:FindFirstChild("UNIVERSAL_HUB") then
        Players.LocalPlayer.PlayerGui.UNIVERSAL_HUB:Destroy()
    end

    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "UNIVERSAL_HUB"
    ScreenGui.Parent = Players.LocalPlayer.PlayerGui
    ScreenGui.ResetOnSpawn = false

    local MainFrame = Instance.new("Frame")
    MainFrame.Size = UDim2.new(0, 420, 0, 500)
    MainFrame.Position = UDim2.new(0.5, -210, 0.5, -250)
    MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    MainFrame.BorderSizePixel = 0
    MainFrame.Parent = ScreenGui
    MainFrame.Name = "MainFrame"

    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 14)
    UICorner.Parent = MainFrame

    local TitleBar = Instance.new("Frame")
    TitleBar.Size = UDim2.new(1, 0, 0, 60)
    TitleBar.Position = UDim2.new(0, 0, 0, 0)
    TitleBar.BackgroundTransparency = 1
    TitleBar.Parent = MainFrame
    TitleBar.Name = "TitleBar"

    local Title = Instance.new("TextLabel")
    Title.Text = "🌎 UNIVERSAL HUB"
    Title.Font = Enum.Font.GothamBlack
    Title.TextColor3 = Color3.fromRGB(0, 255, 127)
    Title.TextSize = 32
    Title.BackgroundTransparency = 1
    Title.Size = UDim2.new(1, 0, 1, 0)
    Title.Parent = TitleBar

    -- Botão de fechar
    local Close = Instance.new("TextButton")
    Close.Text = "✕"
    Close.Font = Enum.Font.GothamBold
    Close.TextColor3 = Color3.new(1,0,0)
    Close.TextSize = 28
    Close.Size = UDim2.new(0, 40, 0, 40)
    Close.Position = UDim2.new(1, -48, 0, 10)
    Close.BackgroundTransparency = 1
    Close.Parent = TitleBar
    Close.MouseButton1Click:Connect(function()
        stopFly()
        ScreenGui:Destroy()
    end)

    -- Botão de minimizar
    local MinButton = Instance.new("TextButton")
    MinButton.Text = "-"
    MinButton.Font = Enum.Font.GothamBold
    MinButton.TextColor3 = Color3.fromRGB(255, 255, 0)
    MinButton.TextSize = 32
    MinButton.Size = UDim2.new(0, 40, 0, 40)
    MinButton.Position = UDim2.new(1, -92, 0, 10)
    MinButton.BackgroundTransparency = 1
    MinButton.Parent = TitleBar

    -- SCROLLING FRAME para lista rolável de funções
    local Scrolling = Instance.new("ScrollingFrame")
    Scrolling.Size = UDim2.new(1, -32, 1, -110)
    Scrolling.Position = UDim2.new(0, 16, 0, 70)
    Scrolling.BackgroundTransparency = 1
    Scrolling.CanvasSize = UDim2.new(0, 0, 0, 0)
    Scrolling.ScrollBarThickness = 8
    Scrolling.Parent = MainFrame

    local UIList = Instance.new("UIListLayout")
    UIList.Parent = Scrolling
    UIList.Padding = UDim.new(0, 10)
    UIList.SortOrder = Enum.SortOrder.LayoutOrder

    -- Funções do hub
    local function teleportToSpawn()
        local char = Players.LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            char.HumanoidRootPart.CFrame = CFrame.new(0, 10, 0)
            StarterGui:SetCore("SendNotification", {
                Title = "Universal Hub";
                Text = "Teleportado para o spawn!";
                Duration = 2;
            })
        end
    end

    local speedActive = false
    local function toggleSpeed()
        local char = Players.LocalPlayer.Character
        if char and char:FindFirstChildOfClass("Humanoid") then
            if not speedActive then
                char.Humanoid.WalkSpeed = 50
                speedActive = true
            else
                char.Humanoid.WalkSpeed = 16
                speedActive = false
            end
            StarterGui:SetCore("SendNotification", {
                Title = "Universal Hub";
                Text = speedActive and "Velocidade aumentada!" or "Velocidade normalizada!";
                Duration = 2;
            })
        end
    end

    local jumpActive = false
    local function toggleJump()
        local char = Players.LocalPlayer.Character
        if char and char:FindFirstChildOfClass("Humanoid") then
            if not jumpActive then
                char.Humanoid.JumpPower = 100
                jumpActive = true
            else
                char.Humanoid.JumpPower = 50
                jumpActive = false
            end
            StarterGui:SetCore("SendNotification", {
                Title = "Universal Hub";
                Text = jumpActive and "Pulo aumentado!" or "Pulo normalizado!";
                Duration = 2;
            })
        end
    end

    local gravityActive = false
    local function toggleGravity()
        local ws = game:GetService("Workspace")
        if not gravityActive then
            ws.Gravity = 50
            gravityActive = true
        else
            ws.Gravity = 196.2
            gravityActive = false
        end
        StarterGui:SetCore("SendNotification", {
            Title = "Universal Hub";
            Text = gravityActive and "Gravidade baixa ativada!" or "Gravidade normalizada!";
            Duration = 2;
        })
    end

    local noclipActive = false
    local noclipConnection = nil
    local function toggleNoclip()
        local char = Players.LocalPlayer.Character
        if not char then return end
        if not noclipActive then
            noclipActive = true
            noclipConnection = game:GetService("RunService").Stepped:Connect(function()
                if Players.LocalPlayer.Character and Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
                    Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState(11)
                end
            end)
            StarterGui:SetCore("SendNotification", {
                Title = "Universal Hub";
                Text = "NoClip ativado!";
                Duration = 2;
            })
        else
            noclipActive = false
            if noclipConnection then
                noclipConnection:Disconnect()
                noclipConnection = nil
            end
            StarterGui:SetCore("SendNotification", {
                Title = "Universal Hub";
                Text = "NoClip desativado!";
                Duration = 2;
            })
        end
    end

    local invisActive = false
    local function toggleInvis()
        local char = Players.LocalPlayer.Character
        if char then
            for _, v in pairs(char:GetDescendants()) do
                if v:IsA("BasePart") and v.Name ~= "HumanoidRootPart" then
                    if not invisActive then
                        v.Transparency = 1
                        v.CanCollide = false
                    else
                        v.Transparency = 0
                        v.CanCollide = true
                    end
                end
            end
            invisActive = not invisActive
            StarterGui:SetCore("SendNotification", {
                Title = "Universal Hub";
                Text = invisActive and "Invisibilidade ativada!" or "Invisibilidade desativada!";
                Duration = 2;
            })
        end
    end

    local function resetChar()
        local char = Players.LocalPlayer.Character
        if char and char:FindFirstChildOfClass("Humanoid") then
            char:FindFirstChildOfClass("Humanoid").Health = 0
            StarterGui:SetCore("SendNotification", {
                Title = "Universal Hub";
                Text = "Personagem resetado!";
                Duration = 2;
            })
        end
    end

    local function quickRespawn()
        Players.LocalPlayer:LoadCharacter()
        StarterGui:SetCore("SendNotification", {
            Title = "Universal Hub";
            Text = "Respawn instantâneo!";
            Duration = 2;
        })
    end

    local buttons = {
        {Text = "Fly (PC e Mobile)", Callback = toggleFly, Color = Color3.fromRGB(255, 255, 120)},
        {Text = "Teleportar para o Spawn", Callback = teleportToSpawn, Color = Color3.fromRGB(0,170,255)},
        {Text = "Ativar/Desativar Velocidade", Callback = toggleSpeed, Color = Color3.fromRGB(0,200,127)},
        {Text = "Ativar/Desativar Super Pulo", Callback = toggleJump, Color = Color3.fromRGB(200,100,255)},
        {Text = "Ativar/Desativar Baixa Gravidade", Callback = toggleGravity, Color = Color3.fromRGB(255,185,0)},
        {Text = "Ativar/Desativar NoClip", Callback = toggleNoclip, Color = Color3.fromRGB(120,120,120)},
        {Text = "Ativar/Desativar Invisibilidade", Callback = toggleInvis, Color = Color3.fromRGB(180,0,255)},
        {Text = "Resetar Personagem", Callback = resetChar, Color = Color3.fromRGB(255,50,50)},
        {Text = "Respawn Instantâneo", Callback = quickRespawn, Color = Color3.fromRGB(0,255,200)},
        {Text = "Mensagem do JMODS", Callback = function()
            StarterGui:SetCore("SendNotification", {
                Title = "JMODS HUB";
                Text = "O melhor hub do mundo! 😎";
                Duration = 3;
            })
        end, Color = Color3.fromRGB(255, 140, 0)}
    }

    for i, btnData in ipairs(buttons) do
        local btn = Instance.new("TextButton")
        btn.Text = btnData.Text
        btn.Font = Enum.Font.Gotham
        btn.TextColor3 = Color3.fromRGB(255,255,255)
        btn.TextSize = 18
        btn.Size = UDim2.new(1, 0, 0, 40)
        btn.BackgroundColor3 = btnData.Color
        btn.AutoButtonColor = true
        btn.Parent = Scrolling
        btn.MouseButton1Click:Connect(btnData.Callback)
    end

    -- Ajusta o CanvasSize para rolar até o fim (dinâmico)
    local function updateCanvas()
        Scrolling.CanvasSize = UDim2.new(0, 0, 0, UIList.AbsoluteContentSize.Y+10)
    end
    UIList:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(updateCanvas)
    updateCanvas()

    -- Créditos
    local Cred = Instance.new("TextLabel")
    Cred.Text = "by d3lta-hack | JMODS"
    Cred.Font = Enum.Font.Gotham
    Cred.TextColor3 = Color3.fromRGB(200,200,200)
    Cred.TextSize = 13
    Cred.BackgroundTransparency = 1
    Cred.Size = UDim2.new(1, 0, 0, 30)
    Cred.Position = UDim2.new(0, 0, 1, -30)
    Cred.Parent = MainFrame

    -- Minimizar/Restaurar
    local minimized = false
    local originalSize = MainFrame.Size

    MinButton.MouseButton1Click:Connect(function()
        minimized = not minimized
        if minimized then
            MainFrame.Size = UDim2.new(0, 240, 0, 60)
            Scrolling.Visible = false
            Cred.Visible = false
            MinButton.Text = "+"
        else
            MainFrame.Size = originalSize
            Scrolling.Visible = true
            Cred.Visible = true
            MinButton.Text = "-"
        end
    end)

    -- Frame móvel/arrastável pela barra de título
    makeDraggable(TitleBar)
end

-- HUB INICIAL
local function createHub()
    if Players.LocalPlayer.PlayerGui:FindFirstChild("JMODS_HUB") then
        Players.LocalPlayer.PlayerGui.JMODS_HUB:Destroy()
    end

    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "JMODS_HUB"
    ScreenGui.Parent = Players.LocalPlayer:WaitForChild("PlayerGui")
    ScreenGui.ResetOnSpawn = false

    local Frame = Instance.new("Frame")
    Frame.Size = UDim2.new(0, 350, 0, 250)
    Frame.Position = UDim2.new(0.5, -175, 0.5, -125)
    Frame.BackgroundColor3 = Color3.fromRGB(42, 42, 42)
    Frame.BorderSizePixel = 0
    Frame.Parent = ScreenGui

    local Title = Instance.new("TextLabel")
    Title.Text = "JMODS HUB\nO MELHOR HUB DO MUNDO"
    Title.Font = Enum.Font.GothamBlack
    Title.TextColor3 = Color3.fromRGB(255, 215, 0)
    Title.TextSize = 22
    Title.BackgroundTransparency = 1
    Title.Size = UDim2.new(1, 0, 0, 60)
    Title.Parent = Frame

    local Close = Instance.new("TextButton")
    Close.Text = "✕"
    Close.Font = Enum.Font.GothamBold
    Close.TextColor3 = Color3.new(1,0,0)
    Close.TextSize = 24
    Close.Size = UDim2.new(0, 40, 0, 40)
    Close.Position = UDim2.new(1, -45, 0, 5)
    Close.BackgroundTransparency = 1
    Close.Parent = Frame
    Close.MouseButton1Click:Connect(function()
        ScreenGui:Destroy()
    end)

    local Button1 = Instance.new("TextButton")
    Button1.Text = "Ativar Exemplo 1"
    Button1.Font = Enum.Font.Gotham
    Button1.TextColor3 = Color3.fromRGB(255,255,255)
    Button1.TextSize = 18
    Button1.Size = UDim2.new(0.8, 0, 0, 40)
    Button1.Position = UDim2.new(0.1, 0, 0, 80)
    Button1.BackgroundColor3 = Color3.fromRGB(50,120,255)
    Button1.BorderSizePixel = 0
    Button1.Parent = Frame
    Button1.MouseButton1Click:Connect(function()
        ScreenGui:Destroy()
        createUniversalHub()
    end)

    local Button2 = Instance.new("TextButton")
    Button2.Text = "Ativar Exemplo 2"
    Button2.Font = Enum.Font.Gotham
    Button2.TextColor3 = Color3.fromRGB(255,255,255)
    Button2.TextSize = 18
    Button2.Size = UDim2.new(0.8, 0, 0, 40)
    Button2.Position = UDim2.new(0.1, 0, 0, 130)
    Button2.BackgroundColor3 = Color3.fromRGB(40,200,80)
    Button2.BorderSizePixel = 0
    Button2.Parent = Frame
    Button2.MouseButton1Click:Connect(function()
        StarterGui:SetCore("SendNotification", {
            Title = 
