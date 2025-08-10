-- Kazak GAG UI v2.0 — Trade + Spawner, readable text, RGB border, blur, drag, hotkey K
-- Local-only UI. Pet: anchored part that follows HRP on RenderStepped.

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local StarterGui = game:GetService("StarterGui")

local LP = Players.LocalPlayer

-- Helpers
local function safeParent(gui)
    local ok,root = pcall(function() return gethui and gethui() end)
    if ok and root then return root end
    return LP:FindFirstChildOfClass("PlayerGui") or LP.PlayerGui
end

local function makeTween(obj, props, time, style, dir)
    return TweenService:Create(obj, TweenInfo.new(time or 0.25, style or Enum.EasingStyle.Quint, dir or Enum.EasingDirection.Out), props)
end

local function notify(title, text)
    pcall(function()
        StarterGui:SetCore("SendNotification",{Title=title,Text=text,Duration=2})
    end)
end

-- Blur
local blur = Lighting:FindFirstChild("KazakBlur") or Instance.new("BlurEffect")
blur.Name = "KazakBlur"
blur.Size = 0
blur.Parent = Lighting

-- ScreenGui
local gui = Instance.new("ScreenGui")
gui.Name = "Kazak_GAG_v2"
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.Parent = safeParent()

-- Shadow
local shadow = Instance.new("Frame")
shadow.Name = "Shadow"
shadow.BackgroundColor3 = Color3.fromRGB(0,0,0)
shadow.BorderSizePixel = 0
shadow.BackgroundTransparency = 0.4
shadow.AnchorPoint = Vector2.new(0.5,0.5)
shadow.Position = UDim2.fromScale(0.5,0.5)
shadow.Size = UDim2.fromOffset(524, 364)
shadow.Parent = gui

-- Main window
local main = Instance.new("Frame")
main.Name = "Main"
main.BackgroundColor3 = Color3.fromRGB(16,16,20)
main.BorderSizePixel = 0
main.AnchorPoint = Vector2.new(0.5,0.5)
main.Position = UDim2.fromScale(0.5,0.5)
main.Size = UDim2.fromOffset(520, 360)
main.ClipsDescendants = true
main.Parent = gui

local corner = Instance.new("UICorner", main)
corner.CornerRadius = UDim.new(0,12)

local stroke = Instance.new("UIStroke", main)
stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
stroke.Thickness = 2
stroke.Color = Color3.fromRGB(255,0,110)

-- RGB border animation
task.spawn(function()
    local t = 0
    while main.Parent do
        t = (t + RunService.Heartbeat:Wait()*0.35) % 1
        stroke.Color = Color3.fromHSV(t, 0.9, 1)
    end
end)

-- Title bar
local titleBar = Instance.new("Frame")
titleBar.Name = "TitleBar"
titleBar.BackgroundColor3 = Color3.fromRGB(22,22,28)
titleBar.Size = UDim2.new(1,0,0,42)
titleBar.BorderSizePixel = 0
titleBar.Parent = main
Instance.new("UICorner", titleBar).CornerRadius = UDim.new(0,12)

local title = Instance.new("TextLabel")
title.Name = "Title"
title.BackgroundTransparency = 1
title.Position = UDim2.fromOffset(16,0)
title.Size = UDim2.fromOffset(280,42)
title.Font = Enum.Font.GothamBold
title.Text = "Trade Freez GAG — Kazak Edition"
title.TextColor3 = Color3.fromRGB(240,244,255)
title.TextSize = 18
title.TextXAlignment = Enum.TextXAlignment.Left
title.TextStrokeTransparency = 0.4
title.TextStrokeColor3 = Color3.fromRGB(0,0,0)
title.Parent = titleBar

-- Drag handle (entire title bar)
local drag = Instance.new("Frame")
drag.BackgroundTransparency = 1
drag.Size = UDim2.new(1,-120,1,0)
drag.Position = UDim2.fromOffset(0,0)
drag.Parent = titleBar

-- Close button
local closeBtn = Instance.new("TextButton")
closeBtn.Name = "Close"
closeBtn.Text = "✕"
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 18
closeBtn.TextColor3 = Color3.fromRGB(240,244,255)
closeBtn.TextStrokeTransparency = 0.4
closeBtn.TextStrokeColor3 = Color3.fromRGB(0,0,0)
closeBtn.BackgroundColor3 = Color3.fromRGB(32,32,40)
closeBtn.AutoButtonColor = false
closeBtn.Size = UDim2.fromOffset(40,28)
closeBtn.Position = UDim2.new(1,-52,0,7)
closeBtn.Parent = titleBar
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0,8)
local closeStroke = Instance.new("UIStroke", closeBtn)
closeStroke.Color = Color3.fromRGB(70,70,90)
closeStroke.Thickness = 1

-- Tab buttons
local tabBar = Instance.new("Frame")
tabBar.Name = "TabBar"
tabBar.BackgroundTransparency = 1
tabBar.Size = UDim2.fromOffset(200,32)
tabBar.Position = UDim2.new(1,-200,0,5)
tabBar.Parent = titleBar

