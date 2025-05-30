loadstring([[
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Protege múltiplas execuções
if playerGui:FindFirstChild("SeedSpawnerGUI") then
	playerGui:FindFirstChild("SeedSpawnerGUI"):Destroy()
end

-- GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SeedSpawnerGUI"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = playerGui

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 300, 0, 150)
Frame.Position = UDim2.new(0.35, 0, 0.4, 0)
Frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
Frame.Active = true
Frame.Draggable = true
Frame.BorderSizePixel = 0
Frame.Parent = ScreenGui

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 16)

local TitleBox = Instance.new("TextLabel", Frame)
TitleBox.Size = UDim2.new(0.9, 0, 0.2, 0)
TitleBox.Position = UDim2.new(0.05, 0, 0.05, 0)
TitleBox.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
TitleBox.BackgroundTransparency = 0.4
TitleBox.Font = Enum.Font.Gotham
TitleBox.Text = "Seed Spawner"
TitleBox.TextColor3 = Color3.fromRGB(255, 215, 0)
TitleBox.TextScaled = true
TitleBox.TextStrokeTransparency = 0.7
TitleBox.TextTransparency = 0.05
TitleBox.BorderSizePixel = 0
TitleBox.Parent = Frame

local titleCorner = Instance.new("UICorner", TitleBox)
titleCorner.CornerRadius = UDim.new(0, 12)

local TextBox = Instance.new("TextBox", Frame)
TextBox.Position = UDim2.new(0.05, 0, 0.35, 0)
TextBox.Size = UDim2.new(0.9, 0, 0.2, 0)
TextBox.PlaceholderText = "Digite o nome da semente"
TextBox.Font = Enum.Font.Gotham
TextBox.TextSize = 18
TextBox.TextColor3 = Color3.new(1, 1, 1)
TextBox.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
TextBox.BorderSizePixel = 0

local textBoxCorner = Instance.new("UICorner", TextBox)
textBoxCorner.CornerRadius = UDim.new(0, 12)

local Button = Instance.new("TextButton", Frame)
Button.Position = UDim2.new(0.05, 0, 0.65, 0)
Button.Size = UDim2.new(0.9, 0, 0.25, 0)
Button.Text = "Spawn"
Button.Font = Enum.Font.Gotham
Button.TextSize = 24
Button.TextColor3 = Color3.new(1, 1, 1)
Button.BackgroundColor3 = Color3.fromRGB(100, 150, 255)
Button.BorderSizePixel = 0

local btnCorner = Instance.new("UICorner", Button)
btnCorner.CornerRadius = UDim.new(0, 12)

-- Função para clonar e adicionar a semente ao inventário
local function spawnSeed(seedName)
	local seedFolder = ReplicatedStorage:FindFirstChild("Seeds")
	if not seedFolder then
		warn("Pasta 'Seeds' não encontrada em ReplicatedStorage.")
		return
	end

	local seed = seedFolder:FindFirstChild(seedName)
	if not seed then
		warn("Semente '" .. seedName .. "' não encontrada.")
		return
	end

	local clone = seed:Clone()
	clone.Parent = player.Backpack
end

Button.MouseButton1Click:Connect(function()
	local inputText = TextBox.Text
	if inputText == "" then
		return
	end

	-- Normaliza o nome da semente
	local seedName = inputText:gsub(" seed", ""):gsub("Seed", ""):gsub("^%l", string.upper)

	spawnSeed(seedName)
end)
]])()
