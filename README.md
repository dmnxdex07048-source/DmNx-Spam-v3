--[[
    =====================================================================
    👑 DmNx Ji GAND FAAD ALL-IN-ONE (FINAL BOSS) + CURSED LAURA HYBRID
    =====================================================================
]]

local Players = game:GetService("Players")
local TextChatService = game:GetService("TextChatService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()

--------------------------------------------------
-- SETTINGS & GLOBALS
--------------------------------------------------
local saying = false
local auraEnabled = false
local targetName = "ALL"
local delayTime = 1
local brandName = "DmNx Ji"

-- Aura Config
local CURSED_COLOR = Color3.fromRGB(20, 0, 40)
local PARTICLE_COLOR = Color3.fromRGB(138, 3, 3)
local BASE_OFFSET = Vector3.new(0, 4, 3)
local handParts = {}

--------------------------------------------------
-- PATTERNS & ROASTS
--------------------------------------------------
local patterns = {"_","-","=","~","•","*","#","@"}

local function generateLine()
	local p = patterns[math.random(1,#patterns)]
	local result = ""
	while #result < 120 do result = result .. p end
	return result
end

local function getRoast(name)
	local roasts = {
		name.." tu sach me itna weak hai ya acting kar raha hai? 🤡",
		name.." tera gameplay dekh ke lagta hai tutorial bhi fail 😭",
		name.." tu fight karega? hasi aa rahi hai 😂",
		name.." itna try karke bhi kuch nahi kar pa raha 😹",
		name.." tera aim dekh ke bot bhi sharmaye 🤣",
		"Gavaro ko gali nhi",
		"jalne ke badabu aarhi",
		brandName.." ON TOP 👑 "..name.." niche hi rahega 😂",
		name.." "..brandName.." ke saamne zero hai 🤡",
		"LOADOUT SPAMMER APNE BHAI DmNx Ji KA HAI",
		brandName.." dominate karega aur tu dekhega 😈",
		name.." papa bol bete",
		name.." tera game over, "..brandName.." takeover 🔥"
	}
	return roasts[math.random(1,#roasts)]
end

--------------------------------------------------
-- AURA SYSTEM LOGIC
--------------------------------------------------
local function createCursedPart(name, size)
    local part = Instance.new("Part")
    part.Name = name
    part.Size = size
    part.Material = Enum.Material.Neon
    part.Color = CURSED_COLOR
    part.CanCollide = false
    part.Anchored = true
    part.Transparency = 0.2
    
    local emitter = Instance.new("ParticleEmitter")
    emitter.Texture = "rbxassetid://284205403"
    emitter.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(1, 3)})
    emitter.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.5), NumberSequenceKeypoint.new(1, 1)})
    emitter.Color = ColorSequence.new(PARTICLE_COLOR)
    emitter.Lifetime = NumberRange.new(1, 2)
    emitter.Rate = 20
    emitter.Parent = part
    return part
end

local function clearAura()
    for _, p in ipairs(handParts) do p:Destroy() end
    handParts = {}
end

local function spawnAura()
    clearAura()
    local character = player.Character
    if not character then return end
    local root = character:WaitForChild("HumanoidRootPart")

    local palm = createCursedPart("CursedPalm", Vector3.new(3.5, 3.5, 1))
    palm.Parent = character
    table.insert(handParts, palm)

    local fingerData = {
        {"Thumb", -2.2, -0.5, 2.5, math.rad(45)},
        {"Index", -1.2, 2.0, 3.5, math.rad(10)},
        {"Middle", 0, 2.3, 4.0, math.rad(0)},
        {"Ring", 1.2, 2.0, 3.5, math.rad(-10)},
        {"Pinky", 2, 1.0, 2.8, math.rad(-25)}
    }

    local fingers = {}
    for _, d in ipairs(fingerData) do
        local f = createCursedPart("Cursed"..d[1], Vector3.new(0.8, d[4], 0.8))
        f.Parent = character
        table.insert(fingers, {part = f, x = d[2], y = d[3], length = d[4], baseRotZ = d[5]})
        table.insert(handParts, f)
    end

    -- Animation Heartbeat
    task.spawn(function()
        while auraEnabled and character.Parent do
            local t = tick()
            local rootPart = character:FindFirstChild("HumanoidRootPart")
            if not rootPart then break end

            local floatY = math.sin(t * 1.5) * 0.5
            local twitchX = math.noise(t * 8, 0, 0) * 0.3
            
            local baseCFrame = rootPart.CFrame
            local palmOffset = CFrame.new(BASE_OFFSET.X + twitchX, BASE_OFFSET.Y + floatY, BASE_OFFSET.Z)
            local palmRotation = CFrame.Angles(math.rad(-20), 0, 0)
            palm.CFrame = baseCFrame * palmRotation * palmOffset

            for i, fData in ipairs(fingers) do
                local grasp = math.rad(-30) + (math.sin(t * 2 + i) * math.rad(20))
                local pivotPoint = palm.CFrame * CFrame.new(fData.x, fData.y, 0)
                fData.part.CFrame = pivotPoint * CFrame.Angles(grasp, 0, fData.baseRotZ) * CFrame.new(0, fData.length/2, 0)
            end
            RunService.Heartbeat:Wait()
        end
        clearAura()
    end)
end

