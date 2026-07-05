-- ROBLOX ADMIN SCRIPT
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local RootPart = Character:WaitForChild("HumanoidRootPart")
local Mouse = Player:GetMouse()


ScreenGui.Parent = Player.PlayerGui

-- Main Frame
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 250, 0, 400)
MainFrame.Position = UDim2.new(0, 100, 0, 100)
MainFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 20)
MainFrame.BackgroundTransparency = 0.1
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

-- Shadow
local Shadow = Instance.new("ImageLabel")
Shadow.Size = UDim2.new(1, 20, 1, 20)
Shadow.Position = UDim2.new(0, -10, 0, -10)
Shadow.BackgroundTransparency = 1
Shadow.Image = "rbxassetid://6014261993"
Shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
Shadow.ImageTransparency = 0.5
Shadow.Parent = MainFrame

-- Corner
local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 10)
Corner.Parent = MainFrame

-- Title Bar
local TitleBar = Instance.new("Frame")
TitleBar.Size = UDim2.new(1, 0, 0, 40)
TitleBar.BackgroundColor3 = Color3.fromRGB(20, 20, 40)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, 10)
TitleCorner.Parent = TitleBar

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 1, 0)
Title.BackgroundTransparency = 1
Title.Text = "✦ ROBLOX ADMIN ✦"
Title.TextColor3 = Color3.fromRGB(0, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 18
Title.TextStrokeTransparency = 0.5
Title.Parent = TitleBar

-- Glow Effect
local Glow = Instance.new("ImageLabel")
Glow.Size = UDim2.new(1, 0, 1, 0)
Glow.BackgroundTransparency = 1
Glow.Image = "rbxassetid://6014261993"
Glow.ImageColor3 = Color3.fromRGB(0, 255, 255)
Glow.ImageTransparency = 0.8
Glow.Parent = TitleBar

-- Scroll Frame
local ScrollFrame = Instance.new("ScrollingFrame")
ScrollFrame.Size = UDim2.new(1, -10, 1, -50)
ScrollFrame.Position = UDim2.new(0, 5, 0, 45)
ScrollFrame.BackgroundTransparency = 1
ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 500)
ScrollFrame.ScrollBarThickness = 3
ScrollFrame.ScrollBarImageColor3 = Color3.fromRGB(0, 255, 255)
ScrollFrame.Parent = MainFrame

local buttons = {}

local function createButton(text, key, toggleFunc)
    local ButtonFrame = Instance.new("Frame")
    ButtonFrame.Size = UDim2.new(1, -10, 0, 40)
    ButtonFrame.Position = UDim2.new(0, 5, 0, #ScrollFrame:GetChildren() * 45)
    ButtonFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 50)
    ButtonFrame.BackgroundTransparency = 0.3
    ButtonFrame.BorderSizePixel = 0
    ButtonFrame.Parent = ScrollFrame
    
    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.CornerRadius = UDim.new(0, 8)
    ButtonCorner.Parent = ButtonFrame
    
    local ButtonGlow = Instance.new("ImageLabel")
    ButtonGlow.Size = UDim2.new(1, 0, 1, 0)
    ButtonGlow.BackgroundTransparency = 1
    ButtonGlow.Image = "rbxassetid://6014261993"
    ButtonGlow.ImageColor3 = Color3.fromRGB(0, 255, 255)
    ButtonGlow.ImageTransparency = 0.9
    ButtonGlow.Parent = ButtonFrame
    
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(1, 0, 1, 0)
    Button.BackgroundTransparency = 1
    Button.Text = text .. "  [" .. key:upper() .. "]"
    Button.TextColor3 = Color3.fromRGB(200, 200, 255)
    Button.Font = Enum.Font.Gotham
    Button.TextSize = 14
    Button.TextXAlignment = Enum.TextXAlignment.Left
    Button.TextStrokeTransparency = 0.8
    Button.Parent = ButtonFrame
    
    local StatusIndicator = Instance.new("Frame")
    StatusIndicator.Size = UDim2.new(0, 10, 0, 10)
    StatusIndicator.Position = UDim2.new(1, -20, 0.5, -5)
    StatusIndicator.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    StatusIndicator.BorderSizePixel = 0
    StatusIndicator.Parent = ButtonFrame
    
    local StatusCorner = Instance.new("UICorner")
    StatusCorner.CornerRadius = UDim.new(1, 0)
    StatusCorner.Parent = StatusIndicator
    
    local enabled = false
    local connection
    
    Button.MouseButton1Click:Connect(function()
        enabled = not enabled
        if enabled then
            StatusIndicator.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
            ButtonFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 70)
            ButtonGlow.ImageTransparency = 0.7
            toggleFunc(true)
        else
            StatusIndicator.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
            ButtonFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 50)
            ButtonGlow.ImageTransparency = 0.9
            toggleFunc(false)
        end
    end)
    
    connection = Mouse.KeyDown:Connect(function(keyPressed)
        if keyPressed == key then
            enabled = not enabled
            if enabled then
                StatusIndicator.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
                ButtonFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 70)
                ButtonGlow.ImageTransparency = 0.7
                toggleFunc(true)
            else
                StatusIndicator.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
                ButtonFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 50)
                ButtonGlow.ImageTransparency = 0.9
                toggleFunc(false)
            end
        end
    end)
    
    table.insert(buttons, {enabled = enabled, connection = connection})
end

-- Fly with WASD control
local flying = false
local flySpeed = 50
local bodyGyro, bodyVelocity

local UserInputService = game:GetService("UserInputService")

