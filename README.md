local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "BC9_TSBG"
screenGui.ResetOnSpawn = false
screenGui.IgnoreGuiInset = true
screenGui.Parent = playerGui

local menuButton = Instance.new("TextButton")
menuButton.Size = UDim2.new(0, 60, 0, 40)
menuButton.Position = UDim2.new(0, 10, 0, 50)
menuButton.Text = "BC9"
menuButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
menuButton.TextColor3 = Color3.new(1, 1, 1)
menuButton.Font = Enum.Font.SourceSansBold
menuButton.TextSize = 20
menuButton.Parent = screenGui

local uicorner = Instance.new("UICorner")
uicorner.CornerRadius = UDim.new(0.3, 0)
uicorner.Parent = menuButton

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0.8, 0, 0.7, 0)
mainFrame.Position = UDim2.new(0.1, 0, 0.15, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.Visible = true
mainFrame.Parent = screenGui

local pageSelector = Instance.new("Frame")
pageSelector.Size = UDim2.new(1, 0, 0, 40)
pageSelector.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
pageSelector.Parent = mainFrame

local function createPageButton(name, index)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1/6, 0, 1, 0)
    button.Position = UDim2.new((index-1)/6, 0, 0, 0)
    button.Text = name
    button.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 16
    button.BorderSizePixel = 1
    button.Parent = pageSelector
    return button
end

local pageButtons = {
    createPageButton("Misc", 1),
    createPageButton("Tech", 2),
    createPageButton("Esp", 3),
    createPageButton("Aimbot", 4),
    createPageButton("Admin", 5),
    createPageButton("Server", 6)
}

local titleBar = Instance.new("TextLabel")
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.Position = UDim2.new(0, 0, 0, 40)
titleBar.Text = "BC9 TSBG"
titleBar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
titleBar.TextColor3 = Color3.new(1, 1, 1)
titleBar.Font = Enum.Font.SourceSansBold
titleBar.TextSize = 18
titleBar.Parent = mainFrame

local function createPage()
    local page = Instance.new("ScrollingFrame")
    page.Size = UDim2.new(1, 0, 1, -70)
    page.Position = UDim2.new(0, 0, 0, 70)
    page.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    page.CanvasSize = UDim2.new(0, 0, 0, 0)
    page.ScrollBarThickness = 8
    page.Visible = false
    page.Parent = mainFrame
    return page
end

local pages = {}
for i = 1, 6 do
    pages[i] = createPage()
end
pages[1].Visible = true

for i, btn in ipairs(pageButtons) do
    btn.MouseButton1Click:Connect(function()
        for _, p in ipairs(pages) do p.Visible = false end
        pages[i].Visible = true
    end)
end

menuButton.MouseButton1Click:Connect(function()
    mainFrame.Visible = not mainFrame.Visible
end)

local function addButton(page, name, code, isToggle, onCode, offCode)
    local btn = Instance.new("TextButton")
    local count = #page:GetChildren()
    btn.Size = UDim2.new(1, -10, 0, 40)
    btn.Position = UDim2.new(0, 5, 0, count * 45)
    btn.Text = name
    btn.BackgroundColor3 = Color3.fromRGB(130, 130, 130)
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.SourceSans
    btn.TextSize = 18
    btn.Parent = page
    page.CanvasSize = UDim2.new(0, 0, 0, (count + 1) * 45)

    if isToggle then
        local toggled = false
        btn.MouseButton1Click:Connect(function()
            toggled = not toggled
            btn.Text = toggled and name .. " [ON]" or name
            pcall(function() loadstring(toggled and onCode or offCode)() end)
        end)
    else
        btn.MouseButton1Click:Connect(function()
            pcall(function() loadstring(code)() end)
        end)
    end
end

addButton(pages[1], "Anti Lag", [[loadstring(game:HttpGet("https://raw.githubusercontent.com/Kietba/Kietba/refs/heads/main/Antilag%20script"))()]])
addButton(pages[1], "M1 Reach Rework", [[loadstring(game:HttpGet("https://raw.githubusercontent.com/Kietba/Kietba/refs/heads/main/M1%20Reach%20Rework"))()]])
addButton(pages[4], "Aimbot", [[loadstring(game:HttpGet("https://raw.githubusercontent.com/Kietba/Kietba/refs/heads/main/Aimlock%20By%20YQANTG"))()]])
addButton(pages[5], "Infinite Yield", [[loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()]])
