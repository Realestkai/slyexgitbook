---
description: A complete script that uses every feature, ready to copy as a starting point.
---

# Full example

Copy this as the starting point for a new script. It shows:

* a window with Premium from the Discord role, config saving and the mobile open button
* free features, options that only appear when a feature is on (`DependsOn`), and Premium-only features
* a locked "coming soon" element
* every element type, dialogs and notifications
* the settings tab
* loops that stop cleanly when the UI is unloaded

Each part is explained in the [step-by-step guides](../guides/how-to.md).

```lua
--// Sylex UI: full example script
--// Copy this as a starting point. Every section is explained in the docs.

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Realestkai/uilib/refs/heads/main/sylex.lua"))()

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local function getHumanoid()
    local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    return character:WaitForChild("Humanoid")
end

--// 1. Window
local Window = Library:Window({
    Title = "Sylex",
    Desc = "Example script",
    Theme = "Midnight",
    Search = true,                     -- search box at the top of every tab

    -- Premium from the Discord role (players link once with /link)
    PremiumCheck = "https://sylex-premium.sylex.workers.dev/tier",

    Config = {
        Keybind = Enum.KeyCode.LeftAlt, -- hides / shows the window
        Size = UDim2.new(0, 550, 0, 440),
    },
    CloseUIButton = {
        Enabled = true,                -- floating logo button for mobile players
    },
    ConfigSaving = {
        Folder = "SylexExample",       -- one folder per script
        AutoSave = true,
    },

    -- Debug = true,                   -- turn on while developing, off before shipping
})

--// 2. Tabs
local Main = Window:Tab({ Title = "Main", Icon = "house" })
local Player = Window:Tab({ Title = "Player", Icon = "user" })
Window:Line()
local Misc = Window:Tab({ Title = "Misc", Icon = "settings" })

--// 3. Main tab: a free feature, its options, and Premium features
Main:Section({ Title = "Farming" })

Main:Paragraph({
    Title = "Hello, " .. LocalPlayer.DisplayName,
    Content = "Press Left Alt to hide or show this window.\nYour settings save automatically.",
})

Main:Toggle({
    Title = "Auto Farm",
    Flag = "AutoFarm",
    Tooltip = "Farms while you're AFK",
    Callback = function(Value)
        print("Auto Farm:", Value)
    end,
})

-- Only shown while Auto Farm is on
Main:Slider({
    Title = "Farm speed",
    Min = 1, Max = 10, Value = 5, Rounding = 0,
    Flag = "FarmSpeed",
    DependsOn = "AutoFarm",
})

-- Shown only for one dropdown value
Main:Dropdown({
    Title = "Mode",
    List = { "Easy", "Normal", "Hard" },
    Value = "Normal",
    Flag = "Mode",
})
Main:Toggle({
    Title = "Hard-mode helper",
    Flag = "HardHelper",
    DependsOn = { Flag = "Mode", Value = "Hard" },
})

Main:Section({ Title = "Premium" })

-- Locked with a gold "Premium only" for Freemium players
Main:Toggle({
    Title = "Auto Collect",
    Flag = "AutoCollect",
    Premium = true,
    Callback = function(Value)
        -- also runs with false if the player's Premium ends while the script is open
        print("Auto Collect:", Value)
    end,
})

Main:Button({
    Title = "Collect Everything",
    Premium = true,
    PremiumReason = "Premium: one-click collect",
    Callback = function()
        Window:Notify({ Title = "Collected!", Type = "Success", Time = 3 })
    end,
})

-- Locked from code until you unlock it
local Event = Main:Button({
    Title = "Event Teleport",
    Locked = true,
    LockReason = "Coming soon",
})
-- later: Event:Unlock()

local Progress = Main:ProgressBar({ Title = "Session progress", Max = 10, Suffix = "runs" })

--// 4. Player tab
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

Player:PlayerDropdown({
    Title = "Target",
    IncludeSelf = false,
    Flag = "Target",
})

--// 5. Misc tab
Misc:Textbox({
    Title = "Message",
    Placeholder = "Type something...",
    Flag = "Message",
})

Misc:ColorPicker({
    Title = "ESP Color",
    Value = Color3.fromRGB(72, 104, 214),
    Flag = "ESPColor",
})

Misc:Divider()

Misc:Button({
    Title = "Ask first",
    Callback = function()
        Window:Dialog({
            Title = "Reset WalkSpeed?",
            Buttons = {
                { Title = "Reset", Color = Color3.fromRGB(226, 39, 6), Callback = function()
                    Library.Options.WalkSpeed:Set(16)
                end },
                { Title = "Cancel" },
            },
        })
    end,
})

--// 6. Settings tab: theme, accent, menu key, configs, sharing, Link Discord, unload
Window:SettingsTab()

--// 7. Your loops: always stop when the UI is unloaded
task.spawn(function()
    while not Library.Unloaded do
        if Library.Flags.AutoFarm then
            Progress:Increment()
        end
        task.wait(2)
    end
end)

Library:OnPremiumChanged(function(isPremium)
    print("Tier:", isPremium and "Premium" or "Freemium")
end)

Library:OnUnload(function()
    getHumanoid().WalkSpeed = 16
end)

--// 8. Load saved settings last, after every element exists
Library:LoadConfig()
```
