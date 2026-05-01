loadstring([[
local player = game.Players.LocalPlayer
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local StarterGui = game:GetService("StarterGui")

local FARM = false

-- 🔔 NOTIFICAÇÃO
local function notify(titulo, texto)
	pcall(function()
		StarterGui:SetCore("SendNotification", {
			Title = titulo,
			Text = texto,
			Duration = 5
		})
	end)
end

notify("Interlagos Hub", "Pegue o emprego antes de farmar!")

-- 👀 ALERTA PLAYER
Players.PlayerAdded:Connect(function(plr)
	if plr ~= player then
		task.wait(1)
		notify("👀 Jogador entrou", plr.Name.." entrou no servidor!")
	end
end)

-- 📍 LIXOS
local LIXOS = {
	Vector3.new(273.66,18.84,-129.54),
	Vector3.new(1173.23,19.01,-762.65),
	Vector3.new(1106.38,19.01,-88.06),
	Vector3.new(1109.92,19.01,271.93),
	Vector3.new(115.37,18.84,-303.32),
	Vector3.new(115.37,18.84,-225.24),
	Vector3.new(324.96,18.84,-793.60),
	Vector3.new(115.37,18.84,-126.69),
	Vector3.new(367.53,18.84,-126.69),
	Vector3.new(320.89,18.84,-188.15),
	Vector3.new(320.64,18.84,-245.53),
	Vector3.new(319.38,18.84,-408.38),
	Vector3.new(321.38,18.84,-322.39),
	Vector3.new(319.38,18.84,-519.02),
	Vector3.new(319.38,18.84,-598.97),
	Vector3.new(157.15,18.84,-80.85),
	Vector3.new(319.38,18.84,-645.57),
	Vector3.new(1106.19,19.01,-758.92),
	Vector3.new(331.69,18.84,-751.39),
	Vector3.new(1108.72,19.01,-600.10),
	Vector3.new(1173.23,19.01,-531.13),
	Vector3.new(599.96,18.84,-40.24),
	Vector3.new(599.96,18.84,-126.69),
	Vector3.new(649.44,19.01,-53.18),
	Vector3.new(1111.77,19.01,-423.47),
	Vector3.new(723.01,19.01,-90.56),
	Vector3.new(-69.86,18.83,-793.15),
	Vector3.new(687.06,19.06,-793.18),
	Vector3.new(263.43,19.06,-836.45),

	-- NOVOS
	Vector3.new(90.84,18.83,-495.78),
	Vector3.new(-95.99,18.83,-77.71),
	Vector3.new(123.68,18.83,-793.15),
	Vector3.new(951.07,19.06,-792.32),
	Vector3.new(88.04,18.83,-379.74),
	Vector3.new(231.49,18.83,-517.08),
	Vector3.new(1119.38,18.77,-850.85),
	Vector3.new(1065.77,19.06,-834.89),
	Vector3.new(380.67,19.06,-836.45)
}

-- 🗑️ LIXEIRA
local LIXEIRA = Vector3.new(1093.37,23.35,-441.97)

-- 🏢 PREFEITURA NOVA
local PREFEITURA = Vector3.new(365.0993,20.4230,-292.7679)

-- FUNÇÕES
local function getHRP()
	return (player.Character or player.CharacterAdded:Wait()):WaitForChild("HumanoidRootPart")
end

local function tp(pos)
	getHRP().CFrame = CFrame.new(pos + Vector3.new(0,5,0))
end

local function pegarPrompt()
	for _,v in pairs(workspace:GetDescendants()) do
		if v:IsA("ProximityPrompt") then
			fireproximityprompt(v)
		end
	end
end

-- REMOVE UI ANTIGA
pcall(function()
	if game.CoreGui:FindFirstChild("InterlagosHub") then
		game.CoreGui.InterlagosHub:Destroy()
	end
end)

-- GUI
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "InterlagosHub"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0,220,0,150)
frame.Position = UDim2.new(0,50,0,200)
frame.BackgroundColor3 = Color3.fromRGB(15,15,15)
frame.Active = true

-- TÍTULO
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1,0,0,25)
title.Text = "Interlagos Lixo Hub"
title.BackgroundColor3 = Color3.fromRGB(20,20,20)
title.TextColor3 = Color3.new(1,1,1)

-- BOTÃO FARM
local btnFarm = Instance.new("TextButton", frame)
btnFarm.Size = UDim2.new(1,-10,0,30)
btnFarm.Position = UDim2.new(0,5,0,35)
btnFarm.Text = "Farmar Lixo: OFF"
btnFarm.BackgroundColor3 = Color3.fromRGB(20,20,20)
btnFarm.TextColor3 = Color3.new(1,1,1)

btnFarm.MouseButton1Click:Connect(function()
	FARM = not FARM
	btnFarm.Text = FARM and "Farmar Lixo: ON" or "Farmar Lixo: OFF"
end)

-- BOTÃO LIXEIRA
local btnLixo = Instance.new("TextButton", frame)
btnLixo.Size = UDim2.new(1,-10,0,30)
btnLixo.Position = UDim2.new(0,5,0,70)
btnLixo.Text = "Ir para Lixeira"
btnLixo.BackgroundColor3 = Color3.fromRGB(20,20,20)
btnLixo.TextColor3 = Color3.new(1,1,1)

btnLixo.MouseButton1Click:Connect(function()
	tp(LIXEIRA)
end)

-- BOTÃO PREFEITURA
local btnJob = Instance.new("TextButton", frame)
btnJob.Size = UDim2.new(1,-10,0,30)
btnJob.Position = UDim2.new(0,5,0,105)
btnJob.Text = "Pegar Emprego"
btnJob.BackgroundColor3 = Color3.fromRGB(20,20,20)
btnJob.TextColor3 = Color3.new(1,1,1)

btnJob.MouseButton1Click:Connect(function()
	tp(PREFEITURA)
end)

-- DRAG
local dragging, dragStart, startPos

title.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 
	or input.UserInputType == Enum.UserInputType.Touch then
		
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

UIS.InputChanged:Connect(function(input)
	if dragging then
		local delta = input.Position - dragStart
		frame.Position = UDim2.new(0,startPos.X.Offset + delta.X,0,startPos.Y.Offset + delta.Y)
	end
end)

-- LOOP FARM
task.spawn(function()
	while true do
		task.wait(0.1)

		if FARM then
			for _,pos in ipairs(LIXOS) do
				if not FARM then break end
				
				tp(pos)
				task.wait(0.1)
				pegarPrompt()
			end
		end
	end
end)

]])()
