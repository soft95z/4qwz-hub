--[[ 
    4QWZ BEST DUEL SCRIPT - V11 (RUNNING AIMBOT)
    - Fix: Aimbot runs toward target (no teleport)
    - Fix: Bat Spam Auto-Equip
    - UI: Rainbow, Speed Titles, "Soon..." Widgets
--]]

local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")
local Lighting = game:GetService("Lighting")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Player = Players.LocalPlayer

-- Nettoyage
if CoreGui:FindFirstChild("4qwz_Final_V11") then CoreGui:FindFirstChild("4qwz_Final_V11"):Destroy() end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "4qwz_Final_V11"
ScreenGui.Parent = CoreGui
ScreenGui.ResetOnSpawn = false

-- --- UTILS ---
local function applyRainbow(instance)
    task.spawn(function()
        while task.wait() do
            for i = 0, 1, 0.01 do
                local col = Color3.fromHSV(i, 0.8, 1)
                if instance:IsA("TextLabel") or instance:IsA("TextButton") then instance.TextColor3 = col
                elseif instance:IsA("UIStroke") then instance.Color = col end
                task.wait(0.03)
            end
        end
    end)
end

local function makeDraggable(frame)
    local dragging, dragStart, startPos
    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true; dragStart = input.Position; startPos = frame.Position
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end
    end)
end

-- --- MAIN FRAME ---
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 420, 0, 520); MainFrame.Position = UDim2.new(0.5, -210, 0.5, -260)
MainFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 15); MainFrame.Active = true; MainFrame.Parent = ScreenGui
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 15)
local MainStroke = Instance.new("UIStroke", MainFrame); MainStroke.Thickness = 3; applyRainbow(MainStroke)

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 60); Title.Text = "4QWZ BEST DUEL SCRIPT"; Title.TextSize = 24; Title.Font = "LuckiestGuy"
Title.BackgroundTransparency = 1; Title.Parent = MainFrame; applyRainbow(Title)

local Scroll = Instance.new("ScrollingFrame")
Scroll.Size = UDim2.new(0.9, 0, 0.65, 0); Scroll.Position = UDim2.new(0.05, 0, 0.15, 0)
Scroll.BackgroundTransparency = 1; Scroll.ScrollBarThickness = 0; Scroll.Parent = MainFrame
Instance.new("UIListLayout", Scroll).Padding = UDim.new(0, 8)

-- --- WIDGETS ---
local function CreateWidget(name, sizeX, sizeY)
    local W = Instance.new("Frame")
    W.Size = UDim2.new(0, sizeX, 0, sizeY); W.Position = UDim2.new(0.75, 0, 0.3, 0)
    W.BackgroundColor3 = Color3.fromRGB(15, 15, 25); W.Visible = false; W.Parent = ScreenGui
    Instance.new("UICorner", W); local S = Instance.new("UIStroke", W); S.Thickness = 2; applyRainbow(S)
    makeDraggable(W)
    local T = Instance.new("TextLabel")
    T.Size = UDim2.new(1, 0, 0, 30); T.Text = name; T.Font = "FredokaOne"; T.BackgroundTransparency = 1; T.Parent = W; applyRainbow(T)
    return W
end

local SpeedWidget = CreateWidget("SPEED CONTROL", 220, 200)
local AimWidget = CreateWidget("BAT AIMBOT", 180, 100)
local RightWidget = CreateWidget("AUTO RIGHT", 150, 80)
local LeftWidget = CreateWidget("AUTO LEFT", 150, 80)

-- Auto Soon
local function AddSoon(w)
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(0.8, 0, 0, 30); b.Position = UDim2.new(0.1,0,0.5,0); b.Text = "ACTIVATE"
    b.Parent = w; Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(function() b.Text = "Soon..." end)
end
AddSoon(RightWidget); AddSoon(LeftWidget)

-- Speed Inputs
local _G_Speed = 16
local function SpeedInput(title, pos)
    local t = Instance.new("TextLabel")
    t.Size = UDim2.new(1,0,0,20); t.Position = UDim2.new(0,0,pos-0.12,0); t.Text = title
    t.TextColor3 = Color3.fromRGB(255,255,255); t.Font = "FredokaOne"; t.BackgroundTransparency = 1; t.Parent = SpeedWidget
    local box = Instance.new("TextBox")
    box.Size = UDim2.new(0.8,0,0,30); box.Position = UDim2.new(0.1,0,pos,0); box.Text = "16"
    box.BackgroundColor3 = Color3.fromRGB(30,30,45); box.TextColor3 = Color3.fromRGB(255,255,255); box.Parent = SpeedWidget; Instance.new("UICorner", box)
    box.FocusLost:Connect(function() _G_Speed = tonumber(box.Text) or 16 end)
end
SpeedInput("SPEED REGULAR", 0.3); SpeedInput("SPEED STEALING", 0.7)

-- --- CORE LOGIC ---
_G.InfJ = false; _G.Spin = false; _G.Aim = false; _G.Neon = false; _G.Spam = false; _G.AntiRag = false; _G.Grab = false

