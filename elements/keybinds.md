---
description: A toggle that's flipped with a rebindable key.
---

# Keybinds

A keybind is a toggle with a key box next to it. Pressing the key, or clicking the row, flips it on and off. Clicking the key box lets the player choose a new key.

```lua
local Keybind = Tab:Keybind({
    Title = "Keybind Example",
    Desc = "Click the key to rebind",
    Key = Enum.KeyCode.E,
    Value = false,
    Callback = function(Key, Value)
        -- Key   is the current Enum.KeyCode
        -- Value is true or false
    end,
})
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Title` | string | `"null"` | |
| `Desc` | string | `""` | |
| `Image` | string / number | `""` | |
| `Key` | `Enum.KeyCode` | `E` | Starting key. |
| `Value` | boolean | `false` | Starting on/off state. |
| `Callback` | `function(Key, Value)` | — | See below. |

## When the callback runs

| Event | Runs? |
| --- | --- |
| Element created | Yes, once |
| Player presses the key | Yes, after flipping the value |
| Player picks a new key | Yes, with the new key |
| `SetKey` is called | Yes |
| Player **clicks** the row | **No.** The value flips but the callback doesn't run. |

{% hint style="warning" %}
The key also fires while the player is typing in chat or a textbox. If that matters, check `UserInputService:GetFocusedTextBox()` inside your callback.
{% endhint %}

## Update a keybind

```lua
Keybind:SetKey(Enum.KeyCode.F)  -- runs the callback
Keybind:SetValue(true)          -- doesn't run the callback
```
