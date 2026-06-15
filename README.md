if getgenv().CocoHub_Executed then
	return
end

getgenv().CocoHub_Executed = true

----------------------------------------------------
-- LOADING SCREEN
----------------------------------------------------

local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local Lighting = game:GetService("Lighting")

local loadingGui = Instance.new("ScreenGui")
loadingGui.Name = "CHubLoading"
loadingGui.Parent = CoreGui
loadingGui.IgnoreGuiInset = true
loadingGui.ResetOnSpawn = false
loadingGui.ZIndexBehavior = Enum.ZIndexBehavior.Global

local loadingFrame = Instance.new("Frame")
loadingFrame.Parent = loadingGui
loadingFrame.Size = UDim2.new(1,0,1,0)
loadingFrame.BackgroundColor3 = Color3.new(0,0,0)
loadingFrame.BackgroundTransparency = 1

local blur = Instance.new("BlurEffect")
blur.Size = 0
blur.Parent = Lighting

local loadBox = Instance.new("Frame")
loadBox.Parent = loadingFrame
loadBox.Size = UDim2.new(0,540,0,250)
loadBox.Position = UDim2.new(0.5,-270,0.5,-125)
loadBox.BackgroundColor3 = Color3.fromRGB(10,10,10)
loadBox.BorderSizePixel = 0

local loadCorner = Instance.new("UICorner")
loadCorner.CornerRadius = UDim.new(0,18)
loadCorner.Parent = loadBox

local loadStroke = Instance.new("UIStroke")
loadStroke.Parent = loadBox
loadStroke.Thickness = 3

local title = Instance.new("TextLabel")
title.Parent = loadBox
title.BackgroundTransparency = 1
title.TextTransparency = 1
title.Size = UDim2.new(1,0,0,50)
title.Position = UDim2.new(0,0,0,35)
title.Font = Enum.Font.GothamBlack
title.Text = "STA PREMIUM V1.0.1"
title.TextSize = 28
title.TextColor3 = Color3.new(1,1,1)

TweenService:Create(title,TweenInfo.new(2),{
	TextTransparency = 0
}):Play()

task.spawn(function()
	while task.wait() do
		local hue = (tick() * 0.2) % 1
		title.TextColor3 = Color3.fromHSV(hue,1,1)
	end
end)

local desc = Instance.new("TextLabel")
desc.Parent = loadBox
desc.BackgroundTransparency = 1
desc.Size = UDim2.new(1,0,0,30)
desc.Position = UDim2.new(0,0,0,95)
desc.Font = Enum.Font.Gotham
desc.Text = "Join our Discord for updates & support:"
desc.TextColor3 = Color3.fromRGB(200,200,200)
desc.TextSize = 18

local discord = Instance.new("TextButton")
discord.Parent = loadBox
discord.Size = UDim2.new(0,300,0,38)
discord.Position = UDim2.new(0.5,-150,0,138)
discord.BackgroundColor3 = Color3.fromRGB(88,101,242)
discord.Text = "discord.gg/STA.VIP"
discord.Font = Enum.Font.GothamBold
discord.TextSize = 18
discord.TextColor3 = Color3.new(1,1,1)

local discordCorner = Instance.new("UICorner")
discordCorner.CornerRadius = UDim.new(0,10)
discordCorner.Parent = discord

local continue = Instance.new("TextButton")
continue.Parent = loadBox
continue.Size = UDim2.new(0,130,0,34)
continue.Position = UDim2.new(0.5,-65,1,-50)
continue.BackgroundColor3 = Color3.fromRGB(25,25,25)
continue.Text = "Continue"
continue.Font = Enum.Font.GothamBold
continue.TextSize = 18
continue.TextColor3 = Color3.new(1,1,1)

local continueCorner = Instance.new("UICorner")
continueCorner.CornerRadius = UDim.new(0,10)
continueCorner.Parent = continue

----------------------------------------------------
-- DISCORD COPY LINK
----------------------------------------------------

discord.MouseButton1Click:Connect(function()
	setclipboard("https://discord.gg/GJHdx6BEU")
	discord.Text = "COPIED!"
	task.wait(1)
	discord.Text = "discord.gg/STA-HUB"
end)

----------------------------------------------------
-- START INVISIBLE
----------------------------------------------------

loadBox.BackgroundTransparency = 1
loadStroke.Transparency = 1
title.TextTransparency = 1
desc.TextTransparency = 1
discord.BackgroundTransparency = 1
discord.TextTransparency = 1
continue.BackgroundTransparency = 1
continue.TextTransparency = 1

----------------------------------------------------
-- RAINBOW BORDER
----------------------------------------------------

task.spawn(function()
	while task.wait() do
		local hue = (tick() * 0.15) % 1
		local rainbow = Color3.fromHSV(hue,0.8,0.6)
		loadStroke.Color = rainbow
	end
end)

TweenService:Create(loadingFrame,TweenInfo.new(1.5),{
	BackgroundTransparency = 0.3
}):Play()

TweenService:Create(loadBox,TweenInfo.new(1.5),{
	BackgroundTransparency = 0
}):Play()

TweenService:Create(loadStroke,TweenInfo.new(1.5),{
	Transparency = 0
}):Play()

TweenService:Create(title,TweenInfo.new(2),{
	TextTransparency = 0
}):Play()

TweenService:Create(desc,TweenInfo.new(2),{
	TextTransparency = 0
}):Play()

TweenService:Create(discord,TweenInfo.new(2),{
	BackgroundTransparency = 0,
	TextTransparency = 0
}):Play()

TweenService:Create(continue,TweenInfo.new(2),{
	BackgroundTransparency = 0,
	TextTransparency = 0
}):Play()

TweenService:Create(blur,TweenInfo.new(1.5),{
	Size = 18
}):Play()

----------------------------------------------------
-- WAIT FOR CONTINUE
----------------------------------------------------

local loaded = false

continue.MouseButton1Click:Connect(function()
	loaded = true
	TweenService:Create(loadingFrame,TweenInfo.new(1),{
		BackgroundTransparency = 1
	}):Play()
	TweenService:Create(loadBox,TweenInfo.new(1),{
		BackgroundTransparency = 1
	}):Play()
	TweenService:Create(blur,TweenInfo.new(1),{
		Size = 0
	}):Play()
	task.wait(1)
	loadingGui:Destroy()
	blur:Destroy()
	getgenv().CocoHub_Loading = false
end)

getgenv().CocoHub_Loading = true

repeat task.wait() until loaded

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")

local player = Players.LocalPlayer

----------------------------------------------------
-- VARIABLES
----------------------------------------------------

local enabled = false
local flying = false
local minimized = false
local noclipEnabled = false
local noclipConnection = nil
local flySpeed = 75
local basePosition = nil
local baseEnabled = false
local returnPosition = nil
local walkingSpeed = 16

local items = {
	["Fuel"] = true,
	["Bandage"] = true,
	["Medkit"] = true
}

----------------------------------------------------
-- CLEAN UP OLD GUI
----------------------------------------------------
for _, v in ipairs(CoreGui:GetChildren()) do
	if v:IsA("ScreenGui") and (v.Name == "CocoHub" or v.Name == "CHubLoading") then
		v:Destroy()
	end
end

local gui = Instance.new("ScreenGui")
gui.Name = "CocoHub"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.ZIndexBehavior = Enum.ZIndexBehavior.Global
gui.DisplayOrder = 999999999
gui.Parent = CoreGui

----------------------------------------------------
-- MAIN FRAME
----------------------------------------------------

local frame = Instance.new("Frame")
frame.Parent = gui
frame.Size = UDim2.new(0,800,0,550)
frame.Position = UDim2.new(0.5,-400,0.5,-275)
frame.BackgroundColor3 = Color3.fromRGB(15,15,15)
frame.BorderSizePixel = 0

local frameCorner = Instance.new("UICorner")
frameCorner.CornerRadius = UDim.new(0,16)
frameCorner.Parent = frame

local stroke = Instance.new("UIStroke")
stroke.Parent = frame
stroke.Thickness = 2
stroke.Color = Color3.fromRGB(255,0,140)

----------------------------------------------------
-- TOPBAR DRAG AREA
----------------------------------------------------

local topbar = Instance.new("Frame")
topbar.Parent = frame
topbar.Size = UDim2.new(1,0,0,50)
topbar.BackgroundTransparency = 1

----------------------------------------------------
-- SIDEBAR
----------------------------------------------------

local sidebar = Instance.new("Frame")
sidebar.Parent = frame
sidebar.Size = UDim2.new(0,180,1,0)
sidebar.BackgroundColor3 = Color3.fromRGB(10,10,10)
sidebar.BorderSizePixel = 0

local sideCorner = Instance.new("UICorner")
sideCorner.CornerRadius = UDim.new(0,16)
sideCorner.Parent = sidebar

local titleLabel = Instance.new("TextLabel")
titleLabel.Parent = sidebar
titleLabel.Size = UDim2.new(1,0,0,60)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "⭐ STA HUB"
titleLabel.Font = Enum.Font.GothamBlack
titleLabel.TextSize = 24
titleLabel.TextColor3 = Color3.new(1,1,1)

----------------------------------------------------
-- SEARCH
----------------------------------------------------

local searchBox = Instance.new("TextBox")
searchBox.Parent = sidebar
searchBox.Size = UDim2.new(1,-20,0,38)
searchBox.Position = UDim2.new(0,10,0,60)
searchBox.BackgroundColor3 = Color3.fromRGB(22,22,22)
searchBox.PlaceholderText = "Search..."
searchBox.Text = ""
searchBox.Font = Enum.Font.Gotham
searchBox.TextSize = 14
searchBox.TextColor3 = Color3.new(1,1,1)
searchBox.PlaceholderColor3 = Color3.fromRGB(120,120,120)

local searchCorner = Instance.new("UICorner")
searchCorner.CornerRadius = UDim.new(0,10)
searchCorner.Parent = searchBox

----------------------------------------------------
-- TAB SYSTEM
----------------------------------------------------

local tabContainers = {}
local tabButtons = {}
local currentTab = nil

local function switchTab(tabName)
	if currentTab then
		currentTab.Visible = false
	end
	currentTab = tabContainers[tabName]
	if currentTab then
		currentTab.Visible = true
	end
	for name, btn in pairs(tabButtons) do
		if name == tabName then
			btn.BackgroundColor3 = Color3.fromRGB(45,45,45)
		else
			btn.BackgroundColor3 = Color3.fromRGB(25,25,25)
		end
	end
end

local function createTab(text, y)
	local tab = Instance.new("TextButton")
	tab.Parent = sidebar
	tab.Size = UDim2.new(1,-20,0,42)
	tab.Position = UDim2.new(0,10,0,y)
	tab.BackgroundColor3 = Color3.fromRGB(25,25,25)
	tab.TextColor3 = Color3.new(1,1,1)
	tab.Font = Enum.Font.GothamBold
	tab.TextSize = 15
	tab.Text = text
	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0,10)
	corner.Parent = tab
	return tab
end

