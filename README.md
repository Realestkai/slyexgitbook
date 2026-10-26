---
description: A clean, themeable UI library for Roblox scripts.
---

# Introduction

**Dummy Kawaii** is a UI library for Roblox. It gives you a draggable, resizable window with tabs and a full set of elements: buttons, toggles, sliders, dropdowns, textboxes, keybinds, color pickers, code blocks, notifications and dialogs.

It ships with two themes, **Dark** and **Midnight**, and players can switch between them live from the dropdown in the window's top bar.

## What's included

| Feature | Details |
| --- | --- |
| Window | Draggable top bar, resize handle, maximise, hide, close-with-confirm |
| Tabs | Icon + title tabs in a scrolling sidebar, with dividers |
| Elements | Button, Toggle, Slider, Dropdown (single and multi), Textbox, Keybind, Color Picker, Label, Section, Code |
| Feedback | Toast notifications and two-button dialogs |
| Themes | Dark and Midnight (midnight-blue-to-black gradient), switchable at runtime |
| Mobile | A floating "open" button for touch players, toggleable per script |

## Get started

{% content-ref url="getting-started/loading-the-library.md" %}
[loading-the-library.md](getting-started/loading-the-library.md)
{% endcontent-ref %}

{% content-ref url="getting-started/windows.md" %}
[windows.md](getting-started/windows.md)
{% endcontent-ref %}

## Quick look

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/YOUR_NAME/YOUR_REPO/main/DummyKawaii_Midnight.lua"))()

local Window = Library:Window({
    Title = "My Hub",
    Theme = "Midnight",
    Config = { Keybind = Enum.KeyCode.LeftControl },
    CloseUIButton = { Enabled = true, Text = "Open" },
})

local Main = Window:Tab({ Title = "Main", Icon = "house" })

Main:Toggle({
    Title = "Hello",
    Callback = function(Value)
        print("Toggle is now", Value)
    end,
})
```
