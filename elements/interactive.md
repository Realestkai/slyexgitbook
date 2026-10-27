---
description: Buttons, toggles, sliders, dropdowns, textboxes and color pickers.
---

# Interactive elements

All elements also take the [shared options](shared.md) (`Flag`, `Tooltip`, `Desc`, `Image`) and have the shared methods (`Get`, `Set`, `SetVisible`, `Destroy`, `SetTitle`, `SetDesc`).

***

## Button

```lua
local Button = Tab:Button({
    Title = "Teleport to spawn",
    Desc = "Runs once per click",
    Callback = function()
        -- runs when pressed
    end,
})
```

```lua
Button:SetCallback(function() print("new action") end)
Button:Fire()          -- run the callback from code
```

***

## Toggle

```lua
local Toggle = Tab:Toggle({
    Title = "Auto Farm",
    Value = false,
    Flag = "AutoFarm",
    Callback = function(Value)
        -- Value is true or false
    end,
})

Toggle:Set(true)
print(Toggle:Get())
```

***

## Slider

```lua
local Slider = Tab:Slider({
    Title = "WalkSpeed",
    Min = 16,
    Max = 100,
    Value = 16,
    Rounding = 0,
    Suffix = "studs",
    Flag = "WalkSpeed",
    Callback = function(Value)
        -- Value is a number between Min and Max
    end,
})
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Min` | number | `0` | Lowest value. |
| `Max` | number | `100` | Highest value. |
| `Value` | number | `Min` | Starting value. |
| `Rounding` | number | `2` | Decimal places. `0` gives whole numbers. |
| `Suffix` | string | none | Shown after the number, e.g. `"studs"`. |
| `Callback` | `function(Value)` | — | Runs whenever the value changes. Dragging only fires it when the number actually changes. |

Players can drag the bar or type a number into the box.

```lua
Slider:Set(75)       -- clamped to Min/Max
Slider:SetMin(10)
Slider:SetMax(200)
```

***

## Range slider

Two handles that pick a minimum and a maximum.

```lua
local Range = Tab:RangeSlider({
    Title = "Price range",
    Min = 0,
    Max = 1000,
    Value = { 100, 500 },
    Rounding = 0,
    Flag = "PriceRange",
    Callback = function(Low, High)
        print(Low, High)
    end,
})

Range:Set({ 200, 800 })
local low, high = table.unpack(Range:Get())
```

Clicking the bar grabs whichever handle is closer.

***

## Dropdown

```lua
local Dropdown = Tab:Dropdown({
    Title = "Mode",
    List = { "Easy", "Normal", "Hard" },
    Value = "Normal",
    Flag = "Mode",
    Callback = function(Value)
        print(Value) -- "Normal"
    end,
})
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `List` | `{string}` | `{}` | The options. |
| `Value` | string, or `{string}` when multi | first option (single), nothing (multi) | Starting selection. |
| `Multi` | boolean | `false` | Allow more than one selection. |
| `Callback` | function | — | Single: `Callback(string)`. Multi: `Callback({string})`. |

The open list has a search box.

### Multi-select

```lua
local Teams = Tab:Dropdown({
    Title = "Teams",
    List = { "Red", "Green", "Blue" },
    Value = { "Red", "Blue" },
    Multi = true,
    Callback = function(Values)
        print(table.concat(Values, ", ")) -- always a list, in the order picked
    end,
})

Teams:Set({ "Green" })
```

### Changing the options

```lua
Dropdown:Refresh({ "A", "B", "C" })    -- replace the list, keeping any selection that still exists
Dropdown:Add("D")                      -- add one
Dropdown:Clear("A")                    -- remove one
Dropdown:Clear({ "B", "C" })           -- remove several
Dropdown:Clear()                       -- remove all
Dropdown:GetOptions()                  -- { "D", ... }
```

`Refresh` only runs the callback if a selected option was removed. `Add` and `Clear` never run it.

***

## Player dropdown

A dropdown of player names that updates itself as players join and leave.

```lua
local Target = Tab:PlayerDropdown({
    Title = "Target",
    IncludeSelf = false,
    Flag = "Target",
    Callback = function(Name)
        print("Targeting", Name)
    end,
})
```

It takes all the Dropdown options except `List`, plus `IncludeSelf` (default `true`). By default it selects you, or the first player in the list when `IncludeSelf` is false. Pass `Multi = true` to pick several players.

***

## Textbox

```lua
local Textbox = Tab:Textbox({
    Title = "Message",
    Placeholder = "Type here...",
    Value = "",
    ClearText = false,
    Flag = "Message",
    Callback = function(Text)
        print(Text)
    end,
})
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Value` | string | `""` | Starting text. |
| `Placeholder` | string | `"Paste Your Text"` | Grey hint text. |
| `ClearText` | boolean | `false` | `true` clears the box each time it's clicked. `ClearTextOnFocus` is an alias. |
| `Numeric` | boolean | `false` | Only accept numbers. The callback receives a `number`. |
| `Callback` | `function(Text)` | — | Runs when the box loses focus, if it isn't empty. |

```lua
Textbox:Set("Hello")                 -- runs the callback
Textbox:SetPlaceholderText("Name...")
Textbox:SetClearTextOnFocus(true)
```

***

## Color picker

```lua
local ColorPicker = Tab:ColorPicker({
    Title = "ESP Color",
    Value = Color3.fromRGB(72, 104, 214),
    Flag = "ESPColor",
    Callback = function(R, G, B)
        local color = Color3.fromRGB(R, G, B)
    end,
})
```

{% hint style="info" %}
The callback gets **three numbers from 0 to 255**. `ColorPicker:Get()` and `Library.Flags` give you a `Color3`.
{% endhint %}

Clicking the swatch opens a popup with a saturation/value square, a hue bar, and Hex and R, G, B boxes.

```lua
ColorPicker:Set(Color3.fromRGB(255, 0, 0))
ColorPicker:Set("#FF0000")
ColorPicker:Set({ R = 255, G = 0, B = 0 })   -- missing keys keep their value
```
