loadstring([[
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Remove GUI se já existir
if playerGui:FindFirstChild("IngeniousScriptDupeGUI") then
	playerGui.IngeniousScriptDupeGUI:Destroy()
end
if playerGui:FindFirstChild("WeatherControlGUI") then
	playerGui.WeatherControlGUI:Destroy()
end

-- Função para criar UICorner facilmente
local function addUICorner(parent, radius)
	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, radius)
	corner.Parent = parent
	return corner
end

-- ======= GUI PRINCIPAL =======
local ScreenGui = Instance.new("ScreenGui", playerGui)
ScreenGui.Name = "IngeniousScriptDupeGUI"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

local Frame = Instance.new("Frame", ScreenGui)
Frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Frame.Position = UDim2.new(0.35, 0, 0.3, 0)
Frame.Size = UDim2.new(0, 350, 0, 360)
Frame.Active = true
Frame.Draggable = true
Frame.BorderSizePixel = 0
addUICorner(Frame, 20)

-- Título Macabro
local TitleBox = Instance.new("TextLabel", Frame)
TitleBox.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
TitleBox.BackgroundTransparency = 0.4
TitleBox.Position = UDim2.new(0.05, 0, 0.02, 0)
TitleBox.Size = UDim2.new(0.9, 0, 0.12, 0)
TitleBox.Font = Enum.Font.GothamBlack
TitleBox.Text = "DEV BY Tr1nX_777 | Macabro"
TitleBox.TextColor3 = Color3.fromRGB(255, 0, 0)
TitleBox.TextScaled = true
TitleBox.TextStrokeTransparency = 0.5
TitleBox.BorderSizePixel = 0
addUICorner(TitleBox, 16)

-- Efeito de brilho sinistro no título
local ShineLine = Instance.new("Frame", TitleBox)
ShineLine.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
ShineLine.Size = UDim2.new(0.08, 0, 1, 0)
ShineLine.Position = UDim2.new(-0.1, 0, 0, 0)
ShineLine.BackgroundTransparency = 0.7
ShineLine.BorderSizePixel = 0

task.spawn(function()
	while true do
		ShineLine.Position = UDim2.new(-0.1, 0, 0, 0)
		ShineLine:TweenPosition(UDim2.new(1.1, 0, 0, 0), Enum.EasingDirection.Out, Enum.EasingStyle.Linear, 0.9, true)
		wait(1)
	end
end)

-- Texto de rodapé macabro
local FooterText = Instance.new("TextLabel", Frame)
FooterText.BackgroundTransparency = 1
FooterText.Position = UDim2.new(0.05, 0, 0.9, 0)
FooterText.Size = UDim2.new(0.9, 0, 0.07, 0)
FooterText.Font = Enum.Font.GothamBlack
FooterText.Text = "Keep it dark and deadly..."
FooterText.TextColor3 = Color3.fromRGB(200, 0, 0)
FooterText.TextScaled = true
FooterText.TextStrokeTransparency = 0.6
FooterText.BorderSizePixel = 0

-- === Botão Dupe ===
local ButtonDupe = Instance.new("TextButton", Frame)
ButtonDupe.Position = UDim2.new(0.05, 0, 0.15, 0)
ButtonDupe.Size = UDim2.new(0.9, 0, 0.15, 0)
ButtonDupe.Text = "💀 DUPLICAR ITEM NA MÃO 💀"
ButtonDupe.Font = Enum.Font.GothamBold
ButtonDupe.TextSize = 26
ButtonDupe.TextColor3 = Color3.fromRGB(255, 20, 20)
ButtonDupe.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
ButtonDupe.BorderSizePixel = 0
addUICorner(ButtonDupe, 16)

-- Feedback do botão dupe
local function setDupeButtonState(text, color)
	ButtonDupe.Text = text
	ButtonDupe.BackgroundColor3 = color
	task.delay(1.3, function()
		ButtonDupe.Text = "💀 DUPLICAR ITEM NA MÃO 💀"
		ButtonDupe.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
	end)
end

-- Função para obter controlador de animação (Humanoid ou AnimationController)
local function getAnimController(tool)
	return tool:FindFirstChildOfClass("AnimationController") or tool:FindFirstChildOfClass("Humanoid")
end

-- Evento clique dupe
ButtonDupe.MouseButton1Click:Connect(function()
	local character = player.Character or player.CharacterAdded:Wait()
	local humanoid = character:FindFirstChildOfClass("Humanoid")
	local heldTool = character:FindFirstChildOfClass("Tool")

	if heldTool and humanoid then
		-- Clona e equipa o clone
		local clone = heldTool:Clone()
		clone.Parent = character
		humanoid:EquipTool(clone)

		-- Replicar animações do original para o clone
		local origAnimCtrl = getAnimController(heldTool)
		local cloneAnimCtrl = getAnimController(clone)

		if origAnimCtrl and cloneAnimCtrl then
			for _, track in pairs(origAnimCtrl:GetPlayingAnimationTracks()) do
				local anim = track.Animation
				if anim then
					local newTrack = cloneAnimCtrl:LoadAnimation(anim)
					newTrack:Play()
					newTrack.TimePosition = track.TimePosition
					newTrack.Speed = track.Speed
				end
			end
		end

		setDupeButtonState("✔️ DUPLICADO!", Color3.fromRGB(0, 180, 0))
	else
		setDupeButtonState("✖️ SEM ITEM NA MÃO!", Color3.fromRGB(180, 0, 0))
	end
end)

-- === Título do menu clima ===
local WeatherTitle = Instance.new("TextLabel", Frame)
WeatherTitle.Position = UDim2.new(0.05, 0, 0.35, 0)
WeatherTitle.Size = UDim2.new(0.9, 0, 0.1, 0)
WeatherTitle.Font = Enum.Font.GothamBlack
WeatherTitle.Text = "🌩️ CONTROLE DE CLIMA / EVENTOS 🌩️"
WeatherTitle.TextColor3 = Color3.fromRGB(255, 50, 50)
WeatherTitle.TextScaled = true
WeatherTitle.TextStrokeTransparency = 0.6
WeatherTitle.BackgroundTransparency = 1

-- Função para enviar evento para servidor
local function activateWeather(weatherType)
	-- Atenção: o RemoteEvent 'WeatherEvent' deve existir no ReplicatedStorage do jogo
	local weatherEvent = game:GetService("ReplicatedStorage"):FindFirstChild("WeatherEvent")
	if weatherEvent then
		weatherEvent:FireServer(weatherType)
		-- Feedback visual de sucesso
		WeatherStatus.Text = "Evento ativado: "..weatherType
		WeatherStatus.TextColor3 = Color3.fromRGB(0, 255, 0)
	else
		WeatherStatus.Text = "Erro: WeatherEvent não encontrado"
		WeatherStatus.TextColor3 = Color3.fromRGB(255, 0, 0)
	end
end

-- Botões do menu clima
local WeatherButtons = {
	{Text = "☀️ Sunny", Color = Color3.fromRGB(50, 205, 50), Event = "Sunny"},
	{Text = "🌧️ Rain", Color = Color3.fromRGB(30, 144, 255), Event = "Rain"},
	{Text = "⚡ Thunderstorm", Color = Color3.fromRGB(255, 140, 0), Event = "Thunderstorm"},
	{Text = "❄️ Frost", Color = Color3.fromRGB(138, 43, 226), Event = "Frost"},
	{Text = "💰 Sheckle Rain", Color = Color3.fromRGB(255, 215, 0), Event = "SheckleRain"},
}

local startY = 0.46
local btnHeight = 0.09
local WeatherStatus = Instance.new("TextLabel", Frame)
WeatherStatus.Position = UDim2.new(0.05, 0, 0.85, 0)
WeatherStatus.Size = UDim2.new(0.9, 0, 0.1, 0)
WeatherStatus.BackgroundTransparency = 1
WeatherStatus.Text = ""
WeatherStatus.TextColor3 = Color3.fromRGB(255, 255, 255)
WeatherStatus.Font = Enum.Font.GothamBold
WeatherStatus.TextScaled = true

for i, info in ipairs(WeatherButtons) do
	local btn = Instance.new("TextButton", Frame)
	btn.Size = UDim2.new(0.9, 0, btnHeight, 0)
	btn.Position = UDim2.new(0.05, 0, startY + (i-1)*btnHeight, 0)
	btn.Text = info.Text
	btn.Font = Enum.Font.GothamBold
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.BackgroundColor3 = info.Color
	btn.BorderSizePixel = 0
	btn.AutoButtonColor = true
	addUICorner(btn, 14)

	btn.MouseButton1Click:Connect(function()
		activateWeather(info.Event)
	end)
end
]])()