-- Create tab buttons
local playerTabBtn = createTab("Player", 120)
local movementTabBtn = createTab("Movement", 170)
local combatTabBtn = createTab("Combat", 220)
local visualTabBtn = createTab("Visual", 270)
local settingsTabBtn = createTab("Settings", 320)

tabButtons["Player"] = playerTabBtn
tabButtons["Movement"] = movementTabBtn
tabButtons["Combat"] = combatTabBtn
tabButtons["Visual"] = visualTabBtn
tabButtons["Settings"] = settingsTabBtn

----------------------------------------------------
-- CONTENT AREA
----------------------------------------------------

local contentArea = Instance.new("Frame")
contentArea.Parent = frame
contentArea.Size = UDim2.new(1,-200,1,-20)
contentArea.Position = UDim2.new(0,190,0,10)
contentArea.BackgroundTransparency = 1

-- Player Tab
local playerTab = Instance.new("ScrollingFrame")
playerTab.Parent = contentArea
playerTab.Size = UDim2.new(1,0,1,0)
playerTab.BackgroundTransparency = 1
playerTab.BorderSizePixel = 0
playerTab.CanvasSize = UDim2.new(0,0,0,200)
playerTab.ScrollBarThickness = 4
playerTab.Visible = true

local playerSection = Instance.new("Frame")
playerSection.Parent = playerTab
playerSection.Size = UDim2.new(1,-20,0,150)
playerSection.Position = UDim2.new(0,10,0,10)
playerSection.BackgroundColor3 = Color3.fromRGB(22,22,22)
playerSection.BorderSizePixel = 0
local playerSectionCorner = Instance.new("UICorner")
playerSectionCorner.CornerRadius = UDim.new(0,14)
playerSectionCorner.Parent = playerSection

local playerTitle = Instance.new("TextLabel")
playerTitle.Parent = playerSection
playerTitle.Size = UDim2.new(1,-20,0,40)
playerTitle.Position = UDim2.new(0,15,0,10)
playerTitle.BackgroundTransparency = 1
playerTitle.Text = "Player Settings"
playerTitle.Font = Enum.Font.GothamBold
playerTitle.TextSize = 22
playerTitle.TextColor3 = Color3.new(1,1,1)
playerTitle.TextXAlignment = Enum.TextXAlignment.Left

-- Movement Tab
local movementTab = Instance.new("ScrollingFrame")
movementTab.Parent = contentArea
movementTab.Size = UDim2.new(1,0,1,0)
movementTab.BackgroundTransparency = 1
movementTab.BorderSizePixel = 0
movementTab.CanvasSize = UDim2.new(0,0,0,500)
movementTab.ScrollBarThickness = 4
movementTab.Visible = false

local movementSection = Instance.new("Frame")
movementSection.Parent = movementTab
movementSection.Size = UDim2.new(1,-20,0,450)
movementSection.Position = UDim2.new(0,10,0,10)
movementSection.BackgroundColor3 = Color3.fromRGB(22,22,22)
movementSection.BorderSizePixel = 0
local movementSectionCorner = Instance.new("UICorner")
movementSectionCorner.CornerRadius = UDim.new(0,14)
movementSectionCorner.Parent = movementSection

local movementTitle = Instance.new("TextLabel")
movementTitle.Parent = movementSection
movementTitle.Size = UDim2.new(1,-20,0,40)
movementTitle.Position = UDim2.new(0,15,0,10)
movementTitle.BackgroundTransparency = 1
movementTitle.Text = "Movement Settings"
movementTitle.Font = Enum.Font.GothamBold
movementTitle.TextSize = 22
movementTitle.TextColor3 = Color3.new(1,1,1)
movementTitle.TextXAlignment = Enum.TextXAlignment.Left

-- Combat Tab (may Kill Aura dito)
local combatTab = Instance.new("ScrollingFrame")
combatTab.Parent = contentArea
combatTab.Size = UDim2.new(1,0,1,0)
combatTab.BackgroundTransparency = 1
combatTab.BorderSizePixel = 0
combatTab.CanvasSize = UDim2.new(0,0,0,400)
combatTab.ScrollBarThickness = 4
combatTab.Visible = false

local combatSection = Instance.new("Frame")
combatSection.Parent = combatTab
combatSection.Size = UDim2.new(1,-20,0,350)
combatSection.Position = UDim2.new(0,10,0,10)
combatSection.BackgroundColor3 = Color3.fromRGB(22,22,22)
combatSection.BorderSizePixel = 0
local combatSectionCorner = Instance.new("UICorner")
combatSectionCorner.CornerRadius = UDim.new(0,14)
combatSectionCorner.Parent = combatSection

local combatTitle = Instance.new("TextLabel")
combatTitle.Parent = combatSection
combatTitle.Size = UDim2.new(1,-20,0,40)
combatTitle.Position = UDim2.new(0,15,0,10)
combatTitle.BackgroundTransparency = 1
combatTitle.Text = "Combat Settings"
combatTitle.Font = Enum.Font.GothamBold
combatTitle.TextSize = 22
combatTitle.TextColor3 = Color3.new(1,1,1)
combatTitle.TextXAlignment = Enum.TextXAlignment.Left

-- Visual Tab
local visualTab = Instance.new("ScrollingFrame")
visualTab.Parent = contentArea
visualTab.Size = UDim2.new(1,0,1,0)
visualTab.BackgroundTransparency = 1
visualTab.BorderSizePixel = 0
visualTab.CanvasSize = UDim2.new(0,0,0,200)
visualTab.ScrollBarThickness = 4
visualTab.Visible = false

local visualSection = Instance.new("Frame")
visualSection.Parent = visualTab
visualSection.Size = UDim2.new(1,-20,0,150)
visualSection.Position = UDim2.new(0,10,0,10)
visualSection.BackgroundColor3 = Color3.fromRGB(22,22,22)
visualSection.BorderSizePixel = 0
local visualSectionCorner = Instance.new("UICorner")
visualSectionCorner.CornerRadius = UDim.new(0,14)
visualSectionCorner.Parent = visualSection

local visualTitle = Instance.new("TextLabel")
visualTitle.Parent = visualSection
visualTitle.Size = UDim2.new(1,-20,0,40)
visualTitle.Position = UDim2.new(0,15,0,10)
visualTitle.BackgroundTransparency = 1
visualTitle.Text = "Visual Settings"
visualTitle.Font = Enum.Font.GothamBold
visualTitle.TextSize = 22
visualTitle.TextColor3 = Color3.new(1,1,1)
visualTitle.TextXAlignment = Enum.TextXAlignment.Left

-- Settings Tab
local settingsTab = Instance.new("ScrollingFrame")
settingsTab.Parent = contentArea
settingsTab.Size = UDim2.new(1,0,1,0)
settingsTab.BackgroundTransparency = 1
settingsTab.BorderSizePixel = 0
settingsTab.CanvasSize = UDim2.new(0,0,0,200)
settingsTab.ScrollBarThickness = 4
settingsTab.Visible = false

local settingsSection = Instance.new("Frame")
settingsSection.Parent = settingsTab
settingsSection.Size = UDim2.new(1,-20,0,150)
settingsSection.Position = UDim2.new(0,10,0,10)
settingsSection.BackgroundColor3 = Color3.fromRGB(22,22,22)
settingsSection.BorderSizePixel = 0
local settingsSectionCorner = Instance.new("UICorner")
settingsSectionCorner.CornerRadius = UDim.new(0,14)
settingsSectionCorner.Parent = settingsSection

local settingsTitle = Instance.new("TextLabel")
settingsTitle.Parent = settingsSection
settingsTitle.Size = UDim2.new(1,-20,0,40)
settingsTitle.Position = UDim2.new(0,15,0,10)
settingsTitle.BackgroundTransparency = 1
settingsTitle.Text = "Settings"
settingsTitle.Font = Enum.Font.GothamBold
settingsTitle.TextSize = 22
settingsTitle.TextColor3 = Color3.new(1,1,1)
settingsTitle.TextXAlignment = Enum.TextXAlignment.Left

tabContainers["Player"] = playerTab
tabContainers["Movement"] = movementTab
tabContainers["Combat"] = combatTab
tabContainers["Visual"] = visualTab
tabContainers["Settings"] = settingsTab

-- Tab click handlers
playerTabBtn.MouseButton1Click:Connect(function() switchTab("Player") end)
movementTabBtn.MouseButton1Click:Connect(function() switchTab("Movement") end)
combatTabBtn.MouseButton1Click:Connect(function() switchTab("Combat") end)
visualTabBtn.MouseButton1Click:Connect(function() switchTab("Visual") end)
settingsTabBtn.MouseButton1Click:Connect(function() switchTab("Settings") end)

switchTab("Player")

----------------------------------------------------
-- HELPER FUNCTIONS
----------------------------------------------------

local function createToggle(parent, text, y, callback)
	local holder = Instance.new("Frame")
	holder.Parent = parent
	holder.Size = UDim2.new(1,-30,0,35)
	holder.Position = UDim2.new(0,15,0,y)
	holder.BackgroundColor3 = Color3.fromRGB(30,30,30)
	holder.BorderSizePixel = 0
	local holderCorner = Instance.new("UICorner")
	holderCorner.CornerRadius = UDim.new(0,10)
	holderCorner.Parent = holder
	
	local label = Instance.new("TextLabel")
	label.Parent = holder
	label.Size = UDim2.new(0.7,0,1,0)
	label.Position = UDim2.new(0,15,0,0)
	label.BackgroundTransparency = 1
	label.Text = text
	label.Font = Enum.Font.GothamBold
	label.TextSize = 16
	label.TextColor3 = Color3.new(1,1,1)
	label.TextXAlignment = Enum.TextXAlignment.Left
	
	local toggle = Instance.new("TextButton")
	toggle.Parent = holder
	toggle.Size = UDim2.new(0,55,0,28)
	toggle.Position = UDim2.new(1,-75,0.5,-14)
	toggle.Text = ""
	toggle.BackgroundColor3 = Color3.fromRGB(50,50,50)
	
	local toggleCorner = Instance.new("UICorner")
	toggleCorner.CornerRadius = UDim.new(1,0)
	toggleCorner.Parent = toggle
	
	local circle = Instance.new("Frame")
	circle.Parent = toggle
	circle.Size = UDim2.new(0,22,0,22)
	circle.Position = UDim2.new(0,3,0.5,-11)
	circle.BackgroundColor3 = Color3.new(1,1,1)
	
	local circleCorner = Instance.new("UICorner")
	circleCorner.CornerRadius = UDim.new(1,0)
	circleCorner.Parent = circle
	
	local state = false
	
	toggle.MouseButton1Click:Connect(function()
		state = not state
		if state then
			TweenService:Create(circle, TweenInfo.new(0.2), {Position = UDim2.new(1,-25,0.5,-11)}):Play()
			toggle.BackgroundColor3 = Color3.fromRGB(0,170,90)
		else
			TweenService:Create(circle, TweenInfo.new(0.2), {Position = UDim2.new(0,3,0.5,-11)}):Play()
			toggle.BackgroundColor3 = Color3.fromRGB(50,50,50)
		end
		callback(state)
	end)
	
	return {toggle, circle}
