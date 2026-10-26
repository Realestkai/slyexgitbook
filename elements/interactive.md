---
description: Buttons, toggles, sliders, dropdowns, textboxes and color pickers.
---

# Interactive elements

Every element takes `Title` and, optionally, `Desc` (a smaller line underneath) and `Image` (an icon on the left). `Title` and `Desc` support [RichText](https://create.roblox.com/docs/ui/rich-text).

Most elements return an object you can update later. All of them have `SetTitle`, `SetDesc` and `SetVisible`.

```lua
Element:SetTitle("New title")
Element:SetDesc("New description")
Element:SetVisible(false)
```

***

## Button

```lua
Tab:Button({
    Title = "Button Example",
    Desc = "Runs once per click",
    Image = "",
    Callback = function()
        -- runs when the button is pressed
    end,
})
```

| Option | Type | Default |
| --- | --- | --- |
| `Title` | string | `"null"` |
| `Desc` | string | `""` |
| `Image` | string / number | `""` |
| `Callback` | `function()` | — |

{% hint style="warning" %}
`Button` doesn't return an object, so you can't change its title after creating it.
{% endhint %}

***

## Toggle

```lua
local Toggle = Tab:Toggle({
    Title = "Toggle Example",
    Desc = "",
    Value = false,
    Callback = function(Value)
        -- Value is true or false
    end,
})
```

| Option | Type | Default |
| --- | --- | --- |
| `Title` | string | `"null"` |
| `Desc` | string | `""` |
| `Image` | string / number | `""` |
| `Value` | boolean | `false` |
| `Callback` | `function(Value: boolean)` | — |

The callback runs once right after the toggle is created, with its starting value.

### Update a toggle

```lua
Toggle:SetValue(true) -- also runs the callback
```

***

## Slider

```lua
local Slider = Tab:Slider({
    Title = "Slider Example",
    Min = 0,
    Max = 100,
    Value = 50,
    Rounding = 0,
    Callback = function(Value)
        -- Value is a number between Min and Max
    end,
})
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Title` | string | `"null"` | |
| `Desc` | string | `""` | |
| `Image` | string / number | `""` | |
| `Min` | number | `0` | Lowest value. |
| `Max` | number | `100` | Highest value. |
| `Value` | number | `Min + 1` | Starting value. |
| `Rounding` | number | `2` | Decimal places. `0` gives whole numbers. |
| `Callback` | `function(Value: number)` | — | Runs on every change, while dragging too. |

Players can drag the bar or type a number into the box next to it.

### Update a slider

```lua
Slider:SetValue(75)  -- clamped to Min/Max, runs the callback
Slider:SetMin(10)
Slider:SetMax(200)
```

***

## Dropdown

```lua
local Dropdown = Tab:Dropdown({
    Title = "Dropdown Example",
    List = { "Option 1", "Option 2", "Option 3" },
    Value = "Option 1",
    Multi = false,
    Callback = function(Value)
        -- single: Value is a string
        -- multi:  Value is a table of strings
    end,
})
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Title` | string | `"null"` | |
| `Desc` | string | `""` | |
| `Image` | string / number | `""` | |
| `List` | `{string}` | `{}` | The options. |
| `Value` | string, or `{string}` when `Multi` | `List[1]` | What's selected at the start. |
| `Multi` | boolean | `false` | Allow more than one selection. |
| `Callback` | function | — | See below. |

* **Single:** `Callback(Value: string)`
* **Multi:** `Callback(Values: {string})`, for example `{ "Red", "Blue" }`

The list has a built-in search box.

### Multi-select example

```lua
Tab:Dropdown({
    Title = "Teams",
    List = { "Red", "Green", "Blue" },
    Value = { "Red", "Blue" },
    Multi = true,
    Callback = function(Values)
        print(table.concat(Values, ", "))
    end,
})
```

### Change the options

```lua
Dropdown:Add("Option 4")                 -- add one option
Dropdown:Clear("Option 2")               -- remove one option
Dropdown:Clear({ "Option 1", "Option 3" }) -- remove several
Dropdown:Clear()                         -- remove everything
```

Refresh a list by clearing it and adding again:

```lua
Dropdown:Clear()
for _, player in ipairs(game.Players:GetPlayers()) do
    Dropdown:Add(player.Name)
end
```

### Set the selection

```lua
Dropdown:SetValue("Option 2") -- runs the callback
```

{% hint style="warning" %}
On a **multi** dropdown, `SetValue` selects just that one option, and its callback receives `{ ["Option 2"] = true }` instead of a list. See [Known quirks](../reference/known-quirks.md).
{% endhint %}

***

## Textbox

```lua
local Textbox = Tab:Textbox({
    Title = "Textbox Example",
    Placeholder = "Type here...",
    Value = "",
    ClearText = false,
    Callback = function(Text)
        -- Text is the string the player typed
    end,
})
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Title` | string | — | |
| `Desc` | string | `""` | |
| `Image` | string / number | `""` | |
| `Value` | string | `""` | Starting text. |
| `Placeholder` | string | `"Paste Your Text"` | Grey hint text. |
| `ClearText` | boolean | `false` | Despite the name, `true` **keeps** the text when the player clicks the box, and `false` clears it. `ClearTextOnFocus` is an alias. |
| `Callback` | `function(Text: string)` | — | Runs when the box loses focus, but only if it isn't empty. |

### Update a textbox

```lua
Textbox:SetValue("Hello")          -- doesn't run the callback
Textbox:SetPlaceholderText("Name...")
Textbox:SetClearTextOnFocus(true)  -- same inverted meaning as ClearText
```

***

## Color picker

```lua
local ColorPicker = Tab:ColorPicker({
    Title = "Color Picker",
    Value = Color3.fromRGB(72, 104, 214),
    Callback = function(R, G, B)
        local color = Color3.fromRGB(R, G, B)
    end,
})
```

| Option | Type | Default |
| --- | --- | --- |
| `Title` | string | — |
| `Desc` | string | `""` |
| `Image` | string / number | `""` |
| `Value` | `Color3` | white |
| `Callback` | `function(R, G, B)` | — |

{% hint style="info" %}
The callback gets **three numbers from 0 to 255**, not a `Color3`. Use `Color3.fromRGB(R, G, B)` to build one.
{% endhint %}

Clicking the swatch opens a popup with a saturation/value square, a hue bar, and Hex and R, G, B boxes.

### Update a color picker

```lua
ColorPicker:SetValue({ R = 255, G = 0, B = 0 })
```

`SetValue` takes a table of 0–255 values. Missing keys keep their current value.
