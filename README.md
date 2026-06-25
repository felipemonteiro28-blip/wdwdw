--// Auto Block UI
--// LocalScript

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local Player = Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

-- Estado inicial
_G.AutoBlockEnabled = false

--------------------------------------------------
-- ScreenGui
--------------------------------------------------

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AutoBlockUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui

--------------------------------------------------
-- Janela Principal
--------------------------------------------------

local Frame = Instance.new("Frame")
Frame.Name = "MainFrame"
Frame.Size = UDim2.new(0, 220, 0, 100)
Frame.Position = UDim2.new(0.5, -110, 0.5, -50)
Frame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Frame.BorderSizePixel = 0
Frame.Parent = ScreenGui

local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 8)
Corner.Parent = Frame

--------------------------------------------------
-- Barra Superior
--------------------------------------------------

local TitleBar = Instance.new("Frame")
TitleBar.Size = UDim2.new(1, 0, 0, 30)
TitleBar.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = Frame

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, 8)
TitleCorner.Parent = TitleBar

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 1, 0)
Title.BackgroundTransparency = 1
Title.Text = "Auto Block Tester"
Title.TextColor3 = Color3.new(1,1,1)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 14
Title.Parent = TitleBar

--------------------------------------------------
-- Botão Toggle
--------------------------------------------------

local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0.85, 0, 0, 40)
ToggleButton.Position = UDim2.new(0.075, 0, 0.5, -5)
ToggleButton.BorderSizePixel = 0
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.TextSize = 16
ToggleButton.Parent = Frame

local ButtonCorner = Instance.new("UICorner")
ButtonCorner.CornerRadius = UDim.new(0, 6)
ButtonCorner.Parent = ToggleButton

--------------------------------------------------
-- Atualização Visual
--------------------------------------------------

local function UpdateButton()
	if _G.AutoBlockEnabled then
		ToggleButton.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
		ToggleButton.Text = "Auto Block: ON"
		ToggleButton.TextColor3 = Color3.new(1,1,1)
	else
		ToggleButton.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
		ToggleButton.Text = "Auto Block: OFF"
		ToggleButton.TextColor3 = Color3.new(1,1,1)
	end
end

UpdateButton()

--------------------------------------------------
-- Alternar Estado
--------------------------------------------------

ToggleButton.MouseButton1Click:Connect(function()
	_G.AutoBlockEnabled = not _G.AutoBlockEnabled
	UpdateButton()

	print("Auto Block:", _G.AutoBlockEnabled)
end)

--------------------------------------------------
-- Sistema de Arrastar Janela
--------------------------------------------------

local Dragging = false
local DragStart
local StartPos

TitleBar.InputBegan:Connect(function(Input)
	if Input.UserInputType == Enum.UserInputType.MouseButton1 then
		Dragging = true
		DragStart = Input.Position
		StartPos = Frame.Position

		Input.Changed:Connect(function()
			if Input.UserInputState == Enum.UserInputState.End then
				Dragging = false
			end
		end)
	end
end)

TitleBar.InputChanged:Connect(function(Input)
	if Input.UserInputType == Enum.UserInputType.MouseMovement then
		DragInput = Input
	end
end)

UserInputService.InputChanged:Connect(function(Input)
	if Dragging and Input.UserInputType == Enum.UserInputType.MouseMovement then

		local Delta = Input.Position - DragStart

		Frame.Position = UDim2.new(
			StartPos.X.Scale,
			StartPos.X.Offset + Delta.X,
			StartPos.Y.Scale,
			StartPos.Y.Offset + Delta.Y
		)
	end
end)