local function makeTab(text, x)
    local b = Instance.new("TextButton")
    b.Text = text
    b.Font = Enum.Font.GothamBold
    b.TextSize = 14
    b.TextColor3 = Color3.fromRGB(220,225,235)
    b.TextStrokeTransparency = 0.6
    b.TextStrokeColor3 = Color3.fromRGB(0,0,0)
    b.BackgroundColor3 = Color3.fromRGB(28,28,36)
    b.AutoButtonColor = false
    b.Size = UDim2.fromOffset(92,28)
    b.Position = UDim2.fromOffset(x,2)
    b.Parent = tabBar
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,8)
    local s = Instance.new("UIStroke", b)
    s.Color = Color3.fromRGB(70,70,90); s.Thickness=1
    return b
end

local tradeTabBtn = makeTab("Trade", 0)
local spawnTabBtn = makeTab("Spawner", 100)

-- Content holder
local content = Instance.new("Frame")
content.Name = "Content"
content.BackgroundColor3 = Color3.fromRGB(14,14,18)
content.BorderSizePixel = 0
content.Position = UDim2.fromOffset(8,50)
content.Size = UDim2.new(1,-16,1,-58)
content.Parent = main
Instance.new("UICorner", content).CornerRadius = UDim.new(0,10)

-- Pages
local tradePage = Instance.new("Frame")
tradePage.Name = "TradePage"
tradePage.BackgroundTransparency = 1
tradePage.Size = UDim2.fromScale(1,1)
tradePage.Parent = content

local spawnerPage = Instance.new("Frame")
spawnerPage.Name = "SpawnerPage"
spawnerPage.BackgroundTransparency = 1
spawnerPage.Visible = false
spawnerPage.Size = UDim2.fromScale(1,1)
spawnerPage.Parent = content

-- Reusable button factory
local function makeActionButton(parent, text, pos)
    local b = Instance.new("TextButton")
    b.Text = text
    b.Font = Enum.Font.GothamBold
    b.TextSize = 16
    b.TextColor3 = Color3.fromRGB(240,244,255)
    b.TextStrokeTransparency = 0.4
    b.TextStrokeColor3 = Color3.fromRGB(0,0,0)
    b.BackgroundColor3 = Color3.fromRGB(26,26,32)
    b.AutoButtonColor = false
    b.Size = UDim2.fromOffset(160,44)
    b.Position = pos
    b.Parent = parent
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,10)
    local s = Instance.new("UIStroke", b)
    s.Color = Color3.fromRGB(80,80,110); s.Thickness=1
    b.MouseEnter:Connect(function() makeTween(b,{BackgroundColor3=Color3.fromRGB(32,32,40)},0.15):Play() end)
    b.MouseLeave:Connect(function() makeTween(b,{BackgroundColor3=Color3.fromRGB(26,26,32)},0.2):Play() end)
    b.MouseButton1Down:Connect(function() makeTween(b,{Size=b.Size+UDim2.fromOffset(2,2)},0.08):Play() end)
    b.MouseButton1Up:Connect(function() makeTween(b,{Size=UDim2.fromOffset(160,44)},0.12):Play() end)
    return b
end

-- Trade page UI
local tradeLbl = Instance.new("TextLabel")
tradeLbl.BackgroundTransparency = 1
tradeLbl.Position = UDim2.fromOffset(16,10)
tradeLbl.Size = UDim2.fromOffset(360,28)
tradeLbl.Font = Enum.Font.GothamBold
tradeLbl.Text = "Trade Controls (визуальные заглушки)"
tradeLbl.TextSize = 16
tradeLbl.TextColor3 = Color3.fromRGB(230,235,245)
tradeLbl.TextStrokeTransparency = 0.5
tradeLbl.TextStrokeColor3 = Color3.fromRGB(0,0,0)
tradeLbl.TextXAlignment = Enum.TextXAlignment.Left
tradeLbl.Parent = tradePage

local freezeBtn = makeActionButton(tradePage, "Freeze", UDim2.fromOffset(16,60))
local acceptBtn = makeActionButton(tradePage, "Accept", UDim2.fromOffset(196,60))

freezeBtn.MouseButton1Click:Connect(function() notify("Trade","Freeze: заглушка") end)
acceptBtn.MouseButton1Click:Connect(function() notify("Trade","Accept: заглушка") end)

-- Spawner page UI
local spLbl = Instance.new("TextLabel")
spLbl.BackgroundTransparency = 1
spLbl.Position = UDim2.fromOffset(16,10)
spLbl.Size = UDim2.fromOffset(420,28)
spLbl.Font = Enum.Font.GothamBold
spLbl.Text = "Pet Spawner — локальный кубик, следует за вами"
spLbl.TextSize = 16
spLbl.TextColor3 = Color3.fromRGB(230,235,245)
spLbl.TextStrokeTransparency = 0.5
spLbl.TextStrokeColor3 = Color3.fromRGB(0,0,0)
spLbl.TextXAlignment = Enum.TextXAlignment.Left
spLbl.Parent = spawnerPage