RunService.Heartbeat:Connect(function()
    local char = Player.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return end
    local hrp = char.HumanoidRootPart
    local hum = char.Humanoid

    -- Anti-Ragdoll
    if _G.AntiRag then
        hum:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.FallingDown, false)
        hum.PlatformStand = false
    end

    -- Speed Control
    hum.WalkSpeed = _G_Speed

    -- Spin
    if _G.Spin then hrp.CFrame = hrp.CFrame * CFrame.Angles(0, math.rad(25), 0) end

    -- Bat & Aimbot
    if _G.Spam or _G.Aim then
        local tool = char:FindFirstChildOfClass("Tool") or Player.Backpack:FindFirstChildOfClass("Tool")
        if tool then
            if tool.Parent ~= char then hum:EquipTool(tool) end
            
            if _G.Aim then
                local target = nil; local dist = 100 -- Rayon de détection
                for _, p in pairs(Players:GetPlayers()) do
                    if p ~= Player and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character.Humanoid.Health > 0 then
                        local d = (p.Character.HumanoidRootPart.Position - hrp.Position).Magnitude
                        if d < dist then dist = d; target = p.Character end
                    end
                end
                
                if target then
                    -- COURIR VERS LA CIBLE
                    hum:MoveTo(target.HumanoidRootPart.Position)
                    -- FRAPPER SI ASSEZ PROCHE
                    if (target.HumanoidRootPart.Position - hrp.Position).Magnitude < 10 then
                        tool:Activate()
                    end
                end
            elseif _G.Spam then
                tool:Activate()
            end
        end
    end

    -- Auto Grab
    if _G.Grab then
        for _, v in pairs(workspace:GetDescendants()) do
            if v:IsA("TouchTransmitter") and (v.Parent.Name:lower():find("brainrot") or v.Parent:IsA("Tool")) then
                firetouchinterest(hrp, v.Parent, 0)
                firetouchinterest(hrp, v.Parent, 1)
            end
        end
    end
end)

-- Jump
UserInputService.JumpRequest:Connect(function()
    if _G.InfJ and Player.Character then Player.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end
end)

-- Neon
task.spawn(function()
    while task.wait(0.5) do
        if _G.Neon then Lighting.Ambient = Color3.fromRGB(30,0,60) Lighting.ClockTime = 0 end
    end
end)

-- --- TOGGLES ---
local function AddTog(name, callback)
    local s = false; local b = Instance.new("TextButton")
    b.Size = UDim2.new(0.95, 0, 0, 40); b.Text = "○ " .. name; b.Parent = Scroll
    b.BackgroundColor3 = Color3.fromRGB(30, 25, 40); b.TextColor3 = Color3.fromRGB(200,200,200); b.Font = "FredokaOne"; Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(function() 
        s = not s; callback(s); b.Text = s and "● " .. name or "○ " .. name
        b.TextColor3 = s and Color3.fromRGB(0, 255, 150) or Color3.fromRGB(200, 200, 200)
    end)
end

AddTog("Speed Widget", function(v) SpeedWidget.Visible = v end)
AddTog("Bat Aimbot Widget", function(v) AimWidget.Visible = v end)
AddTog("Auto Right Widget", function(v) RightWidget.Visible = v end)
AddTog("Auto Left Widget", function(v) LeftWidget.Visible = v end)
AddTog("Auto Grab Brainrot", function(v) _G.Grab = v end)
AddTog("Anti Ragdoll", function(v) _G.AntiRag = v end)
AddTog("Infinite Jump", function(v) _G.InfJ = v end)
AddTog("Spin Bot", function(v) _G.Spin = v end)
AddTog("Dark Neon Sky", function(v) _G.Neon = v if not v then Lighting.Ambient = Color3.fromRGB(127,127,127) Lighting.ClockTime = 12 end end)
AddTog("Spam Bat", function(v) _G.Spam = v end)

-- Resize Buttons
local SizeFrame = Instance.new("Frame")
SizeFrame.Size = UDim2.new(0.9, 0, 0, 40); SizeFrame.Position = UDim2.new(0.05, 0, 0.88, 0); SizeFrame.BackgroundTransparency = 1; SizeFrame.Parent = MainFrame
local function CreateSizeBtn(txt, offset, posX)
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(0, 100, 0, 30); b.Position = UDim2.new(posX, 0, 0, 0); b.Text = txt; b.BackgroundColor3 = Color3.fromRGB(255,215,0); b.Parent = SizeFrame; Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(function() MainFrame.Size = UDim2.new(0, math.max(250, MainFrame.Size.X.Offset + offset), 0, math.max(200, MainFrame.Size.Y.Offset + offset)) end)
end
CreateSizeBtn("RÉDUIRE", -40, 0.05); CreateSizeBtn("AGRANDIR", 40, 0.6)

makeDraggable(MainFrame)
MainFrame.Size = UDim2.new(0, 0, 0, 0)
TweenService:Create(MainFrame, TweenInfo.new(0.8, Enum.EasingStyle.Elastic), {Size = UDim2.new(0, 420, 0, 520)}):Play()
