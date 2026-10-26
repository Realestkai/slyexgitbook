---
description: Built-in themes and how to add your own.
---

# Themes

## Built-in themes

| Theme | Look |
| --- | --- |
| `Dark` | Neutral greys with a steel-blue accent. The default. |
| `Midnight` | Midnight blue at the top fading to near-black at the bottom, with a soft blue accent. |

Pick one when you create the window:

```lua
Library:Window({
    Theme = "Midnight",
    -- ...
})
```

Players can also switch themes at any time from the dropdown in the top bar. Every element recolours instantly.

## Adding your own theme

Themes live in the `themes` table at the top of the library file. To add one:

1. Copy the `Midnight` block and give it a new name, for example `Crimson`.
2. Change the colours.
3. Add the name to `themes.index` so it shows up in the dropdown:

```lua
local themes = {
    index = { 'Dark', 'Midnight', 'Crimson' },
    Crimson = {
        -- ...
    },
    -- ...
}
```

{% hint style="danger" %}
Every theme must include **every key** listed below. A missing key errors as soon as an element that uses it is created or recoloured.
{% endhint %}

## Theme keys

### Top level

| Key | Type | Used for |
| --- | --- | --- |
| `Shadow` | `Color3` | Drop shadow around the window, notifications and the open button |
| `Background` | `Color3` | Notifications, the open button, and the base under the window |
| `Background Gradient` | `ColorSequence` | Window background, top to bottom |
| `Page` | `Color3` | The content panel behind elements, and the top-bar divider |
| `Main` | `Color3` | Accent: selected-tab marker, scrollbars, keybind knob |
| `Text & Icon` | `Color3` | All text and icons |

The gradient is drawn top to bottom. To get a flat colour, use a single-colour sequence:

```lua
['Background Gradient'] = ColorSequence.new(Color3.fromRGB(20, 20, 20)),
```

### `Function` (per element)

| Element | Keys |
| --- | --- |
| `Toggle` | `Background`, `True` → `Toggle Background` / `Toggle Value`, `False` → `Toggle Background` / `Toggle Value` |
| `Label` | `Background` |
| `Dropdown` | `Background`, `Value Background`, `Value Stroke`, `Dropdown Select` → `Background` / `Search` / `Item Background` |
| `Slider` | `Background`, `Value Background`, `Value Stroke`, `Slider Bar`, `Slider Bar Value`, `Circle Value` |
| `Code` | `Background` (`ColorSequence`), `Background Code`, `Background Code Value`, `ScrollingFrame Code` |
| `Button` | `Background`, `Click` |
| `Textbox` | `Background`, `Value Background`, `Value Stroke` |
| `Keybind` | `Background`, `Value Background`, `Value Stroke`, `True` / `False` (same as Toggle) |
| `Color Picker` | `Background`, `Color Select` → `Background` / `UIStroke` |

`True` and `False` are the switch colours when a toggle or keybind is on or off. `Toggle Background` is the track and `Toggle Value` is the knob.

## Midnight palette

For reference when designing a matching theme:

| Role | RGB |
| --- | --- |
| Gradient top | `20, 28, 68` |
| Gradient bottom | `6, 7, 12` |
| Background | `16, 22, 52` |
| Page | `10, 12, 26` |
| Element background | `18, 24, 50` |
| Accent (`Main`) | `72, 104, 214` |
| Text & Icon | `235, 238, 255` |