end

----------------------------------------------------
-- FLY FUNCTIONS
----------------------------------------------------

local function stopFly()
	local character = player.Character
	if not character then return end
	local hrp = character:FindFirstChild("HumanoidRootPart")
	if not hrp then return end
	local bv = hrp:FindFirstChild("AutoFlyVelocity")
	if bv then bv:Destroy() end
	local bg = hrp:FindFirstChild("AutoFlyGyro")
	if bg then bg:Destroy() end
end

local function equipWeapons()
	local character = player.Character
	if not character then return end
	local humanoid = character:FindFirstChildOfClass("Humanoid")
	local backpack = player:FindFirstChild("Backpack")
	if not humanoid or not backpack then return end
	for _, tool in ipairs(backpack:GetChildren()) do
		if tool:IsA("Tool") then
			humanoid:EquipTool(tool)
		end
	end
	task.delay(2, function()
		if humanoid then humanoid:UnequipTools() end
	end)
end

local function flyTo(position)
	flying = false
	task.wait()
	flying = true
	local character = player.Character
	if not character then flying = false return end
	local hrp = character:FindFirstChild("HumanoidRootPart")
	if not hrp then flying = false return end
	stopFly()
	
	local bv = Instance.new("BodyVelocity")
	bv.Name = "AutoFlyVelocity"
	bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
	bv.Velocity = Vector3.zero
	bv.Parent = hrp
	
	local bg = Instance.new("BodyGyro")
	bg.Name = "AutoFlyGyro"
	bg.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
	bg.P = 50000
	bg.CFrame = hrp.CFrame
	bg.Parent = hrp
	
	while flying do
		local distance = (position - hrp.Position).Magnitude
		if distance <= 5 then break end
		local direction = (position - hrp.Position).Unit
		bv.Velocity = direction * flySpeed
		bg.CFrame = CFrame.new(hrp.Position, position)
		RunService.RenderStepped:Wait()
	end
	stopFly()
	flying = false
end

----------------------------------------------------
-- MOVEMENT TAB CONTROLS
----------------------------------------------------

-- Walk Speed Slider
local wsLabel = Instance.new("TextLabel")
wsLabel.Parent = movementSection
wsLabel.Size = UDim2.new(1,-30,0,30)
wsLabel.Position = UDim2.new(0,15,0,60)
wsLabel.BackgroundTransparency = 1
wsLabel.Text = "Walk Speed: 16"
wsLabel.Font = Enum.Font.GothamBold
wsLabel.TextSize = 16
wsLabel.TextColor3 = Color3.new(1,1,1)
wsLabel.TextXAlignment = Enum.TextXAlignment.Left

local wsBox = Instance.new("TextBox")
wsBox.Parent = movementSection
wsBox.Size = UDim2.new(0,100,0,35)
wsBox.Position = UDim2.new(0,15,0,95)
wsBox.BackgroundColor3 = Color3.fromRGB(35,35,35)
wsBox.TextColor3 = Color3.new(1,1,1)
wsBox.Font = Enum.Font.GothamBold
wsBox.TextSize = 16
wsBox.Text = "16"
local wsBoxCorner = Instance.new("UICorner")
wsBoxCorner.CornerRadius = UDim.new(0,10)
wsBoxCorner.Parent = wsBox

local wsSliderBack = Instance.new("Frame")
wsSliderBack.Parent = movementSection
wsSliderBack.Size = UDim2.new(0,250,0,8)
wsSliderBack.Position = UDim2.new(0,130,0,108)
wsSliderBack.BackgroundColor3 = Color3.fromRGB(45,45,45)
wsSliderBack.BorderSizePixel = 0
local wsSliderBackCorner = Instance.new("UICorner")
wsSliderBackCorner.CornerRadius = UDim.new(1,0)
wsSliderBackCorner.Parent = wsSliderBack

local wsSliderFill = Instance.new("Frame")
wsSliderFill.Parent = wsSliderBack
wsSliderFill.Size = UDim2.new(0.1,0,1,0)
wsSliderFill.BackgroundColor3 = Color3.fromRGB(0,170,255)
wsSliderFill.BorderSizePixel = 0
local wsSliderFillCorner = Instance.new("UICorner")
wsSliderFillCorner.CornerRadius = UDim.new(1,0)
wsSliderFillCorner.Parent = wsSliderFill

local wsSliderKnob = Instance.new("Frame")
wsSliderKnob.Parent = wsSliderBack
wsSliderKnob.Size = UDim2.new(0,18,0,18)
wsSliderKnob.AnchorPoint = Vector2.new(0.5,0.5)
wsSliderKnob.Position = UDim2.new(0.1,0,0.5,0)
wsSliderKnob.BackgroundColor3 = Color3.new(1,1,1)
local wsKnobCorner = Instance.new("UICorner")
wsKnobCorner.CornerRadius = UDim.new(1,0)
wsKnobCorner.Parent = wsSliderKnob

local draggingWS = false

local function updateWalkSpeed(value)
	walkingSpeed = math.clamp(value, 14, 33)
	wsLabel.Text = "Walk Speed: " .. walkingSpeed
	wsBox.Text = tostring(walkingSpeed)
	local pos = (walkingSpeed - 14) / 19
	wsSliderFill.Size = UDim2.new(pos,0,1,0)
	wsSliderKnob.Position = UDim2.new(pos,0,0.5,0)
	local character = player.Character
	local humanoid = character and character:FindFirstChildOfClass("Humanoid")
	if humanoid then humanoid.WalkSpeed = walkingSpeed end
end

wsSliderBack.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		draggingWS = true
		local pos = math.clamp((input.Position.X - wsSliderBack.AbsolutePosition.X) / wsSliderBack.AbsoluteSize.X, 0, 1)
		local speed = math.floor(14 + (pos * 19))
		updateWalkSpeed(speed)
	end
end)

UIS.InputChanged:Connect(function(input)
	if draggingWS and input.UserInputType == Enum.UserInputType.MouseMovement then
		local pos = math.clamp((input.Position.X - wsSliderBack.AbsolutePosition.X) / wsSliderBack.AbsoluteSize.X, 0, 1)
		local speed = math.floor(14 + (pos * 19))
		updateWalkSpeed(speed)
	end
end)

UIS.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		draggingWS = false
	end
end)

wsBox.FocusLost:Connect(function()
	local num = tonumber(wsBox.Text)
	if num then updateWalkSpeed(num) else wsBox.Text = tostring(walkingSpeed) end
end)

-- Fly Speed Slider
local fsLabel = Instance.new("TextLabel")
fsLabel.Parent = movementSection
fsLabel.Size = UDim2.new(1,-30,0,30)
fsLabel.Position = UDim2.new(0,15,0,160)
fsLabel.BackgroundTransparency = 1
fsLabel.Text = "Fly Speed: 75"
fsLabel.Font = Enum.Font.GothamBold
fsLabel.TextSize = 16
fsLabel.TextColor3 = Color3.new(1,1,1)
fsLabel.TextXAlignment = Enum.TextXAlignment.Left

local fsBox = Instance.new("TextBox")
fsBox.Parent = movementSection
fsBox.Size = UDim2.new(0,100,0,35)
fsBox.Position = UDim2.new(0,15,0,195)
fsBox.BackgroundColor3 = Color3.fromRGB(35,35,35)
fsBox.TextColor3 = Color3.new(1,1,1)
fsBox.Font = Enum.Font.GothamBold
fsBox.TextSize = 16
fsBox.Text = "75"
local fsBoxCorner = Instance.new("UICorner")
fsBoxCorner.CornerRadius = UDim.new(0,10)
fsBoxCorner.Parent = fsBox

local fsSliderBack = Instance.new("Frame")
fsSliderBack.Parent = movementSection
fsSliderBack.Size = UDim2.new(0,250,0,8)
fsSliderBack.Position = UDim2.new(0,130,0,208)
fsSliderBack.BackgroundColor3 = Color3.fromRGB(45,45,45)
fsSliderBack.BorderSizePixel = 0
local fsSliderBackCorner = Instance.new("UICorner")
fsSliderBackCorner.CornerRadius = UDim.new(1,0)
fsSliderBackCorner.Parent = fsSliderBack

local fsSliderFill = Instance.new("Frame")
fsSliderFill.Parent = fsSliderBack
fsSliderFill.Size = UDim2.new(0.25,0,1,0)
fsSliderFill.BackgroundColor3 = Color3.fromRGB(0,170,255)
fsSliderFill.BorderSizePixel = 0
local fsSliderFillCorner = Instance.new("UICorner")
fsSliderFillCorner.CornerRadius = UDim.new(1,0)
fsSliderFillCorner.Parent = fsSliderFill

local fsSliderKnob = Instance.new("Frame")
fsSliderKnob.Parent = fsSliderBack
fsSliderKnob.Size = UDim2.new(0,18,0,18)
fsSliderKnob.AnchorPoint = Vector2.new(0.5,0.5)
fsSliderKnob.Position = UDim2.new(0.25,0,0.5,0)
fsSliderKnob.BackgroundColor3 = Color3.new(1,1,1)
local fsKnobCorner = Instance.new("UICorner")
fsKnobCorner.CornerRadius = UDim.new(1,0)
fsKnobCorner.Parent = fsSliderKnob

local draggingFS = false

local function updateFlySpeed(value)
	flySpeed = math.clamp(value, 0, 300)
	fsLabel.Text = "Fly Speed: " .. flySpeed
	fsBox.Text = tostring(flySpeed)
	local pos = flySpeed / 300
	fsSliderFill.Size = UDim2.new(pos,0,1,0)
	fsSliderKnob.Position = UDim2.new(pos,0,0.5,0)
end

fsSliderBack.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		draggingFS = true
		local pos = math.clamp((input.Position.X - fsSliderBack.AbsolutePosition.X) / fsSliderBack.AbsoluteSize.X, 0, 1)
		updateFlySpeed(math.floor(pos * 300))
	end
end)

UIS.InputChanged:Connect(function(input)
	if draggingFS and input.UserInputType == Enum.UserInputType.MouseMovement then
		local pos = math.clamp((input.Position.X - fsSliderBack.AbsolutePosition.X) / fsSliderBack.AbsoluteSize.X, 0, 1)
		updateFlySpeed(math.floor(pos * 300))
	end
end)

UIS.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		draggingFS = false
	end
end)

fsBox.FocusLost:Connect(function()
	local num = tonumber(fsBox.Text)
	if num then updateFlySpeed(num) else fsBox.Text = tostring(flySpeed) end
end)

-- Noclip Toggle
local noclipState = false
local noclipConnection = nil

local noclipHolder = Instance.new("Frame")
noclipHolder.Parent = movementSection
noclipHolder.Size = UDim2.new(1,-30,0,35)
noclipHolder.Position = UDim2.new(0,15,0,260)
noclipHolder.BackgroundColor3 = Color3.fromRGB(30,30,30)
noclipHolder.BorderSizePixel = 0
local noclipCorner = Instance.new("UICorner")
noclipCorner.CornerRadius = UDim.new(0,10)
noclipCorner.Parent = noclipHolder

