local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

local teleportEnabled = false
local targetPlayer = nil

-- Tạo GUI
local gui = Instance.new("ScreenGui")
gui.Name = "AutoTeleportGui"
gui.ResetOnSpawn = false
gui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 250)
frame.Position = UDim2.new(0, 20, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Parent = gui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Text = "Auto Teleport Player"
title.TextColor3 = Color3.new(1, 1, 1)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 22
title.Parent = frame

local playerList = Instance.new("ScrollingFrame")
playerList.Size = UDim2.new(1, -20, 0, 150)
playerList.Position = UDim2.new(0, 10, 0, 40)
playerList.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
playerList.BorderSizePixel = 0
playerList.CanvasSize = UDim2.new(0, 0, 0, 0)
playerList.ScrollBarThickness = 6
playerList.Parent = frame

local uiListLayout = Instance.new("UIListLayout")
uiListLayout.Parent = playerList
uiListLayout.SortOrder = Enum.SortOrder.LayoutOrder
uiListLayout.Padding = UDim.new(0, 5)

local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(1, -20, 0, 40)
toggleButton.Position = UDim2.new(0, 10, 1, -60)
toggleButton.BackgroundColor3 = Color3.fromRGB(60, 180, 60)
toggleButton.TextColor3 = Color3.new(1, 1, 1)
toggleButton.Text = "Bật Dịch Chuyển"
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextSize = 18
toggleButton.Parent = frame

local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(1, -20, 0, 30)
closeButton.Position = UDim2.new(0, 10, 1, -25)
closeButton.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
closeButton.TextColor3 = Color3.new(1, 1, 1)
closeButton.Text = "❌ Đóng GUI"
closeButton.Font = Enum.Font.SourceSansBold
closeButton.TextSize = 18
closeButton.Parent = frame

-- Hàm update danh sách người chơi
local function updatePlayerList()
    -- Xóa các nút cũ
    for _, child in pairs(playerList:GetChildren()) do
        if child:IsA("TextButton") then
            child:Destroy()
        end
    end

    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            local btn = Instance.new("TextButton")
            btn.Size = UDim2.new(1, -10, 0, 30)
            btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
            btn.TextColor3 = Color3.new(1, 1, 1)
            btn.Font = Enum.Font.SourceSans
            btn.TextSize = 16
            btn.Text = player.Name
            btn.Parent = playerList

            btn.MouseButton1Click:Connect(function()
                targetPlayer = player
                print("Đã chọn:", player.Name)
            end)
        end
    end

    -- Update canvas size cho scrolling frame
    local layout = playerList:FindFirstChildOfClass("UIListLayout")
    if layout then
        playerList.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 10)
    end
end

-- Gọi lần đầu
updatePlayerList()

-- Nút bật tắt dịch chuyển
toggleButton.MouseButton1Click:Connect(function()
    if teleportEnabled then
        teleportEnabled = false
        toggleButton.Text = "Bật Dịch Chuyển"
        toggleButton.BackgroundColor3 = Color3.fromRGB(60, 180, 60)
        print("Tắt dịch chuyển.")
    else
        if not targetPlayer then
            warn("Vui lòng chọn người chơi trước khi bật dịch chuyển.")
            return
        end
        teleportEnabled = true
        toggleButton.Text = "Tắt Dịch Chuyển"
        toggleButton.BackgroundColor3 = Color3.fromRGB(180, 60, 60)
        print("Bật dịch chuyển đến:", targetPlayer.Name)
    end
end)

-- Nút đóng GUI
closeButton.MouseButton1Click:Connect(function()
    teleportEnabled = false
    gui:Destroy()
    print("Đã đóng GUI.")
end)

-- Update liên tục dịch chuyển nếu bật
RunService.RenderStepped:Connect(function()
    if teleportEnabled and targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local targetPos = targetPlayer.Character.HumanoidRootPart.CFrame
        local character = LocalPlayer.Character
        if character and character:FindFirstChild("HumanoidRootPart") then
            -- Teleport local player tới vị trí HumanoidRootPart của người mục tiêu
            character.HumanoidRootPart.CFrame = targetPos * CFrame.new(0, 3, 0)
        end
    end
end)

-- Update danh sách khi có người chơi mới vào hoặc ra
Players.PlayerAdded:Connect(updatePlayerList)
Players.PlayerRemoving:Connect(updatePlayerList)
