-- ROBLOX ADMIN SCRIPT
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local RootPart = Character:WaitForChild("HumanoidRootPart")
local Mouse = Player:GetMouse()

-- Infinite Yield
loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()

-- GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = Player.PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 200, 0, 300)
MainFrame.Position = UDim2.new(0, 50, 0, 50)
MainFrame.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
MainFrame.BackgroundTransparency = 0.3
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
Title.Text = "ROBLOX ADMIN SCRIPT"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 18
Title.Parent = MainFrame

local ScrollFrame = Instance.new("ScrollingFrame")
ScrollFrame.Size = UDim2.new(1, 0, 1, -30)
ScrollFrame.Position = UDim2.new(0, 0, 0, 30)
ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 500)
ScrollFrame.ScrollBarThickness = 5
ScrollFrame.Parent = MainFrame

local function createButton(text, key, toggleFunc)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(1, -10, 0, 30)
    Button.Position = UDim2.new(0, 5, 0, #ScrollFrame:GetChildren() * 35)
    Button.BackgroundColor3 = Color3.new(0.3, 0.3, 0.3)
    Button.Text = text .. " [" .. key:upper() .. "]"
    Button.TextColor3 = Color3.new(1, 1, 1)
    Button.Font = Enum.Font.SourceSans
    Button.TextSize = 16
    Button.Parent = ScrollFrame
    
    local enabled = false
    Button.MouseButton1Click:Connect(function()
        enabled = not enabled
        if enabled then
            Button.BackgroundColor3 = Color3.new(0, 1, 0)
            toggleFunc(true)
        else
            Button.BackgroundColor3 = Color3.new(0.3, 0.3, 0.3)
            toggleFunc(false)
        end
    end)
    
    Mouse.KeyDown:Connect(function(keyPressed)
        if keyPressed == key then
            enabled = not enabled
            if enabled then
                Button.BackgroundColor3 = Color3.new(0, 1, 0)
                toggleFunc(true)
            else
                Button.BackgroundColor3 = Color3.new(0.3, 0.3, 0.3)
                toggleFunc(false)
            end
        end
    end)
end

-- Fly
createButton("Fly", "f", function(state)
    if state then
        Humanoid.PlatformStand = true
        local bodyGyro = Instance.new("BodyGyro")
        bodyGyro.Parent = RootPart
        bodyGyro.MaxTorque = Vector3.new(0, 0, 0)
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Parent = RootPart
        bodyVelocity.MaxForce = Vector3.new(10000, 10000, 10000)
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        spawn(function()
            while state do
                local moveDirection = Vector3.new(0, 0, 0)
                if Mouse.Target then
                    local target = Mouse.Hit.p
                    local direction = (target - RootPart.Position).Unit
                    moveDirection = direction
                end
                bodyVelocity.Velocity = moveDirection * 50
                wait()
            end
        end)
    else
        Humanoid.PlatformStand = false
        if RootPart:FindFirstChild("BodyGyro") then RootPart.BodyGyro:Destroy() end
        if RootPart:FindFirstChild("BodyVelocity") then RootPart.BodyVelocity:Destroy() end
    end
end)

-- Noclip
createButton("Noclip", "n", function(state)
    if state then
        for _, part in pairs(Character:GetDescendants()) do
            if part:IsA("BasePart") then part.CanCollide = false end
        end
        Character.ChildAdded:Connect(function(child)
            if child:IsA("BasePart") and state then child.CanCollide = false end
        end)
    else
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
createButton("ESP", "e", function(state)
    if state then
        for _, plr in pairs(game.Players:GetPlayers()) do
            if plr ~= Player then
                local highlight = Instance.new("Highlight")
                highlight.Parent = plr.Character
                highlight.FillColor = Color3.new(1, 0, 0)
                highlight.OutlineColor = Color3.new(1, 1, 1)
                table.insert(espObjects, highlight)
            end
        end
        game.Players.PlayerAdded:Connect(function(plr)
            plr.CharacterAdded:Connect(function(char)
                if state then
                    local highlight = Instance.new("Highlight")
                    highlight.Parent = char
                    highlight.FillColor = Color3.new(1, 0, 0)
                    highlight.OutlineColor = Color3.new(1, 1, 1)
                    table.insert(espObjects, highlight)
                end
            end)
        end)
    else
        for _, obj in pairs(espObjects) do obj:Destroy() end
        espObjects = {}
    end
end)

-- Aimbot
createButton("Aimbot", "g", function(state)
    if state then
        spawn(function()
            while state do
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
createButton("Infinite Jump", "h", function(state)
    if state then
        Humanoid.JumpPower = 100
        Humanoid.Jump = true
        Humanoid.StateChanged:Connect(function(oldState, newState)
            if newState == Enum.HumanoidStateType.Jumping and state then
                Humanoid.Jump = true
            end
        end)
    else
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
createButton("God Mode", "u", function(state)
    if state then
        Humanoid.MaxHealth = math.huge
        Humanoid.Health = math.huge
        Humanoid.BreakJointsOnDeath = false
        Humanoid.Died:Connect(function()
            Humanoid.Health = math.huge
        end)
    else
        Humanoid.MaxHealth = 100
        Humanoid.Health = 100
    end
end)
