---
description: A complete settings page in one line.
---

# Settings tab

```lua
Window:SettingsTab()
```

Call it after your other tabs so it sits at the bottom of the sidebar. It creates a tab with:

| Section | Contents |
| --- | --- |
| Interface | Theme dropdown, accent colour picker, menu keybind |
| Overlays | Watermark toggle, keybind list toggle |
| Configuration | Config name box, saved-configs dropdown, Save / Load / Set as Autoload / Delete buttons |
| Premium | Link Discord, Check Premium Again (only with `PremiumCheck`) |
| Share | Copy Config Code, Import Config Code (see [Sharing configs](../state/flags-and-configs.md#sharing-configs)) |
| Other | Unload UI (asks first) |

The Configuration section only appears when the executor supports file functions.

## Options

```lua
Window:SettingsTab({
    Title = "Settings",
    Icon = "settings",
    Overlays = true,
})
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Title` | string | `"Settings"` | Tab name. |
| `Icon` | string / number | `"settings"` | Tab icon. |
| `Overlays` | boolean | `true` | Include the Overlays section. |

`SettingsTab` returns the tab object, so you can add your own elements to it:

```lua
local Settings = Window:SettingsTab()
Settings:Section({ Title = "Credits" })
Settings:Label({ Title = "Made by Sylex" })
```

## Saved settings

The settings tab's own elements have flags, so they're saved in configs alongside yours: `SylexTheme`, `SylexAccent`, `SylexMenuKey`, `SylexWatermark` and `SylexKeybindList`. Call `Library:LoadConfig()` at the end of your script to restore them.
