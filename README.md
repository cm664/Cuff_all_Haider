local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Active = false
local CurrentTool = nil

local TOOL_NAMES = {
    "\217\131\217\132\216\168\216\180\217\135 \217\135\217\138\217\132\217\136\217\131\217\138\216\170\217\135",
    "\217\131\217\132\216\168\216\180\216\169 \217\131\216\177\217\136\217\133\217\138",
    "\217\131\217\132\216\168\216\180\216\169 \216\168\216\167\216\170\217\133\216\167\217\134",
    "\217\131\217\132\216\168\216\180\216\169 \216\167\217\132\217\134\216\182\216\167\217\133",
    "\217\131\217\132\216\168\216\180\216\169 \216\167\217\132\216\167\217\134\216\172\217\138\217\132",
    "\217\131\217\132\216\168\216\180\216\169 \216\167\217\132\216\177\216\185\216\168",
    "\217\131\217\132\216\168\216\180\216\169",
    "\217\131\217\132\216\168\216\180\216\169\32\216\167\217\132\216\176\217\135\216\168\217\138\216\169",
    "\217\131\217\132\216\168\216\180\216\169\32\216\167\217\132\216\167\217\134\216\172\217\138\217\132",
    "\217\131\217\132\216\168\216\180\216\169\32\216\167\217\132\216\174\217\129\216\167\216\180",
    "\217\131\217\132\216\168\216\180\217\135\32\217\135\217\138\217\132\217\136\217\131\217\132\216\170\217\135",
    "\217\131\217\132\216\168\216\180\216\169\32\216\167\217\132\216\177\216\185\216\168",
    "\217\131\217\132\216\168\216\180\216\169\32\216\179\216\168\217\136\217\134\216\172\32\216\168\217\136\216\168",
    "\217\131\217\132\216\168\216\180\216\169\32\216\167\217\132\216\168\216\167\216\170\217\133\216\167\217\134",
    "\217\131\217\132\216\168\216\180\216\169\32\217\131\216\177\217\136\217\133\217\138",
    "كلبشة الخفاش"
}

local ScreenGui = Instance.new("ScreenGui", game:GetService("CoreGui"))
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 170, 0, 196)
MainFrame.Position = UDim2.new(0.5, -85, 0.3, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 22)
MainFrame.BackgroundTransparency = 0.1
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.BorderSizePixel = 1
MainFrame.BorderColor3 = Color3.fromRGB(0, 210, 255)
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 8)

local ToggleGuiBtn = Instance.new("TextButton", ScreenGui)
ToggleGuiBtn.Size = UDim2.new(0, 35, 0, 35)
ToggleGuiBtn.Position = UDim2.new(0.5, 95, 0.3, 0)
ToggleGuiBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
ToggleGuiBtn.Text = "خفاء"
ToggleGuiBtn.TextColor3 = Color3.fromRGB(0, 210, 255)
ToggleGuiBtn.Font = Enum.Font.GothamBold
ToggleGuiBtn.TextSize = 12
ToggleGuiBtn.BorderSizePixel = 1
ToggleGuiBtn.BorderColor3 = Color3.fromRGB(0, 210, 255)
ToggleGuiBtn.Active = true
ToggleGuiBtn.Draggable = true
Instance.new("UICorner", ToggleGuiBtn).CornerRadius = UDim.new(1, 0)

local function createBtn(text, pos, color)
    local btn = Instance.new("TextButton", MainFrame)
    btn.Size = UDim2.new(1, -16, 0, 36)
    btn.Position = pos
    btn.Text = text
    btn.BackgroundColor3 = Color3.fromRGB(22, 25, 35)
    btn.TextColor3 = Color3.fromRGB(240, 240, 240)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 12
    btn.BorderSizePixel = 1
    btn.BorderColor3 = color
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    return btn
end

local StealBtn = createBtn("سرقة كل الكلبشات", UDim2.new(0, 8, 0, 10), Color3.fromRGB(0, 255, 130))
local ToggleBtn = createBtn("تفعيل الكلبشة: إيقاف", UDim2.new(0, 8, 0, 56), Color3.fromRGB(255, 170, 0))
local ClearBtn = createBtn("حذف كلبشاتي", UDim2.new(0, 8, 0, 102), Color3.fromRGB(255, 255, 255))
local ExitBtn = createBtn("الخروج من اللعبة", UDim2.new(0, 8, 0, 148), Color3.fromRGB(255, 0, 85))

ToggleGuiBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
    ToggleGuiBtn.Text = MainFrame.Visible and "خفاء" or "إظهار"
    ToggleGuiBtn.BorderColor3 = MainFrame.Visible and Color3.fromRGB(0, 210, 255) or Color3.fromRGB(0, 255, 130)
    ToggleGuiBtn.TextColor3 = MainFrame.Visible and Color3.fromRGB(0, 210, 255) or Color3.fromRGB(0, 255, 130)
end)

StealBtn.MouseButton1Click:Connect(function()
    for _, name in ipairs(TOOL_NAMES) do
        if not (LocalPlayer.Backpack:FindFirstChild(name) or (LocalPlayer.Character and LocalPlayer.Character:FindFirstChild(name))) then
            for _, p in ipairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Backpack and p.Backpack:FindFirstChild(name) then 
                    local original = p.Backpack[name]
                    local clone = original:Clone()
                    clone.Parent = LocalPlayer.Backpack
                    break 
                end
            end
        end
    end
end)

ToggleBtn.MouseButton1Click:Connect(function()
    Active = not Active
    ToggleBtn.Text = Active and "تفعيل الكلبشة: يعمل" or "تفعيل الكلبشة: إيقاف"
end)

ClearBtn.MouseButton1Click:Connect(function()
    Active = false
    ToggleBtn.Text = "تفعيل الكلبشة: إيقاف"
    CurrentTool = nil
    for _, item in ipairs(LocalPlayer.Backpack:GetChildren()) do
        if table.find(TOOL_NAMES, item.Name) then item:Destroy() end
    end
    if LocalPlayer.Character then
        for _, item in ipairs(LocalPlayer.Character:GetChildren()) do
            if item:IsA("Tool") and table.find(TOOL_NAMES, item.Name) then item:Destroy() end
        end
    end
end)

ExitBtn.MouseButton1Click:Connect(function() game:Shutdown() end)

task.spawn(function()
    while task.wait(0.1) do
        if Active then
            if not CurrentTool or not CurrentTool.Parent then 
                for _, name in ipairs(TOOL_NAMES) do
                    if LocalPlayer.Backpack:FindFirstChild(name) then
                        CurrentTool = LocalPlayer.Backpack[name]
                        break
                    end
                end
            end
            
            if CurrentTool then
                local remote = CurrentTool:FindFirstChild("RemoteEvent") or CurrentTool:FindFirstChildOfClass("RemoteEvent")
                if remote then
                    for _, p in ipairs(Players:GetPlayers()) do
                        if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                            remote:FireServer("Cuff", p.Character.HumanoidRootPart)
                        end
                    end
                end
            end
        end
    end
end)

