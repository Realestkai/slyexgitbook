---
description: A complete script that uses every feature.
---

# Full example

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Realestkai/uilib/refs/heads/main/sylex.lua"))()

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local function getHumanoid()
    local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    return character:WaitForChild("Humanoid")
end

local Window = Library:Window({
    Title = "Sylex Hub",
    Desc = "Example script",
    Theme = "Midnight",
    Search = true,
    Config = {
        Keybind = Enum.KeyCode.LeftAlt,
        Size = UDim2.new(0, 550, 0, 440),
    },
    CloseUIButton = {
        Enabled = true,
    },
    ConfigSaving = {
        Folder = "SylexHub",
        AutoSave = true,
    },
})

local Main = Window:Tab({ Title = "Main", Icon = "house" })
local Player = Window:Tab({ Title = "Player", Icon = "user" })
Window:Line()
local Misc = Window:Tab({ Title = "Misc", Icon = "settings" })

-- Main
Main:Section({ Title = "Welcome" })

Main:Paragraph({
    Title = "Hello, " .. LocalPlayer.DisplayName,
    Content = "Press Left Alt to hide or show this window.\nYour settings save automatically.",
})

local farming = false
Main:Toggle({
    Title = "Auto Farm",
    Flag = "AutoFarm",
    Tooltip = "Farms while you're AFK",
    Callback = function(Value)
        -- the callback also runs once on load; only notify on a real change
        if Value == farming then return end
        farming = Value
        Window:Notify({
            Title = "Auto Farm",
            Desc = Value and "Started." or "Stopped.",
            Type = Value and "Success" or "Info",
            Time = 3,
        })
    end,
})

local Progress = Main:ProgressBar({ Title = "Session progress", Max = 10, Suffix = "runs" })

Main:Dropdown({
    Title = "Mode",
    List = { "Easy", "Normal", "Hard" },
    Value = "Normal",
    Flag = "Mode",
    Callback = function(Value)
        print("Mode:", Value)
    end,
})

Main:PlayerDropdown({
    Title = "Target",
    IncludeSelf = false,
    Flag = "Target",
    Callback = function(Name)
        print("Target:", Name)
    end,
})

-- Player
Player:Section({ Title = "Movement" })

Player:Slider({
    Title = "WalkSpeed",
    Min = 16, Max = 100, Value = 16, Rounding = 0, Suffix = "studs",
    Flag = "WalkSpeed",
    Callback = function(Value)
        getHumanoid().WalkSpeed = Value
    end,
})

Player:Keybind({
    Title = "Sprint",
    Key = Enum.KeyCode.Q,
    Flag = "Sprint",
    Callback = function(Key, Value)
        getHumanoid().WalkSpeed = Value and 32 or Library.Flags.WalkSpeed
    end,
})

Player:Section({ Title = "Advanced", Collapsed = true })

Player:RangeSlider({
    Title = "Random delay",
    Min = 0, Max = 10, Value = { 1, 3 }, Rounding = 1,
    Flag = "Delay",
    Callback = function(Low, High)
        print("Delay between", Low, "and", High)
    end,
})

-- Misc
Misc:Textbox({
    Title = "Message",
    Placeholder = "Type something...",
    Flag = "Message",
    Callback = function(Text)
        print("Textbox:", Text)
    end,
})

Misc:ColorPicker({
    Title = "ESP Color",
    Value = Color3.fromRGB(72, 104, 214),
    Flag = "ESPColor",
    Callback = function(R, G, B)
        print("Color:", R, G, B)
    end,
})

Misc:Divider()

Misc:Code({
    Title = "Loader",
    Code = 'loadstring(game:HttpGet("https://raw.githubusercontent.com/Realestkai/uilib/refs/heads/main/sylex.lua"))()',
})

-- Settings tab: theme, accent, menu key, overlays, configs, unload
Window:SettingsTab()

-- Stop our loops when the UI is unloaded
task.spawn(function()
    while not Library.Unloaded do
        if Library.Flags.AutoFarm then
            Progress:Increment()
        end
        task.wait(2)
    end
end)

Library:OnUnload(function()
    getHumanoid().WalkSpeed = 16
end)

-- Load saved settings last, after every element exists
Library:LoadConfig()

Window:Notify({
    Title = '<font color="#4868D6">Sylex</font>',
    Desc = "Press <b>Left Alt</b> to hide the menu.",
    Time = 8,
})
```