createButton("Fly", "f", function(state)
    flying = state
    if state then
        Humanoid.PlatformStand = true
        bodyGyro = Instance.new("BodyGyro")
        bodyGyro.Parent = RootPart
        bodyGyro.MaxTorque = Vector3.new(0, 0, 0)
        bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Parent = RootPart
        bodyVelocity.MaxForce = Vector3.new(10000, 10000, 10000)
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        
        spawn(function()
            while flying do
                local moveDirection = Vector3.new(0, 0, 0)
                
                if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                    moveDirection = moveDirection + workspace.CurrentCamera.CFrame.LookVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                    moveDirection = moveDirection - workspace.CurrentCamera.CFrame.LookVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                    moveDirection = moveDirection - workspace.CurrentCamera.CFrame.RightVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                    moveDirection = moveDirection + workspace.CurrentCamera.CFrame.RightVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                    moveDirection = moveDirection + Vector3.new(0, 1, 0)
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
                    moveDirection = moveDirection - Vector3.new(0, 1, 0)
                end
                
                if moveDirection.Magnitude > 0 then
                    moveDirection = moveDirection.Unit * flySpeed
                end
                
                bodyVelocity.Velocity = moveDirection
                wait()
            end
        end)
    else
        Humanoid.PlatformStand = false
        if bodyGyro then bodyGyro:Destroy() bodyGyro = nil end
        if bodyVelocity then bodyVelocity:Destroy() bodyVelocity = nil end
    end
end)

-- Noclip
local noclipEnabled = false
local noclipConnection

createButton("Noclip", "n", function(state)
    noclipEnabled = state
    if state then
        for _, part in pairs(Character:GetDescendants()) do
            if part:IsA("BasePart") then part.CanCollide = false end
        end
        noclipConnection = Character.ChildAdded:Connect(function(child)
            if child:IsA("BasePart") and noclipEnabled then child.CanCollide = false end
        end)
    else
        if noclipConnection then noclipConnection:Disconnect() noclipConnection = nil end
        for _, part in pairs(Character:GetDescendants()) do
            if part:IsA("BasePart") then part.CanCollide = true end
        end
    end
end)

-- Speed
createButton("Speed", "x", function(state)
    if state then Humanoid.WalkSpeed = 100 else Humanoid.WalkSpeed = 16 end
end)

-- Jump
createButton("Jump", "j", function(state)
    if state then Humanoid.JumpPower = 200 else Humanoid.JumpPower = 50 end
end)

-- ESP
local espObjects = {}
local espConnection

createButton("ESP", "e", function(state)
    if state then
        for _, plr in pairs(game.Players:GetPlayers()) do
            if plr ~= Player then
                local highlight = Instance.new("Highlight")
                highlight.Parent = plr.Character
                highlight.FillColor = Color3.fromRGB(0, 255, 255)
                highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                highlight.FillTransparency = 0.5
                highlight.OutlineTransparency = 0.2
                table.insert(espObjects, highlight)
            end
        end
        espConnection = game.Players.PlayerAdded:Connect(function(plr)
            plr.CharacterAdded:Connect(function(char)
                if state then
                    local highlight = Instance.new("Highlight")
                    highlight.Parent = char
                    highlight.FillColor = Color3.fromRGB(0, 255, 255)
                    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                    highlight.FillTransparency = 0.5
                    highlight.OutlineTransparency = 0.2
                    table.insert(espObjects, highlight)
                end
            end)
        end)
    else
        if espConnection then espConnection:Disconnect() espConnection = nil end
        for _, obj in pairs(espObjects) do obj:Destroy() end
        espObjects = {}
    end
end)

-- Aimbot
local aimbotEnabled = false

createButton("Aimbot", "g", function(state)
    aimbotEnabled = state
    if state then
        spawn(function()
            while aimbotEnabled do
                local closestPlayer = nil
                local closestDistance = math.huge
                for _, plr in pairs(game.Players:GetPlayers()) do
                    if plr ~= Player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                        local distance = (RootPart.Position - plr.Character.HumanoidRootPart.Position).Magnitude
                        if distance < closestDistance then
                            closestDistance = distance
                            closestPlayer = plr
                        end
                    end
                end
                if closestPlayer and closestPlayer.Character and closestPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    RootPart.CFrame = CFrame.new(RootPart.Position, closestPlayer.Character.HumanoidRootPart.Position)
                end
                wait()
            end
        end)
    end
end)

-- Infinite Jump
local infiniteJumpEnabled = false
local infiniteJumpConnection

createButton("Infinite Jump", "h", function(state)
    infiniteJumpEnabled = state
    if state then
        Humanoid.JumpPower = 100
        Humanoid.Jump = true
        infiniteJumpConnection = Humanoid.StateChanged:Connect(function(oldState, newState)
            if newState == Enum.HumanoidStateType.Jumping and infiniteJumpEnabled then
                Humanoid.Jump = true
            end
        end)
    else
        if infiniteJumpConnection then infiniteJumpConnection:Disconnect() infiniteJumpConnection = nil end
        Humanoid.JumpPower = 50
    end
end)

-- Teleport
createButton("Teleport", "t", function(state)
    if state then
        if Mouse.Target then
            RootPart.CFrame = CFrame.new(Mouse.Hit.p)
        end
    end
end)

-- God Mode
local godModeEnabled = false
local godModeConnection

createButton("God Mode", "u", function(state)
    godModeEnabled = state
    if state then
        Humanoid.MaxHealth = math.huge
        Humanoid.Health = math.huge
        Humanoid.BreakJointsOnDeath = false
        godModeConnection = Humanoid.Died:Connect(function()
            if godModeEnabled then
                Humanoid.Health = math.huge
            end
        end)
    else
        if godModeConnection then godModeConnection:Disconnect() godModeConnection = nil end
        Humanoid.MaxHealth = 100
        Humanoid.Health = 100
    end
end)