local noclipLabel = Instance.new("TextLabel")
noclipLabel.Parent = noclipHolder
noclipLabel.Size = UDim2.new(0.7,0,1,0)
noclipLabel.Position = UDim2.new(0,15,0,0)
noclipLabel.BackgroundTransparency = 1
noclipLabel.Text = "Noclip"
noclipLabel.Font = Enum.Font.GothamBold
noclipLabel.TextSize = 16
noclipLabel.TextColor3 = Color3.new(1,1,1)
noclipLabel.TextXAlignment = Enum.TextXAlignment.Left

local noclipToggle = Instance.new("TextButton")
noclipToggle.Parent = noclipHolder
noclipToggle.Size = UDim2.new(0,55,0,28)
noclipToggle.Position = UDim2.new(1,-75,0.5,-14)
noclipToggle.Text = ""
noclipToggle.BackgroundColor3 = Color3.fromRGB(50,50,50)
local noclipToggleCorner = Instance.new("UICorner")
noclipToggleCorner.CornerRadius = UDim.new(1,0)
noclipToggleCorner.Parent = noclipToggle

local noclipCircle = Instance.new("Frame")
noclipCircle.Parent = noclipToggle
noclipCircle.Size = UDim2.new(0,22,0,22)
noclipCircle.Position = UDim2.new(0,3,0.5,-11)
noclipCircle.BackgroundColor3 = Color3.new(1,1,1)
local noclipCircleCorner = Instance.new("UICorner")
noclipCircleCorner.CornerRadius = UDim.new(1,0)
noclipCircleCorner.Parent = noclipCircle

noclipToggle.MouseButton1Click:Connect(function()
	noclipState = not noclipState
	if noclipState then
		TweenService:Create(noclipCircle, TweenInfo.new(0.2), {Position = UDim2.new(1,-25,0.5,-11)}):Play()
		noclipToggle.BackgroundColor3 = Color3.fromRGB(0,170,90)
		local character = player.Character
		if character then
			if noclipConnection then noclipConnection:Disconnect() end
			noclipConnection = RunService.Stepped:Connect(function()
				for _, part in ipairs(character:GetDescendants()) do
					if part:IsA("BasePart") then part.CanCollide = false end
				end
			end)
		end
	else
		TweenService:Create(noclipCircle, TweenInfo.new(0.2), {Position = UDim2.new(0,3,0.5,-11)}):Play()
		noclipToggle.BackgroundColor3 = Color3.fromRGB(50,50,50)
		if noclipConnection then
			noclipConnection:Disconnect()
			noclipConnection = nil
		end
		local character = player.Character
		if character then
			for _, part in ipairs(character:GetDescendants()) do
				if part:IsA("BasePart") then part.CanCollide = true end
			end
		end
	end
end)

-- Auto Fly Toggle
local autoFlyState = false

local autoFlyHolder = Instance.new("Frame")
autoFlyHolder.Parent = movementSection
autoFlyHolder.Size = UDim2.new(1,-30,0,35)
autoFlyHolder.Position = UDim2.new(0,15,0,305)
autoFlyHolder.BackgroundColor3 = Color3.fromRGB(30,30,30)
autoFlyHolder.BorderSizePixel = 0
local autoFlyCorner = Instance.new("UICorner")
autoFlyCorner.CornerRadius = UDim.new(0,10)
autoFlyCorner.Parent = autoFlyHolder

local autoFlyLabel = Instance.new("TextLabel")
autoFlyLabel.Parent = autoFlyHolder
autoFlyLabel.Size = UDim2.new(0.7,0,1,0)
autoFlyLabel.Position = UDim2.new(0,15,0,0)
autoFlyLabel.BackgroundTransparency = 1
autoFlyLabel.Text = "Auto Fly (Collect Items)"
autoFlyLabel.Font = Enum.Font.GothamBold
autoFlyLabel.TextSize = 16
autoFlyLabel.TextColor3 = Color3.new(1,1,1)
autoFlyLabel.TextXAlignment = Enum.TextXAlignment.Left

local autoFlyToggle = Instance.new("TextButton")
autoFlyToggle.Parent = autoFlyHolder
autoFlyToggle.Size = UDim2.new(0,55,0,28)
autoFlyToggle.Position = UDim2.new(1,-75,0.5,-14)
autoFlyToggle.Text = ""
autoFlyToggle.BackgroundColor3 = Color3.fromRGB(50,50,50)
local autoFlyToggleCorner = Instance.new("UICorner")
autoFlyToggleCorner.CornerRadius = UDim.new(1,0)
autoFlyToggleCorner.Parent = autoFlyToggle

local autoFlyCircle = Instance.new("Frame")
autoFlyCircle.Parent = autoFlyToggle
autoFlyCircle.Size = UDim2.new(0,22,0,22)
autoFlyCircle.Position = UDim2.new(0,3,0.5,-11)
autoFlyCircle.BackgroundColor3 = Color3.new(1,1,1)
local autoFlyCircleCorner = Instance.new("UICorner")
autoFlyCircleCorner.CornerRadius = UDim.new(1,0)
autoFlyCircleCorner.Parent = autoFlyCircle

autoFlyToggle.MouseButton1Click:Connect(function()
	autoFlyState = not autoFlyState
	if autoFlyState then
		TweenService:Create(autoFlyCircle, TweenInfo.new(0.2), {Position = UDim2.new(1,-25,0.5,-11)}):Play()
		autoFlyToggle.BackgroundColor3 = Color3.fromRGB(0,170,90)
		enabled = true
		local character = player.Character
		local hrp = character and character:FindFirstChild("HumanoidRootPart")
		if hrp then returnPosition = hrp.Position end
		equipWeapons()
	else
		TweenService:Create(autoFlyCircle, TweenInfo.new(0.2), {Position = UDim2.new(0,3,0.5,-11)}):Play()
		autoFlyToggle.BackgroundColor3 = Color3.fromRGB(50,50,50)
		enabled = false
		flying = false
		stopFly()
	end
end)

-- Save Base Buttons
local saveBase = Instance.new("TextButton")
saveBase.Parent = movementSection
saveBase.Size = UDim2.new(0,170,0,40)
saveBase.Position = UDim2.new(0,15,0,360)
saveBase.BackgroundColor3 = Color3.fromRGB(40,40,40)
saveBase.Text = "Save Base"
saveBase.TextColor3 = Color3.new(1,1,1)
saveBase.Font = Enum.Font.GothamBold
saveBase.TextSize = 15
local saveCorner = Instance.new("UICorner")
saveCorner.CornerRadius = UDim.new(0,10)
saveCorner.Parent = saveBase

saveBase.MouseButton1Click:Connect(function()
	local character = player.Character
	if not character then return end
	local hrp = character:FindFirstChild("HumanoidRootPart")
	if not hrp then return end
	baseEnabled = not baseEnabled
	if baseEnabled then
		basePosition = hrp.Position
		saveBase.Text = "GOTO BASE : ON"
		saveBase.BackgroundColor3 = Color3.fromRGB(0,170,90)
	else
		saveBase.Text = "Save Base"
		saveBase.BackgroundColor3 = Color3.fromRGB(40,40,40)
	end
end)

local nowButton = Instance.new("TextButton")
nowButton.Parent = movementSection
nowButton.Size = UDim2.new(0,170,0,40)
nowButton.Position = UDim2.new(0,200,0,360)
nowButton.BackgroundColor3 = Color3.fromRGB(0,170,255)
nowButton.Text = "NOW"
nowButton.TextColor3 = Color3.new(1,1,1)
nowButton.Font = Enum.Font.GothamBold
nowButton.TextSize = 15
local nowCorner = Instance.new("UICorner")
nowCorner.CornerRadius = UDim.new(0,10)
nowCorner.Parent = nowButton

nowButton.MouseButton1Click:Connect(function()
	enabled = false
	flying = false
	stopFly()
	if baseEnabled and basePosition then flyTo(basePosition) end
end)

local stopButton = Instance.new("TextButton")
stopButton.Parent = movementSection
stopButton.Size = UDim2.new(0,170,0,40)
stopButton.Position = UDim2.new(0,385,0,360)
stopButton.BackgroundColor3 = Color3.fromRGB(255,70,70)
stopButton.Text = "STOP"
stopButton.TextColor3 = Color3.new(1,1,1)
stopButton.Font = Enum.Font.GothamBold
stopButton.TextSize = 15
local stopCorner = Instance.new("UICorner")
stopCorner.CornerRadius = UDim.new(0,10)
stopCorner.Parent = stopButton

stopButton.MouseButton1Click:Connect(function()
	enabled = false
	flying = false
	stopFly()
end)

----------------------------------------------------
-- ZOMBIE KILL AURA (FULLY WORKING - FROM SCRAP SCRIPT)
----------------------------------------------------

local autoHitEnabled = false
local lastHitTime = 0
local HITS_PER_SECOND = 10
local MAX_SPEED = 50
local MIN_SPEED = 10
local killRange = 10  -- Added range control

-- Cache for weapon
local bat, swingRemote, hitTargetsRemote = nil, nil, nil
local lastBatUpdate = 0

-- Cache for zombie
local cachedZombiePart = nil
local cachedZombieDist = 999
local lastZombieScan = 0
local SCAN_INTERVAL = 0.35

-- Try to get Hitbox module (same as working script)
local Hitbox = nil
local hitboxSuccess = pcall(function()
    Hitbox = require(game.ReplicatedStorage.Modules.Combat.Hitbox)
end)

if not hitboxSuccess then
    -- Fallback hitbox
    Hitbox = {
        RadiusHitbox = function(cframe, radius, angle, character)
            local results = {}
            local hrp = character and character:FindFirstChild("HumanoidRootPart")
            if not hrp then return results end
            for _, obj in ipairs(workspace:GetDescendants()) do
                if obj:IsA("Model") and obj ~= character then
                    local humanoid = obj:FindFirstChildOfClass("Humanoid")
                    local part = obj:FindFirstChild("HumanoidRootPart") or obj:FindFirstChild("Head")
                    if humanoid and humanoid.Health > 0 and part then
                        local dist = (hrp.Position - part.Position).Magnitude
                        if dist <= radius then table.insert(results, part) end
                    end
                end
            end
            return results
        end
    }
end

local function updateBat()
    local now = tick()
    if now - lastBatUpdate < 0.2 then 
        return bat, swingRemote, hitTargetsRemote
    end
    lastBatUpdate = now
    
    local char = player.Character
    if not char then 
        bat, swingRemote, hitTargetsRemote = nil, nil, nil
        return bat, swingRemote, hitTargetsRemote
    end
    
    bat = char:FindFirstChildWhichIsA("Tool")
    if bat then
        swingRemote = bat:FindFirstChild("Swing")
        hitTargetsRemote = bat:FindFirstChild("HitTargets")
    else
        swingRemote, hitTargetsRemote = nil, nil
    end
    
    return bat, swingRemote, hitTargetsRemote
