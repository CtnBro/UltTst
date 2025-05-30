loadstring([[
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Remove GUI se já existir
if playerGui:FindFirstChild("IngeniousScriptDupeGUI") then
	playerGui.IngeniousScriptDupeGUI:Destroy()
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

local function setDupeButtonState(text, color)
	ButtonDupe.Text = text
	ButtonDupe.BackgroundColor3 = color
	task.delay(1.3, function()
		ButtonDupe.Text = "💀 DUPLICAR ITEM NA MÃO 💀"
		ButtonDupe.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
	end)
end

local function getAnimController(tool)
	return tool:FindFirstChildOfClass("AnimationController") or tool:FindFirstChildOfClass("Humanoid")
end

ButtonDupe.MouseButton1Click:Connect(function()
	local character = player.Character or player.CharacterAdded:Wait()
	local humanoid = character:FindFirstChildOfClass("Humanoid")
	local heldTool = character:FindFirstChildOfClass("Tool")

	if heldTool and humanoid then
		local clone = heldTool:Clone()
		clone.Parent = character
		humanoid:EquipTool(clone)

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

-- === MENU ESP E VOAR ===
local ESPEnabled = false
local FlyEnabled = false

local ESPButton = Instance.new("TextButton", Frame)
ESPButton.Position = UDim2.new(0.05, 0, 0.35, 0)
ESPButton.Size = UDim2.new(0.9, 0, 0.13, 0)
ESPButton.Text = "🧟‍♂️ ESP: OFF"
ESPButton.Font = Enum.Font.GothamBold
ESPButton.TextSize = 24
ESPButton.TextColor3 = Color3.fromRGB(255, 0, 0)
ESPButton.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
ESPButton.BorderSizePixel = 0
addUICorner(ESPButton, 14)

local FlyButton = Instance.new("TextButton", Frame)
FlyButton.Position = UDim2.new(0.05, 0, 0.5, 0)
FlyButton.Size = UDim2.new(0.9, 0, 0.13, 0)
FlyButton.Text = "🦇 VOAR: OFF"
FlyButton.Font = Enum.Font.GothamBold
FlyButton.TextSize = 24
FlyButton.TextColor3 = Color3.fromRGB(255, 0, 0)
FlyButton.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
FlyButton.BorderSizePixel = 0
addUICorner(FlyButton, 14)

local FooterStatus = Instance.new("TextLabel", Frame)
FooterStatus.Position = UDim2.new(0.05, 0, 0.75, 0)
FooterStatus.Size = UDim2.new(0.9, 0, 0.1, 0)
FooterStatus.BackgroundTransparency = 1
FooterStatus.Font = Enum.Font.GothamBold
FooterStatus.TextSize = 20
FooterStatus.TextColor3 = Color3.fromRGB(255, 100, 100)
FooterStatus.Text = "Modo ESP e Voo desativados"

-- ESP Função
local espHighlights = {}

local function toggleESP()
	ESPEnabled = not ESPEnabled
	if ESPEnabled then
		ESPButton.Text = "🧟‍♂️ ESP: ON"
		ESPButton.TextColor3 = Color3.fromRGB(0, 255, 0)
		FooterStatus.Text = "ESP ATIVADO"
		-- Criar Highlights nos jogadores
		for _, plr in pairs(Players:GetPlayers()) do
			if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
				local highlight = Instance.new("Highlight")
				highlight.Adornee = plr.Character
				highlight.FillColor = Color3.new(1, 0, 0)
				highlight.OutlineColor = Color3.new(1, 0, 0)
				highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
				highlight.Parent = player.PlayerGui
				espHighlights[plr] = highlight
			end
		end
		-- Atualizar highlights para novos jogadores que entrarem
		Players.PlayerAdded:Connect(function(plr)
			if ESPEnabled and plr ~= player then
				plr.CharacterAdded:Connect(function(char)
					task.wait(1)
					if ESPEnabled then
						local highlight = Instance.new("Highlight")
						highlight.Adornee = char
						highlight.FillColor = Color3.new(1, 0, 0)
						highlight.OutlineColor = Color3.new(1, 0, 0)
						highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
						highlight.Parent = player.PlayerGui
						espHighlights[plr] = highlight
					end
				end)
			end
		end)
	else
		ESPButton.Text = "🧟‍♂️ ESP: OFF"
		ESPButton.TextColor3 = Color3.fromRGB(255, 0, 0)
		FooterStatus.Text = "ESP DESATIVADO"
		for plr, hl in pairs(espHighlights) do
			if hl then hl:Destroy() end
		end
		espHighlights = {}
	end
end

ESPButton.MouseButton1Click:Connect(toggleESP)

-- Voo simples
local flying = false
local flySpeed = 100
local bodyVelocity, bodyGyro

local function startFly()
	if flying then return end
	flying = true
	local character = player.Character
	if not character then return end
	local hrp = character:FindFirstChild("HumanoidRootPart")
	if not hrp then return end

	bodyGyro = Instance.new("BodyGyro")
	bodyGyro.P = 9e4
	bodyGyro.MaxTorque = Vector3.new(9e4, 9e4, 9e4)
	bodyGyro.CFrame = hrp.CFrame
	bodyGyro.Parent = hrp

	bodyVelocity = Instance.new("BodyVelocity")
	bodyVelocity.Velocity = Vector3.new(0,0,0)
	bodyVelocity.MaxForce = Vector3.new(9e4, 9e4, 9e4)
	bodyVelocity.Parent = hrp

	RunService:BindToRenderStep("Fly", 301, function()
		if not flying then return end
		local moveVector = Vector3.new(0,0,0)
		if UserInputService:IsKeyDown(Enum.KeyCode.W) then
			moveVector = moveVector + workspace.CurrentCamera.CFrame.LookVector
		end
		if UserInputService:IsKeyDown(Enum.KeyCode.S) then
			moveVector = moveVector - workspace.CurrentCamera.CFrame.LookVector
		end
		if UserInputService:IsKeyDown(Enum.KeyCode.A) then
			moveVector = moveVector - workspace.CurrentCamera.CFrame.RightVector
		end
		if UserInputService:IsKeyDown(Enum.KeyCode.D) then
			moveVector = moveVector + workspace.CurrentCamera.CFrame.RightVector
		end
		if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
			moveVector = moveVector + Vector3.new(0,1,0)
		end
		if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then
			moveVector = moveVector - Vector3.new(0,1,0)
		end
		bodyVelocity.Velocity = moveVector.Unit * flySpeed
		bodyGyro.CFrame = workspace.CurrentCamera.CFrame
	end)
end

local function stopFly()
	if not flying then return end
	flying = false
	RunService:UnbindFromRenderStep("Fly")
	if bodyVelocity then
		bodyVelocity:Destroy()
		bodyVelocity = nil
	end
	if bodyGyro then
		bodyGyro:Destroy()
		bodyGyro = nil
	end
end

FlyButton.MouseButton1Click:Connect(function()
	if FlyEnabled then
		FlyEnabled = false
		FlyButton.Text = "🦇 VOAR: OFF"
		FlyButton.TextColor3 = Color3.fromRGB(255, 0, 0)
		FooterStatus.Text = "Voo desativado"
		stopFly()
	else
		FlyEnabled = true
		FlyButton.Text = "🦇 VOAR: ON"
		FlyButton.TextColor3 = Color3.fromRGB(0, 255, 0)
		FooterStatus.Text = "Voo ativado - Use WASD, Espaço e Ctrl"
		startFly()
	end
end)
]])()
