local OrionLib = loadstring(game:HttpGet(
                                ('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
				
local Window = OrionLib:MakeWindow({
    Name = [[<font color="rgb(30,0,255)">Luck Scripts</font>]],
    HidePremium = false,
    SaveConfig = true,
    ConfigFolder = "OrionTest"
})

local Tab1 = Window:MakeTab({Name = "Menu", Icon = "", PremiumOnly = false})
local Tab3 = Window:MakeTab({Name = "Esp", Icon = "", PremiumOnly = false})
local Tab2 = Window:MakeTab({Name = "Velocidade", Icon = "", PremiumOnly = false})
local Expander = false

local HB_Settings = {
    Color = Color3.new(1, 0, 0), -- Default to red if not set
    Transparency = 0.5,
    Size = 2
}

local Player = game.Players.LocalPlayer

-- Function to add hitbox changes
function addHitBoxChanges()
    while Expander do
        wait(0.1)
        for i, plr in pairs(game.Players:GetPlayers()) do
            if plr ~= Player and plr.Team ~= Player.Team then
                local character = plr.Character
                if character then
                    local hrp = character:FindFirstChild("HumanoidRootPart")
                    if hrp then
                        hrp.Size = Vector3.new(HB_Settings.Size, HB_Settings.Size, HB_Settings.Size)
                        hrp.CanCollide = false
                        hrp.Transparency = HB_Settings.Transparency
                        hrp.Color = HB_Settings.Color
                    end
                end
            end
        end
    end

    -- Revert changes when Expander is false
    for i, plr in pairs(game.Players:GetPlayers()) do
        if plr ~= Player and plr.Team ~= Player.Team then
            local character = plr.Character
            if character then
                local hrp = character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    hrp.Size = Vector3.new(2, 2, 2)
                    hrp.CanCollide = false
                    hrp.Transparency = 1
                    hrp.Color = Color3.new(1, 1, 1)
                end
            end
        end
    end
end

-- Function to teleport enemy to click position
local function teleportToClick()
    local mouse = Player:GetMouse()
    mouse.Button1Down:Connect(function()
        if AimBotEnabled then
            local targetPosition = mouse.Hit.p
            for i, plr in pairs(game.Players:GetPlayers()) do
                if plr ~= Player and plr.Team ~= Player.Team then
                    local character = plr.Character
                    if character then
                        local hrp = character:FindFirstChild("HumanoidRootPart")
                        if hrp then
                            hrp.CFrame = CFrame.new(targetPosition)
                        end
                    end
                end
            end
        end
    end)
end

-- GUI Elements
Tab1:AddToggle({
    Name = "HitBox",
    Default = false,
    Callback = function(v)
        Expander = v
        if Expander then
            addHitBoxChanges()
        end
    end
})

Tab1:AddToggle({
    Name = "AimBot",
    Default = false,
    Callback = function(v)
        AimBotEnabled = v
        if AimBotEnabled then
            HB_Settings.Size = 9999999
            HB_Settings.Transparency = 1
            addHitBoxChanges()
            teleportToClick()
        else
            HB_Settings.Size = 2
            HB_Settings.Transparency = 0.5
        end
    end
})

Tab1:AddSection({
    Name = "Configurações da HitBox"
})

Tab1:AddColorpicker({
	Name = "Cor",
	Default = Color3.fromRGB(255, 0, 0),
	Callback = function(Value)
		HB_Settings.Color = Value
        print(Value)
	end	  
})

Tab1:AddSlider({
    Name = "Tamanho",
    Min = 2,
    Max = 50,
    Default = 2,
    Color = Color3.fromRGB(30,0,255),
    Increment = 1,
    ValueName = "Size",
    Callback = function(Value)
        HB_Settings.Size = Value
    end
})

Tab1:AddSlider({
    Name = "Transparência",
    Min = 0,
    Max = 1,
    Default = .5,
    Color = Color3.fromRGB(30,0,255),
    Increment = .1,
    ValueName = "Transparency",
    Callback = function(Value)
        HB_Settings.Transparency = Value
    end
})

Tab2:AddSlider({
    Name = "WalkSpeed",
    Min = 0,
    Max = 50,
    Default = 16,
    Color = Color3.fromRGB(30,0,255),
    Increment = 1,
    ValueName = "WalkSpeed",
    Callback = function(Value)
        Player.Character.Humanoid.WalkSpeed = Value
    end
})

Tab3:AddButton({
    Name = "Skeleton ESP",
    Callback = function(v)
        loadstring(game:HttpGet("https://raw.githubusercontent.com/Blissful4992/ESPs/main/SkeletonESP.lua"))()
    end
})