--------------------------------------------------
-- MAIN GUI SETUP
--------------------------------------------------
local gui = Instance.new("ScreenGui", player.PlayerGui)
gui.Name = "DmNx Ji_HYBRID"
gui.Enabled = false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0,320,0,520)
frame.Position = UDim2.new(0,80,0.5,-260)
frame.BackgroundColor3 = Color3.fromRGB(30,30,45)
frame.Active = true
frame.Draggable = true
Instance.new("UICorner", frame)

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1,0,0,50)
title.Text = "👑 DmNx Ji PANEL ULTIMATE"
title.BackgroundColor3 = Color3.fromRGB(60,120,255)
title.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", title)

-- BUTTON FACTORY
local function createBtn(text, pos, color, callback)
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(0.8,0,0,40)
    btn.Position = pos
    btn.Text = text
    btn.BackgroundColor3 = color
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.GothamBold
    Instance.new("UICorner", btn)
    btn.MouseButton1Click:Connect(callback)
    return btn
end

-- Spam Controls
createBtn("START SPAM", UDim2.new(0.1,0,0.15,0), Color3.fromRGB(0,170,100), function()
    if saying then return end
    saying = true
    task.spawn(function()
        while saying do
            local msg = generateLine().."\n"..brandName.." ON TOP 👑\n"..targetName.."\n"..getRoast(targetName)
            pcall(function() TextChatService.TextChannels.RBXGeneral:SendAsync(msg) end)
            task.wait(delayTime)
        end
    end)
end)

createBtn("STOP SPAM", UDim2.new(0.1,0,0.25,0), Color3.fromRGB(170,0,0), function() saying = false end)

-- Aura Toggle
local auraBtn = createBtn("AURA: OFF", UDim2.new(0.1,0,0.35,0), Color3.fromRGB(80, 0, 150), function()
    auraEnabled = not auraEnabled
    if auraEnabled then
        frame:FindFirstChild("AURA: OFF").Text = "AURA: ON"
        frame:FindFirstChild("AURA: OFF").Name = "AURA: ON"
        spawnAura()
    else
        frame:FindFirstChild("AURA: ON").Text = "AURA: OFF"
        frame:FindFirstChild("AURA: ON").Name = "AURA: OFF"
        clearAura()
    end
end)
auraBtn.Name = "AURA: OFF"

-- Input Boxes
local function createBox(placeholder, pos, callback)
    local box = Instance.new("TextBox", frame)
    box.Size = UDim2.new(0.8,0,0,35)
    box.Position = pos
    box.PlaceholderText = placeholder
    box.BackgroundColor3 = Color3.fromRGB(45,45,60)
    box.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", box)
    box.FocusLost:Connect(function() callback(box.Text) end)
end

createBox("Delay (Sec)", UDim2.new(0.1,0,0.48,0), function(t) delayTime = tonumber(t) or 1 end)
createBox("Target Name", UDim2.new(0.1,0,0.58,0), function(t) if t ~= "" then targetName = t end end)
createBox("Brand Name", UDim2.new(0.1,0,0.68,0), function(t) if t ~= "" then brandName = t end end)

local targetLabel = Instance.new("TextLabel", frame)
targetLabel.Size = UDim2.new(1,0,0,25)
targetLabel.Position = UDim2.new(0,0,0.8,0)
targetLabel.BackgroundTransparency = 1
targetLabel.TextColor3 = Color3.new(1,1,1)
RunService.Heartbeat:Connect(function()
	targetLabel.Text = "🎯 "..targetName.." | 👑 "..brandName
end)

-- Main Toggle Button
local toggle = Instance.new("TextButton", gui)
toggle.Size = UDim2.new(0,50,0,50)
toggle.Position = UDim2.new(0,10,0.5,-25)
toggle.Text = "👑"
toggle.BackgroundColor3 = Color3.fromRGB(60,120,255)
Instance.new("UICorner", toggle)
toggle.MouseButton1Click:Connect(function() frame.Visible = not frame.Visible end)

--------------------------------------------------
-- CINEMATIC INTRO (Remains untouched)
--------------------------------------------------
local introGui = Instance.new("ScreenGui", player.PlayerGui)
local bg = Instance.new("Frame", introGui)
bg.Size = UDim2.new(1,0,1,0)
bg.BackgroundColor3 = Color3.new(0,0,0)

local textLabel = Instance.new("TextLabel", bg)
textLabel.Size = UDim2.new(1,0,0.2,0)
textLabel.Position = UDim2.new(0,0,0.4,0)
textLabel.BackgroundTransparency = 1
textLabel.TextColor3 = Color3.new(1,1,1)
textLabel.TextScaled = true
textLabel.Font = Enum.Font.GothamBold

task.spawn(function()
    local function typeText(text)
        textLabel.Text = ""
        for i = 1, #text do textLabel.Text = string.sub(text,1,i) task.wait(0.03) end
    end
    
    typeText("Ham sharif kya hue puri duniya hi badmash ho gyi...")
    task.wait(1.5)
    typeText("Inki aukat dikhani hogi 😈")
    task.wait(2)
    
    TweenService:Create(bg, TweenInfo.new(1.5), {BackgroundTransparency = 1}):Play()
    TweenService:Create(textLabel, TweenInfo.new(1.5), {TextTransparency = 1}):Play()
    task.wait(1.5)
    introGui:Destroy()
    gui.Enabled = true
end)