end

local function findZombieCached()
    local now = tick()
    if now - lastZombieScan < SCAN_INTERVAL then
        return cachedZombiePart, cachedZombieDist
    end
    
    lastZombieScan = now
    cachedZombiePart = nil
    cachedZombieDist = 999
    
    local char = player.Character
    if not char then return nil, nil end
    
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if not hrp then return nil, nil end
    
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("Model") then
            local humanoid = obj:FindFirstChildOfClass("Humanoid")
            local targetPart = obj:FindFirstChild("HumanoidRootPart") or obj:FindFirstChild("Head") or obj:FindFirstChildWhichIsA("BasePart")
            
            -- Check for ZOMBIES
            if humanoid and humanoid.Health > 0 and targetPart then
                local name = obj.Name:lower()
                if name:find("zombie") or name:find("infected") or name:find("crawler") then
                    local dist = (hrp.Position - targetPart.Position).Magnitude
                    if dist < cachedZombieDist and dist <= killRange then
                        cachedZombieDist = dist
                        cachedZombiePart = targetPart
                    end
                end
            end
            
            -- Check for SCRAP PILE and BARREL
            if obj.Name == "Scrap Pile" or obj.Name == "Barrel" then
                local scrapPart = obj:FindFirstChildWhichIsA("BasePart")
                if scrapPart then
                    local dist = (hrp.Position - scrapPart.Position).Magnitude
                    if dist < cachedZombieDist and dist <= killRange then
                        cachedZombieDist = dist
                        cachedZombiePart = scrapPart
                    end
                end
            end
        end
    end
    
    return cachedZombiePart, cachedZombieDist
end

-- HIT FUNCTION (EXACT SAME AS WORKING SCRIPT)
local function doHit()
    local now = tick()
    local minInterval = 1 / HITS_PER_SECOND
    if now - lastHitTime < minInterval then return end
    lastHitTime = now
    
    local char = player.Character
    if not char then return end
    
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    
    local _, sRemote, htRemote = updateBat()
    if not sRemote or not htRemote then return end
    
    local zombiePart = findZombieCached()
    if not zombiePart then return end
    
    -- Swing
    pcall(function() sRemote:FireServer() end)
    
    -- Hitbox (same as working script)
    local hitResults = Hitbox.RadiusHitbox(hrp.CFrame, killRange, 180, char)
    
    if #hitResults > 0 then
        pcall(function() htRemote:FireServer(hitResults) end)
    else
        pcall(function() htRemote:FireServer({zombiePart}) end)
    end
end

local hitTimer = nil
local function startLoop()
    if hitTimer then return end
    hitTimer = RunService.Heartbeat:Connect(function()
        if autoHitEnabled then
            doHit()
        end
    end)
end

local function stopLoop()
    if hitTimer then
        hitTimer:Disconnect()
        hitTimer = nil
    end
end

-- GUI Elements for Combat Tab (CocoHub style)
local killAuraHolder = Instance.new("Frame")
killAuraHolder.Parent = combatSection
killAuraHolder.Size = UDim2.new(1,-30,0,40)
killAuraHolder.Position = UDim2.new(0,15,0,60)
killAuraHolder.BackgroundColor3 = Color3.fromRGB(30,30,30)
killAuraHolder.BorderSizePixel = 0
local killAuraCorner = Instance.new("UICorner")
killAuraCorner.CornerRadius = UDim.new(0,10)
killAuraCorner.Parent = killAuraHolder

local killAuraLabel = Instance.new("TextLabel")
killAuraLabel.Parent = killAuraHolder
killAuraLabel.Size = UDim2.new(0.7,0,1,0)
killAuraLabel.Position = UDim2.new(0,15,0,0)
killAuraLabel.BackgroundTransparency = 1
killAuraLabel.Text = "� Zombie Kill Aura"
killAuraLabel.Font = Enum.Font.GothamBold
killAuraLabel.TextSize = 16
killAuraLabel.TextColor3 = Color3.new(1,1,1)
killAuraLabel.TextXAlignment = Enum.TextXAlignment.Left

local killAuraToggle = Instance.new("TextButton")
killAuraToggle.Parent = killAuraHolder
killAuraToggle.Size = UDim2.new(0,55,0,30)
killAuraToggle.Position = UDim2.new(1,-75,0.5,-15)
killAuraToggle.Text = ""
killAuraToggle.BackgroundColor3 = Color3.fromRGB(50,50,50)
local killAuraToggleCorner = Instance.new("UICorner")
killAuraToggleCorner.CornerRadius = UDim.new(1,0)
killAuraToggleCorner.Parent = killAuraToggle

local killAuraCircle = Instance.new("Frame")
killAuraCircle.Parent = killAuraToggle
killAuraCircle.Size = UDim2.new(0,24,0,24)
killAuraCircle.Position = UDim2.new(0,3,0.5,-12)
killAuraCircle.BackgroundColor3 = Color3.new(1,1,1)
local killAuraCircleCorner = Instance.new("UICorner")
killAuraCircleCorner.CornerRadius = UDim.new(1,0)
killAuraCircleCorner.Parent = killAuraCircle

killAuraToggle.MouseButton1Click:Connect(function()
    autoHitEnabled = not autoHitEnabled
    if autoHitEnabled then
        TweenService:Create(killAuraCircle, TweenInfo.new(0.2), {Position = UDim2.new(1,-27,0.5,-12)}):Play()
        killAuraToggle.BackgroundColor3 = Color3.fromRGB(0,170,90)
        killAuraLabel.TextColor3 = Color3.fromRGB(0,255,100)
        startLoop()
        print("� Kill Aura ON")
    else
        TweenService:Create(killAuraCircle, TweenInfo.new(0.2), {Position = UDim2.new(0,3,0.5,-12)}):Play()
        killAuraToggle.BackgroundColor3 = Color3.fromRGB(50,50,50)
        killAuraLabel.TextColor3 = Color3.new(1,1,1)
        stopLoop()
        print("� Kill Aura OFF")
    end
end)

-- Speed Control
local speedLabel = Instance.new("TextLabel")
speedLabel.Parent = combatSection
speedLabel.Size = UDim2.new(1,-30,0,30)
speedLabel.Position = UDim2.new(0,15,0,115)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "⚡ Speed: " .. HITS_PER_SECOND .. " hits/sec"
speedLabel.Font = Enum.Font.GothamBold
speedLabel.TextSize = 14
speedLabel.TextColor3 = Color3.fromRGB(0,255,100)
speedLabel.TextXAlignment = Enum.TextXAlignment.Left

local speedSliderBack = Instance.new("Frame")
speedSliderBack.Parent = combatSection
speedSliderBack.Size = UDim2.new(0,250,0,8)
speedSliderBack.Position = UDim2.new(0,15,0,150)
speedSliderBack.BackgroundColor3 = Color3.fromRGB(45,45,45)
speedSliderBack.BorderSizePixel = 0
local speedSliderBackCorner = Instance.new("UICorner")
speedSliderBackCorner.CornerRadius = UDim.new(1,0)
speedSliderBackCorner.Parent = speedSliderBack

local speedSliderFill = Instance.new("Frame")
speedSliderFill.Parent = speedSliderBack
speedSliderFill.Size = UDim2.new((HITS_PER_SECOND - MIN_SPEED) / (MAX_SPEED - MIN_SPEED),0,1,0)
speedSliderFill.BackgroundColor3 = Color3.fromRGB(255,0,100)
speedSliderFill.BorderSizePixel = 0
local speedSliderFillCorner = Instance.new("UICorner")
speedSliderFillCorner.CornerRadius = UDim.new(1,0)
speedSliderFillCorner.Parent = speedSliderFill

local speedSliderKnob = Instance.new("Frame")
speedSliderKnob.Parent = speedSliderBack
speedSliderKnob.Size = UDim2.new(0,18,0,18)
speedSliderKnob.AnchorPoint = Vector2.new(0.5,0.5)
speedSliderKnob.Position = UDim2.new((HITS_PER_SECOND - MIN_SPEED) / (MAX_SPEED - MIN_SPEED),0,0.5,0)
speedSliderKnob.BackgroundColor3 = Color3.new(1,1,1)
local speedKnobCorner = Instance.new("UICorner")
speedKnobCorner.CornerRadius = UDim.new(1,0)
speedKnobCorner.Parent = speedSliderKnob

local draggingSpeed = false

local function updateKillSpeed(value)
    HITS_PER_SECOND = math.clamp(value, MIN_SPEED, MAX_SPEED)
    speedLabel.Text = "⚡ Speed: " .. HITS_PER_SECOND .. " hits/sec"
    local pos = (HITS_PER_SECOND - MIN_SPEED) / (MAX_SPEED - MIN_SPEED)
    speedSliderFill.Size = UDim2.new(pos,0,1,0)
    speedSliderKnob.Position = UDim2.new(pos,0,0.5,0)
    
    if HITS_PER_SECOND >= 40 then
        speedLabel.TextColor3 = Color3.fromRGB(255,0,0)
        speedSliderFill.BackgroundColor3 = Color3.fromRGB(255,0,0)
    elseif HITS_PER_SECOND >= 30 then
        speedLabel.TextColor3 = Color3.fromRGB(255,100,0)
        speedSliderFill.BackgroundColor3 = Color3.fromRGB(255,100,0)
    else
        speedLabel.TextColor3 = Color3.fromRGB(0,255,100)
        speedSliderFill.BackgroundColor3 = Color3.fromRGB(255,0,100)
    end
end

speedSliderBack.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        draggingSpeed = true
        local pos = math.clamp((input.Position.X - speedSliderBack.AbsolutePosition.X) / speedSliderBack.AbsoluteSize.X, 0, 1)
        updateKillSpeed(math.floor(MIN_SPEED + (pos * (MAX_SPEED - MIN_SPEED))))
    end
end)

UIS.InputChanged:Connect(function(input)
    if draggingSpeed and input.UserInputType == Enum.UserInputType.MouseMovement then
        local pos = math.clamp((input.Position.X - speedSliderBack.AbsolutePosition.X) / speedSliderBack.AbsoluteSize.X, 0, 1)
        updateKillSpeed(math.floor(MIN_SPEED + (pos * (MAX_SPEED - MIN_SPEED))))
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then draggingSpeed = false end
end)

-- Range Control
local rangeLabel = Instance.new("TextLabel")
rangeLabel.Parent = combatSection
rangeLabel.Size = UDim2.new(1,-30,0,30)
rangeLabel.Position = UDim2.new(0,15,0,195)
rangeLabel.BackgroundTransparency = 1
rangeLabel.Text = "� Range: " .. killRange .. " studs"
rangeLabel.Font = Enum.Font.GothamBold
rangeLabel.TextSize = 14
rangeLabel.TextColor3 = Color3.fromRGB(100,200,255)
rangeLabel.TextXAlignment = Enum.TextXAlignment.Left

