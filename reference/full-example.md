---
description: A complete script that uses every element.
---

# Full example

Copy this into your executor to see every element in action.

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Realestkai/uilib/refs/heads/main/sylex"))()

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local function getHumanoid()
    local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    return character:WaitForChild("Humanoid")
end

local Window = Library:Window({
    Title = "Midnight Hub",
    Desc = "Example script",
    Icon = "door-open",
    Theme = "Midnight",
    Config = {
        Keybind = Enum.KeyCode.LeftControl,
        Size = UDim2.new(0, 530, 0, 400),
    },
    CloseUIButton = {
        Enabled = true,
        Text = "Midnight",
    },
})

local Main = Window:Tab({ Title = "Main", Icon = "house" })
local Player = Window:Tab({ Title = "Player", Icon = "user" })
Window:Line()
local Misc = Window:Tab({ Title = "Misc", Icon = "settings" })

Window:SelectTab(1)

-- Main
Main:Section({ Title = "Welcome" })

Main:Label({
    Title = "Hello, " .. LocalPlayer.DisplayName,
    Desc = "Press Left Ctrl to hide or show this window.",
})

Main:Button({
    Title = "Send Notification",
    Callback = function()
        Window:Notify({ Title = "Midnight Hub", Desc = "This is a test notification.", Time = 5 })
    end,
})

Main:Toggle({
    Title = "Example Toggle",
    Value = false,
    Callback = function(Value)
        print("Toggle:", Value)
    end,
})

Main:Dropdown({
    Title = "Pick One",
    List = { "Option A", "Option B", "Option C" },
    Value = "Option A",
    Callback = function(Value)
        print("Picked:", Value)
    end,
})

Main:Dropdown({
    Title = "Pick Many",
    List = { "Red", "Green", "Blue" },
    Multi = true,
    Callback = function(Values)
        print("Picked:", table.concat(Values, ", "))
    end,
})

-- Player
Player:Section({ Title = "Movement" })

Player:Slider({
    Title = "WalkSpeed",
    Min = 16, Max = 100, Value = 16, Rounding = 0,
    Callback = function(Value)
        getHumanoid().WalkSpeed = Value
    end,
})

Player:Slider({
    Title = "JumpPower",
    Min = 50, Max = 200, Value = 50, Rounding = 0,
    Callback = function(Value)
        local humanoid = getHumanoid()
        humanoid.UseJumpPower = true
        humanoid.JumpPower = Value
    end,
})

-- Misc
Misc:Section({ Title = "Inputs" })

Misc:Textbox({
    Title = "Message",
    Placeholder = "Type something...",
    Callback = function(Text)
        print("Textbox:", Text)
    end,
})

Misc:Keybind({
    Title = "Example Keybind",
    Key = Enum.KeyCode.E,
    Callback = function(Key, Value)
        print("Keybind:", Key.Name, Value)
    end,
})

Misc:ColorPicker({
    Title = "Accent Color",
    Value = Color3.fromRGB(72, 104, 214),
    Callback = function(R, G, B)
        print("Color:", R, G, B)
    end,
})

Misc:Code({
    Title = "Loader",
    Code = 'loadstring(game:HttpGet("https://raw.githubusercontent.com/Realestkai/uilib/refs/heads/main/sylex"))()',
})
```
