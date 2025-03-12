local TweenService = game:GetService("TweenService")

local screenGui = Instance.new("ScreenGui")
local button = Instance.new("TextButton")
local textLabel = Instance.new("TextButton")
local xButton = Instance.new("TextButton")
local clickCount = 0

screenGui.Name = "SubirGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

button.Name = "SubirButton"
button.Size = UDim2.new(0, 100, 0, 50)
button.Position = UDim2.new(0.5, -50, 0.5, -25)
button.BackgroundColor3 = Color3.new(0, 0, 0)
button.TextColor3 = Color3.new(1, 1, 1)
button.Text = "Subir"
button.Font = Enum.Font.SourceSans
button.TextSize = 24
button.Active = true
button.Draggable = true
button.Parent = screenGui

textLabel.Name = "By ninja"
textLabel.Text = "By ninja"
textLabel.TextColor3 = Color3.new(1, 0, 0)
textLabel.Size = UDim2.new(0, 150, 0, 30)
textLabel.BackgroundTransparency = 1
textLabel.Font = Enum.Font.SourceSans
textLabel.TextSize = 14
textLabel.Position = UDim2.new(button.Position.X.Scale, button.Position.X.Offset, button.Position.Y.Scale + 0.002, button.Position.Y.Offset + button.Size.Y.Offset)
textLabel.Parent = screenGui

xButton.Name = "CloseButton"
xButton.Text = "X"
xButton.Size = UDim2.new(0, 20, 0, 20)
xButton.Position = UDim2.new(1, -25, 0, 5)
xButton.BackgroundColor3 = Color3.new(1, 0, 0)
xButton.TextColor3 = Color3.new(1, 1, 1)
xButton.Parent = button

xButton.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

local function updatePosition()
    textLabel.Position = UDim2.new(button.Position.X.Scale, button.Position.X.Offset, button.Position.Y.Scale + 0.002, button.Position.Y.Offset + button.Size.Y.Offset)
    xButton.Position = UDim2.new(1, -25, 0, 5)
end

button:GetPropertyChangedSignal("Position"):Connect(updatePosition)
updatePosition()

local function changeColorRGBOnText()
    local colors = {Color3.fromRGB(255, 0, 0), Color3.fromRGB(0, 255, 0), Color3.fromRGB(0, 0, 255), Color3.fromRGB(255, 255, 0), Color3.fromRGB(255, 0, 255), Color3.fromRGB(0, 255, 255)}
    local index = 1
    local cycles = 0
    local totalCycles = 3

    while cycles < totalCycles do
        textLabel.TextColor3 = colors[index]
        index = index + 1
        if index > #colors then
            index = 1
        end
        wait(0.1)
        if index == 1 then
            cycles = cycles + 1
        end
    end

    textLabel.TextColor3 = Color3.new(1, 0, 0)
end

local function calculatePosition(direction, distance)
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local rootPart = character:WaitForChild("HumanoidRootPart")
    local rightVector = rootPart.CFrame.RightVector
    return rootPart.Position + (rightVector * direction * distance)
end

local function moveTo(position)
    local player = game.Players.LocalPlayer
    local humanoid = player.Character:WaitForChild("Humanoid")
    humanoid:MoveTo(position)
    humanoid.MoveToFinished:Wait()
end

local function jump()
    local player = game.Players.LocalPlayer
    local humanoid = player.Character:WaitForChild("Humanoid")
    humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
end

textLabel.MouseButton1Click:Connect(function()
    clickCount = clickCount + 1
    print("Clique " .. clickCount)

    if clickCount == 10 then
        spawn(changeColorRGBOnText)
        clickCount = 0
    end
end)

button.MouseButton1Click:Connect(function()
    button.Text = "Movendo..."
    local leftDistance = 2
    local rightDistance = 3

    local leftPosition = calculatePosition(-1, leftDistance)
    moveTo(leftPosition)
    jump()
    local rightPosition = calculatePosition(1, rightDistance)
    moveTo(rightPosition)
    button.Text = "Subir"
end)
