---
description: Behaviour worth knowing about before you build on the library.
---

# Known quirks

Most of the v1 quirks are fixed (see the [changelog](changelog.md)). These remain, mostly by design.

## Callbacks run once on creation

Toggles, sliders, dropdowns with a value, keybinds, colour pickers and textboxes with a starting value run their callback once, a moment after they're created. This keeps your variables in sync with the UI. Write callbacks so running them with the starting value is harmless. For example, only show a notification when the value actually changes:

```lua
local running = false
Tab:Toggle({
    Title = "Auto Farm",
    Callback = function(v)
        local changed = v ~= running
        running = v
        if changed then
            Window:Notify({ Title = v and "Started" or "Stopped" })
        end
    end,
})
```

## `Clear` and `Add` don't run the dropdown callback

`Dropdown:Clear()` and `Dropdown:Add()` change the options silently. Use `Dropdown:Refresh(list)` when you want the callback to hear about a removed selection.

## Color picker callbacks get numbers

The callback receives `R, G, B` from 0 to 255, while `Get()` and `Library.Flags` give a `Color3`. This was kept so v1 scripts keep working.

## Notifications belong to the last window

`Library:Notify` sends to the most recently created window. With a single window, which is the normal case, this doesn't matter.

## Loops are yours to stop

`Library:Unload()` disconnects everything the library made, but it can't stop loops your script started. Check `Library.Unloaded` or use `Library:OnUnload`. See [Unloading](../state/unloading.md).
