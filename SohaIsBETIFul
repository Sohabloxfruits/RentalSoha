-- Load Kavo UI Library
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()

-- Define Colors for Custom Theme
local defaultColors = {
    SchemeColor = Color3.fromRGB(0, 255, 255),
    Background = Color3.fromRGB(0, 0, 0),
    Header = Color3.fromRGB(0, 0, 0),
    TextColor = Color3.fromRGB(255, 255, 255),
    ElementColor = Color3.fromRGB(20, 20, 20)
}

local colors = defaultColors

-- Create Main UI Window
local Window = Library.CreateLib("Universal Script Hub", colors)

-- Create Tabs
local MainTab = Window:NewTab("Main")
local ESPTab = Window:NewTab("ESP")
local BloxFruitsTab = Window:NewTab("BloxFruits")
local SettingsTab = Window:NewTab("Settings")

-- Create Sections
local MainSection = MainTab:NewSection("Main Features")
local ESPSection = ESPTab:NewSection("ESP Features")
local BloxFruitsSection = BloxFruitsTab:NewSection("BloxFruits Features")
local SettingsSection = SettingsTab:NewSection("Settings")

-- Variables
local mainUIVisible = true
local ESPEnabled = false
local AutoGKEnabled = false
local AutoFarmEnabled = false

-- ESP Feature
local function ToggleESP(state)
    ESPEnabled = state
    if ESPEnabled then
        print("ESP Enabled")
        -- Code for enabling ESP: Highlight players, parts, etc.
    else
        print("ESP Disabled")
        -- Code for disabling ESP
    end
end

ESPSection:NewToggle("Enable ESP", "Highlights all players and parts within range", function(state)
    ToggleESP(state)
end)

-- AutoFarm Feature
local function AutoFarm()
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()

    while AutoFarmEnabled do
        wait(0.5) -- Small delay to prevent overwhelming the game

        -- Find nearest NPC that can be fought within workspace.NPCs
        local nearestNPC
        local nearestDistance = math.huge

        for _, npc in pairs(workspace.NPCs:GetChildren()) do
            if npc:IsA("Model") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                local distance = (npc.HumanoidRootPart.Position - character.HumanoidRootPart.Position).Magnitude
                if distance < nearestDistance then
                    nearestDistance = distance
                    nearestNPC = npc
                end
            end
        end

        -- If an NPC is found, teleport to it and start attacking
        if nearestNPC then
            character:MoveTo(nearestNPC.HumanoidRootPart.Position)
            wait(0.3)  -- Wait briefly for teleport
            keypress(Enum.KeyCode.One) -- Press '1' to equip weapon
            wait(0.1)
            mouse1press() -- Start autoclicking
            wait(0.5)
            mouse1release()
            print("AutoFarm attacking: " .. nearestNPC.Name)
        end
    end
end

BloxFruitsSection:NewToggle("Enable AutoFarm", "Teleports to nearest NPC and starts attacking", function(state)
    AutoFarmEnabled = state
    if AutoFarmEnabled then
        AutoFarm()
    else
        print("AutoFarm Disabled")
    end
end)

-- Teleport Tool
local function CreateTeleportTool()
    local tool = Instance.new("Tool")
    tool.RequiresHandle = false
    tool.Name = "TeleportTool"
    tool.Activated:Connect(function()
        local player = game.Players.LocalPlayer
        local mouse = player:GetMouse()
        if mouse.Target then
            local teleportPosition = mouse.Hit.p
            player.Character:MoveTo(teleportPosition)
            print("Teleported to: " .. tostring(teleportPosition))
        end
    end)
    tool.Parent = game.Players.LocalPlayer.Backpack
end

MainSection:NewButton("Equip Teleport Tool", "Click anywhere to teleport", function()
    CreateTeleportTool()
end)

-- Settings Features
SettingsSection:NewLabel("Credits")
SettingsSection:NewLabel("Script by: hamadlla")

SettingsSection:NewLabel("Customize Theme")
SettingsSection:NewColorPicker("Scheme Color", "Change the scheme color", colors.SchemeColor, function(color)
    colors.SchemeColor = color
end)

SettingsSection:NewColorPicker("Background Color", "Change the background color", colors.Background, function(color)
    colors.Background = color
end)

SettingsSection:NewColorPicker("Text Color", "Change the text color", colors.TextColor, function(color)
    colors.TextColor = color
end)

-- UI Toggle Button (Black Square)
local toggleButton = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
toggleButton.Name = "ToggleUIButton"

local buttonFrame = Instance.new("TextButton", toggleButton)
buttonFrame.Size = UDim2.new(0, 50, 0, 50)
buttonFrame.Position = UDim2.new(0, 10, 0, 10)
buttonFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
buttonFrame.Text = ""
buttonFrame.BorderSizePixel = 0

buttonFrame.MouseButton1Click:Connect(function()
    mainUIVisible = not mainUIVisible
    Library:ToggleUI()
end)

-- Clean-up function to destroy unused UI elements
game.Players.LocalPlayer.PlayerGui.ChildRemoved:Connect(function(child)
    if child.Name == "PartNamesUI" then
        child:Destroy()
    end
end)
