---
description: Options and methods every element has.
---

# Shared element methods

## Shared options

These options work on every element:

| Option | Type | Description |
| --- | --- | --- |
| `Title` | string | Main text. RichText supported. |
| `Desc` | string | Smaller line under the title. RichText supported. |
| `Image` | string / number | Icon on the left (an icon name or an asset ID). |
| `Flag` | string | Registers the element so its value is in `Library.Flags` and gets saved in configs. See [Flags and configs](../state/flags-and-configs.md). |
| `Tooltip` | string | Text shown next to the cursor while hovering the element. |
| `Locked` | boolean | Start the element locked. See [Locking](locking.md). |
| `LockReason` | string | Text shown on the lock. Defaults to `"Locked"`. |
| `Premium` | boolean | Locked for Freemium users. See [Freemium and Premium](../state/premium.md). |
| `DependsOn` | string / table | Only show the element while another flag is on. See [Locking](locking.md). |

```lua
Tab:Toggle({
    Title = "Auto Farm",
    Flag = "AutoFarm",
    Tooltip = "Farms the nearest enemy automatically",
})
```

## Shared methods

Every element returns an object with these:

```lua
Element:SetVisible(false)   -- hide (the space it took up collapses too)
Element:Destroy()           -- remove it for good
Element.Type                -- "Toggle", "Slider", "Dropdown", ...
Element.Instance            -- the element's root Frame
Element:Lock("Reason")      -- grey it out and block clicks (see Locking)
Element:Unlock()
Element:IsLocked()
```

Elements that show a title also have:

```lua
Element:SetTitle("New title")
Element:SetDesc("New description")
```

Elements that hold a value also have:

```lua
Element:Get()               -- current value
Element:Set(value)          -- change it (runs the callback)
Element:SetValue(value)     -- same as Set
Element:Set(value, true)    -- change it without running the callback
```

| Element | `Get()` returns |
| --- | --- |
| Toggle | `boolean` |
| Slider | `number` |
| RangeSlider | `{ min, max }` |
| Dropdown | `string`, or `{ string }` when multi |
| Textbox | `string`, or `number` when `Numeric` |
| Keybind | `{ Key = Enum.KeyCode, Value = boolean }` |
| ColorPicker | `Color3` |
| ProgressBar | `number` |

## Callbacks

* **Errors are reported.** If a callback errors, the error is printed as a warning starting with `[Sylex] callback error:` instead of disappearing silently.
* **Callbacks run once on creation.** Toggles, sliders, dropdowns with a value, keybinds, colour pickers and textboxes with a starting value run their callback once just after they're created, with that value. This keeps your script's variables in sync with what the UI shows.
* **Nothing runs after unloading.** Callbacks stop firing once the library is [unloaded](../state/unloading.md).
