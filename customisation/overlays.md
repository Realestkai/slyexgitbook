---
description: Small always-on-screen panels that sit outside the main window.
---

# Overlays

Overlays stay on screen while the window is hidden. Players can drag them anywhere, and they follow the theme.

## Watermark

Shows your title, FPS and ping.

```lua
local Watermark = Window:Watermark({
    Text = "Sylex | Roville",
    FPS = true,
    Ping = true,
    Position = UDim2.new(0, 10, 0, 10),
})

Watermark:SetText("Sylex | v2")
Watermark:SetVisible(false)
Watermark:Destroy()
```

| Option | Type | Default |
| --- | --- | --- |
| `Text` | string | window title |
| `FPS` | boolean | `true` |
| `Ping` | boolean | `true` |
| `Position` | `UDim2` | top-left |

## Keybind list

Lists the keybinds that are currently switched on, with their keys.

```lua
local List = Window:KeybindList({
    Title = "Keybinds",
    ShowAll = false,
    Position = UDim2.new(0, 10, 0.35, 0),
})

List:SetVisible(true)
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Title` | string | `"Keybinds"` | Header text. |
| `ShowAll` | boolean | `false` | List every keybind. Ones that are switched off are dimmed. |
| `Position` | `UDim2` | left side | Starting position. |

The list updates by itself whenever a keybind is toggled, rebound or destroyed.

{% hint style="info" %}
The [settings tab](settings-tab.md) has toggles for both overlays, so you don't need to create them yourself unless you want custom options.
{% endhint %}
