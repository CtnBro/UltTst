loadstring([[
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

if playerGui:FindFirstChild("HellSeedGUI") then
	playerGui.HellSeedGUI:Destroy()
end

local gui = Instance.new("ScreenGui", playerGui)
gui.Name = "HellSeedGUI"
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.ResetOnSpawn = false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 420, 0, 240)
frame.Position = UDim2.new(0.3, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 14)

local glow = Instance.new("UIGradient", frame)
glow.Color = ColorSequence.new{
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 0)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 0, 0))
}
glow.Rotation = 90

-- Título
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0.2, 0)
title.Position = UDim2.new(0, 0, 0, 0)
title.Text = "🌑 Seed Spawner 🌑"
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.BackgroundTransparency = 1
title.TextScaled = true
title.Font = Enum.Font.GothamBold

-- Barreira Infernal
local bottom = Instance.new("TextLabel", frame)
bottom.Size = UDim2.new(1, 0, 0.1, 0)
bottom.Position = UDim2.new(0, 0, 0.9, 0)
bottom.BackgroundTransparency = 1
bottom.TextColor3 = Color3.fromRGB(200, 0, 0)
bottom.Text = "DEV BY Tr1nX_777"
bottom.Font = Enum.Font.Arcade
bottom.TextScaled = true

local textbox = Instance.new("TextBox", frame)
textbox.Size = UDim2.new(0.9, 0, 0.2, 0)
textbox.Position = UDim2.new(0.05, 0, 0.3, 0)
textbox.PlaceholderText = "Digite o nome da fruta sombria..."
textbox.Font = Enum.Font.FredokaOne
textbox.TextScaled = true
textbox.TextColor3 = Color3.fromRGB(255, 0, 0)
textbox.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
textbox.BorderSizePixel = 0

Instance.new("UICorner", textbox).CornerRadius = UDim.new(0, 10)

local button = Instance.new("TextButton", frame)
button.Size = UDim2.new(0.9, 0, 0.25, 0)
button.Position = UDim2.new(0.05, 0, 0.55, 0)
button.Text = "🩸 INVOCAR 🩸"
button.Font = Enum.Font.GothamBlack
button.TextScaled = true
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.BackgroundColor3 = Color3.fromRGB(60, 0, 0)
button.BorderSizePixel = 0

Instance.new("UICorner", button).CornerRadius = UDim.new(0, 12)

-- Sombra pulsante
task.spawn(function()
	while true do
		button.BackgroundColor3 = Color3.fromRGB(80, 0, 0)
		wait(0.3)
		button.BackgroundColor3 = Color3.fromRGB(60, 0, 0)
		wait(0.3)
	end
end)

-- Som demoníaco
local function spawnHellSeed(seedName)
	local seedFolder = ReplicatedStorage:FindFirstChild("Seeds")
	if not seedFolder then
		warn("Pasta Seeds não encontrada.")
		return
	end

	local realName = seedName:gsub(" seed", ""):gsub("Seed", ""):gsub("^%l", string.upper)
	local seed = seedFolder:FindFirstChild(realName)
	if seed then
		local clone = seed:Clone()
		clone.Parent = player.Backpack

		button.Text = "✔️ INVOCADA"
		button.BackgroundColor3 = Color3.fromRGB(0, 120, 0)
		wait(1)
		button.Text = "🩸 INVOCAR 🩸"
	else
		button.Text = "❌ DESCONHECIDA"
		button.BackgroundColor3 = Color3.fromRGB(120, 0, 0)
		wait(1)
		button.Text = "🩸 INVOCAR 🩸"
	end
end

button.MouseButton1Click:Connect(function()
	local text = textbox.Text
	if text and #text > 0 then
		spawnHellSeed(text)
	end
end)
]])()
