local player = game.Players.LocalPlayer
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "Chat GPT Hub", HidePremium = false, SaveConfig = true, ConfigFolder = "OrionTest"})

local Tab = Window:MakeTab({
    Name = "Tab 1",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

local Section = Tab:AddSection({
    Name = "LocalPlayer"
})

OrionLib:MakeNotification({
    Name = "Welcome!",
    Content = "Welcome to my hub!",
    Image = "rbxassetid://4483345998",
    Time = 5
})

local autoFarmActive = false
local lastNpc = nil

local function activateHaki()
    local hakiKey = "J"
    game:GetService("VirtualInputManager"):SendKeyEvent(true, hakiKey, false, game)
    wait(0.5)
end

local function autoClick()
    game:GetService("VirtualUser"):CaptureController()
    game:GetService("VirtualUser"):Button1Down(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
end

local function isInWater()
    local character = player.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        local position = character.HumanoidRootPart.Position
        return workspace:FindPartOnRay(Ray.new(position, Vector3.new(0, -5, 0))) == workspace.Water
    end
    return false
end

local function autoFarm()
    autoFarmActive = true
    while autoFarmActive do
        wait(1)
        
        for _, npc in pairs(workspace.Enemies:GetChildren()) do
            if npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                player.Character.HumanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame
                activateHaki()
                lastNpc = npc
                
                while npc.Humanoid.Health > 0 and autoFarmActive do
                    if isInWater() then
                        player.Character.HumanoidRootPart.CFrame = lastNpc.HumanoidRootPart.CFrame
                    else
                        autoClick()
                    end
                    wait(0.1)
                end
                wait(1)
            end
        end
    end
end

Tab:AddButton({
    Name = "Auto Farm",
    Callback = function()
        if not autoFarmActive then
            autoFarm()
            OrionLib:MakeNotification({
                Name = "Auto Farm",
                Content = "Auto Farm ativado com Haki e auto click!",
                Image = "rbxassetid://4483345998",
                Time = 5
            })
        else
            autoFarmActive = false
            OrionLib:MakeNotification({
                Name = "Auto Farm",
                Content = "Auto Farm desativado!",
                Image = "rbxassetid://4483345998",
                Time = 5
            })
        end
    end
})
