---
description: Rebindable keys, either as an on/off switch or a plain trigger.
---

# Keybinds

A keybind row has a key box on the right. Clicking the key box lets the player press a new key, and **Escape** cancels.

## Toggle mode (default)

The key flips the row on and off, like a toggle.

```lua
local Fly = Tab:Keybind({
    Title = "Fly",
    Key = Enum.KeyCode.F,
    Value = false,
    Flag = "Fly",
    Callback = function(Key, Value)
        -- Value is true or false
    end,
})
```

## Press mode

There's no switch: the callback just runs on every press. This is useful for one-off actions.

```lua
Tab:Keybind({
    Title = "Teleport to mouse",
    Key = Enum.KeyCode.T,
    Mode = "Press",
    Callback = function(Key)
        -- runs every time T is pressed
    end,
})
```

## Options

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Key` | `Enum.KeyCode` | `E` | Starting key. |
| `Value` | boolean | `false` | Starting on/off state (toggle mode). |
| `Mode` | `"Toggle"` / `"Press"` | `"Toggle"` | See above. |
| `Callback` | `function(Key, Value)` | — | Runs on key press, row click, rebind and `Set`. |

Key presses are ignored while the player is typing in chat or any textbox.

## Methods

```lua
Fly:SetKey(Enum.KeyCode.G)          -- runs the callback
Fly:Set(true)                       -- runs the callback
Fly:Get()                           -- { Key = Enum.KeyCode.G, Value = true }
```

Keybinds show up in the [keybind list overlay](../customisation/overlays.md#keybind-list).