local rangeSliderBack = Instance.new("Frame")
rangeSliderBack.Parent = combatSection
rangeSliderBack.Size = UDim2.new(0,250,0,8)
rangeSliderBack.Position = UDim2.new(0,15,0,230)
rangeSliderBack.BackgroundColor3 = Color3.fromRGB(45,45,45)
rangeSliderBack.BorderSizePixel = 0
local rangeSliderBackCorner = Instance.new("UICorner")
rangeSliderBackCorner.CornerRadius = UDim.new(1,0)
rangeSliderBackCorner.Parent = rangeSliderBack

local rangeSliderFill = Instance.new("Frame")
rangeSliderFill.Parent = rangeSliderBack
rangeSliderFill.Size = UDim2.new(killRange/30,0,1,0)
rangeSliderFill.BackgroundColor3 = Color3.fromRGB(100,200,255)
rangeSliderFill.BorderSizePixel = 0
local rangeSliderFillCorner = Instance.new("UICorner")
rangeSliderFillCorner.CornerRadius = UDim.new(1,0)
rangeSliderFillCorner.Parent = rangeSliderFill

local rangeSliderKnob = Instance.new("Frame")
rangeSliderKnob.Parent = rangeSliderBack
rangeSliderKnob.Size = UDim2.new(0,18,0,18)
rangeSliderKnob.AnchorPoint = Vector2.new(0.5,0.5)
rangeSliderKnob.Position = UDim2.new(killRange/30,0,0.5,0)
rangeSliderKnob.BackgroundColor3 = Color3.new(1,1,1)
local rangeKnobCorner = Instance.new("UICorner")
rangeKnobCorner.CornerRadius = UDim.new(1,0)
rangeKnobCorner.Parent = rangeSliderKnob

local draggingRange = false

local function updateKillRange(value)
    killRange = math.clamp(value, 5, 30)
    rangeLabel.Text = "� Range: " .. killRange .. " studs"
    local pos = killRange / 30
    rangeSliderFill.Size = UDim2.new(pos,0,1,0)
    rangeSliderKnob.Position = UDim2.new(pos,0,0.5,0)
end

rangeSliderBack.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        draggingRange = true
        local pos = math.clamp((input.Position.X - rangeSliderBack.AbsolutePosition.X) / rangeSliderBack.AbsoluteSize.X, 0, 1)
        updateKillRange(math.floor(pos * 30))
    end
end)

UIS.InputChanged:Connect(function(input)
    if draggingRange and input.UserInputType == Enum.UserInputType.MouseMovement then
        local pos = math.clamp((input.Position.X - rangeSliderBack.AbsolutePosition.X) / rangeSliderBack.AbsoluteSize.X, 0, 1)
        updateKillRange(math.floor(pos * 30))
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then draggingRange = false end
end)

-- Keybind Info
local keybindLabel = Instance.new("TextLabel")
keybindLabel.Parent = combatSection
keybindLabel.Size = UDim2.new(1,-30,0,30)
keybindLabel.Position = UDim2.new(0,15,0,275)
keybindLabel.BackgroundTransparency = 1
keybindLabel.Text = "� K = Toggle | +/- = Speed"
keybindLabel.Font = Enum.Font.Gotham
keybindLabel.TextSize = 12
keybindLabel.TextColor3 = Color3.fromRGB(150,150,150)
keybindLabel.TextXAlignment = Enum.TextXAlignment.Left

-- Keybind K
UIS.InputBegan:Connect(function(input, gp)
    if gp then return end
    if input.KeyCode == Enum.KeyCode.K then
        autoHitEnabled = not autoHitEnabled
        if autoHitEnabled then
            TweenService:Create(killAuraCircle, TweenInfo.new(0.2), {Position = UDim2.new(1,-27,0.5,-12)}):Play()
            killAuraToggle.BackgroundColor3 = Color3.fromRGB(0,170,90)
            killAuraLabel.TextColor3 = Color3.fromRGB(0,255,100)
            startLoop()
            print("� Kill Aura ON (K)")
        else
            TweenService:Create(killAuraCircle, TweenInfo.new(0.2), {Position = UDim2.new(0,3,0.5,-12)}):Play()
            killAuraToggle.BackgroundColor3 = Color3.fromRGB(50,50,50)
            killAuraLabel.TextColor3 = Color3.new(1,1,1)
            stopLoop()
            print("� Kill Aura OFF (K)")
        end
    end
end)

-- +/- keys for speed
UIS.InputBegan:Connect(function(input, gp)
    if gp then return end
    if autoHitEnabled then
        if input.KeyCode == Enum.KeyCode.Equals or input.KeyCode == Enum.KeyCode.KeypadPlus then
            updateKillSpeed(HITS_PER_SECOND + 1)
        elseif input.KeyCode == Enum.KeyCode.Minus or input.KeyCode == Enum.KeyCode.KeypadMinus then
            updateKillSpeed(HITS_PER_SECOND - 1)
        end
    end
end)

print("� Kill Aura Ready! Press K to toggle | +/- for speed")

----------------------------------------------------
-- MINI BUTTON & WINDOW CONTROLS
----------------------------------------------------

local miniButton = Instance.new("ImageButton")
miniButton.Parent = gui
miniButton.Size = UDim2.new(0,65,0,65)
miniButton.Position = UDim2.new(0,20,0.5,-32)
miniButton.BackgroundTransparency = 0
miniButton.BorderSizePixel = 0
miniButton.Visible = false
miniButton.Image = "rbxthumb://type=Asset&id=140648386437314&w=420&h=420"
miniButton.ScaleType = Enum.ScaleType.Fit
local miniCorner = Instance.new("UICorner")
miniCorner.CornerRadius = UDim.new(1,0)
miniCorner.Parent = miniButton

local minimizeButton = Instance.new("TextButton")
minimizeButton.Parent = frame
minimizeButton.Size = UDim2.new(0,34,0,34)
minimizeButton.Position = UDim2.new(1,-85,0,12)
minimizeButton.BackgroundColor3 = Color3.fromRGB(50,50,50)
minimizeButton.Text = "-"
minimizeButton.TextColor3 = Color3.new(1,1,1)
minimizeButton.Font = Enum.Font.GothamBold
minimizeButton.TextSize = 18
local minCorner = Instance.new("UICorner")
minCorner.CornerRadius = UDim.new(0,8)
minCorner.Parent = minimizeButton

minimizeButton.MouseButton1Click:Connect(function()
	minimized = true
	frame.Visible = false
	miniButton.Visible = true
end)

local closeButton = Instance.new("TextButton")
closeButton.Parent = frame
closeButton.Size = UDim2.new(0,34,0,34)
closeButton.Position = UDim2.new(1,-45,0,12)
closeButton.BackgroundColor3 = Color3.fromRGB(255,60,60)
closeButton.Text = "X"
closeButton.TextColor3 = Color3.new(1,1,1)
closeButton.Font = Enum.Font.GothamBold
closeButton.TextSize = 18
local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0,8)
closeCorner.Parent = closeButton

----------------------------------------------------
-- CLOSE CONFIRMATION
----------------------------------------------------

local darkBG = Instance.new("Frame")
darkBG.Parent = gui
darkBG.Size = UDim2.new(1,0,1,0)
darkBG.BackgroundColor3 = Color3.new(0,0,0)
darkBG.BackgroundTransparency = 0.4
darkBG.Visible = false
darkBG.ZIndex = 998

local confirmFrame = Instance.new("Frame")
confirmFrame.Parent = gui
confirmFrame.Size = UDim2.new(0,380,0,180)
confirmFrame.Position = UDim2.new(0.5,-190,0.5,-90)
confirmFrame.BackgroundColor3 = Color3.fromRGB(15,15,15)
confirmFrame.Visible = false
confirmFrame.ZIndex = 999
local confirmCorner = Instance.new("UICorner")
confirmCorner.CornerRadius = UDim.new(0,16)
confirmCorner.Parent = confirmFrame
local confirmStroke = Instance.new("UIStroke")
confirmStroke.Parent = confirmFrame
confirmStroke.Thickness = 2

local confirmTitle = Instance.new("TextLabel")
confirmTitle.Parent = confirmFrame
confirmTitle.BackgroundTransparency = 1
confirmTitle.Size = UDim2.new(1,0,0,50)
confirmTitle.Position = UDim2.new(0,0,0,15)
confirmTitle.Font = Enum.Font.GothamBold
confirmTitle.Text = "Are you sure you want to close?"
confirmTitle.TextSize = 22
confirmTitle.TextColor3 = Color3.fromRGB(255,255,255)
confirmTitle.ZIndex = 1000

local confirmLine = Instance.new("Frame")
confirmLine.Parent = confirmFrame
confirmLine.Size = UDim2.new(1,-40,0,2)
confirmLine.Position = UDim2.new(0,20,0,70)
confirmLine.BorderSizePixel = 0
confirmLine.BackgroundColor3 = Color3.new(1,1,1)
confirmLine.ZIndex = 1000
local lineCorner = Instance.new("UICorner")
lineCorner.CornerRadius = UDim.new(1,0)
lineCorner.Parent = confirmLine

local yesButton = Instance.new("TextButton")
yesButton.Parent = confirmFrame
yesButton.Size = UDim2.new(0,145,0,50)
yesButton.Position = UDim2.new(0,35,1,-70)
yesButton.BackgroundColor3 = Color3.fromRGB(35,35,35)
yesButton.Text = "Close"
yesButton.Font = Enum.Font.GothamBold
yesButton.TextSize = 22
yesButton.TextColor3 = Color3.new(1,1,1)
yesButton.ZIndex = 1000
local yesCorner = Instance.new("UICorner")
yesCorner.CornerRadius = UDim.new(0,12)
yesCorner.Parent = yesButton

local cancelButton = Instance.new("TextButton")
cancelButton.Parent = confirmFrame
cancelButton.Size = UDim2.new(0,145,0,50)
cancelButton.Position = UDim2.new(1,-180,1,-70)
cancelButton.BackgroundColor3 = Color3.fromRGB(35,35,35)
cancelButton.Text = "Cancel"
cancelButton.Font = Enum.Font.GothamBold
cancelButton.TextSize = 22
cancelButton.TextColor3 = Color3.new(1,1,1)
cancelButton.ZIndex = 1000
local cancelCorner = Instance.new("UICorner")
cancelCorner.CornerRadius = UDim.new(0,12)
cancelCorner.Parent = cancelButton

yesButton.MouseEnter:Connect(function()
	TweenService:Create(yesButton, TweenInfo.new(0.15), {BackgroundTransparency = 0.15}):Play()
end)
yesButton.MouseLeave:Connect(function()
	TweenService:Create(yesButton, TweenInfo.new(0.15), {BackgroundTransparency = 0}):Play()
end)
cancelButton.MouseEnter:Connect(function()
	TweenService:Create(cancelButton, TweenInfo.new(0.15), {BackgroundTransparency = 0.15}):Play()
end)
cancelButton.MouseLeave:Connect(function()
	TweenService:Create(cancelButton, TweenInfo.new(0.15), {BackgroundTransparency = 0}):Play()
end)

