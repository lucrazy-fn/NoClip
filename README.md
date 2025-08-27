local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local player = Players.LocalPlayer
local gui = script.Parent
local toggleBtn = gui:WaitForChild("ToggleBtn")
local logFrame = gui:WaitForChild("LogFrame")

local SetEnabledEvent = ReplicatedStorage:WaitForChild("AntiNoclip:SetEnabled")
local ReportEvent = ReplicatedStorage:WaitForChild("AntiNoclip:Report")

local enabled = false

toggleBtn.TextScaled = true
toggleBtn.AutoButtonColor = true

local function addLog(msg)
	local line = Instance.new("TextLabel")
	line.Size = UDim2.new(1, -10, 0, 20)
	line.Position = UDim2.new(0, 5, 0, #logFrame:GetChildren() * 22)
	line.BackgroundTransparency = 1
	line.TextXAlignment = Enum.TextXAlignment.Left
	line.TextYAlignment = Enum.TextYAlignment.Center
	line.Font = Enum.Font.Gotham
	line.TextSize = 14
	line.Text = os.date("[%H:%M:%S] ") .. msg
	line.TextColor3 = Color3.new(1,1,1)
	line.Parent = logFrame
	logFrame.CanvasSize = UDim2.new(0,0,0,line.Position.Y.Offset + 24)
end

toggleBtn.MouseButton1Click:Connect(function()
	enabled = not enabled
	toggleBtn.Text = enabled and "Detector: ON" or "Detector: OFF"
	SetEnabledEvent:FireServer(enabled)
	addLog("Detector " .. (enabled and "ativado" or "desativado"))
end)

ReportEvent.OnClientEvent:Connect(function(payload)
	local msg = string.format("Suspeita: %s (score=%d)", payload.reason or "Motivo desconhecido", payload.score or 0)
	if payload.partName then
		msg ..= " | parte: " .. payload.partName
	end
	addLog(msg)
end)

toggleBtn.Text = "Detector: OFF"

-- NoClip by LucrazyFN
