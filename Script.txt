-- Auto Clicker + AntiLag + GUI Size (mobile)
if not game:IsLoaded() then game.Loaded:Wait() end

local Players = game:GetService("Players")
local VIM = game:GetService("VirtualInputManager")
local Lighting = game:GetService("Lighting")
local lp = Players.LocalPlayer

-- remove old copies
pcall(function() game:GetService("CoreGui").MiniClicker:Destroy() end)
pcall(function() lp.PlayerGui.MiniClicker:Destroy() end)
pcall(function() gethui().MiniClicker:Destroy() end)

local gui = Instance.new("ScreenGui")
gui.Name = "MiniClicker"
gui.ResetOnSpawn = false
gui.DisplayOrder = 999
local ok = pcall(function()
    gui.Parent = (gethui and gethui()) or game:GetService("CoreGui")
end)
if not ok or not gui.Parent then
    gui.Parent = lp:WaitForChild("PlayerGui")
end

local function corner(o, r)
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, r or 6)
    c.Parent = o
end

-- ===== 5 second loading box =====
local splash = Instance.new("TextLabel")
splash.Size = UDim2.fromOffset(220, 50)
splash.Position = UDim2.new(0.5, -110, 0.1, 0)
splash.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
splash.TextColor3 = Color3.new(1, 1, 1)
splash.Font = Enum.Font.GothamBold
splash.TextSize = 15
splash.Parent = gui
corner(splash, 8)
for i = 5, 1, -1 do
    splash.Text = "Auto Clicker loading... " .. i
    task.wait(1)
end
splash:Destroy()

-- ===== Main window =====
local W, H = 190, 250
local main = Instance.new("Frame")
main.Size = UDim2.fromOffset(W, H)
main.Position = UDim2.new(0, 15, 0.15, 0)
main.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
main.BorderSizePixel = 0
main.Active = true
main.Draggable = true
main.Parent = gui
corner(main, 8)

local scale = Instance.new("UIScale")
scale.Scale = 1
scale.Parent = main

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -30, 0, 24)
title.BackgroundTransparency = 1
title.Text = "  Auto Clicker + AntiLag"
title.TextColor3 = Color3.new(1, 1, 1)
title.TextXAlignment = Enum.TextXAlignment.Left
title.Font = Enum.Font.GothamBold
title.TextSize = 12
title.Parent = main

local mini = Instance.new("TextButton")
mini.Size = UDim2.fromOffset(24, 24)
mini.Position = UDim2.new(1, -26, 0, 0)
mini.BackgroundTransparency = 1
mini.Text = "-"
mini.TextColor3 = Color3.new(1, 1, 1)
mini.TextSize = 18
mini.Font = Enum.Font.GothamBold
mini.Parent = main

local body = Instance.new("Frame")
body.Size = UDim2.new(1, 0, 1, -24)
body.Position = UDim2.fromOffset(0, 24)
body.BackgroundTransparency = 1
body.Parent = main

local minimized = false
mini.MouseButton1Click:Connect(function()
    minimized = not minimized
    body.Visible = not minimized
    main.Size = minimized and UDim2.fromOffset(W, 24) or UDim2.fromOffset(W, H)
    mini.Text = minimized and "+" or "-"
end)

-- ===== helpers =====
local function btn(par, text, y, color, h)
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(1, -16, 0, h or 26)
    b.Position = UDim2.fromOffset(8, y)
    b.BackgroundColor3 = color or Color3.fromRGB(70, 90, 160)
    b.Text = text
    b.TextColor3 = Color3.new(1, 1, 1)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 11
    b.Parent = par
    corner(b, 6)
    return b
end

local function label(par, text, y, h)
    local l = Instance.new("TextLabel")
    l.Size = UDim2.new(1, -16, 0, h or 16)
    l.Position = UDim2.fromOffset(8, y)
    l.BackgroundTransparency = 1
    l.Text = text
    l.TextColor3 = Color3.fromRGB(170, 170, 170)
    l.Font = Enum.Font.Gotham
    l.TextSize = 10
    l.TextWrapped = true
    l.Parent = par
    return l
end

local function row(par, y, text, default)
    local l = Instance.new("TextLabel")
    l.Size = UDim2.new(0.5, -6, 0, 26)
    l.Position = UDim2.fromOffset(8, y)
    l.BackgroundTransparency = 1
    l.Text = text
    l.TextColor3 = Color3.fromRGB(200, 200, 200)
    l.TextXAlignment = Enum.TextXAlignment.Left
    l.Font = Enum.Font.Gotham
    l.TextSize = 10
    l.TextWrapped = true
    l.Parent = par

    local b = Instance.new("TextBox")
    b.Size = UDim2.new(0.5, -14, 0, 26)
    b.Position = UDim2.new(0.5, 6, 0, y)
    b.BackgroundColor3 = Color3.fromRGB(45,