closeButton.MouseButton1Click:Connect(function()
	darkBG.Visible = true
	confirmFrame.Visible = true
	confirmFrame.Size = UDim2.new(0,320,0,150)
	TweenService:Create(confirmFrame, TweenInfo.new(0.25), {Size = UDim2.new(0,380,0,180)}):Play()
end)

cancelButton.MouseButton1Click:Connect(function()
	darkBG.Visible = false
	confirmFrame.Visible = false
end)

yesButton.MouseButton1Click:Connect(function()
	gui:Destroy()
	getgenv().CocoHub_Executed = false
end)

----------------------------------------------------
-- MAIN AUTO FLY LOOP
----------------------------------------------------

task.spawn(function()
	while true do
		if enabled then
			local foundItem = false
			for _, obj in ipairs(workspace:GetDescendants()) do
				if items[obj.Name] then
					local part = obj:IsA("BasePart") and obj or obj:FindFirstChildWhichIsA("BasePart", true)
					if part and enabled then
						foundItem = true
						flyTo(part.Position + Vector3.new(0,3,0))
						task.wait(0.2)
					end
				end
			end
			if enabled and not foundItem and returnPosition then
				flyTo(returnPosition)
			end
		end
		task.wait(1)
	end
end)

----------------------------------------------------
-- B KEY MINIMIZE
----------------------------------------------------

UIS.InputBegan:Connect(function(input, gp)
	if gp then return end
	if input.KeyCode == Enum.KeyCode.B then
		minimized = not minimized
		frame.Visible = not minimized
		miniButton.Visible = minimized
	end
end)

----------------------------------------------------
-- MINI BUTTON DRAG
----------------------------------------------------

local draggingMini = false
local dragStartMini, startPosMini, dragInputMini
local draggingLogo = false

local function updateMiniDrag(input)
	local delta = input.Position - dragStartMini
	if math.abs(delta.X) > 3 or math.abs(delta.Y) > 3 then draggingLogo = true end
	miniButton.Position = UDim2.new(startPosMini.X.Scale, startPosMini.X.Offset + delta.X, startPosMini.Y.Scale, startPosMini.Y.Offset + delta.Y)
end

miniButton.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		draggingMini = true
		draggingLogo = false
		dragStartMini = input.Position
		startPosMini = miniButton.Position
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				draggingMini = false
				task.wait(0.1)
				draggingLogo = false
			end
		end)
	end
end)

miniButton.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement then dragInputMini = input end
end)

UIS.InputChanged:Connect(function(input)
	if input == dragInputMini and draggingMini then updateMiniDrag(input) end
end)

miniButton.MouseButton1Click:Connect(function()
	if draggingLogo then return end
	frame.Visible = true
	miniButton.Visible = false
	minimized = false
end)

----------------------------------------------------
-- TOPBAR DRAGGING
----------------------------------------------------

local draggingFrame = false
local dragStartFrame, startPosFrame, dragInputFrame

topbar.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		draggingFrame = true
		dragStartFrame = input.Position
		startPosFrame = frame.Position
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then draggingFrame = false end
		end)
	end
end)

topbar.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement then dragInputFrame = input end
end)

UIS.InputChanged:Connect(function(input)
	if input == dragInputFrame and draggingFrame then
		local delta = input.Position - dragStartFrame
		frame.Position = UDim2.new(startPosFrame.X.Scale, startPosFrame.X.Offset + delta.X, startPosFrame.Y.Scale, startPosFrame.Y.Offset + delta.Y)
	end
end)

----------------------------------------------------
-- RAINBOW BORDERS
----------------------------------------------------

local miniStroke = Instance.new("UIStroke")
miniStroke.Parent = miniButton
miniStroke.Thickness = 4
miniStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
miniStroke.LineJoinMode = Enum.LineJoinMode.Round

task.spawn(function()
	while true do
		local hue = (tick() * 0.15) % 1
		local rainbow = Color3.fromHSV(hue, 1, 1)
		stroke.Color = rainbow
		confirmStroke.Color = rainbow
		confirmLine.BackgroundColor3 = rainbow
		yesButton.BackgroundColor3 = Color3.fromHSV(hue, 0.8, 0.55)
		cancelButton.BackgroundColor3 = Color3.fromHSV(hue, 0.8, 0.35)
		miniStroke.Color = rainbow
		titleLabel.TextColor3 = rainbow
		RunService.RenderStepped:Wait()
	end
end)

----------------------------------------------------
-- SCRAP PILE ESP (ADD TO VISUAL TAB)
----------------------------------------------------

-- Hanapin ang Visual Tab section at doon ilagay ang ESP
local function addESPToVisualTab()
    -- Wait para mag-load ang visualSection
    task.wait(0.5)
    
    if not visualSection then
        print("Visual section not found, retrying...")
        task.wait(1)
        if not visualSection then return end
    end
    
    local espEnabled = false
    local espHighlights = {}
    
    local function updateESP()
        for _, h in pairs(espHighlights) do
            pcall(function() h:Destroy() end)
        end
        espHighlights = {}
        if not espEnabled then return end
        
        local structures = workspace:FindFirstChild("Structures")
        if structures then
            for _, obj in ipairs(structures:GetChildren()) do
                if obj.Name == "Scrap Pile" then
                    local h = Instance.new("Highlight")
                    h.FillColor = Color3.fromRGB(255, 0, 0)
                    h.OutlineColor = Color3.fromRGB(255, 0, 0)
                    h.FillTransparency = 0.3
                    h.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                    h.Adornee = obj
                    h.Parent = obj
                    table.insert(espHighlights, h)
                end
            end
        end
    end
    
    -- Update loop
    task.spawn(function()
        while true do
            if espEnabled then
                updateESP()
            end
            task.wait(1)
        end
    end)
    
    -- ESP Toggle Button sa Visual Tab
    local espHolder = Instance.new("Frame")
    espHolder.Parent = visualSection
    espHolder.Size = UDim2.new(1,-30,0,40)
    espHolder.Position = UDim2.new(0,15,0,60)
    espHolder.BackgroundColor3 = Color3.fromRGB(30,30,30)
    espHolder.BorderSizePixel = 0
    
    local espHolderCorner = Instance.new("UICorner")
    espHolderCorner.CornerRadius = UDim.new(0,10)
    espHolderCorner.Parent = espHolder
    
    local espLabel = Instance.new("TextLabel")
    espLabel.Parent = espHolder
    espLabel.Size = UDim2.new(0.7,0,1,0)
    espLabel.Position = UDim2.new(0,15,0,0)
    espLabel.BackgroundTransparency = 1
    espLabel.Text = "� Scrap Pile ESP"
    espLabel.Font = Enum.Font.GothamBold
    espLabel.TextSize = 14
    espLabel.TextColor3 = Color3.new(1,1,1)
    espLabel.TextXAlignment = Enum.TextXAlignment.Left
    
    local espToggle = Instance.new("TextButton")
    espToggle.Parent = espHolder
    espToggle.Size = UDim2.new(0,55,0,28)
    espToggle.Position = UDim2.new(1,-75,0.5,-14)
    espToggle.Text = ""
    espToggle.BackgroundColor3 = Color3.fromRGB(50,50,50)
    
    local espToggleCorner = Instance.new("UICorner")
    espToggleCorner.CornerRadius = UDim.new(1,0)
    espToggleCorner.Parent = espToggle
    
    local espCircle = Instance.new("Frame")
    espCircle.Parent = espToggle
    espCircle.Size = UDim2.new(0,22,0,22)
    espCircle.Position = UDim2.new(0,3,0.5,-11)
    espCircle.BackgroundColor3 = Color3.new(1,1,1)
    
    local espCircleCorner = Instance.new("UICorner")
    espCircleCorner.CornerRadius = UDim.new(1,0)
    espCircleCorner.Parent = espCircle
    
    espToggle.MouseButton1Click:Connect(function()
        espEnabled = not espEnabled
        if espEnabled then
            TweenService:Create(espCircle, TweenInfo.new(0.2), {Position = UDim2.new(1,-25,0.5,-11)}):Play()
            espToggle.BackgroundColor3 = Color3.fromRGB(0,170,90)
            espLabel.TextColor3 = Color3.fromRGB(255,100,100)
            updateESP()
        else
            TweenService:Create(espCircle, TweenInfo.new(0.2), {Position = UDim2.new(0,3,0.5,-11)}):Play()
            espToggle.BackgroundColor3 = Color3.fromRGB(50,50,50)
            espLabel.TextColor3 = Color3.new(1,1,1)
            updateESP()
        end
    end)
    
    -- Keybind M
    UIS.InputBegan:Connect(function(input, gp)
        if gp then return end
        if input.KeyCode == Enum.KeyCode.M then
            espEnabled = not espEnabled
            if espEnabled then
                TweenService:Create(espCircle, TweenInfo.new(0.2), {Position = UDim2.new(1,-25,0.5,-11)}):Play()
                espToggle.BackgroundColor3 = Color3.fromRGB(0,170,90)
                espLabel.TextColor3 = Color3.fromRGB(255,100,100)
                updateESP()
            else
                TweenService:Create(espCircle, TweenInfo.new(0.2), {Position = UDim2.new(0,3,0.5,-11)}):Play()
                espToggle.BackgroundColor3 = Color3.fromRGB(50,50,50)
                espLabel.TextColor3 = Color3.new(1,1,1)
                updateESP()
            end
        end
    end)
    
    print("✅ Scrap Pile ESP added to Visual Tab! Press M to toggle")
end

-- Run the function to add ESP
task.spawn(function()
    addESPToVisualTab()
end)

----------------------------------------------------
-- COMPLETE ESP (Zombies + Guns + Ammos + Loot)
----------------------------------------------------

