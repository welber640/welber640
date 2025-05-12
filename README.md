_G.infinjump = not _G.infinjump

local plr = game:GetService'Players'.LocalPlayer
local m = plr:GetMouse()
m.KeyDown:connect(function(k)
    if _G.infinjump then
        if k:byte() == 32 then
            plrh = game:GetService'Players'.LocalPlayer.Character:FindFirstChildOfClass'Humanoid'
            plrh:ChangeState('Jumping')
            wait()
            plrh:ChangeState('Seated')
        end
    end
end)

local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local camera = game.Workspace.CurrentCamera
local UIS = game:GetService("UserInputService")
local isFollowing = false
local targetPlayer = nil

local function toggleCameraFollow()
    isFollowing = not isFollowing
end

UIS.InputBegan:Connect(function(input, gameProcessedEvent)
    if input.KeyCode == Enum.KeyCode.U then
        toggleCameraFollow()
    end
end)

local function findClosestPlayer()
    local closestPlayer = nil
    local shortestDistance = math.huge

    for _, otherPlayer in pairs(game.Players:GetPlayers()) do
        if otherPlayer ~= player and otherPlayer.Character then
            local humanoidRootPart = otherPlayer.Character:FindFirstChild("HumanoidRootPart")
            if humanoidRootPart then
                local distance = (player.Character.HumanoidRootPart.Position - humanoidRootPart.Position).Magnitude
                if distance < shortestDistance then
                    shortestDistance = distance
                    closestPlayer = otherPlayer
                end
            end
        end
    end

    return closestPlayer
end

game:GetService("RunService").Heartbeat:Connect(function()
    if isFollowing then
        targetPlayer = findClosestPlayer()
        if targetPlayer and targetPlayer.Character then
            local humanoidRootPart = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
            if humanoidRootPart then
                camera.CFrame = CFrame.new(camera.CFrame.Position, humanoidRootPart.Position)
            end
        end
    end
end)
