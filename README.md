---
description: A clean, themeable UI library for Roblox scripts.
---

# Introduction

**Sylex UI** is a UI library for Roblox scripts. It gives you a draggable, resizable window with tabs, a full set of elements, notifications, dialogs, themes and config saving.

## What's included

| Feature | Details |
| --- | --- |
| Window | Draggable, resizable, maximise, hide, close-with-confirm; remembers its position and size |
| Tabs | Icon + title tabs, dividers, optional per-tab search, collapsible sections |
| Elements | Button, Toggle, Slider, Range Slider, Dropdown (single and multi), Player Dropdown, Textbox, Keybind, Color Picker, Label, Paragraph, Section, Divider, Progress Bar, Image, Code |
| Element API | Every element has `Get`, `Set`, `SetVisible`, `Destroy`, plus `Flag` and `Tooltip` options |
| Saving | Flags, save/load/delete configs, autoload, autosave |
| Feedback | Typed notifications (info, success, warning, error) and dialogs with any number of buttons and an optional input |
| Themes | Ten built in, add your own at runtime, change the accent colour |
| Extras | Ready-made settings tab, watermark (FPS and ping), keybind list overlay |
| Mobile | Floating open button and automatic scaling on small screens |
| Locking | `Lock` / `Unlock` any element, and `DependsOn` to show options only when a feature is on |
| Premium | `Premium = true` locks features for Freemium players; Premium is given automatically from the Discord role |
| Profile card | The player's avatar, display name and tier in the sidebar |
| Sharing | Players share settings as a config code |
| Debug mode | Prints flag changes, config activity and full error tracebacks |
| Clean unload | `Library:Unload()` disconnects everything and removes the UI |

## Get started

New here? Start with the **step-by-step guides**, then copy the **full example** as a starting point.

{% content-ref url="guides/how-to.md" %}
[how-to.md](guides/how-to.md)
{% endcontent-ref %}

{% content-ref url="reference/full-example.md" %}
[full-example.md](reference/full-example.md)
{% endcontent-ref %}

{% content-ref url="getting-started/loading-the-library.md" %}
[loading-the-library.md](getting-started/loading-the-library.md)
{% endcontent-ref %}

{% content-ref url="getting-started/windows.md" %}
[windows.md](getting-started/windows.md)
{% endcontent-ref %}

## Quick look

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Realestkai/uilib/refs/heads/main/sylex.lua"))()

local Window = Library:Window({
    Title = "Sylex",
    Theme = "Midnight",
    PremiumCheck = "https://sylex-premium.sylex.workers.dev/tier",
    Config = { Keybind = Enum.KeyCode.LeftControl },
})

local Main = Window:Tab({ Title = "Main", Icon = "house" })

Main:Toggle({
    Title = "Hello",
    Flag = "Hello",
    Callback = function(Value)
        print("Toggle is now", Value)
    end,
})

Main:Toggle({ Title = "Premium feature", Flag = "PremiumFeature", Premium = true })

Window:SettingsTab()
Library:LoadConfig()
```
