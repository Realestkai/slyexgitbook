---
description: Behaviour worth knowing about before you build on the library.
---

# Known quirks

These are current behaviours of the library, not mistakes in your script. Each one comes with a workaround.

## Callbacks run on creation

`Toggle`, `Slider`, `Keybind`, `ColorPicker` and `Textbox` (when it has a starting `Value`) run their callback once, a moment after they're created. Write callbacks so running them with the starting value is harmless.

## Button has no return value

`Tab:Button` returns nothing, so you can't call `SetTitle` or `SetVisible` on it. If you need a button you can change later, use a `Label` for the text next to a `Button`, or rebuild the tab.

## Multi-dropdown `SetValue`

On a `Multi = true` dropdown, `SetValue("X")` replaces the whole selection with just `"X"`, and the callback receives `{ X = true }` instead of `{ "X" }`. When clicked normally, the callback receives a list. To handle both shapes:

```lua
Callback = function(Values)
    local list = {}
    for k, v in pairs(Values) do
        table.insert(list, type(k) == "string" and k or v)
    end
    -- use list
end
```

## Textbox `ClearText` is inverted

`ClearText = true` **keeps** the text when the box is clicked, and `false` (the default) clears it. The same goes for `SetClearTextOnFocus`.

## Keybind ignores chat focus

A keybind's key fires even while the player is typing in chat. Guard against it:

```lua
local UIS = game:GetService("UserInputService")

Callback = function(Key, Value)
    if UIS:GetFocusedTextBox() then return end
    -- ...
end
```

## Clicking a keybind row skips the callback

Clicking the keybind row flips its value but doesn't run the callback. Only key presses, rebinding and `SetKey` do.

## Notify yields

`Window:Notify` waits about 0.15 seconds before it returns. Wrap it in `task.spawn` when you don't want to wait.

## `Config` and `CloseUIButton` are required

`Library:Window` errors if either table is missing. Pass `{}` when you don't need any settings, plus `CloseUIButton = { Enabled = false, Text = "" }`.

## Global names

The library defines globals such as `Library`, `SaveTheme`, `tw`, `click` and `background`. Avoid using those names for your own globals in the same environment.
