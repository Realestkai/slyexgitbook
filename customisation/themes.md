---
description: Built-in themes, adding your own, and changing the accent colour.
---

# Themes

## Built-in themes

| Theme | Look |
| --- | --- |
| `Midnight` | Flat dark midnight blue with a soft blue accent. |
| `Dark` | Neutral greys with a steel-blue accent. The default. |
| `Onyx` | Near-black with a light grey accent. |
| `Slate` | Cool blue-grey with a muted blue accent. |
| `Ocean` | Deep teal-blue with a cyan accent. |
| `Emerald` | Dark green with a green accent. |
| `Crimson` | Dark red with a red accent. |
| `Rose` | Dark plum with a pink accent. |
| `Amber` | Dark brown-orange with an amber accent. |
| `Mocha` | Warm dark brown with a tan accent. |

Pick one when you create the window, or switch at any time:

```lua
Library:Window({ Theme = "Midnight" })

Window:SetTheme("Dark")
Window:GetTheme()          -- "Dark"
Library:GetThemes()        -- { "Midnight", "Dark", "Onyx", ... }
```

Players switch themes from the [settings tab](settings-tab.md). Every element recolours instantly.

## Accent colour

```lua
Window:SetAccent(Color3.fromRGB(255, 90, 140))
```

This recolours the selected-tab marker, scrollbars, slider fills, and toggles and keybinds that are switched on.

## Adding a theme

Add themes at runtime with `Library:AddTheme`. You only need to give the keys you want to change: anything you leave out is copied from `Dark`.

```lua
Library:AddTheme("Wine", {
    ['Shadow'] = Color3.fromRGB(10, 4, 6),
    ['Background'] = Color3.fromRGB(40, 12, 18),
    ['Background Gradient'] = ColorSequence.new(Color3.fromRGB(70, 16, 26), Color3.fromRGB(10, 6, 8)),
    ['Page'] = Color3.fromRGB(18, 8, 10),
    ['Main'] = Color3.fromRGB(220, 60, 80),
    ['Function'] = {
        ['Toggle'] = { ['True'] = { ['Toggle Value'] = Color3.fromRGB(220, 60, 80) } },
        ['Slider'] = { ['Slider Bar Value'] = Color3.fromRGB(220, 60, 80) },
    },
})

Window:SetTheme("Wine")
```

The new theme also shows up in the settings tab's theme dropdown.

## Theme keys

### Top level

| Key | Type | Used for |
| --- | --- | --- |
| `Shadow` | `Color3` | Drop shadow around the window, notifications and the open button |
| `Background` | `Color3` | Notifications, overlays, the open button |
| `Background Gradient` | `ColorSequence` | Window background, top to bottom |
| `Page` | `Color3` | The content panel behind elements, tooltips |
| `Main` | `Color3` | Accent: selected-tab marker, scrollbars, keybind knob |
| `Text & Icon` | `Color3` | All text and icons |

For a flat background, use a single-colour sequence: `ColorSequence.new(Color3.fromRGB(20, 20, 20))`.

### `Function` (per element)

| Element | Keys |
| --- | --- |
| `Toggle` | `Background`, `True` → `Toggle Background` / `Toggle Value`, `False` → `Toggle Background` / `Toggle Value` |
| `Label` | `Background` |
| `Dropdown` | `Background`, `Value Background`, `Value Stroke`, `Dropdown Select` → `Background` / `Search` / `Item Background` |
| `Slider` | `Background`, `Value Background`, `Value Stroke`, `Slider Bar`, `Slider Bar Value`, `Circle Value` (also used by RangeSlider and ProgressBar) |
| `Code` | `Background` (`ColorSequence`), `Background Code`, `Background Code Value`, `ScrollingFrame Code` |
| `Button` | `Background`, `Click` |
| `Textbox` | `Background`, `Value Background`, `Value Stroke` (also used by tab search boxes) |
| `Keybind` | `Background`, `Value Background`, `Value Stroke`, `True` / `False` (same as Toggle) |
| `Color Picker` | `Background`, `Color Select` → `Background` / `UIStroke` |

## Midnight palette

| Role | RGB |
| --- | --- |
| Background | `12, 17, 40` |
| Page | `8, 11, 27` |
| Element background | `16, 22, 50` |
| Accent (`Main`) | `72, 104, 214` |
| Text & Icon | `235, 238, 255` |