local function addCompleteESPToVisualTab()
    task.wait(0.5)
    
    if not visualSection then
        print("Visual section not found for Complete ESP, retrying...")
        task.wait(1)
        if not visualSection then return end
    end
    
    -- ================= ZOMBIE NAMES =================
    local zombieNames = {
        ["Zombie"] = true, ["Crawler"] = true, ["Green Zombie"] = true,
        ["Riot Zombie"] = true, ["Bloater"] = true, ["Muscle Zombie"] = true,
        ["Spitter"] = true, ["Screamer"] = true, ["Phaser"] = true,
        ["Hazmat Zombie"] = true, ["Military Zombie"] = true, ["Purple Elite"] = true,
        ["Radioactive Zombie"] = true, ["Night Hunter"] = true, ["Elemental Zombie"] = true,
        ["Infected"] = true, ["Walker"] = true, ["Runner"] = true, ["Standard Infected"] = true,
    }
    
    -- ================= LOOT & GUNS NAMES =================
    local lootNames = {
        -- Heals
        ["Bandage"] = true, ["Medkit"] = true,
        ["Compound I"] = true, ["Compound R"] = true,
        ["Compound S"] = true, ["Compound H"] = true,
        
        -- Ammos
        ["Pistol Ammo"] = true, ["Medium Ammo"] = true, ["Long Ammo"] = true,
        ["Shells"] = true, ["Ammo Box"] = true,
        
        -- Throwables
        ["Flashbang"] = true, ["Grenade"] = true, ["Tear Gas"] = true, ["Molotov"] = true,
        
        -- Guns
        ["AA-12"] = true, ["AK-47"] = true, ["Assault Rifle"] = true,
        ["Combat SMG"] = true, ["LMG"] = true, ["Minigun"] = true,
        ["Shotgun"] = true, ["Sniper"] = true, ["Heavy Sniper"] = true,
        ["Rifle"] = true, ["SVD"] = true, ["Ray Gun"] = true,
        ["Flamethrower"] = true, ["Grenade Launcher"] = true,
        ["Pistol"] = true, ["Desert Eagle"] = true, ["Revolver"] = true,
        ["Uzi"] = true, ["Double Barrel"] = true,
    }
    
    -- ================= COLOR CONFIGURATION =================
    local HIGHLIGHT_COLORS = {
        ZOMBIE = Color3.fromRGB(255, 50, 50),     -- Red
        GUN = Color3.fromRGB(150, 100, 255),      -- Purple
        AMMO = Color3.fromRGB(255, 200, 50),      -- Yellow
        MED = Color3.fromRGB(50, 200, 200),       -- Cyan
        THROWABLE = Color3.fromRGB(255, 100, 50), -- Orange
    }
    
    local completeESPEnabled = false
    local completeHighlights = {}
    
    local function getItemColor(itemName)
        if itemName:find("Ammo") or itemName:find("Shells") then
            return HIGHLIGHT_COLORS.AMMO
        elseif itemName:find("Compound") or itemName == "Bandage" or itemName == "Medkit" then
            return HIGHLIGHT_COLORS.MED
        elseif itemName:find("Grenade") or itemName:find("Gas") or itemName:find("Molotov") or itemName:find("Flashbang") then
            return HIGHLIGHT_COLORS.THROWABLE
        else
            return HIGHLIGHT_COLORS.GUN
        end
    end
    
    local function getItemIcon(itemName)
        if itemName:find("Ammo") or itemName:find("Shells") then
            return "� "
        elseif itemName:find("Compound") or itemName == "Bandage" or itemName == "Medkit" then
            return "� "
        elseif itemName:find("Grenade") or itemName:find("Gas") or itemName:find("Molotov") or itemName:find("Flashbang") then
            return "� "
        else
            return "� "
        end
    end
    
    local function applyHighlight(model, color, name, textColor)
        for _, h in pairs(completeHighlights) do
            if h.Adornee == model then return end
        end
        
        local highlight = Instance.new("Highlight")
        highlight.FillColor = color
        highlight.OutlineColor = color
        highlight.FillTransparency = 0.3
        highlight.OutlineTransparency = 0
        highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
        highlight.Adornee = model
        highlight.Parent = model
        table.insert(completeHighlights, highlight)
        
        local rootPart = model:FindFirstChild("HumanoidRootPart") or model:FindFirstChild("Head") or model:FindFirstChildWhichIsA("BasePart")
        if rootPart then
            local oldBill = model:FindFirstChild("ESPBillboard")
            if oldBill then oldBill:Destroy() end
            
            local billboard = Instance.new("BillboardGui")
            billboard.Name = "ESPBillboard"
            billboard.Size = UDim2.new(0, 120, 0, 30)
            billboard.Adornee = rootPart
            billboard.AlwaysOnTop = true
            billboard.Parent = model
            
            local textLabel = Instance.new("TextLabel")
            textLabel.Size = UDim2.new(1, 0, 1, 0)
            textLabel.BackgroundTransparency = 1
            textLabel.Text = name
            textLabel.TextColor3 = textColor or color
            textLabel.TextStrokeTransparency = 0
            textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
            textLabel.Font = Enum.Font.GothamBold
            textLabel.TextSize = 14
            textLabel.Parent = billboard
        end
    end
    
    local function updateCompleteESP()
        -- Remove all existing highlights
        for _, h in pairs(completeHighlights) do
            pcall(function() h:Destroy() end)
        end
        completeHighlights = {}
        
        -- Also cleanup any leftover billboards
        for _, obj in ipairs(workspace:GetDescendants()) do
            if obj:IsA("Model") then
                local bill = obj:FindFirstChild("ESPBillboard")
                if bill then pcall(function() bill:Destroy() end) end
            end
        end
        
        if not completeESPEnabled then return end
        
        for _, obj in ipairs(workspace:GetDescendants()) do
            if obj:IsA("Model") then
                if zombieNames[obj.Name] then
                    local hum = obj:FindFirstChildOfClass("Humanoid")
                    if hum and hum.Health > 0 then
                        applyHighlight(obj, HIGHLIGHT_COLORS.ZOMBIE, "� " .. obj.Name, HIGHLIGHT_COLORS.ZOMBIE)
                    end
                elseif lootNames[obj.Name] then
                    local color = getItemColor(obj.Name)
                    local icon = getItemIcon(obj.Name)
                    applyHighlight(obj, color, icon .. obj.Name, color)
                end
            end
        end
    end
    
    -- Update loop
    task.spawn(function()
        while true do
            if completeESPEnabled then
                updateCompleteESP()
            end
            task.wait(1)
        end
    end)
    
    -- ESP Toggle Button sa Visual Tab
    local espHolder2 = Instance.new("Frame")
    espHolder2.Parent = visualSection
    espHolder2.Size = UDim2.new(1,-30,0,40)
    espHolder2.Position = UDim2.new(0,15,0,115)
    espHolder2.BackgroundColor3 = Color3.fromRGB(30,30,30)
    espHolder2.BorderSizePixel = 0
    
    local espHolder2Corner = Instance.new("UICorner")
    espHolder2Corner.CornerRadius = UDim.new(0,10)
    espHolder2Corner.Parent = espHolder2
    
    local espLabel2 = Instance.new("TextLabel")
    espLabel2.Parent = espHolder2
    espLabel2.Size = UDim2.new(0.7,0,1,0)
    espLabel2.Position = UDim2.new(0,15,0,0)
    espLabel2.BackgroundTransparency = 1
    espLabel2.Text = "� COMPLETE ESP (Zombies + Loot)"
    espLabel2.Font = Enum.Font.GothamBold
    espLabel2.TextSize = 13
    espLabel2.TextColor3 = Color3.new(1,1,1)
    espLabel2.TextXAlignment = Enum.TextXAlignment.Left
    
    local espToggle2 = Instance.new("TextButton")
    espToggle2.Parent = espHolder2
    espToggle2.Size = UDim2.new(0,55,0,28)
    espToggle2.Position = UDim2.new(1,-75,0.5,-14)
    espToggle2.Text = ""
    espToggle2.BackgroundColor3 = Color3.fromRGB(50,50,50)
    
    local espToggle2Corner = Instance.new("UICorner")
    espToggle2Corner.CornerRadius = UDim.new(1,0)
    espToggle2Corner.Parent = espToggle2
    
    local espCircle2 = Instance.new("Frame")
    espCircle2.Parent = espToggle2
    espCircle2.Size = UDim2.new(0,22,0,22)
    espCircle2.Position = UDim2.new(0,3,0.5,-11)
    espCircle2.BackgroundColor3 = Color3.new(1,1,1)
    
    local espCircle2Corner = Instance.new("UICorner")
    espCircle2Corner.CornerRadius = UDim.new(1,0)
    espCircle2Corner.Parent = espCircle2
    
    espToggle2.MouseButton1Click:Connect(function()
        completeESPEnabled = not completeESPEnabled
        if completeESPEnabled then
            TweenService:Create(espCircle2, TweenInfo.new(0.2), {Position = UDim2.new(1,-25,0.5,-11)}):Play()
            espToggle2.BackgroundColor3 = Color3.fromRGB(0,170,90)
            espLabel2.TextColor3 = Color3.fromRGB(100,255,100)
            updateCompleteESP()
        else
            TweenService:Create(espCircle2, TweenInfo.new(0.2), {Position = UDim2.new(0,3,0.5,-11)}):Play()
            espToggle2.BackgroundColor3 = Color3.fromRGB(50,50,50)
            espLabel2.TextColor3 = Color3.new(1,1,1)
            updateCompleteESP()  -- This will clear all when OFF
        end
    end)
    
    -- Keybind N
    UIS.InputBegan:Connect(function(input, gp)
        if gp then return end
        if input.KeyCode == Enum.KeyCode.N then
            completeESPEnabled = not completeESPEnabled
            if completeESPEnabled then
                TweenService:Create(espCircle2, TweenInfo.new(0.2), {Position = UDim2.new(1,-25,0.5,-11)}):Play()
                espToggle2.BackgroundColor3 = Color3.fromRGB(0,170,90)
                espLabel2.TextColor3 = Color3.fromRGB(100,255,100)
            else
                TweenService:Create(espCircle2, TweenInfo.new(0.2), {Position = UDim2.new(0,3,0.5,-11)}):Play()
                espToggle2.BackgroundColor3 = Color3.fromRGB(50,50,50)
                espLabel2.TextColor3 = Color3.new(1,1,1)
            end
            updateCompleteESP()
        end
    end)
    
    print("✅ Complete ESP added to Visual Tab! Press N to toggle")
end

-- Run the function to add Complete ESP
task.spawn(function()
    addCompleteESPToVisualTab()
end)

----------------------------------------------------
-- RAINBOW BOTTOM GLOW
----------------------------------------------------
task.spawn(function()
	local glowFrame = Instance.new("Frame")
	glowFrame.Size = UDim2.new(1,0,0.35,0)
	glowFrame.Position = UDim2.new(0,0,0.65,0)
	glowFrame.BackgroundColor3 = Color3.fromRGB(255,255,255)
	glowFrame.BorderSizePixel = 0
	glowFrame.ZIndex = 1
	glowFrame.Parent = frame
	local gc = Instance.new("UICorner")
	gc.CornerRadius = UDim.new(0,16)
	gc.Parent = glowFrame
	local gg = Instance.new("UIGradient")
	gg.Rotation = 90
	gg.Transparency = NumberSequence.new({
		NumberSequenceKeypoint.new(0,1),
		NumberSequenceKeypoint.new(0.5,0.88),
		NumberSequenceKeypoint.new(1,0.55)
	})
	gg.Parent = glowFrame
	local h = 0
	while glowFrame.Parent do
		h = (h+0.004)%1
		gg.Color = ColorSequence.new({
			ColorSequenceKeypoint.new(0,Color3.fromRGB(0,0,0)),
			ColorSequenceKeypoint.new(0.4,Color3.fromHSV(h,1,0.85)),
			ColorSequenceKeypoint.new(1,Color3.fromHSV((h+0.15)%1,1,1))
		})
		task.wait(0.03)
	end
end)

print("✅ CocoHub Fully Loaded!")
print("� Zombie Kill Aura - Press K to toggle, +/- to adjust speed")
print("� Walk Speed & Fly Speed controls in Movement tab")
