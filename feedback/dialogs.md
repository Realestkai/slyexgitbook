---
description: A confirm/cancel overlay on top of the window.
---

# Dialogs

A dialog darkens the window and shows a question with two buttons. Either button closes the dialog after running its callback.

```lua
Window:Dialog({
    Title = "Reset all settings?",
    Button1 = {
        Title = "Confirm",
        Color = Color3.fromRGB(0, 188, 0),
        Callback = function()
            -- runs when Confirm is pressed
        end,
    },
    Button2 = {
        Title = "Cancel",
        Color = Color3.fromRGB(226, 39, 6),
        Callback = function()
            -- optional
        end,
    },
})
```

| Option | Type | Description |
| --- | --- | --- |
| `Title` | string | The question. RichText supported. |
| `Button1` | table | Left button. **Required.** |
| `Button2` | table | Right button. **Required.** |

### Button table

| Option | Type | Default |
| --- | --- | --- |
| `Title` | string | `"null"` |
| `Color` | `Color3` | green for `Button1`, red for `Button2` |
| `Callback` | `function()` | does nothing |

{% hint style="info" %}
Only one dialog can be open at a time. Calling `Dialog` while one is showing does nothing.
{% endhint %}

The window's own ✕ button uses this same dialog to ask before destroying the UI.