local spawnBtn = makeActionButton(spawnerPage, "Spawn Pet", UDim2.fromOffset(16,60))
local followToggle = makeActionButton(spawnerPage, "Follow: ON", UDim2.fromOffset(196,60))
local despawnBtn = makeActionButton(spawnerPage, "Despawn", UDim2.fromOffset(16,116))

-- Pet logic
local pet
local followConn
local followEnabled = true

local function getHRP()
    local char = LP.Character or LP.CharacterAdded:Wait()
    return char:FindFirstChild("HumanoidRootPart")
end

local function destroyPet()
    if followConn then followConn:Disconnect() followConn=nil end
    if pet then pet:Destroy() pet=nil end
end

local function spawnPet()
    destroyPet()
    local hrp = getHRP()
    if not hrp then notify("Spawner","HRP не найден"); return end
    pet = Instance.new("Part")
    pet.Name = "KazakPet"
    pet.Size = Vector3.new(1.4,1.4,1.4)
    pet.Color = Color3.fromRGB(255, 90, 140)
    pet.Material = Enum.Material.Neon
    pet.Anchored = true
    pet.CanCollide = false
    pet.CastShadow = false
    pet.Parent = workspace
    local atStart = hrp.CFrame * CFrame.new(2, 1.5, -2)
    pet.CFrame = atStart

    -- Nice outline using SelectionBox (client-side visual)
    local sel = Instance.new("SelectionBox")
    sel.LineThickness = 0.04
    sel.Color3 = Color3.fromRGB(255,255,255)
    sel.SurfaceTransparency = 1
    sel.Adornee = pet
    sel.Parent = pet

    followConn = RunService.RenderStepped:Connect(function(t)
        if not pet or not pet.Parent then return end
        local pHRP = getHRP()
        if not pHRP then return end
        local time = os.clock()
        local bob = math.sin(time*2.5)*0.35
        local offset = Vector3.new(2, 1.5 + bob, -2)
        local targetCF = pHRP.CFrame * CFrame.new(offset)
        if followEnabled then
            -- Smooth follow
            pet.CFrame = pet.CFrame:Lerp(targetCF * CFrame.Angles(0, math.rad((time*120)%360), 0), 0.18)
        end
    end)
end

spawnBtn.MouseButton1Click:Connect(function()
    spawnPet()
    notify("Spawner","Pet создан")
end)

followToggle.MouseButton1Click:Connect(function()
    followEnabled = not followEnabled
    followToggle.Text = followEnabled and "Follow: ON" or "Follow: OFF"
    notify("Spawner", followEnabled and "Следование включено" or "Следование выключено")
end)

despawnBtn.MouseButton1Click:Connect(function()
    destroyPet()
    notify("Spawner","Pet удалён")
end)

-- Tabs switching
local function setTab(active)
    if active=="Trade" then
        tradePage.Visible = true
        spawnerPage.Visible = false
        makeTween(tradeTabBtn,{BackgroundColor3=Color3.fromRGB(38,38,48)},0.18):Play()
        makeTween(spawnTabBtn,{BackgroundColor3=Color3.fromRGB(28,28,36)},0.18):Play()
    else
        tradePage.Visible = false
        spawnerPage.Visible = true
        makeTween(spawnTabBtn,{BackgroundColor3=Color3.fromRGB(38,38,48)},0.18):Play()
        makeTween(tradeTabBtn,{BackgroundColor3=Color3.fromRGB(28,28,36)},0.18):Play()
    end
end

tradeTabBtn.MouseButton1Click:Connect(function() setTab("Trade") end)
spawnTabBtn.MouseButton1Click:Connect(function() setTab("Spawner") end)

-- Dragging
do
    local dragging=false; local dragStart; local startPos
    local function update(input)
        local delta = input.Position - dragStart
        main.Position = UDim2.fromOffset(startPos.X + delta.X, startPos.Y + delta.Y)
        shadow.Position = main.Position
    end
    drag.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = main.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then dragging=false end
            end)
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            update(input)
        end
    end)
end

-- Open/close + blur
local isOpen = true
local function openUI()
    gui.Enabled = true
    main.Visible = true
    shadow.Visible = true
    main.Position = UDim2.fromScale(0.5,0.45)
    main.Size = UDim2.fromOffset(480, 320)
    makeTween(main, {Position=UDim2.fromScale(0.5,0.5), Size=UDim2.fromOffset(520,360)}, 0.25):Play()
    makeTween(shadow, {Position=UDim2.fromScale(0.5,0.5)}, 0.25):Play()
    makeTween(blur, {Size=12}, 0.25):Play()
    isOpen = true
end

local function closeUI()
    makeTween(blur, {Size=0}, 0.2):Play()
    makeTween(main, {Position=UDim2.fromScale(0.5,0.55), Size=UDim2.fromOffset(480,320)}, 0.2):Play()
    task.delay(0.2, function()
        gui.Enabled = false
        main.Visible = false
        shadow.Visible = false
    end)
    isOpen = false
end

closeBtn.MouseButton1Click:Connect(closeUI)

UserInputService.InputBegan:Connect(function(input,gp)
    if gp then return end
    if input.KeyCode == Enum.KeyCode.K then
        if isOpen then closeUI() else openUI() end
    end
end)

-- Default state
setTab("Trade")
openUI()
