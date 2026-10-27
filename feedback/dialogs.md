---
description: A prompt on top of the window with buttons and an optional text input.
---

# Dialogs

A dialog darkens the window and asks a question. Pressing any button runs that button's callback and closes the dialog.

```lua
Window:Dialog({
    Title = "Reset all settings?",
    Content = "This can't be undone.",
    Buttons = {
        { Title = "Reset", Color = Color3.fromRGB(226, 39, 6), Callback = function()
            -- reset here
        end },
        { Title = "Cancel" },
    },
})
```

| Option | Type | Description |
| --- | --- | --- |
| `Title` | string | The question. RichText supported. |
| `Content` | string | Optional smaller text under the title. |
| `Buttons` | table | Any number of buttons, left to right. Leave it out to get a single **OK** button. Pass `{}` for no buttons (close it from code). |
| `Input` | table | Optional text box: `{ Placeholder = "...", Value = "..." }`. |

### Button

| Option | Type | Default |
| --- | --- | --- |
| `Title` | string | `"null"` |
| `Color` | `Color3` | dark grey |
| `Callback` | `function(InputText)` | does nothing |

## With a text input

When `Input` is set, every button's callback receives what the player typed.

```lua
Window:Dialog({
    Title = "Name this config",
    Input = { Placeholder = "my-config" },
    Buttons = {
        { Title = "Save", Color = Color3.fromRGB(0, 188, 0), Callback = function(Text)
            Library:SaveConfig(Text)
        end },
        { Title = "Cancel" },
    },
})
```

## Closing from code

```lua
local dialog = Window:Dialog({ Title = "Working...", Buttons = {} })
task.wait(3)
dialog.Close()
```

{% hint style="info" %}
Only one dialog can be open at a time. Calling `Dialog` while one is showing does nothing and returns `nil`.

The older `Button1` / `Button2` format still works.
{% endhint %}
